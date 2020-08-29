---
title: Cache Timing Attack on AES
date: 2017-07-10
tag: [수학, 암호학]
summary: AES를 향한 부채널 공격 중 Cache Timing Attack을 알아보자.
---

​	AES는 알고리즘 상의 하자는 없다는 것이 확실시되지만, 그렇다고 아주 안전한 것도 아니다. AES가 널리 인정받은 이후로 많은 암호학자들이 알고리즘을 공격하려고 시도했고, 몇몇은 성공하기까지 했다. 그 중 Cache Timing Attack이 가장 위협적으로 여겨지고 있다.

​	Timing Attack이란 특정한 조건의 평문을 암호화할 때 다른 평문들을 암호화할 때보다 더 빠르거나 느린 것을 이용한 공격이다. 공격자는 목표 서버에 여러 명령을 내리고 응답을 받는 시간을 각각 재서 이를 분석할 수 있다. 특히 AES는 구조상 캐시를 이용한 Timing Attack에 취약하다.

​	컴퓨터에 조금이라도 관심이 있는 사람은 알겠지만, 현대의 CPU에는 여러 겹의 보조 저장 장치, 캐시(Cache)가 붙어 있다. 캐시는 레벨에 따라서 L1, L2, …식으로 이름붙인다.[^1] 물론 컴퓨터에는 보조 저장 장치인 메모리(RAM)이 있지만, 메모리의 속도는 CPU의 속도에 비해 월등히 느리기 때문에 조금 더 빠르게 작업을 처리하고자 메모리에서 데이터를 미리 가져와 캐시에 저장해 놓는다.

​	AES는 본래 4개의 단계(`SubBytes`, `ShiftRows`, `MixColumns`, `AddRoundKey`)를 한 라운드로 묶은 뒤 10라운드를 반복한다. 하지만 이 4단계를 하나씩 시행하는 것은 시간과 자원이 너무 많이 소모되기에 실제로 적용되는 AES 알고리즘은 상당히 많은 경량화를 거친다. 그 중 하나는 미리 `SubBytes`, `ShiftRows`, `MixColumns`를 계산해놓는 것이다. 먼저 입력을 $$4 \times 4$$ 행렬 $$A = (a_i)_{i=0,1,\cdots ,15}$$로, 출력을 $$A'$$ 로 표현하자. 그렇다면 $$A$$를 입력하고 한 라운드를 거쳤을 때의 출력값 $$A'$$는 다음과 같이 계산된다.

$$
\begin{eqnarray*}
A'[0] &:=& M[0] \cdot S[a_0] \oplus M[1] \cdot S[a_5] \oplus M[2] \cdot S[a_{10}] \oplus M[3] \cdot S[a_{15}] \oplus k[0]\\
A'[1] &:=& M[0] \cdot S[a_1] \oplus M[1] \cdot S[a_6] \oplus M[2] \cdot S[a_{11}] \oplus M[3] \cdot S[a_{12}] \oplus k[1]\\
A'[2] &:=& M[0] \cdot S[a_2] \oplus M[1] \cdot S[a_7] \oplus M[2] \cdot S[a_{8}] \oplus M[3] \cdot S[a_{13}] \oplus k[2]\\
A'[3] &:=& M[0] \cdot S[a_3] \oplus M[1] \cdot S[a_4] \oplus M[2] \cdot S[a_{9}] \oplus M[3] \cdot S[a_{14}] \oplus k[3]
\end{eqnarray*}
$$

​	여기에서 $$M[t]$$는 `MixColumns` 단계에서 사용되는 행렬의 $$t$$번째 열이고, $$S[t]$$는 `SubBytes`단계에서 사용되는 S-Box로 $$t$$를 치환한 값이다. 이제 테이블 $$T_i := M[i] \cdot S[a] $$를 미리 계산해놓는다면

$$
\begin{eqnarray*}
A'[0] &:=& T_0[a_0] \oplus  T_1[a_5] \oplus  T_2[a_{10}] \oplus  T_3[a_{15}] \oplus k[0]\\
A'[1] &:=& T_0[a_1] \oplus  T_1[a_6] \oplus  T_2[a_{11}] \oplus T_3[a_{12}] \oplus k[1] \\
A'[2] &:=& T_0[a_2] \oplus  T_1[a_7] \oplus  T_2[a_{8}] \oplus T_3[a_{13}] \oplus k[2]\\
A'[3] &:=& T_0[a_3] \oplus  T_1[a_4] \oplus  T_2[a_{9}] \oplus T_3[a_{14}] \oplus k[3]\\
\end{eqnarray*}
$$

​	AES에서 사용되던 복잡한 계산을 단순화했다. 이제 AES의 모든 라운드를 거칠 필요 없이 미리 계산해 둔 테이블을 찾기만 하면 된다. 물론 AES의 마지막 라운드에서는 `MixColumns`를 거치지 않기 때문에 $$T_i$$의 합 대신에 각각 $$S$$를 써야 한다.

