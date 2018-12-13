---
title: Operationeel maken van een pijplijn voor gegevensanalyse - Azure
description: Instellen en uitvoeren van een voorbeeld van de pijplijn die wordt geactiveerd door nieuwe gegevens beknopte resultaten oplevert.
services: hdinsight
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: 93c2808dc244a86f7a58aa65d649e9c3e8c17f7c
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251705"
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Een pijplijn voor gegevensanalyse operationeel maken

*Gegevens-pipelines* rela‑ veel data analytics-oplossingen. Zoals de naam al aangeeft, een pijplijn wordt in de onbewerkte gegevens, opschonen en verwijdert deze indien nodig, en normaal gesproken voert vervolgens berekeningen of aggregaties voordat de verwerkte gegevens worden opgeslagen. De verwerkte gegevens wordt gebruikt door clients, rapporten of API's. Een gegevenspijplijn moet herhaalbare resultaten, opgeven of volgens een planning of wanneer ze worden geactiveerd door nieuwe gegevens.

Dit artikel wordt beschreven hoe u om uw gegevenspijplijnen voor herhaalbaarheid, Oozie, die worden uitgevoerd op HDInsight Hadoop-clusters met operationeel te maken. Het voorbeeldscenario begeleidt u bij een pijplijn die wordt voorbereid en luchtvaartmaatschappij flight time series-gegevens worden verwerkt.

In het volgende scenario is de ingevoerde gegevens een plat bestand met een batch van vertragingen van vluchtgegevens gedurende één maand. Deze vluchtgegevens vertragingen van omvatten informatie zoals de bron en doel luchthaven, de mijl alleen gesynchroniseerd, de vertrek en aankomst tijden, enzovoort. Het doel met deze pijplijn is om samen te vatten dagelijkse luchtvaartmaatschappij prestaties, waarbij elke luchtvaartmaatschappij één rij voor elke dag met de gemiddelde vertrek- en vertragingen in minuten, en het totaal aantal kilometers alleen gesynchroniseerd die dag heeft.

| YEAR | MONTH | DAY_OF_MONTH | PROVIDER |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | AA | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | DL | 6.935409 | -2.1893024 | 1909696 |

De voorbeeld-pijplijn wacht totdat een nieuwe periode flight data binnenkomt en vervolgens die gedetailleerde vluchtgegevens in uw Hive-datawarehouse voor analyses die op de lange termijn worden opgeslagen. De pijplijn maakt ook een veel kleinere gegevensset met een overzicht van alleen de dagelijkse flight gegevens. Deze dagelijkse gegevens van vertragingen van vluchten de samenvatting is verzonden naar een SQL-database voor rapporten, zoals voor een website.

Het volgende diagram illustreert de voorbeeld-pijplijn.

![Flight Data Pipeline](./media/hdinsight-operationalize-data-pipeline/pipeline-overview.png)

## <a name="oozie-solution-overview"></a>Overzicht van de oplossing Oozie

Apache Oozie die worden uitgevoerd op een HDInsight Hadoop-cluster maakt gebruik van deze pijplijn.

Oozie beschrijving van de pijplijnen in termen van *acties*, *werkstromen*, en *coördinatoren*. Acties bepalen het echte werk uit te voeren, zoals het uitvoeren van een Hive-query. Werkstromen definiëren de volgorde van de acties. Coördinatoren definieert de planning voor wanneer de werkstroom wordt uitgevoerd. Coördinatoren kunnen ook wachten op de beschikbaarheid van nieuwe gegevens voordat een exemplaar van de werkstroom wordt gestart.

Het volgende diagram toont de ontwerpen op hoog niveau van dit voorbeeld Oozie-pijplijn.

