---
title: Azure Storage analytics gebruiken om gegevens in Logboeken en metrische gegevens te verzamelen | Microsoft Docs
description: Storage Analytics kunt u voor het bijhouden van metrische gegevens voor alle storage-services en het verzamelen van Logboeken voor Blob, Queue en Table storage.
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: eb85f8c756e7373a8dedabbce362cfa534e56fd8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849173"
---
# <a name="storage-analytics"></a>Storage Analytics

Met Azure Storage Analytics wordt logboekregistratie uitgevoerd en kunnen metrische gegevens voor een opslagaccount worden geleverd. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt dit inschakelen via de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [een opslagaccount in Azure portal controleren](storage-monitor-storage-account.md). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de [Blob Service-eigenschappen instellen](/rest/api/storageservices/set-blob-service-properties), [eigenschappen van de Queue-Service instellen](/rest/api/storageservices/set-queue-service-properties), [tabel-Service-eigenschappen instellen](/rest/api/storageservices/set-table-service-properties), en [ingesteld File Service Properties](/rest/api/storageservices/Get-File-Service-Properties)bewerkingen voor het inschakelen van Storage Analytics voor elke service.

De samengevoegde gegevens worden opgeslagen in een bekende blob (voor logboekregistratie) en in bekende tabellen (voor metrische gegevens), die kunnen worden geopend met behulp van de Blob-service en service-tabel API's.

Storage Analytics heeft een limiet van 20 TB voor de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw storage-account. Zie voor meer informatie over limieten voor opslagaccounts [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).

Zie voor een uitgebreide handleiding over het gebruik van Storage Analytics en andere hulpprogramma's om te identificeren, onderzoeken en oplossen van problemen met betrekking tot Azure Storage, [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="billing-for-storage-analytics"></a>Facturering voor Storage Analytics

Alle metrische gegevens is geschreven door de services van een storage-account. Als gevolg hiervan wordt elke schrijfbewerking is uitgevoerd door Storage Analytics gefactureerd. Daarnaast is de hoeveelheid opslag die wordt gebruikt door metrische gegevens ook factureerbaar.

De volgende acties uitgevoerd door Storage Analytics zijn factureerbaar:

* Aanvragen voor het maken van blobs voor logboekregistratie.
* Aanvragen voor de tabelentiteiten voor metrische gegevens maken.

Als u een beleid voor Gegevensretentie hebt geconfigureerd, u betaalt geen voor verwijdertransacties wanneer Storage Analytics oude gegevens voor logboekregistratie en metrische gegevens worden verwijderd. Verwijdertransacties van een client zijn echter factureerbaar. Zie voor meer informatie over het bewaarbeleid [instellen van een bewaarbeleid voor gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Understanding factureerbare aanvragen

Elk verzoek aan de opslag van een serviceaccount is factureerbare of niet-factureerbaar. Opslaganalyse-logboeken elke afzonderlijke aanvraag aan een service, inclusief een statusbericht dat geeft aan hoe de aanvraag is verwerkt. Op deze manier in Storage Analytics metrische gegevens voor zowel een service en de API-bewerkingen van die service, inclusief de percentages en de telling van bepaalde statusberichten worden opgeslagen. Samen vormen kunt deze functies u analyseren van uw factureerbare aanvragen, breng verbeteringen aan op uw toepassing en onderzoeken van problemen met aanvragen voor uw services. Zie voor meer informatie over facturering [inzicht in Azure Storage Billing - bandbreedte, transacties en capaciteit](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Wanneer Storage Analytics-gegevens bekijkt, kunt u de tabellen in de [Storage Analytics geregistreerde bewerkingen en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) onderwerp om te bepalen welke aanvragen zijn factureerbaar. U kunt vervolgens uw logboeken en metrische gegevens om de statusberichten om te zien als u in de rekening zijn gebracht voor een bepaalde aanvraag te vergelijken. U kunt ook de tabellen in het vorige onderwerp gebruiken voor het onderzoeken van de beschikbaarheid van een storage-service of een afzonderlijke API-bewerking.

## <a name="next-steps"></a>Volgende stappen
* [Een opslagaccount in Azure portal controleren](storage-monitor-storage-account.md)
* [Storage Analytics Metrics](storage-analytics-metrics.md)
* [Logboekregistratie van opslag Analytics](storage-analytics-logging.md)
