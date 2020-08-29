---
title: 격자 암호(Lattice Cryptography)의 소개
date: 2018-02-04
tag: [수학, 암호학]
summary: 최근 Post-Quantam Cryptography에서 각광받는 격자 암호에 대해 알아보자.
---

> 아직 업데이트 중입니다.

## Lattice

### Lattice and Basis

추상적으로, $\mathbb{R}^n$의 이산적 덧셈부분군(discrete additive subgroup) $\mathcal{L}$을 **격자**(Lattice)라고 한다. *덧셈부분군*이라는 것은 $\mathcal{L}$이 $\mathbb{R}^n$의 부분군(subgroup)이며 덧셈에 대해 닫혀있다는 것을 뜻한다. *이산적*이라는 것은 모든 $\mathbf{v} \in \mathcal{L}$에 대하여 적당한 양수 $\varepsilon > 0$이 존재하여

$$
\mathcal{L} \cap \{ \mathbf{w} \in \mathbb{R}^n : \lVert \mathbf{v} - \mathbf{w} \rVert < \varepsilon \} = \{ \mathbf{v} \}
$$

가 성립함을 뜻한다.
단번에 이 말을 이해하기엔 쉽지 않으니 차근차근 논리를 전개시켜 나가자.

직관적으로, 일차독립인 벡터 $\mathcal{B}=(\mathbf{b}_1, \mathbf{b}_2, \cdots, \mathbf{b}_n) \in \mathbb{R}^n$에 대해

$$
\mathcal{L} = \mathcal{B} \cdot \mathbb{Z}^n =\left\{ \sum_{i=1}^n z_i \mathbf{b}_i : z_i \in \mathbb{Z} \right\}
$$

인 공간 $\mathcal{L}$을 $\mathbb{R}^n$의 격자라 하고, $\mathcal{L}$을 생성하는 집합 $\mathcal{B}$를 $\mathcal{L}$의 **기저**(Basis)라 한다. 이 정의는 처음의 정의와 동치이다. 이를 증명하기 전에 먼저 격자의 기본적인 성질을 살펴보도록 하자.

다시 $\mathcal{L}$ 위의 벡터집합 $\mathcal{V} = (\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n)$를 생각하자. 기저는 $\mathcal{L}$을 생성하므로 $\mathcal{V}$를 $\mathcal{B}$의 (정수 계수) 일차결합으로 나타낼 수 있다.

$$
\begin{gather}
\mathbf{v_1} = z_{11}\mathbf{b}_1 + z_{12}\mathbf{b}_2 + \cdots + z_{1n}\mathbf{b}_n\\
\mathbf{v_2} = z_{21}\mathbf{b}_1 + z_{22}\mathbf{b}_2 + \cdots +  z_{2n}\mathbf{b}_n\\
\vdots&  \\
\mathbf{v_n} = z_{n1}\mathbf{b}_1 + z_{n2}\mathbf{b}_2 + \cdots + z_{nn}\mathbf{b}_n\\
\end{gather}
$$

이는 행렬

$$
U =
\begin{pmatrix}
z_{11} & z_{12} & \cdots & z_{1n} \\
z_{21} & z_{22} & \cdots & z_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
z_{n1} & z_{n2} & \cdots & z_{nn} \\
\end{pmatrix}
$$

을 이용하여 $\mathcal{V} = U \cdot \mathcal{B}$로 표현할 수도 있다. 이제 반대로 $\mathcal{B}$를 $\mathcal{V}$로 표현해 보자. $\mathcal{V}$를 $\mathcal{L}$의 또다른 기저로 간주하는 것이다. 즉,

$$
\mathcal{B} = U^{-1} \cdot \mathcal{V}
$$

을 생각해 보자. 이제

$$
\det(U)\det(U^{-1}) = \det({UU^{-1}}) = \det(I)=1
$$

이므로, $\det{U} = \pm 1$이어야 한다.[^1] 역으로 격자의 한 기저에다 $\det{U} = \pm 1$인 정수 행렬 $U$를 곱하여 무한히 많은 기저를 구할 수 있음을 알 수 있다! 이는 아주 중요한 성질이며 앞으로도 자주 써먹을 것이다.
이러한 행렬 $U$의 집합을 **General Linear Group**(Over $\mathbb{Z}$)이라 하며 $\operatorname{GL}(n, \mathbb{Z})$라 쓴다.

