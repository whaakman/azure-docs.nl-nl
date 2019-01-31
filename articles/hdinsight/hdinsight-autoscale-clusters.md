---
title: Automatisch schalen van Azure HDInsight-clusters (Preview)
description: De functie HDInsight Autoscale gebruiken voor het automatisch schalen van clusters
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: bd1ffcfd915fe9ece683ec88d27f54b3a9214621
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475671"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Automatisch schalen van Azure HDInsight-clusters (Preview)

Azure HDInsight-cluster voor automatisch schalen functie het aantal worker-knooppunten automatisch in een cluster omhoog of omlaag wordt geschaald op basis van belasting in een vooraf gedefinieerd bereik. Tijdens het maken van een nieuw HDInsight-cluster, kan een minimum en maximum aantal worker-knooppunten worden ingesteld. Voor automatisch schalen en monitors de resourcevereisten van de analyse laden en het aantal worker-knooppunten schaalt omhoog of omlaag dienovereenkomstig. Er is geen extra kosten voor deze functie.

## <a name="getting-started"></a>Aan de slag

### <a name="create-a-cluster-with-the-azure-portal"></a>Een cluster maken met de Azure-portal

> [!Note]
> Automatisch schalen is momenteel alleen ondersteund voor Azure HDInsight Hive, MapReduce en Spark-clusters versie 3.6.

Om in te schakelen van de functie voor automatisch schalen, doet u het volgende als onderdeel van het proces voor het maken van normale cluster:

1. Selecteer **aangepast (grootte, instellingen en apps)** in plaats van **snelle invoer**.
2. Op **aangepaste** stap 5 (**clustergrootte**) Controleer de **Worker-knooppunt voor automatisch schalen** selectievakje.
3. Voer de gewenste waarden in voor:  

    * Eerste **nummer van de Worker-knooppunten**.  
    * **Minimale** aantal worker-knooppunten.  
    * **Maximale** aantal worker-knooppunten.  

