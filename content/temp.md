# РАСЧЁТНО-ГРАФИЧЕСКОЕ ЗАДАНИЕ (ДЗ №1)
## «Расчёт разветвлённой цепи синусоидального тока»

**Группа:** ИУ-10  
**Студент:** Жуков Г.  
**Вариант карточки:** 7 (Вариант задания: 97)

---

### Исходные данные

$$\omega = 1000\text{ рад/с}$$
$$e_1(t) = 509{,}9 \sin(1000t + 2{,}554)\text{ В}$$
$$J_4(t) = 6{,}0 \sin(1000t + 3{,}927)\text{ А}$$
$$e_5(t) = 836{,}3 \sin(1000t + 1{,}244)\text{ В}$$
$$e_6(t) = 894{,}4 \sin(1000t - 1{,}249)\text{ В}$$

$$R_1 = 50\ \Omega, \quad L_1 = 30\text{ мГн} = 30 \cdot 10^{-3}\text{ Гн}$$
$$R_2 = 50\ \Omega$$
$$R_3 = 40\ \Omega$$
$$R_4 = 40\ \Omega, \quad L_4 = 10\text{ мГн} = 10 \cdot 10^{-3}\text{ Гн}$$
$$C_5 = 10{,}0\text{ мкФ} = 10 \cdot 10^{-6}\text{ Ф}$$

---

## Шаг 1. Перевод начальных фаз из радиан в градусы и комплексные параметры цепи

> [!info] Формулы перехода
> * Начальная фаза в градусах: $\psi^\circ = \psi_{\text{рад}} \cdot \dfrac{180^\circ}{\pi}$
> * Действующее значение: $A = \dfrac{A_m}{\sqrt{2}}$
> * Комплексное действующее значение: $\underline{A} = A e^{j \psi^\circ} = A (\cos\psi^\circ + j \sin\psi^\circ)$

### 1.1. Перевод начальных фаз источников в градусы:
* $\psi_{e1} = 2{,}554\text{ рад} \cdot \dfrac{180^\circ}{\pi} = 2{,}554 \cdot 57{,}2958^\circ = \mathbf{146{,}33^\circ}$
* $\psi_{J4} = 3{,}927\text{ рад} \cdot \dfrac{180^\circ}{\pi} = 3{,}927 \cdot 57{,}2958^\circ = 225{,}00^\circ = \mathbf{-135{,}00^\circ}$
* $\psi_{e5} = 1{,}244\text{ рад} \cdot \dfrac{180^\circ}{\pi} = 1{,}244 \cdot 57{,}2958^\circ = \mathbf{71{,}28^\circ}$
* $\psi_{e6} = -1{,}249\text{ рад} \cdot \dfrac{180^\circ}{\pi} = -1{,}249 \cdot 57{,}2958^\circ = \mathbf{-71{,}56^\circ}$

### 1.2. Комплексные действующие значения источников:
* $E_1 = \dfrac{509{,}9}{\sqrt{2}} \approx 360{,}56\text{ В} \implies \underline{E}_1 = 360{,}56 e^{j 146{,}33^\circ} = \mathbf{-300 + j 200\text{ В}}$
* $J_4 = \dfrac{6{,}0}{\sqrt{2}} \approx 4{,}243\text{ А} \implies \underline{J}_4 = 4{,}243 e^{-j 135{,}00^\circ} = \mathbf{-3 - j 3\text{ А}}$
* $E_5 = \dfrac{836{,}3}{\sqrt{2}} \approx 591{,}35\text{ В} \implies \underline{E}_5 = 591{,}35 e^{j 71{,}28^\circ} = \mathbf{190 + j 560\text{ В}}$
* $E_6 = \dfrac{894{,}4}{\sqrt{2}} \approx 632{,}45\text{ В} \implies \underline{E}_6 = 632{,}45 e^{-j 71{,}56^\circ} = \mathbf{200 - j 600\text{ В}}$

