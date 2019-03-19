---
title: Uw gegevens migreren naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw gegevens naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 6a2acf602252ee4319f9a5eccef53a25d8e2dd7f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58014263"
---
# <a name="migrate-your-data"></a>Uw gegevens migreren
Gegevens kunnen worden verplaatst uit verschillende bronnen in uw SQL Data Warehouse met een verschillende hulpprogramma's.  ADF kopiëren, SSIS en bcp kunnen alles worden gebruikt om dit doel te bereiken. Echter, als de hoeveelheid gegevens toeneemt moet u bedenken afbreken van de procedure voor het migreren van gegevens in stappen. Dit biedt u de mogelijkheid om elke stap voor prestaties en flexibiliteit om te controleren of een soepele migratie te optimaliseren.

Dit artikel worden eerst de eenvoudige migratiescenario's van ADF kopiëren, SSIS en bcp. Deze vervolgens kijken meer wilt weten hoe de migratie kan worden geoptimaliseerd.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADF) kopiëren
[ADF-kopie] [ ADF Copy] maakt deel uit van [Azure Data Factory][Azure Data Factory]. ADF-exemplaar kunt u uw gegevens te exporteren naar platte bestanden op de lokale opslag, naar externe platte bestanden die zijn ondergebracht in Azure blob-opslag of rechtstreeks in SQL Data Warehouse.

Als uw gegevens wordt gestart in platte bestanden, dan u eerst moet voordat u begint een belasting overzetten naar Azure storage-blob naar SQL Data Warehouse. Zodra de gegevens worden overgebracht naar Azure blob-opslag kunt u gebruiken [ADF kopie] [ ADF Copy] opnieuw om de gegevens in SQL Data Warehouse.

PolyBase biedt ook een optie voor hoge prestaties voor het laden van de gegevens. Dat betekent echter twee hulpprogramma's gebruiken in plaats van een. Als u de beste prestaties moet gebruik van PolyBase. Als u wilt dat een enkele hulpprogramma-ervaring (en de gegevens niet enorme zijn) is ADF uw antwoord.

Ga als volgt [in deze zelfstudie](../data-factory/load-azure-sql-data-warehouse.md) voor informatie over het gebruik van ADF om gegevens te laden in uw datawarehouse.

## <a name="integration-services"></a>Integratieservices
Integration Services (SSIS) is een krachtige en flexibele extraheren transformeren en laden (ETL) hulpprogramma die ondersteuning biedt voor complexe werkstromen, transformatie en verschillende opties voor het laden van gegevens. SSIS gebruiken voor gegevens over te dragen naar Azure of als onderdeel van een bredere migratie.

> [!NOTE]
> SSIS kunt exporteren naar UTF-8 zonder de bytevolgordemarkering in het bestand. Voor het configureren van dit moet u eerst het afgeleide kolom-onderdeel gebruiken het tekengegevens in de gegevensstroom gebruiken de 65001 UTF-8-codetabel te converteren. Zodra de kolommen zijn geconverteerd, kunt u de gegevens schrijven naar de doel-adapter van plat bestand ervoor te zorgen dat 65001 ook is geselecteerd als de pagina voor het bestand.
> 
> 

SSIS maakt verbinding met SQL Data Warehouse, net zoals voor de implementatie van een SQL Server waarmee verbinding wordt gemaakt. Uw verbindingen moet echter een ADO.NET-Verbindingsbeheer gebruiken. U moet ook oplossen het configureren van de 'Gebruik bulksgewijs invoegen wanneer deze beschikbaar' instellen voor een maximale doorvoer. Raadpleeg de [ADO.NET-doeladapter] [ ADO.NET destination adapter] artikel voor meer informatie over deze eigenschap

> [!NOTE]
> Verbinding maken met Azure SQL Data Warehouse via OLEDB wordt niet ondersteund.
> 
> 

Er is bovendien altijd de kans dat een pakket vanwege problemen met beperking of netwerk mislukken. Ontwerp pakketten, zodat ze kunnen worden hervat moment van de fout, zonder opnieuw uitvoeren, werken die vóór de fout is voltooid.

Raadpleeg voor meer informatie de [SSIS-documentatie][SSIS documentation].

## <a name="bcp"></a>BCP
BCP is een opdrachtregelprogramma dat is ontworpen voor plat bestand gegevens importeren en exporteren. Sommige transformatie kan plaatsvinden tijdens het exporteren van gegevens. Eenvoudige transformaties gebruik om uit te voeren een query voor het selecteren en de gegevens te transformeren. Nadat u hebt geëxporteerd, kunnen de platte bestanden vervolgens worden geladen rechtstreeks in het doel de SQL Data Warehouse-database.

