---
title: Instellingen voor evaluatie van Azure Migrate aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een evaluatie voor VMware-VM's migreren naar Azure met behulp van de Azure-migratie-Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/10/2019
ms.author: raynew
ms.openlocfilehash: 8419d7e7a91e4cbfd0eebfe00d35bf498cf5998c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62129820"
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure Migrate](migrate-overview.md) evaluaties met standaard-eigenschappen maakt. Na het maken van een beoordeling, kunt u de standaard-eigenschappen van de instructies in dit artikel.


## <a name="edit-assessment-properties"></a>Evaluatie-eigenschappen bewerken

1. Selecteer de evaluatie op de pagina **Evaluaties** van het migratieproject en klik op **Eigenschappen bewerken**.
2. De evaluatie-eigenschappen op basis van de volgende informatie aanpassen:

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/> Azure Migrate ondersteunt momenteel 30 regio's, waaronder AU - oost, AU - zuidoost, BR - zuid, Canada - centraal, Canada - oost, India - centraal, US - centraal, China - oost, China - noord, Azië - oost, US - oost, Duitsland - centraal, Duitsland - noordoost, US - oost 2, Japan - oost, Japan - west, Korea - centraal, Korea - zuid, US - noord-centraal, Europa - noord, US - zuid-centraal, Azië - zuidoost, India - zuid, UK - zuid, UK - west, US Gov - AZ, US Gov - TX, US Gov - Virginia, US - west-centraal, Europa - west, India - west, US - west en US - west 2. |  VS-West 2 is de standaardlocatie.
    **Opslagtype** | U kunt deze eigenschap gebruiken om op te geven van het type van de schijven die u verplaatsen wilt naar in Azure. Voor als-on premises formaat wijzigen, kunt u het doeltype van de schijf als Premium-beheerde schijven of Standard-beheerde schijven. Voor prestatie gebaseerde schaling, kunt u het doeltype van de schijf als Premium-beheerde schijven met automatische of Standard-beheerde schijven opgeven. Wanneer u het opslagtype als automatische opgeeft, wordt de schijf-aanbeveling gedaan op basis van de prestatiegegevens van de schijven (IOPS en doorvoer). Bijvoorbeeld, als u wilt bereiken een [één exemplaar VM SLA van 99,9%](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), kunt u het opslagtype opgeven als Premium-beheerde schijven. Dit zorgt ervoor dat alle schijven in de evaluatie worden aanbevolen als Premium-beheerde schijven. Houd er rekening mee dat Azure Migrate voor migratiebeoordeling alleen ondersteuning voor beheerde schijven biedt. | De standaardwaarde is een Premium-beheerde schijven (met criterium voor het instellen als *zoals on-premises '* ).
    **Gereserveerde instanties** |  U kunt ook opgeven of u hebt [gereserveerde instanties](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure hebt. De kosten worden dan door Azure Migrate dienovereenkomstig vastgesteld. Gereserveerde instanties zijn momenteel alleen ondersteund voor betalen per gebruik-aanbieding in Azure Migrate. | De standaardwaarde voor deze eigenschap is 3 jaar gereserveerde instanties.
    **Criterium voor het aanpassen van de grootte** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt de grootte bepalen *op basis van prestaties* of de grootte van de virtuele machines *als on-premises* bepalen, zonder rekening te houden de prestatiegeschiedenis. | Groottebepaling op basis van prestaties is de standaardoptie.
    **Prestatiegeschiedenis** | De duur die in overweging moet worden genomen voor de evaluatie van de prestaties van de virtuele machines. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is. | De standaardoptie is één dag.
    **Percentiel gebruik** | De percentielwaarde van de prestatievoorbeeldset waarmee rekening moet worden gehouden voor een juiste groottebepaling. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is.  | De standaardwaarde is 95e percentiel.
    **VM-reeks** | U kunt de VM-reeks opgeven die u wilt overwegen voor de juiste grootte. Bijvoorbeeld, als u een productie-omgeving die u niet van plan bent om te migreren naar de A-serie VM's in Azure hebt, u A-serie kunt uitsluiten van de lijst of reeksen en het juiste formaat is uitgevoerd op de geselecteerde reeks. | Alle VM-reeksen zijn standaard ingeschakeld.
    **Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. | De standaardinstelling is 1,3x.
    **Aanbieding** | [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarvoor u zich hebt ingeschreven. | De standaardinstelling is [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/).
    **Valuta** | Factureringsvaluta. | De standaardinstelling is Amerikaanse dollars.
    **Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt. | De standaardinstelling is 0%.
    **VM tijd actief** | Als uw virtuele machines niet meer 24 x 7 worden uitgevoerd op Azure, kunt u de duur (aantal dagen per maand) en het aantal uur per dag voor dat deze zou worden uitgevoerd en de kostenramingen dienovereenkomstig wordt uitgevoerd. | De standaardwaarde is 31 dagen per maand en 24 uur per dag.
    **Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. | Standaard is Ja.

3. Klik op **opslaan** aan de evaluatie van updates.

## <a name="faqs-on-assessment-properties"></a>Veelgestelde vragen over de evaluatie-eigenschappen

### <a name="what-is-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Wat is het verschil tussen de grootte van de as-on-premises en de prestaties gebaseerde groottebepaling?

Wanneer u het criterium voor het instellen als-on-premises formaat, Azure Migrate houdt geen rekening met de prestatiegegevens van de virtuele machines en het formaat van de virtuele machines op basis van de on-premises configuratie. Als het criterium voor het instellen op basis van prestaties is, wordt de grootte gedaan op basis van gegevens over het gebruik. Neem bijvoorbeeld een on-premises VM met 4 kernen en 8 GB geheugen, met 50% CPU-gebruik en 50% geheugengebruik. Als het groottecriterium is ingesteld om de grootte van de on-premises VM te emuleren, wordt een Azure VM-SKU met 4 kernen en 8 GB geheugen aanbevolen. Als het criterium echter op basis van prestaties wordt ingesteld, wordt een VM-SKU met 2 kernen en 4 GB geheugen aanbevolen, omdat bij het aanbevelen van de grootte gekeken wordt naar het gebruikspercentage.

Op dezelfde manier voor schijven, de grootte van de schijf is afhankelijk van twee evaluatie-eigenschappen - formaat type criterium en opslag. Als het groottecriterium op basis van prestaties is en het opslagtype automatisch is, wordt gekeken naar de doorvoerwaarden van de schijf om het type doelschijf te bepalen (Standard of Premium). Als groottecriterium op basis van prestaties is en het opslagtype premium is, wordt een premiumschijf aanbevolen en wordt de SKU voor premiumschijven geselecteerd in Azure, op basis van de grootte van de on-premises schijf. Dezelfde logica wordt gebruikt wanneer het groottecriterium is ingesteld op basis van de on-premises grootte en het opslagtype standard of premium is.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Wat zijn de gevolgen heeft de gebruik van de geschiedenis en percentiel van de prestaties van de aanbevelingen voor de grootte?

Deze eigenschappen zijn alleen van toepassing wanneer de grootte wordt bepaald op basis van prestaties. Azure Migrate verzamelt de prestatiegeschiedenis van on-premises machines en gebruikt deze om de grootte en het schijftype van virtuele Azure-machines aan te bevelen.

- De collector profileert continu de on-premises omgeving door iedere 20 seconden realtime gebruiksgegevens voor elke virtuele machine te verzamelen.
- De collector verzamelt de gegevens die iedere 20 seconden worden opgehaald en maakt een enkel gegevenspunt voor elke 15 minuten. Om dat gegevenspunt te bepalen, selecteert de collector de hoogste waarde uit alle opgehaalde waarden voor die 15 minuten en verzendt de collector deze naar Azure.
- Wanneer u een evaluatie in Azure maakt, op basis van de prestatieduur en de percentielwaarde van de prestatiegeschiedenis, berekent Azure Migrate een waarde voor het daadwerkelijke gebruik. Deze waarde wordt vervolgens gebruikt om de grootte te bepalen.

Als u de prestatieduur bijvoorbeeld instelt op 1 dag en de percentielwaarde op 95e percentiel, gebruikt Azure Migrate de meetwaarden voor de blokken van 15 minuten van de afgelopen dag. Deze worden vervolgens in oplopende volgorde gesorteerd en de 95e-percentielwaarde wordt dan gekozen als het effectieve gebruik. De 95e-percentielwaarde zorgt ervoor dat uitbijters worden genegeerd, die mogelijk wel zouden worden meegenomen als u het 99e percentiel kiest. Als u het piekgebruik voor de periode wilt ophalen en uitbijters niet wilt uitsluiten, selecteert u het 99e percentiel.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
