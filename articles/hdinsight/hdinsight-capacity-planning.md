---
title: Plannen van capaciteit in Azure HDInsight-cluster
description: Klik hier voor meer informatie over het opgeven van een HDInsight-cluster voor capaciteit en prestaties.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/22/2017
ms.author: maxluk
ms.openlocfilehash: 4438cff0dcf5e896f39729d9871d4deb3207b4b8
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107992"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Capaciteitsplanning voor HDInsight-clusters

Plan voor de capaciteit van het gewenste cluster voordat u een HDInsight-cluster implementeert, door te bepalen van de benodigde prestaties en schaalbaarheid. Met deze planning kunt optimaliseren bruikbaarheid en de kosten. Beslissingen voor een capaciteit van de cluster kunnen niet worden gewijzigd na de implementatie. Als de van prestatieparameters wijzigt, kunt u een cluster worden afgeschaft en opnieuw wordt gemaakt zonder verlies van opgeslagen gegevens.

De belangrijkste vragen die voor het plannen van capaciteit zijn:

* In welke geografische regio moet u uw cluster implementeren?
* Hoeveel opslagruimte heb ik nodig?
* Welk clustertype moet u implementeren?
* Welke grootte en hetzelfde type van de virtuele machine (VM) moeten uw clusterknooppunten dan gebruiken?
* Het aantal worker-knooppunten moet hebben voor uw cluster?

## <a name="choose-an-azure-region"></a>Kies een Azure-regio

De Azure-regio bepaalt waar het cluster fysiek is ingericht. Om te beperken de latentie van lees- en schrijfbewerkingen, moet het cluster in de buurt van uw gegevens.

HDInsight is beschikbaar in veel Azure-regio's. De dichtstbijzijnde regio, Zie de *HDInsight Linux* item onder *gegevens en analyses* in [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Opslaglocatie en grootte kiezen

### <a name="location-of-default-storage"></a>Locatie van standaardopslag

De standaardopslag, een Azure Storage-account of een Azure Data Lake Store, moet zich in dezelfde locatie als uw cluster. Azure Storage is beschikbaar op alle locaties. Data Lake Store is beschikbaar in bepaalde regio's: Zie de huidige beschikbaarheid van de Data Lake Store onder *opslag* in [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Locatie van de bestaande gegevens

Als u al een storage-account of een Data Lake Store met uw gegevens en deze opslag gebruiken als standaardopslag van uw cluster wilt, moet u uw cluster op die dezelfde locatie implementeren.

### <a name="storage-size"></a>Opslaggrootte

Nadat u een HDInsight-cluster geïmplementeerd hebt, kunt u aanvullende Azure Storage-accounts koppelen of toegang tot andere Data Lake Stores. Alle opslagaccounts moeten zich bevinden op dezelfde locatie als uw cluster. Een Data Lake Store kunnen zich in een andere locatie, maar dit leiden bepaalde gegevens lezen/schrijven-latentie tot kan.

