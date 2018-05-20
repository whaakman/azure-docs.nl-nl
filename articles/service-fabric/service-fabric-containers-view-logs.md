---
title: Containers logboeken weergeven in Azure Service Fabric | Microsoft Docs
description: Beschrijft hoe om Logboeken van de container voor een actieve Service Fabric-containerservices met behulp van Service Fabric Explorer weer te geven.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2018
ms.author: ryanwi
ms.openlocfilehash: b2b3562f65e7e861b7e4dff7b7c26d58081ff29e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Logboeken bekijken voor een Service Fabric-containerservice
Azure Service Fabric is een container orchestrator en ondersteunt zowel [Linux- en Windows-containers](service-fabric-containers-overview.md).  Dit artikel wordt beschreven hoe u logboeken van de container van een actieve containerservice of een inactieve container weergeven, zodat u kunt onderzoeken en oplossen van problemen.

## <a name="access-the-logs-of-a-running-container"></a>Toegang tot de logboeken van een actieve container
Container logboeken zijn toegankelijk via [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Open in een webbrowser, Service Fabric Explorer van het cluster management eindpunt door te navigeren naar [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Container logboeken bevinden zich op het clusterknooppunt dat op de container service-exemplaar wordt uitgevoerd. Een voorbeeld: ophalen van de logboeken van de web-front-container van de [voorbeeldtoepassing Linux stemmen](service-fabric-quickstart-containers-linux.md). Vouw in de structuurweergave **Cluster**>**toepassingen**>**VotingType**>**fabric: / Voting / azurevotefront**.  Vervolgens vouwt u de partitie (d1aa737e-f22a-e347-be16-eec90be24bc1, in dit voorbeeld) en of de container wordt uitgevoerd op het clusterknooppunt *_lnxvm_0*.

In de structuurweergave, kunt u de codepakket-zoeken op de *_lnxvm_0* knooppunt door het uitbreiden van **knooppunten**>**_lnxvm_0**>**fabric: / stemmen**  > **azurevotfrontPkg**>**Code pakketten**>**code**.  Selecteer vervolgens de **Container logboeken** optie voor het weergeven van de logboeken van de container.

![Service Fabric-platform][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Toegang tot de logboeken van een container dode of vastgelopen
Vanaf v6.2, u kunt ook ophalen van de logboeken voor een inactieve of gecrashte container met [REST-API's](/rest/api/servicefabric/sfclient-index) of [Service Fabric CLI (SFCTL)](service-fabric-cli.md) opdrachten.

### <a name="rest"></a>REST
Gebruik de [ophalen Container logboeken geïmplementeerd op knooppunt](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) bewerking voor het verkrijgen van de logboeken voor een vastgelopen container. Geef de naam van het knooppunt dat de container werd uitgevoerd op, toepassingsnaam, servicenaam manifest en de codenaam van het pakket.  Geef `&Previous=true`. Het antwoord bevat de logboeken van de container voor de inactieve container van het pakket code exemplaar.

De aanvraag-URI heeft de volgende notatie:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Voorbeeld van de aanvraag:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

200 antwoordtekst:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Service Fabric (SFCTL)
Gebruik de [sfctl get-container-servicelogboeken](service-fabric-sfctl-service.md) opdracht voor het ophalen van de logboeken voor een vastgelopen container.  Geef de naam van het knooppunt dat de container werd uitgevoerd op, toepassingsnaam, servicenaam manifest en de codenaam van het pakket. Geef de `-previous` vlag.  Het antwoord bevat de logboeken van de container voor de inactieve container van het pakket code exemplaar.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –previous
```
Antwoord:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Volgende stappen
- Doorloop de [maken van een zelfstudie over Linux-container](service-fabric-tutorial-create-container-images.md).
- Meer informatie over [Service Fabric en containers](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
