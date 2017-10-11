---
title: HBase oplossen met behulp van Azure HDInsight | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het werken met HBase en Azure HDInsight.
services: hdinsight
documentationcenter: 
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 7/7/2017
ms.author: nitinver
ms.openlocfilehash: 15412c3853a2b8436c5e96034c9a92a2a1094662
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a>HBase oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen bij het werken met Apache HBase nettoladingen in Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Hoe voer hbck opdracht rapporten met meerdere niet-toegewezen gebieden

Een algemene foutmelding dat u mogelijk zien bij het uitvoeren van de `hbase hbck` opdracht is "meerdere regio's wordt niet-toegewezen of gaten in de keten van regio's."

In de UI HBase Master ziet u het aantal regio's die zijn op alle servers van de regio in onbalans gebracht. U kunt vervolgens uitvoert `hbase hbck` opdracht om te zien gaten in de keten regio.

Gaten wordt mogelijk veroorzaakt door de offline-regio's, de toewijzingen dus eerst los. 

Voor het maken van de niet-toegewezen gebieden terug naar een normale status, moet u de volgende stappen uitvoeren:

1. Meld u aan het HDInsight HBase-cluster via SSH.
2. Voor verbinding met de ZooKeeper-shell, voer de `hbase zkcli` opdracht.
3. Voer de `rmr /hbase/regions-in-transition` opdracht of het `rmr /hbase-unsecure/regions-in-transition` opdracht.
4. Om af te sluiten van de `hbase zkcli` -shell, gebruikt u de `exit` opdracht.
5. Open de Apache Ambari-gebruikersinterface en start de service actief HBase Master opnieuw.
6. Voer de `hbase hbck` opdracht (zonder opties) opnieuw. Controleer de uitvoer van deze opdracht om ervoor te zorgen dat alle regio's worden toegewezen.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hoe los ik problemen time-out bij gebruik van hbck opdrachten voor toewijzingen regio

### <a name="issue"></a>Probleem

Een mogelijke oorzaak voor time-out van problemen wanneer u de `hbck` opdracht mogelijk verschillende regio's zijn in de status 'in de overgangsfase' gedurende een lange periode. Deze regio's kunt u zien als offline in de HBase-Master-UI. Omdat een groot aantal gebieden om een overgang probeert, HBase Master time-out mogelijk en geen regio's weer online brengen.

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Meld u aan het HDInsight HBase-cluster via SSH.
2. Voor verbinding met de ZooKeeper-shell, voer de `hbase zkcli` opdracht.
3. Voer de `rmr /hbase/regions-in-transition` of de `rmr /hbase-unsecure/regions-in-transition` opdracht.
4. Om af te sluiten de `hbase zkcli` -shell, gebruikt u de `exit` opdracht.
5. In de Ambari-UI de actieve HBase Master-service opnieuw te starten.
6. Voer de `hbase hbck -fixAssignments` opdracht opnieuw.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hoe ik geforceerde-/ uitschakelen HDFS veilige modus in een cluster

### <a name="issue"></a>Probleem

De lokale Hadoop Distributed File System (HDFS) is vastgelopen in de veilige modus op het HDInsight-cluster.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Deze fout kan zijn veroorzaakt door een fout opgetreden tijdens het uitvoeren van de volgende HDFS-opdracht:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

De fout die u tegenkomen kunt wanneer u probeert uit te voeren van de opdracht ziet er als volgt:

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a>Mogelijke oorzaak

De grootte van het HDInsight-cluster is gewijzigd om een zeer weinig knooppunten. Het aantal knooppunten is lager dan of dicht bij de replicatie HDFS factor.

### <a name="resolution-steps"></a>Stappen voor het oplossen 

