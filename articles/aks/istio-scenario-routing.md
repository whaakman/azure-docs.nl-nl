---
title: Intelligente routering en canary releases met Istio in Azure Kubernetes Service (AKS)
description: Informatie over het gebruik van Istio bieden een intelligente Routering en canary releases in een Azure Kubernetes Service (AKS)-cluster implementeren
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: bd660a2b6ffb96478c3170cc7013ff22518b758f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702210"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Gebruik intelligente Routering en canary releases met Istio in Azure Kubernetes Service (AKS)

[Istio] [ istio-github] is een open-source-service Net die een belangrijke set functionaliteit voor de microservices in een Kubernetes-cluster biedt. Deze functies omvatten beheer van verkeer, service-identiteit en beveiliging afdwingen van beleid en observability. Zie voor meer informatie over Istio, de officiële [wat is er Istio?] [ istio-docs-concepts] documentatie.

In dit artikel leest u hoe u de functionaliteit voor het beheer van verkeer van Istio. Een voorbeeld van AKS stem-app wordt gebruikt om te verkennen en canary releases van intelligente routering.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De toepassing implementeren
> * De toepassing bijwerken
> * Implementatie van een canary-release van de toepassing
> * De implementatie te voltooien

## <a name="before-you-begin"></a>Voordat u begint

> [!NOTE]
> In dit scenario is getest met Istio versie `1.1.3`.

De stappen die worden beschreven in dit artikel wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt (Kubernetes `1.11` en hoger, met RBAC ingeschakeld) en tot stand hebt gebracht een `kubectl` verbinding met het cluster. U moet ook Istio geïnstalleerd in uw cluster.

Als u hulp nodig met een van deze items, Zie de [Quick Start voor AKS] [ aks-quickstart] en [Istio installeren in AKS] [ istio-install] richtlijnen.

## <a name="about-this-application-scenario"></a>Over dit toepassingsscenario

Het voorbeeld AKS stem-app biedt twee opties voor stemmen (**katten** of **honden**) voor gebruikers. Er is een onderdeel van opslag die het aantal stemmen voor elke optie. Daarnaast is er een analytics-onderdeel waarmee u meer informatie over de stemmen voor de cast-conversie uitvoeren voor elke optie.

In dit toepassingsscenario u beginnen met het implementeren van versie `1.0` van het stem-app en de versie `1.0` van het onderdeel analytics. De component analytics biedt een eenvoudige telt het aantal voor het aantal stemmen. De stem-app en analyse-onderdelen met elkaar communiceren met versie `1.0` van het opslagonderdeel, die wordt ondersteund door Redis.

Upgrade van het onderdeel analytics naar versie `1.1`, die aantallen biedt, en nu totalen en percentages.

Een subset van gebruikers testversie `2.0` van de app via een canary-release. Deze nieuwe versie maakt gebruik van een onderdeel van opslag die wordt ondersteund door een MySQL-database.

Bent u er zeker van zijn dat versie `2.0` werkt zoals verwacht op uw subset van gebruikers, die u implementeert versie `2.0` naar al uw gebruikers.

## <a name="deploy-the-application"></a>De toepassing implementeren

Begin met het implementeren van de toepassing in uw cluster Azure Kubernetes Service (AKS). Het volgende diagram toont wat wordt uitgevoerd door het einde van deze sectie - versie `1.0` van alle onderdelen met binnenkomende aanvragen verwerkt via de gateway van de inkomende Istio:

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-01.png)

De artefacten die u moet volgen beschreven in dit artikel zijn beschikbaar in de [Azure-Samples/aks-stem-app] [ github-azure-sample] GitHub-opslagplaats. U kunt downloaden van de artefacten of kloon de opslagplaats als volgt:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

Ga naar de volgende map in de gedownloade / gekloonde opslagplaats en alle volgende stappen uitvoeren vanuit deze map:

```console
cd scenarios/intelligent-routing-with-istio
```

Maak eerst een naamruimte in uw AKS-cluster voor het voorbeeld AKS stem-app met de naam `voting` als volgt:

```azurecli
kubectl create namespace voting
```

De naamruimte met een label `istio-injection=enabled`. Dit label geïnstrueerd Istio naar de istio-proxy's automatisch invoeren als sidecars in al uw schillen in deze naamruimte.

```azurecli
kubectl label namespace voting istio-injection=enabled
```

Nu gaan we de onderdelen voor de AKS Voting-app maken. Maken van deze onderdelen in de `voting` naamruimte in de vorige stap hebt gemaakt.