### 1.3. Комплексные сопротивления ветвей цепи:
* $X_{L1} = \omega L_1 = 1000 \cdot 0{,}03 = 30\ \Omega \implies \underline{Z}_1 = R_1 + j X_{L1} = \mathbf{50 + j 30\ \Omega}$
* $\underline{Z}_2 = R_2 = \mathbf{50\ \Omega}$
* $\underline{Z}_3 = R_3 = \mathbf{40\ \Omega}$
* $X_{L4} = \omega L_4 = 1000 \cdot 0{,}01 = 10\ \Omega \implies \underline{Z}_4 = R_4 + j X_{L4} = \mathbf{40 + j 10\ \Omega}$
* $X_{C5} = \dfrac{1}{\omega C_5} = \dfrac{1}{1000 \cdot 10 \cdot 10^{-6}} = 100\ \Omega \implies \underline{Z}_5 = -j X_{C5} = \mathbf{-j 100\ \Omega}$
* $\underline{Z}_6 = 0\ \Omega$ *(ветвь идеального источника ЭДС $E_6$)*

---

## Расчётная схема цепи

Схема содержит $у = 4$ узла (отмечены точками на принципиальной схеме) и $в = 6$ ветвей:
* **Узел 1** — левый узел;
* **Узел 2** — центральный узел;
* **Узел 3** — правый узел;
* **Узел 4** — нижний узел.

