---
title: Hoge beschikbaarheid en betrouwbaarheid
description: Hoge beschikbaarheid en betrouwbaarheid
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 7e7fe49de7814b6058468d630f8638720e5864f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>Hoge beschikbaarheid en betrouwbaarheid
## <a name="azure-scheduler-high-availability"></a>Hoge beschikbaarheid met Azure Scheduler
Als een Azure-platform-service core Azure Scheduler is maximaal beschikbaar en functionaliteit voor zowel geografisch redundante service-implementatie en geo-landinstelling taak replicatie.

### <a name="geo-redundant-service-deployment"></a>Geografisch redundante service-implementatie
Azure Scheduler is beschikbaar via de gebruikersinterface in bijna elke geografische regio die deel uitmaakt van Azure vandaag. De lijst met regio's die beschikbaar is in Azure Scheduler is [hier vermeld](https://azure.microsoft.com/regions/#services). Als een datacentrum in een gehoste regio niet beschikbaar wordt weergegeven, zijn de failover-mogelijkheden van Azure Scheduler zodat de service beschikbaar vanuit een ander datacenter is.

### <a name="geo-regional-job-replication"></a>Geo-landinstelling taak replicatie
Is niet alleen de Azure Scheduler front-end beschikbaar voor de management-aanvragen, maar uw eigen baan is ook geo-replicatie. Wanneer er een storing in een regio, wordt Azure Scheduler failover wordt uitgevoerd en zorgt ervoor dat de taak wordt uitgevoerd vanaf een ander datacenter in gekoppelde geografische regio.

Bijvoorbeeld, als u een taak hebt gemaakt in Zuid-centraal VS, repliceert Azure Scheduler automatisch deze taak in Noordelijk Centraal, VS. Wanneer er een fout in Zuid-centraal VS, Azure Scheduler zorgt ervoor dat de taak wordt uitgevoerd vanaf Noordelijk Centraal, VS. 

![][1]

Azure Scheduler als gevolg hiervan zorgt ervoor dat uw gegevens binnen dezelfde breder geografische regio in geval van een Azure storing blijft. Als gevolg hiervan moet u de taak voor het toevoegen van hoge beschikbaarheid dubbele – Azure Scheduler automatisch biedt mogelijkheden voor hoge beschikbaarheid voor uw taken.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler-betrouwbaarheid
Azure Scheduler wordt gegarandeerd dat een eigen hoge beschikbaarheid en gaat een andere benadering naar taken gebruiker gemaakt. De taak kan bijvoorbeeld een HTTP-eindpunt is niet beschikbaar aanroepen. Azure Scheduler probeert niettemin niet goed worden uitgevoerd uw taak, doordat u alternatieve opties om te gaan met de fout. Azure Scheduler wordt dit op twee manieren:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Configureerbare beleid voor opnieuw proberen via "retryPolicy"
Azure Scheduler kunt u een beleid voor opnieuw proberen te configureren. Standaard, als een taak mislukt, probeert Scheduler de taak opnieuw vier keer, met een interval van 30 seconden. U kunt dit beleid voor opnieuw proberen om te worden agressievere (bijvoorbeeld tien keer met een interval van 30 seconden) opnieuw configureren of lossere (bijvoorbeeld twee keer per dag intervallen.)

Als een voorbeeld van wanneer Hiermee kunt kunt u een taak die eenmaal per week wordt uitgevoerd en roept een HTTP-eindpunt maken. Als het HTTP-eindpunt is niet actief voor een paar uur wanneer de taak wordt uitgevoerd is, u mogelijk niet wilt wachten één meer week voor de taak opnieuw uit te voeren omdat zelfs het standaardbeleid voor opnieuw proberen mislukken. In dergelijke gevallen kunt u het beleid standaardproces voor nieuwe pogingen om opnieuw te proberen om de drie uur (bijvoorbeeld) opnieuw configureren in plaats van elke 30 seconden.

Raadpleeg voor meer informatie over het configureren van een beleid voor opnieuw proberen, [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatieve eindpunt configuratiemogelijkheden via "errorAction"
Als het doel-eindpunt voor uw Azure Scheduler-taak niet bereikbaar blijft, schakelt Azure Scheduler terug naar het andere eindpunt voor foutafhandeling nadat u het beleid voor opnieuw proberen. Als een alternatieve eindpunt voor foutafhandeling is geconfigureerd, wordt het door Azure Scheduler aanroept. Uw eigen taken zijn met een alternatieve eindpunt maximaal beschikbaar met betrekking tot de fout.

Azure Scheduler volgt een voorbeeld: in het onderstaande diagram het beleid voor opnieuw proberen voor een webservice Den Haag bereikt. Nadat de nieuwe pogingen mislukken, wordt gecontroleerd of er een alternatief is. Vervolgens gaat het verder en wordt gestart die aanvragen indienen bij de alternatieve met de dezelfde beleid voor opnieuw proberen.

![][2]

Houd er rekening mee dat de dezelfde beleid voor opnieuw proberen voor zowel de oorspronkelijke actie en de alternatieve fout geldt. Het is ook mogelijk om de alternatieve foutbewerking actietype afwijken van de belangrijkste actie action-type. Bijvoorbeeld, terwijl de belangrijkste actie wordt een HTTP-eindpunt kan worden aangeroepen, de actie bij fout in plaats daarvan mogelijk een opslagwachtrij-, service bus-wachtrij- of service bus-onderwerpactie die foutregistratie biedt.

Raadpleeg voor meer informatie over het configureren van een eindpunt alternate, [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Complexe schema's en geavanceerde terugkeerpatronen bouwen met Azure Scheduler](scheduler-advanced-complexity.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
