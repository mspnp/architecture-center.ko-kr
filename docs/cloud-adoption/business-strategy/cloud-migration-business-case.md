---
title: 'CAF: 클라우드 마이그레이션 비즈니스 사례 빌드'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: 클라우드 마이그레이션의 비즈니스 근거를 마련할 때 고려해야 할 사항입니다.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: a2fdb225fb978eaade39850560ca1adbbb3476e7
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640654"
---
# <a name="build-a-business-justification-for-cloud-migration"></a>클라우드 마이그레이션의 비즈니스 근거 마련

클라우드 마이그레이션은 클라우드 전환 활동으로 초기 투자 수익률(ROI)을 창출할 수 있습니다. 그러나 실체적인 관련 비용과 수익으로 명확한 비즈니스 근거를 개발하는 것은 복잡한 프로세스가 될 수 있습니다. 이 문서는 클라우드 마이그레이션 결과와 부합하는 금융 모델을 만드는 데 필요한 데이터에 대해 생각하도록 도와줍니다. 먼저, 조직이 일반적인 실수를 피할 수 있도록 클라우드 마이그레이션에 관한 몇 가지 통념을 불식시키겠습니다.

## <a name="dispelling-cloud-migration-myths"></a>클라우드 마이그레이션에 대한 통념 불식

