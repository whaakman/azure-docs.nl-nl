---
title: Intelligente routering en canary releases met Istio in Azure Kubernetes Service (AKS)
description: Informatie over het gebruik van Istio bieden een intelligente Routering en canary releases in een Azure Kubernetes Service (AKS)-cluster implementeren
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: 0a4e5e7e310a9949ee59291c2032eafda46955a9
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892223"
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

De stappen die worden beschreven in dit artikel wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt (Kubernetes 1.10 en hoger, met RBAC ingeschakeld) en tot stand hebt gebracht een `kubectl` verbinding met het cluster. U moet ook Istio geïnstalleerd in uw cluster.

Als u hulp nodig met een van deze items, Zie de [Quick Start voor AKS] [ aks-quickstart] en [Istio installeren in AKS][istio-install].

## <a name="about-this-application-scenario"></a>Over dit toepassingsscenario

Het voorbeeld AKS stem-app biedt twee stemopties (katten of honden) voor gebruikers. Er is een onderdeel van opslag die het aantal stemmen voor elke optie. Daarnaast is er een analytics-onderdeel waarmee u meer informatie over de stemmen voor de cast-conversie uitvoeren voor elke optie.

In dit artikel, begint u door het implementeren van versie *1.0* van het stem-app en de versie *1.0* van het onderdeel analytics. De component analytics biedt een eenvoudige telt het aantal voor het aantal stemmen. De stem-app en analyse-onderdelen met elkaar communiceren met versie *1.0* van het opslagonderdeel, die wordt ondersteund door Redis.

Upgrade van het onderdeel analytics naar versie *1.1*, die aantallen biedt, en nu totalen en percentages.

Een subset van gebruikers testversie *2.0* van de app via een canary-release. Deze nieuwe versie maakt gebruik van een onderdeel van opslag die wordt ondersteund door een MySQL-database.

Bent u er zeker van zijn dat versie *2.0* werkt zoals verwacht op uw subset van gebruikers, die u implementeert versie *2.0* naar al uw gebruikers.

## <a name="deploy-the-application"></a>De toepassing implementeren

Begin met het implementeren van de toepassing in uw cluster Azure Kubernetes Service (AKS). Het volgende diagram toont wat wordt uitgevoerd door het einde van deze sectie - versie *1.0* van alle onderdelen met binnenkomende aanvragen verwerkt via de gateway van de inkomende Istio:

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-01.png)

De artefacten die u moet volgen beschreven in dit artikel zijn beschikbaar in de [Azure-Samples/aks-stem-app] [ github-azure-sample] GitHub-opslagplaats. U kunt downloaden van de artefacten of kloon de opslagplaats als volgt:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

Ga naar de volgende map in de gedownloade / gekloonde opslagplaats en alle volgende stappen uitvoeren vanuit deze map:

```console
cd scenarios/intelligent-routing-with-istio
```

Maak eerst een naamruimte in uw AKS-cluster voor het voorbeeld AKS stem-app met de naam *stemmende* als volgt:

```console
kubectl create namespace voting
```

De naamruimte met een label `istio-injection=enabled`. Dit label geïnstrueerd Istio naar de istio-proxy's automatisch invoeren als sidecars in al uw schillen in deze naamruimte.

```console
kubectl label namespace voting istio-injection=enabled
```

Nu gaan we de onderdelen voor de AKS Voting-app maken. Maken van deze onderdelen in de *stemmende* naamruimte in de vorige stap hebt gemaakt.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u dat de resources zijn gemaakt:

```
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

```console
kubectl get pods -n voting
```

De volgende voorbeelduitvoer ziet er zijn drie exemplaren van de *stem-app* pod en één exemplaar van zowel de *stem-analytics* en *stem-opslag* schillen. Elk van de schillen heeft twee containers. Een van deze containers is het onderdeel en de andere is het *istio-proxy*:

```
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-669f99dcc8-lzh7k   2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-bdmld         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-gcrng         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-strzc         2/2       Running   0          1m
voting-storage-1-0-7954799d96-5fv9r     2/2       Running   0          1m
```

Gegevens over de schil wilt bekijken, gebruikt u de [kubectl beschrijven pod][kubectl-describe]. De naam van de schil vervangen door de naam van een schil in uw eigen AKS-cluster uit de vorige uitvoer:

```console
kubectl describe pod voting-app-1-0-6c65c4bdd4-bdmld --namespace voting
```

De *istio-proxy* container is automatisch is toegevoegd door Istio voor het beheren van het netwerkverkeer naar en van de onderdelen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.0.4
[...]
```

