---
title: Analyseren van sensorgegevens via Hive en Hadoop - Azure HDInsight
description: Informatie over het analyseren van sensorgegevens met behulp van de Queryconsole van Hive met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel met PowerView visualiseren.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: f82bac1b478183cad41e1bb9f7dce3fed8b5417b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048895"
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analyseren van sensorgegevens met behulp van de Hive-Query-Console van Hadoop in HDInsight

Informatie over het analyseren van sensorgegevens met behulp van de Queryconsole van Hive met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel visualiseren met behulp van Power View.

> [!IMPORTANT]
> De stappen in dit document werken alleen met HDInsight op basis van een Windows-clusters. HDInsight is alleen beschikbaar voor Windows voor versies lager dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.


In dit voorbeeld gebruikt u Hive voor het verwerken van historische gegevens en identificeren van problemen met verwarming en airconditioning systemen. Specifiek, het identificeren van systemen kunnen geen een set temperaturen betrouwbaar kunnen onderhouden door het uitvoeren van de volgende taken:

* HIVE-tabellen query uitvoeren op gegevens die zijn opgeslagen in bestanden met door komma's gescheiden waarden (CSV) maken.
* HIVE-query's voor het analyseren van de gegevens maken.
* Als u wilt de geanalyseerde gegevens ophalen, Microsoft Excel verbinding maken met HDInsight te gebruiken.
* Gebruik het om gegevens te visualiseren, Power View.

![Een diagram van de oplossingsarchitectuur](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Vereisten

* Een HDInsight (Hadoop)-cluster: Zie [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) voor informatie over het maken van een cluster.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel wordt gebruikt voor gegevensvisualisatie met [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC-stuurprogramma](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Het voorbeeld uitvoeren

1. Vanuit de webbrowser, navigeer naar de volgende URL: 

         https://<clustername>.azurehdinsight.net

    Vervang `<clustername>` door de naam van uw HDInsight-cluster.

    Wanneer u hierom wordt gevraagd, kunt u verifiëren met behulp van de gebruikersnaam van beheerder en het wachtwoord die u hebt gebruikt bij het inrichten van dit cluster.

2. Van de webpagina die wordt geopend, klikt u op de **Getting Started galerie** tabblad, en klik vervolgens onder de **oplossingen met voorbeeldgegevens** categorie, klikt u op de **analyse van de sensorgegevens** voorbeeld.

    ![Gestarte afbeelding ophalen](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Volg de instructies op de pagina voor het voltooien van het voorbeeld.
