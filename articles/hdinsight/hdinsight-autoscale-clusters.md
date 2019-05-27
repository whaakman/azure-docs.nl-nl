---
title: Automatisch schalen van Azure HDInsight-clusters (preview)
description: De functie HDInsight Autoscale gebruiken voor het automatisch schalen van clusters
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: hrasheed
ms.openlocfilehash: 6ec981164de0ff61b0e83d54255d046a1418ed96
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66000109"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Automatisch schalen van Azure HDInsight-clusters (preview)

> [!Important]
> De functie voor automatisch schalen werkt alleen voor Spark, Hive en MapReduce-clusters die zijn gemaakt na 8 mei-2019. 

Functie voor automatisch schalen van Azure HDInsight-cluster automatisch het aantal worker-knooppunten in een cluster omhoog of omlaag schaalt. Andere typen knooppunten in het cluster kunnen niet op dit moment worden geschaald.  Tijdens het maken van een nieuw HDInsight-cluster, kan een minimum en maximum aantal worker-knooppunten worden ingesteld. Automatisch schalen vervolgens de resourcevereisten van de belasting analytics bewaakt en wordt het aantal worker-knooppunten omhoog of omlaag wordt geschaald. Er is geen extra kosten voor deze functie.

## <a name="cluster-compatibility"></a>Cluster-compatibiliteit

De volgende tabel beschrijft de clustertypen en versies die compatibel met de functie voor automatisch schalen zijn.

| Versie | Spark | Hive | LLAP | Hbase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 zonder ESP | Ja | Ja | Nee | Nee | Nee | Nee | Nee |
| HDInsight 4.0 zonder ESP | Ja | Ja | Nee | Nee | Nee | Nee | Nee |
| HDInsight 3.6 met ESP | Ja | Ja | Nee | Nee | Nee | Nee | Nee |
| HDInsight 3.6 met ESP | Ja | Ja | Nee | Nee | Nee | Nee | Nee |

## <a name="how-it-works"></a>Hoe het werkt

U kunt kiezen op basis van een load schalen of schalen op basis van een schema voor uw HDInsight-cluster. Schalen op basis van een Load, wijzigt het aantal knooppunten in uw cluster, binnen een bereik dat u instelt, zorg ervoor dat de optimale CPU-gebruik en de lopende kosten minimaliseren.

Op basis van een planning vergroten/verkleinen het aantal knooppunten in uw cluster op basis van voorwaarden die van op specifieke tijdstippen kracht. Deze voorwaarden schalen het cluster om een gewenste aantal knooppunten te.

### <a name="metrics-monitoring"></a>Metrische gegevens controleren

Voor automatisch schalen het cluster controleert voortdurend en verzamelt de volgende metrische gegevens:

* **Totaal aantal in behandeling zijnde CPU**: Het totale aantal kernen vereist voor de uitvoering van alle in behandeling containers worden gestart.
* **Totaal aantal in behandeling zijnde geheugen**: Het totale geheugen (in MB) vereist voor de uitvoering van alle actieve containers worden gestart.
* **Totaal aantal gratis CPU**: De som van alle niet-gebruikte kernen op de actieve worker-knooppunten.
* **Totaal vrij geheugen**: De som van niet-gebruikte geheugen (in MB) op de actieve worker-knooppunten.
* **Gebruikt geheugen per knooppunt**: De belasting van een worker-knooppunt. Een worker-knooppunt waarop 10 GB aan geheugen wordt gebruikt, wordt beschouwd als onder meer belasting dan van een werknemer met 2 GB van het gebruikte geheugen.
* **Aantal Masters toepassing per knooppunt**: Het nummer van de toepassing Master (uur)-containers die worden uitgevoerd op een worker-knooppunt. Een worker-knooppunt dat als host fungeert voor containers van twee uur, wordt beschouwd als belangrijker dan een worker-knooppunt dat als host voor de containers van nul uur fungeert.