**통념: 클라우드가 항상 더 저렴하다.** 클라우드에서 데이터 센터를 운영하는 것이 온-프레미스보다 항상 더 저렴하다는 것은 일반적인 통념입니다. 경우에 따라 맞는 말일 수도 있지만 절대적으로 그렇지는 않습니다. 하지만 클라우드 운영 비용이 실제로 더 많이 드는 경우도 있을 수 있습니다. 이는 잘못된 비용 관리, 부적절한 시스템 아키텍처, 프로세스 복제, 비정상적인 시스템 구성 또는 파견 비용 증가 때문인 경우가 많습니다. 다행히 이러한 문제를 대부분 완화하여 초기 ROI를 창출할 수 있습니다. [비즈니스 근거 개발](#building-the-business-justification)의 지침을 따르면 이러한 불일치를 감지하고 피할 수 있습니다. 여기에 나열된 다른 통념을 불식시키는 것도 도움이 될 수 있습니다.

**통념: 모든 것을 클라우드로 전환해야 한다.** 사실 하이브리드 솔루션을 선택하게 만드는 비즈니스 동기는 여러 가지가 있습니다. 비즈니스 모델을 완성하기 전에 [디지털 자산 문서](../digital-estate/5-rs-of-rationalization.md)에 설명된 대로 1차 정량적 분석을 완료하는 것이 좋습니다. 합리화와 관련된 개별 정량적 동기에 대한 추가 정보는 [합리화의 5가지 R](../digital-estate/5-rs-of-rationalization.md)을 참조하세요. 각 접근 방식에서는 쉽게 얻을 수 있는 인벤토리 데이터와 간략한 정량적 분석을 사용하여 클라우드에 더 많은 비용을 초래할 수 있는 워크로드나 애플리케이션을 파악할 수 있습니다. 이러한 접근 방식을 통해 하이브리드 솔루션을 필요하게 만드는 종속성이나 트래픽 패턴도 파악할 수 있습니다.

**통념: 내 온-프레미스 환경을 미러링하면 클라우드 비용을 절약하는 데 도움이 된다.** 디지털 자산 계획 시 고객이 프로비전된 환경의 50%를 초과하는 미사용 용량을 감지한다는 것은 금시초문입니다. 현재 프로비저닝에 맞게 클라우드에 자산이 프로비전될 경우 비용 절감을 구현하기가 어려워집니다. 프로비저닝 패턴이 아닌 사용량 패턴에 맞게 배포 자산의 크기를 줄이는 것이 좋습니다.

**통념: 서버 비용에 따라 비즈니스 클라우드 마이그레이션 사례가 달라진다.** 사실인 경우도 있습니다. 일부 회사에서는 서버와 관련된 지속적인 자본 비용을 줄이는 것이 중요합니다. 하지만 이는 여러 요인에 따라 달라집니다. 하드웨어 갱신 주기가 5~8년인 회사는 클라우드 마이그레이션 후 빠르게 수익을 볼 가능성이 낮습니다. 갱신 주기가 표준화되거나 엄격히 시행 중인 회사는 손익분기점에 빠르게 도달할 수 있습니다. 두 경우 모두 다른 비용이 마이그레이션을 정당화하는 금융 트리거가 될 수 있습니다. 다음은 서버 전용 또는 VM 전용 비용 관점을 취할 때 일반적으로 간과되는 비용의 몇 가지 예입니다.

- 가상화, 서버 및 미들웨어의 소프트웨어 비용이 아주 많을 수 있습니다. 클라우드 공급자는 이러한 비용 중 일부를 없애줍니다. 가상화 비용을 절감하는 클라우드 공급자의 두 가지 예로는 [Azure 하이브리드 혜택](https://azure.microsoft.com/pricing/hybrid-benefit/#services)과 [예약](https://azure.microsoft.com/reservations/) 프로그램이 있습니다.
- 서비스 중단으로 인한 비즈니스 손실은 하드웨어 또는 소프트웨어 비용을 금세 초과할 수 있습니다. 현재 데이터 센터는 불안정 한 경우, 영업 기회 비용 또는 실제 비즈니스 비용 측면에서 가동 중단의 효과 정량화 비즈니스 작업 인 경우
- 또한 환경 비용 중요할 수 있습니다. 미국 평균 가족의 경우 집이 가장 큰 투자 대상이고, 예산에서 가장 큰 비용을 차지합니다. 데이터 센터는 동일한 경우가 그렇습니다. 부동산, 시설 및 유틸리티 비용은 온-프레미스 비용의 상당한 부분을 차지합니다. 데이터 센터는 사용 중지 되 면 비즈니스에서 이러한 기능의 용도 변경할 수 또는 잠재적으로 비즈니스 해제 못했습니다 비용을 완전히 합니다.

**통념: 운영 비용(OpEx)이 자본 비용(CapEx)보다 낫다.** [회계 결과](business-outcomes/fiscal-outcomes.md) 문서에 설명된 것처럼, OpEx는 긍정적으로 볼 수 있습니다. 하지만 OpEx를 부정적으로 보는 여러 업계가 있습니다. 다음은 회계 및 비즈니스 부서와 더 긴밀히 OpEx에 관한 대화를 나눌 수 있게 해주는 몇 가지 예입니다.

- 비즈니스에서 자본 자산을 비즈니스 평가의 동기로 볼 경우 CapEx 절감은 부정적인 결과가 될 수 있습니다. 범용 표준은 아니지만 이는 소매, 제조, 건설 업계에서 일반적으로 보이는 감정입니다.
- 또한 사모 투자 전문 회사 소속의 비즈니스 팀이나 자본 유입을 원하는 비즈니스 팀에서 OpEx 증가는 부정적인 결과로 보일 수 있습니다.
- 비즈니스가 판매 이윤 개선이나 COGS(판매 제품 원가) 절감에 크게 중점을 두고 있다면 OpEx는 부정적인 결과로 보일 수 있습니다.

OpEx가 항상 나쁜 것만은 아닙니다. 비즈니스 팀에서는 CapEx보다 OpEx에 더 호의를 보일 가능성이 높습니다. 예를 들어 현금 흐름 개선, 자본 투자 축소 또는 자산 보유액 축소를 시도하는 비즈니스 팀에서는 이러한 접근 방식을 잘 받아들일 수 있습니다.

CapEx에서 OpEx로의 전환에 중점을 두는 비즈니스 근거를 제공하기 전에 비즈니스에 어떤 것이 더 좋을지 파악하세요. 회계 및 조달 팀은 금융 목표에 최대한 메시지를 맞추는 데 도움이 되는 경우가 많습니다.

**통념: 클라우드로 전환하는 것은 스위치를 누르는 것과 같습니다.** 마이그레이션은 수동 작업이 집약된 기술 전환입니다. 비즈니스 근거(특히 시간에 민감한 근거)를 개발할 때는 자산을 마이그레이션하는 데 소요되는 시간을 늘릴 수 있는 다음 요소를 고려하세요.

- 대역폭 제한 사항: 현재 데이터 센터와 클라우드 공급자 간의 대역폭 양에 따라 마이그레이션 중 타임라인이 결정됩니다.
- 비즈니스 테스트 타임라인: 비즈니스와 함께 애플리케이션을 테스트하여 준비 여부와 성능을 인증하는 과정은 시간이 오래 걸릴 수 있습니다. 고급 사용자를 조율하고 프로세스를 테스트하는 것은 중요합니다.
- 마이그레이션 실행 타임라인: 마이그레이션을 실행하는 데 필요한 시간과 인력으로 인해 비용이 증가하고 타임라인이 지연될 수 있습니다. 직원이나 계약 파트너 할당 과정은 프로세스를 지연시킬 수 있고, 계획 시 고려되어야 합니다.

기술 및 문화적 장애는 클라우드 도입을 늦출 수 있습니다. 시간이 비즈니스 근거의 중요한 요소일 경우 계획을 적절히 세우는 것이 최고의 완화 전략입니다. 계획 시 타임라인 지연 위험을 완화하는 데 도움이 될 수 있는 두 가지 권장 접근 방식이 있습니다.

- 첫째, 기술 채택 제약 조건을 이해하는 데 시간과 에너지를 투자합니다. 빠르게 전환해야 한다는 압박이 많을 수 있지만 현실적인 실행 타임라인을 고려하는 것이 중요합니다.
- 둘째, 문화권이나 사람과 관련된 제한이 발생할 경우 기술 제약 조건보다 심각한 영향을 줄 수 있습니다. 클라우드를 채택하면 변경 사항이 구현되어 원하는 전환이 수행됩니다. 그러나 사람들은 때로 변화를 두려워하고, 계획에 맞추기 위해 추가적인 지원을 필요로 할 수 있습니다. 팀에서 변화를 반대하는 핵심 인물을 파악하고 초기에 소통하세요.

만반의 준비를 하고 타임라인 지연 위험을 최대한 완화하려면 비즈니스 가치와 비즈니스 결과를 확실히 맞춰서 경영 이해 관계자를 준비시키세요. 이러한 이해 관계자가 이러한 전환 과정에서 발생하는 변화를 이해하도록 도와주세요. 처음부터 명확한 태도를 보이고 현실적인 기대치를 설정하세요. 사람이나 기술로 인해 프로세스가 느려질 경우 임원의 지지를 받기가 더 쉬워질 것입니다.

## <a name="building-the-business-justification"></a>비즈니스 근거 개발

다음 프로세스는 클라우드 마이그레이션의 비즈니스 근거를 개발하는 방법을 정의합니다. 이 콘텐츠를 읽을 때 계산이나 금융 용어와 관련하여 추가적인 설명이 필요할 경우 추가 설명을 위해 [금융 모델](financial-models.md)에서 문서를 참조하세요.

비즈니스 근거의 수식은 대략적으로 보면 단순합니다. 하지만 수식을 채우는 데 필요한 미묘한 데이터 요소를 맞추기가 어려울 수 있습니다. 비즈니스 근거는 결국 제안된 기술적 변화와 관련된 투자 수익률(ROI)에 중점을 둡니다. ROI의 일반적인 수식은 다음과 같습니다.

ROI = (투자 수익 &minus; 초기 투자) / 초기 투자

이 수식을 풀면 이 등식의 오른쪽에 있는 각 입력 변수를 도출하는 마이그레이션 측면의 수식이 생성됩니다. 이 문서의 나머지 섹션에서는 몇 가지 고려 사항을 제공합니다.

![ROI = (투자 수익 – 투자 비용) / 투자 비용](../_images/formula-roi.png)

## <a name="migration-specific-initial-investment"></a>마이그레이션 관련 초기 투자

- Azure와 같은 클라우드 공급자는 클라우드 투자액을 예측하는 계산기를 제공합니다. 이러한 계산기의 예로 [Azure 가격 계산기](https://azure.microsoft.com/pricing)가 있습니다.
- 일부 클라우드 공급자는 비용 델타 계산기도 지원합니다. 비용 델타 계산기의 예로는 [Azure TCO(총 소요 비용) 계산기](https://azure.com/tco)가 있습니다.
- 보다 구체적인 비용 구조를 보려면 [디지털 자산 계획](../digital-estate/overview.md) 활동을 고려하세요.
- 마이그레이션 비용을 예측합니다.
- 모든 예상 교육 기회의 비용을 예측합니다. [Microsoft Learn](/learn)이 이러한 비용을 완화하는 데 도움이 될 수 있습니다.
- 일부 회사에서는 기존 직원이 투자한 시간이 초기 비용에 포함되어야 할 수 있습니다. 자세한 지침은 경리과에 문의하세요.
- 추가 비용이나 부담 비용을 경리과와 논의하여 확인하세요.

## <a name="migration-specific-revenue-deltas"></a>마이그레이션 관련 수익 델타

이러한 측면은 마이그레이션 비즈니스 근거를 만들 때 종종 간과됩니다. 일부 영역에서는 클라우드로 비용을 절감할 수 있습니다. 그러나 모든 전환의 궁극적인 목적은 시간이 지남에 따라 더 나은 결과를 창출하는 것입니다. 장기적인 매출 개선을 이해하려면 일의 중요도를 고려하세요. 현재 사용할 수 없는이 마이그레이션 후 새 기술을 비즈니스 사용할 수 있습니다? 기존 기술에서 종속성으로 인해 차단되는 프로젝트 또는 비즈니스 목표는 무엇입니까? 어떤 프로그램이 보류 중이고, 자본 비용이 많이 드는 보류 중인 기술의 비용은 얼마입니까?

클라우드가 제공하는 기회를 고려한 후 비즈니스와 협업하여 이러한 기회로 인해 얻을 수 있는 수익 증대량을 계산하세요.

## <a name="migration-specific-cost-deltas"></a>마이그레이션 관련 비용 델타

제안된 마이그레이션으로 인한 비용 변화를 계산하세요. 여러 가지 비용 델타에 대한 자세한 내용은 [금융 모델](financial-models.md)을 참조하세요. 클라우드 공급자는 비용 델타 계산을 위한 도구를 제공하는 경우가 많습니다. 비용 델타 계산기의 예로는 [Azure TCO(총 소요 비용) 계산기](https://azure.com/tco)가 있습니다.

클라우드 마이그레이션 줄일 수 있습니다 하는 비용의 다른 예제:

- 데이터 센터 종료 또는 감소 (환경 비용)
- 전원 사용 (환경 비용) 절감
- 랙 종료 (실제 자산 복구)
- 하드웨어 새로 고침 방지 (비용 방지)
- 갱신 방지 소프트웨어 (운영 비용 절감 또는 비용 방지)
- 공급 업체 통합 (운영 비용 절감 및 소프트 잠재적 절감)

## <a name="when-roi-results-are-surprising"></a>ROI 결과가 예상과 다른 경우

클라우드 마이그레이션에 대 한 ROI 기대 수준을 일치 하지 않으면,이 문서의 시작 부분에 나열 된 일반적인 신화 다시 방문 하는 데 유용 수도 있습니다.

그러나 항상 비용 절감 효과를 볼 수 있는 것은 아님을 이해하는 것이 중요합니다. 클라우드에서 작동 시 온-프레미스에서 작동할 때보다 더 많은 비용이 드는 애플리케이션이 있습니다. 이러한 애플리케이션은 분석 결과를 상당히 왜곡시킬 수 있습니다.

ROI가 20% 미만일 경우 [합리화](../digital-estate/rationalize.md)에 특히 주의하여 [디지털 자산 계획](../digital-estate/overview.md) 활동을 고려하세요. 정량적 분석 중에는 각 애플리케이션에 대한 검토를 수행하여 결과를 왜곡시키는 워크로드를 찾으세요. 해당 워크로드는 계획에서 제거하는 것이 좋습니다. 사용 현황 데이터를 볼 수 있는 경우 사용량에 맞게 VM 크기를 줄이는 것이 좋습니다.

그래도 ROI를 맞추지 못할 경우 Microsoft 영업 담당자에게 도움을 구하거나 [숙련된 파트너에게 문의](https://azure.microsoft.com/migration/support)하세요.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [클라우드 전환을 위한 금융 모델 만들기](./financial-models.md)