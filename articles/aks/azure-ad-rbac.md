---
title: Beheer van clusterresources met RBAC en Azure AD in Azure Kubernetes Service
description: Informatie over het gebruik van Azure Active Directory-groepslidmaatschap om toegang te beperken tot clusterresources met behulp van op rollen gebaseerd toegangsbeheer (RBAC) in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/16/2019
ms.author: iainfou
ms.openlocfilehash: e974c47d1dfb04f66b622c64a7143d00de87c4cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467541"
---
# <a name="control-access-to-cluster-resources-using-role-based-access-control-and-azure-active-directory-identities-in-azure-kubernetes-service"></a>Toegang tot resources van het cluster met behulp van op rollen gebaseerd toegangsbeheer en Azure Active Directory-identiteiten in Azure Kubernetes Service beheren

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. In deze configuratie moet zich u aanmeldt bij een AKS-cluster met behulp van een Azure AD-verificatietoken. U kunt ook configureren met Kubernetes op rollen gebaseerd toegangsbeheer (RBAC om toegang tot clusterresources te beperken) op basis van een gebruiker id of een groepslidmaatschap.

Dit artikel ziet u hoe u het lidmaatschap van Azure AD voor toegangsbeheer voor naamruimten en cluster-resources met behulp van Kubernetes RBAC in een AKS-cluster. Voorbeeld van de groepen en gebruikers worden gemaakt in Azure AD en functies en RoleBindings worden gemaakt in de AKS-cluster naar de juiste machtigingen maken en weergeven van resources.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster met Azure AD-integratie is ingeschakeld hebt. Als u een cluster AKS nodig hebt, raadpleegt u [Integreer Azure Active Directory met AKS][azure-ad-aks-cli].

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

## <a name="create-demo-groups-in-azure-ad"></a>Demo-groepen maken in Azure AD

In dit artikel gaan we twee gebruikersrol maken die kunnen worden gebruikt om weer te geven hoe Kubernetes RBAC en Azure AD toegang tot clusterresources beheren. Het volgende voorbeeld van de twee rollen worden gebruikt:

* **Toepassingsontwikkelaar**
    * Een gebruiker met de naam *aksdev* die deel uitmaakt van de *appdev* groep.
* **Site betrouwbaarheid engineer**
    * Een gebruiker met de naam *akssre* die deel uitmaakt van de *opssre* groep.

In een productieomgeving, kunt u bestaande gebruikers en groepen binnen een Azure AD-tenant.

Haal eerst de resource-ID van uw AKS-cluster met de [az aks show] [ az-aks-show] opdracht. De resource-ID toewijzen aan een variabele met de naam *AKS_ID* zodat deze kan worden verwezen in de aanvullende opdrachten.

```azurecli-interactive
AKS_ID=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query id -o tsv)
```

Het eerste voorbeeld-groep maken in Azure AD voor de ontwikkelaars van toepassingen met behulp van de [az ad-groep maken] [ az-ad-group-create] opdracht. Het volgende voorbeeld wordt een groep met de naam *appdev*:

```azurecli-interactive
APPDEV_ID=$(az ad group create --display-name appdev --mail-nickname appdev --query objectId -o tsv)
```

Maak nu een Azure roltoewijzing voor de *appdev* groeperen met behulp van de [az roltoewijzing maken] [ az-role-assignment-create] opdracht. Deze toewijzing kunt elk lid van de groep gebruiken `kubectl` om te communiceren met een AKS-cluster door toe te kennen ze de *Azure Kubernetes Service-Cluster-gebruikersrol*.

```azurecli-interactive
az role assignment create \
  --assignee $APPDEV_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

> [!TIP]
> Als u een foutbericht, zoals ontvangt `Principal 35bfec9328bd4d8d9b54dea6dac57b82 does not exist in the directory a5443dcd-cd0e-494d-a387-3039b419f0d5.`, wacht een paar seconden voor de Azure AD-object-ID aan doorgegeven via de map en probeer het vervolgens de `az role assignment create` opdracht opnieuw uit.

Maak een tweede voorbeeld-groep, deze één voor SREs met de naam *opssre*:

```azurecli-interactive
OPSSRE_ID=$(az ad group create --display-name opssre --mail-nickname opssre --query objectId -o tsv)
```

Opnieuw maken van een Azure roltoewijzing voor het verlenen van leden van de groep de *Azure Kubernetes Service-Cluster-gebruikersrol*:

```azurecli-interactive
az role assignment create \
  --assignee $OPSSRE_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

