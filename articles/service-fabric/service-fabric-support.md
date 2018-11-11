---
title: Meer informatie over ondersteuningsopties voor Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric-clusterversies die worden ondersteund en koppelingen naar bestand ondersteuningstickets
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: 5d2edd3b4d8655133ef2835f0d0e579280123ae7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249418"
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric-ondersteuningsopties

Met het oog op de juiste ondersteuning voor uw Service Fabric-clusters die u op de werkbelastingen van uw toepassing worden uitgevoerd, hebben we verschillende opties voor u ingesteld. Afhankelijk van het niveau van ondersteuning die nodig zijn en de ernst van het probleem, krijgt u de juiste opties kiezen. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Productieproblemen melden of betaalde ondersteuning aanvragen voor Azure

Open een ticket voor ondersteuning voor het rapporteren van problemen met uw Service Fabric-cluster geïmplementeerd in Azure, [in Azure portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) of [Microsoft support portal](https://support.microsoft.com/oas/default.aspx?prid=16146).

Meer informatie over:
 
- [Ondersteuning van Microsoft voor Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Productieproblemen melden of betaalde ondersteuning aanvragen voor zelfstandige die service Fabric-clusters

Voor het melden van on-premises problemen op uw Service Fabric-cluster geïmplementeerd, of op andere clouds, opent u een ticket voor professionele ondersteuning op [Microsoft support portal](https://support.microsoft.com/oas/default.aspx?prid=16146).

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

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric Preview-versies - wordt niet ondersteund voor gebruik in productieomgevingen

Van tijd tot tijd release we-versies die we willen dat feedback op belangrijke functies die worden uitgebracht als Preview-versies. Deze preview-versies moeten alleen worden gebruikt voor testdoeleinden. Uw productiecluster moet altijd een ondersteunde, stabiel, Service Fabric-versie worden uitgevoerd. Een preview-versie begint altijd met een primaire en secundaire versienummer van 255. Bijvoorbeeld, als u een Service Fabric versie 255.255.5703.949 ziet, die versie is alleen moet worden gebruikt in testclusters en is beschikbaar als preview. Deze preview-versies worden ook aangekondigd op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric) en bevatten de details voor de functies die zijn opgenomen.
Er bestaat geen ondersteuningsoptie betaalde voor deze preview-versies. Gebruik een van de opties die worden weergegeven onder [rapport Azure Service Fabric problemen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) vragen of feedback geven.

## <a name="next-steps"></a>Volgende stappen

[Ondersteunde versies van de Service Fabric](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples