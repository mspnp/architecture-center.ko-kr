---
title: HA/DR용으로 빌드된 다중 계층 웹 애플리케이션
titleSuffix: Azure Example Scenarios
description: Azure Virtual Machines, 가용성 집합, 가용성 영역, Azure Site Recovery 및 Azure Traffic Manager를 사용하여 Azure의 고가용성 및 재해 복구를 위한 다중 계층 웹 애플리케이션을 만듭니다.
author: sujayt
ms.date: 11/16/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: product-team
social_image_url: /azure/architecture/example-scenario/infrastructure/media/arhitecture-disaster-recovery-multi-tier-app.png
ms.openlocfilehash: 1f82f0bf5421bb251bda2eb60349cc74014fd454
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58249568"
---
# <a name="multitier-web-application-built-for-high-availability-and-disaster-recovery-on-azure"></a>Azure의 고가용성 및 재해 복구를 위한 다중 계층 웹 애플리케이션

이 예제 시나리오는 고가용성 및 재해 복구를 위해 빌드된 복원력 있는 다중 계층 애플리케이션을 배포해야 하는 모든 산업에 적용됩니다. 이 시나리오에서는 애플리케이션이 세 계층으로 구성됩니다.

- 웹 계층: 사용자 인터페이스를 포함하는 최상위 레이어입니다. 이 계층은 사용자 상호 작용을 구문 분석하고 작업을 다음 계층으로 전달하여 처리합니다.
- 비즈니스 계층: 사용자 상호 작용을 처리하고 다음 단계에 대한 논리적 결정을 내립니다. 이 계층은 웹 계층과 데이터 계층을 연결합니다.
- 데이터 계층: 애플리케이션 데이터를 저장합니다. 일반적으로 데이터베이스, 개체 스토리지 또는 파일 스토리지가 사용됩니다.

일반적인 애플리케이션 시나리오에는 Windows 또는 Linux에서 실행되는 모든 업무상 중요한 애플리케이션이 포함됩니다. SAP 및 SharePoint 같은 기성품 애플리케이션 또는 사용자 지정 LOB(기간 업무) 애플리케이션이 이에 해당합니다.

## <a name="relevant-use-cases"></a>관련 사용 사례

관련된 다른 사용 사례는 다음과 같습니다.

- SAP 및 SharePoint처럼 복원력이 뛰어난 애플리케이션 배포
- LOB(기간 업무) 애플리케이션을 위한 비즈니스 연속성 및 재해 복구 계획 디자인
- 재해 복구를 구성하고 규정 준수를 위해 관련 연습 수행

## <a name="architecture"></a>아키텍처

