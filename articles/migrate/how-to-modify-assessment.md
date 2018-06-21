---
title: Azure migreren assessment instellingen aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een beoordeling voor VMware-machines migreren naar Azure met de Azure-migratie-Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 9ddd6c32388b2e05fd97138414958b67c009f9ee
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284910"
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure migreren](migrate-overview.md) beoordelingen maakt met standaardeigenschappen. Na het maken van een beoordeling, kunt u de standaard-eigenschappen van de instructies in dit artikel.


## <a name="edit-assessment-properties"></a>Evaluatie-eigenschappen bewerken

1. Selecteer de evaluatie op de pagina **Evaluaties** van het migratieproject en klik op **Eigenschappen bewerken**.
2. Wijzig de eigenschappen in overeenstemming met de volgende tabel:

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/> Azure Migrate ondersteunt momenteel 30 regio's, waaronder Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, Centraal-India, VS - midden, China - oost, China - noord, Oost-Azië, VS - oost, Duitsland - centraal, Duitsland - noordoost, VS - oost 2, Japan - oost, Japan - west, Korea - midden, Korea - zuid, Noord-centraal VS, Noord-Europa, Zuid-centraal VS , Zuidoost-Azië, Zuid-India, UK - zuid, UK - west, US Gov - Arizona, US Gov - Texas, US Gov - Virginia, West-centraal VS, West-Europa, India - west, VS - west en VS - west 2. |  VS West 2 is de standaardlocatie.
    **Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u bijvoorbeeld van plan bent om een productieomgeving te migreren, doet u er waarschijnlijk goed aan te kiezen voor de categorie Standard. Hiermee beschikt u over VM's met lage latentie, maar zijn de kosten mogelijk wel hoger. Hebt u daarentegen een ontwikkel-/testomgeving, dan is de categorie Basic wellicht de juiste keuze. Hierbij krijgt u VM's met een hogere latentie, maar lagere kosten. | Standaard wordt de categorie [Standard](../virtual-machines/windows/sizes-general.md) gebruikt.
    **Opslagtype** | U kunt opgeven welke type schijf u wilt toewijzen in Azure. Deze eigenschap is van toepassing wanneer het criterium voor het aanpassen van de grootte 'Zoals on-premises' is. U kunt het doeltype van de schijf opgeven als beheerde Premium-schijven of beheerde Standard schijven. Voor het aanpassen van de grootte op basis van prestaties geldt dat de aanbeveling automatisch wordt gedaan op basis van de prestatiegegevens van de virtuele machines. Houd er rekening mee dat Azure Migrate voor migratiebeoordeling alleen ondersteuning voor beheerde schijven biedt. | De standaardwaarde is beheerd Premium-schijven (met Sizing criterium als *als lokale sizing*).
    **Gereserveerde exemplaren** |  U kunt ook opgeven of u hebt [exemplaren gereserveerd](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure en Azure migreren wordt schat de kosten dienovereenkomstig. Gereserveerde instanties zijn niet van toepassing op soevereine regio's (Azure Government, Duitsland en China) en ze zijn alleen van toepassing op de aanbieding betalen naar gebruik in Azure migreren. | De standaardwaarde voor deze eigenschap is gereserveerd voor 3 jaar exemplaren.
    **Criterium voor het aanpassen van de grootte** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt de grootte bepalen *op basis van prestaties* of de grootte van de virtuele machines *als on-premises* bepalen, zonder rekening te houden de prestatiegeschiedenis. | Groottebepaling op basis van prestaties is de standaardoptie.
    **Prestatiegeschiedenis** | De duur die in overweging moet worden genomen voor de evaluatie van de prestaties van de virtuele machines. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is. | De standaardoptie is één dag.
    **Percentiel gebruik** | De percentielwaarde van de prestatievoorbeeldset waarmee rekening moet worden gehouden voor een juiste groottebepaling. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is.  | De standaardwaarde is 95e percentiel.
    **VM-reeks** | U kunt de VM-reeks opgeven die u wilt overwegen voor de juiste grootte. Als u bijvoorbeeld een productieomgeving hebt die u niet gaat migreren naar de A-serie van virtuele machines in Azure, kunt u de A-serie uitsluiten van de lijst of reeks om alleen de geselecteerde reeks te analyseren voor het selecteren van de juiste grootte. | Standaard worden alle VM-reeks geselecteerd.
    **Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. | De standaardinstelling is 1,3x.
    **Aanbieding** | [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarvoor u zich hebt ingeschreven. | De standaardinstelling is [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/).
    **Valuta** | Factureringsvaluta. | De standaardinstelling is Amerikaanse dollars.
    **Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt. | De standaardinstelling is 0%.
    **VM bedrijfstijd** | Als uw VM's niet 24 x 7 worden uitgevoerd in Azure gaat, kunt u de duur (aantal dagen per maand) en het aantal uren per dag voor dat ze zouden worden uitgevoerd en de kosten schattingen dienovereenkomstig wordt uitgevoerd. | De standaardwaarde is 31 dagen per maand en 24 uur per dag.
    **Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. | Standaard is Ja.

3. Klik op **opslaan** bijwerken van de evaluatie.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
