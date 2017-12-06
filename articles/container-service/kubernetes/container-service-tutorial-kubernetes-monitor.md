---
title: Zelfstudie voor Azure Container Service - Monitor Kubernetes
description: Zelfstudie voor Azure Container Service - Monitor Kubernetes met Microsoft Operations Management Suite (OMS)
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 948e3aeea34a0355c3d958f29008c26499e19ba4
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Monitor voor een cluster Kubernetes met Operations Management Suite

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Uw cluster Kubernetes en containers is niet kritiek, vooral wanneer u een productiecluster op schaal met meerdere apps beheren. 

U kunt profiteren van verschillende Kubernetes bewakingsoplossingen, van Microsoft of andere providers. In deze zelfstudie maakt u uw cluster Kubernetes controleren met behulp van de oplossing Containers in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), van de Microsoft cloud-gebaseerde IT-oplossing. (De Containers OMS-oplossing is in preview.)

Deze zelfstudie maakt deel uit zeven zeven, komen de volgende taken:

> [!div class="checklist"]
> * OMS-werkruimte-instellingen ophalen
> * OMS-agents op de knooppunten Kubernetes instellen
> * Toegang tot gegevens in de OMS-portal of Azure-portal bewaken

## <a name="before-you-begin"></a>Voordat u begint

Een toepassing is in de vorige zelfstudies ingepakt in container-installatiekopieën, deze installatiekopieën geüpload naar het register van Azure-Container en een Kubernetes-cluster gemaakt. 

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Werkruimte-instellingen ophalen

Wanneer u toegang hebt tot de [OMS-portal](https://mms.microsoft.com), gaat u naar **instellingen** > **verbonden bronnen** > **Linux-Servers**. U vindt er, de *werkruimte-ID* en een primaire of secundaire site *Werkruimtesleutel*. Noteer deze waarden, u moet voor het instellen van OMS-agents op het cluster.

## <a name="set-up-oms-agents"></a>Instellen van OMS-agents

Hier volgt een YAML-bestand voor het instellen van OMS-agents op de clusterknooppunten Linux. Het maken van een Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), die een enkele identiek schil wordt uitgevoerd op elk clusterknooppunt. De bron DaemonSet is ideaal voor het implementeren van een agent voor toepassingsbewaking. 

Opslaan van de volgende tekst in een bestand met de naam `oms-daemonset.yaml`, en vervang de tijdelijke aanduiding voor waarden voor *myWorkspaceID* en *myWorkspaceKey* OMS-werkruimte-ID en sleutel van uw. (In productie, kunt u deze waarden coderen als geheimen.)

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

De DaemonSet maken met de volgende opdracht:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Om te zien dat de DaemonSet is gemaakt, worden uitgevoerd:

```azurecli-interactive
kubectl get daemonset
```

De uitvoer lijkt op het volgende:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Nadat de agents uitvoert, duurt het enkele minuten voor OMS opnemen en verwerken van de gegevens.

## <a name="access-monitoring-data"></a>Toegang tot bewakingsgegevens

Weergeven en analyseren van de OMS-container bewakingsgegevens met de [Container oplossing](../../log-analytics/log-analytics-containers.md) in de OMS-portal of de Azure-portal. 

Voor het installeren van de Container oplossing met behulp van de [OMS-portal](https://mms.microsoft.com), gaat u naar **galerie met oplossingen**. Voeg vervolgens **Container oplossing**. U kunt ook toevoegen de Containers-oplossing van de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

Zoek in de OMS-portal een **Containers** tegel samenvatting op het dashboard OMS. Klik op de tegel voor meer informatie, met inbegrip van: container gebeurtenissen, fouten, status, inventaris van de installatiekopie en CPU- en geheugengebruik. Voor meer gedetailleerde informatie op een rij op een tegel of voer een [logboek zoeken](../../log-analytics/log-analytics-log-searches.md).

![Containers dashboard in OMS-portal](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Op deze manier in de Azure-portal, gaat u naar **logboekanalyse** en selecteer de Werkruimtenaam van uw. Om te zien de **Containers** tegel samenvatting, klikt u op **oplossingen** > **Containers**. Klik op de tegel voor details.

Zie de [Azure Log Analytics-documentatie](../../log-analytics/index.yml) voor gedetailleerde hulp bij het uitvoeren van query's en analyseren van bewakingsgegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt bewaakt u uw cluster Kubernetes met OMS. Taken aan de orde opgenomen:

> [!div class="checklist"]
> * OMS-werkruimte-instellingen ophalen
> * OMS-agents op de knooppunten Kubernetes instellen
> * Toegang tot gegevens in de OMS-portal of Azure-portal bewaken


Volg deze link om te zien van vooraf samengestelde scriptvoorbeelden voor Container Service.

> [!div class="nextstepaction"]
> [Azure Container Service script-voorbeelden](cli-samples.md)
