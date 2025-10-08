# LaTeX 가이드

## 1. LaTeX란?

**LaTeX**(레이텍, 라텍)는 고품질의 수학 수식과 과학 문서를 작성하기 위한 조판 시스템입니다.

### 주요 특징
- 아름다운 수식 표현
- 자동 번호 매기기 및 상호 참조
- 논문, 보고서, 프레젠테이션 작성
- 일관된 서식 유지
- 복잡한 문서 구조 지원

### Obsidian에서 LaTeX 사용
Obsidian은 **MathJax**를 사용하여 LaTeX 수식을 렌더링합니다.

- **인라인 수식**: `$수식$` 
- **블록 수식**: `$$수식$$` 또는 `$$\n수식\n$$`

---

## 2. 기본 수식 문법

### 2.1 인라인 vs 블록 수식

**인라인 수식** (문장 내부):
```latex
이차방정식의 해는 $x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}$ 입니다.
```
렌더링: 이차방정식의 해는 $x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}$ 입니다.

**블록 수식** (독립된 줄):
```latex
$$
E = mc^2
$$
```
렌더링:
$$
E = mc^2
$$

---

## 3. 기본 연산자

### 3.1 산술 연산

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `a + b` | $a + b$ | 덧셈 |
| `a - b` | $a - b$ | 뺄셈 |
| `a \times b` | $a \times b$ | 곱셈 (×) |
| `a \cdot b` | $a \cdot b$ | 곱셈 (·) |
| `a \div b` | $a \div b$ | 나눗셈 |
| `\frac{a}{b}` | $\frac{a}{b}$ | 분수 |
| `\dfrac{a}{b}` | $\dfrac{a}{b}$ | 큰 분수 (display style) |

### 3.2 위첨자와 아래첨자

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `x^2` | $x^2$ | 위첨자 (제곱) |
| `x^{10}` | $x^{10}$ | 위첨자 (여러 자리) |
| `x_i` | $x_i$ | 아래첨자 |
| `x_{ij}` | $x_{ij}$ | 아래첨자 (여러 자리) |
| `x_i^2` | $x_i^2$ | 위/아래 동시 |

### 3.3 근호와 절댓값

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\sqrt{x}` | $\sqrt{x}$ | 제곱근 |
| `\sqrt[3]{x}` | $\sqrt[3]{x}$ | 세제곱근 |
| `\sqrt[n]{x}` | $\sqrt[n]{x}$ | n제곱근 |
| `\lvert x \rvert` | $\lvert x \rvert$ | 절댓값 |

---

## 4. 그리스 문자

### 4.1 소문자 그리스 문자

| LaTeX | 렌더링 | LaTeX | 렌더링 |
|-------|--------|-------|--------|
| `\alpha` | $\alpha$ | `\nu` | $\nu$ |
| `\beta` | $\beta$ | `\xi` | $\xi$ |
| `\gamma` | $\gamma$ | `\pi` | $\pi$ |
| `\delta` | $\delta$ | `\rho` | $\rho$ |
| `\epsilon` | $\epsilon$ | `\sigma` | $\sigma$ |
| `\zeta` | $\zeta$ | `\tau` | $\tau$ |
| `\eta` | $\eta$ | `\phi` | $\phi$ |
| `\theta` | $\theta$ | `\chi` | $\chi$ |
| `\kappa` | $\kappa$ | `\psi` | $\psi$ |
| `\lambda` | $\lambda$ | `\omega` | $\omega$ |
| `\mu` | $\mu$ | | |

### 4.2 대문자 그리스 문자

| LaTeX | 렌더링 | LaTeX | 렌더링 |
|-------|--------|-------|--------|
| `\Gamma` | $\Gamma$ | `\Sigma` | $\Sigma$ |
| `\Delta` | $\Delta$ | `\Phi` | $\Phi$ |
| `\Theta` | $\Theta$ | `\Psi` | $\Psi$ |
| `\Lambda` | $\Lambda$ | `\Omega` | $\Omega$ |
| `\Pi` | $\Pi$ | | |

### 4.3 변형 그리스 문자

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\varepsilon` | $\varepsilon$ | 변형 입실론 |
| `\vartheta` | $\vartheta$ | 변형 세타 |
| `\varphi` | $\varphi$ | 변형 파이 |
| `\varpi` | $\varpi$ | 변형 파이 |
| `\varrho` | $\varrho$ | 변형 로 |

