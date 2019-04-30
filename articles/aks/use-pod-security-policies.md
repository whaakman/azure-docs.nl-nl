---
title: Beveiligingsbeleidsregels voor pod gebruiken in Azure Kubernetes Service (AKS)
description: Meer informatie over het beheren van pod Admission via PodSecurityPolicy in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/17/2019
ms.author: iainfou
ms.openlocfilehash: 7ce311ab9c554481f64c6c9be40e2018893a0966
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027372"
---
# <a name="preview---secure-your-cluster-using-pod-security-policies-in-azure-kubernetes-service-aks"></a>Voorbeeld: Beveilig uw cluster met behulp van pod-beveiligingsbeleid in Azure Kubernetes Service (AKS)

Ter verbetering van de beveiliging van uw AKS-cluster, kunt u beperken welke schillen kunnen worden gepland. Schillen die de aanvraag voor resources die u niet toestaan kunnen niet worden uitgevoerd in de AKS-cluster. U definiëren deze toegang met behulp van pod beveiligingsbeleid. Dit artikel ziet u hoe u beveiligingsbeleidsregels van pod gebruiken voor het beperken van de implementatie van schillen in AKS.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice en aanmelden. Previews worden opgegeven voor het verzamelen van fouten en feedback van onze community. Ze worden echter niet ondersteund door Azure technische ondersteuning. Als u een cluster maken of deze functies aan bestaande clusters toevoegen, is dat cluster wordt niet ondersteund totdat de functie niet langer in preview is en is geslaagd voor algemene beschikbaarheid (GA).
>
> Als u problemen met de preview-functies ondervindt, [opent u een probleem op de AKS-GitHub-opslagplaats] [ aks-github] met de naam van de preview-functie in de titel van fout.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren

AKS-clusters worden bijgewerkt om in te schakelen pod-beveiligingsbeleid met behulp van de *aks-preview* CLI-extensie. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Als u eerder hebt geïnstalleerd het *aks-preview* extensie, installatie van de beschikbare updates met behulp van de `az extension update --name aks-preview` opdracht.

### <a name="register-pod-security-policy-feature-provider"></a>Beleid pod-functie beveiligingsprovider registreren

Als u wilt maken of bijwerken van een AKS-cluster voor het gebruik van pod-beveiligingsbeleid, moet u eerst een functievlag voor uw abonnement inschakelen. Om u te registreren de *PodSecurityPolicyPreview* vlag functie, gebruikt u de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --name PodSecurityPolicyPreview --namespace Microsoft.ContainerService
```

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSecurityPolicyPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-pod-security-policies"></a>Overzicht van pod-beveiligingsbeleid

Een controller voor toegangsbeheer wordt in een Kubernetes-cluster gebruikt om aanvragen naar de API-server worden onderschept wanneer een resource moet worden gemaakt. De invoer-controller kan vervolgens *valideren* de resourceaanvraag aan de hand van regels, of *muteren* de resource te wijzigen van de implementatieparameters.

*PodSecurityPolicy* is een toelating-controller die een pod-specificatie valideert voldoet aan de gedefinieerde vereisten. Deze vereisten kunnen het gebruik van beschermde containers beperken, de toegang tot bepaalde soorten opslag, of de gebruiker of groep die de container kan worden uitgevoerd als. Wanneer u een bron waar de specificaties pod niet voldoen aan de vereisten die worden beschreven in het beveiligingsbeleid pod implementeren probeert, wordt de aanvraag geweigerd. Deze mogelijkheid om te bepalen welke schillen kunnen worden in de AKS gepland cluster voorkomt u dat sommige mogelijke beveiligingsproblemen of escalaties van bevoegdheden.

Wanneer u de schil beveiligingsbeleid in een AKS-cluster inschakelt, worden sommige standaard-beleid toegepast. Deze standaardbeleid biedt u een out-of-the-box-ervaring te definiëren wat schillen kan worden gepland. Cluster-gebruikers kunnen echter uitgevoerd in problemen met het implementeren van schillen totdat u uw eigen beleid is gedefinieerd. De aanbevolen aanpak is:

* Een AKS-cluster maken
* Het beveiligingsbeleid van uw eigen pod definiëren
* De schil beveiliging beleid inschakelen

Om weer te geven hoe het standaardbeleid pod-implementaties beperken, in dit artikel we eerst de schil beveiliging beleid inschakelen en vervolgens een aangepast beleid maken.

## <a name="enable-pod-security-policy-on-an-aks-cluster"></a>Inschakelen van pod-beveiligingsbeleid op een AKS-cluster

U kunt inschakelen of uitschakelen pod beveiliging beleid via de [az aks bijwerken] [ az-aks-update] opdracht. Het volgende voorbeeld maakt pod beveiligingsbeleid op de clusternaam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*.

> [!NOTE]
> Voor real-world wordt gebruikt, niet het beveiligingsbeleid pod inschakelen totdat u uw eigen aangepaste beleidsregels hebt gedefinieerd. In dit artikel, schakelt u pod beveiligingsbeleid voor als de eerste stap om te zien hoe het standaardbeleid pod beperken implementaties.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-pod-security-policy
```

