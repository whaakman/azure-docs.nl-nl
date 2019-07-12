---
title: Een zelfstandig Prometheus exemplaar in een Azure Red Hat OpenShift-cluster implementeren | Microsoft Docs
description: Maakt een exemplaar Prometheus in een Azure Red Hat OpenShift-cluster voor het bewaken van metrische gegevens van uw toepassing.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, aro, openshift, metrische gegevens, red hat
ms.openlocfilehash: a9748932a72106413677b21fe0efd1f69fb02e47
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827018"
---
# <a name="deploy-a-standalone-prometheus-instance-in-an-azure-red-hat-openshift-cluster"></a>Een zelfstandig Prometheus exemplaar in een Azure Red Hat OpenShift-cluster implementeren

In dit artikel wordt beschreven hoe u een zelfstandige Prometheus exemplaar die gebruikmaakt van servicedetectie in een Azure Red Hat OpenShift-cluster configureren.

> [!NOTE]
> Klant-beheerderstoegang tot Azure Red Hat OpenShift-cluster is niet vereist.

Setup van doel:

- Een project (prometheus-project), die Prometheus en Alertmanager bevat.
- Twee projecten (app-project1 en app-project2), waarin de toepassingen te bewaken.

U zult enkele Prometheus config-bestanden lokaal voorbereiden. Maak een nieuwe map voor het opslaan van deze. Configuratiebestanden worden opgeslagen in het cluster als geheimen, in het geval geheime tokens later worden toegevoegd aan het cluster.

## <a name="sign-in-to-the-cluster-by-using-the-oc-tool"></a>Meld u aan het cluster met behulp van het hulpprogramma OC

1. Open een webbrowser en ga vervolgens naar de web-console van het cluster (https://openshift. *willekeurige-id*. *regio*. azmosa.io).
2. Meld u aan met uw Azure-referenties.
3. Selecteer uw gebruikersnaam in de rechterbovenhoek en selecteer vervolgens **kopie aanmeldingsopdracht**.
4. Plak uw gebruikersnaam in de terminal die u gaat gebruiken.

> [!NOTE]
> Als u wilt zien als u bent aangemeld met het juiste cluster, de `oc whoami -c` opdracht.

## <a name="prepare-the-projects"></a>Voorbereiden van de projecten

Voer de volgende opdrachten voor het maken van de projecten:
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Kunt u ofwel de `-n` of `--namespace` parameter of Selecteer een actieve project door het uitvoeren van de `oc project` opdracht.

## <a name="prepare-the-prometheus-configuration-file"></a>Het configuratiebestand Prometheus voorbereiden
Maak een bestand prometheus.yml door in te voeren van de volgende inhoud:
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Maak een geheim Prom aangeroepen door in te voeren van de volgende configuratie:
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Het bestand prometheus.yml is een eenvoudige Prometheus-configuratiebestand. Hiermee stelt u het interval en configureert deze automatische detectie in drie de projecten (prometheus-project, app-project1, app-project2). In het vorige configuratiebestand van de eindpunten automatisch gedetecteerd zijn die is geëxtraheerd via HTTP zonder verificatie.

Zie voor meer informatie over eindpunten slijmen [Prometheus speciaal config](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config).


## <a name="prepare-the-alertmanager-config-file"></a>Het configuratiebestand Alertmanager voorbereiden
Maak een bestand alertmanager.yml door in te voeren van de volgende inhoud:
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Maak een geheim met de naam Prom waarschuwingen door te voeren van de volgende configuratie:
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Alertmanager.yml is het configuratiebestand Alert Manager.

> [!NOTE]
> Als u wilt controleren of de vorige twee stappen, voer de `oc get secret -n prometheus-project` opdracht.

## <a name="start-prometheus-and-alertmanager"></a>Prometheus en Alertmanager starten
Ga naar [openshift/oorspronkelijke opslagplaats](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) en download de [prometheus standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) sjabloon. De sjabloon toepassen op prometheus-project door op te geven van de volgende configuratie:
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
Het bestand prometheus standalone.yaml is een OpenShift-sjabloon. Een Prometheus-exemplaar wordt gemaakt met oauth-proxy vóór het en een exemplaar van Alertmanager, ook beveiligd met oauth-proxy. In deze sjabloon oauth-proxy is geconfigureerd, zodat elke gebruiker die u '' de naamruimte prometheus-project ophalen kunt (Zie de `-openshift-sar` vlag).

> [!NOTE]
> Uitvoeren om te controleren of de prom StatefulSet gelijk gewenst en het huidige aantal replica's heeft, de `oc get statefulset -n prometheus-project` opdracht. Uitvoeren om te controleren of alle resources in het project, de `oc get all -n prometheus-project` opdracht.

## <a name="add-permissions-to-allow-service-discovery"></a>Voeg machtigingen toe aan de servicedetectie toestaan

Maak een bestand prometheus sdrole.yml door in te voeren van de volgende inhoud:
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus service discovery role
    description: |
      Role and rolebinding added permissions required for service discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Als sjabloon wilt toepassen op alle projecten die u wilt toestaan servicedetectie, voer de volgende opdrachten:
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Als u wilt Prometheus voor het verzamelen van metrische gegevens van zichzelf, pas de machtigingen in prometheus-project.

> [!NOTE]
> Uitvoeren om te controleren of de rol en RoleBinding correct zijn gemaakt, de `oc get role` en `oc get rolebinding` opdrachten.

## <a name="optional-deploy-example-application"></a>Optioneel: Van de voorbeeldtoepassing implementeren

Alles goed werkt, maar er zijn geen gegevensbronnen metrische. Ga naar de URL Prometheus (https://prom-prometheus-project.apps. *willekeurige-id*. *regio*.azmosa.io/). U vindt deze met behulp van de volgende opdracht:

```
oc get route prom -n prometheus-project
```
> [!IMPORTANT]
> Vergeet niet het voorvoegsel https:// toevoegen aan begin van de hostnaam.

De **Status > Servicedetectie** pagina ziet u de actieve doelen 0/0.

Voor het implementeren van een voorbeeldtoepassing, waarmee wordt aangegeven dat Python basismetriek onder het eindpunt /metrics, voer de volgende opdrachten:
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1

oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
De nieuwe toepassingen die moeten worden weergegeven als geldige doelen op de pagina Servicedetectie binnen 30 seconden na de implementatie.

Voor meer informatie, selecteert u **Status** > **doelen**.

> [!NOTE]
> Voor elke is scraped doel voegt Prometheus een gegevenspunt in de van metrische gegevens. Selecteer **Prometheus** invoeren in de linkerbovenhoek **van** als de expressie, en selecteer vervolgens **Execute**.

## <a name="next-steps"></a>Volgende stappen

U kunt aangepaste Prometheus instrumentatie toevoegen aan uw toepassingen. De Prometheus Client-bibliotheek die Prometheus metrische gegevens voorbereiden vereenvoudigt, is gereed voor verschillende programmeertalen.

Zie voor meer informatie de volgende GitHub-bibliotheken:

 - [Java](https://github.com/prometheus/client_java)
 - [Python](https://github.com/prometheus/client_python)
 - [Go](https://github.com/prometheus/client_golang)
 - [Ruby](https://github.com/prometheus/client_ruby)
