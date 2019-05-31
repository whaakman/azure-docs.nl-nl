---
title: OS-patches planning voor clusters in HDInsight op basis van Linux - Azure configureren
description: Informatie over het configureren van OS-patches plannen voor Linux gebaseerde HDInsight-clusters.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: cfbd68e66730fc338130bc16849fe0b2f4abd6be
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244413"
---
# <a name="os-patching-for-hdinsight"></a>OS-patches voor HDInsight 

> [!IMPORTANT]
> Ubuntu-installatiekopieën beschikbaar voor nieuwe HDInsight-cluster maken binnen drie maanden van het worden gepubliceerd. Vanaf januari 2019, actieve clusters zijn **niet** automatisch gevuld. Klanten moeten scriptacties of andere methoden gebruiken voor het vullen van een actief cluster. Nieuwe clusters hebben altijd de meest recente updates, met inbegrip van de meest recente beveiligingspatches.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Het configureren van de OS-patches plannen voor Linux gebaseerde HDInsight-clusters
De virtuele machines in een HDInsight-cluster moet opnieuw worden opgestart af en toe zodat belangrijke beveiligingspatches kunnen worden geïnstalleerd. 

Met behulp van de scriptactie die wordt beschreven in dit artikel, kunt u de OS-patches schema als volgt wijzigen:
1. Volledige updates van het besturingssysteem te installeren of alleen beveiligingsupdates installeren
2. De virtuele machine opnieuw opstarten

> [!NOTE]  
> Deze scriptactie werkt alleen met HDInsight op basis van Linux-clusters die zijn gemaakt na 1 augustus 2016. Patches worden van kracht wanneer virtuele machines opnieuw zijn opgestart. Met dit script worden de updates voor alle toekomstige update cycli niet automatisch toegepast. Voer het script dat elke wanneer er nieuwe updates worden toegepast moeten om de updates installeren en opnieuw opstarten van de virtuele machine.

## <a name="how-to-use-the-script"></a>Het gebruik van het script 

Met dit script vereist wanneer de volgende gegevens:
1. Locatie van het script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/os-patching-reboot-config.sh.  HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster.
  
2. Knooppunt met de clustertypen waarmee het script wordt toegepast op: het hoofdknooppunt, workernode, zookeeper. Met dit script moet worden toegepast op alle typen in het cluster. Als deze niet op een knooppunttype toegepast wordt, wordt klikt u vervolgens de virtuele machines voor dat knooppunttype niet bijgewerkt.


3.  Parameter: Met dit script accepteert één numerieke parameter:

    | Parameter | Definitie |
    | --- | --- |
    | Volledige OS-updates/installatie alleen beveiligingsupdates installeren |0 of 1. De waarde 0 wordt beveiligingsupdates geïnstalleerd alleen vast als 1 volledige updates van het besturingssysteem wordt geïnstalleerd. Als er geen parameter is opgegeven op dat de standaardwaarde is 0. |

> [!NOTE]  
> U kunt dit script moet markeren als behouden bij het toepassen van aan een bestaand cluster. Nieuwe knooppunten die zijn gemaakt via vergroten / verkleinen Gebruik anders de standaardwaarde toepassen van patches planning.  Als u het script als onderdeel van het proces voor het maken van cluster toepast, worden deze automatisch opgeslagen.


## <a name="next-steps"></a>Volgende stappen

Voor specifieke stappen over het gebruik van de scriptactie, Zie de volgende secties in het [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md):

* [Een scriptactie tijdens het maken van clusters gebruiken](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Een scriptactie toepassen op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
