---
title: Service-principals voor AKS (Azure Kubernetes Services)
description: Een Azure Active Directory-service-principal maken en beheren voor een cluster in AKS (Azure Kubernetes Service)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mlearned
ms.openlocfilehash: 304b9dae9f3a1e134809d8959a96dc4e3ec0edd3
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615115"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Service-principals met AKS (Azure Kubernetes Service)

Om te communiceren met Azure API's, een AKS-cluster vereist een [Azure Active Directory (AD) service-principal][aad-service-principal]. De service-principal is nodig voor het dynamisch maken en beheren van andere Azure-resources zoals een Azure Load Balancer of een Azure Container Registry (ACR).

In dit artikel ziet u hoe u een service-principal voor uw AKS-clusters maakt en gebruikt.

## <a name="before-you-begin"></a>Voordat u begint

Als u een service-principal voor Azure AD wilt maken, moet u beschikken over machtigingen voor het registreren van een toepassing bij de Azure AD-tenant. U moet ook machtigingen hebben om de toepassing aan een rol toe te wijzen in uw abonnement. Als u niet beschikt over de benodigde machtigingen, moet u mogelijk de Azure AD- of abonnementsbeheerder vragen om de benodigde machtigingen toe te wijzen, of vooraf een service-principal maken voor gebruik met het AKS-cluster.

Als u een service-principal van een andere Azure AD-tenant, zijn er aanvullende overwegingen over de machtigingen beschikbaar wanneer u het cluster implementeert. U wellicht niet de juiste machtigingen voor lezen en schrijven van directory-informatie. Zie voor meer informatie, [wat zijn de standaardmachtigingen van de gebruiker in Azure Active Directory?][azure-ad-permissions]

U ook moet de Azure CLI versie 2.0.59 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Automatisch een service-principal maken en gebruiken

Wanneer u een AKS-cluster maakt in Azure portal of met de [az aks maken][az-aks-create] opdracht, Azure kan automatisch genereren van een service-principal.

