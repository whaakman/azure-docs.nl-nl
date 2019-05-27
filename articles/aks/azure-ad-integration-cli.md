---
title: Azure Active Directory integreren met Azure Kubernetes Service
description: Informatie over het gebruik van de Azure CLI voor het maken en de Azure Active Directory-functionaliteit Azure Kubernetes Service (AKS)-cluster
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/16/2019
ms.author: iainfou
ms.openlocfilehash: d80ad5abecc968a9fe3c82d62ddd8577856a3c54
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835191"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service-using-the-azure-cli"></a>Azure Active Directory integreren met Azure Kubernetes Service met behulp van de Azure CLI

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. In deze configuratie kunt u zich aanmelden bij een AKS-cluster met behulp van een Azure AD-verificatietoken. Clusteroperators kunnen ook configureren voor Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) op basis van lidmaatschap van een gebruiker-id of de map.

Dit artikel leest u over het maken van de vereiste onderdelen voor Azure AD, implementeert u vervolgens een Azure AD-functionaliteit-cluster en een eenvoudige RBAC-rol in het AKS-cluster maken. U kunt ook [deze stappen met behulp van de Azure-portal][azure-ad-portal].

Zie voor de volledige voorbeeldscript in dit artikel, [voorbeelden van Azure CLI - AKS-integratie met Azure AD][complete-script].

Er gelden de volgende beperkingen:

- Azure AD kan alleen worden ingeschakeld wanneer u een nieuwe, RBAC-functionaliteit cluster maakt. U kunt Azure AD op een bestaand AKS-cluster niet inschakelen.

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

Voor consistentie en om u te helpen bij het uitvoeren van de opdrachten in dit artikel, maakt u een variabele voor de gewenste naam voor de AKS-cluster. Het volgende voorbeeld wordt de naam van de *myakscluster*:

```azurecli-interactive
aksname="myakscluster"
```

## <a name="azure-ad-authentication-overview"></a>Overzicht van Azure AD-verificatie

Azure AD-verificatie wordt geleverd met AKS-clusters met OpenID Connect. OpenID Connect is een identiteitslaag is gebaseerd op het OAuth 2.0-protocol. Zie voor meer informatie over het OpenID Connect, de [Open ID connect documentatie][open-id-connect].

Uit binnen het Kubernetes-cluster, wordt Webhook-tokenverificatie gebruikt om te controleren of de verificatietokens. Webhook-tokenverificatie is geconfigureerd en beheerd als onderdeel van het AKS-cluster. Zie voor meer informatie over Webhook-tokenverificatie de [webhook verificatie documentatie][kubernetes-webhook].

> [!NOTE]
> Wanneer u Azure AD voor de AKS-verificatie configureert, worden twee Azure AD-toepassing geconfigureerd. Deze bewerking moet worden voltooid door de beheerder van de Azure-tenant.

## <a name="create-azure-ad-server-component"></a>Maken van Azure AD-serveronderdeel

Als u wilt integreren met AKS, maken en gebruiken van een Azure AD-toepassing die als een eindpunt voor de identiteit-aanvragen fungeert. De eerste Azure AD-toepassing u moet Hiermee haalt u het lidmaatschap van Azure AD voor een gebruiker.

