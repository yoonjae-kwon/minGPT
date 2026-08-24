# minGPT — # minGPT — 문자 단위 Transformer 언어모델 from scratch 구현

Transformer 기반 문자 단위 언어모델의 구조를 이해하기 위해 정리한 학습 노트입니다. 코드는 Karpathy의 nanoGPT 강의와 LLM의 도움을 받아 작성했으며, 각 구성요소의 역할과 왜 필요한지를 한국어로 정리하는 데 중점을 두었습니다.

## 구현

Bigram 베이스라인 → Self-attention → Multi-head → Feed-forward
→ Residual connection → Layer normalization → Positional encoding → Dropout

## 결과

**단계별 validation loss**

| 단계 | val loss |
|---|---|
| 학습 전 (랜덤 초기화) | 5.03 |
| Bigram | 2.38 |
| Self-attention (1 head) | 2.29 |
| Transformer (6층 6 head) | **1.47** |

**과적합 관측**

step 4000: train 1.1222, val 1.4786
step 4500: train 1.0845, val 1.4696 ← val 최저
step 4999: train 1.0503, val 1.4898 ← val 상승


train loss는 계속 하락하지만 val loss는 4,500 스텝 부근에서 최저를 찍고
반등한다. 두 값의 격차도 벌어진다 (1.05 vs 1.49).

10.8M 파라미터로 약 100만 자를 학습하므로, 모델이 일반화 대신 암기를
시작한 것으로 보인다. 개선 방향으로는 val 최저 시점 모델 저장(early
stopping), dropout 상향, 데이터 확대, 모델 축소를 고려할 수 있다.

**생성 결과에서 확인된 것**

- 희곡 구조 (화자 대문자 + 콜론 + 줄바꿈)
- 셰익스피어 실존 인물명 (JULIET, POLIXENES, HASTINGS, Rosaline, Claudio)
- 고어체 어휘 (thou, thee, Beseech)
- 아포스트로피 축약형의 정확한 위치
- 한 문장 내에서 유지되는 문맥 → block_size=256이 작동한 결과

**한계**

의미가 없다. 존재하지 않는 단어가 섞이고 문장 간 논리가 없다.
문자 단위 + 10.8M 파라미터 + 100만 자로는 여기가 한계다.
구조 자체는 대규모 언어모델과 동일하며, 규모만 다르다.

## 환경

Python / PyTorch / Google Colab (GPU)
학습 데이터: Tiny Shakespeare (약 100만 자)

## 파일

`minGPT.ipynb` — 전체 구현 및 학습 과정 (한국어 주석)

## 참고

- Vaswani et al., *Attention Is All You Need* (2017)
- Andrej Karpathy, *Let's build GPT: from scratch, in code, spelled out*

PyTorch만 사용해 Transformer 기반 문자 단위 언어모델을 라이브러리 없이
직접 구현한 학습 프로젝트입니다. 구성요소를 한 번에 넣지 않고 단계별로
추가하며 각각의 기여를 검증하는 방식으로 진행했습니다.

## 구현

Bigram 베이스라인 → Self-attention → Multi-head → Feed-forward
→ Residual connection → Layer normalization → Positional encoding → Dropout

## 결과

**단계별 validation loss**

| 단계 | val loss |
|---|---|
| 학습 전 (랜덤 초기화) | 5.03 |
| Bigram | 2.38 |
| Self-attention (1 head) | 2.29 |
| Transformer (6층 6 head) | **1.47** |

**과적합 관측**

step 4000: train 1.1222, val 1.4786
step 4500: train 1.0845, val 1.4696 ← val 최저
step 4999: train 1.0503, val 1.4898 ← val 상승


train loss는 계속 하락하지만 val loss는 4,500 스텝 부근에서 최저를 찍고
반등한다. 두 값의 격차도 벌어진다 (1.05 vs 1.49).

10.8M 파라미터로 약 100만 자를 학습하므로, 모델이 일반화 대신 암기를
시작한 것으로 보인다. 개선 방향으로는 val 최저 시점 모델 저장(early
stopping), dropout 상향, 데이터 확대, 모델 축소를 고려할 수 있다.

**생성 결과에서 확인된 것**

- 희곡 구조 (화자 대문자 + 콜론 + 줄바꿈)
- 셰익스피어 실존 인물명 (JULIET, POLIXENES, HASTINGS, Rosaline, Claudio)
- 고어체 어휘 (thou, thee, Beseech)
- 아포스트로피 축약형의 정확한 위치
- 한 문장 내에서 유지되는 문맥 → block_size=256이 작동한 결과

**한계**

의미가 없다. 존재하지 않는 단어가 섞이고 문장 간 논리가 없다.
문자 단위 + 10.8M 파라미터 + 100만 자로는 여기가 한계다.
구조 자체는 대규모 언어모델과 동일하며, 규모만 다르다.

## 환경

Python / PyTorch / Google Colab (GPU)
학습 데이터: Tiny Shakespeare (약 100만 자)

## 파일

`minGPT.ipynb` — 전체 구현 및 학습 과정 (한국어 주석)

## 참고

- Vaswani et al., *Attention Is All You Need* (2017)
- Andrej Karpathy, *Let's build GPT: from scratch, in code, spelled out*
