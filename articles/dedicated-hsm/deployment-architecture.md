---
title: Azure toegewezen HSM implementatie-architectuur | Microsoft Docs
description: Azure toegewezen HSM biedt mogelijkheden voor opslag van sleutels in Azure die voldoet aan FIPS 140-2 Level 3-certificering
services: dedicated-hsm
author: barclayn
manager: mbaldwin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: barclayn
ms.openlocfilehash: 8481f444464135a24d42612f334ac6096cba7b21
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319412"
---
# <a name="deployment-architecture"></a>Implementatie-architectuur

Azure toegewezen HSM biedt opslag van cryptografische sleutels in Azure. Deze voldoet aan strenge beveiligingsvereisten. Klanten profiteren van Azure toegewezen HSM als ze:

* FIPS moet 140-2 Level 3-certificering voldoen
* Vereisen dat ze exclusieve toegang tot de HSM hebben
* volledige controle over hun apparaten moeten hebben

De HSM's worden verdeeld over de Microsoft-datacenters en kunnen eenvoudig worden ingericht als een paar apparaten als de basis van een maximaal beschikbare oplossing. Ze kunnen ook worden geïmplementeerd in regio's voor een robuuste oplossing voor noodherstel. De regio's met toegewezen HSM die beschikbaar zijn op dit moment:

* US - oost
* US - oost 2
* US - west
* US - zuid-centraal
* Azië - zuidoost
* Azië - oost
* Europa - noord
* Europa -west

Elk van deze regio's heeft HSM rekken geïmplementeerd in datacenters met twee onafhankelijke of ten minste twee onafhankelijke beschikbaarheidszones. Zuidoost-Azië heeft drie beschikbaarheidszones en VS-Oost 2 heeft twee. Er is een totaal van acht regio's in Europa, Azië en de VS die worden geboden door de toegewezen HSM-service. Zie voor meer informatie over Azure-regio's, de officiële [informatie van de Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/).
Bepaalde factoren ontwerp voor een oplossing op basis van toegewezen HSM locatie/latentie, hoge beschikbaarheid en ondersteuning voor andere gedistribueerde toepassingen.

## <a name="device-location"></a>Locatie apparaat

Locatie van het apparaat optimale HSM is in de dichtstbijzijnde nabijheid tot de toepassingen uitvoeren van cryptografische bewerkingen. Regionale latentie wordt verwacht één cijfer milliseconden. Regio-overschrijdende latentie kan 5 tot 10 keer hoger zijn dan dit.

## <a name="high-availability"></a>Hoge beschikbaarheid

Voor het bereiken van hoge beschikbaarheid, moet een klant twee HSM-apparaten in een regio die zijn geconfigureerd met behulp van Gemalto software als een paar met hoge beschikbaarheid gebruiken. Dit type implementatie zorgt ervoor dat de beschikbaarheid van sleutels als er een probleem te voorkomen dat deze sleutel verwerkingen is een enkel apparaat. Bovendien vermindert het risico bij het uitvoeren van onderhoud, zoals power supply vervanging ondersteuning voor probleemoplossing. Het is belangrijk voor een ontwerp voor elk soort regionaal niveau. Storingen in andere niveau kunnen gebeuren wanneer er natuurrampen zoals orkanen, overstromingen of aardbevingen. Deze typen gebeurtenissen moeten worden verholpen door het inrichten van HSM-apparaten in een andere regio. Apparaten die zijn geïmplementeerd in een andere regio kunnen samen worden gekoppeld via de softwareconfiguratie voor Gemalto. Dit betekent dat de minimale implementatie voor een maximaal beschikbare en noodherstel flexibele oplossing is vier HSM-apparaten in twee regio's. Lokale redundantie en redundantie in regio's kunnen worden gebruikt als een basislijn toe te voegen verdere HSM-apparaat implementaties voor de ondersteuning van latentie, capaciteit of om te voldoen aan andere vereisten toepassingsspecifieke.

## <a name="distributed-application-support"></a>Gedistribueerde toepassingsondersteuning

Toegewezen HSM-apparaten worden doorgaans geïmplementeerd ter ondersteuning van toepassingen die nodig zijn om uit te voeren van de opslag van sleutels en bewerkingen voor het sleutel ophalen. Toegewezen HSM apparaten hebben 10 partities voor ondersteuning van onafhankelijke toepassing. Locatie van het apparaat moet worden gebaseerd op een holistische weergave van alle toepassingen die u nodig hebt om de service te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zodra de implementatie-architectuur wordt bepaald, worden de meeste configuratieactiviteiten te implementeren die architectuur wordt geleverd door Gemalto. Dit omvat zowel apparaatconfiguratie als toepassing integratiescenario's. Gebruik voor meer informatie de [Gemalto klantondersteuning](https://supportportal.gemalto.com/csm/) portal en download handleidingen voor beheer en de configuratie. De website van Microsoft-partner heeft een aantal integratiehandleidingen.
Het is raadzaam dat alle belangrijke concepten van de service, zoals hoge beschikbaarheid en beveiliging bijvoorbeeld duidelijk zijn voordat het apparaat inrichten of toepassingsontwerp en implementatie.
Verdere concept niveau onderwerpen:

* [Hoge beschikbaarheid](high-availability.md)
* [Fysieke beveiliging](physical-security.md)
* [Netwerken](networking.md)
* [Ondersteuning](supportability.md)
* [Controle](monitoring.md)