Maken van de server toepassing onderdeel met de [az ad app maken] [ az-ad-app-create] opdracht en vervolgens update het lidmaatschap van de claims met behulp van de [az ad app update] [ az-ad-app-update] opdracht. Het volgende voorbeeld wordt de *aksname* variabele gedefinieerd de [voordat u begint met](#before-you-begin) uit en maakt u een variabele

```azurecli-interactive
# Create the Azure AD application
serverApplicationId=$(az ad app create \
    --display-name "${aksname}Server" \
    --identifier-uris "https://${aksname}Server" \
    --query appId -o tsv)

# Update the application group memebership claims
az ad app update --id $serverApplicationId --set groupMembershipClaims=All
```

Maak nu een service-principal voor de server-app via de [az ad sp maken] [ az-ad-sp-create] opdracht. Deze service-principal wordt gebruikt voor verificatie zelf binnen het Azure-platform. Vervolgens kunt u de service principal geheim met behulp de [az ad sp referentie opnieuw] [ az-ad-sp-credential-reset] beheren en toewijzen aan de variabele met de naam *serverApplicationSecret* voor gebruik in een van de volgende stappen uit:

```azurecli-interactive
# Create a service principal for the Azure AD application
az ad sp create --id $serverApplicationId

# Get the service principal secret
serverApplicationSecret=$(az ad sp credential reset \
    --name $serverApplicationId \
    --credential-description "AKSPassword" \
    --query password -o tsv)
```

De Azure AD moet toegangsmachtigingen voor de volgende acties uitvoeren:

* Adreslijstgegevens lezen
* Aanmelden en gebruikersprofiel lezen

Deze machtigingen met behulp van de [az ad app-machtiging toevoegen] [ az-ad-app-permission-add] opdracht:

```azurecli-interactive
az ad app permission add \
    --id $serverApplicationId \
    --api 00000003-0000-0000-c000-000000000000 \
    --api-permissions e1fe6dd8-ba31-4d61-89e7-88639da4683d=Scope 06da0dbc-49e2-44d2-8312-53f166ab848a=Scope 7ab1d382-f21e-4acd-a863-ba3e13f7da61=Role
```

Ten slotte de machtigingen toegewezen in de vorige stap voor de server-toepassing met de [az ad app machtiging grant] [ az-ad-app-permission-grant] opdracht. Deze stap mislukt als het huidige account niet een tenantbeheerder is. U moet ook machtigingen voor Azure AD-toepassing om informatie die anders mogelijk met behulp van toestemming van een beheerder te vragen toevoegen de [az ad app machtiging-beheerderstoestemming][az-ad-app-permission-admin-consent]:

```azurecli-interactive
az ad app permission grant --id $serverApplicationId --api 00000003-0000-0000-c000-000000000000
az ad app permission admin-consent --id  $serverApplicationId
```

## <a name="create-azure-ad-client-component"></a>Maken van Azure AD-clientonderdeel

De tweede Azure AD-toepassing wordt gebruikt wanneer een gebruiker meldt zich aan het AKS-cluster met de Kubernetes-CLI (`kubectl`). Deze clienttoepassing neemt de verificatieaanvraag van de gebruiker en controleert of de referenties en machtigingen. Maken van de Azure AD-app voor de client onderdeel met de [az ad app maken] [ az-ad-app-create] opdracht:

```azurecli-interactive
clientApplicationId=$(az ad app create \
    --display-name "${aksname}Client" \
    --native-app \
    --reply-urls "https://${aksname}Client" \
    --query appId -o tsv)
```

Maken van een service-principal voor de client-toepassing met de [az ad sp maken] [ az-ad-sp-create] opdracht:

```azurecli-interactive
az ad sp create --id $clientApplicationId
```

Haal de oAuth2-ID voor de serverapp waarmee de verificatie-stroom tussen de twee app-onderdelen met behulp van de [az ad app show] [ az-ad-app-show] opdracht. Deze oAuth2-ID wordt gebruikt in de volgende stap.

```azurecli-interactive
oAuthPermissionId=$(az ad app show --id $serverApplicationId --query "oauth2Permissions[0].id" -o tsv)
```

De machtigingen voor de clienttoepassing toevoegen en onderdelen van de server-toepassing voor gebruik van de oAuth2-clientcommunicatie stromen met behulp van de [az ad app-machtiging toevoegen] [ az-ad-app-permission-add] opdracht. Vervolgens verleent machtigingen voor de clienttoepassing voor communicatie met de server-toepassing met de [az ad app machtiging grant] [ az-ad-app-permission-grant] opdracht:

```azurecli-interactive
az ad app permission add --id $clientApplicationId --api $serverApplicationId --api-permissions $oAuthPermissionId=Scope
az ad app permission grant --id $clientApplicationId --api $serverApplicationId
```

## <a name="deploy-the-cluster"></a>Het cluster implementeren

Met de twee Azure AD-toepassingen die zijn gemaakt, moet u nu het AKS-cluster zelf maken. Maak eerst een resource-groep met de [az-groep maken] [ az-group-create] opdracht. Het volgende voorbeeld wordt de resourcegroep in de *EastUS* regio:

Maak een resourcegroep voor het cluster:

```azurecli-interactive
az group create --name myResourceGroup --location EastUS
```

Haal de tenant-ID van uw Azure-abonnement met de [az account show] [ az-account-show] opdracht. Maak vervolgens de AKS-cluster met de [az aks maken] [ az-aks-create] opdracht. De opdracht voor het maken van het AKS-cluster bevat de server en client-id's van toepassing, de server application service principal-geheim en uw tenant-ID:

```azurecli-interactive
tenantId=$(az account show --query tenantId -o tsv)

az aks create \
    --resource-group myResourceGroup \
    --name $aksname \
    --node-count 1 \
    --generate-ssh-keys \
    --aad-server-app-id $serverApplicationId \
    --aad-server-app-secret $serverApplicationSecret \
    --aad-client-app-id $clientApplicationId \
    --aad-tenant-id $tenantId
```

Ten slotte het cluster ophalen met behulp van beheerdersreferenties de [az aks get-credentials] [ az-aks-get-credentials] opdracht. In een van de volgende stappen, krijgt u de normale *gebruiker* cluster-referenties om te zien van de Azure AD-verificatie stroom in actie.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --admin
```

## <a name="create-rbac-binding"></a>RBAC-binding maken

Voordat u een Azure Active Directory-account kan worden gebruikt met het AKS-cluster, moet een binding van de rol of een cluster rol binding worden gemaakt. *Rollen* definiëren de machtigingen te verlenen en *bindingen* toepassen op de gewenste gebruikers. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][rbac-authorization].

Ophalen van de UPN (User Principal Name) voor de gebruiker die momenteel aangemeld met behulp van de [az ad ondertekend-in-user show] [ az-ad-signed-in-user-show] opdracht. Dit gebruikersaccount is ingeschakeld voor Azure AD-integratie in de volgende stap.

```azurecli-interactive
az ad signed-in-user show --query userPrincipalName -o tsv
```

> [!IMPORTANT]
> Als de gebruiker verlenen van de binding RBAC voor zich in dezelfde Azure AD-tenant, worden de machtigingen die op basis van de *userPrincipalName*. Als de gebruiker in een andere Azure AD-tenant, zoeken en gebruiken de *objectId* eigenschap in plaats daarvan.

Maken van een YAML-manifest met de naam `basic-azure-ad-binding.yaml` en plak de volgende inhoud. Vervang in de laatste regel *userPrincipalName_or_objectId* met de UPN of object-ID-uitvoer van de vorige opdracht:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userPrincipalName_or_objectId
```

Maakt de ClusterRoleBinding via de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de bestandsnaam van uw YAML-manifest:

```console
kubectl apply -f basic-azure-ad-binding.yaml
```

## <a name="access-cluster-with-azure-ad"></a>RAS-cluster met Azure AD

Nu gaan we testen de integratie van Azure AD-verificatie voor het AKS-cluster. Stel de `kubectl` config context reguliere gebruikersreferenties gebruiken. Deze context wordt doorgegeven voor alle verificatieaanvragen terug via Azure AD.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --overwrite-existing
```

Nu gebruik van de [kubectl ophalen schillen] [ kubectl-get] opdracht om schillen in alle naamruimten weer te geven:

```console
kubectl get pods --all-namespaces
```

U ontvangt een teken in vraag om te verifiëren met behulp van Azure AD-referenties via een webbrowser. Nadat u hebt geverifieerd, de `kubectl` opdracht geeft u de schillen in de AKS-cluster, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```console
$ kubectl get pods --all-namespaces

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BYMK7UXVD to authenticate.

NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   coredns-754f947b4-2v75r                 1/1     Running   0          23h
kube-system   coredns-754f947b4-tghwh                 1/1     Running   0          23h
kube-system   coredns-autoscaler-6fcdb7d64-4wkvp      1/1     Running   0          23h
kube-system   heapster-5fb7488d97-t5wzk               2/2     Running   0          23h
kube-system   kube-proxy-2nd5m                        1/1     Running   0          23h
kube-system   kube-svc-redirect-swp9r                 2/2     Running   0          23h
kube-system   kubernetes-dashboard-847bb4ddc6-trt7m   1/1     Running   0          23h
kube-system   metrics-server-7b97f9cd9-btxzz          1/1     Running   0          23h
kube-system   tunnelfront-6ff887cffb-xkfmq            1/1     Running   0          23h
```

Het verificatietoken voor ontvangen `kubectl` opgeslagen in de cache. U wordt alleen reprompted te melden wanneer het token is verlopen of het Kubernetes-configuratiebestand opnieuw gemaakt is.

Als u een autorisatiefoutbericht nadat u hebt aangemeld met een webbrowser zoals in de volgende voorbeelduitvoer ziet, controleert u de volgende problemen:

```console
error: You must be logged in to the server (Unauthorized)
```

* U hebt de juiste object-ID of UPN, afhankelijk van of het gebruikersaccount dat in dezelfde Azure AD-tenant is of niet gedefinieerd.
* De gebruiker is geen lid van meer dan 200 groepen.
* Geheim dat is gedefinieerd in de registratie van de toepassing voor de server overeenkomt met de waarde die is geconfigureerd met behulp van `--aad-server-app-secret`

## <a name="next-steps"></a>Volgende stappen

Zie voor het volledige script met de opdrachten in dit artikel wordt weergegeven, de [opslagplaats-voorbeelden voor Azure AD-integratiescript in de AKS][complete-script].

Voor het gebruik van Azure AD-gebruikers en groepen om toegang tot clusterresources te beheren, Zie [toegang tot resources met behulp van op rollen gebaseerd toegangsbeheer en Azure AD-identiteiten in AKS-cluster beheren][azure-ad-rbac].

Zie voor meer informatie over het beveiligen van Kubernetes-clusters [opties voor toegang en identiteit voor AKS)][rbac-authorization].

Zie voor best practices voor identiteits- en resource-besturingselement [aanbevolen procedures voor verificatie en autorisatie in AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[complete-script]: https://github.com/Azure-Samples/azure-cli-samples/tree/master/aks/azure-ad-integration/azure-ad-integration.sh

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-ad-app-create]: /cli/azure/ad/app#az-ad-app-create
[az-ad-app-update]: /cli/azure/ad/app#az-ad-app-update
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create
[az-ad-app-permission-add]: /cli/azure/ad/app/permission#az-ad-app-permission-add
[az-ad-app-permission-grant]: /cli/azure/ad/app/permission#az-ad-app-permission-grant
[az-ad-app-permission-admin-consent]: /cli/azure/ad/app/permission#az-ad-app-permission-admin-consent
[az-ad-app-show]: /cli/azure/ad/app#az-ad-app-show
[az-group-create]: /cli/azure/group#az-group-create
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-signed-in-user-show]: /cli/azure/ad/signed-in-user#az-ad-signed-in-user-show
[azure-ad-portal]: azure-ad-integration.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
