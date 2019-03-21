---
title: (AFGESCHAFT) Azure Kubernetes-cluster - operationeel beheer bewaken
description: Bewaking van Kubernetes-cluster in Azure Container Service met behulp van Log Analytics
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 0d9f13003a9b81085fda9635fc8b07566a1c0c66
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58107522"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>(AFGESCHAFT) Een Azure Container Service-cluster bewaken met Log Analytics

> [!TIP]
> Zie voor de bijgewerkte versie dit artikel die gebruikmaakt van Azure Kubernetes Service, [Azure Monitor voor containers](../../azure-monitor/insights/container-insights-overview.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u hebt de `az` Azure cli en `kubectl` hulpprogramma's zijn geïnstalleerd.

U kunt testen als u hebt de `az` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ az --version
```

Als u geen de `az` hulpprogramma is geïnstalleerd, worden er instructies [hier](https://github.com/azure/azure-cli#installation).
U kunt ook gebruiken [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), heeft de `az` Azure cli en `kubectl` hulpprogramma's voor u geïnstalleerd.

U kunt testen als u hebt de `kubectl` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:
```console
$ az acs kubernetes install-cli
```

Om te controleren of er kubernetes-sleutels die zijn geïnstalleerd in uw kubectl-hulpprogramma die kan worden uitgevoerd:
```console
$ kubectl get nodes
```

Als de bovenstaande opdracht fouten af, moet u kubernetes-cluster sleutels in uw hulpprogramma kubectl installeren. U kunt dit doen met de volgende opdracht:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Bewaking van Containers met Log Analytics

Log Analytics is van Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur. Container-oplossing is een oplossing in Log Analytics, dat helpt u bij het weergeven van de container-inventaris, prestaties en logboeken op één locatie. U kunt controleren, containers oplossen door de logboeken in de centrale locatie bekijken en zoeken ruis verbruikt overtollige container op een host.

![](media/container-service-monitoring-oms/image1.png)

Raadpleeg voor meer informatie over Container-oplossing, de [Container-oplossing Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="installing-log-analytics-on-kubernetes"></a>Installeren van Log Analytics in Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Uw werkruimte-ID en sleutel ophalen
Agent om te communiceren met de service het moet worden geconfigureerd met een werkruimte-ID en de sleutel van een werkruimte voor Log Analytics. Om op te halen van de werkruimte-ID en de sleutel die u wilt maken van een account bij <https://mms.microsoft.com>.
Volg de stappen om een account te maken. Wanneer u bent klaar met het account te maken kunt u uw ID verkrijgen en sleutel door te klikken op de **Log Analytics** blade, gevolgd door de naam van uw werkruimte. Klik vervolgens onder **geavanceerde instellingen**, **verbonden bronnen**, en vervolgens **Linux-Servers**, vindt u de informatie die u nodig hebt, zoals hieronder wordt weergegeven.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>De Log Analytics-agent met behulp van een DaemonSet installeren
DaemonSets worden gebruikt door Kubernetes om uit te voeren van één exemplaar van een container op elke host in het cluster.
Ze zijn ideaal voor het uitvoeren van bewakingsagents.

Hier volgt de [DaemonSet YAML-bestand](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Sla deze op een bestand met de naam `oms-daemonset.yaml` en vervang de tijdelijke aanduiding waarden voor `WSID` en `KEY` met uw werkruimte-ID en sleutel in het bestand.

Nadat u uw werkruimte-ID en sleutel aan de configuratie van de DaemonSet hebt toegevoegd, kunt u de Log Analytics-agent installeren op uw cluster met de `kubectl` opdrachtregelprogramma:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Installatie van de Log Analytics-agent met behulp van een Kubernetes-geheim
U kunt ter bescherming van uw Log Analytics-werkruimte-ID en sleutel Kubernetes-geheim gebruiken als onderdeel van de DaemonSet YAML-bestand.

- Kopieer het script, geheime sjabloonbestand en de DaemonSet YAML-bestand (van [opslagplaats](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) en zorg ervoor dat ze zich in dezelfde map.
  - Geheim script - geheim gen.sh genereren
  - geheime sjabloon - geheim template.yaml
    - DaemonSet YAML-bestand - omsagent-ds-secrets.yaml
- Voer het script uit. Het script vraagt om de Log Analytics-werkruimte-ID en primaire sleutel. Plaats die en het script maakt u een geheim yaml-bestand, zodat u deze kunt uitvoeren.
  ```
  #> sudo bash ./secret-gen.sh
  ```

  - Maak de schil geheimen door het uitvoeren van de volgende: ``` kubectl create -f omsagentsecret.yaml ```

  - Als u wilt controleren, voert u het volgende uit:

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

  - Maken van uw omsagent-daemon-set door uit te voeren ``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Conclusie
Dat is alles. Na een paar minuten, zou het mogelijk om gegevens naar uw Log Analytics-dashboard te bekijken.