이 시나리오에서는 ASP.NET 및 Microsoft SQL Server를 사용하는 다중 계층 애플리케이션을 보여줍니다. [가용성 영역을 지원하는 Azure 지역](/azure/availability-zones/az-overview#regions-that-support-availability-zones)에서, 원본 지역의 VM(가상 머신)을 여러 가용성 영역에 배포하고 재해 복구에 사용되는 대상 지역에 VM을 복제할 수 있습니다. 가용성 영역을 지원하지 않는 Azure 지역에서는 가용성 집합 내에서 VM을 배포하고 대상 영역에 VM을 복제할 수 있습니다.

![복원력이 우수한 다중 계층 웹 애플리케이션의 아키텍처 개요][architecture]

- 영역을 지원하는 지역의 두 가용성 영역에 걸쳐 각 계층에 VM을 배포합니다. 다른 지역에서는 한 가용성 집합의 각 계층에 VM을 배포합니다.
- 데이터베이스 계층에서는 Always On 가용성 그룹을 사용하도록 구성할 수 있습니다. 이 SQL Server 구성을 사용하면 클러스터 내에서 하나의 주 데이터베이스가 최대 8개의 보조 데이터베이스로 구성됩니다. 주 데이터베이스에 문제가 발생하면 클러스터가 보조 데이터베이스 중 하나로 장애 조치(failover)되어 애플리케이션 가용성이 유지됩니다. 자세한 내용은 [SQL Server에 대한 Always On 가용성 그룹 개요][docs-sql-always-on]를 참조하세요.
- 재해 복구 시나리오의 경우 재해 복구에 사용되는 대상 지역에 SQL Always On 비동기 기본 복제를 구성할 수 있습니다. 또한 데이터 변경 속도가 Azure Site Recovery에서 지원되는 한도를 초과하지 않는 경우 대상 지역으로 Azure Site Recovery 복제를 구성할 수 있습니다.
- 사용자는 Traffic Manager 엔드포인트를 통해 프런트 엔드 ASP.NET 웹 계층에 액세스합니다.
- Traffic Manager는 주 원본 지역의 기본 공용 IP 엔드포인트로 트래픽을 리디렉션합니다.
- 공용 IP는 공용 부하 분산 장치를 통해 웹 계층 VM 인스턴스 중 하나에 대한 호출을 리디렉션합니다. 모든 웹 계층 VM 인스턴스는 한 서브넷에 있습니다.
- 웹 계층 VM에서 각 호출은 내부 부하 분산 장치를 통해 비즈니스 계층의 VM 인스턴스 중 하나로 라우팅되어 처리됩니다. 모든 비즈니스 계층 VM은 별도의 서브넷에 있습니다.
- 작업은 비즈니스 계층에서 처리되고 ASP.NET 애플리케이션은 Azure 내부 부하 분산 장치를 통해 백 엔드 계층의 Microsoft SQL Server 클러스터에 연결합니다. 이러한 백 엔드 SQL Server 인스턴스는 별도의 서브넷에 있습니다.
- Traffic Manager의 보조 엔드포인트는 재해 복구에 사용되는 대상 지역에 공용 IP로 구성됩니다.
- 주 지역이 중단될 경우 Azure Site Recovery 장애 조치(failover)가 호출되고 애플리케이션이 대상 지역에서 활성화됩니다.
- Traffic Manager 엔드포인트는 자동으로 클라이언트 트래픽을 대상 지역의 공용 IP로 리디렉션합니다.

### <a name="components"></a>구성 요소

- [가용성 집합][docs-availability-sets]을 사용하면 Azure에 배포한 VM이 클러스터의 격리된 여러 하드웨어 노드에 분산되도록 할 수 있습니다. Azure 내에서 하드웨어 또는 소프트웨어 오류가 발생하더라도 VM의 하위 집합에만 영향이 있고 전체 솔루션은 계속 작동하므로 계속 사용할 수 있습니다.
- [가용성 영역][docs-availability-zones]은 데이터 센터 오류로부터 애플리케이션과 데이터를 보호합니다. 가용성 영역은 Azure 지역 내에서 독립된 물리적 위치입니다. 각 영역은 독립된 전원, 냉각 및 네트워킹을 갖춘 하나 이상의 데이터 센터로 구성됩니다.
- [ASR(Azure Site Recovery)][docs-azure-site-recovery]를 사용하여 VM을 다른 Azure 지역으로 복제하면 비즈니스 연속성 및 재해 복구 요구 사항을 충족할 수 있습니다. 규정 준수 요구 사항을 충족하도록 정기적인 재해 복구 훈련을 수행할 수 있습니다. VM은 원본 지역에서 중단이 발생한 경우 애플리케이션을 복구할 수 있도록 선택한 지역에 지정된 설정을 사용하여 복제됩니다.
- [Azure Traffic Manager][docs-traffic-manager]는 트래픽을 글로벌 Azure 지역의 서비스에 적절하게 분산하는 한편, 고가용성과 빠른 응답성을 제공하는 DNS 기반 트래픽 부하 분산 장치입니다.
- [Azure 부하 분산 장치][docs-load-balancer]는 정의된 규칙 및 상태 프로브에 따라 인바운드 트래픽을 분산합니다. 부하 분산 장치는 짧은 대기 시간과 높은 처리량을 제공하고, 모든 TCP 및 UDP 애플리케이션을 처리할 수 있도록 최대 수백만 개의 흐름으로 확장됩니다. 공용 부하 분산 장치는 들어오는 클라이언트 트래픽을 웹 계층으로 분산하는 시나리오에 사용됩니다. 내부 부하 분산 장치는 비즈니스 계층의 트래픽을 백 엔드 SQL Server 클러스터로 분산하는 시나리오에 사용됩니다.

### <a name="alternatives"></a>대안

- 인프라의 어떤 것도 운영 체제에 종속되지 않으므로 Windows를 다른 운영 체제로 대체할 수 있습니다.
- [Linux용 SQL Server][docs-sql-server-linux]는 백 엔드 데이터 저장소를 대체할 수 있습니다.
- 데이터베이스를 사용 가능한 아무 표준 데이터베이스 애플리케이션으로 대체할 수 있습니다.

## <a name="other-considerations"></a>기타 고려 사항

### <a name="scalability"></a>확장성

크기 조정 요구 사항에 따라 각 계층에서 VM을 추가 또는 제거할 수 있습니다. 이 시나리오에서는 부하 분산 장치를 사용하므로 애플리케이션 가동 시간에 영향을 주지 않고 계층에 더 많은 VM을 추가할 수 있습니다.

다른 확장성 항목에 대해서는 Azure 아키텍처 센터의 [확장성 검사 목록][scalability]을 참조하세요.

### <a name="security"></a>보안

프론트 엔드 애플리케이션 계층으로 전송되는 모든 가상 네트워크 트래픽은 네트워크 보안 그룹을 통해 보호됩니다. 규칙은 프런트 엔드 애플리케이션 계층 VM 인스턴스만 백 엔드 데이터베이스 계층에 액세스할 수 있도록 트래픽 흐름을 제한합니다. 비즈니스 계층 또는 데이터베이스 계층에는 아웃바운드 인터넷 트래픽이 허용되지 않습니다. 공격 공간을 줄이기 위해 직접 원격 관리 포트가 열려 있지 않습니다. 자세한 내용은 [Azure 네트워크 보안 그룹][docs-nsg]을 참조하세요.

보안 시나리오 설계에 대한 일반적인 지침은 [Azure 보안 설명서][security]를 참조하세요.

## <a name="pricing"></a>가격

Azure Site Recovery를 사용하여 Azure VM에 대한 재해 복구를 구성하면 지속적으로 다음 요금이 부과됩니다.

- VM당 Azure Site Recovery 라이선스 비용.
- 원본 VM 디스크에서 다른 Azure 지역으로 데이터 변경 내용을 복제하기 위한 네트워크 송신 비용. Azure Site Recovery는 기본 압축을 사용하여 데이터 전송 요구 사항을 약 50% 줄입니다.
- 복구 사이트의 스토리지 비용. 일반적으로 원본 지역 스토리지와 복구용 스냅숏으로 복구 지점을 유지하는 데 필요한 추가 스토리지를 합친 양과 동일합니다.

가상 머신 6개를 사용하는 3계층 애플리케이션에 대한 재해 복구를 구성할 수 있는 [샘플 비용 계산기][calculator]가 제공됩니다. 모든 서비스는 비용 계산기에 미리 구성되어 있습니다. 특정 사용 사례의 가격 책정이 어떻게 변동되는지 확인하려면 예상 사용량에 맞게 변수를 적절하게 변경하세요.

<!-- links -->
[architecture]: ./media/arhitecture-disaster-recovery-multi-tier-app.png
[autoscaling]: /azure/architecture/best-practices/auto-scaling
[availability]: ../../checklist/availability.md
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[docs-availability-zones]: /azure/availability-zones/az-overview
[docs-load-balancer]: /azure/load-balancer/load-balancer-overview
[docs-nsg]: /azure/virtual-network/security-overview
[docs-vmss]: /azure/virtual-machine-scale-sets/overview
[docs-sql-always-on]: /sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server
[docs-vmss-autoscale]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview
[docs-vnet]: /azure/virtual-network/virtual-networks-overview
[docs-sql-server-linux]: /sql/linux/sql-server-linux-overview?view=sql-server-linux-2017
[docs-traffic-manager]: /azure/traffic-manager/
[docs-azure-site-recovery]: /azure/site-recovery/azure-to-azure-quickstart/
[docs-availability-sets]: /azure/virtual-machines/windows/manage-availability/
[calculator]: https://azure.com/e/6835332265044d6d931d68c917979e6d/