1. De status van de HDFS ophalen op het HDInsight-cluster met de volgende opdrachten:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. U kunt ook de integriteit van de HDFS op het HDInsight-cluster controleren met behulp van de volgende opdrachten:

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   The filesystem under path '/' is HEALTHY
   ```

3. Als u dat vaststelt er zijn geen ontbreekt, beschadigd of under-gerepliceerde blokken of dat deze blokken kunnen worden genegeerd, voer de volgende opdracht te laten de naam van knooppunt buiten de veilige modus:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Hoe los JDBC of SQLLine verbinding problemen met Apache Phoenix

### <a name="resolution-steps"></a>Stappen voor het oplossen

Als u wilt verbinden met Phoenix, moet u het IP-adres van een actief ZooKeeper-knooppunt opgeven. Zorg ervoor dat de service ZooKeeper welke sqlline.py verbinding probeert te maken is actief.
1. Meld u aan het HDInsight-cluster via SSH.
2. Voer de volgende opdracht in:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Het IP-adres van het actieve knooppunt van de ZooKeeper kunt u krijgen via de UI Ambari. Ga naar **HBase** > **snelle koppelingen** > **ZK\* (actief)** > **Zookeeper Info**. 

3. Als de sqlline.py met Phoenix verbindt en geen time-out biedt, voert u de volgende opdracht om de beschikbaarheid en de status van Phoenix valideren:

   ```apache
           !tables
           !quit
   ```      
4. Als deze opdracht werkt, is er geen probleem. Het IP-adres opgegeven door de gebruiker is mogelijk onjuist. Echter, als de opdracht voor langere tijd wordt onderbroken en wordt vervolgens de volgende fout weergegeven, verder met stap 5.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. De volgende opdrachten uitvoeren vanaf het hoofdknooppunt (hn0) voor het vaststellen van de voorwaarde van het systeem Phoenix. Catalogustabel:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   De opdracht moet retourneren een foutmelding ziet er als volgt: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Voer de volgende stappen uit om de service HMaster op alle knooppunten van de ZooKeeper opnieuw te starten in de UI Ambari:

    1. In de **samenvatting** sectie van HBase, gaat u naar **HBase** > **actieve HBase Master**. 
    2. In de **onderdelen** sectie, opnieuw opstarten van de HBase-hoofdservice.
    3. Herhaal deze stappen voor alle resterende **stand-by HBase Master** services. 

Het kan tot vijf minuten duren voordat de HBase-Master service stabiel en het herstelproces te voltooien. Herhaal de sqlline.py-opdrachten om te bevestigen dat het systeem na een paar minuten. Catalogustabel actief is en dat deze kan worden opgevraagd. 

Wanneer het systeem. Catalogustabel is weer in de normale, het connectiviteitsprobleem naar Phoenix moet worden automatisch opgelost.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Wat zorgt ervoor dat een master server niet worden gestart

### <a name="error"></a>Fout 

Een atomic naamswijzigingen fout optreedt.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Tijdens het opstarten voltooid HMaster veel initialisatiestappen. Deze omvatten het verplaatsen van gegevens uit de map maken (TMP) naar de map data. HMaster ook gekeken naar de map schrijven-ahead logs (WALs) om te zien of er niet-reagerende regio-servers zijn, enzovoort. 

Tijdens het opstarten HMaster biedt een eenvoudige `list` opdracht op deze mappen. Als HMaster op elk gewenst moment een onverwacht bestand in een van deze mappen ziet, er een uitzondering gegenereerd en niet kan worden gestart.  

### <a name="probable-cause"></a>Mogelijke oorzaak

Probeer om te bepalen van de tijdlijn van het maken van het bestand en controleer vervolgens of er is een proces crash rond de tijd die het bestand is gemaakt in de logboeken van de server regio. (Contact op met ondersteuning van HBase om u te helpen dit uit te voeren). Dit helpt ons bieden krachtiger mechanismen, zodat u kunt u door deze fout voorkomen en zorg ervoor dat de correcte proces afsluitingen over te slaan.

### <a name="resolution-steps"></a>Stappen voor het oplossen

Controleer de aanroepstack en probeer het bepalen van de map waarin het probleem mogelijk worden veroorzaakt (bijvoorbeeld het moet mogelijk de map WALs of TMP). In de Cloud Explorer of via de HDFS-opdrachten, probeer het probleembestand te zoeken. Meestal is dit een \*-renamePending.json-bestand. (De \*-renamePending.json bestand is een journaalbestand dat wordt gebruikt voor het implementeren van de atomic naamswijziging in het stuurprogramma WASB. Als gevolg van fouten in deze implementatie van kunnen deze bestanden blijven via na proces crashes, enzovoort.) Force verwijderen van dit bestand in de Cloud Explorer of via de HDFS-opdrachten. 

Soms wordt er mogelijk ook een tijdelijk bestand dat lijkt op met de naam *$$$. $$$* op deze locatie. U moet gebruiken de HDFS `ls` opdracht om te zien van dit bestand; u kunt het bestand in de Cloud Explorer kan niet controleren. Als u wilt verwijderen van dit bestand, gebruikt u de opdracht HDFS `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Na het uitvoeren van deze opdrachten moet HMaster onmiddellijk starten. 

### <a name="error"></a>Fout

Er is geen serveradres wordt vermeld in *hbase: meta* voor xxx regio.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

