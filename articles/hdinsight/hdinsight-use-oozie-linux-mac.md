---
title: Hadoop Oozie-werkstromen in Azure HDInsight op basis van Linux gebruiken
description: Gebruik Oozie met Hadoop in HDInsight op basis van Linux. Informatie over het definiëren van een Oozie-workflow en het verzenden van een Oozie-taak.
services: hdinsight
ms.service: hdinsight
ms.custom: hdinsightactive
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 02/28/2019
ms.openlocfilehash: 7fc7f63539e65618f00d75d5392ad1e96b7aab3e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533448"
---
# <a name="use-apache-oozie-with-apache-hadoop-to-define-and-run-a-workflow-on-linux-based-azure-hdinsight"></a>Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren op Azure HDInsight op basis van Linux

Leer hoe u Apache Oozie gebruiken met Apache Hadoop op Azure HDInsight. Oozie is een werkstroom en coördinatie systeem waarmee Hadoop-taken worden beheerd. Oozie is geïntegreerd met de Hadoop-stack en ondersteunt de volgende taken:

* Apache Hadoop MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

U kunt ook Oozie gebruiken voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

> [!NOTE]  
> Er is een andere optie voor het definiëren van werkstromen met HDInsight met Azure Data Factory. Zie voor meer informatie over Data Factory, [gebruik Apache Pig- en Apache Hive met Data Factory][azure-data-factory-pig-hive]. Gebruik Oozie op clusters met Enterprise-beveiligingspakket Zie [Apache Oozie uitvoeren in HDInsight Hadoop-clusters met Enterprise-beveiligingspakket](domain-joined/hdinsight-use-oozie-domain-joined-clusters.md).


## <a name="prerequisites"></a>Vereisten

