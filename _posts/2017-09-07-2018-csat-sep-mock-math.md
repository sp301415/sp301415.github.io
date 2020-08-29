---
title: 2018학년도 대수능 9월 모의고사 수학 가형 정리
date: 2017-09-08
tag: [수학, 입시]
summary: 2018학년도 대수능 9월 모의고사 수학 가형 문제 중 가장 어려웠던 21, 29, 30번 풀이를 정리해 보았다.
---


## 21.

수열 $\\{a_n\\}$이

$$
a_1 = -1, \quad a_n = 2-\frac{1}{2^{n-1}} \ (n \ge 2)
$$

이다. 구간 $[-1 ,2)$에서 정의된 함수 $f(x)$가 모든 자연수 $n$에 대하여

$$
f(x) = \sin(2^n\pi x) \quad (a_n \le x \le a_{n+1})
$$

이다. $-1 < \alpha < 0$인 실수 $\alpha$에 대하여 $\displaystyle \int_\alpha^t f(x) dx=0$을 만족시키는 $t(0<t<2)$의 값의 개수가 $103$일 때, $\log_2(1-\cos(2\pi\alpha))$의 값은?


### sol.

일단 다음 사실을 확인해 보자.

$$
\begin{gather}
\sin(2^n \pi a_n) = \sin(2^n\pi (2-\frac{1}{2^{n-1}})) = 0 \\
\sin(2^n \pi a_{n+1}) = \sin(2^n\pi (2-\frac{1}{2^{n}})) =0
\end{gather}
$$

또한 $\sin(2^n\pi x)$의 주기는 $\displaystyle \frac{1}{2^n} = a_{n+1}-a_n$이다. 즉 처음 몇 개만 그래프를 그려 보면

<p class="center">
  <span id = 'sin'></span>
</p>

꼴이 됨을 알 수 있다.

이제 $F(x)=\displaystyle\int_0^x f(x) dx = -\frac{1}{2^n\pi}(\cos(2^n\pi x)-1)$를 생각해 보자.

<p class="center">
  <span id = 'coslarge'></span>
</p>

그러면 준식을 $\displaystyle \int_\alpha^t f(x) dx= F(t) - F(\alpha)=0$ 로 정리할 수 있다. 그런데 이를 만족하는 $t$가 $103$개, 즉 홀수라는 것은, 직선 $y=F(\alpha)$가 **언젠가 $F(x)$와 접한다는** 뜻이다. 조금만 더 생각을 확장해 보면, 이 직선은 한 주기당 $F$와 두 번 만나므로, 구간 $[a_{52}, a_{53}]$에서 접한다는 사실을 알 수 있다.

이 구간에서의 최댓값, 즉 $F(\alpha)$는  $\displaystyle \frac{2}{2^{52}\pi} = \frac{1}{2^{51}\pi}$이다. 또, 정의에 의해서 $\displaystyle F(\alpha) = -\frac{1}{2}(\cos(2\pi\alpha)-1)$이므로  $\displaystyle 1-\cos(2\pi\alpha) = \frac{1}{2^{50}}$이다.

$$\therefore \log_2(1-\cos(2\pi\alpha))=-50 \quad \square$$


## 29.

좌표공간에 세 점 $O(0, 0, 0), A(1, 0, 0), B(0, 0, 2)$가 있다. 점 $P$가 $\overrightarrow{OB} \cdot \overrightarrow{OP} = 0$, $\lvert \overrightarrow{OP} \rvert \le 4$를 만족시키며 움직일 때,

$$
\lvert \overrightarrow{PQ} \rvert = 1, \quad \overrightarrow{PQ} \cdot \overrightarrow{OA} \ge \frac{\sqrt 3}{2}
$$

을 만족시키는 $Q$에 대하여 $\lvert \overrightarrow{BQ} \lvert$의 최댓값과 최솟값을 각각 $M, m$이라 하자. $M+m=a+b\sqrt{5}$일 때, $6(a+b)$의 값을 구하시오. (단, $a, b$는 유리수이다.)


### sol.

