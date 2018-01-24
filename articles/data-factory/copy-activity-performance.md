---
title: "Prestatieafstemming handleiding en prestaties van de Kopieeractiviteit in Azure Data Factory kopiëren | Microsoft Docs"
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de gegevensverplaatsing in Azure Data Factory als u Kopieeractiviteit gebruiken.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: jingwang
ms.openlocfilehash: 53f2b59e57d49a409552aebbdb1b0e81ccd5200c
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Prestaties van de activiteit en prestatieafstemming handleiding kopiëren
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-activity-performance.md)
> * [Versie 2 - Preview](copy-activity-performance.md)


Azure Data Factory-Kopieeractiviteit biedt een uitstekende gegevens veilig, betrouwbaar en hoge prestaties bij het laden van oplossing. Deze Hiermee kunt u voor het kopiëren van tientallen terabytes aan gegevens elke dag op uitgebreide tal van cloud en het on-premises gegevensopslagexemplaren. Razendsnelle snel gegevens laden van prestaties is sleutel zodat u zich kunt richten op het probleem van core 'big data': bouwen van oplossingen voor geavanceerde analyses om inzichten te verkrijgen grondige van die gegevens.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [prestaties van de activiteit in de Data Factory versie 1 kopiëren](v1/data-factory-copy-activity-performance.md).

Azure biedt een reeks bedrijfsniveau oplossingen voor opslag en data warehouse en Kopieeractiviteit biedt een maximaal geoptimaliseerd gegevens te laden ervaring die eenvoudig te configureren en instellen. Met slechts één exemplaar-activiteit, kunt u bereiken:

* Laden van gegevens in **Azure SQL Data Warehouse** op **1,2 GBps**.
* Laden van gegevens in **Azure Blob storage** op **1.0 GBps**
* Laden van gegevens in **Azure Data Lake Store** op **1.0 GBps**

Dit artikel wordt beschreven:

