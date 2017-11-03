---
title: Cluster capaciteitsplanning in Azure HDInsight | Microsoft Docs
description: Het opgeven van een HDInsight-cluster voor capaciteit en prestaties.
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
ms.date: 09/22/2017
ms.author: maxluk
ms.openlocfilehash: b4bdf3339e585a7b22a1945871f802854020fb94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Capaciteitsplanning voor HDInsight-clusters

Plannen voordat u een HDInsight-cluster implementeert, de capaciteit van het gewenste cluster door het bepalen van de benodigde prestaties en schaal. Deze planning kunt optimaliseren zowel gebruiksvriendelijkheid en kosten. Beslissingen voor een capaciteit van de cluster kunnen niet worden gewijzigd na de implementatie. Als de prestatieparameters wijzigt, kunt u een cluster worden afgeschaft en opnieuw gemaakt zonder verlies van opgeslagen gegevens.

De belangrijke vragen te vragen voor de capaciteitsplanning zijn:

* In welke geografische regio moet u uw cluster implementeren?
* Hoeveel opslagruimte hebt u nodig?
* Welk clustertype moet u implementeren?
* Welke grootte en het type van de virtuele machine (VM) moeten uw clusterknooppunten dan gebruiken?
* Het aantal worker-knooppunten moet hebben voor uw cluster?

## <a name="choose-an-azure-region"></a>Een Azure-regio kiezen

De Azure-regio bepaalt waar uw cluster fysiek is ingericht. Om te beperken de latentie van lees- en schrijfbewerkingen, moet het cluster in de buurt van uw gegevens.

HDInsight is beschikbaar in vele Azure-regio's. De dichtstbijzijnde regio, Zie de *HDInsight Linux* item onder *gegevens en analyse* in [Azure producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Kies de locatie voor de opslag en de grootte

### <a name="location-of-default-storage"></a>Locatie van standaard-opslag

