---
title: Gebruik Azure Toolkit voor IntelliJ met Hortonworks Sandbox
description: Informatie over het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ met Hortonworks Sandbox.
keywords: hadoop-hulpprogramma's, hive-query, intellij, hortonworks sandbox-, azure toolkit voor intellij
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 98f22f531ca15bf88cd7d0a9add2851651e2eec7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58118320"
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken

Informatie over het gebruik van HDInsight-hulpprogramma's voor IntelliJ Apache Scala-toepassingen ontwikkelen en testen klikt u vervolgens de toepassingen op [Hortonworks Sandbox](https://hortonworks.com/products/sandbox/) op uw computer uitgevoerd. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) is een geïntegreerde ontwikkelomgeving (IDE) van Java voor het ontwikkelen van computersoftware. Als u ontwikkelt en uw toepassingen op Hortonworks Sandbox test, kunt u de toepassingen te verplaatsen [Azure HDInsight](apache-hadoop-introduction.md).

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

- Hortonworks Data Platform (HDP) 2.4 op Hortonworks Sandbox wordt uitgevoerd op uw lokale computer. Als u HDP instelt, Zie [aan de slag in het Apache Hadoop-ecosysteem met een Hadoop-sandbox op een virtuele machine](apache-hadoop-emulator-get-started.md). 
    > [!NOTE]
    > HDInsight-hulpprogramma's voor IntelliJ is getest met HDP 2.4. Als u HDP 2.4, vouw **Hortonworks sandbox-archief** op de [Hortonworks Sandbox downloads site](https://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) versie 1.8 of hoger](https://aka.ms/azure-jdks). Azure Toolkit voor IntelliJ vereist JDK.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) met de [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) invoegtoepassing en de [Azure Toolkit voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij) invoegtoepassing. HDInsight-hulpprogramma's voor IntelliJ is beschikbaar als onderdeel van de Azure Toolkit voor IntelliJ. 

