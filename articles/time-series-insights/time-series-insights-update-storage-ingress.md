---
title: Opslag van gegevens en de inkomende gegevens in Azure Time Series Insights-Preview | Microsoft Docs
description: Inzicht krijgen in de opslag van gegevens en inkomend verkeer in Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/05/2018
ms.custom: seodec18
ms.openlocfilehash: f0326e6f05f44b5f2c4416a1c1db09bc5816a297
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53558308"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Opslag van gegevens en de inkomende gegevens in Azure Time Series Insights-Preview

Dit artikel beschrijft de wijzigingen in de opslag van gegevens en inkomend verkeer van Azure Time Series Insights Preview. Hierin worden de onderliggende opslagstructuur, de bestandsindeling en de Time Series-ID-eigenschap. Het artikel wordt ook beschreven voor het onderliggende proces van inkomend verkeer, doorvoer en beperkingen.

## <a name="data-storage"></a>Gegevensopslag

Wanneer u een Time Series Insights Preview betalen per gebruik SKU-omgeving maakt, maakt u twee resources:

* Een Time Series Insights-omgeving.
* Een Azure-opslag voor algemeen gebruik V1-account waar de gegevens worden opgeslagen.

Time Series Insights Preview maakt gebruik van Azure Blob-opslag met het type Parquet-bestand. Time Series Insights beheert alle gegevensbewerkingen, waaronder het maken van blobs, indexering en partitioneren van de gegevens in de Azure storage-account. U kunt deze blobs maken met behulp van een Azure storage-account.

Zoals andere Azure Storage-blobs kunnen blobs Time Series Insights is gemaakt u lezen en schrijven naar ter ondersteuning van verschillende scenario's voor integratie.

> [!TIP]
> Time Series Insights-prestaties kan negatief worden beïnvloed als u lezen of naar uw blobs te vaak schrijven.

