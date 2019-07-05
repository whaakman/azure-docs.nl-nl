---
title: Kopiëren en afstemmingshandleiding van activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de verplaatsing van gegevens in Azure Data Factory wanneer u de kopieeractiviteit.
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
ms.openlocfilehash: face3719f32ccb44e7479150e94417496141f90b
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509556"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiëren en afstemmingshandleiding van activiteit
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-copy-activity-performance.md)
> * [Huidige versie](copy-activity-performance.md)


De kopieeractiviteit in Azure Data Factory biedt een eersteklas veilige, betrouwbare en hoogwaardige gegevens laden. U kunt deze kopie tientallen van terabytes aan gegevens elke dag in uitgebreide tal van cloud en on-premises gegevensarchieven. Snel gegevens laden prestaties is essentieel om ervoor te zorgen dat u zich op het probleem van core big data richten kunt: het bouwen van geavanceerde analyseoplossingen en diepe inzichten ophalen uit alle gegevens.

Azure biedt een set geavanceerde datawarehouseoplossingen voor opslag en gegevens. De kopieeractiviteit biedt een uiterst geoptimaliseerde gegevens laden die gemakkelijk te configureren en instellen. U kunt gegevens laden met een één kopieeractiviteit:

* Azure SQL datawarehouse met 1,2 GBps.
* Azure Blob-opslag op 1,0 GBps.
* Azure Data Lake Store met 1,0 GBps.

Dit artikel wordt beschreven:

