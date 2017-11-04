---
title: Met behulp van Hive en Hadoop - Azure HDInsight-sensorgegevens analyseren | Microsoft Docs
description: Informatie over het analyseren van sensorgegevens met behulp van de Console Query Hive met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel met PowerView visualiseren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: d4d216cd1d8de7c86f71d0181270607e12f761cf
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Met behulp van de Console Hive-Query met Hadoop in HDInsight-sensorgegevens analyseren

Informatie over het analyseren van sensorgegevens met behulp van de Console Query Hive met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel visualiseren met behulp van Power View.

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight op basis van Windows-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.


In dit voorbeeld gebruikt u Hive voor historische gegevens verwerken en identificeren van problemen met verwarming en airconditioning systemen. In het bijzonder het identificeren van systemen kunnen geen betrouwbaar kunnen onderhouden van een set temperaturen door de volgende taken:

* HIVE-tabellen query uitvoeren op gegevens opgeslagen in door komma's gescheiden waarden (CSV)-bestanden maken.
* Maak HIVE-query's om de gegevens te analyseren.
* Gebruik Microsoft Excel verbinding maken met HDInsight voor het ophalen van de geanalyseerde gegevens.
* Gebruik de gegevens visualiseren, Power View.

![Een diagram van de oplossingsarchitectuur](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Vereisten

* Een HDInsight (Hadoop)-cluster: Zie [maken Hadoop-clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie over het maken van een cluster.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel wordt gebruikt voor gegevensvisualisatie met [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC-stuurprogramma](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Het voorbeeld uitvoeren

1. Navigeer naar de volgende URL van uw webbrowser: 

         https://<clustername>.azurehdinsight.net

    Vervang `<clustername>` door de naam van uw HDInsight-cluster.

    Wanneer u wordt gevraagd, worden geverifieerd met behulp van de administrator-gebruikersnaam en wachtwoord waarmee u bij het inrichten van dit cluster.

2. Op de webpagina klikt u op de **Getting Started galerie** tabblad en klik vervolgens onder de **oplossingen met voorbeeldgegevens** categorie, klikt u op de **Sensor gegevensanalyse** voorbeeld.

    ![Gestarte afbeelding ophalen](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Volg de instructies op de pagina voor het voltooien van de steekproef.
