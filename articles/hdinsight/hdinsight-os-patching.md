---
title: Configureren van de OS-patches planning voor clusters in HDInsight op basis van Linux - Azure
description: Informatie over het configureren van OS-patches plannen voor Linux gebaseerde HDInsight-clusters.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: efe74618b269000749f7ba6c24d35903e540dcfb
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657059"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>De OS-patches plannen voor Linux gebaseerde HDInsight-clusters configureren 

> [!IMPORTANT]
> Ubuntu-installatiekopieën beschikbaar voor nieuwe Azure HDInsight-cluster maken binnen drie maanden wordt gepubliceerd. Vanaf januari 2019, actieve clusters worden niet automatisch gevuld. Klanten moeten scriptacties of andere methoden gebruiken voor het vullen van een actief cluster. Nieuwe clusters hebben altijd de meest recente updates, met inbegrip van de meest recente beveiligingspatches.

In sommige gevallen moet u virtuele machines (VM's) opnieuw opstarten in een HDInsight-cluster voor het installeren van belangrijke beveiligingspatches.

Met behulp van de scriptacties die in dit artikel wordt beschreven, kunt u de OS-patches schema als volgt wijzigen:

1. Installeer alle updates of alleen kernel + beveiligingsupdates of kernelupdates worden geïnstalleerd.
2. Een direct opnieuw doen of een opnieuw opstarten van de virtuele machine te plannen.

> [!NOTE]  
> De scriptacties die worden beschreven in dit artikel werkt alleen met HDInsight op basis van Linux-clusters die zijn gemaakt na 1 augustus 2016. Patches gelden alleen na het opnieuw opstarten van virtuele machines.
> Scriptacties niet automatisch updates voor alle toekomstige update cycli van toepassing. Voer de scripts telkens wanneer nieuwe updates moeten worden toegepast om de updates installeren en start de virtuele machine opnieuw op.

## <a name="add-information-to-the-script"></a>Voeg informatie toe aan het script

Met behulp van een script vereist de volgende gegevens:

- De installatie-updates-schema-opnieuw wordt opgestart scriptlocatie: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster. Met dit script biedt opties voor het installeren van updates en opnieuw starten van de virtuele machine.
  
- De schema-opnieuw wordt opgestart scriptlocatie: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster. Met dit script wordt de VM opnieuw opgestart.
  
- Knooppunt met de clustertypen waarmee het script wordt toegepast op zijn hoofdknooppunt, workernode en zookeeper. Het script van toepassing op alle typen in het cluster. Als het script wordt niet op een knooppunttype toegepast, wordt niet de virtuele machines voor dit knooppunttype worden bijgewerkt of opnieuw opgestart.

- Het script install-updates-schema-opnieuw wordt opgestart accepteert twee numerieke parameters:

    | Parameter | Definitie |
    | --- | --- |
    | Kernel-updates installeren / installeert alle updates/installatie kernel + security-updates worden alleen|0, 1 of 2. De waarde 0 wordt alleen kernel-updates geïnstalleerd. Een waarde van 1 installeert alle updates, en enige kernel 2 installeert en beveiligingsupdates. Als er geen parameter is opgegeven, wordt de standaardwaarde is 0. |
    | Er is geen opnieuw opstarten/Enable schema opnieuw opstarten/Enable onmiddellijk opnieuw opstarten |0, 1 of 2. De waarde 0 wordt opnieuw opstarten uitgeschakeld. Een waarde van 1 schakelt schema opnieuw opstarten en 2 kunnen onmiddellijk opnieuw opstarten. Als er geen parameter is opgegeven, wordt de standaardwaarde is 0. De gebruiker moet invoerparameter 1 voor het invoeren van de parameter 2 te wijzigen. |
   
 - Het schema-opnieuw wordt opgestart script accepteert één numerieke parameter:

    | Parameter | Definitie |
    | --- | --- |
    | Schema opnieuw opstarten/Enable onmiddellijk opnieuw opstarten inschakelen |1 of 2. Een waarde van 1 schakelt schema opnieuw opstarten (gepland in 12 tot 24 uur). De waarde van 2 kunnen onmiddellijk opnieuw opstarten (in vijf minuten). Als er geen parameter is opgegeven, wordt de standaardwaarde is 1. |  

> [!NOTE]
> U moet een script markeren als behouden nadat u deze aan een bestaand cluster toepassen. Nieuwe knooppunten die zijn gemaakt via vergroten / verkleinen Gebruik anders de standaardwaarde toepassen van patches planning. Als u het script als onderdeel van het proces voor het maken van cluster toepast, heeft deze automatisch opgeslagen.


## <a name="next-steps"></a>Volgende stappen

Voor specifieke stappen over het gebruik van scriptacties, Zie de volgende secties in [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md):

* [Een scriptactie tijdens het maken van clusters gebruiken](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Een scriptactie toepassen op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