---

## 5. 합, 곱, 적분

### 5.1 합 (Sum)

```latex
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$
```
렌더링:
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

### 5.2 곱 (Product)

```latex
$$
\prod_{i=1}^{n} i = n!
$$
```
렌더링:
$$
\prod_{i=1}^{n} i = n!
$$

### 5.3 적분 (Integral)

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\int` | $\int$ | 적분 |
| `\int_a^b` | $\int_a^b$ | 정적분 |
| `\iint` | $\iint$ | 이중적분 |
| `\iiint` | $\iiint$ | 삼중적분 |
| `\oint` | $\oint$ | 선적분 |

**예시:**
```latex
$$
\int_0^{\infty} e^{-x^2} dx = \frac{\sqrt{\pi}}{2}
$$
```

### 5.4 극한 (Limit)

```latex
$$
\lim_{x \to 0} \frac{\sin x}{x} = 1
$$
```

---

## 6. 행렬과 벡터

### 6.1 다양한 괄호 행렬

| 환경 | LaTeX 코드 | 설명 |
|------|-----------|------|
| 소괄호 | `\begin{pmatrix}...\end{pmatrix}` | ( ) |
| 대괄호 | `\begin{bmatrix}...\end{bmatrix}` | [ ] |
| 중괄호 | `\begin{Bmatrix}...\end{Bmatrix}` | { } |
| 수직선 | `\begin{vmatrix}...\end{vmatrix}` | \| \| (행렬식) |

### 6.2 행렬 예시

```latex
$$
M = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$
```

### 6.3 벡터

```latex
$$
\vec{v} = \begin{pmatrix}
x \\ y \\ z
\end{pmatrix}
$$
```

---

## 7. 괄호와 구분자

### 7.1 기본 괄호

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `(x)` | $(x)$ | 소괄호 |
| `[x]` | $[x]$ | 대괄호 |
| `\{x\}` | $\{x\}$ | 중괄호 |
| `\langle x \rangle` | $\langle x \rangle$ | 각괄호 |
| `\lfloor x \rfloor` | $\lfloor x \rfloor$ | 내림 |
| `\lceil x \rceil` | $\lceil x \rceil$ | 올림 |

### 7.2 크기 자동 조절

```latex
$$
\left( \frac{a}{b} \right) \quad \text{vs} \quad ( \frac{a}{b} )
$$
```

`\left`와 `\right`를 사용하면 괄호 크기가 자동으로 조절됩니다.

---

## 8. 수식 정렬

### 8.1 정렬 환경 (align)

```latex
$$
\begin{align}
f(x) &= x^2 + 2x + 1 \\
&= (x+1)^2
\end{align}
$$
```

### 8.2 경우 나누기 (cases)

```latex
$$
f(x) = \begin{cases}
x^2 & \text{if } x \geq 0 \\
-x^2 & \text{if } x < 0
\end{cases}
$$
```

---

## 9. 관계 연산자

### 9.1 비교 연산자

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `=` | $=$ | 같음 |
| `\neq` | $\neq$ | 같지 않음 |
| `<` | $<$ | 작음 |
| `>` | $>$ | 큼 |
| `\leq` | $\leq$ | 작거나 같음 |
| `\geq` | $\geq$ | 크거나 같음 |
| `\ll` | $\ll$ | 매우 작음 |
| `\gg` | $\gg$ | 매우 큼 |
| `\approx` | $\approx$ | 근사 |
| `\equiv` | $\equiv$ | 합동 |

### 9.2 집합 연산자

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\in` | $\in$ | 속함 |
| `\notin` | $\notin$ | 속하지 않음 |
| `\subset` | $\subset$ | 부분집합 |
| `\cap` | $\cap$ | 교집합 |
| `\cup` | $\cup$ | 합집합 |
| `\emptyset` | $\emptyset$ | 공집합 |

---

## 10. 특수 기호

### 10.1 화살표