### Fundemental Domain and volume of a Lattice

격자는 이산적이기에, 체스판이 단위 칸으로 쪼개지듯 격자도 단위 칸으로 쪼개진다. 이 단위 칸을

$$
\mathcal{P}(\mathcal{B}) := \left\{ \sum_{i=1}^n c_i\mathbf{b}_i : c_i \in \left[ -\frac{1}{2}, \frac{1}{2} \right)\right\}
$$

이라 정의하고 **Fundemental Domain** (또는 Fundemental Parallelpiped)이라 한다. 책에 따라서는 위의 정의 대신에

$$
\mathcal{P}(\mathcal{B}) := \left\{ \sum_{i=1}^n c_i\mathbf{b}_i : c_i \in \left[ 0, 1 \right)\right\}
$$

을 사용하는 경우도 있다. 이는 기준을 어디로 잡느냐의 문제이고, 본질적인 정의의 차이는 없다. 이하로는 더 아름다운 후자의 정의를 쓰겠다.
 또한, 자명하게 $\mathcal{P}$를 모으면 전체집합 $\mathbb{R}^n$이 된다. 즉

$$
\mathbb{R}^n = \bigcup_{\mathbf{v} \in \mathcal{L}} (\mathbf{v}+\mathcal{P}(\mathcal{B}))
$$

가 성립한다. $\mathcal{P}$가 중요한 이유는 그 부피, 즉 $\operatorname{vol}(\mathcal{P})$가 어떤 격자 $\mathcal{L}$에 대해 불변량(invariant)이기 때문이다. 이제부터 살펴보자.

$\mathcal{L}$의 한 기저 $\mathcal{B}$의 원소를 $\mathbf{b}_i = (v\_{i1}, v\_{i2}, \cdots, v\_{in})$이라고 하자. 이제 이를 행렬로 표현해보면

$$
A =
\begin{pmatrix}
v_{11} & v_{12} & \cdots & v_{1n} \\
v_{21} & v_{22} & \cdots & v_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
v_{n1} & v_{n2} & \cdots & v_{nn} \\
\end{pmatrix}
$$

이제 큐브 $C_n = [0, 1)^n$을 생각하면

$$
\mathcal{P} = C_n \cdot A
$$

로 표현할 수 있다. 이제 $\operatorname{vol}(\mathcal{P})$를 구해보면

$$
\begin{align}
\operatorname{vol}(\mathcal{P}) &= \int_\mathcal{P} dx_1 dx_2 \cdots dx_n \\
&= \int_{C_n \cdot A} dx_1 dx_2 \cdots dx_n \\
&= \int_{C_n} \lvert \det(A) \rvert \ dx_1 dx_2 \cdots dx_n \\
&= \lvert \det(A) \rvert \operatorname{vol}(C_n) \\
&= \lvert \det(A) \rvert
\end{align}
$$

그런데 서로 다른 두 기저는 행렬식이 $\pm 1$인 행렬을 곱해서 얻어낼 수 있으므로, 결국 어떤 $\mathcal{L}$에 대해 $\lvert \det(A) \rvert$의 값은 일정하다. 즉, $\lvert \det(A) \rvert = \operatorname{vol}(\mathcal{P})$는 $\mathcal{L}$의 불변량이다. 보통 이 값을 격자 $\mathcal{L}$의 **행렬식**(Determinant)라고 하며 $\det(\mathcal{L})$로 표기한다. 즉

$$
\det(\mathcal{L}) := \operatorname{vol}(\mathcal{P})
$$

이다.

한편 $\det(\mathcal{L})$의 상한은 아래의 **Hadamard's Inequality**로 구할 수 있다. 격자 $\mathcal{L}$과 그 기저 $\mathcal{B}=(\mathbf{b}_1, \mathbf{b}_2, \cdots, \mathbf{b}_n)$에 대해

