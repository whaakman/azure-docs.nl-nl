---
title: "Prestaties van de activiteit en prestatieafstemming handleiding kopiëren | Microsoft Docs"
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de gegevensverplaatsing in Azure Data Factory als u Kopieeractiviteit gebruiken.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 2bec612b1d67eceb0e62b28524b98e852d31ad0f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Prestaties van de activiteit en prestatieafstemming handleiding kopiëren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [kopiëren van de prestaties van de activiteit en prestatieafstemming guide voor Data Factory versie 2](../copy-activity-performance.md).

Azure Data Factory-Kopieeractiviteit biedt een uitstekende gegevens veilig, betrouwbaar en hoge prestaties bij het laden van oplossing. Deze Hiermee kunt u voor het kopiëren van tientallen terabytes aan gegevens elke dag op uitgebreide tal van cloud en het on-premises gegevensopslagexemplaren. Razendsnelle snel gegevens laden van prestaties is sleutel zodat u zich kunt richten op het probleem van core 'big data': bouwen van oplossingen voor geavanceerde analyses om inzichten te verkrijgen grondige van die gegevens.

Azure biedt een reeks bedrijfsniveau oplossingen voor opslag en data warehouse en Kopieeractiviteit biedt een maximaal geoptimaliseerd gegevens te laden ervaring die eenvoudig te configureren en instellen. Met slechts één exemplaar-activiteit, kunt u bereiken:

* Laden van gegevens in **Azure SQL Data Warehouse** op **1,2 GBps**. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).
* Laden van gegevens in **Azure Blob storage** op **1.0 GBps**
* Laden van gegevens in **Azure Data Lake Store** op **1.0 GBps**

Dit artikel wordt beschreven:

* [Prestaties nummers](#performance-reference) voor ondersteunde gegevensarchieven bron- en sink voor het plannen van uw project.
* Functies die de kopie doorvoer in verschillende scenario's, inclusief kunnen stimuleren [cloud data movement eenheden](#cloud-data-movement-units), [kopie parallelle](#parallel-copy), en [kopie gefaseerde](#staged-copy);
* [Prestaties afstemmen richtlijnen](#performance-tuning-steps) op het afstemmen van de prestaties en de belangrijkste factoren die invloed op prestaties van de kopie hebben kunnen.

> [!NOTE]
> Als u niet bekend met de Kopieeractiviteit in het algemeen bent, Zie [verplaatsen van gegevens met behulp van de Kopieeractiviteit](data-factory-data-movement-activities.md) voordat u dit artikel leest.
>

## <a name="performance-reference"></a>Verwijzing voor prestaties

Bevat onderstaande tabel als referentie, het nummer van de doorvoer kopiëren in MBps voor de opgegeven bron- en sink-paren die zijn gebaseerd op het intern testen. Ter vergelijking: u ziet ook hoe verschillende instellingen van [cloud data movement eenheden](#cloud-data-movement-units) of [Data Management Gateway-schaalbaarheid](data-factory-data-management-gateway-high-availability-scalability.md) (meerdere gateway knooppunten) kunt u op de prestaties van de kopie.

![Matrix van prestaties](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>In Azure Data Factory versie 1 is de minimale cloud data movement eenheden voor exemplaar van cloud-naar-cloud twee. Als niet wordt opgegeven, raadpleegt u data movement standaardeenheden wordt gebruikt in [cloud data movement eenheden](#cloud-data-movement-units).

**Verwijst naar Let op:**
* Doorvoer wordt berekend met behulp van de volgende formule: [grootte van de gegevens lezen uit bron] / [Kopieeractiviteit uitvoeren duur].
* De prestaties verwijzing getallen in de tabel zijn gemeten met behulp van [TPC-H](http://www.tpc.org/tpch/) gegevensset in een activiteit die één exemplaar wordt uitgevoerd.
* In Azure gegevensarchieven zich de bron- en sink in dezelfde Azure-regio.
* Voor hybride exemplaar tussen on-premises en cloud gegevensarchieven, elk gateway-knooppunt wordt uitgevoerd op een machine die gescheiden van het lokale gegevensarchief met hieronder specificatie is. Wanneer een enkele activiteit werd uitgevoerd op de gateway, wordt in de kopieerbewerking slechts een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte verbruikt. Klik hier als u meer wilt weten van [overweging voor Data Management Gateway](#considerations-for-data-management-gateway).
    <table>
    <tr>
        <td>CPU</td>
        <td>32 cores 2.20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Geheugen</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Netwerk</td>
        <td>Internet-interface: 10 Gbps; intranet-interface: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> U kunt hogere doorvoer bereiken door gebruik te maken van meer gegevens gegevensverplaatsing eenheden (DMUs) dan de standaardwaarde maximale DMUs die 32 voor een exemplaar van cloud-naar-cloud activiteit die wordt uitgevoerd. Bijvoorbeeld, met 100 DMUs bereikt u kopiëren van gegevens van Azure-Blob in Azure Data Lake Store op **1.0GBps**. Zie de [Cloud data movement eenheden](#cloud-data-movement-units) sectie voor meer informatie over deze functie en de ondersteunde scenario. Neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/) meer DMUs aanvragen.

## <a name="parallel-copy"></a>Parallelle kopie
U kunt gegevens van de bron lezen of schrijven van gegevens naar de bestemming **parallel in een Kopieeractiviteit uitvoeren**. Deze functie verbetert de doorvoer van een kopieerbewerking en vermindert de tijd die nodig is om gegevens te verplaatsen.

Deze instelling verschilt van de **gelijktijdigheid** eigenschap in de definitie van de activiteit. De **gelijktijdigheid** eigenschap bepaalt het aantal **gelijktijdige Kopieeractiviteit wordt uitgevoerd** om gegevens te verwerken van andere activiteit windows (1 uur naar 2 uur, 2 uur tot 3 uur en 3 uur tot en met 4 uur). Deze mogelijkheid is handig wanneer u een historisch belasting uitvoeren. De mogelijkheid parallelle kopie is van toepassing op een **één activiteit die wordt uitgevoerd**.

Bekijk een voorbeeldscenario. Meerdere segmenten in het verleden moeten worden verwerkt in het volgende voorbeeld. Data Factory uitvoert een exemplaar van de Kopieeractiviteit (een activiteit uitvoeren) voor elk segment:

* Het gegevenssegment van het eerste activiteitvenster (1 uur naar 2 uur) == > activiteit uitgevoerd 1
* Het gegevenssegment van het tweede venster van activiteit (2 uur op 3 uur) == > activiteit uitgevoerd 2
* Het gegevenssegment van het tweede venster van activiteit (3 uur op 4 uur) == > activiteit 3 uitvoeren

Enzovoort.

In dit voorbeeld wanneer de **gelijktijdigheid** waarde is ingesteld op 2, **activiteit uitgevoerd 1** en **activiteit uitgevoerd 2** gegevens gekopieerd van de twee activiteitsvensters **gelijktijdig** data movement prestaties te verbeteren. Echter als meerdere bestanden gekoppeld aan activiteit 1 uitvoert zijn, kopieert data movement service bestanden van de bron op de bestemming één bestand tegelijk.

### <a name="cloud-data-movement-units"></a>Cloud data movement-eenheden
Een **cloud gegevensverplaatsing gegevenseenheid (DMU)** is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in de Data Factory. DMU is van toepassing voor cloud-naar-cloud te kopiëren, maar niet in een hybride-exemplaar.

**De minimale cloud gegevens gegevensverplaatsing eenheden zorgen Kopieeractiviteit uitvoeren is twee.** Als niet wordt opgegeven, bevat de volgende tabel de standaard DMUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario voor kopiëren | Standaard DMUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 2 en 16 afhankelijk van het aantal en de grootte van de bestanden. |
| Alle andere kopie-scenario 's | 2 |

Om deze standaardinstelling negeren, Geef een waarde op voor de **cloudDataMovementUnits** eigenschap als volgt. De **toegestane waarden** voor de **cloudDataMovementUnits** eigenschap 2, 4, 8, 16 of 32 zijn. De **werkelijke aantal cloud DMUs** dat de kopieerbewerking wordt gebruikt tijdens de uitvoering is gelijk aan of kleiner zijn dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk wanneer u meer eenheden voor een specifieke kopieerbron en sink configureert, de [prestaties verwijzing](#performance-reference).

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```

> [!NOTE]
> Als u meer cloud DMUs voor een hogere doorvoer moet, neem dan contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/). Instellen van 8 en hoger momenteel werkt alleen als u **meerdere bestanden kopiëren van Blob-opslag/Data Lake Store/Amazon S3/cloud FTP-/ cloud SFTP naar Blob storage/Data Lake Store/Azure SQL Database**.
>

### <a name="parallelcopies"></a>parallelCopies
U kunt de **parallelCopies** eigenschap om aan te geven van de parallelle uitvoering die u wilt dat de Kopieeractiviteit gebruiken. U kunt deze eigenschap zien als het maximum aantal threads in de Kopieeractiviteit die kunnen lezen uit de bron- of schrijven naar uw gegevensarchieven sink parallel.

Data Factory bepaalt voor elke kopie-activiteit is uitgevoerd, het aantal parallelle exemplaren te gebruiken om gegevens te kopiëren van de bron gegevens opgeslagen en wordt opgeslagen in de doelgegevens. Het aantal parallelle exemplaren die worden gebruikt, hangt af van het type van de bron en sink die u gebruikt.  

| Bron- en sink | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van bestanden (Blob-opslag; Data Lake Store; Amazon S3; een on-premises bestandssysteem; een lokale HDFS) |Tussen 1 en 32. Afhankelijk van de grootte van de bestanden en het aantal cloud data movement eenheden (DMUs) gebruikt om te kopiëren van gegevens tussen twee cloud gegevensarchieven of de fysieke configuratie van het Gateway-apparaat gebruikt voor een hybride-exemplaar (om gegevens te kopiëren naar of van een on-premises gegevensopslag). |
| Kopiëren van gegevens van **alle brongegevens opslaan naar Azure Table storage** |4 |
| Alle andere bron en sink paren |1 |

Normaal gesproken geeft het standaardgedrag de beste doorvoer. Evenwel waarmee de belasting op computers die als host fungeren van uw gegevens opslaat of als u wilt kopiëren prestaties afstemmen, u kunt de standaardwaarde onderdrukken en een waarde opgeven voor de **parallelCopies** eigenschap. De waarde moet tussen 1 en 32 liggen (zowel liggen). Tijdens de runtime voor de beste prestaties Kopieeractiviteit maakt gebruik van een waarde die kleiner is dan of gelijk aan de waarde die u instelt.

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Verwijst naar Let op:

* Als u gegevens tussen winkels op basis van bestanden, kopieert de **parallelCopies** bepalen de parallelle uitvoering op bestandsniveau. De verdeling in segmenten binnen één bestand gebeurt onder automatisch en transparant en deze is ontworpen voor het gebruik van de meest geschikte chunkgrootte voor een bepaalde bron-gegevenstype store laden van gegevens in de parallelle en rechthoekige naar parallelCopies. Het werkelijke aantal parallelle kopieën data movement service wordt gebruikt voor de kopieerbewerking tijdens de uitvoering is niet meer dan het aantal bestanden die u hebt. Als u het gedrag van de kopie is **mergeFile**, Kopieeractiviteit kan niet profiteren van bestandsniveau parallelle uitvoering.
* Wanneer u een waarde opgeven voor de **parallelCopies** eigenschap, moet u overwegen de toename van de belasting op de bron- en sink-gegevensarchieven en gateway als dit een hybride-kopie is. Dit gebeurt met name wanneer er meerdere activiteiten of gelijktijdige wordt uitgevoerd dezelfde activiteiten die hetzelfde gegevensarchief uitgevoerd. Als u merkt dat het gegevensarchief of de Gateway met de belasting wordt overbelast, vermindert u de **parallelCopies** waarde om te helpen de belasting.
* Als u gegevens van winkels die zich niet op basis van bestanden naar winkels op basis van een bestand kopieert, data movement service negeert de **parallelCopies** eigenschap. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.

> [!NOTE]
> Moet u Data Management Gateway 1.11 of hoger gebruiken de **parallelCopies** functie als u een kopie van de hybride doet.
>
>

Zie voor een beter gebruik van deze twee eigenschappen en voor het verbeteren van de doorvoer van uw gegevens verkeer de [steekproef gebruiksvoorbeelden](#case-study-use-parallel-copy). U hoeft niet te configureren **parallelCopies** om te profiteren van het standaardgedrag. Als u configureert en **parallelCopies** is te klein, meerdere cloud DMUs mogelijk niet volledig worden gebruikt.  

### <a name="billing-impact"></a>Gevolgen van facturering
Deze heeft **belangrijke** te onthouden dat u in rekening worden gebracht op basis van de totale tijd van de kopieerbewerking. Als een taak kopiëren gebruikt voor één uur duren voordat aan één cloud-eenheid en nu het met vier cloud eenheden 15 minuten duurt, blijft de algehele factuur bijna hetzelfde. Bijvoorbeeld, u vier eenheden van de cloud. De eerste cloud unit doorbrengt 10 minuten, de tweede waarde 10 minuten, de derde, 5 minuten, en de vierde, 5 minuten, allemaal in één Kopieeractiviteit uitvoeren. Worden in rekening gebracht voor het totaal aantal copy (gegevensverplaatsing)-tijd 10 + 10 + 5 + 5 = 30 minuten is. Met behulp van **parallelCopies** heeft geen invloed op de facturering.

## <a name="staged-copy"></a>Gefaseerde kopiëren
Wanneer u gegevens uit een brongegevensarchief naar een gegevensopslag sink kopiëren, kunt u kiezen om het Blob-opslag gebruiken als een tussentijdse staging store. Fasering is vooral nuttig in de volgende gevallen:

1. **U wilt opnemen van gegevens uit verschillende gegevensarchieven in SQL Data Warehouse met PolyBase**. SQL Data Warehouse gebruikmaakt van PolyBase als een mechanisme voor hoge gegevensdoorvoer een grote hoeveelheid gegevens laden in SQL Data Warehouse. Echter de brongegevens moet zich in de Blob-opslag en het moet voldoen aan de aanvullende criteria. Wanneer u gegevens uit een ander gegevensarchief dan Blob-opslag laden, kunt u gegevens kopiëren via fasering tussentijdse Blob-opslag kunt activeren. In dat geval voert Data Factory de vereiste gegevenstransformaties om ervoor te zorgen dat deze voldoet aan de vereisten van PolyBase. Vervolgens wordt PolyBase gegevens laadt in SQL Data Warehouse. Zie voor meer informatie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).
2. **Soms duurt even om uit te voeren van een hybride gegevensverplaatsing (dat wil zeggen kopiëren tussen een on-premises gegevens opslaan en een cloudgegevens kunnen worden opgeslagen) via een trage netwerkverbinding**. Om prestaties te verbeteren, kunt u de gegevens on-premises comprimeren zodat kost het minder tijd om gegevens te verplaatsen naar de staging gegevensopslag in de cloud. Vervolgens kunt u de gegevens in het archief met staging decomprimeren voordat u de gegevens in het gegevensarchief van de bestemming laadt.
3. **U niet wilt openen van poorten dan poort 80 en 443 in uw firewall poort vanwege zakelijke IT-beleid**. Wanneer u gegevens uit een on-premises gegevensopslag naar een Azure SQL Database-sink of een Azure SQL Data Warehouse sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en uw bedrijfsfirewall activeren. In dit scenario te profiteren van de gateway naar gegevens eerst kopiëren naar een exemplaar van Blob-opslag-staging via HTTP of HTTPS op poort 443. Vervolgens de gegevens in SQL-Database of SQL Data Warehouse laden van fasering van Blob-opslag. In deze stroom moet u geen poort 1433 inschakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerde kopiëren werkt
Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens gekopieerd uit de gegevensopslag van de bron in het gegevensarchief fasering (bring uw eigen). Vervolgens is de gegevens uit de staging gegevensopslag gekopieerd naar het sink-gegevensarchief. De stroom twee fasen voor u worden beheerd, Data Factory. Tijdelijke gegevens uit de tijdelijke opslag ruimt Data Factory ook nadat de gegevensverplaatsing voltooid is.

In het scenario cloud kopiëren (bron- en sink gegevens opslaat in de cloud zijn), is geen gateway gebruikt. De Data Factory-service voert de kopieerbewerkingen.

![Gefaseerde kopiëren: cloudscenario](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

In het scenario met hybride kopiëren (bron op lokale en sink is in de cloud), de gateway worden gegevens uit de gegevensopslag van de bron verplaatst naar een gefaseerde installatie gegevensarchief. Data Factory-service worden gegevens uit de staging gegevensopslag verplaatst naar het gegevensarchief sink. Kopiëren van gegevens uit een gegevensopslag cloud naar een on-premises gegevensopslag via fasering wordt ook ondersteund met de omgekeerde stroom.

![Gefaseerde kopiëren: hybride scenario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Wanneer u verplaatsing van gegevens met behulp van een gefaseerde installatie archief activeert, kunt u opgeven of u wilt dat de gegevens te comprimeren voordat u gegevens uit de gegevensopslag van de bron naar een tijdelijke of staging gegevensarchief, en vervolgens gedecomprimeerd voordat het verplaatsen van gegevens vanuit een tussentijdse of staging-gegevens opslaan in het gegevensarchief sink.

Op dit moment kunt kopiëren u gegevens tussen twee on-premises gegevensopslagexemplaren met behulp van een gefaseerde installatie archief niet. We verwachten dat deze optie om te zijn binnenkort beschikbaar.

### <a name="configuration"></a>Configuratie
Configureer de **enableStaging** instellen in de kopieerbewerking om op te geven of u wilt dat de gegevens naar het tijdelijk opgeslagen in blobopslag voordat u deze naar een doelgegevensopslagplaats laden. Als u instelt **enableStaging** om op te geven waar, de aanvullende eigenschappen in de volgende tabel weergegeven. Als u niet hebt, moet u ook voor het maken van een Azure Storage of opslag gedeelde handtekening-gekoppelde access-service voor fasering.

| Eigenschap | Beschrijving | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| **enableStaging** |Geef op of u wilt kopiëren van gegevens via een tussentijdse staging-store. |False |Nee |
| **linkedServiceName** |Geef de naam van een [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) gekoppelde service die verwijst naar het exemplaar van de opslag die u als een tussentijdse staging store gebruiken. <br/><br/> U kunt opslag met een shared access signature niet gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| **pad** |Geef het pad voor Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u niet een pad opgeeft, maakt de service een container voor het opslaan van tijdelijke gegevens. <br/><br/> Geef een pad op als u opslag met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| **enableCompression** |Hiermee geeft u op of de gegevens moeten worden gecomprimeerd voordat deze is gekopieerd naar de bestemming. Deze instelling beperkt de hoeveelheid gegevens die worden overgedragen. |False |Nee |

Hier volgt een voorbeeld-definitie van de Kopieeractiviteit met de eigenschappen die worden beschreven in de voorgaande tabel:

```json
"activities":[  
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Gevolgen van facturering
U kosten in rekening gebracht op basis van twee stappen: kopieer duur en type kopiëren.

* Wanneer u in een cloud gekopieerde (gegevens uit een cloud-gegevensarchief kopiëren naar een andere cloud data store) voor fasering u in rekening worden gebracht de [som van de duur van de kopie voor stap 1 en 2] x [cloud kopie eenheidsprijs].
* Wanneer u in een hybride gekopieerde (gegevens uit een on-premises gegevensopslag kopiëren naar een gegevensopslag cloud) voor fasering u in rekening worden gebracht [duur hybride kopiëren] x [hybride kopie eenheidsprijs] + [cloud kopie duur] x [cloud kopie eenheidsprijs].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen
We raden aan dat u deze stappen nemen om afstemmen van de prestaties van uw Data Factory-service met de Kopieeractiviteit:

1. **Stel een basislijn vast**. Tijdens de ontwikkelingsfase uw pijplijn te testen met behulp van de Kopieeractiviteit tegen een representatieve steekproef. Kunt u de Data Factory [segmentering model](data-factory-scheduling-and-execution.md) te beperken van de hoeveelheid gegevens die u samenwerkt.

   Uitvoeringstijd en prestatiekenmerken verzamelen met behulp van de **bewaking en beheer-App**. Kies **Monitor & beheren** op de startpagina van de Data Factory. Kies in de structuurweergave de **uitvoergegevensset**. In de **Activiteitsvensters** Kies de Kopieeractiviteit uitvoeren. **Activiteit Windows** geeft een lijst van de Kopieeractiviteit duur en de grootte van de gegevens die worden gekopieerd. De doorvoer wordt vermeld in **activiteit venster Explorer**. Zie voor meer informatie over de app, [bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de bewaking en beheer-App](data-factory-monitor-manage-app.md).

   ![Details uitvoering van activiteit](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Later in dit artikel kunt u de prestaties en de configuratie van uw scenario om door te kopiëren van de activiteit vergelijken [prestaties verwijzing](#performance-reference) van onze tests.
2. **Diagnose en prestaties te optimaliseren**. Als de prestaties die u merkt niet voldoet aan uw verwachtingen, moet u het identificeren van knelpunten. Vervolgens optimaliseren als u wilt verwijderen of verklein het effect van knelpunten. Een volledige beschrijving van de prestatiediagnose valt buiten het bereik van dit artikel, maar hier volgen enkele algemene overwegingen:

   * Prestatiefuncties:
     * [Parallelle kopie](#parallel-copy)
     * [Cloud data movement-eenheden](#cloud-data-movement-units)
     * [Gefaseerde kopiëren](#staged-copy)
     * [Data Management Gateway-schaalbaarheid](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Gegevensbeheergateway](#considerations-for-data-management-gateway)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)
3. **Vouw de configuratie op uw volledige gegevensset**. Wanneer u tevreden met de prestaties en resultaten bent, kunt u de definitie en de actieve periode van de pijplijn voor uw volledige gegevensset uitvouwen.

## <a name="considerations-for-data-management-gateway"></a>Overwegingen voor Data Management Gateway
**Gateway setup**: het is raadzaam dat u een specifieke host Data Management Gateway-machine. Zie [overwegingen voor het gebruik van Data Management Gateway](data-factory-data-management-gateway.md#considerations-for-using-gateway).  

**Bewaking van de gateway en scale-up/out**: één logische gateway met een of meer knooppunten van de gateway meerdere Kopieeractiviteit wordt uitgevoerd op hetzelfde moment gelijktijdig kan fungeren. U kunt vrijwel in realtime momentopname van Resourcegebruik (CPU, geheugen, network(in/out), enz.) weergeven op een computer met de gateway en het aantal gelijktijdige taken uitgevoerd versus limiet in de Azure portal, Zie [Monitor gateway in de portal](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Als u een zware nodig op hybride gegevensverplaatsing met een groot aantal gelijktijdige kopie activiteit wordt uitgevoerd of grote hoeveelheid gegevens hebt te kopiëren, rekening moet houden, [omhoog schalen of uitschalen gateway](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) om te voorkomen beter gebruikmaken van de bron- of inrichten meer resource zorgen kopiëren. 

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron
### <a name="general"></a>Algemeen
Zorg ervoor dat het onderliggende gegevensarchief niet door andere werkbelastingen die worden uitgevoerd op of tegen deze wordt overbelast.

Zie voor Microsoft-gegevensarchieven, [controleren en afstemmen van onderwerpen](#performance-reference) die specifiek zijn voor de opgeslagen gegevens en u gegevens opslaan prestatiekenmerken, reactietijden minimaliseren en maximaliseren van doorvoer laten zien.

Als u gegevens van Blob-opslag naar SQL Data Warehouse kopiëren, overweeg dan **PolyBase** op de prestaties verbeteren. Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden
*(Inclusief Blob storage, Data Lake Store, Amazon S3, on-premises bestandssystemen en lokale HDFS)*

* **Gemiddelde grootte en het aantal bestanden**: Kopieeractiviteit overgedragen gegevens één bestand tegelijk. Met dezelfde hoeveelheid gegevens die moet worden verplaatst, is de totale doorvoer lagere als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Indien mogelijk, daarom kleine bestanden combineren in grote bestanden krijgen hogere doorvoer.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren de [overwegingen voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.
* Voor de **lokaal bestandssysteem** scenario waarin **Data Management Gateway** is vereist, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway) sectie.

### <a name="relational-data-stores"></a>Opgeslagen relationele gegevens
*(Inclusief SQL-Database. SQL datawarehouse; Amazon Redshift; SQL Server-databases. (en Oracle, MySQL, DB2 Teradata, Sybase en PostgreSQL-databases, enz.)*

* **Patroon voor gegevens**: uw tabelschema is van invloed op de doorvoer van de kopie. Een grote rijgrootte biedt een betere prestaties dan kleine rijgrootte dezelfde hoeveelheid gegevens te kopiëren. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of opgeslagen procedure**: optimaliseren van de logica van de query of een opgeslagen procedure die u opgeeft in de bron van de Kopieeractiviteit efficiënter gegevens ophaalt.
* Voor **lokale relationele databases**, zoals SQL Server en Oracle, waarvoor het gebruik van **Data Management Gateway**, Zie de [overwegingen voor Data Management Gateway](#considerations-on-data-management-gateway) sectie.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink
### <a name="general"></a>Algemeen
Zorg ervoor dat het onderliggende gegevensarchief niet door andere werkbelastingen die worden uitgevoerd op of tegen deze wordt overbelast.

Raadpleeg voor Microsoft-gegevensarchieven, [controleren en afstemmen van onderwerpen](#performance-reference) die specifiek zijn voor de gegevensarchieven. Deze onderwerpen kunnen u helpen begrijpen data store prestatiekenmerken en hoe minimaliseren responstijden en het maximaliseren van doorvoer.

Als u gegevens van kopieert **Blob storage** naar **SQL Data Warehouse**, kunt u overwegen **PolyBase** op de prestaties verbeteren. Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden
*(Inclusief Blob storage, Data Lake Store, Amazon S3, on-premises bestandssystemen en lokale HDFS)*

* **Kopieer gedrag**: als u gegevens in een store, andere bestandsgebaseerde gegevens kopieert, Kopieeractiviteit heeft drie opties via de **copyBehavior** eigenschap. Deze hiërarchie van bewaart, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Voor het behouden of plat hiërarchie een weinig of geen prestatieoverhead, maar samenvoegen van bestanden veroorzaakt prestatieoverhead te verhogen.
* **Bestand-indeling en compressie**: Zie de [overwegingen voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om prestaties te verbeteren.
* **BLOB-opslag**: op dit moment Blob storage ondersteunt alleen blok-blobs voor geoptimaliseerde gegevensoverdracht en doorvoer.
* Voor **lokale bestandssystemen** scenario's waarvoor het gebruik van **Data Management Gateway**, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway) sectie.

### <a name="relational-data-stores"></a>Opgeslagen relationele gegevens
*(Inclusief SQL-Database, SQL Data Warehouse, SQL Server-databases en Oracle-databases)*

* **Kopieer gedrag**: afhankelijk van de eigenschappen die u hebt ingesteld voor **sqlSink**, Kopieeractiviteit schrijft gegevens naar de doeldatabase op verschillende manieren.
  * Standaard toevoegen data movement service gebruikt de bulksgewijs kopiëren API om gegevens in te voegen modus, die de beste prestaties biedt.
  * Als u een opgeslagen procedure in de sink is geconfigureerd, geldt de database de ene gegevensrij tegelijk in plaats van als bulksgewijs laden. Prestaties aanzienlijk verwijderd. Als uw gegevensset groot is, indien van toepassing, overweeg over te schakelen voor het gebruik van de **sqlWriterCleanupScript** eigenschap.
  * Als u configureert de **sqlWriterCleanupScript** eigenschap voor elke kopie-activiteit uitgevoerd, de service activeert het script en u de bulksgewijs kopiëren API gebruiken voor het invoegen van de gegevens. Bijvoorbeeld, als u wilt overschrijven de gehele tabel met de meest recente gegevens, kunt u een script voor het eerst alle records verwijderd voordat de nieuwe gegevens van de bron voor bulksgewijs laden.
* **Gegevensgrootte patroon en batch**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren dezelfde hoeveelheid gegevens, biedt een grote rijgrootte u betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van gegevens worden doorgevoerd.
  * Kopieeractiviteit voegt de gegevens in een reeks van batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens altijd kleine rijen, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere overhead van de batch- en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een exemplaar is mislukt vanwege overbelasting van de database.
* Voor **lokale relationele databases** , zoals SQL Server en Oracle, waarvoor het gebruik van **Data Management Gateway**, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway)sectie.

### <a name="nosql-stores"></a>NoSQL-opslag
*(Inclusief Table storage en Azure Cosmos DB)*

* Voor **tabel opslag**:
  * **Partitie**: vermindert de prestaties aanzienlijk schrijven van gegevens naar interleaved partities. Sorteren van de brongegevens op partitiesleutel, zodat de gegevens efficiënt in één partitie wordt ingevoegd na de andere, of aanpassen van de logica voor het schrijven van de gegevens aan één partitie.
* Voor **Azure Cosmos DB**:
  * **Batchgrootte**: de **writeBatchSize** eigenschap stelt u het aantal parallelle aanvragen naar de service Azure Cosmos DB om documenten te maken. U kunt betere prestaties verwachten wanneer u verhogen **writeBatchSize** omdat meer parallelle aanvragen worden verzonden naar Azure Cosmos DB. Gecontroleerd echter beperken wanneer u schrijft naar Azure Cosmos-DB (het foutbericht is 'aanvraagsnelheid is hoog'). Verschillende factoren kunnen leiden tot het aantal termen in de documenten en de doelverzameling indexeringsbeleid beperking, met inbegrip van de grootte van het document. Overweeg een betere verzameling, bijvoorbeeld S3 zodat hogere doorvoer van de kopie.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie
Serialisatie en deserialisatie kunnen optreden wanneer uw gegevensset invoer of uitvoer gegevensset een bestand is. Zie [ondersteunde indelingen voor bestands- en compressie](data-factory-supported-file-and-compression-formats.md) met meer informatie over ondersteunde bestandsindelingen door Kopieeractiviteit.

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven op basis van bestanden:
  * Een binaire kopiëren zonder een serialiseren of deserialiseren als invoer en uitvoer gegevenssets beide zijn met dezelfde of geen bestandsindelingsinstellingen, data movement service worden uitgevoerd. Er is een hogere doorvoer in vergelijking met het scenario, waarin de bron- en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer en uitvoer gegevenssets beide zijn in tekstindeling en alleen de codering type anders is, data movement service biedt alleen codering conversie. Deze bevat niet alle serialisatie en deserialisatie, waardoor overhead vergeleken met een kopie van de binaire prestaties.
  * Als invoer en uitvoer gegevenssets beide zijn met verschillende bestandsindelingen of verschillende configuraties, zoals scheidingstekens, data movement service deserializes brongegevens stream, transformeren en deze vervolgens in de indeling van de uitvoer die u hebt opgegeven te serialiseren. Deze bewerking leidt tot een veel grotere rol performance overhead ten opzichte van andere scenario's.
* Als u kopieert bestanden naar een gegevensopslag die niet op basis van bestanden (bijvoorbeeld in een store, op basis van bestanden naar een relationele opslag), is het serialiseren of deserialiseren stap vereist. Deze stap resulteert in belangrijke prestatieverbetering overhead.

**Bestandsindeling**: de door u gekozen bestandsindeling kan invloed hebben op prestaties van de kopie. Avro is bijvoorbeeld een compacte binaire indeling die de metagegevens met gegevens opslaat. Brede ondersteuning heeft in het Hadoop-ecosysteem voor het verwerken en het uitvoeren van query's. Avro is echter kostbaar voor serialisatie en deserialisatie, wat tot lagere kopie doorvoer in vergelijking met tekst leidt. Controleer uw keuze van de bestandsindeling in de stroom verwerken holistische. Beginnen met welke vorm van de gegevens wordt opgeslagen in de gegevensarchieven bron of moet worden geëxtraheerd in externe systemen; de beste indeling voor opslag, analytische verwerking en doorzoeken; en in welke indeling moet de gegevens worden geëxporteerd naar de datamarts voor hulpprogramma's voor rapportage en visualisatie. Soms een bestandsindeling die is suboptimale voor lees- en schrijfprestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie
Als uw gegevensset invoer- of een bestand is, kunt u Kopieeractiviteit compressie of decompressie uitvoeren zoals het schrijft gegevens naar de bestemming instellen. Wanneer u compressie kiest, maakt u een afweging tussen het input/output (I/O) en de CPU. Comprimeren van de gegevens van in rekenresources extra kosten. Maar in ruil vermindert het i/o-netwerk en opslag. Afhankelijk van uw gegevens ziet u een verbetering van de totale doorvoer van de kopie.

**Codec**: Kopieeractiviteit gzip, bzip2 en Deflate compressietypen ondersteunt. Azure HDInsight gebruiken alle drie typen voor verwerking. Elke compressiecodec heeft voordelen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u de beste prestaties van Hive-query met bzip2 niet ophalen omdat u deze voor verwerking verdelen kunt. Gzip is de optie meest taakverdeling en het vaakst wordt gebruikt. Kies de codec die het beste past bij uw scenario end-to-end.

**Niveau**: U kunt kiezen uit twee opties voor elke compressiecodec: snelste gecomprimeerd en optimaal gecomprimeerd. De snelste gecomprimeerde optie comprimeert de gegevens zo snel mogelijk, zelfs als het resulterende bestand is niet optimaal gecomprimeerd. De optie optimaal gecomprimeerde veel meer tijd kwijt is aan compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Overweging**: overweeg het gebruik van tussentijdse blob-opslag met compressie voor het kopiëren van een grote hoeveelheid gegevens tussen een winkel lokale en de cloud. Tijdelijke opslag is nuttig wanneer de bandbreedte van het bedrijfsnetwerk en uw Azure-services de beperkende factor is en u wilt dat de gegevensset invoer en uitvoer gegevensset beide in niet-gecomprimeerde vorm. U kunt meer specifiek, een enkel kopieeractiviteit opsplitsen in twee kopie activiteiten. Het kopiëren van de eerste activiteit wordt opgehaald uit de bron naar een tijdelijke of staging-blob in gecomprimeerde vorm. De tweede kopieeractiviteit kopieert de gecomprimeerde gegevens van fasering, en vervolgens decomprimeert terwijl deze naar de sink schrijft.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing
U kunt instellen de **columnMappings** eigenschap in de kopieerbewerking alle kaart of een subset van de invoerkolommen voor de uitvoerkolommen. Nadat data movement service de gegevens van de bron leest, moet deze kolomtoewijzing uitvoeren op de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking, vermindert kopie doorvoer.

Als uw brongegevensarchief waarop, bijvoorbeeld als het een relationele archief zoals SQL-Database of SQL Server of als het een NoSQL-archief zoals Table storage of Azure Cosmos DB, kunt u pushen van de kolom voor het filteren en volgorde van de logica voor de **query** eigenschap in plaats van de kolomtoewijzing. Op deze manier de projectie treedt op wanneer data movement service leest de gegevens uit het brongegevensarchief waar het is veel efficiënter.

## <a name="other-considerations"></a>Andere overwegingen
Als de grootte van gegevens die u wilt kopiëren groot is, kunt u uw bedrijfslogica aan verdere partitie de gegevens met behulp van het mechanisme voor segmenteringshulplijnen in Gegevensfactory aanpassen. Plan de Kopieeractiviteit vaker uitgevoerd om deze te verkleinen van de gegevens voor elke activiteit kopie die wordt uitgevoerd.

Wees voorzichtig bij het aantal gegevenssets en kopiëren activiteiten Data Factory met hetzelfde gegevensarchief-connector op hetzelfde moment vereisen. Veel gelijktijdige kopie taken mogelijk een gegevensarchief beperken en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen kan fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeldscenario: kopiëren vanuit een lokale SQL Server naar Blob storage
**Scenario**: een pijplijn om gegevens te kopiëren van een lokale SQL Server naar Blob storage in CSV-indeling is gebouwd. Als u de kopieertaak sneller, moeten de CSV-bestanden in de notatie bzip2 worden gecomprimeerd.

**Test- en analyse**: de doorvoer van de kopieerbewerking is minder dan 2 MBps veel lager is dan de benchmark prestaties.

**Analyse van prestaties en het afstemmen**: voor het oplossen van het prestatieprobleem bekijken we hoe de gegevens worden verwerkt en verplaatst.

1. **Gegevens lezen**: Gateway opent een verbinding met SQL Server en de query te verzenden. SQL Server reageert met de gegevensstroom Gateway verzenden via het intranet.
2. **Serialiseren en gegevens comprimeren**: Gateway serialiseert de gegevensstroom naar de CSV-indeling en de gegevens naar een stream bzip2 worden gecomprimeerd.
3. **Schrijven van gegevens**: Gateway bzip2 stream uploadt naar blobopslag via Internet.

Zoals u ziet de gegevens worden verwerkt en verplaatst streaming redirector: SQL Server > LAN > Gateway > WAN > Blob storage. **De algehele prestaties worden geregeld door de minimale doorvoer in de pijplijn**.

![Gegevensstroom](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

De prestatieknelpunt mogelijk worden veroorzaakt door een of meer van de volgende factoren:

* **Bron**: SQL-Server zelf lage doorvoer is vanwege een zware belasting.
* **Data Management Gateway**:
  * **LAN**: Gateway bevindt deze formule de SQL Server-machine en een lage bandbreedte verbinding heeft.
  * **Gateway**: Gateway heeft bereikt load beperkingen als u wilt de volgende bewerkingen uitvoeren:
    * **Serialisatie**: trage doorvoer serialiseren van de gegevensstroom naar de CSV-indeling heeft.
    * **Compressie**: U hebt ervoor gekozen een trage compressiecodec (bijvoorbeeld, bzip2, namelijk 2,8 MBps met Core i7).
  * **WAN**: is een lage bandbreedte tussen het bedrijfsnetwerk en uw Azure-services (bijvoorbeeld T1 = 1,544 kbps. T2 = 6,312 kbps).
* **Sink**: Blob-opslag heeft lage doorvoer. (Dit scenario is niet waarschijnlijk omdat een minimum van 60 MBps wordt gegarandeerd dat de SLA.)

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="sample-scenarios-use-parallel-copy"></a>Voorbeeld van scenario's: parallelle kopie gebruiken
**Scenario I:** kopie 1.000 1 MB bestanden van de on-premises bestandssysteem naar Blob storage.

**Analyse en prestatieafstemming**: voor een voorbeeld: als u gateway hebt geïnstalleerd op een machine quad-core, Data Factory maakt gebruik van 16 parallelle kopieën om bestanden te verplaatsen van het bestandssysteem naar Blob storage gelijktijdig. Deze parallelle uitvoering moet resulteren in hoge doorvoersnelheden. U kunt ook expliciet opgeven het aantal parallelle exemplaren. Wanneer u veel kleine bestanden kopieert, helpen parallelle kopieën doorvoer aanzienlijk bij het effectiever gebruik van bronnen.

![Scenario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scenario II**: 20 blobs van 500 MB kopiëren van Blob-opslag naar Data Lake Store Analytics en vervolgens prestaties afstemmen.

**Analyse en prestatieafstemming**: In dit scenario Data Factory kopieert de gegevens uit Blob storage naar Data Lake Store met behulp van één exemplaar (**parallelCopies** ingesteld op 1) en één cloud data movement eenheden. De doorvoer u merkt dicht bij die een beschrijving van de [prestaties verwijzing sectie](#performance-reference).   

![Scenario 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scenario III**: afzonderlijk bestand is groter dan tientallen MB en het totale volume is groot.

**Analyse en prestaties schakelen**: verhogen **parallelCopies** vanwege de bronbeperkingen van een één-cloud-DMU niet resulteren in betere prestaties van de kopie. In plaats daarvan moet u meer cloud DMUs meer bronnen voor het uitvoeren van de gegevensverplaatsing ophalen. Geef een waarde op voor de **parallelCopies** eigenschap. Data Factory verwerkt de parallelle uitvoering voor u. In dit geval als u instelt **cloudDataMovementUnits** moet 4, een doorvoer van over vier keer voordoet.

![Scenario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referentie
Hier zijn prestaties controleren en afstemmen verwijzingen voor enkele van de ondersteunde gegevensarchieven:

* Azure-opslag (inclusief Blob storage en Table storage): [schaalbaarheidsdoelen van Azure Storage](../../storage/common/storage-scalability-targets.md) en [controlelijst voor prestaties en schaalbaarheid van Azure Storage](../../storage/common/storage-performance-checklist.md)
* Azure SQL Database: U kunt [de prestaties controleren](../../sql-database/sql-database-single-database-monitor.md) en controleert u het percentage database transaction unit (DTU)
* Azure SQL datawarehouse: De mogelijkheid wordt gemeten in datawarehouse units (dwu's); Zie [beheren rekencapaciteit in Azure SQL Data Warehouse (overzicht)](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [prestatieniveaus in Azure Cosmos-DB](../../cosmos-db/performance-levels.md)
* Lokale SQL Server: [bewaakt en geoptimaliseerd voor prestaties](https://msdn.microsoft.com/library/ms189081.aspx)
* Lokale bestandsserver: [prestaties afstemmen voor bestandsservers](https://msdn.microsoft.com/library/dn567661.aspx)
