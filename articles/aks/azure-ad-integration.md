---
title: Azure Active Directory integreren met Azure Kubernetes Service
description: Over het maken van clusters met Azure Active Directory-functionaliteit Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 04/26/2019
ms.author: mlearned
ms.openlocfilehash: 80137023643630e8472a70fcca6cb656aeba7123
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616393"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Active Directory integreren met Azure Kubernetes Service

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory (Azure AD) voor verificatie van de gebruiker. In deze configuratie, kunt u aanmelden bij een AKS-cluster met behulp van uw Azure AD-verificatietoken.

Clusterbeheerders kunnen Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) op basis van lidmaatschap van een gebruiker identiteit of directory configureren.

Dit artikel wordt uitgelegd hoe u:

- Implementeer de vereisten voor AKS en Azure AD.
- Een Azure AD-functionaliteit-cluster implementeren.
- Maak een eenvoudige RBAC-rol in het AKS-cluster met behulp van de Azure-portal.

U kunt deze stappen ook uitvoeren met behulp van de [Azure CLI][azure-ad-cli].

> [!NOTE]
> Azure AD kan alleen worden ingeschakeld wanneer u een nieuw cluster met RBAC-functionaliteit maakt. U kunt Azure AD op een bestaand AKS-cluster niet inschakelen.

## <a name="authentication-details"></a>Verificatiegegevens

Azure AD-verificatie wordt geleverd met AKS-clusters met OpenID Connect. OpenID Connect is een identiteitslaag is gebaseerd op het OAuth 2.0-protocol.

Zie voor meer informatie over het OpenID Connect, [toegang verlenen aan webtoepassingen met OpenID Connect en Azure AD][open-id-connect].

Webhook-tokenverificatie wordt in een Kubernetes-cluster gebruikt om verificatietokens. Webhook-tokenverificatie is geconfigureerd en beheerd als onderdeel van het AKS-cluster.

Zie voor meer informatie over webhook-tokenverificatie, de [Webhook-tokenverificatie][kubernetes-webhook] sectie in het Kubernetes-documentatie.

Voor Azure AD-verificatie voor een AKS-cluster, worden twee Azure AD-toepassingen gemaakt. De eerste toepassing is een serveronderdeel waarmee de verificatie van de gebruiker. De tweede toepassing is een client-component die wordt gebruikt wanneer u wordt gevraagd door de CLI voor verificatie. Deze clienttoepassing maakt gebruik van de servertoepassing voor de werkelijke verificatie van de referenties die zijn opgegeven door de client.

> [!NOTE]
> Wanneer u Azure AD voor de AKS-verificatie configureert, worden twee Azure AD-toepassingen zijn geconfigureerd. De stappen voor het overdragen van machtigingen voor elke toepassing moeten worden voltooid door de beheerder van de Azure-tenant.

## <a name="create-the-server-application"></a>De servertoepassing maken

De eerste Azure AD-toepassing wordt toegepast om het lidmaatschap van een gebruiker van Azure AD ophalen. Deze toepassing maken in Azure portal:

1. Selecteer **Azure Active Directory** > **App-registraties** > **registratie van nieuwe**.

    a. Geef de toepassing een naam, zoals *AKSAzureADServer*.

    b. Voor **ondersteund accounttypen**, selecteer **Accounts in deze organisatie-map alleen**.
    
    c. Kies **Web** voor de omleidings-URI typt, en voer een waarde URI-indeling, zoals *https://aksazureadserver* .

    d. Selecteer **registreren** wanneer u klaar bent.

2. Selecteer **Manifest**, en bewerk vervolgens de **groupMembershipClaims:** als de waarde **alle**. Wanneer u klaar met de updates bent, selecteert u **opslaan**.

    ![Groepslidmaatschap op Alles bijwerken](media/aad-integration/edit-manifest.png)

3. Selecteer in het linkerdeelvenster van de Azure AD-toepassing, **certificaten en geheimen**.

    a. Selecteer **+ nieuwe clientgeheim**.

    b. Een beschrijving van de sleutel, zoals toevoegen *AKS Azure AD server*. Kies een verlooptijd en selecteer vervolgens **toevoegen**.

    c. Houd er rekening mee de sleutelwaarde die alleen op dit moment wordt weergegeven. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, wordt deze waarde het servergeheim van de toepassing genoemd.

