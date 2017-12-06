---
title: Ambari Tez weergave gebruiken met HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van de weergave Ambari Tez fouten opsporen in Tez-taken in HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: b565ef0f7672d1288e922e28551ad3f6ec5b6cb7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a>Ambari-weergaven gebruiken om op te sporen Tez-taken in HDInsight

De Ambari-Webgebruikersinterface voor HDInsight bevat een Tez-weergave die kan worden gebruikt om te begrijpen en functies die gebruikmaken van Tez voor foutopsporing. De Tez-weergave kunt u de taak als een grafiek van verbonden items visualiseren, Inzoomen op elk item en statistieken en logboekregistratie informatie ophalen.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor stapsgewijze instructies voor het maken van een cluster, [aan de slag met HDInsight op basis van Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).
* Een moderne webbrowser met ondersteuning voor HTML5.

## <a name="understanding-tez"></a>Understanding Tez

Tez is een uitbreidbaar framework voor gegevensverwerking in Hadoop. deze groter snelheden dan traditionele MapReduce-verwerking biedt. Voor Linux gebaseerde HDInsight-clusters is de standaard-engine voor Hive.

Tez maakt een omgeleid acyclische grafiek (DAG) die in de volgorde van acties die zijn vereist voor de taken worden beschreven. Afzonderlijke acties hoekpunten worden genoemd, en een onderdeel van de algehele taak uitvoeren. De werkelijke uitvoering van het werk dat is beschreven door een hoekpunt een taak wordt aangeroepen en kan worden verdeeld over meerdere knooppunten in het cluster.

### <a name="understanding-the-tez-view"></a>Inzicht in de weergave Tez

De Tez-weergave bevat zowel historische gegevens en informatie over de processen die worden uitgevoerd. Deze informatie laat zien hoe een taak wordt verdeeld tussen verschillende clusters. Ook wordt gebruikt door taken en hoekpunten tellers en informatie over de fout is gerelateerd aan de taak weergegeven. Dit kan nuttige informatie in de volgende scenario's bieden:

* Bewaking van langlopende verwerkt, wordt de voortgang van de kaart weergeven en taken te verminderen.
* Analyse van historische gegevens voor de geslaagde of mislukte processen voor meer informatie over hoe de verwerking kan worden verbeterd of waarom is mislukt.

## <a name="generate-a-dag"></a>Genereren van een DAG

De Tez-weergave bevat alleen gegevens als een taak die eerder is gebruikt de Tez-engine wordt uitgevoerd of is uitgevoerd. Eenvoudige Hive-query's kunnen worden opgelost zonder Tez. Meer complexe query's die wilt filteren, groeperen, rangschikken, joins, enzovoort. Gebruik de Tez-engine.

Gebruik de volgende stappen uitvoeren van een Hive-query die gebruikmaakt van Tez:

1. Navigeer in een webbrowser naar https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster.

2. Selecteer in het menu aan de bovenkant van de pagina de **weergaven** pictogram. Dit pictogram ziet er als een reeks van de kwadraten uit. Selecteer in de vervolgkeuzelijst **Hive-weergave**.

    ![Hive-weergave selecteren](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Wanneer de Hive-weergave wordt geladen, plak de volgende query in de Query-Editor en klik vervolgens op **uitvoeren**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    Zodra de taak is voltooid, ziet u de uitvoer weergegeven in de **resultaten queryproces** sectie. De resultaten moeten zijn vergelijkbaar met de volgende tekst:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Selecteer de **logboek** tabblad. Ziet u de informatie is vergelijkbaar met de volgende tekst:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Sla de **App-id** waarde, zoals deze waarde wordt gebruikt in de volgende sectie.

## <a name="use-the-tez-view"></a>De weergave Tez gebruiken

1. Selecteer in het menu aan de bovenkant van de pagina de **weergaven** pictogram. Selecteer in de vervolgkeuzelijst **Tez weergave**.

    ![Tez weergave selecteren](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Als de Tez-weergave wordt geladen, ziet u een lijst met hive-query's die momenteel worden uitgevoerd, of zijn uitgevoerd op het cluster.

    ![Alle dag 's](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Als u slechts één vermelding hebt, is het voor de query die u in de vorige sectie hebt uitgevoerd. Als u meerdere vermeldingen hebt, kunt u zoeken met behulp van de velden boven aan de pagina.

4. Selecteer de **Query-ID** voor een Hive-query. Informatie over de query wordt weergegeven.

    ![Details van de DAG](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. De tabbladen op deze pagina kunnen u de volgende informatie bekijken:

    * **Details van een query**: meer informatie over de Hive-query.
    * **Tijdlijn**: informatie over hoe lang duurde in elke fase van de verwerking.
    * **Configuraties**: de configuratie voor deze query gebruikt.

    Van __Query Details__ kunt u de koppelingen vindt u informatie over de __toepassing__ of de __DAG__ voor deze query.
    
    * De __toepassing__ koppeling geeft informatie weer over de YARN-toepassing voor deze query. Hier kunt u de YARN-Logboeken openen.
    * De __DAG__ koppeling geeft informatie weer over de gerichte acyclische grafiek voor deze query. Hier vindt u een grafische weergave van de DAG. U kunt ook informatie over de hoekpunten binnen de DAG vinden.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u de weergave Tez, meer informatie over [met behulp van Hive in HDInsight](hadoop/hdinsight-use-hive.md).

Zie voor meer technische informatie over Tez, de [Tez-pagina op Hortonworks](http://hortonworks.com/hadoop/tez/).

Zie voor meer informatie over het gebruik van Ambari met HDInsight [HDInsight-clusters beheren met de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)
