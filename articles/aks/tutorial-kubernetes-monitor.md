---
title: Kubernetes op Azure zelfstudie - Monitor Kubernetes
description: AKS zelfstudie - Monitor Kubernetes met Microsoft Operations Management Suite (OMS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b01aa01df198ce75b2f8b66d28a2db68b1c30b87
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="monitor-azure-container-service-aks"></a>Monitor Azure Containerservice (AKS)

Uw cluster Kubernetes en containers is niet kritiek, vooral bij een productiecluster op grote schaal met meerdere toepassingen.

In deze zelfstudie configureert u bewaking van uw AKS cluster met de [Containers oplossing voor logboekanalyse][log-analytics-containers].

Deze zelfstudie maakt deel uit zeven acht, komen de volgende taken:

> [!div class="checklist"]
> * De container bewakingsoplossing configureren
> * De bewaking agents configureren
> * Toegang tot de controlegegevens in de Azure portal

## <a name="before-you-begin"></a>Voordat u begint

Een toepassing is in de vorige zelfstudies ingepakt in container-installatiekopieën, deze installatiekopieën geüpload naar het register van Azure-Container en een Kubernetes-cluster gemaakt.

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken][aks-tutorial-prepare-app].

## <a name="configure-the-monitoring-solution"></a>De oplossing voor controle configureren

Selecteer in de Azure-portal **nieuw** en zoek naar `Container Monitoring Solution`. Zodra u zich bevindt, selecteert u **maken**.

![Oplossing toevoegen](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Maak een nieuwe OMS-werkruimte of een bestaande set selecteren. Het formulier OMS-werkruimte begeleidt u bij dit proces.

Selecteer bij het maken van de werkruimte **vastmaken aan dashboard** gemakkelijk kunt ophalen.

![OMS-werkruimte](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Wanneer u klaar bent, selecteer **OK**. Als validatie is voltooid, selecteert u **maken** de bewakingsoplossing container maken.

Zodra de werkruimte is gemaakt, wordt deze in de Azure portal aan u gepresenteerd.

## <a name="get-workspace-settings"></a>Werkruimte-instellingen ophalen

De logboekanalyse werkruimte-ID en sleutel nodig zijn voor het configureren van de oplossing-agent op de knooppunten Kubernetes.

Als u wilt deze waarden ophaalt, selecteer **OMS-werkruimte** in het menu van container-oplossingen-links. Selecteer **geavanceerde instellingen** en noteer de **WERKRUIMTE-ID** en de **primaire sleutel**.

## <a name="configure-monitoring-agents"></a>Monitoring-agents configureren

Het volgende Kubernetes manifestbestand kan worden gebruikt voor het configureren van de container monitoring-agents op een cluster Kubernetes. Het maken van een Kubernetes [DaemonSet][kubernetes-daemonset], die een enkele schil wordt uitgevoerd op elk clusterknooppunt.

Opslaan van de volgende tekst in een bestand met de naam `oms-daemonset.yaml`, en vervang de tijdelijke aanduiding voor waarden voor `WSID` en `KEY` Log Analytics-werkruimte-ID en sleutel van uw.

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
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
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
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
        - mountPath: /var/log
          name: host-log
        - mountPath: /var/lib/docker/containers/
          name: container-log
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
    - name: container-hostname
      hostPath:
       path: /etc/hostname
    - name: host-log
      hostPath:
       path: /var/log
    - name: container-log
      hostPath:
       path: /var/lib/docker/containers/
```

De DaemonSet maken met de volgende opdracht:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Om te zien dat de DaemonSet is gemaakt, worden uitgevoerd:

```azurecli-interactive
kubectl get daemonset
```

De uitvoer lijkt op het volgende:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Nadat de agents uitvoert, duurt het enkele minuten voor OMS opnemen en verwerken van de gegevens.

## <a name="access-monitoring-data"></a>Toegang tot bewakingsgegevens

Selecteer de werkruimte voor logboekanalyse die is vastgemaakt aan het portaldashboard in de Azure-portal. Klik op de **Container bewaking oplossing** tegel. Hier vindt u informatie over de AKS cluster en de containers uit het cluster.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Zie de [Azure Log Analytics-documentatie] [ log-analytics-docs] voor gedetailleerde hulp bij het uitvoeren van query's en analyseren van bewakingsgegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt bewaakt u uw cluster Kubernetes met OMS. Taken aan de orde opgenomen:

> [!div class="checklist"]
> * De container bewakingsoplossing configureren
> * De bewaking agents configureren
> * Toegang tot de controlegegevens in de Azure portal

Ga naar de volgende zelfstudie voor meer informatie over het upgraden van Kubernetes naar een nieuwe versie.

> [!div class="nextstepaction"]
> [Upgrade Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
