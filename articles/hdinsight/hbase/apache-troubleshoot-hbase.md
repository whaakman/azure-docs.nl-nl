---
title: Problemen met HBase oplossen met behulp van Azure HDInsight
description: Vind antwoorden op veelgestelde vragen over het werken met HBase en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: nitinver
ms.author: nitinver
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 7/7/2017
ms.openlocfilehash: e25a2dcaf9b7c820f5d7e0312fb2cb55fc558882
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593896"
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a>Problemen met HBase oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen als u werkt met Apache HBase-nettoladingen in Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Hoe voer ik hbck opdracht rapporten met meerdere niet-toegewezen regio's?

Een algemene foutbericht weergegeven dat u mogelijk zien bij het uitvoeren van de `hbase hbck` opdracht is "meerdere regio's worden niet-toegewezen of gaten in de keten van regio's."

In de HBase Master UI ziet u het aantal regio's die zijn niet-regelmatige voor alle regioservers. U kunt vervolgens uitvoert `hbase hbck` opdracht om te zien van gaten in de keten regio.

Gaten kunnen zijn veroorzaakt door de offline-regio's, de toewijzingen dus eerst oplossen. 

Om de niet-toegewezen gebieden terug naar een normale status, voert u de volgende stappen uit:

1. Meld u aan het HDInsight-HBase-cluster met behulp van SSH.
2. Uitvoeren om te verbinden met de ZooKeeper-shell, de `hbase zkcli` opdracht.
3. Voer de `rmr /hbase/regions-in-transition` opdracht of het `rmr /hbase-unsecure/regions-in-transition` opdracht.
4. Om af te sluiten van de `hbase zkcli` -shell, gebruikt u de `exit` opdracht.
5. Open de Apache Ambari-gebruikersinterface en de actieve HBase Master-service vervolgens opnieuw te starten.
6. Voer de `hbase hbck` opdracht opnieuw (zonder opties). Controleer de uitvoer van deze opdracht om ervoor te zorgen dat alle regio's worden toegewezen.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hoe kan ik time-outproblemen oplossen bij het gebruik van hbck opdrachten voor toewijzingen voor regio?

### <a name="issue"></a>Probleem

Een mogelijke oorzaak voor time-outproblemen wanneer u de `hbck` opdracht is mogelijk dat meerdere regio's in de status 'in de overgangsfase"gedurende een lange periode zijn. U ziet deze regio's als offline in de HBase Master UI. Omdat een groot aantal regio's om een overgang probeert, wordt HBase Master mogelijk time-out en kan deze regio's weer online brengen.

### <a name="resolution-steps"></a>Oplossingen

1. Meld u aan het HDInsight-HBase-cluster met behulp van SSH.
2. Uitvoeren om te verbinden met de ZooKeeper-shell, de `hbase zkcli` opdracht.
3. Voer de `rmr /hbase/regions-in-transition` of de `rmr /hbase-unsecure/regions-in-transition` opdracht.
4. Om af te sluiten de `hbase zkcli` -shell, gebruikt u de `exit` opdracht.
5. In de Ambari-UI de actieve HBase Master-service opnieuw te starten.
6. Voer de `hbase hbck -fixAssignments` opdracht opnieuw uit.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hoe ik geforceerde-/ uitschakelen HDFS veilige modus in een cluster?

### <a name="issue"></a>Probleem

Het lokale Hadoop Distributed File System (HDFS) is vastgelopen in de veilige modus op het HDInsight-cluster.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Deze fout kan zijn veroorzaakt door een fout op wanneer u de volgende HDFS-opdracht uitvoeren:

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

De grootte van het HDInsight-cluster is gewijzigd om een zeer weinig knooppunten. Het aantal knooppunten is hieronder of dicht bij de replicatiefactor HDFS.

### <a name="resolution-steps"></a>Oplossingen 

1. De status van het HDFS op het HDInsight-cluster ophalen door het uitvoeren van de volgende opdrachten:

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
2. U kunt ook de integriteit van het HDFS op het HDInsight-cluster controleren met behulp van de volgende opdrachten:

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

3. Als u dat vaststelt er zijn geen ontbreekt, beschadigd of under-gerepliceerde blokken of dat deze blokken kunnen worden genegeerd, voer de volgende opdracht worden de naam van knooppunt wordt uit de veilige modus:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Hoe herstel ik JDBC of SQLLine connectiviteit problemen met Apache Phoenix?

### <a name="resolution-steps"></a>Oplossingen