$$
\det(\mathcal{L}) = \operatorname{vol}(\mathcal{P}) \le \lVert \mathbf{b}_1 \rVert \lVert \mathbf{b}_2 \rVert \cdots \lVert \mathbf{b}_n \rVert
$$

이 때 부등호는 기저 $\mathcal{B}$가 서로 수직이면 성립한다. 반대로 말하면, 위 부등식이 등식에 가까워질수록 기저는 수직에 가까워진다. 이를 이용하여 기저가 수직인 정도를 구할 수 있는데, 이를 **Hadamard Ratio**라고 한다. 자세한 것은 다음 장에서 다룬다.


## Lattice-Based Problems & Algorithms

### Lattice-Based Problems

격자 암호에 쓰이는 문제에는 **SVP**(Shortest Vector Problem), **CVP**(Closest Vector Problem) 및 그 변종들이 있다. 이들의 공통점은 모두 격자 위의 *짧은 벡터*에 대한 문제라는 점이다.

* **SVP**(Shortest Vector Problem): 격자 위의 가장 짧은 (0이 아닌) 벡터는 무엇인가?
* **CVP**(Closest Vector Problem): 주어진 벡터와 가장 가까운 격자 위의 벡터는 무엇인가?

이들의 변종 또한 중요하게 다루어진다. 대표적으로 다음의 예가 있다.

* **SBP**(Shortest Basis Problem): 주어진 격자의 가장 작은 기저는 무엇인가?
* **apprSVP**(Approximate SVP): 주어진 상수 $\gamma$에 대해서 격자 위의 가장 짧은 벡터의 $\gamma$배보다 짧은 격자 위의 벡터는 무엇인가?
* **apprCVP**(Approximate CVP): **apprSVP**의 **CVP** 버전.

이제 본격적으로 수학적 background를 알아보자.

### Introduction to Short Vector on Lattice

격자 $\mathcal{L}$ 위에 있는 벡터의 길이의 최솟값을 생각하자. 이 길이를 $\lambda_1$[^2]이라고 쓰고 다음과 같이 정의한다.

$$
\lambda_1(\mathcal{L}) := \min_{\textbf{v} \in \mathcal{L} \setminus \{0\}} \lVert \mathbf{v} \rVert
$$

즉, $\lambda_1(\mathcal{L})$은 직관적으로 "격자 $\mathcal{L}$위의 가장 짧은 벡터의 길이"를 말한다. 이제 이 값의 상한과 하한을 살펴보자.

