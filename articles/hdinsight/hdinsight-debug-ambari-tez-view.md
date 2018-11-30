---
title: Gebruik Apache Ambari Tez weergave met HDInsight - Azure
description: Informatie over het gebruik van de Apache Ambari Tez-weergave fouten opsporen in Tez-taken in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 0d2f55538517881ce6cc237885f3bcadfa084520
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494959"
---
# <a name="use-apache-ambari-views-to-debug-apache-tez-jobs-on-hdinsight"></a>Apache Ambari-weergaven gebruiken om op te sporen Apache Tez-taken in HDInsight

De [Apache Ambari](https://ambari.apache.org/) Webgebruikersinterface voor HDInsight bevat een [Apache TEZ](https://tez.apache.org/) weergeven die kan worden gebruikt om te begrijpen en taken die gebruikmaken van Tez. De Tez-weergave kunt u de taak als een diagram van verbonden items visualiseren, Inzoomen op elk item en statistieken en waardevolle informatie ophalen.

> [!IMPORTANT]
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor stappen voor het maken van een cluster [aan de slag met HDInsight op basis van Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).
* Een moderne webbrowser met ondersteuning voor HTML5.

## <a name="understanding-apache-tez"></a>Wat is Apache Tez?

Tez is een uitbreidbaar framework voor gegevensverwerking in Apache Hadoop. deze meer snelheden worden bereikt dan traditionele MapReduce-verwerking biedt. Voor Linux gebaseerde HDInsight-clusters is de standaard-engine voor Hive.

Tez maakt een omgeleid Acyclic Graph (DAG) die worden beschreven de volgorde van de acties die zijn vereist door taken. Afzonderlijke acties hoekpunten worden genoemd, en een deel van de algemene taak wordt uitgevoerd. De daadwerkelijke uitvoering van het werk dat wordt beschreven door een hoekpunt een taak wordt genoemd, en kan worden verdeeld over meerdere knooppunten in het cluster.

### <a name="understanding-the-tez-view"></a>Informatie over de Tez-weergave

De Tez-weergave bevat zowel historische gegevens en informatie over de processen die worden uitgevoerd. Deze informatie laat zien hoe een taak wordt verdeeld tussen verschillende clusters. Ook wordt gebruikt door taken en hoekpunten tellers en foutinformatie met betrekking tot de taak weergegeven. Deze bieden mogelijk nuttige informatie in de volgende scenario's:

* Bewaking van langlopende processen, de voortgang van de kaart bekijken en aantal taken te verkleinen.
* Analyse van historische gegevens voor geslaagde of mislukte processen voor meer informatie over hoe de verwerking kan worden verbeterd of waarom is mislukt.

## <a name="generate-a-dag"></a>Genereren van een DAG

De Tez-weergave bevat alleen gegevens als een taak die u gebruikt de Tez-engine wordt uitgevoerd of is eerder is uitgevoerd. Eenvoudige Hive-query's kunnen worden opgelost zonder gebruik van Tez. Meer complexe query's die wilt filteren, groeperen, bestellen, joins, enzovoort. Gebruik de Tez-engine.

Gebruik de volgende stappen uit om uit te voeren van een Hive-query die gebruikmaakt van Tez:

1. Navigeer in een webbrowser naar https://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw HDInsight-cluster.

2. Selecteer in het menu aan de bovenkant van de pagina, de **weergaven** pictogram. Dit pictogram ziet eruit als een reeks van de kwadraten. Selecteer in de vervolgkeuzelijst die wordt weergegeven, **Hive-weergave**.

    ![Hive-weergave selecteren](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Wanneer de Hive-weergave wordt geladen, plakt u de volgende query in de Query-Editor en klik vervolgens op **uitvoeren**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    Als de taak is voltooid, ziet u de uitvoer weergegeven in de **resultaten queryproces** sectie. De resultaten moeten vergelijkbaar met de volgende tekst:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Selecteer de **Log** tabblad. Ziet u informatie die vergelijkbaar is met de volgende tekst:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Sla de **App-id** waarde, want deze waarde wordt gebruikt in de volgende sectie.

## <a name="use-the-tez-view"></a>De Tez-weergave gebruiken

1. Selecteer in het menu aan de bovenkant van de pagina, de **weergaven** pictogram. Selecteer in de vervolgkeuzelijst die wordt weergegeven, **Tez weergave**.

    ![Tez weergave selecteren](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Wanneer de Tez-weergave wordt geladen, ziet u een lijst met hive-query's die momenteel worden uitgevoerd, of zijn uitgevoerd op het cluster.

    ![Alle dag 's](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Als u slechts één vermelding hebt, is het voor de query die u in de vorige sectie hebt uitgevoerd. Als u meerdere vermeldingen hebt, kunt u zoeken met behulp van de velden aan de bovenkant van de pagina.

4. Selecteer de **Query-ID** voor een Hive-query. Informatie over de query wordt weergegeven.

    ![Details van de DAG](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. De tabbladen op deze pagina kunnen u de volgende informatie bekijken:

    * **Querydetails**: meer informatie over de Hive-query.
    * **Tijdlijn**: informatie over hoe lang duurde in elke fase van de verwerking.
    * **Configuraties**: de configuratie voor deze query gebruikt.

    Van __Querydetails__ kunt u de koppelingen naar meer informatie over de __toepassing__ of de __DAG__ voor deze query.
    
    * De __toepassing__ koppeling geeft informatie weer over de YARN-toepassing voor deze query. Hier kunt u toegang tot de YARN-toepassingslogboeken.
    * De __DAG__ koppeling geeft informatie weer over de gerichte acyclische grafiek voor deze query. Hier vindt u een grafische weergave van de DAG. U vindt hier ook informatie op de hoekpunten in de DAG.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe u de Apache Tez-weergave, meer informatie over [Apache Hive in HDInsight met behulp van](hadoop/hdinsight-use-hive.md).

Zie voor meer technische informatie over Apache Tez, de [Apache Tez-pagina op Hortonworks](http://hortonworks.com/hadoop/tez/).

Zie voor meer informatie over het gebruik van Apache Ambari met HDInsight [beheren HDInsight-clusters met behulp van de Apache Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)
