---
title: Apache HBase Master kan niet worden gestart in azure HDInsight
description: Apache HBase Master (HMaster) kan niet worden gestart in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/06/2019
ms.openlocfilehash: 8368ebfca4cdd72c5c455a04e29b6c0cb44938ea
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935404"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Apache HBase Master (HMaster) kan niet worden gestart in azure HDInsight

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen bij het werken met Azure HDInsight-clusters.

## <a name="scenario-atomic-renaming-failure"></a>Scenario: Fout bij het wijzigen van atomische naam

### <a name="issue"></a>Probleem

Onverwachte bestanden geïdentificeerd tijdens het opstart proces.

### <a name="cause"></a>Oorzaak

Tijdens het opstart proces voert HMaster veel initialisatie stappen uit, waaronder het verplaatsen van gegevens uit een nieuwe map (. tmp) naar de map Data. HMaster controleert ook de map WALs (write-Ahead Logboeken) om te zien of er dode regio servers zijn. In al deze situaties wordt een eenvoudige `list` opdracht voor deze mappen. Als er op elk gewenst moment een onverwacht bestand in een van deze mappen wordt weer gave, wordt er een uitzonde ring gegenereerd en kan deze niet worden gestart.

### <a name="resolution"></a>Oplossing

Controleer in een dergelijke situatie de aanroep stack om te zien welke map het probleem veroorzaakt (bijvoorbeeld WALs map of. tmp-map). Ga vervolgens via Cloud Explorer of via hdfs opdrachten naar het probleem bestand. Het probleem bestand is doorgaans een `*-renamePending.json` bestand (een logboek bestand dat wordt gebruikt voor het implementeren van een Atomic-naamswijziging in het WASB-stuur programma). Als gevolg van fouten in deze implementatie kunnen dergelijke bestanden overblijven in geval van proces storingen. Dit bestand geforceerd verwijderen via Cloud Explorer. Daarnaast kan er een tijdelijk bestand zijn met de aard $ op deze locatie. Het bestand kan niet worden weer gegeven via Cloud Explorer en alleen `ls` via de opdracht hdfs. U kunt de opdracht `hdfs dfs -rm //\$\$\$.\$\$\$` hdfs gebruiken om dit bestand te verwijderen.

Zodra het probleem bestand is verwijderd, moet HMaster onmiddellijk worden opgestart.

---

## <a name="scenario-no-server-address-listed"></a>Scenario: Er is geen server adres vermeld

### <a name="issue"></a>Probleem

In HMaster-Logboeken wordt een fout bericht weer gegeven dat vergelijkbaar is met ' geen server adres vermeld in hbase: meta voor regio xxx '.

### <a name="cause"></a>Oorzaak

HMaster kan niet worden geïnitialiseerd na het opnieuw opstarten van HBase.

### <a name="resolution"></a>Oplossing

1. Voer de volgende opdrachten uit in de HBase-shell (werkelijke waarden wijzigen indien van toepassing):

    ```
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>' 
    ```

1. De vermelding van hbase: naam ruimte verwijderen als dezelfde fout kan worden gerapporteerd tijdens het scannen van hbase: naam ruimte tabel.

1. Start de actieve HMaster van de Ambari-gebruikers interface opnieuw op om HBase in de actieve staat te brengen.

1. Voer de volgende opdracht uit in HBase shell om alle offline tabellen te openen:

    ```
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Scenario: Java. io. IOException: Out

### <a name="issue"></a>Probleem

HMaster is een time-out met `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`een onherstelbare uitzonde ring zoals.

### <a name="cause"></a>Oorzaak

De time-out is een bekend defect met HMaster. Algemene opstart taken van het cluster kunnen veel tijd in beslag nemen. HMaster wordt afgesloten als de naam ruimte tabel nog niet is toegewezen. De langdurige opstart taken worden uitgevoerd, waarbij een grote hoeveelheid niet-geflushe gegevens bestaat en een time-out van vijf minuten niet voldoende is.

### <a name="resolution"></a>Oplossing

1. Toegang tot de Ambari-gebruikers interface, gaat u naar HBase-> `hbase-site.xml` -configuraties. in aangepaste voegt u de volgende instelling toe:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. De vereiste services opnieuw opstarten (voornamelijk HMaster en mogelijk andere HBase-Services).

---

## <a name="scenario-frequent-regionserver-restarts"></a>Scenario: Frequente regionserver opnieuw opstarten

### <a name="issue"></a>Probleem

Knoop punten worden periodiek opnieuw opgestart. In de regionserver-logboeken ziet u mogelijk vermeldingen die vergelijkbaar zijn met:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Oorzaak

Lange regionserver JVM GC-onderbreking. De onderbreking zorgt ervoor dat regionserver niet meer reageert en geen harte slag kan verzenden naar HMaster binnen de time-out van de ZK-sessie 40s. HMaster gaat ervan uit dat regionserver dood is en de regionserver afbreekt en opnieuw wordt opgestart.

### <a name="resolution"></a>Oplossing

Wijzig de time-out van de Zookeeper-sessie, niet `zookeeper.session.timeout` alleen hbase-site-instelling, `maxSessionTimeout` maar ook Zookeeper Zoo. cfg-instelling moet worden gewijzigd.

1. Open de Ambari-gebruikers interface, ga naar **HBase-> configuraties-> instellingen**in de sectie time-outs, wijzig de waarde van Zookeeper sessietime time-out.

1. Toegang tot de Ambari-gebruikers interface, gaat u naar **Zookeeper->-configuratie-> aangepaste** Zoo. cfg, toevoegen/wijzigen van de volgende instelling. Zorg ervoor dat de waarde gelijk is aan hbase `zookeeper.session.timeout`.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. De vereiste services opnieuw opstarten.

---

## <a name="scenario-log-splitting-failure"></a>Scenario: Fout bij splitsen logboek

### <a name="issue"></a>Probleem

HMasters is niet beschikbaar op een HBase-cluster.

### <a name="cause"></a>Oorzaak

Onjuist geconfigureerde HDFS-en HBase-instellingen voor een secundair opslag account.

### <a name="resolution"></a>Oplossing

Stel hbase. rootdir: wasb://@.blob.core.windows.net/hbase en start services opnieuw op Ambari.

---

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring. Verbinding maken met de Azure-community met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees [hoe u een ondersteunings aanvraag voor Azure kunt maken](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)voor meer informatie. De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
