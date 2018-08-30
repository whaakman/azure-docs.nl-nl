---
title: 'Zelfstudie: Een Scala Maven-toepassing maken voor Apache Spark in Azure HDInsight met behulp van IntelliJ'
description: U gaat een Spark-toepassing maken die is geschreven in Scala met Apache Maven als het buildsysteem en een bestaand Maven-archetype voor Scala geleverd door IntelliJ IDEA.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: fc1f952128b4cfbb082f4c539a102f40d3b85e8d
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43040295"
---
# <a name="tutorial-create-a-scala-maven-application-for-spark-in-hdinsight-using-intellij"></a>Zelfstudie: Een Scala Maven-toepassing maken voor Spark in HDInsight met behulp van IntelliJ

In deze zelfstudie leert u hoe u met behulp van Maven met IntelliJ IDEA een Spark-toepassing maakt die is geschreven in Scala. In dit artikel wordt Apache Maven gebruikt als het buildsysteem en er is een bestaand Maven-archetype voor Scala beschikbaar geleverd door IntelliJ IDEA.  Het maken van een Scala-toepassing in IntelliJ IDEA omvat de volgende stappen:

* Maven gebruiken als het buildsysteem.
* POM-bestand (Project Object Model) bijwerken om afhankelijkheden van Spark-module om te zetten.
* De toepassing schrijven in Scala.
* Een JAR-bestand genereren dat kan worden verstuurd naar HDInsight Spark-clusters.
* De toepassing uitvoeren in een Spark-cluster met behulp van Livy.

> [!NOTE]
> HDInsight biedt ook een invoegtoepassing voor IntelliJ IDEA om het proces van het maken en verzenden van toepassingen naar een HDInsight Spark-cluster in Linux te vereenvoudigen. Zie [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md) voor meer informatie.
> 

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * IntelliJ gebruiken voor het ontwikkelen van een Scala Maven-toepassing

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.


## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* Oracle Java Development kit. Deze kunt u [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) downloaden.
* Een Java-IDE. In dit artikel wordt IntelliJ IDEA 18.1.1 gebruikt. Deze kunt u [hier](https://www.jetbrains.com/idea/download/) downloaden.

## <a name="use-intellij-to-create-application"></a>IntelliJ gebruiken om een toepassing te maken

1. Start IntelliJ IDEA en maak een project. Voer in het dialoogvenster **Nieuw project** de volgende handelingen uit: 

   a. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.

   b. Selecteer in de lijst **Build-programma** een van de volgende opties, afhankelijk van uw behoeften:

      * **Maven**, voor de ondersteuning van de wizard Scala-project maken
      * **SBT**, voor het beheren van de afhankelijkheden en het maken van het Scala-project

   ![Het dialoogvenster Nieuw project](./media/apache-spark-create-standalone-application/create-hdi-scala-app.png)

1. Selecteer **Volgende**.

1. Met de wizard Scala-project maken wordt automatisch gedetecteerd of u de Scala-invoegtoepassing hebt geïnstalleerd. Selecteer **Installeren**.

   ![Scala-invoegtoepassing controleren](./media/apache-spark-create-standalone-application/Scala-Plugin-check-Reminder.PNG) 

1. Selecteer **OK** om de Scala-invoegtoepassing te downloaden. Volg de instructies om IntelliJ opnieuw te starten. 

   ![Het dialoogvenster Scala-invoegtoepassing installeren](./media/apache-spark-create-standalone-application/Choose-Scala-Plugin.PNG)

1. Voer in het venster **Nieuw project** de volgende handelingen uit:  

    ![De Spark SDK selecteren](./media/apache-spark-create-standalone-application/hdi-new-project.png)

   a. Geef een projectnaam en locatie op.

   b. Selecteer in de vervolgkeuzelijst **Project SDK** de optie **Java 1.8** voor het Spark 2.x-cluster of selecteer **Java 1.7** voor het Spark 1.x-cluster.

   c. In de vervolgkeuzelijst **Spark-versie** wordt de juiste versie voor Spark SDK en Scala SDK geïntegreerd via de wizard Scala-project maken. Selecteer **Spark 1.x** als de Spark-clusterversie ouder is dan 2.0. Selecteer anders **Spark 2.x**. In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)** gebruikt.

