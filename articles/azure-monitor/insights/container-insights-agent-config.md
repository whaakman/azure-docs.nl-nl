---
title: Azure Monitor configureren voor het verzamelen van gegevens van containers en agents | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Azure Monitor voor containers-agent kunt configureren voor het beheren van de logboek verzameling van stdout/stderr en omgevings variabelen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 12010aaa7bc90bd200264549ad3efb79f46576c6
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867658"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Gegevens verzameling van agents voor Azure Monitor voor containers configureren

Azure Monitor voor containers worden stdout-, stderr-en omgevings variabelen verzameld van container werkbelastingen die zijn geïmplementeerd op beheerde Kubernetes-clusters die worden gehost op de Azure Kubernetes service (AKS) van de container agent. Deze agent kan ook tijdreeks gegevens (ook wel metrische waarden genoemd) verzamelen van Prometheus met behulp van de container agent zonder dat u een Prometheus-server en-data base hoeft in te stellen en te beheren. U kunt instellingen voor het verzamelen van agent gegevens configureren door een aangepaste Kubernetes ConfigMaps te maken om deze ervaring te beheren. 

In dit artikel wordt beschreven hoe u ConfigMap maakt en hoe u gegevens verzameling kunt configureren op basis van uw vereisten.

>[!NOTE]
>De ondersteuning voor Prometheus is op dit moment een functie in open bare preview.
>

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Uw cluster configureren met instellingen voor aangepaste gegevens verzameling

Er wordt een sjabloon ConfigMap-bestand geleverd waarmee u het eenvoudig kunt bewerken met uw aanpassingen zonder dat u het helemaal opnieuw hoeft te maken. Voordat u begint, raadpleegt u de Kubernetes-documentatie over [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) en leert u hoe u ConfigMaps maakt, configureert en implementeert. Hierdoor kunt u stderr en stdout per naam ruimte of in het hele cluster filteren, en omgevings variabelen voor elke container die wordt uitgevoerd op alle verschillende knoop punten in het cluster.

>[!IMPORTANT]
>De minimale agent versie die wordt ondersteund voor het verzamelen van stdout-, stderr-en omgevings variabelen van container werkbelastingen, is ciprod06142019 of hoger. De minimale agent versie die wordt ondersteund voor het terugvallen van Prometheus-metrische gegevens is ciprod07092019 of hoger. Als u de versie van de agent wilt controleren, selecteert u een knoop punt op het tabblad **knoop** punt en noteert u in het deel venster Eigenschappen de waarde van de **Agent-installatie kopie label** eigenschap.  

### <a name="overview-of-configurable-data-collection-settings"></a>Overzicht van Configureer bare instellingen voor gegevens verzameling

Hieronder vindt u de instellingen die kunnen worden geconfigureerd om het verzamelen van gegevens te beheren.

