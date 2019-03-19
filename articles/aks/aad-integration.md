---
title: Azure Active Directory integreren met Azure Kubernetes Service
description: Over het maken van Azure Active Directory-functionaliteit Azure Kubernetes Service (AKS)-clusters.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/09/2018
ms.author: iainfou
ms.openlocfilehash: 0cf83180647c142c9db2a1229674de96fec6a6bb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087530"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Active Directory integreren met Azure Kubernetes Service

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. In deze configuratie kunt u zich aanmelden bij een AKS-cluster met behulp van uw Azure Active Directory-verificatietoken. Bovendien, clusterbeheerders kunnen Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) op basis van lidmaatschap van een gebruikers-id of de map configureren.

Dit artikel leest u hoe u implementeert de vereisten voor AKS en Azure AD en over het implementeren van een Azure AD-functionaliteit-cluster en een eenvoudige RBAC-rol in het AKS-cluster maken.

Er gelden de volgende beperkingen:

- Azure AD kan alleen worden ingeschakeld wanneer u een nieuwe, RBAC-functionaliteit cluster maakt. U kunt Azure AD op een bestaand AKS-cluster niet inschakelen.
- *Gast* gebruikers in Azure AD, bijvoorbeeld als u van een federatieve aanmelding vanuit een andere map gebruikmaakt, worden niet ondersteund.

## <a name="authentication-details"></a>Verificatiegegevens

Azure AD-verificatie wordt geleverd met AKS-clusters met OpenID Connect. OpenID Connect is een identiteitslaag is gebaseerd op het OAuth 2.0-protocol. Zie voor meer informatie over het OpenID Connect, de [Open ID connect documentatie][open-id-connect].

Uit binnen het Kubernetes-cluster, wordt Webhook-tokenverificatie gebruikt om te controleren of de verificatietokens. Webhook-tokenverificatie is geconfigureerd en beheerd als onderdeel van het AKS-cluster. Zie voor meer informatie over Webhook-tokenverificatie de [webhook verificatie documentatie][kubernetes-webhook].

> [!NOTE]
> Wanneer u Azure AD voor de AKS-verificatie configureert, worden twee Azure AD-toepassing geconfigureerd. Deze bewerking moet worden voltooid door de beheerder van de Azure-tenant.

## <a name="create-server-application"></a>Server-toepassing maken

De eerste Azure AD-toepassing wordt gebruikt om op te halen van een groepslidmaatschap voor Azure AD van gebruikers.

1. Selecteer achtereenvolgens **Azure Active Directory** > **App-registraties** > **Registratie van nieuwe toepassing**.

   Geef de toepassing een naam, selecteer **Web-app / API** voor het toepassingstype, en voer een waarde van de URI-indeling voor **aanmeldings-URL**. Selecteer **maken** wanneer u klaar bent.

   ![Maken van Azure AD-registratie](media/aad-integration/app-registration.png)

2. Selecteer **Manifest** en bewerk de `groupMembershipClaims` waarde die moet worden `"All"`.

   Opslaan van de updates als u klaar bent.

   ![Groepslidmaatschap op Alles bijwerken](media/aad-integration/edit-manifest.png)

3. Selecteer terug op de Azure AD-toepassing **instellingen** > **sleutels**.

   Een beschrijving van de sleutel toevoegen, schakelt u een vervaldatum en selecteer **opslaan**. Noteer de sleutelwaarde. Wanneer de AKS-cluster implementeren van een Azure AD worden ingeschakeld, deze waarde wordt aangeduid als de `Server application secret`.

   ![De persoonlijke sleutel van de toepassing ophalen](media/aad-integration/application-key.png)

4. Ga terug naar de Azure AD-toepassing, selecteer **instellingen** > **vereiste machtigingen** > **toevoegen**  >   **Een API selecteren** > **Microsoft Graph** > **Selecteer**.

   ![Graph API selecteren](media/aad-integration/graph-api.png)

5. Onder **TOEPASSINGSMACHTIGINGEN** zet een vinkje naast **mapgegevens lezen**.

   ![Graph-machtigingen van de toepassing instellen](media/aad-integration/read-directory.png)

6. Onder **GEDELEGEERDE MACHTIGINGEN**, schakel het selectievakje naast **aanmelden en gebruikersprofiel lezen** en **mapgegevens lezen**. Sla de updates die één keer uitgevoerd.

   ![Graph-machtigingen van de toepassing instellen](media/aad-integration/delegated-permissions.png)

   Selecteer **Done**.

