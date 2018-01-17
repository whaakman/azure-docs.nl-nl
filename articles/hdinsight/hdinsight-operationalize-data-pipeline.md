---
title: Een data analytics pipeline - Azure operationeel | Microsoft Docs
description: Instellen en uitvoeren van een voorbeeld van de gegevens-pijplijn die wordt geactiveerd door nieuwe gegevens en beknopte resultaten oplevert.
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
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 7a439c9d25a470a2474b427f6b20addb6ff3b0c7
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Een pijplijn van gegevens analytics operationeel maken

*Gegevenspijplijnen* rela‑ veel gegevens analytics-oplossingen. Zoals de naam al aangeeft, een pijplijn gegevens wordt in de onbewerkte gegevens, schoongemaakt en verwijdert deze zo nodig en vervolgens voert berekeningen of aggregaties voordat verwerkte gegevens op te slaan. Verwerkte gegevens wordt gebruikt door clients, rapporten of API's. Een pijplijn gegevens moet herhaalbare resultaten opgeven of volgens een planning of wanneer deze worden geactiveerd door nieuwe gegevens.

In dit artikel wordt beschreven hoe uw gegevenspijplijnen voor herhaalbaarheid, Oozie uitgevoerd op een HDInsight Hadoop-clusters met operationeel. Het voorbeeldscenario leert u een pijplijn met gegevens die wordt voorbereid en luchtvaartmaatschappij vlucht timeseries gegevens worden verwerkt.

In het volgende scenario is de invoergegevens een plat bestand met een reeks vluchtgegevens voor één maand. Deze overdracht gegevens omvatten informatie zoals de herkomst en de bestemming luchthaven, de mijl flown, het vertrek en tijden van de aankomst, enzovoort. Het doel met deze pipeline is om samen te vatten dagelijkse luchtvaartmaatschappij prestaties, waarbij elke luchtvaartmaatschappij één rij voor elke dag met de gemiddelde vertrek- en vertragingen in minuten, en het totaal aantal kilometers flown die dag heeft.

| JAAR | MAAND | DAY_OF_MONTH | PROVIDER |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | AA | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | DL | 6.935409 | -2.1893024 | 1909696 |

De voorbeeld-pijplijn wacht totdat een nieuwe periode vluchtgegevens binnenkomt en die informatie gedetailleerde vlucht in uw Hive-datawarehouse voor de lange termijn analyses slaat. De pijplijn maakt ook een veel kleinere gegevensset die alleen de dagelijkse vluchtgegevens worden samengevat. Deze dagelijkse samenvatting vluchtgegevens is verzonden naar een SQL-database te voorzien in rapporten, zoals een website.

Het volgende diagram illustreert de voorbeeld-pijplijn.

![Vlucht Data Pipeline](./media/hdinsight-operationalize-data-pipeline/pipeline-overview.png)

## <a name="oozie-solution-overview"></a>Overzicht van de oplossing Oozie

Apache Oozie uitgevoerd op een HDInsight Hadoop-cluster maakt gebruik van deze pijplijn.

Oozie beschrijft de pijplijnen in termen van *acties*, *werkstromen*, en *coördinator*. Acties bepalen het echte werk uit te voeren, zoals het uitvoeren van een Hive-query. Werkstromen definiëren de volgorde van de acties. Coördinator Definieer de planning voor wanneer de werkstroom wordt uitgevoerd. Coördinator kunnen ook wachten op de beschikbaarheid van nieuwe gegevens voordat een exemplaar van de werkstroom wordt gestart.

Het volgende diagram toont het ontwerp op hoog niveau van dit voorbeeld Oozie-pijplijn.

