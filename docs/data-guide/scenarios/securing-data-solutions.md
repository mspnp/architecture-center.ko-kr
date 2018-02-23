---
title: "데이터 솔루션 보안"
description: 
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: 57897c31a8abdcd801874bf92d60360f7a80d1fa
ms.sourcegitcommit: 90cf2de795e50571d597cfcb9b302e48933e7f18
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2018
---
# <a name="securing-data-solutions"></a><span data-ttu-id="947c4-102">데이터 솔루션 보안</span><span class="sxs-lookup"><span data-stu-id="947c4-102">Securing data solutions</span></span>

<span data-ttu-id="947c4-103">많은 경우, 특히 온-프레미스 데이터 저장소에서만 작업하던 방식에서 전환하여 클라우드에서 데이터에 액세스할 수 있도록 하면, 해당 데이터에 대한 액세스가 증가하는 문제와 새로운 보안 유지 방법을 고민하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-103">For many, making data accessible in the cloud, particularly when transitioning from working exclusively in on-premises data stores, can cause some concern around increased accessibility to that data and new ways in which to secure it.</span></span>

## <a name="challenges"></a><span data-ttu-id="947c4-104">과제</span><span class="sxs-lookup"><span data-stu-id="947c4-104">Challenges</span></span>

* <span data-ttu-id="947c4-105">다양한 로그에 저장되어 있는 보안 이벤트를 중앙에서 모니터링하고 분석합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-105">Centralizing the monitoring and analysis of security events stored in numerous logs.</span></span>
* <span data-ttu-id="947c4-106">여러 응용 프로그램 및 서비스에서 암호화 및 권한 부여 관리를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-106">Implementing encryption and authorization management across your applications and services.</span></span>
* <span data-ttu-id="947c4-107">중앙 집중식 ID 관리가 온-프레미스 또는 클라우드의 모든 솔루션 구성 요소에 작동되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-107">Ensuring that centralized identity management works across all of your solution components, whether on-premises or in the cloud.</span></span>

## <a name="data-protection"></a><span data-ttu-id="947c4-108">데이터 보호</span><span class="sxs-lookup"><span data-stu-id="947c4-108">Data Protection</span></span>

<span data-ttu-id="947c4-109">정보를 보호하는 첫 번째 단계는 보호할 대상을 식별하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-109">The first step to protecting information is identifying what to protect.</span></span> <span data-ttu-id="947c4-110">가장 중요한 데이터 자산이 어디에 위치하든, 식별하고, 보호하고, 모니터링할 수 있는 명확하고 간단하며 잘 소통되는 지침을 개발합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-110">Develop clear, simple, and well-communicated guidelines to identify, protect, and monitor the most important data assets anywhere they reside.</span></span> <span data-ttu-id="947c4-111">조직의 업무 또는 수익성에 과도한 영향을 미치는 자산에 대해 가장 강력한 보호를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-111">Establish the strongest protection for assets that have a disproportionate impact on the organization's mission or profitability.</span></span> <span data-ttu-id="947c4-112">이러한 자산을 고부가가치 자산 또는 HVA라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-112">These are known as high value assets, or HVAs.</span></span> <span data-ttu-id="947c4-113">HVA 수명 주기 및 보안 종속성을 엄격히 분석하고, 적절한 보안 제어 및 상태를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-113">Perform stringent analysis of HVA lifecycle and security dependencies, and establish appropriate security controls and conditions.</span></span> <span data-ttu-id="947c4-114">마찬가지로 중요한 자산을 식별 및 분류하고, 보안 제어를 자동으로 적용할 기술 및 프로세스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-114">Similarly, identify and classify sensitive assets, and define the technologies and processes to automatically apply security controls.</span></span>

<span data-ttu-id="947c4-115">보호해야 하는 데이터가 식별되면 *미사용* 데이터 및 *전송 중* 데이터를 보호하는 방법을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-115">Once the data you need to protect has been identified, consider how you will protect the data *at rest* and data *in transit*.</span></span>

* <span data-ttu-id="947c4-116">**미사용 데이터**: 물리적 미디어(자기 디스크 또는 광 디스크), 온-프레미스 또는 클라우드에 정적으로 존재하는 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-116">**Data at rest**: Data that exists statically on physical media, whether magnetic or optical disk, on premises or in the cloud.</span></span>
* <span data-ttu-id="947c4-117">**전송 중 데이터**: 네트워크를 통해, 서비스 버스를 통해(온-프레미스에서 클라우드로 또는 그 반대로) 또는 입/출력 프로세스 동안 구성 요소, 위치 또는 프로그램 간에 전송되는 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-117">**Data in transit**: Data while it is being transferred between components, locations or programs, such as over the network, across a service bus (from on-premises to cloud and vice-versa), or during an input/output process.</span></span>

