---
title: Apache DataFu gebruiken met Apache Pig in HDInsight - Azure
description: Apache DataFu Pig is een verzameling van bibliotheken voor gebruik met Apache Pig op Apache Hadoop. Leer hoe u DataFu kunt gebruiken met Pig op uw HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/16/2018
ms.author: hrasheed
ms.openlocfilehash: 982ed4533c34cd4a6613d289ef4712001977538f
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436208"
---
# <a name="use-apache-datafu-pig-with-apache-pig-on-hdinsight"></a>Apache DataFu Pig gebruiken met Apache Pig in HDInsight

Leer hoe u Apache DataFu Pig gebruiken met HDInsight.

Apache DataFu Pig is een verzameling van Open Source-bibliotheken voor gebruik met Apache Pig op Apache Hadoop.
Zie voor meer informatie over DataFu Pig [ https://datafu.apache.org/ ](https://datafu.apache.org/).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement.

* Een Azure HDInsight-cluster (Linux of op basis van Windows)

  > [!IMPORTANT]  
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een vertrouwd zijn met [met Apache Pig in HDInsight](hdinsight-use-pig.md)

## <a name="install-datafu-on-linux-based-hdinsight"></a>DataFu installeren op Linux gebaseerde HDInsight

> [!IMPORTANT]  
> DataFu is geïnstalleerd op Linux gebaseerde clusters versie 3.3 en hoger, en op Windows-clusters. Het is niet geïnstalleerd op Linux gebaseerde clusters ouder dan 3.3.
>
> Als u een cluster op basis van Windows, of een hogere versie 3.3 op basis van Linux cluster gebruikt, moet u deze sectie overslaan.

DataFu kan worden gedownload en geïnstalleerd vanuit de Maven-opslagplaats. Gebruik de volgende stappen uit om versie u nodig hebt en deze toevoegen aan uw HDInsight-cluster te bekijken:

> [!WARNING]  
> DataFu versies hebben mogelijk vereisten die niet wordt voldaan door HDInsight. Als u een oudere versie van DataFu gebruikt, kan er bijvoorbeeld een andere versie van Pig dan wat is inbegrepen bij HDInsight vereist.

### <a name="find-a-version"></a>Een versie vinden

1. Navigeer in uw webbrowser naar https://mvnrepository.com/artifact/org.apache.datafu/datafu-pig en zien welke versie u nodig hebt.

2. Selecteer het gekoppelde versienummer.

3. Selecteer __weergeven van alle__ om alle bestanden weer te geven.

4. In de lijst met bestanden, het JAR-bestand niet vinden. Meestal is dit bestand de grootste weergegeven, zoals het alle afhankelijkheden bevat. Met de rechtermuisknop op de koppeling en het koppelingsadres kopiëren.

### <a name="download-datafu-to-hdinsight"></a>DataFu naar HDInsight downloaden

1. Verbinding maken met uw HDInsight op basis van Linux-cluster via SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdracht voor het downloaden van het gebruik van het hulpprogramma wget DataFu jar-bestand:

    > [!IMPORTANT]  
    > Vervang de koppeling in de opdracht door de URL die u eerder hebt gekopieerd.

    ```
    wget https://central.maven.org/maven2/org/apache/datafu/datafu-pig/1.4.0/datafu-pig-1.4.0.jar
    ```

3. Het bestand vervolgens uploaden naar de standaardopslag voor uw HDInsight-cluster. Plaatsen van het bestand in standaard maakt opslag ze beschikbaar voor alle knooppunten in het cluster.

    > [!IMPORTANT]  
    > Het versienummer in de naam van het bestand vervangen door de versie die u hebt gedownload.

    ```
    hdfs dfs -put datafu-pig-1.4.0.jar /example/jars
    ```

    > [!NOTE]  
    > De vorige opdracht slaat de jar in `/example/jars` omdat deze map al in de clusteropslag bestaat. U kunt een locatie die u wilt dat op de opslag voor HDInsight-cluster gebruiken.

## <a name="use-datafu-with-pig"></a>DataFu gebruiken met Pig

De stappen in deze sectie wordt ervan uitgegaan dat u bekend bent met Pig gebruiken op HDInsight. Zie voor meer informatie over het gebruik Pig met HDInsight [Pig gebruiken met HDInsight](hdinsight-use-pig.md).

> [!IMPORTANT]  
> Als u handmatig DataFu met behulp van de stappen in de vorige sectie hebt geïnstalleerd, moet u het registreren voordat u deze gebruikt.
>
> * Als uw cluster gebruikmaakt van Azure Storage, gebruikt u een `wasb://` pad. Bijvoorbeeld `register wasb:///example/jars/datafu-pig-1.4.0.jar`.
>
> * Als uw cluster maakt gebruik van Azure Data Lake Store, gebruikt u een `adl://` pad. Bijvoorbeeld `register adl://home/example/jars/datafu-pig-1.4.0.jar`.

Vaak definieert u een alias voor DataFu functies. Het volgende voorbeeld definieert een alias van `SHA`:

```piglatin
DEFINE SHA datafu.pig.hash.SHA();
```

U kunt vervolgens deze alias gebruiken in een Pig Latin-script voor het genereren van een hash van de ingevoerde gegevens. De volgende code wordt de locatie in de ingevoerde gegevens bijvoorbeeld vervangen door een hash-waarde:

```piglatin
raw = LOAD '/HdiSamples/HdiSamples/SensorSampleData/building/building.csv' USING
    org.apache.pig.piggybank.storage.CSVExcelStorage(',', 'NO_MULTILINE', 'UNIX', 'SKIP_INPUT_HEADER') AS
    (int1:int,
     id1:chararray,
     int2:int,
     id2:chararray,
     location:chararray);
mask = FOREACH raw GENERATE int1, id1, int2, id2, SHA(location);
DUMP mask;
```

De volgende uitvoer wordt gegenereerd:

    (1,M1,25,AC1000,aa5ab35a9174c2062b7f7697b33fafe5ce404cf5fecf6bfbbf0dc96ba0d90046)
    (2,M2,27,FN39TG,7a1ca4ef7515f7276bae7230545829c27810c9d9e98ab2c06066bee6270d5153)
    (3,M3,28,JDNS77,07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)
    (4,M4,17,GG1919,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (5,M5,3,ACMAX22,1ed8c7e56c947bebc0cfcf88c4ea0f02c944568f71df893a99970e4f0c78cddc)
    (6,M6,9,AC1000,c96dd81db196cca5f57bd4270bbb9d9e9d1b242d67f9364005ee1dfdc2632523)
    (7,M7,13,FN39TG,3e049d78d958038ac6bd5dcf038075cc73362b4956aaf7308c3a69c8eca76297)
    (8,M8,25,JDNS77,c1ef40ce0484c698eb4bd27fe56c1e7b68d74f9780ed674210d0e5013dae45e9)
    (9,M9,11,GG1919,a7d355b26bda6bf1196ccffead0b2cf2b81f0a9de5b4876b44407f1dc07e51e6)
    (10,M10,23,ACMAX22,10436829032f361a3de50048de41755140e581467bc1895e6c1a17f423e42d10)
    (11,M11,14,AC1000,348841ef53dbd5a64008a86be432973db790774fb28b59b0d99702a3188b3705)
    (12,M12,26,FN39TG,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (13,M13,25,JDNS77,ed9ad13611d7164839dd3feaf9a7f6a75a4138f389e0d45f8e07fa38da1116a2)
    (14,M14,17,GG1919,80db4ccdca106d37b920206331fcfe3e9e50a9e763d89b54ce3ad5ac8cf30f03)
    (15,M15,19,ACMAX22,3552ac0c032467fbf592cb4d10c5c9267800c01e343ad8ca557256d882ae9327)
    (16,M16,23,AC1000,07c67d76ef92ac9853588e098983fefba4ba5965f8fec95f42ab0d04c27865ba)
    (17,M17,11,FN39TG,557c1599d9a04895d3817d293e0806a4419a14de31958386182798d0d2ed3a56)
    (18,M18,25,JDNS77,dbc74a36d8e7439c45c64d856388506cc9b1218619cef979c3d605115a7a4546)
    (19,M19,14,GG1919,be55ef3f4c4e6c2d9c2afe2a33ac90ad0f50d4de7f9163999877e2a9ca5a54f8)
    (20,M20,19,ACMAX22,ea0b937ea317101ee2c26b03a4843a19ceced8a2b9673c3cf409a726ca2b0fd8)

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over DataFu of Pig:

* [Apache DataFu Pig aan de slag](https://datafu.apache.org/docs/datafu/getting-started.html).
* [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)
