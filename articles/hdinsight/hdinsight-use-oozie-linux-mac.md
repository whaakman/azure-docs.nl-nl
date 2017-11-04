---
title: Gebruik van Hadoop Oozie werkstromen in Azure HDInsight op basis van Linux | Microsoft Docs
description: "Gebruik Hadoop Oozie in HDInsight op basis van Linux. Informatie over het definiëren van een werkstroom Oozie en het verzenden van een Oozie-taak."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: larryfr
ms.openlocfilehash: 7f341b550316b44f7b6677bf59f323b568007700
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-azure-hdinsight"></a>Oozie gebruiken met Hadoop om te definiëren en een werkstroom uitgevoerd op Azure HDInsight op basis van Linux

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Informatie over het gebruiken van Apache Oozie met Hadoop op Azure HDInsight. Oozie is een werkstroom en coördinatie systeem waarmee Hadoop-taken worden beheerd. Oozie is geïntegreerd met de Hadoop-stack en ondersteunt de volgende taken:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

U kunt ook Oozie taken plannen die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn gebruiken.

> [!NOTE]
> Een andere optie voor het definiëren van werkstromen met HDInsight is het gebruik van Azure Data Factory. Zie voor meer informatie over Data Factory, [Use Pig en Hive met Data Factory][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie is niet ingeschakeld op HDInsight domein.

## <a name="prerequisites"></a>Vereisten

* **Een HDInsight-cluster**: Zie [aan de slag met HDInsight op Linux](/hadoop/apache-hadoop-linux-tutorial-get-started.md)

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="example-workflow"></a>Van de voorbeeldwerkstroom

De werkstroom gebruikt in dit document bevat twee acties. Acties zijn definities voor taken zoals het uitvoeren van Hive, Sqoop, MapReduce of andere processen:

![Werkstroomdiagram][img-workflow-diagram]

1. Een Hive-actie wordt uitgevoerd een HiveQL-script om uit te pakken records uit de **hivesampletable** die is opgenomen in HDInsight. Elke rij gegevens beschrijft een bezoek van een specifieke mobiele apparaat. De recordindeling wordt weergegeven zoals de volgende tekst:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Het Hive-script in dit document gebruikt het totaal aantal bezoeken voor elk platform, zoals Android of iPhone, telt en slaat het aantal aan een nieuwe Hive-tabel.

    Zie [Hive gebruiken met HDInsight][hdinsight-use-hive] voor meer informatie over Hive.

2. Een actie Sqoop exporteert u de inhoud van de nieuwe Hive-tabel naar een tabel gemaakt in Azure SQL Database. Zie voor meer informatie over Sqoop [Hadoop-Sqoop gebruiken met HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Zie voor ondersteunde versies van de Oozie op HDInsight-clusters, [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight][hdinsight-versions].

## <a name="create-the-working-directory"></a>De werkmap maken

Oozie verwacht u alle resources die vereist zijn voor een taak in dezelfde map opslaan. In dit voorbeeld wordt **wasb: / / / zelfstudies/useoozie**. Voer de volgende stappen uit voor het maken van deze map:

1. Verbinding maken met het HDInsight-cluster via SSH:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` met de SSH-gebruikersnaam voor het cluster. Vervang `clustername` met de naam van het cluster. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdracht voor het maken van de map:

    ```bash
    hdfs dfs -mkdir -p /tutorials/useoozie/data
    ```

    > [!NOTE]
    > De `-p` parameter zorgt ervoor dat het maken van alle mappen in het pad. De **gegevens** directory wordt gebruikt om de gegevens die worden gebruikt door de **useooziewf.hql** script.

3. Om ervoor te zorgen dat uw gebruikersaccount kan worden geïmiteerd Oozie, gebruik de volgende opdracht:

    ```bash
    sudo adduser username users
    ```

    Vervang `username` door uw SSH-gebruikersnaam.

    > [!NOTE]
    > U kunt negeren fouten die wijzen op de gebruiker is al een lid van de `users` groep.

## <a name="add-a-database-driver"></a>Toevoegen van een databasestuurprogramma

Omdat deze werkstroom Sqoop gebruikt gegevens exporteren naar de SQL-database, moet u een kopie van de JDBC-stuurprogramma gebruikt voor communicatie met de SQL-database opgeven. Om te kopiëren JDBC-stuurprogramma voor de werkmap, gebruikt u de volgende opdracht bij de SSH-sessie:

```bash
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

Als uw werkstroom andere resources, zoals een jar dat een MapReduce-toepassing bevat gebruikt, moet u ook deze resources toevoegen.

## <a name="define-the-hive-query"></a>Definieer de Hive-query

Gebruik de volgende stappen voor het maken van een Hive-query language (HiveQL)-script dat definieert u een query. U gebruikt de query in een werkstroom Oozie verderop in dit document.

1. De volgende opdracht uit de SSH-verbinding gebruiken voor het maken van een bestand met de naam `useooziewf.hql`:

    ```bash
    nano useooziewf.hql
    ```

3. Nadat de GNU nano-editor wordt geopend, gebruikt u de volgende query als de inhoud van het bestand:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Er zijn twee variabelen die worden gebruikt in het script:

    * `${hiveTableName}`: Bevat de naam van de tabel moet worden gemaakt.

    * `${hiveDataFolder}`: Bevat de locatie voor het opslaan van de gegevensbestanden voor de tabel.

    De werkstroom-definitiebestand workflow.xml in deze zelfstudie geeft deze waarden aan dit script HiveQL tijdens runtime.

4. Selecteer Ctrl + X om af te sluiten van de editor. Wanneer u wordt gevraagd, selecteert u `Y` invoeren om het bestand opslaat, `useooziewf.hql` als de bestandsnaam en selecteer vervolgens **Enter**.

5. Gebruik de volgende opdrachten om te kopiëren `useooziewf.hql` naar `wasb:///tutorials/useoozie/useooziewf.hql`:

    ```bash
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Met deze opdracht slaat de `useooziewf.hql` bestand in de HDFS-compatibele opslag voor het cluster.

## <a name="define-the-workflow"></a>De werkstroom definiëren

Oozie werkstroomdefinities worden geschreven in Hadoop proces Definition Language (hPDL), die een XML-proces definitietaal. Gebruik de volgende stappen voor het definiëren van de werkstroom:

1. Gebruik de volgende instructie voor het maken en bewerken van een nieuw bestand:

    ```bash
    nano workflow.xml
    ```

2. Nadat de nano-editor wordt geopend, voert u het volgende XML-bestand als de inhoud van het bestand:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    Er zijn twee acties die zijn gedefinieerd in de werkstroom:

   * `RunHiveScript`: Met deze actie wordt de startactie en start de `useooziewf.hql` Hive-script.

   * `RunSqoopExport`: Met deze actie exporteert de gegevens die vanuit het Hive-script naar een SQL-database gemaakt met behulp van Sqoop. Deze actie wordt alleen uitgevoerd als de `RunHiveScript` actie is geslaagd.

     De werkstroom heeft enkele items zoals `${jobTracker}`. U kunt deze vermeldingen wordt vervangen door de waarden die u in de taakdefinitie. U maakt de taakdefinitie verderop in dit document.

     Let ook op de `<archive>sqljdbc4.jar</archive>` vermelding in de sectie Sqoop. Deze vermelding geeft opdracht Oozie om dit archief beschikbaar maken voor Sqoop wanneer deze actie wordt uitgevoerd.

3. Om het bestand opslaat, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**. 

4. Gebruik de volgende opdracht om te kopiëren de `workflow.xml` van het bestand in `/tutorials/useoozie/workflow.xml`:

    ```bash
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-the-database"></a>De database maken

Voor het maken van een SQL-database, volg de stappen in de [maken van een SQL-database](../sql-database/sql-database-get-started.md) document. Wanneer u de database maakt, gebruikt u `oozietest` als naam van de database. Ook Noteer de naam van de database-server.

### <a name="create-the-table"></a>De tabel maken

> [!NOTE]
> Er zijn veel manieren om te verbinden met SQL Database om een tabel te maken. De volgende stappen uitvoeren om [FreeTDS](http://www.freetds.org/) van het HDInsight-cluster.


1. Gebruik de volgende opdracht FreeTDS installeren op het HDInsight-cluster:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Nadat FreeTDS is geïnstalleerd, gebruikt u de volgende opdracht verbinding maken met de SQL database-server die u eerder hebt gemaakt:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    U ontvangt uitvoer zoals de volgende tekst:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. Op de `1>` vragen, voer de volgende regels:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Wanneer de `GO` instructie wordt ingevoerd, worden de vorige instructies geëvalueerd. Een tabel met de naam van deze instructies maken **mobiledata**, dat wordt gebruikt door de werkstroom.

    Om te bevestigen dat de tabel is gemaakt, gebruikt u de volgende opdrachten:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Ziet u uitvoer zoals de volgende tekst:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

4. Voer het hulpprogramma tsql om af te sluiten `exit` op de `1>` prompt.

## <a name="create-the-job-definition"></a>De taakdefinitie maken

Waar vind ik het workflow.xml beschrijving van de taakdefinitie. Ook wordt beschreven waar vind ik andere bestanden die wordt gebruikt door de werkstroom, zoals `useooziewf.hql`. Bovendien worden de waarden van eigenschappen die worden gebruikt binnen de werkstroom en de bijbehorende bestanden gedefinieerd.

1. Als u het volledige adres van de standaard-opslag, gebruikt u de volgende opdracht. Dit adres wordt gebruikt in het configuratiebestand dat u in de volgende stap maakt.

    ```bash
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Met deze opdracht retourneert informatie, zoals de volgende XML-code:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Als het HDInsight-cluster gebruikmaakt van Azure Storage als de opslag standaard de `<value>` element inhoud beginnen met `wasb://`. Als Azure Data Lake Store in plaats daarvan wordt gebruikt, deze begint met `adl://`.

    Sla de inhoud van de `<value>` element, zoals deze wordt gebruikt in de volgende stappen.

2. Gebruik de volgende opdracht voor het maken van de configuratie van Oozie taak definitie:

    ```bash
    nano job.xml
    ```

3. Nadat de nano-editor wordt geopend, gebruikt u het volgende XML-bestand als de inhoud van het bestand:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>headnodehost:8050</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * Vervang alle exemplaren van `wasb://mycontainer@mystorageaccount.blob.core.windows.net` met de waarde die u eerder hebt ontvangen voor de standaard-opslag.

     > [!WARNING]
     > Als het pad is een `wasb` pad, moet u het volledige pad gebruiken. Het net niet verkorten `wasb:///`.

   * Vervang `YourName` met de aanmeldingsnaam van uw voor het HDInsight-cluster.
   * Vervang `serverName`, `adminLogin`, en `adminPassword` met de gegevens voor de SQL-database.

     Het merendeel van de informatie in dit bestand wordt gebruikt voor het vullen van de waarden gebruikt zoals in de bestanden workflow.xml of ooziewf.hql `${nameNode}`.

     > [!NOTE]
     > De `oozie.wf.application.path` vermelding bepaalt waar het bestand workflow.xml te vinden. Dit bestand bevat de werkstroom die door deze taak is uitgevoerd.

5. Om het bestand opslaat, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

## <a name="submit-and-manage-the-job"></a>Indienen en beheren van de taak

De volgende stappen uit de Oozie-opdracht gebruiken voor het verzenden en Oozie-werkstromen op het cluster beheren. De Oozie-opdracht is een beschrijvende interface via het [Oozie REST-API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> Wanneer u de opdracht Oozie gebruikt, moet u de FQDN-naam voor het hoofdknooppunt van HDInsight. Deze FDQN-naam is alleen toegankelijk is vanaf het cluster, of als het cluster zich op een Azure-netwerk vanuit andere machines op hetzelfde netwerk.


1. Als u de URL van de service Oozie, gebruik de volgende opdracht:

    ```bash
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Hiermee wordt informatie, zoals de volgende XML-code:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    De `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` gedeelte is de URL voor gebruik met de opdracht Oozie.

2. Voor het maken van een omgevingsvariabele voor de URL, gebruikt u de volgende, zodat u hoeft deze voor elke opdracht opgeven:

    ```bash
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    Vervang de URL in met de categorie die u eerder hebt ontvangen.
3. Als u de taak, gebruikt u het volgende:

    ```bash
    oozie job -config job.xml -submit
    ```

    Met deze opdracht wordt de taakgegevens uit geladen `job.xml` en verstuurt die naar Oozie, maar wordt niet uitgevoerd.

    Nadat de opdracht is voltooid, moet er bijvoorbeeld de ID van de taak geretourneerd `0000005-150622124850154-oozie-oozi-W`. Deze ID wordt gebruikt voor het beheren van de taak.

4. Gebruik de volgende opdracht om de status van de taak, weergeven:

    ```bash
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Vervang `<JOBID>` met de ID die is geretourneerd in de vorige stap.

    Hiermee wordt informatie, zoals de volgende tekst:

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasb:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Deze taak heeft status `PREP`. Deze status geeft aan dat de taak is gemaakt, maar niet gestart.

5. Gebruik de volgende opdracht voor het starten van de taak:

    ```bash
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Vervang `<JOBID>` met de ID die eerder zijn geretourneerd.

    Als u de status na deze opdracht controleren, actief is en gegevens worden geretourneerd voor de acties in de taak.

6. Nadat de taak voltooid wordt, kunt u controleren dat de gegevens is gegenereerd en geëxporteerd naar de tabel van de SQL-database met behulp van de volgende opdracht:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    Op de `1>` gevraagd, voert u de volgende query:

    ```sql
    SELECT * FROM mobiledata
    GO
    ```

    De geretourneerde informatie is vergelijkbaar met de volgende tekst:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Zie voor meer informatie over de opdracht Oozie [Oozie-opdrachtregelprogramma](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST-API

U kunt uw eigen hulpprogramma's die met Oozie werken opbouwen met de Oozie REST-API. Hier volgt een HDInsight-specifieke informatie over het gebruik van de Oozie REST-API:

* **URI**: U hebt toegang tot de REST-API van buiten het cluster op `https://CLUSTERNAME.azurehdinsight.net/oozie`.

* **Verificatie**: om te verifiëren, de API gebruiken de HTTP-clusteraccount (admin) en het wachtwoord. Bijvoorbeeld:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Zie voor meer informatie over het gebruik van de REST-API voor Oozie [Oozie-webservices-API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie-webgebruikersinterface

De Oozie-webgebruikersinterface biedt een webweergave in de status van Oozie taken op het cluster. Met de webgebruikersinterface kunt u de volgende informatie bekijken:

   * De status van taak
   * Taakdefinitie
   * Configuratie
   * Een grafiek van de acties in de taak
   * Logboeken voor de taak

U kunt ook de details voor de acties binnen een taak weergeven.

Voor toegang tot de webgebruikersinterface Oozie, moet u de volgende stappen uitvoeren:

1. Maak een SSH-tunnel naar het HDInsight-cluster. Zie voor meer informatie [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

2. Nadat u een tunnel maakt, opent u de Ambari-webgebruikersinterface in uw webbrowser. De URI voor de site Ambari is `https://CLUSTERNAME.azurehdinsight.net`. Vervang `CLUSTERNAME` met de naam van uw Linux gebaseerde HDInsight-cluster.

3. Selecteer in de linkerkant van de pagina **Oozie** > **snelkoppelingen** > **Oozie-Webgebruikersinterface**.

    ![afbeelding van de menu 's](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. De Oozie-webgebruikersinterface standaard om de werkstroomtaken weer te geven. Selecteer de werkstroomtaken vindt **alle taken**.

    ![Alle taken die worden weergegeven](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Als u meer informatie over een taak, selecteert u de taak.

    ![Taakinformatie](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Van de **taakinformatie** tabblad ziet u de informatie over basic taak en de afzonderlijke acties in de taak. Kunt u de tabbladen boven om weer te geven de **taakdefinitie**, **taakconfiguratie**, toegang tot de **taaklogboek**, of een gerichte acyclische grafiek (DAG) van de taak onder weergeven**Taak DAG**.

   * **Taaklogboek**: Selecteer de **Get Logs** voor alle logboeken voor de taak, of gebruikt u de **zoekfilter Voer** veld voor het filteren van de logboeken.

       ![Logboekbestand van taak](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **DAG van de taak**: de DAG is een grafisch overzicht van de gegevenspaden via de werkstroom wordt uitgevoerd.

       ![Taak DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Als u een van de acties van de **taakinformatie** tabblad brengt gegevens over de actie. Selecteer bijvoorbeeld de **RunSqoopExport** in te grijpen.

    ![Actie-info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Ziet u details voor de actie, zoals een koppeling naar de **Console URL**. Gebruik deze koppeling om informatie over het vastleggen van taak voor de taak weer te geven.

## <a name="schedule-jobs"></a>Taken plannen

U kunt de coördinator gebruiken om op te geven van een start, een end en de exemplaar-frequentie voor taken. Als u wilt een schema voor de werkstroom definiëren, moet u de volgende stappen uitvoeren:

1. Gebruik de volgende opdracht voor het maken van een bestand met de naam **coordinator.xml**:

    ```bash
    nano coordinator.xml
    ```

    Gebruik de volgende XML-code als de inhoud van het bestand:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > De `${...}` variabelen zijn vervangen door waarden in de taakdefinitie tijdens runtime. De variabelen zijn:
    >
    > * `${coordFrequency}`: De tijd tussen het actieve exemplaren van de taak.
    > * `${coordStart}`: De begintijd van taak.
    > * `${coordEnd}`: De eindtijd van de taak.
    > * `${coordTimezone}`: Coordinator taken zijn in een vaste tijdzone geen zomertijd gewoonlijk weergegeven met behulp UTC. Deze tijdzone wordt verwezen als de *Oozie verwerking tijdzone.*
    > * `${wfPath}`: Het pad naar de workflow.xml.

2. Om het bestand opslaat, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

3. Om het bestand naar de werkmap voor deze taak gekopieerd, moet u de volgende opdracht gebruiken:

    ```bash
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Wijzig de `job.xml` bestand, gebruikt u de volgende opdracht:

    ```
    nano job.xml
    ```

    De volgende wijzigingen aanbrengen:

   * Instructies Oozie de coördinator-bestand in plaats van de werkstroom uit te voeren, wijzigen `<name>oozie.wf.application.path</name>` naar `<name>oozie.coord.application.path</name>`.

   * Om in te stellen de `workflowPath` variabele die wordt gebruikt door de coördinator, voeg de volgende XML-code:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Vervang de `wasb://mycontainer@mystorageaccount.blob.core.windows` tekst met de waarde in de andere vermeldingen in het bestand job.xml gebruikt.

   * Definieer de start, eindgebruikers en frequentie voor de coördinator toevoegen de volgende XML-code:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Deze waarden instellen voor de begintijd tot 12:00 uur op 10 mei 2017 en de eindtijd op 12 mei 2017. Het interval voor het uitvoeren van deze taak is ingesteld op dagelijks. De frequentie in minuten, wordt dus 24 uur x 60 minuten = 1440 minuten. Ten slotte is de tijdzone ingesteld op UTC.

5. Om het bestand opslaat, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

6. Als u wilt de taak uitvoert, moet u de volgende opdracht gebruiken:

    ```
    oozie job -config job.xml -run
    ```

    Deze opdracht worden verzonden en wordt de taak wordt gestart.

7. Wanneer u gaat naar de Oozie-webgebruikersinterface en selecteer de **coördinator taken** tabblad ziet u informatie, zoals in de volgende afbeelding:

    ![Tabblad Coordinator-taken](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    De **volgende Materialisatie** invoer bevat de volgende keer dat de taak wordt uitgevoerd.

8. Als de werkstroomtaak eerdere geeft informatie weer als u de vermelding van de taak in de webgebruikersinterface selecteert op de taak:

    ![De taakinformatie Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Deze afbeelding ziet u alleen geslaagde wordt uitgevoerd van de taak niet de afzonderlijke acties binnen de geplande werkstroom. Overzicht van de afzonderlijke acties, selecteer een van de **actie** vermeldingen.

    ![Actie-info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Problemen oplossen

U kunt met de UI Oozie Oozie logboeken weergeven. De UI Oozie bevat ook koppelingen naar de JobTracker-logboeken voor de MapReduce-taken die zijn gestart door de werkstroom. Het patroon voor het oplossen van moet zijn:

   1. De taak in Oozie-webgebruikersinterface weergeven.

   2. Als er een fout of een fout voor een specifieke actie, selecteer de actie om te controleren of de **foutbericht** veld bevat meer informatie over de fout.

   3. Indien beschikbaar, gebruikt u de URL van de actie om meer details, zoals de logboeken JobTracker voor de actie weer te geven.

Hieronder vindt u specifieke fouten die optreden en hoe u deze kunt oplossen.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Cluster kan niet worden geïnitialiseerd

**Symptomen**: de taakstatus verandert in **onderbroken**. Details voor het weergeven van de taak de `RunHiveScript` status als **START_MANUAL**. Als u de actie wordt het volgende foutbericht weergegeven:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Oorzaak**: de Azure Blob storage-adressen die worden gebruikt de **job.xml** bestand bevat niet de storage-container of de naam van het opslagaccount. De notatie van het Blob-opslag-adres moet `wasb://containername@storageaccountname.blob.core.windows.net`.

**Resolutie**: wijzigen van de adressen van de Blob-opslag die gebruikmaakt van de taak.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie mag niet worden geïmiteerd &lt;gebruiker >

**Symptomen**: de taakstatus verandert in **onderbroken**. Details voor het weergeven van de taak de `RunHiveScript` status als **START_MANUAL**. Als u de actie selecteert, ziet u het volgende foutbericht weergegeven:

    JA002: User: oozie is not allowed to impersonate <USER>

**Oorzaak**: de huidige machtigingsinstellingen Oozie imiteren van de opgegeven gebruikersaccount niet toestaan.

**Resolutie**: Oozie gebruikers kan imiteren gebruikers in de **gebruikers** groep. Gebruik de `groups USERNAME` om te zien van de groepen die lid van het gebruikersaccount is. Als de gebruiker geen lid is van is de **gebruikers** groep, gebruikt u de volgende opdracht de gebruiker wilt toevoegen aan de groep:

    sudo adduser USERNAME users

> [!NOTE]
> Het kan enkele minuten duren voordat HDInsight herkent dat de gebruiker is toegevoegd aan de groep.

### <a name="launcher-error-sqoop"></a>FOUT (Sqoop) starten

**Symptomen**: de taakstatus verandert in **KILLED**. Details voor het weergeven van de taak de `RunSqoopExport` status als **fout**. Als u de actie selecteert, ziet u het volgende foutbericht weergegeven:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Oorzaak**: Sqoop kan niet worden geladen stuurprogramma voor de database vereist voor toegang tot de database is.

**Resolutie**: wanneer u Sqoop vanaf een Oozie-taak gebruiken, moet u het stuurprogramma voor de database met de andere resources, zoals de workflow.xml, het gebruik van de taak opnemen. Ook verwijzen naar het archief waarin het stuurprogramma voor de database van de `<sqoop>...</sqoop>` sectie van de workflow.xml.

Bijvoorbeeld: voor de taak in dit document gebruikt u de volgende stappen uit:

1. Kopieer de `sqljdbc4.1.jar` van het bestand in de **zelfstudies/useoozie** directory:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Wijzig de `workflow.xml` om toe te voegen van de volgende XML-code op een nieuwe regel hierboven `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een werkstroom Oozie definiëren en hoe een Oozie-taak uit te voeren. Zie de volgende artikelen voor meer informatie over het werken met HDInsight:

* [De tijd gebaseerde Oozie-coördinator gebruiken met HDInsight][hdinsight-oozie-coordinator-time]
* [Uploaden van gegevens voor Hadoop-taken in HDInsight][hdinsight-upload-data]
* [Sqoop gebruiken met Hadoop in HDInsight][hdinsight-use-sqoop]
* [Hive gebruiken met Hadoop in HDInsight][hdinsight-use-hive]
* [Pig gebruiken met Hadoop in HDInsight][hdinsight-use-pig]
* [Het ontwikkelen van Java-MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
