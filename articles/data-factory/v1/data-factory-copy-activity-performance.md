---
title: Kopiëren en afstemmingshandleiding activiteit | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de verplaatsing van gegevens in Azure Data Factory wanneer u een Kopieeractiviteit gebruiken.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: ec8c58e4ced0d8df958e242b9c1671aeed8c2ee6
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812086"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiëren en afstemmingshandleiding van activiteit

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-copy-activity-performance.md)
> * [Versie 2 (huidige versie)](../copy-activity-performance.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [afstemmingshandleiding voor Data Factory en activiteit kopiëren](../copy-activity-performance.md).

Copy Activity in Azure Data Factory biedt een eersteklas veilige, betrouwbare en hoogwaardige gegevens laden. Dit kunt u gegevens kunt kopiëren tientallen van terabytes aan gegevens elke dag in uitgebreide tal van cloud en het on-premises gegevensarchieven. Zeer snel gegevens laden van prestaties is erg belangrijk om te controleren of u zich kunt richten op het probleem van de 'big data' core: het bouwen van geavanceerde analyseoplossingen en diepe inzichten ophalen uit alle gegevens.

Azure biedt een set geavanceerde datawarehouseoplossingen voor opslag en gegevensbeheer en Copy Activity biedt een uiterst geoptimaliseerde gegevens laden die gemakkelijk te configureren en instellen. Met slechts één kopieeractiviteit, kunt u bereiken:

* Het laden van gegevens in **Azure SQL Data Warehouse** op **1,2 GBps**. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).
* Het laden van gegevens in **Azure Blob-opslag** op **1,0 GBps**
* Het laden van gegevens in **Azure Data Lake Store** op **1,0 GBps**

Dit artikel wordt beschreven:

