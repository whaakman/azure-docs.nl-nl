---
title: Gebruik Azure Toolkit voor IntelliJ met Hortonworks Sandbox | Microsoft Docs
description: Informatie over het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ met Hortonworks Sandbox.
keywords: hadoop-hulpprogramma's, hive-query, intellij, hortonworks sandbox, azure toolkit voor intellij
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 6bb29dc4e231bc859be620e56a606fbbfade102b
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox

Informatie over het gebruik van HDInsight Tools for IntelliJ Apache Scala-toepassingen te ontwikkelen en vervolgens de toepassingen te testen op [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) op uw computer uitgevoerd. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) is van een Java integrated development environment (IDE) voor het ontwikkelen van computersoftware. Nadat u ontwikkelen en testen van uw toepassingen op de Hortonworks Sandbox, kunt u de toepassingen naar [Azure HDInsight](apache-hadoop-introduction.md).

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

- Hortonworks Data Platform HDP () 2.4 op de Hortonworks Sandbox op uw lokale computer. Als u HDP instelt, Zie [aan de slag in de Hadoop-ecosysteem aan een Hadoop-sandbox op een virtuele machine](apache-hadoop-emulator-get-started.md). 
    > [!NOTE]
    > HDInsight Tools for IntelliJ is getest met de HDP 2.4. Als u HDP 2.4, vouw **Hortonworks Sandbox archief** op de [Hortonworks Sandbox site downloadt](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) versie 1.8 of hoger](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Azure Toolkit voor IntelliJ vereist JDK.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) met de [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) invoegtoepassing en de [Azure Toolkit voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij) invoegtoepassing. HDInsight Tools for IntelliJ is beschikbaar als onderdeel van Azure Toolkit voor IntelliJ. 

