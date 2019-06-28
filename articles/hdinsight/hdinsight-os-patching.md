---
title: OS-patches planning voor clusters in HDInsight op basis van Linux - Azure configureren
description: Informatie over het configureren van OS-patches plannen voor Linux gebaseerde HDInsight-clusters.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 3fad8869a31688e9e2413abb350eccf1f871f7dd
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330706"
---
# <a name="os-patching-for-hdinsight"></a>OS-patches voor HDInsight 

> [!IMPORTANT]
> Ubuntu-installatiekopieën beschikbaar voor nieuwe HDInsight-cluster maken binnen drie maanden wordt gepubliceerd. Vanaf januari 2019, actieve clusters zijn **niet** automatisch gevuld. Klanten moeten scriptacties of andere methoden gebruiken voor het vullen van een actief cluster. Nieuwe clusters hebben altijd de meest recente updates, met inbegrip van de meest recente beveiligingspatches.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Het configureren van de OS-patches plannen voor Linux gebaseerde HDInsight-clusters
De virtuele machines in een HDInsight-cluster moet opnieuw worden opgestart af en toe zodat belangrijke beveiligingspatches kunnen worden geïnstalleerd. 

Met behulp van de scriptacties die in dit artikel wordt beschreven, kunt u de OS-patches schema als volgt wijzigen:
1. Installeer alle updates of installatie kernel + security-updates worden alleen of kernel-updates installeren.
2. Onmiddellijk opnieuw opstarten of het schema op de virtuele machine opnieuw worden opgestart.

> [!NOTE]  
> Deze scriptacties werkt alleen met HDInsight op basis van Linux-clusters die zijn gemaakt na 1 augustus 2016. Patches worden van kracht wanneer virtuele machines opnieuw zijn opgestart. Deze scripts worden updates voor alle toekomstige update cycli niet automatisch toegepast. Voer de scripts die elke wanneer er nieuwe updates worden toegepast moeten om de updates installeren en opnieuw opstarten van de virtuele machine.

## <a name="how-to-use-the-script"></a>Het gebruik van het script 

Met dit script vereist de volgende gegevens:
1. De installatie-updates-schema-opnieuw wordt opgestart scriptlocatie: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster. Met dit script biedt opties voor het installeren van updates en opnieuw opstarten van de virtuele machine.
  
2. De schema-opnieuw wordt opgestart scriptlocatie: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster. Met dit script opnieuw opgestart voor de virtuele machine.
  
3. Knooppunt met de clustertypen waarmee het script wordt toegepast op: het hoofdknooppunt, workernode, zookeeper. Met dit script moet worden toegepast op alle typen in het cluster. Als deze niet op een knooppunttype toegepast wordt, zal klikt u vervolgens de virtuele machines voor dat knooppunttype niet worden bijgewerkt of opnieuw opgestart.

4. Parameter: Het script install-updates-schema-opnieuw wordt opgestart accepteert twee numerieke parameters:

    | Parameter | Definitie |
    | --- | --- |
    | Kernel-updates installeren / installeert alle updates/installatie kernel + security-updates worden alleen |0 of 1 of 2. Een waarde van 0 wordt alleen kernel-updates bij 1 installeert alle updates, en 2 installeert kernel + security updates worden alleen geïnstalleerd. Als er geen parameter is opgegeven, wordt de standaardwaarde is 0. |
    | Er is geen opnieuw opstarten/Enable schema opnieuw opstarten/Enable onmiddellijk opnieuw opstarten |0 of 1 of 2. Een waarde van 0 schakelt opnieuw opstarten, terwijl 1 schakelt u schema opnieuw opstarten en 2 onmiddellijk opnieuw opstarten kunnen. Als er geen parameter is opgegeven, wordt de standaardwaarde is 0. Parameter 1 voor het invoeren van de parameter 2 moet gebruikersinvoer. |
   
 5. Parameter: Het schema-opnieuw wordt opgestart script accepteert één numerieke parameter:

    | Parameter | Definitie |
    | --- | --- |
    | Schema opnieuw opstarten/Enable onmiddellijk opnieuw opstarten inschakelen |1 of 2. Een waarde van 1 schakelt schema opnieuw opstarten (opnieuw opstarten is gepland in de komende 12 tot 24 uur) terwijl 2 kunnen onmiddellijk opnieuw (in vijf minuten opstarten). Als er geen parameter is opgegeven, wordt de standaardwaarde is 1. |  

> [!NOTE] 
> U kunt het script moet markeren als behouden bij het toepassen van aan een bestaand cluster. Nieuwe knooppunten die zijn gemaakt via vergroten / verkleinen Gebruik anders de standaardwaarde toepassen van patches planning.  Als u het script als onderdeel van het proces voor het maken van cluster toepast, worden deze automatisch opgeslagen.


## <a name="next-steps"></a>Volgende stappen

Voor specifieke stappen over het gebruik van de scriptactie, Zie de volgende secties in [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md):

* [Een scriptactie tijdens het maken van clusters gebruiken](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Een scriptactie toepassen op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
