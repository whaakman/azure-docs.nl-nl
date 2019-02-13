---
title: Aanbevelingen voor beveiliging in Azure Security Center beheren | Microsoft Docs
description: Dit document helpt u bij hoe aanbevelingen in Azure Security Center helpen u bij het beveiligen van uw Azure-resources en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: a5ca8efd43244825eef412cf3c05c8df20094112
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109536"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Aanbevelingen voor beveiliging in Azure Security Center beheren
Dit document helpt u bij het gebruik van de aanbevelingen in Azure Security Center kunt u uw Azure-resources beveiligen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze handleiding.
>
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
Nadat er een beveiligingsbeleid is ingesteld, wordt met Security Center de beveiligingsstatus van de Azure-resources geanalyseerd om potentiële beveiligingsproblemen op te sporen. De **aanbevelingen** tegel onder **overzicht** kunt u het totale aantal aanbevelingen geïdentificeerd door Security Center weten.

![Aanbevelingen tegel][1]

De details van elke aanbeveling, selecteer de **aanbevelingen tegel** onder **overzicht**. **Aanbevelingen** wordt geopend.

![Aanbevelingen voor filteren][2]

U kunt filteren, aanbevelingen. Als u wilt de aanbevelingen filteren, selecteert u **Filter** op de **aanbevelingen** blade. De **Filter** blade wordt geopend en selecteert u de ernst en status waarden die u wilt zien.

De aanbevelingen worden weergegeven in tabelindeling, waarbij elke regel één bepaalde aanbeveling vertegenwoordigt. De kolommen van deze tabel zijn:

* **BESCHRIJVING**: Verklaart de aanbeveling en wat er moet gebeuren om deze op te lossen.
* **RESOURCE**: Geeft een lijst van de resources die deze aanbeveling van toepassing is.
* **STATUS**: Beschrijving van de huidige status van de aanbeveling:
  * **Open**: De aanbeveling niet is nog opgelost.
  * **Bezig**: De aanbeveling wordt momenteel toegepast op de resources en er is geen actie te ondernemen.
  * **Opgelost**: De aanbeveling is al voltooid (in dit geval wordt de regel is lichter gekleurd).
* **ERNST**: Hierin wordt de ernst van deze bepaalde aanbeveling beschreven:
  * **Hoge**: Een beveiligingslek in de bestaat een belangrijke resource (zoals een toepassing, een virtuele machine of een netwerkbeveiligingsgroep) en aandacht vereist.
  * **Gemiddeld**: Een beveiligingslek en niet-kritieke of extra stappen zijn vereist om dit te dichten of om een proces te voltooien.
  * **Lage**: Een beveiligingslek dat moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op lage aanbevelingen als u wilt zien.)

Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare aanbevelingen en wat elke doet als u deze toepast.

> [!NOTE]
> Wilt u inzicht in de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>
### <a name="apply-recommendations"></a>Toepassen van aanbevelingen
Bekijk alle aanbevelingen en besluiten die u moet eerst worden toegepast. U wordt aangeraden de ernstclassificatie die de te gebruiken aangezien de belangrijkste parameter om te evalueren welke aanbevelingen voor moet eerst worden toegepast.



## <a name="next-steps"></a>Volgende stappen
In dit document, kunt u kennisgemaakt met aanbevelingen voor beveiliging in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