Om te verbinden met Phoenix, moet u het IP-adres van een actieve ZooKeeper-knooppunt opgeven. Zorg ervoor dat de ZooKeeper-service die sqlline.py probeert verbinding te maken is actief en werkend.
1. Meld u aan het HDInsight-cluster met behulp van SSH.
2. Voer de volgende opdracht in:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > U krijgt het IP-adres van de actieve ZooKeeper-knooppunt van de Ambari UI. Ga naar **HBase** > **snelkoppelingen** > **ZK\* (actief)** > **Zookeeper Info**. 

3. Als de sqlline.py maakt verbinding met Phoenix en geen time-out heeft, voert u de volgende opdracht uit om te valideren van de beschikbaarheid en de status van Phoenix uit:

   ```apache
           !tables
           !quit
   ```      
4. Als deze opdracht werkt, is er geen probleem. Het IP-adres dat is opgegeven door de gebruiker is mogelijk onjuist. Echter, als de opdracht wordt onderbroken voor een langere periode en vervolgens de volgende fout wordt weergegeven, verder met stap 5.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. De volgende opdrachten uitvoeren vanaf het hoofdknooppunt (hn0) om de status van het systeem Phoenix vast te stellen. Catalogustabel:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   De opdracht moet een fout geretourneerd die vergelijkbaar is met het volgende: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. In de Ambari-UI, voert u de volgende stappen uit om de HMaster-service op alle ZooKeeper-knooppunten opnieuw te starten:

    1. In de **samenvatting** sectie van HBase, Ga naar **HBase** > **actieve HBase Master**. 
    2. In de **onderdelen** sectie, de HBase Master-service opnieuw starten.
    3. Herhaal deze stappen voor alle resterende **stand-by HBase Master** services. 

Het kan duren voordat tot vijf minuten om de service HBase Master te laten stabiliseren en het herstelproces te voltooien. Herhaal de sqlline.py-opdrachten om te bevestigen dat het systeem na een paar minuten. Catalogustabel is en dat deze kan worden opgevraagd. 

Wanneer het systeem. Catalogustabel is weer in de normale, het probleem met de netwerkverbinding met Phoenix moet worden automatisch opgelost.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Wat ervoor zorgt dat een hoofd-server niet kunnen opstarten?

### <a name="error"></a>Fout 

Een atomic renaming fout optreedt.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Tijdens het opstarten is HMaster voltooid veel initialisatiestappen. Het gaat hierbij om gegevens uit de map maken (.tmp) naar de gegevensmap te verplaatsen. HMaster ook gekeken naar de map write-ahead Logboeken (WALs) om te zien of er een niet-reagerende regioservers, enzovoort. 

Tijdens het opstarten HMaster biedt een eenvoudige `list` opdracht op deze mappen. Als u op elk gewenst moment ziet HMaster een onverwacht bestand in een van deze mappen, een uitzondering genereert en niet wordt gestart.  

### <a name="probable-cause"></a>Mogelijke oorzaak

Probeer te identificeren van de tijdlijn van het bestand maken en vervolgens ziet als er is een proces loopt vast rond de tijd die het bestand is gemaakt in de logboeken van de server regio. (Contact opnemen met ondersteuning van HBase om u te helpen u dat doet). Dit helpt ons bieden krachtigere mechanismen, zodat u kunt voorkomen dat deze bug te maken en zorg ervoor dat de correcte proces afsluiten.

### <a name="resolution-steps"></a>Oplossingen

Controleer de aanroepstack en probeer om te bepalen welke map wordt mogelijk veroorzaakt door het probleem (bijvoorbeeld het mogelijk is de map WALs of .tmp). In de Cloud Explorer of via de HDFS-opdrachten, probeer vervolgens het probleembestand te vinden. Meestal is dit een \*-renamePending.json-bestand. (De \*-renamePending.json bestand is een logboekbestand dat wordt gebruikt voor het implementeren van de atomic-naamswijziging in het WASB-stuurprogramma. Vanwege fouten in deze implementatie, deze bestanden kunnen worden van links via na proces vastloopt, enzovoort.) Geforceerd verwijderen dit bestand in de Cloud Explorer of via de HDFS-opdrachten. 