$\overrightarrow{OB} \cdot \overrightarrow{OP} = 0$에서 점 $P$는 $xy$ 평면 위에 존재한다는 것을 알 수 있다. 또한 $\lvert \overrightarrow{OP} \rvert \le 4$에서 $P$는 $x^2+y^2=16$ 내부의 점임을 알 수 있다. 이 때 최대는 $P$가 원의 경계에 있고, $\overrightarrow{BP} = k\overrightarrow{PQ}$일 때이다. 즉,

<p class="center">
  <span id = 'vector-max'></span>
</p>

(이 때, 파란색 벡터는 $\overrightarrow{BP}$이고, 빨간색 벡터는 $\overrightarrow{PQ}$이다.) 이 때 $\lvert \overrightarrow{BP} \rvert = \sqrt{2^2+4^2}=2\sqrt{5}$이고 정의상 $\lvert \overrightarrow{PQ} \rvert = 1$이므로

$$
M=2\sqrt{5}+1 \label{29-1} \tag{1}
$$

또한 계산해보면 두 번째 조건을 어기지 않는다.

최소는 자명하게 $\overrightarrow{OB} = k\overrightarrow{OQ}$일 때이다. 즉,

<p class="center">
  <span id = 'vector-min'></span>
</p>

이 때 $\lvert \overrightarrow{OQ} \rvert$의 최댓값은 두 번째 조건에 의해 $\displaystyle \frac{1}{2}$이다. 따라서

$$
m=2-\frac{1}{2} = \frac{3}{2} \label{29-2}\tag{2}
$$

$\eqref{29-1}, \eqref{29-2}$에 의해 $\displaystyle M+m = \frac{5}{2} + 2\sqrt{5}$이므로 $\displaystyle a = \frac{5}{2}, b= 2$

$$\therefore 6(a+b)= 27 \quad \square$$


## 30.

함수 $f(x) = \ln(e^x + 1) +2e^x$에 대해서 이차함수 $g(x)$와 실수 $k$는 다음 조건을 만족시킨다.

------

 함수 $h(x) = \lvert g(x) - f(x-k) \rvert$는 $x=k$에서 최솟값 $g(k)$를  갖고, 닫힌 구간 $[k-1, k+1]$에서 최댓값 $\displaystyle 2e+\ln \left( \frac{1+e}{\sqrt 2}\right)$를 갖는다.

------

$\displaystyle g^\prime \left(k-\frac{1}{2}\right)$의 값을 구하시오. (단, $\displaystyle \frac{5}{2} < e <3$이다.)


### sol.

먼저 $\displaystyle f^\prime(x) =  \frac{e^x}{e^x + 1} + 2e^x > 0$이므로 $f$는 증가한다. $h(k)= \lvert g(k)-f(0) \rvert =g(k)$인데 $f(0) \neq 0$이므로 $f(0) > g(k)$이고 $f(0) - g(k) = g(k)$이다. 따라서

$$
g(k) = \frac{1}{2}f(0) = \frac{1}{2}\ln2+1 \label{30-1}\tag{1}
$$

한편 $h(k) \neq 0$이므로 최소를 가지려면  $h^\prime (k) = 0$이어야 한다. 즉,

$$
\begin{gather}
h^\prime(k) = g^\prime(k) - f^\prime(0) = g^\prime (k) - \left( \frac{5}{2} \right) = 0 \\
g^\prime(k) = \frac{5}{2} \label{30-2}\tag{2}
\end{gather}
$$

또한 이를 통해  $g$의 그래프 개형을 추측해볼 수 있다. $g$와 $f$가 $x=k$ 이외의 점에서 만난다면 그 점에서 $h(x)=0$, 즉 최솟값이 되므로 조건에 모순이다. 따라서 $g$와 $f$는 만나지 않거나 $x=k$에서만 만나야 한다. 그런데 $f$가 증가함수이므로 $g$의 최고차항의 계수가 양수이면 둘 다 충족시키지 못한다. 따라서 $g$의 최고차항의 계수는 음수, 즉 아래로 볼록이다.

이제 $\eqref{30-1}$을 고려하여 $\displaystyle g^\prime (x) = 2a(x-k)+\frac{5}{2}$라 두면 $\displaystyle g(x) = a(x-k)^2 + \frac{5}{2}x + C$이다. 또한 간단한 계산을 통해  $h(k+1) > h(k-1)$임을 알 수 있고 구간 $[k-1, k+1]$에서 극댓값이 존재하지 않으므로($g$의 극솟값이 존재하지 않음을 상기하면 된다.) 이 구간에서의 $h$의 최댓값은 $h(k+1)$이다. 즉