7. Kies *Microsoft Graph* uit de lijst met API's, selecteert u vervolgens **machtigingen verlenen**. Deze stap mislukt als het huidige account niet een tenantbeheerder is.

   ![Graph-machtigingen van de toepassing instellen](media/aad-integration/grant-permissions.png)

   Wanneer de machtigingen hebt verleend, wordt het volgende bericht weergegeven in de portal:

   ![Kennisgeving van geslaagde machtigingen](media/aad-integration/permissions-granted.png)

8. Ga terug naar de toepassing en noteer de **toepassings-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Server application ID`.

   ![Toepassings-ID ophalen](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>Client-toepassing maken

De tweede Azure AD-toepassing wordt gebruikt wanneer u zich aan met de Kubernetes-CLI (kubectl).

1. Selecteer achtereenvolgens **Azure Active Directory** > **App-registraties** > **Registratie van nieuwe toepassing**.

   Geef de toepassing een naam, selecteer **systeemeigen** voor het toepassingstype, en voer een opgemaakte URI-waarde voor **omleidings-URI**. Selecteer **maken** wanneer u klaar bent.

   ![AAD-registratie maken](media/aad-integration/app-registration-client.png)

2. Selecteer in de Azure AD-toepassing **instellingen** > **vereiste machtigingen** > **toevoegen** > **selecteert u een API** en zoek de naam van de server-toepassing in de laatste stap van dit document hebt gemaakt.

   ![Machtigingen van de toepassing configureren](media/aad-integration/select-api.png)

3. Schakel het selectievakje naast de toepassing en klik op **Selecteer**.

   ![AKS AAD-servereindpunt toepassing selecteren](media/aad-integration/select-server-app.png)

   Selecteer **gedaan**

4. Selecteer uw API-server in de lijst en kies vervolgens **machtigingen verlenen**:

   ![Machtigingen verlenen](media/aad-integration/grant-permissions-client.png)

5. Terug op de AD-toepassing en noteer de **toepassings-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Client application ID`.

   ![De toepassings-ID ophalen](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Tenant-id ophalen

Ten slotte, haal de ID van uw Azure-tenant. Deze waarde wordt ook gebruikt bij het implementeren van het AKS-cluster.

Selecteer in de Azure-portal **Azure Active Directory** > **eigenschappen** en noteer de **map-ID**. Wanneer u een Azure AD-functionaliteit AKS-cluster implementeert, deze waarde wordt aangeduid als de `Tenant ID`.

![De Azure-tenant-ID ophalen](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Cluster implementeren

Gebruik de [az-groep maken] [ az-group-create] opdracht om een resourcegroep voor het AKS-cluster te maken.

```azurecli
az group create --name myResourceGroup --location eastus
```

Implementeer het cluster met de [az aks maken] [ az-aks-create] opdracht. Vervang de waarden in de voorbeeldopdracht hieronder door de waarden die worden verzameld bij het maken van de Azure AD-toepassingen.

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

## <a name="create-rbac-binding"></a>RBAC-binding maken

Voordat u een Azure Active Directory-account kan worden gebruikt met het AKS-cluster, moet een binding van de rol of een cluster rol binding worden gemaakt. *Rollen* definiëren de machtigingen te verlenen en *bindingen* toepassen op de gewenste gebruikers. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][rbac-authorization].

Gebruik eerst de [az aks get-credentials] [ az-aks-get-credentials] opdracht met de `--admin` argument voor aanmelding bij het cluster met beheerderstoegang.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Gebruik vervolgens de volgende manifest te maken van een ClusterRoleBinding voor een Azure AD-account. In dit voorbeeld geeft de account volledige toegang tot alle naamruimten van het cluster. Maak een bestand, zoals *rbac-aad-user.yaml*, en plak de volgende inhoud. Update voor de naam van de gebruiker met een van uw Azure AD-tenant:

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
  name: "user@contoso.com"
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

Nadat een kubectl-opdracht is uitgevoerd, wordt u gevraagd om te verifiëren met Azure. Volg de aanwijzingen op het scherm instructies.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-1   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-2   Ready     agent     1h        v1.9.9
```

Als u klaar bent, wordt het verificatietoken in cache opgeslagen. U wordt alleen reprompted om aan te melden wanneer het token is verlopen of het Kubernetes-configuratiebestand opnieuw wordt gemaakt.

Als u een autorisatiefoutbericht ziet na de aanmelding is, controleert u of:
1. De gebruiker u zich aanmeldt is niet een gast in de Azure AD-exemplaar (dit is vaak het geval als u van een federatieve aanmelding vanuit een andere map gebruikmaakt).
2. De gebruiker is geen lid van meer dan 200 groepen.

```console
error: You must be logged in to the server (Unauthorized)
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beveiligen van Kubernetes-clusters met RBAC met de [RBAC-autorisatie met behulp van] [ rbac-authorization] documentatie.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
