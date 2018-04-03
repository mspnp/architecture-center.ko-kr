---
title: "Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현"
description: "Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지를 구현하는 방법입니다."
author: telmosampaio
ms.date: 02/25/2018
pnp.series.title: Implement a hub-spoke network topology with shared services in Azure
pnp.series.prev: hub-spoke
ms.openlocfilehash: c0fb1d1ddd7c70ed914d58e7c73b10475b91aedf
ms.sourcegitcommit: 2123c25b1a0b5501ff1887f98030787191cf6994
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2018
---
# <a name="implement-a-hub-spoke-network-topology-with-shared-services-in-azure"></a>Azure에서 공유 서비스를 사용하여 허브-스포크 네트워크 토폴로지 구현

이 참조 아키텍처는 모든 스포크에서 사용할 수 있는 허브의 공유 서비스를 포함하는 [hub-spoke][guidance-hub-spoke] 아키텍처에 기반합니다. 데이터 센터를 클라우드로 마이그레이션하고 [가상 데이터 센터]를 빌드하는 첫 번째 단계로 공유해야 하는 첫 번째 서비스는 ID와 보안입니다. 이 참조 아키텍처에서는 온-프레미스 데이터 센터에서 Azure로 Active Directory 서비스를 확장하는 방법 및 허브-스포크 토폴로지에서 방화벽으로 사용할 수 있는 NVA(네트워크 가상 어플라이언스)를 추가하는 방법을 보여줍니다.  [**이 솔루션을 배포합니다**](#deploy-the-solution).

![[0]][0]

*이 아키텍처의 [Visio 파일][visio-download] 다운로드*

이 토폴로지의 이점은 다음과 같습니다.

* **비용 절감** 네트워크 가상 어플라이언스(NVA), DNS 서버와 같이 여러 워크로드에서 공유할 수 있는 서비스를 하나의 위치로 일원화하여 비용을 절감합니다.
* **구독 제한 극복**: 여러 구독의 VNet을 중앙 허브로 피어링하여 구독의 제한을 극복합니다.
* **문제 구분**: 중앙 IT(SecOps, InfraOps)와 워크로드(DevOps)를 문제를 분리합니다.

이 아키텍처의 일반적인 용도는 다음과 같습니다.

* 개발, 테스트, 생산과 같이 서로 다른 환경에 배포되고 DNS, IDS, NTP 또는 AD DS와 같은 공유 서비스가 필요한 워크로드. 공유 서비스는 허브 VNet에 배치되고 각 환경은 스포크에 배포되어 격리 상태 유지.
* 서로 연결할 필요가 없으나 공유 서비스에 대한 액세스가 필요한 워크로드.
* DMZ로 기능한 허브의 방화벽, 각 스포크에서 워크로드에 대한 별도의 관리 등 보안 측면에 대한 중앙 제어가 필요한 엔터프라이즈.

## <a name="architecture"></a>건축

이 아키텍처는 다음 구성 요소로 구성됩니다.

* **온-프레미스 네트워크**. 조직 내에서 실행되는 개인 로컬 영역 네트워크입니다.

* **VPN 장치**. 온-프레미스 네트워크에 외부 연결을 제공하는 장치 또는 서비스입니다. VPN 장치는 하드웨어 장치일 수도 있고 Windows Server 2012의 RRAS(라우팅 및 원격 액세스 서비스)와 같은 소프트웨어 솔루션일 수도 있습니다. 지원되는 VPN 어플라이언스 목록 및 선택한 VPN 어플라이언스를 Azure에 연결하도록 구성하는 방법에 대한 자세한 내용은 [사이트 간 VPN 게이트웨이 연결을 위한 VPN 장치 정보][vpn-appliance]를 참조하세요.

* **VPN 가상 네트워크 게이트웨이 또는 ExpressRoute 게이트웨이**. 가상 네트워크 게이트웨이를 사용하면 VNet을 온-프레미스 네트워크에 연결하는 데 사용되는 VPN 장치 또는 ExpressRoute 회로에 연결할 수 있습니다. 자세한 내용은 [온-프레미스 네트워크를 Microsoft Azure virtual network에 연결][connect-to-an-Azure-vnet]을 참조하세요.

> [!NOTE]
> 이 참조 아키텍처의 배포 스크립트는 연결을 위해 VPN 게이트웨이를 사용하고, 사용자의 온-프레미스 네트워크를 시뮬레이션하기 위해 Azure의 VNet을 사용합니다.

* **허브 VNet**. 허브-스포크 토폴로지에서 허브로 사용되는 Azure VNet입니다. 허브는 사용자의 온-프레미스 네트워크에 대한 연결의 중앙 위치이자 스포크 VNet에 호스팅된 다양한 워크로드에 의해 사용되는 서비스가 호스팅되는 장소입니다.

* **게이트웨이 서브넷**. 가상 네트워크 게이트웨이는 동일한 서브넷에 있습니다.

* **공유 서비스 서브넷**. DNS, AD DS를 비롯한 모든 스포크에서 공유될 수 있는 서비스를 호스팅하는 데 사용되는 허브 VNet의 서브넷입니다.

* **DMZ 서브넷**. 방화벽과 같은 보안 어플라이언스로 사용할 수 있는 NVA를 호스트하는 데 사용되는 허브 VNet의 서브넷입니다.

* **스포크 VNet**. 허브-스포크 토폴로지에서 스포크로 사용되는 하나 이상의 Azure VNet입니다. 스포크는 다른 스포크와 별도로 관리되는 자체 VNet에서 워크로드를 격리하는 데 사용할 수 있습니다. 각 워크로드에는 Azure Load Balancer를 통해 여러 서브넷이 연결된 여러 계층이 포함될 수 있습니다. 응용 프로그램 인프라에 대한 자세한 내용은 [Windows VM 워크로드 실행][windows-vm-ra] 및 [Linux VM 워크로드 실행][linux-vm-ra]을 참조하세요.

* **VNet 피어링**. 동일한 Azure 지역에 있는 2개의 VNet을 [피어링 연결][vnet-peering]을 사용하여 연결할 수 있습니다. 피어링 연결은 VNet 사이에 적용되는 비전이적이고 대기 시간이 낮은 연결입니다. 피어링이 적용되면 VNet은 라우터가 없어도 Azure 백본을 사용하여 트래픽을 교환합니다. 허브-스포크 네트워크 토폴로지에서는 VNet 피어링을 사용하여 허브를 각 스포크에 연결합니다.

> [!NOTE]
> 이 문서에서는 [리소스 관리자](/azure/azure-resource-manager/resource-group-overview) 배포만 다루고 있지만, 동일한 구독에서 클래식 VNet을 리소스 관리자에 연결할 수도 있습니다. 이렇게 하면 스포크가 클래식 배포를 호스팅하면서도 허브에서 공유되는 서비스를 이용할 수 있습니다.

## <a name="recommendations"></a>권장 사항

[hub-spoke][guidance-hub-spoke] 참조 아키텍처에 대한 모든 권장 사항은 공유 서비스 참조 아키텍처에도 적용됩니다. 

또한 공유 서비스에서 대부분의 시나리오에 다음 권장 사항이 적용됩니다. 이러한 권장 사항을 재정의하라는 특정 요구 사항이 있는 경우가 아니면 따릅니다.

### <a name="identity"></a>ID

대부분의 엔터프라이즈 조직은 온-프레미스 데이터 센터에 ADDS(Active Directory 디렉터리 서비스) 환경을 포함합니다. ADDS를 사용하는 온-프레미스 네트워크에서 Azure로 이동된 자산 관리를 용이하게 하려면 Azure에서 ADDS 도메인 컨트롤러를 호스트하는 것이 좋습니다.

Azure 및 온-프레미스 환경에서 개별적으로 제어하도록 그룹 정책 개체를 사용하는 경우 각 Azure 지역에 다른 AD 사이트를 사용합니다. 종속 워크로드가 액세스할 수 있는 중앙 VNet(허브)에 도메인 컨트롤러를 배치합니다.

### <a name="security"></a>보안

온-프레미스 환경에서 Azure로 워크로드를 이동할 때 이러한 일부 워크로드는 VM에서 호스트되어야 합니다. 규정 준수상 해당 워크로드를 트래버스하는 트래픽에 대한 제한 사항을 적용해야 합니다. 

Azure에서 NVA(네트워크 가상 어플라이언스)를 사용하여 다른 형식의 보안 및 성능 서비스를 호스트할 수 있습니다. 지정된 집합의 어플라이언스 온-프레미스에 익숙하다면 해당되는 경우 Azure에서 동일한 가상 어플라이언스를 사용하는 것이 좋습니다.

> [!NOTE]
> 이 참조 아키텍처의 배포 스크립트는 네트워크 가상 어플라이언스를 모방하기 위해 사용하는 IP를 전달하여 Ubuntu VM을 사용합니다.

## <a name="considerations"></a>고려 사항

### <a name="overcoming-vnet-peering-limits"></a>VNet 피어링 제한 문제 해결

반드시 Azure의 [VNet 하나당 허용되는 VNet 피어링 개수 제한][vnet-peering-limit]을 고려해야 합니다. 허용되는 한도보다 많은 스포크가 필요한 경우 첫 번째 수준의 스포크가 허브로서 기능하는 허브-스포크-허브-스포크 토폴로지를 만드는 방법을 고려할 수 있습니다. 다음 다이어그램은 이 토폴로지를 보여줍니다.

![[3]][3]

또한, 허브가 다수의 스포크에 맞게 확장될 수 있도록 허브에서 어떤 서비스가 공유되는지도 고려해야 합니다. 예를 들어 허브에서 방화벽 서비스를 제공한다면 복수의 스포크를 추가할 때 방화벽 솔루션의 대역폭 제한을 고려해야 합니다. 공유 서비스 중 일부를 두 번째 수준의 허브로 이동하는 것이 좋을 수 있습니다.

## <a name="deploy-the-solution"></a>솔루션 배포

이 아키텍처에 대한 배포는 [GitHub][ref-arch-repo]에서 사용할 수 있습니다. 이 아키텍처에 대한 배포는 연결을 테스트하기 위해 각 VNet에서 Ubuntu VM을 사용합니다. **허브 VNet**의 **shared-services** 서브넷에 실제로 호스팅된 서비스는 없습니다.

### <a name="prerequisites"></a>필수 조건

사용자의 구독에 참조 아키텍처를 배포하려면 먼저 다음 단계를 수행해야 합니다.

1. [AzureCAT 참조 아키텍처][ref-arch-repo] GitHub 리포지토리의 zip 파일을 복제, 포크 또는 다운로드합니다.

2. Azure CLI 2.0이 컴퓨터에 설치되어 있는지 확인합니다. CLI 설치 지침은 [Install Azure CLI 2.0][azure-cli-2](Azure CLI 2.0 설치)을 참조하세요.

3. [Azure 빌딩 블록][azbb] npm 패키지를 설치합니다.

4. 명령 프롬프트, bash 프롬프트 또는 PowerShell 프롬프트에서 아래 명령을 사용하여 Azure 계정에 로그인하고, 프롬프트에 따릅니다.

  ```bash
  az login
  ```

### <a name="deploy-the-simulated-on-premises-datacenter-using-azbb"></a>azbb를 사용하여 시뮬레이션된 온-프레미스 데이터 센터 배포

시뮬레이션된 온-프레미스 데이터 센터를 Azure VNet으로서 배포하려면 다음 단계를 수행합니다.

1. 위의 필수 조건 단계에서 다운로드한 리포지토리가 있는 `hybrid-networking\shared-services-stack\` 폴더로 이동합니다.

2. `onprem.json` 파일을 열고 아래에 나와 있는 대로 45열과 46열의 큰따옴표 사이에 사용자 이름과 암호를 입력한 다음, 파일을 저장합니다.

  ```bash
  "adminUsername": "XXX",
  "adminPassword": "YYY",
  ```

3. `azbb`를 실행하여 아래와 같이 시뮬레이션된 온-프레미스 환경을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g onprem-vnet-rg - l <location> -p onoprem.json --deploy
  ```
  > [!NOTE]
  > `onprem-vnet-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

4. 배포가 완료될 때까지 기다립니다. 이 배포는 가상 네트워크, Windows를 실행하는 가상 머신 및 VPN 게이트웨이를 생성합니다. VPN 게이트웨이의 생성이 완료되기까지 40분 이상이 걸릴 수 있습니다.

### <a name="azure-hub-vnet"></a>Azure 허브 VNet

허브 VNet을 배포하고 위에서 만든 시뮬레이션된 온-프레미스 VNet에 연결하려면 다음 단계를 수행합니다.

1. `hub-vnet.json` 파일을 열고 아래에 나와 있는 대로 50열과 51열의 큰따옴표 사이에 사용자 이름과 암호를 입력합니다.

  ```bash
  "adminUsername": "XXX",
  "adminPassword": "YYY",
  ```

2. `osType`의 52열에서 `Windows` 또는 `Linux`를 입력하여 jumpbox의 운영 체제로 Windows Server 2016 데이터 센터 또는 Ubuntu 16.04를 설치합니다.

3. 아래에 나와 있는 대로 83열의 큰따옴표 사이에 공유 키를 입력한 다음, 파일을 저장합니다.

  ```bash
  "sharedKey": "",
  ```

4. `azbb`를 실행하여 아래와 같이 시뮬레이션된 온-프레미스 환경을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g hub-vnet-rg - l <location> -p hub-vnet.json --deploy
  ```
  > [!NOTE]
  > `hub-vnet-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

5. 배포가 완료될 때까지 기다립니다. 이 배포는 가상 네트워크, 가상 머신, VPN 게이트웨이 및 이전 섹션에서 생성한 게이트웨이에 대한 연결을 생성합니다. VPN 게이트웨이의 생성이 완료되기까지 40분 이상이 걸릴 수 있습니다.

### <a name="adds-in-azure"></a>Azure의 ADDS

Azure에서 ADDS 도메인 컨트롤러를 배포하려면 다음 단계를 수행합니다.

1. `hub-adds.json` 파일을 열고 아래에 나와 있는 대로 14열과 15열의 큰따옴표 사이에 사용자 이름과 암호를 입력한 다음, 파일을 저장합니다.

  ```bash
  "adminUsername": "XXX",
  "adminPassword": "YYY",
  ```

2. 아래와 같이 `azbb`를 실행하여 ADDS 도메인 컨트롤러를 배포합니다.

  ```bash
  azbb -s <subscription_id> -g hub-adds-rg - l <location> -p hub-adds.json --deploy
  ```
  
  > [!NOTE]
  > `hub-adds-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

  > [!NOTE]
  > 두 개의 VM을 시뮬레이션된 온-프레미스 데이터 센터의 호스트 도메인에 조인한 다음, AD DS를 설치해야 하므로 배포에서 이 파트는 몇 분 정도 걸릴 수 있습니다.

### <a name="nva"></a>NVA

`dmz` 서브넷에서 NVA를 배포하려면 다음 단계를 수행합니다.

1. `hub-nva.json` 파일을 열고 아래에 나와 있는 대로 13열과 14열의 큰따옴표 사이에 사용자 이름과 암호를 입력한 다음, 파일을 저장합니다.

  ```bash
  "adminUsername": "XXX",
  "adminPassword": "YYY",
  ```
2. `azbb`를 실행하여 NVA VM 및 사용자 정의 경로를 배포합니다.

  ```bash
  azbb -s <subscription_id> -g hub-nva-rg - l <location> -p hub-nva.json --deploy
  ```
  > [!NOTE]
  > `hub-nva-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

### <a name="azure-spoke-vnets"></a>Azure 스포크 VNet

스포크 VNet을 배포하려면 다음 단계를 수행합니다.

1. `spoke1.json` 파일을 열고 아래에 나와 있는 대로 52열과 53열의 큰따옴표 사이에 사용자 이름과 암호를 입력한 다음, 파일을 저장합니다.

  ```bash
  "adminUsername": "XXX",
  "adminPassword": "YYY",
  ```

2. `osType`의 54열에서 `Windows` 또는 `Linux`를 입력하여 jumpbox의 운영 체제로 Windows Server 2016 데이터 센터 또는 Ubuntu 16.04를 설치합니다.

3. `azbb`를 실행하여 아래와 같이 첫 번째 스포크 VNet 환경을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g spoke1-vnet-rg - l <location> -p spoke1.json --deploy
  ```
  
  > [!NOTE]
  > `spoke1-vnet-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

3. `spoke2.json` 파일에서 위의 1단계를 반복합니다.

4. `azbb`를 실행하여 아래와 같이 두 번째 스포크 VNet 환경을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g spoke2-vnet-rg - l <location> -p spoke2.json --deploy
  ```
  > [!NOTE]
  > `spoke2-vnet-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

### <a name="azure-hub-vnet-peering-to-spoke-vnets"></a>스포크 VNet에 대한 Azure 허브 VNet 피어링

허브 VNet에서 스포크 VNet으로의 피어링을 연결하려면 다음 단계를 수행합니다.

1. `hub-vnet-peering.json` 파일을 열고, 29열에서 리소스 그룹 이름 및 각 가상 네트워크 피어링의 가상 네트워크 이름이 올바른지 확인합니다.

2. `azbb`를 실행하여 아래와 같이 첫 번째 스포크 VNet 환경을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g hub-vnet-rg - l <location> -p hub-vnet-peering.json --deploy
  ```

  > [!NOTE]
  > `hub-vnet-rg`이 아닌 다른 리소스 그룹 이름을 사용하려면 해당 이름을 사용하는 모든 매개 변수 파일을 검색하여 각 파일에서 사용자의 리소스 그룹 이름을 사용하도록 편집해야 합니다.

<!-- links -->

[azure-cli-2]: /azure/install-azure-cli
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[guidance-hub-spoke]: ./hub-spoke.md
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[best-practices-security]: /azure/best-practices-network-securit
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[guidance-expressroute]: ./expressroute.md
[guidance-vpn]: ./vpn.md
[linux-vm-ra]: ../virtual-machines-linux/index.md
[hybrid-ha]: ./expressroute-vpn-failover.md
[naming conventions]: /azure/guidance/guidance-naming-conventions
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[가상 데이터 센터]: https://aka.ms/vdc
[vnet-peering]: /azure/virtual-network/virtual-network-peering-overview
[vnet-peering-limit]: /azure/azure-subscription-service-limits#networking-limits
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[windows-vm-ra]: ../virtual-machines-windows/index.md

[visio-download]: https://archcenter.azureedge.net/cdn/hybrid-network-hub-spoke.vsdx
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[0]: ./images/shared-services.png "Azure의 공유 서비스 토폴로지"
[3]: ./images/hub-spokehub-spoke.svg "Azure의 허브-스포크-허브-스포크 토폴로지"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/