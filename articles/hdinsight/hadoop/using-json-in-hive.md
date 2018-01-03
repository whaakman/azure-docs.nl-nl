---
title: Analyseren en verwerken van JSON-documenten met Apache Hive in Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van JSON-documenten en analyseer ze met behulp van apache Hive in Azure HDInsight
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 96faa0cf518e4b02b5ba9fd8aedaf21bd6288b02
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="process-and-analyze-json-documents-by-using-apache-hive-in-azure-hdinsight"></a>Verwerken en analyseren van JSON-documenten met behulp van Apache Hive in Azure HDInsight

Informatie over het verwerken en analyseren van JSON JavaScript Object Notation ()-bestanden met behulp van Apache Hive in Azure HDInsight. Deze zelfstudie maakt gebruik van de volgende JSON-document:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
                    ]
    }

Het bestand kan worden gevonden op  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Zie voor meer informatie over het gebruik van Azure Blob storage met HDInsight [gebruik HDFS-compatibele Azure Blob storage met Hadoop in HDInsight](../hdinsight-hadoop-use-blob-storage.md). U kunt het bestand kopiëren naar de standaardcontainer van uw cluster.

In deze zelfstudie gebruikt u de Hive-console. Zie voor instructies voor het openen van de console Hive [Hive gebruiken met Hadoop op HDInsight met extern bureaublad](apache-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>JSON-documenten plat
De methoden die in de volgende sectie vereisen dat het JSON-document bestaat uit één rij. U moet dus het JSON-document naar een tekenreeks afvlakken. Als uw JSON-document al afgevlakt is, kunt u deze stap overslaan en meteen naar de volgende sectie gaat over het analyseren van JSON-gegevens. Als u wilt het JSON-document afvlakken, voer het volgende script:

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

De onbewerkte JSON-bestand bevindt zich op  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . De **StudentsRaw** Hive-tabel verwijst naar de onbewerkte JSON-document dat niet plat is gemaakt.

De **StudentsOneLine** Hive-tabel de gegevens worden opgeslagen in het standaardbestandssysteem voor HDInsight onder de **/json/studenten/** pad.

De **invoegen** instructie vult de **StudentOneLine** tabel met de platte JSON-gegevens.

De **Selecteer** instructie retourneert slechts één rij.

Dit is de uitvoer van de **Selecteer** instructie:

![Het JSON-document plat][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>JSON-documenten in Hive analyseren
Hive biedt drie verschillende mechanismen voor het uitvoeren van query's op JSON-documenten, of kunt u uw eigen:

* Gebruik de get_json_object gebruiker gedefinieerde functie (UDF).
* Gebruik de json_tuple UDF.
* Gebruik de aangepaste serialisatiefunctie/Deserializer (serde-schrijfbewerking).
* Uw eigen UDF schrijven met behulp van Python of andere talen. Zie voor meer informatie over het uitvoeren van uw eigen Python-code met Hive [Python UDF met Apache Hive en Pig][hdinsight-python].

### <a name="use-the-getjsonobject-udf"></a>Gebruik de get_json_object UDF
Hive biedt een ingebouwde UDF aangeroepen [get_json_object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) die JSON opvragen tijdens runtime kunt uitvoeren. Deze methode heeft twee argumenten--de tabelnaam en de methodenaam waarvoor de platte JSON-document en het JSON-veld dat moet worden geparseerd. Bekijk een voorbeeld te zien hoe deze UDF werkt.

De volgende query retourneert de voornaam en achternaam op voor elke student:

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Dit is de uitvoer wanneer u deze query in het consolevenster uitvoert:

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Er zijn beperkingen van de get_json_object UDF:

* Omdat elk veld in de query reparsing van de query is vereist, maar dit invloed op de prestaties.
* **OPHALEN van\_JSON_OBJECT()** de tekenreeksweergave van een matrix retourneert. Als u wilt deze matrix converteren naar een Hive-matrix, hebt u reguliere expressies ter vervanging van de vierkante haken "[' en '] ', en ook hebt u aan te roepen gesplitste om op te halen van de matrix.

Dit is de reden waarom de Hive-wiki adviseert json_tuple te gebruiken.  

### <a name="use-the-jsontuple-udf"></a>Gebruik de json_tuple UDF
Een andere UDF geleverd door Hive heet [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), uitvoert, wordt er beter dan [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Deze methode heeft een set van sleutels en een JSON-tekenreeks en retourneert een tuple van waarden met een functie. De volgende query retourneert de ID van de studenten en de kwaliteit van het JSON-document:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

De uitvoer van dit script in de Hive-console:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

De json_tuple UDF gebruikt de [lateral weergave](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaxis in Hive, waardoor json\_tuple voor het maken van een virtuele tabel door de functie UDT toepassen op elke rij van de oorspronkelijke tabel. Complexe JSONs onhandig te vanwege herhaald gebruik van **LATERALE weergave**. Bovendien **JSON_TUPLE** geneste JSONs kan niet worden verwerkt.

### <a name="use-a-custom-serde"></a>Gebruik een aangepaste serde-schrijfbewerking
Serde-schrijfbewerking is de beste keuze voor het parseren van geneste JSON-documenten. Hiermee kunt u de JSON-schema definiëren en vervolgens kunt u het schema voor het parseren van de documenten. Zie voor instructies [het gebruik van een aangepaste JSON serde-schrijfbewerking met Microsoft Azure HDInsight](https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/).

## <a name="summary"></a>Samenvatting
Het type van JSON-operator in component die u kiest is Kortom, afhankelijk van uw scenario. Als u een eenvoudige JSON-document hebt en u slechts één veld opzoeken op hebt, kunt u de get_json_object UDF Hive gebruiken. Als u meer dan één sleutel opzoeken op hebt, kunt u json_tuple kunt gebruiken. Als u een geneste document hebt, moet u de JSON-serde-schrijfbewerking gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie voor verwante artikelen:

* [Hive en HiveQL gebruiken met Hadoop in HDInsight voor het analyseren van een voorbeeldbestand van de Apache-log4j](../hdinsight-use-hive.md)
* [Vertraging vluchtgegevens analyseren met behulp van Hive in HDInsight](../hdinsight-analyze-flight-delay-data.md)
* [Twitter-gegevens analyseren met behulp van Hive in HDInsight](../hdinsight-analyze-twitter-data.md)
* [Een Hadoop-taak uitvoert met behulp van Azure DB die Cosmos en HDInsight](../../cosmos-db/run-hadoop-with-hdinsight.md)

[hdinsight-python]:python-udf-hdinsight.md

[image-hdi-hivejson-flatten]: ./media/using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png

