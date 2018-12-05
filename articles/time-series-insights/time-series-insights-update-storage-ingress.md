---
title: De opslag van gegevens en inkomend verkeer in de Azure Time Series Insights (preview) | Microsoft Docs
description: Inzicht krijgen in de opslag van gegevens en de inkomende gegevens in de Azure Time Series Insights (preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: d4c82f01b64ebb0248367f595fe25b2ec50f2d77
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876030"
---
# <a name="data-storage-and-ingress-in-the-azure-time-series-insights-preview"></a>De opslag van gegevens en inkomend verkeer in de Azure Time Series Insights (Preview)

Dit artikel beschrijft de wijzigingen in de opslag van gegevens en inkomend verkeer van de Azure Time Series Insights (Preview). Het betreft de onderliggende opslagstructuur, bestandsindeling, en **Time Series-ID** eigenschap. Hierin worden ook het onderliggende proces van inkomend verkeer, doorvoer en beperkingen.

## <a name="data-storage"></a>Gegevensopslag

Bij het maken van een Time Series Insights-update (**PAYG SKU**)-omgeving, maakt u twee resources:

* Een Azure TSI-omgeving.
* Een Azure-opslag voor algemeen gebruik V1-account waar de gegevens worden opgeslagen.

De TSI (Preview) maakt gebruik van Azure Blob-opslag met het type Parquet-bestand. Azure TSI beheert alle gegevensbewerkingen, waaronder het maken van blobs, indexering en partitioneren van de gegevens in de Azure Storage-account. Deze blobs worden gemaakt met behulp van een Azure Storage-account.

Net als elke andere Azure Storage-blob, kunt u lezen en schrijven naar uw blobs Azure TSI gemaakt voor de ondersteuning van verschillende integratiescenario's.

> [!TIP]
> Het is belangrijk te onthouden TSI-prestaties kan negatief worden beïnvloed door lezen of schrijven naar uw blobs te vaak.

Lees voor een overzicht van Azure Blob-opslag werkt, de [Storage-blobs inleiding](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Raadpleeg voor meer informatie over het type Parquet-bestand, [bestandstypen worden ondersteund in Azure Storage](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Parquet-indeling

Parquet is gegevens kolomgebaseerde-bestandsindeling die is ontworpen voor:

* Interoperabiliteit
* Ruimte-efficiëntie
* Query-efficiëntie

Azure TSI gekozen Parquet omdat efficiënte compressie biedt en codering schema's met prestaties verbeterde voor de verwerking van complexe gegevens in één bulkbewerking.

Voor een beter inzicht in wat de Parquet-bestand indeling heeft, Ga naar de [officiële Parquet-pagina](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Gebeurtenisstructuur in Parquet

Twee kopieën van de blobs die zijn gemaakt door Azure TSI worden opgeslagen in de volgende indelingen:

1. De eerste, een initiële kopie wordt worden gepartitioneerd op basis van aankomsttijd:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI internal suffix>.parquet`
    * Aanmaaktijd BLOB voor blobs gepartitioneerd op basis van aankomsttijd.

1. De tweede, een repartitioned kopie wordt worden gepartitioneerd op basis van de dynamische groepering van time series-ID:

    • `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI internal suffix>.parquet`
    * Tijdstempel van de min-gebeurtenis in de blob voor blobs gepartitioneerd op basis van de tijdreeks-id.

> [!NOTE]
> * `<YYYY>` toegewezen aan jaar.
> * `<MM>` toegewezen aan de maand.
> * `<YYYYMMDDHHMMSSfff>` Maps volledige tijdstempel in milliseconden.

Azure TSI-gebeurtenissen worden toegewezen aan de inhoud van de Parquet-bestand als volgt:

* Elke gebeurtenis wordt toegewezen aan één rij.
* Ingebouwde **Timestamp** kolom met de tijdstempel van een gebeurtenis. De **Timestamp** eigenschap nooit null is.  Wordt standaard **gebeurtenistijd bron in de wachtrij geplaatste** als de **Timestamp** eigenschap niet is opgegeven in de gebeurtenisbron.  **Tijdstempel** is ingesteld op UTC.  
* Alle andere eigenschappen toewijzen aan kolommen wordt beëindigd met `_string` (tekenreeks), `_bool` (boolean), `_datetime` (datetime), en `_double` (double), afhankelijk van de eigenschapstype.
* Dat is de eerste versie van de bestandsindeling en verwijzen we naar het bestand als **V = 1**.  Als deze functie zich verder ontwikkelt de naam van de dienovereenkomstig wordt verhoogd naar **V = 2**, **V = 3**, enzovoort.

## <a name="how-to-partition"></a>Hoe partitie

Elke omgeving Azure TSI (Preview) moet een **Time Series-ID** eigenschap en een **Timestamp** eigenschap, die wordt geïdentificeerd. Uw **Time Series-ID** fungeert als een logische partitie voor uw gegevens en biedt de Azure TSI (Preview)-omgeving met een natuurlijke grens voor het distribueren van gegevens over fysieke partities. Beheer van fysieke partitie wordt beheerd door Azure TSI (Preview) in een Azure Storage-account.

Azure TSI maakt gebruik van dynamische partitioneren storage-gebruik en query's uitvoeren om prestaties te optimaliseren door verwijderen en opnieuw maken van partities. De TSI (Preview) algoritme voor dynamische partitioneren streeft ernaar om te voorkomen dat gegevens voor meerdere verschillende logische partities met één fysieke partitie. Of met andere woorden de partitionering algoritme doel is dat alle gegevens die betrekking hebben op één **Time Series-ID** moet exclusief aanwezig zijn in Parquet-bestanden zonder wordt met andere interleaved **Time Series-id's**. Het algoritme voor dynamische partitioneren ook wil behouden de oorspronkelijke volgorde van gebeurtenissen in een enkel **Time Series-ID**.

In eerste instantie wordt gelijktijdig met inkomend verkeer, de gegevens zijn gepartitioneerd door de **tijdstempel** , zodat een enkele, logische partitie binnen een bepaald tijdsinterval kan zijn verdeeld over meerdere fysieke partities. Een enkele fysieke partitie kan ook veel of alle logische partities bevatten.  Vanwege beperkingen van de bestandsgrootte blob, zelfs met optimale partitioneren van één logische partitie kan innemen meerdere fysieke partities.

> [!NOTE]
> De **Timestamp** waarde is het bericht **Wachtrijduur** in de geconfigureerde gebeurtenisbron standaard.  

Als u historische gegevens of berichten batchgewijs uploadt, moet u opgeven de **Timestamp** eigenschap in de gegevens die is toegewezen aan de juiste **Timestamp** waarde die u met uw gegevens wenst op te slaan.  De **Timestamp** eigenschap is hoofdlettergevoelig. Lees voor meer het [Tijdreeksmodel artikel](./time-series-insights-update-tsm.md).

## <a name="physical-partition"></a>Fysieke partitie

Een fysieke partitie is een blok-blob die zijn opgeslagen in Azure Storage. De werkelijke grootte van de blobs variëren omdat deze is afhankelijk van de push-frequentie, maar we verwachten blobs ongeveer 20-50 MB groot zijn. Het team TSI geselecteerd vanwege die verwachtingen, 20 MB groot zijn om queryprestaties te optimaliseren. Dit kan na verloop van tijd op basis van de grootte en de snelheid van binnenkomende gegevens wijzigen.

> [!NOTE]
> * BLOBs zijn grootte van 20MB.
> * Azure-blobs worden af en toe opnieuw gepartitioneerd voor betere prestaties door wordt verwijderd en opnieuw gemaakt.
> * Houd er ook rekening mee dat de dezelfde TSI-gegevens gebruikt in meerdere blobs worden kunnen.

## <a name="logical-partition"></a>Logische partitie

Een logische partitie is een partitie in een fysieke partitie waarin alle gegevens die zijn gekoppeld aan een sleutelwaarde van één partitie. De TSI (Preview) worden logisch partitie voor elke blob op basis van twee eigenschappen:

1. **Time Series-ID** -de partitiesleutel voor alle TSI-gegevens in de gebeurtenisstroom en het model.
1. **Tijdstempel** - op basis van de eerste binnenkomende.

De Azure TSI (Preview) biedt een goed presterende query's die zijn gebaseerd op deze twee eigenschappen. Deze twee eigenschappen bieden ook de meest efficiënte methode voor het snel leveren van gegevens van TSI.

Het is belangrijk om te selecteren van een geschikte **Time Series-ID**, zoals het is een onveranderbare eigenschap.  Zie [kiezen Time Series-id's](./time-series-insights-update-how-to-id.md) voor meer informatie.

## <a name="your-azure-storage-account"></a>Uw Azure Storage-account

### <a name="storage"></a>Storage

Bij het maken van een TSI **PAYG** omgeving, maakt u twee resources: de TSI-omgeving en een Azure storage voor algemeen gebruik V1-account waar de gegevens worden opgeslagen. We hebben gekozen Azure-opslag voor algemeen gebruik V1 standaardbrowser vanwege de interoperabiliteit, de prijs en prestaties.  

Azure TSI publiceert maximaal twee exemplaren van elke gebeurtenis in uw Azure Storage-account. De eerste kopie blijft altijd behouden om te controleren of dat u kunt deze een query sommen met andere services. Dus, Spark, Hadoop en andere vertrouwde hulpprogramma's gemakkelijk kunnen worden gebruikt voor **-id's van Time Series** via onbewerkte Parquet-bestanden omdat deze engines ondersteuning biedt voor basic bestand naam filteren. Blobs op jaar en maand groepering is nuttig voor het verzamelen van blob-lijst binnen een bepaalde periode voor een aangepaste taak.  

Bovendien TSI wordt opnieuw partitioneren van de Parquet-bestanden om te optimaliseren voor de Azure TSI-API's en het meest recent repartitioned bestand wordt ook opgeslagen.

Tijdens de openbare Preview worden de gegevens voor onbepaalde tijd worden opgeslagen in uw Azure Storage-account.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Schrijven en bewerken van Time Series Insights-blobs

Prestaties van query's en beschikbaarheid van gegevens te garanderen, niet bewerken of verwijderen van alle bestaande blobs die zijn gemaakt door TSI.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Toegang tot en het exporteren van gegevens uit de Time Series Insights (Preview)

U wilt toegang tot gegevens die zijn opgeslagen in Azure TSI (Preview)-Verkenner te gebruiken in combinatie met andere services. Bijvoorbeeld, kunt u uw gegevens gebruiken voor het melden van in Power BI voor machine learning met behulp van Azure Machine Learning Studio of in een notitieblok toepassing Jupyter Notebooks, enzovoort.

Er zijn drie algemene paden voor toegang tot uw gegevens:

* De Azure TSI (Preview)-Verkenner.
* De Azure TSI (Preview)-API's.
* Rechtstreeks vanuit een Azure Storage-account.

### <a name="from-the-time-series-insights-preview-explorer"></a>Vanuit de Verkenner van Time Series Insights (Preview)

U kunt gegevens exporteren als een CSV-bestand vanuit de explorer TSI (Preview). Meer informatie over de [de (Preview) TSI-Verkenner](./time-series-insights-update-explorer.md).

### <a name="from-the-time-series-insights-preview-apis"></a>Van de Time Series Insights (Preview) API 's

De API-eindpunt kan worden bereikt op `/getRecorded`. Lees voor meer informatie over deze API [Time Series Query](./time-series-insights-update-tsq.md).

### <a name="from-an-azure-storage-account"></a>Uit een Azure Storage-account

1. U moet leestoegang hebben tot deze is verleend aan uw account u wilt gebruiken voor toegang tot uw gegevens van TSI. Lees voor meer informatie over het verlenen van toegang voor lezen naar Azure Blob Storage [beheer van toegang tot opslagresources](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

1. Lees voor meer informatie over direct manieren om te lezen van gegevens uit Azure Blob Storage [om gegevens te verplaatsen naar en van Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

1. Het exporteren van gegevens uit een Azure Storage-account:

    * Controleer eerst of dat uw account heeft de benodigde vereisten voldaan om gegevens te exporteren. Lezen [opslag importeren en exporteren van vereisten](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements) voor meer informatie.
    * Meer informatie over andere manieren voor het exporteren van gegevens uit uw Azure Storage-account door naar de pagina [opslag importeren en exporteren van gegevens in blobs](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs)

### <a name="data-deletion"></a>Gegevens verwijderen

Blobs niet verwijderen omdat Time Series Insights (Preview) metagegevens over de blobs in de TSI-update houdt.

## <a name="ingress"></a>Inkomend verkeer

### <a name="azure-time-series-insights-ingress-policies"></a>Beleid voor inkomend verkeer van Azure Time Series Insights

De Azure TSI (Preview) biedt ondersteuning voor de dezelfde bronnen van gebeurtenissen en de bestandstypen die normaal.

Ondersteunde gebeurtenisbronnen zijn onder andere:

* Azure IoT Hub
* Azure Event Hubs
  * Opmerking: Azure Event Hub-instanties ondersteuning voor Kafka.

Ondersteunde bestandstypen zijn onder andere:

* JSON
  * Zie voor meer op de ondersteunde JSON-vormen we kunnen worden verwerkt, de [hoe vorm JSON](./time-series-insights-send-events.md#json) documentatie.

### <a name="data-availability"></a>Beschikbaarheid van gegevens

In de openbare preview indexeert Azure TSI (Preview) gegevens met behulp van een strategie voor een blob-grootte. Dit betekent dat gegevens zijn beschikbaar voor query's uitvoeren zodra deze geïndexeerd (die is gebaseerd op hoeveel gegevens er in en welke uit te voeren).

> [!IMPORTANT]
> * GA TSI maken beschikbaar voor gegevens in 60 seconden ervan een gebeurtenisbron te maken.  
> * We verwachten tijdens de Preview-versie om te zien van een langere periode voordat de gegevens beschikbaar worden gesteld.  
>   * Als u een aanzienlijke latentie ondervinden, neem dan contact met ons.

### <a name="scale"></a>Schalen

De schaal van een eerste inkomend verkeer van maximaal 6 Mbps per omgeving biedt ondersteuning voor Azure TSI (Preview). Uitgebreide ondersteuning voor vergroten/verkleinen wordt momenteel is. Documentatie wordt bijgewerkt om deze verbeteringen weer te geven.

## <a name="next-steps"></a>Volgende stappen

Lees de [opslag van Azure TSI (Preview) en Ingress](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [gegevensmodellering](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png