## <a name="default-aks-policies"></a>Standaardbeleidsregels voor AKS

Wanneer u de schil beveiligingsbeleid inschakelt, maakt u twee standaardbeleid met de naam AKS *bevoegde* en *beperkte*. Niet bewerken of verwijderen van deze standaard-beleid. In plaats daarvan uw eigen beleid maken dat de instellingen die u wilt definiëren om te bepalen. Laten we eerst kijken wat deze standaardbeleidsregels zijn invloed op pod-implementaties.

Als u wilt weergeven van de beschikbare beleidsregels, gebruikt u de [kubectl ophalen psp] [ kubectl-get] opdracht, zoals wordt weergegeven in het volgende voorbeeld. Als onderdeel van de standaard *beperkte* beleid, de gebruiker geen *PRIV* gebruikt voor bevoegde pod escalatie- en de gebruiker *MustRunAsNonRoot*.

```console
$ kubectl get psp

NAME         PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged   true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted   false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

De *beperkte* pod-beveiligingsbeleid wordt toegepast op een geverifieerde gebruiker in het AKS-cluster. Deze toewijzing wordt bepaald door ClusterRoles en ClusterRoleBindings. Gebruik de [kubectl ophalen clusterrolebindings] [ kubectl-get] opdracht en zoek de *standaard: beperkte:* binding:

```console
kubectl get clusterrolebindings default:restricted -o yaml
```

Zoals wordt weergegeven in de volgende verkorte uitvoer, de *psp: beperkte* ClusterRole is toegewezen aan een *system: geverifieerde* gebruikers. Deze mogelijkheid biedt een basisniveau van beperkingen zonder uw eigen beleid wordt gedefinieerd.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  [...]
  name: default:restricted
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp:restricted
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:authenticated
```

Het is belangrijk om te begrijpen hoe deze standaardbeleidsregels communiceren met gebruikersaanvragen schillen plannen voordat u begint met het beveiligingsbeleid van uw eigen pod maken. In de volgende gedeelten laten we enkele schillen om te zien van dit standaardbeleid in actie plannen.

## <a name="create-a-test-user-in-an-aks-cluster"></a>Maak een testgebruiker in een AKS-cluster

Standaard, wanneer u de [az aks get-credentials] [ az-aks-get-credentials] opdracht, de *admin* referenties voor het AKS-cluster en toegevoegd aan uw `kubectl` config. De gebruiker met beheerdersrechten omzeilt de naleving van beveiligingsbeleid pod. Als u Azure Active Directory-integratie voor uw AKS-clusters, kunt u zich aanmelden met de referenties van een niet-beheerdersaccount het afdwingen van beleid in actie zien. In dit artikel gaan we maken een test-gebruikersaccount in het AKS-cluster die u kunt gebruiken.

Maken van een voorbeeld-naamruimte met de naam *psp aks* voor test-resources met behulp van de [kubectl-naamruimte maken] [ kubectl-create] opdracht. Maak vervolgens een service-account met de naam *nonadmin gebruiker* met behulp van de [kubectl maken serviceaccount] [ kubectl-create] opdracht:

```console
kubectl create namespace psp-aks
kubectl create serviceaccount --namespace psp-aks nonadmin-user
```

Maak vervolgens een RoleBinding voor de *nonadmin gebruiker* basisacties uitvoeren in de naamruimte met de [kubectl maken rolebinding] [ kubectl-create] opdracht:

```console
kubectl create rolebinding \
    --namespace psp-aks \
    psp-aks-editor \
    --clusterrole=edit \
    --serviceaccount=psp-aks:nonadmin-user
```

### <a name="create-alias-commands-for-admin-and-non-admin-user"></a>Aliasopdrachten voor beheerder en niet-beheerder-gebruiker maken

Markeer het verschil tussen de normale beheerder bij het gebruik van `kubectl` en de gebruiker die geen beheerder hebt gemaakt in de vorige stappen, twee opdrachtregel aliassen maken:

* De **kubectl-beheerder** alias is voor de gebruiker met beheerdersrechten reguliere en is afgestemd op de *psp aks* naamruimte.
* De **kubectl-nonadminuser** alias is voor de *nonadmin gebruiker* in de vorige stap hebt gemaakt en is afgestemd op de *psp aks* naamruimte.

Deze twee aliassen maken zoals wordt weergegeven in de volgende opdrachten:

```console
alias kubectl-admin='kubectl --namespace psp-aks'
alias kubectl-nonadminuser='kubectl --as=system:serviceaccount:psp-aks:nonadmin-user --namespace psp-aks'
```

## <a name="test-the-creation-of-a-privileged-pod"></a>Het maken van een bevoegde schil testen

Laten we eerst testen wat gebeurt er wanneer u een schil met de beveiligingscontext van plant `privileged: true`. Deze beveiligingscontext escaleert van de schil bevoegdheden. In de vorige sectie met de standaard AKS-schil beveiligingsbeleid, de *beperkte* beleid moet deze aanvraag weigeren.

Maak een bestand met de naam `nginx-privileged.yaml` en plak de volgende YAML-manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx:1.14.2
      securityContext:
        privileged: true
```

Maakt de schil via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl-nonadminuser apply -f nginx-privileged.yaml
```

De schil niet kan worden gepland, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```console
$ kubectl-nonadminuser apply -f nginx-privileged.yaml

Error from server (Forbidden): error when creating "nginx-privileged.yaml": pods "nginx-privileged" is forbidden: unable to validate against any pod security policy: [spec.containers[0].securityContext.privileged: Invalid value: true: Privileged containers are not allowed]
```

De schil niet stadium van de planning niet bereiken, zodat er geen resources om te verwijderen voordat u op verplaatsen.

## <a name="test-creation-of-an-unprivileged-pod"></a>Het maken van een niet-gemachtigde pod testen

In het vorige voorbeeld aangevraagd de specificatie pod bevoegdheden. Deze aanvraag is geweigerd door de standaard *beperkte* pod beveiligingsbeleid, zodat de schil niet kan worden gepland. We gaan nu deze dezelfde NGINX-pod zonder dat de aanvraag van de uitbreiding van bevoegdheden uitgevoerd.