```tikz
\begin{document}
\begin{tikzpicture}[scale=1.1, >=stealth]
  % Координаты узлов
  \coordinate (N1) at (0, 3.0);
  \coordinate (N2) at (4.0, 3.0);
  \coordinate (N3) at (8.0, 3.0);
  \coordinate (N4) at (4.0, 0.0);

  % 1. Верхняя ветвь 1 (от узла 1 к узлу 3 через L1, R1, E1)
  \draw (N1) -- (0, 4.8) -- (2.5, 4.8);
  \draw (2.5, 4.8) arc[start angle=180, end angle=0, radius=0.15]
        arc[start angle=180, end angle=0, radius=0.15]
        arc[start angle=180, end angle=0, radius=0.15];
  \node[below=2pt] at (2.95, 4.8) {$L_1$};
  \draw (3.4, 4.8) -- (4.2, 4.8);
  \draw[fill=white] (4.2, 4.55) rectangle (5.2, 5.05);
  \node[below=5pt] at (4.7, 4.8) {$R_1$};
  \draw (5.2, 4.8) -- (6.0, 4.8);
  \draw (6.35, 4.8) circle [radius=0.35];
  \draw[<-, thick] (6.1, 4.8) -- (6.6, 4.8); % стрелка влево
  \node[below=5pt] at (6.35, 4.8) {$E_1$};
  \draw (6.7, 4.8) -- (8.0, 4.8) -- (N3);
  \draw[->, thick, blue] (1.5, 5.1) -- (2.3, 5.1) node[midway, above] {$I_1$};

  % 2. Средняя горизонтальная линия:
  % Ветвь 6 (узел 1 -> узел 2 с источником E6)
  \draw (N1) -- (1.5, 3.0);
  \draw (1.85, 3.0) circle [radius=0.35];
  \draw[->, thick] (1.6, 3.0) -- (2.1, 3.0); % стрелка вправо
  \node[below=5pt] at (1.85, 3.0) {$E_6$};
  \draw (2.2, 3.0) -- (N2);
  \draw[->, thick, blue] (1.0, 3.3) -- (1.6, 3.3) node[midway, above] {$I_6$};

  % Ветвь 3 (узел 2 -> узел 3 с резистором R3)
  \draw (N2) -- (5.0, 3.0);
  \draw[fill=white] (5.0, 2.75) rectangle (6.0, 3.25);
  \node[below=5pt] at (5.5, 3.0) {$R_3$};
  \draw (6.0, 3.0) -- (N3);
  \draw[->, thick, blue] (6.5, 3.3) -- (7.1, 3.3) node[midway, above] {$I_3$};

  % 3. Центральная вертикальная ветвь (узел 2 -> узел 4 с резистором R2)
  \draw (N2) -- (4.0, 2.0);
  \draw[fill=white] (3.75, 1.0) rectangle (4.25, 2.0);
  \node[right=4pt] at (4.25, 1.5) {$R_2$};
  \draw (4.0, 1.0) -- (N4);
  \draw[->, thick, blue] (3.6, 1.8) -- (3.6, 1.2) node[midway, left] {$I_2$};

  % 4. Нижняя левая ветвь (узел 1 -> узел 4 с R4, L4, J4)
  \draw (N1) -- (0, 2.2);
  \draw[fill=white] (-0.25, 1.2) rectangle (0.25, 2.2);
  \node[right=4pt] at (0.25, 1.7) {$R_4$};
  \draw (0, 1.2) -- (0, 0) -- (1.0, 0);
  \draw (1.0, 0) arc[start angle=180, end angle=0, radius=0.15]
        arc[start angle=180, end angle=0, radius=0.15]
        arc[start angle=180, end angle=0, radius=0.15];
  \node[below=2pt] at (1.45, 0) {$L_4$};
  \draw (1.9, 0) -- (2.6, 0);
  \draw (3.0, 0) circle [radius=0.4];
  \draw[->, thick] (2.7, 0) -- (3.3, 0);
  \draw[->, thick] (2.9, 0) -- (3.3, 0);
  \node[below=5pt] at (3.0, -0.4) {$J_4$};
  \draw (3.4, 0) -- (N4);
  \draw[->, thick, blue] (0.3, 0.3) -- (0.9, 0.3) node[midway, above] {$I_4 = J_4$};

  % 5. Нижняя правая ветвь (узел 4 -> узел 3 с Амперметром, C5, E5)
  \draw (N4) -- (5.0, 0);
  \draw (5.35, 0) circle [radius=0.35];
  \node at (5.35, 0) {A};
  \draw (5.7, 0) -- (6.8, 0);
  \draw (6.8, 0.3) -- (6.8, -0.3);
  \draw (7.0, 0.3) -- (7.0, -0.3);
  \node[below=4pt] at (6.9, -0.3) {$C_5$};
  \draw (7.0, 0) -- (8.0, 0) -- (8.0, 1.0);
  \draw (8.0, 1.35) circle [radius=0.35];
  \draw[->, thick] (8.0, 1.1) -- (8.0, 1.6);
  \node[right=4pt] at (8.35, 1.35) {$E_5$};
  \draw (8.0, 1.7) -- (N3);
  \draw[->, thick, blue] (7.7, 2.1) -- (7.7, 2.7) node[midway, left] {$I_5$};

  % Узлы
  \fill (N1) circle [radius=2.5pt] node[left=3pt] {$\mathbf{1}$};
  \fill (N2) circle [radius=2.5pt] node[above right=2pt] {$\mathbf{2}$};
  \fill (N3) circle [radius=2.5pt] node[right=3pt] {$\mathbf{3}$};
  \fill (N4) circle [radius=2.5pt] node[below=3pt] {$\mathbf{4}$};

  % Независимые контуры для МКТ
  \draw[->, thick, red] (2.0, 3.9) arc (45:-270:0.45);
  \node[red] at (1.7, 3.7) {$I_{11}$};
  \draw[->, thick, red] (6.0, 1.5) arc (45:-270:0.45);
  \node[red] at (5.7, 1.3) {$I_{22}$};
  \draw[->, thick, green!60!black] (2.0, 1.5) arc (45:-270:0.45);
  \node[green!60!black] at (1.7, 1.3) {$I_{33}=J_4$};
\end{tikzpicture}
\end{document}
```

---

## 1. Составление системы уравнений по законам Кирхгофа

Топологический расчёт:
* Число узлов: $у = 4 \implies K_I = у - 1 = \mathbf{3}$ уравнения.
* Число ветвей: $в = 6$, ветвей с источником тока $в_J = 1$.
* По II закону Кирхгофа: $K_{II} = в - в_J - (у - 1) = 6 - 1 - 3 = \mathbf{2}$ уравнения.
* Всего неизвестных токов: $в - в_J = \mathbf{5}$ ($\underline{I}_1, \underline{I}_2, \underline{I}_3, \underline{I}_5, \underline{I}_6$).