U geen verbinding maken met de stem-app totdat u de Istio [Gateway] [ istio-reference-gateway] en [Virtual Service][istio-reference-virtualservice]. Deze resources Istio routeren van verkeer van de gateway standaard Istio inkomend verkeer naar de toepassing.

> [!NOTE]
> Een *Gateway* is een onderdeel aan de rand van het service-Net die binnenkomend of uitgaand HTTP- en TCP-verkeer ontvangt.
>
> Een *Virtual Service* definieert een reeks routeringsregels voor een of meer doel-services.

Gebruik de `istioctl` client binaire de yaml-Gateway en Virtual Service implementeren. Net als bij de `kubectl apply` opdracht, houd er rekening mee om op te geven van de naamruimte die deze resources worden geïmplementeerd in.

```console
istioctl create -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Haal de IP-adres van de Istio Ingress-Gateway met de volgende opdracht:

```console
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

De volgende voorbeelduitvoer ziet u het IP-adres van de binnenkomende Gateway:

```
52.187.250.239
```

Open een browser en plak in het IP-adres. Het voorbeeld AKS stem-app wordt weergegeven.

![De AKS stem-app uitgevoerd in onze Istio ingeschakeld AKS-cluster.](media/istio/deploy-app-01.png)

De informatie aan de onderkant van het scherm wordt weergegeven dat de app gebruikmaakt van versie *1.0* van de *stem-app* en versie *1.0* (Redis) als de opslagoptie voor.

## <a name="update-the-application"></a>De toepassing bijwerken

We gaan we een nieuwe versie van het onderdeel analytics implementeren. Deze nieuwe versie *1.1* totalen en percentages naast het aantal voor elke categorie worden weergegeven.

Het volgende diagram toont wat wordt uitgevoerd aan het einde van deze sectie - alleen versie *1.1* van onze *stem-analytics* onderdeel heeft gegevensverkeer van de *stem-app* het onderdeel. Hoewel versie *1.0* van onze *stem-analytics* onderdeel blijft actief en wordt verwezen door de *stem-analytics* service, de Istio-proxy's niet toestaan van verkeer vanuit en naar deze.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-02.png)

We gaan implementeren versie *1.1* van de *stem-analytics* onderdeel. Maken van dit onderdeel in de *stemmende* naamruimte:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Open het voorbeeld AKS stem-app in een browser, met behulp van het IP-adres van de Gateway van de inkomende Istio hebt verkregen in de vorige stap.

Uw browser wisselt tussen de twee weergaven die hieronder wordt weergegeven. Omdat u een Kubernetes [Service] [ kubernetes-service] voor de *stem-analytics* onderdeel met alleen een enkelvoudige selector (`app: voting-analytics`), Kubernetes gebruikmaakt van de gedrag van round robin tussen de pods die overeenkomen met die selector. In dit geval is het zowel versie *1.0* en *1.1* van uw *stem-analytics* schillen.

![Versie 1.0 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/deploy-app-01.png)

![Versie 1.1 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/update-app-01.png)

U kunt visualiseren het schakelen tussen de twee versies van de *stem-analytics* onderdeel als volgt te werk. Houd er rekening mee te gebruiken van het IP-adres van de Gateway van uw eigen Istio inkomend verkeer.