Zie voor een overzicht van Azure Blob-opslag, [Storage-blobs inleiding](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Zie voor meer informatie over het type van het bestand Parquet [bestandstypen worden ondersteund in Azure Storage](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Parquet-indeling

Parquet is een kolom is gericht, gegevensbestandsindeling die is ontworpen voor:

* Interoperabiliteit
* Ruimte-efficiëntie
* Query-efficiëntie

Time Series Insights Parquet gekozen omdat deze efficiënte gegevenscompressie biedt en codering schema's, met verbeterde prestaties die u kunt de verwerking van complexe gegevens in één bulkbewerking.

Zie voor een beter begrip van de Parquet-indeling, [Parquet documentatie](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Gebeurtenisstructuur in Parquet

Time Series Insights maakt en slaat kopieën van de blobs in de volgende twee indelingen:

1. De eerste, de eerste kopie is gepartitioneerd op basis van aankomsttijd:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Aanmaaktijd BLOB voor blobs gepartitioneerd op basis van aankomsttijd.

1. De tweede, opnieuw gepartitioneerd kopie is gepartitioneerd op basis van een dynamische groepering van Time Series-ID:

    * `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Tijdstempel van de minimale gebeurtenis in een blob voor blobs gepartitioneerd op basis van Time Series-ID.

> [!NOTE]
> * `<YYYY>` toegewezen aan de weergave van een jaar 4 cijfers.
> * `<MM>` toegewezen aan de weergave van een maand 2 cijfers.
> * `<YYYYMMDDHHMMSSfff>` toegewezen aan een timestamp-weergave met 4-cijferige jaar (`YYYY`), maand 2 cijfers (`MM`), 2 cijfers dag (`DD`), 2 cijfers uur (`HH`), 2 cijfers minuut (`MM`), 2 cijfers seconde (`SS`), en 3 cijfers milliseconden (`fff`).

Time Series Insights-gebeurtenissen worden toegewezen aan de inhoud van de Parquet-bestand als volgt:

* Elke gebeurtenis wordt toegewezen aan één rij.
* Ingebouwde **Timestamp** kolom met de tijdstempel van een gebeurtenis. De eigenschap Timestamp is nooit null. Wordt standaard **gebeurtenistijd bron in de wachtrij geplaatste** als de eigenschap Timestamp niet is opgegeven in de gebeurtenis bron. De tijdstempel is ingesteld op UTC. 
* Alle andere eigenschappen die zijn toegewezen aan kolommen eindigen met `_string` (tekenreeks), `_bool` (Boolean), `_datetime` (datetime), en `_double` (double), afhankelijk van de eigenschapstype.
* Dit is het toewijzingsschema voor de eerste versie van de bestandsindeling, als **V = 1**. Als deze functie zich verder ontwikkelt, de naam wordt verhoogd naar **V = 2**, **V = 3**, enzovoort.

## <a name="partitions"></a>Partities

Elke Time Series Insights Preview-omgeving moet de eigenschap van een Time Series-ID en een Timestamp-eigenschap die unieke identificatie hebben. Uw Time Series-ID fungeert als een logische partitie voor uw gegevens en een natuurlijke grens van de Time Series Insights Preview-omgeving biedt voor het distribueren van gegevens over fysieke partities. Beheer van fysieke partitie wordt beheerd door de Time Series Insights Preview in Azure storage-account.

Time Series Insights maakt gebruik van dynamische partitioneren opslag-en queryprestaties optimaliseren door verwijderen en opnieuw maken van partities. De Preview tijd Series Insights partitioneren algoritme probeert om te voorkomen dat één fysieke partitie gegevens voor meerdere, afzonderlijke, logische partities. Met andere woorden, houdt het algoritme voor partitionering u alle gegevens specifieke naar een één Time Series-ID uitsluitend aanwezig zijn in Parquet-bestanden zonder wordt interleaved met andere Time Series-id. Het algoritme voor dynamische partitioneren ook wil behouden de oorspronkelijke volgorde van gebeurtenissen in een één Time Series-ID.

In eerste instantie wordt gelijktijdig met inkomend verkeer, de gegevens zijn gepartitioneerd door het tijdstempel zodat een enkele, logische partitie binnen een bepaald tijdsinterval kan worden verdeeld over meerdere fysieke partities. Een enkele fysieke partitie bevat mogelijk ook veel of alle logische partities. Vanwege de beperkingen van het blob-grootte, zelfs met optimale partitionering, één logische partitie in beslag kan nemen meerdere fysieke partities.

> [!NOTE]
> De Timestamp-waarde is standaard het bericht *Wachtrijduur* in de geconfigureerde gebeurtenisbron. 

Als u historische gegevens of berichten batchgewijs uploaden bent, moet u de waarde die u wilt opslaan met uw gegevens om de Timestamp eigenschap die is toegewezen aan de juiste tijdstempel toewijzen. De tijdstempeleigenschap is hoofdlettergevoelig. Zie voor meer informatie, [Tijdreeksmodel](./time-series-insights-update-tsm.md).

## <a name="physical-partitions"></a>Fysieke partities

Een fysieke partitie is een blok-blob die opgeslagen in uw storage-account. De werkelijke grootte van de blobs kan verschillen omdat de grootte is afhankelijk van het push-tarief. We verwachten echter blobs ongeveer 20 MB tot 50 MB groot zijn. Dit kan worden uitgegaan onder leiding van de Time Series Insights-team om te selecteren van 20 MB aan de grootte van de prestaties van query's optimaliseren. Deze grootte kan na verloop van tijd, afhankelijk van de grootte en de snelheid van binnenkomende gegevens wijzigen.

> [!NOTE]
> * BLOBs zijn grootte van 20 MB.
> * Azure-blobs worden af en toe opnieuw gepartitioneerd voor betere prestaties doordat verwijderen en opnieuw maken.
> * De dezelfde Time Series Insights-gegevens kunnen ook worden gebruikt in twee of meer blobs.

## <a name="logical-partitions"></a>Logische partities

Een logische partitie is een partitie in een fysieke partitie waarin alle gegevens die zijn gekoppeld aan een sleutelwaarde van één partitie. Time Series Insights Preview partities logisch elke blob op basis van twee eigenschappen:

* **Time Series-ID**: De partitiesleutel voor alle Time Series Insights-gegevens in de gebeurtenisstroom en het model.
* **Tijdstempel**: De tijd op basis van de eerste inkomend verkeer.

Time Series Insights Preview biedt goed presterende query's die zijn gebaseerd op deze twee eigenschappen. Deze twee eigenschappen bieden ook de meest efficiënte methode voor het snel leveren van Time Series Insights-gegevens.

Het is belangrijk om te selecteren van een juiste ID in de reeks tijd, omdat het is een onveranderbare eigenschap. Zie voor meer informatie, [kiezen Time Series-id's](./time-series-insights-update-how-to-id.md).

## <a name="your-azure-storage-account"></a>Uw Azure storage-account

### <a name="storage"></a>Storage

Wanneer u een betalen per gebruik Time Series Insights-omgeving maakt, maakt u twee resources: een Time Series Insights-omgeving en een Azure Storage voor algemeen gebruik V1-account waar de gegevens worden opgeslagen. We hebben gekozen Azure-opslag voor algemeen gebruik V1 van de standaardbron maken vanwege de interoperabiliteit, de prijs en prestaties. 

Time Series Insights publiceert maximaal twee exemplaren van elke gebeurtenis in uw Azure storage-account. De eerste kopie altijd behouden, zodat u kunt een query het sommen met behulp van andere services. Eenvoudig kunt u Spark, Hadoop en andere vertrouwde hulpprogramma's voor Time Series-id's via onbewerkte Parquet-bestanden, omdat deze engines standaardfilters bestandsnaam ondersteunen. Groeperen van blobs op jaar en maand is een handige manier om de lijst met blobs in een bepaalde periode voor een aangepaste taak. 

Time Series Insights repartitions bovendien de Parquet-bestanden om te optimaliseren voor de Time Series Insights API's. Het meest recent repartitioned bestand wordt ook opgeslagen.

Gegevens worden tijdens de openbare Preview voor onbepaalde tijd opgeslagen in uw Azure storage-account.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Schrijven en bewerken van Time Series Insights-blobs

Prestaties van query's en beschikbaarheid van gegevens te garanderen, niet bewerken of verwijderen van alle bestaande blobs die zijn gemaakt door Time Series Insights.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Toegang tot en het exporteren van gegevens van Time Series Insights Preview

Het is raadzaam voor toegang tot gegevens die zijn opgeslagen in de Verkenner van Time Series Insights Preview te gebruiken in combinatie met andere services. Bijvoorbeeld, kunt u uw gegevens aan rapport in Power BI gebruiken voor machine learning met behulp van Azure Machine Learning Studio, of in een toepassing laptop met Jupyter-Notebooks.

U kunt toegang tot uw gegevens op drie algemene manieren:

* Vanuit de Verkenner van Time Series Insights Preview.
* Van de API's van Time Series Insights Preview-versie.
* Rechtstreeks vanuit Azure storage-account.

#### <a name="from-the-time-series-insights-preview-explorer"></a>Vanuit de Verkenner van Time Series Insights Preview

U kunt gegevens exporteren als een CSV-bestand van de Verkenner van Time Series Insights Preview. Zie voor meer informatie, [Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md).

#### <a name="from-the-time-series-insights-preview-apis"></a>Van de API's van Time Series Insights Preview-versie

De API-eindpunt kan worden bereikt op `/getRecorded`. Zie voor meer informatie over deze API, [Time Series Query](./time-series-insights-update-tsq.md).

#### <a name="from-an-azure-storage-account"></a>Uit Azure storage-account

* U moet leestoegang tot de account die u toegang tot uw Time Series Insights-gegevens. Zie voor meer informatie, [toegang tot de resources van uw opslagaccount beheren](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

* Zie voor meer informatie over het direct manieren om gegevens te lezen uit Azure Blob storage [om gegevens te verplaatsen naar en van uw opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* Gegevens exporteren uit Azure storage-account:

    * Controleer eerst of uw account voldoet aan de vereisten voor het exporteren van gegevens. Zie voor meer informatie, [opslag importeren en exporteren van vereisten](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements).

    * Zie voor meer informatie over andere manieren voor het exporteren van gegevens uit uw Azure storage-account, [importeren en exporteren van gegevens uit blobs](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs).

### <a name="data-deletion"></a>Gegevens verwijderen

Geen blobs niet verwijderen omdat Time Series Insights Preview metagegevens over de blobs erin houdt.

## <a name="ingress"></a>Inkomend verkeer

### <a name="time-series-insights-ingress-policies"></a>Time Series Insights inkomend beleid

Time Series Insights Preview biedt ondersteuning voor de dezelfde bronnen van gebeurtenissen en de bestandstypen die op dit moment Time Series Insights ondersteunt.

Ondersteunde gebeurtenisbronnen zijn onder andere:

- Azure IoT Hub
- Azure Event Hubs
  
  > [!NOTE]
  > Azure Event Hub-instanties ondersteuning voor Kafka.

Ondersteunde bestandstypen zijn onder andere:

* JSON: Zie voor meer informatie over de ondersteunde JSON-vormen kunnen worden verwerkt, [hoe vorm JSON](./time-series-insights-send-events.md#json).

### <a name="data-availability"></a>Beschikbaarheid van gegevens

Time Series Insights Preview gegevens met behulp van een strategie voor optimalisatie van de grootte van de blob worden geïndexeerd. Gegevens beschikbaar om op te vragen nadat deze geïndexeerd, die is gebaseerd op hoeveel gegevens er in en welke uit te voeren.

> [!IMPORTANT]
> * De Time Series Insights-versie voor algemene beschikbaarheid (GA) maken beschikbaar voor gegevens in 60 seconden van een gebeurtenisbron te maken. 
> * Tijdens de Preview-versie, verwacht een langere periode voordat de gegevens beschikbaar worden gesteld. 
> * Als u een aanzienlijke latentie ondervinden, moet u contact met ons opnemen.

### <a name="scale"></a>Schalen

Time Series Insights Preview biedt ondersteuning voor de schaal van een eerste inkomend verkeer van maximaal 6 megabits per seconde (Mbps) per omgeving. Uitgebreide ondersteuning voor vergroten/verkleinen wordt momenteel is. We willen onze documentatie bij zodat deze verbeteringen bijwerken.

## <a name="next-steps"></a>Volgende stappen

Lees de [Azure Time Series Insights Preview opslag- en Uitgangsclaims](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [gegevensmodellering](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png
