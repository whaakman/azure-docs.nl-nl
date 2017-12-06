---
title: Azure Kubernetes cluster - Operations Management bewaken
description: Bewaking van Kubernetes cluster in Azure Container Service met behulp van Microsoft Operations Management Suite
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e8a68896c923d785fea84cef60f8d2015906f342
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Een Azure Container Service-cluster met de Microsoft Operations Management Suite (OMS) bewaken

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u hebt de `az` Azure cli en `kubectl` hulpprogramma's geïnstalleerd.

U kunt testen, hebt u de `az` hulpprogramma is geïnstalleerd door te voeren:

```console
$ az --version
```

Als u hebt de `az` hulpprogramma is geïnstalleerd, er zijn instructies [hier](https://github.com/azure/azure-cli#installation).  
U kunt ook gebruiken [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), die de `az` Azure cli en `kubectl` hulpprogramma's voor u hebt geïnstalleerd.  

U kunt testen, hebt u de `kubectl` hulpprogramma is geïnstalleerd door te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:
```console
$ az acs kubernetes install-cli
```

Om te controleren of er kubernetes sleutels die zijn geïnstalleerd in uw kubectl-hulpprogramma die kan worden uitgevoerd:
```console
$ kubectl get nodes
```

Als de bovenstaande opdracht fouten af, moet u kubernetes cluster sleutels in de kubectl tool te installeren. U kunt dat doen met de volgende opdracht:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Bewaking van Containers met Operations Management Suite (OMS)

Microsoft Operations Management (OMS) is van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Container oplossing is een oplossing in OMS Log Analytics, waardoor u de inventaris van de container, prestaties en logboeken weergeven op één locatie. U kunt controleren, containers oplossen door de logboeken in een centrale locatie bekijken en veel ruis veroorzaken verbruikt overtollige container op een host vinden.

![](media/container-service-monitoring-oms/image1.png)

Raadpleeg voor meer informatie over Container oplossing de [Container oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>OMS installeren op Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Uw werkruimte-ID en sleutel ophalen
Neem contact op met de service het-agent moet worden geconfigureerd met een werkruimte-id en een werkruimtesleutel voor de OMS. Ophalen van de werkruimte-id en de sleutel die u wilt maken van een OMS-account bij <https://mms.microsoft.com>. Volg de stappen om een account te maken. Wanneer u klaar bent voor het maken van het account moet u eerst uw id en -sleutel door te klikken op **instellingen**, klikt u vervolgens **verbonden bronnen**, en vervolgens **Linux-Servers**, zoals hieronder weergegeven.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a>Installeer de OMS-agent met behulp van een DaemonSet
DaemonSets worden gebruikt door Kubernetes één exemplaar van een container uitvoeren op elke host in het cluster.
Ze zijn ideaal voor bewaking agenten uit te voeren.

Hier volgt de [DaemonSet YAML bestand](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Sla het op een bestand met de naam `oms-daemonset.yaml` en vervang de waarden van de tijdelijke aanduiding voor `WSID` en `KEY` met uw werkruimte-id en sleutel in het bestand.

Wanneer u uw werkruimte-ID en sleutel aan de configuratie van de DaemonSet hebt toegevoegd, kunt u de OMS-agent installeren op uw cluster met de `kubectl` opdrachtregelprogramma:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a>Installatie van de OMS-agent met behulp van een geheim Kubernetes
Als u wilt beveiligen van uw OMS-werkruimte-ID en u kunnen Kubernetes geheim gebruiken als onderdeel van DaemonSet YAML-bestand.

 - Kopieer het script, geheime sjabloonbestand en het bestand DaemonSet YAML (van [opslagplaats](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) en zorg ervoor dat ze zich op dezelfde map. 
      - geheim script - geheim gen.sh genereren
      - geheime sjabloon - geheim template.yaml
   - Bestand DaemonSet YAML - omsagent-ds-secrets.yaml
 - Voer het script uit. Het script vraagt naar de OMS-werkruimte-ID en de primaire sleutel. Plaats die en het script maakt een geheime yaml-bestand zodat u deze kunt uitvoeren.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Geheimen schil maken door het volgende:``` kubectl create -f omsagentsecret.yaml ```
 
   - Als u wilt controleren, voert u de volgende: 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - Maken van uw omsagent daemon-set door te voeren``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Conclusie
Dat is alles. Na een paar minuten, moet u mogelijk om gegevens naar uw OMS-dashboard te bekijken.
