---
title: Over het oplossen van Azure Monitor voor containers (Preview) | Microsoft Docs
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
ms.date: 10/19/2018
ms.author: magoedte
ms.openlocfilehash: f5b08efda80eefad736386cd74ec1d40f05a6f3a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994826"
---
# <a name="troubleshooting-azure-monitor-for-containers-preview"></a>Oplossen van problemen met Azure Monitor voor containers (Preview)

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

## <a name="error-messages"></a>Foutberichten

De onderstaande tabel bevat een overzicht van de bekende problemen die optreden kunnen tijdens het gebruik van Azure Monitor voor containers.

| Foutberichten  | Bewerking |  
| ---- | --- |  
| Foutbericht `No data for selected filters`  | Het duurt enige tijd tot stand brengen van bewaking gegevensstroom voor nieuwe clusters. Wacht ten minste 10 tot 15 minuten voor de gegevens worden weergegeven voor uw cluster. |   
| Foutbericht `Error retrieving data` | Terwijl Azure Kubenetes Service-cluster voor basisstatus en -prestaties instelt, een verbinding tot stand is gebracht tussen het cluster en Azure Log Analytics-werkruimte. Een Log Analytics-werkruimte wordt gebruikt voor het opslaan van alle bewakingsgegevens voor uw cluster. Deze fout kan optreden wanneer uw Log Analytics-werkruimte is verwijderd of verloren gaan. Controleer of uw werkruimte beschikbaar aan de hand is [toegang beheren](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json#workspace-information). Als de werkruimte ontbreekt, moet u voor de re-onboarding uw cluster met Azure Monitor voor containers. Voor de re-onboarding, moet u [uitschakelen](container-insights-optout.md) bewaking voor het cluster en [inschakelen](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-for-a-new-cluster) Azure Monitor voor containers opnieuw. |  
| `Error retrieving data` na het toevoegen van Azure Monitor voor containers via az aks cli | Wanneer met behulp van onboarding `az aks cli`, zelden, Azure-Monitor voor containers mogelijk niet correct zijn toegevoegd. Controleer of de oplossing geïmplementeerd is. Om dit te doen, gaat u naar uw Log Analytics-werkruimte en of de oplossing beschikbaar is, door te selecteren **oplossingen** in het deelvenster aan de linkerkant. U lost dit probleem, moet u de oplossing implementeren door de instructies te volgen op [Azure Monitor for containers implementeren](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json) |  

Om u te helpen bij het vaststellen van het probleem, wij een probleemoplossing script beschikbaar hebt opgegeven [hier](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script).  

## <a name="next-steps"></a>Volgende stappen
Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](container-insights-analyze.md).