U ziet een bericht op uw Linux-cluster waarmee wordt aangegeven dat de *hbase: meta* tabel is niet online. Met `hbck` kunnen melden die ' hbase: meta tabel replicaId 0 is niet gevonden in elke regio. " Het probleem mogelijk HMaster kan niet initialiseren nadat u HBase opnieuw opgestart. In de logboeken HMaster ziet u het bericht: "Er is geen serveradres vermeld in hbase: meta voor regio hbase: back- \<regionaam\>'.  

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Voer de volgende opdrachten (werkelijke waarden voor de wijziging van toepassing) in de HBase-shell:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Verwijder de *hbase: naamruimte* vermelding. Dit item is mogelijk dezelfde fout die wordt gemeld wanneer de *hbase: naamruimte* tabel wordt gescand.

3. Start de actieve HMaster-service actief is, in de UI Ambari HBase weer te geven.  

4. In de HBase-shell, zodat alle offline tabellen, voer de volgende opdracht:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Aanvullende bronnen

[Kan niet worden verwerkt de HBase-tabel](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Fout

HMaster een time-out optreedt bij een fatale uitzondering die vergelijkbaar is met ' java.io.IOException: time-out 300000ms wachten naamruimte tabel moet worden toegewezen. "

### <a name="detailed-description"></a>Gedetailleerde beschrijving

U kunt dit probleem kan optreden als er veel tabellen en regio's die niet is leeggemaakt wanneer u uw HMaster services opnieuw start. Opnieuw opstarten kan mislukken en ziet u het vorige foutbericht wordt weergegeven.  

### <a name="probable-cause"></a>Mogelijke oorzaak

Dit is een bekend probleem met de service HMaster. Algemene cluster starten van de taken kunnen lang duren. HMaster afgesloten omdat de tabel naamruimte nog niet is toegewezen. Dit gebeurt alleen in scenario's waarin grote hoeveelheid gegevens unflushed bestaat en een time-out van vijf minuten is niet voldoende.
  
### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Ga in de UI Ambari naar **HBase** > **Configs**. Voeg de volgende instelling in het bestand aangepaste hbase-site.xml: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. De vereiste services (HMaster en mogelijk andere HBase-services) opnieuw te starten.  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Wat veroorzaakt een fout opnieuw opstarten op een server regio

### <a name="issue"></a>Probleem

Een fout opnieuw opstarten op een server regio kan worden voorkomen door de volgende best practices. U wordt aangeraden zwaar worden belast activiteit te onderbreken, wanneer u van plan bent om te HBase regio servers opnieuw opstarten. Als een toepassing wordt voortgezet verbinding maken met servers in regio als shutdown bezig is, wordt regio server opnieuw opstarten trager worden door enkele minuten. Het is ook een goed idee eerst leegmaken van alle tabellen. Zie voor informatie over het leegmaken van tabellen [HDInsight HBase: suggesties voor verbeteringen aan het opstarten van de HBase-cluster door het leegmaken van tabellen](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Als u de bewerking voor opnieuw opstarten op HBase regio servers van de Ambari UI hebt gestart, ziet u onmiddellijk dat de servers regio werd afgesloten, maar ze niet meteen opnieuw. 

Dit is wat er gebeurt achter de schermen: 

1. De Ambari-agent verzendt een aanvraag tot stoppen met de regio-server.
2. De Ambari-agent wacht gedurende 30 seconden voor de regio server afsluiten. 
3. Als uw toepassing blijft om te verbinden met de regio-server, sluit de server niet omlaag onmiddellijk. De 30 seconden time-out is verstreken voordat de afsluiting. 
4. Na 30 seconden, de Ambari-agent verzendt een force-kill (`kill -9`) opdracht met de regio-server. U kunt dit zien in het logboek ambari-agent (in de /var/log /-map van het respectieve werkrolknooppunt):

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
Vanwege het abrupte afsluiten, kan de poort die is gekoppeld aan het proces niet worden vrijgegeven, hoewel de regio serverproces is gestopt. Deze situatie kan leiden tot een AddressBindException bij het starten van de regio-server, zoals wordt weergegeven in de volgende Logboeken. U kunt dit controleren in de regio-server.log in de map /var/log/hbase op de worker-knooppunten waar de regio-server niet kan worden gestart. 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Wilt u de belasting op de HBase regio-servers te verminderen voordat u een opnieuw opstarten initiëren. 
2. U kunt ook (als stap 1 niet help), probeer het opnieuw wilt opstarten regio servers op de worker-knooppunten met behulp van de volgende opdrachten:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

