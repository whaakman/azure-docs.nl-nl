---
title: Zelfstudie voor Azure Container Service - Kubernetes bewaken
description: Zelfstudie voor Azure Container Service - Kubernetes bewaken met Microsoft Operations Management Suite (OMS)
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 965ce4b7e154684fc1d171c90f17498afc828a66
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Kubernetes-cluster bewaken met Operations Management Suite

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Het is erg belangrijk om uw Kubernetes-cluster en -containers te bewaken, vooral wanneer u een productiecluster op schaal bewaakt met meerdere apps. 

U kunt gebruikmaken van meerdere Kubernetes-bewakingsoplossingen, zowel van Microsoft als van derden. In deze zelfstudie bewaakt u het Kubernetes-cluster met behulp van Containers in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), de cloud-IT-beheeroplossing van Microsoft. (OMS Containers is beschikbaar als preview-versie.)

In deze zelfstudie, deel zeven van zeven, komen de volgende taken aan bod:

> [!div class="checklist"]
> * Instellingen voor OMS Workspace ophalen
> * OMS-agenten instellen op de Kubernetes-knooppunten
> * Toegang tot de bewakingsgegevens in de OMS-portal of in Azure Portal

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is er een toepassing verpakt in containerinstallatiekopieën, zijn de installatiekopieën geüpload naar Azure Container Registry en is er een Kubernetes-cluster gemaakt. 

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Instellingen voor werkruimte ophalen

Als u de [OMS-portal](https://mms.microsoft.com) opent, gaat u naar **Instellingen** > **Verbonden bronnen** > **Linux-servers**. Hier vindt u de *werkstroom-id* en een primaire of secundaire *werkruimtesleutel*. Noteer deze waarden. U hebt ze nodig om OMS-agenten voor het cluster in te stellen.

## <a name="set-up-oms-agents"></a>OMS-agenten instellen

Hier ziet u een YAML-bestand waarmee u OMS-agenten op de eindpunten van het Linux-cluster kunt instellen. Er wordt een Kubernetes-[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) gemaakt, die één identieke pod op elk clusterknooppunt uitvoert. De resource DaemonSet is zeer geschikt voor het implementeren van een bewakingsagent. 

Sla de volgende tekst op in een bestand met de naam `oms-daemonset.yaml` en vervang de waarden in de tijdelijke aanduidingen met *myWorkspaceID* en *myWorkspaceKey* door de OMS Workspace-id en -sleutel. (Tijdens productie kunt u deze waarden als geheimen coderen.)

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

Maak de DaemonSet met de volgende opdracht:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Voer de volgende opdracht uit om te controleren dat de DaemonSet is gemaakt:

```azurecli-interactive
kubectl get daemonset
```

De uitvoer lijkt op het volgende:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Wanneer de agents worden uitgevoerd, duurt het een aantal minuten voordat de OMS de gegevens heeft opgenomen en verwerkt.

## <a name="access-monitoring-data"></a>Toegang tot bewakingsgegevens

Bekijk en analyseer met de [Container-oplossing](../../log-analytics/log-analytics-containers.md) de bewakingsgegevens van de OMS-container. U kunt dit doen in de OMS-portal of in Azure Portal. 

Als u de Container-oplossing wilt installeren via de [OMS-portal](https://mms.microsoft.com), gaat u naar **Oplossingengalerie**. Voeg vervolgens **Container-oplossing** toe. U kunt de Containers-oplossing ook toevoegen vanaf [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

Zoek in de OMS-portal, op het OMS-dashboard, naar een overzichtstegel **Containers**. Klik op de tegel voor details, waaronder: containergebeurtenissen, fouten, status, voorraad installatiekopieën en CPU- en geheugengebruik. Klik voor meer gedetailleerde informatie op een rij op een tegel of voer een [zoekopdracht in een logboek](../../log-analytics/log-analytics-log-searches.md) uit.

![Dashboard Containers in OMS-portal](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Op dezelfde wijze gaat u in Azure Portal naar **Log Analytics** en selecteert u de naam van uw werkruimte. Klik voor de overzichtstegel **Containers** op **Oplossingen** > **Containers**. Klik voor details op de tegel.

Zie de [Azure Log Analytics-documentatie](../../log-analytics/index.yml) voor gedetailleerde hulp bij het uitvoeren van query's en het analyseren van bewakingsgegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u uw Kubernetes-cluster met OMS bewaakt. Behandelde taken zijn:

> [!div class="checklist"]
> * Instellingen voor OMS Workspace ophalen
> * OMS-agenten instellen op de Kubernetes-knooppunten
> * Toegang tot de bewakingsgegevens in de OMS-portal of in Azure Portal


Volg deze koppeling om voorbeeldscripts te zien voor Containerservice.

> [!div class="nextstepaction"]
> [Voorbeeldscripts voor Azure Container Service](cli-samples.md)
