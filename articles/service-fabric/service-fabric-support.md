---
title: Meer informatie over ondersteuningsopties voor Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric-clusterversies die worden ondersteund en koppelingen naar bestand ondersteuningstickets
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/13/2018
ms.author: pkc
ms.openlocfilehash: 596e71be75453874492aac15d91cb6153c2076f5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112887"
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric-ondersteuningsopties

Met het oog op de juiste ondersteuning voor uw Service Fabric-clusters die u op de werkbelastingen van uw toepassing worden uitgevoerd, hebben we verschillende opties voor u ingesteld. Afhankelijk van het niveau van ondersteuning die nodig zijn en de ernst van het probleem, krijgt u de juiste opties kiezen. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Productieproblemen melden of betaalde ondersteuning aanvragen voor Azure

Open een ticket voor ondersteuning voor het rapporteren van problemen met uw Service Fabric-cluster geïmplementeerd in Azure, [in Azure portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) of [Microsoft support portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

Meer informatie over:
 
- [Ondersteuning van Microsoft voor Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Productieproblemen melden of betaalde ondersteuning aanvragen voor zelfstandige die service Fabric-clusters

Voor het melden van on-premises problemen op uw Service Fabric-cluster geïmplementeerd, of op andere clouds, opent u een ticket voor professionele ondersteuning op [Microsoft support portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

Meer informatie over:

- [Professionele ondersteuning van Microsoft voor on-premises](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Rapport Azure Service Fabric-problemen 
We hebben ingesteld om een GitHub-opslagplaats voor het melden van Service Fabric-problemen.  We zijn de volgende forums ook actief te controleren.

### <a name="github-repo"></a>GitHub-opslagplaats 
Problemen met Azure Service Fabric een rapport over [git-opslagplaats voor Service-Fabric-problemen](https://github.com/Azure/service-fabric-issues). Deze opslagplaats is bedoeld voor rapportage en het bijhouden van problemen met Azure Service Fabric en voor het aanbrengen van kleine functie-aanvragen. **Gebruik dit niet live-site om problemen te melden**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow en MSDN-forums
De [Service Fabric-code op StackOverflow] [ stackoverflow] en de [Service Fabric-forum op MSDN] [ msdn-forum] worden gebruikt voor het stellen van vragen over hoe u de beste platform werkt en hoe u bepaalde taken ermee kunt uitvoeren.

### <a name="azure-feedback-forum"></a>Azure-Feedbackforum
De [Forum met Feedback van Azure voor Service Fabric] [ uservoice-forum] is de beste plaats voor het indienen van grote functie ideeën voor het product hebt als we de meest populaire aanvragen bekijken, maken deel uit van onze middelgrote tot planning op lange termijn. We raden u om een bijdrage ondersteuning voor uw suggesties binnen de community te.


## <a name="supported-service-fabric-versions"></a>Ondersteunde versies van de Service Fabric.

Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Service Fabric. Als en wanneer we kondigen wij de release van een nieuwe versie van Service Fabric, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf die datum is gemarkeerd voor einde van ondersteuning. De nieuwe releases worden aangekondigd [op de blog van het Service Fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/).

Raadpleeg de volgende documenten voor meer informatie over het behouden van uw cluster met een ondersteunde versie van Service Fabric.

- [Service Fabric-versie op een Azure-cluster upgraden ](service-fabric-cluster-upgrade.md)
- [Service Fabric-versie op een zelfstandige windows server-cluster upgraden ](service-fabric-cluster-upgrade-windows-server.md)
 
Hier volgen de lijst met de Service Fabric-versies die worden ondersteund en de einddatum van ondersteuning.

| **Service Fabric-runtime in het cluster** | **Rechtstreeks vanuit de clusterversie kunt upgraden** |**Compatibel SDK / NuGet-pakket versies** | **Einde van ondersteuning voor datum** |
| --- | --- |--- | --- |
| Alle cluster versies vóór 5.3.121 | 5.1.158* |Kleiner dan of gelijk zijn aan versie 2.3 |20 januari 2017 |
| 5.3.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.3 |24 februari 2017 |
| 5.4.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.4 |Kan 10,2017       |
| 5.5.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.5 |Augustus 10,2017    |
| 5.6.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.6 |Oktober 13,2017   |
| 5.7.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.7 |December 15,2017  |
| 6.0.* | 5.6.205.* |Kleiner dan of gelijk zijn aan versie 2.8 |30,2018 maart     | 
| 6.1.* | 5.7.221.* |Kleiner dan of gelijk zijn aan versie 3.0 |Juli 15,2018      |
| 6.2. * | 6.0.232.* |Kleiner dan of gelijk zijn aan versie 3.1 |September 15,2018 |
| 6.3. * | 6.1.480.* |Kleiner dan of gelijk zijn aan versie 3.2 |Huidige versie en dus geen einddatum |

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric Preview-versies - wordt niet ondersteund voor gebruik in productieomgevingen.
Van tijd tot tijd release we-versies die we willen dat feedback op belangrijke functies die worden uitgebracht als Preview-versies. Deze preview-versies moeten alleen worden gebruikt voor testdoeleinden. Uw productiecluster moet altijd een ondersteunde, stabiel, Service Fabric-versie worden uitgevoerd. Een preview-versie begint altijd met een primaire en secundaire versienummer van 255. Bijvoorbeeld, als u een Service Fabric versie 255.255.5703.949 ziet, die versie is alleen moet worden gebruikt in testclusters en is beschikbaar als preview. Deze preview-versies worden ook aangekondigd op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric) en bevatten de details voor de functies die zijn opgenomen.

Er bestaat geen ondersteuningsoptie betaalde voor deze preview-versies. Gebruik een van de opties die worden weergegeven onder [rapport Azure Service Fabric problemen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) vragen of feedback geven.

## <a name="next-steps"></a>Volgende stappen

- [Service fabric-versie op een Azure-cluster upgraden ](service-fabric-cluster-upgrade.md)
- [Service Fabric-versie op een zelfstandige windows server-cluster upgraden ](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
