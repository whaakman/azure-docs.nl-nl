---
title: Aanbevelingen voor beveiliging in Azure Security Center | Microsoft Docs
description: Dit document helpt u bij hoe aanbevelingen in Azure Security Center helpen u bij het beveiligen van uw Azure-resources en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2019
ms.author: v-mohabe
ms.openlocfilehash: fe1d4bf27f3c4bb1f70c1c1fa9767c27f8767998
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064244"
---
# <a name="security-recommendations-in-azure-security-center"></a>Aanbevelingen voor beveiliging in Azure Security Center 
In dit onderwerp wordt uitgelegd hoe u bekijken en te begrijpen van de aanbevelingen in Azure Security Center voor het beschermen van uw Azure-resources.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze handleiding.
>

## <a name="what-are-security-recommendations"></a>Wat zijn aanbevelingen voor beveiliging?
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

## <a name="implementing-security-recommendations"></a>Beveiligingsaanbevelingen implementeren
### <a name="set-recommendations"></a>Set-aanbevelingen
In [beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md), leert u het:

* Beveiligingsbeleid configureren.
* Schakel het verzamelen van gegevens.
* Kies welke aanbevelingen om te zien als onderdeel van uw beveiligingsbeleid.

Aanbevelingen van huidige beleidscentrum om de systeemupdates, basislijnregels, anti-malware-programma's, [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) op subnetten en netwerkinterfaces, controleren voor SQL database, transparent data encryption voor SQL database, Apps en web application firewalls.  [Beveiligingsbeleid instellen](tutorial-security-policy.md) bevat een beschrijving van elke optie aanbeveling.

### <a name="monitor-recommendations"></a>Monitor voor aanbevelingen
Nadat er een beveiligingsbeleid is ingesteld, wordt met Security Center de beveiligingsstatus van de Azure-resources geanalyseerd om potentiële beveiligingsproblemen op te sporen. De **aanbevelingen** tegel onder **overzicht** toont het totale aantal aanbevelingen geïdentificeerd door Security Center.

![Security center-overzicht](./media/security-center-recommendations/asc-overview.png)

1. Selecteer de **aanbevelingen tegel** onder **overzicht**. De **aanbevelingen** lijst wordt geopend.
    
      ![Aanbevelingen bekijken](./media/security-center-recommendations/view-recommendations.png)

    U kunt filteren, aanbevelingen. Als u wilt de aanbevelingen filteren, selecteert u **Filter** op de **aanbevelingen** blade. De **Filter** blade wordt geopend en selecteert u de ernst en status waarden die u wilt zien.

   * **AANBEVELINGEN**: De aanbeveling.
   * **BEVEILIGDE SCORE IMPACT**: Een score die worden gegenereerd door Security Center met behulp van de aanbevelingen voor beveiliging, en het toepassen van geavanceerde algoritmen om te bepalen hoe essentieel is voor elke aanbeveling. Zie voor meer informatie, [scoreberekening Secure](security-center-secure-score.md#secure-score-calculation).
   * **RESOURCE**: Geeft een lijst van de resources die deze aanbeveling van toepassing is.
   * **STATUS STAVEN**:  Hierin wordt de ernst van deze bepaalde aanbeveling beschreven:
       * **Hoog (rood)** : Een beveiligingslek in de bestaat een belangrijke resource (zoals een toepassing, een virtuele machine of een netwerkbeveiligingsgroep) en aandacht vereist.
       * **Gemiddeld (oranje)** : Een beveiligingslek en niet-kritieke of extra stappen zijn vereist om dit te dichten of om een proces te voltooien.
       * **Laag (blauw)** : Een beveiligingslek dat moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op lage aanbevelingen als u wilt zien.) 
       * **In orde (groen)** :
       * **Niet beschikbaar (grijs)** :

1. Als u wilt weergeven van details van elke aanbeveling, klikt u op de aanbeveling.

    ![Details van de aanbeveling](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Zie [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
  
 ### <a name="apply-recommendations"></a>Toepassen van aanbevelingen
> Bekijk alle aanbevelingen en bepalen welke sjabloon eerst toepassen. Het is raadzaam dat u de beveiligde score gevolgen te evalueren welke aanbevelingen voor moeten eerst worden toegepast.

1. Klik op de aanbeveling in de lijst.
1. Volg de instructies in de *herstelstappen* sectie.

## <a name="next-steps"></a>Volgende stappen
In dit document, kunt u kennisgemaakt met aanbevelingen voor beveiliging in Security Center. Zie de volgende onderwerpen voor meer informatie over Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