<span data-ttu-id="947c4-118">미사용 데이터 또는 전송 중 데이터를 보호하는 방법에 대한 자세한 내용은 [Azure 데이터 보안 및 암호화 모범 사례](/azure/security/azure-security-data-encryption-best-practices)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-118">To learn more about protecting your data at rest or in transit, see [Azure Data Security and Encryption Best Practices](/azure/security/azure-security-data-encryption-best-practices).</span></span>

## <a name="access-control"></a><span data-ttu-id="947c4-119">Access Control</span><span class="sxs-lookup"><span data-stu-id="947c4-119">Access Control</span></span>

<span data-ttu-id="947c4-120">클라우드의 데이터 보호 방식은 기본적으로 ID 관리 및 액세스 제어를 통합하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-120">Central to protecting your data in the cloud is a combination of identity management and access control.</span></span> <span data-ttu-id="947c4-121">클라우드 서비스 유형이 다양하고, [하이브리드 클라우드](../scenarios/hybrid-on-premises-and-cloud.md)의 사용도 증가하는 가운데, ID 및 액세스 제어와 관련해서 다음과 같은 몇 가지 핵심 방식을 따라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-121">Given the variety and type of cloud services, as well as the rising popularity of [hybrid cloud](../scenarios/hybrid-on-premises-and-cloud.md), there are several key practices you should follow when it comes to identity and access control:</span></span>

* <span data-ttu-id="947c4-122">ID 관리를 중앙에서 집중적으로 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-122">Centralize your identity management.</span></span>
* <span data-ttu-id="947c4-123">SSO(Single Sign-On)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-123">Enable Single Sign-On (SSO).</span></span>
* <span data-ttu-id="947c4-124">암호 관리를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-124">Deploy password management.</span></span>
* <span data-ttu-id="947c4-125">사용자에 대해 MFA(Multi-Factor Authentication)를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-125">Enforce multi-factor authentication (MFA) for users.</span></span>
* <span data-ttu-id="947c4-126">RBAC(역할 기반 액세스 제어)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-126">Use role based access control (RBAC).</span></span>
* <span data-ttu-id="947c4-127">사용자 위치, 장치 유형, 패치 수준 등과 관련된 추가 속성을 포함하는 사용자 ID의 기본 개념을 향상시키는 조건부 액세스 정책을 구성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-127">Conditional Access Policies should be configured, which enhances the classic concept of user identity with additional properties related to user location, device type, patch level, and so on.</span></span>
* <span data-ttu-id="947c4-128">Resource Manager를 사용하여 리소스가 만들어지는 위치를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-128">Control locations where resources are created using resource manager.</span></span>
* <span data-ttu-id="947c4-129">의심스러운 활동을 적극적으로 모니터링</span><span class="sxs-lookup"><span data-stu-id="947c4-129">Actively monitor for suspicious activities</span></span>

<span data-ttu-id="947c4-130">자세한 내용은 [Azure Identity Management 및 액세스 제어 보안 모범 사례](/azure/security/azure-security-identity-management-best-practices)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-130">For more information, see [Azure Identity Management and access control security best practices](/azure/security/azure-security-identity-management-best-practices).</span></span>

## <a name="auditing"></a><span data-ttu-id="947c4-131">감사</span><span class="sxs-lookup"><span data-stu-id="947c4-131">Auditing</span></span>

<span data-ttu-id="947c4-132">앞서 언급한 ID 및 액세스 모니터링 외에도, 클라우드에서 사용하는 서비스 및 응용 프로그램은 모니터링할 수 있는 보안 관련 이벤트를 생성하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-132">Beyond the identity and access monitoring previously mentioned, the services and applications that you use in the cloud should be generating security-related events that you can monitor.</span></span> <span data-ttu-id="947c4-133">이러한 이벤트를 모니터링할 때의 가장 큰 해결 과제는 잠재적인 문제를 방지하거나 지난 문제를 해결하기 위해 방대한 로그를 처리하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-133">The primary challenge to monitoring these events is handling the quantities of logs , in order to avoid potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="947c4-134">클라우드 기반 응용 프로그램은 많은 이동 부분을 포함하게 되며, 이러한 부분은 대개 일정 수준의 로깅 및 원격 분석을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-134">Cloud-based applications tend to contain many moving parts, most of which generate some level of logging and telemetry.</span></span> <span data-ttu-id="947c4-135">중앙 집중식 모니터링 및 분석을 사용하면 대량의 정보를 관리하고 이해하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-135">Use centralized monitoring and analysis to help you manage and make sense of the large amount of information.</span></span>

