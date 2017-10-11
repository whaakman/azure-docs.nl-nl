---
title: Hive gebruiken met Hadoop voor websitelogboekanalyse - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Hive met HDInsight websitelogboeken analyseren. U hebt een logboekbestand gebruiken als invoer in een tabel met HDInsight en HiveQL gebruiken voor het opvragen van de gegevens.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: e1cdb786bb6049980aafc0213abf53013e342618
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Hive gebruiken met HDInsight op basis van Windows logboeken van websites te analyseren
Informatie over het HiveQL gebruiken met HDInsight voor het analyseren van Logboeken vanaf een website. Websitelogboekanalyse kan worden gebruikt om segmenteren van uw doelgroep op basis van soortgelijke activiteiten, demografische gegevens van bezoekers categoriseren en wilt weten de inhoud ze weergeven, de websites die ze afkomstig zijn van, enzovoort.

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight op basis van Windows-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

In dit voorbeeld gebruikt u een HDInsight-cluster voor het analyseren van de logboekbestanden van de website om inzicht te krijgen in de frequentie van bezoeken van externe websites in een dag naar de website. Ook genereert u een overzicht van website-fouten die optreden van de gebruikers. U leert hoe:

* Verbinding maken met een Azure Blob storage, bevat de logboekbestanden van de website.
* HIVE-tabellen om op te vragen deze logboeken maken.
* Maak HIVE-query's om de gegevens te analyseren.
* Microsoft Excel gebruiken voor verbinding met HDInsight (met behulp van (ODBC) open database connectivity geanalyseerde gegevens ophalen.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>Vereisten
* U moet een Hadoop-cluster in Azure HDInsight hebt ingericht. Zie voor instructies [HDInsight-Clusters inrichten][hdinsight-provision].
* U moet Microsoft Excel 2013 of Excel 2010 is geïnstalleerd.
* U moet hebben [stuurprogramma Microsoft Hive ODBC](http://www.microsoft.com/download/details.aspx?id=40886) gegevens importeren uit Hive in Excel.

## <a name="to-run-the-sample"></a>Het voorbeeld uitvoeren
1. Van de [Azure Portal](https://portal.azure.com/), vanaf het Startboard (als u er in het cluster hebt vastgemaakt), klik op de tegel van het cluster waarop u wilt uitvoeren van het voorbeeld.
2. De cluster-blade onder **snelkoppelingen**, klikt u op **Cluster-Dashboard**, en vervolgens naar de **Cluster-Dashboard** blade, klikt u op **HDInsight-Cluster Dashboard**. U kunt het dashboard ook rechtstreeks openen met behulp van de volgende URL:

         https://<clustername>.azurehdinsight.net

    Wanneer u wordt gevraagd, worden geverifieerd met behulp van de administrator-gebruikersnaam en wachtwoord waarmee u bij het inrichten van het cluster.
3. Op de webpagina klikt u op de **Getting Started galerie** tabblad en klik vervolgens onder de **oplossingen met voorbeeldgegevens** categorie, klikt u op de **Websitelogboekanalyse** voorbeeld.
4. Volg de instructies op de pagina voor het voltooien van de steekproef.

## <a name="next-steps"></a>Volgende stappen
Probeer het volgende voorbeeld: [analyseren van sensorgegevens met Hive met HDInsight](hdinsight-hive-analyze-sensor-data.md).

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