![Oozie Flight Data Pipeline](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

### <a name="provision-azure-resources"></a>Azure-resources inrichten

Deze pijplijn vereist een Azure SQL Database en een HDInsight Hadoop-cluster op dezelfde locatie. De Azure SQL Database slaat de samenvattende gegevens die worden geproduceerd door de pijplijn en de store Oozie-metagegevens.

#### <a name="provision-azure-sql-database"></a>Azure SQL-Database inrichten

1. Met behulp van Azure portal, maakt een nieuwe resourcegroep met de naam `oozie` bevat alle resources die worden gebruikt door het volgende voorbeeld.
2. Binnen de `oozie` resourcegroep, richt Azure SQL-Server en Database. U hoeft niet een-database groter is dan de prijscategorie S1 Standard.
3. Met behulp van de Azure-portal, gaat u naar het deelvenster voor uw zojuist geïmplementeerde SQL-Database en selecteer **extra**.

    ![Knop Extra](./media/hdinsight-operationalize-data-pipeline/sql-db-tools.png)

4. Selecteer **Query-Editor**.

    ![Knop voor query-Editor](./media/hdinsight-operationalize-data-pipeline/sql-db-query-editor.png)

5. In de **Query-Editor** venster **aanmelding**.

    ![Aanmeldknop](./media/hdinsight-operationalize-data-pipeline/sql-db-login1.png)

6. Voer de referenties van uw SQL-Database en selecteer **OK**.

   ![Aanmeldformulier](./media/hdinsight-operationalize-data-pipeline/sql-db-login2.png)

7. Voer de volgende SQL-instructies gebruikt om te maken in het gebied voor de Query-Editor de `dailyflights` tabel de samengevatte gegevens van elke uitvoering van de pijplijn worden opgeslagen.

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

8. Selecteer **uitvoeren** voor het uitvoeren van de SQL-instructies.

    ![Knop uitvoeren](./media/hdinsight-operationalize-data-pipeline/sql-db-run.png)

Uw Azure SQL Database is nu gereed.

#### <a name="provision-an-hdinsight-hadoop-cluster"></a>Een HDInsight Hadoop-Cluster inrichten

1. Selecteer in de Azure portal, **+ nieuw** en zoek naar HDInsight.
2. Selecteer **Maken**.
3. Geef een unieke naam voor uw cluster in het deelvenster basisbeginselen en kiest u uw Azure-abonnement.

    ![De naam van de HDInsight-cluster en abonnement](./media/hdinsight-operationalize-data-pipeline/hdi-name-sub.png)

4. In de **clustertype** venster de **Hadoop** clustertype, **Linux** besturingssysteem en de meest recente versie van het HDInsight-cluster. Laat de **Cluster laag** op **Standard**.

    ![HDInsight-clustertype](./media/hdinsight-operationalize-data-pipeline/hdi-cluster-type.png)

5. Kies **Selecteer** om toe te passen van de selectie van uw cluster.
6. Voltooid de **basisbeginselen** deelvenster door te geven van een wachtwoord voor clusteraanmelding en selecteren van uw `oozie` resource groep uit de lijst en selecteer vervolgens **volgende**.

    ![Grondbeginselen van HDInsight-deelvenster](./media/hdinsight-operationalize-data-pipeline/hdi-basics.png)

7. In de **opslag** deelvenster, typt u de primaire opslag is ingesteld op laat **Azure Storage**, selecteer **nieuw**, en geef een naam voor het nieuwe account.

    ![HDInsight-Opslagaccountinstellingen](./media/hdinsight-operationalize-data-pipeline/hdi-storage.png)

8. Voor de **Metastore-instellingen**onder **SQL-database voor Hive selecteren**, kiest u de database die u eerder hebt gemaakt.

    ![HDInsight Hive-Metastore-instellingen](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-hive.png)

9. Selecteer **SQL-Database verifiëren**.

    ![HDInsight Hive-Metastore verifiëren](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql.png)

10. Voer uw SQL database-gebruikersnaam en wachtwoord in en kies **Selecteer**. 

       ![HDInsight Hive-Metastore aanmelding verifiëren](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql-login.png)

11. Terug op de **Metastore-instellingen** deelvenster, selecteer uw database voor Oozie-metagegevens opslaan en verifiëren als u eerder hebt gedaan. 

       ![HDInsight-Metastore-instellingen](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-settings.png)

12. Selecteer **Volgende**.
13. Op de **samenvatting** venster **maken** om uw cluster te implementeren.

### <a name="verify-ssh-tunneling-setup"></a>Controleer of de SSH-tunneling van setup

Als u wilt de webconsole Oozie gebruiken om de status van uw coordinator en werkstroomexemplaren weer te geven, instellen van een SSH-tunnel met uw HDInsight-cluster. Zie voor meer informatie, [SSH-Tunnel](hdinsight-linux-ambari-ssh-tunnel.md).

> [!NOTE]
> U kunt ook Chrome met de [Foxy Proxy](https://getfoxyproxy.org/) extensie om te bladeren van uw cluster webresources via de SSH-tunnel. Alle aanvragen via de host configureren voor proxy `localhost` op tunnelpoort 9876. Deze aanpak is compatibel met het Windows-subsysteem voor Linux, ook wel bekend als Bash in Windows 10.

1. Voer de volgende opdracht om te openen van een SSH-tunnel naar het cluster:

    ```
    ssh -C2qTnNf -D 9876 sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
    ```

2. Controleer of dat de tunnel operationeel is door te navigeren naar Ambari op uw hoofdknooppunt door te bladeren naar:

    http://headnodehost:8080

3. Toegang krijgen tot de **Oozie-webconsole** vanuit Ambari, de optie **Oozie**, **snelkoppelingen**, en selecteer vervolgens **Oozie-webconsole**.

### <a name="configure-hive"></a>Hive configureren

1. Download een voorbeeld van de CSV-bestand met vluchtgegevens van vertragingen van gedurende één maand. Download het ZIP-bestand `2017-01-FlightData.zip` uit de [HDInsight GitHub-opslagplaats](https://github.com/hdinsight/hdinsight-dev-guide) en pak het uit in het CSV-bestand `2017-01-FlightData.csv`. 

2. Kopieer dit CSV-bestand tot de Azure Storage-account dat is gekoppeld aan uw HDInsight-cluster op en plaats deze in de `/example/data/flights` map.

Kunt u het bestand met SCP in uw `bash` shell-sessie.

1. SCP gebruiken om te kopiëren van de bestanden op uw lokale machine in de lokale opslag van het hoofdknooppunt van HDInsight-cluster.

    ```bash
    scp ./2017-01-FlightData.csv sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:2017-01-FlightData.csv
    ```

2. Gebruik de HDFS-opdracht om het bestand vanuit uw lokale opslag van hoofdknooppunt kopiëren naar Azure Storage.

    ```bash
    hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
    ```

De voorbeeldgegevens is nu beschikbaar. De pijplijn vereist echter twee Hive-tabellen voor de verwerking, één voor de inkomende gegevens (`rawFlights`) en één voor de samengevatte gegevens (`flights`). Deze tabellen als volgt in Ambari maken.

1. Meld u aan bij de Ambari door te navigeren naar [ http://headnodehost:8080 ](http://headnodehost:8080).
2. Selecteer in de lijst met services, **Hive**.

    ![Hive selecteren in Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Selecteer **gaat u naar de weergave** naast het label Hive-weergave 2.0.

    ![Hive-weergave selecteren in Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Plak de volgende instructies om te maken in het tekstgebied query de `rawFlights` tabel. De `rawFlights` tabel bevat een schema van de leesbewerking voor de CSV-bestanden in de `/example/data/flights` map in Azure Storage. 

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

6. Maakt de `flights` tabel, vervang de tekst in het tekstgebied query door de volgende instructies. De `flights` tabel is een beheerde Hive-tabel die gegevens zijn geladen met jaar, maand en dag van maand partitioneert. Deze tabel bevat alle vluchtgegevens van de historische, met de laagste granulatie aanwezig zijn in de brongegevens van één rij per vlucht.

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

### <a name="create-the-oozie-workflow"></a>De Oozie-workflow maken

Gegevens in batches verwerken pijplijnen doorgaans met een bepaald tijdsinterval. In dit geval wordt verwerkt de pijplijn de vluchtgegevens van vertragingen van dagelijks. Hierdoor kunnen de invoerbestanden CSV dagelijks, wekelijks, maandelijks of jaarlijks aankomt.

De voorbeeldwerkstroom verwerkt de vlucht gegevens per dag, in drie belangrijke stappen:

1. Uitvoeren van een Hive-query om de gegevens voor het datumbereik van de dag extraheren uit het bronbestand van de CSV wordt vertegenwoordigd door de `rawFlights` tabel en voeg de gegevens in de `flights` tabel.
2. Een Hive-query voor het maken van een tijdelijke tabel dynamisch in Hive voor die dag, waarin een kopie van de vluchtgegevens samengevat per dag en provider uitvoeren.
3. Apache Sqoop gebruiken om te kopiëren van alle gegevens van de dagelijkse faseringstabel in Hive naar de bestemming `dailyflights` tabel in Azure SQL Database. Sqoop leest de bronrijen van de gegevens achter de Hive-tabel die zich bevinden in Azure Storage en worden ze in SQL Database met behulp van een JDBC-verbinding.

Deze drie stappen worden gecoördineerd door een Oozie-workflow. 

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

    Oozie variabelen gebruikt u de syntaxis `${variableName}`. Deze variabelen worden ingesteld de `job.properties` bestand zoals beschreven in een latere stap. Oozie vervangt door de werkelijke waarden tijdens runtime.

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

    Deze query maakt u een tijdelijke tabel die wordt alleen de samengevatte gegevens voor één dag opslaan, noteer de SELECT-instructie die de gemiddelde vertraging en het totaal van afstand alleen gesynchroniseerd door provider per dag worden berekend. De gegevens ingevoegd in deze tabel is opgeslagen in een bekende locatie (het pad dat wordt aangegeven door de variabele hiveDataFolder) zodat deze kan worden gebruikt als bron voor Sqoop in de volgende stap.

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

De twee Hive-query's worden gebruikt door het bijbehorende pad in Azure Storage en de resterende waarden van variabelen worden geleverd door de volgende `job.properties` bestand. Dit bestand configureert u de werkstroom moet worden uitgevoerd voor de datum die 3e januari 2017.

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

De volgende tabel geeft een overzicht van elk van de eigenschappen en wordt aangegeven waar u de waarden voor uw eigen omgeving kunt vinden.

| Eigenschap | Waardebron |
| --- | --- |
| nameNode | Het volledige pad naar de Azure-Opslagcontainer gekoppeld aan uw HDInsight-cluster. |
| jobTracker | De hostnaam van de interne aan uw actieve cluster YARN head knooppunt. Op de startpagina Ambari YARN selecteren in de lijst met services, en kies vervolgens actieve Resource Manager. De hostnaam van de URI wordt weergegeven boven aan de pagina. De poort 8050 toevoegen. |
| queueName | De naam van de YARN-wachtrij die wordt gebruikt bij het plannen van de Hive-acties. Laat als standaard. |
| oozie.use.system.libpath | Laat de eigenschap als waar. |
| appBase | Het pad naar de submap in de Azure-opslag waar u de Oozie-workflow en ondersteunende bestanden implementeren. |
| oozie.WF.Application.Path | De locatie van de Oozie-workflow `workflow.xml` om uit te voeren. |
| hiveScriptLoadPartition | Het pad naar het Hive-query-bestand in Azure Storage `hive-load-flights-partition.hql`. |
| hiveScriptCreateDailyTable | Het pad naar het Hive-query-bestand in Azure Storage `hive-create-daily-summary-table.hql`. |
| hiveDailyTableName | De dynamisch gegenereerde naam moet worden gebruikt voor de tijdelijke tabel. |
| hiveDataFolder | Het pad naar de gegevens in de tijdelijke tabel is opgenomen in Azure Storage. |
| sqlDatabaseConnectionString | De JDBC syntaxis van de verbindingsreeks voor uw Azure SQL Database. |
| sqlDatabaseTableName | De naam van de tabel in Azure SQL Database waarin u overzicht rijen zijn ingevoegd. Laat de `dailyflights`. |
| jaar | Het jaargedeelte van de dag voor welke vlucht samenvattingen worden berekend. Laat is. |
| maand | Het maandgedeelte van de dag voor welke vlucht samenvattingen worden berekend. Laat is. |
| dag | Het maandonderdeel dag van van de dag voor welke vlucht samenvattingen worden berekend. Laat is. |

> [!NOTE]
> Zorg ervoor dat u het bijwerken van uw exemplaar van de `job.properties` -bestand met de waarden die specifiek zijn voor uw omgeving voordat u kunt implementeren en uitvoeren van uw Oozie-workflow.

### <a name="deploy-and-run-the-oozie-workflow"></a>Implementeren en uitvoeren van de Oozie-workflow

SCP vanuit de bash-sessie gebruiken om te implementeren van uw Oozie-workflow (`workflow.xml`), de Hive-query's (`hive-load-flights-partition.hql` en `hive-create-daily-summary-table.hql`) en de taakconfiguratie van de (`job.properties`).  In Oozie, alleen de `job.properties` bestand kan bestaan op de lokale opslag van het hoofdknooppunt. Alle andere bestanden moeten worden opgeslagen in HDFS, in dit geval Azure Storage. De actie van het Sqoop die worden gebruikt door de werkstroom, is afhankelijk van een JDBC-stuurprogramma om te communiceren met uw SQL-Database, die moet worden gekopieerd van het hoofdknooppunt naar HDFS.

1. Maak de `load_flights_by_day` submap onder het pad van de gebruiker in de lokale opslag van het hoofdknooppunt.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 'mkdir load_flights_by_day'

2. Kopieert u alle bestanden in de huidige map (de `workflow.xml` en `job.properties` bestanden) tot de `load_flights_by_day` submap.

        scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:load_flights_by_day

3. SSH naar het hoofdknooppunt en navigeer naar de `load_flights_by_day` map.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
        cd load_flights_by_day

4. Kopieer de Werkstroombestanden naar HDFS.

        hdfs dfs -put ./* /oozie/load_flights_by_day

5. Kopie `sqljdbc41.jar` uit het lokale hoofdknooppunt naar de werkstroommap in HDFS:

        hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /oozie/load_flights_by_day

6. De werkstroom uitvoert.

        oozie job -config job.properties -run

7. Bekijk de status van de webconsole Oozie gebruiken. Vanuit Ambari, de optie **Oozie**, **snelkoppelingen**, en vervolgens **Oozie-webconsole**. Onder de **werkstroomtaken** tabblad **alle taken**.

    ![Werkstromen voor Oozie-Web-Console](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

8. Wanneer de status is geslaagd, query uitvoeren op de SQL database-tabel om de ingevoegde rijen weer te geven. Met behulp van Azure portal, gaat u naar het deelvenster voor uw SQL-Database, selecteer **extra**, en open de **Query-Editor**.

        SELECT * FROM dailyflights

Nu dat de werkstroom wordt uitgevoerd voor de dag één test, kunt u deze werkstroom met een coördinator die de werkstroom, zodat deze wordt dagelijks uitgevoerd te verpakken.

### <a name="run-the-workflow-with-a-coordinator"></a>De werkstroom uitvoert met een coördinator

Als u deze werkstroom plannen zodat deze dagelijks (of alle dagen in een bepaalde periode) wordt uitgevoerd, kunt u een coördinator. Een coördinator wordt gedefinieerd door een XML-bestand, bijvoorbeeld `coordinator.xml`:

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

Zoals u ziet, wordt de meerderheid van de coördinator alleen configuratie-informatie doorgegeven aan het werkstroomexemplaar. Er zijn echter enkele belangrijke items te belichten.

* Punt 1: De `start` en `end` kenmerken op de `coordinator-app` element zelf bepalen het tijdsinterval op waarover de coördinator wordt uitgevoerd.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Een coördinator is verantwoordelijk voor het plannen van acties in de `start` en `end` datumbereik, op basis van het interval dat is opgegeven door de `frequency` kenmerk. Elke geplande actie op zijn beurt de werkstroom wordt uitgevoerd zoals geconfigureerd. In het definitie van de coördinator hierboven, is de coördinator geconfigureerd voor het uitvoeren van acties vanaf 1 januari 2017 tot en met 5 januari 2017. De frequentie is ingesteld op 1 dag door de [Oozie expressietaal](http://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) frequentie expressie `${coord:days(1)}`. Dit resulteert in de coördinator plannen van een actie (en dus de werkstroom) één keer per dag. Voor datumbereiken die zich in het verleden, zoals in dit voorbeeld wordt de actie om uit te voeren zonder vertraging worden gepland. Het begin van de datum van waaruit een actie is gepland voor uitvoering heet de *nominale tijd*. Bijvoorbeeld, voor het verwerken van de gegevens voor 1 januari 2017 de coördinator wordt gepland actie met een nominaal bedrag tijd van 2017-01-01T00:00:00 GMT.

* Punt 2: Binnen het datumbereik van de werkstroom, de `dataset` element geeft aan waar de gegevens voor een bepaald datumbereik zoeken in HDFS en configureert u hoe Oozie bepaalt of de gegevens beschikbaar zijn voor verwerking.

    ```
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    Het pad naar de gegevens in HDFS dynamisch is gebouwd op basis van de expressie die is opgegeven in de `uri-template` element. In deze coordinator wordt ook een frequentie van één dag gebruikt met de gegevensset. Terwijl de begin- en einddatums van het element coordinator besturingselement wanneer de acties die zijn gepland (en hun nominale tijden definieert), de `initial-instance` en `frequency` beheren op de gegevensset van de berekening van de datum die wordt gebruikt bij het maken van de `uri-template`. In dit geval wordt de eerste instantie ingesteld op een dag vóór het begin van de coördinator om ervoor te zorgen dat dit de eerste dag neemt de (1/1/2017) aan gegevens. Berekening van de gegevensset wordt getotaliseerd doorsturen van de waarde van `initial-instance` (31-12-2016) stijgen in stappen van de gegevenssetfrequentie (1 dag) totdat de meest recente datum waarop niet de nominale tijd geeft instellen door de coördinator (2017-01-01T00:00:00 GMT voor de eerste actie).

    De lege `done-flag` element geeft aan dat wanneer Oozie wordt gecontroleerd op de aanwezigheid van ingevoerde gegevens op de aangewezen moment, Oozie bepaalt gegevens of beschikbaar door aanwezigheid van een bestand of map. In dit geval is de aanwezigheid van een csv-bestand. Als een csv-bestand aanwezig is, Oozie wordt ervan uitgegaan dat de gegevens gereed is, en een werkstroomexemplaar voor het verwerken van het bestand opent. Als er geen csv-bestand aanwezig is, Oozie wordt ervan uitgegaan dat de gegevens zijn nog niet gereed is en dat uitvoeren van de werkstroom krijgt de wachtstatus.

* Punt 3: De `data-in` element Hiermee geeft u de specifieke tijdstempel te gebruiken als de nominale Vervang de waarden in de tijd `uri-template` voor de bijbehorende dataset.

    ```
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    In dit geval stelt u het exemplaar op de expressie `${coord:current(0)}`, die wordt omgezet met behulp van de nominale tijd van de actie zoals oorspronkelijk gepland door de coördinator. Met andere woorden, wanneer u de coördinator plant de actie om uit te voeren met een nominaal bedrag tijd van 01-01-2017, is 01-01-2017 wat wordt gebruikt ter vervanging van het jaar (2017) en de variabelen voor de maand (01) in de URI-sjabloon. Zodra de URI-sjabloon wordt berekend voor dit exemplaar, controleert Oozie of de verwachte map of het bestand beschikbaar en dienovereenkomstig plant u de volgende keer uitvoeren van de werkstroom.

De drie punten in de voorgaande combineren om een situatie waarin de coördinator verwerking van de brongegevens op een dag manier plant. 

* Punt 1: De coördinator begint met een nominaal bedrag datum van 01-01-2017.

* Punt 2: Oozie gezocht naar gegevens die beschikbaar zijn in `sourceDataFolder/2017-01-FlightData.csv`.

* Punt 3: Wanneer Oozie dat bestand vindt, plant u een exemplaar van de werkstroom die de gegevens te voor 01-01-2017 verwerken. Oozie voordat de verwerking van voor 2017-01-02. Deze evaluatie wordt herhaald tot maar niet inclusief 01-2017-05.

Zoals met werkstromen, de configuratie van een coördinator is gedefinieerd in een `job.properties` -bestand, dat een superset is van de instellingen die door de werkstroom.

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
| hiveDailyTableNamePrefix | Het voorvoegsel dat wordt gebruikt wanneer de naam van de tabel van de faseringstabel op dynamische wijze gemaakt. |
| hiveDataFolderPrefix | Het voorvoegsel van het pad waar de faseringstabellen worden opgeslagen. |

### <a name="deploy-and-run-the-oozie-coordinator"></a>Implementeren en uitvoeren van de Oozie-coördinator

Voor het uitvoeren van de pijplijn met een coördinator, gaan op dezelfde manier als voor de werkstroom, tenzij u uit een map werkt met één niveau boven de map met uw werkstroom. Deze afspraak map scheidt u de coördinatoren van de werkstromen op schijf, zodat u een coördinator aan andere onderliggende werkstromen koppelen kunt.

1. Gebruik de SCP van uw lokale computer om de coördinator bestanden tot de lokale opslag van het hoofdknooppunt van het cluster te kopiëren.

    ```bash
    scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:~
    ```

2. SSH naar het hoofdknooppunt.

    ```bash
    ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 
    ```

3. Kopieer de bestanden coordinator naar HDFS.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. De coördinator worden uitgevoerd.

    ```bash
    oozie job -config job.properties -run
    ```

5. Controleer of de status van de webconsole Oozie gebruiken deze tijd selecteren de **Coordinator taken** tabblad, en vervolgens **alle taken**.

    ![Oozie-Console Coordinator webtaken](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Selecteer een coördinator-exemplaar om de lijst met geplande acties weer te geven. In dit geval ziet u vier acties met een nominaal bedrag minder in het bereik van 1/1/2017 tot 1/4/2017.

    ![Oozie-Web-Console Coordinator Job](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Elke actie in deze lijst komt overeen met een exemplaar van de werkstroom die uit één dag aan gegevens, waar het begin van dag wordt aangegeven door de nominale tijd.

## <a name="next-steps"></a>Volgende stappen

* [Apache Oozie-documentatie](http://oozie.apache.org/docs/4.2.0/index.html)

<!-- * Build the same pipeline [using Azure Data Factory](tbd.md).  -->
