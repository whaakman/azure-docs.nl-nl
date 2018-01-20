---
title: Uitvoeren van aangepaste programma's van MapReduce - Azure HDInsight | Microsoft Docs
description: Wanneer en hoe aangepaste MapReduce-programma's uitvoeren in HDInsight.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: 8e65c946d2cfcc830a1b9fa59b3f7886857f4f7d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="run-custom-mapreduce-programs"></a>Aangepaste MapReduce-programma's uitvoeren

Op basis van Hadoop big-datasystemen zoals HDInsight inschakelen met behulp van een breed scala aan hulpprogramma's en technologieën gegevensverwerking. De volgende tabel beschrijft de belangrijkste voordelen en overwegingen voor elk adres.

| Query-mechanisme | Voordelen | Overwegingen |
| --- | --- | --- |
| **Hive HiveQL gebruiken** | <ul><li>Een uitstekende oplossing voor batchverwerking en analyseren van grote hoeveelheden niet-wijzigbaar gegevens, voor gegevenssamenvatting en voor op aanvraag opvragen. Een bekende SQL-achtige syntaxis wordt gebruikt.</li><li>Het kan worden gebruikt voor het produceren van permanente gegevenstabellen die eenvoudig kunnen worden gepartitioneerd en geïndexeerd.</li><li>Meerdere externe tabellen en weergaven kunnen worden gemaakt via dezelfde gegevens.</li><li>Biedt ondersteuning voor een eenvoudige datawarehouse-implementatie waarbij grote scale-out en fouttolerantie voor verwerking en opslag van gegevens biedt.</li></ul> | <ul><li>Hiervoor moeten de brongegevens te hebben van ten minste sommige persoonsgegevens structuur.</li><li>Het is niet geschikt is voor realtime query's en rij niveau updates. Het is best gebruikt voor batchtaken via grote gegevenssets.</li><li>Het niet mogelijk bij het uitvoeren van bepaalde typen complexe taken.</li></ul> |
| **Pig Latin voor Pig met** | <ul><li>Een uitstekende oplossing voor het bewerken van gegevens, zoals ingesteld, samenvoegen en filteren van gegevenssets, functies wilt toepassen op records of groepen van records, en voor reorganisatie van gegevens met het definiëren van kolommen met groeperingswaarden, of door het omzetten van kolommen in rijen.</li><li>Kan gebruikmaken van een werkstroom gebaseerde benadering als een reeks bewerkingen op gegevens.</li></ul> | <ul><li>SQL-gebruikers kunnen vinden dat pig Latin is minder bekende en moeilijker te gebruiken dan HiveQL.</li><li>De standaarduitvoer kan is meestal een tekstbestand en dus moeilijker te gebruiken met visualisatie hulpprogramma's zoals Excel. Doorgaans wordt u een Hive-tabel via de uitvoer van de laag.</li></ul> |
| **Aangepaste kaart/verkleinen** | <ul><li>Het volledige controle over de kaart bevat en minder fasen en worden uitgevoerd.</li><li>Kunt u query's optimaliseren als u de maximale prestaties uit het cluster of de belasting van de servers en het netwerk te minimaliseren.</li><li>De onderdelen kunnen worden geschreven in een bereik van bekende talen.</li></ul> | <ul><li>Er is het moeilijker dan het gebruik van Pig- of Hive omdat moet u uw eigen map maken en onderdelen te verminderen.</li><li>Processen waarvoor lid te worden van gegevenssets zijn moeilijker te implementeren.</li><li>Hoewel er test frameworks beschikbaar zijn, is foutopsporing complexer dan een normale toepassing omdat de code wordt uitgevoerd als een batch-job onder het beheer van de Hadoop-Taakplanner.</li></ul> |
| **HCatalog** | <ul><li>Deze isoleert de details van het pad van opslag, waardoor het beheer eenvoudiger en verwijderen van de noodzaak van gebruikers om te weten waar de gegevens worden opgeslagen.</li><li>Hiermee kunt meldingen van gebeurtenissen, zoals de beschikbaarheid van gegevens, zodat andere hulpprogramma's zoals Oozie om te detecteren wanneer bewerkingen hebben plaatsgevonden.</li><li>Het beschrijft een relationele weergave van gegevens, inclusief partitioneren door sleutel, en kunt de gegevens gemakkelijk toegang.</li></ul> | <ul><li>Deze ondersteuning biedt voor RCFile, CSV-tekst, JSON-tekst, SequenceFile en ORC bestandsindelingen standaard, maar mogelijk moet u een aangepaste serde-schrijfbewerking voor andere indelingen met schrijven.</li><li>HCatalog is niet thread-safe.</li><li>Er zijn enkele beperkingen voor de gegevenstypen voor kolommen bij gebruik van de lader HCatalog in Pig-scripts. Zie voor meer informatie [HCatLoader gegevenstypen](http://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) in de documentatie van Apache HCatalog.</li></ul> |

