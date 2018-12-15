---
title: Uitvoeren van aangepaste MapReduce-programma's - Azure HDInsight
description: Wanneer en hoe u aangepaste MapReduce-programma's uitvoeren in HDInsight.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: c9cbea913a86a681620eea4adc0a5c99cc84f920
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53433862"
---
# <a name="run-custom-mapreduce-programs"></a>Aangepaste MapReduce-programma's uitvoeren

Apache Hadoop gebaseerde big data-systemen, zoals HDInsight inschakelen gegevens verwerken met behulp van een breed scala aan hulpprogramma's en technologieën. De volgende tabel beschrijft de belangrijkste voordelen en overwegingen voor elk adres.

| Query-mechanisme | Voordelen | Overwegingen |
| --- | --- | --- |
| **Apache Hive HiveQL gebruiken** | <ul><li>Een uitstekende oplossing voor batch-verwerking en analyse van grote hoeveelheden gegevens onveranderbare, overzichten, en voor op aanvraag uitvoeren van query's. Het maakt gebruik van een bekende SQL-achtige syntaxis.</li><li>Het kan worden gebruikt voor het produceren van permanente tabellen met gegevens die eenvoudig kunnen worden gepartitioneerd en geïndexeerd.</li><li>Meerdere externe tabellen en weergaven kunnen worden gemaakt via dezelfde gegevens.</li><li>Deze biedt ondersteuning voor een eenvoudige datawarehouse-implementatie enorme mogelijkheden voor scale-out en fouttolerantie voor gegevensopslag en verwerking biedt.</li></ul> | <ul><li>Hiervoor de brongegevens die moeten ten minste enkele identificeerbare structuur hebben.</li><li>Het is niet geschikt voor realtime query's uitvoeren en de rij op updates. Het wordt beste voor batch-taken via grote gegevenssets gebruikt.</li><li>Het is mogelijk niet sommige typen van complexe verwerkingstaken uitvoeren.</li></ul> |
| **Apache Pig Latin voor Pig gebruiken** | <ul><li>Een uitstekende oplossing voor het bewerken van gegevens, zoals ingesteld, samenvoegen en gegevenssets, toepassen van functies op records of groepen records, filteren en voor de herstructurering van gegevens door te geven kolommen, door de groeperingswaarden of door de kolommen omzetten in rijen.</li><li>Als een reeks bewerkingen op gegevens kan het een werkstroom gebaseerde benadering gebruiken.</li></ul> | <ul><li>SQL-gebruikers wellicht dat pig Latin is minder bekende en moeilijker te gebruiken dan HiveQL.</li><li>De standaarduitvoer is meestal een tekstbestand en dus kan worden te gebruiken met gegevensvisualisatieprogramma's zoals Excel. U wordt doorgaans een Hive-tabel laag via de uitvoer.</li></ul> |
| **Aangepaste/mapreduce** | <ul><li>Het biedt volledige controle over de kaart en verminder fasen en kan worden uitgevoerd.</li><li>Hiermee kunt query's om te worden geoptimaliseerd voor maximale prestaties van het cluster en om de belasting op de servers en het netwerk.</li><li>De onderdelen kunnen worden geschreven in een reeks bekende talen.</li></ul> | <ul><li>Het is moeilijker dan het gebruik van Pig of Hive omdat u moet uw eigen map maken en onderdelen te verminderen.</li><li>Processen waarvoor lid worden van gegevenssets zijn moeilijker te implementeren.</li><li>Hoewel er testframeworks beschikbaar zijn, is foutopsporing van code complexer dan een normale toepassing omdat de code wordt uitgevoerd als een batch-taak onder het beheer van de Taakplanner van Hadoop.</li></ul> |
| **Apache HCatalog** | <ul><li>Deze isoleert de details van het pad van de opslag en beheer eenvoudiger maken en verwijderen van de noodzaak voor gebruikers om te weten waar de gegevens zijn opgeslagen.</li><li>Hiermee kunt meldingen van gebeurtenissen, zoals de beschikbaarheid van gegevens, zodat andere hulpprogramma's zoals Oozie om te detecteren wanneer bewerkingen hebben plaatsgevonden.</li><li>Het wordt aangegeven dat een relationele weergave van gegevens, met inbegrip van partitioneren met sleutel, en maakt de gegevens gemakkelijk toegankelijk.</li></ul> | <ul><li>Azureondersteunt RCFile, CSV-tekst, JSON-tekst, SequenceFile en ORC bestandsindelingen standaard, maar mogelijk moet u een aangepaste serde-schrijfbewerking voor andere indelingen schrijven.</li><li>HCatalog is niet thread-veilig.</li><li>Er zijn enkele beperkingen voor de gegevenstypen voor kolommen bij het gebruik van het laadprogramma HCatalog in Pig-scripts. Zie voor meer informatie, [HCatLoader gegevenstypen](https://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) in de documentatie van Apache HCatalog.</li></ul> |

