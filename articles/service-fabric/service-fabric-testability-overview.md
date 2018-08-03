---
title: Fault Analysis Service-overzicht | Microsoft Docs
description: Dit artikel beschrijft de Fault Analysis Service in Service Fabric voor fouten veroorzaken en Testscenario's uitvoeren op basis van uw services.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: a4ddfc17a81a6816bc797bab4c3b5a8b2fc4334e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425235"
---
# <a name="introduction-to-the-fault-analysis-service"></a>Inleiding tot de Fault Analyseservice
De Fault Analysis Service is ontworpen voor het testen van services die zijn gebouwd op Microsoft Azure Service Fabric. U kunt met de Fault Analysis Service zinvolle fouten veroorzaken en volledige Testscenario's uitvoeren op basis van uw toepassingen. Deze fouten en scenario's maken en valideren van de talloze Staten en overgangen die een service tijdens de levensduur, allemaal in een gecontroleerde, veilige en consistente manier optreden zal.

Acties zijn de afzonderlijke fouten die zijn gericht op een service voor het testen. Een ontwikkelaar van de service kunt gebruiken als bouwstenen schrijven van complexe scenario's. Bijvoorbeeld:

* Start opnieuw op een knooppunt om te simuleren van een willekeurig aantal situaties waarin een machine of virtuele machine opnieuw wordt opgestart.
* Verplaatsen van een replica van de stateful service voor het simuleren van taakverdeling, failover of upgrade van de toepassing.
* Quorumverlies in een stateful service voor het maken van een situatie waarbij schrijfbewerkingen kunnen niet doorgaan omdat er niet voldoende 'back-up' of 'secundair' replica's te accepteren van nieuwe gegevens worden aangeroepen.
* Roep verlies van gegevens op een stateful service voor het maken van een situatie waarbij alle in het geheugen status volledig zijn gewist.

Scenario's zijn complexe bewerkingen bestaat uit een of meer acties. De Fault Analysis Service biedt twee ingebouwde voltooid scenario's:

* Chaos-Scenario
* Failover-Scenario

## <a name="testing-as-a-service"></a>Als een service testen
De Fault Analysis Service is een Service Fabric system-service die automatisch wordt gestart met een Service Fabric-cluster. Deze service fungeert als host voor foutinjectie scenario testuitvoering en statusanalyse. 

![Fault Analyseservice][0]

Wanneer een fout met betrekking tot actie of een test-scenario wordt gestart, wordt een opdracht verzonden naar de Fault Analysis Service om uit te voeren van de fout met betrekking tot actie of een test-scenario. De Fault Analysis Service is stateful zodat deze op betrouwbare wijze kan fouten en scenario's worden uitgevoerd en valideer de resultaten. Een Testscenario voor het van langlopende kan bijvoorbeeld betrouwbaar uitgevoerd door de Fault Analysis Service. En omdat tests worden uitgevoerd binnen het cluster, de service de status van het cluster en uw services voor meer gedetailleerde informatie over fouten bij kunt bekijken.

## <a name="testing-distributed-systems"></a>Testen van gedistribueerde systemen
Service Fabric zorgt de taak van het schrijven en beheren van gedistribueerde schaalbare toepassingen aanzienlijk eenvoudiger. De Fault Analysis Service vereenvoudigt het testen van een gedistribueerde toepassing die op dezelfde manier gemakkelijker. Er zijn drie belangrijke problemen die worden opgelost moeten bij het testen van:

1. Simuleren/genereren van fouten die in de praktijk beproefd scenario optreden: een van de belangrijke aspecten van Service Fabric is dat het gedistribueerde toepassingen voor het herstellen van diverse fouten kunt. Als u wilt testen dat de toepassing kan om deze fouten te herstellen, moet er echter een mechanisme voor het simuleren/genereren van deze echte storingen in een testomgeving.
1. De mogelijkheid voor het genereren van gecorreleerde fouten: Basic fouten in het systeem, zoals netwerkstoringen en fouten van de machine, zijn gemakkelijk te produceren afzonderlijk. Genereren van een groot aantal scenario's die in de praktijk als gevolg van de interacties van deze afzonderlijke fouten kunnen ontstaan is essentieel.
1. Uniforme wijze van werken over verschillende niveaus van de ontwikkeling en implementatie: Er zijn veel systemen van de fout met betrekking tot injectie die verschillende typen fouten kunnen uitvoeren. De ervaring in al deze waarden is slecht bij het verplaatsen van scenario's van de in-één ontwikkelaars, dezelfde tests uitgevoerd op grote testomgevingen, deze alleen gebruiken voor de tests in de productieomgeving.

Er zijn veel mechanismen voor het oplossen van deze problemen, een systeem dat hetzelfde met vereiste garanties--helemaal uit een in-één ontwikkelomgeving gebeurt, om te testen in productieclusters--ontbreekt. De Fault Analysis Service helpt de toepassingsontwikkelaars zich concentreren op het testen van hun zakelijke logica. De Fault Analysis Service biedt alle mogelijkheden die nodig zijn voor het testen van de interactie van de service met de onderliggende gedistribueerd systeem.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Scenario's voor echte storingen simuleren/genereren
Als u wilt testen de robuustheid van een gedistribueerd systeem tegen storingen, moeten we een mechanisme voor het genereren van fouten. In theorie, begint genereert een fout als een knooppunt is uitgevallen eenvoudig lijkt, deze te maken met dezelfde set consistentieproblemen die Service Fabric is probeert op te lossen. Een voorbeeld: als we willen dat een knooppunt afsluiten de vereiste werkstroom is het volgende:

1. Uitgeven van de client een aanvraag tot afsluiten knooppunt.
1. De aanvraag verzenden naar het juiste knooppunt.
   
    a. Als het knooppunt niet wordt gevonden, wordt dat zou moeten mislukken.
   
    b. Als het knooppunt wordt gevonden, moet worden geretourneerd alleen als het knooppunt wordt afgesloten.

Als u wilt controleren of de fout vanuit het oogpunt van de test, moet de test om te weten dat wanneer deze fout is veroorzaakt, de fout daadwerkelijk gebeurt. De garantie dat de Service Fabric biedt is dat een van beide het knooppunt wordt uitvallen of was al na het verstrijken van de opdracht het knooppunt. In beide gevallen moet de proef kunnen correct reden over de status en slagen of mislukken goed in de validatie. Een systeem geïmplementeerd buiten een Service Fabric te doen van dezelfde set fouten kan veel netwerk, hardware en softwareproblemen, waardoor het geven van de voorgaande garanties bereikt. Met de problemen vermeld voordat u Service Fabric wordt opnieuw configureert de clusterstatus om de problemen te voorkomen, en daarom de Fault Analysis Service is nog steeds mogelijk dat de juiste set met garanties.

### <a name="generating-required-events-and-scenarios"></a>Scenario's en de vereiste gebeurtenissen worden gegenereerd
Hoewel een echte fout consistent simuleren moeilijke beginnen met, is de mogelijkheid voor het genereren van gecorreleerde fouten zelfs verscherping. Bijvoorbeeld, een verlies van gegevens in een stateful service persistente gebeurt wanneer de volgende dingen gebeuren:

1. Alleen een quorum schrijven van de replica's zijn bijgewerkt voor replicatie. De primaire volgen op de secundaire replica's.
1. Het quorum schrijven wordt uitgeschakeld omdat de replica's omlaag gaat (als gevolg van een codepakket of knooppunt omlaag gaat).
1. Het quorum schrijven kan niet overstappen van omdat de gegevens voor de replica's verloren gaat (als gevolg van bij een beschadigde schijf of machine installatiekopie terugzetten).

Deze gecorreleerde fouten plaatsvindt in de praktijk, maar niet als vaak als afzonderlijke fouten. De mogelijkheid om te testen voor deze scenario's voordat ze in productie optreden is essentieel. Zelfs belangrijker is de mogelijkheid voor het simuleren van deze scenario's met productieworkloads in gecontroleerde omstandigheden (in het midden van de dag met alle technici op dek). Dat is beter dan realiseren voor de eerste keer in de productieomgeving om 2:00 uur

### <a name="unified-experience-across-different-environments"></a>Geïntegreerde ervaring in verschillende omgevingen
De oefening is traditioneel voor het maken van drie verschillende sets van ervaringen, één voor de ontwikkelomgeving, één voor tests en één voor productie. Het model is:

1. In de ontwikkelomgeving, produceren statusovergangen waarmee eenheidstests van afzonderlijke methoden.
1. In de testomgeving, produceren, mislukte aanmeldingen dat end-to-end-tests die verschillende foutscenario's.
1. Houd de productie-omgeving Nieuw om te voorkomen dat niet-natuurlijk fouten en om ervoor te zorgen dat er zeer snel menselijke antwoord bij een storing.

In Service Fabric, via de Fault Analysis Service, zijn we voorstellen en schakelt u deze optie om gebruik van dezelfde methode van de ontwikkelomgeving instellen voor de productie. Er zijn twee manieren om dit te bereiken:

1. Als u wilt een gecontroleerde fouten, gebruiken de Fault Analysis Service-API's van een one-box-omgeving helemaal naar productieclusters.
1. Om te geven van het cluster een KVP die ervoor zorgt automatische doen ontstaan van fouten dat, gebruikt u de Fault Analysis Service voor het genereren van automatische fouten. Beheren van de frequentie van fouten door te configureren, kunt dezelfde service anders worden getest in verschillende omgevingen.

Met Service Fabric, hoewel de schaal van fouten in de verschillende omgevingen, anders zou zijn zou de werkelijke mechanismen identiek zijn. Dit maakt een pijplijn voor veel sneller code-naar-implementatie en de mogelijkheid voor het testen van de services onder de werkelijke belasting.

## <a name="using-the-fault-analysis-service"></a>De Fault Analyseservice gebruiken
**C#**

Fault Analysis Service-functies zijn in de naamruimte System.Fabric in het Microsoft.ServiceFabric NuGet-pakket. Voor het gebruik van de Fault Analysis Service-functies omvatten het nuget-pakket als een verwijzing in uw project.

**PowerShell**

Voor het gebruik van PowerShell, moet u de Service Fabric SDK installeren. Nadat de SDK is geïnstalleerd, is de ServiceFabric-PowerShell-module automatisch geladen voor gebruik.

## <a name="next-steps"></a>Volgende stappen
Als u wilt maken echt schaalbare services, is het essentieel om ervoor te zorgen, zowel vóór als na de implementatie dat services bestand is tegen storingen van de echte wereld werkt. In de wereld services nu is de mogelijkheid snel innoveren en code snel naar productie gaan erg belangrijk. De Fault Analysis Service helpt service waarmee ontwikkelaars om precies dat te doen.

Beginnen met het testen van uw toepassingen en services met behulp van de ingebouwde [Testscenario's](service-fabric-testability-scenarios.md), of maak uw eigen test-scenario's met behulp van de [fault acties](service-fabric-testability-actions.md) geleverd door de Fault Analysis Service.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
