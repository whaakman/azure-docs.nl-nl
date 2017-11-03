---
title: Uw gegevens te migreren naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw gegevens naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 0d156bc2eecf8220bd5ff4eb811d91482f216837
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-data"></a>Uw gegevens migreren
Gegevens kunnen worden verplaatst van verschillende bronnen in uw SQL Data Warehouse met een aantal verschillende hulpprogramma's.  ADF kopiëren, SSIS en bcp kunnen alle worden gebruikt om dit doel te bereiken. Echter, als de hoeveelheid gegevens toeneemt moet u bedenken splitsen van het migratieproces in stappen. Hierdoor kunnen u elke stap voor prestaties en herstelmogelijkheden bij een migratie smooth gegevens optimaliseren.

Dit artikel wordt eerst de eenvoudige migratiescenario's van ADF kopiëren, SSIS en bcp. Vervolgens uiterlijk enigszins dieper in hoe de migratie kan worden geoptimaliseerd.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADF) kopiëren
[Kopiëren van de ADF] [ ADF Copy] maakt deel uit van [Azure Data Factory][Azure Data Factory]. ADF kopiëren kunt u uw gegevens exporteren naar platte bestanden op de lokale opslag, naar externe platte bestanden die zijn ondergebracht in Azure blob-opslag of rechtstreeks in SQL Data Warehouse.

Als uw gegevens in platte bestanden begint en vervolgens u eerst moet overzetten naar Azure storage-blob voordat u begint een laden in SQL Data Warehouse. Nadat de gegevens worden overgedragen naar Azure blob-opslag kunt u gebruiken [ADF kopiëren] [ ADF Copy] opnieuw om de gegevens in SQL Data Warehouse.

PolyBase biedt ook een optie voor het laden van de gegevens voor hoge prestaties. Dat betekent dat twee hulpprogramma's voor gebruik in plaats van een. Als u de beste prestaties moet u PolyBase gebruiken. Als u een ervaring één hulpprogramma wilt (en de gegevens niet enorme zijn) is ADF uw antwoord.


> 
> 

HEAD via het volgende artikel voor een groot [ADF voorbeelden][ADF samples].

## <a name="integration-services"></a>Integratieservices
Integration Services (SSIS) is een krachtige en flexibele extraheren Transform and Load (ETL) hulpprogramma die ondersteuning biedt voor complexe werkstromen, gegevenstransformatie en verschillende opties voor het laden van gegevens. SSIS gebruiken voor gegevens over te dragen naar Azure of als onderdeel van een bredere migratie.

> [!NOTE]
> SSIS kunt exporteren naar UTF-8 zonder de bytevolgordemarkering in het bestand. Als u wilt configureren gebruiken dit moet u eerst het onderdeel afgeleide kolom converteren van de tekengegevens in de gegevensstroom gebruiken de 65001 UTF-8-codetabel. Zodra de kolommen zijn geconverteerd, moet u de gegevens schrijven naar de plat bestand-doeladapter ervoor te zorgen dat 65001 ook is geselecteerd als de codetabel voor het bestand.
> 
> 

SSIS maakt verbinding met SQL Data Warehouse, net zoals deze verbinding met een SQL Server-implementatie maken wilt. Uw verbindingen moet echter worden door een ADO.NET-Verbindingsbeheer te gebruiken. U moet ook zorgt voor het configureren van de 'Gebruik bulksgewijs invoegen indien beschikbaar' instelling doorvoer maximaliseren. Raadpleeg de [ADO.NET-doeladapter] [ ADO.NET destination adapter] artikel voor meer informatie over deze eigenschap

> [!NOTE]
> Verbinding maken met Azure SQL Data Warehouse met behulp van OLEDB wordt niet ondersteund.
> 
> 

Er is bovendien altijd de mogelijkheid om een pakket vanwege problemen met beperking of het netwerk mislukken. Ontwerp pakketten zodat ze kunnen worden hervat op het moment van mislukt, zonder opnieuw uitvoeren, werkt die vóór de fout is voltooid.

Raadpleeg voor meer informatie de [SSIS-documentatie][SSIS documentation].

## <a name="bcp"></a>bcp
BCP is een opdrachtregelprogramma dat is ontworpen voor een plat bestand gegevens importeren en exporteren. Sommige transformatie kan worden uitgevoerd tijdens het exporteren van gegevens. Eenvoudige transformaties gebruik om uit te voeren een query om te selecteren en de gegevens transformeren. Eenmaal geëxporteerd, kunnen de platte bestanden vervolgens worden geladen rechtstreeks in het doel de SQL Data Warehouse-database.

