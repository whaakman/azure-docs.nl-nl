---
title: Opties van de context voor het ML-Services op HDInsight - Azure COMPUTE | Microsoft Docs
description: Meer informatie over de verschillende rekenscenario context opties beschikbaar voor gebruikers met ML-Services op HDInsight
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 57480cef48182a56b315d7d6932883c485f5a7c8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050105"
---
# <a name="compute-context-options-for-ml-services-on-hdinsight"></a>Opties van de context voor het ML-Services op HDInsight berekenen

ML-Services in Azure HDInsight bepaalt hoe aanroepen worden uitgevoerd door de instelling van de compute-context. In dit artikel bevat een overzicht van de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerde over kernen van de edge-knooppunt of de HDInsight-cluster.

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts. Met een edge-knooppunt hebt u de optie van de parallelized gedistribueerde functies van RevoScaleR over de kernen van de edge-knooppunt server wordt uitgevoerd. U kunt ook deze uitvoeren op de knooppunten van het cluster met behulp van RevoScaleR Hadoop kaart verminderen of Spark compute-contexten.

## <a name="ml-services-on-azure-hdinsight"></a>ML-Services in Azure HDInsight
[ML-Services in Azure HDInsight](r-server-overview.md) biedt de nieuwste mogelijkheden voor analyses op basis van R. Het kan gebruiken gegevens die zijn opgeslagen in een HDFS-container in uw [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob storage") storage-account, een Data Lake store of het lokale bestandssysteem van Linux. Aangezien ML-Services is gebouwd op open-source R, wordt in de R-toepassingen die u bouwt de 8000 + open-source R-pakketten kunnen toepassen. Ze kunnen ook de routines in gebruiken [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), van Microsoft big analytics gegevenspakket die is opgenomen in het ML-Services.  

## <a name="compute-contexts-for-an-edge-node"></a>COMPUTE contexten voor een edge-knooppunt
In het algemeen een R-script wordt uitgevoerd in de cluster ML-Services op de edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen zijn de stappen die een RevoScaleR functie aanroept. Het aanroepen van de RevoScaleR uitgevoerd in een compute-omgeving die wordt bepaald door de manier waarop u de context van de compute RevoScaleR instelt.  Wanneer u uw R-script vanaf een edge-knooppunt uitvoeren, wordt de mogelijke waarden van de compute-context zijn:

- lokale opeenvolgende (*lokale*)
- lokale parallel (*localpar*)
- Kaart verminderen
- Spark

De *lokale* en *localpar* opties verschillen alleen in het **rxExec** aanroepen uitgevoerd. Ze beide uitgevoerd andere rx-functieaanroepen op een parallelle manier over alle beschikbare kernen tenzij anders is aangegeven door het gebruik van de RevoScaleR **numCoresToUse** optie, bijvoorbeeld `rxOptions(numCoresToUse=6)`. Parallelle uitvoeringsopties bieden optimale prestaties.

De volgende tabel geeft een overzicht van de verschillende compute context opties in te stellen hoe aanroepen uitgevoerd:

| Compute-context  | Het instellen                      | Context voor uitvoering                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Lokale sequentiële | rxSetComputeContext('local')    | Uitvoering geparallelliseerde over de kernen van de edge-knooppunt server, met uitzondering van rxExec-aanroepen, opeenvolgend worden uitgevoerd |
| Lokale parallel   | rxSetComputeContext('localpar') | Uitvoering geparallelliseerde over de kernen van de edge-knooppunt-server |
| Spark            | RxSpark()                       | Geparallelliseerde gedistribueerde uit te voeren via Spark op de knooppunten van het HDI-cluster |
| Kaart verminderen       | RxHadoopMR()                    | Geparallelliseerde gedistribueerde worden uitgevoerd via toewijzing verminderen op de knooppunten van het HDI-cluster |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlijnen voor het kiezen van een compute-context

Welke van de drie opties die u kiest die geparallelliseerde uitvoering bieden is afhankelijk van de aard van uw werk analytics, de grootte en de locatie van uw gegevens. Er is geen eenvoudige formule waarin wordt uitgelegd welke compute context moet worden gebruikt. Er zijn echter enkele principes kunt u de juiste keuze te maken of ten minste, zodat u kunt beperken uw keuzes gemaakt voordat u een benchmark uitvoert. Deze richtsnoer omvatten:

- Het lokale Linux-bestandssysteem is sneller dan HDFS.
- Herhaalde analyses zijn sneller als de gegevens lokaal is en als het zich in XDF.
- Is het raadzaam om te streamen kleine hoeveelheden gegevens uit een tekst-gegevensbron. Als de hoeveelheid gegevens groter is, deze converteren naar XDF voor analyse.
- De overhead voor het kopiëren van of de gegevens naar het edge-knooppunt voor analyse streaming wordt voor zeer grote hoeveelheden gegevens.
- Spark is sneller dan kaart verminderen voor analyse in Hadoop.

Deze principes gezien, bieden de volgende secties een aantal algemene regels van miniatuur voor het selecteren van een compute-context.

### <a name="local"></a>Lokaal
* Als de hoeveelheid gegevens te analyseren klein is en geen herhaalde analyse vereist, klikt u vervolgens stream deze rechtstreeks in de analysis routinematige via *lokale* of *localpar*.
* Als de hoeveelheid gegevens te analyseren kleine of middelgrote en herhaalde analyse is vereist, klikt u vervolgens kopiëren naar het lokale bestandssysteem op XDF importeren en analyseren via *lokale* of *localpar*.

### <a name="hadoop-spark"></a>Hadoop, Spark
* Als de hoeveelheid gegevens te analyseren groot is, dan importeren naar een Spark-DataFrame met **RxHiveData** of **RxParquetData**, of XDF in HDFS (tenzij er een probleem is met de opslag), en analyseren met behulp van de compute Spark context.

### <a name="hadoop-map-reduce"></a>Beperken van Hadoop-kaart
* De kaart verminderen compute-context alleen gebruiken als u een onoverkomelijke probleem met de compute-context voor Spark optreden omdat in het algemeen langzamer.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Inline-help op rxSetComputeContext
Zie voor meer informatie over en voorbeelden van RevoScaleR compute contexten, de inline in R help bij de methode rxSetComputeContext, bijvoorbeeld:

    > ?rxSetComputeContext

U kunt ook verwijzen naar de [gedistribueerde computing overzicht](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing) in [Machine Learning-Server-documentatie](https://docs.microsoft.com/machine-learning-server/).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerde over kernen van de edge-knooppunt of de HDInsight-cluster. Zie de volgende onderwerpen voor meer informatie over het ML-Services gebruiken met HDInsight-clusters:

* [Overzicht van Services voor Hadoop ML](r-server-overview.md)
* [Aan de slag met ML-Services voor Hadoop](r-server-get-started.md)
* [Azure Storage-opties voor ML-Services op HDInsight](r-server-storage.md)

