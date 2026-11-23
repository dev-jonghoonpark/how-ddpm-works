# How DDPM Works

**Denoising Diffusion Probabilistic Models — 노이즈를 더하는 과정을 거꾸로 배운다.** Ho, Jain & Abbeel(NeurIPS 2020)의 「Denoising Diffusion Probabilistic Models」를 브라우저에서 직접 실험하며 배우는 인터랙티브 교육 자료입니다. 논문의 식 (1)~(14)와 알고리즘 1·2를 순서대로 따라가며, **생성이라는 어려운 문제를 "방금 섞인 노이즈 맞히기"라는 회귀 문제 T개로 쪼갠다**는 것을 축으로 순방향 과정·사후분포·손실 가중치를 계산하고 작은 확산 모델을 실제로 학습시켜 봅니다.

🔗 **[데모 바로가기](https://dev-jonghoonpark.github.io/how-ddpm-works/)**

## 다루는 내용

1. **무엇이 달라졌나: 게임 대신 회귀** · Dirac-GAN(Mescheder 외 2018)의 (θ, ψ) 궤적과 회귀 손실의 수렴 비교, GAN vs DDPM 표
2. **순방향 과정** · 식 (2). 64×64 그림에 논문 스케줄(T = 1000)을 적용해 신호 + 노이즈 = x_t로 분해해 보기
3. **한 번에 건너뛰기와 노이즈 스케줄** · 식 (4). 스케줄 계산기 — √ᾱ_t·√(1−ᾱ_t), ᾱ_T ≈ 4×10⁻⁵, L_T ≈ 10⁻⁵ bits/dim
4. **역방향 과정** · 식 (1)·(6)·(7). 사후 평균에서 x_0와 x_t의 계수, β̃_t / β_t
5. **손실** · 식 (5)·(8)·(11)·(12)·(14). 변분 하한 → ε 예측 → L_simple, 식 (12) 가중치(t=1에서 0.5, t=1000에서 0.01)와 알고리즘 1
6. **정답을 아는 1차원에서 역방향 돌려 보기** · 가우시안 혼합에서 닫힌 꼴로 구한 ε* = −√(1−ᾱ_t)∇log q(x_t)로 알고리즘 2 실행. T를 줄일 때, σ_t² = β_t vs β̃_t, σ_t = 0일 때
7. **브라우저에서 직접 학습시키기** · 2차원 점 데이터(가우시안 8개·두 초승달·나선)에 MLP ε_θ를 L_simple로 학습하고 EMA 가중치로 샘플링
8. **신경망** · 부록 B의 U-Net 구성, 시간 임베딩, 학습 설정
9. **논문의 결과** · 표 1(CIFAR10 IS 9.46 · FID 3.17 · ≤ 3.75 bits/dim), 표 2 절제, rate 1.78 + distortion 1.97
10. **한계와 그다음** · DDIM, Improved DDPM, Score SDE, guidance, Latent Diffusion
11. **정리** · 12. **퀴즈** 8문항

## 참고

- Ho, Jain & Abbeel (2020), *Denoising Diffusion Probabilistic Models*, NeurIPS 2020 — [arXiv:2006.11239](https://arxiv.org/abs/2006.11239)
- Sohl-Dickstein 외 (2015), *Deep Unsupervised Learning using Nonequilibrium Thermodynamics* — [arXiv:1503.03585](https://arxiv.org/abs/1503.03585)
- Mescheder, Geiger & Nowozin (2018), *Which Training Methods for GANs do actually Converge?* — [arXiv:1801.04406](https://arxiv.org/abs/1801.04406)

데모는 원리를 보이는 장난감 모형이며 논문의 U-Net을 재현한 것이 아닙니다. 외부 의존성 없이 `index.html` 하나로 동작합니다.

[how-ai-works](https://github.com/dev-jonghoonpark/how-ai-works) 시리즈의 일부입니다.