> [!NOTE]
> Het is vaak een goed idee om de transformaties gebruikt tijdens het exporteren van gegevens in een weergave in het bronsysteem inkapselen. Dit zorgt ervoor dat de logica wordt bewaard, en het proces herhaald wordt.
> 
> 

Voordelen van bcp zijn:

* Eenvoud. BCP opdrachten zijn eenvoudig te bouwen en uitvoeren
* Het laadproces is opnieuw gestart. Eenmaal geëxporteerde de belasting kan worden uitgevoerd van een willekeurig aantal malen

Beperkingen van bcp zijn:

* BCP werkt met gebruikmaking platte bestanden alleen. Deze functie niet samenwerkt met zoals XML- of JSON-bestanden
* Data transformation mogelijkheden zijn beperkt tot alleen de export-fase en eenvoudige aard zijn
* BCP is niet aangepast aan robuuste worden bij het laden van gegevens via het internet. Netwerk instabiliteit kan ertoe leiden dat een fout bij het laden.
* BCP is afhankelijk van het schema wordt aanwezig is in de doeldatabase voorafgaand aan de belasting

Zie voor meer informatie [gebruikt bcp om gegevens te laden in SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Gegevensmigratie optimaliseren
Een migratieproces SQLDW kan worden effectief onderverdeeld in drie afzonderlijke stappen:

1. Het exporteren van de brongegevens
2. Overdracht van gegevens naar Azure
3. In de doeldatabase SQLDW laden

Elke stap kan afzonderlijk worden geoptimaliseerd voor het maken van een robuuste, opnieuw gestart en robuuste migratieproces die optimaliseert de prestaties bij elke stap.

## <a name="optimizing-data-load"></a>Optimaliseren laden van gegevens
Kijken naar deze in omgekeerde volgorde voor even; Er is de snelste manier om gegevens te laden met PolyBase. Optimaliseren voor een laadproces PolyBase plaatst vereisten op de voorgaande stappen is het verstandig om te begrijpen dit vooraf. Ze zijn:

1. Codering van gegevensbestanden
2. Indeling van de gegevensbestanden worden opgeslagen
3. Locatie van gegevensbestanden

### <a name="encoding"></a>Encoding
PolyBase vereist-gegevensbestanden UTF-8-of UTF-16FE. 



### <a name="format-of-data-files"></a>Indeling van de gegevensbestanden worden opgeslagen
PolyBase vereist een vaste rijeinde van \n of nieuwe regel aangeeft. Uw gegevensbestanden moeten voldoen aan deze standaard. Er zijn geen beperkingen op tekenreeks- of afsluitingen.

U moet voor elke kolom in het bestand als onderdeel van de externe tabel in PolyBase definiëren. Zorg ervoor dat alle geëxporteerde kolommen vereist zijn en dat de typen aan de vereiste normen voldoen.

Raadpleeg terug naar de [migreren uw schema] artikel voor informatie over de ondersteunde gegevenstypen.

### <a name="location-of-data-files"></a>Locatie van gegevensbestanden
SQL Data Warehouse gebruikmaakt van PolyBase uitsluitend laden van gegevens uit Azure Blob Storage. Als gevolg daarvan kan zijn de gegevens eerst overgebracht naar de blobopslag.

## <a name="optimizing-data-transfer"></a>Gegevensoverdracht optimaliseren
Een van de traagste onderdelen van de gegevensmigratie is de overdracht van gegevens naar Azure. Niet alleen kunt netwerkbandbreedte een probleem kunnen zijn, maar ook netwerk betrouwbaarheid kunt ernstig wordt verstoord uitgevoerd. Gegevens migreren naar Azure is standaard via internet zodat de kans op fouten van overdracht plaatsvinden redelijkerwijs waarschijnlijk zijn. Deze fouten vereisen echter opnieuw worden verzonden geheel of gedeeltelijk.

Gelukkig hebt u verschillende mogelijkheden voor het verbeteren van de snelheid en herstelmogelijkheden van dit proces:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
U kunt overwegen om [ExpressRoute] [ ExpressRoute] om de overdracht te versnellen. [ExpressRoute] [ ExpressRoute] biedt een particuliere verbinding met Azure zodat de verbinding niet via het openbare internet gaat. Dit is geen een verplichte stap. Echter, het verbeteren van doorvoer wanneer gegevens worden gepusht naar Azure uit een on-premises of CO-locatiefaciliteit.

De voordelen van het gebruik van [ExpressRoute] [ ExpressRoute] zijn:

1. Hogere mate van betrouwbaarheid
2. Snellere netwerksnelheid
3. Lagere netwerklatentie
4. hogere netwerkbeveiliging

[ExpressRoute] [ ExpressRoute] is handig voor een aantal scenario's; niet alleen de migratie.

Geïnteresseerd? Ga voor meer informatie en prijzen Ga naar de [ExpressRoute-documentatie][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure Import- en Export-Service
De Azure importeren en exporteren-Service is een overdrachtsproces gegevens is ontworpen voor grote (GB ++) voor grootschalige (TB ++) de overdracht van gegevens in Azure. Dit omvat het schrijven van uw gegevens naar de schijven en ze verzenden naar een Azure-Datacenter. Inhoud van de schijf wordt vervolgens in Azure Storage-Blobs namens jou worden geladen.

Een weergave op hoog niveau van het importeerproces voor het exporteren is als volgt:

1. Een Azure Blob Storage-container voor het ontvangen van de gegevens configureren
2. Uw gegevens worden geëxporteerd naar de lokale opslag
3. Kopieer de gegevens naar 3.5 inch SATA III-II harde schijven met behulp van de [Azure Import/Export hulpprogramma]
4. Maken van een Import-taak met de Azure importeren en exporteren Service bieden de journaal-bestanden die wordt geproduceerd door de [Azure hulpprogramma voor importeren/exporteren]
5. Verzenden van de schijven uw aangewezen Azure-Datacenter
6. Uw gegevens worden overgedragen naar uw Azure Blob Storage-container
7. De gegevens in SQLDW met PolyBase laden

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] hulpprogramma
De [AZCopy][AZCopy] hulpprogramma is een uitstekend hulpprogramma voor het ophalen van de gegevens die binnenkomen in Azure Storage-Blobs. Het is ontworpen voor kleine (MB ++) voor zeer grote (GB ++) gegevensoverdracht. [AZCopy] is ontworpen voor een goede robuuste doorvoer bij de overdracht van gegevens naar Azure en daarom is een uitstekende keuze is voor de stap van de gegevensoverdracht. Eenmaal overgebrachte kunt u de gegevens met PolyBase in SQL Data Warehouse laden. U kunt ook AZCopy opnemen in uw SSIS-pakketten met behulp van een 'Proces uitvoeren'.

Voor het gebruik van AZCopy moet u eerst te downloaden en installeren. Er is een [productieversie] [ production version] en een [preview-versie] [ preview version] beschikbaar.

Als u wilt uploaden van een bestand van het bestandssysteem moet u een opdracht zoals hieronder:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Een samenvatting van het proces op hoog niveau kan worden:

1. Een Azure-opslag-blob-container voor het ontvangen van de gegevens configureren
2. Uw gegevens worden geëxporteerd naar de lokale opslag
3. AZCopy uw gegevens in de Azure Blob Storage-container
4. De gegevens laden in SQL Data Warehouse met PolyBase

Volledige documentatie beschikbaar: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>Optimaliseren gegevens exporteren
Naast ervoor te zorgen dat de export aan de voorschriften door PolyBase voldoet kunt u ook zoeken naar de uitvoer van de gegevens voor het verbeteren van het proces verder optimaliseren.



### <a name="data-compression"></a>Gegevenscompressie
Gzip gecomprimeerde gegevens kan worden gelezen door PolyBase. Als u uw gegevens naar de gzip-bestanden te comprimeren wordt u de hoeveelheid gegevens die via het netwerk wordt gepusht minimaliseren.

### <a name="multiple-files"></a>Meerdere bestanden
Grote tabellen in verschillende bestanden splitsen niet alleen kan worden verbeterd export snelheid, maar ook met overdracht re-startability en de algehele beheerbaarheid van de gegevens eenmaal in Azure blob storage. Een van de vele nice functies van PolyBase is Hiermee worden de bestanden naar de map lezen en behandelen als een tabel. Daarom een goed idee om te isoleren van de bestanden voor elke tabel in de eigen map.

PolyBase biedt ook ondersteuning voor een functie die bekend als 'recursieve map traversal'. U kunt deze functie gebruiken voor het verder verbeteren van de organisatie van de geëxporteerde gegevens voor het verbeteren van uw gegevens beheren.

Zie voor meer informatie over het laden van gegevens met PolyBase, [gebruik PolyBase gegevens laadt in SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de migratie [migreren van uw oplossing naar SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/v1/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/v1/data-factory-samples.md
[ADF Copy examples]: ../data-factory/v1/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase to load data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
