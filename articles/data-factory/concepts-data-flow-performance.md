---
title: Toewijzing van de gegevensstroom prestaties en afstemmen begeleiden u bij het Azure Data Factory | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van gegevensstromen in Azure Data Factory wanneer u gegevensstromen toewijzen.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 46be01c57be0e4f5fa74f8e8b0d91db3d78f441c
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480412"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Toewijzing van gegevensstromen prestaties en afstemmen van de handleiding

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory toewijzing gegevens stromen bieden een browserinterface zonder code voor het ontwerpen, implementeren en beheren gegevenstransformaties op schaal.

> [!NOTE]
> Als u niet bekend met ADF toewijzing gegevens stromen in het algemeen bent, Zie [gegevens stromen overzicht](concepts-data-flow-overview.md) voordat het lezen van dit artikel.
>

> [!NOTE]
> Wanneer u ontwerpen en testen van Data-stromen van de ADF UI, zorg er dan voor dat inschakelen op de switch foutopsporing zodat u kunt uw gegevensstromen in uitvoeren realtime zonder te wachten op een cluster opgewarmd.
>

![Fouten opsporen in knop](media/data-flow/debugb1.png "foutopsporing")

## <a name="monitor-data-flow-performance"></a>De prestaties van de stroom gegevens controleren

Tijdens het ontwerpen van uw kaartgegevens in de browser stromen, kunt u op test jednotky elke afzonderlijke transformatie door te klikken op het tabblad voor het voorbeeld van gegevens in het deelvenster onder instellingen voor elke transformatie. De volgende stap die moet u rekening houden is het testen van uw gegevensstroom end-to-end in de pijplijnontwerper. Toevoegen van een activiteit uitvoeren van de gegevensstroom en gebruik van de knop foutopsporing voor het testen van de prestaties van de gegevensstroom. In het onderste deelvenster van de pijplijn-venster ziet u een pictogram eyeglass onder 'acties':

![Data Flow Monitor](media/data-flow/mon002.png "Data Flow Monitor 2")

Als u dit pictogram te klikken op weergegeven de uitvoeringsplan en latere prestatieprofiel van de gegevensstroom. U kunt deze informatie gebruiken om te schatten van de prestaties van de gegevensstroom op basis van verschillende grootte gegevensbronnen. Houd er rekening mee dat u ervan uitgaan 1 minuut van de cluster taak uitvoeringstijd instellen in de algehele prestaties berekeningen dat kunt en als u van de standaard Azure Integration Runtime gebruikmaakt, moet u mogelijk om toe te voegen van 5 minuten hiervan ook cluster kringveld-up-tijd.

![Data Flow Monitoring](media/data-flow/mon003.png "Data Flow Monitor 3")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Optimaliseren voor Azure SQL Database en Azure SQL datawarehouse

![Deel de gegevensbron](media/data-flow/sourcepart2.png "deel uit van bron")

### <a name="partition-your-source-data"></a>Partities voor uw brongegevens

* Ga naar 'Optimaliseren' en 'Bron' selecteren. Ingesteld op een specifieke kolom of een type in een query.
* Als u 'kolom' kiest, kies dan de partitiekolom.
* Ook het maximum aantal verbindingen instellen met uw Azure SQL DB. U kunt een hogere instelling om te krijgen van parallelle verbindingen met uw database. Sommige gevallen kunnen echter leiden tot snellere prestaties met een beperkt aantal verbindingen.
* Uw database brontabellen hoeft te worden gepartitioneerd.
* Instellen van een query in uw bron-transformatie die overeenkomt met het partitieschema van de databasetabel kunt de database-engine van de gegevensbron gebruikmaken van partitie opheffing.
* Als de bron niet al is gepartitioneerd, worden gegevens partitioneren in de Spark gegevenstransformatie-omgeving op basis van de sleutel die u in de bron-transformatie selecteert nog steeds gebruiken door ADF.

