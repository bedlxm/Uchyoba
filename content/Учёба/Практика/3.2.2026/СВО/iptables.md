
```c
//----------------------------------------------------------------------------------
// iptables v1.8+ Architecture & Packet Flow
//----------------------------------------------------------------------------------
// Tables:
//   filter      Default table. Packet filtering (firewalling)
//   nat         Network Address Translation (port forwarding, masquerade)
//   mangle      Specialized packet alteration (TOS, TTL, MARK)
//   raw         Configuration exemptions from connection tracking (NOTRACK)
//
// Chains by Path:
//   Incoming to Local Machine:    PREROUTING (nat/mangle) -> INPUT (filter/mangle)
//   Outgoing from Local Machine:  OUTPUT (all) -> POSTROUTING (nat/mangle)
//   Forwarded (Routed/Transit):   PREROUTING (nat/mangle) -> FORWARD (filter/mangle) -> POSTROUTING (nat/mangle)
```

```c
//----------------------------------------------------------------------------------
// Chain Operations & Inspection (Управление цепочками и просмотр)
//----------------------------------------------------------------------------------
iptables -L [-t table]                      // Вывести список правил (по умолчанию: filter)
iptables -L -n -v --line-numbers            // Детальный просмотр: IP/порты в цифрах + счетчики + номера строк
iptables -S [-t table]                      // Вывести все правила в виде исполняемых команд (ready to copy)
iptables -F [chain] [-t table]              // Очистить (Flush) все правила в цепочке/таблице
iptables -Z [chain] [-t table]              // Сбросить счетчики пакетов и байт в 0 (Zero counters)
iptables -P <chain> <ACCEPT|DROP>           // Задать политику по умолчанию для цепочки (Default Policy)
iptables -N <chain>                         // Создать новую пользовательскую цепочку
iptables -X <chain>                         // Удалить пустую пользовательскую цепочку
```

```c
//----------------------------------------------------------------------------------
// Rule Manipulation Commands (Добавление, вставка, удаление правил)
//----------------------------------------------------------------------------------
iptables -A <chain> [matches] -j <target>   // Добавить правило в КОНЕЦ цепочки (Append)
iptables -I <chain> [index] [m] -j <target> // Вставить правило в НАЧАЛО (или на позицию index) (Insert)
iptables -D <chain> <index>                 // Удалить правило по порядковому номеру строки (Delete by line)
iptables -D <chain> [exact rule match]      // Удалить правило по точному совпадению его параметров
iptables -R <chain> <index> [new rule]      // Заменить правило на указанной позиции (Replace)
```

```c
//----------------------------------------------------------------------------------
// Basic Match Selectors (Базовые критерии фильтрации L3/L4)
//----------------------------------------------------------------------------------
-p <tcp|udp|icmp|all>                       // Протокол пакета (Protocol)
-s <ip|cidr>                                // IP-источника (Source: 192.168.1.5, 10.0.0.0/24)
-d <ip|cidr>                                // IP-назначения (Destination)
-i <interface>                              // Входящий интерфейс (In-interface: eth0, tun+, lo) [INPUT/FORWARD]
-o <interface>                              // Исходящий интерфейс (Out-interface) [OUTPUT/FORWARD]
! <criterion>                               // Логическое отрицание (пример: ! -s 10.0.0.0/8)

// TCP/UDP Extensions (требуют предварительного указания -p tcp или -p udp):
--sport <port[:port]>                       // Порт источника (Source Port, одиночный или диапазон 1000:2000)
--dport <port[:port]>                       // Порт назначения (Destination Port)
--tcp-flags <mask> <comp>                   // Проверка TCP флагов (пример: --tcp-flags SYN,ACK,FIN,RST SYN)
--syn                                       // Эквивалент проверки начала TCP-сессии (--tcp-flags SYN,RST,ACK SYN)

// ICMP Extensions (требует -p icmp):
--icmp-type <type[/code]|name>              // Тип ICMP (пример: echo-request, 8/0, 3/4)
```