Soms is er ook mogelijk een tijdelijk bestand met de naam er ongeveer als *$$$. $$$* op deze locatie. U moet gebruiken de HDFS `ls` opdracht om te zien van dit bestand; u kunt het bestand in de Cloud Explorer niet zien. Als u wilt dit bestand verwijderen, gebruikt u de opdracht HDFS `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Nadat u deze opdrachten hebt uitgevoerd, moet dat HMaster onmiddellijk worden gestart. 

### <a name="error"></a>Fout

Er is geen serveradres wordt vermeld in *hbase: meta* voor regio xxx.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

U ziet mogelijk een bericht op uw Linux-cluster waarmee wordt aangegeven dat de *hbase: meta* tabel is niet online. Met `hbck` kan rapporteren dat ' hbase: meta tabel replicaId 0 is niet gevonden in elke regio. " Het probleem kan zijn dat HMaster kan geen initialisatie uitvoeren nadat u HBase opnieuw opgestart. In de HMaster-Logboeken, ziet u het bericht: "Er is geen serveradres vermeld in hbase: metagegevens voor de regio hbase: back-up \<regionaam\>'.  

### <a name="resolution-steps"></a>Oplossingen

1. Voer de volgende opdrachten (werkelijke waarden voor de wijziging zoals van toepassing) in de HBase-shell:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Verwijder de *hbase: naamruimte* vermelding. Dit item is mogelijk dezelfde fout die wordt gerapporteerd als de *hbase: naamruimte* tabel wordt gescand.

3. HBase in een status running doorbrengt, in de Ambari-UI weer te geven de actieve HMaster-service opnieuw te starten.  

4. In de HBase-shell, zodat alle tabellen op die offline, voer de volgende opdracht:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Meer lezen

[Kan niet worden verwerkt de HBase-tabel](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Fout

HMaster een time-out optreedt bij een onherstelbare uitzondering die vergelijkbaar is met ' java.io.IOException: time-out 300000ms wachten op de tabel naamruimte moet worden toegewezen. "

### <a name="detailed-description"></a>Gedetailleerde beschrijving

U kunt dit probleem kan optreden als er veel tabellen en regio's die niet is leeggemaakt wanneer u uw HMaster-services opnieuw start. Opnieuw opstarten kan mislukken en ziet u de vorige foutbericht wordt weergegeven.  

### <a name="probable-cause"></a>Mogelijke oorzaak

Dit is een bekend probleem met de HMaster-service. Algemene cluster opstarttaken kunnen lang duren. HMaster wordt afgesloten omdat de tabel naamruimte nog niet is toegewezen. Dit gebeurt alleen in scenario's waarin grote hoeveelheid gegevens die unflushed bestaat en er is een time-out van vijf minuten is niet voldoende.
  
### <a name="resolution-steps"></a>Oplossingen

1. Ga in de Ambari-UI naar **HBase** > **Peeringconfiguraties**. Voeg de volgende instelling in het bestand aangepaste hbase-site.xml: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Start opnieuw op de vereiste services (HMaster en mogelijk andere HBase-services).  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Wat veroorzaakt een storing opnieuw opstarten op een regioserver?

### <a name="issue"></a>Probleem

Een fout opnieuw opstarten op een regioserver kan worden voorkomen door de volgende aanbevolen procedures. Het is raadzaam dat u zware werkbelasting activiteit onderbreken wanneer u van plan bent om opnieuw te starten van servers voor HBase-regio. Als een toepassing wordt voortgezet om te verbinden met regioservers wanneer shutdown bezig is, wordt regio server opnieuw opstarten trager worden door enkele minuten. Het is ook een goed idee om eerst alle tabellen leeg te maken. Zie voor informatie over het leegmaken van tabellen [HDInsight HBase: over het verbeteren van het opstarten van de HBase-cluster door het verplaatsen van tabellen](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Als u de opstartbewerking voor HBase-regioservers van de Ambari UI hebt gestart, ziet u meteen dat de regioservers werd afgesloten, maar ze niet meteen opnieuw. 

Dit is wat er gebeurt achter de schermen: 

1. Een aanvraag tot stoppen verzendt de Ambari-agent naar de regioserver.
2. De Ambari-agent wacht gedurende 30 seconden voor de regioserver zonder problemen worden afgesloten. 
3. Als uw toepassing nog steeds verbinding maken met de regioserver, wordt niet de server onmiddellijk afgesloten. De 30 seconden time-out is verstreken voordat de afsluiting. 
4. Na 30 seconden, de Ambari-agent verzendt een geforceerde-kill-instructie (`kill -9`) opdracht naar de regioserver. U kunt dit zien in het logboek van de ambari-agent (in de /var/log/map van de respectieve worker-knooppunt):

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
Vanwege de abrupte afsluiting, kan de poort die is gekoppeld aan het proces niet worden vrijgegeven, hoewel het serverproces regio is gestopt. Deze situatie kan leiden tot een AddressBindException bij het starten van de regioserver, zoals wordt weergegeven in de volgende Logboeken. U kunt dit controleren in de regio-server.log in de map /var/log/hbase op de worker-knooppunten waar de regioserver niet kan worden gestart. 

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

### <a name="resolution-steps"></a>Oplossingen

1. Probeer te verminderen van de werkbelasting op de HBase-regioservers voordat u een opnieuw opstarten initiëren. 
2. U kunt ook (als stap 1, niet wordt opgelost) Probeer handmatig opnieuw wilt opstarten regioservers op de worker-knooppunten met behulp van de volgende opdrachten:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)
