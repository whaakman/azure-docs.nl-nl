---
title: Kopiëren en afstemmingshandleiding van activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de verplaatsing van gegevens in Azure Data Factory wanneer u een Kopieeractiviteit gebruiken.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: jingwang
ms.openlocfilehash: 47b9ede2d529f78b14c21f53c6cd18ed691a3df3
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445828"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiëren en afstemmingshandleiding van activiteit
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-copy-activity-performance.md)
> * [Huidige versie](copy-activity-performance.md)


Copy Activity in Azure Data Factory biedt een eersteklas veilige, betrouwbare en hoogwaardige gegevens laden. Dit kunt u gegevens kunt kopiëren tientallen van terabytes aan gegevens elke dag in uitgebreide tal van cloud en het on-premises gegevensarchieven. Zeer snel gegevens laden van prestaties is erg belangrijk om te controleren of u zich kunt richten op het probleem van de 'big data' core: het bouwen van geavanceerde analyseoplossingen en diepe inzichten ophalen uit alle gegevens.

Azure biedt een set geavanceerde datawarehouseoplossingen voor opslag en gegevensbeheer en Copy Activity biedt een uiterst geoptimaliseerde gegevens laden die gemakkelijk te configureren en instellen. Met slechts één kopieeractiviteit, kunt u bereiken:

* Het laden van gegevens in **Azure SQL Data Warehouse** op **1,2 GBps**.
* Het laden van gegevens in **Azure Blob-opslag** op **1,0 GBps**
* Het laden van gegevens in **Azure Data Lake Store** op **1,0 GBps**

Dit artikel wordt beschreven:

* [Naslaginformatie over prestatiecijfers](#performance-reference) voor ondersteunde bron- en sink-gegevensopslag voor het plannen van uw project.
* Functies die de kopie-doorvoer in verschillende scenario's, met inbegrip van kunnen verhogen [integratie gegevenseenheden](#data-integration-units), [kopie parallelle](#parallel-copy), en [gefaseerd kopiëren](#staged-copy);
* [Richtlijnen voor afstemmen van prestaties](#performance-tuning-steps) over het afstemmen van de prestaties en de belangrijkste factoren die de kopieerprestaties kunnen beïnvloeden.

> [!NOTE]
> Als u niet bekend met de Kopieeractiviteit in het algemeen bent, Zie [activiteit overzicht](copy-activity-overview.md) voordat het lezen van dit artikel.
>

## <a name="performance-reference"></a>Naslaginformatie over prestaties

Als een verwijzing naar een onderstaande tabel ziet u het nummer van de doorvoer kopie **in MBps** voor de opgegeven bron en sink-paren **in één enkele kopieeractiviteit** op basis van interne testen. Ter vergelijking: u kunt ook zien hoe de verschillende instellingen van [integratie gegevenseenheden](#data-integration-units) of [zelfgehoste Cloudintegratieruntime schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime) (meerdere knooppunten) kunt u op kopiëren.

![Matrix van prestaties](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Wanneer de kopieeractiviteit wordt uitgevoerd op een Azure Integration Runtime, is de minimaal toegestane integratie gegevenseenheden (voorheen bekend als eenheden voor gegevensverplaatsing) twee. Als niet is opgegeven, ziet u standaard gegevens integratie eenheden wordt gebruikt [integratie gegevenseenheden](#data-integration-units).

Die u moet weten:

* Doorvoer wordt berekend met behulp van de volgende formule: [grootte van de gegevens lezen uit bron] / [Copy Activity uitvoeringsduur].
* De prestaties van verwijzing getallen in de tabel zijn gemeten met behulp van [TPC-H](http://www.tpc.org/tpch/) gegevensset in een activiteit die één exemplaar wordt uitgevoerd. Testbestanden voor bestandsgebaseerde winkels zijn meerdere bestanden met 10GB groot.
* In de Azure-gegevensopslag zijn de bron en sink in dezelfde Azure-regio.
* Voor hybride kopiëren tussen on-premises en cloud gegevensarchieven, elk knooppunt zelfgehoste Cloudintegratieruntime werd uitgevoerd op een computer die gescheiden van het gegevensarchief met onderstaande specificatie is. Als een enkele activiteit werd uitgevoerd, gebruikt de kopieerbewerking alleen een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte.
    <table>
    <tr>
        <td>CPU</td>
        <td>32 kernen 2,20 GHz Intel Xeon E5-2660 v2</td>
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
> U kunt hogere doorvoer bereiken met behulp van meer gegevens integratie eenheden (DIU). Bijvoorbeeld, met 100 DIUs, kunt u bereiken kopiëren van gegevens van Azure-Blob in Azure Data Lake Store op **1.0GBps**. Zie de [integratie gegevenseenheden](#data-integration-units) sectie voor meer informatie over deze functie en de ondersteunde scenario. 

## <a name="data-integration-units"></a>Eenheden van de integratie van gegevens

Een **gegevens integratie eenheid (DIU)** (voorheen bekend als Cloud Data Movement Unit of DMU) is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in Data Factory. **DIU is alleen van toepassing op [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime)**, maar niet [zelfgehoste Cloudintegratieruntime](concepts-integration-runtime.md#self-hosted-integration-runtime).

**De minimale eenheden van de integratie van gegevens om te kopiëren activiteit die wordt uitgevoerd is twee.** Indien niet opgegeven, bevat de volgende tabel de standaard-DIUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario kopiëren | Standaard DIUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 4 en 32 liggen, afhankelijk van het aantal en de grootte van de bestanden. |
| Alle andere kopieerscenario 's | 4 |

Als u wilt deze standaardwaarde onderdrukken, Geef een waarde op voor de **dataIntegrationUnits** eigenschap als volgt te werk. De **toegestane waarden** voor de **dataIntegrationUnits** eigenschap **maximaal 256**. De **werkelijke aantal DIUs** dat gebruikmaakt van de kopieerbewerking tijdens de uitvoering is gelijk aan of kleiner is dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk bij het configureren van meer eenheden voor een specifiek exemplaar gegevensbronnen en sinks, de [prestaties verwijzing](#performance-reference).

Hier ziet u de daadwerkelijk gebruikte eenheden van de integratie van gegevens voor elk exemplaar dat wordt uitgevoerd in de kopieeractiviteit uitvoer bij het bewaken van een activiteit die wordt uitgevoerd. Informatie over de details van [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

> [!NOTE]
> Instellen van DIUs **groter is dan 4** op dit moment werkt alleen als u **meerdere bestanden kopiëren van Blob-opslag/Data Lake Storage/Amazon S3/cloud FTP/cloud SFTP naar een andere cloudgegevens worden opgeslagen.**.
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
            "dataIntegrationUnits": 32
        }
    }
]
```

### <a name="data-integration-units-billing-impact"></a>Data-integratie eenheden facturering impact

Er **belangrijk** te onthouden dat de kosten worden berekend op basis van de totale tijd van de kopieerbewerking. De totale duur die in rekening gebracht voor verplaatsing van gegevens is de som van de duur voor DIUs. Als een kopieertaak voorheen een uur duren voordat met twee cloudeenheden en nu wordt 15 minuten met acht cloudeenheden, blijft de totale factuur bijna hetzelfde.

## <a name="parallel-copy"></a>Parallelle kopiëren

U kunt de **parallelCopies** eigenschap om aan te geven van de parallelle uitvoering die u wilt dat de Kopieeractiviteit om te gebruiken. U kunt deze eigenschap beschouwen als het maximum aantal threads in een Kopieeractiviteit waarmee u kunt vanuit de bron lezen of schrijven naar de sinkgegevensopslag parallel.

Data Factory bepaalt voor elke kopie-activiteit die wordt uitgevoerd, het aantal parallelle exemplaren te gebruiken om gegevens te kopiëren van de bron-gegevens opslaan en op te slaan op de doelgegevens. Het aantal parallelle exemplaren die worden gebruikt, is afhankelijk van het type van de bron en sink die u gebruikt:

| Scenario kopiëren | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van bestanden |Afhankelijk van de grootte van de bestanden en het aantal eenheden van de integratie van gegevens (DIUs) gebruikt om te kopiëren van gegevens tussen twee cloudlocaties voor gegevensopslag, of de fysieke configuratie van de machine zelfgehoste Cloudintegratieruntime. |
| Gegevens kopiëren van een brongegevensarchief naar Azure Table storage |4 |
| Alle andere kopieerscenario 's |1 |

> [!TIP]
> Bij het kopiëren van gegevens tussen winkels op basis van bestanden, het standaardgedrag (automatisch bepaald) doorgaans geeft u de beste doorvoer. 

Voor het beheren van de belasting van de machines waarop uw gegevens opslaat, of als u wilt kopiëren-prestaties afstemmen, kunt u besluiten de standaardwaarde overschrijven en geef een waarde op voor de **parallelCopies** eigenschap. De waarde moet een geheel getal groter dan of gelijk aan 1 zijn. Tijdens de uitvoering voor de beste prestaties Copy-activiteit maakt gebruik van een waarde die kleiner is dan of gelijk zijn aan de waarde die u hebt ingesteld.

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

Die u moet weten:

* Als u gegevens tussen bestanden zijn gebaseerd, kopieert de **parallelCopies** bepalen de parallelle uitvoering op bestandsniveau. De logische groepen te verdelen binnen een enkel bestand onder er zou gebeuren automatisch en transparant en is ontworpen om de beste geschikt chunkgrootte voor een bepaalde bron-gegevenstype store gebruiken om gegevens in parallelle en rechthoekige naar parallelCopies te laden. Het werkelijke aantal parallelle exemplaren data movement service wordt gebruikt voor de kopieerbewerking tijdens runtime is niet meer dan het aantal bestanden dat u hebt. Als flattenhierarchy is **mergeFile**, Kopieeractiviteit niet profiteren van parallelle uitvoering op bestandsniveau.
* Wanneer u een waarde opgeven voor de **parallelCopies** eigenschap, houd rekening met de toename van de belasting op uw bron en sink-gegevensopslag en zelfgehoste Cloudintegratieruntime als de copy-activiteit bevoegd is door er bijvoorbeeld voor hybride exemplaar. Dit gebeurt met name wanneer u hebt meerdere activiteiten of gelijktijdige uitvoeringen van de dezelfde activiteiten worden uitgevoerd voor hetzelfde gegevensarchief. Als u merkt dat het gegevensarchief of een zelfgehoste Integration Runtime met de belasting wordt overbelast, vermindert u de **parallelCopies** waarde om te ontlasten van de belasting.
* Wanneer u gegevens van winkels die niet op bestanden zijn gebaseerd op winkels die op basis van bestanden kopieert, data movement service negeert de **parallelCopies** eigenschap. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.
* **parallelCopies** staat haaks op **dataIntegrationUnits**. De eerste wordt in de eenheden voor de integratie van gegevens geteld.

## <a name="staged-copy"></a>Gefaseerd kopiëren

Wanneer u gegevens van een brongegevensarchief naar een sink-gegevensarchief kopieert, kunt u Blob-opslag gebruiken als een tussentijdse faseringsopslag. Fasering is vooral nuttig in de volgende gevallen:

- **U wilt opnemen van gegevens uit verschillende gegevensarchieven in SQL Data Warehouse met PolyBase**. SQL Data Warehouse gebruikmaakt van een grote hoeveelheid gegevens laden in SQL Data Warehouse PolyBase als een mechanisme voor hoge doorvoer. Echter, de brongegevens moet zich in Blob storage of Azure Data Lake Store, en moet deze voldoen aan aanvullende criteria. Bij het laden van gegevens vanuit een ander gegevensarchief dan Blob storage of Azure Data Lake Store, kunt u gegevens kopiëren via tussentijdse staging Blob-opslag kunt activeren. In dat geval voert de Data Factory de benodigde gegevenstransformaties om ervoor te zorgen dat het voldoet aan de vereisten van PolyBase. Vervolgens maakt PolyBase om gegevens te laden in SQL Data Warehouse efficiënt gebruik. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Soms duurt het even uit te voeren van een hybride verplaatsing van gegevens (dat wil zeggen, om te kopiëren uit een on-premises gegevens kunnen worden opgeslagen naar een gegevensarchief cloud) via een trage netwerkverbinding**. Voor betere prestaties kunt u gefaseerd kopiëren voor het comprimeren van de gegevens on-premises zodat kost het minder tijd om te verplaatsen van gegevens naar de staging gegevensarchief in de cloud en decomprimeren van de gegevens in de faseringsopslag voordat deze worden geladen in het beoogde gegevensarchief.
- **U niet wilt openen van poorten dan poort 80 en poort 443 in uw firewall vanwege bedrijfsbeleid IT**. Wanneer u gegevens van een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse-sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en firewall van uw bedrijf te activeren. In dit scenario wordt gefaseerd kopiëren kunt profiteren van de zelfgehoste Cloudintegratieruntime eerst om gegevens te kopiëren naar een Blob-opslag staging-exemplaar via HTTP of HTTPS op poort 443, waarna de gegevens in SQL-Database of SQL Data Warehouse laden van Blob-opslag staging. In deze stroom moet u geen poort 1433 in te schakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerd kopiëren werkt

Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens worden gekopieerd uit de brongegevensopslag naar de Blob-opslag staging (Voeg uw eigen). Vervolgens is de gegevens van de faseringsgegevensopslag gekopieerd naar de sinkgegevensopslag. Data Factory wordt automatisch de gegevensstroom twee fasen voor u beheerd. Tijdelijke gegevens uit de opslag staging ruimt Data Factory ook nadat de verplaatsing van gegevens voltooid is.

![Gefaseerd kopiëren](media/copy-activity-performance/staged-copy.png)

Wanneer u verplaatsing van gegevens met behulp van een faseringsopslag activeert, kunt u opgeven of u wilt dat de gegevens worden gecomprimeerd voordat het verplaatsen van gegevens uit de brongegevensopslag naar een tijdelijke of tijdelijke gegevensopslag en vervolgens voor het verplaatsen van gegevens vanuit een tussentijdse of faseert gegevens gedecomprimeerd opslaan naar de sinkgegevensopslag.

U kunt gegevens tussen twee on-premises gegevensarchieven met behulp van een faseringsopslag op dit moment niet kopiëren.

### <a name="configuration"></a>Configuratie

Configureer de **enableStaging** instellen in de Kopieeractiviteit om op te geven of u wilt dat de gegevens naar de tijdelijk opgeslagen in Blob-opslag voordat u deze te in een doelgegevensarchief laden. Als u instelt **enableStaging** naar `TRUE`, geeft u de aanvullende eigenschappen in de volgende tabel weergegeven. Als u niet hebt, moet u ook om te maken van een Azure Storage of opslag shared access signature-gekoppelde service voor fasering.

| Eigenschap | Description | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| **enableStaging** |Geef op of u wilt het kopiëren van gegevens via een tussentijdse opslag staging. |False |Nee |
| **linkedServiceName** |Geef de naam van een [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) gekoppelde service, die verwijst naar het exemplaar van de opslag die u als een tussentijdse faseringsopslag. <br/><br/> U kunt opslag met een shared access signature niet gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| **Pad** |Geef het pad op Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u een pad opgeeft, wordt een container voor het opslaan van tijdelijke gegevens gemaakt in de service. <br/><br/> Geef een pad alleen als u Storage met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| **enableCompression** |Hiermee geeft u op of gegevens moeten worden gecomprimeerd voordat deze wordt gekopieerd naar de bestemming. Deze instelling beperkt de hoeveelheid gegevens die worden overgebracht. |False |Nee |

>[!NOTE]
> Als u gefaseerd kopiëren met compressie is ingeschakeld, service-principal of MSI-verificatie gebruikt voor het Faseren van blob gekoppelde wordt service niet ondersteund.

Hier volgt een voorbeelddefinitie van Kopieeractiviteit met de eigenschappen die worden beschreven in de voorgaande tabel:

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

### <a name="staged-copy-billing-impact"></a>Gefaseerd kopiëren facturering impact

De kosten worden berekend op basis van twee stappen: duur van de kopieerbewerking en type kopiëren.

* Wanneer u staging tijdens een kopieerbewerking cloud (gegevens kopiëren van een cloud-gegevensarchief naar een andere cloudgegevensopslag beide fasen die zijn aangewezen door Azure Integration Runtime) in rekening gebracht de [som van de duur van de kopieerbewerking voor de stap 1 en 2] x [cloud kopie unit price].
* Wanneer u staging tijdens een kopieerbewerking hybride (gegevens kopiëren van een on-premises gegevensarchief naar een cloud-gegevensarchief, één fase gemachtigd door zelfgehoste Integration Runtime) in rekening worden gebracht [duur van de kopieerbewerking hybride] x [hybride exemplaar unit price] + [cloud duur van de kopieerbewerking] x [cloud kopie eenheidsprijs].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen

Het is raadzaam dat u deze stappen voor het afstemmen van de prestaties van uw Data Factory-service met de Kopieeractiviteit uitvoeren:

1. **Basislijn van**. Test uw pijplijn met behulp van de Kopieeractiviteit op basis van een representatieve steekproef tijdens de ontwikkelingsfase. Uitvoeringsdetails en prestatiekenmerken volgende verzamelen [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

2. **Diagnose en prestaties te optimaliseren**. Als de prestaties die u ziet dat niet voldoet aan uw verwachtingen, moet u prestatieknelpunten. Vervolgens prestaties als u wilt verwijderen of het effect van knelpunten te optimaliseren. 

    In sommige gevallen, als u een kopieeractiviteit in ADF uitvoert, rechtstreeks ziet u '**tips afstemmen van prestaties**"boven de [kopieeractiviteit bewaking pagina](copy-activity-overview.md#monitor-visually) zoals wordt weergegeven in het volgende voorbeeld. Het niet alleen leest u het knelpunt geïdentificeerd voor de uitvoering van het opgegeven exemplaar, maar ook helpt u op wat u wilt wijzigen zodat deze kopie doorvoer te verhogen. De prestaties van tips voor het afstemmen momenteel suggesties wilt bieden het kopiëren van gegevens in Azure SQL Data Warehouse PolyBase gebruiken om te vergroten Azure Cosmos DB RU of Azure SQL DB DTU wanneer de resource op gegevens opslaan aan clientzijde is het knelpunt, de overbodige gefaseerd verwijderen kopiëren, enzovoort. De afstemming van regels wordt geleidelijk ook uitgebreid zijn.

    **Voorbeeld: kopiëren naar Azure SQL-database met tips afstemmen van prestaties**

    In dit voorbeeld tijdens het kopiëren van uitvoert, ADF kennisgeving die de sink-Azure SQL DB bereikt hoog DTU-gebruik die de bewerkingen voor schrijven vertraagt, dus het voorstel is om te verhogen van de Azure SQL DB-laag met meer DTU. 

    ![Kopiëren met tips afstemmen van de prestaties controleren](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Hier volgen bovendien enkele algemene overwegingen. Er is een volledige beschrijving van de prestatiediagnose van buiten het bereik van dit artikel.

   * Prestatiefuncties:
     * [Parallelle kopiëren](#parallel-copy)
     * [Eenheden van de integratie van gegevens](#data-integration-units)
     * [Gefaseerd kopiëren](#staged-copy)
     * [Zelf-hostende Integration Runtime-schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Zelf-hostende Integratieruntime](#considerations-for-self-hosted-integration-runtime)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)

3. **De configuratie van de uitbreiden naar uw volledige gegevensset**. Wanneer u tevreden met de resultaten van het uitvoeren en de prestaties bent, kunt u de definitie en de pijplijn voor uw volledige gegevensset kunt uitbreiden.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Overwegingen voor het zelf-hostende Integratieruntime

Als de kopieeractiviteit wordt uitgevoerd op een zelfgehoste Cloudintegratieruntime, Let op het volgende:

**Setup**: U wordt aangeraden dat u een toegewezen machine Host Integration Runtime. Zie [overwegingen voor het gebruik van zelfgehoste Cloudintegratieruntime](concepts-integration-runtime.md).

**Uitschalen**: Met een enkele logische zelfgehoste Integration Runtime met een of meer knooppunten kan meerdere Kopieeractiviteit wordt uitgevoerd op hetzelfde moment gelijktijdig fungeren. Als u zware nodig voor hybride verplaatsing van gegevens met een groot aantal uitvoeringen van activiteit gelijktijdige kopiëren of met een grote hoeveelheid gegevens hebt te kopiëren, kunt u overwegen om te [scale-out zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md#high-availability-and-scalability) om meer resource die u wilt inrichten te kopiëren.

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet wordt overbelast door andere werkbelastingen die worden uitgevoerd op of tegen.

Zie voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensarchieven en geven u inzicht in gegevens van de prestatiekenmerken opslaan, responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens kopieert **van Blob storage naar SQL Data Warehouse**, kunt u overwegen **PolyBase** om prestaties te verbeteren. Zie [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie.
* Als u gegevens kopieert **van HDFS naar Azure Blob/Azure Data Lake Store**, kunt u overwegen **DistCp** om prestaties te verbeteren. Zie [DistCp gebruiken om te kopiëren van gegevens uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) voor meer informatie.
* Als u gegevens kopieert **van Redshift naar Azure SQL Data Warehouse/Azure BLob/Azure Data Lake Store**, kunt u overwegen **UNLOAD** om prestaties te verbeteren. Zie [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Gemiddelde van de bestandsgrootte en het aantal bestanden**: Kopieeractiviteit brengt gegevens één bestand tegelijk. Met de dezelfde hoeveelheid gegevens worden verplaatst, wordt de algehele doorvoer lager als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Indien mogelijk, dus kleine bestanden combineren in grotere bestanden om te krijgen hogere doorvoer.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren, de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Een gegevenspatroon**: Uw tabelschema is van invloed op de doorvoer van de kopie. Een grote rijgrootte biedt u een betere prestaties dan kleine rijgrootte, om te kopiëren van de dezelfde hoeveelheid gegevens. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of een opgeslagen procedure**: Optimaliseer de logica van de query of een opgeslagen procedure die u in de bron van Kopieeractiviteit opgeeft voor het ophalen van gegevens efficiënter.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet wordt overbelast door andere werkbelastingen die worden uitgevoerd op of tegen.

Raadpleeg voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensopslag. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens kopieert **van Blob storage naar SQL Data Warehouse**, kunt u overwegen **PolyBase** om prestaties te verbeteren. Zie [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie.
* Als u gegevens kopieert **van HDFS naar Azure Blob/Azure Data Lake Store**, kunt u overwegen **DistCp** om prestaties te verbeteren. Zie [DistCp gebruiken om te kopiëren van gegevens uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) voor meer informatie.
* Als u gegevens kopieert **van Redshift naar Azure SQL Data Warehouse/Azure BLob/Azure Data Lake Store**, kunt u overwegen **UNLOAD** om prestaties te verbeteren. Zie [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Kopieer gedrag**: Als u gegevens van een ander bestand op basis van gegevensarchief kopieert, Copy-activiteit heeft drie opties via de **copyBehavior** eigenschap. Deze hiërarchie behoudt, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Hiërarchie relatiegegevens of behouden heeft weinig of geen performance overhead, maar samenvoegen van bestanden zorgt ervoor dat de prestatieoverhead te verhogen.
* **Bestand-indeling en compressie**: Zie de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om prestaties te verbeteren.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Kopieer gedrag**: Afhankelijk van de eigenschappen die u hebt ingesteld voor **sqlSink**, Kopieeractiviteit schrijft gegevens naar de doeldatabase op verschillende manieren.
  * Standaard voegt de data movement service wordt gebruikt de bulksgewijs kopiëren-API om gegevens in te voegen modus, waarbij de beste prestaties biedt.
  * Als u een opgeslagen procedure in de sink is geconfigureerd, is de database de één rij met gegevens van toepassing op een tijdstip in plaats van als bulksgewijs laden. Prestaties aanzienlijk wordt geweigerd. Als uw gegevensset is groot, indien van toepassing is, overweeg over te schakelen voor het gebruik van de **preCopyScript** eigenschap.
  * Als u configureert de **preCopyScript** eigenschap voor elke activiteit kopiëren uitvoeren, het script door de service wordt geactiveerd en u de bulksgewijs kopiëren-API gebruiken voor het invoegen van de gegevens. Bijvoorbeeld, als u wilt overschrijven de hele tabel met de meest recente gegevens, kunt u een script voor het eerst alle records verwijderen voordat u de nieuwe gegevens uit de bron voor bulksgewijs laden.
* **Patroon en batch gegevensgrootte**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren van de dezelfde hoeveelheid gegevens, biedt een grote rijgrootte betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van de gegevens worden doorgevoerd.
  * Kopieeractiviteit voegt de gegevens in een reeks van batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens klein rijen is, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere batch overhead en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een mislukte kopieerbewerking veroorzaakt door overbelasting van de database.

### <a name="nosql-stores"></a>NoSQL-archieven

* Voor **Table storage**:
  * **Partitie**: Gegevens schrijven naar een interleaved partities aanzienlijk vermindert de prestaties. De brongegevens op partitiesleutel sorteren, zodat de gegevens efficiënt in één partitie is ingevoegd achter elkaar, of aanpassen van de logica voor het schrijven van de gegevens naar één partitie.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie

Serialisatie en deserialisatie kunnen optreden wanneer uw gegevensset voor invoer of uitvoer gegevensset een bestand is. Zie [ondersteunde indelingen voor bestanden en compressie](supported-file-formats-and-compression-codecs.md) met informatie over ondersteunde bestandsindelingen door de Kopieeractiviteit.

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven in op basis van bestanden:
  * Als invoer- en data-sets beide hebben hetzelfde of er zijn geen bestandsindelingsinstellingen, data movement service wordt uitgevoerd een **binaire kopie** zonder serialisatie of deserialisatie. Ziet u een hogere doorvoer in vergelijking met het scenario, waarin de bron en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer en uitvoer gegevenssets beide zijn in de tekstindeling en alleen de codering type is anders, data movement service biedt alleen codering conversie. Doet het nog serialisatie en deserialisatie, waardoor overhead in vergelijking met een binaire kopie prestaties.
  * Brongegevens om te streamen, transformeren en deze vervolgens te serialiseren in de indeling van de uitvoer die u hebt aangegeven als invoer- en data-sets beide hebben verschillende bestandsindelingen of verschillende configuraties, zoals scheidingstekens, data movement service worden gedeserialiseerd. Deze bewerking resulteert in een veel belangrijker performance overhead in vergelijking met andere scenario's.
* Wanneer u kopieert bestanden naar/van een gegevensarchief dat niet op basis van bestanden (bijvoorbeeld in een store, op basis van bestanden naar een relationele archief), wordt de stap serialisatie of deserialisatie is vereist. Deze stap resulteert in van aanzienlijke prestatieoverhead.

**Bestandsindeling**: De bestandsindeling die u kiest kan kopiëren prestaties beïnvloeden. Avro is bijvoorbeeld een compacte binaire indeling die metagegevens op met de gegevens slaat. Het biedt een brede ondersteuning in het Hadoop-ecosysteem voor het verwerken en uitvoeren van query's. Avro is echter duurder voor serialisatie en deserialisatie, wat in het onderste exemplaar doorvoer in vergelijking met de tekstindeling resulteert. Uw keuze van de bestandsindeling in de stroom voor de verwerking van zuinigste maken. Beginnen met welke vorm de gegevens worden opgeslagen in brongegevens of moet worden geëxtraheerd uit externe systemen; de beste indeling voor opslag, analytische verwerking en uitvoeren van query's; en in welke indeling moet de gegevens worden geëxporteerd naar de datamarts voor hulpprogramma's voor rapportage en visualisatie. Soms een bestandsindeling die is suboptimale voor lezen en schrijven prestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie

Als uw gegevensset voor invoer- of een bestand is, kunt u Copy Activity om uit te voeren van compressie of decompressie als gegevens naar de bestemming schrijft instellen. Wanneer u compressie kiest, maakt u een balans tussen input/output (I/O) en de CPU. Comprimeren van de kosten van de gegevens een extra in compute-resources. Maar hierna het i/o-netwerk en opslag verlaagt. Afhankelijk van uw gegevens ziet u een boost in de algemene doorvoer van de kopie.

**Codec**: Elke compressiecodec heeft voordelen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u krijgt de beste prestaties van Hive-query met bzip2 omdat u deze voor verwerking opsplitsen kunt. Gzip is de meest met gelijke taakverdeling optie en deze het vaakst wordt gebruikt. Kies de codec die het beste bij uw end-to-end-scenario.

**Niveau**: U kunt kiezen uit twee opties voor elke compressiecodec: snelste gecomprimeerde en optimaal gecomprimeerd. De snelste gecomprimeerde optie worden de gegevens zo snel mogelijk, gecomprimeerd, zelfs als het resulterende bestand is niet optimaal gecomprimeerd. De optimaal gecomprimeerde optie meer tijd in de taakwachtrij doorbrengt op compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Overweging**: Als u wilt kopiëren van een grote hoeveelheid gegevens tussen een on-premises gegevensopslag en de cloud, kunt u overwegen [gefaseerd kopiëren](#staged-copy) met gebruik van compressie. Tijdelijke opslag is nuttig wanneer de bandbreedte van uw bedrijfsnetwerk en uw Azure-services de beperkende factor is en u wilt dat de gegevensset voor invoer en uitvoer gegevensset beide in niet-gecomprimeerde vorm.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing

U kunt instellen dat de **columnMappings** eigenschap in de Kopieeractiviteit alle kaart of een subset van de invoerkolommen voor de uitvoerkolommen. Nadat de data movement service leest de gegevens van de bron, moet deze kolomtoewijzing uitvoeren voor de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking vermindert kopie doorvoer.

Als uw bron-gegevensopslag waarin u kunt zoeken, bijvoorbeeld als het is een relationele opslagplaats, zoals SQL-Database of SQL Server, of als het is een NoSQL-archief, zoals Table storage of Azure Cosmos DB, kunt u de kolom filteren pushen en logica voor opnieuw ordenen de **query** eigenschap in plaats van in de kolomtoewijzing. Op deze manier de projectie treedt op wanneer data movement service gegevens uit de brongegevensopslag leest, waar het is veel efficiënter.

Meer informatie uit [activiteit kopiëren-schematoewijzing](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Andere overwegingen

Als de grootte van gegevens die u wilt kopiëren groot is, kunt u uw zakelijke logica voor de partitie van de gegevens verder en plannen van de Kopieeractiviteit om uit te voeren steeds vaker voor het verkleinen van de gegevens voor elke activiteit kopiëren die wordt uitgevoerd.

Wees voorzichtig bij het aantal gegevenssets en een Data Factory verbinding maken met hetzelfde gegevensarchief op hetzelfde moment vereisen kopieeractiviteiten. Veel gelijktijdige kopie taken mogelijk beperken van een gegevensarchief en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen, fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeldscenario: Kopiëren van een on-premises SQL Server naar Blob-opslag

**Scenario**: Een pijplijn is gebouwd om gegevens te kopiëren uit een on-premises SQL Server naar Blob-opslag in CSV-indeling. Als u sneller de taak voor het kopiëren, moeten de CSV-bestanden worden gecomprimeerd naar bzip2-indeling.

**Test- en analyse**: De doorvoer van de Kopieeractiviteit is minder dan 2 MBps, dat is veel lager is dan de benchmark prestaties.

**Analyse van prestaties en afstemmen**: Voor het oplossen van het prestatieprobleem, laten we kijken hoe de gegevens worden verwerkt en verplaatst.

1. **Gegevens lezen**: Integratieruntime een verbinding met SQL Server wordt geopend en stuurt de query. SQL-Server reageert door te sturen van de gegevensstroom voor integratieruntime via het intranet.
2. **Serialiseren en gegevens comprimeren**: Integratieruntime serialiseert de gegevensstroom naar de CSV-indeling en de gegevens naar een stream bzip2 worden gecomprimeerd.
3. **Gegevens schrijven**: Integratieruntime wordt de stroom bzip2 geüpload naar Blob storage via Internet.

Zoals u zien kunt, de gegevens worden verwerkt en verplaatst op sequentiële wijze streaming: SQL Server > LAN > integratieruntime > WAN > Blob storage. **De algehele prestaties worden geregeld door de minimale doorvoer via de pijplijn**.

![Gegevensstroom](./media/copy-activity-performance/case-study-pic-1.png)

Een of meer van de volgende factoren kan leiden tot de bottleneck in de prestaties:

* **Bron**: SQL-Server zelf heeft lage doorvoer vanwege de zware belastingen.
* **Zelf-hostende Integratieruntime**:
  * **LAN**: Integratieruntime bevindt zich ver van de SQL Server-machine en een verbinding met een lage bandbreedte heeft.
  * **Integratieruntime**: Integratieruntime heeft bereikt van de load-beperkingen voor de volgende bewerkingen uitvoeren:
    * **Serialisatie**: Tijdens het serialiseren van de gegevensstroom naar de CSV-indeling is traag doorvoer.
    * **Compressie**: U hebt ervoor gekozen een trage compressiecodec (bijvoorbeeld, bzip2, namelijk 2,8 MBps met Core i7).
  * **WAN**: Is een lage bandbreedte tussen het bedrijfsnetwerk en uw Azure-services (bijvoorbeeld T1 = 1,544 kbps. Tijdstip T2 = 6,312 kbps).
* **Sink**: BLOB-opslag heeft lage doorvoer. (In dit scenario is niet waarschijnlijk omdat de SLA wordt gegarandeerd minimaal 60 MBps.)

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="reference"></a>Referentie

Hier zijn prestaties controleren en afstemmen van verwijzingen voor enkele van de ondersteunde gegevensarchieven:

* Azure Storage (inclusief Blob storage en Table storage): [Azure Storage-schaalbaarheidsdoelen](../storage/common/storage-scalability-targets.md) en [controlelijst voor de prestaties en schaalbaarheid van Azure Storage](../storage/common/storage-performance-checklist.md)
* Azure SQL Database: U kunt [bewaken van de prestaties](../sql-database/sql-database-single-database-monitor.md) en controleert u het percentage van database transaction unit (DTU)
* Azure SQL Data Warehouse: De mogelijkheid wordt gemeten in datawarehouse units (dwu's); Zie [beheren rekenkracht in Azure SQL Data Warehouse (overzicht)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Prestatieniveaus in Azure Cosmos DB](../cosmos-db/performance-levels.md)
* On-premises SQL Server: [Controleren en afstemmen van prestaties](https://msdn.microsoft.com/library/ms189081.aspx)
* On-premises bestandsserver: [Prestaties afstemmen voor bestandsservers](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen van de Kopieeractiviteit:

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Schematoewijzing activiteit kopiëren](copy-activity-schema-and-type-mapping.md)
- [Fouttolerantie van activiteit kopiëren](copy-activity-fault-tolerance.md)
