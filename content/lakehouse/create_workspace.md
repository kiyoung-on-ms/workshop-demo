+++
title = "Fabric 작업영역 만들기"
date = 2024-10-27T22:32:15+09:00
pageRef = '/'
weight = 12
chapter = true
pre = "<b> </b>"
+++

### 필수 조건

* 무료 [Microsoft Fabric 평가판](https://learn.microsoft.com/ko-kr/fabric/get-started/fabric-trial)에 등록
* Fabric 평가판에는 Power BI 라이선스가 필요합니다. 라이선스가 없는 경우 [Power BI 무료 라이선스에 등록](https://app.fabric.microsoft.com/)

### 작업 영역 만들기
이 단계에서는 Fabric 작업 영역을 만듭니다. 작업 영역에는 Lakehouse, 데이터 흐름, Data Factory 파이프라인, Notebook, Power BI 의미 체계 모델 및 보고서를 포함하는 이 Lakehouse 자습서에 필요한 모든 항목이 포함됩니다.

1. [Power BI](https://powerbi.com/)에 로그인합니다.
2. **작업 영역**, **새로운 작업 영역**를 차례로 선택합니다.

데이터 저장소의 통합과 Delta Lake 형식의 표준화를 사용하는 Fabric을 사용하면 사일로를 제거하고 데이터 중복을 제거하며 총 소유 비용을 크게 줄일 수 있습니다.

Fabric에서 제공하는 유연성을 통해 Lakehouse 또는 데이터 Warehouse 아키텍처를 구현하거나 함께 결합하여 간단한 구현으로 둘 다 최대한 활용할 수 있습니다. 이 자습서에서는 리테일 조직의 예를 들어 처음부터 끝까지 Lakehouse를 빌드합니다. 브론즈 계층에 원시 데이터가 있고, 실버 계층에 유효성이 검사되고 중복 제거된 데이터가 있으며, 골드 계층에 고도로 세련된 데이터가 있는 medallion 아키텍처를 사용합니다. 모든 업계의 모든 조직에 대해 Lakehouse를 구현하는 동일한 접근 방식을 취할 수 있습니다.

이 자습서에서는 리테일 도메인의 가상 Wide World Importers 회사의 개발자가 다음 단계를 완료하는 방법을 설명합니다.

Power BI 계정에 로그인하고 무료 Microsoft Fabric 평가판에 등록합니다. Power BI 라이선스 가 없는 경우 Power BI 무료 라이선스에 등록한 다음 Fabric 평가판을 시작할 수 있습니다.

조직에 대한 엔드 투 엔드 Lakehouse를 빌드하고 구현합니다.

Fabric 작업 영역 만들기.
Lakehouse를 만들기.
데이터를 수집하고, 데이터를 변환하고, Lakehouse에 로드합니다. Lakehouse 모드 및 SQL 분석 엔드포인트 모드에서 하나의 데이터 복사본인 OneLake를 탐색할 수도 있습니다.
SQL 분석 엔드포인트를 사용하여 Lakehouse에 연결하고 DirectLake를 사용하여 Power BI 보고서를 만들어 다양한 차원의 판매 데이터를 분석합니다.
필요에 따라 파이프라인을 사용하여 데이터 수집 및 변환 흐름을 오케스트레이션하고 예약할 수 있습니다.
작업 영역 및 기타 항목을 삭제하여 리소스를 정리합니다.

### 아키텍처

다음 이미지는 Lakehouse 엔드 투 엔드 아키텍처를 보여줍니다. 다음 표에는 두 가지 해당 구성 요소가 정리되어 있습니다.

![Fabric Architecture](/images/lakehouse/lakehouse-end-to-end-architecture.png)

* **데이터 원본**: Fabric을 사용하면 간소화된 데이터 수집을 위해 Azure Data Services뿐만 아니라 다른 클라우드 기반 플랫폼 및 온-프레미스 데이터 원본에 빠르고 쉽게 연결할 수 있습니다.

* **수집**: 200개 이상의 네이티브 커넥터를 사용하여 조직에 대한 인사이트를 빠르게 작성할 수 있습니다. 이러한 커넥터는 Fabric 파이프라인에 통합되며 데이터 흐름을 사용하여 사용자에게 친숙한 끌어서 놓기 데이터 변환을 활용합니다. 또한 Fabric의 바로 가기 기능을 사용하면 복사하거나 이동하지 않고도 기존 데이터에 연결할 수 있습니다.

* **변환 및 저장**: Fabric은 Delta Lake 형식으로 표준화됩니다. 즉, 모든 Fabric 엔진은 데이터를 복제하지 않고 OneLake에 저장된 동일한 데이터 세트에 액세스하고 조작할 수 있습니다. 이 스토리지 시스템은 조직의 요구 사항에 따라 medallion 아키텍처 또는 데이터 메시를 사용하여 Lakehouse를 유연하게 빌드할 수 있습니다. 코드 우선 환경을 위해 파이프라인/데이터 흐름 또는 Notebook/Spark를 활용하여 데이터 변환을 위한 하위 코드 또는 코드 없음 환경 중에서 선택할 수 있습니다.

* **사용**: Power BI는 보고 및 시각화를 위해 Lakehouse의 데이터를 사용할 수 있습니다. 각 Lakehouse에는 다른 보고 도구에서 Lakehouse 테이블의 데이터를 쉽게 연결하고 쿼리할 수 있도록 SQL 분석 엔드포인트라는 기본 제공 TDS 엔드포인트 가 있습니다. SQL 분석 엔드포인트는 사용자에게 SQL 연결 기능을 제공합니다.