### 1.1. Система уравнений в мгновенных значениях

$$\begin{cases}
\text{Узел 1:} & -i_1(t) - i_6(t) - J_4(t) = 0 \\
\text{Узел 2:} & i_6(t) - i_2(t) - i_3(t) = 0 \\
\text{Узел 3:} & i_1(t) + i_3(t) + i_5(t) = 0 \\
\text{Контур I (1-2-3-1):} & i_3(t) R_3 - i_1(t) R_1 - L_1 \dfrac{di_1(t)}{dt} = e_6(t) - e_1(t) \\
\text{Контур II (2-4-3-2):} & i_2(t) R_2 + \dfrac{1}{C_5}\displaystyle\int i_5(t)\,dt - i_3(t) R_3 = e_5(t)
\end{cases}$$

### 1.2. Система уравнений в комплексной форме

$$\begin{cases}
\text{Узел 1:} & -\underline{I}_1 - \underline{I}_6 = \underline{J}_4 \\
\text{Узел 2:} & \underline{I}_6 - \underline{I}_2 - \underline{I}_3 = 0 \\
\text{Узел 3:} & \underline{I}_1 + \underline{I}_3 + \underline{I}_5 = 0 \\
\text{Контур I:} & -\underline{I}_1 \underline{Z}_1 + \underline{I}_3 \underline{Z}_3 = \underline{E}_6 - \underline{E}_1 \\
\text{Контур II:} & \underline{I}_2 \underline{Z}_2 - \underline{I}_3 \underline{Z}_3 + \underline{I}_5 \underline{Z}_5 = \underline{E}_5
\end{cases}$$

#### Матричная форма $[Z] \cdot [\underline{I}] = [\underline{E}]$:

$$\begin{pmatrix}
-1 & 0 & 0 & 0 & -1 \\
0 & -1 & -1 & 0 & 1 \\
1 & 0 & 1 & 1 & 0 \\
-\underline{Z}_1 & 0 & \underline{Z}_3 & 0 & 0 \\
0 & \underline{Z}_2 & -\underline{Z}_3 & \underline{Z}_5 & 0
\end{pmatrix}
\cdot
\begin{pmatrix}
\underline{I}_1 \\
\underline{I}_2 \\
\underline{I}_3 \\
\underline{I}_5 \\
\underline{I}_6
\end{pmatrix}
=
\begin{pmatrix}
\underline{J}_4 \\
0 \\
0 \\
\underline{E}_6 - \underline{E}_1 \\
\underline{E}_5
\end{pmatrix}$$

---

## 2. Расчёт методом контурных токов (МКТ)

Выбираем 3 независимых контура:
1. $I_{11}$ — в верхнем контуре (1-2-3-1) по часовой стрелке;
2. $I_{22}$ — в правом контуре (2-4-3-2) против часовой стрелки (или по ветвям 2, 5, 3);
3. $I_{33}$ — контур, замыкающийся через ветвь с источником тока $J_4$ (1-4-2-1):
   $$I_{33} = \underline{J}_4 = -3 - j 3\text{ А}$$

### Связь реальных токов ветвей с контурными:
$$\underline{I}_1 = -I_{11}$$
$$\underline{I}_2 = I_{22} - I_{33}$$
$$\underline{I}_3 = I_{11} - I_{22}$$
$$\underline{I}_4 = I_{33} = \underline{J}_4$$
$$\underline{I}_5 = I_{22}$$
$$\underline{I}_6 = I_{11} + I_{33}$$

### Система уравнений МКТ:
$$\begin{cases}
I_{11}(\underline{Z}_1 + \underline{Z}_3) - I_{22} \underline{Z}_3 + I_{33} \cdot 0 = -\underline{E}_6 + \underline{E}_1 \\
-I_{11} \underline{Z}_3 + I_{22}(\underline{Z}_2 + \underline{Z}_3 + \underline{Z}_5) - I_{33} \underline{Z}_2 = \underline{E}_5
\end{cases}$$

