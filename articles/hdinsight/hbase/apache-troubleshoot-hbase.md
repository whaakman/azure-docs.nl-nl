---
title: Problemen met HBase oplossen met behulp van Azure HDInsight
description: Krijg antwoorden op veelgestelde vragen over het werken met HBase en Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 13a4831d946eb7e25e586cafae4cae51b49fd8a7
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780760"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Problemen met Apache HBase oplossen met Azure HDInsight

Meer informatie over de belangrijkste problemen en hun oplossingen bij het werken met Apache HBase-nettoladingen in Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Hoe kan ik hbck-opdracht rapporten met meerdere niet-toegewezen regio's uitvoeren?

Een veelvoorkomend fout bericht dat kan worden weer gegeven wanneer u de `hbase hbck` opdracht uitvoert, is dat meerdere regio's niet-toegewezen of gaten in de keten van regio's zijn.

In de HBase Master gebruikers interface ziet u het aantal regio's dat niet in balans is over alle regio servers. Vervolgens kunt u de opdracht `hbase hbck` uitvoeren om gaten in de regio keten weer te geven.

Openingen kunnen worden veroorzaakt door de offline regio's, daarom moet u de toewijzingen eerst herstellen. 

Voer de volgende stappen uit om de niet-toegewezen regio's terug te brengen naar een normale status:

1. Meld u aan bij het HDInsight HBase-cluster met behulp van SSH.
2. Als u verbinding wilt maken met de Apache ZooKeeper shell `hbase zkcli` , voert u de opdracht uit.
3. Voer de `rmr /hbase/regions-in-transition` opdracht of de `rmr /hbase-unsecure/regions-in-transition` opdracht uit.
4. Als u de `hbase zkcli` shell wilt afsluiten, gebruikt `exit` u de opdracht.
5. Open de Apache Ambari-gebruikers interface en start de Active HBase Master-service opnieuw.
6. Voer de `hbase hbck` opdracht opnieuw uit (zonder opties). Controleer de uitvoer van deze opdracht om te controleren of alle regio's worden toegewezen.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hoe kan ik time-outproblemen oplossen bij het gebruik van hbck-opdrachten voor regio toewijzingen?

### <a name="issue"></a>Probleem

Een mogelijke oorzaak van problemen met de time-out `hbck` wanneer u de opdracht gebruikt, kan het zijn dat verschillende regio's in de status ' in transitie ' lang zijn. U kunt deze regio's als offline weer geven in de gebruikers interface van HBase Master. Omdat er een groot aantal regio's probeert te overstappen, HBase Master mogelijk time-out en kan deze regio's niet weer online zetten.

### <a name="resolution-steps"></a>Oplossingen

1. Meld u aan bij het HDInsight HBase-cluster met behulp van SSH.
2. Als u verbinding wilt maken met de Apache ZooKeeper shell `hbase zkcli` , voert u de opdracht uit.
3. Voer de `rmr /hbase/regions-in-transition` of de `rmr /hbase-unsecure/regions-in-transition` opdracht uit.
4. Als u de `hbase zkcli` shell wilt afsluiten, `exit` gebruikt u de opdracht.
5. Start de Active HBase Master-service opnieuw in de gebruikers interface van Ambari.
6. Voer de `hbase hbck -fixAssignments` opdracht opnieuw uit.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hoe kan ik Force-veilige modus van HDFS in een cluster uitschakelen?

### <a name="issue"></a>Probleem

De lokale Apache Hadoop Distributed File System (HDFS) is vastgelopen in de veilige modus op het HDInsight-cluster.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Deze fout kan zijn veroorzaakt door een fout wanneer u de volgende HDFS-opdracht uitvoert:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

De fout die wordt weer gegeven wanneer u probeert de opdracht uit te voeren, ziet er als volgt uit:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
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

Het HDInsight-cluster is naar een paar knoop punten omlaag geschaald. Het aantal knoop punten is lager of dicht bij de HDFS-replicatie factor.

### <a name="resolution-steps"></a>Oplossingen 

1. Haal de status van de HDFS op het HDInsight-cluster op door de volgende opdrachten uit te voeren:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
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
   hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
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

3. Als u vaststelt dat er geen ontbrekende, beschadigde of onder gerepliceerde blokken bestaan of dat deze blokken kunnen worden genegeerd, voert u de volgende opdracht uit om het naam-knoop punt uit de veilige modus te halen:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Hoe kan ik kunt u de verbindings problemen van JDBC of sqlline gebruiken met Apache Phoenix oplossen?

### <a name="resolution-steps"></a>Oplossingen

