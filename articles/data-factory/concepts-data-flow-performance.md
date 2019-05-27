---
title: Toewijzing van de gegevensstroom prestaties en afstemmen begeleiden u bij het Azure Data Factory | Microsoft Docs
description: Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van gegevensstromen in Azure Data Factory wanneer u gegevensstromen toewijzen.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 7fca586083f70e0b0f7e593d5203392260cd2136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172338"
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

## <a name="optimizing-for-azure-sql-database"></a>Optimaliseren voor Azure SQL Database

![Deel de gegevensbron](media/data-flow/sourcepart2.png "deel uit van bron")

### <a name="you-can-match-spark-data-partitioning-to-your-source-database-partitioning-based-on-a-database-table-column-key-in-the-source-transformation"></a>U kunt Spark partitioneren van gegevens naar uw bron database partitioneren op basis van een database tabelsleutel kolom in de bron-transformatie vergelijken

* Ga naar 'Optimaliseren' en 'Bron' selecteren. Ingesteld op een specifieke kolom of een type in een query.
* Als u 'kolom' kiest, kies dan de partitiekolom.
* Ook het maximum aantal verbindingen instellen met uw Azure SQL DB. U kunt een hogere instelling om te krijgen van parallelle verbindingen met uw database. Sommige gevallen kunnen echter leiden tot snellere prestaties met een beperkt aantal verbindingen.

### <a name="set-batch-size-and-query-on-source"></a>Batchgrootte en query's uitvoeren op de bron instellen

![Bron](media/data-flow/source4.png "bron")

* Instellen van de batchgrootte van de vertelt u ADF voor het opslaan van gegevens in sets in het geheugen in plaats van per rij. Er is een optionele instelling en u buiten resources kan uitvoeren op de rekenknooppunten als ze niet de juiste grootte hebben zijn.
* Instellen van een query, kunt u rechts op de bronlocatie rijen filteren voordat ze zelfs binnenkomen voor de gegevensstroom voor verwerking, waardoor de aanschaf initiële gegevens sneller.
* Als u een query gebruikt, kunt u optioneel queryhints toevoegen voor uw Azure SQL DB, dat wil zeggen READ UNCOMMITTED

### <a name="set-sink-batch-size"></a>Set-sink-batchgrootte

![Sink](media/data-flow/sink4.png "Sink")

* Instellen om te voorkomen dat per rij verwerking van uw gegevens floes, de 'batchgrootte' in de sinkinstellingen voor Azure SQL-database. Dit vertelt dat ADF met proces-database worden geschreven in batches op basis van de opgegeven grootte.

### <a name="set-partitioning-options-on-your-sink"></a>Set opties voor de sink partitioneren

* Zelfs als u uw gegevens op uw Azure SQL DB doeltabellen gepartitioneerd hebt, gaat u naar het tabblad optimaliseren en stel partitioneren.
* Heel vaak vertellen gewoon ADF Round-Robin resulteert in veel sneller gegevens laden in plaats van dat alle verbindingen met één knooppunt/partitie-partities op de uitvoering van Spark-clusters gebruiken.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Verhoog de grootte van uw Reken-engine in de Azure Integration Runtime

![Nieuwe IR](media/data-flow/ir-new.png "nieuwe IR")

* Het aantal kernen, waardoor de verhogen van het aantal knooppunten, en u voorzien van meer verwerkingskracht om te zoeken en schrijven naar uw Azure SQL DB verhogen.
* Probeer 'Compute geoptimaliseerd' en 'Geoptimaliseerd voor geheugen' opties om toe te passen van andere bronnen in uw rekenknooppunten.

### <a name="disable-indexes-on-write"></a>Indexen op schrijven uitschakelen
* Gebruik een ADF pijplijn opgeslagen procedureactiviteit voorafgaand aan de gegevensstroom activiteit waarmee indexen voor de doeltabellen die uit uw Sink worden geschreven om te worden uitgeschakeld.
* Nadat uw activiteit gegevensstroom toevoegen een andere opgeslagen procedure-activiteit die deze indexen ingeschakeld.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Verhoog de grootte van uw Azure SQL DB
* Plan een vergroten of verkleinen van de bron en sink-Azure SQL-database voordat u uw uitvoering de pijplijn voor het verhogen van de doorvoer en Azure beperking zodra u DTU bereiken minimaliseren beperkt.
* Nadat de pijplijnuitvoering voltooid is, kunt u het formaat van uw databases terug naar het normale tarief voor uitvoeren.

## <a name="next-steps"></a>Volgende stappen
Zie de andere gegevensstroom artikelen:

- [Overzicht van stroom](concepts-data-flow-overview.md)
- [Gegevens Stroomactiviteit controleren](control-flow-execute-data-flow-activity.md)

