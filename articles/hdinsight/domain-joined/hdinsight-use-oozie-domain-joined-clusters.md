---
title: Hadoop Oozie-werkstromen in domein aan een gekoppelde HDInsight-Clusters
description: Gebruik Hadoop Oozie in Linux gebaseerde HDInsight-domein lid zijn van Enterprise-beveiligingspakket. Informatie over het definiëren van een Oozie-workflow en het verzenden van een Oozie-taak.
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 928f6adbb348683a110f7da9b20efaae998290ca
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972210"
---
#<a name="run-apache-oozie-in-domain-joined-hdinsight-hadoop-clusters"></a>Apache Oozie in domein gekoppeld HDInsight Hadoop-clusters uitvoeren
Oozie is een werkstroom en coördinatie systeem waarmee Hadoop-taken worden beheerd. Oozie is geïntegreerd met de Hadoop-stack en ondersteunt de volgende taken:
- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

U kunt ook Oozie gebruiken voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

##<a name="prerequisites"></a>Vereisten:
- Een Domain-joined HDInsight Hadoop-cluster. Zie [configureren Domain-joined HDInsight-clusters](./apache-domain-joined-configure-using-azure-adds.md)

    > [!NOTE]
    > Zie de gedetailleerde instructies voor Oozie gebruiken op niet-domein toegevoegd clusters Zie [dit](../hdinsight-use-oozie-linux-mac.md)

