---
title: Upgrade uitvoeren van Azure Monitor voor de agent containers (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een upgrade van de Log Analytics-agent die wordt gebruikt door Azure Monitor voor containers uitvoeren.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 08206b9de4fca3b422c5b076d7e0c1c180f82fbd
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184080"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Upgrade uitvoeren van de Azure-Monitor voor de agent containers (Preview)
Azure Monitor voor containers maakt gebruik van een beperkte versie van de Log Analytics-agent voor Linux. Wanneer een nieuwe versie van de agent wordt uitgebracht, wordt de agent automatisch bijgewerkt op uw beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS).  

Als de upgrade van de agent mislukt, wordt in dit artikel wordt beschreven van het proces van de agent handmatig te upgraden. Als u wilt volgen de versies die zijn uitgebracht, Zie [agent aankondigingen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Agent op de bewaakte Kubernetes-cluster upgraden
Het proces voor het bijwerken van de agent bestaat uit twee duidelijk stappen. De eerste stap is om uit te schakelen controleren met Azure Monitor voor containers met behulp van Azure CLI.  Volg de stappen de [uitschakelen bewaking](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) artikel. Met behulp van Azure CLI, kunnen we de agent verwijderen van de knooppunten in het cluster zonder gevolgen voor de oplossing en de bijbehorende gegevens die zijn opgeslagen in de werkruimte. 

>[!NOTE]
>Terwijl u deze activiteit onderhoud uitvoert, de knooppunten in het cluster niet van verzamelde gegevens doorsturen zijn en prestatieweergaven worden geen gegevens weergegeven tussen het moment dat u de agent verwijderen en de nieuwe versie installeert. 
>

Voor het installeren van de nieuwe versie van de agent, volg de stappen de [ingebouwde bewaking](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) artikel met behulp van Azure CLI, om dit proces te voltooien.  

Wanneer u bewaking opnieuw inschakelt, is het duurt ongeveer 15 minuten voordat u de bijgewerkte status metrische gegevens voor het cluster kunt weergeven. Als u wilt controleren of de agent is bijgewerkt, moet u de opdracht uitvoeren: `kubectl logs omsagent-484hw --namespace=kube-system`

De status is vergelijkbaar met het volgende voorbeeld, waarbij de waarde voor *omi* en *omsagent* moet overeenkomen met de meest recente versie die is opgegeven in de [releasegeschiedenis van agent](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt tijdens het bijwerken van de agent, raadpleegt u de [problemen oplossen met](container-insights-troubleshoot.md) voor ondersteuning.