* [Naslaginformatie over prestatiecijfers](#performance-reference) voor ondersteunde bron- en sink-gegevensopslag voor het plannen van uw project.
* Functies die de kopie-doorvoer in verschillende scenario's, waaronder kunnen stimuleren [integratie gegevenseenheden](#data-integration-units) (DIUs), [kopie parallelle](#parallel-copy), en [gefaseerd kopiëren](#staged-copy).
* [Richtlijnen voor afstemmen van prestaties](#performance-tuning-steps) over het afstemmen van de prestaties en de belangrijkste factoren die invloed kunnen hebben op kopiëren.

> [!NOTE]
> Als u niet bekend bent met de kopieeractiviteit in het algemeen, ziet de [overzicht kopieeractiviteit](copy-activity-overview.md) voordat u dit artikel leest.
>

## <a name="performance-reference"></a>Naslaginformatie over prestaties

Als een verwijzing in de volgende tabel bevat het nummer van de kopie-doorvoer in MBps voor de opgegeven bron en sink-paren in een activiteit die één exemplaar wordt uitgevoerd op basis van het interne testen. Ter vergelijking: u kunt ook zien hoe de verschillende instellingen van [integratie gegevenseenheden](#data-integration-units) of [zelf-hostende integration runtime schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime) (meerdere knooppunten) kunt u op kopiëren.

![Matrix van prestaties](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Wanneer de kopieeractiviteit wordt uitgevoerd op een Azure integratieruntime, is de minimaal toegestane integratie gegevenseenheden (voorheen bekend als eenheden voor gegevensverplaatsing) twee. Als niet is opgegeven, ziet u de standaardeenheid-integratie wordt gebruikt [integratie gegevenseenheden](#data-integration-units).

**Die u moet weten:**

* Doorvoer wordt berekend met behulp van de volgende formule: [grootte van de gegevens lezen uit bron] / [duur van de uitvoering van activiteit kopiëren].
* De prestaties van verwijzing getallen in de tabel zijn gemeten met behulp van een [TPC-H](http://www.tpc.org/tpch/) gegevensset in een activiteit die één exemplaar wordt uitgevoerd. Testbestanden voor bestandsgebaseerde winkels zijn meerdere bestanden met 10 GB groot.
* In de Azure-gegevensopslag zijn de bron en sink in dezelfde Azure-regio.
* Voor hybride kopiëren tussen on-premises en cloud gegevensarchieven, elk zelf-hostende integration runtime-knooppunt werd uitgevoerd op een computer die gescheiden van de gegevensopslag met de volgende specificatie is. Als een enkele activiteit werd uitgevoerd, gebruikt de kopieerbewerking alleen een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte.
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
> U kunt hogere doorvoer bereiken met behulp van meer DIUs. Bijvoorbeeld, met 100 DIUs, kunt u gegevens kopiëren van Azure Blob-opslag naar Azure Data Lake Store met 1,0 GBps. Zie voor meer informatie over deze functie en de ondersteunde scenario, de [integratie gegevenseenheden](#data-integration-units) sectie. 

## <a name="data-integration-units"></a>Eenheden van de integratie van gegevens

Een eenheid van de integratie van gegevens is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in Azure Data Factory. Gegevenseenheid integratie is alleen van toepassing op [Azure integratieruntime](concepts-integration-runtime.md#azure-integration-runtime), maar niet [zelf-hostende integratieruntime](concepts-integration-runtime.md#self-hosted-integration-runtime).

De minimale DIUs voor die te maken van een kopieeractiviteit uitvoert is twee. Indien niet opgegeven, bevat de volgende tabel de standaard-DIUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario kopiëren | Standaard DIUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 4 en 32 liggen, afhankelijk van het aantal en de grootte van de bestanden |
| Gegevens kopiëren naar Azure SQL Database of Azure Cosmos DB |Tussen 4 en 16, afhankelijk van de sink-Azure SQL Database of Cosmos-DB-laag (het aantal dtu's / ru's) |
| Alle andere kopie-scenario 's | 4 |

Als u wilt deze standaardwaarde onderdrukken, Geef een waarde op voor de **dataIntegrationUnits** eigenschap als volgt te werk. De *toegestane waarden* voor de **dataIntegrationUnits** eigenschap is tot 256. De *werkelijke aantal DIUs* dat gebruikmaakt van de kopieerbewerking tijdens de uitvoering is gelijk aan of kleiner is dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk bij het configureren van meer eenheden voor een specifiek exemplaar gegevensbronnen en sinks, de [prestaties verwijzing](#performance-reference).

Hier ziet u de DIUs gebruikt voor elk exemplaar dat wordt uitgevoerd in de uitvoer van de activiteit kopiëren wanneer u een uitvoering van activiteit controleert. Zie voor meer informatie, [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

> [!NOTE]
> Instellen van DIUs groter is dan vier is momenteel van toepassing alleen wanneer u meerdere bestanden kopiëren van Azure Storage, Azure Data Lake Storage, Amazon S3, Google Cloud Storage, cloud FTP of SFTP naar een andere cloud-gegevensarchieven in de cloud.
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

Houd er rekening mee dat u in rekening op basis van de totale tijd van de kopieerbewerking gebracht bent. De totale duur die in rekening gebracht voor verplaatsing van gegevens is de som van de duur voor DIUs. Als een kopieertaak voorheen een uur duren voordat met twee cloudeenheden en nu wordt 15 minuten met acht cloudeenheden, blijft de totale factuur bijna hetzelfde.

## <a name="parallel-copy"></a>Parallelle kopiëren

U kunt de **parallelCopies** eigenschap om aan te geven van de parallelle uitvoering die u wilt dat de kopieeractiviteit te gebruiken. U kunt deze eigenschap beschouwen als het maximum aantal threads in de kopieeractiviteit die u kunt vanuit de bron lezen of schrijven naar de sinkgegevensopslag parallel.

Azure Data Factory bepaalt voor elke kopieeractiviteit die wordt uitgevoerd, het aantal parallelle exemplaren te gebruiken om gegevens te kopiëren van de bron-gegevens opslaan en op te slaan op de doelgegevens. Het aantal parallelle exemplaren die worden gebruikt, is afhankelijk van het type van de bron en sink die u gebruikt.

| Scenario kopiëren | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van bestanden |Afhankelijk van de grootte van de bestanden en het aantal DIUs gebruikt om gegevens tussen twee cloudlocaties voor gegevensopslag, of de fysieke configuratie van de zelf-hostende integration runtime-machine te kopiëren. |
| Gegevens kopiëren van een brongegevensopslag naar Azure Table storage |4 |
| Alle andere kopieerscenario 's |1 |

> [!TIP]
> Bij het kopiëren van gegevens tussen winkels op basis van bestanden, biedt de standaardinstelling meestal u de beste doorvoer. Het standaardgedrag is automatisch bepaald op basis van uw bestandspatroon bron.

Voor het beheren van de belasting van de machines waarop uw gegevens opslaat, of om af te stemmen kopiëren, kunt u de standaardwaarde overschrijven en geef een waarde op voor de **parallelCopies** eigenschap. De waarde moet een geheel getal groter dan of gelijk aan 1 zijn. Tijdens de uitvoering voor de beste prestaties met de kopieeractiviteit maakt gebruik van een waarde die kleiner is dan of gelijk zijn aan de waarde die u hebt ingesteld.

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

**Die u moet weten:**

* Bij het kopiëren van gegevens tussen bestanden zijn gebaseerd, **parallelCopies** bepaalt de parallelle uitvoering op bestandsniveau. De logische groepen te verdelen binnen een enkel bestand onder gebeurt automatisch en transparant. Het is ontwikkeld voor het gebruik van het beste geschikt segment grootte voor een bepaalde bron store gegevenstype om gegevens parallel te laden en rechthoekige te **parallelCopies**. Het werkelijke aantal parallelle exemplaren data movement service wordt gebruikt voor de kopieerbewerking tijdens runtime is niet meer dan het aantal bestanden dat u hebt. Als flattenhierarchy is **mergeFile**, de copy-activiteit niet profiteren van parallelle uitvoering op bestandsniveau.
* Als u gegevens kopieert van winkels die niet op bestanden gebaseerde (met uitzondering van Oracle-database als bron met het partitioneren van gegevens is ingeschakeld) naar de winkels die op basis van bestanden, data movement service negeert de **parallelCopies** eigenschap. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.
* De **parallelCopies** eigenschap staat haaks op **dataIntegrationUnits**. De eerste wordt in de eenheden voor de integratie van gegevens geteld.
* Wanneer u een waarde opgeven voor de **parallelCopies** eigenschap, houd rekening met de toename van de belasting op de bron en sink-gegevensopslag. Ook rekening houden met de toename van de belasting voor de zelf-hostende integratieruntime als de kopieerbewerking bevoegd is, bijvoorbeeld voor hybride-exemplaar. Deze toename load gebeurt er met name wanneer u hebt meerdere activiteiten of gelijktijdige uitvoeringen van de dezelfde activiteiten worden uitgevoerd voor hetzelfde gegevensarchief. Als u merkt dat het gegevensarchief of de zelf-hostende integratieruntime met de belasting wordt overbelast, vermindert u de **parallelCopies** waarde om te ontlasten van de belasting.

## <a name="staged-copy"></a>Gefaseerd kopiëren

Wanneer u gegevens van een brongegevensarchief naar een sink-gegevensarchief kopieert, kunt u Blob-opslag gebruiken als een tussentijdse faseringsopslag. Fasering is vooral nuttig in de volgende gevallen:

- **Wilt u gegevens uit verschillende gegevensarchieven opnemen in SQL Data Warehouse met PolyBase.** SQL Data Warehouse gebruikmaakt van een grote hoeveelheid gegevens laden in SQL Data Warehouse PolyBase als een mechanisme voor hoge doorvoer. De brongegevens moet zich in Blob storage of Azure Data Lake Store en moet deze voldoen aan aanvullende criteria. Bij het laden van gegevens vanuit een ander gegevensarchief dan Blob storage of Azure Data Lake Store, kunt u gegevens kopiëren via tussentijdse staging Blob-opslag kunt activeren. In dat geval voert de Azure Data Factory de benodigde gegevenstransformaties om ervoor te zorgen dat het voldoet aan de vereisten van PolyBase. Vervolgens maakt PolyBase om gegevens te laden in SQL Data Warehouse efficiënt gebruik. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Soms duurt het even uit te voeren van een hybride verplaatsing van gegevens (dat wil zeggen, om te kopiëren uit een on-premises gegevens kunnen worden opgeslagen naar een gegevensarchief cloud) via een trage netwerkverbinding.** Voor betere prestaties kunt u gefaseerd kopiëren voor het comprimeren van de gegevens on-premises, zodat er minder tijd om gegevens te verplaatsen naar de tijdelijke gegevensopslag in de cloud nodig. U kunt vervolgens de gegevens in de staging-archief decomprimeren voordat u naar de doelgegevensopslagplaats laden.
- **U wilt niet dat andere poorten dan poort 80 en poort 443 openen in uw firewall vanwege zakelijke IT-beleid.** Wanneer u gegevens van een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse-sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en firewall van uw bedrijf te activeren. In dit scenario wordt kunt gefaseerd kopiëren profiteren van de zelf-hostende integratieruntime eerst om gegevens te kopiëren naar een Blob-opslag staging-exemplaar via HTTP of HTTPS op poort 443. Deze kan vervolgens de gegevens in SQL-Database of SQL Data Warehouse laden van Blob-opslag staging. In deze stroom moet u geen poort 1433 in te schakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerd kopiëren werkt

Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens worden gekopieerd uit de brongegevensopslag naar de Blob-opslag staging (Voeg uw eigen). Vervolgens is de gegevens van de faseringsgegevensopslag gekopieerd naar de sinkgegevensopslag. Azure Data Factory wordt automatisch de gegevensstroom twee fasen voor u beheerd. Tijdelijke gegevens uit de tijdelijke opslag ruimt in Azure Data Factory ook nadat de verplaatsing van gegevens voltooid is.

![Gefaseerd kopiëren](media/copy-activity-performance/staged-copy.png)

Wanneer u verplaatsing van gegevens met behulp van een faseringsopslag activeert, kunt u opgeven of u wilt de gegevens moeten worden gecomprimeerd voordat u gegevens uit de brongegevensopslag verplaatsen opslaan voor tussentijdse of tijdelijke gegevens opslaan en vervolgens gedecomprimeerd voordat u gegevens van een tijdelijke of staging dat verplaatsen een archief voor de sink-gegevensopslag.

U kunt gegevens tussen twee gegevensarchieven die zijn verbonden via verschillende zelfgehoste IRs met noch zonder gefaseerd kopiëren op dit moment niet kopiëren. U kunt twee expliciet keten kopieeractiviteit om te kopiëren vanuit de bron voor fasering en vervolgens van de staging-sink-configureren voor dergelijke scenario.

### <a name="configuration"></a>Configuratie

Configureer de **enableStaging** instellen in de kopieeractiviteit om op te geven of u wilt dat de gegevens naar de tijdelijk opgeslagen in Blob-opslag voordat u deze te in een doelgegevensarchief laden. Als u instelt **enableStaging** naar `TRUE`, geeft u de aanvullende eigenschappen in de volgende tabel weergegeven. U moet ook een Azure-opslag maken of opslag handtekening gekoppelde access-service voor het Faseren van als u dit niet hebt gedeeld.

| Eigenschap | Description | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| enableStaging |Geef op of u wilt het kopiëren van gegevens via een tussentijdse opslag staging. |False |Nee |
| linkedServiceName |Geef de naam van een [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) gekoppelde service, die verwijst naar het exemplaar van de opslag die u als een tussentijdse faseringsopslag. <br/><br/> U kunt opslag met een shared access signature niet gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| pad |Geef het pad op Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u een pad niet opgeeft, wordt een container voor het opslaan van tijdelijke gegevens gemaakt in de service. <br/><br/> Geef een pad alleen als u Storage met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| enableCompression |Hiermee geeft u op of gegevens moeten worden gecomprimeerd voordat deze wordt gekopieerd naar de bestemming. Deze instelling beperkt de hoeveelheid gegevens die worden overgebracht. |False |Nee |

>[!NOTE]
> Als u gefaseerd kopiëren met compressie is ingeschakeld, wordt de service-principal gebruiken of MSI-verificatie voor het Faseren van de gekoppelde blob-service wordt niet ondersteund.

Hier volgt de voorbeelddefinitie van een van een kopieeractiviteit met de eigenschappen die worden beschreven in de voorgaande tabel:

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

U betaalt op basis van twee stappen: duur van de kopieerbewerking en type kopiëren.

* Wanneer u staging tijdens het kopiëren van een cloud, die is kopiëren van gegevens uit een cloud-gegevensarchief naar een andere cloudgegevensopslag beide fasen die zijn aangewezen door Azure integratieruntime, dat u in de [som van de duur van de kopieerbewerking voor de stap 1 en 2] x [cloud kopie unit price] wordt gebracht.
* Wanneer u tijdens het kopiëren van een hybride, die is kopiëren van gegevens uit een on-premises gegevensarchief naar een cloud-gegevensarchief, fasering één fase gemachtigd door een zelf-hostende integratieruntime, moet u betalen voor [duur van de kopieerbewerking hybride] x [hybride exemplaar unit price] + [cloud duur van de kopieerbewerking] x [cloud kopie unit price].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen

Volg deze stappen voor het afstemmen van de prestaties van uw Azure Data Factory-service met de kopieeractiviteit.

1. **Stel een basislijn.** Test uw pijplijn met behulp van de kopieeractiviteit op basis van een representatieve steekproef tijdens de ontwikkelingsfase. Uitvoeringsdetails en prestatiekenmerken volgende verzamelen [kopiëren activiteitenbewaking](copy-activity-overview.md#monitoring).

2. **Diagnose en prestaties te optimaliseren.** Als de prestaties die u ziet dat niet voldoet aan uw verwachtingen, Identificeer knelpunten. Vervolgens prestaties als u wilt verwijderen of het effect van knelpunten te optimaliseren.

    In sommige gevallen, wanneer u een kopieeractiviteit in Azure Data Factory uitvoert, ziet u een bericht "Het afstemmen van prestaties tips" boven de [kopieeractiviteit bewaking pagina](copy-activity-overview.md#monitor-visually), zoals wordt weergegeven in het volgende voorbeeld. Het bericht ziet u het knelpunt die is geïdentificeerd voor de opgegeven kopie. Ook helpt u op wat u wilt wijzigen in boost kopie doorvoer. De tips voor het afstemmen van prestaties bieden momenteel suggesties, zoals:

    - PolyBase gebruiken wanneer u gegevens naar Azure SQL Data Warehouse kopiëren.
    - Azure Cosmos DB-Aanvraageenheden of Azure SQL Database dtu's (Database Throughput Units) verhogen als de bron aan de store gegevens het knelpunt is.
    - Verwijder de onnodige gefaseerd kopiëren.

    De afstemming van regels wordt geleidelijk ook uitgebreid zijn.

    **Voorbeeld: Kopiëren naar Azure SQL Database met tips voor het afstemmen van prestaties**

    In dit voorbeeld kennisgevingen Azure Data Factory tijdens een kopie die wordt uitgevoerd, de sink die Azure SQL Database hoog DTU-gebruik, die de schrijfbewerkingen vertraagt wordt bereikt. Het voorstel is het verhogen van de Azure SQL Database-laag met meer dtu's. 

    ![Kopiëren met tips afstemmen van de prestaties controleren](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Hier volgen bovendien enkele algemene overwegingen. Er is een volledige beschrijving van de prestatiediagnose van buiten het bereik van dit artikel.

   * Prestatiefuncties:
     * [Parallelle kopiëren](#parallel-copy)
     * [Eenheden voor gegevensintegratie](#data-integration-units)
     * [Gefaseerd kopiëren](#staged-copy)
     * [Zelf-hostende integration runtime schaalbaarheid](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Zelf-hostende Integration Runtime](#considerations-for-self-hosted-integration-runtime)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)

3. **De configuratie van de uitbreiden naar uw volledige gegevensset.** Wanneer u tevreden met de resultaten van het uitvoeren en de prestaties bent, kunt u de definitie en de pijplijn voor uw volledige gegevensset kunt uitbreiden.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Overwegingen voor het zelf-hostende integratieruntime

Als de kopieeractiviteit wordt uitgevoerd op een zelf-hostende integratieruntime, Let op het volgende:

**Setup**: U wordt aangeraden dat u een toegewezen machine in host integratieruntime. Zie [overwegingen voor het gebruik van zelf-hostende integratieruntime](concepts-integration-runtime.md).

**Uitschalen**: Een enkele logische zelf-hostende integratieruntime met een of meer knooppunten kan meerdere kopie activiteit wordt uitgevoerd op hetzelfde moment gelijktijdig fungeren. Als u zware nodig voor hybride verplaatsing van gegevens met een groot aantal uitvoeringen van activiteit gelijktijdige kopiëren of een grote hoeveelheid gegevens hebt te kopiëren, kunt u overwegen [geschaalde uitbreiding van de zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md#high-availability-and-scalability) voor het inrichten van aanvullende bronnen te kopiëren.

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet wordt overspoeld door andere werkbelastingen die worden uitgevoerd op of tegen.

Zie voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensopslag. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens van Blob storage naar SQL Data Warehouse kopiëren, kunt u overwegen PolyBase om prestaties te verbeteren. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Als u gegevens uit HDFS naar Azure Blob storage of Azure Data Lake Store kopiëren, kunt u overwegen DistCp om prestaties te verbeteren. Zie voor meer informatie, [DistCp gebruiken om te kopiëren van gegevens uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Als u gegevens van Redshift naar Azure SQL Data Warehouse, Azure BLob storage of Azure Data Lake Store kopiëren, kunt u overwegen verwijderen om prestaties te verbeteren. Zie voor meer informatie, [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Gemiddelde van de bestandsgrootte en het aantal bestanden**: De kopieeractiviteit brengt gegevens één bestand tegelijk. Met de dezelfde hoeveelheid gegevens worden verplaatst, wordt de algehele doorvoer lager als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Indien mogelijk, Combineer kleine bestanden in grotere bestanden om te krijgen hogere doorvoer.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren, de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Een gegevenspatroon**: Uw tabelschema is van invloed op de doorvoer van de kopie. Een grote rijgrootte biedt betere prestaties dan een kleine rijgrootte te kopiëren van de dezelfde hoeveelheid gegevens. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of een opgeslagen procedure**: Optimaliseer de logica van de query of opgeslagen procedure die u in de bron voor kopiëren-activiteit opgeeft voor het ophalen van gegevens efficiënter.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink

### <a name="general"></a>Algemeen

Zorg ervoor dat het onderliggende gegevensarchief niet wordt overspoeld door andere werkbelastingen die worden uitgevoerd op of tegen.

Zie voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensopslag. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

* Als u gegevens van een gegevensarchief naar Azure SQL Data Warehouse kopiëren, kunt u overwegen PolyBase om prestaties te verbeteren. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Als u gegevens uit HDFS naar Azure Blob storage of Azure Data Lake Store kopiëren, kunt u overwegen DistCp om prestaties te verbeteren. Zie voor meer informatie, [DistCp gebruiken om te kopiëren van gegevens uit HDFS](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Als u gegevens van Redshift naar Azure SQL Data Warehouse, Azure Blob storage of Azure Data Lake Store kopiëren, kunt u overwegen verwijderen om prestaties te verbeteren. Zie voor meer informatie, [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden

* **Kopieer gedrag**: Als u gegevens van een ander bestand op basis van gegevensarchief kopieert, de copy-activiteit heeft drie opties via de **copyBehavior** eigenschap. Deze hiërarchie behoudt, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Hiërarchie relatiegegevens of behouden heeft weinig of geen performance overhead, maar samenvoegen van bestanden zorgt ervoor dat de prestatieoverhead te verhogen.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren, de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens

* **Kopieer de gevolgen voor de werking en prestaties**: Er zijn verschillende manieren om gegevens te schrijven naar een SQL-sink. Meer informatie uit [Best practice om gegevens te laden in Azure SQL Database](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database).

* **Patroon en batch gegevensgrootte**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren van de dezelfde hoeveelheid gegevens, biedt een grote rijgrootte betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van de gegevens worden doorgevoerd.
  * De kopieeractiviteit voegt de gegevens in een reeks van batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens klein rijen is, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere batch overhead en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een mislukte kopieerbewerking veroorzaakt door overbelasting van de database.

### <a name="nosql-stores"></a>NoSQL-archieven

* Voor **Table storage**:
  * **Partitie**: Gegevens schrijven naar een interleaved partities aanzienlijk vermindert de prestaties. Sorteren op partitiesleutel uw brongegevens zodat de gegevens efficiënt in één partitie is ingevoegd achter elkaar. Of u de logica voor het schrijven van de gegevens naar één partitie kunt aanpassen.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie

Serialisatie en deserialisatie kunnen optreden wanneer de invoergegevensset of uitvoergegevensset een bestand is. Zie voor meer informatie over ondersteunde bestandsindelingen door kopieeractiviteit [ondersteunde indelingen voor bestanden en compressie](supported-file-formats-and-compression-codecs.md).

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven in op basis van bestanden:
  * Als invoer- en uitvoergegevenssets beide hebben hetzelfde of er zijn geen bestandsindelingsinstellingen, data movement service wordt uitgevoerd een *binaire kopie* zonder serialisatie of deserialisatie. Ziet u een hogere doorvoer in vergelijking met het scenario, waarin de bron en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer- en uitvoergegevenssets beide zijn in de tekstindeling en alleen de codering type is anders, data movement service biedt alleen codering conversie. Doet het nog serialisatie en deserialisatie, waardoor overhead in vergelijking met een binaire kopie prestaties.
  * Brongegevens om te streamen, transformeren en deze vervolgens te serialiseren in de indeling van de uitvoer die u hebt aangegeven als invoer- en uitvoergegevenssets beide hebben verschillende bestandsindelingen of verschillende configuraties, zoals scheidingstekens, data movement service worden gedeserialiseerd. Deze bewerking resulteert in een veel belangrijker performance overhead in vergelijking met andere scenario's.
* De serialisatie of deserialisatie stap is vereist als u bestanden kopiëren naar of van een gegevensarchief dat niet gebaseerd is, bijvoorbeeld in een store, op basis van bestanden naar een relationele opslagplaats, bestand. Deze stap resulteert in van aanzienlijke prestatieoverhead.

**Bestandsindeling**: De bestandsindeling die u kiest kan kopiëren prestaties beïnvloeden. Avro is bijvoorbeeld een compacte binaire indeling die metagegevens op met de gegevens slaat. Het biedt een brede ondersteuning in het Hadoop-ecosysteem voor het verwerken en uitvoeren van query's. Avro is duurder voor serialisatie en deserialisatie, wat in het onderste exemplaar doorvoer in vergelijking met de tekstindeling resulteert. 

Uw keuze van de bestandsindeling in de stroom voor de verwerking van zuinigste maken. Beginnen met:

- Welke vorm de gegevens worden opgeslagen in de brongegevens of moet worden geëxtraheerd uit externe systemen.
- De beste indeling voor opslag, analytische verwerking en uitvoeren van query's.
- In welke indeling moet de gegevens worden geëxporteerd naar de datamarts voor hulpprogramma's voor rapportage en visualisatie.

Soms een bestandsindeling die is suboptimale voor lezen en schrijven prestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie

Als uw gegevensset voor invoer- of een bestand is, kunt u de kopieeractiviteit om uit te voeren van compressie of decompressie als gegevens naar de bestemming schrijft instellen. Wanneer u compressie kiest, maakt u een balans tussen input/output (I/O) en de CPU. Comprimeren van de kosten van de gegevens een extra in compute-resources. Maar hierna het i/o-netwerk en opslag verlaagt. Afhankelijk van uw gegevens ziet u mogelijk een boost in de algemene doorvoer van de kopie.

**Codec**: Elke compressiecodec heeft voordelen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u krijgt de beste prestaties van Hive-query met bzip2 omdat u deze voor verwerking opsplitsen kunt. Gzip is de meest met gelijke taakverdeling optie en het is vaak het meest gebruikt. Kies de codec die het beste bij uw end-to-end-scenario.

**Niveau**: U kunt kiezen uit twee opties voor elke compressiecodec: snelste gecomprimeerde en optimaal gecomprimeerd. De snelste gecomprimeerde optie worden de gegevens zo snel mogelijk, gecomprimeerd, zelfs als het resulterende bestand is niet optimaal gecomprimeerd. De optimaal gecomprimeerde optie meer tijd in de taakwachtrij doorbrengt op compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Overweging**: Als u wilt kopiëren van een grote hoeveelheid gegevens tussen een on-premises gegevensopslag en de cloud, kunt u overwegen [gefaseerd kopiëren](#staged-copy) met gebruik van compressie. Tijdelijke opslag is nuttig wanneer de bandbreedte van uw bedrijfsnetwerk en uw Azure-services de beperkende factor is en u wilt dat de invoergegevensset en de uitvoer van de gegevensset voor de niet-gecomprimeerde vorm.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing

U kunt instellen dat de **columnMappings** eigenschap in een kopieeractiviteit alle kaart of een subset van de invoer kolommen aan de uitvoerkolommen. Nadat de data movement service leest de gegevens van de bron, moet deze kolomtoewijzing uitvoeren voor de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking vermindert kopie doorvoer.

Als uw bron-gegevensopslag waarin u kunt zoeken, bijvoorbeeld als het is een relationele opslagplaats, zoals SQL-Database of SQL Server, of als het is een NoSQL-archief, zoals Table storage of Azure Cosmos DB, kunt u de kolom filteren pushen en logica voor opnieuw ordenen de **query** eigenschap in plaats van in de kolomtoewijzing. Op deze manier de projectie treedt op wanneer data movement service gegevens uit de brongegevensopslag leest, waar het is veel efficiënter.

Meer informatie uit [schematoewijzing activiteit kopiëren](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Andere overwegingen

Als de grootte van gegevens die u wilt kopiëren groot is, kunt u uw bedrijfslogica naar verdere partitie de gegevens kunt aanpassen. De kopieeractiviteit om uit te voeren vaker Verklein de gegevensgrootte voor elke kopieeractiviteit die wordt uitgevoerd, kunt u plannen.

Wees voorzichtig met het aantal gegevenssets en kopieert u activiteiten waarvoor Azure Data Factory verbinding maken met hetzelfde gegevensarchief op hetzelfde moment. Veel gelijktijdige kopie taken mogelijk beperken van een gegevensarchief en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen, fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeldscenario: Kopiëren uit een on-premises SQL server naar Blob storage

**Scenario**: Een pijplijn is gebouwd om gegevens te kopiëren uit een on-premises SQL server naar Blob-opslag in CSV-indeling. Als u sneller de taak voor het kopiëren, moeten de CSV-bestanden worden gecomprimeerd naar bzip2-indeling.

**Test- en analyse**: De doorvoer van de kopieerbewerking is minder dan 2 MBps, dat is veel lager is dan de benchmark prestaties.

**Analyse van prestaties en afstemmen**: Voor het oplossen van het prestatieprobleem, laten we kijken hoe de gegevens worden verwerkt en verplaatst.

- **Gegevens lezen**: De integratieruntime een verbinding met SQL Server wordt geopend en stuurt de query. SQL-Server reageert door de gegevensstroom te sturen naar de integratieruntime via het intranet.
- **Serialiseren en gegevens comprimeren**: De integratieruntime serialiseert de gegevensstroom naar de CSV-indeling en de gegevens naar een stream bzip2 worden gecomprimeerd.
- **Gegevens schrijven**: De integratieruntime wordt de stroom bzip2 geüpload naar Blob storage via internet.

Zoals u ziet, worden de gegevens worden verwerkt en verplaatst op sequentiële wijze streaming: SQL Server > LAN > integratieruntime > WAN > Blob storage. De algehele prestaties worden geregeld door de minimale doorvoer via de pijplijn.

![Gegevensstroom](./media/copy-activity-performance/case-study-pic-1.png)

Een of meer van de volgende factoren kan leiden tot de bottleneck in de prestaties:

* **Bron**: SQL-Server zelf heeft lage doorvoer vanwege de zware belastingen.
* **Zelf-hostende integratieruntime**:
  * **LAN**: Integratieruntime bevindt zich ver van de SQL Server-machine en een verbinding met een lage bandbreedte heeft.
  * **Integratieruntime**: Integratieruntime heeft bereikt van de load-beperkingen voor de volgende bewerkingen uitvoeren:
    * **Serialisatie**: Tijdens het serialiseren van de gegevensstroom naar de CSV-indeling is traag doorvoer.
    * **Compressie**: U hebt gekozen, een trage compressiecodec, bijvoorbeeld, bzip2, die is 2,8 MBps met Core i7.
  * **WAN**: De bandbreedte tussen het bedrijfsnetwerk en uw Azure-services wordt belast, bijvoorbeeld T1 = 1,544 kbps. Tijdstip T2 = 6,312 kbps.
* **Sink**: BLOB-opslag heeft lage doorvoer. In dit scenario is het onwaarschijnlijk omdat de service level agreement (SLA) een minimum van 60 MBps garandeert.

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="references"></a>Verwijzingen

Hier zijn prestaties controleren en afstemmen van verwijzingen voor enkele van de ondersteunde gegevensarchieven:

* Azure Storage, met inbegrip van Blob storage en Table storage: [Azure Storage-schaalbaarheidsdoelen](../storage/common/storage-scalability-targets.md) en [controlelijst voor de prestaties en schaalbaarheid van Azure Storage](../storage/common/storage-performance-checklist.md).
* Azure SQL Database: U kunt [bewaken van de prestaties](../sql-database/sql-database-single-database-monitor.md) en controleert u het percentage van de Database Transaction Unit (DTU).
* Azure SQL Data Warehouse: De mogelijkheid wordt gemeten in Data Warehouse Units (dwu's). Zie [beheren in Azure SQL Data Warehouse (overzicht)-rekenkracht](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Prestatieniveaus in Azure Cosmos DB](../cosmos-db/performance-levels.md).
* On-premises SQL Server: [Controleren en afstemmen van prestaties](https://msdn.microsoft.com/library/ms189081.aspx).
* On-premises bestandsserver: [Prestaties afstemmen voor bestandsservers](https://msdn.microsoft.com/library/dn567661.aspx).

## <a name="next-steps"></a>Volgende stappen
Zie de andere kopie activiteit-artikelen:

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Schematoewijzing activiteit kopiëren](copy-activity-schema-and-type-mapping.md)
- [Fouttolerantie van activiteit kopiëren](copy-activity-fault-tolerance.md)
