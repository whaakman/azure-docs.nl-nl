---
title: Azure Active Directory integreren met Azure Kubernetes Service
description: Over het maken van Azure Active Directory-functionaliteit Azure Kubernetes Service (AKS)-clusters.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/26/2019
ms.author: iainfou
ms.openlocfilehash: a6ed8ec37a3b20ccdbd2b013ba308518d8e3b97c
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849879"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Active Directory integreren met Azure Kubernetes Service

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. In deze configuratie kunt u zich aanmelden bij een AKS-cluster met behulp van uw Azure Active Directory-verificatietoken. Bovendien, clusterbeheerders kunnen Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) op basis van lidmaatschap van een gebruiker identiteit of directory configureren.

Dit artikel leest u over het implementeren van de vereisten voor AKS en Azure AD en over het implementeren van een Azure AD-functionaliteit-cluster en het maken van een eenvoudige RBAC-rol in de AKS-cluster met behulp van de Azure portal. U kunt ook [deze stappen met behulp van de Azure CLI][azure-ad-cli].

Er gelden de volgende beperkingen:

- Azure AD kan alleen worden ingeschakeld wanneer u een nieuwe, RBAC-functionaliteit cluster maakt. U kunt Azure AD op een bestaand AKS-cluster niet inschakelen.

## <a name="authentication-details"></a>Verificatiegegevens

Azure AD-verificatie wordt geleverd met AKS-clusters met OpenID Connect. OpenID Connect is een identiteitslaag is gebaseerd op het OAuth 2.0-protocol. Zie voor meer informatie over het OpenID Connect, de [Open ID connect documentatie][open-id-connect].

Uit binnen het Kubernetes-cluster, wordt Webhook-tokenverificatie gebruikt om te controleren of de verificatietokens. Webhook-tokenverificatie is geconfigureerd en beheerd als onderdeel van het AKS-cluster. Zie voor meer informatie over Webhook-tokenverificatie de [webhook verificatie documentatie][kubernetes-webhook].

Voor Azure AD-verificatie voor een AKS-cluster, worden twee Azure AD-toepassingen gemaakt. De eerste toepassing is een serveronderdeel waarmee de verificatie van de gebruiker. De tweede toepassing is een clientonderdeel dat wordt gebruikt wanneer u wordt gevraagd door de CLI voor verificatie. Deze clienttoepassing maakt gebruik van de servertoepassing voor de werkelijke verificatie van de referenties die zijn opgegeven door de client.

> [!NOTE]
> Wanneer u Azure AD voor de AKS-verificatie configureert, worden twee Azure AD-toepassing geconfigureerd. De stappen voor het overdragen van machtigingen voor elk van de toepassingen die moeten worden uitgevoerd door een Azure-tenant-beheerder.

## <a name="create-server-application"></a>Server-toepassing maken

De eerste Azure AD-toepassing wordt gebruikt om op te halen van een groepslidmaatschap voor Azure AD van gebruikers. Deze toepassing maken in Azure portal.

1. Selecteer **Azure Active Directory** > **App-registraties** > **registratie van nieuwe**.

    * Geef de toepassing een naam, zoals *AKSAzureADServer*.
    * Voor **ondersteund accounttypen**, kiest u *Accounts in deze organisatie-map alleen*.
    * Kies *Web* voor de **omleidings-URI** typt, en voer een opgemaakte URI-waarde, zoals *https://aksazureadserver*.
    * Selecteer **registreren** wanneer u klaar bent.

1. Selecteer **Manifest** en bewerk de `groupMembershipClaims` waarde die moet worden `"All"`.

    ![Groepslidmaatschap op Alles bijwerken](media/aad-integration/edit-manifest.png)

    **Sla** de updates als u klaar bent.