Als u verbinding wilt maken met Apache Phoenix, moet u het IP-adres van een actief Apache ZooKeeper knoop punt opgeven. Zorg ervoor dat de ZooKeeper-service waarmee sqlline.py verbinding probeert te maken, actief is.
1. Meld u aan bij het HDInsight-cluster met behulp van SSH.
2. Voer de volgende opdracht in:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > U kunt het IP-adres van het actieve ZooKeeper-knoop punt ophalen uit de Ambari-gebruikers interface. Ga naar **HBase** > **Quick links** > **ZK(\* Active)**  > **Zookeeper info**. 

3. Als de sqlline.py verbinding maakt met Phoenix en geen time-out heeft, voert u de volgende opdracht uit om de beschik baarheid en de status van Breda te valideren:

   ```apache
           !tables
           !quit
   ```      
4. Als deze opdracht werkt, is er geen probleem. Het IP-adres van de gebruiker is mogelijk onjuist. Als de opdracht echter een lange tijd pauzeert en vervolgens de volgende fout wordt weer gegeven, gaat u verder met stap 5.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Voer de volgende opdrachten uit vanuit het hoofd knooppunt (hn0) om de voor waarde van het Phoenix-systeem vast te stellen. CATALOGUS tabel:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   De opdracht moet een fout retour neren die vergelijkbaar is met het volgende: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. In de Apache Ambari-gebruikers interface voert u de volgende stappen uit om de HMaster-service opnieuw op alle ZooKeeper-knoop punten te starten:

    1. Ga in de sectie **samen vatting** van HBase naar **HBase** > **Active HBase Master**. 
    2. Start in de sectie **onderdelen** de HBase Master-service opnieuw.
    3. Herhaal deze stappen voor alle resterende **stand-by-HBase Master** Services. 

Het kan tot vijf minuten duren voordat de HBase Master service het herstel proces stabiliseren en voltooit. Na enkele minuten herhaalt u de sqlline.py-opdrachten om te bevestigen dat het systeem. De catalogus tabel is actief en er kan een query op worden uitgevoerd. 

Wanneer het systeem. De catalogus tabel is weer normaal, het connectiviteits probleem met de Breda moet automatisch worden opgelost.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Wat leidt ertoe dat een master-server niet kan worden gestart?

### <a name="error"></a>Fout 

Er treedt een Atomic-naamswijziging op.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Tijdens het opstart proces voert HMaster veel initialisatie stappen uit. Dit zijn onder andere het verplaatsen van gegevens van de map Scratch (. tmp) naar de map Data. HMaster controleert ook de map write-Ahead logs (WALs) om te zien of er niet-reagerende regio servers, enzovoort. 

HMaster voert een eenvoudige `list` opdracht uit voor deze mappen tijdens het opstarten. Als er op elk moment HMaster een onverwacht bestand in een van deze mappen ziet, wordt er een uitzonde ring gegenereerd en start niet.  

### <a name="probable-cause"></a>Mogelijke oorzaak

In de logboeken van de regio server probeert u de tijd lijn van het maken van het bestand te identificeren en vervolgens te zien of er een proces is vastgelopen rond het tijdstip waarop het bestand is gemaakt. (Neem contact op met HBase ondersteuning voor hulp bij het uitvoeren van dit probleem.) Dit helpt ons krachtigere mechanismen te bieden, zodat u deze fout kunt voor komen en ervoor zorgen dat het proces zonder problemen wordt afgesloten.

### <a name="resolution-steps"></a>Oplossingen

Controleer de aanroep stack en probeer te bepalen welke map het probleem veroorzaakt (bijvoorbeeld omdat het de map WALs of de map. tmp kan zijn). Probeer vervolgens in Cloud Explorer of met HDFS-opdrachten het probleem bestand te vinden. Doorgaans is dit een \*renamePending. JSON-bestand. (Het \*bestand-renamePending. json is een logboek bestand dat wordt gebruikt voor het implementeren van de bewerking voor Atoom wijzigen in het WASB-stuur programma. Als gevolg van fouten in deze implementatie kunnen deze bestanden overblijven nadat het proces is vastgelopen, enzovoort.) Force-Hiermee kunt u dit bestand in Cloud Verkenner of met HDFS-opdrachten verwijderen. 

Soms kan er ook een tijdelijk bestand zijn met de naam iets zoals *$ $ $. $ $ $* op deze locatie. U moet de HDFS `ls` -opdracht gebruiken om dit bestand te bekijken. u kunt het bestand niet zien in Cloud Explorer. Als u dit bestand wilt verwijderen, gebruikt u `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`de opdracht HDFS.  

Nadat u deze opdrachten hebt uitgevoerd, moet HMaster onmiddellijk worden gestart. 

### <a name="error"></a>Fout

