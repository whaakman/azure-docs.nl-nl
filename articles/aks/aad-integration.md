---
title: Azure Active Directory integreren met Azure Kubernetes-Service
description: Het maken van Azure Active Directory-functionaliteit Azure Kubernetes Service-clusters.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/17/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 272d98613e13c1bb76c75befd6bd5e0115c32610
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097237"
---
# <a name="integrate-azure-active-directory-with-aks---preview"></a>Azure Active Directory integreren met AKS - Preview

Azure Kubernetes Service (AKS) kunnen worden geconfigureerd voor het gebruik van Azure Active Directory voor gebruikersverificatie. In deze configuratie kunt u zich aanmelden bij een Azure Kubernetes Service-cluster met behulp van uw Azure Active Directory-verificatietoken. Bovendien kunnen clusterbeheerders Kubernetes op rollen gebaseerde toegangsbeheer configureren op basis van lidmaatschap van een gebruikers-id of de map.

In dit document worden alle vereisten voor de AKS en Azure AD maken, het implementeren van een Azure AD-functionaliteit-cluster en het maken van een eenvoudige RBAC-rol in het cluster AKS.

> [!IMPORTANT]
> Azure RBAC Kubernetes Service (AKS) en Azure AD-integratie is geopend in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.
>

## <a name="authentication-details"></a>Verificatie-details

Azure AD-verificatie wordt geleverd aan Azure Kubernetes clusters met OpenID Connect. OpenID Connect is een identiteitlaag gebouwd op het OAuth 2.0-protocol. Meer informatie over het OpenID Connect vindt u in de [Open ID verbinding documentatie][open-id-connect].

Van binnen het cluster Kubernetes Webhook tokenverificatie wordt gebruikt om te controleren of verificatietokens. Webhook tokenverificatie is geconfigureerd en beheerd als onderdeel van het cluster AKS. Meer informatie over Webhook tokenverificatie vindt u in de [webhook verificatie documentatie][kubernetes-webhook].

> [!NOTE]
> Wanneer u Azure AD voor AKS authenticatie configureert, worden twee Azure AD-toepassing geconfigureerd. Deze bewerking moet worden voltooid door de beheerder van de Azure-tenant.

## <a name="create-server-application"></a>Server-toepassing maken

De eerste Azure AD-toepassing wordt gebruikt voor het ophalen van een Azure AD het lidmaatschap van gebruikers.

1. Selecteer achtereenvolgens **Azure Active Directory** > **App-registraties** > **Registratie van nieuwe toepassing**.

  Geef de toepassing een naam, selecteer **Web-app / API** voor het toepassingstype, en voer de waarde van een URI-indeling voor **aanmeldings-URL**. Selecteer **maken** wanneer u klaar bent.

  ![Maken van Azure AD-registratie](media/aad-integration/app-registration.png)

2. Selecteer **Manifest** en bewerkt u de `groupMembershipClaims` van waarde naar `"All"`.

  Opslaan van de updates als u klaar.

  ![Groepslidmaatschap voor alle bijwerken](media/aad-integration/edit-manifest.png)

3. Selecteer terug op de Azure AD-toepassing **instellingen** > **sleutels**.

  Een beschrijving van de sleutel toevoegen, selecteert u een deadline voor de vervaldatum en selecteer **opslaan**. Noteer de sleutelwaarde. Wanneer het implementeren van een Azure AD AKS cluster ingeschakeld, deze waarde wordt aangeduid als de `Server application secret`.

  ![De persoonlijke sleutel van de toepassing ophalen](media/aad-integration/application-key.png)

4. Ga terug naar de Azure AD-toepassing, selecteer **instellingen** > **vereist machtigingen** > **toevoegen**  >   **Selecteer een API** > **Microsoft Graph** > **Selecteer**.

  Onder **TOEPASSINGSMACHTIGINGEN** zet een vinkje naast **mapgegevens lezen**.

  ![Grafiek-toepassing worden machtigingen ingesteld](media/aad-integration/read-directory.png)

5. Onder **GEDELEGEERDE MACHTIGINGEN**, zet een vinkje naast **aanmelden en gebruikersprofiel lezen** en **mapgegevens lezen**. Sla de updates die zijn gedaan.

  ![Grafiek-toepassing worden machtigingen ingesteld](media/aad-integration/delegated-permissions.png)

6. Selecteer **gedaan** en **machtiging verlenen** om deze stap te voltooien. Deze stap mislukt als de huidige account niet een tenantbeheerder is.

  ![Grafiek-toepassing worden machtigingen ingesteld](media/aad-integration/grant-permissions.png)

7. Ga terug naar de toepassing en noteer de **toepassings-ID**. Bij het implementeren van een cluster met Azure AD-functionaliteit AKS deze waarde wordt aangeduid als de `Server application ID`.

  ![Toepassings-ID ophalen](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>Client-toepassing maken

De tweede Azure AD-toepassing wordt gebruikt wanneer u zich heeft aangemeld met de Kubernetes CLI (kubectl).

1. Selecteer achtereenvolgens **Azure Active Directory** > **App-registraties** > **Registratie van nieuwe toepassing**.

  Geef de toepassing een naam, selecteer **systeemeigen** voor het toepassingstype, en voer de waarde van een URI-indeling voor **omleidings-URI**. Selecteer **maken** wanneer u klaar bent.

  ![AAD-registratie maken](media/aad-integration/app-registration-client.png)

2. Selecteer in de Azure AD-toepassing **instellingen** > **vereist machtigingen** > **toevoegen** > **Selecteer een API** en zoek naar de naam van de servertoepassing die is gemaakt in de laatste stap van dit document.

  ![Toepassingsmachtigingen configureren](media/aad-integration/select-api.png)

3. Schakel het selectievakje naast de toepassing en klik op **Selecteer**.

  ![AKS AAD-servereindpunt toepassing selecteren](media/aad-integration/select-server-app.png)

4. Selecteer **gedaan** en **machtiging verlenen** om deze stap te voltooien.

  ![Machtigingen verlenen](media/aad-integration/grant-permissions-client.png)

5. Terug op de AD-toepassing Noteer de **toepassings-ID**. Bij het implementeren van een cluster met Azure AD-functionaliteit AKS deze waarde wordt aangeduid als de `Client application ID`.

  ![De toepassings-ID ophalen](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Tenant-id ophalen

Ten slotte de ID van uw Azure-tenant verkrijgen. Deze waarde wordt ook gebruikt bij het implementeren van het cluster AKS.

Selecteer in de Azure-portal **Azure Active Directory** > **eigenschappen** en noteer de **map-ID**. Bij het implementeren van een cluster met Azure AD-functionaliteit AKS deze waarde wordt aangeduid als de `Tenant ID`.

![De Azure-tenant-ID niet ophalen](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Cluster implementeren

Gebruik de [az groep maken] [ az-group-create] opdracht om een resourcegroep voor het cluster AKS te maken.

```azurecli
az group create --name myAKSCluster --location eastus
```

Implementatie van het cluster gebruikt de [az aks maken] [ az-aks-create] opdracht. Vervang de waarden in de voorbeeldopdracht hieronder door de waarden die worden verzameld bij het maken van de Azure AD-toepassingen.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --generate-ssh-keys --enable-rbac \
  --aad-server-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>RBAC binding maken

Voordat u een Azure Active Directory-account kan worden gebruikt met het cluster AKS, moet een binding van de functie of het cluster rol binding worden gemaakt.

Gebruik eerst de [az aks get-referenties] [ az-aks-get-credentials] opdracht met de `--admin` argument voor aanmelding bij het cluster met beheerderstoegang.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster --admin
```

Vervolgens gebruikt u het volgende manifest voor het maken van een ClusterRoleBinding voor een Azure AD-account. Werk de gebruikersnaam met een van uw Azure AD-tenant. In dit voorbeeld geeft de account volledige toegang tot alle naamruimten van het cluster.

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

De binding van een rol kan ook worden gemaakt voor alle leden van een Azure AD-groep. Het volgende manifest geeft alle leden van de `kubernetes-admin` groep beheerderstoegang tot het cluster.

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
   name: "kubernetes-admin"
```

Zie voor meer informatie over het beveiligen van een cluster Kubernetes met RBAC [met behulp van RBAC autorisatie][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>RAS-cluster met Azure AD

Pull-vervolgens de context voor de niet-beheerders gebruiker met de [az aks get-referenties] [ az-aks-get-credentials] opdracht.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Na elke opdracht kubectl wordt uitgevoerd, wordt u gevraagd om te verifiëren met Azure. Volg de aanwijzingen op het scherm instructies.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-42032720-0   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-1   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-2   Ready     agent     1h        v1.9.6
```

Als u klaar is, wordt het verificatietoken in cache opgeslagen. U bent alleen reprompted aan te melden wanneer het token is verlopen of het configuratiebestand Kubernetes is opnieuw gemaakt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beveiligen van clusters met RBAC met Kubernetes de [met behulp van RBAC autorisatie] [ rbac-authorization] documentatie.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-create]: /cli/azure/group#az_group_create
[open-id-connect]: ../active-directory/develop/active-directory-protocols-openid-connect-code.md