```azurecli
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de resources die worden gemaakt:

```console
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> Istio heeft enkele specifieke vereisten met betrekking schillen en -services. Zie voor meer informatie de [Istio vereisten voor documentatie over schillen en Services][istio-requirements-pods-and-services].

Als u wilt zien van de pods die zijn gemaakt, gebruikt u de [kubectl ophalen schillen] [ kubectl-get] opdracht als volgt:

```azurecli
kubectl get pods -n voting
```

De volgende voorbeelduitvoer ziet er zijn drie exemplaren van de `voting-app` pod en één exemplaar van zowel de `voting-analytics` en `voting-storage` schillen. Elk van de schillen heeft twee containers. Een van deze containers is het onderdeel en de andere is het `istio-proxy`:

```console
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s
```

Gegevens over de schil wilt bekijken, gebruikt u de [kubectl beschrijven pod][kubectl-describe]. De naam van de schil vervangen door de naam van een schil in uw eigen AKS-cluster uit de vorige uitvoer:

```azurecli
kubectl describe pod voting-app-1-0-956756fd-d5w7z --namespace voting
```

De `istio-proxy` container is automatisch is toegevoegd door Istio voor het beheren van het netwerkverkeer naar en van de onderdelen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.1.3
[...]
```

U geen verbinding maken met de stem-app totdat u de Istio [Gateway] [ istio-reference-gateway] en [Virtual Service][istio-reference-virtualservice]. Deze resources Istio routeren van verkeer van de gateway standaard Istio inkomend verkeer naar de toepassing.

> [!NOTE]
> Een **Gateway** is een onderdeel aan de rand van het service-Net die binnenkomend of uitgaand HTTP- en TCP-verkeer ontvangt.
> 
> Een **Virtual Service** definieert een reeks routeringsregels voor een of meer doel-services.

Gebruik de `kubectl apply` opdracht voor het implementeren van de Gateway en Virtual Service yaml. Vergeet niet om op te geven van de naamruimte die deze resources worden geïmplementeerd in.

```azurecli
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de nieuwe Gateway en Virtual-Service wordt gemaakt:

```console
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Haal de IP-adres van de Istio Ingress-Gateway met de volgende opdracht:

```azurecli
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

De volgende voorbeelduitvoer ziet u het IP-adres van de binnenkomende Gateway:

```
20.188.211.19
```

Open een browser en plak in het IP-adres. Het voorbeeld AKS stem-app wordt weergegeven.

![De AKS stem-app uitgevoerd in onze Istio ingeschakeld AKS-cluster.](media/istio/deploy-app-01.png)

De informatie aan de onderkant van het scherm wordt weergegeven dat de app gebruikmaakt van versie `1.0` van `voting-app` en versie `1.0` van `voting-storage` (Redis).

## <a name="update-the-application"></a>De toepassing bijwerken

We gaan we een nieuwe versie van het onderdeel analytics implementeren. Deze nieuwe versie `1.1` totalen en percentages naast het aantal voor elke categorie worden weergegeven.

Het volgende diagram toont wat zal worden uitgevoerd aan het einde van deze sectie - alleen versie `1.1` van onze `voting-analytics` onderdeel heeft gegevensverkeer van de `voting-app` onderdeel. Hoewel versie `1.0` van onze `voting-analytics` onderdeel blijft actief en wordt verwezen door de `voting-analytics` service, de Istio-proxy's niet toestaan van verkeer van en naar deze.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-02.png)

We gaan implementeren versie `1.1` van de `voting-analytics` onderdeel. Maken van dit onderdeel in de `voting` naamruimte:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de resources die worden gemaakt:

```console
deployment.apps/voting-analytics-1-1 created
```

Open het voorbeeld AKS stem-app in een browser, met behulp van het IP-adres van de Gateway van de inkomende Istio hebt verkregen in de vorige stap.

Uw browser wisselt tussen de twee weergaven die hieronder wordt weergegeven. Omdat u een Kubernetes [Service] [ kubernetes-service] voor de `voting-analytics` onderdeel met alleen een enkelvoudige selector (`app: voting-analytics`), Kubernetes maakt gebruik van het standaardgedrag van round robin tussen de schillen die overeenkomen met die selector. In dit geval is het zowel versie `1.0` en `1.1` van uw `voting-analytics` schillen.

![Versie 1.0 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/deploy-app-01.png)

![Versie 1.1 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/update-app-01.png)