* [Prestaties nummers](#performance-reference) voor ondersteunde gegevensarchieven bron- en sink voor het plannen van uw project.
* Functies die de kopie doorvoer in verschillende scenario's, inclusief kunnen stimuleren [cloud Data Movement Units](#cloud-data-movement-units), [kopie parallelle](#parallel-copy), en [kopie gefaseerde](#staged-copy);
* [Prestaties afstemmen richtlijnen](#performance-tuning-steps) op het afstemmen van de prestaties en de belangrijkste factoren die invloed op prestaties van de kopie hebben kunnen.

> [!NOTE]
> Als u niet bekend met de Kopieeractiviteit in het algemeen bent, Zie [activiteit overzicht](copy-activity-overview.md) voordat u dit artikel leest.
>

## <a name="performance-reference"></a>Verwijzing voor prestaties

Als een verwijzing onderstaande tabel ziet u het nummer van de doorvoer kopiëren **in MBps** voor de opgegeven bron- en sink-paren **in een enkel exemplaar activiteit uitgevoerd** op basis van een intern testen. Ter vergelijking: u ziet ook hoe verschillende instellingen van [cloud data movement eenheden](#cloud-data-movement-units) of [Self-hosted integratie Runtime schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime) (meerdere knooppunten) kunt u op de prestaties van de kopie.

![Matrix van prestaties](./media/copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>In Azure Data Factory versie 2 als de kopieerbewerking wordt uitgevoerd op een Runtime-integratie van Azure de minimale toegestane cloud data movement eenheden is twee. Als niet wordt opgegeven, raadpleegt u data movement standaardeenheden wordt gebruikt in [cloud data movement eenheden](#cloud-data-movement-units).

Verwijst naar Let op:

* Doorvoer wordt berekend met behulp van de volgende formule: [grootte van de gegevens lezen uit bron] / [Kopieeractiviteit uitvoeren duur].
* De prestaties verwijzing getallen in de tabel zijn gemeten met behulp van [TPC-H](http://www.tpc.org/tpch/) gegevensset in een activiteit die één exemplaar wordt uitgevoerd.
* In Azure gegevensarchieven zich de bron- en sink in dezelfde Azure-regio.
* Voor hybride exemplaar tussen on-premises en cloud gegevensarchieven, elk knooppunt Self-hosted integratie Runtime werd uitgevoerd op een computer die gescheiden van het gegevensarchief met hieronder specificatie is. Wanneer een enkele activiteit werd uitgevoerd, wordt in de kopieerbewerking slechts een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte verbruikt.
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
> Met behulp van meer gegevens gegevensverplaatsing eenheden (DMUs) dan de standaardwaarde toegestaan maximum DMUs die 32 voor een exemplaar van cloud-naar-cloud activiteit die wordt uitgevoerd, kunt u hogere doorvoer behalen. Bijvoorbeeld, met 100 DMUs bereikt u kopiëren van gegevens van Azure-Blob in Azure Data Lake Store op **1.0GBps**. Zie de [Cloud data movement eenheden](#cloud-data-movement-units) sectie voor meer informatie over deze functie en de ondersteunde scenario. Neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/) meer DMUs aanvragen.

## <a name="cloud-data-movement-units"></a>Cloud data movement-eenheden

Een **cloud gegevensverplaatsing gegevenseenheid (DMU)** is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in de Data Factory. **DMU is alleen van toepassing op [Azure integratie Runtime](concepts-integration-runtime.md#azure-integration-runtime)**, maar niet [Self-hosted integratie Runtime](concepts-integration-runtime.md#self-hosted-integration-runtime).

**De minimale cloud gegevens gegevensverplaatsing eenheden zorgen Kopieeractiviteit uitvoeren is twee.** Als niet wordt opgegeven, bevat de volgende tabel de standaard DMUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario voor kopiëren | Standaard DMUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 4 en 32 liggen, afhankelijk van het aantal en de grootte van de bestanden. |
| Alle andere kopie-scenario 's | 4 |

Om deze standaardinstelling negeren, Geef een waarde op voor de **cloudDataMovementUnits** eigenschap als volgt. De **toegestane waarden** voor de **cloudDataMovementUnits** eigenschap 2, 4, 8, 16 of 32 zijn. De **werkelijke aantal cloud DMUs** dat de kopieerbewerking wordt gebruikt tijdens de uitvoering is gelijk aan of kleiner zijn dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk wanneer u meer eenheden voor een specifieke kopieerbron en sink configureert, de [prestaties verwijzing](#performance-reference).

Hier ziet u de eenheden daadwerkelijk gebruikte cloud data movement voor elke kopie die wordt uitgevoerd in de kopieerbewerking uitvoer bij de bewaking van een activiteit die wordt uitgevoerd. Meer informatie over de details van [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

> [!NOTE]
> Als u meer cloud DMUs voor een hogere doorvoer moet, neem dan contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/). Instellen van 8 en hoger momenteel werkt alleen als u **meerdere bestanden kopiëren van Blob-opslag/Data Lake Store/Amazon S3/cloud FTP-/ cloud SFTP naar een andere cloud gegevensarchieven**.
>

**Voorbeeld:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
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

### <a name="cloud-data-movement-units-billing-impact"></a>Cloud data movement eenheden impact facturering

Deze heeft **belangrijke** te onthouden dat u in rekening worden gebracht op basis van de totale tijd van de kopieerbewerking. De totale duur die u wordt gefactureerd voor gegevensverplaatsing is de som van de duur voor DMUs. Als een taak kopiëren gebruikt voor één uur duren voordat met twee cloud eenheden en nu het met acht cloud eenheden 15 minuten duurt, blijft de algehele factuur bijna hetzelfde.

## <a name="parallel-copy"></a>Parallelle kopie

U kunt de **parallelCopies** eigenschap om aan te geven van de parallelle uitvoering die u wilt dat de Kopieeractiviteit gebruiken. U kunt deze eigenschap zien als het maximum aantal threads in de Kopieeractiviteit die kunnen lezen uit de bron- of schrijven naar uw gegevensarchieven sink parallel.

Data Factory bepaalt voor elke kopie-activiteit is uitgevoerd, het aantal parallelle exemplaren te gebruiken om gegevens te kopiëren van de bron gegevens opgeslagen en wordt opgeslagen in de doelgegevens. Het aantal parallelle exemplaren die worden gebruikt, hangt af van het type van de bron en sink die u gebruikt:

| Scenario voor kopiëren | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van bestanden |Tussen 1 en 64. Afhankelijk van de grootte van de bestanden en het aantal cloud data movement eenheden (DMUs) gebruikt om te kopiëren van gegevens tussen twee cloud gegevensarchieven of de fysieke configuratie van de machine Self-hosted integratie Runtime. |
| Gegevens kopiëren van een brongegevensarchief naar Azure Table storage |4 |
| Alle andere kopie-scenario 's |1 |

Normaal gesproken geeft het standaardgedrag de beste doorvoer. Evenwel waarmee de belasting op computers die als host fungeren van uw gegevens opslaat of als u wilt kopiëren prestaties afstemmen, u kunt de standaardwaarde onderdrukken en een waarde opgeven voor de **parallelCopies** eigenschap. De waarde moet een geheel getal groter dan of gelijk zijn aan 1 zijn. Tijdens de runtime voor de beste prestaties Kopieeractiviteit maakt gebruik van een waarde die kleiner is dan of gelijk aan de waarde die u instelt.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

Verwijst naar Let op:

* Als u gegevens tussen winkels op basis van bestanden, kopieert de **parallelCopies** bepalen de parallelle uitvoering op bestandsniveau. De verdeling in segmenten binnen één bestand gebeurt onder automatisch en transparant en deze is ontworpen voor het gebruik van de meest geschikte chunkgrootte voor een bepaalde bron-gegevenstype store laden van gegevens in de parallelle en rechthoekige naar parallelCopies. Het werkelijke aantal parallelle kopieën data movement service wordt gebruikt voor de kopieerbewerking tijdens de uitvoering is niet meer dan het aantal bestanden die u hebt. Als u het gedrag van de kopie is **mergeFile**, Kopieeractiviteit kan niet profiteren van bestandsniveau parallelle uitvoering.
* Wanneer u een waarde opgeven voor de **parallelCopies** eigenschap, moet u overwegen de toename van de belasting op de bron- en sink-gegevensarchieven en voor Self-Hosted integratie Runtime als de kopieeractiviteit bevoegd is door er bijvoorbeeld voor hybride exemplaar. Dit gebeurt met name wanneer er meerdere activiteiten of gelijktijdige wordt uitgevoerd dezelfde activiteiten die hetzelfde gegevensarchief uitgevoerd. Als u merkt dat het gegevensarchief of het Self-hosted integratie Runtime met de belasting wordt overbelast, vermindert u de **parallelCopies** waarde om te helpen de belasting.
* Als u gegevens van winkels die zich niet op basis van bestanden naar winkels op basis van een bestand kopieert, data movement service negeert de **parallelCopies** eigenschap. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.
* **parallelCopies** staat haaks op **cloudDataMovementUnits**. De voormalige wordt over alle cloud data movement eenheden geteld.

## <a name="staged-copy"></a>Gefaseerde kopiëren

Wanneer u gegevens uit een brongegevensarchief naar een gegevensopslag sink kopiëren, kunt u kiezen om het Blob-opslag gebruiken als een tussentijdse staging store. Fasering is vooral nuttig in de volgende gevallen:

- **U wilt opnemen van gegevens uit verschillende gegevensarchieven in SQL Data Warehouse met PolyBase**. SQL Data Warehouse gebruikmaakt van PolyBase als een mechanisme voor hoge gegevensdoorvoer een grote hoeveelheid gegevens laden in SQL Data Warehouse. Echter de brongegevens moet zich in Blob storage of Azure Data Lake Store en deze moet voldoen aan de aanvullende criteria. Wanneer u gegevens vanuit een ander gegevensarchief dan Blob storage of Azure Data Lake Store laadt, kunt u gegevens kopiëren via fasering tussentijdse Blob-opslag kunt activeren. In dat geval voert Data Factory de vereiste gegevenstransformaties om ervoor te zorgen dat deze voldoet aan de vereisten van PolyBase. Vervolgens wordt PolyBase gegevens laadt in SQL Data Warehouse efficiënt. Zie voor meer informatie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Soms duurt even om uit te voeren van een hybride gegevensverplaatsing (dat wil zeggen, voor het kopiëren van een on-premises gegevens opslaan in een cloud-gegevensarchief) via een trage netwerkverbinding**. Om prestaties te verbeteren, kunt u voorbereide kopiëren naar de gegevens on-premises comprimeren zodat kost het minder tijd om te verplaatsen van gegevens in het gegevensarchief fasering in de cloud vervolgens decomprimeren van de gegevens in het archief staging voordat deze worden geladen in het gegevensarchief bestemming.
- **U niet wilt openen van poorten dan poort 80 en 443 in uw firewall poort vanwege zakelijke IT-beleid**. Wanneer u gegevens uit een on-premises gegevensopslag naar een Azure SQL Database-sink of een Azure SQL Data Warehouse sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en uw bedrijfsfirewall activeren. In dit scenario gefaseerde kopie kunt profiteren van de Runtime Self-hosted integratie eerst om gegevens te kopiëren naar een Blob storage exemplaar via HTTP of HTTPS op poort 443 voor fasering en vervolgens de gegevens in SQL-Database of SQL Data Warehouse laden van fasering van Blob-opslag. In deze stroom moet u geen poort 1433 inschakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerde kopiëren werkt

Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens gekopieerd uit de gegevensopslag van bron naar de tijdelijke Blob-opslag (bring uw eigen). Vervolgens is de gegevens uit de staging gegevensopslag gekopieerd naar het sink-gegevensarchief. De stroom twee fasen voor u worden beheerd, Data Factory. Tijdelijke gegevens uit de tijdelijke opslag ruimt Data Factory ook nadat de gegevensverplaatsing voltooid is.

![Gefaseerde kopiëren](media/copy-activity-performance/staged-copy.png)

Wanneer u verplaatsing van gegevens met behulp van een gefaseerde installatie archief activeert, kunt u opgeven of u wilt dat de gegevens te comprimeren voordat u gegevens uit de gegevensopslag van de bron naar een tijdelijke of staging gegevensarchief, en vervolgens gedecomprimeerd voordat het verplaatsen van gegevens vanuit een tussentijdse of staging-gegevens opslaan in het gegevensarchief sink.

Op dit moment kunt kopiëren u gegevens tussen twee on-premises gegevensopslagexemplaren met behulp van een gefaseerde installatie archief niet.

### <a name="configuration"></a>Configuratie

Configureer de **enableStaging** instellen in de kopieerbewerking om op te geven of u wilt dat de gegevens naar het tijdelijk opgeslagen in blobopslag voordat u deze naar een doelgegevensopslagplaats laden. Als u instelt **enableStaging** naar `TRUE`, geeft u de extra eigenschappen in de volgende tabel weergegeven. Als u niet hebt, moet u ook voor het maken van een Azure Storage of opslag gedeelde handtekening-gekoppelde access-service voor fasering.

| Eigenschap | Beschrijving | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| **enableStaging** |Geef op of u wilt kopiëren van gegevens via een tussentijdse staging-store. |False |Nee |
| **linkedServiceName** |Geef de naam van een [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) gekoppelde service die verwijst naar het exemplaar van de opslag die u als een tussentijdse staging store gebruiken. <br/><br/> U kunt opslag met een shared access signature niet gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| **pad** |Geef het pad voor Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u niet een pad opgeeft, maakt de service een container voor het opslaan van tijdelijke gegevens. <br/><br/> Geef een pad op als u opslag met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| **enableCompression** |Hiermee geeft u op of de gegevens moeten worden gecomprimeerd voordat deze is gekopieerd naar de bestemming. Deze instelling beperkt de hoeveelheid gegevens die worden overgedragen. |False |Nee |

Hier volgt een voorbeeld-definitie van de Kopieeractiviteit met de eigenschappen die worden beschreven in de voorgaande tabel:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Gefaseerde kopie impact facturering

U kosten in rekening gebracht op basis van twee stappen: kopieer duur en type kopiëren.

* Wanneer u in een cloud gekopieerde (gegevens uit een cloud-gegevensarchief kopiëren naar een andere cloud data store, beide fasen gemachtigd door Azure integratie Runtime) voor fasering u in rekening worden gebracht de [som van de duur van de kopie voor stap 1 en 2] x [cloud kopie eenheidsprijs].
* Wanneer u in een hybride gekopieerde (gegevens uit een on-premises gegevensopslag kopiëren naar een cloud-gegevensopslag één fase gemachtigd door Self-hosted integratie Runtime) voor fasering u in rekening worden gebracht [duur hybride kopiëren] x [hybride kopie eenheidsprijs] + [cloud kopie duur] x [cloud kopiëren eenheidsprijs].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen

We raden aan dat u deze stappen nemen om afstemmen van de prestaties van uw Data Factory-service met de Kopieeractiviteit:

1. **Stel een basislijn vast**. Tijdens de ontwikkelingsfase uw pijplijn te testen met behulp van de Kopieeractiviteit tegen een representatieve steekproef. Details van de uitvoering en volgende prestatiekenmerken verzamelen [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

2. **Diagnose en prestaties te optimaliseren**. Als de prestaties die u merkt niet voldoet aan uw verwachtingen, moet u het identificeren van knelpunten. Vervolgens optimaliseren als u wilt verwijderen of verklein het effect van knelpunten. Een volledige beschrijving van de prestatiediagnose valt buiten het bereik van dit artikel, maar hier volgen enkele algemene overwegingen:

   * Prestatiefuncties:
     * [Parallelle kopie](#parallel-copy)
     * [Cloud data movement-eenheden](#cloud-data-movement-units)
     * [Gefaseerde kopiëren](#staged-copy)
     * [Host zichzelf integratie Runtime-schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Host zichzelf integratie Runtime](#considerations-for-self-hosted-integration-runtime)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)

3. **Vouw de configuratie op uw volledige gegevensset**. Wanneer u tevreden met de prestaties en resultaten bent, kunt u de definitie en de pijplijn voor uw volledige gegevensset uitvouwen.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Overwegingen voor host zichzelf integratie Runtime

Als uw kopieeractiviteit wordt uitgevoerd op een Self-hosted integratie Runtime, moet u rekening houden met het volgende:

**Setup**: het is raadzaam dat u een specifieke host Integration-Runtime-machine. Zie [overwegingen voor het gebruik van Self-hosted integratie Runtime](concepts-integration-runtime.md).

**Uitschalen**: met een enkele logische Self-hosted integratie Runtime met een of meer knooppunten meerdere Kopieeractiviteit wordt uitgevoerd op hetzelfde moment gelijktijdig kan fungeren. Als u een zware nodig op hybride gegevensverplaatsing met een groot aantal gelijktijdige kopie activiteit wordt uitgevoerd of grote hoeveelheid gegevens hebt te kopiëren, rekening moet houden, [scale-out Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md#high-availability-and-scalability) om te voorkomen meer resource toe aan inrichten meer mogelijkheden bieden kopiëren.

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet door andere werkbelastingen die worden uitgevoerd op of tegen deze wordt overbelast.

Zie voor Microsoft-gegevensarchieven, [controleren en afstemmen van onderwerpen](#performance-reference) die specifiek zijn voor de opgeslagen gegevens en u gegevens opslaan prestatiekenmerken, reactietijden minimaliseren en maximaliseren van doorvoer laten zien.

* Als u gegevens kopiëren **van Blob-opslag naar SQL Data Warehouse**, kunt u overwegen **PolyBase** op de prestaties verbeteren. Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie.
* Als u gegevens kopiëren **uit HDFS naar Azure Blob-/ Azure Data Lake Store**, kunt u overwegen **DistCp** op de prestaties verbeteren. Zie [DistCp gebruiken om gegevens te kopiëren uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) voor meer informatie.
* Als u gegevens kopiëren **van Redshift naar Azure SQL Data Warehouse/Azure-BLob/Azure Data Lake Store**, kunt u overwegen **UNLOAD** op de prestaties verbeteren. Zie [UNLOAD gebruiken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Gemiddelde grootte en het aantal bestanden**: Kopieeractiviteit overgedragen gegevens één bestand tegelijk. Met dezelfde hoeveelheid gegevens die moet worden verplaatst, is de totale doorvoer lagere als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Indien mogelijk, daarom kleine bestanden combineren in grote bestanden krijgen hogere doorvoer.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren de [overwegingen voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.

### <a name="relational-data-stores"></a>Opgeslagen relationele gegevens

* **Patroon voor gegevens**: uw tabelschema is van invloed op de doorvoer van de kopie. Een grote rijgrootte biedt een betere prestaties dan kleine rijgrootte dezelfde hoeveelheid gegevens te kopiëren. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of opgeslagen procedure**: optimaliseren van de logica van de query of een opgeslagen procedure die u opgeeft in de bron van de Kopieeractiviteit efficiënter gegevens ophaalt.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet door andere werkbelastingen die worden uitgevoerd op of tegen deze wordt overbelast.

Raadpleeg voor Microsoft-gegevensarchieven, [controleren en afstemmen van onderwerpen](#performance-reference) die specifiek zijn voor de gegevensarchieven. Deze onderwerpen kunnen u helpen begrijpen data store prestatiekenmerken en hoe minimaliseren responstijden en het maximaliseren van doorvoer.

* Als u gegevens kopiëren **van Blob-opslag naar SQL Data Warehouse**, kunt u overwegen **PolyBase** op de prestaties verbeteren. Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie.
* Als u gegevens kopiëren **uit HDFS naar Azure Blob-/ Azure Data Lake Store**, kunt u overwegen **DistCp** op de prestaties verbeteren. Zie [DistCp gebruiken om gegevens te kopiëren uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) voor meer informatie.
* Als u gegevens kopiëren **van Redshift naar Azure SQL Data Warehouse/Azure-BLob/Azure Data Lake Store**, kunt u overwegen **UNLOAD** op de prestaties verbeteren. Zie [UNLOAD gebruiken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Kopieer gedrag**: als u gegevens in een store, andere bestandsgebaseerde gegevens kopieert, Kopieeractiviteit heeft drie opties via de **copyBehavior** eigenschap. Deze hiërarchie van bewaart, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Voor het behouden of plat hiërarchie een weinig of geen prestatieoverhead, maar samenvoegen van bestanden veroorzaakt prestatieoverhead te verhogen.
* **Bestand-indeling en compressie**: Zie de [overwegingen voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om prestaties te verbeteren.

### <a name="relational-data-stores"></a>Opgeslagen relationele gegevens

* **Kopieer gedrag**: afhankelijk van de eigenschappen die u hebt ingesteld voor **sqlSink**, Kopieeractiviteit schrijft gegevens naar de doeldatabase op verschillende manieren.
  * Standaard toevoegen data movement service gebruikt de bulksgewijs kopiëren API om gegevens in te voegen modus, die de beste prestaties biedt.
  * Als u een opgeslagen procedure in de sink is geconfigureerd, geldt de database de ene gegevensrij tegelijk in plaats van als bulksgewijs laden. Prestaties aanzienlijk verwijderd. Als uw gegevensset groot is, indien van toepassing, overweeg over te schakelen voor het gebruik van de **preCopyScript** eigenschap.
  * Als u configureert de **preCopyScript** eigenschap voor elke kopie-activiteit uitgevoerd, de service activeert het script en u de bulksgewijs kopiëren API gebruiken voor het invoegen van de gegevens. Bijvoorbeeld, als u wilt overschrijven de gehele tabel met de meest recente gegevens, kunt u een script voor het eerst alle records verwijderd voordat de nieuwe gegevens van de bron voor bulksgewijs laden.
* **Gegevensgrootte patroon en batch**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren dezelfde hoeveelheid gegevens, biedt een grote rijgrootte u betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van gegevens worden doorgevoerd.
  * Kopieeractiviteit voegt de gegevens in een reeks van batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens altijd kleine rijen, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere overhead van de batch- en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een exemplaar is mislukt vanwege overbelasting van de database.

### <a name="nosql-stores"></a>NoSQL-opslag

* Voor **tabel opslag**:
  * **Partitie**: vermindert de prestaties aanzienlijk schrijven van gegevens naar interleaved partities. Sorteren van de brongegevens op partitiesleutel, zodat de gegevens efficiënt in één partitie wordt ingevoegd na de andere, of aanpassen van de logica voor het schrijven van de gegevens aan één partitie.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie

Serialisatie en deserialisatie kunnen optreden wanneer uw gegevensset invoer of uitvoer gegevensset een bestand is. Zie [ondersteunde indelingen voor bestands- en compressie](supported-file-formats-and-compression-codecs.md) met meer informatie over ondersteunde bestandsindelingen door Kopieeractiviteit.

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven op basis van bestanden:
  * Als invoer en uitvoer gegevenssets beide zijn met dezelfde of geen bestandsindelingsinstellingen, data movement service wordt uitgevoerd een **binaire kopiëren** zonder serialisatie of deserialisatie. Er is een hogere doorvoer in vergelijking met het scenario, waarin de bron- en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer en uitvoer gegevenssets beide zijn in tekstindeling en alleen de codering type anders is, data movement service biedt alleen codering conversie. Deze bevat niet alle serialisatie en deserialisatie, waardoor overhead vergeleken met een kopie van de binaire prestaties.
  * Als invoer en uitvoer gegevenssets beide zijn met verschillende bestandsindelingen of verschillende configuraties, zoals scheidingstekens, data movement service deserializes brongegevens stream, transformeren en deze vervolgens in de indeling van de uitvoer die u hebt opgegeven te serialiseren. Deze bewerking leidt tot een veel grotere rol performance overhead ten opzichte van andere scenario's.
* Als u kopieert bestanden naar een gegevensopslag die niet op basis van bestanden (bijvoorbeeld in een store, op basis van bestanden naar een relationele opslag), is het serialiseren of deserialiseren stap vereist. Deze stap resulteert in belangrijke prestatieverbetering overhead.

**Bestandsindeling**: de door u gekozen bestandsindeling kan invloed hebben op prestaties van de kopie. Avro is bijvoorbeeld een compacte binaire indeling die de metagegevens met gegevens opslaat. Brede ondersteuning heeft in het Hadoop-ecosysteem voor het verwerken en het uitvoeren van query's. Avro is echter kostbaar voor serialisatie en deserialisatie, wat tot lagere kopie doorvoer in vergelijking met tekst leidt. Controleer uw keuze van de bestandsindeling in de stroom verwerken holistische. Beginnen met welke vorm van de gegevens wordt opgeslagen in de gegevensarchieven bron of moet worden geëxtraheerd in externe systemen; de beste indeling voor opslag, analytische verwerking en doorzoeken; en in welke indeling moet de gegevens worden geëxporteerd naar de datamarts voor hulpprogramma's voor rapportage en visualisatie. Soms een bestandsindeling die is suboptimale voor lees- en schrijfprestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie

Als uw gegevensset invoer- of een bestand is, kunt u Kopieeractiviteit compressie of decompressie uitvoeren zoals het schrijft gegevens naar de bestemming instellen. Wanneer u compressie kiest, maakt u een afweging tussen het input/output (I/O) en de CPU. Comprimeren van de gegevens van in rekenresources extra kosten. Maar in ruil vermindert het i/o-netwerk en opslag. Afhankelijk van uw gegevens ziet u een verbetering van de totale doorvoer van de kopie.

**Codec**: elke compressiecodec heeft voordelen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u de beste prestaties van Hive-query met bzip2 niet ophalen omdat u deze voor verwerking verdelen kunt. Gzip is de optie meest taakverdeling en het vaakst wordt gebruikt. Kies de codec die het beste past bij uw scenario end-to-end.

**Niveau**: U kunt kiezen uit twee opties voor elke compressiecodec: snelste gecomprimeerd en optimaal gecomprimeerd. De snelste gecomprimeerde optie comprimeert de gegevens zo snel mogelijk, zelfs als het resulterende bestand is niet optimaal gecomprimeerd. De optie optimaal gecomprimeerde veel meer tijd kwijt is aan compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Overweging**: als u wilt kopiëren van een grote hoeveelheid gegevens tussen een winkel lokale en de cloud, kunt u overwegen [kopie gefaseerde](#staged-copy) met gebruik van compressie. Tijdelijke opslag is nuttig wanneer de bandbreedte van het bedrijfsnetwerk en uw Azure-services de beperkende factor is en u wilt dat de gegevensset invoer en uitvoer gegevensset beide in niet-gecomprimeerde vorm.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing

U kunt instellen de **columnMappings** eigenschap in de kopieerbewerking alle kaart of een subset van de invoerkolommen voor de uitvoerkolommen. Nadat data movement service de gegevens van de bron leest, moet deze kolomtoewijzing uitvoeren op de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking, vermindert kopie doorvoer.

Als uw brongegevensarchief waarop, bijvoorbeeld als het een relationele archief zoals SQL-Database of SQL Server of als het een NoSQL-archief zoals Table storage of Azure Cosmos DB, kunt u pushen van de kolom voor het filteren en volgorde van de logica voor de **query** eigenschap in plaats van de kolomtoewijzing. Op deze manier de projectie treedt op wanneer data movement service leest de gegevens uit het brongegevensarchief waar het is veel efficiënter.

Klik hier als u meer wilt weten van [Kopieeractiviteit schematoewijzing](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Andere overwegingen

Als de grootte van gegevens die u wilt kopiëren groot is, kunt u uw bedrijfslogica als u wilt nog verder partitie de gegevens en plannen van de Kopieeractiviteit vaker uitgevoerd om deze te verkleinen van de gegevens voor elke Kopieeractiviteit uitgevoerd aanpassen.

Wees voorzichtig bij het aantal gegevenssets en kopiëren activiteiten Data Factory verbinding maken met hetzelfde gegevensarchief op hetzelfde moment vereisen. Veel gelijktijdige kopie taken mogelijk een gegevensarchief beperken en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen kan fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeldscenario: kopiëren vanuit een lokale SQL Server naar Blob storage

**Scenario**: een pijplijn om gegevens te kopiëren van een lokale SQL Server naar Blob storage in CSV-indeling is gebouwd. Als u de kopieertaak sneller, moeten de CSV-bestanden in de notatie bzip2 worden gecomprimeerd.

**Test- en analyse**: de doorvoer van de kopieerbewerking is minder dan 2 MBps veel lager is dan de benchmark prestaties.

**Analyse van prestaties en het afstemmen**: voor het oplossen van het prestatieprobleem bekijken we hoe de gegevens worden verwerkt en verplaatst.

1. **Gegevens lezen**: integratie runtime opent een verbinding met SQL Server en de query te verzenden. SQL Server reageert met het verzenden van de gegevensstroom naar de runtime via het intranet integratie.
2. **Serialiseren en gegevens comprimeren**: integratie runtime serialiseert de gegevensstroom naar de CSV-indeling en de gegevens naar een stream bzip2 worden gecomprimeerd.
3. **Schrijven van gegevens**: integratie runtime bzip2 stream uploadt naar blobopslag via Internet.

Zoals u ziet de gegevens worden verwerkt en verplaatst streaming redirector: SQL Server > LAN > integratie runtime > WAN > Blob storage. **De algehele prestaties worden geregeld door de minimale doorvoer in de pijplijn**.

![Gegevensstroom](./media/copy-activity-performance/case-study-pic-1.png)

De prestatieknelpunt mogelijk worden veroorzaakt door een of meer van de volgende factoren:

* **Bron**: SQL-Server zelf lage doorvoer is vanwege een zware belasting.
* **Integratie Runtime zelf de hosting**:
  * **LAN**: integratie runtime bevindt deze formule de SQL Server-machine en een lage bandbreedte verbinding heeft.
  * **Integratie runtime**: integratie runtime heeft bereikt load beperkingen als u wilt de volgende bewerkingen uitvoeren:
    * **Serialisatie**: trage doorvoer serialiseren van de gegevensstroom naar de CSV-indeling heeft.
    * **Compressie**: U hebt ervoor gekozen een trage compressiecodec (bijvoorbeeld, bzip2, namelijk 2,8 MBps met Core i7).
  * **WAN**: is een lage bandbreedte tussen het bedrijfsnetwerk en uw Azure-services (bijvoorbeeld T1 = 1,544 kbps. T2 = 6,312 kbps).
* **Sink**: Blob-opslag heeft lage doorvoer. (Dit scenario is niet waarschijnlijk omdat een minimum van 60 MBps wordt gegarandeerd dat de SLA.)

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="reference"></a>Referentie

Hier zijn prestaties controleren en afstemmen verwijzingen voor enkele van de ondersteunde gegevensarchieven:

* Azure-opslag (inclusief Blob storage en Table storage): [schaalbaarheidsdoelen van Azure Storage](../storage/common/storage-scalability-targets.md) en [controlelijst voor prestaties en schaalbaarheid van Azure Storage](../storage/common/storage-performance-checklist.md)
* Azure SQL Database: U kunt [de prestaties controleren](../sql-database/sql-database-single-database-monitor.md) en controleert u het percentage database transaction unit (DTU)
* Azure SQL datawarehouse: De mogelijkheid wordt gemeten in datawarehouse units (dwu's); Zie [beheren rekencapaciteit in Azure SQL Data Warehouse (overzicht)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [prestatieniveaus in Azure Cosmos-DB](../cosmos-db/performance-levels.md)
* Lokale SQL Server: [bewaakt en geoptimaliseerd voor prestaties](https://msdn.microsoft.com/library/ms189081.aspx)
* Lokale bestandsserver: [prestaties afstemmen voor bestandsservers](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen voor de Kopieeractiviteit:

- [Activiteit-overzicht](copy-activity-overview.md)
- [Schematoewijzing voor kopiëren-activiteit](copy-activity-schema-and-type-mapping.md)
- [Activiteit fouttolerantie kopiëren](copy-activity-fault-tolerance.md)