De invoegtoepassingen installeren:

  1. Open IntelliJ IDEA.
  2. Op de **Welkom** weergeeft, schakelt **configureren**, en selecteer vervolgens **invoegtoepassingen**.
  3. Selecteer in de linkerbenedenhoek **-invoegtoepassing installeren JetBrains**.
  4. De zoekfunctie gebruiken om te zoeken naar **Scala**, en selecteer vervolgens **installeren**.
  5. Als u wilt de installatie hebt voltooid, selecteert u **opnieuw IntelliJ IDEA**.
  6. Herhaal stap 4 en 5 voor het installeren van **Azure Toolkit voor IntelliJ**. Zie voor meer informatie, [Installeer Azure Toolkit voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="create-an-apache-spark-scala-application"></a>Een Apache Spark Scala-toepassing maken

In deze sectie maakt maken u een Scala-voorbeeldproject met behulp van IntelliJ IDEA gebruiken. In de volgende sectie koppelt u IntelliJ IDEA aan de Hortonworks Sandbox (emulator) voordat u het project indient.

1. Open IntelliJ IDEA op uw computer. In de **nieuw Project** dialoogvenster vak, voert u deze stappen uit:

   1. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.
   2. In de **Build-hulpprogramma** lijst, selecteert u een van de volgende, afhankelijk van uw scenario:

      * **Maven**: Voor ondersteuning van Scala maken van het project wizard.
      * **SBT**: Voor het beheren van afhankelijkheden en ontwikkelen voor het Scala-project.

   ![Het dialoogvenster Nieuw project](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Selecteer **Volgende**.
3. In de volgende **nieuw Project** dialoogvenster vak, voer de volgende stappen uit:

   1. In de **projectnaam** voert u de projectnaam van een.
   2. In de **projectlocatie** voert u de projectlocatie van een.
   3. Naast de **Project SDK** vervolgkeuzelijst, selecteer **nieuw**, selecteer **JDK**, en geef vervolgens de map voor Java JDK versie 1.7 of hoger. Selecteer **Java 1.8** voor de Spark-cluster 2.x. Selecteer **Java 1.7** voor de 1.x Spark-cluster. De standaardlocatie is C:\Program Files\Java\jdk1.8.x_xxx.
   4. In de **Spark-versie** vervolgkeuzelijst, de wizard Scala-project maken voor de Spark-SDK en Scala-SDK de juiste versie kan worden geïntegreerd. Selecteer **Spark 1.x** als de Spark-clusterversie ouder is dan 2.0. Selecteer anders **Spark 2.x**. In dit voorbeeld wordt Spark 1.6.2 (Scala 2.10.5). Zorg ervoor dat u van de opslagplaats gemarkeerd gebruikmaakt **Scala 2.10.x**. Gebruik niet de opslagplaats die is gemarkeerd als Scala 2.11.x.
    
      ![IntelliJ Scala eigenschappen-project maken](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)


4. Selecteer **Voltooien**.
5. Als de **Project** weergave nog niet is geopend, drukt u op **Alt + 1** om dit te openen.
6. In **Projectverkenner**, vouw het project en selecteer vervolgens **src**.
7. Met de rechtermuisknop op **src**, wijst u **nieuw**, en selecteer vervolgens **Scala klasse**.
8. In de **naam** vak, voer een naam in. In de **soort** Schakel **Object**. Selecteer vervolgens **OK**.

    ![Het dialoogvenster Nieuwe Scala-klasse maken](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. Plak de volgende code in het bestand .scala:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache.
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. Op de **bouwen** in het menu **Build project**. Zorg ervoor dat de compilatie voltooid is.


## <a name="link-to-the-hortonworks-sandbox"></a>Koppeling naar de Hortonworks Sandbox

Voordat u kunt koppelen aan een Hortonworks Sandbox (emulator), moet u een bestaande IntelliJ toepassing hebben.

Koppeling naar een emulator:

1. Open het project in IntelliJ.
2. Op de **weergave** in het menu **hulpprogramma's voor Windows**, en selecteer vervolgens **Azure Explorer**.
3. Vouw **Azure**, met de rechtermuisknop op **HDInsight**, en selecteer vervolgens **koppelen van een Emulator**.
4. In de **koppeling een nieuwe Emulator** dialoogvenster vak, voer het wachtwoord die u hebt ingesteld voor het root-account van de Hortonworks Sandbox. Voer vervolgens de waarden die vergelijkbaar is met die worden gebruikt in de volgende schermafbeelding. Selecteer vervolgens **OK**. 

   ![De koppeling in het dialoogvenster Nieuwe Emulator](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Selecteer voor het configureren van de emulator, **Ja**.

Wanneer de emulator is verbonden, wordt de emulator (Hortonworks Sandbox) vermeld op het HDInsight-knooppunt.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Verzenden van de Spark Scala-toepassing naar de Hortonworks Sandbox

Nadat u hebt IntelliJ IDEA gekoppeld aan de emulator, kunt u uw project verzenden.

Indienen van een project voor een emulator:

1. In **Projectverkenner**, met de rechtermuisknop op het project en selecteer vervolgens **-toepassing voor Spark verzenden naar HDInsight**.
2. Voltooi de volgende stappen:

    1. In de **Spark-cluster (alleen Linux)** vervolgkeuzelijst, selecteert u uw lokale Hortonworks Sandbox.
    2. In de **de naam van de Main-klasse** selecteert of Voer de naam van de main-klasse. Voor deze zelfstudie is de naam **GroupByTest**.

3. Selecteer **Indienen**. De logboeken van de verzending van taak worden weergegeven in het venster Spark inzending hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [HDInsight hulpprogramma's in Azure Toolkit voor IntelliJ gebruiken voor het maken van Apache Spark-toepassingen voor een HDInsight Spark Linux-cluster](../spark/apache-spark-intellij-tool-plugin.md).

- Zie voor een video over de HDInsight-hulpprogramma's voor IntelliJ [HDInsight-hulpprogramma's voor IntelliJ voor de ontwikkeling van Apache Spark introduceren](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Meer informatie over het [op afstand fouten opsporen in Apache Spark-toepassingen op een HDInsight-cluster met de Azure Toolkit voor IntelliJ via SSH](../spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Meer informatie over het [gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand op een HDInsight Spark Linux-cluster](../spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Meer informatie over het [gebruik HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Apache Spark-toepassingen maken](../spark/apache-spark-eclipse-tool-plugin.md).

- Zie voor een video over de HDInsight-hulpprogramma's voor Eclipse [gebruik HDInsight-hulpprogramma's voor Eclipse om een Spark-toepassingen maken](https://mix.office.com/watch/1rau2mopb6fha).