## <a name="create-demo-users-in-azure-ad"></a>Van demogebruikers maken in Azure AD

Met twee voorbeeld-groepen gemaakt in Azure AD voor onze ontwikkelaars van toepassingen en SREs, moet u nu kunt maken van twee voorbeeldgebruikers. Als u wilt testen de RBAC-integratie aan het einde van het artikel, aanmelden u bij de AKS-cluster met deze accounts.

De eerste gebruikersaccount maken in Azure AD via de [az ad-gebruiker maken] [ az-ad-user-create] opdracht.

Het volgende voorbeeld wordt een gebruiker met de weergavenaam *AKS Dev* en de user principal name (UPN) van `aksdev@contoso.com`. Bijwerken van de UPN om op te nemen van een geverifieerd domein voor uw Azure AD-tenant (Vervang *contoso.com* met uw eigen domein), en geef uw eigen veilige `--password` referentie:

```azurecli-interactive
AKSDEV_ID=$(az ad user create \
  --display-name "AKS Dev" \
  --user-principal-name aksdev@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)
```

Nu u toevoegen aan de *appdev* groep gemaakt in de vorige sectie met de [az ad group lid toevoegen] [ az-ad-group-member-add] opdracht:

```azurecli-interactive
az ad group member add --group appdev --member-id $AKSDEV_ID
```

Maak een tweede gebruikersaccount. Het volgende voorbeeld wordt een gebruiker met de weergavenaam *AKS SRE* en de user principal name (UPN) van `akssre@contoso.com`. Nogmaals, werken de UPN om op te nemen van een geverifieerd domein voor uw Azure AD-tenant (Vervang *contoso.com* met uw eigen domein), en geef uw eigen veilige `--password` referentie:

```azurecli-interactive
# Create a user for the SRE role
AKSSRE_ID=$(az ad user create \
  --display-name "AKS SRE" \
  --user-principal-name akssre@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)

# Add the user to the opssre Azure AD group
az ad group member add --group opssre --member-id $AKSSRE_ID
```

## <a name="create-the-aks-cluster-resources-for-app-devs"></a>De AKS-cluster-resources voor app-ontwikkelaars

De Azure AD-groepen en gebruikers worden nu gemaakt. Azure-roltoewijzingen zijn voor de leden van de groep verbinding maken met een AKS-cluster als een gewone gebruiker gemaakt. Nu gaan we het AKS-cluster voor deze verschillende groepen toegang tot bepaalde resources configureren.

Haal eerst de cluster-beheerdersreferenties met behulp van de [az aks get-credentials] [ az-aks-get-credentials] opdracht. In een van de volgende secties, krijgt u de normale *gebruiker* cluster-referenties om te zien van de Azure AD-verificatie stroom in actie.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Een naamruimte maken in de AKS-cluster met de [kubectl-naamruimte maken] [ kubectl-create] opdracht. Het volgende voorbeeld wordt de naam van een naamruimte *dev*:

```console
kubectl create namespace dev
```

In Kubernetes, *rollen* definiëren de machtigingen te verlenen en *RoleBindings* toepassen op de gewenste gebruikers of groepen. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][rbac-authorization].

Maak eerst een rol voor de *dev* naamruimte. Deze rol hebben volledige machtigingen voor de naamruimte. In een productieomgeving, kunt u meer gedetailleerde machtigingen voor verschillende gebruikers of groepen opgeven.

Maak een bestand met de naam `role-dev-namespace.yaml` en plak de volgende YAML-manifest:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-user-full-access
  namespace: dev
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

Maakt de rol via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de bestandsnaam van uw YAML-manifest:

```console
kubectl apply -f role-dev-namespace.yaml
```

Vervolgens ophalen van de resource-ID voor de *appdev* groeperen met behulp van de [az ad group show] [ az-ad-group-show] opdracht. Deze groep is ingesteld als het onderwerp van een RoleBinding in de volgende stap.

