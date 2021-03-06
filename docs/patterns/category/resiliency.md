---
title: 복원력 패턴
titleSuffix: Cloud Design Patterns
description: 복원력은 중단 없이 오류를 처리하고 정상적으로 복구하는 시스템 기능입니다. 애플리케이션이 다중 테넌트인 경우가 많고, 공유 플랫폼 서비스를 사용하고, 리소스 및 대역폭을 두고 경합하고, 인터넷을 통해 통신하고, 주로 하드웨어에서 실행되는 클라우드 호스팅의 특성 상, 일시적 오류와 영구적 오류가 모두 발생할 가능성이 좀 더 높습니다. 복원력을 유지하려면 오류를 감지하여 신속하고 효율적으로 복구하는 기능이 필요합니다.
keywords: 디자인 패턴
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 5863cf9491434e2ddd683178591aff09c40e7315
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248898"
---
# <a name="resiliency-patterns"></a>복원력 패턴

복원력은 중단 없이 오류를 처리하고 정상적으로 복구하는 시스템 기능입니다. 애플리케이션이 다중 테넌트인 경우가 많고, 공유 플랫폼 서비스를 사용하고, 리소스 및 대역폭을 두고 경합하고, 인터넷을 통해 통신하고, 주로 하드웨어에서 실행되는 클라우드 호스팅의 특성 상, 일시적 오류와 영구적 오류가 모두 발생할 가능성이 좀 더 높습니다. 복원력을 유지하려면 오류를 감지하여 신속하고 효율적으로 복구하는 기능이 필요합니다.

|                            패턴                             |                                                                                                      요약                                                                                                       |
|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   [격벽](../bulkhead.md)                   |                                                     하나가 고장 나더라도 나머지는 정상적으로 작동하도록 애플리케이션의 요소를 여러 풀에 격리합니다.                                                      |
|            [회로 차단기](../circuit-breaker.md)            |                                                  원격 서비스 또는 리소스에 연결할 때 해결하는 데 걸리는 시간이 유동적인 오류를 처리합니다.                                                   |
|   [보정 트랜잭션](../compensating-transaction.md)   |                                                      여러 단계로 나뉘어 있지만 결국에는 일관적인 작업을 정의하는 일련의 단계에서 수행한 작업을 실행 취소합니다.                                                       |
| [상태 엔드포인트 모니터링](../health-endpoint-monitoring.md) |                                            외부 도구가 노출된 엔드포인트를 통해 주기적으로 액세스할 수 있는 기능 검사를 애플리케이션 내부에 구현합니다.                                            |
|            [리더 선택](../leader-election.md)            | 인스턴스 중 하나를 다른 인스턴스를 관리하는 리더로 선택하여 분산된 애플리케이션의 공동 작업 인스턴스 컬렉션이 수행하는 작업을 조정합니다. |
|  [큐 기반 부하 평준화](../queue-based-load-leveling.md)  |                                            작업 그리고 그 작업이 일시적인 높은 부하를 부드럽게 처리하기 위해 호출하는 서비스 사이에서 버퍼 역할을 하는 큐를 사용합니다.                                             |
|                      [다시 시도](../retry.md)                      |             이전에 실패한 작업을 투명하게 다시 시도하여 서비스 또는 네트워크 리소스에 연결하려 할 때 애플리케이션을 사용하여 예상된 일시적 오류를 처리합니다.             |
| [Scheduler 에이전트 감독자](../scheduler-agent-supervisor.md) |                                                            서비스 및 기타 원격 리소스의 분산된 집합에서 일련의 작업을 조정합니다.                                                            |