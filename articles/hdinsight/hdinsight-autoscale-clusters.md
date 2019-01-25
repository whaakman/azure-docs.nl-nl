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
ms.openlocfilehash: 977466d77bee2d6fe49c0438aa1d9d4489f8eb5f
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888223"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Automatisch schalen van Azure HDInsight-clusters (Preview)

Azure HDInsight-cluster voor automatisch schalen functie het aantal worker-knooppunten automatisch in een cluster omhoog of omlaag wordt geschaald op basis van belasting in een vooraf gedefinieerd bereik. Tijdens het maken van een nieuw HDInsight-cluster, kan een minimum en maximum aantal worker-knooppunten worden ingesteld. Voor automatisch schalen en monitors de resourcevereisten van de analyse laden en het aantal worker-knooppunten schaalt omhoog of omlaag dienovereenkomstig. Er is geen extra kosten voor deze functie.

## <a name="getting-started"></a>Aan de slag

### <a name="create-cluster-with-azure-portal"></a>Cluster maken met Azure portal

> [!Note]
> Automatisch schalen is momenteel alleen ondersteund voor Azure HDInsight Hive, MapReduce en Spark-clusters versie 3.6.

HDInsight-cluster maken stappen met behulp van de Azure-Portal kunnen u vinden op [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).  Automatisch schalen inschakelen tijdens het maken, is een paar afwijkingen van de gebruikelijke installatiestappen vereist.  

1. Selecteer **aangepast (grootte, instellingen en apps)** in plaats van **snelle invoer**.
2. Stap 5 van aangepaste **clustergrootte**, Controleer de **Worker-knooppunt voor automatisch schalen** selectievakje.
3. Voer de gewenste waarden in voor:  
  &#8226;Eerste **nummer van de Worker-knooppunten**.  
  &#8226;**Minimum** aantal worker-knooppunten.  
  &#8226;**Maximale** aantal worker-knooppunten.  

![Optie voor worker-knooppunt voor automatisch schalen inschakelen](./media/hdinsight-autoscale-clusters/usingAutoscale.png)


Het oorspronkelijke aantal worker-knooppunten moet liggen tussen het minimum en maximum, inclusief. Deze waarde bepaalt de aanvankelijke grootte van het cluster wanneer deze wordt gemaakt. Het minimum aantal worker-knooppunten moet groter zijn dan nul zijn.

Nadat u de VM-type voor elk knooppunttype hebt gekozen, kunt u zich om te zien van het bereik van de geschatte kosten voor het hele cluster. U kunt deze instellingen voor elk budget aanpassen.

Uw abonnement heeft een quotum van de capaciteit voor elke regio. Het totale aantal kernen van de hoofdknooppunten in combinatie met het maximum aantal worker-knooppunten kan niet groter zijn dan het quotum van de capaciteit. Dit quotum is echter een limiet zachte; u kunt altijd een ondersteuningsticket om hiermee aan verhoogd eenvoudig te maken.

> [!Note]  
> Als u het totaal aantal core limiet overschrijdt, ontvangt u een foutmelding dat "het maximale aantal knooppunt overschrijdt de beschikbare kerngeheugens gebruikt in deze regio Kies een andere regio of neem contact op met de ondersteuning om het quotum te verhogen."

### <a name="create-cluster-with-an-resource-manager-template"></a>Cluster maken met een Resource Manager-sjabloon

HDInsight-cluster maken stappen met behulp van Resource Manager-sjablonen kunnen u vinden op [Apache Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  Wanneer u een HDInsight-cluster met een Azure Resource Manager-sjabloon maakt, moet u de volgende instellingen in de sectie "computeProfile"-"workernode" toevoegen en dienovereenkomstig te bewerken:

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

### <a name="enable-and-disabling-autoscale-for-a-running-cluster"></a>Automatisch schalen voor een actieve cluster uitschakelen en inschakelen

Automatisch schalen inschakelen voor een actieve cluster wordt niet ondersteund tijdens de private preview. Dit moet worden ingeschakeld tijdens het maken van clusters.

Automatisch schalen uitschakelen of wijzigen van instellingen voor automatisch schalen voor een actieve cluster wordt niet ondersteund in private preview. U moet het cluster verwijderen en maak een nieuw cluster wilt verwijderen of de instellingen wijzigen.

## <a name="monitoring"></a>Bewaking

U kunt de schaal cluster omhoog en omlaag geschiedenis als onderdeel van de cluster-metrische gegevens weergeven. U kunt alle schaalacties lijst gedurende de laatste dag, week of een langere periode.

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

Er wordt berekend dat de nieuwe werkrolknooppunten N nodig zijn om te voldoen aan de huidige vereisten van de CPU en geheugen en uitgeven van een schaal van de aanvraag vervolgens door aan te vragen van de nieuwe werkrolknooppunten N.

### <a name="cluster-scale-down"></a>Cluster omlaag schalen

Wanneer de volgende voorwaarden worden gedetecteerd, wordt een neerschalen aanvraag voert u automatisch schalen:

* Totaal aantal in afwachting van CPU is kleiner dan de totale vrije CPU gedurende meer dan 10 minuten.
* Totaal aantal in afwachting van geheugen is kleiner dan de totale beschikbare geheugen voor meer dan 10 minuten.

Op basis van het aantal uur containers per knooppunt, evenals de huidige vereisten voor CPU en geheugen, verleent automatisch schalen een aanvraag voor het verwijderen van de N-knooppunten, op te geven welke knooppunten zijn mogelijke kandidaten voor verwijdering. Standaard wordt twee knooppunten verwijderd in een cyclus.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over aanbevolen procedures voor het schalen van clusters handmatig in [best practices voor schalen](hdinsight-scaling-best-practices.md)