| LaTeX | 렌더링 | LaTeX | 렌더링 |
|-------|--------|-------|--------|
| `\rightarrow` | $\rightarrow$ | `\leftarrow` | $\leftarrow$ |
| `\Rightarrow` | $\Rightarrow$ | `\Leftarrow` | $\Leftarrow$ |
| `\uparrow` | $\uparrow$ | `\downarrow` | $\downarrow$ |
| `\to` | $\to$ | `\mapsto` | $\mapsto$ |

### 10.2 수학 기호

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\infty` | $\infty$ | 무한대 |
| `\partial` | $\partial$ | 편미분 |
| `\nabla` | $\nabla$ | 델 (그래디언트) |
| `\pm` | $\pm$ | 플러스마이너스 |
| `\cdots` | $\cdots$ | 가운데 점들 |
| `\ldots` | $\ldots$ | 아래 점들 |
| `\vdots` | $\vdots$ | 수직 점들 |
| `\ddots` | $\ddots$ | 대각 점들 |

---

## 11. 함수와 연산

### 11.1 표준 함수

| LaTeX | 렌더링 | LaTeX | 렌더링 |
|-------|--------|-------|--------|
| `\sin x` | $\sin x$ | `\cos x` | $\cos x$ |
| `\tan x` | $\tan x$ | `\log x` | $\log x$ |
| `\ln x` | $\ln x$ | `\exp x` | $\exp x$ |
| `\max` | $\max$ | `\min` | $\min$ |

### 11.2 미분과 편미분

```latex
$$
\frac{dy}{dx}, \quad \frac{\partial f}{\partial x}, \quad \nabla f
$$
```

### 11.3 벡터 표기

| LaTeX | 렌더링 | 설명 |
|-------|--------|------|
| `\vec{v}` | $\vec{v}$ | 벡터 (화살표) |
| `\mathbf{v}` | $\mathbf{v}$ | 벡터 (볼드) |
| `\hat{v}` | $\hat{v}$ | 단위벡터 |
| `\bar{x}` | $\bar{x}$ | 평균 |
| `\tilde{x}` | $\tilde{x}$ | 물결표 |
| `\dot{x}` | $\dot{x}$ | 시간 미분 |
| `\ddot{x}` | $\ddot{x}$ | 이계 시간 미분 |

---

## 12. 텍스트 삽입

### 12.1 수식 내 텍스트

```latex
$$
f(x) = \begin{cases}
x^2 & \text{if } x > 0 \\
0 & \text{otherwise}
\end{cases}
$$
```

### 12.2 다양한 텍스트 스타일

| LaTeX | 설명 | 예시 |
|-------|------|------|
| `\text{...}` | 일반 텍스트 | $\text{Normal}$ |
| `\mathbf{...}` | 볼드체 | $\mathbf{Bold}$ |
| `\mathit{...}` | 이탤릭체 | $\mathit{Italic}$ |
| `\mathcal{...}` | 필기체 | $\mathcal{SCRIPT}$ |
| `\mathbb{...}` | 칠판 볼드 | $\mathbb{R}$ |

---

## 13. 반도체/물리학 수식 예제

### 13.1 옴의 법칙

```latex
$$
V = IR, \quad R = \frac{\rho L}{A}
$$
```

### 13.2 면저항

```latex
$$
R_s = \frac{\rho}{t}, \quad \frac{1}{R_s} = \frac{1}{R_{s1}} + \frac{1}{R_{s2}}
$$
```

### 13.3 반도체 캐리어 농도

```latex
$$
n = N_C \exp\left(-\frac{E_C - E_F}{k_B T}\right)
$$
```

### 13.4 확산 방정식

```latex
$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$
```

### 13.5 전기장과 전위

```latex
$$
\vec{E} = -\nabla V
$$
```

### 13.6 맥스웰 방정식

```latex
$$
\begin{aligned}
\nabla \cdot \vec{E} &= \frac{\rho}{\epsilon_0} \\
\nabla \cdot \vec{B} &= 0 \\
\nabla \times \vec{E} &= -\frac{\partial \vec{B}}{\partial t} \\
\nabla \times \vec{B} &= \mu_0 \vec{J} + \mu_0 \epsilon_0 \frac{\partial \vec{E}}{\partial t}
\end{aligned}
$$
```

---

## 14. 복잡한 수식 예제

### 14.1 슈뢰딩거 방정식

```latex
$$
i\hbar\frac{\partial}{\partial t}\Psi(\vec{r},t) = \left[-\frac{\hbar^2}{2m}\nabla^2 + V(\vec{r},t)\right]\Psi(\vec{r},t)
$$
```

### 14.2 푸리에 변환

```latex
$$
F(\omega) = \int_{-\infty}^{\infty} f(t) e^{-i\omega t} dt
$$
```

### 14.3 가우스 적분

```latex
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