Maak een bestand met de naam `nginx-unprivileged.yaml` en plak de volgende YAML-manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
```

Maakt de schil via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

De Kubernetes-scheduler accepteert de schil-aanvraag. Echter, als u de status van het gebruik van de schil kijken `kubectl get pods`, er een fout is:

```console
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS                       RESTARTS   AGE
nginx-unprivileged   0/1     CreateContainerConfigError   0          26s
```

Gebruik de [kubectl beschrijven pod] [ kubectl-describe] opdracht om te kijken naar de gebeurtenissen voor de schil. Het volgende verkorte voorbeeld ziet u dat de container en de installatiekopie moeten machtigingen voor hoofdmap, hoewel we deze niet hebt aangevraagd:

```console
$ kubectl-nonadminuser describe pod nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                     From                               Message
  ----     ------     ----                    ----                               -------
  Normal   Scheduled  7m14s                   default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged to aks-agentpool-34777077-0
  Warning  Failed     5m2s (x12 over 7m13s)   kubelet, aks-agentpool-34777077-0  Error: container has runAsNonRoot and image will run as root
  Normal   Pulled     2m10s (x25 over 7m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
```

Hoewel we niet alle bevoorrechte toegang aanvragen, de installatiekopie van de container voor NGINX moet maken van een binding van poort *80*. Poorten binden *1024* en lager, de *hoofdmap* gebruiker is vereist. Wanneer de schil probeert te starten, de *beperkte* pod beveiligingsbeleid voor deze aanvraag wordt geweigerd.

In dit voorbeeld laat zien dat het beveiligingsbeleid van de standaard pod die zijn gemaakt door AKS van kracht worden en de acties die een gebruiker kan uitvoeren beperken. Het is belangrijk om te begrijpen van de werking van deze standaardbeleid, zoals verwacht u misschien niet dat een eenvoudige NGINX-schil worden geweigerd.

Voordat u naar de volgende stap op verplaatst, verwijdert deze test pod met de [kubectl verwijderen pod] [ kubectl-delete] opdracht:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="test-creation-of-a-pod-with-a-specific-user-context"></a>Het maken van een schil met een specifieke gebruikerscontext testen

In het vorige voorbeeld is de containerinstallatiekopie automatisch heeft geprobeerd te basis NGINX binden aan poort 80 te gebruiken. Deze aanvraag is geweigerd door de standaard *beperkte* pod beveiligingsbeleid, zodat de schil niet kan worden gestart. We gaan nu uitgevoerd die dezelfde NGINX-schil met de context van een specifieke gebruiker, zoals `runAsUser: 2000`.

Maak een bestand met de naam `nginx-unprivileged-nonroot.yaml` en plak de volgende YAML-manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged-nonroot
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
      securityContext:
        runAsUser: 2000
```

Maakt de schil via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml
```

De Kubernetes-scheduler accepteert de schil-aanvraag. Echter, als u de status van het gebruik van de schil kijken `kubectl get pods`, er is een andere fout dan het vorige voorbeeld:

```console
$ kubectl-nonadminuser get pods

NAME                         READY   STATUS              RESTARTS   AGE
nginx-unprivileged-nonroot   0/1     CrashLoopBackOff    1          3s
```

Gebruik de [kubectl beschrijven pod] [ kubectl-describe] opdracht om te kijken naar de gebeurtenissen voor de schil. Het volgende verkorte voorbeeld ziet u de schil gebeurtenissen:

```console
$ kubectl-nonadminuser describe pods nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                   From                               Message
  ----     ------     ----                  ----                               -------
  Normal   Scheduled  2m14s                 default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged-nonroot to aks-agentpool-34777077-0
  Normal   Pulled     118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
  Normal   Created    118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Created container
  Normal   Started    118s (x3 over 2m12s)  kubelet, aks-agentpool-34777077-0  Started container
  Warning  BackOff    105s (x5 over 2m11s)  kubelet, aks-agentpool-34777077-0  Back-off restarting failed container
```

De gebeurtenissen geven aan dat de container is gemaakt en gestart. Er is niets direct duidelijk over waarom de schil bevindt zich in een foutstatus. Bekijk de logboeken van de schil met behulp van de [kubectl logboeken] [ kubectl-logs] opdracht:

```console
kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous
```

De volgende voorbeelduitvoer van logboek geeft een indicatie dat in de configuratie van NGINX zelf, is er een machtigingsfout optreedt wanneer de service probeert te starten. Deze fout wordt opnieuw veroorzaakt door die verbinding maken met poort 80. Hoewel de specificatie pod gedefinieerd een normaal gebruikersaccount, wordt dit gebruikersaccount niet voldoende voor de OS-niveau voor de NGINX-service om te starten en verbinding maken met de beperkte poort.

```console
$ kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous

2019/03/28 22:38:29 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2019/03/28 22:38:29 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```

Nogmaals, is het belangrijk te begrijpen van het gedrag van het beveiligingsbeleid van de standaard-schil. Deze fout is iets moeilijker te achterhalen en opnieuw, verwacht u misschien niet dat een eenvoudige NGINX-schil worden geweigerd.

Voordat u naar de volgende stap op verplaatst, verwijdert deze test pod met de [kubectl verwijderen pod] [ kubectl-delete] opdracht:

```console
kubectl-nonadminuser delete -f nginx-unprivileged-nonroot.yaml
```

## <a name="create-a-custom-pod-security-policy"></a>Een aangepaste pod-beveiligingsbeleid maken

Nu dat u het gedrag van het beveiligingsbeleid van de standaard-schil hebt gezien, we bieden een manier voor het *nonadmin gebruiker* schillen met succes te plannen.

We maken een beleid voor het weigeren van schillen die bevoorrechte toegang aanvragen. Andere opties, zoals *uitvoerenals* of toegestaan *volumes*, worden niet expliciet is beperkt. Dit type beleid een aanvraag voor bevoegde toegang wordt geweigerd, maar kunt anders het cluster de aangevraagde schillen uitgevoerd.

Maak een bestand met de naam `psp-deny-privileged.yaml` en plak de volgende YAML-manifest:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: psp-deny-privileged
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
  - '*'
```

Maakt het beleid met behulp van de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl apply -f psp-deny-privileged.yaml
```

Als u wilt weergeven van de beschikbare beleidsregels, gebruikt u de [kubectl ophalen psp] [ kubectl-get] opdracht, zoals wordt weergegeven in het volgende voorbeeld. Vergelijk de *psp weigeren-bevoegdheden* beleid met de standaard *beperkte* beleid dat is afgedwongen in de vorige voorbeelden om te maken van een schil. Alleen het gebruik van *PRIV* escalatie is geweigerd door uw beleid. Er zijn geen beperkingen voor de gebruiker of groep voor de *psp weigeren-bevoegdheden* beleid.

```console
$ kubectl get psp

NAME                  PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged            true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
psp-deny-privileged   false          RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted            false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

## <a name="allow-user-account-to-use-the-custom-pod-security-policy"></a>Gebruikersaccount te gebruiken van het beveiligingsbeleid van de aangepaste pod toestaan

In de vorige stap hebt u een pod beveiligingsbeleid gemaakt voor het weigeren van schillen die aanvraag bevoegde toegang. Als u wilt toestaan dat het beleid moet worden gebruikt, maakt u een *rol* of een *ClusterRole*. Vervolgens koppelt u een van deze rollen met behulp van een *RoleBinding* of *ClusterRoleBinding*.

In dit voorbeeld maakt u een ClusterRole waarmee u *gebruiken* de *psp weigeren-bevoegdheden* beleid in de vorige stap hebt gemaakt. Maak een bestand met de naam `psp-deny-privileged-clusterrole.yaml` en plak de volgende YAML-manifest:

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-deny-privileged-clusterrole
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - psp-deny-privileged
  verbs:
  - use
```

Maakt de ClusterRole via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl apply -f psp-deny-privileged-clusterrole.yaml
```

Maak nu een ClusterRoleBinding voor het gebruik van de ClusterRole in de vorige stap hebt gemaakt. Maak een bestand met de naam `psp-deny-privileged-clusterrolebinding.yaml` en plak de volgende YAML-manifest:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: psp-deny-privileged-clusterrolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp-deny-privileged-clusterrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:serviceaccounts
```

Maak een ClusterRoleBinding met de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl apply -f psp-deny-privileged-clusterrolebinding.yaml
```

> [!NOTE]
> In de eerste stap van dit artikel worden de schil beveiligingsfunctie voor het beleid is ingeschakeld op het AKS-cluster. De aanbevolen procedure is alleen de schil beleid beveiligingsfunctie inschakelen nadat u uw eigen beleid hebt gedefinieerd. Dit is het stadium waar u de schil beveiliging beleid-functie wilt inschakelen. Een of meer aangepaste beleidsregels zijn gedefinieerd en gebruikersaccounts zijn gekoppeld aan dit beleid. Nu u veilig het beveiligingsbeleid pod kunt presenteren en op problemen als gevolg van de standaardbeleidsregels minimaliseren.

## <a name="test-the-creation-of-an-unprivileged-pod-again"></a>Het maken van een niet-gemachtigde pod opnieuw testen

Met uw aangepaste pod-beveiligingsbeleid toegepast en een binding voor het gebruikersaccount dat u het beleid, we proberen een niet-gemachtigde pod opnieuw maken. Gebruik dezelfde `nginx-privileged.yaml` manifest het maken van de schil met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

De schil is gepland. Wanneer u de status van het gebruik van de schil controleren de [kubectl ophalen schillen] [ kubectl-get] opdracht, de schil is *met*:

```
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          7m14s
```

In dit voorbeeld laat zien hoe u aangepaste pod beveiligingsbeleid om te definiëren de toegang tot het AKS-cluster voor verschillende gebruikers of groepen kunt maken. De standaardwaarde AKS-beleid biedt u krachtige besturingselementen op welke schillen kunnen uitvoeren, dus uw eigen aangepaste beleidsregels voor het correct definiëren vervolgens de beperkingen die u moet maken.

Verwijderen van de niet-gemachtigd NGINX pod met behulp de [kubectl verwijderen] [ kubectl-delete] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt uitschakelen pod-beveiligingsbeleid, gebruiken de [az aks bijwerken] [ az-aks-update] opdracht opnieuw uit. Het volgende voorbeeld wordt uitgeschakeld pod beveiligingsbeleid op de clusternaam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --disable-pod-security-policy
```

Verwijder vervolgens de ClusterRole en ClusterRoleBinding:

```console
kubectl delete -f psp-deny-privileged-clusterrolebinding.yaml
kubectl delete -f psp-deny-privileged-clusterrole.yaml
```

Verwijderen van het netwerk beleid met [kubectl verwijderen] [ kubectl-delete] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl delete -f psp-deny-privileged.yaml
```

Ten slotte verwijdert de *psp aks* naamruimte:

```console
kubectl delete namespace psp-aks
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd u het maken van een beveiligingsbeleid schil om te voorkomen dat het gebruik van bevoegde toegang. Er zijn tal van functies die een beleid, zoals het type van het volume of de RunAs-gebruiker afdwingen kunt. Zie voor meer informatie over de beschikbare opties, de [Kubernetes-schil beveiliging beleid-referentiedocumenten][kubernetes-policy-reference].

Zie voor meer informatie over het beperken van netwerkverkeer pod [beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in AKS][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubernetes-policy-reference]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#policy-reference
[aks-github]: https://github.com/azure/aks/issues

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
