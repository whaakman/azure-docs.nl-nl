---
title: Beveiligde Apache Oozie-werkstromen met een Enterprise-beveiligingspakket - Azure HDInsight
description: Beveiligen met behulp van de Enterprise-beveiligingspakket van Azure HDInsight Apache Oozie-werkstromen. Informatie over het definiëren van een Oozie-workflow en het verzenden van een Oozie-taak.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 86cb6f6a18cb799574ae9badc0f02144b3a6e1d7
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445123"
---
# <a name="run-apache-oozie-in-hdinsight-hadoop-clusters-with-enterprise-security-package"></a>Voer Apache Oozie in HDInsight Hadoop clusters met Enterprise-beveiligingspakket

Apache Oozie is een werkstroom en coördinatie systeem waarmee Apache Hadoop-taken worden beheerd. Oozie is geïntegreerd met de Hadoop-stack en ondersteunt de volgende taken:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

U kunt ook Oozie gebruiken voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

## <a name="prerequisite"></a>Vereiste

- Een Azure HDInsight Hadoop-cluster met Enterprise Security Package (ESP). Zie [configureren HDInsight-clusters met ESP](./apache-domain-joined-configure-using-azure-adds.md).

    > [!NOTE]  
    > Zie voor gedetailleerde instructies over het gebruik van Oozie op niet-ESP clusters [gebruik Apache Oozie-werkstromen in Azure HDInsight op basis van Linux](../hdinsight-use-oozie-linux-mac.md).

## <a name="connect-to-an-esp-cluster"></a>Verbinding maken met een cluster ESP

