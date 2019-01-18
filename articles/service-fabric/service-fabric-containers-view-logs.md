---
title: Containers logboeken bekijken in Azure Service Fabric | Microsoft Docs
description: Beschrijft hoe u Logboeken voor containers voor een actieve Service Fabric-containerservices met Service Fabric Explorer weergeeft.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2018
ms.author: twhitney
ms.openlocfilehash: d66e27d860d18a37ffd9c6355b8d769116f26d73
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391237"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Logboeken weergeven voor een Service Fabric containerservice
Azure Service Fabric is een containerorchestrator en ondersteunt zowel [Linux- en Windows-containers](service-fabric-containers-overview.md).  In dit artikel wordt beschreven hoe u Logboeken voor containers van een actieve containerservice of een andere container weergeven, zodat u kunt vaststellen en oplossen van problemen.

## <a name="access-the-logs-of-a-running-container"></a>Toegang tot de logboeken van een container die wordt uitgevoerd
Logboeken voor containers kunnen worden geopend met behulp van [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Openen in een webbrowser, Service Fabric Explorer vanuit het beheereindpunt van het cluster door te navigeren naar [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Containerlogboeken bevinden zich op het clusterknooppunt waarop de container service-exemplaar wordt uitgevoerd op. Als voorbeeld krijgen de logboeken van de web-front-container van de [voorbeeldtoepassing Linux stemmen](service-fabric-quickstart-containers-linux.md). Vouw in de structuurweergave wordt weergegeven, **Cluster**>**toepassingen**>**VotingType**>**fabric: / Voting / azurevotefront**.  Vervolgens vouwt u de partitie (d1aa737e-f22a-e347-be16-eec90be24bc1, in dit voorbeeld) en Zie dat de container wordt uitgevoerd op clusterknooppunt *_lnxvm_0*.

Zoek in de structuurweergave wordt weergegeven, het codepakket op de *_lnxvm_0* knooppunt door uit te vouwen **knooppunten**>**_lnxvm_0**>**fabric: / Voting**  > **azurevotfrontPkg**>**codepakketten**>**code**.  Selecteer vervolgens de **Containerlogboeken** optie om de containerlogboeken van de weer te geven.

![Service Fabric-platform][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Toegang tot de logboeken van een container dode of vastgelopen
Vanaf v6.2, u kunt ook ophalen van de logboeken voor een inactieve of gecrashte container met [REST-API's](/rest/api/servicefabric/sfclient-index) of [Service Fabric CLI (SFCTL)](service-fabric-cli.md) opdrachten.

### <a name="set-container-retention-policy"></a>Bewaarbeleid voor containers instellen
Om gemakkelijker opstartfouten bij containers te analyseren, ondersteunt Service Fabric (versie 6.1 of hoger) het bewaren van containers die zijn gestopt of niet kunnen starten. Dit beleid kan worden ingesteld in het bestand **ApplicationManifest.xml**, zoals u in het volgende fragment ziet:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

De instelling **ContainersRetentionCount** geeft aan hoeveel containers er moeten worden bewaard wanneer ze fouten genereren. Als er een negatieve waarde wordt opgegeven, worden alle niet goed werkende containers bewaard. Wanneer de **ContainersRetentionCount** kenmerk is niet opgegeven, wordt er zijn geen containers bewaard. Het kenmerk **ContainersRetentionCount** ondersteunt ook toepassingsparameters, zodat gebruikers verschillende waarden kunnen opgeven voor test- en productieclusters. Bij het gebruik van deze functies dient u plaatsingsbeperkingen in te stellen om de containerservice op een bepaald knooppunt te richten. Zo voorkomt u dat de containerservice naar een ander knooppunt wordt verplaatst. Containers die met behulp van deze functie zijn bewaard, moeten handmatig worden verwijderd.

De instelling **RunInteractive** komt overeen met de Docker `--interactive` en `tty` [vlaggen](https://docs.docker.com/engine/reference/commandline/run/#options). Wanneer deze instelling is ingesteld op true in het manifestbestand, deze vlaggen worden gebruikt voor het starten van de container.  

### <a name="rest"></a>REST
Gebruik de [ophalen Container logboeken geïmplementeerd op knooppunt](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) bewerking voor het verkrijgen van de logboeken voor een container vastgelopen. Geef de naam van het knooppunt waarop de container is uitgevoerd op, toepassingsnaam, servicenaam manifest en de naam van het code-pakket.  Geef `&Previous=true`. Het antwoord bevat de logboeken voor containers voor de andere container van het exemplaar van code-pakket.

De aanvraag-URI heeft de volgende notatie:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Van de voorbeeldaanvraag:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

200 antwoordtekst:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Service Fabric (SFCTL)
Gebruik de [sfctl get-container-logboeken](service-fabric-sfctl-service.md) opdracht voor het ophalen van de logboeken voor een container vastgelopen.  Geef de naam van het knooppunt waarop de container is uitgevoerd op, toepassingsnaam, servicenaam manifest en de naam van het code-pakket. Geef de `--previous` vlag.  Het antwoord bevat de logboeken voor containers voor de andere container van het exemplaar van code-pakket.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –-previous
```
Reactie:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Volgende stappen
- Doorloop de [maken van een zelfstudie over het Linux-container toepassingen](service-fabric-tutorial-create-container-images.md).
- Meer informatie over [Service Fabric en containers](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
