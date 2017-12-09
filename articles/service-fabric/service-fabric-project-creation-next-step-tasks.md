---
title: Volgende stappen voor het maken van service Fabric-project | Microsoft Docs
description: Meer informatie over het toepassingsproject die u zojuist hebt gemaakt in Visual Studio.  Informatie over het bouwen van services met behulp van zelfstudies en meer informatie over het ontwikkelen van services voor Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: rwike77
ms.openlocfilehash: 17eb1e7c2184fe9cae19685a47ea80716292b754
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Uw Service Fabric-toepassing en de volgende stappen
Uw Azure Service Fabric-toepassing is gemaakt. Dit artikel worden enkele zelfstudies om uit te proberen, de samenstelling van uw project en sommige informatie die u mogelijk geïnteresseerd in mogelijke volgende stappen.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Aan de slag met zelfstudies, zelfstudies en voorbeelden
Klaar om aan de slag te gaan?  

Werken de zelfstudie .NET-toepassing. Meer informatie over hoe [bouwen van een app](service-fabric-tutorial-create-dotnet-app.md) met een front-ASP.NET Core en een stateful back-end, [implementeert u de toepassing](service-fabric-tutorial-deploy-app-to-party-cluster.md) naar een cluster [configureren CI/CD](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), en [instellen controle en diagnostische gegevens](service-fabric-tutorial-monitoring-aspnet.md).

Een van de volgende zelfstudies uitproberen of maken van uw eerste...
- [C# Reliable Services-service op Windows](service-fabric-reliable-services-quick-start.md) 
- [C# Reliable Actors service in Windows](service-fabric-reliable-actors-get-started.md) 
- [Gast uitvoerbare service in Windows](quickstart-guest-app.md) 
- [Windows-container-toepassing](service-fabric-get-started-containers.md) 

Hebt u mogelijk ook geïnteresseerd in het uitproberen van onze [voorbeeldtoepassingen](http://aka.ms/servicefabricsamples).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Vragen of feedback hebt?  Wilt u een probleem melden?
Lees [Veelgestelde vragen](service-fabric-common-questions.md) en vindt u antwoorden op wat Service Fabric kan doen en hoe moet worden gebruikt.

[Ondersteuningsopties](service-fabric-support.md) geeft een lijst van forums over StackOverflow en MSDN vragen, evenals de opties voor het melden van problemen en verzenden van productfeedback ondersteuning wordt gevraagd.

## <a name="the-application-project"></a>Het toepassingsproject
Elke nieuwe toepassing bevat een toepassingsproject. Er zijn een of twee extra projecten, afhankelijk van het type service gekozen.

Het toepassingsproject bestaat uit:

* Een aantal verwijzingen naar de services die gezamenlijk uw toepassing.
* Drie publiceren profielen (1-knooppunt lokale, 5-knooppunt lokale en Cloud) die u gebruiken kunt voor het onderhouden van voorkeuren voor het werken met verschillende omgevingen--zoals voorkeuren die betrekking hebben op een clustereindpunt en of u wilt uitvoeren van upgrades implementaties standaard.
* Drie toepassing parameter bestanden (zelfde als hierboven) waarmee u kunt onderhouden omgeving-specifieke toepassingsconfiguraties, zoals het aantal partities voor een service maken. Meer informatie over hoe [configureren van uw toepassing voor omgevingen met meerdere](service-fabric-manage-multiple-environment-app-configuration.md).
* Een script voor een implementatie die u gebruiken kunt om uw toepassing vanaf de opdrachtregel of als onderdeel van een geautomatiseerde continue integratie en implementatie-pijplijn te implementeren. Meer informatie over [implementeren van toepassingen met behulp van PowerShell](service-fabric-deploy-remove-applications.md).
* Het toepassingsmanifest waarin de toepassing. U kunt het manifest vinden onder de map ApplicationPackageRoot. Meer informatie over [toepassing en service manifesten](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Meer informatie over de programmeermodellen
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services.  Hier volgt overzicht en algemene informatie op [staatloze en stateful Reliable Services](service-fabric-reliable-services-introduction.md), [Reliable Actors](service-fabric-reliable-actors-introduction.md), [containers](service-fabric-containers-overview.md), [Gast uitvoerbare bestanden ](service-fabric-deploy-existing-app.md), en [staatloze en stateful ASP.NET Core services](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Meer informatie over servicecommunicatie
Een Service Fabric-toepassing bestaat uit verschillende services, waarbij elke service een speciale taak uitvoert. Deze services met elkaar kunnen communiceren en kunnen er clienttoepassingen buiten het cluster die verbinding maken met en communiceren met de services. Meer informatie over hoe [communicatie met en tussen uw services instellen](service-fabric-connect-and-communicate-with-services.md) in Service Fabric. 

## <a name="learn-about-configuring-application-security"></a>Informatie over het configureren van beveiliging van toepassingen
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen. Service Fabric helpt ook bij de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts--bijvoorbeeld, bestanden, mappen en certificaten beveiligen. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde gehoste omgeving veiliger van elkaar.  Meer informatie over hoe [beveiligingsbeleid voor uw toepassing configureren](service-fabric-application-runas-security.md).

Uw toepassing mogelijk vertrouwelijke gegevens, zoals de storage-verbindingsreeksen, wachtwoorden of andere waarden die niet moeten worden behandeld als tekst zonder opmaak bevatten. Meer informatie over hoe [beheren geheimen in uw toepassing](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Meer informatie over de levenscyclus van de toepassing
Als een Service Fabric-toepassing met andere platforms, meestal via de volgende fasen verloopt: ontwerpen, ontwikkelen, testen, implementatie, upgraden, onderhoud en verwijderen. [In dit artikel](service-fabric-application-lifecycle.md) biedt een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levenscyclus van de Service Fabric-toepassing.

## <a name="next-steps"></a>Volgende stappen
- [Maken van een Windows-cluster in Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Uw cluster, inclusief geïmplementeerde toepassingen en de fysieke indeling visualiseren [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Versie en werk uw services](service-fabric-application-upgrade-tutorial.md)