U kunt visualiseren het schakelen tussen de twee versies van de `voting-analytics` onderdeel als volgt te werk. Houd er rekening mee te gebruiken van het IP-adres van de Gateway van uw eigen Istio inkomend verkeer.

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

De volgende voorbeelduitvoer ziet u het relevante deel van de geretourneerde website als de site-switches tussen versies:

```
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Verkeer naar versie 1.1 van de toepassing

Nu gaan we vergrendelen verkeer naar alleen versie `1.1` van de `voting-analytics` onderdelen en versie `1.0` van de `voting-storage` onderdeel. Definieer regels voor doorsturen voor alle andere onderdelen.

> * Een **Virtual Service** definieert een reeks routeringsregels voor een of meer doel-services.
> * Een **doelregel** definieert specifieke beleidsregels voor verkeer en versie.
> * Een **beleid** wordt gedefinieerd welke verificatiemethoden kunnen worden geaccepteerd op workload(s).

Gebruik de `kubectl apply` opdracht uit om te vervangen door de definitie van de virtuele-Service op uw `voting-app` en toe te voegen [bestemming regels] [ istio-reference-destinationrule] en [Virtual Services] [ istio-reference-virtualservice] voor de andere onderdelen. Voegt u een [beleid] [ istio-reference-policy] naar de `voting` naamruimte om ervoor te zorgen dat alle communicatie tussen services is beveiligd met wederzijdse TLS- en clientcertificaten.

* Het beleid heeft `peers.mtls.mode` ingesteld op `STRICT` om ervoor te zorgen dat wederzijdse TLS wordt afgedwongen tussen uw services binnen de `voting` naamruimte.
* Stellen we de `trafficPolicy.tls.mode` naar `ISTIO_MUTUAL` in alle regels voor onze bestemming. Istio biedt services met sterke identiteiten en de communicatie tussen services met behulp van wederzijdse TLS- en clientcertificaten die transparant beheerd door Istio beveiligt.

```azurecli
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u het nieuwe beleid, doel regels en Virtual Services wordt bijgewerkt/gemaakt:

```console
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

Als u de AKS Voting-app in opnieuw een browser alleen de nieuwe versie openen `1.1` van de `voting-analytics` onderdeel wordt gebruikt door de `voting-app` onderdeel.

![Versie 1.1 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/update-app-01.png)

U kunt visualiseren dat u nu alleen doorgestuurd naar versie `1.1` van uw `voting-analytics` onderdeel als volgt te werk. Moet u het IP-adres van de Gateway van uw eigen Istio inkomend gebruiken:

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

De volgende voorbeelduitvoer ziet u het relevante deel van de geretourneerde website:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Laten we nu bevestigen dat Istio wederzijdse TLS wordt gebruikt voor het beveiligen van communicatie tussen elk van onze services. Hiervoor gebruiken we de [authn tls-controle] [ istioctl-authn-tls-check] opdracht op de `istioctl` client binaire, de volgende vorm.

```console
istioctl authn tls-check <pod-name[.namespace]> [<service>]
```

Deze reeks opdrachten bevatten informatie over de toegang aan de opgegeven services van alle schillen die zich in een naamruimte en overeenkomen met een set van labels:

Bash

```bash
# mTLS configuration between each of the istio ingress pods and the voting-app service
kubectl get pod -n istio-system -l app=istio-ingressgateway | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.istio-system voting-app.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-analytics service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-analytics.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-storage service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
kubectl get pod -n voting -l app=voting-analytics,version=1.1 | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local
```

PowerShell

```powershell
# mTLS configuration between each of the istio ingress pods and the voting-app service
(kubectl get pod -n istio-system -l app=istio-ingressgateway | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".istio-system") voting-app.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-analytics service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-analytics.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-analytics,version=1.1 | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }
```

De volgende voorbeelduitvoer ziet u dat wederzijdse TLS wordt afgedwongen voor elk van de bovenstaande query's. De uitvoer ziet ook het beleid en de doel-regels die de wederzijdse TLS wordt afgedwongen:

```console
# mTLS configuration between istio ingress pods and the voting-app service
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

# mTLS configuration between each of the voting-app pods and the voting-analytics service
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

# mTLS configuration between each of the voting-app pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Implementatie van een canary-release van de toepassing

Nu gaan we een nieuwe versie implementeren `2.0` van de `voting-app`, `voting-analytics`, en `voting-storage` onderdelen. De nieuwe `voting-storage` onderdeel MySQL gebruiken in plaats van Redis en de `voting-app` en `voting-analytics` onderdelen worden bijgewerkt zodat ze met deze nieuwe `voting-storage` onderdeel.

