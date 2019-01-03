---
title: Cluster-prestaties bewaken - Azure HDInsight
description: Klik hier voor meer informatie over het bewaken van een HDInsight-cluster voor capaciteit en prestaties.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: arindamc
ms.openlocfilehash: 83cbb2a54ed712f8aa5084408ab852432470b73c
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742470"
---
# <a name="monitor-cluster-performance"></a>Cluster-prestaties bewaken

Bewaking van de status en prestaties van een HDInsight-cluster is het essentieel is voor het onderhouden van optimale prestaties en gebruik van resources. Controle kan ook helpen bij het detecteren en fouten van configuratie van de cluster en de gebruiker code problemen.

De volgende secties wordt beschreven hoe u bewaken en optimaliseren van de belasting van uw clusters, Apache Hadoop YARN-wachtrijen en opslag beperking problemen detecteren.

## <a name="monitor-cluster-load"></a>Monitor cluster laden

Hadoop-clusters kunnen de meest optimale prestaties leveren wanneer de belasting van het cluster wordt evenredig verdeeld over alle knooppunten. Hierdoor kunnen de verwerkingstaken die worden uitgevoerd zonder te worden beperkt door het RAM-geheugen of CPU, schijfbronnen op afzonderlijke knooppunten.

Als u een Kijk op de knooppunten van het cluster en hun laden, moet u zich aanmelden bij de [Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)en selecteer vervolgens de **Hosts** tabblad. Uw hosts worden weergegeven door de volledig gekwalificeerde domeinnamen. Operationele status van elke host wordt weergegeven door een gekleurde integriteitsindicator:

| Kleur | Description |
| --- | --- |
| Rood | Ten minste één master onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen betrokken knopinfo. |
| Orange | Ten minste één ondergeschikt onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen betrokken knopinfo. |
| Geel | Ambari-Server is geen heartbeat ontvangen van de host voor meer dan 3 minuten. |
| Groen | Normaal uitgevoerd staat. |

U ziet ook de kolommen van het aantal kernen en de hoeveelheid RAM-geheugen voor elke host en het gebruik van de schijf en de belasting gemiddelde.

![Tabblad hosts](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Selecteer een van de hostnamen voor een gedetailleerde Kijk op de onderdelen die worden uitgevoerd op die host en de metrische gegevens. De metrische gegevens worden weergegeven als een selecteerbaar tijdlijn van CPU-gebruik, load schijfgebruik, geheugengebruik, netwerkgebruik en aantallen processen.

![details van de host](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Zie [beheren HDInsight-clusters met behulp van de Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) voor meer informatie over het instellen van waarschuwingen en metrische gegevens weergeven.

## <a name="yarn-queue-configuration"></a>Configuratie van de YARN-wachtrij

Hadoop heeft verschillende services die op de gedistribueerde platform worden uitgevoerd. YARN (nog een andere Resource Negotiator) coördineert van deze services en resources van het cluster om ervoor te zorgen dat een elke belasting evenredig verdeeld over de cluster kan worden toegewezen.

YARN verdeelt de verantwoordelijkheden van de twee van de JobTracker, resourcebeheer en -taak plannen/bewaking, in twee daemons: een globale Resource Manager en een per toepassing ApplicationMaster (uur).

De Resource Manager is een *pure scheduler*, en uitsluitend arbitrates beschikbare resources tussen alle concurrerende toepassingen. De Resource Manager zorgt ervoor dat alle resources zich altijd in gebruik, optimaliseren voor verschillende constanten, sla's, zoals capaciteit gegarandeerd, enzovoort. De ApplicationMaster onderhandelt over de resources van de Resource Manager, en werkt met de NodeManager(s) uit te voeren en de containers en hun gebruik van resources bewaken.

Wanneer u meerdere tenants deelt een groot cluster, is de concurrentie voor resources van het cluster. De CapacityScheduler is een pluggable scheduler die bij het resource helpt-wachtrij plaatsen van aanvragen voor delen. Ook biedt ondersteuning voor de CapacityScheduler *hiërarchische wachtrijen* om ervoor te zorgen dat resources worden gedeeld tussen de onderliggende wachtrijen van een organisatie, voordat de andere toepassingen wachtrijen gratis resources te gebruiken die zijn toegestaan.

YARN kan we resources toewijzen aan deze wachtrijen, en laat u zien of al uw beschikbare resources zijn toegewezen. Als u informatie over uw wachtrijen, zich aanmelden bij de Ambari-Webgebruikersinterface, en selecteer vervolgens **YARN wachtrijbeheerder** in het menu bovenaan.

![YARN-wachtrijbeheerder](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

De wachtrijbeheerder van de YARN-pagina bevat een overzicht van de wachtrijen aan de linkerkant, samen met het percentage van de capaciteit die zijn toegewezen aan elk.

![Pagina met details van YARN wachtrijbeheerder](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Voor een meer gedetailleerde Kijk op uw wachtrijen van de Ambari-dashboard, selecteert u de **YARN** service in de lijst aan de linkerkant. Klik vervolgens onder de **snelkoppelingen** in het vervolgkeuzemenu selecteren **Resource Manager UI** onder het actieve knooppunt.

![Koppeling van Resource Manager-UI-menu](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

Selecteer in het Resource Manager-UI **Scheduler** in het menu links. U ziet een lijst met uw wachtrijen onder *toepassingswachtrijen*. Hier ziet u de capaciteit die wordt gebruikt voor elk van de wachtrijen zijn, hoe goed de taken zijn verdeeld over deze, en of alle taken zijn beperkte capaciteit.

![Koppeling van Resource Manager-UI-menu](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Beperking van opslag

Bottleneck in de prestaties van een cluster kan gebeuren op het opslagniveau. Dit type knelpunt wordt meestal veroorzaakt *blokkeren* invoer/uitvoer (I/O)-bewerkingen, die zich voordoen wanneer uw actieve taken meer i/o verzendt dan de storage-service kan verwerken. Deze blokkering, maakt een wachtrij van i/o-aanvragen moeten worden verwerkt totdat nadat de huidige IOs worden verwerkt. De blokken zijn vanwege *opslag beperking*, dit is geen limiet voor een fysieke, maar in plaats daarvan een limiet die zijn opgelegd door de storage-service door een service level agreement (SLA). Deze beperking zorgt ervoor dat geen enkele client of de tenant in beslag de service nemen kan. De SLA beperkt het aantal i/o per seconde (IOP's) voor Azure Storage - voor meer informatie, Zie [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Als u gebruikmaakt van Azure Storage, voor informatie over het bewaken van problemen met betrekking tot opslag, met inbegrip van beperking, Zie [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Als Azure Data Lake Storage (ADLS) van uw cluster externe opslag is is de beperking waarschijnlijk vanwege de bandbreedte. Beperking kan in dit geval worden geïdentificeerd met geobserveerd beperkingsfouten in Logboeken van de taak. Zie voor ADLS, de bandbreedteregeling punt voor de betreffende service in deze artikelen:

* [Richtlijnen voor Apache Hive in HDInsight en Azure Data Lake Storage afstemmen van prestaties](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Richtlijnen voor MapReduce in HDInsight en Azure Data Lake Storage afstemmen van prestaties](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Richtlijnen voor Apache Storm op HDInsight en Azure Data Lake Storage afstemmen van prestaties](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over het oplossen van problemen en uw clusters controleren:

* [HDInsight-logboekbestanden analyseren](hdinsight-debug-jobs.md)
* [Fouten opsporen in apps met Apache Hadoop YARN-Logboeken](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Heapdumps voor Apache Hadoop-services op Linux gebaseerde HDInsight inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