##<a name="connecting-to-domain-joined-cluster"></a>Verbinding maken met domein een cluster is toegevoegd
Zie voor meer informatie over SSH [verificatie: Domain-joined HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
- Maak verbinding met het HDInsight-cluster via SSH:
     ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```
Gebruik om te controleren als Kerberos-verificatie geslaagd is, `klist` opdracht. Als dit niet het geval is, gebruikt u `kinit` initiëren van Kerberos-verificatie.

- Meld u aan bij de HDInsight-gateway voor het registreren van het oAuth-token dat is vereist voor toegang tot Azure Data Lake Store (ADLS)
     ```bash
     curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
     ```

    Een status-Antwoordcode van _200 OK_ geeft aan dat succesvolle registratie. Controleer de gebruikersnaam en wachtwoord als een niet-geautoriseerde antwoord is ontvangen (401).

## <a name="define-the-workflow"></a>De werkstroom definiëren
Oozie werkstroomdefinities worden geschreven in Hadoop proces Definition Language (hPDL), die een definition language met XML-proces. Gebruik de volgende stappen uit om de werkstroom te definiëren:

-   Instellen van de werkruimte van de domeingebruiker:
 ```bash
hdfs dfs -mkdir /user/<DomainUser>
cd /home/<DomainUserPath>
cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
tar -xvf oozie-examples.tar.gz
hdfs dfs -put examples /user/<DomainUser>/
 ```
Vervang `DomainUser` met de naam van de gebruiker domein. Vervang `DomainUserPath` met het pad naar de basismap voor de domeingebruiker. Vervang `ClusterVersion` met uw cluster HDP-versie.

-   Gebruik de volgende instructie maken en bewerken van een nieuw bestand:
 ```bash
nano workflow.xml
 ```

- Nadat de nano-editor wordt geopend, voert u het volgende XML-bestand als de inhoud van het bestand:
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
Vervang `clustername` met de naam van het cluster. 

Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

De werkstroom is onderverdeeld in twee delen:
*   Referentie-sectie: De sectie referenties worden gebruikt in de referenties die worden gebruikt voor het verifiëren van oozie acties:

    In dit voorbeeld wordt verificatie voor Hive acties gebruikt. Zie voor meer informatie, [dit]( https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

    De referentie-service kunt oozie-bewerkingen voor het imiteren van de gebruiker voor toegang tot Hadoop-services.

*   Sectie van de actie: We hebben drie acties hier mapreduce, hive server 2 actie en een hive-server 1:

    De map-reduce wordt uitgevoerd een voorbeeld van oozie-pakket voor mapreduce-die het aantal samengevoegde woorden levert.

    Het hive server 2 en hive server 1-acties die een eenvoudige query op hivesample tabel met HDInsight wordt uitgevoerd.

    De hive-acties gebruiken de referenties die zijn opgegeven in de sectie referenties voor verificatie met behulp van het sleutelwoord `cred` in het actie-element

- Gebruik de volgende opdracht uit om te kopiëren de `workflow.xml` van het bestand in `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:
     ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
     ```

    Vervang `domainuser` met uw gebruikersnaam voor het domein.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definieer het eigenschappenbestand voor de taak Oozie

1.  Gebruik de volgende instructie maken en bewerken van een nieuw bestand voor de taakeigenschappen van de:
     ```bash
    nano job.properties
     ```

2.   Nadat de nano-editor wordt geopend, gebruikt u het volgende XML-bestand als de inhoud van het bestand:

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
    

   * Vervang `domainuser` met uw gebruikersnaam voor het domein.
   * Vervang `ClusterShortName` met korte naam voor het cluster. Als de clusternaam is https://sechadoopcontoso.azurehdisnight.net, wordt de `clustershortname` is de eerste zes letters van het cluster: sechad
   * Vervang `jdbcurlvalue` met JDBC-url van de hive-configuratie. Bijvoorbeeld, jdbc:hive2: / / headnodehost:10001 /; transportMode = http.
    
   * Om het bestand hebt opgeslagen, selecteert u Ctrl + X, voer `Y`, en selecteer vervolgens **Enter**.

   * Dit eigenschappenbestand moet aanwezig zijn lokaal bij het uitvoeren van taken voor oozie

## <a name="creating-custom-hive-scripts-for-the-oozie-job"></a>Het maken van aangepaste hive-scripts voor de taak Oozie
De 2 hive-scripts voor hive-server 1 en hive server 2 kunnen worden gemaakt als volgt:
-   Hive Server 1 bestand:
1.  Gebruik de volgende instructie maken en bewerken van een bestand voor de hive-server 1 actie:
    ```bash
    nano countrowshive1.hql
    ```

2.  Het script maken
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemake from hivesampletable limit 2;
    ```

3.  Sla het bestand op Hdfs
    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

-   Hive Server 2-bestand:
1.  Gebruik de volgende instructie maakt en bewerkt een veld voor hive server 2 actie:
    ```bash
    nano countrowshive2.hql
    ```

2.  Het script maken
    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3.  Sla het bestand op Hdfs
    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submission-of-oozie-jobs"></a>Verzending van Oozie-taken
Verzenden van taken voor domein-clusters oozie is vergelijkbaar met het verzenden van taken in niet-domein gekoppelde clusters oozie.

Zie voor meer informatie [indienen en beheren van de taak](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-oozie-job-submission"></a>Resultaten van Oozie taken verzenden
Omdat oozie-taken worden uitgevoerd namens de gebruiker, controleren zowel Yarn en Ranger logboeken tonen de taken worden uitgevoerd als de geïmiteerde gebruiker. De CLI-uitvoer van de taak oozie als volgt uit:


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

*    De Ranger-auditlogboeken beschikbaar zijn voor hive server 2 acties toont oozie uitvoeren van de actie namens de gebruiker. De ranger en Yarn weergave is alleen zichtbaar voor de cluster-beheerder.

## <a name="configuration-of-user-authorization-in-oozie"></a>Configuratie van gebruikersautorisatie in Oozie
Oozie op zichzelf heeft een configuratie gebruikersverificatie, waardoor gebruikers moet worden gestopt, doden van andere gebruikers taken kunt blokkeren. Deze optie is ingeschakeld door in te stellen de `oozie.service.AuthorizationService.security.enabled` naar `true`. 

Meer informatie hierover vindt u in Oozie documentatie sectie - [autorisatie Gebruikersconfiguratie]( https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html):

Voor onderdelen, zoals hive server 1 waar Ranger-invoegtoepassing niet beschikbaar/ondersteund is, is het alleen grofkorrelige hdfs autorisatie mogelijk. Goed korrelig autorisatie is alleen beschikbaar via ranger invoegtoepassingen.

## <a name="oozie-web-ui"></a>Oozie-Webgebruikersinterface
De Oozie-webgebruikersinterface biedt webgebaseerde inzicht in de status van Oozie-taken op het cluster. In het domein gekoppeld clusters die u wilt:

1. Voeg een [edge-knooppunt](../hdinsight-apps-use-edge-node.md) en in te schakelen [SSH Kerberos-verificatie](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Ga als volgt de [Oozie-Webgebruikersinterface](../hdinsight-use-oozie-linux-mac.md) stappen voor het inschakelen van de SSH-tunneling met het edge-knooppunt en toegang tot de web-UI.

## <a name="next-steps"></a>Volgende stappen
* [Oozie gebruiken met Hadoop om te definiëren en een werkstroom uitvoeren op Azure HDInsight op basis van Linux](../hdinsight-use-oozie-linux-mac.md)
* [Op tijd gebaseerde Oozie-coördinator gebruiken](../hdinsight-use-oozie-coordinator-time.md)
* [Uitvoeren van Hive-query's met behulp van SSH op Domain-joined HDInsight clusters](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