4. Selecteer in het linkerdeelvenster van de Azure AD-toepassing, **API-machtigingen**, en selecteer vervolgens **+ toevoegen van een machtiging**.

    a. Onder **Microsoft APIs**, selecteer **Microsoft Graph**.

    b. Selecteer **overgedragen machtigingen**, en selecteer vervolgens het selectievakje in naast **Directory > Directory.Read.All (directory-gegevens lezen)** .

    c. Als een standaard de machtiging voor gedelegeerd **gebruiker > User.Read (aanmelden en gebruikersprofiel lezen)** niet bestaat, schakel het selectievakje ernaast.

    d. Selecteer **Toepassingsmachtigingen**, en selecteer vervolgens het selectievakje in naast **Directory > Directory.Read.All (directory-gegevens lezen)** .

    ![Graph-machtigingen instellen](media/aad-integration/graph-permissions.png)

    e. Selecteer **machtigingen toevoegen** om op te slaan van de updates.

    f. Onder **toestemming verlenen**, selecteer **verlenen van toestemming van een beheerder**. Deze knop is niet beschikbaar als het huidige account is niet een tenantbeheerder.

    Wanneer de machtigingen zijn verleend, wordt het volgende bericht weergegeven in de portal:

   ![Kennisgeving van geslaagde machtigingen](media/aad-integration/permissions-granted.png)

5. Selecteer in het linkerdeelvenster van de Azure AD-toepassing, **beschikbaar maken van een API**, en selecteer vervolgens **+ toevoegen van een scope**.
    
    a. Voer een **scopenaam**, een **beheerder toestemming weergavenaam**, en vervolgens een **beschrijving van de beheerderstoestemming** zoals *AKSAzureADServer*.

    b. Zorg ervoor dat **status** is ingesteld op **ingeschakeld**.

    ![De serverapp als een API voor gebruik met andere services beschikbaar](media/aad-integration/expose-api.png)

    c. Selecteer **bereik toevoegen**.

6. Ga terug naar de toepassing **overzicht** pagina en noteer de **(client) toepassings-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde is met de naam de server-toepassings-ID.

    ![Toepassings-ID ophalen](media/aad-integration/application-id.png)

## <a name="create-the-client-application"></a>De clienttoepassing maken

De tweede Azure AD-toepassing wordt gebruikt wanneer u zich aanmelden met de Kubernetes-CLI (kubectl).

1. Selecteer **Azure Active Directory** > **App-registraties** > **registratie van nieuwe**.

    a. Geef de toepassing een naam, zoals *AKSAzureADClient*.

    b. Voor **ondersteund accounttypen**, selecteer **Accounts in deze organisatie-map alleen**.

    c. Selecteer **Web** voor de omleidings-URI, klikt u vervolgens te typen en een willekeurige waarde URI-indeling, zoals *https://aksazureadclient* .

    d. Selecteer **registreren** wanneer u klaar bent.

2. Selecteer in het linkerdeelvenster van de Azure AD-toepassing, **API-machtigingen**, en selecteer vervolgens **+ toevoegen van een machtiging**.

    a. Selecteer **mijn API's**, en kies vervolgens de Azure AD-servertoepassing in de vorige stap hebt gemaakt zoals *AKSAzureADServer*.

    b. Selecteer **overgedragen machtigingen**, en selecteer vervolgens het selectievakje in naast uw Azure AD server-app.

    ![Machtigingen van de toepassing configureren](media/aad-integration/select-api.png)

    c. Selecteer **machtigingen toevoegen**.

    d. Onder **toestemming verlenen**, selecteer **verlenen van toestemming van een beheerder**. Deze knop is niet beschikbaar als het huidige account niet een tenantbeheerder. Wanneer de machtigingen zijn verleend, wordt het volgende bericht weergegeven in de portal:

    ![Kennisgeving van geslaagde machtigingen](media/aad-integration/permissions-granted.png)

3. Selecteer in het linkerdeelvenster van de Azure AD-toepassing, **verificatie**.

    - Onder **standaard clienttype**, selecteer **Ja** naar **behandelen van de client als een openbare client**.

5. In het linkerdeelvenster van de Azure AD-toepassing, houd er rekening mee de toepassings-ID. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde heet de client-toepassings-ID.

   ![De toepassings-ID ophalen](media/aad-integration/application-id-client.png)

## <a name="get-the-tenant-id"></a>Haal de tenant-ID

Haal vervolgens de ID van uw Azure-tenant. Deze waarde wordt gebruikt bij het maken van het AKS-cluster.

Selecteer in de Azure-portal **Azure Active Directory** > **eigenschappen** en noteer de **map-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster maakt, deze waarde heet de tenant-ID.

![De Azure-tenant-ID ophalen](media/aad-integration/tenant-id.png)

## <a name="deploy-the-aks-cluster"></a>Het AKS-cluster implementeren

Gebruik de [az-groep maken][az-group-create] opdracht om een resourcegroep voor het AKS-cluster te maken.

```azurecli
az group create --name myResourceGroup --location eastus
```

Gebruik de [az aks maken][az-aks-create] opdracht voor het implementeren van het AKS-cluster. Vervang vervolgens de waarden in de volgende voorbeeldopdracht. Gebruik de waarden die worden verzameld tijdens het maken van de Azure AD-toepassingen voor de server app-ID, app-geheim, client-app-ID en tenant-ID.

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

