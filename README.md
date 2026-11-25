# 이혁수 · Hyeoksu Lee

**경희대학교 컴퓨터공학과 · 백엔드 개발 / 데이터 엔지니어링**

차량 공매 기록으로 참고 가격을 계산하고, 여섯 대의 카메라와 LiDAR로 수집한 주행 장면을 문장으로 검색하는 서비스를 만들었습니다. 데이터를 다룰 때 자주 돌아가는 질문은 두 가지입니다. **이 응답은 어떤 원본에서 나왔을까? 처리 방식을 바꿔도 같은 결과를 돌려줄까?** DriveMate에서는 가격의 근거를 추적했고, DriveScene2Label에서는 검색 속도를 높이면서 응답이 그대로 유지되는지 확인했습니다.

<p>
  <img alt="Java" src="https://img.shields.io/badge/Java-9E4C16?style=flat-square">
  <img alt="Spring Boot" src="https://img.shields.io/badge/Spring_Boot-386C27?style=flat-square&logo=springboot&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-315A83?style=flat-square&logo=postgresql&logoColor=white">
  <img alt="Python" src="https://img.shields.io/badge/Python-356783?style=flat-square&logo=python&logoColor=white">
  <img alt="AWS" src="https://img.shields.io/badge/AWS-293546?style=flat-square">
  <img alt="Docker" src="https://img.shields.io/badge/Docker-176A98?style=flat-square&logo=docker&logoColor=white">
  <img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-A13D2D?style=flat-square&logo=pytorch&logoColor=white">
</p>

