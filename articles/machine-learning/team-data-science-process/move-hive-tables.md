---
title: Hive-tabellen maken en gegevens laden uit Azure Blob Storage | Microsoft Docs
description: Hive-tabellen maken en gegevens in blob naar de hive-tabellen laden
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 42911c347cd055f37f7fe8f31b6d22cc18a78662
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442877"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive-tabellen maken en gegevens laden uit Azure Blob Storage

In dit artikel geeft algemene Hive-query's die Hive-tabellen maken en gegevens laden vanuit Azure blob storage. Sommige richtlijnen zijn beschikbaar op de Hive-tabellen partitioneren en over het gebruik van de geoptimaliseerd rij kolommen (ORC) te verbeteren, opmaak.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht.  Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-clusters voor geavanceerde analyses](customize-hadoop-cluster.md).
* Ingeschakelde externe toegang tot het cluster, aangemeld en de Hadoop-opdrachtregelconsole geopend. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop Cluster](customize-hadoop-cluster.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Als u een virtuele Azure-machine hebt gemaakt door de instructies in [instellen van een virtuele machine van Azure voor geavanceerde analyses](../data-science-virtual-machine/setup-virtual-machine.md), dit scriptbestand moet worden gedownload naar de *C:\\gebruikers \\ \<gebruikersnaam\>\\documenten\\Data Science Scripts* map op de virtuele machine. Deze Hive-query's is alleen vereist dat u in uw eigen gegevensschema en configuratie van de Azure blob-opslag in de juiste velden gereed is voor het indienen van aansluit.

We gaan ervan uit dat de gegevens voor Hive-tabellen een **niet-gecomprimeerde** tabelvorm, en dat de gegevens zijn geüpload naar de standaardwaarde (of een extra) container van het opslagaccount dat wordt gebruikt door de Hadoop-cluster.

Als u wilt om te oefenen op de **NYC Taxi reisgegevens**, moet u:

* **Download** de 24 [NYC Taxi reisgegevens](http://www.andresmh.com/nyctaxitrips) bestanden (12 reis-bestanden en 12 Fare-bestanden)
* **Pak deze uit** alle bestanden in CSV-bestanden, en vervolgens
* **uploaden** deze naar de standaard (of de juiste container) van de Azure storage-account dat is gemaakt door de procedure die wordt beschreven de [aanpassen Azure HDInsight Hadoop-clusters voor Advanced Analytics Process and Technology](customize-hadoop-cluster.md)onderwerp. Het proces voor het uploaden van de CSV-bestanden naar de standaard-container in de storage-account kan worden gevonden op deze [pagina](hive-walkthrough.md#upload).

## <a name="submit"></a>Het indienen van Hive-query 's
Hive-query's kunnen worden verzonden met behulp van:

1. [Indienen van Hive-query's via Hadoop vanaf de opdrachtregel in het hoofdknooppunt van Hadoop-cluster](#headnode)
2. [Indienen van Hive-query's met de Hive-Editor](#hive-editor)
3. [Indienen van Hive-query's met Azure PowerShell-opdrachten](#ps)

Hive-query's zijn SQL-achtige. Als u bekend met SQL bent, vindt u de [Hive voor SQL gebruikers Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) nuttig.

Bij het indienen van een Hive-query, kunt u ook de bestemming van de uitvoer van Hive-query's, beheren, ongeacht of deze op het scherm of een lokaal bestand op het hoofdknooppunt van of naar een Azure-blob.

### <a name="headnode"></a> 1. Indienen van Hive-query's via Hadoop vanaf de opdrachtregel in het hoofdknooppunt van Hadoop-cluster
Als de Hive-query complex is, leidt verzendt, wordt deze rechtstreeks in het hoofdknooppunt van het Hadoop cluster meestal tot sneller omlooptijd dan verzendt, wordt deze met een Hive-Editor of Azure PowerShell-scripts.

Meld u aan bij het hoofdknooppunt van het Hadoop-cluster, opent u de Hadoop-opdrachtregel op het bureaublad van het hoofdknooppunt en voer de opdracht `cd %hive_home%\bin`.

Hebt u drie manieren om in te dienen Hive-query's in de Hadoop-opdrachtregel:

* rechtstreeks
* met behulp van .hql bestanden
* met de opdrachtconsole Hive

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Indienen van Hive-query's rechtstreeks in Hadoop vanaf de opdrachtregel.
U kunt de opdracht, zoals uitvoeren `hive -e "<your hive query>;` om in te dienen eenvoudige Hive-query's rechtstreeks in Hadoop vanaf de opdrachtregel. Hier volgt een voorbeeld, waarbij een rood kader geeft een overzicht van de opdracht die is ingediend door de Hive-query en de groene vakje geeft een overzicht van de uitvoer van de Hive-query.

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Indienen van Hive-query's in .hql bestanden
Wanneer de Hive-query gecompliceerder is en meerdere regels heeft, is het bewerken van query's in vanaf de opdrachtregel of in de opdrachtconsole Hive het niet praktisch. Een alternatief is het gebruik van een teksteditor in het hoofdknooppunt van het Hadoop-cluster voor het opslaan van de Hive-query's in een bestand .hql in een lokale map van het hoofdknooppunt. En vervolgens de Hive-query in het bestand .hql kan worden verzonden met behulp van de `-f` argument als volgt te werk:

    hive -f "<path to the .hql file>"

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-3.png)

**Voortgang van de status scherm afdrukken van Hive-query's onderdrukken**

Standaard nadat Hive-query wordt verzonden in Hadoop vanaf de opdrachtregel, wordt de voortgang van de taak voor toewijzen/verminderen afgedrukt op het scherm. Als u wilt onderdrukken van het scherm afdrukken van de voortgang van de taak toewijzen/verminderen, kunt u een argument `-S` ("S" in hoofdletters) in de opdracht regel als volgt:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Indienen van Hive-query's in de opdrachtconsole Hive.
U kunt ook eerst de opdrachtconsole Hive invoeren door uit te voeren opdracht `hive` in Hadoop vanaf de opdrachtregel, en vervolgens verzendt Hive-query's in de opdrachtconsole Hive. Hier volgt een voorbeeld. In dit voorbeeld Markeer de twee rode vakken de opdrachten gebruikt voor het invoeren van de opdrachtconsole Hive en de Hive-query verzonden in de opdrachtconsole Hive, respectievelijk. De groene vakje markeert de uitvoer van de Hive-query.

![Werkruimte maken](./media/move-hive-tables/run-hive-queries-2.png)

De eerdere voorbeelden uitvoer rechtstreeks van de resultaten van de Hive-query op het scherm. U kunt de uitvoer ook schrijven naar een lokaal bestand op het hoofdknooppunt, of naar een Azure-blob. Vervolgens kunt u andere hulpprogramma's voor verdere analyse van de uitvoer van Hive-query's.

**Uitvoerresultaten van het Hive-query naar een lokaal bestand.**
Als u wilt uitvoeren van resultaten van Hive-query naar een lokale map op het hoofdknooppunt, hebt u om in te dienen de Hive-query in de Hadoop-opdrachtregel als volgt:

    hive -e "<hive query>" > <local path in the head node>

In het volgende voorbeeld wordt de uitvoer van Hive-query naar een bestand wordt geschreven `hivequeryoutput.txt` in directory `C:\apps\temp`.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-1.png)

**Hive-query resultaten uit naar een Azure-blob**

U kunt ook de resultaten van de Hive-query naar een Azure-blob, binnen de standaardcontainer van het Hadoop-cluster uitvoeren. De Hive-query voor deze is als volgt:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

In het volgende voorbeeld wordt de uitvoer van Hive-query is geschreven naar een blob-map `queryoutputdir` binnen de standaardcontainer van het Hadoop-cluster. Hier, hoeft u alleen voor de naam van de map zonder naam van de blob. Wordt er een fout gegenereerd als u zowel de directory als de blob-namen, zoals `wasb:///queryoutputdir/queryoutput.txt`.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-2.png)

Als u de standaardcontainer van het Hadoop-cluster met behulp van Azure Storage Explorer opent, ziet u de uitvoer van de Hive-query, zoals wordt weergegeven in de volgende afbeelding. U kunt het filter (gemarkeerd met een rood kader) toepassen om op te halen alleen de blob met de opgegeven letters in namen.

![Werkruimte maken](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Indienen van Hive-query's met de Hive-Editor
U kunt ook de Queryconsole (Hive-Editor) door een URL van het formulier *https://<Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* in een webbrowser. U moet zijn geregistreerd in de zien deze console en dus moet u uw Hadoop-clusterreferenties hier.

### <a name="ps"></a> 3. Indienen van Hive-query's met Azure PowerShell-opdrachten
U kunt ook PowerShell gebruiken om in te dienen Hive-query's. Zie voor instructies [indienen Hive-taken met behulp van PowerShell](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Hive-database en tabellen maken
Het Hive-query's worden gedeeld in de [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) en van daaruit kan worden gedownload.

Hier volgt de Hive-query die wordt gemaakt van een Hive-tabel.

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

Hier volgen de beschrijvingen van de velden die u nodig hebt om aan te sluiten en andere configuraties:

* **<database name>**: de naam van de database die u wilt maken. Als u alleen wilt gebruiken de standaard-database, de query *-database maken...*  kunnen worden weggelaten.
* **<table name>**: de naam van de tabel die u wilt maken binnen de opgegeven database. Als u gebruiken de standaard-database wilt, de tabel rechtstreeks kan worden verwezen door *<table name>* zonder <database name>.
* **<field separator>**: het scheidingsteken die velden in het gegevensbestand worden geüpload naar de Hive-tabel.
* **<line separator>**: het scheidingsteken die regels in het gegevensbestand.
* **<storage location>**: de Azure-opslaglocatie op te slaan van de gegevens van Hive-tabellen. Als u geen opgeeft *locatie <storage location>* , tabellen en de database worden opgeslagen in *hive/datawarehouse/* Active directory in de standaardcontainer van het cluster Hive standaard. Als u opgeven van de opslaglocatie wilt, heeft de opslaglocatie moet zich binnen de standaard-container voor de database en tabellen. Deze locatie heeft naar worden verwezen als locatie ten opzichte van de standaardcontainer van het cluster in de indeling van *' wasb: / / / < map 1 > / "* of *' wasb: / / / < map 1 > / < map 2 > /"*, enzovoort. Nadat de query wordt uitgevoerd, worden de relatieve mappen in de standaardcontainer worden gemaakt.
* **TBLPROPERTIES("SKIP.header.line.Count"="1")**: als het bestand met een kopregel heeft, u moet deze eigenschap toevoegen **aan het einde** van de *-tabel maken* query. Anders wordt de kopregel geladen als een record in de tabel. Als het gegevensbestand geen een kopregel heeft, kan deze configuratie worden weggelaten in de query.

## <a name="load-data"></a>Gegevens laden in Hive-tabellen
Hier volgt de Hive-query die gegevens in een Hive-tabel laadt.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **<path to blob data>**: Of de blob-bestand worden geüpload naar de Hive-tabel in de standaardcontainer van het HDInsight Hadoop-cluster, is de *<path to blob data>* moet zich in de indeling *' wasb: / / /<directory in this container> / <blob file name>'*. De blob-bestand kan ook worden in een andere container van het HDInsight Hadoop-cluster. In dit geval *<path to blob data>* moet zich in de indeling *' wasb: / /<container name><storage account name>.blob.core.windows.net/<blob file name>'*.

  > [!NOTE]
  > De blob-gegevens worden geüpload naar de Hive-tabel heeft zich in de standaard- of extra container van het opslagaccount voor het Hadoop-cluster. Anders wordt de *LOADING* query klagen dat deze geen toegang de gegevens tot is mislukt.
  >
  >

## <a name="partition-orc"></a>Geavanceerde onderwerpen: gepartitioneerde tabel en store Hive-gegevens in ORC-indeling
Als de gegevens te groot is, is het partitioneren van de tabel nuttig zijn voor query's die alleen moeten worden gescand van een aantal partities van de tabel. Het is bijvoorbeeld redelijk voor het partitioneren van de logboekgegevens van een website met datums.

Naast het Hive-tabellen partitioneren, is het ook nuttig voor het opslaan van de Hive-gegevens in de indeling geoptimaliseerd rij kolommen (ORC). Zie voor meer informatie over het ORC-opmaak <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">met behulp van ORC-bestanden worden de prestaties verbeterd wanneer Hive wordt lezen, schrijven en verwerken van gegevens</a>.

### <a name="partitioned-table"></a>Gepartitioneerde tabel
Hier volgt de Hive-query die een gepartitioneerde tabel maakt en gegevens in het laadt.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Bij het opvragen van gepartitioneerde tabellen, het is raadzaam om toe te voegen van de voorwaarde van de partitie in de **begin** van de `where` component als dit de effectiviteit van het zoeken in aanzienlijk worden verbeterd.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Store Hive-gegevens in ORC-indeling
U laden geen gegevens rechtstreeks van blob-opslag in Hive-tabellen die zijn opgeslagen in het ORC-indeling. Hier volgen de stappen die u moet uitvoeren om te laden van gegevens van Azure-blobs met Hive-tabellen die zijn opgeslagen in ORC-indeling.

Maken van een externe tabel **opgeslagen als TEXTFILE** en gegevens uit blob-opslag laden in de tabel.

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

Een interne tabel maken met hetzelfde schema als de externe tabel in stap 1, met hetzelfde veld scheidingsteken wordt gebruikt en het Hive-gegevens opslaan in het ORC-indeling.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Gegevens selecteren uit de externe tabel in stap 1 en invoegen in het ORC-tabel

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Als de tabel TEXTFILE  *<database name>.<external textfile table name>* partities in stap 3, heeft de `SELECT * FROM <database name>.<external textfile table name>` opdracht wordt de variabele partitie geselecteerd als een veld in de geretourneerde gegevens. Invoegen in de  *<database name>.<ORC table name>* mislukt sinds  *<database name>.<ORC table name>* geen de variabele voor de partitie niet als een veld in het tabelschema. In dit geval moet u specifiek de velden selecteren om te worden ingevoegd in  *<database name>.<ORC table name>* als volgt:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Het is veilig verwijderen de *<external textfile table name>* wanneer met behulp van de volgende query uit nadat alle gegevens is ingevoegd in *<database name>.<ORC table name>*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Na deze procedure uitvoert, moet u een tabel met gegevens in de klaar voor gebruik ORC-indeling hebben.  