De standaard-opslag, een Azure Storage-account of een Azure Data Lake Store moet zich in dezelfde locatie als uw cluster. Azure-opslag is beschikbaar op alle locaties. Data Lake Store is beschikbaar in bepaalde gebieden - Zie de beschikbaarheid van de huidige Data Lake Store onder *opslag* in [Azure producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Locatie van de bestaande gegevens

Als u al een opslagaccount of de Data Lake Store met uw gegevens en deze opslag gebruiken als uw cluster standaard opslag wilt, moet u uw cluster op die dezelfde locatie implementeren.

### <a name="storage-size"></a>Maximale grootte

Nadat u een HDInsight-cluster dat is geïmplementeerd hebt, kunt u toegang tot andere gegevensarchieven Lake of extra Azure Storage-accounts te koppelen. Alle opslagaccounts moeten zich bevinden op dezelfde locatie als uw cluster. Een Data Lake Store kunnen zich in een andere locatie, hoewel dit leiden enige vertraging worden bijgewerkt gegevens lezen/schrijven tot kan.

Azure-opslag, is enkele [Capaciteitslimieten](../azure-subscription-service-limits.md#storage-limits), terwijl vrijwel onbeperkte Data Lake Store.

Een cluster, hebben toegang tot een combinatie van verschillende opslagaccounts. Typische voorbeelden zijn:

* Wanneer de hoeveelheid gegevens is waarschijnlijk zijn dan de capaciteit van een enkele blob storage-container.
* Wanneer de snelheid van toegang tot de blob-container mogelijk groter is dan de drempelwaarde waar beperking optreedt.
* Als u maken van gegevens wilt, hebt u al geüpload naar een blob-container beschikbaar voor het cluster.
* Als u wilt isoleren van de verschillende onderdelen van de opslag uit veiligheidsoverwegingen of u wilt vereenvoudigen.

Voor een cluster met knooppunten 48 aangeraden 4 tot en met 8 storage-accounts. Hoewel er mogelijk al voldoende totale opslag, biedt elk opslagaccount extra netwerken bandbreedte voor de rekenknooppunten. Wanneer u meerdere opslagaccounts hebt, gebruikt u een willekeurige naam voor elk opslagaccount zonder een voorvoegsel. Het doel van willekeurige naamgeving is de kans op opslag knelpunten (beperking) of gemeenschappelijke-modus storingen alle accounts wordt verminderd. Voor betere prestaties door slechts één container per storage-account te gebruiken.

## <a name="choose-a-cluster-type"></a>Kies een clustertype

Het clustertype bepaalt de werkbelasting van die uw HDInsight-cluster is geconfigureerd om te worden uitgevoerd, zoals Hadoop, Storm, Kafka of Spark. 
<!-- For a detailed description of the available cluster types, see [HDInsight Architecture](hdinsight-architecture.md). -->
Elk clustertype heeft een specifieke implementatietopologie die voldoet aan vereisten voor de grootte en het aantal knooppunten.

## <a name="choose-the-vm-size-and-type"></a>Kies de VM-grootte en het type

Elk clustertype beschikt over een reeks knooppunttypen en elk knooppunttype specifieke opties voor hun VM-grootte en het type heeft.

Om te bepalen de optimale clustergrootte voor uw toepassing, kunt u de benchmark cluster capaciteit en verhoog de grootte, zoals wordt aangegeven. Bijvoorbeeld, kunt u een gesimuleerde werkbelasting, of een *kokospalm query*. Met een gesimuleerde werkbelasting voert u uw verwachte werkbelastingen op clusters van verschillende grootte, de grootte van de geleidelijk te verhogen tot de gewenste prestaties is bereikt. Een kokospalm query kan periodiek worden ingevoegd tussen de andere productie query's om weer te geven of het cluster niet voldoende bronnen heeft.

De VM-grootte en het type wordt bepaald door de CPU-verwerking voeding, RAM-geheugen en netwerklatentie:

* CPU: De VM-grootte bepaalt het aantal kernen. Meer cores, hoe groter de mate van parallelle berekeningen elk knooppunt kunt bereiken. Ook hebben bepaalde typen VM sneller kernen.

* RAM-geheugen: De VM-grootte bepaalt ook de hoeveelheid RAM die beschikbaar zijn in de virtuele machine. Worker-knooppunten hebben voor de werklast die gegevens opslaan in het geheugen voor de verwerking, in plaats van lezen van de schijf, zorg ervoor dat voldoende geheugen aanpassen aan de gegevens.

* Netwerk: Voor de meeste clustertypen, de gegevens die zijn verwerkt door het cluster is niet op de lokale schijf, maar in een service voor externe opslag zoals Data Lake Store of Azure Storage. Houd rekening met de netwerkbandbreedte en de doorvoer tussen de VM-knooppunt en de storage-service. De beschikbare netwerkbandbreedte voor een virtuele machine wordt doorgaans met grotere verhoogt. Zie voor meer informatie [VM-groottes overzicht](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Kies de schaal van het cluster

Schaal van een cluster wordt bepaald door het aantal van de VM-knooppunten. Voor alle clustertypen zijn knooppunttypen waarvoor een specifieke schaal en knooppunttypen die ondersteuning bieden voor scale-out. Een cluster kan bijvoorbeeld vereist precies drie ZooKeeper-knooppunten of twee Head-knooppunten. Worker-knooppunten dat gegevens worden verwerkt in een gedistribueerde manier doen kunnen profiteren van uitbreiden, door aanvullende worker-knooppunten toe te voegen.

Afhankelijk van het clustertype van uw, waardoor het aantal worker-knooppunten voegt extra rekenkundige capaciteit (zoals meer cores), maar mogelijk ook toevoegen aan de totale hoeveelheid geheugen die nodig zijn voor het hele cluster ter ondersteuning van de opslag van gegevens wordt verwerkt in het geheugen. Net als bij de keuze van VM-grootte en het type, is de juiste cluster schaal selecteren doorgaans bereikt langs, met gesimuleerde werkbelastingen of kokospalm query's.

U kunt schalen uit het cluster om te voldoen aan de eisen van piek laden en vervolgens weer omlaag schalen, wanneer die extra knooppunten zijn niet meer nodig.
<!-- - see [Scaling - best practices](hdinsight-scaling-best-practices.md). -->

### <a name="cluster-lifecycle"></a>De levenscyclus van het cluster

Worden in rekening gebracht voor de levensduur van het cluster. Als er alleen op specifieke tijdstippen die u uw cluster nodig en die wordt uitgevoerd, kunt u op aanvraag clusters met behulp van Azure Data Factory.
<!-- [create on-demand clusters using Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md). -->
U kunt ook PowerShell-scripts die in te richten en verwijder uw cluster maken en vervolgens deze scripts met plannen [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!NOTE]
> Wanneer een cluster wordt verwijderd, wordt de standaard Hive-metastore ook verwijderd. Gebruiken om te blijven behouden de metastore voor de volgende cluster opnieuw maken, een externe metagegevens store, zoals Azure Database of Oozie.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Taakfouten cluster isoleren

Soms fouten kunnen optreden vanwege de parallelle uitvoering van meerdere kaart en onderdelen op een cluster met meerdere knooppunten verminderen. Meerdere taken op een cluster met één knooppunt, vouw vervolgens deze benadering van meerdere taken op clusters met meerdere knooppunten gelijktijdig worden uitgevoerd om te helpen het probleem te isoleren, probeert u gedistribueerde testen door gelijktijdige die wordt uitgevoerd. Gebruik voor het maken van een cluster met één knooppunt HDInsight in Azure de *geavanceerde* optie.

U kunt ook een ontwikkelingsomgeving met één knooppunt op uw lokale computer installeren en testen van de oplossing er. Hortonworks biedt een één knooppunt lokale ontwikkelingsomgeving voor Hadoop-oplossingen die is nuttig voor de ontwikkelingsfase, bewijs van het concept, en testen. Zie voor meer informatie [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/).

Als u wilt vaststellen van het probleem op een lokaal cluster met één knooppunt kunt u mislukte taken opnieuw uitvoeren en aanpassen van de invoergegevens of kleinere gegevenssets gebruiken. Hoe u deze taken uitvoeren, is afhankelijk van het platform en het type toepassing.

## <a name="quotas"></a>Quota

Controleer na het vaststellen van uw doel cluster VM-grootte, schaal en type, de huidige quotalimieten voor een capaciteit van uw abonnement. Wanneer u een quotumlimiet bereikt, kunt u mogelijk niet implementeren van nieuwe clusters of bestaande clusters uitbreiden door meer worker-knooppunten toe te voegen. De meest voorkomende quotumlimiet bereikt is de quota voor CPU-kernen die op het abonnement, regio en niveaus van VM-reeks bestaat. Uw abonnement mogelijk bijvoorbeeld een totale limiet van 200 core met een 30 core limiet in uw regio en de limiet voor een 30 kernen op VM-exemplaren. U kunt [contact op met ondersteuning om aan te vragen een verhoging van het quotum](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Echter, er zijn enkele beperkingen vaste quota, bijvoorbeeld één Azure-abonnement beschikken over maximaal 10.000 kernen. Zie voor meer informatie over deze limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits#limits-and-the-azure-resource-manager).

## <a name="next-steps"></a>Volgende stappen

* [Instellen van clusters in HDInsight met Hadoop, Spark en Kafka](hdinsight-hadoop-provision-linux-clusters.md): informatie over het instellen en configureren van clusters in HDInsight met Hadoop, Spark, Kafka, interactieve Hive, HBase, R Server of Storm.
* [Prestaties van het cluster controleren](hdinsight-key-scenarios-to-monitor.md): meer informatie over belangrijke scenario's om te controleren op uw HDInsight-cluster die invloed kan zijn op de capaciteit van uw cluster.