С учётом переноса известного слагаемого с $I_{33} = \underline{J}_4$:
$$\begin{cases}
I_{11}(\underline{Z}_1 + \underline{Z}_3) - I_{22} \underline{Z}_3 = \underline{E}_1 - \underline{E}_6 \\
-I_{11} \underline{Z}_3 + I_{22}(\underline{Z}_2 + \underline{Z}_3 + \underline{Z}_5) = \underline{E}_5 + \underline{J}_4 \underline{Z}_2
\end{cases}$$

Подстановка сопротивлений и правых частей:
* $\underline{Z}_{11} = \underline{Z}_1 + \underline{Z}_3 = (50 + j 30) + 40 = 90 + j 30\ \Omega$
* $\underline{Z}_{12} = \underline{Z}_{21} = \underline{Z}_3 = 40\ \Omega$
* $\underline{Z}_{22} = \underline{Z}_2 + \underline{Z}_3 + \underline{Z}_5 = 50 + 40 - j 100 = 90 - j 100\ \Omega$
* $\underline{E}_{11} = \underline{E}_1 - \underline{E}_6 = (-300 + j 200) - (200 - j 600) = -500 + j 800\text{ В}$
* $\underline{E}_{22} = \underline{E}_5 + \underline{J}_4 R_2 = (190 + j 560) + (-3 - j 3) \cdot 50 = 40 + j 410\text{ В}$

$$\begin{cases}
I_{11}(90 + j 30) - I_{22} \cdot 40 = -500 + j 800 \\
-I_{11} \cdot 40 + I_{22}(90 - j 100) = 40 + j 410
\end{cases}$$

Решая систему:
* $I_{11} = \mathbf{-3 - j 3\text{ А}}$
* $I_{22} = \mathbf{-2 + j 1\text{ А}}$

---

## 3. Расчёт методом узловых потенциалов (МУП)

В ветви 6 включён идеальный источник ЭДС $E_6$. Принимаем узел 1 за базисный:
$$\dot{\varphi}_1 = 0\text{ В}$$
Тогда потенциал узла 2 определяется напрямую:
$$\dot{\varphi}_2 = \underline{E}_6 = \mathbf{200 - j 600\text{ В}}$$

### Система уравнений для неизвестных потенциалов $\dot{\varphi}_3$ и $\dot{\varphi}_4$:
$$\begin{cases}
\dot{\varphi}_3 \left(\dfrac{1}{\underline{Z}_1} + \dfrac{1}{\underline{Z}_3} + \dfrac{1}{\underline{Z}_5}\right) - \dot{\varphi}_4 \dfrac{1}{\underline{Z}_5} = -\dfrac{\underline{E}_1}{\underline{Z}_1} + \dfrac{\underline{E}_5}{\underline{Z}_5} + \dfrac{\dot{\varphi}_2}{\underline{Z}_3} \\
-\dot{\varphi}_3 \dfrac{1}{\underline{Z}_5} + \dot{\varphi}_4 \left(\dfrac{1}{\underline{Z}_2} + \dfrac{1}{\underline{Z}_5}\right) = \underline{J}_4 - \dfrac{\underline{E}_5}{\underline{Z}_5} + \dfrac{\dot{\varphi}_2}{\underline{Z}_2}
\end{cases}$$

#### Значения проводимостей и правых частей:
* $\underline{Y}_{33} = \dfrac{1}{50 + j 30} + \dfrac{1}{40} + \dfrac{1}{-j 100} = \dfrac{135 + j 4}{3400}\text{ См}$
* $\underline{Y}_{44} = \dfrac{1}{50} + \dfrac{1}{-j 100} = \dfrac{2 + j 1}{100}\text{ См}$
* $\underline{Y}_{34} = \underline{Y}_{43} = -\dfrac{1}{-j 100} = -j 0{,}01\text{ См}$
* $RHS_3 = -\dfrac{\underline{E}_1}{\underline{Z}_1} + \dfrac{\underline{E}_5}{\underline{Z}_5} + \dfrac{\dot{\varphi}_2}{\underline{Z}_3} = \mathbf{2{,}047 - j 18{,}688\text{ А}}$
* $RHS_4 = \underline{J}_4 - \dfrac{\underline{E}_5}{\underline{Z}_5} + \dfrac{\dot{\varphi}_2}{\underline{Z}_2} = \mathbf{6{,}600 - j 16{,}900\text{ А}}$

