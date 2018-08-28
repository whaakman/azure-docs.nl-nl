---
title: Hive met Hadoop gebruiken voor analyse van websitelogboeken - Azure HDInsight
description: Meer informatie over het gebruik van Hive met HDInsight voor het analyseren van websitelogboeken. U moet een logboekbestand gebruiken als invoer in een HDInsight-tabel en gebruik HiveQL om de gegevens op te vragen.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 47175ed4bc01a43d1f21cbfc84b8a788f11b7338
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048021"
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Hive gebruiken met HDInsight op basis van Windows voor het analyseren van Logboeken van websites
Leer hoe u HiveQL gebruiken met HDInsight voor het analyseren van Logboeken van een website. Websitelogboekanalyse kan worden gebruikt om te Segmenteer uw doelgroep op basis van de vergelijkbare activiteiten, Categoriseer bezoekers van de site op basis van demografische gegevens, en om erachter te komen de inhoud ze weergeven, de websites die ze afkomstig zijn uit, enzovoort.

> [!IMPORTANT]
> De stappen in dit document werken alleen met HDInsight op basis van een Windows-clusters. HDInsight is alleen beschikbaar voor Windows voor versies lager dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

In dit voorbeeld gebruikt u een HDInsight-cluster voor het analyseren van website-logboekbestanden om inzicht te krijgen in de frequentie van dagelijkse bezoeken naar de website vanaf externe websites in een dag. U genereert ook een samenvatting van websitefouten die gebruikers ondervinden. In deze zelfstudie leert u procedures om het volgende te doen:

* Verbinding maken met een Azure Blob-opslag, waarin de logboekbestanden van de website.
* HIVE-tabellen om deze logboeken query's te maken.
* HIVE-query's voor het analyseren van de gegevens maken.
* Microsoft Excel gebruiken voor verbinding met HDInsight (met behulp van open database connectivity (ODBC) om op te halen van de geanalyseerde gegevens.

![HDI. Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Vereisten
* U moet een Hadoop-cluster op Azure HDInsight hebt ingericht. Zie voor instructies [HDInsight-Clusters inrichten](../hdinsight-hadoop-provision-linux-clusters.md).
* U moet Microsoft Excel 2013 of Excel 2010 is geïnstalleerd.
* U moet hebben [stuurprogramma Microsoft Hive ODBC](http://www.microsoft.com/download/details.aspx?id=40886) gegevens van Hive in Excel importeren.

## <a name="to-run-the-sample"></a>Het voorbeeld uitvoeren
1. Uit de [Azure-portal](https://portal.azure.com/), vanaf het Startboard (als u er in het cluster hebt vastgemaakt), klikt u op de tegel van het cluster waarop u wilt het voorbeeld uitvoert.
2. Vanuit de clusterblade onder **snelkoppelingen**, klikt u op **Clusterdashboard**, en vervolgens naar de **Clusterdashboard** blade, klikt u op **HDInsight-Cluster Dashboard**. U kunt het dashboard ook rechtstreeks openen met behulp van de volgende URL:

         https://<clustername>.azurehdinsight.net

    Wanneer u hierom wordt gevraagd, kunt u verifiëren met behulp van de gebruikersnaam van beheerder en het wachtwoord die u hebt gebruikt bij het inrichten van het cluster.
3. Van de webpagina die wordt geopend, klikt u op de **Getting Started galerie** tabblad, en klik vervolgens onder de **oplossingen met voorbeeldgegevens** categorie, klikt u op de **Websitelogboekanalyse** voorbeeld.
4. Volg de instructies op de pagina voor het voltooien van het voorbeeld.

## <a name="next-steps"></a>Volgende stappen
Probeer het volgende voorbeeld: [analyseren van sensorgegevens via Hive met HDInsight](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
