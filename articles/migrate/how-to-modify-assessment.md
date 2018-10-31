---
title: Instellingen voor evaluatie van Azure Migrate aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een evaluatie voor VMware-VM's migreren naar Azure met behulp van de Azure-migratie-Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/30/2018
ms.author: raynew
ms.openlocfilehash: d0cfab51b686b5b6eb9617d4424ac3f834de8d6f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241069"
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure Migrate](migrate-overview.md) evaluaties met standaard-eigenschappen maakt. Na het maken van een beoordeling, kunt u de standaard-eigenschappen van de instructies in dit artikel.


## <a name="edit-assessment-properties"></a>Evaluatie-eigenschappen bewerken

1. Selecteer de evaluatie op de pagina **Evaluaties** van het migratieproject en klik op **Eigenschappen bewerken**.
2. De evaluatie-eigenschappen op basis van de gegevens hieronder aanpassen:

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/> Azure Migrate ondersteunt momenteel 30 regio's, waaronder AU - oost, AU - zuidoost, BR - zuid, Canada - centraal, Canada - oost, India - centraal, US - centraal, China - oost, China - noord, Azië - oost, US - oost, Duitsland - centraal, Duitsland - noordoost, US - oost 2, Japan - oost, Japan - west, Korea - centraal, Korea - zuid, US - noord-centraal, Europa - noord, US - zuid-centraal, Azië - zuidoost, India - zuid, UK - zuid, UK - west, US Gov - AZ, US Gov - TX, US Gov - Virginia, US - west-centraal, Europa - west, India - west, US - west en US - west 2. |  VS-West 2 is de standaardlocatie.
    **Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u van plan bent voor het migreren van een productie-omgeving, zoals u kunt u de Standard-laag. Hebt u daarentegen een ontwikkel-/testomgeving, dan is de categorie Basic wellicht de juiste keuze. Hierbij krijgt u VM's met een hogere latentie, maar lagere kosten. | Standaard wordt de categorie [Standard](../virtual-machines/windows/sizes-general.md) gebruikt.
    **Opslagtype** | U kunt deze eigenschap gebruiken om op te geven van het type van de schijven die u wilt toewijzen in Azure. Voor als-on premises formaat wijzigen, kunt u het doeltype van de schijf als Premium-beheerde schijven of Standard-beheerde schijven. Voor prestatie gebaseerde schaling, kunt u het doeltype van de schijf als Premium-beheerde schijven met automatische of Standard-beheerde schijven opgeven. Wanneer u het opslagtype als automatische opgeeft, wordt de schijf-aanbeveling gedaan op basis van de prestatiegegevens van de schijven (IOPS en doorvoer). Bijvoorbeeld, als u wilt bereiken een [één exemplaar VM SLA van 99,9%](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), kunt u het opslagtype opgeven als Premium beheerde schijven waarvoor een zorgt ervoor dat alle schijven in de evaluatie wordt aangeraden als Premium beheerde schijven. Houd er rekening mee dat Azure Migrate voor migratiebeoordeling alleen ondersteuning voor beheerde schijven biedt. | De standaardwaarde is een Premium-beheerde schijven (met criterium voor het instellen als *zoals on-premises '*).
    **Gereserveerde instanties** |  U kunt ook opgeven of u hebt [gereserveerde instanties](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure hebt. De kosten worden dan door Azure Migrate dienovereenkomstig vastgesteld. Gereserveerde instanties gelden niet voor soevereine regio's (Azure Government, Duitsland en China) en ze zijn alleen van toepassing op betalen naar gebruik-aanbieding in Azure Migrate. | De standaardwaarde voor deze eigenschap is 3 jaar gereserveerde instanties.
    **Criterium voor het aanpassen van de grootte** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt de grootte bepalen *op basis van prestaties* of de grootte van de virtuele machines *als on-premises* bepalen, zonder rekening te houden de prestatiegeschiedenis. | Groottebepaling op basis van prestaties is de standaardoptie.
    **Prestatiegeschiedenis** | De duur die in overweging moet worden genomen voor de evaluatie van de prestaties van de virtuele machines. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is. | De standaardoptie is één dag.
    **Percentiel gebruik** | De percentielwaarde van de prestatievoorbeeldset waarmee rekening moet worden gehouden voor een juiste groottebepaling. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is.  | De standaardwaarde is 95e percentiel.
    **VM-reeks** | U kunt de VM-reeks opgeven die u wilt overwegen voor de juiste grootte. Als u bijvoorbeeld een productieomgeving hebt die u niet gaat migreren naar de A-serie van virtuele machines in Azure, kunt u de A-serie uitsluiten van de lijst of reeks om alleen de geselecteerde reeks te analyseren voor het selecteren van de juiste grootte. | Alle VM-reeksen zijn standaard ingeschakeld.
    **Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. | De standaardinstelling is 1,3x.
    **Aanbieding** | [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarvoor u zich hebt ingeschreven. | De standaardinstelling is [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/).
    **Valuta** | Factureringsvaluta. | De standaardinstelling is Amerikaanse dollars.
    **Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt. | De standaardinstelling is 0%.
    **VM tijd actief** | Als uw virtuele machines niet meer 24 x 7 worden uitgevoerd op Azure, kunt u de duur (aantal dagen per maand) en het aantal uur per dag voor dat deze zou worden uitgevoerd en de kostenramingen dienovereenkomstig wordt uitgevoerd. | De standaardwaarde is 31 dagen per maand en 24 uur per dag.
    **Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. | Standaard is Ja.

3. Klik op **opslaan** aan de evaluatie van updates.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