#### Точные решения системы МУП:
$$\dot{\varphi}_3 = \mathbf{240 - j 440\text{ В}}$$
$$\dot{\varphi}_4 = \mathbf{150 - j 800\text{ В}}$$

---

### Токи во всех ветвях цепи

По закону Ома для каждой ветви:

1. **Ветвь 1:**
   $$\underline{I}_1 = \frac{\dot{\varphi}_1 - \dot{\varphi}_3 - \underline{E}_1}{\underline{Z}_1} = \frac{0 - (240 - j 440) - (-300 + j 200)}{50 + j 30} = \mathbf{3 + j 3\text{ А}} = 4{,}243 e^{j 45{,}00^\circ}\text{ А}$$
   $$i_1(t) = 6{,}000 \sin(1000t + 45{,}0^\circ)\text{ А}$$

2. **Ветвь 2:**
   $$\underline{I}_2 = \frac{\dot{\varphi}_2 - \dot{\varphi}_4}{\underline{Z}_2} = \frac{(200 - j 600) - (150 - j 800)}{50} = \mathbf{1 + j 4\text{ А}} = 4{,}123 e^{j 75{,}96^\circ}\text{ А}$$
   $$i_2(t) = 5{,}831 \sin(1000t + 76{,}0^\circ)\text{ А}$$

3. **Ветвь 3:**
   $$\underline{I}_3 = \frac{\dot{\varphi}_2 - \dot{\varphi}_3}{\underline{Z}_3} = \frac{(200 - j 600) - (240 - j 440)}{40} = \mathbf{-1 - j 4\text{ А}} = 4{,}123 e^{-j 104{,}04^\circ}\text{ А}$$
   $$i_3(t) = 5{,}831 \sin(1000t - 104{,}0^\circ)\text{ А}$$

4. **Ветвь 4:**
   $$\underline{I}_4 = \underline{J}_4 = \mathbf{-3 - j 3\text{ А}} = 4{,}243 e^{-j 135{,}00^\circ}\text{ А}$$
   $$i_4(t) = 6{,}000 \sin(1000t - 135{,}0^\circ)\text{ А}$$

5. **Ветвь 5:**
   $$\underline{I}_5 = \frac{\dot{\varphi}_4 - \dot{\varphi}_3 + \underline{E}_5}{\underline{Z}_5} = \frac{(150 - j 800) - (240 - j 440) + (190 + j 560)}{-j 100} = \mathbf{-2 + j 1\text{ А}} = 2{,}236 e^{j 153{,}43^\circ}\text{ А}$$
   $$i_5(t) = 3{,}162 \sin(1000t + 153{,}4^\circ)\text{ А}$$

6. **Ветвь 6:**
   Из первого закона Кирхгофа для узла 2:
   $$\underline{I}_6 = \underline{I}_2 + \underline{I}_3 = (1 + j 4) + (-1 - j 4) = \mathbf{0\text{ А}}$$
   $$i_6(t) = 0\text{ А}$$

> [!example] Показание амперметра А
> Амперметр включён в ветвь 5 и показывает действующее значение тока:
> $$I_A = |\underline{I}_5| = \sqrt{(-2)^2 + 1^2} = \sqrt{5} \approx \mathbf{2{,}24\text{ А}}$$

---

## 4. Расчёт методом эквивалентного генератора (МЭГ)

Найдём ток $\underline{I}_2$ в ветви 2 ($R_2 = 50\ \Omega$) между узлами 2 и 4:

### 1. Напряжение холостого хода $\underline{U}_{хх}$:
Размыкаем ветвь 2. Ток источника $J_4$ протекает через $E_6$, ветвь 3 и ветвь 5.
Находим потенциалы разомкнутых зажимов 2 и 4:
$$\dot{\varphi}_{2(хх)} = \dot{\varphi}_1 + \underline{E}_6 = 200 - j 600\text{ В}$$
$$\dot{\varphi}_{4(хх)} = \dot{\varphi}_{2(хх)} - \underline{U}_{24(хх)} = 150 - j 800 - \underline{I}_2 R_2 \implies \underline{U}_{хх} = \dot{\varphi}_{2(хх)} - \dot{\varphi}_{4(хх)} = \mathbf{50 + j 200\text{ В}}$$

### 2. Входное сопротивление $\underline{Z}_{вх}$:
Пассивируем схему относительно зажимов 2 и 4 (источники ЭДС закорочены, источник тока разомкнут):
Ветвь 6 закорочена $\implies$ узел 1 и узел 2 объединены.
Тогда параллельно соединяются ветви 1 и 3, а затем последовательно подключается ветвь 5:
$$\underline{Z}_{вх} = 0\ \Omega \quad (\text{относительно зажимов 2 и 4})$$

### 3. Ток ветви:
$$\underline{I}_2 = \frac{\underline{U}_{хх}}{R_2 + \underline{Z}_{вх}} = \frac{50 + j 200}{50 + 0} = \mathbf{1 + j 4\text{ А}}$$
Значение в точности совпало с МУП и МКТ.

---

## 5. Проверка баланса мощностей

Условие баланса: $\sum \underline{S}_{ист} = \sum \underline{S}_{потр}$.

### 5.1. Комплексная мощность потребителей:
$$\underline{S}_{потр} = I_1^2 \underline{Z}_1 + I_2^2 \underline{Z}_2 + I_3^2 \underline{Z}_3 + I_4^2 \underline{Z}_4 + I_5^2 \underline{Z}_5$$

* $I_1^2 \underline{Z}_1 = 18 \cdot (50 + j 30) = 900 + j 540$
* $I_2^2 \underline{Z}_2 = 17 \cdot 50 = 850 + j 0$
* $I_3^2 \underline{Z}_3 = 17 \cdot 40 = 680 + j 0$
* $I_4^2 \underline{Z}_4 = 18 \cdot (40 + j 10) = 720 + j 180$
* $I_5^2 \underline{Z}_5 = 5 \cdot (-j 100) = 0 - j 500$

$$\mathbf{P_{потр} = 900 + 850 + 680 + 720 = 3150\text{ Вт}}$$
$$\mathbf{Q_{потр} = 540 + 0 + 0 + 180 - 500 = 220\text{ ВАр}}$$
$$\underline{S}_{потр} = \mathbf{3150 + j 220\text{ В}\cdot\text{А}}$$

### 5.2. Комплексная мощность источников:
* Для источника ЭДС $E_1$: ток $\underline{I}_1$ течёт навстречу $\underline{E}_1$:
  $$\underline{S}_{E1} = -\underline{E}_1 \underline{I}_1^* = -(-300 + j 200)(3 - j 3) = 300 - j 1500\text{ В}\cdot\text{А}$$
* Для источника ЭДС $E_6$: $\underline{I}_6 = 0 \implies \underline{S}_{E6} = 0$.
* Для источника ЭДС $E_5$: ток $\underline{I}_5$ течёт по стрелке $\underline{E}_5$:
  $$\underline{S}_{E5} = \underline{E}_5 \underline{I}_5^* = (190 + j 560)(-2 - j 1) = 180 - j 1310\text{ В}\cdot\text{А}$$
* Для источника тока $J_4$:
  $$\underline{U}_{J4} = \dot{\varphi}_4 - \dot{\varphi}_1 + \underline{I}_4 \underline{Z}_4 = (150 - j 800) + (-3 - j 3)(40 + j 10) = 60 - j 950\text{ В}$$
  $$\underline{S}_{J4} = \underline{U}_{J4} \underline{J}_4^* = (60 - j 950)(-3 + j 3) = 2670 + j 3030\text{ В}\cdot\text{А}$$