<span data-ttu-id="947c4-136">자세한 내용은 [Azure 로깅 및 감사](/azure/security/azure-log-audit)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-136">For more information, see [Azure Logging and Auditing](/azure/security/azure-log-audit).</span></span>



## <a name="securing-data-solutions-in-azure"></a><span data-ttu-id="947c4-137">Azure에서 데이터 솔루션 보안</span><span class="sxs-lookup"><span data-stu-id="947c4-137">Securing data solutions in Azure</span></span>

### <a name="encryption"></a><span data-ttu-id="947c4-138">암호화</span><span class="sxs-lookup"><span data-stu-id="947c4-138">Encryption</span></span>

<span data-ttu-id="947c4-139">**가상 머신**.</span><span class="sxs-lookup"><span data-stu-id="947c4-139">**Virtual machines**.</span></span> <span data-ttu-id="947c4-140">[Azure Disk Encryption](/azure/security/azure-security-disk-encryption)을 사용하여 Windows 또는 Linux VM에서 연결된 디스크를 암호화합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-140">Use [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) to encrypt the attached disks on Windows or Linux VMs.</span></span> <span data-ttu-id="947c4-141">이 솔루션은 [Azure Key Vault](/azure/key-vault/)와 통합되어 디스크 암호화 키 및 비밀을 제어 및 관리할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-141">This solution integrates with [Azure Key Vault](/azure/key-vault/) to control and manage the disk-encryption keys and secrets.</span></span> 

<span data-ttu-id="947c4-142">**Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="947c4-142">**Azure Storage**.</span></span> <span data-ttu-id="947c4-143">[Azure Storage Service Encryption](/azure/storage/common/storage-service-encryption)를 사용하여 Azure Storage에서 미사용 데이터를 자동으로 암호화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-143">Use [Azure Storage Service Encryption](/azure/storage/common/storage-service-encryption) to automatically encrypt data at rest in Azure Storage.</span></span> <span data-ttu-id="947c4-144">암호화, 암호 해독 및 키 관리는 사용자에게 완전히 투명하게 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-144">Encryption, decryption, and key management are totally transparent to users.</span></span> <span data-ttu-id="947c4-145">Azure Key Vault와 클라이언트 쪽 암호화를 사용하여 전송 중인 데이터도 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-145">Data can also be secured in transit by using client-side encryption with Azure Key Vault.</span></span> <span data-ttu-id="947c4-146">자세한 내용은 [Microsoft Azure Storage용 클라이언트 쪽 암호화 및 Azure Key Vault](/azure/storage/common/storage-client-side-encryption)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-146">For more information, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](/azure/storage/common/storage-client-side-encryption).</span></span>

<span data-ttu-id="947c4-147">**SQL Database** 및 **Azure SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="947c4-147">**SQL Database** and **Azure SQL Data Warehouse**.</span></span> <span data-ttu-id="947c4-148">TDE([투명한 데이터 암호화](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql))를 사용하여 응용 프로그램을 변경하지 않고도 데이터베이스, 연결된 백업 및 트랜잭션 로그 파일의 실시간 암호화 및 암호 해독을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-148">Use [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (TDE)  to perform real-time encryption and decryption of your databases, associated backups, and transaction log files without requiring any changes to your applications.</span></span> <span data-ttu-id="947c4-149">또한 SQL Database는 [상시 암호화](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)를 사용하여 클라이언트와 서버 간을 이동하는 동안, 그리고 데이터를 사용 중일 때 서버에서 중요한 미사용 데이터를 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-149">SQL Database can also use [Always Encrypted](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) to help protect sensitive data at rest on the server, during movement between client and server, and while the data is in use.</span></span> <span data-ttu-id="947c4-150">Azure Key Vault를 사용하여 상시 암호화의 암호화 키를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-150">You can use Azure Key Vault to store your Always Encrypted encryption keys.</span></span> 

### <a name="rights-management"></a><span data-ttu-id="947c4-151">권한 관리</span><span class="sxs-lookup"><span data-stu-id="947c4-151">Rights management</span></span>

