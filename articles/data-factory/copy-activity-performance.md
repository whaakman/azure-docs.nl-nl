---
title: De gids voor de prestaties en het afstemmen van de activiteit kopiëren in Azure Data Factory | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die van invloed zijn op de prestaties van het verplaatsen van gegevens in Azure Data Factory wanneer u de Kopieer activiteit gebruikt.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: d8ce0a4f6bacdd1c8c858d474e6f3957a23c6357
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967366"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Gids voor de prestaties en het afstemmen van Kopieer activiteiten
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-copy-activity-performance.md)
> * [Huidige versie](copy-activity-performance.md)


De Azure Data Factory Copy-activiteit levert een eersteklas, veilige, betrouw bare en hoogwaardige oplossing voor het laden van gegevens. U kunt dit gebruiken om tien tallen terabytes aan gegevens elke dag te kopiëren over een groot aantal verschillende Cloud-en on-premises gegevens opslag. Snelle prestaties bij het laden van gegevens zijn belang rijk om ervoor te zorgen dat u zich kunt richten op het belangrijkste big data probleem: het bouwen van geavanceerde analyse oplossingen en het verkrijgen van grondige inzichten van al deze gegevens.

Azure biedt een reeks hoogwaardige oplossingen voor gegevens opslag en Data Warehouse. De Kopieer activiteit biedt een zeer geoptimaliseerde ervaring voor het laden van gegevens die eenvoudig kan worden geconfigureerd en ingesteld. Met één Kopieer activiteit kunt u gegevens laden in:

* Azure SQL Data Warehouse op 1,2 GBps.
* Azure Blob-opslag op 1,0 GBps.
* Azure Data Lake Store op 1,0 GBps.

Dit artikel wordt beschreven:

