# 이혁수 · Hyeoksu Lee

차량과 주행 데이터를 다루는 백엔드 개발에 관심이 있습니다. Spring Boot와 PostgreSQL로 데이터를 저장하고 조회하는 API를 만들고, Python으로 작성한 검색·추론 기능을 서비스에 연결해 왔습니다. 성능을 개선할 때는 처리 시간과 함께 같은 입력에서 같은 결과가 나오는지도 확인합니다.

<p>
  <img alt="Java" src="https://img.shields.io/badge/Java-ED8B00?style=flat-square">
  <img alt="Spring Boot" src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white">
  <img alt="AWS" src="https://img.shields.io/badge/AWS-232F3E?style=flat-square">
  <img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white">
</p>

## 주요 프로젝트

<table>
  <tr>
    <td width="50%" valign="top">
      <h3><a href="https://github.com/cloud-project-mobility/make_drive_mate">DriveMate</a></h3>
      <p><b>차량 관리와 공매 참고 가격 조회</b></p>
      <p>차량·정비 기록과 공매 데이터를 한 서비스에서 조회할 수 있도록 Spring 백엔드와 Python 데이터 처리 작업을 연결했습니다. 온비드 데이터를 가져올 때 원본을 추적하고 중복을 확인하는 흐름을 다뤘습니다.</p>
      <a href="https://github.com/cloud-project-mobility/make_drive_mate/blob/dde036b0c88f95a2e0241ce6d6605b30673eeb14/README.md#L65-L90">서비스 구조 보기 →</a>
    </td>
    <td width="50%" valign="top">
      <h3><a href="https://github.com/DriveScene2Label-Team/DriveScene2Label">DriveScene2Label</a></h3>
      <p><b>주행 장면 검색과 3D 라벨링</b></p>
      <p>nuScenes 주행 데이터를 PostgreSQL에 적재하고, 문장으로 장면을 검색해 3D 라벨링 작업으로 넘기는 API를 연결했습니다. 검색 SQL과 텍스트 임베딩 캐시를 개선하고 변경 전후의 응답을 비교했습니다.</p>
      <a href="https://github.com/DriveScene2Label-Team/DriveScene2Label/blob/d26c246a7e718cc17173e0e0d6696ad9a9f41e15/docs/optimization-before-after-2026-11-19.md#L14">검색 성능 실험 보기 →</a>
    </td>
  </tr>
</table>

## 연구

**DeiT의 Attention Sink 완화를 위한 게이트 기반 Self-Attention 구조** · KCC 2026 포스터 공동저자

언어모델에서 어텐션 출력을 조절하는 게이트가 특정 토큰으로의 쏠림을 줄였다는 연구를 읽고, 이미지 분류 모델인 DeiT에서도 비슷한 변화가 나타날지 가설을 세웠습니다. 게이트를 적용할 위치를 정하고 기본 모델과 분류 정확도, 이미지 패치별 어텐션 분포를 비교했습니다. [연구 코드와 실험 결과](https://github.com/leehyeoksu/gated-vit-attention-study)

## 대회

| 대회 · 수상 | 프로젝트와 기여 |
| --- | --- |
| **KHUTHON 2026 우수상** | **이어줄게** · 문화유산을 이미지·음악·영상으로 재창작하는 서비스에서 OpenAI 이미지 생성 API를 활용할 범위를 검토하고 시연 버전을 테스트했습니다. [프로젝트](https://github.com/leehyeoksu/khuthon_chuchuss) |
| **2026 Let’s GO! COSS 스타트업 경진대회 최우수상** | **이어집** · 고객이 원룸 사진을 보고 VR로 방을 둘러본 뒤 방문할 집을 고르는 상담 흐름을 설계하고 기획서를 작성했습니다. 휴대폰 3D 스캔에 흰 점 형태의 노이즈가 나타나는 문제를 개선하는 과정에서 Tripo AI를 활용했습니다. [프로젝트](https://github.com/leehyeoksu/ieojip-vr-demo) |