> [!NOTE]
> Het is vaak een goed idee om in te kapselen de transformaties die tijdens het exporteren van gegevens in een weergave in het bronsysteem wordt gebruikt. Dit zorgt ervoor dat de logica worden bewaard en het proces herhaald wordt.
> 
> 

Voordelen van bcp zijn:

* Eenvoud. BCP-opdrachten zijn eenvoudig te bouwen en uitvoeren
* Laadproces opnieuw meer gestart. Eenmaal geëxporteerde de belasting mag een willekeurig aantal keren uitgevoerd

Beperkingen van bcp zijn:

* BCP werkt met alleen gebruikmaking platte bestanden. Werkt niet met, zoals xml of JSON-bestanden
* Gegevenstransformatie mogelijkheden zijn beperkt tot alleen de export-fase en kunnen eenvoudig in de natuur
* BCP is niet aangepast aan de robuuste worden bij het laden van gegevens via internet. Alle instabiliteit van het netwerk kan ertoe leiden dat een fout bij het laden.
* BCP is afhankelijk van het schema aanwezig is in de doeldatabase voorafgaand aan de belasting

Zie voor meer informatie, [bcp gebruiken om gegevens te laden in SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>De gegevensmigratie te optimaliseren
Een proces gegevensmigratie RESOURCEKLASSE kan effectief worden onderverdeeld in drie afzonderlijke stappen:

1. Exporteren van brongegevens
2. Overdracht van gegevens naar Azure
3. In de doeldatabase RESOURCEKLASSE laden

Elke stap kan afzonderlijk worden geoptimaliseerd voor het maken van een robuuste, opnieuw meer gestart en flexibele migratieproces die optimaliseert de prestaties bij elke stap.

## <a name="optimizing-data-load"></a>Optimaliseren laden van gegevens
Kijken naar deze in omgekeerde volgorde voor de even; Er is de snelste manier om gegevens te laden met PolyBase. Optimaliseren voor een laadproces PolyBase plaatst vereisten op de voorgaande stappen, zodat het is raadzaam om te begrijpen dit vooraf. Dit zijn:

1. Codering van gegevensbestanden
2. Indeling van gegevensbestanden
3. Locatie van gegevensbestanden

### <a name="encoding"></a>Encoding
PolyBase is gegevensbestanden UTF-8-of UTF-16FE vereist. 



### <a name="format-of-data-files"></a>Indeling van gegevensbestanden
PolyBase mandaten een vaste rijeinde \n of nieuwe regel. De gegevensbestanden van uw moeten voldoen aan deze standaard. Er zijn geen beperkingen voor tekenreeks of een kolom afsluitingen.

U moet voor elke kolom in het bestand als onderdeel van uw externe tabel in PolyBase definiëren. Zorg ervoor dat alle geëxporteerde kolommen vereist zijn en dat de typen aan de vereiste standaarden voldoen.

Raadpleeg terug naar de [uw schema migreren] artikel voor informatie over ondersteunde gegevenstypen.

### <a name="location-of-data-files"></a>Locatie van gegevensbestanden
SQL Data Warehouse maakt gebruik van PolyBase om gegevens te laden uit Azure Blob Storage exclusief. Als gevolg daarvan kan moet de gegevens eerst overgedragen naar de blobopslag.

## <a name="optimizing-data-transfer"></a>Optimaliseren gegevensoverdracht
Een van de traagste onderdelen van de gegevensmigratie wordt de overdracht van de gegevens naar Azure. Niet alleen netwerkbandbreedte kan wel een probleem, maar ook de betrouwbaarheid van het netwerk kunt ernstig wordt verstoord wordt uitgevoerd. Is via internet, zodat de kans op fouten in de gegevensoverdracht plaatsvindt redelijkerwijs waarschijnlijk standaard gegevens migreren naar Azure. Deze fouten kunnen echter opnieuw worden verzonden geheel of gedeeltelijk vereist.

Gelukkig hebt u verschillende mogelijkheden voor het verbeteren van de snelheid en flexibiliteit van dit proces:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
U kunt overwegen [ExpressRoute] [ ExpressRoute] om de overdracht te versnellen. [ExpressRoute] [ ExpressRoute] vindt u een persoonlijke verbinding naar Azure, zodat de verbinding niet via het openbare internet loopt. Dit is geen een verplichte stap. Het wordt echter doorvoer verbeteren bij het pushen van gegevens naar Azure vanaf een on-premises of CO-locatiefaciliteit.

De voordelen van het gebruik van [ExpressRoute] [ ExpressRoute] zijn:

1. Verbeterde betrouwbaarheid
2. Snellere netwerksnelheid
3. Lagere netwerklatentie
4. hogere netwerkbeveiliging

[ExpressRoute] [ ExpressRoute] is het nuttig zijn voor een aantal scenario's, niet alleen de migratie.

Geïnteresseerd? Voor meer informatie en prijzen Ga naar de [documentatie voor ExpressRoute][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure Import- en Export-Service
De Azure importeren en exporteren-Service is een overdracht van de synchronisatiegegevens voor grote (GB ++) is ontworpen voor grote (TB ++) overdracht van gegevens naar Azure. Dit omvat het schrijven van uw gegevens op schijven en ze verzenden naar een Azure-Datacenter. Inhoud van de schijf wordt vervolgens uit uw naam in Azure Storage-Blobs worden geladen.

Een weergave op hoog niveau van het importproces uitvoer is als volgt:

1. Een Azure Blob Storage-container voor het ontvangen van de gegevens configureren
2. Uw gegevens worden geëxporteerd naar de lokale opslag
3. Kopieer de gegevens naar 3,5-inch SATA II/III harde schijven met behulp van de [Azure Import/Export-hulpprogramma]
4. Maken van een Import-taak met de Azure importeren en exporteren Service leveren van de logboekbestanden die worden geproduceerd door de [Azure Import/Export-hulpprogramma]
5. Stuur de schijven dan uw aangewezen Azure-Datacenter
6. Uw gegevens worden overgedragen naar uw Azure Blob Storage-container
7. De gegevens laden in RESOURCEKLASSE met PolyBase

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] hulpprogramma
De [AZCopy][AZCopy] hulpprogramma is een uitstekend hulpprogramma voor het ophalen van uw gegevens overgebracht naar Azure Storage-Blobs. Het is ontworpen voor kleine (MB ++) op zeer grote gegevensoverdrachten voor (GB ++). [AZCopy] is ook ontworpen voor goede flexibele doorvoer bij de overdracht van gegevens naar Azure en dit is een uitstekende keuze voor de stap van de overdracht van gegevens. Eenmaal overgebrachte kunt u de gegevens met PolyBase in SQL Data Warehouse laden. U kunt ook AZCopy opnemen in uw SSIS-pakketten met behulp van een 'Proces uitvoeren'-taak.

Voor het gebruik van AZCopy moet u eerst te downloaden en installeren. Er is een [productieversie] [ production version] en een [preview-versie] [ preview version] beschikbaar.

Als u wilt uploaden van een bestand van het bestandssysteem moet u een opdracht zoals hieronder wordt weergegeven:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Een samenvatting van het proces op hoog niveau kan worden:

1. Een Azure storage blob-container voor het ontvangen van de gegevens configureren
2. Uw gegevens worden geëxporteerd naar de lokale opslag
3. AZCopy uw gegevens in de Azure Blob Storage-container
4. De gegevens laden in SQL Data Warehouse met PolyBase

Volledige documentatie beschikbaar: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>Optimaliseren gegevens exporteren
Naast ervoor te zorgen dat de export aan de voorschriften met PolyBase voldoet kunt u ook zoeken naar de export van de gegevens voor het verbeteren van het proces verder optimaliseren.



### <a name="data-compression"></a>Gegevenscompressie
PolyBase kan gecomprimeerde gzip-gegevens lezen. Als u uw gegevens naar de gzip-bestanden te comprimeren zal u de hoeveelheid gegevens die via het netwerk worden gepusht minimaliseren.

### <a name="multiple-files"></a>Meerdere bestanden
Grote tabellen in verschillende bestanden worden opgesplitst niet alleen kunt u de uitvoer sneller, maar ook met overdracht re-startability en de algehele beheerbaarheid van de gegevens eenmaal in de Azure blob-opslag. Een van de vele leuke functies van PolyBase is dat wordt gelezen van alle bestanden in een map en deze als één tabel behandelen. Het is daarom een goed idee om te isoleren van de bestanden voor elke tabel in de eigen map.

PolyBase biedt ook ondersteuning voor een functie die bekend als 'recursieve map traversal'. U kunt deze functie gebruiken voor het verder verbeteren van de organisatie van de geëxporteerde gegevens voor het verbeteren van het beheer van uw gegevens.

Zie voor meer informatie over het laden van gegevens met PolyBase, [gebruik PolyBase om gegevens te laden in SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het migreren van [uw oplossing migreren naar SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/load-azure-sql-data-warehouse.md 
[ADF Copy examples]: ../data-factory/quickstart-create-data-factory-dot-net.md
[development overview]: sql-data-warehouse-overview-develop.md
[uw schema migreren]: sql-data-warehouse-migrate-schema.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: /sql/tools/bcp-utility
[Use PolyBase to load data into SQL Data Warehouse]: load-data-wideworldimportersdw.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: https://azure.microsoft.com/services/data-factory/
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: https://azure.microsoft.com/documentation/services/expressroute/

[production version]: https://aka.ms/downloadazcopy/
[preview version]: https://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
