---
title: Controleren van de prestaties van het cluster - Azure HDInsight | Microsoft Docs
description: Het bewaken van een HDInsight-cluster voor capaciteit en prestaties.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: maxluk
ms.openlocfilehash: f2333202cdccc82edea649ff1c295752a414c8b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-cluster-performance"></a>Cluster-prestaties bewaken

Bewaking van de status en prestaties van een HDInsight-cluster is essentieel voor het onderhouden van de maximale prestaties en bronnen beter worden benut. Controle ook kunt u mogelijke coderen adres of configuratiefouten van de cluster.

De volgende secties beschrijven het optimaliseren van cluster laden, YARN wachtrij efficiëntie en toegankelijkheid van de opslag.

## <a name="cluster-loading"></a>Laden van het cluster

Hadoop-clusters moeten in evenwicht laden op de knooppunten van het cluster. Deze balancing voorkomt u dat verwerkingstaken wordt beperkt door het RAM-geheugen, CPU of schijfbronnen.

Als u de knooppunten van het cluster en hun laden op hoog niveau bekijken, moet u zich aanmelden bij de [Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md), selecteer vervolgens de **Hosts** tabblad. Uw hosts worden weergegeven door de volledig gekwalificeerde domeinnamen. Operationele status van elke host wordt weergegeven door een indicator gekleurde health:

| Kleur | Beschrijving |
| --- | --- |
| Rood | Ten minste één master onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen geïnfecteerd knopinfo. |
| Orange | Ten minste één slave onderdeel op de host is niet beschikbaar. Beweeg de muisaanwijzer om te zien dat een lijst met onderdelen geïnfecteerd knopinfo. |
| Geel | Ambari-Server heeft geen heartbeat van de host niet meer dan 3 minuten ontvangen. |
| Groen | Standaard-actieve status. |

U ziet ook kolommen met het aantal kernen en de hoeveelheid RAM-geheugen voor elke host en het schijfgebruik en de belasting gemiddelde.

![Tabblad](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Selecteer een van de hostnamen voor een gedetailleerde beschrijving onderdelen die worden uitgevoerd op die host en hun metrische gegevens. De metrische gegevens worden weergegeven als een selecteerbare tijdlijn van CPU-gebruik, load schijfgebruik, geheugengebruik, netwerkgebruik en aantal processen.

![details van de host](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Zie [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) voor meer informatie over het instellen van waarschuwingen en metrische gegevens te bekijken.

## <a name="yarn-queue-configuration"></a>YARN wachtrij configuratie

Hadoop heeft verschillende services uitvoert via het gedistribueerde platform. YARN (nog een andere Resource onderhandelaar) coördineert deze services, cluster-resources toegewezen en beheert de toegang tot een algemene gegevensset.

YARN verdeelt de twee verantwoordelijkheden van de JobTracker, bronbeheer en taak plannen/bewaking, in twee daemons: een globale ResourceManager en per toepassing ApplicationMaster (uur).

De ResourceManager is een *pure scheduler*, en uitsluitend arbitrates beschikbare resources tussen alle concurrerende toepassingen. De ResourceManager zorgt ervoor dat alle resources zich altijd in gebruik, optimaliseren voor verschillende constanten zoals Sla's, wordt gegarandeerd dat capaciteit, enzovoort. De ApplicationMaster resources uit de ResourceManager onderhandelt en werkt met de NodeManager(s) uitvoeren en controleren van de containers en het verbruik van hun.

Wanneer u meerdere tenants delen een grote cluster, is dit concurrentie voor resources van het cluster. De CapacityScheduler is een pluggable scheduler die helpt bij de resources te delen door de wachtrij van aanvragen. Ondersteunt ook de CapacityScheduler *hiërarchische wachtrijen* om ervoor te zorgen dat bronnen worden gedeeld tussen de onderliggende wachtrijen van een organisatie, voordat u andere toepassingen wachtrijen mogen gratis resources gebruiken.

YARN kan we resources toewijzen aan deze wachtrijen en ziet u of alle beschikbare resources zijn toegewezen. U kunt informatie over uw wachtrijen, meld u aan de Ambari-Webgebruikersinterface bij en selecteer vervolgens **YARN Queue Manager** in het menu bovenaan.

![YARN Queue Manager](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

De pagina YARN Queue Manager bevat een overzicht van uw wachtrijen aan de linkerkant, samen met het percentage van de capaciteit toegewezen aan elk.

![Pagina YARN Queue Manager-gegevens](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Selecteer voor een meer gedetailleerde Bekijk uw wachtrijen, vanuit het dashboard Ambari de **YARN** service in de lijst aan de linkerkant. Klik onder de **snelkoppelingen** Selecteer vervolgkeuzemenu **ResourceManager UI** onder uw actieve knooppunt.

![ResourceManager UI menu koppeling](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

Selecteer in de UI ResourceManager **Scheduler** uit in het menu links. U ziet een lijst met uw wachtrijen onder *toepassingswachtrijen*. Hier ziet u de capaciteit die wordt gebruikt voor elk van uw wachtrijen, hoe goed de taken worden gedistribueerd tussen deze twee, en of alle taken zijn beperkte capaciteit.

![ResourceManager UI menu koppeling](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Beperking van opslag

Een cluster prestatieknelpunt kan gebeuren op het opslagniveau van de. Dit type knelpunt meestal het gevolg is *blokkeren* input/output (I/O)-bewerkingen, die zich voordoen wanneer uw actieve taken meer IO verzenden dan de storage-service kan verwerken. Deze worden geblokkeerd, maakt een wachtrij van i/o-aanvragen moeten worden verwerkt totdat nadat de huidige IOs worden verwerkt. De blokken zijn vanwege *opslag beperking*, dit is geen limiet voor een fysieke, maar in plaats daarvan een limiet die zijn opgelegd door de storage-service door een service level agreement (SLA). Deze limiet zorgt ervoor dat geen enkele client of de tenant in beslag de service nemen kan. De SLA beperkt het aantal i/o per seconde (IOPS) voor Azure Storage - voor meer informatie, Zie [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Als u gebruikmaakt van Azure Storage, voor informatie over het controleren van problemen met de opslag, met inbegrip van beperking, Zie [Monitor, vaststellen en oplossen van Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Als uw cluster bevindt, is Azure Data Lake opslaan (ADLS), is uw beperking waarschijnlijk veroorzaakt door beperkingen van de bandbreedte. Beperking kan in dit geval worden geïdentificeerd door observeren bandbreedteregeling fouten in de logboeken van de taak. Zie voor ADLS, de bandbreedteregeling sectie voor de betreffende service in deze artikelen:

* [Richtlijnen voor het afstemmen van de prestaties van Hive in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Richtlijnen voor het afstemmen van de prestaties van MapReduce in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Richtlijnen voor het afstemmen van de prestaties van Storm in HDInsight en Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over het oplossen van problemen en controleren van clusters:

* [HDInsight-logboekbestanden analyseren](hdinsight-debug-jobs.md)
* [Fouten in apps opsporen met YARN-logboeken](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight heap inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
