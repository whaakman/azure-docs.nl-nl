---
title: Opties van de context voor R Server op HDInsight - Azure COMPUTE | Microsoft Docs
description: Meer informatie over de verschillende rekenscenario context opties beschikbaar voor gebruikers met R Server op HDInsight
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 4c839bf0c39bf10855f8a31770b82a04ed1ca457
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>COMPUTE context opties voor R Server op HDInsight

Microsoft R Server op Azure HDInsight bepaalt hoe aanroepen worden uitgevoerd door de instelling van de compute-context. In dit artikel bevat een overzicht van de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerde over kernen van de edge-knooppunt of de HDInsight-cluster.

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts. Met een edge-knooppunt hebt u de optie van de parallelized gedistribueerde functies van ScaleR over de kernen van de edge-knooppunt server wordt uitgevoerd. U kunt ook deze uitvoeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark compute-contexten.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Microsoft R Server op Azure HDInsight
[Microsoft R Server op Azure HDInsight](r-server-overview.md) biedt de nieuwste mogelijkheden voor analyses op basis van R. Het kan gebruiken gegevens die zijn opgeslagen in een HDFS-container in uw [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob storage") storage-account, een Data Lake store of het lokale bestandssysteem van Linux. Aangezien R Server is gebouwd op open-source R, wordt in de R-toepassingen die u bouwt de 8000 + open-source R-pakketten kunnen toepassen. Ze kunnen ook de routines in gebruiken [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler), van Microsoft big analytics gegevenspakket die is opgenomen in R Server.  

## <a name="compute-contexts-for-an-edge-node"></a>COMPUTE contexten voor een edge-knooppunt
In het algemeen een R-script wordt uitgevoerd in R Server op de edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen zijn de stappen die een ScaleR functie aanroept. Het aanroepen van de ScaleR uitgevoerd in een compute-omgeving die wordt bepaald door de manier waarop u de context van de compute ScaleR instelt.  Wanneer u uw R-script vanaf een edge-knooppunt uitvoeren, wordt de mogelijke waarden van de compute-context zijn:

- lokale opeenvolgende (*lokale*)
- lokale parallel (*localpar*)
- Kaart verminderen
- Spark

De *lokale* en *localpar* opties verschillen alleen in het **rxExec** aanroepen uitgevoerd. Ze beide uitgevoerd andere rx-functieaanroepen op een parallelle manier over alle beschikbare kernen tenzij anders is aangegeven door het gebruik van de ScaleR **numCoresToUse** optie, bijvoorbeeld `rxOptions(numCoresToUse=6)`. Parallelle uitvoeringsopties bieden optimale prestaties.

De volgende tabel geeft een overzicht van de verschillende compute context opties in te stellen hoe aanroepen uitgevoerd:

| Compute-context  | Het instellen                      | Uitvoeringscontext                        |
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
Zie voor meer informatie over en voorbeelden van ScaleR compute contexten, de inline in R help bij de methode rxSetComputeContext, bijvoorbeeld:

    > ?rxSetComputeContext

U kunt ook verwijzen naar de [ScaleR gedistribueerde Computing handleiding](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing) die beschikbaar is in de [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx) bibliotheek.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerde over kernen van de edge-knooppunt of de HDInsight-cluster. Zie de volgende onderwerpen voor meer informatie over het gebruik van R Server met HDInsight-clusters:

* [Overzicht van R Server voor Hadoop](r-server-overview.md)
* [Aan de slag met R Server voor Hadoop](r-server-get-started.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](r-server-storage.md)

