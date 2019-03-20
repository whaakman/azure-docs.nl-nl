---
title: Istio installeren in Azure Kubernetes Service (AKS)
description: Meer informatie over het installeren en gebruiken van Istio voor het maken van een service-Net in een cluster Azure Kubernetes Service (AKS)
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: d85b830b63e2d52f3eeb5df8645edccfccf43c76
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58138147"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Installeren en gebruiken van Istio in Azure Kubernetes Service (AKS)

[Istio] [ istio-github] is een open-source-service Net die een belangrijke set functionaliteit voor de microservices in een Kubernetes-cluster biedt. Deze functies omvatten beheer van verkeer, service-identiteit en beveiliging afdwingen van beleid en observability. Zie voor meer informatie over Istio, de officiële [wat is er Istio?] [ istio-docs-concepts] documentatie.

Dit artikel leest u hoe Istio installeren. De Istio `istioctl` binaire-client is geïnstalleerd op de clientcomputer, en vervolgens de Istio-onderdelen zijn geïnstalleerd in een Kubernetes-cluster in AKS. Deze instructies verwijzen naar Istio versie *1.0.4*. U vindt aanvullende Istio versies op [GitHub - Istio Releases][istio-github-releases].

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Istio downloaden
> * De binaire Istio-client installeren
> * Installeer de onderdelen van Istio Kubernetes
> * De installatie valideren

## <a name="before-you-begin"></a>Voordat u begint

De stappen die worden beschreven in dit artikel wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt (Kubernetes 1.10 en hoger, met RBAC ingeschakeld) en tot stand hebt gebracht een `kubectl` verbinding met het cluster. Als u hulp nodig met een van deze items, Zie de [Quick Start voor AKS][aks-quickstart].

Als u wilt installeren Istio, moet u [Helm] [ helm] versie *2.10.0* of later correct geïnstalleerd en geconfigureerd in uw cluster. Als u hulp bij het installeren van Helm nodig hebt, raadpleegt u de [AKS Helm installatie richtlijnen][helm-install]. Als u ten minste hebt versie *2.10.0* van Helm is geïnstalleerd, update, of Raadpleeg de [Istio - installatie met Helm handleiding] [ istio-install-helm] voor andere opties voor de installatie.

In dit artikel worden gescheiden van de richtlijnen van de installatie Istio in meerdere afzonderlijke stappen. Elk van deze stappen wordt beschreven, zodat u meer informatie over het installeren van Istio en ontdek hoe Istio werkt met Kubernetes. Het eindresultaat is hetzelfde als in de structuur als de installatie van de officiële Istio [richtlijnen][istio-install-k8s-quickstart].

## <a name="download-istio"></a>Istio downloaden

Eerst, downloaden en uitpakken van de meest recente versie van de Istio. De stappen zijn enigszins anders voor een bash-shell op MacOS, Linux of Windows-subsysteem voor Linux en een PowerShell-shell. Kies een van de installatie van de volgende stappen voor uw favoriete omgeving:

* [Bash on MacOS, Linux of Windows-subsysteem voor Linux](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

Gebruik op MacOS, `curl` download de nieuwste release van Istio en vervolgens uitpakken met `tar` als volgt:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Gebruik op Linux of Windows-subsysteem voor Linux, `curl` download de nieuwste release van Istio en vervolgens uitpakken met `tar` als volgt:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

### <a name="powershell"></a>PowerShell

In PowerShell, gebruikt u [Invoke-WebRequest] [ Invoke-WebRequest] download de nieuwste release van Istio en vervolgens uitpakken met [uit te breiden-archief] [ Expand-Archive] als met de volgende:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.0.4"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

## <a name="install-the-istio-client-binary"></a>De binaire Istio-client installeren

De `istioctl` binaire-client wordt uitgevoerd op de clientcomputer en kunt u regels voor het doorsturen van Istio en beleidsregels beheert. Nogmaals, de installeren-stappen zijn enigszins verschillen tussen de client-besturingssystemen. Kies een van de installatie van de volgende stappen voor uw favoriete omgeving.

> [!IMPORTANT]
> Zorg ervoor dat u de stappen in deze sectie worden uitgevoerd vanuit de map op het hoogste niveau van de Istio-versie die u hebt gedownload en uitgepakt.

### <a name="macos"></a>MacOS

Voor het installeren van de Istio `istioctl` client binaire bestanden in een op basis van een bash-shell op MacOS, gebruik de volgende opdrachten. Met deze opdrachten kopieert de `istioctl` client binaire naar de locatie van de standaardgebruiker programma in uw `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Als u opdrachtregelprogramma voltooid voor de Istio `istioctl` client binaire, vervolgens instellen als volgt:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Nu wordt verplaatst naar de sectie [Installeer de onderdelen van Istio Kubernetes](#install-the-istio-kubernetes-components).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux of Windows-subsysteem voor Linux

Gebruik de volgende opdrachten voor het installeren van de Istio `istioctl` client binaire in een op basis van een bash-shell op Linux of [Windows-subsysteem voor Linux][install-wsl]. Met deze opdrachten kopieert de `istioctl` client binaire naar de locatie van de standaardgebruiker programma in uw `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Als u opdrachtregelprogramma voltooid voor de Istio `istioctl` client binaire, vervolgens instellen als volgt:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Nu wordt verplaatst naar de sectie [Installeer de onderdelen van Istio Kubernetes](#install-the-istio-kubernetes-components).

### <a name="windows"></a>Windows

Voor het installeren van de Istio `istioctl` client binaire bestand in een shell Powershell gebaseerd op Windows, gebruik de volgende opdrachten. Met deze opdrachten kopieert de `istioctl` client binair aan een nieuwe gebruiker-locatie-programma en zodat deze beschikbaar zijn via uw `PATH`.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:/Program Files/Istio"
mv ./bin/istioctl.exe "C:/Program Files/Istio/"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Program Files\Istio\", "User")
```

## <a name="install-the-istio-kubernetes-components"></a>Installeer de onderdelen van Istio Kubernetes

> [!IMPORTANT]
> Zorg ervoor dat u de stappen in deze sectie worden uitgevoerd vanuit de map op het hoogste niveau van de Istio-versie die u hebt gedownload en uitgepakt.

> [!NOTE]
> Versie `1.0.6` en nieuwere van het Istio Helm-diagram bevat belangrijke wijzigingen. Als u voor het installeren van deze versie selecteert, moet u nu handmatig maken van een geheim voor Kiali. U moet ook handmatig maken van een geheim voor Grafana als u hebt ingesteld `grafana.security.enabled=true`. Zie het Helm-diagram Istio [README.md](https://github.com/istio/istio/tree/master/install/kubernetes/helm/istio#installing-the-chart) voor meer informatie over het maken van deze geheime gegevens.

Gebruik Helm om de onderdelen Istio naar uw AKS-cluster. Installeren van de resources Istio in de `istio-system` naamruimte, en aanvullende opties voor beveiliging en bewaking als volgt inschakelen:

```azurecli
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set grafana.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

Het Helm-diagram implementeert een groot aantal objecten. De implementatie kunt 2-3 minuten duren, afhankelijk van de clusteromgeving.

## <a name="validate-the-installation"></a>De installatie valideren

Om ervoor te zorgen dat u een geslaagde implementatie van Istio hebt, gaan we de installatie valideren.

Eerst bevestigen dat de verwachte services zijn gemaakt. Gebruik de [kubectl ophalen svc] [ kubectl-get] opdracht om de actieve services weer te geven. Query de *istio system* naamruimte, waar de Istio en Add-on-onderdelen zijn geïnstalleerd door het Helm-diagram:

```console
kubectl get svc --namespace istio-system --output wide
```

De volgende voorbeelduitvoer ziet u de services die u moeten nu worden uitgevoerd:

- *istio -** services
- * jaeger-**, *tracering*, en *zipkin* tracering invoegtoepassingsservices
- *prometheus* invoegtoepassingsservice metrische gegevens
- *grafana* invoegtoepassing analyses en dashboard-bewakingsservice
- *kiali* invoegtoepassingsservice service mesh-dashboard

Als de `istio-ingressgateway` ziet u een extern IP-adres van `<pending>`, wacht een paar minuten totdat een IP-adres is toegewezen door de Azure-netwerken.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                   AGE       SELECTOR
grafana                  ClusterIP      10.0.26.60     <none>           3000/TCP                                                                                                                  3m        app=grafana
istio-citadel            ClusterIP      10.0.88.87     <none>           8060/TCP,9093/TCP                                                                                                         3m        istio=citadel
istio-egressgateway      ClusterIP      10.0.115.115   <none>           80/TCP,443/TCP                                                                                                            3m        app=istio-egressgateway,istio=egressgateway
istio-galley             ClusterIP      10.0.104.183   <none>           443/TCP,9093/TCP                                                                                                          3m        istio=galley
istio-ingressgateway     LoadBalancer   10.0.12.216    52.187.250.239   80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30469/TCP,8060:31999/TCP,853:31235/TCP,15030:32000/TCP,15031:30293/TCP   3m        app=istio-ingressgateway,istio=ingressgateway
istio-pilot              ClusterIP      10.0.38.134    <none>           15010/TCP,15011/TCP,8080/TCP,9093/TCP                                                                                     3m        istio=pilot
istio-policy             ClusterIP      10.0.253.81    <none>           9091/TCP,15004/TCP,9093/TCP                                                                                               3m        istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.181.186   <none>           443/TCP                                                                                                                   3m        istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.177.113   <none>           9091/TCP,15004/TCP,9093/TCP,42422/TCP                                                                                     3m        istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                3m        app=jaeger
jaeger-collector         ClusterIP      10.0.112.81    <none>           14267/TCP,14268/TCP                                                                                                       3m        app=jaeger
jaeger-query             ClusterIP      10.0.179.193   <none>           16686/TCP                                                                                                                 3m        app=jaeger
kiali                    ClusterIP      10.0.211.63    <none>           20001/TCP                                                                                                                 3m        app=kiali
prometheus               ClusterIP      10.0.70.113    <none>           9090/TCP                                                                                                                  3m        app=prometheus
tracing                  ClusterIP      10.0.139.121   <none>           80/TCP                                                                                                                    3m        app=jaeger
zipkin                   ClusterIP      10.0.60.155    <none>           9411/TCP                                                                                                                  3m        app=jaeger
```

Controleer vervolgens of dat de vereiste schillen zijn gemaakt. Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht en het opvragen van opnieuw de *istio system* naamruimte:

```console
kubectl get pods --namespace istio-system
```

De volgende voorbeelduitvoer ziet u de schillen die worden uitgevoerd:

- de *istio -** schillen
- de *prometheus -** pod voor aanvullende metrische gegevens
- de *grafana -** aanvullende analyse en bewaking dashboard-pod
- de *kiali* invoegtoepassing service mesh dashboard-pod

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-59b787b9b-cr6d7                  1/1       Running     0          6m
istio-citadel-78df8c67d9-9lfpf           1/1       Running     0          6m
istio-egressgateway-6b96cd7f5-k848h      1/1       Running     0          6m
istio-galley-58f566cb66-8mhbv            1/1       Running     0          6m
istio-ingressgateway-6cbbf596f6-6jz8g    1/1       Running     0          6m
istio-pilot-8449d555fc-sl6kp             2/2       Running     0          6m
istio-policy-6b99d88bc5-55s52            2/2       Running     0          6m
istio-sidecar-injector-b88dfb954-8m86s   1/1       Running     0          6m
istio-telemetry-675cb4cb9d-8s7wd         2/2       Running     0          6m
istio-tracing-7596597bd7-sbnt9           1/1       Running     0          6m
kiali-5fbd6ffb-4qcxw                     1/1       Running     0          6m
prometheus-76db5fddd5-2tkxs              1/1       Running     0          6m
```

Alle van de pods weergeven met de status van `Running`. Als uw schillen geen deze statussen, wacht u enkele minuten totdat ze doen. Als een schillen een probleem melden, gebruikt u de [kubectl beschrijven pod] [ kubectl-describe] opdracht uit om te controleren van de uitvoer en de status.

## <a name="accessing-the-add-ons"></a>Toegang tot de uitbreidingen

Een aantal invoegtoepassingen zijn geïnstalleerd Istio in het bovenstaande onze installatieprogramma die aanvullende functionaliteit bieden. De gebruikersinterfaces voor de invoegtoepassingen niet openbaar beschikbaar via een externe ip-adres. Voor toegang tot de invoegtoepassing gebruikersinterfaces, gebruiken de [kubectl poort-zone voor forward] [ kubectl-port-forward] opdracht. Met deze opdracht maakt een beveiligde verbinding tussen een lokale poort op de clientcomputer en de relevante schil in uw AKS-cluster.

### <a name="grafana"></a>Grafana

De analyse en bewaking van dashboards voor Istio wordt geleverd door [Grafana][grafana]. Doorsturen van de lokale poort *3000* op de clientcomputer naar poort *3000* op de schil die Grafana wordt uitgevoerd in uw AKS-cluster:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Grafana:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Nu kunt u Grafana via de volgende URL bereiken op de clientcomputer - [ http://localhost:3000 ](http://localhost:3000).

### <a name="prometheus"></a>Prometheus

De browser van de expressie voor de metrische gegevens die wordt geleverd door [Prometheus][prometheus]. Doorsturen van de lokale poort *9090* op de clientcomputer naar poort *9090* op de schil die Prometheus wordt uitgevoerd in uw AKS-cluster:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Prometheus:

```console
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

U kunt nu de browser van de expressie Prometheus op de volgende URL bereiken op de clientcomputer - [ http://localhost:9090 ](http://localhost:9090).

### <a name="jaeger"></a>Jaeger

Tracering van de gebruikersinterface wordt geleverd door [Jaeger][jaeger]. Doorsturen van de lokale poort *16686* op de clientcomputer naar poort *16686* op de schil die Jaeger wordt uitgevoerd in uw AKS-cluster:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Jaeger:

```console
Forwarding from 127.0.0.1:16686 -> 16686
Forwarding from [::1]:16686 -> 16686
```

U kunt nu de gebruikersinterface voor tracering van Jaeger op de volgende URL bereiken op de clientcomputer - [ http://localhost:16686 ](http://localhost:16686).

### <a name="kiali"></a>Kiali

Een dashboard mesh observability wordt geleverd door [Kiali][kiali]. Doorsturen van de lokale poort *20001* op de clientcomputer naar poort *20001* op de schil die Kiali wordt uitgevoerd in uw AKS-cluster:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Kiali:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

U kunt nu het Kiali mesh observability dashboard op de volgende URL bereiken op de clientcomputer - [ http://localhost:20001 ](http://localhost:20001).

De standaardgebruikersnaam en het wachtwoord voor het dashboard Kiali is *username:admin / wachtwoord: beheerder*. Deze referenties kunnen worden geconfigureerd via de *kiali.dashboard.username* en *kiali.dashboard.passphrase* Helm waarden.

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe u intelligente routering tussen meerdere versies van uw toepassing en implementeer een canary-release Istio kunt gebruiken, Zie de volgende documentatie:

> [!div class="nextstepaction"]
> [AKS Istio intelligente routeringsscenario][istio-scenario-routing]

Als u wilt meer opties voor de installatie en configuratie voor Istio verkennen, Zie de volgende officiële Istio artikelen:

- [Istio - snelstartgids van Kubernetes-installatie][istio-install-k8s-quickstart]
- [Istio - installatiehandleiding Helm][istio-install-helm]
- [Istio - Helm-installatieopties][istio-install-helm-options]

U kunt ook aanvullende scenario's met behulp van volgen de [Istio Bookinfo toepassing voorbeeld][istio-bookinfo-example].

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-k8s-quickstart]: https://istio.io/docs/setup/kubernetes/quick-start/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/helm-install/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10
[kubernetes-crd]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
[Invoke-WebRequest]: /powershell/module/microsoft.powershell.utility/invoke-webrequest
[Expand-Archive]: /powershell/module/Microsoft.PowerShell.Archive/Expand-Archive