* **Een Hadoop-cluster op HDInsight**. Zie [aan de slag met HDInsight op Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Een SSH-client**. Zie [verbinding maken met HDInsight (Apache Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

* **Een Azure SQL-Database**.  Zie [maken van een Azure SQL database in Azure portal](../sql-database/sql-database-get-started.md).  In dit artikel wordt een database met de naam `oozietest`.

* **Mogelijke wijziging in de opslagconfiguratie.**  Zie [opslagconfiguratie](#storage-configuration) als type opslagaccount `BlobStorage`.

## <a name="storage-configuration"></a>Opslagconfiguratie
Er is geen actie is vereist als het opslagaccount dat wordt gebruikt van het type `Storage (general purpose v1)` of `StorageV2 (general purpose v2)`.  Het proces in het artikel produceren uitvoer naar ten minste `/mapreducestaging`.  Een standaard hadoop-configuratie bevat `/mapreducestaging` in de `fs.azure.page.blob.dir` configuratievariabele van de in `core-site.xml` voor service `HDFS`.  Deze configuratie zorgt ervoor dat de uitvoer naar de map worden pagina-blobs, wat niet wordt ondersteund voor het type opslagaccount `BlobStorage`.  Gebruik `BlobStorage` voor dit artikel, verwijdert u `/mapreducestaging` uit de `fs.azure.page.blob.dir` configuratievariabele.  De configuratie is toegankelijk vanuit de [Ambari UI](/hdinsight-hadoop-manage-ambari.md).  Anders ontvangt u het foutbericht weergegeven: `Page blob is not supported for this account type.`

> [!NOTE]  
> Het opslagaccount dat wordt gebruikt in dit artikel heeft [veilige overdracht](../storage/common/storage-require-secure-transfer.md) ingeschakeld en dus `wasbs` in plaats van `wasb` wordt gebruikt in het artikel.

## <a name="example-workflow"></a>Van de voorbeeldwerkstroom

De werkstroom gebruikt in dit document bevat twee acties. Acties zijn definities voor taken, zoals het uitvoeren van Hive, Sqoop, MapReduce of andere processen:

![Diagram van werkstroom][img-workflow-diagram]

1. Een Hive-bewerking wordt een HiveQL-script om uit te pakken records uit de `hivesampletable` die is opgenomen in HDInsight. Elke rij gegevens beschrijft een bezoek van een specifieke mobiele apparaat. De recordindeling wordt weergegeven, zoals de volgende tekst:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Het Hive-script gebruikt in dit document telt het totaal aantal bezoeken voor elk platform, zoals Android of iPhone, en slaat het aantal in een nieuw Hive-tabel.

    Zie voor meer informatie over Hive [Apache Hive gebruiken met HDInsight][hdinsight-use-hive].

2. Een actie Sqoop exporteert de inhoud van de nieuwe Hive-tabel naar een tabel gemaakt in Azure SQL Database. Zie voor meer informatie over Sqoop [Apache Sqoop gebruiken met HDInsight][hdinsight-use-sqoop].

> [!NOTE]  
> Zie voor ondersteunde versies van de Oozie op HDInsight-clusters, [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight][hdinsight-versions].

## <a name="create-the-working-directory"></a>De werkmap maken

Oozie wordt verwacht dat u voor het opslaan van alle bronnen die nodig zijn voor een taak in dezelfde map. In dit voorbeeld wordt `wasbs:///tutorials/useoozie`. Voor het maken van deze map, voert u de volgende stappen uit:

1. De code hieronder om te vervangen bewerken `sshuser` met de SSH gebruiker naam voor het cluster en vervang `clustername` met de naam van het cluster.  Voer de code om verbinding met het HDInsight-cluster door [met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).  

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

2. Gebruik de volgende opdracht voor het maken van de map:

    ```bash
    hdfs dfs -mkdir -p /tutorials/useoozie/data
    ```

    > [!NOTE]  
    > De `-p` parameter zorgt ervoor dat het maken van alle mappen in het pad. De `data` directory wordt gebruikt voor het opslaan van de gegevens die worden gebruikt door de `useooziewf.hql` script.

3. De code hieronder om te vervangen bewerken `username` met de naam van de SSH-gebruiker.  Om ervoor te zorgen dat uw account kan worden geïmiteerd Oozie, gebruikt u de volgende opdracht:

    ```bash
    sudo adduser username users
    ```

    > [!NOTE]  
    > U kunt negeren fouten die wijzen op de gebruiker is al een lid van de `users` groep.

## <a name="add-a-database-driver"></a>Een databasestuurprogramma toevoegen

Omdat deze werkstroom Sqoop gebruikt gegevens te exporteren naar de SQL-database, moet u een kopie van het JDBC-stuurprogramma gebruikt om te communiceren met de SQL-database opgeven. Als u wilt het JDBC-stuurprogramma kopiëren naar de werkmap, gebruik de volgende opdracht uit de SSH-sessie:

```bash
hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc*.jar /tutorials/useoozie/
```

> [!IMPORTANT]  
> Controleer of de werkelijke JDBC-stuurprogramma dat bestaat op `/usr/share/java/`.

Als de werkstroom andere resources, zoals een jar dat een MapReduce-toepassing bevat gebruikt, moet u deze resources ook toevoegen.

## <a name="define-the-hive-query"></a>Definieer de Hive-query

Gebruik de volgende stappen uit om een Hive query language (HiveQL) script die definieert u een query te maken. U gebruikt de query in een Oozie-workflow verderop in dit document.

1. Gebruik de SSH-verbinding, de volgende opdracht om te maken van een bestand met de naam `useooziewf.hql`:

    ```bash
    nano useooziewf.hql
    ```

3. Nadat de GNU nano-editor wordt geopend, gebruikt u de volgende query uit als de inhoud van het bestand:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Er zijn twee variabelen die worden gebruikt in het script:

    * `${hiveTableName}`: Bevat de naam van de tabel moet worden gemaakt.

    * `${hiveDataFolder}`: Bevat de locatie voor het opslaan van de gegevensbestanden voor de tabel.

    De werkstroom-definitiebestand workflow.xml in deze zelfstudie, geeft deze waarden aan deze HiveQL-script uit tijdens runtime.

4. Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.  

5. Gebruik de volgende opdracht uit om te kopiëren `useooziewf.hql` naar `wasbs:///tutorials/useoozie/useooziewf.hql`:

    ```bash
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Met deze opdracht slaat de `useooziewf.hql` bestand in de HDFS-compatibele opslag voor het cluster.

## <a name="define-the-workflow"></a>De werkstroom definiëren

Oozie werkstroomdefinities worden geschreven in Hadoop proces Definition Language (hPDL), die een definition language met XML-proces. Gebruik de volgende stappen uit om de werkstroom te definiëren:

1. Gebruik de volgende instructie maken en bewerken van een nieuw bestand:

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
            <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
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

   * `RunHiveScript`: Met deze actie wordt de startactie en wordt uitgevoerd de `useooziewf.hql` Hive-script.

   * `RunSqoopExport`: Deze actie exporteert de gegevens die zijn gemaakt op basis van het Hive-script naar een SQL-database met behulp van Sqoop. Deze actie kan alleen worden uitgevoerd als de `RunHiveScript` actie is geslaagd.

     De werkstroom heeft verschillende vermeldingen, zoals `${jobTracker}`. U kunt deze vermeldingen worden vervangen door de waarden die u in de taakdefinitie. U maakt de taakdefinitie verderop in dit document.

     Let ook op de `<archive>mssql-jdbc-7.0.0.jre8.jar</archive>` vermelding in de sectie Sqoop. Deze vermelding geeft Oozie om dit archief beschikbaar maken voor Sqoop wanneer deze actie wordt uitgevoerd.

3. Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.  

4. Gebruik de volgende opdracht uit om te kopiëren de `workflow.xml` van het bestand in `/tutorials/useoozie/workflow.xml`:

    ```bash
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-a-table"></a>Een tabel maken

> [!NOTE]  
> Er zijn veel manieren om te verbinden met SQL Database om een tabel te maken. In de volgende stappen wordt [FreeTDS](http://www.freetds.org/) gebruikt vanuit het HDInsight-cluster.

1. Gebruik de volgende opdracht voor het installeren van FreeTDS op het HDInsight-cluster:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. De code hieronder om te vervangen bewerken `<serverName>` met de naam van uw Azure SQL-server en `<sqlLogin>` met de Azure SQL server-aanmelding.  Voer de opdracht verbinding maken met de vereiste SQL-database.  Voer het wachtwoord bij de prompt.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    Ontvangt u uitvoer zoals de volgende tekst:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. Voer de volgende regels in bij de prompt `1>`:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Wanneer u de instructie `GO` invoert, worden de vorige instructies geëvalueerd. Een tabel met de naam van deze instructies maken `mobiledata`, die wordt gebruikt door de werkstroom.

    Om te controleren of de tabel is gemaakt, gebruikt u de volgende opdrachten:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    U ziet uitvoer zoals de volgende tekst:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo             mobiledata      BASE TABLE

4. Het hulpprogramma tsql af te sluiten door te voeren `exit` op de `1>` prompt.

## <a name="create-the-job-definition"></a>De taakdefinitie van de maken

Waar vind ik de workflow.xml beschrijving van de taakdefinitie. Ook wordt beschreven waar u wilt zoeken naar andere bestanden die worden gebruikt door de werkstroom, zoals `useooziewf.hql`. Bovendien worden de waarden van eigenschappen die worden gebruikt binnen de werkstroom en de bijbehorende bestanden gedefinieerd.

1. Als u het volledige adres van de standaardopslag, gebruikt u de volgende opdracht uit. Dit adres wordt gebruikt in het configuratiebestand dat u in de volgende stap maakt.

    ```bash
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Met deze opdracht retourneert informatie, zoals de volgende XML:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]  
    > Als het HDInsight-cluster maakt gebruik van Azure Storage als de standaardopslag, de `<value>` element inhoud beginnen met `wasbs://`. Als Azure Data Lake Storage Gen1 in plaats daarvan wordt gebruikt, deze begint met `adl://`. Als Azure Data Lake Storage Gen2 wordt gebruikt, deze begint met `abfs://`.

    Sla de inhoud van de `<value>` -element, zoals deze wordt gebruikt in de volgende stappen.

2. Bewerk het volgende XML-bestand als volgt:

    |Tijdelijke aanduiding| Waarde vervangen|
    |---|---|
    |wasbs://mycontainer@mystorageaccount.blob.core.windows.net| De waarde van stap 1 hebt ontvangen.|
    |beheerder| Uw aanmeldingsnaam voor de HDInsight-cluster als dat niet-beheerder.|
    |Servernaam| Azure SQL database-servernaam.|
    |sqlLogin| Azure SQL database server-aanmelding.|
    |sqlPassword| Azure SQL database aanmeldingswachtwoord voor server.|

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
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
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=sqlLogin;password=sqlPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>admin</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

    De meeste van de informatie in dit bestand wordt gebruikt voor het vullen van de waarden in de workflow.xml of ooziewf.hql-bestanden, zoals `${nameNode}`.  Als het pad is een `wasbs` pad, moet u het volledige pad gebruiken. Geen korter maken om alleen `wasbs:///`. De `oozie.wf.application.path` vermelding bepaalt waar het bestand workflow.xml te vinden. Dit bestand bevat de werkstroom die door deze taak is uitgevoerd.

3. Gebruik de volgende opdracht voor het maken van de configuratie van de definitie Oozie:

    ```bash
    nano job.xml
    ```

4. Nadat de nano-editor wordt geopend, plakt u het bewerkte XML-bestand als de inhoud van het bestand.

5. Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

## <a name="submit-and-manage-the-job"></a>Indienen en beheren van de taak

De volgende stappen gebruikt de opdracht Oozie indienen en beheren van Oozie-werkstromen op het cluster. De Oozie-opdracht is een gebruiksvriendelijke interface via het [Oozie REST-API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]  
> Wanneer u de opdracht Oozie gebruiken, moet u de FQDN-naam gebruiken voor het hoofdknooppunt van HDInsight. Deze FQDN is alleen toegankelijk is vanaf het cluster, of als het cluster zich in een Azure-netwerk, van andere virtuele machines op hetzelfde netwerk.

1. Als u de URL naar de service Oozie, gebruik de volgende opdracht:

    ```bash
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Dit retourneert informatie, zoals de volgende XML:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    De `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` gedeelte is de URL voor gebruik met de opdracht Oozie.

2. De code bewerken om de URL vervangen door de reeks die u eerder hebt ontvangen. Voor het maken van een omgevingsvariabele voor de URL, gebruikt u de volgende, zodat u niet hoeft op te geven voor elke opdracht:

    ```bash
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

3. Als u wilt verzenden van de taak, gebruikt u het volgende:

    ```bash
    oozie job -config job.xml -submit
    ```

    Met deze opdracht wordt de informatie over de taak van geladen `job.xml` en verzendt deze naar Oozie, maar wordt niet uitgevoerd.

    Nadat de opdracht is voltooid, moet deze de ID van de taak, bijvoorbeeld retourneren `0000005-150622124850154-oozie-oozi-W`. Deze ID wordt gebruikt voor het beheren van de taak.

4. De code hieronder om te vervangen bewerken `<JOBID>` met de ID die wordt geretourneerd in de vorige stap.  Als u wilt weergeven van de status van de taak, gebruik de volgende opdracht:

    ```bash
    oozie job -info <JOBID>
    ```

    Dit retourneert informatie, zoals de volgende tekst:

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

    Deze taak heeft de status van `PREP`. Deze status geeft aan dat de taak is gemaakt, maar niet is gestart.

5. De code hieronder om te vervangen bewerken `<JOBID>` met de ID die eerder zijn geretourneerd.  Gebruik de volgende opdracht voor het starten van de taak:

    ```bash
    oozie job -start JOBID
    ```

    Als u de status na deze opdracht controleren, deel uitmaakt van een actieve status en gegevens worden geretourneerd voor de acties in de taak.  De taak duurt enkele minuten om te voltooien.

6. De code hieronder om te vervangen bewerken `<serverName>` met de naam van uw Azure SQL-server en `<sqlLogin>` met de Azure SQL server-aanmelding.  Nadat de taak voltooid is, kunt u controleren dat de gegevens is gegenereerd en geëxporteerd naar de tabel van de SQL-database met behulp van de volgende opdracht uit.  Voer het wachtwoord bij de prompt.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    Op de `1>` wordt gevraagd, voert u de volgende query:

    ```sql
    SELECT * FROM mobiledata
    GO
    ```

    De informatie die wordt geretourneerd is vergelijkbaar met de volgende tekst:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Zie voor meer informatie over de opdracht Oozie [Apache Oozie-opdrachtregelprogramma](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API

U kunt uw eigen hulpprogramma's die met Oozie werken bouwen met de Oozie REST-API. Hier volgt een HDInsight-specifieke informatie over het gebruik van de REST-API voor Oozie:

* **URI**: U hebt toegang tot de REST-API van buiten het cluster op `https://CLUSTERNAME.azurehdinsight.net/oozie`.

* **Verificatie**: Als u wilt verifiëren, moet u de API gebruiken de HTTP-clusteraccount (beheerder) en het wachtwoord. Bijvoorbeeld:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Zie voor meer informatie over het gebruik van de REST-API voor Oozie [Apache Oozie-webservices-API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie-Webgebruikersinterface

De Oozie-webgebruikersinterface biedt webgebaseerde inzicht in de status van Oozie-taken op het cluster. Met de webgebruikersinterface kunt u de volgende informatie bekijken:

   * Taakstatus
   * Jobdefinitie
   * Configuratie
   * Een grafiek van de acties in de taak
   * Logboeken voor de taak

U kunt ook de details voor de acties binnen een project weergeven.

Voor toegang tot de Oozie-web-UI, voert u de volgende stappen uit:

1. Maak een SSH-tunnel naar het HDInsight-cluster. Zie voor meer informatie, [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

2. Nadat u een tunnel maakt, opent u de Ambari-Webgebruikersinterface in uw webbrowser met behulp van URI `http://headnodehost:8080`.

3. Selecteer in de linkerkant van de pagina **Oozie** > **snelkoppelingen** > **Oozie-Webgebruikersinterface**.

    ![Afbeelding van de menu 's](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. De web-UI van Oozie standaard om weer te geven van de werkstroomtaken die worden uitgevoerd. De werkstroomtaken Selecteer **alle taken**.

    ![Alle taken die worden weergegeven](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Als u meer informatie over een taak, selecteert u de taak.

    ![Taakgegevens](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Uit de **taakgegevens** tabblad ziet u de informatie over de algemene taak en de afzonderlijke acties in de taak. U kunt de tabbladen aan de bovenkant om weer te geven de **taakdefinitie**, **taakconfiguratie**, toegang de **taaklogboek**, of weergeven van een directed acyclic graph (DAG) van de taak onder **Taak DAG**.

   * **Taak Log**: Selecteer de **Get Logs** om op te halen van alle logboeken voor de taak, of gebruikt u de **zoekfilter Voer** veld voor het filteren van de logboeken.

       ![Taaklogboek](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **Job DAG**: De DAG is een grafische overzicht van de gegevenspaden via de werkstroom wordt uitgevoerd.

       ![Job DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Als u een van de acties op basis van de **taakgegevens** tabblad, brengt dit van informatie voor de actie. Selecteer bijvoorbeeld de **RunSqoopExport** actie.

    ![Actie-informatie](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Ziet u details voor de actie, zoals een koppeling naar de **Console-URL**. Gebruik deze koppeling om informatie over het vastleggen van taak voor de taak weer te geven.

## <a name="schedule-jobs"></a>Taken plannen

U kunt de coördinator gebruiken om op te geven van een starten, een end en de frequentie van de gebeurtenis voor taken. Als u wilt een schema voor de werkstroom definiëren, voert u de volgende stappen uit:

1. Gebruik de volgende opdracht om te maken van een bestand met de naam **coordinator.xml**:

    ```bash
    nano coordinator.xml
    ```

    Gebruik het volgende XML-bestand als de inhoud van het bestand:

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
    > De `${...}` variabelen zijn vervangen door waarden in de taakdefinitie tijdens runtime. De variabelen die zijn:
    >
    > * `${coordFrequency}`: De tijd tussen het uitvoeren van exemplaren van de taak.
    > * `${coordStart}`: De begintijd van taak.
    > * `${coordEnd}`: De eindtijd van taak.
    > * `${coordTimezone}`: Coördinator taken zijn in een vaste tijdzone geen zomertijd, meestal met behulp van UTC weergegeven. Deze tijdzone wordt verwezen als de *Oozie verwerking over de tijdzone.*
    > * `${wfPath}`: Het pad naar de workflow.xml.

2. Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

3. Als u wilt kopiëren van het bestand naar de werkmap voor deze taak, gebruik de volgende opdracht:

    ```bash
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Wijzig de `job.xml` bestand dat u eerder hebt gemaakt, gebruikt u de volgende opdracht:

    ```bash
    nano job.xml
    ```

    Breng de volgende wijzigingen aan:

   * Instructies Oozie de coördinator-bestand in plaats van de werkstroom uit te voeren, wijzigen `<name>oozie.wf.application.path</name>` naar `<name>oozie.coord.application.path</name>`.

   * Om in te stellen de `workflowPath` variabele die wordt gebruikt door de coördinator, voeg de volgende XML:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Vervang de `wasbs://mycontainer@mystorageaccount.blob.core.windows` tekst met de waarde die wordt gebruikt in de andere vermeldingen in het bestand job.xml.

   * Voeg het volgende XML-bestand voor het definiëren van het begin, einde en frequentie voor de coördinator:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2018-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2018-05-12T12:00Z</value>
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

       Deze waarden ingesteld voor de begintijd tot 12:00 op 10 mei 2018, en de eindtijd op 12 mei 2018. Het interval voor het uitvoeren van deze taak is ingesteld op dagelijks. De frequentie in minuten, wordt dus 24 uur x 60 minuten = 1440 minuten. Ten slotte is de tijdzone ingesteld op UTC.

5. Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

6. Als u wilt verzenden en start de taak, gebruik de volgende opdracht:

    ```bash
    oozie job -config job.xml -run
    ```

7. Als u gaat u naar de Oozie-Webinterface en selecteer de **Coordinator taken** tabblad ziet u informatie, zoals in de volgende afbeelding:

    ![Tabblad Coordinator-taken](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    De **volgende Materialisatie** vermelding bevat de volgende keer dat de taak wordt uitgevoerd.

8. Als de eerdere werkstroomtaak geeft informatie weer als u de vermelding van de taak in de webgebruikersinterface selecteert op de taak:

    ![Coördinator taakgegevens](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]  
    > Deze afbeelding ziet u alleen geslaagde uitvoeringen van de taak niet de afzonderlijke acties in de geplande werkstroom. Als u wilt zien van de afzonderlijke acties, selecteer een van de **actie** vermeldingen.

    ![Actie-informatie](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Problemen oplossen

U kunt met de UI Oozie Oozie logboeken weergeven. De UI Oozie bevat ook koppelingen naar de JobTracker-logboeken voor de MapReduce-taken die zijn gestart door de werkstroom. Het patroon voor het oplossen van moet zijn:

   1. De taak weergeven in Oozie-Webgebruikersinterface.

   2. Als er een fout of een fout voor een bepaalde actie, selecteer de actie om te controleren of de **foutbericht** veld vindt u meer informatie over de fout.

   3. Als deze beschikbaar is, gebruikt u de URL van de actie voor meer informatie, zoals de JobTracker-logboeken voor de actie.

Hier volgen specifieke fouten die optreden en hoe u ze op te lossen.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Kan het cluster niet initialiseren.

**Symptomen**: De taakstatus verandert in **onderbroken**. Details voor het weergeven van de taak de `RunHiveScript` status als **START_MANUAL**. De bewerking te selecteren, wordt het volgende foutbericht weergegeven:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Oorzaak**: De Azure Blob-opslag-adressen die worden gebruikt de **job.xml** bestand geen het storage-container of de naam van het opslagaccount. De indeling van Blob storage-adres moet `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Oplossing**: De Blob storage-adressen die gebruikmaakt van de taak wijzigen.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltusergt"></a>JA002: Oozie is niet toegestaan om te imiteren &lt;gebruiker&gt;

**Symptomen**: De taakstatus verandert in **onderbroken**. Details voor het weergeven van de taak de `RunHiveScript` status als **START_MANUAL**. Als u de actie selecteert, ziet u de volgende strekking weergegeven:

    JA002: User: oozie is not allowed to impersonate <USER>

**Oorzaak**: Instellingen voor de huidige machtigingen toestaan niet Oozie te imiteren van het opgegeven gebruikersaccount.

**Oplossing**: Oozie kan imiteren gebruikers in de **gebruikers** groep. Gebruik de `groups USERNAME` om te zien van de groepen die het gebruikersaccount dat lid is van is. Als de gebruiker geen lid is van is de **gebruikers** groep, gebruikt u de volgende opdracht uit om toe te voegen van de gebruiker aan de groep:

    sudo adduser USERNAME users

> [!NOTE]  
> Het kan enkele minuten duren voordat HDInsight herkent dat de gebruiker is toegevoegd aan de groep.

### <a name="launcher-error-sqoop"></a>FOUT (Sqoop) starten

**Symptomen**: De taakstatus verandert in **KILLED**. Details voor het weergeven van de taak de `RunSqoopExport` status als **fout**. Als u de actie selecteert, ziet u de volgende strekking weergegeven:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Oorzaak**: Sqoop is kan niet worden geladen stuurprogramma voor de database vereist voor toegang tot de database.

**Oplossing**: Wanneer u Sqoop van een taak Oozie gebruiken, moet u een stuurprogramma voor de database met de andere resources, zoals de workflow.xml, de taak gebruikt opnemen. Ook verwijzen naar het archief met het stuurprogramma voor de database van de `<sqoop>...</sqoop>` sectie van de workflow.xml.

Bijvoorbeeld, voor de taak in dit document, moet u de volgende stappen gebruiken:

1. Kopieer de `mssql-jdbc-7.0.0.jre8.jar` van het bestand in de **/zelfstudies/useoozie** directory:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Wijzig de `workflow.xml` om toe te voegen van het volgende XML-bestand op een nieuwe regel hierboven `</sqoop>`:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Oozie-workflow definieert en hoe u een Oozie-taak uit te voeren. Zie de volgende artikelen voor meer informatie over het werken met HDInsight:

* [De tijd op basis van Apache Oozie-coördinator gebruiken met HDInsight][hdinsight-oozie-coordinator-time]
* [Gegevens uploaden voor Apache Hadoop-taken in HDInsight][hdinsight-upload-data]
* [Apache Sqoop gebruiken met Apache Hadoop in HDInsight][hdinsight-use-sqoop]
* [Apache Hive gebruiken met Apache Hadoop op HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met Apache Hadoop op HDInsight][hdinsight-use-pig]
* [Java MapReduce-programma's ontwikkelen voor HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563
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

[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