De `voting-app` -onderdeel ondersteunt nu de vlag-functionaliteit. De functievlag voor deze kunt u de mogelijkheid canary-release van Istio voor een subset van gebruikers testen.

Het volgende diagram toont wat u moet aan het einde van deze sectie wordt uitgevoerd.

* Versie `1.0` van de `voting-app` onderdeel, versie `1.1` van de `voting-analytics` onderdeel en versie `1.0` van de `voting-storage` onderdeel zijn met elkaar kunnen communiceren.
* Versie `2.0` van de `voting-app` onderdeel, versie `2.0` van de `voting-analytics` onderdeel en versie `2.0` van de `voting-storage` onderdeel zijn met elkaar kunnen communiceren.
* Versie `2.0` van de `voting-app` onderdeel zijn alleen toegankelijk voor gebruikers die beschikken over een specifieke functie vlag is ingesteld. Deze wijziging wordt beheerd met behulp van een functievlag via een cookie.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-03.png)

Eerst de Istio bestemming regels en de virtuele-Services die geschikt zijn voor deze nieuwe onderdelen bijwerken. Deze updates zorgen ervoor dat u geen verkeer onjuist naar de nieuwe onderdelen doorsturen en gebruikers geen onverwachte toegang krijgen:

```azurecli
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de doel-regels en Virtual Services worden bijgewerkt:

```console
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

Vervolgens gaan we de Kubernetes-objecten voor de nieuwe versie toevoegen `2.0` onderdelen. U ook bijwerken de `voting-storage` service om op te nemen de `3306` poort voor MySQL:

```azurecli
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de Kubernetes-objecten zijn bijgewerkt of gemaakt:

```console
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Wachten totdat alle versie `2.0` schillen uitgevoerd. Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht voor het weergeven van alle schillen in de `voting` naamruimte:

```azurecli
kubectl get pods --namespace voting
```

U zou nu moeten kunnen schakelen tussen de versie `1.0` en versie `2.0` (canary) van de stemtoepassing. De functie vlag wisselknop aan de onderkant van het scherm stelt een cookie. Dankzij deze cookie wordt gebruikt door de `voting-app` Virtual Service voor gebruikers van de route naar de nieuwe versie `2.0`.

![Versie 1.0 van de AKS stem-app - functievlag IS niet ingesteld.](media/istio/canary-release-01.png)

![Versie 2.0 van de AKS stem-app - functie vlag IS ingesteld.](media/istio/canary-release-02.png)

Het aantal stemmen verschillen tussen de versies van de app. Dit verschil ziet u dat u van twee verschillende opslag back-ends gebruikmaakt.

## <a name="finalize-the-rollout"></a>De implementatie te voltooien

Nadat u hebt de canary-release is getest, werken de `voting-app` Virtual Service voor het routeren van al het verkeer naar versie `2.0` van de `voting-app` onderdeel. Alle gebruikers zien dan versie `2.0` van de toepassing, ongeacht of de functievlag is ingesteld of niet:

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-04.png)

Alle bestemming regels als u wilt verwijderen van de versies van de onderdelen die u niet meer wilt active bijwerken. Werk vervolgens de virtuele Services om te stoppen die verwijzen naar deze versies.

Omdat er niet meer al het verkeer naar een van de oudere versies van de onderdelen, kunt u de alle implementaties voor deze onderdelen nu veilig verwijderen.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-05.png)

U hebt nu is een nieuwe versie van de AKS Voting-App hebt geïmplementeerd.

## <a name="clean-up"></a>Opruimen 

U kunt de AKS stem-app wordt gebruikt in dit scenario van uw AKS-cluster verwijderen door het verwijderen van de `voting` naamruimte als volgt te werk:

```azurecli
kubectl delete namespace voting
```

De volgende voorbeelduitvoer ziet u dat alle onderdelen van het AKS stem-app zijn verwijderd uit uw AKS-cluster.

```console
namespace "voting" deleted
```

## <a name="next-steps"></a>Volgende stappen

U vindt aanvullende scenario's met behulp van de [Istio Bookinfo toepassing voorbeeld][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio-github]: https://github.com/istio/istio

[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/prepare/requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/networking/v1alpha3/gateway/
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[istioctl-authn-tls-check]: https://istio.io/docs/reference/commands/istioctl/#istioctl-authn-tls-check

[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