---

## 15. 실용 팁

### 15.1 공백 조절

| LaTeX | 간격 | 설명 |
|-------|------|------|
| `\,` | 작은 공백 | 얇은 공백 |
| `\:` | 중간 공백 | 중간 공백 |
| `\;` | 큰 공백 | 두꺼운 공백 |
| `\quad` | 1em 공백 | 큰 공백 |
| `\qquad` | 2em 공백 | 매우 큰 공백 |

### 15.2 번호 매기기

```latex
$$
E = mc^2 \tag{1}
$$
```

### 15.3 복잡한 수식 분할

```latex
$$
\begin{split}
(a + b)^4 &= (a + b)^2 (a + b)^2 \\
&= (a^2 + 2ab + b^2)(a^2 + 2ab + b^2) \\
&= a^4 + 4a^3b + 6a^2b^2 + 4ab^3 + b^4
\end{split}
$$
```

---

## 16. 자주 하는 실수

### 16.1 중괄호 누락

❌ **잘못된 예:** `$x^10$` → 첫 번째 1만 위첨자  
✅ **올바른 예:** `$x^{10}$` → 전체가 위첨자

### 16.2 특수 문자 이스케이프

❌ **잘못된 예:** `${x}$`  
✅ **올바른 예:** `$\{x\}$`

### 16.3 행렬에서 &와 \\\\ 사용

❌ **잘못된 예:**
```latex
\begin{matrix}
a, b \\
c, d
\end{matrix}
```

✅ **올바른 예:**
```latex
\begin{matrix}
a & b \\
c & d
\end{matrix}
```

---

## 17. 빠른 참조 카드

### 자주 사용하는 템플릿

**분수:**
```latex
$\frac{분자}{분모}$
```

**제곱근:**
```latex
$\sqrt{x}$ 또는 $\sqrt[n]{x}$
```

**합:**
```latex
$\sum_{i=1}^{n} a_i$
```

**적분:**
```latex
$\int_{a}^{b} f(x) dx$
```

**극한:**
```latex
$\lim_{x \to \infty} f(x)$
```

**행렬:**
```latex
$\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}$
```

**경우 나누기:**
```latex
$f(x) = \begin{cases}
식1 & \text{조건1} \\
식2 & \text{조건2}
\end{cases}$
```

---

## 18. 온라인 도구

- **[Detexify](http://detexify.kirelabs.org/)** - 기호를 그려서 LaTeX 코드 찾기
- **[LaTeX Equation Editor](https://latex.codecogs.com/eqneditor/editor.php)** - 온라인 수식 편집기
- **[MathJax Docs](https://docs.mathjax.org/)** - MathJax 공식 문서
- **[KaTeX Supported Functions](https://katex.org/docs/supported.html)** - KaTeX 지원 함수 목록

---

## 19. 연습 문제

### 문제 1: 이차방정식의 해
다음 수식을 LaTeX로 작성하세요:
> x의 값은 -b ± √(b²-4ac) 를 2a로 나눈 값이다.

<details>
<summary>정답</summary>

```latex
$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$
```
</details>

### 문제 2: 행렬
3×3 단위행렬을 LaTeX로 작성하세요.

<details>
<summary>정답</summary>

```latex
$$
I = \begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$
```
</details>

### 문제 3: 조건부 함수
절댓값 함수를 경우 나누기로 표현하세요.

<details>
<summary>정답</summary>

```latex
$$
|x| = \begin{cases}
x & \text{if } x \geq 0 \\
-x & \text{if } x < 0
\end{cases}
$$
```
</details>

---

**작성일**: 2025-10-06  
**버전**: 1.0  
**태그**: #latex #수식 #obsidian #mathjax