Er wordt geen server adres vermeld in *hbase: meta* voor regio xxx.

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Mogelijk wordt er een bericht weer gegeven op uw Linux-cluster dat aangeeft dat de *hbase: meta* -tabel niet online is. Als `hbck` wordt uitgevoerd kan worden gerapporteerd dat "hbase: de meta Table replicaId 0 niet in een wille keurige regio is gevonden." Het probleem kan zijn dat HMaster niet kan worden geïnitialiseerd na het opnieuw opstarten van HBase. In de HMaster-logboeken ziet u mogelijk het volgende bericht: "Geen server adres vermeld in hbase: meta voor regio hbase: naam \<\>van back-upregio".  

### <a name="resolution-steps"></a>Oplossingen

1. Voer in de HBase-shell de volgende opdrachten in (werkelijke waarden wijzigen indien van toepassing):  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Verwijder de vermelding *hbase: naam ruimte* . Dit item is mogelijk dezelfde fout die wordt gerapporteerd wanneer de *hbase: naam ruimte* tabel wordt gescand.

3. Als u HBase in de Ambari-gebruikers interface wilt weer geven, start u de actieve HMaster-service opnieuw.  

4. Voer in de HBase-shell de volgende opdracht uit om alle offline tabellen te openen:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Meer lezen

[Kan de HBase-tabel niet verwerken](https://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Fout

HMaster keer een time-out met een onherstelbare uitzonde ring op ' Java. io. IOException: Out 300000ms wacht op toewijzing van naam ruimte tabel. "

### <a name="detailed-description"></a>Gedetailleerde beschrijving

Dit probleem kan optreden als u veel tabellen en regio's hebt die niet zijn leeg gemaakt wanneer u uw HMaster-services opnieuw opstart. Opnieuw opstarten kan mislukken en het vorige fout bericht wordt weer gegeven.  

### <a name="probable-cause"></a>Mogelijke oorzaak

Dit is een bekend probleem met de HMaster-service. Algemene opstart taken van het cluster kunnen veel tijd in beslag nemen. HMaster wordt afgesloten omdat de naam ruimte tabel nog niet is toegewezen. Dit gebeurt alleen in scenario's waarin grote hoeveel heden niet-verleegde gegevens bestaan en een time-out van vijf minuten niet voldoende is.
  
### <a name="resolution-steps"></a>Oplossingen

1. Ga in de Apache Ambari-gebruikers interface naar **HBase** > -**configuraties**. Voeg in het aangepaste hbase-site. XML-bestand de volgende instelling toe: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Start de vereiste services (HMaster en mogelijk andere HBase-Services) opnieuw op.  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Wat veroorzaakt een opstart fout op een regio server?

### <a name="issue"></a>Probleem

Het opnieuw opstarten van een regio server kan worden voor komen door de aanbevolen procedures te volgen. We raden u aan om een zware werk belasting te onderbreken wanneer u van plan bent om de HBase-regio servers opnieuw te starten. Als een toepassing verbinding blijft maken met regio servers wanneer het afsluiten wordt uitgevoerd, wordt de bewerking voor het opnieuw starten van de regio server met enkele minuten langzamer. Het is ook een goed idee om eerst alle tabellen te wissen. Zie [HDInsight HBase voor een Naslag informatie over het leegmaken van tabellen: De start tijd van het Apache HBase-cluster verbeteren door tabellen](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/)te wissen.

Als u de bewerking voor opnieuw opstarten start op HBase regio servers van de Apache Ambari-gebruikers interface, ziet u onmiddellijk dat de regio servers zijn uitgeschakeld, maar ze worden niet meteen opnieuw opgestart. 

De volgende what's vindt plaats achter de schermen: 

1. De Ambari-agent verzendt een Stop aanvraag naar de regio server.
2. De Ambari-agent wacht 30 seconden totdat de regio server op correcte wijze wordt afgesloten. 
3. Als uw toepassing verbinding blijft maken met de regio server, wordt de server niet onmiddellijk afgesloten. De time-out van 30 seconden verloopt voordat het afsluiten plaatsvindt. 
4. Na 30 seconden verzendt de Ambari-agent een opdracht geforceerd afsluiten (`kill -9`) naar de regio server. U kunt dit zien in het logboek van de ambari-agent (in de map/var/log/van het desbetreffende worker-knoop punt):

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
   Vanwege het plotseling afsluiten wordt de poort die is gekoppeld aan het proces mogelijk niet vrijgegeven, ook al is de regio Server proces gestopt. Deze situatie kan leiden tot een AddressBindException wanneer de regio server wordt gestart, zoals wordt weer gegeven in de volgende logboeken. U kunt dit controleren in het Region-server. log in de/var/log/hbase-map op de worker-knoop punten waar de regio server niet kan worden gestart. 

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

1. Probeer de belasting van de HBase-regio servers te verminderen voordat u opnieuw opstart. 
2. U kunt ook de regio servers op de worker-knoop punten hand matig opnieuw starten met de volgende opdrachten (als stap 1 niet helpt):

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)
