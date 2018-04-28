---
title: Azure Kubernetes cluster - Operations Management bewaken
description: Bewaking van Kubernetes cluster in Azure Container Service met Log Analytics
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 3b014ce4c91d1dc9fae744ef4b528c98f9f787b3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="monitor-an-azure-container-service-cluster-with-log-analytics"></a>Een Azure Container Service-cluster met logboekanalyse bewaken

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

## <a name="monitoring-containers-with-log-analytics"></a>Bewaking van Containers met logboekanalyse

Log Analytics is van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Container oplossing is een oplossing in Log Analytics, waardoor u de inventaris van de container, prestaties en logboeken weergeven op één locatie. U kunt controleren, containers oplossen door de logboeken in een centrale locatie bekijken en veel ruis veroorzaken verbruikt overtollige container op een host vinden.

![](media/container-service-monitoring-oms/image1.png)

Raadpleeg voor meer informatie over Container oplossing de [Container oplossing Log Analytics](../../log-analytics/log-analytics-containers.md).

## <a name="installing-log-analytics-on-kubernetes"></a>Log Analytics installeren op Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Uw werkruimte-ID en sleutel ophalen
Neem contact op met de service het-agent moet worden geconfigureerd met een werkruimte-ID en een werkruimtesleutel voor de logboekanalyse. Ophalen van de werkruimte-ID en de sleutel die u wilt maken van een account bij <https://mms.microsoft.com>.
Volg de stappen om een account te maken. Wanneer u klaar bent voor het maken van het account moet u eerst uw ID en -sleutel door te klikken op **instellingen**, klikt u vervolgens **verbonden bronnen**, en vervolgens **Linux-Servers**, zoals hieronder weergegeven.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>De logboekanalyse agent installeert met een DaemonSet
DaemonSets worden gebruikt door Kubernetes één exemplaar van een container uitvoeren op elke host in het cluster.
Ze zijn ideaal voor bewaking agenten uit te voeren.

Hier volgt de [DaemonSet YAML bestand](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Sla het op een bestand met de naam `oms-daemonset.yaml` en vervang de waarden van de tijdelijke aanduiding voor `WSID` en `KEY` met uw werkruimte-ID en sleutel in het bestand.

Nadat u hebt uw werkruimte-ID en sleutel toegevoegd aan de configuratie DaemonSet, kunt u de Log Analytics-agent installeren op uw cluster met de `kubectl` opdrachtregelprogramma:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Installatie van de Log Analytics-agent met behulp van een geheim Kubernetes
U kunt Kubernetes geheim ter bescherming van uw Log Analytics-werkruimte-ID en sleutel gebruiken als onderdeel van DaemonSet YAML-bestand.

 - Kopieer het script, geheime sjabloonbestand en het bestand DaemonSet YAML (van [opslagplaats](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) en zorg ervoor dat ze zich op dezelfde map.
      - geheim script - geheim gen.sh genereren
      - geheime sjabloon - geheim template.yaml
   - Bestand DaemonSet YAML - omsagent-ds-secrets.yaml
 - Voer het script uit. Het script vraagt naar het Log Analytics-werkruimte-ID en de primaire sleutel. Plaats die en het script maakt een geheime yaml-bestand zodat u deze kunt uitvoeren.
   ```
   #> sudo bash ./secret-gen.sh
   ```

   - Geheimen schil maken door het volgende: ``` kubectl create -f omsagentsecret.yaml ```

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

  - Maken van uw omsagent daemon-set door te voeren ``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Conclusie
Dat is alles. Na een paar minuten moet u het volgende kunnen om gegevens naar het Log Analytics-dashboard te bekijken.
