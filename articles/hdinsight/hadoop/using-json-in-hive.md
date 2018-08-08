---
title: Analyseren en verwerken van JSON-documenten met Apache Hive in Azure HDInsight
description: Meer informatie over het gebruik van JSON-documenten en analyseren met behulp van apache Hive in Azure HDInsight
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: fc0eb7e474e7126e850b028b547cc1db0254fb02
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593500"
---
# <a name="process-and-analyze-json-documents-by-using-apache-hive-in-azure-hdinsight"></a>Verwerken en analyseren van JSON-documenten met behulp van Apache Hive in Azure HDInsight

Informatie over het verwerken en analyseren van JavaScript Object Notation (JSON)-bestanden met behulp van Apache Hive in Azure HDInsight. Deze zelfstudie wordt gebruikgemaakt van de volgende JSON-document:

```json
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
    }
  ]
}
```

Het bestand kan worden gevonden op **wasb://processjson@hditutorialdata.blob.core.windows.net/**. Zie voor meer informatie over het gebruik van Azure Blob storage met HDInsight [gebruik HDFS-compatibele Azure Blob-opslag met Hadoop in HDInsight](../hdinsight-hadoop-use-blob-storage.md). U kunt het bestand kopiëren naar de standaardcontainer van uw cluster.

In deze zelfstudie gebruikt u de Hive-console. Zie voor instructies over het openen van de Hive-console, [Hive gebruiken met Hadoop op HDInsight met extern bureaublad](apache-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>JSON-documenten plat
De methoden die worden vermeld in de volgende sectie is vereist dat de JSON-document bestaan uit een enkele rij. Daarom moet u het JSON-document naar een tekenreeks afvlakken. Als uw JSON-document al afgevlakt is, kunt u deze stap overslaan en meteen naar de volgende sectie gaat over het analyseren van JSON-gegevens. Als u wilt het JSON-document afvlakken, voer het volgende script:

```sql
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
```

De onbewerkte JSON-bestand bevindt zich op **wasb://processjson@hditutorialdata.blob.core.windows.net/**. De **StudentsRaw** Hive-tabel verwijst naar de onbewerkte JSON-document dat niet wordt afgevlakt.

De **StudentsOneLine** Hive-tabel de gegevens worden opgeslagen in het HDInsight-standaardbestandssysteem onder de **/json/studenten/** pad.

De **invoegen** instructie vult de **StudentOneLine** tabel met de platte JSON-gegevens.

De **Selecteer** instructie slechts één rij wordt geretourneerd.

Hier volgt de uitvoer van de **Selecteer** instructie:

![Het JSON-document maken][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>JSON-documenten in Hive analyseren
Hive biedt drie verschillende mechanismen voor het uitvoeren van query's op JSON-documenten, of schrijf uw eigen:

* Gebruik de get_json_object gebruiker gedefinieerde functie (UDF's).
* Gebruik de json_tuple UDF.
* Gebruik de aangepaste Serializer/Deserializer (serde-schrijfbewerking).
* Schrijf uw eigen UDF met behulp van Python of andere talen. Zie voor meer informatie over het uitvoeren van uw eigen code Python met Hive [Python-UDF met Apache Hive en Pig][hdinsight-python].

### <a name="use-the-getjsonobject-udf"></a>Gebruik de get_json_object UDF
Hive biedt een ingebouwde UDF met de naam [get_json_object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) die JSON query's uitvoeren tijdens runtime kunt uitvoeren. Deze methode neemt twee argumenten--de tabelnaam en de naam van de methode, met de platte JSON-document en het JSON-veld dat moet worden geparseerd. We bekijken een voorbeeld te zien hoe deze UDF werkt.

De volgende query retourneert de voornaam en achternaam op voor elke student:

```sql
SELECT
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
FROM StudentsOneLine;
```

Hier volgt de uitvoer wanneer u deze query in het consolevenster uitvoert:

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Er zijn beperkingen van de get_json_object UDF:

* Omdat elk veld in de query is vereist reparsing van de query, het is van invloed op de prestaties.
* **OPHALEN\_JSON_OBJECT()** retourneert de tekenreeksweergave van een matrix. Als u wilt deze matrix converteren naar een Hive-matrix, moet u reguliere expressies gebruiken om te vervangen door de vierkante haken "[' en '] ', en vervolgens u ook hebt om aan te roepen splitsen om op te halen van de matrix.

Dit is de reden waarom de Hive-wiki raadt u json_tuple te gebruiken.  

### <a name="use-the-jsontuple-udf"></a>Gebruik de json_tuple UDF
Een andere UDF geleverd door Hive heet [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), uitvoert, wordt er beter dan [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Deze methode wordt een set van sleutels en een JSON-tekenreeks en retourneert een tuple van waarden met behulp van een functie. De volgende query retourneert de student-ID en de kwaliteit van het JSON-document:

```sql
SELECT q1.StudentId, q1.Grade
FROM StudentsOneLine jt
LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
  AS StudentId, Grade;
```

De uitvoer van dit script in de Hive-console:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

De json_tuple UDF maakt gebruik van de [weergave laterale](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) -syntaxis in Hive, waarmee json\_tuple een virtuele-tabel maken door de functie UDT toepassen op elke rij van de oorspronkelijke tabel. Complexe onhandig te vanwege het herhaaldelijk gebruik van de betreffende JSON's **LATERALE weergave**. Bovendien **JSON_TUPLE** geneste betreffende JSON's kan niet worden verwerkt.

### <a name="use-a-custom-serde"></a>Gebruik een aangepaste serde-schrijfbewerking
Serde-schrijfbewerking is de beste keuze voor het parseren van geneste JSON-documenten. Hiermee kunt u het JSON-schema definiëren en kunt u het schema gebruiken voor het parseren van de documenten. Zie voor instructies [over het gebruik van een aangepaste JSON serde-schrijfbewerking met Microsoft Azure HDInsight](https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/).

## <a name="summary"></a>Samenvatting
Het type van de JSON-operator in Hive die u kiest is Kortom, afhankelijk van uw scenario. Als u een eenvoudige JSON-document hebt en u slechts één veld hebt op opzoeken, kunt u de get_json_object UDF Hive gebruiken. Als u meer dan één sleutel om te zoeken op hebt, kunt u json_tuple gebruiken. Als u een geneste document hebt, moet u de JSON-serde-schrijfbewerking gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie voor verwante artikelen:

* [Hive en HiveQL gebruiken met Hadoop in HDInsight kunt u een voorbeeldbestand van de Apache-log4j analyseren](../hdinsight-use-hive.md)
* [Gegevens over vertraagde vluchten analyseren met behulp van Hive in HDInsight](../hdinsight-analyze-flight-delay-data.md)
* [Twitter-gegevens analyseren met behulp van Hive in HDInsight](../hdinsight-analyze-twitter-data.md)

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