1. Selecteer op de navigatiebalk links van de Azure AD-toepassing, **certificaten en geheimen**.

    * Kies **+ nieuwe clientgeheim**.
    * Een beschrijving van de sleutel, zoals toevoegen *AKS Azure AD server*. Kies een verlooptijd, en selecteer vervolgens **toevoegen**.
    * Noteer de sleutelwaarde. Dit is alleen deze initiële tijd weergegeven. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Server application secret`.

1. Selecteer op de navigatiebalk links van de Azure AD-toepassing, **API-machtigingen**, kiest u voor **+ toevoegen van een machtiging**.

    * Onder **Microsoft APIs**, kiest u *Microsoft Graph*.
    * Kies **overgedragen machtigingen**, schakel het selectievakje naast **Directory > Directory.Read.All (directory-gegevens lezen)**.
        * Als een standaard de machtiging voor gedelegeerd **gebruiker > User.Read (aanmelden en gebruikersprofiel lezen)** niet bestaat, schakel het selectievakje Deze machtiging.
    * Kies **Toepassingsmachtigingen**, schakel het selectievakje naast **Directory > Directory.Read.All (directory-gegevens lezen)**.

        ![Graph-machtigingen instellen](media/aad-integration/graph-permissions.png)

    * Kies **machtigingen toevoegen** om op te slaan van de updates.

    * Onder de **toestemming verlenen** sectie, ervoor kiezen om te **verlenen van toestemming van een beheerder**. Deze knop wordt grijs is en is niet beschikbaar als het huidige account niet een tenantbeheerder is.

        Wanneer de machtigingen hebt verleend, wordt het volgende bericht weergegeven in de portal:

        ![Kennisgeving van geslaagde machtigingen](media/aad-integration/permissions-granted.png)

1. Selecteer op de navigatiebalk links van de Azure AD-toepassing, **beschikbaar maken van een API**, kiest u voor **+ toevoegen van een scope**.
    
    * Stel een *scopenaam*, *beheerder toestemming weergavenaam*, en *beschrijving van de beheerderstoestemming*, zoals *AKSAzureADServer*.
    * Zorg ervoor dat de **status** is ingesteld op *ingeschakeld*.

        ![De serverapp als een API voor gebruik met andere services beschikbaar](media/aad-integration/expose-api.png)

    * Kies **bereik toevoegen**.

1. Ga terug naar de toepassing **overzicht** pagina en noteer de **(client) toepassings-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Server application ID`.

   ![Toepassings-ID ophalen](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>Client-toepassing maken

De tweede Azure AD-toepassing wordt gebruikt wanneer u zich aan met de Kubernetes-CLI (`kubectl`).

1. Selecteer **Azure Active Directory** > **App-registraties** > **registratie van nieuwe**.

    * Geef de toepassing een naam, zoals *AKSAzureADClient*.
    * Voor **ondersteund accounttypen**, kiest u *Accounts in deze organisatie-map alleen*.
    * Kies *Web* voor de **omleidings-URI** typt, en voer een opgemaakte URI-waarde, zoals *https://aksazureadclient*.
    * Selecteer **registreren** wanneer u klaar bent.

1. Selecteer op de navigatiebalk links van de Azure AD-toepassing, **API-machtigingen**, kiest u voor **+ toevoegen van een machtiging**.

    * Selecteer **mijn API's**, kiest u uw Azure AD-server-toepassing in de vorige stap hebt gemaakt zoals *AKSAzureADServer*.
    * Kies **overgedragen machtigingen**, klikt u vervolgens zet een vinkje naast uw Azure AD server-app.

        ![Machtigingen van de toepassing configureren](media/aad-integration/select-api.png)

    * Selecteer **machtigingen toevoegen**.

    * Onder de **toestemming verlenen** sectie, ervoor kiezen om te **verlenen van toestemming van een beheerder**. Deze knop wordt grijs is en is niet beschikbaar als het huidige account niet een tenantbeheerder is.

        Wanneer de machtigingen hebt verleend, wordt het volgende bericht weergegeven in de portal:

        ![Kennisgeving van geslaagde machtigingen](media/aad-integration/permissions-granted.png)

1. Selecteer op de navigatiebalk links van de Azure AD-toepassing, **verificatie**.

    * Onder **standaard clienttype**, selecteer **Ja** naar *behandelen van de client als een openbare client*.

1. Op de navigatiebalk links van de Azure AD-toepassing, noteer de **toepassings-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Client application ID`.

   ![De toepassings-ID ophalen](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Tenant-id ophalen

Ten slotte, haal de ID van uw Azure-tenant. Deze waarde wordt gebruikt bij het maken van het AKS-cluster.

Selecteer in de Azure-portal **Azure Active Directory** > **eigenschappen** en noteer de **map-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster maakt, deze waarde wordt aangeduid als de `Tenant ID`.

![De Azure-tenant-ID ophalen](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Cluster implementeren

Gebruik de [az-groep maken] [ az-group-create] opdracht om een resourcegroep voor het AKS-cluster te maken.

```azurecli
az group create --name myResourceGroup --location eastus
```

Implementeer het cluster met de [az aks maken] [ az-aks-create] opdracht. Vervang de waarden in de voorbeeldopdracht hieronder door de waarden die worden verzameld bij het maken van de Azure AD-toepassingen voor de server app-ID en geheim, client-app-ID en tenant-ID:

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

Het duurt een paar minuten om de AKS-cluster te maken.

## <a name="create-rbac-binding"></a>RBAC-binding maken

Voordat u een Azure Active Directory-account kan worden gebruikt met het AKS-cluster, moet een binding van de rol of een cluster rol binding worden gemaakt. *Rollen* definiëren de machtigingen te verlenen en *bindingen* toepassen op de gewenste gebruikers. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][rbac-authorization].

Gebruik eerst de [az aks get-credentials] [ az-aks-get-credentials] opdracht met de `--admin` argument zich aanmeldt bij het cluster met beheerderstoegang.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Maak vervolgens een ClusterRoleBinding voor een Azure AD-account dat u wilt toegang verlenen tot het AKS-cluster. Het volgende voorbeeld geeft de account volledige toegang tot alle naamruimten in het cluster.

- Als de gebruiker die u toewijst, dat de RBAC-binding voor zich in dezelfde Azure AD-tenant, moet u machtigingen op basis van de UPN (User Principal Name) toewijzen. Gaat u met de stap aan het YAML-manifest voor de ClusterRuleBinding maken.

- Als de gebruiker in een andere Azure AD-tenant, zoeken en gebruiken de *objectId* eigenschap in plaats daarvan. Indien nodig, krijgen de *objectId* account met behulp van de gebruiker de [az ad-gebruiker weergeven] [ az-ad-user-show] opdracht. Geef de user principal name (UPN) van het vereiste account:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

Maak een bestand, zoals *rbac-aad-user.yaml*, en plak de volgende inhoud. Vervang in de laatste regel *userPrincipalName_or_objectId* met de UPN of object-ID afhankelijk van of de gebruiker dezelfde Azure AD-tenant is of niet.

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

De binding met behulp van de toepassing de [kubectl toepassen] [ kubectl-apply] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl apply -f rbac-aad-user.yaml
```

De binding van een rol kan ook worden gemaakt voor alle leden van een Azure AD-groep. Azure AD-groepen zijn opgegeven met behulp van het groepsobject-ID, zoals wordt weergegeven in het volgende voorbeeld. Maak een bestand, zoals *rbac-aad-group.yaml*, en plak de volgende inhoud. Het groepsobject-ID bijwerken met een van uw Azure AD-tenant:

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
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

De binding met behulp van de toepassing de [kubectl toepassen] [ kubectl-apply] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl apply -f rbac-aad-group.yaml
```

Zie voor meer informatie over het beveiligen van een Kubernetes-cluster met RBAC [RBAC-autorisatie met behulp van][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>RAS-cluster met Azure AD

Vervolgens halen de context voor de niet-beheerder-gebruiker met de [az aks get-credentials] [ az-aks-get-credentials] opdracht.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Nadat u een `kubectl` opdracht, wordt u gevraagd om u te verifiëren met Azure. Volg de aanwijzingen op het scherm instructies voor het voltooien van het proces, zoals weergegeven in het volgende voorbeeld:

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

Als u klaar bent, wordt het verificatietoken in cache opgeslagen. U wordt alleen reprompted aan te melden wanneer het token is verlopen of het Kubernetes-configuratiebestand opnieuw wordt gemaakt.

Als u een autorisatiefoutbericht ziet na de aanmelding is, controleert u of:

```console
error: You must be logged in to the server (Unauthorized)
```

1. U hebt de juiste object-ID of UPN, afhankelijk van of het gebruikersaccount dat in dezelfde Azure AD-tenant is of niet gedefinieerd.
2. De gebruiker is geen lid van meer dan 200 groepen.
3. Geheim dat is gedefinieerd in de registratie van de toepassing voor de server overeenkomt met de waarde die is geconfigureerd met behulp van `--aad-server-app-secret`

## <a name="next-steps"></a>Volgende stappen

Voor het gebruik van Azure AD-gebruikers en groepen om toegang tot clusterresources te beheren, Zie [toegang tot resources met behulp van op rollen gebaseerd toegangsbeheer en Azure AD-identiteiten in AKS-cluster beheren][azure-ad-rbac].

Zie voor meer informatie over het beveiligen van Kubernetes-clusters [opties voor toegang en identiteit voor AKS)][rbac-authorization].

Zie voor best practices voor identiteits- en resource-besturingselement [aanbevolen procedures voor verificatie en autorisatie in AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
