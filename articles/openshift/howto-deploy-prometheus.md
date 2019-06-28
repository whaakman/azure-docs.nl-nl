---
title: Een zelfstandige Prometheus in een Azure Red Hat OpenShift-cluster implementeren | Microsoft Docs
description: Hier is het maken van een Prometheus-exemplaar op een Azure Red Hat OpenShift-cluster voor het bewaken van metrische gegevens van uw toepassing.
author: Makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, aro, openshift, metrische gegevens, red hat
ms.openlocfilehash: e66658151361edd43f61d398274c88c047928028
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342712"
---
# <a name="deploy-a-standalone-prometheus-in-an-azure-red-hat-openshift-cluster"></a>Een zelfstandige Prometheus in een Azure Red Hat OpenShift-cluster implementeren

Deze handleiding wordt beschreven hoe u een zelfstandige Prometheus configureren met servicedetectie in een cluster Azure Red Hat OpenShift.  "Klant" beheerderstoegang tot het cluster is niet nodig.

De installatie van het doel is als volgt:

- een project (prometheus-project), waarin Prometheus en Alertmanager
- twee projecten (app-project1 en app-project2), waarin de toepassingen te bewaken

U zult de configuratiebestanden van sommige Prometheus lokaal voorbereiden. Maak een nieuwe map voor het opslaan van deze.
Deze configuratiebestanden, opgeslagen in het cluster als geheimen in het geval geheime tokens worden toegevoegd aan deze later opnieuw.

## <a name="step-1-sign-in-to-the-cluster-using-the-oc-tool"></a>Stap 1: Meld u aan met het cluster via de `oc` hulpprogramma
Met behulp van een webbrowser, navigeer naar de web-console van het cluster (https://openshift. *willekeurige-id*. *regio*. azmosa.io).
Meld u aan met uw Azure-referenties.
Klik op uw gebruikersnaam in de rechterbovenhoek en selecteer 'Kopiëren aanmeldingsopdracht'. Plak deze in de terminal die u gebruikt.

U kunt controleren als u bent aangemeld met het juiste cluster met de `oc whoami -c` opdracht.

## <a name="step-2-prepare-the-projects"></a>Stap 2: Voorbereiden van de projecten

Projecten maken.
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Kunt u ofwel de `-n` of `--namespace` parameter of Selecteer een actieve project met de `oc project` opdracht

## <a name="step-3-prepare-prometheus-config"></a>Stap 3: Prometheus config voorbereiden
Maak een bestand met de naam prometheus.yml met de volgende inhoud.
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
Een geheim met de naam 'prom"met de configuratie maken.
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Het genoemde bestand is een eenvoudige Prometheus config-bestand.
Hiermee stelt u het interval en configureert deze automatische detectie in drie de projecten (prometheus-project, app-project1, app-project2).
In dit voorbeeld wordt het automatisch gedetecteerd eindpunten zal worden die is geëxtraheerd via HTTP zonder verificatie.
Zie voor meer informatie over het slijmen eindpunten https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config.


## <a name="step-4-prepare-alertmanager-config"></a>Stap 4: Alertmanager config voorbereiden
Maak een bestand met de naam alertmanager.yml met de volgende inhoud.
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
Een geheim met de naam 'prom-waarschuwingen' met de configuratie maken.
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Het genoemde bestand is het configuratiebestand Alert Manager.

> [!NOTE]
> U kunt controleren of de vorige twee stappen met `oc get secret -n prometheus-project`

## <a name="step-5-start-prometheus-and-alertmanager"></a>Stap 5: Prometheus en Alertmanager starten
Download de [prometheus standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) -sjabloon uit de [openshift/oorspronkelijke opslagplaats](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) en toe te passen in het prometheus-project
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
Het bestand prometheus standalone.yaml is een OpenShift-sjabloon, Hiermee u een Prometheus-exemplaar met een oauth-proxy vóór het en een exemplaar van Alertmanager maakt, ook beveiligd met oauth-proxy.  In deze sjabloon oauth-proxy is geconfigureerd, zodat elke gebruiker die u '' de naamruimte 'prometheus-project ophalen kunt' (Zie de `-openshift-sar` vlag).

> [!NOTE]
> U kunt controleren of "prom" StatefulSet gelijk heeft *DESIRED* en *huidige* aantal replica's met de `oc get statefulset -n prometheus-project` opdracht.
> U kunt ook alle resources in het project met controleren `oc get all -n prometheus-project`.

## <a name="step-6-add-permissions-to-allow-service-discovery"></a>Stap 6: Voeg machtigingen toe aan de Service-detectie toestaan
Prometheus sdrole.yml maken met de volgende inhoud.
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus Service Discovery Role
    description: |
      A role and rolebinding adding permissions required to perform Service Discovery in a given project.
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
De sjabloon toepassen op alle projecten waar u wilt toestaan van servicedetectie.
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Indien ook Prometheus gewenst kunnen metrische gegevens verzamelen uit zichzelf, moet u de machtigingen in prometheus-project te toe te passen.

> [!NOTE]
> U kunt controleren als de rol en de RoleBinding correct zijn gemaakt met de `oc get role` en `oc get rolebinding` respectievelijk-opdrachten

## <a name="optional-deploy-example-application"></a>Optioneel: Van de voorbeeldtoepassing implementeren
Alles goed werkt, maar er zijn geen gegevensbronnen metrische. Ga naar de URL Prometheus (https://prom-prometheus-project.apps. *willekeurige-id*. *regio*.azmosa.io/), die kan worden gevonden met de volgende opdracht.
```
oc get route prom -n prometheus-project
```
Houd er rekening mee aan het voorvoegsel van de hostnaam met https://.

De **Status > Servicedetectie** pagina ziet u de actieve doelen 0/0.

Welke metrische gegevens over eenvoudige python toont onder het eindpunt /metrics uitvoeren voor het implementeren van een voorbeeld van de toepassing, de volgende opdrachten.
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
De nieuwe toepassingen die worden weergegeven als geldige doelen op de pagina Servicedetectie binnen 30 s na de implementatie. U kunt meer informatie krijgen over de **Status > doelen** pagina.

> [!NOTE]
> Voor elke is scraped doel wordt Prometheus een gegevenspunt in de metriek 'tot' toegevoegd. Klik op **Prometheus** in de linkerbovenhoek hoek en voer 'van' als de expressie en op **Execute**.

## <a name="next-steps"></a>Volgende stappen

U kunt aangepaste Prometheus instrumentatie toevoegen aan uw toepassingen.

De Prometheus Client-bibliotheek die voorbereiden Prometheus metrische gegevens vereenvoudigt, is gereed voor verschillende programmeertalen.

 - Java https://github.com/prometheus/client_java
 - Python https://github.com/prometheus/client_python
 - Ga naar https://github.com/prometheus/client_golang
 - Ruby https://github.com/prometheus/client_ruby