De bovenstaande metrische gegevens worden elke 60 seconden gecontroleerd. Automatisch schalen wordt omhoog en omlaag schalen beslissingen op basis van deze metrische gegevens.

### <a name="load-based-cluster-scale-up"></a>Op basis van een Load-cluster omhoog

Wanneer de volgende voorwaarden worden gedetecteerd, wordt voor automatisch schalen een scale-up-aanvraag uitgeven:

* Totaal aantal in afwachting van CPU is groter dan de totale vrije CPU voor meer dan 3 minuten.
* Totaal aantal in afwachting van geheugen is groter dan de totale beschikbare geheugen voor meer dan 3 minuten.

De HDInsight-service wordt berekend hoeveel nieuwe worker-knooppunten nodig zijn om te voldoen aan de huidige CPU en geheugen nodig en vervolgens een scale-up-aanvraag voor het toevoegen van het vereiste aantal knooppunten.

### <a name="load-based-cluster-scale-down"></a>Cluster op basis van een Load omlaag schalen

Wanneer de volgende voorwaarden worden gedetecteerd, wordt een aanvraag voor omlaag schalen voert u automatisch schalen:

* Totaal aantal in afwachting van CPU is kleiner dan de totale vrije CPU gedurende meer dan 10 minuten.
* Totaal aantal in afwachting van geheugen is kleiner dan de totale beschikbare geheugen voor meer dan 10 minuten.

Voor automatisch schalen problemen op basis van het aantal uur containers per knooppunt en de huidige CPU en geheugen, met een aanvraag voor het verwijderen van een bepaald aantal knooppunten. De service detecteert die knooppunten kandidaten voor verwijderen op basis van het uitvoeren van de huidige taak zijn. De bewerking omlaag schalen decommissions eerst de knooppunten en worden deze verwijderd uit het cluster.

## <a name="get-started"></a>Aan de slag

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Een cluster maken met automatisch schalen op basis van een load

Zodat de functie voor automatisch schalen met schalen op basis van een load voert u de volgende stappen uit als onderdeel van het proces voor het maken van normale cluster:

1. Selecteer **aangepast (grootte, instellingen en apps)** in plaats van **snelle invoer**.
1. Op **aangepaste** stap 5 (**clustergrootte**), Controleer de **Worker-knooppunt voor automatisch schalen** selectievakje.
1. Selecteer de optie **op basis van een Load** onder **voor automatisch schalen type**.
1. Voer de gewenste waarden in voor de volgende eigenschappen:  

    * Eerste **nummer van de Worker-knooppunten**.  
    * **Minimale** aantal worker-knooppunten.  
    * **Maximale** aantal worker-knooppunten.  

    ![Worker-knooppunt voor automatisch schalen op basis van een load optie inschakelen](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

Het oorspronkelijke aantal worker-knooppunten moet liggen tussen het minimum en maximum, inclusief. Deze waarde bepaalt de aanvankelijke grootte van het cluster wanneer deze wordt gemaakt. Het minimum aantal worker-knooppunten moet groter zijn dan nul zijn.

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Een cluster met automatisch schalen op basis van een planning maken

Als u wilt de functie voor automatisch schalen met schalen op basis van een planning inschakelt, voert u de volgende stappen uit als onderdeel van het proces voor het maken van normale cluster:

1. Selecteer **aangepast (grootte, instellingen en apps)** in plaats van **snelle invoer**.
1. Op **aangepaste** stap 5 (**clustergrootte**), Controleer de **Worker-knooppunt voor automatisch schalen** selectievakje.
1. Voer de **nummer van de Worker-knooppunten**, die bepaalt de limiet voor het opschalen van het cluster.
1. Selecteer de optie **op basis van een planning** onder **voor automatisch schalen type**.
1. Klik op **configureren** openen de **configuratie voor automatisch schalen** venster.
1. Selecteer uw tijdzone en klik vervolgens op **+ voorwaarde toevoegen**
1. Selecteer de dagen van de week waarop de nieuwe voorwaarde moet worden toegepast.
1. De tijd die de voorwaarde rekening houden met moet kracht en het aantal knooppunten dat het cluster moet worden uitgebreid om te bewerken.
1. Zo nodig meer voorwaarden toevoegen.

    ![Voor automatisch schalen op basis van een planning inschakelt voor worker-knooppunt](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

Het aantal knooppunten moet tussen 1 en het aantal worker-knooppunten die u hebt ingevoerd voordat het voorwaarden toevoegen.

### <a name="final-creation-steps"></a>Stappen voor het maken van laatste

Voor het schalen van zowel op basis van een load en op basis van een planning, selecteert u het VM-type voor worker-knooppunten door te klikken op **de grootte van de Worker-knooppunt** en **knooppuntgrootte Head**. Nadat u de VM-type voor elk knooppunttype hebt gekozen, ziet u het bereik van de geschatte kosten voor het hele cluster. De VM-typen aanpassen aan uw budget passen.

![Voor automatisch schalen op basis van een planning inschakelt voor worker-knooppunt](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-node-size-selection.png)

Uw abonnement heeft een quotum van de capaciteit voor elke regio. Het totale aantal kernen van de hoofdknooppunten in combinatie met het maximum aantal worker-knooppunten kan niet groter zijn dan het quotum van de capaciteit. Dit quotum is echter een limiet zachte; u kunt altijd een ondersteuningsticket om hiermee aan verhoogd eenvoudig te maken.

> [!Note]  
> Als u het totaal aantal core limiet overschrijdt, ontvangt u een foutmelding dat "het maximale aantal knooppunt overschrijdt de beschikbare kerngeheugens gebruikt in deze regio Kies een andere regio of neem contact op met de ondersteuning om het quotum te verhogen."

Zie voor meer informatie over het maken van het HDInsight-cluster met behulp van de Azure portal, [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Een cluster maken met Resource Manager-sjabloon

#### <a name="load-based-autoscaling"></a>Automatisch schalen op basis van een Load

Kunt u een HDInsight-cluster met automatisch schalen op basis van een load een Azure Resource Manager-sjabloon door toe te voegen een `autoscale` knooppunt naar de `computeProfile`  >  `workernode` sectie met de eigenschappen `minInstanceCount` en `maxInstanceCount` als in het json-codefragment hieronder wordt weergegeven.

```json
{
  "name": "workernode",
  "targetInstanceCount": 4,
  "autoscale": {
      "capacity": {
          "minInstanceCount": 2,
          "maxInstanceCount": 10
      }
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

#### <a name="schedule-based-autoscaling"></a>Automatisch schalen op basis van een planning

Kunt u een HDInsight-cluster met automatisch schalen op basis van een schema voor een Azure Resource Manager-sjabloon door toe te voegen een `autoscale` knooppunt naar de `computeProfile`  >  `workernode` sectie. De `autoscale` knooppunt bevat een `recurrence` waarvoor een `timezone` en `schedule` waarin wordt beschreven wanneer de wijziging plaatsvinden.

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        },
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4,
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>In- of uitschakelen voor automatisch schalen voor een actieve cluster

Als u wilt inschakelen voor automatisch schalen op een actieve cluster, selecteer **clustergrootte** onder **instellingen**. Klik vervolgens op **automatisch schalen inschakelen**. Selecteer het type voor automatisch schalen die u wilt gebruiken en voer de opties voor het schalen van belasting of planning gebaseerd. Klik tot slot, **opslaan**.

![Voor automatisch schalen op basis van een planning inschakelt voor worker-knooppunt](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-enable-running-cluster.png)

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Kiezen op basis van een load of op basis van een planning schalen

Houd rekening met de volgende factoren voordat u een beslissing over welke modus om te kiezen:

* Afwijking laden: heeft de belasting van het cluster een consistente patroon volgen op specifieke tijdstippen, op bepaalde dagen. Als dat niet het geval is, is een betere optie laden op basis van planning.
* SLA-vereisten: Schalen voor automatisch schalen is reactieve in plaats van voorspellende. Worden er een voldoende vertraging tussen wanneer de belasting wordt gestart om te vergroten en wanneer het cluster moet worden op de doelgrootte? Als er strenge vereisten voor de SLA en de belasting een vaste bekende patroon is, is 'schedule op basis van' een betere optie.

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Houd rekening met de latentie van de schaal omhoog of omlaag operations schalen

Het kan 10 tot 20 minuten duren voordat een vergroten/verkleinen bewerking is voltooid. Bij het instellen van een aangepaste planning, plan voor deze vertraging. Bijvoorbeeld, als u de clustergrootte moet 20 om 9:00 uur, de planningstrigger in een eerdere tijd bijvoorbeeld 8:30:00 uur zo instellen dat het schalen van 9:00 uur is voltooid.

### <a name="preparation-for-scaling-down"></a>Voorbereiding voor omlaag schalen

Tijdens het omlaag schalen van cluster, automatisch schalen wordt buiten gebruik stellen de knooppunten om te voldoen aan de doelgrootte. Als er taken op deze knooppunten, wacht automatisch schalen totdat de taken zijn voltooid. Omdat elk werkrolknooppunt ook een rol in HDFS dient, wordt de tijdelijke gegevens worden verplaatst naar de resterende knooppunten. Daarom moet u controleren of er voldoende ruimte is op de resterende knooppunten voor het hosten van de tijdelijke gegevens. 

Taken die worden uitgevoerd blijft worden uitgevoerd en voltooid. De in behandeling zijnde taken wachten totdat de gebruikelijke manier met minder beschikbaar worker-knooppunten worden gepland.

## <a name="monitoring"></a>Bewaking

### <a name="cluster-status"></a>Clusterstatus

De status van het cluster die worden vermeld in de Azure-portal kunt u automatisch schalen activiteiten controleren.

![Worker-knooppunt voor automatisch schalen op basis van een load optie inschakelen](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Alle van de cluster-statusberichten die u kunt tegenkomen worden beschreven in de onderstaande lijst.

| Clusterstatus | Uitleg |
|---|---|
| Actief | Het cluster wordt normaal uitgevoerd. Alle vorige voor automatisch schalen activiteiten zijn voltooid. |
| Bijwerken  | De clusterconfiguratie voor automatisch schalen wordt bijgewerkt.  |
| HDInsight-configuratie  | Een cluster omhoog of bewerking omlaag schalen wordt uitgevoerd.  |
| Fout bij bijwerken  | HDInsight er zijn problemen opgetreden tijdens het bijwerken van de configuratie voor automatisch schalen. Klanten kunnen kiezen voor probeert u de update of automatisch schalen uitschakelen.  |
| Fout  | Er is iets mis met het cluster en kan niet worden gebruikt. Dit cluster verwijderen en een nieuw wachtwoord maken.  |

Als u wilt weergeven van het huidige aantal knooppunten in het cluster, gaat u naar de **clustergrootte** grafiek op het **overzicht** pagina voor uw cluster, of klik op **clustergrootte** onder  **Instellingen voor**.

### <a name="operation-history"></a>Geschiedenis van bewerking

U kunt de geschiedenis van cluster omhoog en omlaag schalen als onderdeel van de cluster-metrische gegevens weergeven. U kunt ook alle vergroten/verkleinen acties gedurende de afgelopen dag, week of andere periode weergeven.

Selecteer **metrische gegevens** onder **bewaking**. Klik vervolgens op **metrische waarde toevoegen** en **aantal actieve werknemers** uit de **Metric** vervolgkeuzelijst. Klik op de knop in de rechterbovenhoek het tijdsbereik wijzigen.

![Voor automatisch schalen op basis van een planning inschakelt voor worker-knooppunt](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over aanbevolen procedures voor het schalen van clusters handmatig in [best practices voor schalen](hdinsight-scaling-best-practices.md)