상한을 구하기 위해서는 **민코프스키의 정리**(Minkowski's Theorem)를 알아야 한다. 민코프스키의 정리란, 볼록중점대칭집합  $S$에 대하여 $\operatorname{vol}(S) > 2^n \det(\mathcal{L})$이면 $S$는 적어도 하나의 $0$이 아닌 격자점을 포함한다는 것이다.

증명은 간단하다. 먼저 $S^\prime = S/2$라 하자. 그러면 자명히 $\operatorname{vol}(S^\prime) >  \det(\mathcal{L})$이다. 이제 $S^\prime$을 $\mathcal{P}$로 "쪼개" 보자. 즉, 모든 $\mathbf{v} \in \mathcal{L}$에 대해 $S^\prime_\mathbf{v} = S^\prime \cap (\mathbf{v} + \mathcal{P})$를 생각하자. 평행이동은 넓이를 보존하므로 $\operatorname{vol}(S^\prime_\mathbf{v}) = \operatorname{vol}(S^\prime_\mathbf{v} - \mathbf{v})$이다. 그런데

$$
\sum_{\mathbf{v} \in \mathcal{L}} \mathrm{vol}(S^\prime_\mathbf{v} - \mathbf{v}) = \sum_{\mathbf{v} \in \mathcal{L}} \mathrm{vol}(S^\prime_\mathbf{v}) =  \mathrm{vol}(S^\prime) > \det (\mathcal{L})
$$

이므로 각 $S^\prime_\mathbf{v} - \mathbf{v}$는 "겹친다". 즉,

$$
(S^\prime_\mathbf{u} - \mathrm{u}) \cap (S^\prime_\mathbf{v} - \mathrm{v}) \neq \varnothing
$$

인 $\mathbf{u}, \mathbf{v} \in \mathcal{L}$가 존재한다. 이제 $\mathbf{x} = \mathbf{z} + \mathbf{u}$, $\mathbf{y} = \mathbf{z}+\mathbf{y} \in S^\prime$을 생각하면 $\mathbf{x} - \mathbf{y} = \mathbf{u}-\mathbf{v} \in \mathcal{L}$이다. $S^\prime, S$의 정의에 의해 $2\mathbf{x}, -2\mathbf{y} \in S$이며 그 중점인 $\mathbf{x}-\mathbf{y} \in S$이다. 이제 $\mathcal{L}$에도 속하고 $S$에도 속한 점을 찾아냈다!

이제 따름정리인 **민코프스키의 첫 번째 정리**(Minkowski's First Theorem)을 살펴보자. 이 정리는 $\lambda_1$의 상한을 정해준다.

$$
\lambda_1(\mathcal{L}) \le \sqrt{n}\det(\mathcal{L})^{1/n}
$$

이는 쉽게 증명된다.

- 원점을 중심으로 하고 반지름이 $\sqrt{n}(\det\mathcal{L})^{1/n}$인 공(ball)은 볼록이며 중점대칭이다.
- 모서리가 $2(\det\mathcal{L})^{1/n}$인 큐브는 위의 공을 포함한다.

따라서 공의 부피는 $(2(\det\mathcal{L})^{1/n})^n = 2^n\det\mathcal{L}$보다 작으며, 민코프스키 정리에 의해서 원점이 아닌 격자점을 하나 이상 포함한다. 이제 그 중 원점과 가장 가까운 격자점과 원점 사이의 거리가 바로 $\lambda_1$이다.

이를 확장시킨 것이 **Hermite's Constant** $\gamma_n$이다. 이는 **Hermite's Inequality**

$$
\lambda_1(\mathcal{L})^2 \le \gamma_n \det(\mathcal{L})^{2/n}
$$

이 성립하게 하는 상수 $\gamma_n$의 최댓값으로 정의된다.[^3] 민코프스키의 첫 번째 정리는 $\gamma_n \le n$임을 시사하는 셈이다. 정확한 $\gamma_n$의 값은 구하기 어려우며, $n=2, 3, 4, 5, 6, 7, 8, 24$인 경우의 값만 알려져 있다.

한편, Hermite's Inequality를 변형시키면

$$
\lVert \mathbf{v}_1 \rVert \lVert \mathbf{v}_2 \rVert \cdots \lVert \mathbf{v}_n \rVert \le n^{\frac{n}{2}} \det(\mathcal{L})
$$

앞 장에서 다룬 Hadamard's Inequality와 Hermite's Inequality를 합치면

$$
\det(\mathcal{L}) \le \lVert \mathbf{b}_1 \rVert \lVert \mathbf{b}_2 \rVert \cdots \lVert \mathbf{b}_n \rVert \le n^{\frac{n}{2}}\det(\mathcal{L})
$$

이제 **Hadamard's Ratio**를 다음과 같이 정의하자.

$$
\mathcal{H}(\mathcal{B}) := \left(\frac{\det(\mathcal{L})}{\lVert \mathbf{b}_1 \rVert \lVert \mathbf{b}_2 \rVert \cdots \lVert \mathbf{b}_n \rVert}\right)^{\frac{1}{n}}
$$

그렇다면 자명히 $0 < \mathcal{H}(\mathcal{B}) \le 1$이다. 이 값은 기저가 수직인 정도를 결정하는 데 쓰인다. 즉, $\mathcal{H}$가 $1$에 가까울수록 기저가 수직에 가까움을 뜻한다.

이제 다시 민코프스키의 정리로 돌아가 보자. 충분히 큰 $n$에 대해 $\lambda_1(\mathcal{L})$을 잘 근사시키려면 민코프스키의 정리에 큐브를 쓰는 대신 구(sphere)를 사용해야 한다.

$\mathbb{R}^n$ 위의 초구(hypershpere) $B_n(r)$의 부피는 다음과 같이 정의된다.

$$
\operatorname{vol}(B_n(r)) := \frac{\pi^{\frac{n}{2}}}{\Gamma(1+\frac{n}{2})}r^n
$$

또한 다음의 스털링 근사

$$
\Gamma(1+s) \approx \sqrt{2\pi s}\left(\frac{s}{e}\right)^s
$$

를 이용하면 다음과 같이 근사할 수 있다.

$$
\operatorname{vol}(B_n(r))^{1/n} \approx \sqrt{\frac{2\pi e}{n}}r
$$

이제 다음 근사를 생각해보자.

$$
\# \{\mathbf{v} \in \mathcal{L} : \lvert \mathbf{v} \rvert \le r \} \approx \frac{\operatorname{vol}(B_n(r))}{\operatorname{vol}(\mathcal{P})}
$$

이는 *구 속에 들어있는 격자점의 개수는 구의 부피와 $\mathcal{P}$의 부피의 비와 비례한다*는 뜻이다. 이는 $n$이 커지고 $r$이 작아질수록 부정확해지긴 하지만 적당히 근사하기에는 충분하다. 이제 근사식들을 사용해서 좌변이 $1$이 되는, 즉 $\operatorname{vol}(B_n(r)) = \operatorname{vol}(\mathcal{P})$이 성립하게 하는 $r$을 찾을 수 있다.

$$
r \approx \sqrt{\frac{n}{2\pi e}} \det(\mathcal{L})^{1/n}
$$

즉

$$
\lambda_1(\mathcal{L}) \approx \sqrt{\frac{n}{2\pi e}} \det(\mathcal{L})^{1/n}
$$

으로 근사할 수 있다! 이를 **Gaussian Heuristic**이라고 한다.

### Babai's Algorithm

SVP와 CVP를 쉽게 풀 수 있는 격자가 존재하긴 한다. 대표적으로 기저가 서로 수직이면 SVP와 CVP를 풀기가 매우 쉬워진다. 일단 격자 위의 어떤 벡터를 잡아도

$$
\left\lVert \sum_{i=1}^n a_i\mathbf{b}_i \right\rVert^2 = \sum_{i=1}^n \lVert a_i\mathbf{b}_i \rVert^2 \quad (a_i \in \mathbb{Z})
$$

이므로 당연히 제일 짧은 벡터는 기저의 원소 중 하나일 것이다. CVP도 마찬가지로 $\mathcal{R}^n$ 위의 임의의 벡터 $\mathbf{v} = \sum_{i=1}^n t_i \mathbf{b}_i \ (t_i \in \mathbb{R})$에 대해

$$
\lVert \mathbf{v} - \mathbf{w} \rVert = \sum_{i=1}^n (t_i - a_i)^2\lVert \mathbf{b}_i \rVert^2
$$

이므로 각각의 $\lvert t_i - a_i \rvert$를 최소화할 수 있는 $a_i$를 잡아서 격자 위의 벡터 $\mathbf{w}$를 구성할 수 있다.

모든 격자가 이러면 참 좋겠지만, 안타깝게도 현실은 그리 녹록치 않다. 그래도, *적당히* 수직인 기저가 있다면 비슷한 아이디어로 apprCVP를 풀 수 있다. 즉 위의 CVP에서의 아이디어와 마찬가지로

$$
a_i = \lfloor t_i \rceil
$$

로 근사하면 된다. 이를 **Babai's Algorithm**이라고 한다.

이제부터 SVP와 CVP를 공략하기 위한 다양한 도구들을 살펴보자.

### Gram-Schmidt Orthogonalization



### LLL(Lenstra-Lenstra-Lovász) Algorithm



## Lattice-Based Cryptography



---
[^1]: 이러한 행렬 $U$를 **Unimodular**라고 한다.
[^2]: 왜 $\lambda$도 아니고 $\lambda_0$도 아니고 $\lambda_1$로 표기하는지 궁금한 독자들도 있을 것이다. 이렇게 표기하는 이유는 $\lambda_1$이 일반화된 Successive Minima $\lambda_n$에서 $n=1$인 특수한 경우이기 때문이다.
[^3]: 굳이 제곱으로 정의하는 이유는 이렇게 정의해야 $n$에 대해 선형적으로 증가하기 때문이다.
