---
title: 'Zelfstudie voor Kubernetes in Azure: Kubernetes bewaken'
description: AKS-zelfstudie - Kubernetes bewaken met Microsoft Operations Management Suite (OMS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 227601858dbe07e6cb774a2d24878ddca05aaf56
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="monitor-azure-container-service-aks"></a>Azure Container Service (AKS) bewaken

Het is erg belangrijk om uw Kubernetes-cluster en -containers te bewaken, vooral wanneer u een productiecluster op schaal uitvoert met meerdere toepassingen.

In deze zelfstudie configureert u bewaking van uw AKS-cluster met de [containerbewakingsoplossing voor Log Analytics][log-analytics-containers].

In deze zelfstudie, deel zeven van acht, komen de volgende taken aan bod:

> [!div class="checklist"]
> * De containerbewakingsoplossing configureren
> * De bewakingsagents configureren
> * Toegang tot de controlegegevens in Azure Portal

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is er een toepassing verpakt in containerinstallatiekopieën, zijn de installatiekopieën geüpload naar Azure Container Registry en is er een Kubernetes-cluster gemaakt.

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="configure-the-monitoring-solution"></a>De bewakingsoplossing configureren

Selecteer in Azure Portal **Een resource maken** en zoek naar `Container Monitoring Solution`. Nadat u dat hebt gevonden, selecteert u **Maken**.

![Oplossing toevoegen](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Maak een nieuwe OMS-werkruimte of selecteer een bestaande werkruimte. Het formulier OMS-werkruimte begeleidt u door dit proces.

Wanneer u de werkruimte maakt, selecteert u **Aan dashboard vastmaken** om deze eenvoudig op te halen.

![OMS-werkruimte](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Selecteer **Ok** wanneer u gereed bent. Nadat de validatie is voltooid, selecteert u **Maken** om de containerbewakingsoplossing te maken.

Nadat de werkruimte is gemaakt, wordt deze weergegeven in Azure Portal.

## <a name="get-workspace-settings"></a>Instellingen voor werkruimte ophalen

De werkruimte-id en -sleutel van Log Analytics zijn nodig om de oplossingsagent op de Kubernetes-knooppunten te configureren.

Als u deze waarden wilt ophalen, selecteert u **OMS-werkruimte** in het linkermenu van de containeroplossing. Selecteer **Geavanceerde instellingen** en noteer de **WERKRUIMTE-ID** en de **PRIMAIRE SLEUTEL**.

## <a name="create-kubernetes-secret"></a>Kubernetes-geheim maken

Sla de instellingen van de OMS-werkruimte op in een Kubernetes-geheim met de naam `omsagent-secret`. Doe dit met behulp van de opdracht [kubectl create secret][kubectl-create-secret]. Werk `WORKSPACE_ID` bij met de id van uw OMS-werkruimte en `WORKSPACE_KEY` met de sleutel van de werkruimte.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="configure-monitoring-agents"></a>Bewakingsagents configureren

Het volgende Kubernetes-manifestbestand kan worden gebruikt om de containerbewakingsagents op een Kubernetes-cluster te configureren. Het maakt een Kubernetes-[DaemonSet][kubernetes-daemonset] die één pod op elk clusterknooppunt uitvoert.

Sla de volgende tekst op in een bestand met de naam `oms-daemonset.yaml`.

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
    agentVersion: 1.4.3-174
    dockerProviderVersion: 1.0.0-30
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
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
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        - mountPath: /var/lib/docker/containers 
          name: containerlog-path  
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   nodeSelector:
    beta.kubernetes.io/os: linux    
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"     
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log 
    - name: omsagent-secret
      secret:
       secretName: omsagent-secret
    - name: containerlog-path
      hostPath:
       path: /var/lib/docker/containers    
```

Maak de DaemonSet met de volgende opdracht:

```azurecli
kubectl create -f oms-daemonset.yaml
```

Voer de volgende opdracht uit om te controleren dat de DaemonSet is gemaakt:

```azurecli
kubectl get daemonset
```

De uitvoer lijkt op het volgende:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Wanneer de agents worden uitgevoerd, duurt het een aantal minuten voordat de OMS de gegevens heeft opgenomen en verwerkt.

## <a name="access-monitoring-data"></a>Toegang tot bewakingsgegevens

Selecteer in Azure Portal de Log Analytics-werkruimte die is vastgemaakt aan het portaldashboard. Klik op de tegel **Containerbewakingsoplossing**. Hier vindt u informatie over het AKS-cluster en de containers van het cluster.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Raadpleeg de [Azure Log Analytics-documentatie][log-analytics-docs] voor gedetailleerde hulp bij het uitvoeren van query's en het analyseren van bewakingsgegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u uw Kubernetes-cluster met OMS bewaakt. Behandelde taken zijn:

> [!div class="checklist"]
> * De containerbewakingsoplossing configureren
> * De bewakingsagents configureren
> * Toegang tot de controlegegevens in Azure Portal

Ga naar de volgende zelfstudie om te leren hoe u Kubernetes bijwerkt naar een nieuwe versie.

> [!div class="nextstepaction"]
> [Kubernetes bijwerken][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubectl-create-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
