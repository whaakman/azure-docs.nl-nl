---
title: Scala app maken om te worden uitgevoerd op Spark-clusters - Azure HDInsight | Microsoft Docs
description: Maak een toepassing Spark is geschreven in Scala met Apache Maven build-systeem als een bestaande Maven archetype voor Scala geleverd door IntelliJ IDEA.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 95dba08744357f8800b05e3d4b892e3a363d5985
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-scala-maven-application-to-run-on-apache-spark-cluster-on-hdinsight"></a>Maak een Scala Maven-toepassing uit te voeren op Apache Spark-cluster in HDInsight

Informatie over het maken van een toepassing Spark is geschreven in Scala met behulp van Maven met IntelliJ IDEA. Het artikel Apache Maven gebruikt als de build-systeem en begint met een bestaande Maven archetype voor Scala geleverd door IntelliJ IDEA.  Maken van een toepassing Scala in IntelliJ IDEA omvat de volgende stappen:

* Maven gebruiken als de build-systeem.
* Werkt Project Object Model (POM) Spark module afhankelijkheden moeten worden opgelost.
* Uw toepassing worden geschreven in Scala.
* Genereert een jar-bestand dat kan worden verstuurd naar HDInsight Spark-clusters.
* Voer de toepassing in Spark-cluster met behulp van Livy.

