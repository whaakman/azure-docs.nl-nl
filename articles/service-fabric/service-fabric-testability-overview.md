---
title: Fout met betrekking tot Analysis Services-overzicht | Microsoft Docs
description: In dit artikel beschrijft de fout Analysis Services in Service Fabric voor fouten veroorzaken en Testscenario's uitvoeren op basis van uw services.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: f275fa5d3d6d727b016e55c188321d7e68091a33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-the-fault-analysis-service"></a>Inleiding tot de fout Analysis Services
De fout Analysis Services is ontworpen voor het testen van de services die zijn gebouwd op Microsoft Azure Service Fabric. U kunt met de fout Analysis Service zinvolle fouten veroorzaken en volledige Testscenario's uitvoeren op basis van uw toepassingen. Deze fouten en scenario's uitoefenen en valideren van de talloze statussen en overgangen die een service tijdens de levensduur, allemaal in een gecontroleerde, veilige en consistente manier optreden zal.

Acties zijn de afzonderlijke fouten als een service voor het testen van het doel. Een ontwikkelaar van de service kunt deze als bouwstenen voor het schrijven van complexe scenario's gebruiken. Bijvoorbeeld:

* Start opnieuw op een knooppunt om te simuleren van een willekeurig aantal situaties waarbij een computer of virtuele machine opnieuw is opgestart.
* Verplaatsen van een replica van de stateful service om te simuleren taakverdeling, failover of upgrade van de toepassing.
* Quorumverlies op een stateful service voor het maken van een situatie waarin schrijfbewerkingen kunnen niet doorgaan omdat er niet voldoende 'back-up' of 'secundair' replica's te accepteren van nieuwe gegevens worden aangeroepen.
* Verlies van gegevens op een stateful service voor het maken van een situatie waarin alle geheugenstatus volledig wordt gewist uit worden aangeroepen.

Scenario's zijn complexe bewerkingen bestaat uit een of meer acties. De fout Analysis Services bevat twee ingebouwde voltooien van scenario's:

* Chaos Scenario
* Failover-Scenario

## <a name="testing-as-a-service"></a>Als een service testen
De fout Analysis Services is een Service Fabric-systeemservice die automatisch wordt gestart met een Service Fabric-cluster. Deze service fungeert als host voor fouttolerantie injectie scenario testuitvoering en statusanalyse. 

![Fout met betrekking tot Analysis Services][0]

Wanneer een fout met betrekking tot actie of een test-scenario is gestart, wordt een opdracht verzonden naar de fout Analysis Services om uit te voeren van het scenario van actie- of testomgeving veroorzaakt. De fout Analysis Services is stateful zodat betrouwbaar kunnen fouten en scenario's worden uitgevoerd en valideer de resultaten. Een Testscenario langlopende kan bijvoorbeeld betrouwbaar uitgevoerd door de fout Analysis Services. En omdat tests worden in het cluster wordt uitgevoerd, de service de status van het cluster en de services voor meer gedetailleerde informatie over fouten kunt bekijken.

## <a name="testing-distributed-systems"></a>Gedistribueerde systemen testen
Service Fabric maakt de taak schrijven en het beheren van gedistribueerde schaalbare toepassingen aanzienlijk eenvoudiger. De fout Analysis Service vereenvoudigt het testen van een gedistribueerde toepassing die op dezelfde manier eenvoudiger. Er zijn drie belangrijkste problemen die worden opgelost moeten bij het testen:

1. Simuleren/genereren van fouten die in real-world scenario's optreden: een van de belangrijke aspecten van Service Fabric is dat deze wordt gebruikt om het herstellen van diverse fouten gedistribueerde toepassingen. Als u wilt testen dat de toepassing kunnen herstellen als deze fouten is, moeten we echter een mechanisme voor deze echte storingen in een testomgeving gecontroleerde simuleren/genereren.
2. De mogelijkheid voor het genereren van gecorreleerde fouten: Basic fouten in het systeem, zoals netwerkstoringen en machine, zijn gemakkelijk te produceren afzonderlijk. Genereren van een groot aantal scenario's die kunnen ontstaan in de praktijk als gevolg van de interactie van deze afzonderlijke storingen is essentieel.
3. Uniforme wijze van werken over verschillende niveaus van ontwikkeling en implementatie: Er zijn veel fout injectiesystemen die verschillende typen fouten kunnen doen. De ervaring in al deze waarden is slecht bij het verplaatsen van een selectievakje developer-scenario's, naar dezelfde tests uitgevoerd in grote testomgevingen aan het gebruik ervan voor tests in de productieomgeving.

Er zijn veel mechanismen voor het oplossen van deze problemen, een systeem dat hetzelfde vereiste garanties--helemaal vanuit een developer 1-box-omgeving doet, om te testen in productieclusters ontbreekt. De fout Analysis Service helpt de toepassingsontwikkelaars concentreren op hun zakelijke logica te testen. De fout Analysis Services biedt alle mogelijkheden die nodig zijn voor het testen van de interactie van de service met het onderliggende gedistribueerde systeem.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Scenario's voor echte fouten simuleren/genereren
Als u wilt testen de robuustheid van een gedistribueerde systeem tegen storingen, moeten we een mechanisme voor het genereren van fouten. Terwijl in theorie genereren een fout als een knooppunt omlaag gemakkelijk lijkt, deze wordt gestart kunt u dezelfde set consistentieproblemen die Service Fabric probeert op te lossen door. Bijvoorbeeld als we willen afsluiten van een knooppunt, is de vereiste werkstroom het volgende:

1. Uitgeven van de client een aanvraag tot afsluiten knooppunt.
2. De aanvraag verzenden naar het juiste knooppunt.
   
    a. Als het knooppunt niet wordt gevonden, moet het mislukken.
   
    b. Als het knooppunt wordt gevonden, deze moet worden geretourneerd alleen als het knooppunt is afgesloten.

Om te controleren of de fout vanuit het perspectief van een test, wordt de test moet weten dat wanneer deze fout is veroorzaakt, de fout daadwerkelijk gebeurt. De garantie van Service Fabric bevat, is dat beide het knooppunt zal of was al niet actief wanneer de opdracht het knooppunt bereikt. In beide gevallen moet de proef kunnen correct reden over de status en slagen of mislukken correct in de validatie. Een systeem buiten de Service Fabric dezelfde set fouten doen geïmplementeerd kan veel netwerk, hardware en softwareproblemen die u wilt ervoor zorgen dat deze de voorgaande garanties bereikt. In de aanwezigheid van de problemen vermeld voordat Service Fabric configureert opnieuw de clusterstatus om de problemen te voorkomen, en daarom de fout Analysis Services wordt nog steeds zodat de juiste set met garanties.

### <a name="generating-required-events-and-scenarios"></a>Scenario's en vereiste gebeurtenissen genereren
Hoewel een werkelijke storing consistent simuleren robuust beginnen met, is de mogelijkheid voor het genereren van gecorreleerde fouten zelfs verscherping. Bijvoorbeeld, een verlies van gegevens in een stateful service persistente gebeurt wanneer het volgende plaats:

1. Alleen een quorum van schrijven van de replica's zijn bijgewerkt voor de replicatie. De secundaire replica's lag achter de primaire.
2. Het quorum schrijven wordt uitgeschakeld vanwege de replica's tijdelijk niet beschikbaar (als gevolg van een codepakket of een knooppunt op).
3. Het quorum schrijven kan niet terugkeren van omdat de gegevens voor de replica's verloren (vanwege beschadigde schijf of machine teruggezet).

Deze gecorreleerde storingen in de praktijk maar niet zo vaak als afzonderlijke fouten optreden. De mogelijkheid om te testen voor deze scenario's voordat ze in productie optreden is essentieel. Zelfs belangrijker is de mogelijkheid om te simuleren van deze scenario's met de productie-workloads in gecontroleerde omstandigheden (in het midden van de dag met alle engineers op dek). Dat is beter dan dat het programma worden uitgevoerd voor het eerst in de productieomgeving om 2:00 uur

### <a name="unified-experience-across-different-environments"></a>Geïntegreerde ervaring in verschillende omgevingen
De procedure is normaal gesproken voor het maken van drie verschillende sets ervaringen, één voor de ontwikkelomgeving, één voor de tests en één voor productie. Het model is:

1. In de ontwikkelomgeving produceren statusovergangen waarmee controles van afzonderlijke methoden.
2. In de testomgeving produceren mislukte dat is toegestaan end-to-end-tests die verschillende scenario's voor fout uitoefenen.
3. Houd de productieomgeving Nieuw om te voorkomen dat niet-natuurlijk fouten en zorg ervoor dat er extreem snel menselijke antwoord bij een storing.

In Service Fabric via de fout Analysis Service, willen we dit rond inschakelen en gebruiken van dezelfde methode van de ontwikkelomgeving voor productie. Er zijn twee manieren om dit te bereiken:

1. Gebruik voor het gecontroleerde storingen veroorzaken, de fout Analysis Service API's van een 1-box-omgeving helemaal naar productieclusters.
2. Als u het cluster een KVP dat ervoor zorgt automatische doen ontstaan van fouten dat geven, door de fout Analysis-Service te gebruiken voor het genereren van automatische fouten. De frequentie van fouten via configuratie beheren, schakelt u de dezelfde service anders in verschillende omgevingen worden getest.

Met Service Fabric, hoewel de schaal van fouten in de verschillende omgevingen anders zou de werkelijke mechanismen niet identiek zijn. Dit maakt een veel sneller code implementatie-pijplijn en de mogelijkheid voor het testen van de werkelijke hoeveelheden-services.

## <a name="using-the-fault-analysis-service"></a>Met behulp van de fout Analysis Services
**C#**

Fout met betrekking tot Analysis Services-functies zijn in de naamruimte System.Fabric in het Microsoft.ServiceFabric NuGet-pakket. Voor het gebruik van de Analysis Services-fout functies omvatten het nuget-pakket als een verwijzing in uw project.

**PowerShell**

Als PowerShell wilt gebruiken, moet u de Service Fabric SDK installeren. Nadat de SDK is geïnstalleerd, wordt de ServiceFabric PowerShell-module automatisch geladen moet worden gebruikt.

## <a name="next-steps"></a>Volgende stappen
Werkelijk cloud scale als services wilt maken, is het essentieel om ervoor te zorgen, zowel voor en na de implementatie van dat services werkelijkheid fouten kunnen tolereren. In de wereld services vandaag de dag, is kunt u snel innoveren en code snel naar productie verplaatsen heel belangrijk. De fout Analysis Services kunnen service softwareontwikkelaars nauwkeurig dat doet.

Beginnen met het testen van uw toepassingen en services met behulp van de ingebouwde ['s testen](service-fabric-testability-scenarios.md), of maak uw eigen Testscenario's met behulp van de [acties fault](service-fabric-testability-actions.md) geleverd door de fout Analysis Services.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