![Oozie vluchtgegevens Pipeline](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

### <a name="provision-azure-resources"></a>Azure-resources inrichten

Deze pipeline is vereist voor een Azure SQL Database en een HDInsight Hadoop-cluster op dezelfde locatie bevinden. De Azure SQL Database slaat de samenvatting gegevens geproduceerd door de pijplijn en de store Oozie-metagegevens.

#### <a name="provision-azure-sql-database"></a>Inrichten van Azure SQL Database

1. Een nieuwe resourcegroep met de naam met de Azure portal maken `oozie` bevat alle resources die door dit voorbeeld gebruikt.
2. Binnen de `oozie` resourcegroep, het inrichten van een Azure SQL-Server en Database. U hoeft niet een database groter is dan de Standard-S1 prijscategorie.
3. Met de Azure portal, gaat u naar het deelvenster voor uw zojuist geïmplementeerde SQL-Database en selecteer **extra**.

    ![Knop Extra](./media/hdinsight-operationalize-data-pipeline/sql-db-tools.png)

4. Selecteer **Query-Editor**.

    ![Knop voor query-Editor](./media/hdinsight-operationalize-data-pipeline/sql-db-query-editor.png)

5. In de **Query-Editor** deelvenster **aanmelding**.

    ![Knop aanmelden](./media/hdinsight-operationalize-data-pipeline/sql-db-login1.png)

6. Voer uw referenties in SQL-Database en selecteer **OK**.

   ![Aanmeldformulier](./media/hdinsight-operationalize-data-pipeline/sql-db-login2.png)

7. Voer in het tekstgebied Query-Editor de volgende SQL-instructies voor het maken de `dailyflights` tabel de samengevatte gegevens van elke uitvoeren van de pijplijn worden opgeslagen.

    ```
    CREATE TABLE dailyflights
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER CHAR(2),
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    GO

    CREATE CLUSTERED INDEX dailyflights_clustered_index on dailyflights(YEAR,MONTH,DAY_OF_MONTH,CARRIER)
    GO
    ```

8. Selecteer **uitvoeren** uit te voeren van de SQL-instructies.

    ![Knop uitvoeren](./media/hdinsight-operationalize-data-pipeline/sql-db-run.png)

Uw Azure SQL Database is nu klaar.

#### <a name="provision-an-hdinsight-hadoop-cluster"></a>Een HDInsight Hadoop-Cluster inrichten

1. Selecteer in de Azure-portal **+ nieuw** en zoek naar HDInsight.
2. Selecteer **Maken**.
3. Geef een unieke naam voor uw cluster in het deelvenster basisbeginselen en kiest u uw Azure-abonnement.

    ![De naam van de HDInsight-cluster en -abonnement](./media/hdinsight-operationalize-data-pipeline/hdi-name-sub.png)

4. In de **type Cluster** deelvenster, selecteer de **Hadoop** type cluster **Linux** besturingssysteem en de nieuwste versie van het HDInsight-cluster. Laat de **Cluster laag** op **standaard**.

    ![HDInsight-clustertype](./media/hdinsight-operationalize-data-pipeline/hdi-cluster-type.png)

5. Kies **Selecteer** toepassen van de selectie van uw cluster.
6. Voltooi de **basisbeginselen** deelvenster verstrekken van een aanmeldingswachtwoord en selecteert u uw `oozie` resource groep uit de lijst en selecteer vervolgens **volgende**.

    ![HDInsight-basisbeginselen deelvenster](./media/hdinsight-operationalize-data-pipeline/hdi-basics.png)

7. In de **opslag** deelvenster, laat de primaire opslagtype ingesteld op **Azure Storage**, selecteer **nieuw**, en geef een naam voor de nieuwe account.

    ![HDInsight-opslag-Accountinstellingen](./media/hdinsight-operationalize-data-pipeline/hdi-storage.png)

8. Voor de **Metastore instellingen**onder **selecteert u een SQL-database voor Hive**, kiest u de database die u eerder hebt gemaakt.

    ![Instellingen voor HDInsight Hive-Metastore](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-hive.png)

9. Selecteer **verifiëren van de SQL-Database**.

    ![HDInsight Hive-Metastore verifiëren](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql.png)

10. Voer uw SQL database-gebruikersnaam en wachtwoord in en kies **Selecteer**. 

       ![HDInsight Hive-Metastore aanmelding verifiëren](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql-login.png)

11. Terug op de **Metastore instellingen** deelvenster, selecteer uw database voor de Oozie-metagegevens opslaan en verifiëren als u eerder hebt gedaan. 

       ![Instellingen voor HDInsight-Metastore](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-settings.png)

12. Selecteer **volgende**.
13. Op de **samenvatting** deelvenster **maken** voor het implementeren van uw cluster.

### <a name="verify-ssh-tunneling-setup"></a>Controleer of de SSH-tunneling setup

Instellen voor het gebruik van de webconsole Oozie om de status van de coördinator en workflow-exemplaren weer te geven, een SSH-tunnel met uw HDInsight-cluster. Zie voor meer informatie [SSH-Tunnel](hdinsight-linux-ambari-ssh-tunnel.md).

> [!NOTE]
> U kunt ook Chrome met de [Foxy Proxy](https://getfoxyproxy.org/) uitbreiding van uw cluster webbronnen bladeren in voor de SSH-tunnel. Alle aanvragen via de host configureren naar proxy `localhost` op tunnelpoort 9876. Deze aanpak is compatibel met het Windows-subsysteem voor Linux, ook wel bekend als Bash op Windows 10.

1. Voer de volgende opdracht om te openen, een SSH-tunnel naar het cluster:

    ```
    ssh -C2qTnNf -D 9876 sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
    ```

2. Controleer of dat de tunnel door te navigeren naar de Ambari op uw hoofdknooppunt door te bladeren naar operationeel is:

    http://headnodehost:8080

3. Voor toegang tot de **Oozie-webconsole** binnen Ambari, selecteer **Oozie**, **snelkoppelingen**, en selecteer vervolgens **Oozie-webconsole**.

### <a name="configure-hive"></a>Hive configureren

1. Een voorbeeld van de CSV-bestand downloaden dat vluchtgegevens één maand bevat. Het ZIP-bestand downloaden `2017-01-FlightData.zip` van de [HDInsight Github-opslagplaats](https://github.com/hdinsight/hdinsight-dev-guide) en pak deze naar de CSV-bestand `2017-01-FlightData.csv`. 

2. Kopieer dit CSV-bestand naar de Azure Storage-account dat is gekoppeld aan uw HDInsight-cluster op en plaats deze in de `/example/data/flights` map.

U kunt het bestand kopiëren met SCP in uw `bash` sessie-shell.

1. SCP kopiëren van de bestanden vanuit uw lokale computer naar de lokale opslag van het hoofdknooppunt van uw HDInsight-cluster gebruiken.

    ```bash
    scp ./2017-01-FlightData.csv sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:2017-01-FlightData.csv
    ```

2. Gebruik de HDFS-opdracht voor het bestand kopiëren vanuit uw lokale opslag van hoofdknooppunt naar Azure Storage.

    ```bash
    hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
    ```

De voorbeeldgegevens is nu beschikbaar. De pijplijn vereist echter twee Hive-tabellen voor verwerking, één voor de binnenkomende gegevens (`rawFlights`) en één voor de samengevatte gegevens (`flights`). Deze tabellen als volgt in Ambari maken.

1. Meld u aan bij Ambari door te navigeren naar [http://headnodehost:8080](http://headnodehost:8080).
2. Selecteer in de lijst met services **Hive**.

    ![Hive in Ambari selecteren](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Selecteer **te bekijken** naast het Hive-weergave 2.0-label.

    ![Hive-weergave selecteren in Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Plak de volgende instructies voor het maken in het gebied van de tekst query de `rawFlights` tabel. De `rawFlights` tabel bevat een schema op de leesbewerking voor de CSV-bestanden in de `/example/data/flights` map in Azure Storage. 

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS rawflights (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    ) 
    LOCATION '/example/data/flights'
    ```

5. Selecteer **Execute** om de tabel te maken.

    ![In de Ambari hive-Query](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-query.png)

6. Maken van de `flights` tabel, vervang de tekst in het gebied van de tekst query door de volgende instructies. De `flights` tabel is een beheerde Hive-tabel met partities gegevens geladen voor het jaar, maand en dag van de maand. Deze tabel bevat alle historische vluchtgegevens, met de laagste granulatie aanwezig in de brongegevens van één rij per overdracht.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;

    CREATE TABLE flights
    (
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT
    )
    PARTITIONED BY (YEAR INT, MONTH INT, DAY_OF_MONTH INT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    );
    ```

7. Selecteer **Execute** om de tabel te maken.

### <a name="create-the-oozie-workflow"></a>De werkstroom Oozie maken

Gegevens in batches verwerken pijplijnen doorgaans met een bepaald tijdsinterval. In dit geval wordt verwerkt de pijplijn de vluchtgegevens dagelijks. Deze aanpak kunt u de CSV-invoerbestanden dagelijks, wekelijks, maandelijks of jaarlijks aankomen.

De voorbeeldwerkstroom verwerkt de vlucht gegevens per dag, in drie belangrijke stappen:

1. Uitvoeren van een Hive-query om de gegevens voor die dag datumbereik extraheren uit de bron-CSV-bestand dat wordt vertegenwoordigd door de `rawFlights` tabel en voeg de gegevens in de `flights` tabel.
2. Een Hive-query voor het maken van een faseringstabel dynamisch in component voor de dag waarop een kopie van de vluchtgegevens samengevat per dag en carriernaam uitvoeren.
3. Apache Sqoop gebruiken alle gegevens van de dagelijkse faseringstabel in Component kopiëren naar de bestemming `dailyflights` tabel in Azure SQL Database. Sqoop leest de bronrijen van de gegevens achter de Hive-tabel die zich in Azure Storage en worden ze in SQL-Database met een JDBC-verbinding.

Deze drie stappen worden gecoördineerd door een Oozie-werkstroom. 

1. Een query maken in het bestand `hive-load-flights-partition.hql`.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;
    
    INSERT OVERWRITE TABLE flights
    PARTITION (YEAR, MONTH, DAY_OF_MONTH)
    SELECT  
        FL_DATE,
        CARRIER,
        FL_NUM,
        ORIGIN,
        DEST,
        DEP_DELAY,
        ARR_DELAY,
        ACTUAL_ELAPSED_TIME,
        DISTANCE,
        YEAR,
        MONTH,
        DAY_OF_MONTH
    FROM rawflights
    WHERE year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Gebruik de syntaxis van Oozie variabelen `${variableName}`. Deze variabelen worden ingesteld in de `job.properties` bestand zoals beschreven in de volgende stap. Oozie vervangt door de werkelijke waarden tijdens runtime.

2. Een query maken in het bestand `hive-create-daily-summary-table.hql`.

    ```
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER STRING,
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName}
    SELECT  year, month, day_of_month, carrier, avg(dep_delay) avg_dep_delay, 
            avg(arr_delay) avg_arr_delay, sum(distance) total_distance 
    FROM flights
    GROUP BY year, month, day_of_month, carrier 
    HAVING year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Deze query maakt een tijdelijke tabel die alleen de samengevatte gegevens opslaan voor één dag, dient u de instructie SELECT die berekent de gemiddelde vertraging en het totaal van afstand flown door carrier per dag. De gegevens die zijn opgenomen in deze tabel die is opgeslagen op een bekende locatie (pad aangegeven door de variabele hiveDataFolder) zodat deze kan worden gebruikt als de bron voor Sqoop in de volgende stap.

3. Voer de volgende Sqoop-opdracht.

    ```
    sqoop export --connect ${sqlDatabaseConnectionString} --table ${sqlDatabaseTableName} --export-dir ${hiveDataFolder} -m 1 --input-fields-terminated-by "\t"
    ```

Deze drie stappen worden uitgedrukt in drie afzonderlijke acties in het volgende bestand in de werkstroom Oozie, met de naam `workflow.xml`.

```
<workflow-app name="loadflightstable" xmlns="uri:oozie:workflow:0.5">
    <start to = "RunHiveLoadFlightsScript"/>
    <action name="RunHiveLoadFlightsScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptLoadPartition}</script>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
        </hive>
        <ok to="RunHiveCreateDailyFlightTableScript"/>
        <error to="fail"/>
    </action>

    <action name="RunHiveCreateDailyFlightTableScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptCreateDailyTable}</script>
            <param>hiveTableName=${hiveDailyTableName}</param>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
            <param>hiveDataFolder=${hiveDataFolder}/${year}/${month}/${day}</param>
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
            <arg>${hiveDataFolder}/${year}/${month}/${day}</arg>
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

De twee Hive-query's worden gebruikt door het bijbehorende pad in Azure Storage en de resterende waarden van variabelen worden geleverd door de volgende `job.properties` bestand. Dit bestand configureert u de werkstroom voor de datum 3 januari 2017 worden uitgevoerd.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.wf.application.path=${appBase}/load_flights_by_day
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableName=dailyflights${year}${month}${day}
hiveDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/${year}/${month}/${day}
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights
year=2017
month=01
day=03
```

De volgende tabel geeft een overzicht van elk van de eigenschappen en geeft aan waar u de waarden voor uw eigen omgeving kunt vinden.

| Eigenschap | Waardebron |
| --- | --- |
| nameNode | Het volledige pad naar de Azure Storage-Container is gekoppeld aan uw HDInsight-cluster. |
| jobTracker | De hostnaam van de interne aan uw actieve cluster YARN head knooppunt. Op de startpagina Ambari YARN selecteren in de lijst met services en kies vervolgens Active Resource Manager. De hostnaam van de URI wordt weergegeven boven aan de pagina. De poort 8050 toevoegen. |
| queueName | De naam van de YARN-wachtrij gebruikt bij het plannen van de Hive-acties. Laat als standaard. |
| oozie.use.system.libpath | Laat de eigenschap als waar. |
| appBase | Het pad naar de submap in Azure Storage waar u de werkstroom Oozie en ondersteunende bestanden implementeren. |
| oozie.WF.Application.Path | De locatie van de werkstroom Oozie `workflow.xml` om uit te voeren. |
| hiveScriptLoadPartition | Het pad in Azure Storage naar het Hive-query-bestand `hive-load-flights-partition.hql`. |
| hiveScriptCreateDailyTable | Het pad in Azure Storage naar het Hive-query-bestand `hive-create-daily-summary-table.hql`. |
| hiveDailyTableName | De dynamisch gegenereerde naam moet worden gebruikt voor de faseringstabel. |
| hiveDataFolder | Het pad naar de gegevens in de faseringstabel in Azure Storage. |
| sqlDatabaseConnectionString | De JDBC syntaxis van de verbindingsreeks naar uw Azure SQL Database. |
| sqlDatabaseTableName | De naam van de tabel in Azure SQL Database waarin overzicht in rijen zijn ingevoegd. Behoud `dailyflights`. |
| jaar | Het jaargedeelte van de dag voor welke vlucht samenvattingen worden berekend. Niet wijzigen. |
| maand | Het maandgedeelte van de dag voor welke vlucht samenvattingen worden berekend. Niet wijzigen. |
| dag | De dag van het onderdeel van de maand van de dag voor welke vlucht samenvattingen worden berekend. Niet wijzigen. |

> [!NOTE]
> Zorg ervoor dat uw exemplaar van de `job.properties` bestand met de waarden die specifiek zijn voor uw omgeving voordat u kunt implementeren en uitvoeren van uw Oozie-werkstroom.

### <a name="deploy-and-run-the-oozie-workflow"></a>Implementeren en uitvoeren van de werkstroom Oozie

SCP uit uw sessie bash gebruiken voor het implementeren van uw Oozie-werkstroom (`workflow.xml`), de Hive-query's (`hive-load-flights-partition.hql` en `hive-create-daily-summary-table.hql`) en de taakconfiguratie van de (`job.properties`).  In Oozie, alleen de `job.properties` bestand kan bestaan op de lokale opslag van de headnode. Alle andere bestanden moeten worden opgeslagen in HDFS in dit geval Azure Storage. De Sqoop-actie die wordt gebruikt door de werkstroom, is afhankelijk van een JDBC-stuurprogramma voor communicatie met uw SQL-Database moeten worden gekopieerd vanaf het hoofdknooppunt van naar HDFS.

1. Maak de `load_flights_by_day` submap onder het pad van de gebruiker in de lokale opslag van het hoofdknooppunt.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 'mkdir load_flights_by_day'

2. Kopieer alle bestanden in de huidige map (de `workflow.xml` en `job.properties` bestanden) tot de `load_flights_by_day` submap.

        scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:load_flights_by_day

3. Voeg SSH toe aan uw hoofdknooppunt en navigeer naar de `load_flights_by_day` map.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
        cd load_flights_by_day

4. Kopieer de Werkstroombestanden naar HDFS.

        hdfs dfs -put ./* /oozie/load_flights_by_day

5. Kopiëren `sqljdbc41.jar` uit het lokale hoofdknooppunt naar de werkstroommap in HDFS:

        hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /oozie/load_flights_by_day

6. De werkstroom wordt uitgevoerd.

        oozie job -config job.properties -run

7. Houd rekening met de status van de Oozie-webconsole. Binnen Ambari, selecteer **Oozie**, **snelkoppelingen**, en vervolgens **Oozie-webconsole**. Onder de **werkstroomtaken** tabblad **alle taken**.

    ![Oozie Web Console-werkstromen](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

8. Wanneer de status geslaagd query uitvoeren op de SQL-databasetabel om de ingevoegde rijen weer te geven. Met de Azure portal, gaat u naar het deelvenster voor uw SQL-Database selecteren **extra**, en open de **Query-Editor**.

        SELECT * FROM dailyflights

Nu dat de werkstroom wordt uitgevoerd voor de dag één test, kunt u deze werkstroom met een coördinator die de werkstroom plant zodat deze wordt dagelijks uitgevoerd inpakken.

### <a name="run-the-workflow-with-a-coordinator"></a>De werkstroom uitvoert met een coördinator

Als u deze werkstroom plannen zodat deze dagelijks (of alle dagen in een datumbereik) wordt uitgevoerd, kunt u een coördinator. Een coördinator wordt gedefinieerd door een XML-bestand bijvoorbeeld `coordinator.xml`:

```
<coordinator-app name="daily_export" start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" timezone="UTC" xmlns="uri:oozie:coordinator:0.4">
    <datasets>
        <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
            <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
            <done-flag></done-flag>
        </dataset>
    </datasets>
    <input-events>
        <data-in name="event_input1" dataset="ds_input1">
            <instance>${coord:current(0)}</instance>
        </data-in>
    </input-events>
    <action>
        <workflow>
            <app-path>${appBase}/load_flights_by_day</app-path>
            <configuration>
                <property>
                    <name>year</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'yyyy')}</value>
                </property>
                <property>
                    <name>month</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'MM')}</value>
                </property>
                <property>
                    <name>day</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveScriptLoadPartition</name>
                    <value>${hiveScriptLoadPartition}</value>
                </property>
                <property>
                    <name>hiveScriptCreateDailyTable</name>
                    <value>${hiveScriptCreateDailyTable}</value>
                </property>
                <property>
                    <name>hiveDailyTableNamePrefix</name>
                    <value>${hiveDailyTableNamePrefix}</value>
                </property>
                <property>
                    <name>hiveDailyTableName</name>
                    <value>${hiveDailyTableNamePrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}${coord:formatTime(coord:nominalTime(), 'MM')}${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveDataFolderPrefix</name>
                    <value>${hiveDataFolderPrefix}</value>
                </property>
                <property>
                    <name>hiveDataFolder</name>
                    <value>${hiveDataFolderPrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}/${coord:formatTime(coord:nominalTime(), 'MM')}/${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>sqlDatabaseConnectionString</name>
                    <value>${sqlDatabaseConnectionString}</value>
                </property>
                <property>
                    <name>sqlDatabaseTableName</name>
                    <value>${sqlDatabaseTableName}</value>
                </property>
            </configuration>
        </workflow>
    </action>
</coordinator-app>
```

Zoals u ziet, is het merendeel van de coördinator configuratiegegevens alleen doorgeven aan het werkstroomexemplaar. Er zijn echter enkele belangrijke zaken aan te roepen.

* Punt 1: De `start` en `end` kenmerken op het `coordinator-app` het element zelf bepalen de tijdsinterval waarover de coördinator wordt uitgevoerd.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Een coördinator is verantwoordelijk voor het plannen van acties in de `start` en `end` datumbereik volgens het interval dat is opgegeven door de `frequency` kenmerk. Elke geplande actie op zijn beurt de werkstroom wordt uitgevoerd zoals geconfigureerd. De coördinator is in de bovenstaande definitie coordinator geconfigureerd voor het uitvoeren van acties vanaf 1 januari 2017 naar 5e januari 2017. De frequentie is ingesteld op 1 dag door de [Oozie Expression Language](http://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) frequentie expressie `${coord:days(1)}`. Dit resulteert in de planning van een actie coördinator (en dus de werkstroom) eenmaal per dag. Voor de datumbereiken die in het verleden, zoals in dit voorbeeld wordt de actie om uit te voeren zonder vertraging worden gepland. Het begin van de datum van waaruit een actie is gepland voor uitvoering heet de *nominaal tijd*. Bijvoorbeeld: voor het verwerken van de gegevens voor 1 januari 2017 de coördinator wordt gepland actie met een nominaal tijd van 2017-01-01T00:00:00 GMT.

* Punt 2: binnen het datumbereik van de werkstroom, de `dataset` element geeft aan waar de gegevens voor een bepaald datumbereik in HDFS zoekt en configureert u hoe Oozie bepaalt of de gegevens beschikbaar voor verwerking.

    ```
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    Het pad naar de gegevens in HDFS wordt dynamisch samengesteld volgens de expressie die is opgegeven in de `uri-template` element. In deze coordinator wordt ook een frequentie van één dag gebruikt met de gegevensset. Terwijl de begin- en einddatums op het element coordinator besturingselement wanneer de acties die zijn gepland (en hun nominaal tijden definieert), de `initial-instance` en `frequency` op de gegevensset bepalen de berekening van de datum die wordt gebruikt bij het maken van de `uri-template`. In dit geval wordt de eerste instantie ingesteld op één dag vóór het begin van de coördinator om ervoor te zorgen dat deze neemt over de eerste dag van de (1/1/2017) aan gegevens. Berekening van de gegevensset wordt doorsturen van de waarde van `initial-instance` (31-12-2016) geavanceerde in stappen van gegevenssetfrequentie (1 dag) vindt de meest recente datum waarop de nominaal keer niet slaagt ingesteld door de coördinator (2017-01-01T00:00:00 GMT voor de eerste actie).

    De lege `done-flag` element geeft aan dat wanneer Oozie wordt gecontroleerd op de aanwezigheid van invoergegevens op het aangewezen tijdstip, Oozie bepaalt gegevens of beschikbaar door aanwezigheid van een bestand of map. In dit geval is de aanwezigheid van een csv-bestand. Als een csv-bestand aanwezig is, Oozie wordt ervan uitgegaan dat de gegevens gereed is, en een workflowexemplaar voor het verwerken van het bestand opent. Als er geen csv-bestand aanwezig is, Oozie wordt ervan uitgegaan dat de gegevens nog niet klaar en die de werkstroom wordt uitgevoerd in een wachtstatus terechtkomt.

* Punt 3: De `data-in` element geeft de bepaalde tijdstempel te gebruiken als de nominale Vervang de waarden in de tijd `uri-template` voor de bijbehorende dataset.

    ```
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    In dit geval geeft u het exemplaar van de expressie `${coord:current(0)}`, die wordt omgezet in met de nominaal tijd van de actie zoals oorspronkelijk gepland door de coördinator. Met andere woorden, wanneer u de coördinator plant de actie uit te voeren met een nominaal tijd van 01-01/2017, wordt 01-01/2017 wordt gebruikt voor het vervangen van het jaar (2017) en de maand (01) variabelen in de URI-sjabloon. Zodra de URI-sjabloon voor dit exemplaar wordt berekend, controleert Oozie of de verwachte directory of het bestand beschikbaar en dienovereenkomstig plant u de volgende keer uitvoeren van de werkstroom.

De drie voorgaande punten combineren om een situatie waarin de coördinator verwerking van de brongegevens op een wijze per dag plant. 

* Punt 1: De coördinator begint met een nominaal datum van 01-01-2017.

* Punt 2: Oozie zoekt gegevens beschikbaar zijn in `sourceDataFolder/2017-01-FlightData.csv`.

* Punt 3: Wanneer Oozie dat bestand vindt, plant een exemplaar van de werkstroom die de gegevens voor 01-01-2017 worden verwerkt. Oozie wordt vervolgens doorgaat 2017-01-02 worden verwerkt. Deze evaluatie wordt herhaald tot maar niet inclusief 2017-01-05.

Zoals met werkstromen, de configuratie van een coördinator is gedefinieerd in een `job.properties` bestand, dat een uitbreiding is van de instellingen die worden gebruikt door de werkstroom.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.coord.application.path=${appBase}
sourceDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableNamePrefix=dailyflights
hiveDataFolderPrefix=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights

```

De alleen nieuwe eigenschappen die zijn geïntroduceerd in dit `job.properties` bestand zijn:

| Eigenschap | Waardebron |
| --- | --- |
| oozie.coord.Application.Path | Geeft de locatie van de `coordinator.xml` -bestand met de Oozie-coördinator om uit te voeren. |
| hiveDailyTableNamePrefix | Het voorvoegsel dat wordt gebruikt bij het maken van de tabelnaam van de faseringstabel dynamisch. |
| hiveDataFolderPrefix | Het voorvoegsel van het pad waar de faseringstabellen worden opgeslagen. |

### <a name="deploy-and-run-the-oozie-coordinator"></a>Implementeren en uitvoeren van de Oozie-coördinator

Om de pijplijn hebt uitgevoerd met een coördinator, op dezelfde manier als voor de werkstroom, gaat u verder behalve werken vanuit een map één niveau boven de map waarin uw werkstroom. Deze overeenkomst map scheidt u de coördinator van de werkstromen op schijf, zodat u een coördinator aan andere onderliggende werkstromen koppelen kunt.

1. SCP van uw lokale computer gebruiken om de coördinator bestanden tot de lokale opslag van het hoofdknooppunt van het cluster te kopiëren.

    ```bash
    scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:~
    ```

2. Voeg SSH toe aan uw hoofdknooppunt.

    ```bash
    ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 
    ```

3. Kopieer de bestanden van de coördinator naar HDFS.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. De coördinator worden uitgevoerd.

    ```bash
    oozie job -config job.properties -run
    ```

5. Controleer of de status van de webconsole Oozie selecteren van deze tijd de **coördinator taken** tabblad en vervolgens **alle taken**.

    ![Oozie Web Console-coördinator taken](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Selecteer een coördinator-exemplaar om de lijst met geplande acties weer te geven. In dit geval ziet u vier acties met nominaal tijden in het bereik van 1/1/2017 tot 1/4/2017.

    ![Oozie Web Console-coördinator taak](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Elke actie in deze lijst komt overeen met een exemplaar van de werkstroom die een dag aan gegevens verwerkt, waarbij het begin van die dag wordt aangegeven door de nominaal tijd.

## <a name="next-steps"></a>Volgende stappen

* [Apache Oozie-documentatie](http://oozie.apache.org/docs/4.2.0/index.html)

<!-- * Build the same pipeline [using Azure Data Factory](tbd.md).  -->