1. Selecteer **Voltooien**.

## <a name="install-scala-plugin-for-intellij-idea"></a>Scala-invoegtoepassing voor IntelliJ IDEA installeren
Gebruik de volgende stappen om de Scala-invoegtoepassing te installeren:

1. Open IntelliJ IDEA.
1. Selecteer **Configure** in het welkomstscherm en selecteer vervolgens **Plugins**.
   
    ![Scala-invoegtoepassing inschakelen](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
1. Selecteer **Install JetBrains plugin** in de linkerbenedenhoek. 
1. Zoek in het dialoogvenster **Browse JetBrains Plugins** naar **Scala** en selecteer vervolgens **Install**.
   
    ![Scala-invoegtoepassing installeren](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
1. Als de invoegtoepassing is geïnstalleerd, moet u de IDE opnieuw starten.

## <a name="create-a-standalone-scala-project"></a>Een zelfstandig Scala-project maken
1. Open IntelliJ IDEA.
1. Selecteer **New > Project** in het menu **File** om een nieuw project te maken.
1. Maak de volgende keuzes in het dialoogvenster New Project:
   
    ![Maven-project maken](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * Selecteer **Maven** als het projecttype.
   * Geef een **Project SDK** op. Selecteer **New** en navigeer naar de installatiemap van Java, meestal `C:\Program Files\Java\jdk1.8.0_66`.
   * Selecteer de optie **Create from archetype**.
   * Selecteer **org.scala tools.archetypes:scala-archetype-simple** in de lijst van archetypen. Met dit archetype maakt u de juiste mapstructuur en worden de vereiste standaardafhankelijkheden voor het schrijven van het Scala programma gedownload.
1. Selecteer **Volgende**.
1. Geef relevante waarden op voor **GroupId**, **ArtifactId** en **Version**. In deze zelfstudie worden de volgende waarden gebruikt:

    - GroupId: com.microsoft.spark.example
    - ArtifactId: SparkSimpleApp
1. Selecteer **Volgende**.
1. Controleer de instellingen en selecteer vervolgens **Next**.
1. Controleer de naam en de locatie van het project en selecteer vervolgens **Finish**.
1. Selecteer in het linkerdeelvenster **src > test > scala > com > microsoft > spark > example**, klik met de rechtermuisknop op **MySpec** en selecteer vervolgens **Delete**. U hebt dit bestand niet nodig voor de toepassing.
  
1. In de volgende stappen gaat u het bestand pom.xml bijwerken om de afhankelijkheden voor de Spark Scala-toepassing te definiëren. Om deze afhankelijkheden automatisch te downloaden en om te zetten, moet Maven op de juiste manier worden geconfigureerd.
   
    ![Maven configureren voor automatische downloads](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. Selecteer **Settings** in het menu **File**.
   1. Ga in het dialoogvenster **Settings** naar **Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.
   1. Schakel het selectievakje **Import Maven projects automatically** in.
   1. Selecteer **Apply** en vervolgens **OK**.
1. Selecteer in het linkerdeelvenster **src > main > scala > com.microsoft.spark.example** en dubbelklik vervolgens op **App** om App.scala te openen.

1. Vervang de bestaande voorbeeldcode door de volgende code en sla de wijzigingen op. Deze code leest de gegevens uit HVAC.csv (beschikbaar in alle HDInsight Spark-clusters), haalt de rijen op die maar één cijfer hebben in de zesde kolom en schrijft de uitvoer naar **/HVACOut**, in de standaardopslagcontainer voor het cluster.

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
1. Dubbelklik in het linkerdeelvenster op **pom.xml**.
   
   1. Voeg binnen `<project>\<properties>` de volgende segmenten toe:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   1. Voeg binnen `<project>\<dependencies>` de volgende segmenten toe:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Sla de wijzigingen in pom.xml op.
1. Maak het JAR-bestand. IntelliJ IDEA maakt het mogelijk om het JAR-bestand te maken als een artefact van een project. Voer de volgende stappen uit.
    
    1. Selecteer **Project Structure** in het menu **File**.
    1. Selecteer **Artifacts** in het dialoogvenster **Project Structure** en selecteer vervolgens het plusteken. Selecteer **JAR** in het pop-updialoogvenster en selecteer vervolgens **From modules with dependencies**.
       
        ![JAR maken](./media/apache-spark-create-standalone-application/create-jar-1.png)
    1. Selecteer in het dialoogvenster **Create JAR from Modules** het weglatingsteken (![weglatingsteken](./media/apache-spark-create-standalone-application/ellipsis.png)) bij **Main Class**.
    1. Selecteer in het dialoogvenster **Select Main Class** de klasse die standaard wordt weergegeven en selecteer vervolgens **OK**.
       
        ![JAR maken](./media/apache-spark-create-standalone-application/create-jar-2.png)
    1. Controleer of in het dialoogvenster **Create JAR from Modules** de optie **extract to the target JAR** is geselecteerd en selecteer vervolgens **OK**.  Met deze instelling wordt er één JAR gemaakt met alle afhankelijkheden.
       
        ![JAR maken](./media/apache-spark-create-standalone-application/create-jar-3.png)
    1. Het tabblad Output Layout geeft een overzicht van alle JAR-bestanden die zijn opgenomen als onderdeel van het Maven-project. U kunt de bestanden selecteren en verwijderen waarvan de Scala-toepassing niet direct afhankelijk is. Voor de toepassing die u hier maakt, kunt u alle bestanden behalve het laatste bestand (**SparkSimpleApp compile output**) verwijderen. Selecteer de JAR-bestanden die u wilt verwijderen en selecteer vervolgens het pictogram **Delete**.
       
        ![JAR maken](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Zorg ervoor dat de optie **Include in project build** is geselecteerd, zodat het JAR-bestand tekens wordt gemaakt wanneer het project wordt gecompileerd of bijgewerkt. Selecteer **Apply** en vervolgens **OK**.
    1. Selecteer **Build Artifacts** in het menu **Build** om het JAR-bestand te maken. Het bestand wordt opgeslagen in **\out\artifacts**.
       
        ![JAR maken](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>De toepassing uitvoeren in het Spark-cluster
U kunt de volgende methoden gebruiken om de toepassing uit te voeren in het cluster:

* **Kopieer het JAR-bestand van de toepassing naar de Azure-opslagblob** die aan het cluster is gekoppeld. Dit kan met [**AzCopy**](../../storage/common/storage-use-azcopy.md), een opdrachtregelprogramma. Er zijn echter een heleboel clients die u kunt gebruiken om gegevens te uploaden. Meer informatie hierover vindt u in [Gegevens voor Hadoop-taken uploaden in HDInsight](../hdinsight-upload-data.md).
* **Gebruik Livy om een toepassingstaak op afstand te versturen** naar het Spark-cluster. Spark-clusters in HDInsight ondersteunen Livy voor het aanbieden van REST-eindpunten waarmee Spark-taken op afstand kunnen worden verzonden. Zie [Spark-taken op afstand verzenden met behulp van Livy met Spark-clusters in HDInsight](apache-spark-livy-rest-interface.md) voor meer informatie.

## <a name="next-step"></a>Volgende stap

In dit artikel hebt u geleerd hoe u een Spark Scala-toepassing maakt. Ga naar het volgende artikel om te leren hoe u deze toepassing uitvoert in een HDInsight Spark-cluster met behulp van Livy.

> [!div class="nextstepaction"]
>[Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](./apache-spark-livy-rest-interface.md)

