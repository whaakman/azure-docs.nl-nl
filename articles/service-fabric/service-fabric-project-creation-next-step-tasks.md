---
title: Volgende stappen voor projectcreatie service Fabric | Microsoft Docs
description: Meer informatie over het toepassingsproject die u zojuist hebt gemaakt in Visual Studio.  Informatie over het bouwen van services met behulp van zelfstudies en meer informatie over het ontwikkelen van services voor Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: rwike77
ms.openlocfilehash: 4d5e74b9ecffbf8f1161cf6c5ef948cd154d993f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233153"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Uw Service Fabric-toepassing en de volgende stappen
Uw Azure Service Fabric-toepassing is gemaakt. Dit artikel beschrijft enkele zelfstudies om uit te proberen, de samenstelling van uw project en sommige informatie die u mogelijk geïnteresseerd in mogelijke Vervolgstappen.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Aan de slag met de zelfstudies, zelfstudies en voorbeelden
Klaar om te beginnen?  

Werken met de zelfstudie over .NET-toepassingen. Meer informatie over het [een app bouwen](service-fabric-tutorial-create-dotnet-app.md) met een ASP.NET Core front-end en een stateful back-end, [implementeren van de toepassing](service-fabric-tutorial-deploy-app-to-party-cluster.md) naar een cluster [CI/CD configureren](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), en [instellen controle en diagnose](service-fabric-tutorial-monitoring-aspnet.md).

Probeer een van de volgende zelfstudies en uw eerste...
- [C#Reliable Services-service op Windows](service-fabric-reliable-services-quick-start.md) 
- [C#Reliable Actors-service op Windows](service-fabric-reliable-actors-get-started.md) 
- [Gast uitvoerbare service op Windows](quickstart-guest-app.md) 
- [Windows-containertoepassing](service-fabric-get-started-containers.md) 

Bent u mogelijk ook geïnteresseerd in uit te proberen onze [voorbeeldtoepassingen](https://aka.ms/servicefabricsamples).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Hebt u vragen of feedback?  Wilt u een probleem melden?
Lees het artikel [Veelgestelde vragen over](service-fabric-common-questions.md) en vind antwoorden over Service Fabric kunt doen en hoe deze moet worden gebruikt.

[Ondersteuningsopties](service-fabric-support.md) een lijst met forums op StackOverflow en MSDN voor het stellen van vragen, evenals de opties voor het melden van problemen en verzenden van productfeedback ondersteuning krijgen.

## <a name="the-application-project"></a>Het toepassingsproject
Elke nieuwe toepassing bevat een project voor een toepassing. Er is mogelijk een of twee extra projecten, afhankelijk van het type service gekozen.

Het toepassingsproject bestaat uit:

* Een set van verwijzingen naar de services die gezamenlijk uw toepassing.
* Drie publiceren profielen (1-Node lokale, 5-knooppunten lokale en Cloud) die u gebruiken kunt voor het beheren van voorkeuren voor het werken met verschillende omgevingen, zoals voorkeuren met betrekking tot het eindpunt van een cluster en of u de upgrade implementaties standaard.
* Drie toepassing parameter bestanden (hetzelfde als hierboven) die u gebruiken kunt om te onderhouden omgevingsspecifieke toepassingsconfiguraties, zoals het aantal partities te maken voor een service. Meer informatie over het [uw toepassing configureren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).
* Een script voor een implementatie die u gebruiken kunt om uw toepassing vanaf de opdrachtregel of als onderdeel van een geautomatiseerde continue integratie en implementatie-pijplijn te implementeren. Meer informatie over [implementeren van toepassingen met behulp van PowerShell](service-fabric-deploy-remove-applications.md).
* Het toepassingsmanifest, waarin de toepassing wordt beschreven. U kunt het manifest vinden onder de map ApplicationPackageRoot. Meer informatie over [toepassing en service manifesten](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Meer informatie over de programmeermodellen
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services.  Hier volgt overzicht en algemene informatie van [staatloze en stateful Reliable Services](service-fabric-reliable-services-introduction.md), [Reliable Actors](service-fabric-reliable-actors-introduction.md), [containers](service-fabric-containers-overview.md), [gastbestanden ](service-fabric-guest-executables-introduction.md), en [staatloze en stateful ASP.NET Core services](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Meer informatie over servicecommunicatie
Een Service Fabric-toepassing bestaat uit verschillende services uit, waarbij elke service een speciale taak uitvoert. Deze services kunnen communiceren met elkaar en kunnen er clienttoepassingen buiten het cluster die verbinding maken met en communiceren met services. Meer informatie over het [stelt u de communicatie met en tussen uw services](service-fabric-connect-and-communicate-with-services.md) in Service Fabric. 

## <a name="learn-about-configuring-application-security"></a>Meer informatie over het configureren van beveiliging van toepassingen
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen. Service Fabric helpt ook bij het beveiligen van de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts, bijvoorbeeld, bestanden, mappen en certificaten. Hierdoor actieve toepassingen, zelfs in een gedeelde omgeving, beter te beveiligen van elkaar.  Meer informatie over het [beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md).

Uw toepassing mogelijk vertrouwelijke gegevens, zoals storage-verbindingsreeksen, wachtwoorden, of andere waarden die niet moeten worden verwerkt als tekst zonder opmaak bevatten. Meer informatie over het [geheimen in uw toepassing beheren](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Meer informatie over de levensduur van toepassingen
Wanneer een Service Fabric-toepassing met andere platforms, meestal niet via de volgende fasen: ontwerp, ontwikkeling, testen, implementeren, upgraden, onderhoud en verwijderen. [In dit artikel](service-fabric-application-lifecycle.md) biedt een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levensduur van de Service Fabric-toepassingen.

## <a name="next-steps"></a>Volgende stappen
- [Een Windows-cluster maken in Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Een cluster, met inbegrip van geïmplementeerde toepassingen en de fysieke indeling met visualiseren [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Versie en upgrade van uw services](service-fabric-application-upgrade-tutorial.md)