|Sleutel |Gegevenstype |Waarde |Description |
|----|----------|------|------------|
|`schema-version` |Teken reeks (hoofdletter gevoelig) |v1 |Dit is de schema versie die door de agent wordt gebruikt bij het parseren van deze ConfigMap. Momenteel ondersteunde schema versie v1. Het wijzigen van deze waarde wordt niet ondersteund en wordt afgewezen wanneer ConfigMap wordt geëvalueerd.|
|`config-version` |Tekenreeks | | Ondersteunt de mogelijkheid om de versie van dit configuratie bestand in uw bron beheersysteem of opslag plaats bij te houden. Het maximum aantal toegestane tekens is 10 en alle andere tekens worden afgekapt. |
|`[log_collection_settings.stdout] enabled =` |Boolean-waarde | waar of onwaar | Hiermee wordt bepaald of de collectie stdout container log is ingeschakeld. Als deze optie `true` is ingesteld op en er geen naam ruimten worden uitgesloten voor`log_collection_settings.stdout.exclude_namespaces` de stdout-logboek verzameling (hieronder), worden de stdout-logboeken verzameld van alle containers in alle knoop punten in het cluster. Als dat niet is opgegeven in ConfigMaps, is `enabled = true`de standaard waarde. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Reeks | Door komma's gescheiden matrix |Matrix van Kubernetes-naam ruimten waarvoor stdout-logboeken worden niet verzameld. Deze instelling is alleen effectief als `log_collection_settings.stdout.enabled` deze is ingesteld `true`op. Als dat niet is opgegeven in ConfigMap, is `exclude_namespaces = ["kube-system"]`de standaard waarde.|
|`[log_collection_settings.stderr] enabled =` |Boolean-waarde | waar of onwaar |Hiermee wordt bepaald of de logboek verzameling voor de stderr-container is ingeschakeld. Als deze optie `true` is ingesteld op en geen naam ruimten worden uitgesloten voor de`log_collection_settings.stderr.exclude_namespaces` stdout-logboek verzameling (instelling), worden stderr-logboeken verzameld van alle containers op alle knoop punten in het cluster. Als dat niet is opgegeven in ConfigMaps, is `enabled = true`de standaard waarde. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Reeks |Door komma's gescheiden matrix |Matrix van Kubernetes-naam ruimten waarvoor stderr-logboeken niet zullen worden verzameld. Deze instelling is alleen effectief als `log_collection_settings.stdout.enabled` deze is ingesteld `true`op. Als dat niet is opgegeven in ConfigMap, is `exclude_namespaces = ["kube-system"]`de standaard waarde. |
| `[log_collection_settings.env_var] enabled =` |Boolean-waarde | waar of onwaar | Hiermee wordt bepaald of de verzameling van omgevings variabelen is ingeschakeld. Als deze instelling `false`is ingesteld op, worden er geen omgevings variabelen verzameld voor alle containers die worden uitgevoerd op alle knoop punten in het cluster. Als dat niet is opgegeven in ConfigMap, is `enabled = true`de standaard waarde. |

## <a name="overview-of-configurable-prometheus-scraping-settings"></a>Overzicht van Configureer bare Prometheus-instellingen voor uitval

Actieve uitval van metrische gegevens uit Prometheus worden uitgevoerd vanuit een van de volgende twee perspectieven:

* Cluster-Wide-HTTP URL en Detecteer doelen van de vermelde eind punten van een service, K8S services zoals uitvoeren-DNS en uitvoeren-State-metrics en pod annotaties die specifiek zijn voor een toepassing. De metrische gegevens die in deze context worden verzameld, worden gedefinieerd in de sectie ConfigMap *[Prometheus data_collection_settings. cluster]* .
* Node-Wide-HTTP URL en detectie doelen van de vermelde eind punten van een service. De metrische gegevens die in deze context worden verzameld, worden gedefinieerd in de sectie ConfigMap *[Prometheus_data_collection_settings. node]* .

