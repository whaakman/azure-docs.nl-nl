---
title: Hive-tabellen maken en te laden van gegevens uit Azure Blob Storage | Microsoft Docs
description: Hive-tabellen maken en te laden van gegevens in blob naar de hive-tabellen
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 6d9df88d6047fbe674c216dacc6fa01bad8451ec
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive-tabellen maken en gegevens uit Azure Blob-opslag laden
Dit onderwerp bevat algemene Hive-query's die Hive-tabellen maken en gegevens uit Azure blob-opslag laden. Sommige richtlijnen is ook beschikbaar op het Hive-tabellen te partitioneren en over het gebruik van de optimale rij kolommen (ORC) opmaak ter verbetering van de prestaties van query's.

Dit **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u opnemen van gegevens in de doel-omgevingen waarin de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht.  Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-clusters voor geavanceerde analyses](customize-hadoop-cluster.md).
* Ingeschakelde externe toegang tot het cluster aangemeld en de Hadoop-opdrachtregelconsole geopend. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop-Cluster](customize-hadoop-cluster.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Als u een virtuele machine in Azure gemaakt op basis van de instructies in [instellen van een virtuele machine van Azure voor geavanceerde analyses](../data-science-virtual-machine/setup-virtual-machine.md), dit scriptbestand moet worden gedownload naar de *C:\\gebruikers \\ \<gebruikersnaam\>\\documenten\\gegevens wetenschappelijke Scripts* map op de virtuele machine. Deze Hive-query's vereisen alleen plug-in uw eigen gegevensschema en de configuratie van de Azure blob-opslag in de juiste velden gereed is voor verzending.

Er wordt ervan uitgegaan dat de gegevens voor Hive-tabellen een **niet-gecomprimeerde** tabelvorm en die de gegevens is geüpload naar de standaardwaarde (of een extra) container van het opslagaccount dat wordt gebruikt door het Hadoop-cluster.

Als u wilt oefenen op de **NYC Taxi reis gegevens**, moet u:

* **Download** de 24 [NYC Taxi reis gegevens](http://www.andresmh.com/nyctaxitrips) -bestanden (12 reis bestanden en 12 tarief-bestanden)
* **Pak** alle bestanden in CSV-bestanden, en vervolgens
* **uploaden** ze naar de standaard (of de juiste container) van de Azure storage-account dat is gemaakt door de procedure beschreven in de [aanpassen Azure HDInsight Hadoop-clusters voor Advanced Analytics-proces en technologie](customize-hadoop-cluster.md)onderwerp. Het proces voor het uploaden van de CSV-bestanden naar de standaardcontainer op het opslagaccount kan worden gevonden op deze [pagina](hive-walkthrough.md#upload).

## <a name="submit"></a>Het indienen van Hive-query 's
Hive-query's kunnen worden verzonden met behulp van:

1. [Indienen van Hive-query's via de opdrachtregel voor Hadoop in headnode van Hadoop-cluster](#headnode)
2. [Indienen van Hive-query's met de Hive-Editor](#hive-editor)
3. [Indienen van Hive-query's met Azure PowerShell-opdrachten](#ps)

Hive-query's zijn SQL-achtige. Als u bekend met SQL bent, vindt u de [Hive voor SQL gebruikers cheats blad](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) nuttig.

Bij het indienen van een Hive-query kunt u ook de bestemming van de uitvoer van Hive-query's beheren, ongeacht of deze op het scherm of naar een lokaal bestand op het hoofdknooppunt of met een Azure-blob.

### <a name="headnode"></a> 1. Indienen van Hive-query's via de opdrachtregel voor Hadoop in headnode van Hadoop-cluster
Als de Hive-query complexe, leidt verzendt, wordt deze rechtstreeks in het hoofdknooppunt van het Hadoop-cluster meestal tot sneller inschakelen om dan verzendt, wordt deze met een Editor Hive of Azure PowerShell-scripts.

Aanmelden met het hoofdknooppunt van het Hadoop-cluster, opent u de Hadoop-opdrachtregel op het bureaublad van het hoofdknooppunt en voer de opdracht `cd %hive_home%\bin`.

Hebt u drie manieren om Hive-query's in de Hadoop-opdrachtregel verzenden:

* rechtstreeks
* met behulp van .hql bestanden
* met de Hive-opdrachtconsole

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Indienen van Hive-query's rechtstreeks in Hadoop vanaf de opdrachtregel.
U kunt een opdracht zoals uitvoeren `hive -e "<your hive query>;` verzenden eenvoudige Hive-query's rechtstreeks in Hadoop vanaf de opdrachtregel. Hier volgt een voorbeeld, waarbij een rood kader geeft een overzicht van de opdracht die is ingediend door de Hive-query en het groene vak geeft een overzicht van de uitvoer van de Hive-query.

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Indienen van Hive-query's in .hql bestanden
Wanneer de Hive-query gecompliceerdere is en meerdere regels heeft, is query's in de opdrachtregel of Hive-opdrachtconsole bewerken het niet praktisch. Een alternatief is het gebruik van een teksteditor in het hoofdknooppunt van het Hadoop-cluster de Hive-query's opslaan in een bestand .hql in een lokale map van het hoofdknooppunt. Vervolgens kan de Hive-query in het bestand .hql worden verzonden met behulp van de `-f` argument als volgt:

    hive -f "<path to the .hql file>"

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-3.png)

**Onderdrukken voortgang Statusoverzicht scherm van Hive-query 's**

Standaard nadat Hive-query wordt verzonden in Hadoop opdrachtregel wordt de voortgang van de taak van de kaart/verkleinen afgedrukt op het scherm. Als u wilt onderdrukken het afdrukken van het scherm van de voortgang van de taak kaart/verminderen, kunt u een argument `-S` ("S" in hoofdletters) in de opdracht regel als volgt:

    hive -S -f "<path to the .hql file>"
.    hive -S -e '<Hive queries>'

#### <a name="submit-hive-queries-in-hive-command-console"></a>Indienen van Hive-query's in de opdrachtconsole Hive.
U kunt ook eerst de opdrachtconsole Hive invoeren door de opdracht uit te voeren `hive` in Hadoop vanaf de opdrachtregel, en verzend Hive-query's in de opdrachtconsole Hive. Hier volgt een voorbeeld. In dit voorbeeld zijn de twee rode vakken Markeer de opdrachten voor het invoeren van de Hive-opdrachtconsole en de Hive-query verzonden in de Hive-opdrachtconsole, respectievelijk. Het groene vak licht de uitvoer van de Hive-query.

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-2.png)

De eerdere voorbeelden uitvoer rechtstreeks van de resultaten van de Hive-query op het scherm. U kunt de uitvoer ook schrijven naar een lokaal bestand op het hoofdknooppunt of met een Azure-blob. Vervolgens kunt u andere hulpprogramma's voor het analyseren van verdere de uitvoer van Hive-query's.

**Hive-query naar een lokaal bestand uitvoerresultaten.**
Om het Hive-query naar een lokale map op het hoofdknooppunt uitvoerresultaten, hebt u als volgt de Hive-query in de Hadoop-opdrachtregel verzenden:

    hive -e "<hive query>" > <local path in the head node>

In het volgende voorbeeld wordt de uitvoer van Hive-query naar een bestand is geschreven `hivequeryoutput.txt` in directory `C:\apps\temp`.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-1.png)

**Resultaten voor de uitvoer Hive-query naar een Azure-blob**

U kunt ook de resultaten van de Hive-query naar een Azure-blob, binnen de standaardcontainer van het Hadoop-cluster uitvoeren. De Hive-query voor deze is als volgt:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

In het volgende voorbeeld wordt de uitvoer van Hive-query naar een blob-map geschreven. `queryoutputdir` binnen de standaardcontainer van het Hadoop-cluster. Hier alleen moet u de naam van de map zonder de blob-naam opgeven. Een fout gegenereerd als u zowel de directory en de blob-namen, zoals opgeeft `wasb:///queryoutputdir/queryoutput.txt`.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-2.png)

Als u de standaard-container van het Hadoop-cluster met behulp van Azure Storage Explorer opent, ziet u de uitvoer van de Hive-query wordt weergegeven in de volgende afbeelding. U kunt het filter (gemarkeerd met een rood kader) voor het ophalen van de blob met de opgegeven letters in namen alleen toepassen.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Indienen van Hive-query's met de Hive-Editor
U kunt ook de Query-Console (Hive-Editor) gebruiken door een URL van het formulier *https://&#60; De naam van de Hadoop-cluster >.azurehdinsight.net/Home/HiveEditor* in een webbrowser. U moet zijn geregistreerd in de zien deze console en dus moet u uw hier Hadoop-cluster-referenties.

### <a name="ps"></a> 3. Indienen van Hive-query's met Azure PowerShell-opdrachten
U kunt ook PowerShell gebruiken voor het indienen van Hive-query's. Zie voor instructies [indienen Hive-taken met behulp van PowerShell](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Hive-database en tabellen maken
Het Hive-query's worden gedeeld in de [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) en van daaruit kan worden gedownload.

Hier volgt de Hive-query waarmee een Hive-tabel.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Hier volgen de beschrijvingen van de velden die u plug moet-- en andere configuraties:

* **&#60; databasenaam >**: de naam van de database die u wilt maken. Als u alleen wilt gebruiken de standaard-database, de query *database maken...*  kunnen worden weggelaten.
* **&#60; tabelnaam >**: de naam van de tabel die u wilt maken binnen de opgegeven database. Als u gebruiken de standaard-database wilt, de tabel rechtstreeks kan worden verwezen door *&#60; tabelnaam >* zonder &#60; databasenaam >.
* **&#60; veldscheidingsteken >**: het scheidingsteken op waarmee de velden in het gegevensbestand kunnen worden geüpload naar de Hive-tabel.
* **&#60; regelscheiding >**: het scheidingsteken die regels in het gegevensbestand begrenst.
* **&#60; opslaglocatie >**: de Azure-opslag-locatie voor het opslaan van de gegevens van Hive-tabellen. Als u geen opgeeft *locatie &#60; opslaglocatie >*, tabellen en de database worden opgeslagen in *hive/datawarehouse/* map in de standaardcontainer van het cluster Hive standaard. Als u de opslaglocatie opgeven wilt, is de opslaglocatie zich binnen de standaardcontainer voor de database en tabellen. Deze locatie moet worden verwezen als locatie ten opzichte van de standaard-container van het cluster in de indeling van *' wasb: / / / &#60; map 1 > / "* of *' wasb: / / / &#60; map 1 > / &#60; map 2 > /'*, enzovoort. Nadat de query wordt uitgevoerd, worden de relatieve mappen in de standaardcontainer gemaakt.
* **TBLPROPERTIES("SKIP.header.line.Count"="1")**: als het gegevensbestand een kopregel heeft, hebt u deze eigenschap toevoegen **aan het einde** van de *tabel maken* query. Anders wordt de kopregel geladen als een record aan de tabel. Als het gegevensbestand geen een kopregel heeft, kan deze configuratie worden weggelaten in de query.

## <a name="load-data"></a>Laden van gegevens naar de Hive-tabellen
Hier volgt de Hive-query die gegevens in een Hive-tabel laadt.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **&#60; pad naar de blob-gegevens >**: als de blob-bestand wilt uploaden naar de Hive-tabel in de standaardcontainer van HDInsight Hadoop-cluster de *&#60; pad naar de blob-gegevens >* moet de indeling *' wasb: / / / &#60; directory in deze container > / &#60; blob-bestandsnaam >'*. De blob-bestand kan ook worden in een extra container van het HDInsight Hadoop-cluster. In dit geval *&#60; pad naar de blob-gegevens >* moet de indeling *' wasb: / / &#60; containernaam > @&#60; opslagaccountnaam >.blob.core.windows.net/ &#60; blob-bestandsnaam >'*.

  > [!NOTE]
  > De blob-gegevens kunnen worden geüpload naar de Hive-tabel is in de standaardinstellingen of extra container van het opslagaccount voor het Hadoop-cluster. Anders wordt de *gegevens laden* query klagen dat deze geen toegang de gegevens tot is mislukt.
  >
  >

## <a name="partition-orc"></a>Onderwerpen over geavanceerde: gepartitioneerde tabel en archief Hive-gegevens in ORC-indeling
Als de gegevens te groot is, is partitioneren van de tabel nuttig voor query's die alleen moeten worden gescand van enkele partities van de tabel. Bijvoorbeeld, kan men redelijkerwijs voor het partitioneren van de logboekgegevens van een website door datums.

Naast het partitioneren van Hive-tabellen, is het ook nuttig om de Hive-gegevens opslaat in de indeling geoptimaliseerd rij kolommen (ORC). Zie voor meer informatie over het ORC opmaak <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">met behulp van ORC-bestanden verbetert de prestaties wanneer Hive lezen, schrijven en verwerken van gegevens</a>.

### <a name="partitioned-table"></a>Gepartitioneerde tabel
Hier volgt de Hive-query die een gepartitioneerde tabel maakt en gegevens worden geladen in de App.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Tijdens het opvragen van gepartitioneerde tabellen, verdient het toevoegen van de voorwaarde van de partitie in de **begin** van de `where` component als dit verbetert de effectiviteit van aanzienlijk zoeken.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Hive-gegevens opslaat in ORC-indeling
U kan niet rechtstreeks gegevens uit blob-opslag laden in de Hive-tabellen die zijn opgeslagen in de ORC-indeling. Hier volgen de stappen die u moet uitvoeren om te laden van gegevens van Azure blobs voor Hive-tabellen die zijn opgeslagen in ORC-indeling.

Maken van een externe tabel **opgeslagen AS TEXTFILE** en laden van gegevens uit blob-opslag naar de tabel.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Een interne tabel maken met hetzelfde schema als de externe tabel in stap 1, met dezelfde veldscheidingsteken en het Hive-gegevens opslaat in de ORC-indeling.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Selecteer de gegevens van de externe tabel in stap 1 en in de tabel ORC invoegen

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Als de tabel TEXTFILE *&#60; databasenaam >. &#60; externe textfile tabelnaam >* partities in stap 3 heeft de `SELECT * FROM <database name>.<external textfile table name>` opdracht wordt de variabele partitie geselecteerd als een veld in de geretourneerde gegevensset. Invoegen in de *&#60; databasenaam >. &#60; ORC-tabelnaam >* mislukt sinds *&#60; databasenaam >. &#60; ORC-tabelnaam >* heeft niet de partitie-variabele als een veld in het tabelschema. In dit geval moet u de velden die moeten worden ingevoegd in specifiek selecteren *&#60; databasenaam >. &#60; ORC-tabelnaam >* als volgt:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Het is veilig verwijderen de *&#60; externe textfile tabelnaam >* wanneer met behulp van de volgende query nadat alle gegevens is ingevoegd in *&#60; databasenaam >. &#60; ORC-tabelnaam >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Na deze procedure uitvoert, moet u een tabel met gegevens in de klaar voor gebruik ORC-indeling hebben.  