<span data-ttu-id="947c4-152">[Azure Rights Management](/information-protection/understand-explore/what-is-azure-rms)는 암호화, ID 및 권한 부여 정책을 사용하여 파일 및 전자 메일을 보호하는 클라우드 기반 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-152">[Azure Rights Management](/information-protection/understand-explore/what-is-azure-rms) is a cloud-based service that uses encryption, identity, and authorization policies to secure files and email.</span></span> <span data-ttu-id="947c4-153">이 기능은 휴대폰, 태블릿 및 PC를 비롯한 여러 장치에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-153">It works across multiple devices—phones, tablets, and PCs.</span></span> <span data-ttu-id="947c4-154">정보가 조직의 경계를 벗어나더라도 데이터가 계속 보호되므로 조직 내부 및 외부에서 정보를 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-154">Information can be protected both within your organization and outside your organization because that protection remains with the data, even when it leaves your organization's boundaries.</span></span>

### <a name="access-control"></a><span data-ttu-id="947c4-155">Access Control</span><span class="sxs-lookup"><span data-stu-id="947c4-155">Access control</span></span>

<span data-ttu-id="947c4-156">RBAC([역할 기반 액세스 제어](/azure/active-directory/role-based-access-control-what-is))를 사용하여 사용자 역할을 기준으로 Azure 리소스에 대한 액세스를 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-156">Use [Role-Based Access Control](/azure/active-directory/role-based-access-control-what-is) (RBAC) to restrict access to Azure resources based on user roles.</span></span> <span data-ttu-id="947c4-157">Active Directory 온-프레미스를 사용하는 경우 [Azure AD와 동기화](/azure/active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements)하여 사용자에게 온-프레미스 ID를 기준으로 하는 클라우드 ID를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-157">If you are using Active Directory on-premises, you can [synchronize with Azure AD](/azure/active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements) to provide users with a cloud identity based on their on-premises identity.</span></span>

<span data-ttu-id="947c4-158">[Azure Active Directory의 조건부 액세스](/azure/active-directory/active-directory-conditional-access-azure-portal)를 사용하여 특정 조건에 따라 사용자 환경의 응용 프로그램에 대한 액세스를 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-158">Use [Conditional access in Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal) to enforce controls on the access to applications in your environment based on specific conditions.</span></span> <span data-ttu-id="947c4-159">예를 들어, 정책 설명이 _계약자가 신뢰할 수 없는 네트워크에서 클라우드 앱에 액세스하려고 시도하는 경우 액세스를 차단합니다._ 형식일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-159">For example, your policy statement could take the form of: _When contractors are trying to access our cloud apps from networks that are not trusted, then block access_.</span></span> 

<span data-ttu-id="947c4-160">[Azure AD Privileged Identity Management](/azure/active-directory/active-directory-privileged-identity-management-configure)는 사용자와 사용자가 해당 관리 권한으로 수행하는 태스크 종류를 관리, 제어 및 모니터링하는 데 도움을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-160">[Azure AD Privileged Identity Management](/azure/active-directory/active-directory-privileged-identity-management-configure) can help you manage, control, and monitor your users and what sorts of tasks they are performing with their admin privileges.</span></span> <span data-ttu-id="947c4-161">이것은 조직에서 Azure AD, Azure, Office 365 또는 SaaS 앱에 대해 권한 있는 작업을 수행하고 해당 작업을 모니터링할 수 있는 사용자를 제한하는 중요한 단계입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-161">This is an important step to limiting who in your organization can carry out privileged operations in Azure AD, Azure, Office 365, or SaaS apps, as well as monitor their activities.</span></span>

### <a name="network"></a><span data-ttu-id="947c4-162">네트워크</span><span class="sxs-lookup"><span data-stu-id="947c4-162">Network</span></span>