* [Prestatie referentie nummers](#performance-reference) voor ondersteunde bron-en Sink-gegevens opslag om u te helpen bij het plannen van uw project.
* Functies waarmee de Kopieer doorvoer kan worden verhoogd in verschillende scenario's, waaronder [Data Integration units](#data-integration-units) (DIUs), [parallelle kopieën](#parallel-copy)en [gefaseerde kopieën](#staged-copy).
* [Richt lijnen](#performance-tuning-steps) voor het afstemmen van de prestaties van de prestaties en de belangrijkste factoren die van invloed kunnen zijn op de Kopieer prestaties.

> [!NOTE]
> Als u niet bekend bent met de Kopieer activiteit in het algemeen, raadpleegt u het overzicht van de [Kopieer activiteit](copy-activity-overview.md) voordat u dit artikel leest.
>

## <a name="performance-reference"></a>Naslaginformatie over prestaties

Als referentie bevat de volgende tabel het Kopieer doorvoer nummer in MBps voor de gegeven bron-en Sink-paren in één Kopieer activiteit die wordt uitgevoerd op basis van interne tests. Ter vergelijking ziet u ook hoe verschillende instellingen van [gegevens integratie-eenheden](#data-integration-units) of een [zelf-hostende Integration runtime-schaal baarheid](concepts-integration-runtime.md#self-hosted-integration-runtime) (meerdere knoop punten) kunnen helpen bij het kopiëren van prestaties.

![Matrix van prestaties](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Wanneer de Kopieer activiteit wordt uitgevoerd op een Azure Integration runtime, zijn de mini maal toegestane gegevens integratie-eenheden (voorheen eenheden voor gegevens verplaatsing) twee. Als dat niet is opgegeven, raadpleegt u de standaard eenheden voor gegevens integratie die worden gebruikt in [gegevens integratie-eenheden](#data-integration-units).

**Punten om te noteren:**

* De door Voer wordt berekend met behulp van de volgende formule: [grootte van gegevens die uit de bron zijn gelezen]/[duur van de Kopieer activiteit].
* De prestatie referentie nummers in de tabel zijn gemeten met behulp van een [TPC-H-](http://www.tpc.org/tpch/) gegevensset in één exemplaar van de Kopieer activiteit. Test bestanden voor archieven op basis van bestanden zijn meerdere bestanden met een grootte van 10 GB.
* In de Azure-gegevensopslag zijn de bron en sink in dezelfde Azure-regio.
* Voor hybride kopieën tussen on-premises en gegevens opslag in de Cloud, werd elk zelf-hostende Integration runtime-knoop punt uitgevoerd op een computer die is gescheiden van het gegevens archief, met de volgende specificatie. Als een enkele activiteit werd uitgevoerd, gebruikt de kopieerbewerking alleen een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte.
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
        <td>Internet-interface: 10 Gbps; intranet Interface: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> U kunt de door Voer verhogen door meer DIUs te gebruiken. U kunt bijvoorbeeld met 100 DIUs gegevens uit Azure Blob-opslag kopiëren naar Azure Data Lake Store op 1,0 GBps. Zie de sectie [eenheden voor gegevens integratie](#data-integration-units) voor meer informatie over deze functie en het Ondersteunde scenario. 

## <a name="data-integration-units"></a>Gegevens integratie-eenheden

Een gegevens integratie-eenheid is een meting die de kracht vertegenwoordigt (een combi natie van CPU, geheugen en netwerk bron toewijzing) van één eenheid in Azure Data Factory. De gegevens integratie-eenheid is alleen van toepassing op [Azure Integration runtime](concepts-integration-runtime.md#azure-integration-runtime), maar niet [zelf-hostende Integration runtime](concepts-integration-runtime.md#self-hosted-integration-runtime).

De minimale DIUs om de uitvoering van een Kopieer activiteit te stimuleren, is twee. Indien niet opgegeven, bevat de volgende tabel de standaard-DIUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario kopiëren | Standaard DIUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 4 en 32, afhankelijk van het aantal en de grootte van de bestanden |
| Gegevens kopiëren naar Azure SQL Database of Azure Cosmos DB |Tussen 4 en 16, afhankelijk van de Azure SQL Database van de sink of de Cosmos DB laag (aantal Dtu's/RUs) |
| Alle andere scenario's voor kopiëren | 4 |

Als u wilt deze standaardwaarde onderdrukken, Geef een waarde op voor de **dataIntegrationUnits** eigenschap als volgt te werk. De *toegestane waarden* voor de eigenschap **dataIntegrationUnits** zijn Maxi maal 256. De *werkelijke aantal DIUs* dat gebruikmaakt van de kopieerbewerking tijdens de uitvoering is gelijk aan of kleiner is dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk bij het configureren van meer eenheden voor een specifiek exemplaar gegevensbronnen en sinks, de [prestaties verwijzing](#performance-reference).

U kunt de DIUs die wordt gebruikt voor elke Kopieer bewerking zien in de uitvoer van de Kopieer activiteit wanneer u een uitvoering van een activiteit bewaakt. Zie voor meer informatie [controle activiteit kopiëren](copy-activity-overview.md#monitoring).

> [!NOTE]
> De instelling van DIUs die groter is dan vier, is momenteel alleen van toepassing als u meerdere bestanden kopieert van Azure Storage, Azure Data Lake Storage, Amazon S3, Google Cloud Storage, Cloud FTP of Cloud SFTP naar andere gegevens archieven in de Cloud.
>

**Voorbeeld**

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

Houd er rekening mee dat u wordt gefactureerd op basis van de totale tijd van de Kopieer bewerking. De totale duur die wordt gefactureerd voor het verplaatsen van gegevens is de som van de duur in DIUs. Als een kopieertaak voorheen een uur duren voordat met twee cloudeenheden en nu wordt 15 minuten met acht cloudeenheden, blijft de totale factuur bijna hetzelfde.

## <a name="parallel-copy"></a>Parallelle kopie

U kunt de eigenschap **parallelCopies** gebruiken om aan te geven in welke parallellisme u de Kopieer activiteit wilt gebruiken. U kunt deze eigenschap beschouwen als het maximum aantal threads in de Kopieer activiteit dat kan worden gelezen vanuit uw bron of naar uw Sink-gegevens opslag kunt schrijven, parallel.

Voor elke uitvoering van de Kopieer activiteit bepaalt Azure Data Factory het aantal parallelle kopieën dat moet worden gebruikt voor het kopiëren van gegevens uit de brongegevens opslag en naar het doel gegevens archief. Het standaard aantal parallelle kopieën dat wordt gebruikt, is afhankelijk van het type bron en Sink dat u gebruikt.

| Scenario kopiëren | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van bestanden |Is afhankelijk van de grootte van de bestanden en het aantal DIUs dat wordt gebruikt voor het kopiëren van gegevens tussen twee gegevens archieven in de Cloud of de fysieke configuratie van de zelf-hostende Integration runtime-computer. |
| Gegevens uit een bron archief kopiëren naar Azure-tabel opslag |4 |
| Alle andere kopieerscenario 's |1 |

> [!TIP]
> Wanneer u gegevens kopieert tussen archieven op basis van bestanden, biedt het standaard gedrag doorgaans de beste door voer. Het standaard gedrag wordt automatisch bepaald op basis van het patroon van het bron bestand.

Als u de belasting wilt beheren op machines die uw gegevens archieven hosten, of als u de Kopieer prestaties wilt afstemmen, kunt u de standaard waarde overschrijven en een waarde opgeven voor de eigenschap **parallelCopies** . De waarde moet een geheel getal groter dan of gelijk aan 1 zijn. Tijdens runtime gebruikt de Kopieer activiteit een waarde die kleiner is dan of gelijk is aan de waarde die u hebt ingesteld.

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

**Punten om te noteren:**

* Wanneer u gegevens kopieert tussen archieven op basis van bestanden, bepaalt **parallelCopies** de parallelle factor op bestands niveau. De Chunking binnen één bestand vindt onder automatisch en transparant plaats. Het is ontworpen om de beste geschikte segment grootte te gebruiken voor een bepaald type brongegevens opslag om gegevens parallel en rechthoekig naar **parallelCopies**te laden. Het werkelijke aantal parallelle exemplaren data movement service wordt gebruikt voor de kopieerbewerking tijdens runtime is niet meer dan het aantal bestanden dat u hebt. Als het Kopieer gedrag **mergeFile**is, kan de Kopieer activiteit niet profiteren van parallellisme op bestands niveau.
* Wanneer u gegevens kopieert uit winkels die niet op bestanden zijn gebaseerd (met uitzonde ring van [Oracle](connector-oracle.md#oracle-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP-tabel](connector-sap-table.md#sap-table-as-source)en [SAP open hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) -connector als bron waarvoor partitionering van gegevens is ingeschakeld) voor archieven die zijn gebaseerd op bestand, de service voor gegevens verplaatsing Hiermee wordt de eigenschap **parallelCopies** genegeerd. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.
* De eigenschap **parallelCopies** is rechthoekig naar **dataIntegrationUnits**. De eerste wordt in de eenheden voor de integratie van gegevens geteld.
* Wanneer u een waarde voor de eigenschap **parallelCopies** opgeeft, moet u rekening houden met de toename van de belasting van uw bron-en Sink-gegevens opslag. Houd ook rekening met de toename van de belasting voor de zelf-hostende Integration runtime als de Kopieer activiteit hiervoor is gemachtigd, bijvoorbeeld voor hybride kopieën. Deze belasting toename treedt vooral op wanneer u meerdere activiteiten of gelijktijdige uitvoeringen hebt van dezelfde activiteiten die worden uitgevoerd op hetzelfde gegevens archief. Als u merkt dat het gegevens archief of de zelf-hostende Integration runtime wordt overspoeld met de belasting, verlaagt u de **parallelCopies** -waarde om de belasting te ontlasten.

## <a name="staged-copy"></a>Gefaseerd kopiëren

Wanneer u gegevens van een brongegevensarchief naar een sink-gegevensarchief kopieert, kunt u Blob-opslag gebruiken als een tussentijdse faseringsopslag. Fasering is vooral nuttig in de volgende gevallen:

- **U wilt gegevens uit verschillende gegevens archieven opnemen in SQL Data Warehouse via Poly base.** SQL Data Warehouse gebruikmaakt van een grote hoeveelheid gegevens laden in SQL Data Warehouse PolyBase als een mechanisme voor hoge doorvoer. De bron gegevens moeten zich in Blob Storage of Azure Data Lake Store bekomen en moeten aan aanvullende criteria voldoen. Bij het laden van gegevens vanuit een ander gegevensarchief dan Blob storage of Azure Data Lake Store, kunt u gegevens kopiëren via tussentijdse staging Blob-opslag kunt activeren. In dat geval voert Azure Data Factory de vereiste gegevens transformaties uit om ervoor te zorgen dat het voldoet aan de vereisten van poly base. Vervolgens maakt PolyBase om gegevens te laden in SQL Data Warehouse efficiënt gebruik. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Soms duurt het even om een hybride gegevens verplaatsing (dat wil zeggen, kopiëren van een on-premises gegevens archief naar een gegevens archief in de Cloud) uit te voeren via een trage netwerk verbinding.** Om de prestaties te verbeteren, kunt u gefaseerde kopie gebruiken om de gegevens on-premises te comprimeren, zodat het minder tijd kost om gegevens te verplaatsen naar de faserings gegevens opslag in de Cloud. Vervolgens kunt u de gegevens in het faserings archief decomprimeren voordat u ze laadt in de doel gegevens opslag.
- **U wilt geen andere poorten dan poort 80 en poort 443 openen in uw firewall vanwege het IT-beleid van het bedrijf.** Wanneer u gegevens van een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse-sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en firewall van uw bedrijf te activeren. In dit scenario kan een gefaseerde kopie gebruikmaken van de zelf-hostende Integration runtime om eerst gegevens te kopiëren naar een staging-opslag instantie van een BLOB via HTTP of HTTPS op poort 443. Vervolgens kunnen de gegevens worden geladen in SQL Database of SQL Data Warehouse vanuit de fase van het maken van Blob-opslag. In deze stroom moet u geen poort 1433 in te schakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerd kopiëren werkt

Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens worden gekopieerd uit de brongegevensopslag naar de Blob-opslag staging (Voeg uw eigen). Vervolgens is de gegevens van de faseringsgegevensopslag gekopieerd naar de sinkgegevensopslag. Azure Data Factory beheert automatisch de twee fase stroom voor u. Azure Data Factory verwijdert ook tijdelijke gegevens uit de staging-opslag nadat de gegevens verplaatsing is voltooid.

![Gefaseerd kopiëren](media/copy-activity-performance/staged-copy.png)

Wanneer u gegevens verplaatsing activeert met behulp van een faserings opslag, kunt u opgeven of u wilt dat de gegevens worden gecomprimeerd voordat u gegevens verplaatst van de brongegevens opslag naar een tussentijds-of faserings gegevens archief en vervolgens wordt gedecomprimeerd voordat u gegevens verplaatst van een tussentijds of fasering dat een archief naar de Sink-gegevens opslag.

Op dit moment kunt u geen gegevens kopiëren tussen twee gegevens archieven die zijn verbonden via een andere zelf-hostende IRs, noch zonder een gefaseerde kopie. Voor dit scenario kunt u twee expliciet gekoppelde Kopieer activiteiten configureren om te kopiëren van bron naar fase ring en vervolgens van fase ring naar sink.

### <a name="configuration"></a>Configuratie

Configureer de instelling **enableStaging** in de Kopieer activiteit om op te geven of u wilt dat de gegevens in Blob Storage worden klaargezet voordat u deze in een doel gegevens archief laadt. Wanneer u **enableStaging** instelt op `TRUE`, geeft u de aanvullende eigenschappen op die in de volgende tabel worden weer gegeven. U moet ook een gekoppelde service voor het maken van een hand tekening voor gedeelde toegang met een opslag locatie voor Azure Storage of Storage gebruiken als u er geen hebt.

| Eigenschap | Description | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| enableStaging |Geef op of u wilt het kopiëren van gegevens via een tussentijdse opslag staging. |False |Nee |
| linkedServiceName |Geef de naam van een [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) gekoppelde service, die verwijst naar het exemplaar van de opslag die u als een tussentijdse faseringsopslag. <br/><br/> U kunt opslag niet gebruiken met een Shared Access Signature om gegevens te laden in SQL Data Warehouse via Poly base. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| path |Geef het pad op Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u geen pad opgeeft, maakt de service een container om tijdelijke gegevens op te slaan. <br/><br/> Geef een pad alleen als u Storage met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| enableCompression |Hiermee geeft u op of gegevens moeten worden gecomprimeerd voordat ze naar het doel worden gekopieerd. Deze instelling beperkt de hoeveelheid gegevens die worden overgebracht. |False |Nee |

>[!NOTE]
> Als u een gefaseerde kopie gebruikt terwijl compressie is ingeschakeld, wordt de service-principal of MSI-verificatie voor de gekoppelde BLOB-hostservice niet ondersteund.

Hier volgt een voor beeld van een Kopieer activiteit met de eigenschappen die in de voor gaande tabel worden beschreven:

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

Er worden kosten in rekening gebracht op basis van twee stappen: de duur en het Kopieer type kopiëren.

* Wanneer u fase ring gebruikt tijdens een Cloud kopie, die gegevens uit een gegevens archief in de cloud kopieert naar een ander gegevens archief in de Cloud, wordt de [som van de Kopieer duur voor stap 1 en stap 2] x [eenheids prijs voor de Cloud kopie] in rekening gebracht.
* Wanneer u fase ring gebruikt tijdens een hybride kopie, waarmee gegevens worden gekopieerd van een on-premises gegevens opslag naar een gegevens archief in de Cloud, wordt één fase van een zelf-hostende Integration runtime in rekening gebracht voor [Hybrid Copy duration] x [hybride kopie van eenheids prijs] + [duur van de Cloud kopiëren] x [prijs per eenheid voor Cloud Copy].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen

Voer de volgende stappen uit om de prestaties van uw Azure Data Factory-service af te stemmen met de Kopieer activiteit.

1. **Een basis lijn vaststellen.** Test uw pijp lijn tijdens de ontwikkelings fase met behulp van de Kopieer activiteit op basis van een representatief gegevens voorbeeld. Verzamel Details van de uitvoerings gegevens en prestatie kenmerken na het controleren van de [activiteit](copy-activity-overview.md#monitoring).

2. **Prestaties diagnosticeren en optimaliseren.** Als de prestaties die u ziet niet voldoen aan uw verwachtingen, identificeert u prestatie knelpunten. Vervolgens prestaties als u wilt verwijderen of het effect van knelpunten te optimaliseren.

    In sommige gevallen ziet u, wanneer u een Kopieer activiteit uitvoert in Azure Data Factory, het bericht ' prestatie afstemmings tips ' boven op de [pagina controle activiteit controleren](copy-activity-overview.md#monitor-visually), zoals wordt weer gegeven in het volgende voor beeld. Het bericht vertelt u het knel punt dat is geïdentificeerd voor de opgegeven kopie-uitvoering. Ook wordt u begeleid bij het verbeteren van de door Voer van de Kopieer capaciteit. De tips voor het afstemmen van de prestaties bieden momenteel suggesties als:

    - Gebruik poly Base als u gegevens naar Azure SQL Data Warehouse kopieert.
    - Verg root Azure Cosmos DB aanvraag eenheden of Azure SQL Database Dtu's (data base-doorvoer eenheden) wanneer de resource aan de kant van het gegevens archief het knel punt is.
    - Verwijder de onnodige kopie die u wilt gefaseerd.

    De regels voor het afstemmen van de prestaties worden ook geleidelijk verrijkt.

    **Voorbeeld: Naar Azure SQL Database kopiëren met tips voor het afstemmen van prestaties**

    In dit voor beeld, tijdens het uitvoeren van een kopie, Azure Data Factory wordt de Sink Azure SQL Database het hoge DTU-gebruik te bereiken, waardoor de schrijf bewerkingen worden vertraagd. Het voor stel is om de Azure SQL Database-laag te verg Roten met meer Dtu's. 

    ![Bewaking met tips voor het afstemmen van prestaties kopiëren](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Daarnaast volgen hier enkele algemene overwegingen. Een volledige beschrijving van de prestatie diagnose valt buiten het bereik van dit artikel.

   * Prestatiefuncties:
     * [Parallelle kopiëren](#parallel-copy)
     * [Eenheden voor gegevensintegratie](#data-integration-units)
     * [Gefaseerd kopiëren](#staged-copy)
     * [Schaal baarheid van zelf-hostende Integration runtime](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Zelf-hostende Integration Runtime](#considerations-for-self-hosted-integration-runtime)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)

3. **Breid de configuratie uit naar uw volledige gegevensset.** Wanneer u tevreden bent met de resultaten en prestaties van de uitvoering, kunt u de definitie en pijp lijn uitvouwen om uw hele gegevensset te bedekken.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Overwegingen voor zelf-hostende Integration runtime

Als uw Kopieer activiteit wordt uitgevoerd op een zelf-hostende Integration runtime, let dan op het volgende:

**Instellen**: U wordt aangeraden een specifieke computer te gebruiken voor het hosten van Integration runtime. Zie [overwegingen voor het gebruik van zelf-hostende Integration runtime](concepts-integration-runtime.md).

**Uitschalen**: Eén logische zelf-hostende Integration runtime met een of meer knoop punten kan tegelijkertijd meerdere gelijktijdig uitgevoerde exemplaren van de Kopieer activiteit uitvoeren. Als er veel behoefte is aan hybride gegevens verplaatsing, met een groot aantal gelijktijdige Kopieer activiteiten of met een grote hoeveelheid gegevens die moet worden gekopieerd, kunt u overwegen om [de zelf-hostende Integration runtime](create-self-hosted-integration-runtime.md#high-availability-and-scalability) te verg Roten om meer bronnen in te richten.

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron

### <a name="general"></a>Algemeen

Zorg ervoor dat de onderliggende gegevens opslag niet wordt overbelast door andere workloads die op of worden uitgevoerd.

Zie onderwerpen die specifiek zijn voor gegevens archieven voor informatie over het [bewaken en afstemmen](#performance-reference) van micro soft-gegevens. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens kopieert van Blob-opslag naar SQL Data Warehouse, kunt u overwegen poly Base te gebruiken om de prestaties te verbeteren. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Als u gegevens kopieert van HDFS naar Azure Blob-opslag of Azure Data Lake Store, kunt u DistCp gebruiken om de prestaties te verbeteren. Zie [DistCp gebruiken om gegevens van HDFS te kopiëren](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs)voor meer informatie.
* Als u gegevens kopieert van RedShift naar Azure SQL Data Warehouse, Azure BLob Storage of Azure Data Lake Store, kunt u het gebruik van Unload ongedaan maken om de prestaties te verbeteren. Zie [Unload gebruiken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift)voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Gemiddelde bestands grootte en aantal bestanden**: De Kopieer activiteit brengt gegevens één bestand per keer over. Met de dezelfde hoeveelheid gegevens worden verplaatst, wordt de algehele doorvoer lager als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Als dat mogelijk is, kunt u kleine bestanden in grotere bestanden combi neren om een hogere door voer te krijgen.
* **Bestands indeling en compressie**: Zie [overwegingen voor serialisatie en](#considerations-for-serialization-and-deserialization) deserialisatie en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om de prestaties te verbeteren.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Gegevens patroon**: Uw tabelschema is van invloed op de doorvoer van de kopie. Een grote Rijgrootte biedt betere prestaties dan een kleine Rijgrootte om dezelfde hoeveelheid gegevens te kopiëren. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of opgeslagen procedure**: Optimaliseer de logica van de query of opgeslagen procedure die u opgeeft in de bron van de Kopieer activiteit om gegevens efficiënter op te halen.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink

### <a name="general"></a>Algemeen

Zorg ervoor dat de onderliggende gegevens opslag niet wordt overbelast door andere workloads die op of worden uitgevoerd.

Zie onderwerpen die specifiek zijn voor gegevens archieven voor informatie over het [bewaken en afstemmen](#performance-reference) van micro soft-gegevens. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens uit een gegevens archief naar Azure SQL Data Warehouse kopieert, kunt u gebruikmaken van poly Base om de prestaties te verbeteren. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Als u gegevens kopieert van HDFS naar Azure Blob-opslag of Azure Data Lake Store, kunt u DistCp gebruiken om de prestaties te verbeteren. Zie [DistCp gebruiken om gegevens van HDFS te kopiëren](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs)voor meer informatie.
* Als u gegevens kopieert van RedShift naar Azure SQL Data Warehouse, Azure Blob Storage of Azure Data Lake Store, kunt u het gebruik van Unload ongedaan maken om de prestaties te verbeteren. Zie [Unload gebruiken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift)voor meer informatie.

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Kopieer gedrag**: Als u gegevens uit een ander gegevens archief op basis van een bestand kopieert, heeft de Kopieer activiteit drie opties via de eigenschap **copyBehavior** . Deze hiërarchie behoudt, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Hiërarchie relatiegegevens of behouden heeft weinig of geen performance overhead, maar samenvoegen van bestanden zorgt ervoor dat de prestatieoverhead te verhogen.
* **Bestands indeling en compressie**: Zie [overwegingen voor serialisatie en](#considerations-for-serialization-and-deserialization) deserialisatie en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om de prestaties te verbeteren.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Kopieer gedrag en prestatie implicatie**: Er zijn verschillende manieren om gegevens naar een SQL-sink te schrijven. Meer informatie over [Best practices voor het laden van gegevens in Azure SQL database](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database).

* **Patroon en batch gegevensgrootte**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren van de dezelfde hoeveelheid gegevens, biedt een grote rijgrootte betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van de gegevens worden doorgevoerd.
  * Met de Kopieer activiteit worden gegevens ingevoegd in een reeks batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens klein rijen is, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere batch overhead en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een mislukte kopieerbewerking veroorzaakt door overbelasting van de database.

### <a name="nosql-stores"></a>NoSQL-archieven

* Voor **Table storage**:
  * **Partitie**: Het schrijven van gegevens naar Interleaved partities vermindert de prestaties aanzienlijk. Sorteer de bron gegevens op partitie sleutel zodat de gegevens in één partitie na een andere worden ingevoegd. U kunt ook de logica aanpassen om de gegevens naar één partitie te schrijven.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie

Serialisatie en deserialisatie kunnen zich voordoen als uw gegevensset of uitvoer gegevensset van de invoer een bestand is. Zie [ondersteunde bestands-en compressie-indelingen](supported-file-formats-and-compression-codecs.md)voor meer informatie over ondersteunde bestands indelingen door de Kopieer activiteit.

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven in op basis van bestanden:
  * Als invoer-en uitvoer gegevens sets dezelfde instellingen voor de bestands indeling hebben, voert de service voor gegevens verplaatsing een *binaire kopie* uit zonder serialisatie of deserialisatie. Ziet u een hogere doorvoer in vergelijking met het scenario, waarin de bron en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer-en uitvoer gegevens sets beide in tekst indeling zijn en alleen het coderings type is anders, wordt de conversie alleen door de service voor gegevens verplaatsing versleuteld. Doet het nog serialisatie en deserialisatie, waardoor overhead in vergelijking met een binaire kopie prestaties.
  * Als invoer-en uitvoer gegevens sets beide verschillende bestands indelingen of verschillende configuraties hebben, zoals scheidings tekens, deserialiseren de gegevens verplaatsings service bron gegevens om te streamen, transformeren en vervolgens te serialiseren naar de uitvoer indeling die u hebt opgegeven. Deze bewerking resulteert in een veel belangrijker performance overhead in vergelijking met andere scenario's.
* Wanneer u bestanden kopieert naar of van een gegevens archief dat niet op een bestand is gebaseerd, bijvoorbeeld van een archief op basis van een bestand naar een relationele opslag, is de stap serialisatie of deserialisatie vereist. Deze stap resulteert in van aanzienlijke prestatieoverhead.

**Bestands indeling**: De bestands indeling die u kiest, kan de prestaties van het kopiëren beïnvloeden. Avro is bijvoorbeeld een compacte binaire indeling die metagegevens op met de gegevens slaat. Het biedt een brede ondersteuning in het Hadoop-ecosysteem voor het verwerken en uitvoeren van query's. Avro is duurder voor serialisatie en deserialisatie, wat leidt tot een lagere Kopieer doorvoer ten opzichte van de tekst indeling. 

Uw keuze van de bestandsindeling in de stroom voor de verwerking van zuinigste maken. Beginnen met:

- In welk formulier de gegevens worden opgeslagen, worden bron gegevens opgeslagen of worden ze geëxtraheerd uit externe systemen.
- De beste indeling voor opslag, analytische verwerking en query's.
- In de indeling waarin de gegevens moeten worden geëxporteerd naar Data Marts voor rapportage-en visualisatie hulpprogramma's.

Soms een bestandsindeling die is suboptimale voor lezen en schrijven prestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie

Als uw invoer-of uitvoer gegevensset een bestand is, kunt u de Kopieer activiteit instellen om compressie of decompressie uit te voeren terwijl de gegevens naar het doel worden geschreven. Wanneer u compressie kiest, maakt u een balans tussen input/output (I/O) en de CPU. Comprimeren van de kosten van de gegevens een extra in compute-resources. Maar hierna het i/o-netwerk en opslag verlaagt. Afhankelijk van uw gegevens ziet u mogelijk een boost in de totale Kopieer doorvoer.

**Codec**: Elke compressie-codec heeft voor delen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u krijgt de beste prestaties van Hive-query met bzip2 omdat u deze voor verwerking opsplitsen kunt. Gzip is de meest evenwichtige optie en wordt het meest gebruikt. Kies de codec die het beste bij uw end-to-end-scenario.

**Niveau**: U kunt kiezen uit twee opties voor elke compressie-codec: snelst gecomprimeerd en optimaal gecomprimeerd. Met de snelste compressie optie worden de gegevens zo snel mogelijk gecomprimeerd, zelfs als het resulterende bestand niet optimaal is gecomprimeerd. De optimaal gecomprimeerde optie meer tijd in de taakwachtrij doorbrengt op compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Een overweging**: Als u een grote hoeveelheid gegevens wilt kopiëren tussen een on-premises opslag en de Cloud, overweeg dan het gebruik van [gefaseerde kopie](#staged-copy) met compressie ingeschakeld. Het gebruik van tijdelijke opslag is handig wanneer de band breedte van uw bedrijfs netwerk en uw Azure-Services de beperkende factor zijn, en u wilt dat de invoer-en uitvoer gegevensset in de niet-gecomprimeerde vorm worden weer.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing

U kunt de eigenschap **columnMappings** in een Kopieer activiteit zo instellen dat alle of een subset van de invoer kolommen worden toegewezen aan de uitvoer kolommen. Nadat de data movement service leest de gegevens van de bron, moet deze kolomtoewijzing uitvoeren voor de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking vermindert kopie doorvoer.

Als uw bron-gegevensopslag waarin u kunt zoeken, bijvoorbeeld als het is een relationele opslagplaats, zoals SQL-Database of SQL Server, of als het is een NoSQL-archief, zoals Table storage of Azure Cosmos DB, kunt u de kolom filteren pushen en logica voor opnieuw ordenen de **query** eigenschap in plaats van in de kolomtoewijzing. Op deze manier treedt de projectie op terwijl de gegevens verplaatsings service gegevens uit de brongegevens opslag leest, waar deze veel efficiënter zijn.

Meer informatie over [schema toewijzing van de Kopieer activiteit](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Andere overwegingen

Als de grootte van de gegevens die u wilt kopiëren groot is, kunt u uw bedrijfs logica aanpassen om de gegevens verder te partitioneren. U kunt plannen dat de Kopieer activiteit vaker wordt uitgevoerd om de gegevens grootte voor elke Kopieer activiteit die wordt uitgevoerd, te verminderen.

Wees voorzichtig met het aantal gegevens sets en kopieer activiteiten dat Azure Data Factory nodig heeft om verbinding te maken met dezelfde gegevens opslag. Veel gelijktijdige kopie taken mogelijk beperken van een gegevensarchief en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen, fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeld scenario: Kopiëren van een on-premises SQL-Server naar Blob Storage

**Scenario**: Een pijp lijn is gebouwd om gegevens te kopiëren van een on-premises SQL-Server naar Blob-opslag in CSV-indeling. Als u sneller de taak voor het kopiëren, moeten de CSV-bestanden worden gecomprimeerd naar bzip2-indeling.

**Testen en analyseren**: De door Voer van de Kopieer activiteit is minder dan 2 MBps. Dit is veel langzamer dan de benchmark waarde voor prestaties.

**Prestatie analyse en-aanpassing**: Bekijk hoe de gegevens worden verwerkt en verplaatst om het prestatie probleem op te lossen.

- **Gegevens lezen**: De Integration runtime opent een verbinding met SQL Server en verzendt de query. SQL Server reageert door de gegevens stroom naar de Integration runtime via het intranet te verzenden.
- **Gegevens serialiseren en comprimeren**: De Integration runtime serialeert de gegevens stroom naar de CSV-indeling en comprimeert de gegevens naar een bzip2-stroom.
- **Gegevens schrijven**: De Integration runtime uploadt de bzip2-stroom naar Blob Storage via internet.

Zoals u kunt zien, worden de gegevens verwerkt en verplaatst naar een opeenvolgende streaming: SQL Server > LAN > Integration runtime > WAN > Blob Storage. De algehele prestaties worden gegated door de minimale door voer over de pijp lijn.

![Gegevensstroom](./media/copy-activity-performance/case-study-pic-1.png)

Een of meer van de volgende factoren kan leiden tot de bottleneck in de prestaties:

* **Bron**: SQL Server zichzelf heeft een lage door Voer vanwege zware belasting.
* **Zelf-hostende Integration runtime**:
  * **LAN**: Integration runtime bevindt zich ver van de SQL Server machine en heeft een verbinding met een lage band breedte.
  * **Integration runtime**: Integration runtime heeft de beperkingen voor het laden van de volgende bewerkingen bereikt:
    * **Serialisatie**: Het serialiseren van de gegevens stroom naar de CSV-indeling heeft een trage door voer.
    * **Compressie**: U hebt een trage compressie-codec gekozen, bijvoorbeeld bzip2, 2,8 MBps met Kern i7.
  * **WAN**: De band breedte tussen het bedrijfs netwerk en uw Azure-Services is laag, bijvoorbeeld T1 = 1.544 kbps; T2 = 6.312 kbps.
* **Sink**: Blob-opslag heeft een lage door voer. Dit scenario is waarschijnlijk niet mogelijk omdat de Service Level Agreement (SLA) mini maal 60 MBps garandeert.

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="references"></a>Verwijzingen

Hier volgen de prestaties en het afstemmen van verwijzingen voor een aantal ondersteunde gegevens archieven:

* Azure Storage, met Blob Storage en Table Storage: [Azure Storage schaalbaarheids doelen](../storage/common/storage-scalability-targets.md) en [Azure Storage controle lijst voor prestaties en schaal baarheid](../storage/common/storage-performance-checklist.md).
* Azure SQL Database: U kunt [de prestaties bewaken](../sql-database/sql-database-single-database-monitor.md) en het DTU-percentage (data base Trans Action Unit) controleren.
* Azure SQL Data Warehouse: De functionaliteit wordt gemeten in data warehouse units (Dwu's). Zie [reken kracht beheren in Azure SQL Data Warehouse (overzicht)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Prestatie niveaus in azure Cosmos DB](../cosmos-db/performance-levels.md).
* On-premises SQL Server: [Prestaties bewaken en afstemmen](https://msdn.microsoft.com/library/ms189081.aspx).
* On-premises Bestands server: [Prestaties afstemmen voor bestands servers](https://msdn.microsoft.com/library/dn567661.aspx).

## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen over Kopieer activiteiten:

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Schema toewijzing van Kopieer activiteit](copy-activity-schema-and-type-mapping.md)
- [Fouttolerantie van activiteit kopiëren](copy-activity-fault-tolerance.md)