```console
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
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

Nu gaan we vergrendelen verkeer naar alleen versie *1.1* van de *stem-analytics* onderdelen en versie *1.0* van de *stem-opslag* het onderdeel. Definieer regels voor doorsturen voor alle andere onderdelen.

> * Een *Virtual Service* definieert een reeks routeringsregels voor een of meer doel-services.
> * Een *doelregel* definieert specifieke beleidsregels voor verkeer en versie.
> * Een *beleid* wordt gedefinieerd welke verificatiemethoden kunnen worden geaccepteerd op workload(s).

U gebruikt de `istioctl` client binaire ter vervanging van de virtuele servicedefinitie op uw *stem-app* en toe te voegen [bestemming regels] [ istio-reference-destinationrule] en [ Virtuele Services] [ istio-reference-virtualservice] voor de andere onderdelen.

U ook toevoegen een [beleid] [ istio-reference-policy] naar de *stemmende* naamruimte om ervoor te zorgen dat alle communicatie tussen services is beveiligd met wederzijdse TLS- en clientcertificaten.

Er is een bestaande virtuele-servicedefinitie voor *stem-app* vervangen uit te voeren, gebruikt u de `istioctl replace` opdracht als volgt:

```console
istioctl replace -f istio/step-2a-update-voting-app-virtualservice.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u Virtual Service Istio zijn bijgewerkt:

```
Updated config virtual-service/voting/voting-app to revision 141902
```

Gebruik vervolgens de `istioctl create` opdracht voor het toevoegen van het nieuwe beleid en ook de nieuwe doel-regels en Virtual Services voor alle andere onderdelen.

* Het beleid heeft `peers.mtls.mode` ingesteld op `STRICT` om ervoor te zorgen dat wederzijdse TLS wordt afgedwongen tussen uw services binnen de *stemmende* naamruimte.
* U ook instellen de `trafficPolicy.tls.mode` naar `ISTIO_MUTUAL` in alle regels voor onze bestemming. Istio biedt services met sterke identiteiten en de communicatie tussen services met behulp van wederzijdse TLS- en clientcertificaten die transparant beheerd door Istio beveiligt.

```console
istioctl create -f istio/step-2b-add-routing-for-all-components.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u het nieuwe beleid, doel-regels, en virtuele-Services zijn gemaakt:

```
Created config policy/voting/default to revision 142118
Created config destination-rule/voting/voting-app at revision 142119
Created config destination-rule/voting/voting-analytics at revision 142120
Created config virtual-service/voting/voting-analytics at revision 142121
Created config destination-rule/voting/voting-storage at revision 142122
Created config virtual-service/voting/voting-storage at revision 142123
```

Als u de AKS stem-app in opnieuw een browser alleen de nieuwe versie openen *1.1* van de *stem-analytics* onderdeel wordt gebruikt door de *stem-app* onderdeel.

![Versie 1.1 van de analytics-component die wordt uitgevoerd in onze AKS Voting-app.](media/istio/update-app-01.png)

Eenvoudiger kunt u visualiseren dat we, nu alleen worden gerouteerd naar versie *1.1* van uw *stem-analytics* onderdeel als volgt te werk. Houd er rekening mee te gebruiken van het IP-adres van de gateway van uw Istio inkomend verkeer.

U kunt visualiseren dat u nu alleen doorgestuurd naar versie *1.1* van uw *stem-analytics* onderdeel als volgt te werk. Moet u het IP-adres van de Gateway van uw eigen Istio inkomend gebruiken:

```azurecli-interactive
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

De volgende voorbeelduitvoer ziet u het relevante deel van de geretourneerde website:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Bevestig dat Istio maakt gebruik van wederzijdse TLS voor veilige communicatie tussen elk van onze services. De volgende opdrachten Controleer de TLS-instellingen voor elk van de *stem-app* services:

```console
istioctl authn tls-check voting-app.voting.svc.cluster.local
istioctl authn tls-check voting-analytics.voting.svc.cluster.local
istioctl authn tls-check voting-storage.voting.svc.cluster.local
```

De volgende voorbeelduitvoer ziet u dat wederzijdse TLS wordt afgedwongen voor elk van de services via het beleid en regels voor bestemming:

```
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Implementatie van een canary-release van de toepassing

Nu gaan we een nieuwe versie implementeren *2.0* van de *stem-app*, *stem-analytics*, en *stem-opslag* onderdelen. De nieuwe *stem-opslag* onderdeel MySQL gebruiken in plaats van Redis en de *stem-app* en *stem-analytics* onderdelen worden bijgewerkt zodat ze deze nieuwe gebruiken*stem-opslag* onderdeel.

De *stem-app* -onderdeel ondersteunt nu de vlag-functionaliteit. De functievlag voor deze kunt u de mogelijkheid canary-release van Istio voor een subset van gebruikers testen.

Het volgende diagram toont wat aan het einde van deze sectie wordt uitgevoerd.

* Versie *1.0* van de *stem-app* onderdeel, versie *1.1* van de *stem-analytics* onderdeel en versie  *1.0* van de *stem-opslag* onderdeel zijn met elkaar kunnen communiceren.
* Versie *2.0* van de *stem-app* onderdeel, versie *2.0* van de *stem-analytics* onderdeel en versie  *2.0* van de *stem-opslag* onderdeel zijn met elkaar kunnen communiceren.
* Versie *2.0* van de *stem-app* onderdeel zijn alleen toegankelijk voor gebruikers die beschikken over een specifieke functie vlag is ingesteld. Deze wijziging wordt beheerd met behulp van een functievlag via een cookie.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-03.png)

Eerst de Istio bestemming regels en de virtuele-Services die geschikt zijn voor deze nieuwe onderdelen bijwerken. Deze updates zorgen ervoor dat u geen verkeer onjuist naar de nieuwe onderdelen doorsturen en gebruikers geen onverwachte toegang krijgen:

```console
istioctl replace -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u dat de doel-regels en Virtual Services zijn bijgewerkt:

```
Updated config destination-rule/voting/voting-app to revision 150930
Updated config virtual-service/voting/voting-app to revision 150931
Updated config destination-rule/voting/voting-analytics to revision 150937
Updated config virtual-service/voting/voting-analytics to revision 150939
Updated config destination-rule/voting/voting-storage to revision 150940
Updated config virtual-service/voting/voting-storage to revision 150941
```

Vervolgens gaan we de Kubernetes-objecten voor de nieuwe versie toevoegen *2.0* onderdelen. U ook bijwerken de *stem-opslag* service om op te nemen de *3306* poort voor MySQL:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

De volgende voorbeelduitvoer ziet u de Kubernetes-objecten zijn bijgewerkt of gemaakt:

```
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Wachten totdat alle versie *2.0* schillen uitgevoerd. Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht voor het weergeven van alle schillen in de *stemmende* naamruimte:

```azurecli-interactive
kubectl get pods --namespace voting
```

U zou nu moeten kunnen schakelen tussen de versie *1.0* en versie *2.0* (canary) van de stemtoepassing. De functie vlag wisselknop aan de onderkant van het scherm stelt een cookie. Dankzij deze cookie wordt gebruikt door de *stem-app* Virtual Service voor gebruikers van de route naar de nieuwe versie *2.0*.

![Versie 1.0 van de AKS stem-app - functievlag IS niet ingesteld.](media/istio/canary-release-01.png)

![Versie 2.0 van de AKS stem-app - functie vlag IS ingesteld.](media/istio/canary-release-02.png)

Het aantal stemmen verschillen tussen de versies van de app. Dit verschil ziet u dat u van twee verschillende opslag back-ends gebruikmaakt.

## <a name="finalize-the-rollout"></a>De implementatie te voltooien

Zodra u hebt de canary-release is getest, werken de *stem-app* Virtual Service voor het routeren van al het verkeer naar versie *2.0* van de *stem-app* onderdeel. Alle gebruikers zien dan versie *2.0* van de toepassing, ongeacht of de functievlag is ingesteld of niet:

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-04.png)

Alle bestemming regels als u wilt verwijderen van de versies van de onderdelen die u niet meer wilt active bijwerken. Werk vervolgens de virtuele Services om te stoppen die verwijzen naar deze versies.

Omdat er niet meer al het verkeer naar een van de oudere versies van de onderdelen, kunt u de alle implementaties voor deze onderdelen nu veilig verwijderen.

![De AKS stem-app-onderdelen en routering.](media/istio/components-and-routing-05.png)

U hebt nu is een nieuwe versie van de AKS Voting-App hebt geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

U vindt aanvullende scenario's met behulp van de [Istio Bookinfo toepassing voorbeeld][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/spec-requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#Gateway
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#VirtualService
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#DestinationRule
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
