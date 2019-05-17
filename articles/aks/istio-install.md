---
title: Istio installeren in Azure Kubernetes Service (AKS)
description: Meer informatie over het installeren en gebruiken van Istio voor het maken van een service-Net in een cluster Azure Kubernetes Service (AKS)
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: 12565d2b8004a5119add25473e5b088c9162035f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780501"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Installeren en gebruiken van Istio in Azure Kubernetes Service (AKS)

[Istio] [ istio-github] is een open-source-service Net die een belangrijke set functionaliteit voor de microservices in een Kubernetes-cluster biedt. Deze functies omvatten beheer van verkeer, service-identiteit en beveiliging afdwingen van beleid en observability. Zie voor meer informatie over Istio, de officiële [wat is er Istio?] [ istio-docs-concepts] documentatie.

Dit artikel leest u hoe Istio installeren. De Istio `istioctl` binaire client op de clientcomputer is geïnstalleerd en de Istio-onderdelen zijn geïnstalleerd in een Kubernetes-cluster in AKS.

> [!NOTE]
> Deze instructies verwijzen naar Istio versie `1.1.3`.
>
> De Istio `1.1.x` releases zijn getest door het team Istio op basis van Kubernetes-versies `1.11`, `1.12`, `1.13`. U vindt aanvullende Istio versies op [GitHub - Istio Releases] [ istio-github-releases] en informatie over elk van de releases op [Istio - opmerkingen bij de Release] [ istio-release-notes].

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Istio downloaden
> * De binaire Istio istioctl client installeren
> * Installeer de CRDs Istio op AKS
> * Installeer de onderdelen Istio op AKS
> * De installatie Istio valideren
> * Toegang tot de uitbreidingen
> * Istio verwijderen in AKS

## <a name="before-you-begin"></a>Voordat u begint

De stappen die worden beschreven in dit artikel wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt (Kubernetes `1.11` en hoger, met RBAC ingeschakeld) en tot stand hebt gebracht een `kubectl` verbinding met het cluster. Als u hulp nodig met een van deze items, Zie de [Quick Start voor AKS][aks-quickstart].

U moet [Helm] [ helm] Volg deze instructies en Istio installeren. Het verdient aanbeveling dat u versie hebt `2.12.2` of later correct geïnstalleerd en geconfigureerd in uw cluster. Als u hulp bij het installeren van Helm nodig hebt, raadpleegt de [AKS Helm installatie richtlijnen][helm-install].

In dit artikel worden gescheiden van de richtlijnen van de installatie Istio in meerdere afzonderlijke stappen. Het eindresultaat is hetzelfde als in de structuur als de installatie van de officiële Istio [richtlijnen][istio-install-helm].

## <a name="download-istio"></a>Istio downloaden

Eerst, downloaden en uitpakken van de meest recente versie van de Istio. De stappen zijn enigszins anders voor een bash-shell op MacOS, Linux of Windows-subsysteem voor Linux en een PowerShell-shell. Kies een van de volgende installeren-stappen die overeenkomt met uw favoriete omgeving:

* [Bash on MacOS, Linux of Windows-subsysteem voor Linux](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

Gebruik op MacOS, `curl` download de nieuwste release van Istio en vervolgens uitpakken met `tar` als volgt:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Gebruik op Linux of Windows-subsysteem voor Linux, `curl` download de nieuwste release van Istio en vervolgens uitpakken met `tar` als volgt:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

Nu wordt verplaatst naar de sectie [installeren de binaire Istio istioctl client](#install-the-istio-istioctl-client-binary).

### <a name="powershell"></a>PowerShell

In PowerShell, gebruikt u `Invoke-WebRequest` download de nieuwste release van Istio en vervolgens uitpakken met `Expand-Archive` als volgt:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.1.3"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

Nu wordt verplaatst naar de sectie [installeren de binaire Istio istioctl client](#install-the-istio-istioctl-client-binary).

## <a name="install-the-istio-istioctl-client-binary"></a>De binaire Istio istioctl client installeren

> [!IMPORTANT]
> Zorg ervoor dat u de stappen in deze sectie worden uitgevoerd vanuit de map op het hoogste niveau van de Istio-versie die u hebt gedownload en uitgepakt.

De `istioctl` binaire-client wordt uitgevoerd op de clientcomputer en kunt u communiceren met de service Istio mesh. De installatiestappen zijn enigszins verschillen tussen de client-besturingssystemen. Kies een van de volgende installeren-stappen die overeenkomt met uw favoriete omgeving:

* [MacOS](#macos)
* [Linux of Windows-subsysteem voor Linux](#linux-or-windows-subsystem-for-linux)
* [Windows](#windows)

### <a name="macos"></a>MacOS

Voor het installeren van de Istio `istioctl` client binaire bestanden in een op basis van een bash-shell op MacOS, gebruik de volgende opdrachten. Met deze opdrachten kopieert de `istioctl` client binaire naar de locatie van de standaardgebruiker programma in uw `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
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

Nu verder met de volgende sectie voor [de CRDs Istio installeren op AKS](#install-the-istio-crds-on-aks).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux of Windows-subsysteem voor Linux

Gebruik de volgende opdrachten voor het installeren van de Istio `istioctl` client binaire in een op basis van een bash-shell op Linux of [Windows-subsysteem voor Linux][install-wsl]. Met deze opdrachten kopieert de `istioctl` client binaire naar de locatie van de standaardgebruiker programma in uw `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
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

Nu verder met de volgende sectie voor [de CRDs Istio installeren op AKS](#install-the-istio-crds-on-aks).

### <a name="windows"></a>Windows

Voor het installeren van de Istio `istioctl` client binaire in een **Powershell**-gebaseerde shell op Windows, gebruik de volgende opdrachten. Met deze opdrachten kopieert de `istioctl` binaire naar een map Istio client en het permanent beschikbaar zijn via uw `PATH`. U hoeft geen verhoogde bevoegdheden (Admin) hebben voor deze opdrachten uitvoeren.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Istio\", "User")
```

Nu verder met de volgende sectie voor [de CRDs Istio installeren op AKS](#install-the-istio-crds-on-aks).

## <a name="install-the-istio-crds-on-aks"></a>Installeer de CRDs Istio op AKS

> [!IMPORTANT]
> Zorg ervoor dat u de stappen in deze sectie worden uitgevoerd vanuit de map op het hoogste niveau van de Istio-versie die u hebt gedownload en uitgepakt.

Maakt gebruik van Istio [aangepaste Resourcedefinities (CRDs)] [ kubernetes-crd] voor het beheren van de runtimeconfiguratie. We moeten eerst de CRDs Istio installeren aangezien de Istio onderdelen een afhankelijkheid op deze hebben. Helm gebruiken en de `istio-init` grafiek voor het installeren van de CRDs Istio in de `istio-system` naamruimte in uw AKS-cluster:

```azurecli
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

[Taken] [ kubernetes-jobs] worden geïmplementeerd als onderdeel van de `istio-init` Helm-diagram voor het installeren van de CRDs. Deze taken moeten rekening houden tussen 1 tot 2 minuten duren, afhankelijk van uw clusteromgeving. U kunt controleren of de taken zijn voltooid als volgt:

```azurecli
kubectl get jobs -n istio-system
```

De volgende voorbeelduitvoer ziet u de taken is voltooid.

```console
NAME                COMPLETIONS   DURATION   AGE
istio-init-crd-10   1/1           16s        18s
istio-init-crd-11   1/1           15s        18s
```

Nu dat we hebben vastgesteld dat de voltooiing van de taken, controleren we of dat we het juiste aantal Istio CRDs geïnstalleerd hebben. U kunt controleren of alle 53 Istio CRDs zijn geïnstalleerd met de juiste opdracht voor uw omgeving. Het getal moet worden geretourneerd door de opdracht `53`.

Bash

```bash
kubectl get crds | grep 'istio.io' | wc -l
```

PowerShell

```powershell
(kubectl get crds | Select-String -Pattern 'istio.io').Count
```

Als u op dit punt hebt, klikt u vervolgens dit betekent dat u hebt de CRDs Istio geïnstalleerd. Nu verder met de volgende sectie voor [Installeer de onderdelen Istio op AKS](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>Installeer de onderdelen Istio op AKS

> [!IMPORTANT]
> Zorg ervoor dat u de stappen in deze sectie worden uitgevoerd vanuit de map op het hoogste niveau van de Istio-versie die u hebt gedownload en uitgepakt.

We worden installeert [Grafana] [ grafana] en [Kiali] [ kiali] als onderdeel van onze Istio-installatie. Grafana biedt analyse en bewaking van dashboards en Kiali biedt een dashboard mesh observability. In onze setup van elk van deze onderdelen hebt u referenties nodig die moeten worden opgegeven als een [geheim][kubernetes-secrets].

Voordat we de onderdelen Istio installeren kunt, moeten we de geheimen voor zowel Grafana en Kiali maken. Maak deze geheime gegevens door het uitvoeren van de juiste opdrachten voor uw omgeving.

### <a name="add-grafana-secret"></a>Grafana geheim toevoegen

Vervang de `REPLACE_WITH_YOUR_SECURE_PASSWORD` token met het wachtwoord in en voer de volgende opdrachten uit:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
GRAFANA_USERNAME=$(echo -n "grafana" | base64)
GRAFANA_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$GRAFANA_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("grafana"))
$GRAFANA_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE" | kubectl apply -f -
```

### <a name="add-kiali-secret"></a>Kiali geheim toevoegen

Vervang de `REPLACE_WITH_YOUR_SECURE_PASSWORD` token met het wachtwoord in en voer de volgende opdrachten uit:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
KIALI_USERNAME=$(echo -n "kiali" | base64)
KIALI_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$KIALI_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("kiali"))
$KIALI_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE" | kubectl apply -f -
```

### <a name="install-istio-components"></a>Istio onderdelen installeren

Nu we de geheimen Grafana en Kiali in onze AKS-cluster hebt gemaakt, is het tijd om de Istio-onderdelen te installeren. Helm gebruiken en de `istio` grafiek voor het installeren van de Istio-onderdelen in de `istio-system` naamruimte in uw AKS-cluster. Gebruik de juiste opdrachten voor uw omgeving.

> [!NOTE]
> Als onderdeel van onze installatie gebruiken we de volgende opties:
> - `global.controlPlaneSecurityEnabled=true` -wederzijdse TLS is ingeschakeld voor het besturingselement vlak
> - `mixer.adapters.useAdapterCRDs=false` -controles op mixer-adapter CRDs worden verwijderd omdat ze worden afgeschaft en dit de prestaties verbeteren
> - `grafana.enabled=true` -Grafana-implementatie voor analyses mogelijk te maken en de controle van dashboards
> - `grafana.security.enabled=true` -verificatie inschakelen voor Grafana
> - `tracing.enabled=true` -de implementatie Jaeger voor tracering inschakelen
> - `kiali.enabled=true` -de implementatie Kiali voor een dashboard mesh observability inschakelen

Bash

```bash
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set mixer.adapters.useAdapterCRDs=false \
  --set grafana.enabled=true --set grafana.security.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

PowerShell

```powershell
helm install install/kubernetes/helm/istio --name istio --namespace istio-system `
  --set global.controlPlaneSecurityEnabled=true `
  --set mixer.adapters.useAdapterCRDs=false `
  --set grafana.enabled=true --set grafana.security.enabled=true `
  --set tracing.enabled=true `
  --set kiali.enabled=true
```

De `istio` Helm-diagram implementeert een groot aantal objecten. U ziet de lijst uit de uitvoer van uw `helm install` bovenstaande opdracht. De implementatie van de onderdelen Istio kunt 4 tot en met 5 minuten duren, afhankelijk van uw clusteromgeving.

U hebt op dit moment Istio geïmplementeerd naar uw AKS-cluster. Om ervoor te zorgen dat we beschikken over een geslaagde implementatie van Istio, laten we doorgaan naar de volgende sectie voor [valideren van de installatie van de Istio](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>De installatie Istio valideren

Eerst bevestigen dat de verwachte services zijn gemaakt. Gebruik de [kubectl ophalen svc] [ kubectl-get] opdracht om de actieve services weer te geven. Query de `istio-system` naamruimte, waar de Istio en Add-on-onderdelen zijn geïnstalleerd door de `istio` Helm-diagram:

```console
kubectl get svc --namespace istio-system --output wide
```

De volgende voorbeelduitvoer ziet u de services die u moeten nu worden uitgevoerd:

- `istio-*` Services
- `jaeger-*`, `tracing`, en `zipkin` tracering invoegtoepassingsservices
- `prometheus` service voor aanvullende metrische gegevens
- `grafana` invoegtoepassing analyses en dashboard-bewakingsservice
- `kiali` invoegtoepassing service mesh-dashboard-service

Als de `istio-ingressgateway` ziet u een extern IP-adres van `<pending>`, wacht een paar minuten totdat een IP-adres is toegewezen door de Azure-netwerken.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                                                                                                                                      AGE       SELECTOR
grafana                  ClusterIP      10.0.81.182    <none>          3000/TCP                                                                                                                                     119s      app=grafana
istio-citadel            ClusterIP      10.0.96.33     <none>          8060/TCP,15014/TCP                                                                                                                           119s      istio=citadel
istio-galley             ClusterIP      10.0.237.158   <none>          443/TCP,15014/TCP,9901/TCP                                                                                                                   119s      istio=galley
istio-ingressgateway     LoadBalancer   10.0.154.12    20.188.211.19   15020:30603/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31198/TCP,15030:30610/TCP,15031:30937/TCP,15032:31344/TCP,15443:31499/TCP   119s      app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.178.56    <none>          15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       119s      istio=pilot
istio-policy             ClusterIP      10.0.116.118   <none>          9091/TCP,15004/TCP,15014/TCP                                                                                                                 119s      istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.31.160    <none>          443/TCP                                                                                                                                      119s      istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.187.246   <none>          9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       119s      istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>          5775/UDP,6831/UDP,6832/UDP                                                                                                                   119s      app=jaeger
jaeger-collector         ClusterIP      10.0.116.63    <none>          14267/TCP,14268/TCP                                                                                                                          119s      app=jaeger
jaeger-query             ClusterIP      10.0.22.108    <none>          16686/TCP                                                                                                                                    119s      app=jaeger
kiali                    ClusterIP      10.0.142.50    <none>          20001/TCP                                                                                                                                    119s      app=kiali
prometheus               ClusterIP      10.0.138.134   <none>          9090/TCP                                                                                                                                     119s      app=prometheus
tracing                  ClusterIP      10.0.165.210   <none>          80/TCP                                                                                                                                       118s      app=jaeger
zipkin                   ClusterIP      10.0.126.211   <none>          9411/TCP                                                                                                                                     118s      app=jaeger
```

Controleer vervolgens of dat de vereiste schillen zijn gemaakt. Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht en het opvragen van opnieuw de `istio-system` naamruimte:

```console
kubectl get pods --namespace istio-system
```

De volgende voorbeelduitvoer ziet u de schillen die worden uitgevoerd:

- de `istio-*` schillen
- de `prometheus-*` pod voor aanvullende metrische gegevens
- de `grafana-*` aanvullende analyse en bewaking dashboard-pod
- de `kiali` invoegtoepassing service mesh dashboard-pod

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-88779954d-nzpm7                  1/1       Running     0          6m26s
istio-citadel-7f699dc8c8-n7q8g           1/1       Running     0          6m26s
istio-galley-649bc8cd97-wfjzm            1/1       Running     0          6m26s
istio-ingressgateway-65dfbd566-42wkn     1/1       Running     0          6m26s
istio-init-crd-10-tmtw5                  0/1       Completed   0          20m38s
istio-init-crd-11-ql25l                  0/1       Completed   0          20m38s
istio-pilot-958dd8cc4-4ckf9              2/2       Running     0          6m26s
istio-policy-86b4b7cf9-zf7v7             2/2       Running     4          6m26s
istio-sidecar-injector-d48786c5c-pmrj9   1/1       Running     0          6m26s
istio-telemetry-7f6996fdcc-84w94         2/2       Running     3          6m26s
istio-tracing-79db5954f-h7hmz            1/1       Running     0          6m26s
kiali-5c4cdbb869-s28dv                   1/1       Running     0          6m26s
prometheus-67599bf55b-pgxd8              1/1       Running     0          6m26s
```

Er moeten twee `istio-init-crd-*` schillen met een `Completed` status. Deze schillen zijn verantwoordelijk voor het uitvoeren van de taken die de CRDs in een eerdere stap hebt gemaakt. Alle andere schillen status moet worden weergegeven `Running`. Als uw schillen geen deze statussen, wacht u enkele minuten totdat ze doen. Als een schillen een probleem melden, gebruikt u de [kubectl beschrijven pod] [ kubectl-describe] opdracht uit om te controleren van de uitvoer en de status.

## <a name="accessing-the-add-ons"></a>Toegang tot de uitbreidingen

Een aantal invoegtoepassingen zijn geïnstalleerd Istio in het bovenstaande onze installatieprogramma die aanvullende functionaliteit bieden. De gebruikersinterfaces voor de invoegtoepassingen niet openbaar beschikbaar via een externe ip-adres. Voor toegang tot de invoegtoepassing gebruikersinterfaces, gebruiken de [kubectl poort-zone voor forward] [ kubectl-port-forward] opdracht. Met deze opdracht maakt een beveiligde verbinding tussen de clientcomputer en de relevante schil in uw AKS-cluster.

We een extra beveiligingslaag toegevoegd voor Grafana en Kiali door het opgeven van referenties voor deze eerder in dit artikel.

### <a name="grafana"></a>Grafana

De analyse- en bewaking van dashboards voor Istio worden geleverd door [Grafana][grafana]. Doorsturen van de lokale poort `3000` op de clientcomputer naar poort `3000` op de schil die Grafana wordt uitgevoerd in uw AKS-cluster:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Grafana:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Nu kunt u Grafana via de volgende URL bereiken op de clientcomputer - [ http://localhost:3000 ](http://localhost:3000). Geef de referenties die u eerder hebt gemaakt via het Grafana geheime wanneer hierom wordt gevraagd.

### <a name="prometheus"></a>Prometheus

Metrische gegevens voor Istio worden geleverd door [Prometheus][prometheus]. Doorsturen van de lokale poort `9090` op de clientcomputer naar poort `9090` op de schil die Prometheus wordt uitgevoerd in uw AKS-cluster:

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

Tracering in Istio wordt geleverd door [Jaeger][jaeger]. Doorsturen van de lokale poort `16686` op de clientcomputer naar poort `16686` op de schil die Jaeger wordt uitgevoerd in uw AKS-cluster:

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

Een dashboard mesh observability wordt geleverd door [Kiali][kiali]. Doorsturen van de lokale poort `20001` op de clientcomputer naar poort `20001` op de schil die Kiali wordt uitgevoerd in uw AKS-cluster:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

De volgende voorbeelduitvoer ziet u de poort-zone voor forward die worden geconfigureerd voor Kiali:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

U kunt nu het Kiali mesh observability dashboard op de volgende URL bereiken op de clientcomputer - [ http://localhost:20001/kiali/console/ ](http://localhost:20001/kiali/console/). Geef de referenties die u eerder hebt gemaakt via de Kiali geheime wanneer hierom wordt gevraagd.

## <a name="uninstall-istio-from-aks"></a>Istio verwijderen in AKS

> [!WARNING]
> Istio verwijderen uit een actief systeem kan leiden tot netwerkverkeer problemen tussen uw services. Zorg ervoor dat u hebt aangebracht aan bepalingen voor uw systeem nog steeds correct functioneren zonder Istio voordat u doorgaat.

### <a name="remove-istio-components-and-namespace"></a>Istio onderdelen en -naamruimte verwijderen

Als u wilt verwijderen Istio van uw AKS-cluster, moet u de volgende opdrachten gebruiken. De `helm delete` -opdrachten verwijderen de `istio` en `istio-init` grafieken, en de `kubectl delete ns` opdracht verwijdert u de `istio-system` naamruimte.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete ns istio-system
```

### <a name="remove-istio-crds"></a>Istio CRDs verwijderen

De bovenstaande opdrachten alle Istio onderdelen en -naamruimte verwijderen, maar we zijn nog steeds met de CRDs Istio links. Als u wilt de CRDs verwijderen, kunt u een de volgende methoden.

Methode #1 - met deze opdracht wordt ervan uitgegaan dat u deze stap worden uitgevoerd vanuit de map op het hoogste niveau van de gedownloade en uitgepakte release Istio die u hebt gebruikt voor het installeren van Istio met.

```azure-cli
kubectl delete -f install/kubernetes/helm/istio-init/files
```

Methode #2: gebruik een van deze opdrachten als u niet langer toegang tot de gedownloade en uitgepakte release Istio die u hebt gebruikt hebt voor het installeren van Istio met. Met deze opdracht duurt iets langer - verwacht dat het een paar minuten duren.

Bash
```bash
kubectl get crds -o name | grep 'istio.io' | xargs -n1 kubectl delete
```

PowerShell
```powershell
kubectl get crds -o name | Select-String -Pattern 'istio.io' |% { kubectl delete $_ }
```

## <a name="next-steps"></a>Volgende stappen

De volgende documentatie wordt beschreven hoe u Istio kunt gebruiken voor intelligente routering voor een canary-release implementatie:

> [!div class="nextstepaction"]
> [AKS Istio intelligente routeringsscenario][istio-scenario-routing]

Als u wilt meer opties voor de installatie en configuratie voor Istio verkennen, Zie de volgende officiële Istio artikelen:

- [Istio - installatiehandleiding Helm][istio-install-helm]
- [Istio - Helm-installatieopties][istio-install-helm-options]

U kunt ook aanvullende scenario's met behulp van volgen de [Istio Bookinfo toepassing voorbeeld][istio-bookinfo-example].

Voor informatie over het bewaken van uw AKS-toepassing met behulp van Application Insights en Istio, Zie de volgende documentatie voor Azure Monitor:
- [Nul instrumentatie-toepassingsbewaking voor Kubernetes gehoste toepassingen][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/about/notes/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md