> [!NOTE]
> HDInsight biedt ook een hulpprogramma van de invoegtoepassing voor IntelliJ IDEA vereenvoudigen van het proces van het maken en verzenden van toepassingen met een HDInsight Spark-cluster op Linux. Zie voor meer informatie [gebruik de invoegtoepassing HDInsight Tools for IntelliJ IDEA maken en verzenden van Spark scala-toepassingen](hdinsight-apache-spark-intellij-tool-plugin.md).
> 
> 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development kit. Kunt u het installeren van [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Een Java IDE. Dit artikel wordt IntelliJ IDEA 15.0.1 gebruikt. Kunt u het installeren van [hier](https://www.jetbrains.com/idea/download/).

## <a name="install-scala-plugin-for-intellij-idea"></a>Scala-invoegtoepassing voor IntelliJ IDEA installeren
Als IntelliJ IDEA installatie niet niet gevraagd voor het inschakelen van Scala-invoegtoepassing, IntelliJ IDEA starten en de volgende stappen voor het installeren van de invoegtoepassing doorlopen:

1. IntelliJ IDEA Start en klik in het welkomstscherm op **configureren** en klik vervolgens op **Plugins**.
   
    ![Scala invoegtoepassing inschakelen](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. Klik in het volgende scherm op **-invoegtoepassing installeren JetBrains** van de linkerbenedenhoek. In de **JetBrains Plugins Bladeren** dialoogvenster dat wordt geopend, Scala zoeken en klik vervolgens op **installeren**.
   
    ![Scala-invoegtoepassing installeren](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. Nadat de invoegtoepassing is geïnstalleerd, klikt u op de **opnieuw IntelliJ IDEA knop** opnieuw opstarten van de IDE.

## <a name="create-a-standalone-scala-project"></a>Een zelfstandige Scala-project maken
1. IntelliJ IDEA Start en een nieuw project maken. De volgende opties in het dialoogvenster Nieuw project en klik vervolgens op **volgende**.
   
    ![Maven-project maken](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * Selecteer **Maven** als het projecttype.
   * Geef een **Project SDK**. Klik op Nieuw en navigeer naar de installatiemap Java doorgaans `C:\Program Files\Java\jdk1.8.0_66`.
   * Selecteer de **maken van archetype** optie.
   * Selecteer in de lijst van archetypes **org.scala tools.archetypes:scala-archetype eenvoudige**. Hiermee wordt de juiste mapstructuur en de vereiste standaard afhankelijkheden voor het schrijven van Scala programma downloaden.
2. Relevante waarden opgeven voor **GroupId**, **artefact-id**, en **versie**. Klik op **Volgende**.
3. In het volgende dialoogvenster waarin u Maven-basismap en andere instellingen voor gebruikers opgeven, accepteer de standaardinstellingen en klik op **volgende**.
4. Geef een naam van het project en de locatie in het dialoogvenster laatste en klik vervolgens op **voltooien**.
5. Verwijder de **MySpec.Scala** op het bestand **src\test\scala\com\microsoft\spark\example**. U hoeft niet dit voor de toepassing.
6. Indien nodig, wijzig de naam van de bron- en standaardbestanden. Ga vanuit het linkerdeelvenster in de IntelliJ IDEA naar **src\main\scala\com.microsoft.spark.example**. Met de rechtermuisknop op **App.scala**, klikt u op **opsplitsen**, bestandsnaam wijzigen, klikt u op en geef de nieuwe naam voor de toepassing in het dialoogvenster en klik op **opsplitsen**.
   
    ![De naam van bestanden](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. In de volgende stappen wordt om te definiëren van de afhankelijkheden voor de toepassing Spark Scala pom.xml bijgewerkt. Voor deze afhankelijkheden worden gedownload en automatisch opgelost, moet u Maven dienovereenkomstig configureren.
   
    ![Maven configureren voor automatische downloads](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. Van de **bestand** menu, klikt u op **instellingen**.
   2. In de **instellingen** dialoogvenster vak, gaat u naar **bouwen, kan worden uitgevoerd, implementatie** > **Build Tools** > **Maven** > **Importing**.
   3. Selecteer de optie voor **Import Maven projects automatisch**.
   4. Klik op **toepassen**, en klik vervolgens op **OK**.
8. Werk het bronbestand Scala zodanig dat de toepassingscode. Open en de bestaande voorbeeldcode vervangen door de volgende code en sla de wijzigingen. Deze code leest de gegevens van de HVAC.csv (beschikbaar op alle HDInsight Spark-clusters), worden de rijen die alleen in de zesde kolom één cijfer hebben en schrijft de uitvoer naar **/HVACOut** onder de standaardcontainer opslag voor het cluster.
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. Werk de pom.xml.
   
   1. Binnen `<project>\<properties>` Voeg de volgende:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. Binnen `<project>\<dependencies>` Voeg de volgende:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Sla de wijzigingen aan pom.xml.
10. Het JAR-bestand maken. IntelliJ IDEA kunt maken van de JAR als een artefact van een project. De volgende stappen uitvoeren.
    
    1. Van de **bestand** menu, klikt u op **projectstructuur**.
    2. In de **projectstructuur** in het dialoogvenster, klikt u op **artefacten** en klik vervolgens op het plusteken. Klik in het pop-updialoogvenster op **JAR**, en klik vervolgens op **van modules met afhankelijkheden**.
       
        ![JAR maken](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. In de **maken JAR van Modules** dialoogvenster vak, klikt u op het weglatingsteken (![weglatingsteken](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) op basis van de **Main klasse**.
    4. In de **Main-klasse selecteren** dialoogvenster vak, selecteer de klasse die standaard wordt weergegeven en klik vervolgens op **OK**.
       
        ![JAR maken](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. In de **maken JAR van Modules** dialoogvenster vak, zorg ervoor dat de optie voor het **uitpakken naar het doel JAR** is geselecteerd en klik vervolgens op **OK**. Hiermee maakt u een één JAR met alle afhankelijkheden.
       
        ![JAR maken](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. Het tabblad van de indeling uitvoer geeft een lijst van alle potten die opgenomen als onderdeel van het Maven-project zijn. U kunt selecteren en het verwijderen van ongewenste waarop de toepassing Scala heeft geen directe afhankelijkheid. Voor de toepassing hier maakt, kunt u alles behalve het laatste bestand (**SparkSimpleApp compileren uitvoer**). Selecteer de potten om te verwijderen en klik vervolgens op de **verwijderen** pictogram.
       
        ![JAR maken](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        Zorg ervoor dat **bouwen op Controleer** selectievakje is ingeschakeld, die zorgt ervoor dat de jar wordt gemaakt telkens wanneer het project wordt gemaakt of bijgewerkt. Klik op **toepassen** en vervolgens **OK**.
    7. Klik in de menubalk op **bouwen**, en klik vervolgens op **Project maken**. U kunt ook klikken op **bouwen artefacten** voor het maken van de jar. De uitvoer jar wordt gemaakt onder **\out\artifacts**.
       
        ![JAR maken](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>De toepassing uitvoert op het Spark-cluster
De toepassing op het cluster uitgevoerd, moet u het volgende doen:

* **De toepassing jar kopiëren naar de Azure storage-blob** die zijn gekoppeld aan het cluster. U kunt [ **AzCopy**](../storage/common/storage-use-azcopy.md), een opdrachtregelprogramma, om dit te doen. Er zijn veel andere clients ook die u kunt gebruiken om gegevens te uploaden. U vindt meer informatie hierover op [gegevens voor Hadoop-taken in HDInsight uploaden](hdinsight-upload-data.md).
* **Livy gebruiken voor het verzenden van de taak van een toepassing op afstand** aan het Spark-cluster. Spark-clusters in HDInsight omvat Livy die beschrijft de REST-eindpunten om op afstand Spark-taken verzenden. Zie voor meer informatie [Spark verzenden van taken op afstand met behulp van Livy met Spark-clusters in HDInsight](hdinsight-apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Volgende stap

In dit artikel hebt u geleerd hoe u een Spark scala-toepassing maakt. Ga naar het volgende artikel voor meer informatie over het uitvoeren van deze toepassing op een HDInsight Spark-cluster met behulp van Livy.

> [!div class="nextstepaction"]
>[Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](hdinsight-apache-spark-livy-rest-interface.md)