Zie voor meer informatie over Secure Shell (SSH) [verbinding maken met HDInsight (Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Verbinding maken met het HDInsight-cluster met behulp van SSH:  
   ```bash
   ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
   ```

2. Als u wilt controleren of geslaagde Kerberos-verificatie, gebruikt u de `klist` opdracht. Als dit niet het geval is, gebruikt u `kinit` Kerberos-verificatie te starten.

3. Meld u aan de HDInsight-gateway voor het registreren van het OAuth-token dat is vereist voor toegang tot Azure Data Lake Storage:   
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Een status-Antwoordcode van **200 OK** geeft aan dat succesvolle registratie. Controleer de gebruikersnaam en het wachtwoord als een niet-geautoriseerde reactie wordt ontvangen, zoals 401.

## <a name="define-the-workflow"></a>De werkstroom definiëren
Oozie werkstroomdefinities worden geschreven in Apache Hadoop proces Definition Language (hPDL). hPDL is een definition language met XML-proces. Voer de volgende stappen uit om de werkstroom te definiëren:

1. Werkruimte van de domeingebruiker van een instellen:
   ```bash
   hdfs dfs -mkdir /user/<DomainUser>
   cd /home/<DomainUserPath>
   cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
   tar -xvf oozie-examples.tar.gz
   hdfs dfs -put examples /user/<DomainUser>/
   ```
   Vervang `DomainUser` met de naam van de gebruiker domein. 
   Vervang `DomainUserPath` met het pad naar de basismap voor de domeingebruiker. 
   Vervang `ClusterVersion` met uw cluster Hortonworks Data Platform (HDP)-versie.

2. Gebruik de volgende instructie maken en bewerken van een nieuw bestand:
   ```bash
   nano workflow.xml
   ```

3. Nadat de nano-editor wordt geopend, voert u het volgende XML-bestand als de inhoud van het bestand:
   ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://hn0-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
   ```
4. Vervang `clustername` met de naam van het cluster. 

5. Om het bestand hebt opgeslagen, selecteert u Ctrl + X. Voer `Y` in. Selecteer vervolgens **Enter**.

    De werkstroom is onderverdeeld in twee delen:
   * **Referentie-sectie.** In deze sectie worden gebruikt in de referenties die worden gebruikt voor het verifiëren van Oozie acties:

     In dit voorbeeld gebruikt verificatie voor Hive-acties. Zie voor meer informatie, [actie verificatie](https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

     De referentie-service kunt Oozie-bewerkingen voor het imiteren van de gebruiker voor toegang tot Hadoop-services.

   * **Sectie van de actie.** Dit gedeelte bevat drie acties: mapreduce-, Hive server 2 en Hive-server 1:

     - De mapreduce-een voorbeeld van een Oozie-pakket voor mapreduce-actie-uitvoeringen die het aantal samengevoegde woorden uitvoert.

     - De Hive server 2 en Hive-server 1 acties kunt u een query uitvoeren op een voorbeeld-Hive-tabel voorzien van HDInsight.

     De Hive-acties gebruiken de referenties die zijn opgegeven in de sectie referenties voor verificatie met behulp van het sleutelwoord `cred` in het actie-element.

6. Gebruik de volgende opdracht uit om te kopiëren de `workflow.xml` van het bestand in `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

7. Vervang `domainuser` met uw gebruikersnaam voor het domein.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definieer het eigenschappenbestand voor de taak Oozie

1. Gebruik de volgende instructie maken en bewerken van een nieuw bestand voor de taakeigenschappen van de:

   ```bash
   nano job.properties
   ```

2. Nadat de nano-editor wordt geopend, gebruikt u het volgende XML-bestand als de inhoud van het bestand:

   ```bash
       nameNode=adl://home
       jobTracker=headnodehost:8050
       queueName=default
       examplesRoot=examples
       oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
       hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
       hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
       oozie.use.system.libpath=true
       user.name=[domainuser]
       jdbcPrincipal=hive/hn0-<ClusterShortName>.<Domain>.com@<Domain>.COM
       jdbcURL=[jdbcurlvalue]
       hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
       hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
   ```

   * Gebruik de `adl://home` URI voor de `nameNode` eigenschap als u Azure Data Lake Storage Gen1 als uw primaire clusteropslag. Als u van Azure Blob Storage gebruikmaakt, klikt u vervolgens dit wijzigen naar `wasb://home`. Als u van Azure Data Lake Storage Gen2 gebruikmaakt, klikt u vervolgens dit wijzigen naar `abfs://home`.
   * Vervang `domainuser` met uw gebruikersnaam voor het domein.  
   * Vervang `ClusterShortName` met de korte naam voor het cluster. Bijvoorbeeld, als de naam van het cluster is https:// *[voorbeeld van de koppeling]* sechadoopcontoso.azurehdisnight.net, de `clustershortname` is de eerste zes tekens van het cluster: **sechad**.  
   * Vervang `jdbcurlvalue` met de JDBC-URL van de Hive-configuratie. Een voorbeeld is jdbc:hive2: / / headnodehost:10001 /; transportMode = http.      
   * Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

   Dit eigenschappenbestand moet aanwezig zijn lokaal bij het uitvoeren van Oozie-taken.

## <a name="create-custom-hive-scripts-for-oozie-jobs"></a>Aangepaste Hive-scripts voor Oozie-taken maken

U kunt de twee Hive-scripts voor Hive-server 1 en 2, zoals wordt weergegeven in de volgende secties van de Hive-server maken.

### <a name="hive-server-1-file"></a>Hive server 1 bestand

1.  Maken en bewerken van een bestand voor Hive-server 1 acties:
    ```bash
    nano countrowshive1.hql
    ```

2.  Hiermee maakt u het script:
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Sla het bestand op Apache Hadoop Distributed File System (HDFS):
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

### <a name="hive-server-2-file"></a>Hive server 2-bestand

1.  Maken en bewerken van een veld voor Hive server 2-acties:
    ```bash
    nano countrowshive2.hql
    ```

2.  Hiermee maakt u het script:
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Sla het bestand op HDFS:
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submit-oozie-jobs"></a>Oozie-taken verzenden

Oozie-taken voor clusters met ESP verzenden is als het verzenden van taken Oozie in niet-ESP-clusters.

Zie voor meer informatie, [Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren op Azure HDInsight op basis van Linux](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-an-oozie-job-submission"></a>Resultaten van een Oozie taken verzenden
Oozie-taken worden uitgevoerd voor de gebruiker. Dus controleren zowel Apache Hadoop YARN en Apache Ranger logboeken tonen de taken worden uitgevoerd als de geïmiteerde gebruiker. De opdrachtregelinterface-uitvoer van een taak Oozie lijkt op de volgende code:



```bash
    Job ID : 0000015-180626011240801-oozie-oozi-W
    ------------------------------------------------------------------------------------------------
    Workflow Name : map-reduce-wf
    App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
    Status        : SUCCEEDED
    Run           : 0
    User          : alicetest
    Group         : -
    Created       : 2018-06-26 19:25 GMT
    Started       : 2018-06-26 19:25 GMT
    Last Modified : 2018-06-26 19:30 GMT
    Ended         : 2018-06-26 19:30 GMT
    CoordAction ID: -
    
    Actions
    ------------------------------------------------------------------------------------------------
    ID                      Status  Ext ID          ExtStatus   ErrCode
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
    ------------------------------------------------------------------------------------------------
    0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
    -----------------------------------------------------------------------------------------------
```

De controlelogboeken Ranger Hive server 2-acties weergeven Oozie uitvoeren van de actie voor de gebruiker. De weergaven Ranger en YARN zijn alleen zichtbaar voor de cluster-beheerder.

## <a name="configure-user-authorization-in-oozie"></a>Gebruikersautorisatie in Oozie configureren

Oozie op zichzelf heeft een configuratie voor het autorisatie van gebruikers die de gebruikers van stoppen of verwijderen van andere gebruikers taken kan blokkeren. Instellen zodat deze configuratie de `oozie.service.AuthorizationService.security.enabled` naar `true`. 

Zie voor meer informatie, [Apache Oozie-installatie en configuratie](https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html).

Voor onderdelen, zoals Hive-server 1 waar de Ranger-invoegtoepassing is niet beschikbaar of wordt ondersteund, is het alleen grofkorrelige HDFS-autorisatie mogelijk. Fijnmazig autorisatie is alleen beschikbaar via Ranger-invoegtoepassingen.

## <a name="get-the-oozie-web-ui"></a>De web-UI van Oozie ophalen

De Oozie-webgebruikersinterface biedt webgebaseerde inzicht in de status van Oozie-taken op het cluster. Als u de web-UI, moet u de volgende stappen uitvoeren in ESP clusters:

1. Voeg een [edge-knooppunt](../hdinsight-apps-use-edge-node.md) en in te schakelen [SSH Kerberos-verificatie](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Ga als volgt de [Oozie-Webgebruikersinterface](../hdinsight-use-oozie-linux-mac.md) stappen voor het inschakelen van de SSH-tunneling met het edge-knooppunt en toegang tot de web-UI.

## <a name="next-steps"></a>Volgende stappen
* [Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren op Azure HDInsight op basis van Linux](../hdinsight-use-oozie-linux-mac.md).
* [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
