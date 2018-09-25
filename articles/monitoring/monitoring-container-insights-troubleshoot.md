---
title: Over het oplossen van Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt problemen op te lossen met Azure Monitor voor containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: de7ae5788224b83105e4dc9a24aea35c8b841c88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986724"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Oplossen van problemen met Azure Monitor voor containers

Wanneer u de bewaking van uw cluster Azure Kubernetes Service (AKS) met Azure Monitor voor containers configureert, kunt u een probleem te voorkomen dat het verzamelen van gegevens of rapportage over de status kunt tegenkomen. Dit artikel worden enkele veelvoorkomende problemen en stappen voor probleemoplossing.

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Azure Monitor voor containers zijn ingeschakeld, maar niet alle informatie rapporteren
Als Azure-Monitor voor containers met succes is ingeschakeld en geconfigureerd, maar u kunt geen statusinformatie weergeven of er zijn geen resultaten zijn geretourneerd door een query voor Log Analytics, kunt u het probleem onderzoeken door de volgende stappen: 

1. Controleer de status van de agent met de opdracht: 

    `kubectl get ds omsagent --namespace=kube-system`

    De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Controleer de implementatiestatus met agentversie *06072018* of met behulp van de opdracht:

    `kubectl get deployment omsagent-rs -n=kube-system`

    De uitvoer moet eruitzien zoals in het volgende voorbeeld, waarin wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Controleer de status van de schil om te controleren of deze wordt uitgevoerd met de opdracht: `kubectl get pods --namespace=kube-system`

    De uitvoer is vergelijkbaar met het volgende voorbeeld met de status van *met* voor de omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Raadpleeg de logboeken van de agent. Wanneer de agent in containers wordt geïmplementeerd, wordt een snelle controle uitgevoerd door het OMI-opdrachten uitvoeren en de versie van de agent en de provider weergeven. 

5. Om te controleren dat de agent opnieuw toegevoegd is, moet u de opdracht uitvoeren: `kubectl logs omsagent-484hw --namespace=kube-system`

    De status moet lijken op het volgende voorbeeld:

    ```
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
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Volgende stappen
Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](monitoring-container-insights-analyze.md).