Een AKS-cluster duurt een paar minuten om te maken.

## <a name="create-an-rbac-binding"></a>Een RBAC-binding maken

Voordat u een Azure Active Directory-account met een AKS-cluster gebruiken, moet u de rol-binding of cluster rol-binding maken. Rollen definiëren de machtigingen te verlenen en bindingen toegepast op de gewenste gebruikers. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][rbac-authorization].

Gebruik eerst de [az aks get-credentials][az-aks-get-credentials] opdracht met de `--admin` argument zich aanmeldt bij het cluster met beheerderstoegang.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Maak vervolgens ClusterRoleBinding voor een Azure AD-account dat u wilt toegang verlenen tot het AKS-cluster. Het volgende voorbeeld geeft de account volledige toegang tot alle naamruimten in het cluster:

- Als de gebruiker die u toewijst, dat de RBAC-binding voor zich in dezelfde Azure AD-tenant, moet u machtigingen op basis van de UPN (User Principal Name) toewijzen. Gaat u met de stap aan het YAML-manifest voor ClusterRoleBinding maken.

- Als de gebruiker in een andere Azure AD-tenant, zoeken en gebruiken de **objectId** eigenschap in plaats daarvan. Indien nodig, de object-id van het vereiste gebruikersaccount ophalen met behulp van de [az ad-gebruiker weergeven][az-ad-user-show] opdracht. Geef de user principal name (UPN) van het vereiste account:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

Maak een bestand, zoals *rbac-aad-user.yaml*, en plak de volgende inhoud. Vervang in de laatste regel **userPrincipalName_or_objectId** met de UPN of object-ID. De keuze is afhankelijk van of de gebruiker dezelfde Azure AD-tenant of niet is.

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

Het toepassen van de binding met behulp van de [kubectl toepassen][kubectl-apply] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl apply -f rbac-aad-user.yaml
```

De binding van een rol kan ook worden gemaakt voor alle leden van een Azure AD-groep. Azure AD-groepen zijn opgegeven met behulp van het groepsobject-ID, zoals wordt weergegeven in het volgende voorbeeld.

Maak een bestand, zoals *rbac-aad-group.yaml*, en plak de volgende inhoud. Het groepsobject-ID bijwerken met een van uw Azure AD-tenant:

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

Het toepassen van de binding met behulp van de [kubectl toepassen][kubectl-apply] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl apply -f rbac-aad-group.yaml
```

Zie voor meer informatie over het beveiligen van een Kubernetes-cluster met RBAC [RBAC-autorisatie met behulp van][rbac-authorization].

## <a name="access-the-cluster-with-azure-ad"></a>Toegang tot het cluster met Azure AD

De context voor de niet-beheerdersaccount ophalen met behulp van de [az aks get-credentials][az-aks-get-credentials] opdracht.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Nadat u de `kubectl` opdracht, wordt u gevraagd om te verifiëren met behulp van Azure. Volg de aanwijzingen op het scherm instructies voor het voltooien van het proces, zoals wordt weergegeven in het volgende voorbeeld:

```console
$ kubectl get nodes

To sign in, use a web browser to open https://microsoft.com/devicelogin. Next, enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

Wanneer het proces is voltooid, wordt het verificatietoken in cache opgeslagen. U wordt gevraagd alleen opnieuw aanmelden bij het token is verlopen, of het Kubernetes-configuratiebestand opnieuw worden gemaakt is.

Als u een autorisatiefoutbericht ziet nadat u aanmelden, controleert u de volgende criteria:

```console
error: You must be logged in to the server (Unauthorized)
```


- U hebt de juiste object-ID of UPN, afhankelijk van of het gebruikersaccount dat in dezelfde Azure AD-tenant is of niet gedefinieerd.
- De gebruiker geen lid is van meer dan 200 groepen.
- Het geheim die is gedefinieerd in de registratie van de toepassing voor de server overeenkomt met de waarde die is geconfigureerd met behulp van `--aad-server-app-secret`.

## <a name="next-steps"></a>Volgende stappen

Voor het gebruik van Azure AD-gebruikers en groepen om toegang tot clusterresources te beheren, Zie [toegang tot resources met behulp van op rollen gebaseerd toegangsbeheer en Azure AD-identiteiten in AKS-cluster beheren][azure-ad-rbac].

Zie voor meer informatie over hoe u veilige Kubernetes-clusters, [opties voor toegang en identiteit voor AKS][rbac-authorization].

Zie voor meer informatie over identiteits- en resource-besturingselement, [aanbevolen procedures voor verificatie en autorisatie in AKS][operator-best-practices-identity].

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