```c
//----------------------------------------------------------------------------------
// Extended Match Modules (-m module) (Модули расширенного анализа)
//----------------------------------------------------------------------------------
-m conntrack --ctstate <states>             // Анализ состояния: NEW, ESTABLISHED, RELATED, INVALID
-m multiport --dports <p1,p2,p3:p4>         // До 15 портов назначения списком через запятую
-m multiport --sports <p1,p2,...>           // Несколько портов источника
-m iprange --src-range <ip-ip>              // Диапазон IP-адресов источника (192.168.1.10-192.168.1.50)
-m iprange --dst-range <ip-ip>              // Диапазон IP-адресов назначения
-m limit --limit <n/sec|min|hour>           // Ограничение частоты срабатываний (Rate limiting)
-m limit --limit-burst <n>                  // Максимальный разовый всплеск перед включением лимита
-m mac --mac-source <xx:xx:...>             // Фильтрация по MAC-адресу источника (только локалка)
-m string --string "str" --algo <bm|kmp>    // Поиск текста в полезной нагрузке пакета (Deep Packet Inspection)
-m comment --comment "text"                 // Привязать комментарий к правилу (до 256 символов)
```

```c
//----------------------------------------------------------------------------------
// Targets / Verdicts (-j target) (Действия над пакетом)
//----------------------------------------------------------------------------------
-j ACCEPT                                   // Пропустить пакет (перестает проверяться цепочкой)
-j DROP                                     // Молча уничтожить пакет (Drop, отправитель ждет таймаут)
-j REJECT [--reject-with <type>]            // Отклонить пакет с отправкой ICMP-ошибки или TCP-RST
-j LOG [--log-prefix "TAG: "]               // Записать событие в kernel log/syslog и идти дальше
-j RETURN                                   // Прекратить выполнение цепочки и вернуться в вызывающую
```

```c
//----------------------------------------------------------------------------------
// NAT Table Targets (-t nat -j target) (Трансляция адресов и портов)
//----------------------------------------------------------------------------------
// DNAT (Destination NAT) — подмена адреса получателя (проброс порта внутрь):
-t nat -A PREROUTING [matches] -j DNAT --to-destination <ip[:port]>

// SNAT (Source NAT) — подмена адреса отправителя на статичный IP:
-t nat -A POSTROUTING [matches] -j SNAT --to-source <ip[:port]>

// MASQUERADE — автоматический SNAT под текущий динамический IP внешнего интерфейса:
-t nat -A POSTROUTING -o <out_iface> -j MASQUERADE

// REDIRECT — перенаправление входящего пакета на локальный порт самого сервера:
-t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080
```

```c
//----------------------------------------------------------------------------------
// Mangle Table Targets (-t mangle -j target) (Правка заголовков)
//----------------------------------------------------------------------------------
-t mangle -A POSTROUTING -j TTL --ttl-set <val>   // Жестко перезаписать TTL пакета (обход раздачи)
-t mangle -A POSTROUTING -j TTL --ttl-inc <val>   // Увеличить TTL на значение
-t mangle -A POSTROUTING -j TTL --ttl-dec <val>   // Уменьшить TTL на значение
-t mangle -A PREROUTING -j MARK --set-mark <hex>  // Поставить метку на пакет для iproute2 (policy routing)
```

```c
//----------------------------------------------------------------------------------
// Rule Persistence & Migration (Сохранение и перенос)
//----------------------------------------------------------------------------------
iptables-save > /path/to/rules.v4           // Экспорт текущих правил IPv4 в файл
iptables-restore < /path/to/rules.v4        // Импорт правил из файла в ядро (атомарно)
netfilter-persistent save                  // Сохранить IPv4/IPv6 в Ubuntu/Debian (автозапуск)
netfilter-persistent reload                // Применить сохраненные правила заново
```

```c
//----------------------------------------------------------------------------------
// Production Recipes (Готовые шаблоны боевых конфигураций)
//----------------------------------------------------------------------------------

// 1. Стандартный защитный базис (Stateful Firewall):
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -P INPUT DROP
iptables -P FORWARD DROP

// 2. Блокировка атакующего IP на первой позиции:
iptables -I INPUT 1 -s 203.0.113.195 -j DROP

// 3. Открытие пула портов для веб-сервера одним правилом:
iptables -A INPUT -p tcp -m multiport --dports 80,443,8443 -j ACCEPT

// 4. Защита от SYN-флуда (DDoS mitigation):
iptables -A INPUT -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
iptables -A INPUT -p tcp --syn -j DROP

// 5. Полный Port Forwarding (Ретранслятор порта 8080 на внешний хост 195.63.139.66:2083):
iptables -t nat -A PREROUTING -p udp --dport 8080 -j DNAT --to-destination 195.63.139.66:2083
iptables -t nat -A POSTROUTING -d 195.63.139.66 -p udp --dport 2083 -j MASQUERADE
iptables -A FORWARD -d 195.63.139.66 -p udp --dport 2083 -j ACCEPT
```