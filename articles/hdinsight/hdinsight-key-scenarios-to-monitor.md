---
title: Cluster-prestaties bewaken - Azure HDInsight
description: Klik hier voor meer informatie over het bewaken van een HDInsight-cluster voor capaciteit en prestaties.
services: hdinsight
author: maxluk
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/27/2017
ms.author: maxluk
ms.openlocfilehash: 5f0c390fb5749ec5a7dbf3ca7eb541c0aa1133e9
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599568"
---
# <a name="monitor-cluster-performance"></a>Cluster-prestaties bewaken

Bewaking van de status en prestaties van een HDInsight-cluster is het essentieel is voor het onderhouden van maximale prestaties en gebruik van resources. Controle kan ook helpen bij het adres mogelijk coderen of configuratiefouten van de cluster.

De volgende secties beschrijven over het optimaliseren van het laden van cluster, YARN wachtrij efficiëntie en toegankelijkheid van storage.

## <a name="cluster-loading"></a>Het laden van cluster

Hadoop-clusters moeten in evenwicht laden op de knooppunten van het cluster. Deze balancing voorkomt dat verwerkingstaken wordt beperkt door het RAM-geheugen, CPU of schijfresources.

Als u een Kijk op de knooppunten van het cluster en hun laden, moet u zich aanmelden bij de [Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)en selecteer vervolgens de **Hosts** tabblad. Uw hosts worden weergegeven door de volledig gekwalificeerde domeinnamen. Operationele status van elke host wordt weergegeven door een gekleurde integriteitsindicator:

| Kleur | Beschrijving |
| --- | --- |
| Rood | Ten minste één master onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen betrokken knopinfo. |
| Orange | Ten minste één slave-onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen betrokken knopinfo. |
| Geel | Ambari-Server is geen heartbeat ontvangen van de host voor meer dan 3 minuten. |
| Groen | Normaal uitgevoerd staat. |

U ziet ook de kolommen van het aantal kernen en de hoeveelheid RAM-geheugen voor elke host en het gebruik van de schijf en de belasting gemiddelde.

![Tabblad hosts](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Selecteer een van de hostnamen voor een gedetailleerde Kijk op de onderdelen die worden uitgevoerd op die host en de metrische gegevens. De metrische gegevens worden weergegeven als een selecteerbaar tijdlijn van CPU-gebruik, load schijfgebruik, geheugengebruik, netwerkgebruik en aantallen processen.

![details van de host](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Zie [beheren HDInsight-clusters met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) voor meer informatie over het instellen van waarschuwingen en metrische gegevens weergeven.

## <a name="yarn-queue-configuration"></a>Configuratie van de YARN-wachtrij

Hadoop heeft verschillende services die op de gedistribueerde platform worden uitgevoerd. YARN (nog een andere Resource Negotiator) coördinaten van deze services, clusterbronnen toewijst en beheert de toegang tot een algemene gegevensset.

YARN verdeelt de verantwoordelijkheden van de twee van de JobTracker, resourcebeheer en -taak plannen/bewaking, in twee daemons: een globale ResourceManager en een per toepassing ApplicationMaster (uur).

De ResourceManager is een *pure scheduler*, en uitsluitend arbitrates beschikbare resources tussen alle concurrerende toepassingen. De ResourceManager zorgt ervoor dat alle resources zich altijd in gebruik, optimaliseren voor verschillende constanten, sla's, zoals capaciteit gegarandeerd, enzovoort. De ApplicationMaster onderhandelt over resources uit de ResourceManager, en werkt met de NodeManager(s) uit te voeren en de containers en hun gebruik van resources bewaken.

Wanneer u meerdere tenants deelt een groot cluster, is de concurrentie voor resources van het cluster. De CapacityScheduler is een pluggable scheduler die bij het resource helpt-wachtrij plaatsen van aanvragen voor delen. Ook biedt ondersteuning voor de CapacityScheduler *hiërarchische wachtrijen* om ervoor te zorgen dat resources worden gedeeld tussen de onderliggende wachtrijen van een organisatie, voordat de andere toepassingen wachtrijen gratis resources te gebruiken die zijn toegestaan.

YARN kan we resources toewijzen aan deze wachtrijen, en laat u zien of al uw beschikbare resources zijn toegewezen. Als u informatie over uw wachtrijen, meld u aan bij de Ambari-Webgebruikersinterface, en selecteer vervolgens **YARN wachtrijbeheerder** in het menu bovenaan.

![YARN-wachtrijbeheerder](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

De wachtrijbeheerder van de YARN-pagina bevat een overzicht van de wachtrijen aan de linkerkant, samen met het percentage van de capaciteit die zijn toegewezen aan elk.

![Pagina met details van YARN wachtrijbeheerder](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Voor een meer gedetailleerde Kijk op uw wachtrijen van de Ambari-dashboard, selecteert u de **YARN** service in de lijst aan de linkerkant. Klik onder de **snelkoppelingen** in het vervolgkeuzemenu selecteren **ResourceManager UI** onder het actieve knooppunt.

![ResourceManager UI menu koppeling](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

Selecteer in de UI ResourceManager **Scheduler** in het menu links. U ziet een lijst met uw wachtrijen onder *toepassingswachtrijen*. Hier ziet u de capaciteit die wordt gebruikt voor elk van de wachtrijen zijn, hoe goed de taken zijn verdeeld over deze, en of alle taken zijn beperkte capaciteit.

![ResourceManager UI menu koppeling](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Beperking van opslag

Bottleneck in de prestaties van een cluster kan gebeuren op het opslagniveau. Dit type knelpunt wordt meestal veroorzaakt *blokkeren* invoer/uitvoer (I/O)-bewerkingen, die zich voordoen wanneer uw actieve taken meer i/o verzendt dan de storage-service kan verwerken. Deze blokkering, maakt een wachtrij van i/o-aanvragen moeten worden verwerkt totdat nadat de huidige IOs worden verwerkt. De blokken zijn vanwege *opslag beperking*, dit is geen limiet voor een fysieke, maar in plaats daarvan een limiet die zijn opgelegd door de storage-service door een service level agreement (SLA). Deze beperking zorgt ervoor dat geen enkele client of de tenant in beslag de service nemen kan. De SLA beperkt het aantal i/o per seconde (IOP's) voor Azure Storage - voor meer informatie, Zie [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Als u gebruikmaakt van Azure Storage, voor informatie over het bewaken van problemen met betrekking tot opslag, met inbegrip van beperking, Zie [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Als Azure Data Lake Store (ADLS) van uw cluster externe opslag is is de beperking waarschijnlijk vanwege de bandbreedte. Beperking kan in dit geval worden geïdentificeerd met geobserveerd beperkingsfouten in Logboeken van de taak. Zie voor ADLS, de bandbreedteregeling punt voor de betreffende service in deze artikelen:

* [Richtlijnen voor het afstemmen van de prestaties van Hive in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Richtlijnen voor het afstemmen van de prestaties van MapReduce in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Richtlijnen voor het afstemmen van de prestaties van Storm in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over het oplossen van problemen en uw clusters controleren:

* [HDInsight-logboekbestanden analyseren](hdinsight-debug-jobs.md)
* [Fouten in apps opsporen met YARN-logboeken](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Heapdumps voor Hadoop op Linux gebaseerde HDInsight-services inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