### <a name="set-batch-size-and-query-on-source"></a>Batchgrootte en query's uitvoeren op de bron instellen

![Bron](media/data-flow/source4.png "bron")

* Instellen van de batchgrootte van de vertelt u ADF voor het opslaan van gegevens in sets in het geheugen in plaats van per rij. Er is een optionele instelling en u buiten resources kan uitvoeren op de rekenknooppunten als ze niet de juiste grootte hebben zijn.
* Instellen van een query, kunt u rechts op de bronlocatie rijen filteren voordat ze zelfs binnenkomen voor de gegevensstroom voor verwerking, waardoor de aanschaf initiële gegevens sneller.
* Als u een query gebruikt, kunt u optioneel queryhints toevoegen voor uw Azure SQL DB, dat wil zeggen READ UNCOMMITTED

### <a name="set-sink-batch-size"></a>Set-sink-batchgrootte

![Sink](media/data-flow/sink4.png "Sink")

* Instellen om te voorkomen dat per rij verwerking van de gegevensstromen van uw, de 'batchgrootte' in de sink-instellingen voor Azure SQL DB. Dit vertelt dat ADF met proces-database worden geschreven in batches op basis van de opgegeven grootte.

### <a name="set-partitioning-options-on-your-sink"></a>Set opties voor de sink partitioneren

* Zelfs als u uw gegevens op uw Azure SQL DB doeltabellen gepartitioneerd hebt, gaat u naar het tabblad optimaliseren en stel partitioneren.
* Heel vaak vertellen gewoon ADF Round-Robin resulteert in veel sneller gegevens laden in plaats van dat alle verbindingen met één knooppunt/partitie-partities op de uitvoering van Spark-clusters gebruiken.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Verhoog de grootte van uw Reken-engine in de Azure Integration Runtime

![Nieuwe IR](media/data-flow/ir-new.png "nieuwe IR")

* Het aantal kernen, waardoor de verhogen van het aantal knooppunten, en u voorzien van meer verwerkingskracht om te zoeken en schrijven naar uw Azure SQL DB verhogen.
* Probeer 'Compute geoptimaliseerd' en 'Geoptimaliseerd voor geheugen' opties om toe te passen van andere bronnen in uw rekenknooppunten.

### <a name="unit-test-and-performance-test-with-debug"></a>Test jednotky en prestaties testen met foutopsporing

* Eenheid het testen van de gegevensstromen, wordt wanneer de knop "Gegevens Flow Debug" ingesteld op aan.
* In de ontwerpfunctie gegevensstroom gebruik u het tabblad Voorbeeld van gegevens van transformaties om de resultaten van uw gegevenstransformatielogica weer te geven.
* Uw gegevens stromen van de pijplijnontwerper door een activiteit gegevensstroom op het ontwerp van de pijplijn eenheidstest canvas en de knop "Debug" gebruiken om te testen.
* Testen in de foutopsporingsmodus werkt op basis van een live verwarmde clusteromgeving zonder te wachten op een just-in-time-cluster kringveld-up.

### <a name="disable-indexes-on-write"></a>Indexen op schrijven uitschakelen
* Gebruik een ADF pijplijn opgeslagen procedureactiviteit voorafgaand aan de gegevensstroom activiteit waarmee indexen voor de doeltabellen die uit uw Sink worden geschreven om te worden uitgeschakeld.
* Nadat uw activiteit gegevensstroom toevoegen een andere opgeslagen procedure-activiteit die deze indexen ingeschakeld.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Verhoog de grootte van uw Azure SQL DB
* Plan een vergroten of verkleinen van de bron en sink-Azure SQL-database voordat u uw uitvoering de pijplijn voor het verhogen van de doorvoer en Azure beperking zodra u DTU bereiken minimaliseren beperkt.
* Nadat de pijplijnuitvoering voltooid is, kunt u het formaat van uw databases terug naar het normale tarief voor uitvoeren.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Optimaliseren voor Azure SQL datawarehouse

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Fasering gebruiken om gegevens in één bulkbewerking met Polybase te laden

