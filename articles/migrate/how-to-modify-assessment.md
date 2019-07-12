---
title: Beoordelingen voor Azure Migrate-Server-evaluatie aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe evaluaties die zijn gemaakt met Azure Migrate-Server-evaluatie aanpassen
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: 8b200ce3d6e73a575b1b89d82a9323d58f435a48
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807915"
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

In dit artikel wordt beschreven hoe u evaluaties die zijn gemaakt door Azure Migrate-Server-evaluatie aanpassen.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen, workloads en persoonlijke/openbare cloud, virtuele machines naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

Het hulpprogramma Azure Migrate-Server-evaluatie kunt u een evaluatie voor on-premises VMware-VM's en Hyper-V-machines maken in voorbereiding voor migratie naar Azure. 

## <a name="about-assessments"></a>Over evaluaties

Er zijn twee soorten evaluaties die u kunt uitvoeren met behulp van Azure Migrate-Server-evaluatie.

**Evaluatie** | **Details** | **Gegevens**
--- | --- | ---
**Performance-based** | Evaluaties op basis van verzamelde prestatiegegevens | **VM-grootte aanbevolen**: Op basis van CPU en geheugen gegevens over het gebruik.<br/><br/> **Schijftype (standard of premium beheerde schijven) aanbevolen**: Op basis van de IOPS en doorvoer van de on-premises-schijven.
**As on-premises** | Evaluaties op basis van on-premises formaat wijzigen. | **VM-grootte aanbevolen**: Op basis van de on-premises VM-grootte<br/><br> **Aanbevolen schijftype**: Op basis van de instelling van het type opslag die u voor de evaluatie selecteert.


## <a name="how-is-an-assessment-done"></a>Hoe wordt een evaluatie uitgevoerd?

Een Azure Migrate-evaluatie bestaat uit drie fasen. Evaluatie begint met een analyse van geschiktheid, gevolgd door de grootte instelt, en ten slotte een maandelijks raming van kosten. Een virtuele machine alleen worden verplaatst langs op een later stadium als de vorige bewerking is geslaagd. Bijvoorbeeld, als een virtuele machine de controle met Azure-geschiktheid mislukt, wordt deze gemarkeerd als niet geschikt voor Azure, en de dimensionering en de kosten wordt niet worden uitgevoerd.

## <a name="whats-in-an-assessment"></a>Waaruit bestaat een evaluatie?

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/> Azure Migrate ondersteunt momenteel deze doelregio's: Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, Canada-centraal, Canada-Oost, centraal-India, VS-midden, China-Oost, China-Noord, Oost-Azië, VS-Oost, VS-Oost 2, Duitsland-centraal, Duitsland-Noordoost, Japan-Oost, Japan-West, Korea centraal, Korea Zuid, Noord VS-midden, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, Zuid-India, UK-Zuid, UK-West, VS (overheid)-Arizona, VS (overheid)-Texas, VS (overheid) Virginia, West-Centraal VS, West-Europa, West-India, VS-West, en West vs2.<br/> De doellocatie is standaard ingesteld op US - west 2.
**Opslagtype** | Standard HHD schijven/Standard-SSD-schijven/Premium.<br/> Wanneer u het opslagtype als automatisch in een evaluatie opgeeft, wordt de schijf-aanbeveling is gebaseerd op de prestatiegegevens van de schijven (IOPS en doorvoer).<br/> Als u het opslagtype als Premium of standaard opgeeft, wordt de evaluatie van de gebruiker wordt aanbevolen een schijf SKU binnen het opslagtype geselecteerd.<br/> Als u wilt bereiken van één exemplaar VM SLA van 99,9%, kunt u het opslagtype instellen als Premium-beheerde schijven. Vervolgens worden alle schijven in de evaluatie worden aanbevolen als Premium-beheerde schijven. <br/> Azure Migrate biedt voor migratiebeoordeling alleen ondersteuning voor beheerde schijven.<br/> 
**Gereserveerde instanties (RI)** | Deze eigenschap opgeven als u instanties in Azure gereserveerde. In de evaluatie-kosten schattingen worden gereserveerde instanties kortingen rekening gehouden. Gereserveerde instanties zijn momenteel alleen ondersteund voor betalen per gebruik in Azure Migrate biedt.
**Criterium voor het aanpassen van de grootte** | Gebruikt om virtuele machines de juiste grootte. Grootte kan zijn op basis van prestaties of **zoals on-premises**, zonder rekening te houden de prestatiegeschiedenis.
**Prestatiegeschiedenis** | De duur voor het evalueren van prestaties van de virtuele machine. Deze eigenschap is alleen van toepassing wanneer de grootte op basis van prestaties.
**Percentiel gebruik** | De percentielwaarde van het voorbeeld van de prestaties die wordt gebruikt voor het juiste formaat van virtuele machines. Deze eigenschap is alleen van toepassing wanneer de grootte op basis van prestaties.
**VM-reeks** | De VM-reeks die voor schattingen van grootte wordt gebruikt. Als u bijvoorbeeld een productieomgeving hebt die u niet gaat migreren naar de A-serie van virtuele machines in Azure, kunt u de A-serie uitsluiten van de lijst of reeks. Grootte is alleen gebaseerd op de geselecteerde reeks.
**Comfortfactor** | Azure Migrate-Server-evaluatie, rekening gehouden met een buffer (comfortfactor) tijdens de evaluatie. Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen.
**Aanbieding** | De [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarop u bent geregistreerd. Azure Migrate maakt dienovereenkomstig een schatting van de kosten.
**Valuta** | Factureringsvaluta. 
**Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt.<br/> De standaardinstelling is 0%.
**VM tijd actief** | Als uw virtuele machines niet meer 24 x 7 worden uitgevoerd op Azure, kunt u de duur (aantal dagen per maand) en het aantal uur per dag voor dat deze zou worden uitgevoerd en de kostenramingen dienovereenkomstig wordt uitgevoerd.<br/> De standaardwaarde is 31 dagen per maand en 24 uur per dag.
**Azure Hybrid Benefit** | Geeft aan of u over software Assurance beschikt en komen in aanmerking voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. | Standaard is Ja.


## <a name="edit-assessment-properties"></a>Evaluatie-eigenschappen bewerken

Als u wilt bewerken evaluatie-eigenschappen na het maken van een beoordeling, het volgende doen:

1. Klik in de Azure Migrate-project op **Servers**.
2. In **Azure Migrate: Server-evaluatie**, klikt u op het aantal evaluaties.
3. In **evaluatie**, klikt u op de evaluatie van de relevante > **eigenschappen bewerken**.
5. De evaluatie-eigenschappen in overeenstemming met de bovenstaande tabel aanpassen.
6. Klik op **opslaan** aan de evaluatie van updates.


U kunt ook een evaluatie-eigenschappen bewerken wanneer u een evaluatie maakt.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