Meestal gebruikt u de meest eenvoudige van deze methoden die de resultaten die u nodig hebt. Bijvoorbeeld, kunt u mogelijk die resultaten bereiken met behulp van alleen Hive, maar voor complexere scenario's mogelijk moet u Pig, gebruiken of zelfs uw eigen map schrijven en onderdelen te verminderen. U kunt ook besluiten, na te experimenteren met Hive of Pig, die aangepaste kaart en verminderen onderdelen zorgen voor betere prestaties doordat u verder kunt afstemmen en optimaliseren van de verwerking.

## <a name="custom-mapreduce-components"></a>Aangepaste/mapreduce-onderdelen

Code mapreduce/bestaat uit twee afzonderlijke functies geïmplementeerd als **kaart** en **verminderen** onderdelen. De **kaart** onderdeel wordt parallel uitgevoerd op meerdere clusterknooppunten, elk knooppunt de toewijzing toe te passen op de subset van het knooppunt van de gegevens. De **verminderen** onderdeel worden gesorteerd en bevat een overzicht van de resultaten van alle functies van de kaart. Zie voor meer informatie over deze twee onderdelen [MapReduce gebruiken in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

In de meeste scenario's voor verwerking van HDInsight is het eenvoudiger en efficiënter gebruik van een hoger niveau abstractie zoals Pig of Hive. U kunt ook aangepaste kaart maakt en beperken van onderdelen voor gebruik in Hive-scripts meer geavanceerde verwerking uit te voeren.

Aangepaste/mapreduce-onderdelen zijn meestal geschreven in Java. Hadoop wordt gebruikt om een streaming-interface waarmee ook onderdelen moet worden gebruikt die zijn ontwikkeld in andere talen zoals C#, F#, Visual Basic, Python en JavaScript.

* Zie voor een overzicht over het ontwikkelen van aangepaste Java MapReduce-programma's, [ontwikkel Java MapReduce-programma's voor Hadoop op HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md).
* Zie voor een voorbeeld met behulp van Python [ontwikkelen Python MapReduce-programma's voor HDInsight streaming](apache-hadoop-streaming-python.md).

Houd rekening met het maken van uw eigen kaart en beperken van onderdelen voor de volgende voorwaarden:

* U moet voor het verwerken van gegevens die op het parseren van de gegevens en met aangepaste logica op te halen van gestructureerde gegevens uit het volledig ongestructureerde is.
* U wilt uitvoeren van complexe taken die zijn moeilijk (of zelfs onmogelijk) om uit te drukken in Pig of Hive-zonder het maken van een UDF. Bijvoorbeeld, als u wilt gebruiken van externe geocoderingservice breedtegraad en lengtegraad coördinaten of IP-adressen in de brongegevens converteren naar namen van de geografische locatie.
* Wilt u uw bestaande .NET, Python of JavaScript-code in mapreduce /-onderdelen opnieuw met behulp van de Hadoop-streaming-interface.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Uploaden en uw aangepaste MapReduce-programma uitvoeren

De meest voorkomende MapReduce-programma's zijn geschreven in Java en gecompileerd naar een jar-bestand.

1. Nadat u hebt ontwikkeld, gecompileerd en getest uw MapReduce-programma, gebruikt u de `scp` opdracht voor het uploaden van de jar-bestand met het hoofdknooppunt.

    ```bash
    scp mycustomprogram.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang **gebruikersnaam** met de SSH-gebruikersaccount voor uw cluster. Vervang **CLUSTERNAME** met de naam van het cluster. Als u een wachtwoord gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord invoeren. Als u een certificaat gebruikt, moet u mogelijk gebruik van de `-i` parameter opgeven voor het persoonlijke sleutelbestand.

2. Verbinding maken met het cluster met behulp [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Uitvoeren van de SSH-sessie, uw MapReduce-programma via YARN.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Met deze opdracht verzendt de MapReduce-taak op YARN. Het invoerbestand is `/example/data/sample.log`, en de uitvoermap is `/example/data/logoutput`. Het bestand voor invoer en uitvoerbestanden worden opgeslagen naar de standaardopslag voor het cluster.

## <a name="next-steps"></a>Volgende stappen

* [Gebruik C# met MapReduce, streaming van Apache Hadoop in HDInsight](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Java MapReduce-programma's ontwikkelen voor Apache Hadoop op HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Python MapReduce-programma's voor HDInsight streaming ontwikkelen](apache-hadoop-streaming-python.md)
* [Azure Toolkit voor Eclipse gebruiken voor het maken van Apache Spark-toepassingen voor een HDInsight-cluster](../spark/apache-spark-eclipse-tool-plugin.md)
* [Gebruik Python-gebruiker gedefinieerde functies (UDF's) met Apache Hive en Apache Pig in HDInsight](python-udf-hdinsight.md)