|Scope | Sleutel | Gegevenstype | Value | Description |
|------|-----|-----------|-------|-------------|
| Cluster-breed | | | | Geef een van de volgende drie methoden op om eind punten voor metrische gegevens af te vallen. |
| | `urls` | Reeks | Door komma's gescheiden matrix | HTTP-eind punt (ofwel een IP-adres of een geldig URL-pad opgegeven). Bijvoorbeeld: `urls=[$NODE_IP/metrics]`. ($NODE _IP is een specifieke Azure Monitor voor containers-para meters en kan worden gebruikt in plaats van het IP-adres van het knoop punt. Mag alleen hoofd letters zijn.) |
| | `kubernetes_services` | Tekenreeks | Door komma's gescheiden matrix | Een matrix met Kubernetes-Services voor het opwaarderen van metrische gegevens uit uitvoeren. Bijvoorbeeld`kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics", http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boolean-waarde | waar of onwaar | Als deze optie `true` is ingesteld op in de instellingen voor het hele cluster, worden de Kubernetes voor de volgende Prometheus-aantekeningen door Azure monitor voor de agent van containers.<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boolean-waarde | waar of onwaar | Hiermee schakelt u de pod in. |
| | `prometheus.io/scheme` | Tekenreeks | http of https | De standaard instelling is het uitvallen van HTTP. Stel indien nodig in op `https`. | 
| | `prometheus.io/path` | Tekenreeks | Door komma's gescheiden matrix | Het HTTP-bronpad waarvan de metrische gegevens moeten worden opgehaald. Als het pad naar metrische gegevens niet `/metrics`is, definieert u dit met deze aantekening. |
| | `prometheus.io/port` | Tekenreeks | 9102 | Geef een poort op waarop moet worden geluisterd. Als poort niet is ingesteld, wordt de standaard waarde van 9102. |
| Knooppunt-breed | `urls` | Reeks | Door komma's gescheiden matrix | HTTP-eind punt (ofwel een IP-adres of een geldig URL-pad opgegeven). Bijvoorbeeld: `urls=[$NODE_IP/metrics]`. ($NODE _IP is een specifieke Azure Monitor voor containers-para meters en kan worden gebruikt in plaats van het IP-adres van het knoop punt. Mag alleen hoofd letters zijn.) |
| Het hele knoop punt of het hele cluster | `interval` | Tekenreeks | 60s | De standaard waarde voor de verzamelings interval is 1 minuut (60 seconden). U kunt de verzameling voor *[prometheus_data_collection_settings. node]* en/of *[prometheus_data_collection_settings. cluster]* wijzigen in tijds eenheden zoals NS, US (of Âμs), MS, s, m, h. |
| Het hele knoop punt of het hele cluster | `fieldpass`<br> `fielddrop`| Tekenreeks | Door komma's gescheiden matrix | U kunt bepaalde metrische gegevens opgeven die u wilt verzamelen of niet van het eind punt door de vermelding`fieldpass`toestaan () en weigeren`fielddrop`() in te stellen. U moet eerst de acceptatie lijst instellen. |

ConfigMap is een globale lijst en er kan slechts één ConfigMap op de agent worden toegepast. U kunt de verzamelingen niet overConfigMapen.

### <a name="configure-and-deploy-configmaps"></a>ConfigMaps configureren en implementeren

Voer de volgende stappen uit om uw ConfigMap-configuratie bestand te configureren en te implementeren in uw cluster.

1. [Down load](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) het sjabloon bestand ConfigMap yaml en sla het op als container-AZM-MS-agentconfig. yaml.  
1. Bewerk het ConfigMap yaml-bestand met uw aanpassingen.

    - Als u specifieke naam ruimten wilt uitsluiten voor de stdout-logboek verzameling, configureert u de sleutel/waarde met `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`behulp van het volgende voor beeld:.
    - Als u de verzameling van omgevings variabelen voor een specifieke container wilt uitschakelen, `[log_collection_settings.env_var] enabled = true` stelt u de sleutel/waarde in om variabelen verzameling globaal in te scha kelen en volgt u de [onderstaande stappen om](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) de configuratie voor de specifieke container te volt ooien.
    - Voor het uitschakelen van het hoofdletter gebruik van de logboek verzameling van stderr kunt u de sleutel/waarde configureren `[log_collection_settings.stderr] enabled = false`met het volgende voor beeld:.

1. Maak ConfigMap door de volgende kubectl-opdracht uit `kubectl apply -f <configmap_yaml_file.yaml>`te voeren:.
    
    Voorbeeld: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Het kan een paar minuten duren voordat de configuratie wijziging is doorgevoerd en alle omsagent in het cluster opnieuw worden opgestart. Het opnieuw opstarten is een rolling start voor alle omsagent-peulen, niet allemaal tegelijk opnieuw opstarten. Wanneer het opnieuw opstarten is voltooid, wordt een bericht weer gegeven dat er ongeveer als volgt uitziet en het resultaat bevat `configmap "container-azm-ms-agentconfig" created`:.

Als u wilt controleren of de configuratie is toegepast, gebruikt u de volgende opdracht om de logboeken te controleren `kubectl logs omsagent-fdf58 -n=kube-system`vanuit een agent Pod:. Als er configuratie fouten zijn van de omsagent Peul, worden de volgende fouten weer gegeven in de uitvoer:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fouten met betrekking tot het Toep assen van configuratie wijzigingen voor Prometheus zijn ook beschikbaar voor controle.  Van de logboeken van een agent pod met dezelfde `kubectl logs` opdracht of vanuit Live logs. Live-logboeken toont fouten die vergelijkbaar zijn met de volgende:

```
2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
```

Fouten zorgen ervoor dat omsagent het bestand niet kan parseren, waardoor het opnieuw wordt gestart en de standaard configuratie wordt gebruikt. Nadat u de fout (en) in ConfigMap hebt gecorrigeerd, slaat u het yaml-bestand op en past u de bijgewerkte ConfigMaps `kubectl apply -f <configmap_yaml_file.yaml`toe door de opdracht uit te voeren:.

## <a name="applying-updated-configmap"></a>Bijgewerkte ConfigMap Toep assen

Als u al een ConfigMap op uw cluster hebt geïmplementeerd en u deze wilt bijwerken met een nieuwere configuratie, kunt u gewoon het ConfigMap-bestand dat u eerder hebt gebruikt, bewerken en vervolgens op dezelfde opdracht Toep assen als `kubectl apply -f <configmap_yaml_file.yaml`voorheen.

Het kan een paar minuten duren voordat de configuratie wijziging is doorgevoerd en alle omsagent in het cluster opnieuw worden opgestart. Het opnieuw opstarten is een rolling start voor alle omsagent-peulen, niet allemaal tegelijk opnieuw opstarten. Wanneer het opnieuw opstarten is voltooid, wordt een bericht weer gegeven dat er ongeveer als volgt uitziet en het resultaat bevat `configmap "container-azm-ms-agentconfig" updated`:.

## <a name="verifying-schema-version"></a>De schema versie wordt gecontroleerd

Ondersteunde configuratie schema versies zijn beschikbaar als pod-aantekening (schema-versies) in de omsagent-pod. U kunt deze weer geven met de volgende kubectl-opdracht:`kubectl describe pod omsagent-fdf58 -n=kube-system`

De uitvoer ziet er ongeveer als volgt uit met het aantekening schema-versies:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="review-prometheus-data-usage"></a>Prometheus-gegevens gebruik controleren

De volgende query wordt gegeven voor het identificeren van het opname volume van elke metrische grootte in GB per dag om te begrijpen of deze hoog is.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
De uitvoer ziet er ongeveer als volgt uit:

![Query resultaten van het volume gegevens opname vastleggen](./media/container-insights-agent-config/log-query-example-usage-03.png)

Als u een schatting wilt maken van de grootte van elke metrische waarde in GB, kunt u de volgende query uitvoeren om te begrijpen of het volume van de gegevens die in de werk ruimte zijn opgenomen, hoog is.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize EstimatedGBPer30dayMonth = (sum(_BilledSize) / (1024 * 1024 * 1024)) * 30 by Name
| order by EstimatedGBPer30dayMonth desc
| render barchart
```

De uitvoer ziet er ongeveer als volgt uit:

![Query resultaten van het volume gegevens opname vastleggen](./media/container-insights-agent-config/log-query-example-usage-02.png)

Meer informatie over het bewaken van het gebruik van gegevens en het analyseren van de kosten is beschikbaar in het [beheer van gebruik en kosten met Azure monitor](../platform/manage-cost-storage.md)-Logboeken.

## <a name="next-steps"></a>Volgende stappen

Azure Monitor voor containers bevat geen vooraf gedefinieerde set met waarschuwingen. Raadpleeg de procedures [voor het maken van prestatie waarschuwingen met Azure monitor voor containers](container-insights-alerts.md) voor meer informatie over het maken van aanbevolen waarschuwingen voor hoog CPU-en geheugen gebruik ter ondersteuning van uw DevOps-of operationele processen en procedures.

- Om door te gaan met het leren over het gebruik van Azure Monitor en controleren van andere aspecten van uw AKS-cluster [weergave Azure Kubernetes Service health](container-insights-analyze.md).

- Bekijk de [voor beelden van logboek query's](container-insights-log-search.md#search-logs-to-analyze-data) om vooraf gedefinieerde query's en voor beelden te bekijken voor het evalueren of aanpassen van waarschuwingen, het visualiseren of analyseren van uw clusters.