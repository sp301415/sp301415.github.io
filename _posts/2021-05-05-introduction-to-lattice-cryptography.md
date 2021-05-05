---
title: 격자 암호의 소개
date: 2021-05-05
tag: [수학, 암호학]
summary: 최근 Post-Quantam Cryptography에서 각광받는 격자 암호에 대해 알아보자.
image: images/introduction-to-lattice-cryptography/lattice.png
---

> 아직 업데이트 중입니다.

## Lattice

### Lattice and Basis

눈을 감고 격자를 떠올려 보자. 보통이라면 체스판처럼 생긴 무언가를 떠올릴 것이다. 체스판의 각 점들은 일정한 간격으로 떨어진 채 평면을 가득 채운다. 이처럼, 일정한 규칙성을 가지고 $\mathbb R^n$에 놓인 점들을 **격자(Lattice)**라고 부른다.

![Lattice](/images/introduction-to-lattice-cryptography/lattice.png)

선형대수학을 조금 배운 독자라면 기저(Basis)에 대해서 들어보았을 것이다. 격자에서의 규칙성 또한 결국 기저에 의해 결정된다. 정의를 조금 더 구체화시켜보자. 

**Definition. (Lattice).** 일차독립인 벡터 $\mathcal{B} = \{\mathbf b_1, \mathbf b_2, \cdots, \mathbf b_n\} \in \mathbb R^n$에 대해, 격자 $\mathcal L$ (기저를 강조하고 싶을 때는 $\mathcal L (\mathcal B)$)은

$$\mathcal{L}(\mathcal B)  =\left\{ \sum_{i=1}^n z_i \mathbf{b}_i : z_i \in \mathbb{Z} \right\}$$

로 정의되며, 이 때 $\mathcal B$를 $\mathcal L$의 **기저(Basis)**라고 부른다. 

요약하자면, 격자는 기저의 정수계수 일차결합으로 생성되는 $\mathbb R^n$의 부분집합이다. 위에서 예를 들었던 "체스판"의 경우에는, $\{(1, 0), (0, 1)\}$이 생성하는 격자로 이해할 수 있다. 정수항을 가진 벡터로만 기저를 만들어낼 필요도 없다. 유리수 격자도 가능한 것이다. 하지만, 논의의 편의성을 위해 앞으로는 정수항을 가진, full-rank 격자로 한정지어 이야기하도록 하겠다.

그렇다면, 한 격자를 생성하는 기저는 유일할까? 조금만 생각해본다면 대답은 "아니오"라는 것을 짐작할 수 있을 것이다. 당장 체스판만 보더라도, $\{(1, 0), (1, 1)\}$ 또한 기저가 되기 때문이다. 실제로 한 격자의 기저는 무한할뿐만 아니라, 다음 정리에 따라 한 기저에서 다른 기저를 무한히 만들어낼 수 있다.

**Theorem.** 어떤 두 기저 $\mathcal B_1, \mathcal B_2$가 같은 격자를 생성할 필요충분조건은 $\det U = \pm 1$인 적당한 정사각행렬[^1] $U$가 존재해서 $\mathcal B_1 = U \cdot \mathcal B_2$인 것이다.

**proof.** $\Rightarrow$)  $\mathcal B_1, \mathcal B_2$가 같은 격자를 생성한다고 하자. 격자의 정의에 의해서, 적당한 rank n 정수계수 정사각행렬 $U, V$가 존재해서, $\mathcal B_1 = U\mathcal B_2$와 $\mathcal B_2 = V\mathcal B_1$를 만족한다. 따라서, $\mathcal B_1 = UV\mathcal B_1$이며,

$$\mathcal B_1^T \mathcal B_1 = \mathcal B_1^T (UV)^T(UV)\mathcal B_1$$

이므로, $\det(\mathcal B_1^T \mathcal B_1) = \det(\mathcal B_1^T (UV)^T(UV)\mathcal B_1) = \det(UV)^2\det(\mathcal B_1^T \mathcal B_1)$에서 $\det(UV) = \pm 1$을 얻는다. 그런데 $U, V$의 각 항은 모두 정수이므로, $\det U = \pm 1$이다. 

$\Leftarrow$) 적당한 unimodular $U$가 존재해서 $\mathcal B_1 = U\mathcal B_2$라고 하자. 그렇다면 $\mathcal B_1$의 각 열벡터는 $\mathcal B_2$의 정수계수 일차결합으로 이루어지므로, $\mathcal L (\mathcal B_1) \subseteq \mathcal L (\mathcal B_2)$이다. 비슷하게, $U^{-1}$또한 unimodular이므로(?) $\mathcal L (\mathcal B_2) \subseteq \mathcal L (\mathcal B_1)$이다. 따라서 결론을 얻는다.

### Fundemental Domain and volume of a Lattice

위 정리는 다음의 중요한 Corollary로 이어진다.

**Corollary.** 격자 $\mathcal L$과 임의의 기저 $\mathcal B$에 대해서, $\det \mathcal B$의 값은 언제나 일정하다. 

다른 말로 하면, $\det \mathcal B$는 격자에 대한 불변량(invariant)이다. 그런데 $\det \mathcal B$의 의미가 무엇일까? 행렬식의 기하학적 의미, 즉 부피를 생각해 본다면, 직관적으로 다음과 같은 격자의 "단위 칸"의 부피라고 할 수 있을 것이다.

이 단위 칸을 우리는 **Fundemental Domain**이라고 부른다.

**Definition. (Fundemental Domain).** 격자 $\mathcal L(\mathcal B)$에 대해 집합

$$\mathcal{P}(\mathcal{B}) := \left\{ \sum_{i=1}^n c_i\mathbf{b}_i : c_i \in \left[ 0, 1 \right)\right\}$$

을 이 격자의 Fundemental Domain이라고 한다.

![Fundemental Domain](/images/introduction-to-lattice-cryptography/fundemental-domain.png)

**Remark.** Fundemental Domain을 모으면 $\mathbb R^n$을 구성할 수 있다. 즉,

$$\mathbb R^n = \bigcup_{\mathbf v \in \mathcal L} (\mathbf v + \mathcal P)$$

이제 다음을 얻는다.

**Definition. (Volume of a Lattice).**  $\mathcal L(\mathcal B)$에 대해

$$\operatorname{vol}(\mathcal L) = \operatorname{vol}(\mathcal P(\mathcal B)) = \det \mathcal B$$

의 값은 기저의 선택과 무관하다. 이 값을 격자의 부피로 정의한다.

## Lattice-Based Problems & Algorithms

### Lattice-Based Problems

격자 암호에 쓰이는 문제에는 **SVP**(Shortest Vector Problem), **CVP**(Closest Vector Problem)가 있다. 이들의 공통점은 모두 격자 위의 *짧은 벡터*에 대한 문제라는 점이다.

- **SVP**(Shortest Vector Problem): 격자 위의 가장 짧은 (0이 아닌) 벡터는 무엇인가?
- **CVP**(Closest Vector Problem): 주어진 벡터와 가장 가까운 격자 위의 벡터는 무엇인가?

이제 이 문제들을 공략해보도록 하자!

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