<span data-ttu-id="947c4-163">전송 중인 데이터를 보호하려면 다른 위치 간에 데이터를 교환할 때 항상 SSL/TLS를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-163">To protect data in transit, always use SSL/TLS when exchanging data across different locations.</span></span> <span data-ttu-id="947c4-164">VPN(가상 사설망) 또는 [ExpressRoute](/azure/expressroute/)를 사용하여 온-프레미스와 클라우드 인프라 간에 전체 통신 채널을 격리해야 하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-164">Sometimes you need to isolate your entire communication channel between your on-premises and cloud infrastructure by using either a virtual private network (VPN) or [ExpressRoute](/azure/expressroute/).</span></span> <span data-ttu-id="947c4-165">자세한 내용은 [클라우드로 온-프레미스 데이터 솔루션 확장](../scenarios/hybrid-on-premises-and-cloud.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-165">For more information, see [Extending on-premises data solutions to the cloud](../scenarios/hybrid-on-premises-and-cloud.md).</span></span>

<span data-ttu-id="947c4-166">NSG([네트워크 보안 그룹](/azure/virtual-network/virtual-networks-nsg))를 사용하여 잠재적인 공격 벡터의 수를 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-166">Use [network security groups](/azure/virtual-network/virtual-networks-nsg) (NSGs) to reduce the number of potential attack vectors.</span></span> <span data-ttu-id="947c4-167">네트워크 보안 그룹에는 원본 또는 대상 IP 주소, 포트 및 프로토콜에 따라 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하는 보안 규칙 목록이 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-167">A network security group contains a list of security rules that allow or deny inbound or outbound network traffic based on source or destination IP address, port, and protocol.</span></span> 

<span data-ttu-id="947c4-168">사용자의 가상 네트워크에서 들어오는 트래픽만 이러한 리소스에 액세스할 수 있도록 [Virtual Network 서비스 끝점](/azure/virtual-network/virtual-network-service-endpoints-overview)을 사용하여 Azure SQL 또는 Azure Storage 리소스의 보안을 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-168">Use [Virtual Network service endpoints](/azure/virtual-network/virtual-network-service-endpoints-overview) to secure Azure SQL or Azure Storage resources, so that only traffic from your virtual network can access these resources.</span></span>

<span data-ttu-id="947c4-169">Azure VNet(Virtual Network) 내의 VM은 [가상 네트워크 피어링](/azure/virtual-network/virtual-network-peering-overview)을 사용하여 다른 VNet과 안전하게 통신할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-169">VMs within an Azure Virtual Network (VNet) can securely communicate with other VNets using [virtual network peering](/azure/virtual-network/virtual-network-peering-overview).</span></span> <span data-ttu-id="947c4-170">피어링된 가상 네트워크 간의 네트워크 트래픽이 개인 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-170">Network traffic between peered virtual networks is private.</span></span> <span data-ttu-id="947c4-171">가상 네트워크 간의 트래픽이 Microsoft 백본 네트워크에서 유지됩니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-171">Traffic between the virtual networks is kept on the Microsoft backbone network.</span></span>

<span data-ttu-id="947c4-172">자세한 내용은 [Azure 네트워크 보안](/azure/security/azure-network-security)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="947c4-172">For more information, see [Azure network security](/azure/security/azure-network-security)</span></span>

### <a name="monitoring"></a><span data-ttu-id="947c4-173">모니터링</span><span class="sxs-lookup"><span data-stu-id="947c4-173">Monitoring</span></span>

<span data-ttu-id="947c4-174">[Azure Security Center](/azure/security-center/security-center-intro)는 Azure 리소스, 네트워크 및 연결된 파트너 솔루션(예: 방화벽 솔루션)의 로그 데이터를 자동으로 수집, 분석 및 통합하여 실제 위협을 검색하고 가양성을 줄입니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-174">[Azure Security Center](/azure/security-center/security-center-intro) automatically collects, analyzes, and integrates log data from your Azure resources, the network, and connected partner solutions, such as firewall solutions, to detect real threats and reduce false positives.</span></span> 

<span data-ttu-id="947c4-175">[Log Analytics](/azure/log-analytics/log-analytics-overview)는 중앙에서 로그에 액세스하도록 하고, 해당 데이터를 분석하고 사용자 지정 경고를 만들 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-175">[Log Analytics](/azure/log-analytics/log-analytics-overview) provides centralized access to your logs and helps you analyze that data and create custom alerts.</span></span>

<span data-ttu-id="947c4-176">[Azure SQL Database 위협 감지](/azure/sql-database/sql-database-threat-detection)는 데이터베이스를 액세스하거나 악용하려는 비정상적이고 잠재적으로 해로운 시도를 나타내는 비정상적인 활동을 감지합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-176">[Azure SQL Database Threat Detection](/azure/sql-database/sql-database-threat-detection) detects anomalous activities indicating unusual and potentially harmful attempts to access or exploit databases.</span></span> <span data-ttu-id="947c4-177">보안 책임자 또는 지정된 다른 관리자는 의심스러운 데이터베이스 활동이 발생하는 즉시 알림을 받을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-177">Security officers or other designated administrators can receive an immediate notification about suspicious database activities as they occur.</span></span> <span data-ttu-id="947c4-178">각 알림에서는 의심스러운 활동에 대한 세부 정보를 제공하고 해당 위협을 자세히 조사하고 완화하는 방법을 권장합니다.</span><span class="sxs-lookup"><span data-stu-id="947c4-178">Each notification provides details of the suspicious activity and recommends how to further investigate and mitigate the threat.</span></span>

