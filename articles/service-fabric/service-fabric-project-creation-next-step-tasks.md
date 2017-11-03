---
title: Volgende stappen voor het maken van service Fabric-project | Microsoft Docs
description: In dit artikel bevat koppelingen naar een reeks ontwikkeling kerntaken voor Service Fabric
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
ms.date: 09/28/2017
ms.author: rwike77
ms.openlocfilehash: 4523c63493bc9cb3f96713c4abbc1dd1da9130d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Uw Service Fabric-toepassing en de volgende stappen
Uw Azure Service Fabric-toepassing is gemaakt. In dit artikel beschrijft de samenstelling van uw project en een aantal mogelijke volgende stappen.

## <a name="your-application"></a>Uw toepassing
Elke nieuwe toepassing bevat een toepassingsproject. Er zijn een of twee extra projecten, afhankelijk van het type service gekozen.

### <a name="the-application-project"></a>Het toepassingsproject
Het toepassingsproject bestaat uit:

* Een aantal verwijzingen naar de services die gezamenlijk uw toepassing.
* Drie publiceren profielen (1-knooppunt lokale, 5-knooppunt lokale en Cloud) die u gebruiken kunt voor het onderhouden van voorkeuren voor het werken met verschillende omgevingen--zoals voorkeuren die betrekking hebben op een clustereindpunt en of u wilt uitvoeren van upgrades implementaties standaard.
* Drie toepassing parameter bestanden (zelfde als hierboven) waarmee u kunt onderhouden omgeving-specifieke toepassingsconfiguraties, zoals het aantal partities voor een service maken.
* Een script voor een implementatie die u gebruiken kunt om uw toepassing vanaf de opdrachtregel of als onderdeel van een geautomatiseerde continue integratie en implementatie-pijplijn te implementeren.
* Het toepassingsmanifest waarin de toepassing. U kunt het manifest vinden onder de map ApplicationPackageRoot.

### <a name="stateless-service"></a>Staatloze service
Wanneer u een nieuwe staatloze service toevoegt, wordt een service-project met Visual Studio toegevoegd aan uw oplossing met een type dat is afgeleid van `StatelessService`. De service wordt een lokale variabele in een teller verhoogd.

### <a name="stateful-service"></a>Stateful service
Wanneer u een nieuwe stateful service toevoegt, wordt een service-project met Visual Studio toegevoegd aan uw oplossing met een type dat is afgeleid van `StatefulService`. De service wordt verhoogd van een item in de `RunAsync` methode en slaat de resultaten in een `ReliableDictionary`.

### <a name="actor-service"></a>Actorservice
Wanneer u een nieuwe betrouwbare actor toevoegt, Visual Studio voegt twee projecten toe aan uw oplossing: een actor-project en een interface-project.

Het project actor biedt methoden voor het instellen en betrouwbaar ophalen van de waarde van een prestatiemeteritem dat wordt bewaard in de actor staat. Het project interface biedt een interface die andere services gebruiken kunnen om aan te roepen de actor.

### <a name="stateless-web-api"></a>Staatloze Web-API
Staatloze Web API-project biedt een eenvoudige webservice die u gebruiken kunt om uw toepassing naar externe clients te openen. Zie voor meer informatie over hoe het project gestructureerd, [Service Fabric-Web-API-services met OWIN zelf hosting](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>ASP.NET core
De Service Fabric SDK biedt dezelfde set ASP.NET Core sjablonen die beschikbaar zijn voor zelfstandige ASP.NET Core projecten: leeg is, [Web API][aspnet-webapi], en [webtoepassing][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Gast uitvoerbare bestanden en de Gast-containers

Een Service Fabric 'gast' is een service die niet is gebouwd met het platform programmeermodellen. Kunt u een pakket de binaire bestanden voor een gast ofwel [rechtstreeks in het toepassingspakket](service-fabric-deploy-existing-app.md) of [via een installatiekopie van een container](service-fabric-deploy-container.md). In beide gevallen Visual Studio maakt de benodigde artefacten in de **ApplicationPackageRoot** map van het toepassingsproject. Visual Studio wordt een nieuw serviceproject niet maken omdat de code al elders. Als u uw projecten Gast naast het project Service Fabric-toepassing te beheren wilt, kunt u ze kunt toevoegen aan dezelfde Visual Studio-oplossing.

## <a name="next-steps"></a>Volgende stappen
### <a name="create-an-azure-cluster"></a>Een Azure-cluster maken
De Service Fabric SDK bevat een lokaal cluster voor ontwikkeling en testen. Zie voor informatie over het maken van een cluster in Azure [Service Fabric-cluster instellen vanuit de Azure-portal][create-cluster-in-portal].

### <a name="publish-your-application-to-azure"></a>Uw toepassing publiceren in Azure
U kunt uw toepassing rechtstreeks vanuit Visual Studio om een Azure-cluster te publiceren. Voor meer informatie Zie [publiceren van uw toepassing naar Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Service Fabric Explorer gebruiken voor uw cluster visualiseren
Service Fabric Explorer biedt een eenvoudige manier voor het visualiseren van uw cluster, inclusief geïmplementeerde toepassingen en de fysieke indeling. Zie voor meer informatie, [uw cluster visualiseren met behulp van Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Versie en werk uw services
Service Fabric kunnen onafhankelijke versioning en upgraden van onafhankelijke services in een toepassing. Zie voor meer informatie, [versiebeheer en upgraden van uw services][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Continue integratie met Visual Studio Team Services configureren
Zie voor meer informatie over hoe kunt u een continue integratie-proces instellen voor uw Service Fabric-toepassing, [continue integratie met Visual Studio Team Services configureren][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