```azurecli-interactive
az ad group show --group appdev --query objectId -o tsv
```

Maak nu een RoleBinding voor de *appdev* groep met de eerder gemaakte rol voor toegang tot de naamruimte. Maak een bestand met de naam `rolebinding-dev-namespace.yaml` en plak de volgende YAML-manifest. Vervang in de laatste regel *groupObjectId* met de uitvoer groep object-ID van de vorige opdracht:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-user-access
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: dev-user-full-access
subjects:
- kind: Group
  namespace: dev
  name: groupObjectId
```

Maakt de RoleBinding via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de bestandsnaam van uw YAML-manifest:

```console
kubectl apply -f rolebinding-dev-namespace.yaml
```

## <a name="create-the-aks-cluster-resources-for-sres"></a>De resources van het AKS-cluster maken voor SREs

Nu, Herhaal de vorige stappen voor het maken van een naamruimte, deze rol, en RoleBinding voor de SREs.

Maak eerst een naamruimte voor *sre* met behulp van de [kubectl-naamruimte maken] [ kubectl-create] opdracht:

```console
kubectl create namespace sre
```

Maak een bestand met de naam `role-sre-namespace.yaml` en plak de volgende YAML-manifest:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: sre-user-full-access
  namespace: sre
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

Maakt de rol via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de bestandsnaam van uw YAML-manifest:

```console
kubectl apply -f role-sre-namespace.yaml
```

Haal de resource-ID voor de *opssre* groeperen met behulp van de [az ad group show] [ az-ad-group-show] opdracht:

```azurecli-interactive
az ad group show --group opssre --query objectId -o tsv
```

Maken van een RoleBinding voor de *opssre* groep met de eerder gemaakte rol voor toegang tot de naamruimte. Maak een bestand met de naam `rolebinding-sre-namespace.yaml` en plak de volgende YAML-manifest. Vervang in de laatste regel *groupObjectId* met de uitvoer groep object-ID van de vorige opdracht:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: sre-user-access
  namespace: sre
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: sre-user-full-access
subjects:
- kind: Group
  namespace: sre
  name: groupObjectId
```

Maakt de RoleBinding via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de bestandsnaam van uw YAML-manifest:

```console
kubectl apply -f rolebinding-sre-namespace.yaml
```

## <a name="interact-with-cluster-resources-using-azure-ad-identities"></a>Communiceren met de clusterbronnen met behulp van Azure AD-identiteiten

Nu gaan we testen van het verwachte machtigingen werk bij het maken en beheren van resources in een AKS-cluster. In deze voorbeelden die u kunt plannen en pods weergeven in de toegewezen naamruimte van de gebruiker. Vervolgens kunt proberen u te planning en weergave schillen buiten de toegewezen naamruimte.

Eerst opnieuw instellen van de *kubeconfig* context met behulp van de [az aks get-credentials] [ az-aks-get-credentials] opdracht. In een vorige sectie stelt u de context met behulp van de cluster-beheerdersreferenties. De gebruiker met beheerdersrechten omzeilt aanwijzingen voor het aanmelden van Azure AD. Zonder de `--admin` parameter, de gebruikerscontext waarvoor alle aanvragen moeten worden geverifieerd met behulp van Azure AD wordt toegepast.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Schema voor een eenvoudige NGINX pod met de [kubectl uitvoeren] [ kubectl-run] opdracht in de *dev* naamruimte:

```console
kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev
```

Als de prompt voor aanmelden, voert u de referenties voor uw eigen `appdev@contoso.com` account aan het begin van het artikel gemaakt. Nadat u bent aangemeld, de account-token is opgeslagen in de cache voor toekomstige `kubectl` opdrachten. De NGINX is met succes plannen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code B24ZD6FP8 to authenticate.

pod/nginx-dev created
```

Nu gebruik van de [kubectl ophalen schillen] [ kubectl-get] opdracht om weer te geven van de schillen in de *dev* naamruimte.

```console
kubectl get pods --namespace dev
```

Zoals weergegeven in de volgende voorbeelduitvoer, de NGINX-schil is *met*:

```console
$ kubectl get pods --namespace dev

