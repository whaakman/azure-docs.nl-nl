---
title: Azure migreren assessment instellingen aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een beoordeling voor VMware-machines migreren naar Azure met de Azure-migratie-Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: efb4ad59d25a0c1209e4f0f6cd406c2f0d48159c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure migreren](migrate-overview.md) beoordelingen maakt met standaardeigenschappen. Na het maken van een beoordeling, kunt u de standaard-eigenschappen van de instructies in dit artikel.


## <a name="edit-assessment-properties"></a>Evaluatie-eigenschappen bewerken

1. In de **beoordelingen** pagina van de migratieproject, selecteer de beoordeling en klik op **eigenschappen bewerken**.
2. Wijzig de eigenschappen in overeenstemming met de volgende tabel:

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/> Azure migreren ondersteunt momenteel 30 regio's, inclusief Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, Canada centraal, Canada Oost, India centraal, VS-midden, China Oost, China Noord, Oost-Azië, VS-Oost, Duitsland centraal, Duitsland noordoosten, VS-Oost 2, Japan Oost, Japan-West, Zuid-Korea-midden, Korea, Noord-centraal VS, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, India Zuid, VK Zuid, VK West, West-Centraal VS, West-Europa, India West, VS-West en West US2. |  VS West 2 is de standaardlocatie.
    **Opslagredundantie** | Het type van de redundantie van gegevensopslag die door de Azure VM's na de migratie wordt gebruikt. | [Lokaal redundante opslag (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) is de standaardwaarde. Azure migreren ondersteunt alleen het beheerd beoordelingen op basis van schijven en schijven beheerde ondersteunen alleen LRS, daarom de eigenschap momenteel heeft alleen de optie LRS. 
    **Sizing criterium** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt beide wilt doen *op basis van prestaties* formaat of het formaat van de virtuele machines *als lokale*, zonder rekening te houden de prestatiegeschiedenis van de. | Op basis van prestaties sizing is de standaardoptie.
    **Prestatiegeschiedenis** | De duur in overweging moet nemen voor het evalueren van de prestaties van de virtuele machines. Deze eigenschap is alleen van toepassing wanneer sizing criterium *formaat op basis van prestaties*. | Standaard is één dag.
    **Gebruik percentiel** | De percentielwaarde van de voorbeeld-prestaties ingesteld voor het juiste formaat worden overwogen. Deze eigenschap is alleen van toepassing wanneer sizing criterium *formaat op basis van prestaties*.  | De standaardwaarde is 95e percentiel.
    **Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u bijvoorbeeld van plan bent om een productieomgeving te migreren, doet u er waarschijnlijk goed aan te kiezen voor de categorie Standard. Hiermee beschikt u over VM's met lage latentie, maar zijn de kosten mogelijk wel hoger. Hebt u daarentegen een ontwikkel-/testomgeving, dan is de categorie Basic wellicht de juiste keuze. Hierbij krijgt u VM's met een hogere latentie, maar lagere kosten. | Standaard wordt de categorie [Standard](../virtual-machines/windows/sizes-general.md) gebruikt.
    **Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. | Standaard wordt 1.3 x.
    **Aanbieding** | [Aanbieding voor Azure](https://azure.microsoft.com/support/legal/offer-details/) die u hebt om te worden ingeschreven. | [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) is de standaardinstelling.
    **Valuta** | Factureringsvaluta. | De standaardwaarde is VS bedragen.
    **Korting (%)** | Abonnement-specifieke korting verschijnt boven op de Azure-aanbieding. | De standaardinstelling is 0%.
    **Azure Hybrid Benefit** | Opgeven dat als u software assurance en in aanmerking komen voor [Azure hybride voordeel](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als op Ja instelt, Windows Azure-prijzen worden beschouwd voor het Windows-VM's. | Standaard is Ja.

3. Klik op **opslaan** bijwerken van de evaluatie.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