* [Naslaginformatie over prestatiecijfers](#performance-reference) voor ondersteunde bron- en sink-gegevensopslag voor het plannen van uw project.
* Functies die de kopie-doorvoer in verschillende scenario's, met inbegrip van kunnen verhogen [eenheden voor gegevensverplaatsing in de cloud](#cloud-data-movement-units), [kopie parallelle](#parallel-copy), en [gefaseerd kopiëren](#staged-copy);
* [Richtlijnen voor afstemmen van prestaties](#performance-tuning-steps) over het afstemmen van de prestaties en de belangrijkste factoren die de kopieerprestaties kunnen beïnvloeden.

> [!NOTE]
> Als u niet bekend met de Kopieeractiviteit in het algemeen bent, Zie [gegevens verplaatsen met behulp van Kopieeractiviteit](data-factory-data-movement-activities.md) voordat het lezen van dit artikel.
>

## <a name="performance-reference"></a>Naslaginformatie over prestaties

Bevat onderstaande tabel als referentie, het nummer van de doorvoer kopiëren in MBps voor de opgegeven bron en sink-paren op basis van interne testen. Ter vergelijking: u kunt ook zien hoe de verschillende instellingen van [eenheden voor gegevensverplaatsing in de cloud](#cloud-data-movement-units) of [Data Management Gateway schaalbaarheid](data-factory-data-management-gateway-high-availability-scalability.md) (meerdere gateway-knooppunten) kunt u op kopiëren.

![Matrix van prestaties](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>In Azure Data Factory versie 1, de eenheden voor gegevensverplaatsing minimale cloud voor cloud-naar-cloud-exemplaar is twee. Als niet is opgegeven, ziet u standaardeenheden voor gegevensverplaatsing worden gebruikt [eenheden voor gegevensverplaatsing in de cloud](#cloud-data-movement-units).

**Die u moet weten:**
* Doorvoer wordt berekend met behulp van de volgende formule: [grootte van de gegevens lezen uit bron] / [Copy Activity uitvoeringsduur].
* De prestaties van verwijzing getallen in de tabel zijn gemeten met behulp van [TPC-H](http://www.tpc.org/tpch/) gegevensset in een activiteit die één exemplaar wordt uitgevoerd.
* In de Azure-gegevensopslag zijn de bron en sink in dezelfde Azure-regio.
* Voor hybride kopiëren tussen on-premises en cloud gegevensarchieven, elk gateway-knooppunt werd uitgevoerd op een computer die gescheiden van de gegevens on-premises gegevensopslag met onderstaande specificatie is. Als een enkele activiteit werd uitgevoerd op de gateway, gebruikt de kopieerbewerking alleen een klein deel van de testmachine CPU, geheugen of netwerkbandbreedte. Meer informatie uit [overweging voor Data Management Gateway](#considerations-for-data-management-gateway).
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
> U kunt hogere doorvoer bereiken door gebruik te maken van meer eenheden voor gegevensverplaatsing (DMUs) dan de standaard maximale DMUs is 32 voor cloud-naar-cloud copy activity uitvoeren. Bijvoorbeeld, met 100 DMUs, kunt u bereiken kopiëren van gegevens van Azure-Blob in Azure Data Lake Store op **1.0GBps**. Zie de [eenheden voor gegevensverplaatsing in de Cloud](#cloud-data-movement-units) sectie voor meer informatie over deze functie en de ondersteunde scenario. Neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/) meer DMUs aanvragen.

## <a name="parallel-copy"></a>Parallelle kopiëren
U kunt gegevens uit de bron lezen of schrijven van gegevens naar de bestemming **parallel binnen één Kopieeractiviteit**. Deze functie verbetert de doorvoer van een kopieerbewerking en vermindert de tijd die nodig zijn om gegevens te verplaatsen.

Deze instelling wijkt af van de **gelijktijdigheid** eigenschap in het definitie van de activiteit. De **gelijktijdigheid** eigenschap bepaalt het aantal **gelijktijdige Kopieeractiviteit wordt uitgevoerd** om gegevens te verwerken van andere activiteit windows (1 uur aan 2 uur, 2 uur tot 3 uur, 3 uur tot en met 4 uur, enzovoort). Deze mogelijkheid is handig wanneer u een historische belasting uitvoeren. De mogelijkheid parallelle kopie is van toepassing op een **enkele van de uitvoering van activiteiten**.

We bekijken een voorbeeldscenario. In het volgende voorbeeld moeten meerdere segmenten in het verleden worden verwerkt. Data Factory een exemplaar van Copy Activity (een activiteit uitvoeren) voor elk segment wordt uitgevoerd:

* Het gegevenssegment uit het eerste venster van activiteit (1 uur aan 2 uur) == > uitvoering van activiteit 1
* Het gegevenssegment uit het tweede venster van activiteit (2 uur op 3 uur) == > 2 voor uitvoering van activiteit
* Het gegevenssegment uit het tweede venster van activiteit (3 uur op 4 uur) == > activiteit 3 uitvoeren

Enzovoort.

In dit voorbeeld, wanneer de **gelijktijdigheid** waarde is ingesteld op 2, **uitvoering van activiteit 1** en **uitvoering van activiteit 2** gegevens kopiëren van twee activiteitsvensters **gelijktijdig** data movement prestaties te verbeteren. Echter als meerdere bestanden gekoppeld aan 1 uitvoering van activiteit zijn, kopieert data movement service bestanden van de bron naar het doelbestand voor één op een tijdstip.

### <a name="cloud-data-movement-units"></a>Eenheden voor gegevensverplaatsing cloud
Een **cloud data movement eenheid (DMU)** is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in Data Factory. DMU is van toepassing voor kopieerbewerkingen van cloud-naar-cloud, maar niet in een hybride-exemplaar.

**De eenheden voor gegevensverplaatsing minimale cloud om te kopiëren activiteit die wordt uitgevoerd is twee.** Indien niet opgegeven, bevat de volgende tabel de standaard-DMUs gebruikt in scenario's voor verschillende kopiëren:

| Scenario kopiëren | Standaard DMUs bepaald door de service |
|:--- |:--- |
| Gegevens kopiëren tussen winkels op basis van bestanden | Tussen 4 en 16, afhankelijk van het aantal en de grootte van de bestanden. |
| Alle andere kopieerscenario 's | 4 |

Als u wilt deze standaardwaarde onderdrukken, Geef een waarde op voor de **cloudDataMovementUnits** eigenschap als volgt te werk. De **toegestane waarden** voor de **cloudDataMovementUnits** eigenschap 2, 4, 8, 16 of 32 zijn. De **werkelijke aantal cloud DMUs** dat gebruikmaakt van de kopieerbewerking tijdens de uitvoering is gelijk aan of kleiner is dan de geconfigureerde waarde, afhankelijk van het patroon van uw gegevens. Zie voor informatie over het niveau van prestatieverbetering krijgt u mogelijk bij het configureren van meer eenheden voor een specifiek exemplaar gegevensbronnen en sinks, de [prestaties verwijzing](#performance-reference).

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
> Als u meer cloud DMUs voor een hogere doorvoer nodig hebt, neem dan contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/). Instellen van 8 en hoger op dit moment werkt alleen als u **meerdere bestanden kopiëren van Blob-opslag/Data Lake Store/Amazon S3/cloud FTP-/ cloud SFTP naar Blob-opslag/Data Lake Store/Azure SQL-Database**.
>

### <a name="parallelcopies"></a>parallelCopies
U kunt de **parallelCopies** eigenschap om aan te geven van de parallelle uitvoering die u wilt dat de Kopieeractiviteit om te gebruiken. U kunt deze eigenschap beschouwen als het maximum aantal threads in een Kopieeractiviteit waarmee u kunt vanuit de bron lezen of schrijven naar de sinkgegevensopslag parallel.

Data Factory bepaalt voor elke kopie-activiteit die wordt uitgevoerd, het aantal parallelle exemplaren te gebruiken om gegevens te kopiëren van de bron-gegevens opslaan en op te slaan op de doelgegevens. Het aantal parallelle exemplaren die worden gebruikt, is afhankelijk van het type van de bron en sink die u gebruikt.

| Bron en sink | Standaard parallelle kopie aantal bepaald door de service |
| --- | --- |
| Gegevens kopiëren tussen winkels op basis van een bestand (Blob-opslag; Data Lake Store; Amazon S3; een on-premises bestandssysteem; een on-premises HDFS) |Tussen 1 en 32 liggen. Afhankelijk van de grootte van de bestanden en het nummer van cloud-eenheden voor gegevensverplaatsing (DMUs) gebruikt om te kopiëren van gegevens tussen twee cloudlocaties voor gegevensopslag, of de fysieke configuratie van het Gateway-apparaat gebruikt voor een hybride-kopie (om gegevens te kopiëren naar of van een on-premises gegevensarchief). |
| Gegevens kopiëren naar **een brongegevensarchief naar Azure Table storage** |4 |
| Alle andere gegevensbronnen en sinks sets |1 |

Normaal gesproken geeft het standaardgedrag u de beste doorvoer. Echter voor het beheren van de belasting van de machines waarop uw gegevens worden opgeslagen of als u wilt kopiëren-prestaties afstemmen, kunt u besluiten de standaardwaarde overschrijven en geef een waarde op voor de **parallelCopies** eigenschap. De waarde moet tussen 1 en 32 liggen (zowel inclusief). Tijdens de uitvoering voor de beste prestaties Copy-activiteit maakt gebruik van een waarde die kleiner is dan of gelijk zijn aan de waarde die u hebt ingesteld.

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
Die u moet weten:

* Als u gegevens tussen bestanden zijn gebaseerd, kopieert de **parallelCopies** bepalen de parallelle uitvoering op bestandsniveau. De logische groepen te verdelen binnen een enkel bestand onder er zou gebeuren automatisch en transparant en is ontworpen om de beste geschikt chunkgrootte voor een bepaalde bron-gegevenstype store gebruiken om gegevens in parallelle en rechthoekige naar parallelCopies te laden. Het werkelijke aantal parallelle exemplaren data movement service wordt gebruikt voor de kopieerbewerking tijdens runtime is niet meer dan het aantal bestanden dat u hebt. Als flattenhierarchy is **mergeFile**, Kopieeractiviteit niet profiteren van parallelle uitvoering op bestandsniveau.
* Wanneer u een waarde opgeven voor de **parallelCopies** eigenschap, houd rekening met de toename van de belasting op uw bron en sink-gegevensopslag en gateway als dit een hybride-kopie is. Dit gebeurt met name wanneer u hebt meerdere activiteiten of gelijktijdige uitvoeringen van de dezelfde activiteiten worden uitgevoerd voor hetzelfde gegevensarchief. Als u merkt dat het gegevensarchief of een Gateway met de belasting wordt overbelast, vermindert u de **parallelCopies** waarde om te ontlasten van de belasting.
* Wanneer u gegevens van winkels die niet op bestanden zijn gebaseerd op winkels die op basis van bestanden kopieert, data movement service negeert de **parallelCopies** eigenschap. Zelfs als parallelle uitvoering is opgegeven, wordt deze niet toegepast in dit geval.

> [!NOTE]
> U moet Data Management Gateway versie 1.11 of hoger gebruiken om te gebruiken de **parallelCopies** functie wanneer u een hybride-exemplaar.
>
>

Zie het voorbeeld gebruiksvoorbeelden beter gebruikmaken van deze twee eigenschappen en voor het verbeteren van uw doorvoercapaciteit voor gegevens. U hoeft te configureren **parallelCopies** om te profiteren van het standaardgedrag. Als u configureert en **parallelCopies** is te klein, meerdere cloud DMUs mogelijk niet volledig worden gebruikt.

### <a name="billing-impact"></a>Facturering-impact
Er **belangrijk** te onthouden dat de kosten worden berekend op basis van de totale tijd van de kopieerbewerking. Als een kopieertaak voorheen een uur duren voordat met een cloudeenheid en nu 15 minuten met vier cloudeenheden duurt, blijft de totale factuur bijna hetzelfde. Bijvoorbeeld, u vier cloudeenheden. De eerste cloudeenheid 10 minuten, de tweede waarde 10 minuten, de derde versie, 5 minuten, en de vierde versie, 5 minuten, allemaal in één Kopieeractiviteit uitvoeren. De tijd van totale kopieertaak (verplaatsing van gegevens), dat 10 + 10 + 5 + 5 = 30 minuten in rekening worden gebracht. Met behulp van **parallelCopies** heeft geen invloed op de facturering.

## <a name="staged-copy"></a>Gefaseerd kopiëren
Wanneer u gegevens van een brongegevensarchief naar een sink-gegevensarchief kopieert, kunt u Blob-opslag gebruiken als een tussentijdse faseringsopslag. Fasering is vooral nuttig in de volgende gevallen:

1. **U wilt opnemen van gegevens uit verschillende gegevensarchieven in SQL Data Warehouse met PolyBase**. SQL Data Warehouse gebruikmaakt van een grote hoeveelheid gegevens laden in SQL Data Warehouse PolyBase als een mechanisme voor hoge doorvoer. Echter, de brongegevens in Blob-opslag moet worden en moet deze voldoen aan aanvullende criteria. Bij het laden van gegevens vanuit een ander gegevensarchief dan Blob-opslag, kunt u gegevens kopiëren via tussentijdse staging Blob-opslag kunt activeren. In dat geval voert de Data Factory de benodigde gegevenstransformaties om ervoor te zorgen dat het voldoet aan de vereisten van PolyBase. Vervolgens deze gebruikmaakt van PolyBase om gegevens te laden in SQL Data Warehouse. Zie voor meer informatie, [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).
2. **Soms duurt het even uit te voeren van een hybride verplaatsing van gegevens (dat wil zeggen, om te kopiëren tussen een on-premises gegevens opslaan en een cloudgegevens kunnen worden opgeslagen) via een trage netwerkverbinding**. Voor betere prestaties kunt u de gegevens on-premises comprimeren zodat er minder tijd om gegevens te verplaatsen naar de tijdelijke gegevensopslag in de cloud nodig. U kunt vervolgens de gegevens in de staging-archief decomprimeren voordat u deze te in het beoogde gegevensarchief laden.
3. **U niet wilt openen van poorten dan poort 80 en poort 443 in uw firewall vanwege bedrijfsbeleid IT**. Wanneer u gegevens van een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse-sink kopiëren, moet u bijvoorbeeld uitgaande TCP-communicatie op poort 1433 voor zowel de Windows firewall en firewall van uw bedrijf te activeren. In dit scenario, profiteren van de gateway naar de eerste kopiëren van gegevens naar een Blob-opslag staging-exemplaar via HTTP of HTTPS op poort 443. Vervolgens kunt de gegevens in SQL-Database of SQL Data Warehouse laden van Blob-opslag staging. In deze stroom moet u geen poort 1433 in te schakelen.

### <a name="how-staged-copy-works"></a>Hoe gefaseerd kopiëren werkt
Wanneer u de functie voor gefaseerde installatie activeert, eerst de gegevens worden gekopieerd uit de brongegevensopslag naar de staging data store (Voeg uw eigen). Vervolgens is de gegevens van de faseringsgegevensopslag gekopieerd naar de sinkgegevensopslag. Data Factory wordt automatisch de gegevensstroom twee fasen voor u beheerd. Tijdelijke gegevens uit de opslag staging ruimt Data Factory ook nadat de verplaatsing van gegevens voltooid is.

Gateway wordt niet gebruikt in het scenario van de kopie cloud (bron en sink gegevens winkels in de cloud zijn). De Data Factory-service voert de kopieerbewerkingen.

![Gefaseerd kopiëren: Cloudscenario](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

In het scenario met hybride kopiëren (bron wordt on-premises en sink is in de cloud), de gateway gegevens verplaatst van de brongegevensopslag naar een tijdelijke gegevensopslag. Data Factory-service worden gegevens uit het gegevensarchief staging verplaatst naar het sink-gegevensopslag. Kopiëren van gegevens uit een cloudgegevensopslag met een on-premises gegevensarchief via fasering wordt ook ondersteund in de omgekeerde-werkstroom.

![Gefaseerd kopiëren: Hybride scenario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Wanneer u verplaatsing van gegevens met behulp van een faseringsopslag activeert, kunt u opgeven of u wilt dat de gegevens worden gecomprimeerd voordat het verplaatsen van gegevens uit de brongegevensopslag naar een tijdelijke of tijdelijke gegevensopslag en vervolgens voor het verplaatsen van gegevens vanuit een tussentijdse of faseert gegevens gedecomprimeerd opslaan naar de sinkgegevensopslag.

U kunt gegevens tussen twee on-premises gegevensarchieven met behulp van een faseringsopslag op dit moment niet kopiëren. We verwachten dat deze optie om na te zijn binnenkort beschikbaar.

### <a name="configuration"></a>Configuratie
Configureer de **enableStaging** instellen in de Kopieeractiviteit om op te geven of u wilt dat de gegevens naar de tijdelijk opgeslagen in Blob-opslag voordat u deze te in een doelgegevensarchief laden. Als u instelt **enableStaging** om op te geven waar, de aanvullende eigenschappen in de volgende tabel weergegeven. Als u niet hebt, moet u ook om te maken van een Azure Storage of opslag shared access signature-gekoppelde service voor fasering.

| Eigenschap | Description | Standaardwaarde | Vereist |
| --- | --- | --- | --- |
| **enableStaging** |Geef op of u wilt het kopiëren van gegevens via een tussentijdse opslag staging. |False |Nee |
| **linkedServiceName** |Geef de naam van een [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) gekoppelde service, die verwijst naar het exemplaar van de opslag die u als een tussentijdse faseringsopslag. <br/><br/> U kunt opslag met een shared access signature niet gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase. U kunt deze gebruiken in alle andere scenario's. |N/A |Ja, wanneer **enableStaging** is ingesteld op TRUE |
| **Pad** |Geef het pad op Blob-opslag die u wilt de voorbereide gegevens bevatten. Als u een pad opgeeft, wordt een container voor het opslaan van tijdelijke gegevens gemaakt in de service. <br/><br/> Geef een pad alleen als u Storage met een shared access signature gebruiken of u tijdelijke gegevens op een specifieke locatie te vereisen. |N/A |Nee |
| **enableCompression** |Hiermee geeft u op of gegevens moeten worden gecomprimeerd voordat deze wordt gekopieerd naar de bestemming. Deze instelling beperkt de hoeveelheid gegevens die worden overgebracht. |False |Nee |

Hier volgt een voorbeelddefinitie van Kopieeractiviteit met de eigenschappen die worden beschreven in de voorgaande tabel:

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

### <a name="billing-impact"></a>Facturering-impact
De kosten worden berekend op basis van twee stappen: duur van de kopieerbewerking en type kopiëren.

* Wanneer u tijdens een kopieerbewerking cloud (gegevens uit een cloudgegevensopslag kopiëren naar een andere cloudgegevensopslag) voor fasering in rekening gebracht de [som van de duur van de kopieerbewerking voor de stap 1 en 2] x [cloud kopie unit price].
* Wanneer u tijdens een hybride-kopie (gegevens kopiëren van een on-premises gegevensarchief naar een gegevensarchief cloud), voor fasering in rekening worden gebracht [duur van de kopieerbewerking hybride] x [hybride exemplaar unit price] + [cloud duur van de kopieerbewerking] x [cloud kopie unit price].

## <a name="performance-tuning-steps"></a>Prestaties afstemmen stappen
Het is raadzaam dat u deze stappen voor het afstemmen van de prestaties van uw Data Factory-service met de Kopieeractiviteit uitvoeren:

1. **Basislijn van**. Test uw pijplijn met behulp van de Kopieeractiviteit op basis van een representatieve steekproef tijdens de ontwikkelingsfase. Kunt u de Data Factory [voor het verwerkingsvenster](data-factory-scheduling-and-execution.md) om de hoeveelheid gegevens waarmee u met werkt te beperken.

   Uitvoeringstijd en prestatiekenmerken verzamelen met behulp van de **bewaking en Beheerapp**. Kies **bewaking en beheer** op de startpagina van uw Data Factory. Kies in de structuurweergave wordt weergegeven, de **uitvoergegevensset**. In de **activiteit Windows** Kies de Copy-activiteit uitvoeren. **Activiteit Windows** geeft een lijst van de duur van de Kopieeractiviteit en de grootte van de gegevens die worden gekopieerd. De doorvoer wordt vermeld in **Activiteitsvensterverkenner**. Zie voor meer informatie over de app, [bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de bewaking en Beheerapp](data-factory-monitor-manage-app.md).

   ![Details uitvoering van activiteit](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Later in dit artikel, kunt u de prestaties en configuratie van uw scenario aan Copy-activiteit vergelijken [prestaties verwijzing](#performance-reference) van onze tests.
2. **Diagnose en prestaties te optimaliseren**. Als de prestaties die u ziet dat niet voldoet aan uw verwachtingen, moet u prestatieknelpunten. Vervolgens prestaties als u wilt verwijderen of het effect van knelpunten te optimaliseren. Een volledige beschrijving van de prestatiediagnose van valt buiten het bereik van dit artikel, maar hier volgen enkele algemene overwegingen:

   * Prestatiefuncties:
     * [Parallelle kopiëren](#parallel-copy)
     * [Eenheden voor gegevensverplaatsing cloud](#cloud-data-movement-units)
     * [Gefaseerd kopiëren](#staged-copy)
     * [Schaalbaarheid van Data Management Gateway](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Gegevensbeheergateway](#considerations-for-data-management-gateway)
   * [Bron](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization)
   * [Compressie](#considerations-for-compression)
   * [Kolomtoewijzing](#considerations-for-column-mapping)
   * [Andere overwegingen](#other-considerations)
3. **De configuratie van de uitbreiden naar uw volledige gegevensset**. Wanneer u tevreden met de resultaten van het uitvoeren en de prestaties bent, kunt u de definitie en de actieve periode van de pijplijn voor uw volledige gegevensset kunt uitbreiden.

## <a name="considerations-for-data-management-gateway"></a>Overwegingen voor Data Management Gateway
**Gateway-installatie**: U wordt aangeraden dat u een toegewezen machine naar Data Management Gateway-host. Zie [overwegingen voor het gebruik van Data Management Gateway](data-factory-data-management-gateway.md#considerations-for-using-gateway).

**Controle van de gateway en scale-up/out**: Een enkele logische gateway met een of meer gatewayknooppunten kan meerdere Kopieeractiviteit wordt uitgevoerd op hetzelfde moment gelijktijdig fungeren. U kunt bijna realtime momentopname van Resourcegebruik (CPU, geheugen, network(in/out), enzovoort) weergeven op een gateway-apparaat en het aantal gelijktijdige taken uitgevoerd ten opzichte van de limiet in Azure portal, Zie [Monitor-gateway in de portal](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Als u zware nodig voor hybride verplaatsing van gegevens met een groot aantal uitvoeringen van activiteit gelijktijdige kopiëren of met een grote hoeveelheid gegevens hebt te kopiëren, kunt u overwegen om te [omhoog of uitschalen gateway](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) zodat deze beter gebruikmaken van de bron of om in te richten meer resources om te kopiëren.

## <a name="considerations-for-the-source"></a>Overwegingen voor de bron
### <a name="general"></a>Algemeen
Zorg ervoor dat het onderliggende gegevensarchief niet wordt overbelast door andere werkbelastingen die worden uitgevoerd op of tegen.

Zie voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensarchieven en geven u inzicht in gegevens van de prestatiekenmerken opslaan, responstijden te minimaliseren en doorvoer te maximaliseren.

Als u gegevens van Blob storage naar SQL Data Warehouse kopiëren, kunt u overwegen **PolyBase** om prestaties te verbeteren. Zie [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden
*(Inclusief Blob storage, Data Lake Store, Amazon S3, on-premises bestandssystemen en on-premises HDFS)*

* **Gemiddelde van de bestandsgrootte en het aantal bestanden**: Kopieeractiviteit brengt gegevens één bestand tegelijk. Met de dezelfde hoeveelheid gegevens worden verplaatst, wordt de algehele doorvoer lager als de gegevens van veel kleine bestanden in plaats van enkele grote bestanden vanwege de bootstrap-fase voor elk bestand bestaat. Indien mogelijk, dus kleine bestanden combineren in grotere bestanden om te krijgen hogere doorvoer.
* **Bestand-indeling en compressie**: Zie voor meer manieren om prestaties te verbeteren, de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties.
* Voor de **on-premises bestandssysteem** scenario, waarin **Data Management Gateway** is vereist, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway) sectie.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens
*(Met inbegrip van SQL-Database SQL datawarehouse; Amazon Redshift; SQL Server-databases. (en Oracle, MySQL, DB2, Teradata, Sybase en PostgreSQL-databases, enz.)*

* **Een gegevenspatroon**: Uw tabelschema is van invloed op de doorvoer van de kopie. Een grote rijgrootte biedt u een betere prestaties dan kleine rijgrootte, om te kopiëren van de dezelfde hoeveelheid gegevens. De reden is dat de database minder batches van gegevens die minder rijen bevatten efficiënter kunt ophalen.
* **Query of een opgeslagen procedure**: Optimaliseer de logica van de query of een opgeslagen procedure die u in de bron van Kopieeractiviteit opgeeft voor het ophalen van gegevens efficiënter.
* Voor **lokale relationele databases**, zoals SQL Server en Oracle, waarvoor het gebruik van **Data Management Gateway**, Zie de overwegingen voor Data Management Gateway-sectie.

## <a name="considerations-for-the-sink"></a>Overwegingen voor de sink
### <a name="general"></a>Algemeen
Zorg ervoor dat het onderliggende gegevensarchief niet wordt overbelast door andere werkbelastingen die worden uitgevoerd op of tegen.

Raadpleeg voor Microsoft-gegevensarchieven, [bewaking en afstemming van onderwerpen](#performance-reference) die specifiek zijn voor gegevensopslag. Deze onderwerpen vindt u informatie over data store prestatiekenmerken en hoe u responstijden te minimaliseren en doorvoer te maximaliseren.

Als u gegevens van kopieert **Blob storage** naar **SQL Data Warehouse**, kunt u overwegen **PolyBase** om prestaties te verbeteren. Zie [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) voor meer informatie. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Opgeslagen gegevens op basis van bestanden
*(Inclusief Blob storage, Data Lake Store, Amazon S3, on-premises bestandssystemen en on-premises HDFS)*

* **Kopieer gedrag**: Als u gegevens van een ander bestand op basis van gegevensarchief kopieert, Copy-activiteit heeft drie opties via de **copyBehavior** eigenschap. Deze hiërarchie behoudt, hiërarchie worden samengevoegd of bestanden worden samengevoegd. Hiërarchie relatiegegevens of behouden heeft weinig of geen performance overhead, maar samenvoegen van bestanden zorgt ervoor dat de prestatieoverhead te verhogen.
* **Bestand-indeling en compressie**: Zie de [aandachtspunten voor serialisatie en deserialisatie](#considerations-for-serialization-and-deserialization) en [overwegingen voor compressie](#considerations-for-compression) secties voor meer manieren om prestaties te verbeteren.
* **BLOB-opslag**: Blob storage ondersteunt momenteel alleen blok-blobs voor geoptimaliseerde gegevensoverdracht en doorvoer.
* Voor **on-premises-bestandssystemen** scenario's waarin het gebruik van **Data Management Gateway**, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway) sectie.

### <a name="relational-data-stores"></a>Op opgeslagen relationele gegevens
*(Inclusief SQL Database, SQL Data Warehouse, SQL Server-databases en Oracle-databases)*

* **Kopieer gedrag**: Afhankelijk van de eigenschappen die u hebt ingesteld voor **sqlSink**, Kopieeractiviteit schrijft gegevens naar de doeldatabase op verschillende manieren.
  * Standaard voegt de data movement service wordt gebruikt de bulksgewijs kopiëren-API om gegevens in te voegen modus, waarbij de beste prestaties biedt.
  * Als u een opgeslagen procedure in de sink is geconfigureerd, is de database de één rij met gegevens van toepassing op een tijdstip in plaats van als bulksgewijs laden. Prestaties aanzienlijk wordt geweigerd. Als uw gegevensset is groot, indien van toepassing is, overweeg over te schakelen voor het gebruik van de **sqlWriterCleanupScript** eigenschap.
  * Als u configureert de **sqlWriterCleanupScript** eigenschap voor elke activiteit kopiëren uitvoeren, het script door de service wordt geactiveerd en u de bulksgewijs kopiëren-API gebruiken voor het invoegen van de gegevens. Bijvoorbeeld, als u wilt overschrijven de hele tabel met de meest recente gegevens, kunt u een script voor het eerst alle records verwijderen voordat u de nieuwe gegevens uit de bron voor bulksgewijs laden.
* **Patroon en batch gegevensgrootte**:
  * Uw tabelschema is van invloed op de doorvoer van de kopie. Als u wilt kopiëren van de dezelfde hoeveelheid gegevens, biedt een grote rijgrootte betere prestaties dan een kleine rijgrootte omdat de database kan efficiënter minder batches van de gegevens worden doorgevoerd.
  * Kopieeractiviteit voegt de gegevens in een reeks van batches. U kunt het aantal rijen in een batch instellen met behulp van de **writeBatchSize** eigenschap. Als uw gegevens klein rijen is, kunt u instellen de **writeBatchSize** eigenschap met een hogere waarde om te profiteren van lagere batch overhead en hogere doorvoer. Als de rijgrootte van uw gegevens te groot is, Wees voorzichtig wanneer u verhogen **writeBatchSize**. Een hoge waarde kan leiden tot een mislukte kopieerbewerking veroorzaakt door overbelasting van de database.
* Voor **lokale relationele databases** zoals SQL Server en Oracle, waarvoor het gebruik van **Data Management Gateway**, Zie de [overwegingen voor Data Management Gateway](#considerations-for-data-management-gateway)sectie.

### <a name="nosql-stores"></a>NoSQL-archieven
*(Inclusief Table storage en Azure Cosmos DB)*

* Voor **Table storage**:
  * **Partitie**: Gegevens schrijven naar een interleaved partities aanzienlijk vermindert de prestaties. De brongegevens op partitiesleutel sorteren, zodat de gegevens efficiënt in één partitie is ingevoegd achter elkaar, of aanpassen van de logica voor het schrijven van de gegevens naar één partitie.
* Voor **Azure Cosmos DB**:
  * **Batchgrootte**: De **writeBatchSize** eigenschap stelt u het aantal parallelle aanvragen naar de Azure Cosmos DB-service om documenten te maken. U kunt betere prestaties verwachten wanneer u verhogen **writeBatchSize** omdat meer parallelle aanvragen worden verzonden naar Azure Cosmos DB. Echter, Kijk voor de beperking bij het schrijven naar Azure Cosmos DB (het foutbericht is 'aanvraagsnelheid is hoog'). Verschillende factoren kunnen leiden tot beperking, met inbegrip van de documentgrootte, het aantal voorwaarden in de documenten en de doelverzameling indexeringsbeleid. Als u wilt bereiken hogere doorvoer voor kopiëren, kunt u overwegen een betere verzameling, bijvoorbeeld S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Overwegingen voor serialisatie en deserialisatie
Serialisatie en deserialisatie kunnen optreden wanneer uw gegevensset voor invoer of uitvoer gegevensset een bestand is. Zie [ondersteunde indelingen voor bestanden en compressie](data-factory-supported-file-and-compression-formats.md) met informatie over ondersteunde bestandsindelingen door de Kopieeractiviteit.

**Kopieer gedrag**:

* Kopiëren van bestanden tussen gegevensarchieven in op basis van bestanden:
  * Een binaire kopie zonder serialisatie of deserialisatie als invoer- en data-sets beide hebben hetzelfde of er zijn geen bestandsindelingsinstellingen, data movement service worden uitgevoerd. Ziet u een hogere doorvoer in vergelijking met het scenario, waarin de bron en sink-bestandsindelingsinstellingen zich van elkaar verschillen.
  * Als invoer en uitvoer gegevenssets beide zijn in de tekstindeling en alleen de codering type is anders, data movement service biedt alleen codering conversie. Doet het nog serialisatie en deserialisatie, waardoor overhead in vergelijking met een binaire kopie prestaties.
  * Brongegevens om te streamen, transformeren en deze vervolgens te serialiseren in de indeling van de uitvoer die u hebt aangegeven als invoer- en data-sets beide hebben verschillende bestandsindelingen of verschillende configuraties, zoals scheidingstekens, data movement service worden gedeserialiseerd. Deze bewerking resulteert in een veel belangrijker performance overhead in vergelijking met andere scenario's.
* Wanneer u kopieert bestanden naar/van een gegevensarchief dat niet op basis van bestanden (bijvoorbeeld in een store, op basis van bestanden naar een relationele archief), wordt de stap serialisatie of deserialisatie is vereist. Deze stap resulteert in van aanzienlijke prestatieoverhead.

**Bestandsindeling**: De bestandsindeling die u kiest kan kopiëren prestaties beïnvloeden. Avro is bijvoorbeeld een compacte binaire indeling die metagegevens op met de gegevens slaat. Het biedt een brede ondersteuning in het Hadoop-ecosysteem voor het verwerken en uitvoeren van query's. Avro is echter duurder voor serialisatie en deserialisatie, wat in het onderste exemplaar doorvoer in vergelijking met de tekstindeling resulteert. Uw keuze van de bestandsindeling in de stroom voor de verwerking van zuinigste maken. Beginnen met welke vorm de gegevens worden opgeslagen in brongegevens of moet worden geëxtraheerd uit externe systemen; de beste indeling voor opslag, analytische verwerking en uitvoeren van query's; en in welke indeling moet de gegevens worden geëxporteerd naar de datamarts voor hulpprogramma's voor rapportage en visualisatie. Soms een bestandsindeling die is suboptimale voor lezen en schrijven prestaties mogelijk zijn een goede keuze wanneer u rekening houden met het algemene analytische proces.

## <a name="considerations-for-compression"></a>Overwegingen voor compressie
Als uw gegevensset voor invoer- of een bestand is, kunt u Copy Activity om uit te voeren van compressie of decompressie als gegevens naar de bestemming schrijft instellen. Wanneer u compressie kiest, maakt u een balans tussen input/output (I/O) en de CPU. Comprimeren van de kosten van de gegevens een extra in compute-resources. Maar hierna het i/o-netwerk en opslag verlaagt. Afhankelijk van uw gegevens ziet u een boost in de algemene doorvoer van de kopie.

**Codec**: Copy Activity biedt ondersteuning voor gzip-, bzip2- en Deflate-compressietypen. Azure HDInsight kan gebruikmaken van alle drie typen voor verwerking. Elke compressiecodec heeft voordelen. Bijvoorbeeld, bzip2 heeft de laagste kopie doorvoer, maar u krijgt de beste prestaties van Hive-query met bzip2 omdat u deze voor verwerking opsplitsen kunt. Gzip is de meest met gelijke taakverdeling optie en deze het vaakst wordt gebruikt. Kies de codec die het beste bij uw end-to-end-scenario.

**Niveau**: U kunt kiezen uit twee opties voor elke compressiecodec: snelste gecomprimeerde en optimaal gecomprimeerd. De snelste gecomprimeerde optie worden de gegevens zo snel mogelijk, gecomprimeerd, zelfs als het resulterende bestand is niet optimaal gecomprimeerd. De optimaal gecomprimeerde optie meer tijd in de taakwachtrij doorbrengt op compressie en resulteert in een minimale hoeveelheid gegevens. U kunt beide opties om te zien waarmee u betere algehele prestaties in uw geval testen.

**Overweging**: Als u wilt kopiëren van een grote hoeveelheid gegevens tussen een on-premises gegevensopslag en de cloud, kunt u overwegen tussentijdse blob-opslag met compressie. Tijdelijke opslag is nuttig wanneer de bandbreedte van uw bedrijfsnetwerk en uw Azure-services de beperkende factor is en u wilt dat de gegevensset voor invoer en uitvoer gegevensset beide in niet-gecomprimeerde vorm. Meer specifiek, kunt u een één kopieeractiviteit opdelen in twee kopieeractiviteiten. De eerste kopieeractiviteit opgehaald uit de bron naar een tijdelijke of staging-blob in gecomprimeerde vorm. De tweede kopieeractiviteit kopieert de gecomprimeerde gegevens van fasering en vervolgens decomprimeert terwijl er wordt geschreven naar de sink.

## <a name="considerations-for-column-mapping"></a>Overwegingen voor kolomtoewijzing
U kunt instellen dat de **columnMappings** eigenschap in de Kopieeractiviteit alle kaart of een subset van de invoerkolommen voor de uitvoerkolommen. Nadat de data movement service leest de gegevens van de bron, moet deze kolomtoewijzing uitvoeren voor de gegevens voordat deze de gegevens naar de sink schrijft. Deze extra verwerking vermindert kopie doorvoer.

Als uw bron-gegevensopslag waarin u kunt zoeken, bijvoorbeeld als het is een relationele opslagplaats, zoals SQL-Database of SQL Server, of als het is een NoSQL-archief, zoals Table storage of Azure Cosmos DB, kunt u de kolom filteren pushen en logica voor opnieuw ordenen de **query** eigenschap in plaats van in de kolomtoewijzing. Op deze manier de projectie treedt op wanneer data movement service gegevens uit de brongegevensopslag leest, waar het is veel efficiënter.

## <a name="other-considerations"></a>Andere overwegingen
Als de grootte van gegevens die u wilt kopiëren groot is, kunt u uw bedrijfslogica naar verdere partitie de gegevens in Data Factory met behulp van het mechanisme voor segmenteringshulplijnen aanpassen. Plan de Copy-activiteit naar het vaker uitvoeren voor het verkleinen van de gegevens voor elke activiteit kopiëren die wordt uitgevoerd.

Wees voorzichtig bij het aantal gegevenssets en een Data Factory op connector naar hetzelfde gegevensarchief die op hetzelfde moment kopieeractiviteiten. Veel gelijktijdige kopie taken mogelijk beperken van een gegevensarchief en leiden tot verminderde prestaties, kopie taak interne nieuwe pogingen, en in sommige gevallen, fouten bij de uitvoering.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Voorbeeldscenario: Kopiëren van een on-premises SQL Server naar Blob-opslag
**Scenario**: Een pijplijn is gebouwd om gegevens te kopiëren uit een on-premises SQL Server naar Blob-opslag in CSV-indeling. Als u sneller de taak voor het kopiëren, moeten de CSV-bestanden worden gecomprimeerd naar bzip2-indeling.

**Test- en analyse**: De doorvoer van de Kopieeractiviteit is minder dan 2 MBps, dat is veel lager is dan de benchmark prestaties.

**Analyse van prestaties en afstemmen**: Voor het oplossen van het prestatieprobleem, laten we kijken hoe de gegevens worden verwerkt en verplaatst.

1. **Gegevens lezen**: Gateway een verbinding met SQL Server wordt geopend en stuurt de query. SQL-Server reageert door de gegevensstroom te sturen naar de Gateway via het intranet.
2. **Serialiseren en gegevens comprimeren**: Gateway serialiseert de gegevensstroom naar de CSV-indeling en de gegevens naar een stream bzip2 worden gecomprimeerd.
3. **Gegevens schrijven**: Gateway wordt de stroom bzip2 geüpload naar Blob storage via Internet.

Zoals u zien kunt, de gegevens worden verwerkt en verplaatst op sequentiële wijze streaming: SQL Server > LAN > Gateway > WAN > Blob storage. **De algehele prestaties worden geregeld door de minimale doorvoer via de pijplijn**.

![Gegevensstroom](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Een of meer van de volgende factoren kan leiden tot de bottleneck in de prestaties:

* **Bron**: SQL-Server zelf heeft lage doorvoer vanwege de zware belastingen.
* **Data Management Gateway**:
  * **LAN**: Gateway bevindt zich ver van de SQL Server-machine en een verbinding met een lage bandbreedte heeft.
  * **Gateway**: Gateway heeft bereikt van de load-beperkingen voor de volgende bewerkingen uitvoeren:
    * **Serialisatie**: Tijdens het serialiseren van de gegevensstroom naar de CSV-indeling is traag doorvoer.
    * **Compressie**: U hebt ervoor gekozen een trage compressiecodec (bijvoorbeeld, bzip2, namelijk 2,8 MBps met Core i7).
  * **WAN**: Is een lage bandbreedte tussen het bedrijfsnetwerk en uw Azure-services (bijvoorbeeld T1 = 1,544 kbps. Tijdstip T2 = 6,312 kbps).
* **Sink**: BLOB-opslag heeft lage doorvoer. (In dit scenario is niet waarschijnlijk omdat de SLA wordt gegarandeerd minimaal 60 MBps.)

In dit geval kan bzip2 gegevenscompressie worden vertraagd de hele pijplijn. Overschakelen naar een compressiecodec gzip, kan dit knelpunt vereenvoudigen.

## <a name="sample-scenarios-use-parallel-copy"></a>Voorbeeldscenario's: Parallelle kopie gebruiken
**Scenario I:** 1000 1 MB-bestanden kopiëren van de on-premises bestandssysteem naar de Blob-opslag.

**Analyse en het afstemmen van prestaties**: Voor een voorbeeld: als u gateway hebt geïnstalleerd op een computer quad-core, Data Factory maakt gebruik van 16 parallelle exemplaren om bestanden te verplaatsen van het bestandssysteem naar Blob-opslag gelijktijdig. Deze parallelle uitvoering moet resulteren in hoge doorvoer. U kunt ook expliciet opgeven het aantal parallelle exemplaren. Wanneer u veel kleine bestanden kopieert, helpen parallelle exemplaren doorvoer aanzienlijk efficiënter gebruik van resources.

![Scenario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scenario II**: 20 blobs van 500 MB van Blob-opslag kopiëren met Data Lake Store Analytics en vervolgens het afstemmen van prestaties.

**Analyse en het afstemmen van prestaties**: In dit scenario, Data Factory kopieert de gegevens uit Blob storage naar Data Lake Store met behulp van één exemplaar (**parallelCopies** ingesteld op 1) en één cloud eenheden voor gegevensverplaatsing. De doorvoer die u ziet dat zal worden dicht bij die wordt beschreven in de [prestaties naslagsectie](#performance-reference).

![Scenario 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scenario III**: Afzonderlijk bestand is groter dan tientallen MB/s en totale volume is groot.

**Analyse en prestaties van het draaien**: Toenemende **parallelCopies** niet resulteren in betere kopieerprestaties vanwege de resourcebeperkingen van een DMU single-cloud. In plaats daarvan moet u meer cloud DMUs om op te halen van andere bronnen om uit te voeren van de verplaatsing van gegevens. Geef een waarde voor de **parallelCopies** eigenschap. Data Factory wordt de parallelle uitvoering voor u. In dit geval, als u instelt **cloudDataMovementUnits** tot en met 4, een doorvoer van informatie over vier keer optreedt.

![Scenario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referentie
Hier zijn prestaties controleren en afstemmen van verwijzingen voor enkele van de ondersteunde gegevensarchieven:

* Azure Storage (inclusief Blob storage en Table storage): [Azure Storage-schaalbaarheidsdoelen](../../storage/common/storage-scalability-targets.md) en [controlelijst voor de prestaties en schaalbaarheid van Azure Storage](../../storage/common/storage-performance-checklist.md)
* Azure SQL Database: U kunt [bewaken van de prestaties](../../sql-database/sql-database-single-database-monitor.md) en controleert u het percentage van database transaction unit (DTU)
* Azure SQL Data Warehouse: De mogelijkheid wordt gemeten in datawarehouse units (dwu's); Zie [beheren rekenkracht in Azure SQL Data Warehouse (overzicht)](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Prestatieniveaus in Azure Cosmos DB](../../cosmos-db/performance-levels.md)
* On-premises SQL Server: [Controleren en afstemmen van prestaties](https://msdn.microsoft.com/library/ms189081.aspx)
* On-premises bestandsserver: [Prestaties afstemmen voor bestandsservers](https://msdn.microsoft.com/library/dn567661.aspx)