[DriveMate](#drivemate) · [DriveScene2Label](#drivescene2label) · [Refine AI](#refine-ai) · [연구](#연구) · [대회](#대회)

## 모빌리티 프로젝트

<table>
  <tr>
    <td width="50%" valign="top">
      <a href="https://github.com/cloud-project-mobility/make_drive_mate/blob/dde036b0c88f95a2e0241ce6d6605b30673eeb14/docs/evidence/2026-11-05/public-home-desktop.jpg"><img src="https://raw.githubusercontent.com/cloud-project-mobility/make_drive_mate/dde036b0c88f95a2e0241ce6d6605b30673eeb14/docs/evidence/2026-11-05/public-home-desktop.jpg" alt="DriveMate에서 예시 차량의 주행거리, 공매 참고 가격 분석, 정비 항목을 확인하는 홈 화면"></a>
      <br><b><a href="https://github.com/cloud-project-mobility/make_drive_mate">DriveMate</a></b><br>내 차의 기록과 공매 참고 가격을 한곳에서 확인하는 차량 관리 서비스
    </td>
    <td width="50%" valign="top">
      <a href="https://github.com/DriveScene2Label-Team/DriveScene2Label/blob/d26c246a7e718cc17173e0e0d6696ad9a9f41e15/docs/images/scene-workspace-2026-11-11.jpg"><img src="https://raw.githubusercontent.com/DriveScene2Label-Team/DriveScene2Label/d26c246a7e718cc17173e0e0d6696ad9a9f41e15/docs/images/scene-workspace-2026-11-11.jpg" alt="DriveScene2Label에서 여섯 방향 카메라 영상과 LiDAR 3D 라벨을 함께 검토하는 작업 화면"></a>
      <br><b><a href="https://github.com/DriveScene2Label-Team/DriveScene2Label">DriveScene2Label</a></b><br>문장으로 주행 장면을 찾아 카메라·LiDAR에서 3D 라벨을 검토하는 도구
    </td>
  </tr>
</table>

### DriveMate

차량을 등록한 사용자가 “내 차의 공매 참고 가격은 얼마이고, 그 숫자는 어디서 나왔지?”라는 질문에 답을 얻는 서비스입니다. Spring Boot에서 계정별 차량·정비 기록과 분석 요청을 처리하고, Python에서 온비드 공매 데이터 처리와 가격 추론을 실행하도록 연결했습니다. 배포 환경은 ECS Fargate와 RDS PostgreSQL로 구성했습니다. [서비스 구조](https://github.com/cloud-project-mobility/make_drive_mate/blob/dde036b0c88f95a2e0241ce6d6605b30673eeb14/README.md#L65-L90)

- **가격 추정과 AI 상담.** 공매 기록으로 학습한 Random Forest가 참고 가격을 계산합니다. 상담 답변에는 계산 결과와 확인 가능한 근거 목록을 전달하고, 모델이 돌려준 근거 ID가 실제 목록에 있는지 검사했습니다. 답변이 임의의 가격을 쓰거나 존재하지 않는 근거를 인용하면 답변을 보류하도록 했습니다. [응답 검증 코드](https://github.com/cloud-project-mobility/make_drive_mate/blob/d55c27dfa5066bc31454619593d791e27db59187/backend/scripts/assistant_worker.py#L139-L175)
- **온비드 원본 추적.** 전달받은 파일을 정상적으로 읽어도 최초 수집에서 빠진 공매 건이 있는지는 알 수 없습니다. 원본 바이트와 SHA-256 해시를 보존하고, 파일을 정상적으로 가져왔는지와 최초 수집을 검증했는지를 따로 기록했습니다. 수집 이력이 없는 파일을 가져오는 경우도 테스트했습니다. [원본 보존 테스트](https://github.com/cloud-project-mobility/make_drive_mate/blob/daa2a2436bbceabb99c752da960313e4d429667f/backend/scripts/test_onbid_import.py#L38-L52)

### DriveScene2Label

nuScenes 원본을 올리면 원하는 주행 장면을 자연어로 찾고, 선택한 장면의 3D 라벨을 생성·수정할 수 있습니다. 저는 Spring JDBC로 메타데이터를 PostgreSQL에 적재하는 경로를 구성하고, Spring API를 CLIP 검색 및 VESPA 라벨링 작업과 연결했습니다. 원본 파일은 S3에, 검색용 메타데이터와 라벨은 RDS에 저장하고 GPU 작업은 로컬에서 실행하는 구성을 사용했습니다. [데이터 적재 코드](https://github.com/DriveScene2Label-Team/DriveScene2Label/commit/98700f1015055a306bd38ebda93e1ff7d1947be5) · [서비스 통합 기록](https://github.com/DriveScene2Label-Team/DriveScene2Label/commit/88847a981ecdb43c9daaa3b129be575558c5da79)

| 느려진 구간 | 바꾼 점 | 같은 입력으로 비교한 결과 |
| --- | --- | --- |
| 이미지 검색 인덱싱 | 일괄 조회에서 이미 받은 원본 정보를 재사용해 이미지마다 반복되던 DB 조회를 줄였습니다. | **49.2초 → 42.9초**. 이미지 474장, 전후 3쌍의 중앙값. 벡터 1,422쌍과 검색 결과 54개가 일치했습니다. |
| 문장으로 전체 데이터 검색 | PostgreSQL에서 거리 계산 결과를 재사용하고, 검색 트랜잭션에 맞는 실행 계획을 선택했습니다. 같은 문장의 CLIP 임베딩은 캐시했습니다. | **5.43초 → 0.246초**. 모델을 예열한 뒤 같은 문장을 반복한 로컬 HTTP 시험의 전체 데이터 검색 중앙값. 두 검색 범위를 포함한 전후 응답 54쌍이 일치했습니다. |

[인덱싱 실험 기록](https://github.com/DriveScene2Label-Team/DriveScene2Label/blob/d26c246a7e718cc17173e0e0d6696ad9a9f41e15/docs/optimization-results-2026-11-18.md#L66-L79) · [검색 SQL과 캐시 변경](https://github.com/DriveScene2Label-Team/DriveScene2Label/commit/725c7e3903f87d9c161da81fed92ad1061d29b53) · [검색 API 측정 조건](https://github.com/DriveScene2Label-Team/DriveScene2Label/blob/d26c246a7e718cc17173e0e0d6696ad9a9f41e15/docs/optimization-resume-2026-11-18.md#L84-L93)

## 개인 프로젝트와 교육

### Refine AI

**2025.12 · 디자인 수업 최종 과제 · 개인 개발**  
`FastAPI` · `OpenAI API` · `SQLite` · `SQLAlchemy` · `Jinja2`

AI에게 원하는 결과를 설명하기 어려운 사람을 위해, 짧은 요청을 용도에 맞는 프롬프트로 다듬는 웹 서비스를 혼자 개발했습니다. 예를 들어 사용자가 **“사이버펑크 스타일의 미래 도시 야경”**을 입력하고 이미지 분야를 고르면, GPT-4o가 카메라 구도와 조명 조건을 담은 영문 지시문을 제안합니다. 사용자는 결과를 복사해 창작 도구에서 이어서 쓸 수 있습니다.

입력 화면부터 FastAPI 라우터, OpenAI 호출, 기록 화면까지 구현했습니다. 모델 응답은 개선문·분류·맥락 및 출력 형식의 누락 여부·작성 팁으로 나눠 받았습니다. 이 중 원문과 개선문, 분류, 누락 여부를 SQLite에 저장해 이전 요청을 다시 보고 분야별 사용 횟수를 확인할 수 있게 했습니다. [프로젝트와 실행 방법](https://github.com/leehyeoksu/fastapi) · [프롬프트 설계와 모델 호출](https://github.com/leehyeoksu/fastapi/blob/e9d2c14b821fc1019aa08b0c63d87de9024c004c/app/services/service.py#L63-L113) · [저장·조회·통계 API](https://github.com/leehyeoksu/fastapi/blob/e9d2c14b821fc1019aa08b0c63d87de9024c004c/app/routers/prompt.py#L15-L55)

**WASSUP EST AI 모델 개발자 양성과정 13기** · 이스트소프트 · 2025.11.18–2026.04.10 수료  
부트캠프에서는 트럼프 발언과 업종별 ETF 데이터로 다음 시점의 시장 불확실성 지수를 예측하는 팀 프로젝트에서 AI 모델 개발과 테스트를 맡았습니다. 이후 KHUTHON에서는 문화유산 포스터를 실시간으로 생성할 범위를 검토했고, COSS 창업경진대회에서는 휴대폰으로 스캔한 방의 3D 품질 문제에 Tripo AI를 활용했습니다. [부트캠프 모델 코드](https://github.com/leehyeoksu/-teamproject/commit/6950680c77beeb5a852075ed823dda93c3ace71b) · [대회 프로젝트](#대회)

## 연구

### DeiT의 Attention Sink 완화를 위한 게이트 기반 Self-Attention 구조

**2026 한국컴퓨터종합학술대회(KCC) 포스터 · 공동저자**

배구 선수가 나온 사진에서 모델이 배경의 몇몇 패치에 높은 어텐션을 주는 이유가 궁금했습니다. 특정 토큰에 어텐션이 몰리는 *attention sink* 연구와 언어모델의 게이트 실험을 읽고, **“불필요한 어텐션 출력을 줄일 수 있다면, DeiT에서도 배경 패치에 몰리는 경향이 달라질까?”**라는 가설을 세웠습니다.

- **설계와 구현.** 여러 어텐션 헤드의 출력을 합친 뒤 출력 투영층을 통과하기 직전에, 입력에 따라 0~1 값을 내는 시그모이드 게이트를 적용했습니다. 전체 12개 층에서 패치 토큰의 출력을 조절하고, 이미지 전체의 정보를 모아 분류하는 CLS 토큰의 게이트는 1로 유지했습니다. [게이트 구현](https://github.com/leehyeoksu/gated-vit-attention-study/blob/1b48e881891e5365b8e3fd3081af56312579c4de/src/re_make_Gate.py#L87-L105)
- **결과와 해석.** Tiny ImageNet에서 최고 검증 정확도는 기본 모델 **82.10%**, 게이트 모델 **80.71%**였습니다. 정확도는 1.39%p 낮아졌습니다. 논문의 선수 사진에서는 일부 헤드의 강한 어텐션이 사람 쪽에 모이는 변화를 관찰했습니다. 그 변화가 다른 이미지에서도 반복되는지는 후속 실험 과제로 남겼습니다. [실험 결과와 시각화](https://github.com/leehyeoksu/gated-vit-attention-study/blob/1b48e881891e5365b8e3fd3081af56312579c4de/README.md#L82-L114)

<details>
  <summary><b>실험 그림 보기 · 마지막 층에서 여섯 헤드가 본 이미지 영역</b></summary>
  <p>각 열은 하나의 어텐션 헤드입니다. 첫째 줄은 기본 모델, 둘째 줄은 게이트 모델에서 CLS가 이미지 패치에 준 어텐션입니다. 셋째 줄은 두 값의 차이, 마지막 줄은 게이트 값입니다.</p>
  <img width="680" alt="DeiT 마지막 층의 여섯 헤드에 대해 기본 모델과 게이트 모델의 어텐션 분포, 두 모델의 차이, 게이트 값을 비교한 그림" src="https://raw.githubusercontent.com/leehyeoksu/gated-vit-attention-study/1b48e881891e5365b8e3fd3081af56312579c4de/docs/images/deit_layer_11_comparison.png">
</details>

## 대회

### 이어줄게 · KHUTHON 2026 우수상

첨성대를 3D로 둘러본 사람이 문화유산을 소재로 창작하고 실제 방문까지 이어갈 수 있는 서비스입니다. 저는 해커톤 시간 안에 어떤 창작 기능을 실시간으로 제공할지 논의하고 시연 버전을 테스트했습니다. 이미지에는 OpenAI 생성 API를 연결하고, 음악과 영상에는 분위기별로 준비한 콘텐츠를 썼습니다. 예를 들어 ‘사이버 신라’를 고르면 사용자가 쓴 문장에 네온 조명과 신라 문양, 첨성대 형태를 유지하는 조건을 함께 담아 포스터를 생성합니다. [기획서](https://github.com/leehyeoksu/khuthon_chuchuss/blob/0d1783aa7135fcc004acfa4ae1941a18a1327c6f/01_기획.md#L151-L167) · [스타일별 프롬프트](https://github.com/leehyeoksu/khuthon_chuchuss/blob/0d1783aa7135fcc004acfa4ae1941a18a1327c6f/ieojulge/src/data/contentMap.ts#L27-L51)

### 이어집 · 2026 Let’s GO! COSS 스타트업 경진대회 최우수상

원룸 사진만으로 방의 깊이나 가구 사이 간격을 판단하기 어려운 점에서 출발했습니다. 중개업소에서 **매물 사진 확인 → VR로 실내 둘러보기 → 실제 방문할 집 선택**으로 이어지는 상담 흐름을 설계하고 기획서를 작성했습니다. 휴대폰으로 방을 3D 스캔했을 때 흰 점 형태의 노이즈가 나타나는 문제를 확인했고, 개선 과정에서 Tripo AI를 활용했습니다. 시제품에서는 촬영 경로를 따라 방이 잘 보이는 위치를 차례로 둘러볼 수 있습니다. [프로젝트](https://github.com/leehyeoksu/ieojip-vr-demo) · [시연 경로를 정한 이유](https://github.com/leehyeoksu/ieojip-vr-demo/blob/1e9d35d2a7e80f6d25aeeff405a80f4d92162ed0/7_프론트_시연_운영가이드.md#L5-L11)