NAME        READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          4m
```

### <a name="create-and-view-cluster-resources-outside-of-the-assigned-namespace"></a>Maken en weergeven van clusterbronnen buiten de toegewezen naamruimte

Nu proberen om weer te geven schillen buiten de *dev* naamruimte. Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht nogmaals, deze tijd om te zien `--all-namespaces` als volgt:

```console
kubectl get pods --all-namespaces
```

Lidmaatschap van de gebruiker heeft geen een Kubernetes-rol waarmee deze actie, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```console
$ kubectl get pods --all-namespaces

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot list resource "pods" in API group "" at the cluster scope
```

Op dezelfde manier, probeer het plannen van een schil in verschillende naamruimte, zoals de *sre* naamruimte. Lidmaatschap van de gebruiker komt niet overeen met een Kubernetes-rol en RoleBinding om deze machtigingen te verlenen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace sre

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot create resource "pods" in API group "" in the namespace "sre"
```

### <a name="test-the-sre-access-to-the-aks-cluster-resources"></a>Test de SRE toegang tot de resources van het AKS-cluster

Om te bevestigen dat onze groepslidmaatschap voor Azure AD en Kubernetes RBAC correct tussen verschillende gebruikers en groepen werken, probeert u de vorige opdrachten wanneer u bent aangemeld als de *opssre* gebruiker.

Opnieuw instellen van de *kubeconfig* context met behulp van de [az aks get-credentials] [ az-aks-get-credentials] opdracht die wordt gewist van het eerder in de cache verificatietoken voor de *aksdev*  gebruiker:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Probeer te plannen en weergave schillen in de toegewezen *sre* naamruimte. Wanneer u hierom wordt gevraagd, meldt u zich aan met uw eigen `opssre@contoso.com` referenties aan het begin van het artikel gemaakt:

```console
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre
kubectl get pods --namespace sre
```

Zoals weergegeven in de volgende voorbeelduitvoer, kunt u maken en de pods weergeven:

```console
$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BM4RHP3FD to authenticate.

pod/nginx-sre created

$ kubectl get pods --namespace sre

NAME        READY   STATUS    RESTARTS   AGE
nginx-sre   1/1     Running   0
```

Probeer nu om te bekijken of schillen buiten toegewezen SRE naamruimte plannen:

```console
kubectl get pods --all-namespaces
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
```

Deze `kubectl` opdrachten mislukken, zoals wordt weergegeven in de volgende voorbeelduitvoer. Groepslidmaatschap en Kubernetes-rol en RoleBindings van de gebruiker niet verlenen aan machtigingen voor het maken of manager-resources in andere naamruimten:

```console
$ kubectl get pods --all-namespaces
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot list pods at the cluster scope

$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot create pods in the namespace "dev"
```

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel hebt u resources in de AKS-cluster en de gebruikers en groepen in Azure AD gemaakt. Als u wilt opschonen al deze resources, voer de volgende opdrachten:

```azurecli-interactive
# Get the admin kubeconfig context to delete the necessary cluster resources
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin

# Delete the dev and sre namespaces. This also deletes the pods, Roles, and RoleBindings
kubectl delete namespace dev
kubectl delete namespace sre

# Delete the Azure AD user accounts for aksdev and akssre
az ad user delete --upn-or-object-id $AKSDEV_ID
az ad user delete --upn-or-object-id $AKSSRE_ID

# Delete the Azure AD groups for appdev and opssre. This also deletes the Azure role assignments.
az ad group delete --group appdev
az ad group delete --group opssre
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beveiligen van Kubernetes-clusters [opties voor toegang en identiteit voor AKS)][rbac-authorization].

Zie voor best practices voor identiteits- en resource-besturingselement [aanbevolen procedures voor verificatie en autorisatie in AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-run]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-ad-aks-cli]: azure-ad-integration-cli.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-ad-group-create]: /cli/azure/ad/group#az-ad-group-create
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-ad-user-create]: /cli/azure/ad/user#az-ad-user-create
[az-ad-group-member-add]: /cli/azure/ad/group/member#az-ad-group-member-add
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
