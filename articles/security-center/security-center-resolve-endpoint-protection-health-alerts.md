---
title: Endpoint protection-status meldingen in Azure Security Center oplossen | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** Endpoint Protection oplossen health waarschuwingen **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Endpoint protection-status meldingen in Azure Security Center oplossen
Azure Security Center wordt aanbevolen dat u gedetecteerde endpoint protection-statuswaarschuwingen oplossen.  Security Center kunt u zien welke virtuele machines (VM's) hebt endpoint protection storingen en hoeveel.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie. Dit is geen stapsgewijze handleiding.
> 
> 

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **blade aanbevelingen**, selecteer **waarschuwingen voor Endpoint Protection oplossen health**.
   ![Statusmeldingen endpoint protection oplossen][1]
2. Hiermee opent u de blade **Endpoint Protection fout** die een lijst met virtuele machines met fouten en het aantal fouten voor elke virtuele machine. Selecteer een virtuele machine in de lijst.
   ![Endpoint protection-fout][2]
3. Een **fouten lijst** er wordt een blade geopend voor de geselecteerde virtuele machine met een lijst van fouten. Selecteer een fout in de lijst voor meer informatie. Dit wordt een blade geopend met informatie over de geselecteerde fout.
   ![Lijst met fouten][3]
   ![foutgebeurtenis][4]

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): leer hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
