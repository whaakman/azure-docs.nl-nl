---
title: OS patches planning voor Linux gebaseerde HDInsight-clusters - Azure configureren | Microsoft Docs
description: Informatie over het configureren van OS patches planning voor Linux gebaseerde HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: af3c5a19ae8e2e606e4b0506f9f6dddb41192e40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="os-patching-for-hdinsight"></a>OS patches voor HDInsight 
Als een beheerde Hadoop-service zorgt HDInsight voor het herstellen van het besturingssysteem van de onderliggende virtuele machines die worden gebruikt door HDInsight-clusters. Vanaf 1 augustus 2016, hebben we de toepassing van patches Gast OS-beleid voor Linux gebaseerde HDInsight-clusters (versie 3.4 of hoger) gewijzigd. Het doel van het nieuwe beleid is aanzienlijk minder opnieuw te worden opgestart vanwege patchen. Patch voor virtuele machines (VM's) blijven het nieuwe beleid op Linux-clusters elke maandag of donderdag begint bij 12: 00 A.M. UTC op een wijze gespreid over de knooppunten in een opgegeven cluster. Een bepaalde virtuele machine wordt echter alleen opnieuw opgestart als gevolg van Gast OS patches maximaal één keer elke 30 dagen. Bovendien de eerste herstart voor een nieuw cluster gebeurt niet eerder zijn dan 30 dagen na het maken van het cluster. Patches worden van kracht nadat de virtuele machines opnieuw worden opgestart.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Het configureren van het besturingssysteem patchen planning voor Linux gebaseerde HDInsight-clusters
De virtuele machines in een HDInsight-cluster moet opnieuw worden opgestart af en toe zodat belangrijke beveiligingspatches kunnen worden geïnstalleerd. Vanaf 1 augustus 2016 krijgen nieuwe Linux gebaseerde HDInsight-clusters (versie 3.4 of hoger) opnieuw opgestart met het volgende schema:

1. Een virtuele machine in het cluster kan alleen opnieuw opstarten patches voor maximaal één keer binnen een periode van 30 dagen.
2. Het opnieuw opstarten gebeurt beginnen bij 12: 00 A.M. UTC.
3. Het proces opnieuw opstarten is gespreid over virtuele machines in het cluster, zodat het cluster nog steeds beschikbaar tijdens het opnieuw opstarten is.
4. De eerste herstart voor een nieuw cluster gebeurt niet eerder zijn dan 30 dagen na de aanmaakdatum cluster.

De actie van het script dat wordt beschreven in dit artikel kunt u de OS patches planning als volgt wijzigen:
1. In- of uitschakelen van automatische opnieuw wordt opgestart
2. Stel de frequentie van opnieuw wordt opgestart (in dagen tussen opnieuw is opgestart)
3. Stelt u de dag van de week wanneer opnieuw opstarten wordt uitgevoerd

> [!NOTE]
> Deze scriptactie werkt alleen met Linux gebaseerde HDInsight-clusters die zijn gemaakt na 1e augustus 2016. Patches worden van kracht wanneer virtuele machines opnieuw worden opgestart. 
>

## <a name="how-to-use-the-script"></a>Het gebruik van het script 

Met dit script vereist wanneer de volgende informatie:
1. Locatie van het script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight gebruikt deze URI te vinden en het script uitvoert op de virtuele machines in het cluster.
  
2. Knooppunt met de clustertypen waarmee het script wordt toegepast op: headnode, workernode, zookeeper. Dit script moet worden toegepast op alle typen van de knooppunten in het cluster. Als deze niet op het knooppunttype toegepast wordt, blijven de virtuele machines voor dat knooppunttype om de vorige toepassing van patches schema te gebruiken.


3.  Parameter: Dit script accepteert drie numerieke parameters:

    | Parameter | Definitie |
    | --- | --- |
    | Automatisch opnieuw wordt opgestart in-of uitschakelen |0 of 1. De waarde 0 wordt automatisch opnieuw wordt opgestart uitgeschakeld terwijl 1 automatisch opnieuw wordt opgestart schakelt. |
    | Frequentie |7 tot 90 (inclusief). Het aantal dagen moet worden gewacht voordat opnieuw wordt opgestart nadat de virtuele machines voor patches waarvoor opnieuw opstarten. |
    | Dag van week |1 tot en met 7 (inclusief). Een waarde van 1 geeft aan het opstarten moet worden uitgevoerd op een maandag en 7 geeft aan dat het voorbeeld van een Sunday.For met parameters van 1 60 2 resulteert in het automatisch opnieuw wordt opgestart elke 60 dagen (maximaal) op dinsdag. |
    | Persistentie |Bij het toepassen van een scriptactie aan een bestaand cluster, kunt u het script als persistent markeren. Persistente scripts worden toegepast wanneer nieuwe workernodes worden toegevoegd aan het cluster via het schalen van bewerkingen. |

> [!NOTE]
> U moet dit script markeren als persistent wanneer toegepast op een bestaand cluster. Een nieuwe knooppunten die zijn gemaakt via vergroten/verkleinen operations Gebruik anders de standaardwaarde patchen planning.
Als u het script als onderdeel van een proces voor het maken van het cluster hebt toegepast, wordt deze automatisch bewaard.
>

## <a name="next-steps"></a>Volgende stappen

Voor specifieke stappen over het gebruik van de scriptactie raadpleegt u de volgende secties in de [Linuz aanpassen op basis van een HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md):

* [Gebruik de scriptactie van een tijdens het maken van het cluster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [De scriptactie toepassen op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