Meestal gebruikt u de meest eenvoudige van deze methoden waarmee de resultaten die u nodig hebt. Bijvoorbeeld, u mogelijk dergelijke resultaten bereiken met NET Hive, maar complexere scenario's moet u wellicht om Pig, gebruiken of zelfs uw eigen map schrijven en onderdelen te verminderen. U kunt ook bepalen nadat het experimenteren met Hive of Pig, die aangepaste zijn toegewezen en verminderen onderdelen zorgen voor betere prestaties zodat u kunt aanpassen en de verwerking te optimaliseren.

## <a name="custom-mapreduce-components"></a>Aangepaste kaart/verminderen onderdelen

Kaart/verminderen code bestaat uit twee afzonderlijke functies die worden geïmplementeerd als **kaart** en **verminderen** onderdelen. De **kaart** onderdeel is parallel worden uitgevoerd op meerdere clusterknooppunten elk knooppunt dat de toewijzing wordt toegepast op de subset van het knooppunt van de gegevens. De **verminderen** onderdeel worden gesorteerd en bevat een overzicht van de resultaten van alle functies van de kaart. Zie voor meer informatie over deze twee componenten, [gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

Het is eenvoudiger en efficiënter gebruik van een hoger niveau abstractie zoals Pig of Hive in de meeste scenario's voor HDInsight-verwerking. U kunt ook aangepaste map maken en verminderen van onderdelen voor gebruik in scripts voor Hive meer geavanceerde processen uitvoeren.

Aangepaste kaart/verminderen onderdelen zijn meestal geschreven in Java. Hadoop biedt een streaming-interface waarmee ook onderdelen moet worden gebruikt die zijn ontwikkeld in andere talen zoals C#, F #, Visual Basic, Python en JavaScript.

* Zie voor een overzicht over het ontwikkelen van aangepaste Java-MapReduce-programma's [ontwikkelen van Java-MapReduce-programma's voor Hadoop op HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md).
* Zie voor een voorbeeld met behulp van Python [ontwikkelen Python streaming MapReduce-programma's voor HDInsight](apache-hadoop-streaming-python.md).

Houd rekening met het maken van uw eigen toewijzing en beperken van onderdelen voor de volgende voorwaarden:

* U moet volledig ongestructureerde gegevens door te parseren van de gegevens en aangepaste regels te verkrijgen gestructureerde gegevens verwerken.
* U wilt complexe taken uitvoeren die moeilijk (of onmogelijk) om uit te drukken in Pig of Hive zonder een UDF maken. U wilt bijvoorbeeld een externe geocodering-service gebruiken breedtegraad en lengtegraad coördinaten of IP-adressen in de brongegevens converteren naar namen van de geografische locatie.
* Wilt u uw bestaande .NET, Python of JavaScript-code in kaart/verminderen onderdelen opnieuw wilt gebruiken met behulp van de Hadoop-streaming interface.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Uploaden en uw aangepaste MapReduce-programma uitvoeren

De meest voorkomende MapReduce-programma's zijn geschreven in Java en gecompileerd naar een jar-bestand.

1. Nadat u hebt ontwikkeld, gecompileerd en uw programma MapReduce getest, gebruikt u de `scp` opdracht jar-bestand uploaden naar de headnode.

    ```bash
    scp mycustomprogram.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang **gebruikersnaam** met het SSH-gebruikersaccount voor uw cluster. Vervang **CLUSTERNAME** met de naam van het cluster. Als u een wachtwoord gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord invoeren. Als u een certificaat gebruikt, moet u mogelijk gebruik van de `-i` parameter om een bestand met de persoonlijke sleutel.

2. Verbinding maken met behulp van de cluster [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Uitvoeren van de SSH-sessie de MapReduce-programma via YARN.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Met deze opdracht verzonden door de MapReduce-taak voor het YARN. Het invoerbestand is `/example/data/sample.log`, en de uitvoermap is `/example/data/logoutput`. Het bestand voor invoer en uitvoerbestanden worden opgeslagen naar de opslag van de standaard voor het cluster.

## <a name="next-steps"></a>Volgende stappen

* [Gebruik C# met MapReduce streaming van Hadoop in HDInsight](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Het ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Python streaming MapReduce-programma's voor HDInsight ontwikkelen](apache-hadoop-streaming-python.md)
* [Gebruik van Azure Toolkit voor Eclipse Spark-toepassingen voor een HDInsight-cluster maken](../spark/apache-spark-eclipse-tool-plugin.md)
* [Gebruik Python gebruiker gedefinieerde functies (UDF) met Hive en Pig in HDInsight](python-udf-hdinsight.md)
