---
layout: post
title: "[GoogleStudyJam] Prompt Design in Vertex AI"
date: 2024-04-05 17:59:00 +0900
categories: TIL
---

### 정리

Prompt Design은 시스템이 수행하도록 요청받은 특정 작업에 맞게 조정된 prompt를 만드는 프로세스이다.
Prompt Engineering은 성능을 향상시키도록 설계된 프롬프트를 생성하는 프로세스이다. 도메인별 지식을 사용하거나, 결과에 답을 제시하거나. 높은 수준의 정확성이나 성능이 요구되는 시스템에 필요하다.

이번 챕터에서는 Prompt의 적절한 사용에 대해 실습해보았다.

Prompt를 통해 적절한 결과를 얻어내기 위해서는 다음과 같은 주의사항을 지켜야 한다.

- Be concise
- Be specific and well-defined
- Ask one task at a time
- Watch out for hallucinations
  LLM은 사실이나 현실에 근거하지 않은 진술을 포함하는 텍스트를 생성하는 hallucinations이라고 불리는 오류를 가진다. 창의적인 Prompt를 제시하면 hallucinations이 발생할 가능성이 높아진다.
  Determine Appropriate Response (DARE) prompt를 이용해 질문에 따라 대답을 해야만 하는지 결정하게 한다.
  예를 들어 "가장 먼저 달에 간 코끼리"를 질문하면 Sorry, I can't answer that question. There have been no elephants on the moon. 라는 응답을 해 DARE가 적용됐음을 알 수 있다.
- Turn generative tasks into classification tasks
- Improve response quality by including examples
  제로샷 프롬프트는 좀 더 개방적이고 창의적인 답변을 제공할 수 있으며, 원샷 및 퓨샷 프롬프트는 모델에게 행동 방법을 가르쳐 주어 제공된 예시와 일치하는 보다 예측 가능한 답변을 얻을 수 있습니다.
  다음과 같이 one-shot prompt를 이용하면 모델이 정형화 된 답변을 할 수 있도록 가이드할 수 있다.
  ![스크린샷 2024-04-04 190110](https://github.com/pingu2017/comment/assets/115390100/a093fbaa-1b32-4b1f-a50e-ff5d40ebb2e8)
  ![스크린샷 2024-04-04 190134](https://github.com/pingu2017/comment/assets/115390100/f5b9a8d0-1236-4419-95e7-c844760bf3b6)
  few-shot prompt를 이용해 긍정, 부정, 중립을 정의해주면 가치판단을 할 수 있다.
  ![스크린샷 2024-04-04 190429](https://github.com/pingu2017/comment/assets/115390100/3584735d-940d-4b26-85d1-f767cef2f22f)

  <br>

  **한국어도 잘 동작할까? 궁금했다.**

  zero-shot에서 영어 prompt와 달리 답변을 하지 않고 한영번역을 대답했다.
  ![스크린샷 2024-04-04 190645](https://github.com/pingu2017/comment/assets/115390100/24473ab4-9b90-4012-8322-b5d2230c5962)
  하지만 한국어로 few-shot prompt를 제공하자 바로 긍정으로 판단했다! 더 어려운 한국어-예를들면 "꼴 좋다~" 같은 뉘앙스의 차이도 알아들을까 궁금하긴 하다.
  ![스크린샷 2024-04-04 190624](https://github.com/pingu2017/comment/assets/115390100/81dc011a-60fe-4150-89c7-13a262ca059e)

이렇게
![스크린샷 2024-04-04 191141](https://github.com/pingu2017/comment/assets/115390100/41d3e5d5-7627-4982-8afa-cd4313f7e326)
