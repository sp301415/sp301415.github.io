---
title: Dual_EC_DRBG와 NSA
date: 2017-12-26
tag: [수학, 암호학]
summary: Dual_EC_DRBG와 NSA의 백도어 논란에 대해서 알아보자.
---

지난 2013년, 에드워드 스노든이 폭로한 NSA(National Security Agency)의  [PRISM](https://namu.wiki/w/프리즘%20폭로%20사건) 프로젝트는 암호학계에도 엄청난 반향을 불러일으켰다. 2006년 NIST가 NSA와 협력해 미국 암호화 표준 규격을 발표했을 때부터 생겨난 의심이 사실임이 입증되었고, 나아가 그동안 NSA가 암호와의 전쟁에서 승리하고 있었다는 사실도 명백해졌다. 이 전쟁의 중심에 서 있었던 알고리즘이 바로 **Dual_EC_DRBG**다.

## Dual_EC_DRBG

Dual_EC_DRBG는 2004년 NIST 워크샵에서 처음 공개되었으며, [NIST SP 800-90](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-90.pdf) 표준안과 ANSI X9.82 표준안에 정의된 네 가지 PRG(PseudoRandom Generator)중 하나이다. 이름처럼, Dual_EC_DRBG는 타원 곡선(Elliptic Curve) 암호를 기반으로 설계되었다.

먼저 $p=2^{256}-2^{224}+2^{192}+2^{96}-1$이라 하자. 이제 $\operatorname{E}(\mathbb{F}_p)$를 $\mathbb{F}_p$ 위의 특정한 타원 곡선 $y=x^3+ax+b$으로 정의하고, 이 위의 고정된 점 $P, Q$를 잡자.[^1] 또 $x(P)$를 $P$의 $x$ 좌표를 구하는 함수, $\varphi(S)$를 문자열 $S$의 첫 16비트를 버리는 함수라고 정의하자.
Dual_EC_DRBG는 다음 시행을 $k$번 반복하여 $240k$비트 값을 얻는다.

1. 먼저 첫 시드(seed) $s_0 \in_R \\{0, 1, \cdots, \\# \operatorname{E}(\mathbb{F}_p) \\}$를 정의한다.
2. $0<i\le k$에 대해서 시드 $s_i := x(s_{i-1} P)$를 정의한다.
3. 각 $s_i$마다 값 $r_i := \varphi(x(s_i Q))$를 얻는다.
4. $r_1, r_2, \cdots r_k$를 이어 값을 얻는다.

## 의심

Dual_EC_DRBG는 처음 공개되었을 때부터 사람들의 의심을 사기 충분했다. 첫째, NIST와 NSA는 명확한 proof of security를 공개하지 않았다. 둘째, 같이 공개된 다른 세 개의 알고리즘과 달리 Dual_EC_DRBG는 공개 키 암호에 주로 쓰이는 ECDLP를 사용하였고, 따라서 속도가 수천배가량 느렸다.

2006년, Kristian Gjøsteen, Berry Schoenmakers, Andrey Sidorenko 등의 연구자들은 $r_i$를 생성할 때 16비트를 버리는 것은 충분하지 않다고 지적했다. 이는 $r_i$에 어느 정도 bias를 줄 뿐만 아니라, 공격자가 $2^{16}$번만의 시도로 원래 값, 즉 $x(s_i Q)$를 복원할 수 있게 하기 때문이다. 이 문제는 단순히 버리는 비트를 증가시키면 해결되는 문제였지만, NIST는 이를 무시하고 알고리즘을 확정했다.

2007년, 마이크로소프트의 Dan Shumow와 Niels Ferguson은 공격자가 $P, Q$의 관계를 활용해서 Dual_EC_DRBG를 쉽게 파훼할 수 있음을 증명했다. 즉, 만약 $P=dQ$이게 하는 비밀 상수 $d$가 존재한다면, 그리고 공격자가 이를 알고 있다면,

$$
d(s_i Q)=s_i(dQ)=s_iP
$$

이므로 연쇄적으로 시드 $s_i$를 복원할 수 있게 된다.
이들은 NIST에 알고리즘이 확정되기 전부터 $P, Q$의 유도 과정을 질문했지만, 그 때마다 돌아오는 답은 "공개적으로 밝힐 수 없다(*...I was not allowed to publicly discuss it*)"거나, "안전하고 비밀스러운 방법으로 유도했다(*...generated (P,Q) in a secure, classified way*)"는 답변 뿐이었다. 당연히 이를 기점으로 NSA가 Dual_EC_DRBG에 백도어를 숨겨놓았다는 의심이 기정사실화되었고, 전문가들은 본격적으로 Dual_EC_DRBG의 위험성을 경고했다. 물론 NIST는 끝까지 백도어의 증거가 없다는 이유로 Dual_EC_DRBG를 철회하지 않았다. 대신 표준안 끝에 (이 방법은 추천하지 않으며 FIPS 140-2 인증이 거부될 수 있다는 협박성 멘트와 함께) 개인적으로 $P, Q$를 생성할 수도 있다는 내용을 추가했다.

## 진상

2013년 6월, 에드워드 스노든의 폭로와 함께 NSA의 Bullrun 프로그램이 유출되었다. 이 프로그램은 "국제 암호화 표준에 서서히 취약점을 주입하는(*...to covertly introduce weaknesses into the encryption standards followed by hardware and software developers around the world.*) 것을 목표로 했다. 뉴욕 타임즈는 NSA가 Dual_EC_DRBG에 백도어를 심었다고 보도했다.

2013년 12월, NSA가 100만 달러를 대가로 RSA[^2]의 BSAFE 킷의 기본 PRG로 Dual_EC_DRBG를 지정하도록 요청했다는 사실이 밝혀졌다.

결국 2014년 NIST는 표준안에서 Dual_EC_DRBG를 삭제했다. 하나의 해프닝이라고도 볼 수 있지만, 이 사건이 암호학계에 가져다 준 파장은 어마어마했다. 기본적으로 암호 표준의 지정에 대해서 회의적인 시선이 늘어났으며, Daniel J. Bernstein 등이 제안한 타원곡선 Curve25519가 유명세를 타는 등 대안 암호 움직임도 활발해지게 되었다.

## References
- Daniel J. Bernstein, Tanja Lange, and Ruben Niederhagen. Dual EC: A Standardized Back Door. *June 2015.* *[https://projectbullrun.org/dual-ec/documents/dual-ec-20150731.pdf](https://projectbullrun.org/dual-ec/documents/dual-ec-20150731.pdf)*
- Berry Schoenmakers and Andrey Sidorenko. Cryptanalysis of the Dual Elliptic Curve Pseudorandom Generator. *May 2006.* *[https://eprint.iacr.org/2006/190.pdf](https://eprint.iacr.org/2006/190.pdf)*
- Dan Shumow and Niels Ferguson. On the possibility of a back door in the NIST SP800-90 Dual Ec Prng. CRYPTO 2007 Rump Session. *August 2007.* *[http://rump2007.cr.yp.to/15-shumow.pdf](http://rump2007.cr.yp.to/15-shumow.pdf)*

---

[^1]: 이 때 $a, b, P, Q$의 값은 [NIST SP 800-90](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-90.pdf) 문서의 Appendix A.1에 제시되어 있다.
[^2]: 회사 이름이다.
