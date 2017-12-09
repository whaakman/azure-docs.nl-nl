---
title: Meer informatie over ondersteuningsopties voor Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric-clusterversies die worden ondersteund en koppelingen naar bestand ondersteuningstickets
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/22/2017
ms.author: pkc
ms.openlocfilehash: 0e4a2aa0ed7327a8ed19e9a716b0bd97abc71d5c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-service-fabric-support-options"></a>Opties voor Azure Service Fabric-ondersteuning

We hebben verschillende opties voor u ingesteld voor het leveren van de juiste ondersteuning voor uw Service Fabric-clusters die u op uw toepassing werkbelastingen worden uitgevoerd. Afhankelijk van het niveau van ondersteuning die nodig zijn en de ernst van het probleem, krijgt u de juiste opties kiest. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Productieproblemen melden of vraag ondersteuning voor Azure

Open een ticket voor de ondersteuning voor het melden van problemen op uw Service Fabric-cluster dat is geïmplementeerd in Azure, [in Azure portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) of [Microsoft support-portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

Meer informatie over:
 
- [Ondersteuning van Microsoft voor Azure](https://azure.microsoft.com/en-us/support/plans/?b=16.44).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Productieproblemen melden of vraag ondersteuning voor betaalde voor zelfstandige die service Fabric-clusters

Voor het melden van problemen voor uw Service Fabric-cluster geïmplementeerd lokaal of op andere clouds, opent u een ticket voor professionele ondersteuning op [Microsoft ondersteuning portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

Meer informatie over:

- [Professional-ondersteuning van Microsoft voor on-premises](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Rapport Azure Service Fabric-problemen 
We hebben ingesteld om een GitHub-opslagplaats voor het melden van Service Fabric-problemen.  We zijn ook actief bewaken van de volgende forums.

### <a name="github-repo"></a>GitHub-repo- 
Problemen met de Azure Service Fabric een rapport over [problemen van een Service Fabric git-opslagplaats](https://github.com/Azure/service-fabric-issues). Deze opslagplaats is bedoeld voor rapportage en tracering van problemen met Azure Service Fabric en voor het maken van kleine functieaanvragen. **Gebruik dit niet live-site-problemen rapporteren**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow en MSDN-forums
De [Service Fabric-code op StackOverflow] [ stackoverflow] en de [Service Fabric-forum op MSDN] [ msdn-forum] beste worden gebruikt voor vragen over de werking van het platform en hoe u bepaalde taken ermee kunt uitvoeren.

### <a name="azure-feedback-forum"></a>Forum met Feedback van Azure
De [Forum met Feedback van Azure voor Service Fabric] [ uservoice-forum] is de beste plaats voor het indienen van grote functie ideeën die u hebt voor het product bekijken we de meest populaire aanvragen deel uitmaken van onze middelgrote tot planning op lange termijn. We raden u om een bijdrage ondersteuning voor uw suggesties binnen de community te.


## <a name="supported-service-fabric-versions"></a>Ondersteunde versies van de Service Fabric.

Zorg ervoor dat het cluster een ondersteunde versie van Service Fabric altijd wordt uitgevoerd. Als en dat we de release van een nieuwe versie van Service Fabric aankondigen, wordt de vorige versie gemarkeerd voor einde van ondersteuning na een minimum van 60 dagen na die datum. De nieuwe versies worden aangekondigd [op het Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/).

Raadpleeg de volgende documenten voor meer informatie over hoe u uw een ondersteunde versie van Service Fabric-cluster.

- [Upgrade van Service Fabric-versie op een Azure-cluster](service-fabric-cluster-upgrade.md)
- [Upgrade van Service Fabric-versie op een zelfstandige windows server-cluster](service-fabric-cluster-upgrade-windows-server.md)
 
Hier vindt u de lijst met de Service Fabric-versies die worden ondersteund en de einddatum van de ondersteuning.

| **Service Fabric-runtime in het cluster** | **Kan rechtstreeks upgraden van cluster versie** |**Compatibel SDK / NuGet-pakket versies** | **Einde van ondersteuning datum** |
| --- | --- |--- | --- |
| Alle clusterversies voordat 5.3.121 | 5.1.158* |Kleiner dan of gelijk zijn aan versie 2.3 |20 januari 2017 |
| 5.3.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.3 |24 februari 2017 |
| 5.4.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.4 |Kan 10,2017       |
| 5.5.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.5 |Augustus 10,2017    |
| 5.6.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.6 |Oktober 13,2017   |
| 5.7.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.7 |December 15,2017  |
| 6.0.* | 5.6.205.* |Kleiner dan of gelijk zijn aan versie 2.8 |Huidige versie en dus geen einddatum |

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric-Preview versies - wordt niet ondersteund voor gebruik in productieomgevingen.
Van tijd tot tijd brengen we versies die we feedback willen, belangrijke functies die worden uitgebracht als voorbeelden hebben. Deze preview-versies mag alleen worden gebruikt voor testdoeleinden. Uw productiecluster moet altijd een ondersteunde, stabiele, Service Fabric-versie worden uitgevoerd. Een preview-versie begint altijd met een primaire en secundaire versienummer van 255. Bijvoorbeeld, als er een Service Fabric versie 255.255.5703.949, die versie alleen moet worden gebruikt in testclusters en is Preview-versie. Deze preview-versies worden ook vermeld in de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric) en details hebben op de functies die worden.

Er is geen ondersteuningsoptie betaalde voor deze preview-versies. Gebruik een van de opties die worden vermeld onder [rapport Azure Service Fabric problemen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) vragen of feedback geven.

## <a name="next-steps"></a>Volgende stappen

- [Upgrade van service fabric-versie op een Azure-cluster](service-fabric-cluster-upgrade.md)
- [Upgrade van Service Fabric-versie op een zelfstandige windows server-cluster](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
