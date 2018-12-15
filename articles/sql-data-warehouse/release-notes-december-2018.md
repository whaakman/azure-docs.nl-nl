---
title: Azure SQL Data Warehouse opmerkingen bij de Release van December 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 21baa89293c74ec49720bffc2506e20789fe9e55
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410996"
---
# <a name="whats-new-in-azure-sql-data-warehouse-december-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? December 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in December 2018.

## <a name="virtual-network-service-endpoints-generally-available"></a>Virtual Network-Service-eindpunten zijn algemeen beschikbaar
Deze release bevat de algemene beschikbaarheid van Service-eindpunten voor Virtueelnetwerk (VNet) voor Azure SQL Data Warehouse in alle Azure-regio's. VNet-Service-eindpunten kunt u verbinding met de logische server van een bepaald subnet of een set van subnetten in het virtuele netwerk te isoleren. Het verkeer van uw VNet naar Azure SQL Data Warehouse blijft altijd in het Azure-backbone-netwerk. Deze rechtstreekse route worden voorkeur boven alle routes die specifieke die internetverkeer via on-premises of virtuele apparaten. Er zijn geen extra facturering wordt in rekening gebracht voor toegang tot het virtuele netwerk via service-eindpunten. Huidige prijsmodel voor [Azure SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/) is van toepassing.

Met deze release, we ook PolyBase-connectiviteit met ingeschakeld [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (ADLS) via [Azure Blob-bestandssysteem](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-abfs-driver) (ABFS) stuurprogramma. Azure Data Lake Storage Gen2 brengt alle kenmerken die vereist voor de volledige levenscyclus van analytics-gegevens naar Azure Storage zijn. Functies van de twee bestaande Azure storage-services, Azure Blob Storage en Azure Data Lake Storage Gen1 worden geconvergeerd. Functies van [Azure Data Lake Storage Gen1](https://docs.microsoft.com/azure/data-lake-store/index), zoals de semantiek van het bestandssysteem, op bestandsniveau beveiliging en schaal worden gecombineerd met lage kosten, gelaagde opslag- en herstelfuncties voor hoge beschikbaarheid/noodherstel van [ Azure Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). 

Met Polybase kunt u ook gegevens importeren in Azure SQL Data Warehouse uit Azure Storage is beveiligd met VNet. Exporteren van gegevens uit Azure SQL Data Warehouse naar Azure Storage is beveiligd met VNet wordt op dezelfde manier ook ondersteund via Polybase. 

Raadpleeg voor meer informatie over VNet-Service-eindpunten in Azure SQL Data Warehouse de [blogbericht](https://azure.microsoft.com/blog/general-availability-of-vnet-service-endpoints-for-azure-sql-data-warehouse/) of de [documentatie](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json).

## <a name="automatic-performance-monitoring-preview"></a>Automatische prestatiebewaking (Preview)
[Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017) is nu beschikbaar in Preview-versie van Azure SQL Data Warehouse. Query Store is ontworpen voor hulp bij het oplossen van problemen door bij te houden van query's, queryplannen, runtime-statistieken en de geschiedenis van de query om u te helpen bij het controleren van de activiteit en prestaties van uw datawarehouse prestaties van query's. Query Store is een verzameling van interne stores en dynamische beheerweergaven (DMV's), waarmee u kunt:

- Identificeren en bovenste die veel resources verbruiken van query's af te stemmen
- Identificeren en ad hoc-workloads te verbeteren
- Prestaties van query's en van invloed op het plan evalueren door wijzigingen in statistieken of indexen grootte (DWU-instelling)
- Zie de tekst van de volledige query voor alle query's uitgevoerd

De Query Store bevat drie werkelijke stores:  
- Een archief van het plan voor het opslaan van de plangegevens uitvoering 
- Een archief van de runtime-statistieken voor het opslaan van de uitvoering van statistische gegevens
- Een ogenblik geduld statistieken store voor het opslaan van wacht statistieken informatie. 

SQL Data Warehouse deze archieven automatisch beheert en een onbeperkt aantal query's in de afgelopen zeven dagen zonder extra kosten storied bieden. Het inschakelen van Query Store is zo eenvoudig als het uitvoeren van een instructie ALTER DATABASE T-SQL:

```sql
ALTER DATABASE [Database Name] SET QUERY_STORE = ON;
```
Zie het artikel voor meer informatie over Query Store in Azure SQL Data Warehouse [prestaties controleren via de Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017), en de Query Store DMV's, zoals [sys.query_store_query](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-transact-sql?view=sql-server-2017). Hier volgt de [blogbericht](https://azure.microsoft.com/blog/automatic-performance-monitoring-in-azure-sql-data-warehouse-with-query-store/) aankondiging van de release.

## <a name="lower-compute-tiers-for-azure-sql-data-warehouse-gen2"></a>Lagere Rekenlagen voor Azure SQL datawarehouse Gen2
Lagere rekenlagen biedt nu ondersteuning voor Azure SQL Data Warehouse Gen2. Klanten kunnen ondervinden Azure SQL Data Warehouse van toonaangevende prestaties, flexibiliteit en beveiligingsfuncties die beginnen met 100 cDWU ([Data Warehouse Units](https://docs.microsoft.com/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu)) en de schaal aan 30.000 cDWU in minuten. Vanaf half December 2018, klanten kunnen profiteren van Gen2 prestaties en flexibiliteit in combinatie met lagere lagen in compute [regio's](https://docs.microsoft.com/azure/sql-data-warehouse/gen2-lower-tier-regions), met de rest van de regio's beschikbaar zijn tijdens de 2019.

Door slepen en neerzetten het toegangspunt voor datawarehouses bij een volgende generatie, geeft Microsoft de mogelijkheid op waarde gebaseerde klanten die willen evalueren van alle voordelen van een veilige, krachtige datawarehouse zonder te raden welke evaluatieomgeving voor hen het beste is. Klanten kunnen beginnen slechts 100 cDWU omlaag vanaf het huidige 500 cDWU-beginpunt. SQL Data Warehouse Gen2 blijft ondersteuning voor onderbreken en hervatten van bewerkingen en inhoudt dan alleen de flexibiliteit in compute. Gen2 biedt ook ondersteuning voor columnstore is onbeperkte opslagcapaciteit samen met 2,5 keer zoveel geheugen per query maximaal 128 gelijktijdige query's en [adaptieve cache-opslag](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/) functies. Deze functies op gemiddelde Breng vijf keer betere prestaties in vergelijking met de dezelfde Data Warehouse Unit op Gen1 tegen dezelfde prijs. Geografisch redundante back-ups worden standaard voor Gen2 met ingebouwde gegarandeerde gegevensbeveiliging. Azure SQL Data Warehouse Gen2 is gereed om te schalen wanneer u bent.

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, vindt u de [Azure-woordenlijst] [ Azure glossary] handig zijn bij het leren van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