In het volgende Azure CLI-voorbeeld is geen service-principal opgegeven. In dit scenario maakt de Azure CLI een service-principal voor het AKS-cluster. Om deze bewerking te kunnen voltooien, moet uw Azure-account beschikken over de juiste rechten voor het maken van een service-principal.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup
```

## <a name="manually-create-a-service-principal"></a>Handmatig een service-principal maken

Gebruik voor het handmatig maken van een service-principal met de Azure CLI de [az ad sp create-for-rbac][az-ad-sp-create] opdracht. In het volgende voorbeeld wordt met de parameter `--skip-assignment` voorkomen dat eventuele extra standaardtoewijzingen worden toegewezen:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

De uitvoer lijkt op die in het volgende voorbeeld. Noteer uw eigen `appId` en `password`. Deze waarden worden gebruikt wanneer u in de volgende sectie een AKS-cluster maakt.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2019-03-04-21-35-28",
  "name": "http://azure-cli-2019-03-04-21-35-28",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Een service-principal opgeven voor een AKS-cluster

Een bestaande service-principal gebruiken bij het maken van een AKS-cluster met de [az aks maken][az-aks-create] opdracht, gebruikt u de `--service-principal` en `--client-secret` parameters om op te geven de `appId` en `password` uit de uitvoer van de [az ad sp create-for-rbac][az-ad-sp-create] opdracht:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Als u een AKS-cluster implementeert met behulp van de Azure Portal, kiest u op de pagina *Verificatie* van het dialoogvenster **Kubernetes-cluster maken** de optie **Service-principal configureren**. Selecteer **Bestaande gebruiken** en geef de volgende waarden op:

- **Client-id van de service-principal** is uw *appId*
- **Clientgeheim van de service-principal** is de waarde van het *wachtwoord*

![Afbeelding van browsen naar Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Machtiging afgeven voor toegang tot andere Azure-resources

De service-principal voor het AKS-cluster kan worden gebruikt voor toegang tot andere resources. Als u wilt uw AKS-cluster implementeren in een bestaand virtueel Azure-netwerksubnet of maak verbinding naar Azure Container Registry (ACR), moet u bijvoorbeeld toegang tot deze resources om de service-principal te delegeren.

Als u wilt overdragen van machtigingen, maken van een rol toewijzing met de [az roltoewijzing maken][az-role-assignment-create] opdracht. Toewijzen de `appId` naar een bepaald bereik, zoals een resourcegroep of een VM-resource. Op basis van de rol wordt gedefinieerd welke machtigingen de service-principal heeft voor de resource, zoals in het volgende voorbeeld wordt weergegeven:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

De `--scope` van een resource moet een volledige resource-id zijn, zoals */subscriptions/\<guid\>/resourceGroups/myResourceGroup* of */subscriptions/\<guid \>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*

In de volgende secties wordt meer uitleg gegeven over algemene machtigingen die u mogelijk moet afgeven.

### <a name="azure-container-registry"></a>Azure Container Registry

Als u gebruikmaakt van Azure Container Registry (ACR) als opslagplaats voor installatiekopieën, moet u machtigingen aan uw AKS-cluster afgeven voor het lezen en ophalen van installatiekopieën. De service-principal van het AKS-cluster moet worden overgedragen aan de rol *Lezer* in het register. Zie voor gedetailleerde stappen [Grant AKS toegang tot ACR][aks-to-acr].

### <a name="networking"></a>Netwerken

U kunt gebruikmaken van geavanceerde netwerkmogelijkheden als het virtuele netwerk en het subnet of de openbare IP-adressen zich in een andere resourcegroep bevinden. Wijs een van de volgende sets rolmachtigingen toe:

- Maak een [aangepaste rol][rbac-custom-role] en definieert u de volgende rolmachtigingen:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/virtualNetworks/subnets/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
- Of toe te wijzen de [Inzender voor netwerken][rbac-network-contributor] ingebouwde rol op het subnet binnen het virtuele netwerk

### <a name="storage"></a>Storage

Mogelijk hebt u toegang nodig tot bestaande schijfresources in een andere resourcegroep. Wijs een van de volgende sets rolmachtigingen toe:

- Maak een [aangepaste rol][rbac-custom-role] en definieert u de volgende rolmachtigingen:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Of toe te wijzen de [Inzender voor Opslagaccounts][rbac-storage-contributor] ingebouwde rol voor de resourcegroep

### <a name="azure-container-instances"></a>Azure Container Instances

Als u Virtual Kubelet gebruikt om te integreren met AKS en ervoor kiest Azure Container Instances (ACI) uit te voeren in de resourcegroep, los van de AKS-cluster, dan moet u de AKS-service-principal de machtiging *Inzender* voor de ACI-resourcegroep verlenen.

## <a name="additional-considerations"></a>Aanvullende overwegingen

Houd rekening met het volgende wanneer u werkt met AKS en Azure AD-service-principals.

- De service-principal voor Kubernetes is een onderdeel van de configuratie van het cluster. Gebruik echter niet de id voor het implementeren van het cluster.
- Standaard zijn de referenties voor de service-principal geldig voor één jaar. U kunt [bijwerken of de referenties voor de service-principal draaien][update-credentials] op elk gewenst moment.
- Elke service-principal is gekoppeld aan een Azure AD-toepassing. De service-principal voor een Kubernetes-cluster kan zijn gekoppeld aan elke geldige Azure AD-toepassingsnaam (bijvoorbeeld *https://www.contoso.org/example* ). De URL van de toepassing hoeft geen echt eindpunt te zijn.
- Gebruik bij het opgeven van de **client-id** van de service-principal de waarde van de `appId`.
- Op de agent knooppunt VM's in het Kubernetes-cluster, de referenties voor de service-principal opgeslagen in het bestand `/etc/kubernetes/azure.json`
- Wanneer u gebruikt de [az aks maken][az-aks-create] opdracht voor het automatisch genereren van de service-principal de service principal-referenties worden geschreven naar het bestand `~/.azure/aksServicePrincipal.json` op de computer die wordt gebruikt de opdracht uit te voeren.
- Wanneer u een AKS-cluster dat is gemaakt door verwijdert [az aks maken][az-aks-create], de service-principal die automatisch is gemaakt, wordt niet verwijderd.
    - Als u wilt verwijderen van de service-principal, query's uitvoeren voor uw cluster *servicePrincipalProfile.clientId* en verwijder vervolgens met [az ad app delete][az-ad-app-delete]. Vervang de volgende brongroeps- en clusternamen door uw eigen waarden:

        ```azurecli
        az ad sp delete --id $(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
        ```

## <a name="troubleshoot"></a>Problemen oplossen

De referenties voor de service-principal voor een AKS-cluster in de cache opgeslagen door de Azure CLI. Als deze referenties zijn verlopen, kunt u problemen fouten bij het implementeren van AKS-clusters. De volgende strekking weergegeven bij het uitvoeren van [az aks maken][az-aks-create] kan duiden op een probleem met de referenties voor de cache service-principal:

```console
Operation failed with status: 'Bad Request'.
Details: The credentials in ServicePrincipalProfile were invalid. Please see https://aka.ms/aks-sp-help for more details.
(Details: adal: Refresh request failed. Status Code = '401'.
```

Controleer de leeftijd van het bestand met de volgende opdracht:

```console
ls -la $HOME/.azure/aksServicePrincipal.json
```

De verlooptijd van de standaard voor de referenties voor de service-principal is één jaar. Als uw *aksServicePrincipal.json* bestand is ouder dan één jaar, verwijdert u het bestand en probeer het opnieuw implementeren van een AKS-cluster.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Active Directory service-principals [toepassing en service-principalobjecten][service-principal].

Zie voor meer informatie over het bijwerken van de referenties [bijwerken of de referenties voor een service-principal in AKS draaien][update-credentials].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: ../container-registry/container-registry-auth-aks.md?toc=%2fazure%2faks%2ftoc.json#grant-aks-access-to-acr
[update-credentials]: update-credentials.md
[azure-ad-permissions]: ../active-directory/fundamentals/users-default-permissions.md