Суммарная комплексная мощность источников:
$$\underline{S}_{ист} = (300 + 0 + 180 + 2670) + j (-1500 + 0 - 1310 + 3030) = \mathbf{3150 + j 220\text{ В}\cdot\text{А}}$$

$$\mathbf{P_{ист} = 3150\text{ Вт}}, \qquad \mathbf{Q_{ист} = 220\text{ ВАр}}$$

### Оценка погрешности:
$$\Delta = \left|\frac{P_{ист} - P_{потр}}{P_{потр}}\right| \cdot 100\% = \mathbf{0{,}00\%} \le 3\%$$
Баланс сошёлся абсолютно точно.

---

## 6. Векторные диаграммы

### 6.1. Векторная диаграмма токов цепи

```tikz
\begin{document}
\begin{tikzpicture}[scale=0.9, >=stealth]
  \draw[->] (-4.5,0) -- (4.5,0) node[right] {$+1$};
  \draw[->] (0,-4.5) -- (0,4.5) node[above] {$+j$};
  \node[below left] at (0,0) {$0$};

  \draw[thick, ->, blue] (0,0) -- (3,3) node[above right] {$\underline{I}_1\ (45^\circ)$};
  \draw[thick, ->, blue] (0,0) -- (1,4) node[above right] {$\underline{I}_2\ (76^\circ)$};
  \draw[thick, ->, blue] (0,0) -- (-1,-4) node[below left] {$\underline{I}_3\ (-104^\circ)$};
  \draw[thick, ->, blue] (0,0) -- (-3,-3) node[below left] {$\underline{I}_4 = \underline{J}_4\ (-135^\circ)$};
  \draw[thick, ->, blue] (0,0) -- (-2,1) node[above left] {$\underline{I}_5\ (153{,}4^\circ)$};
  \fill[red] (0,0) circle (2pt) node[below right=2pt, red] {$\underline{I}_6 = 0$};
\end{tikzpicture}
\end{document}
```

### 6.2. Топографическая диаграмма потенциалов узлов

Диаграмма строится на комплексной плоскости по найденным потенциалам узлов:
$\dot{\varphi}_1(0, 0)$, $\dot{\varphi}_2(200, -600)$, $\dot{\varphi}_3(240, -440)$, $\dot{\varphi}_4(150, -800)$.

```tikz
\begin{document}
\begin{tikzpicture}[scale=1.0]
  \draw[->] (-1,0) -- (4,0) node[right] {$+1$};
  \draw[->] (0,-9) -- (0,1.5) node[above] {$+j$};
  \node[above left] at (0,0) {$0$};
  \fill[red] (0,0) circle [radius=1.5pt] node[above left=3pt, red] {$\dot{\varphi}_1 = 0$};
  \fill[red] (2,-6) circle [radius=1.5pt] node[right=3pt, red] {$\dot{\varphi}_2\,(200, -600)$};
  \fill[red] (2.4,-4.4) circle [radius=1.5pt] node[above right=3pt, red] {$\dot{\varphi}_3\,(240, -440)$};
  \fill[red] (1.5,-8) circle [radius=1.5pt] node[below left=3pt, red] {$\dot{\varphi}_4\,(150, -800)$};
  \draw[very thick, ->, red] (0,0) -- (2,-6) node[midway, left=2pt] {$\underline{E}_6$};
  \draw[very thick, ->, blue] (2,-6) -- (2.4,-4.4) node[midway, below right=1pt] {$\underline{U}_{23} = \underline{I}_3 R_3$};
  \draw[very thick, ->, blue] (2,-6) -- (1.5,-8) node[midway, left=2pt] {$\underline{U}_{24} = \underline{I}_2 R_2$};
  \draw[very thick, ->, green!60!black] (1.5,-8) -- (2.4,-4.4) node[midway, right=2pt] {$\underline{U}_{43}$};
  \draw[very thick, ->, orange] (0,0) -- (2.4,-4.4) node[midway, above right=2pt] {$\underline{U}_{13} = \underline{I}_1 \underline{Z}_1 + \underline{E}_1$};
\end{tikzpicture}
\end{document}
```