De invoegtoepassingen installeren:

  1. Open IntelliJ IDEA.
  2. Op de **Welkom** pagina **configureren**, en selecteer vervolgens **Plugins**.
  3. Selecteer in de linkerbenedenhoek **-invoegtoepassing installeren JetBrains**.
  4. Gebruik de zoekfunctie om te zoeken naar **Scala**, en selecteer vervolgens **installeren**.
  5. Selecteer om de installatie, **opnieuw IntelliJ IDEA**.
  6. Herhaal stap 4 en 5 voor het installeren van **Azure Toolkit voor IntelliJ**. Zie voor meer informatie [Azure Toolkit installeren voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="create-a-spark-scala-application"></a>Een Spark Scala-toepassing maken

In deze sectie maakt u een voorbeeldproject Scala via IntelliJ IDEA. In de volgende sectie koppelt u IntelliJ IDEA aan de Hortonworks Sandbox (emulator) voordat u het project indienen.

1. Open IntelliJ IDEA op uw computer. In de **nieuw Project** dialoogvenster Vervolledig de volgende stappen uit:

   1. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.
   2. In de **Build hulpprogramma** lijst, selecteert u een van de volgende, op basis van uw scenario:

    * **Maven**: Scala voor maken van het project wizard-ondersteuning.
    * **SBT**: voor het beheren van afhankelijkheden en bouwen voor het project Scala.

   ![Het dialoogvenster Nieuw Project](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Selecteer **volgende**.
3. In de volgende **nieuw Project** dialoogvenster Vervolledig de volgende stappen uit:

    1. In de **projectnaam** Voer de naam van het project.
    2. In de **projectlocatie** Geef de projectlocatie van een.
    3. Naast de **Project SDK** vervolgkeuzelijst, selecteer **nieuw**, selecteer **JDK**, en geef vervolgens de map voor Java JDK 1,7 of hoger. Selecteer **Java 1.8** voor de 2.x Spark-cluster. Selecteer **Java 1.7** voor de 1.x Spark-cluster. De standaardlocatie C:\Program Files\Java\jdk1.8.x_xxx is.
    4. In de **Spark versie** vervolgkeuzelijst, de wizard Scala project maken voor de Spark-SDK en Scala SDK de juiste versie kan worden geïntegreerd. Als de Spark-cluster versie ouder dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **Spark2.x**. In dit voorbeeld maakt gebruik van Spark 1.6.2 (Scala 2.10.5). Zorg ervoor dat u van de opslagplaats gemarkeerd gebruikmaakt **Scala 2.10.x**. Gebruik niet de opslagplaats gemarkeerd Scala 2.11.x.
    
    ![IntelliJ Scala eigenschappen-project maken](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)


4. Selecteer **Voltooien**.
5. Als de **Project** weergave nog niet is geopend, drukt u op **Alt + 1** om dit te openen.
6. In **Projectverkenner**, vouwt u het project en selecteer vervolgens **src**.
7. Met de rechtermuisknop op **src**, wijs **nieuw**, en selecteer vervolgens **Scala klasse**.
8. In de **naam** Voer een naam. In de **soort** de optie **Object**. Selecteer **OK**.

    ![Het dialoogvenster Nieuwe Scala klasse maken](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

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

10. Op de **bouwen** selecteert u **Build project**. Zorg ervoor dat de compilatie met succes voltooid wordt.


## <a name="link-to-the-hortonworks-sandbox"></a>Koppeling naar de Sandbox Hortonworks

Voordat u kunt koppelen aan een Sandbox met Hortonworks (emulator), moet u een bestaande IntelliJ toepassing hebben.

Om te koppelen aan een emulator:

1. Open het project in IntelliJ.
2. Op de **weergave** selecteert u **'s Windows**, en selecteer vervolgens **Azure Explorer**.
3. Vouw **Azure**, met de rechtermuisknop op **HDInsight**, en selecteer vervolgens **koppelen van een Emulator**.
4. In de **Link A nieuwe Emulator** dialoogvenster en voer het wachtwoord dat u hebt ingesteld voor het hoofdaccount van de Hortonworks Sandbox. Geef vervolgens waarden die overeenkomen met die in de volgende schermafbeelding. Selecteer **OK**. 

   ![De koppeling in het dialoogvenster Nieuwe Emulator](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Selecteer voor het configureren van de emulator **Ja**.

Wanneer de emulator is verbonden, wordt de emulator (Hortonworks Sandbox) weergegeven op het HDInsight-knooppunt.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>De toepassing Spark Scala aan de Hortonworks Sandbox verzenden

Nadat u hebt IntelliJ IDEA gekoppeld aan de emulator, kunt u uw project verzenden.

Een project om een emulator te verzenden:

1. In **Projectverkenner**, met de rechtermuisknop op het project en selecteer vervolgens **toepassing op HDInsight Spark indienen**.
2. De volgende stappen uitvoeren:

    1. In de **Spark-cluster (alleen voor Linux)** vervolgkeuzelijst, selecteert u uw lokale Hortonworks Sandbox.
    2. In de **Main klassenaam** vak Selecteer of typ de naam van de belangrijkste klasse. Voor deze zelfstudie is de naam **GroupByTest**.

3. Selecteer **indienen**. De logboeken van de verzending van taak worden weergegeven in het venster Spark verzending hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [gebruik van HDInsight Tools Spark om toepassingen te maken voor een HDInsight Spark Linux-cluster in Azure Toolkit voor IntelliJ](../spark/apache-spark-intellij-tool-plugin.md).

- Zie voor een video over HDInsight Tools for IntelliJ [introduceren HDInsight Tools voor IntelliJ voor het ontwikkelen van Spark](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Meer informatie over hoe [op afstand fouten opsporen in Spark scala-toepassingen op een HDInsight-cluster in Azure werkset voor IntelliJ via SSH](../spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Meer informatie over hoe [HDInsight Tools in Azure Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand op een HDInsight Spark Linux-cluster](../spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Meer informatie over hoe [gebruik HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](../spark/apache-spark-eclipse-tool-plugin.md).

- Zie voor een video over HDInsight Tools voor Eclipse [gebruik HDInsight Tools voor Eclipse Spark-toepassingen maken](https://mix.office.com/watch/1rau2mopb6fha).
