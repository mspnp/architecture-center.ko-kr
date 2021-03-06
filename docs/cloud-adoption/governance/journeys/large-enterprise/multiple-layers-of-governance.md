---
title: 'CAF: 대규모 엔터프라이즈 – 여러 단계의 대기업에 거 버 넌 스'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 대규모 엔터프라이즈 – 여러 단계의 대기업에 거 버 넌 스
author: BrianBlanchard
ms.openlocfilehash: 1a90f8007077df0ecefa8ec5d8c0dd6bfca9ccc7
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59641215"
---
# <a name="multiple-layers-of-governance-in-large-enterprises"></a>대기업 거버넌스의 여러 계층

대기업에 여러 거버넌스 계층이 필요할 경우 복잡성의 수준이 더 높아져 이를 거버넌스 MVP와 향후 거버넌스 진화에 반영해야 합니다.

이러한 복잡성의 몇 가지 일반적인 예에는 다음이 포함됩니다.

- 배포된 거버넌스 기능
- 비즈니스 단위 IT 조직을 지원하는 기업 IT
- 지리적으로 분산된 IT 조직을 지원하는 기업 IT

이 문서에서는 몇 가지 방법을 통해 이러한 복잡성 유형을 모색해 봅니다.

## <a name="large-enterprise-governance-is-a-team-sport"></a>대기업 거버넌스는 팀 스포츠

대규모로 이루어진 기업에서는 이 과정 전체에서 설명하는 분야에 중점을 두는 팀이나 직원이 있는 경우가 많습니다. 이 과정에서는 거버넌스를 팀 스포츠로 고려하는 방법을 제시합니다.

많은 대기업에는 다섯 가지 분야의 클라우드 거 버 넌 스 블 도입 될 수 있습니다. 기업 전체에 대한 ID, 보안, 운영, 배포 및 구성의 클라우드 전문 기술을 개발하는 데는 시간이 소요됩니다. 전체적으로 구현되는 IT 거버넌스 정책과 IT 보안의 경우 몇 달에서 심지어 몇 년까지 혁신이 지연될 수 있습니다. 혁신의 요구와, 기존 리소스 보호에 대한 거버넌스 요구의 균형을 맞추는 것은 정교한 문제입니다.

클라우드 본연의 기능은 혁신의 저해 요소는 차단하지만 위험은 증대할 수 있습니다. 이 거버넌스 과정에서는 위험 완화를 위해 회사가 가이드 라인을 마련하는 예제를 살펴보았습니다. 환경 보호를 위해 필요한 각 분야를 처리하기 보다는 클라우드 거버넌스 팀이 위험에 기반한 방식을 주도하여 배포할 대상을 관리하고 다른 팀은 필요한 클라우드 완성도를 구축하는 것입니다. 가장 중요한 것은 각 팀이 클라우드 완성도에 도달하면 거버넌스가 솔루션을 전체적으로 적용하는 것입니다. 각 팀이 완성되고 전체 솔루션에 추가될수록 클라우드 거버넌스 팀은 다음 단계를 시작하여 발전을 위한 추가적인 혁신과 도입을 추구할 수 있습니다.

이 모델에서는 클라우드 거버넌스 팀과 기존 기업 팀(보안, IT 거버넌스, 네트워킹, ID 및 기타)의 협력 증대를 설명합니다. 이 과정은 거버넌스 MVP에서 출발하여 거버넌스 진화에 따라 종합적인 최종 상태로 증대합니다.

## <a name="requirements-to-supporting-such-a-team-sport"></a>팀 스포츠와 같은 지원 요구 사항

다계층 거버넌스 모델의 첫 번째 요구 사항은 거버넌스 계층 구조를 이해하는 것입니다. 다음 질문에 대답 일반 거 버 넌 스 계층 구조를 이해 하는 데 도움이 됩니다.

- 비즈니스 단위 전체에 클라우드 계정(클라우드 서비스 요금 청구)이 할당되는 방식
- 기업 IT와 각 비즈니스 단위에 거버넌스 책무가 할당되는 방식
- 각 IT 단위에서 관리하는 환경의 유형

## <a name="central-governance-of-a-distributed-governance-hierarchy"></a>분산된 거버넌스 계층 구조의 중앙 거버넌스

관리 그룹 같은 도구를 통해 기업 IT는 거버넌스 계층 구조에 부합하는 계층 구조를 만들 수 있습니다. Azure Blueprints 같은 도구에서는 자산을 해당 계층 구조의 다른 계층에 적용할 수 있습니다. Azure Blueprints는 버전을 지정할 수 있고 다양한 버전을 관리 그룹, 구독 또는 리소스 그룹에 적용할 수 있습니다. 각 개념에 대한 자세한 설명은 거버넌스 MVP에 자세히 나와 있습니다.

이러한 도구 각각의 중요한 부분은 여러 청사진을 계층 구조에 적용하는 기능입니다. 이를 통해 거버넌스를 계층화된 프로세스로 적용할 수 있습니다. 거버넌스를 계층적으로 적용하는 한 예는 다음과 같습니다.

- 기업 IT: 기업 IT에서는 모든 클라우드 도입에 적용되는 표준 및 정책 세트를 만듭니다. 이것이 "기준" 청사진에 구체화됩니다. 그런 다음, 기업 IT가 관리 그룹 계층 구조를 소유하여 기준의 버전이 계층 구조의 모든 구독에 적용되게 합니다.
- 지역 또는 비즈니스 단위 IT: 여러 IT 팀이 자체 청사진을 만들어 거버넌스의 추가 계층을 적용할 수 있습니다. 이러한 청사진에는 추가적인 정책과 표준이 따릅니다. 개발 후 기업 IT가 관리 그룹 계층 구조 안에서 이러한 청사진을 해당 노드에 적용합니다.
- 클라우드 도입 팀: 거버넌스 요구 사항 컨텍스트 안에서 애플리케이션 또는 워크로드에 대한 상세 의사 결정 및 구현은 클라우드 도입 팀에서 수행할 수 있습니다. 때때로 팀은 추가적인 Azure Resource Consistency 템플릿을 요청하여 도입 노력을 가속화할 수도 있습니다.

각 수준의 거버넌스 구현에 관한 세부 사항에서는 팀 간의 조정이 필요합니다. 이 과정에서 개괄적으로 설명하는 거버넌스 MVP와 거버넌스 진화가 이러한 조정에 도움이 될 수 있습니다.