$$
\begin{align}
h(k+1) &= f(1)-g(k+1)  \\
&= \ln(e+1)+2e-g(k+1) \\
&= 2e+\ln\left(\frac{1+e}{\sqrt2}\right) \\
\end{align}
$$

정리하면

$$
g(k+1) = \frac{1}{2}\ln 2 \label{30-3}\tag{3}
$$

이제 $\eqref{30-2}, \eqref{30-3}$을 대입하면

$$
\begin{gather}
g(k+1) = a+\frac{5}{2}k +\frac{5}{2}+C = \frac{1}{2}\ln2 \\
g(k) = \frac{5}{2}k+C= \frac{1}{2}\ln 2 + 1
\end{gather}
$$

에서 정리하면 $\displaystyle a=-\frac{7}{2}$이다.

$$\therefore g^\prime \left(k-\frac{1}{2}\right) = -a + \frac{5}{2} = 6 \quad \square$$


<!--graph scripts-->
<script src="https://d3js.org/d3.v3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/function-plot/1.18.1/function-plot.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>
<script>
functionPlot({

    target: '#sin',
    disableZoom: true,
    xAxis: {domain: [-1.3, 2.3]},
    yAxis: {domain: [-1.5, 1.5]},
    data: [{
      fn: 'sin(2*PI*x)',
      range: [-1, 1],
    },
    {
      fn: 'sin(4*PI*x)',
      range: [1, 3/2],
    },
    {
      fn: 'sin(8*PI*x)',
      range: [3/2, 7/4],
    },
    {
      fn: 'sin(16*PI*x)',
      range: [7/4, 15/8],
    },
    {
      fn: 'sin(32*PI*x)',
      range: [15/8, 31/16],
    },
    {
      fn: 'sin(64*PI*x)',
      range: [31/16, 63/32],
    }]
  })
</script>
<script>
functionPlot({
    target: '#coslarge',
    disableZoom: true,
    xAxis: {domain: [-0.3, 2.3]},
    yAxis: {domain: [-0.4, 0.4]},
    data: [{
      fn: '(1/(2*PI)) * (-cos(2*PI*x)+1)',
      range: [-1, 1],
    },
    {
      fn: '(1/(4*PI)) * (-cos(4*PI*x)+1)',
      range: [1, 3/2],
    },
    {
      fn: '(1/(8*PI)) * (-cos(8*PI*x)+1)',
      range: [3/2, 7/4],
    },
    {
      fn: '(1/(16*PI)) * (-cos(16*PI*x)+1)',
      range: [7/4, 15/8],
    },
    {
      fn: '(1/(32*PI)) * (-cos(32*PI*x)+1)',
      range: [15/8, 31/16],
    },
    {
      fn: '(1/(64*PI)) * (-cos(64*PI*x)+1)',
      range: [31/16, 63/32],
    }]
  })
</script>
<script>
functionPlot({
    target: '#vector-max',
    xAxis: {domain: [-1, 6], label: 'x축'},
    yAxis: {domain: [-1, 3], label: 'z축'},
    disableZoom: true,
    grid: true,
    data: [
    {
      vector: [4, -2],
      offset: [0, 2],
      graphType: 'polyline',
      fnType: 'vector'
    },
    {
      vector: [2 * Math.sqrt(0.2), -Math.sqrt(0.2)],
      offset: [4, 0],
      graphType: 'polyline',
      fnType: 'vector'
    }]
  })
</script>
<script>
functionPlot({
    target: '#vector-min',
    xAxis: {domain: [-3, 3], label: 'x축'},
    yAxis: {domain: [-1, 3], label: 'z축'},
    disableZoom: true,
    grid: true,
    data: [
    {
      vector: [- 0.5 * Math.sqrt(3), -2],
      offset: [0, 2],
      graphType: 'polyline',
      fnType: 'vector'
    },
    {
      vector: [0.5 * Math.sqrt(3), 0.5],
      offset: [- 0.5 * Math.sqrt(3), 0],
      graphType: 'polyline',
      fnType: 'vector'
    }]
  })
</script>