![Optie voor worker-knooppunt voor automatisch schalen inschakelen](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

Het oorspronkelijke aantal worker-knooppunten moet liggen tussen het minimum en maximum, inclusief. Deze waarde bepaalt de aanvankelijke grootte van het cluster wanneer deze wordt gemaakt. Het minimum aantal worker-knooppunten moet groter zijn dan nul zijn.

Nadat u de VM-type voor elk knooppunttype hebt gekozen, kunt u zich om te zien van het bereik van de geschatte kosten voor het hele cluster. U kunt deze instellingen voor elk budget aanpassen.

Uw abonnement heeft een quotum van de capaciteit voor elke regio. Het totale aantal kernen van de hoofdknooppunten in combinatie met het maximum aantal worker-knooppunten kan niet groter zijn dan het quotum van de capaciteit. Dit quotum is echter een limiet zachte; u kunt altijd een ondersteuningsticket om hiermee aan verhoogd eenvoudig te maken.

> [!Note]  
> Als u het totaal aantal core limiet overschrijdt, ontvangt u een foutmelding dat "het maximale aantal knooppunt overschrijdt de beschikbare kerngeheugens gebruikt in deze regio Kies een andere regio of neem contact op met de ondersteuning om het quotum te verhogen."

Zie voor meer informatie over het maken van het HDInsight-cluster met behulp van de Azure portal, [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Een cluster maken met Resource Manager-sjabloon

Toevoegen voor het maken van een HDInsight-cluster met een Azure Resource Manager-sjabloon, een `autoscale` knooppunt naar de `computeProfile`  >  `workernode` sectie met de eigenschappen `minInstanceCount` en `maxInstanceCount` zoals wordt weergegeven in het json-codefragment hieronder.

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "minInstanceCount": 2,
        "maxInstanceCount": 10
    },
    "hardwareProfile": {
        "vmSize": "Standard_D13_V2"
    },
    "osProfile": {
        "linuxOperatingSystemProfile": {
            "username": "[parameters('sshUserName')]",
            "password": "[parameters('sshPassword')]"
        }
    },
    "virtualNetworkProfile": null,
    "scriptActions": []
}
```

Zie voor meer informatie over het maken van clusters met Resource Manager-sjablonen, [Apache Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>In- of uitschakelen voor automatisch schalen voor een actieve cluster

Automatisch schalen inschakelen voor een actieve cluster wordt niet ondersteund tijdens de private preview. Dit moet worden ingeschakeld tijdens het maken van clusters.

Automatisch schalen uitschakelen of wijzigen van instellingen voor automatisch schalen voor een actieve cluster wordt niet ondersteund in private preview. U moet het cluster verwijderen en maak een nieuw cluster wilt verwijderen of de instellingen wijzigen.

## <a name="monitoring"></a>Bewaking

U kunt de cluster omhoog schalen en schaal omlaag geschiedenis als onderdeel van de cluster-metrische gegevens weergeven. U kunt ook alle vergroten/verkleinen acties weergeven gedurende de laatste dag, week of een langere periode.

## <a name="how-it-works"></a>Hoe werkt het?

### <a name="metrics-monitoring"></a>Metrische gegevens controleren

Voor automatisch schalen het cluster controleert voortdurend en verzamelt de volgende metrische gegevens:

1. **Totaal aantal in behandeling zijnde CPU**: Het totale aantal kernen vereist voor de uitvoering van alle in behandeling containers worden gestart.
2. **Totaal aantal in behandeling zijnde geheugen**: Het totale geheugen (in MB) vereist voor de uitvoering van alle actieve containers worden gestart.
3. **Totaal aantal gratis CPU**: De som van alle niet-gebruikte kernen op de actieve worker-knooppunten.
4. **Totaal vrij geheugen**: De som van niet-gebruikte geheugen (in MB) op de actieve worker-knooppunten.
5. **Gebruikt geheugen per knooppunt**: De belasting van een worker-knooppunt. Een worker-knooppunt waarop 10 GB aan geheugen wordt gebruikt, wordt beschouwd als onder meer belasting dan van een werknemer met 2 GB van het gebruikte geheugen.
6. **Aantal Masters toepassing per knooppunt**: Het nummer van de toepassing Master (uur)-containers die worden uitgevoerd op een worker-knooppunt. Een host 2 AM containers worker-knooppunt wordt beschouwd als belangrijker dan een worker-knooppunt 0 uur-containers hosten.

De bovenstaande metrische gegevens worden elke 60 seconden gecontroleerd. Voor automatisch schalen wordt schaal gezamenlijk en schaal omlaag beslissingen op basis van deze metrische gegevens.

### <a name="cluster-scale-up"></a>Cluster omhoog schalen

Wanneer de volgende voorwaarden worden gedetecteerd, verleent voor automatisch schalen een schaal van aanvraag:

* Totaal aantal in afwachting van CPU is groter dan de totale vrije CPU voor meer dan 1 minuut.
* Totaal aantal in afwachting van geheugen is groter dan de totale beschikbare geheugen voor meer dan 1 minuut.

Er wordt berekend dat een bepaald aantal nieuwe worker-knooppunten nodig zijn om te voldoen aan de huidige CPU en geheugen vereisten en vervolgens geven een schaal van de aanvraag die wordt toegevoegd dat aantal nieuwe worker-knooppunten.

### <a name="cluster-scale-down"></a>Cluster omlaag schalen

Wanneer de volgende voorwaarden worden gedetecteerd, wordt een neerschalen aanvraag voert u automatisch schalen:

* Totaal aantal in afwachting van CPU is kleiner dan de totale vrije CPU gedurende meer dan 10 minuten.
* Totaal aantal in afwachting van geheugen is kleiner dan de totale beschikbare geheugen voor meer dan 10 minuten.

Op basis van het aantal uur containers per knooppunt, evenals de huidige vereisten voor CPU en geheugen, verleent automatisch schalen een aanvraag voor het verwijderen van een bepaald aantal knooppunten, op te geven welke knooppunten zijn mogelijke kandidaten voor verwijdering. Standaard wordt twee knooppunten verwijderd in een cyclus.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over aanbevolen procedures voor het schalen van clusters handmatig in [best practices voor schalen](hdinsight-scaling-best-practices.md)