Azure Storage heeft enkele [Capaciteitslimieten](../azure-subscription-service-limits.md#storage-limits), terwijl vrijwel onbeperkte Data Lake Store.

Een cluster, hebben toegang tot een combinatie van verschillende opslagaccounts. Typische voorbeelden zijn onder meer:

* Wanneer de hoeveelheid gegevens is waarschijnlijk hoger zijn dan de opslagcapaciteit van een enkele blob storage-container.
* Wanneer de snelheid van toegang tot de blob-container mogelijk groter is dan de drempelwaarde waar beperking optreedt.
* Als u wilt om gegevens te maken, hebt u al geüpload naar een blobcontainer beschikbaar voor het cluster.
* Als u wilt voor het isoleren van de verschillende onderdelen van de opslag uit veiligheidsoverwegingen of beheer te vereenvoudigen.

Voor een cluster met 48-knooppunten het beste 4 tot en met 8 storage-accounts. Hoewel er mogelijk al voldoende totale opslag, biedt elk opslagaccount aanvullende netwerken bandbreedte voor de compute-knooppunten. Als u meerdere opslagaccounts hebt, gebruikt u een willekeurige naam voor de storage-account zonder een voorvoegsel. Het doel van een willekeurige naam geven, is de kans op opslag knelpunten (beperking) of gemeenschappelijke-modus fouten verminderen voor alle accounts. Voor betere prestaties, gebruik slechts één container per opslagaccount.

## <a name="choose-a-cluster-type"></a>Kies een clustertype

Het clustertype bepaalt de werkbelasting van die uw HDInsight-cluster is geconfigureerd om uit te voeren, zoals Hadoop, Storm, Kafka en Spark. Zie voor een gedetailleerde beschrijving van de beschikbare typen [Inleiding tot Azure HDInsight](hadoop/apache-hadoop-introduction.md#cluster-types-in-hdinsight). Elk clustertype heeft een specifieke implementatietopologie die voldoet aan vereisten voor de grootte en het aantal knooppunten.

## <a name="choose-the-vm-size-and-type"></a>Kies de VM-grootte en het type

Elk clustertype heeft een set van knooppunttypen en elk knooppunttype specifieke opties voor de VM-grootte en hetzelfde type.

U kunt om te bepalen de optimale clustergrootte voor uw toepassing, benchmark clustercapaciteit en verhoog de grootte, zoals wordt aangegeven. Bijvoorbeeld, kunt u een gesimuleerde werkbelasting of een *canary query*. Met een gesimuleerde werkbelasting voert u uw verwachte workloads op clusters van verschillende grootte, de grootte van de geleidelijk te verhogen tot de gewenste prestaties is bereikt. Canary query kan periodiek worden ingevoegd tussen de andere productie query's om weer te geven of het cluster voldoende resources heeft.

De VM-grootte en het type wordt bepaald door de CPU-verwerking van kracht, RAM-geheugen en de netwerklatentie:

* CPU: De VM-grootte bepaalt het aantal kernen. Elk knooppunt kan maar liefst meer kernen, des te groter de graad van parallelle berekeningen. Bovendien hebben enkele VM-typen sneller kernen.

* RAM: De VM-grootte bepaalt ook de hoeveelheid RAM-geheugen beschikbaar is in de virtuele machine. Voor workloads die gegevens opslaan in het geheugen voor verwerking, in plaats van lezen van de schijf, zorg ervoor dat uw worker-knooppunten hebben onvoldoende geheugen om aan te passen de gegevens.

* Netwerk: Voor de meeste clustertypen, de gegevens die worden verwerkt door het cluster is niet op de lokale schijf, maar in plaats van in een service voor externe opslag, zoals Data Lake Store of Azure Storage. Houd rekening met de netwerkbandbreedte en de doorvoer tussen de VM-knooppunt en de storage-service. De netwerkbandbreedte die beschikbaar zijn voor een virtuele machine wordt doorgaans met grotere verhoogt. Zie voor meer informatie, [VM-grootten overzicht](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Kies de cluster-schaal

De schaal van een cluster wordt bepaald door de hoeveelheid van de VM-knooppunten. Voor alle clustertypen zijn er knooppunttypen waarvoor een specifieke schaal en typen die ondersteuning bieden voor scale-out. Een cluster kan bijvoorbeeld vereisen dat precies drie ZooKeeper-knooppunten of twee hoofdknooppunten. Worker-knooppunten die gegevens in een gedistribueerde manier verwerken kunnen profiteren van het uitschalen, door het toevoegen van extra werkknooppunten.

Afhankelijk van het clustertype, waardoor het aantal worker-knooppunten voegt extra verwerkingscapaciteit (zoals meer kernen), maar kan ook toevoegen aan de totale hoeveelheid geheugen die nodig is voor het hele cluster ter ondersteuning van de opslag in het geheugen van de gegevens die worden verwerkt. Net als bij de keuze van de VM-grootte en hetzelfde type, is de juiste cluster schaal selecteren doorgaans bereikt empirisch, gesimuleerde werkbelasting of canary query's.

U kunt schalen van uw cluster om te voldoen aan de piekvraag laden en vervolgens weer omlaag schalen wanneer deze extra knooppunten niet meer nodig zijn. Zie voor meer informatie, [schaal HDInsight-clusters](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Levenscyclus van cluster

U betaalt voor de levensduur van een cluster. Als er alleen specifieke tijden dat u uw cluster omhoog en die wordt uitgevoerd moet, kunt u [on-demand clusters met Azure Data Factory maken](hdinsight-hadoop-create-linux-clusters-adf.md). U kunt ook PowerShell-scripts die inrichten en verwijderen van uw cluster te maken en plan vervolgens deze scripts met behulp van [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!NOTE]
> Wanneer een cluster wordt verwijderd, wordt de standaard Hive-metastore ook verwijderd. Om te blijven behouden de metastore voor de volgende cluster opnieuw wordt gemaakt, gebruikt u een extern metagegevensarchief zoals Azure-Database of Oozie.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Cluster-taakfouten isoleren

Soms fouten kunnen optreden als gevolg van de parallelle uitvoering van meerdere kaart en onderdelen op een cluster met meerdere knooppunten verminderen. Om te isoleren van het probleem, probeert u gedistribueerde testen door te voeren gelijktijdige vouwt meerdere taken op een cluster met één knooppunt, u deze aanpak meerdere taken gelijktijdig worden uitgevoerd op clusters met meer dan één knooppunt. Gebruik voor het maken van een cluster met één knooppunt HDInsight in Azure de *geavanceerde* optie.

U kunt ook een ontwikkelingsomgeving met één knooppunt op uw lokale computer installeren en testen van de oplossing bevat. Hortonworks biedt een omgeving met één knooppunt lokale ontwikkeling voor Hadoop-gebaseerde oplossingen die is handig voor het eerste ontwikkeling, bewijs van concept, en tests. Zie voor meer informatie, [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/).

Voor het identificeren van het probleem op een lokale cluster met één knooppunt kunt u mislukte taken opnieuw uitvoeren en aanpassen van de ingevoerde gegevens, of gebruik van kleinere gegevenssets. Hoe u deze taken worden uitgevoerd, is afhankelijk van het platform en het type van de toepassing.

## <a name="quotas"></a>Quota

Controleer na het vaststellen van uw doel cluster VM-grootte, de schaal en het type, de huidige quotumlimiet voor een capaciteit van uw abonnement. Wanneer u een limiet bereikt, kunt u mogelijk niet implementeren van nieuwe clusters of bestaande clusters uitschalen door meer worker-knooppunten toe te voegen. De meest voorkomende quotumlimiet bereikt is het quotum voor CPU-kernen die op het abonnement, regio en niveaus van VM-serie bestaat. Uw abonnement mogelijk bijvoorbeeld een totale limiet van 200 core met een limiet van 30 kernen in uw regio, en een limiet van 30 core op VM-instanties. U kunt [contact op met ondersteuning voor het aanvragen van een quotaverhoging](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Echter er gelden enkele beperkingen vaste quota, bijvoorbeeld een enkel Azure-abonnement maximaal 10.000 cores kan hebben. Zie voor meer informatie over deze limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits#limits-and-the-azure-resource-manager).

## <a name="next-steps"></a>Volgende stappen

* [Clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](hdinsight-hadoop-provision-linux-clusters.md): meer informatie over het instellen en configureren van clusters in HDInsight met Hadoop, Spark, Kafka, Interactive Hive, HBase, ML-Services of Storm.
* [Cluster-prestaties bewaken](hdinsight-key-scenarios-to-monitor.md): meer informatie over belangrijke scenario's voor het bewaken van voor uw HDInsight-cluster die invloed kan hebben op de capaciteit van uw cluster.