* Om te voorkomen dat per rij verwerking van de gegevensstromen van uw, de optie "Staging" in de Sink-instellingen zo instellen dat ADF kan gebruikmaken van Polybase om te voorkomen dat per rij invoegen in DW. Dit vertelt u ADF het gebruik van Polybase, zodat gegevens kunnen worden geladen in één bulkbewerking.
* Wanneer u uw gegevens stroomactiviteit controleren van een pijplijn, met fasering ingeschakeld, moet u om de locatie van de Blob-archief van uw tijdelijke gegevens voor bulksgewijs laden te selecteren.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Verhoog de grootte van uw Azure SQL DW

* Plan een vergroten of verkleinen van de bron en sink-Azure SQL DW voordat u de pijplijn voor het verhogen van de doorvoer en minimaliseren Azure beperking zodra u DWU limieten bereiken uitvoeren.

* Nadat de pijplijnuitvoering voltooid is, kunt u het formaat van uw databases terug naar het normale tarief voor uitvoeren.

## <a name="optimize-for-files"></a>Optimaliseren voor bestanden

* Het aantal partities dat door ADF wordt gebruikt, kunt u bepalen. Op elke bron en Sink-transformatie, evenals de transformatie van elke afzonderlijke, kunt u een partitieschema instellen. Voor kleinere bestanden merkt u misschien soms selecteren "Één partitie" kunt werken beter en sneller dan de Spark voor het partitioneren van uw kleine bestanden waarin wordt gevraagd.
* Als u niet voldoende informatie gegeven over de brongegevens hebt, kunt u "Round Robin" partitionering en stel het aantal partities.
* Als u uw gegevens verkennen en u merkt dat u kolommen die goed hash-sleutels kunnen zijn, gebruikt u de optie partitioneren Hash.

### <a name="file-naming-options"></a>Opties voor de naamgeving van bestanden

* De standaard-aard van de getransformeerde gegevens worden geschreven in ADF toewijzing gegevens stromen is het schrijven naar een gegevensset die een Blob of een ADLS-gekoppelde Service. Deze gegevensset om te verwijzen naar een map of container, geen bestand met de naam in te stellen.
* Gegevensstromen gebruikt Azure Databricks Spark wordt uitgevoerd, wat betekent dat de uitvoer zal worden verdeeld over meerdere bestanden op basis van een standaard Spark partitioneren of het schema dat u partitioneren expliciet hebt gekozen.
* Een veelvoorkomende bewerking in ADF gegevens stromen is om te kiezen 'Uitvoer naar één bestand' zodat alle van de uitvoerbestanden deel samen worden samengevoegd in een enkel uitvoerbestand.
* Met deze bewerking is echter vereist dat de uitvoer tot één partitie op één clusterknooppunt beperkt.
* Houd hier rekening mee bij het kiezen van deze populaire optie. U kunt uitvoeren uit cluster knooppunt middelen te halen als u bij het combineren van veel grote bronbestanden in een enkele uitvoer bestandspartitie.
* Om te voorkomen dat knooppunt rekenresources voor drempelwaardemeldingen, kunt u de standaard- of expliciete partitieschema houden in ADF, die is geoptimaliseerd voor prestaties, en voegt u een volgende activiteit kopiëren in de pijplijn die wordt samengevoegd alles van het onderdeel één nieuwe bestanden van de map voor uitvoer het bestand. Deze techniek wordt in wezen worden gescheiden van de actie van de transformatie van het bestand samenvoegen en geven hetzelfde resultaat als de instelling 'uitvoer naar één bestand'.

## <a name="next-steps"></a>Volgende stappen
Zie de andere gegevensstroom artikelen:

- [Overzicht van stroom](concepts-data-flow-overview.md)
- [Gegevens Stroomactiviteit controleren](control-flow-execute-data-flow-activity.md)
- [Gegevensoverdracht-prestaties bewaken](concepts-data-flow-monitoring.md)