​	그런데 이런 식의 경량화는 심각한 허점이 존재한다. 알고리즘 전체가 배열 탐색으로 이루어졌기 때문에 알고리즘 전체의 실행 시간이 입력값에 의존할 수밖에 없다. 이러한 허점을 이용한 것이 Timing Attack이며, Daniel J.Bernstein을 비롯한 여러 연구자들이 시연에 성공한 바 있다.

​	그 중 직접적으로 캐시의 저장 속도를 악용한 방법은 J.Bonneau와 I.Mironov가 제시하였다. 마지막 출력값인 암호문 $$C$$는 10번째 라운드의 입력값 $$A=(a_i)_{i=0,1, \cdots ,15}$$와 10번째 라운드키 $$k$$에 대해

$$
\begin{eqnarray*}
C[0] &:=& S[a_0] \oplus  S[a_5] \oplus  S[a_{10}] \oplus S[a_{15}] \oplus k[0]\\
C[1] &:=& S[a_1] \oplus  S[a_6] \oplus  S[a_{11}] \oplus S[a_{12}] \oplus k[1] \\
C[2] &:=& S[a_2] \oplus  S[a_7] \oplus  S[a_{8}] \oplus S[a_{13}] \oplus k[2]\\
C[3] &:=& S[a_3] \oplus  S[a_4] \oplus  S[a_{9}] \oplus S[a_{14}] \oplus k[3]\\
\end{eqnarray*}
$$

로 표현된다. 이제 AES 알고리즘을 시행할 때 $$S$$를 캐시에 로드함을 상기해보자. 암호문의 어떤 두 바이트 $$c_i = S[a_u] \oplus k_i$$, $$c_j = S[a_w] \oplus k_j$$에 대하여 만약  $$a_u = a_w$$라면,[^2] $$S[a_u]$$가 이미 로드된 상태이므로 $$a_u \neq a_w$$인 경우보다 AES의 암호화 시간이 더 빠름을 유추할 수 있다. 즉, 암호화에 걸리는 시간이 $$c_i$$와 $$c_j$$에 대한 정보를 유출하는 것이다. 이 경우 두 바이트 $$c_i$$, $$c_j$$의 차 $$\Delta$$는


$$
\Delta := c_i \oplus c_j = k_i \oplus k_j
$$


의 형태로 표현될 것이다. 이제 공격자가 할 일은 최대한 많은 암호문과 쌍 $$(i,j)$$에 대해 평균 암호화 시간보다 더 짧은 시간 내에 암호화를 완료하는 $$\Delta^\prime$$를 찾아내는 것이다. 이를 반복하면 실제 $$\Delta = k_i \oplus k_j$$의 값에 가까워질 것이며, 쌍 $$(i, j)$$를 바꾸어 실험하면 공격자는 마지막 라운드키 $$k$$에 대한 정보를 어느 정도 유추할 수 있다. AES의 구조상 라운드키를 알아내면 전체 키 또한 알아낼 수 있으므로 공격이 성공적으로 끝나는 것이다. 역으로, $$a_u = a_w$$일 때 $$S^{-1}[c_i \oplus k_i] = S^{-1}[c_j \oplus k_j]$$임을 이용하여 두 바이트 $$k_i$$, $$k_j$$를 브루트포싱으로 찾아갈 수도 있다. 이는 더 효율적이지는 않지만 앞선 공격을 행하기 전 샘플을 검사하기 위한 용도로 사용될 수도 있다.

​	이러한 Cache-Timing Attack이 작동하는 근본적인 이유는 S-Box 때문이다. 기본적으로 S-Box는 배열이므로, 입력값에 작동 시간이 의존할 수밖에 없다. 바꿔 말하면 작동 시간이 입력값에 대한 정보를 유출하는 것이다. 이를 막는 하나의 방법은 S-Box를 캐시에 미리 저장하지 않고 전부 계산하는 것이지만, 효율성이 극히 떨어진다는 단점이 있겠다.



### References

- J. Bonneau and I. Mironov. Cache-collision timing attacks against AES. *In in Proc. Crypto-graphic Hardware and Embedded Systems (CHES) 2006. Lecture Notes in Computer Science,pages 201–215. Springer, 2006.*
- Daniel J. Bernstein. Cache-timing attacks on AES. *April 2005.*
  *http://cr.yp.to/antiforgery/cachetiming-20050414.pdf.*

---

[^1]: L은 Level의 약자이며, 숫자가 작을수록 CPU에 가까이 위치한다. 숫자가 작을수록, 즉 CPU 가까이 위치할수록 입출력 속도는 빠르지만 용량은 작다.

[^2]: 해쉬 충돌을 생각해보면 바이트끼리의 충돌이 일어날 확률이 생각보다 높다는 것을 알 수 있을 것이다.
