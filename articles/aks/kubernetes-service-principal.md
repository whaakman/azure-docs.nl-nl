---
title: Service-principals voor AKS (Azure Kubernetes Services)
description: Een Azure Active Directory-service-principal maken en beheren voor een cluster in AKS (Azure Kubernetes Service)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 2bc0579d3dd60d66a23a29dabff7e43ca8dfee76
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435392"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Service-principals met AKS (Azure Kubernetes Service)

Voor de interactie met Azure API's heeft een AKS-cluster een [Azure AD-service-principal (Active Directory)][aad-service-principal] nodig. De service-principal is nodig voor het dynamisch maken en beheren van andere Azure-resources zoals een Azure Load Balancer of een Azure Container Registry (ACR).

In dit artikel ziet u hoe u een service-principal voor uw AKS-clusters maakt en gebruikt.

## <a name="before-you-begin"></a>Voordat u begint

Als u een service-principal voor Azure AD wilt maken, moet u beschikken over machtigingen voor het registreren van een toepassing bij de Azure AD-tenant. U moet ook machtigingen hebben om de toepassing aan een rol toe te wijzen in uw abonnement. Als u niet beschikt over de benodigde machtigingen, moet u mogelijk de Azure AD- of abonnementsbeheerder vragen om de benodigde machtigingen toe te wijzen, of vooraf een service-principal maken voor gebruik met het AKS-cluster.

Ook moet de Azure CLI-versie 2.0.46 of later zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Automatisch een service-principal maken en gebruiken

Wanneer u een AKS-cluster in de Azure Portal maakt of de opdracht [az aks create][az-aks-create] gebruikt, kan Azure automatisch een service-principal genereren.

In het volgende Azure CLI-voorbeeld is geen service-principal opgegeven. In dit scenario maakt de Azure CLI een service-principal voor het AKS-cluster. Om deze bewerking te kunnen voltooien, moet uw Azure-account beschikken over de juiste rechten voor het maken van een service-principal.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Handmatig een service-principal maken

Gebruik de opdracht [az ad sp create-for-rbac][az-ad-sp-create] als u handmatig een service-principal wilt maken met de Azure CLI. In het volgende voorbeeld wordt met de parameter `--skip-assignment` voorkomen dat eventuele extra standaardtoewijzingen worden toegewezen:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

De uitvoer lijkt op die in het volgende voorbeeld. Noteer uw eigen `appId` en `password`. Deze waarden worden gebruikt wanneer u in de volgende sectie een AKS-cluster maakt.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Een service-principal opgeven voor een AKS-cluster

Als u een bestaande service-principal wilt gebruiken wanneer u een AKS-cluster maakt met de opdracht [az aks create][az-aks-create], gebruikt u de parameters `--service-principal` en `--client-secret` om de `appId` en het `password` uit de uitvoer van de opdracht [az ad sp create-for-rbac][az-ad-sp-create] op te geven:

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

De service-principal voor het AKS-cluster kan worden gebruikt voor toegang tot andere resources. Als u bijvoorbeeld gebruik wilt maken van geavanceerde netwerkmogelijkheden om verbinding te maken met bestaande virtuele netwerken of om verbinding te maken met Azure Container Registry (ACR), moet u machtigingen afgeven voor toegang tot de service-principal.

Als u machtigingen wilt afgeven, moet u een roltoewijzing maken met de opdracht [az role assignment create][az-role-assignment-create]. U wijst de `appId` toe aan een specifiek bereik, zoals een resourcegroep of een virtuele-netwerkresource. Op basis van de rol wordt gedefinieerd welke machtigingen de service-principal heeft voor de resource, zoals in het volgende voorbeeld wordt weergegeven:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

De `--scope` van een resource moet een volledige resource-id zijn, zoals */subscriptions/\<guid\>/resourceGroups/myResourceGroup* of */subscriptions/\<guid \>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*

In de volgende secties wordt meer uitleg gegeven over algemene machtigingen die u mogelijk moet afgeven.

### <a name="azure-container-registry"></a>Azure Container Registry

Als u gebruikmaakt van Azure Container Registry (ACR) als opslagplaats voor installatiekopieën, moet u machtigingen aan uw AKS-cluster afgeven voor het lezen en ophalen van installatiekopieën. De service-principal van het AKS-cluster moet worden overgedragen aan de rol *Lezer* in het register. Zie [Toegang tot AKS verlenen in ACR][aks-to-acr] voor gedetailleerde stappen.

### <a name="networking"></a>Netwerken

U kunt gebruikmaken van geavanceerde netwerkmogelijkheden als het virtuele netwerk en het subnet of de openbare IP-adressen zich in een andere resourcegroep bevinden. Wijs een van de volgende sets rolmachtigingen toe:

- Maak een [aangepaste rol][rbac-custom-role] en definieer de volgende rolmachtigingen:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
- U kunt ook de ingebouwde rol [Inzender voor netwerken][rbac-network-contributor] gebruiken in het subnet of in het virtuele netwerk

### <a name="storage"></a>Storage

Mogelijk hebt u toegang nodig tot bestaande schijfresources in een andere resourcegroep. Wijs een van de volgende sets rolmachtigingen toe:

- Maak een [aangepaste rol][rbac-custom-role] en definieer de volgende rolmachtigingen:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- U kunt ook de ingebouwde rol [Inzender voor opslagaccounts][rbac-storage-contributor] toewijzen aan de resourcegroep

### <a name="azure-container-instances"></a>Azure Container Instances

Als u Virtual Kubelet gebruikt om te integreren met AKS en ervoor kiest Azure Container Instances (ACI) uit te voeren in de resourcegroep, los van de AKS-cluster, dan moet u de AKS-service-principal de machtiging *Inzender* voor de ACI-resourcegroep verlenen.

## <a name="additional-considerations"></a>Aanvullende overwegingen

Houd rekening met het volgende wanneer u werkt met AKS en Azure AD-service-principals.

- De service-principal voor Kubernetes is een onderdeel van de configuratie van het cluster. Gebruik echter niet de id voor het implementeren van het cluster.
- Elke service-principal is gekoppeld aan een Azure AD-toepassing. De service-principal voor een Kubernetes-cluster kan zijn gekoppeld aan elke geldige Azure AD-toepassingsnaam (bijvoorbeeld *https://www.contoso.org/example*). De URL van de toepassing hoeft geen echt eindpunt te zijn.
- Gebruik bij het opgeven van de **client-id** van de service-principal de waarde van de `appId`.
- Op de hoofd- en knooppunt-VM's in het Kubernetes-cluster worden de referenties voor de service-principal opgeslagen in het bestand `/etc/kubernetes/azure.json`
- Wanneer u de opdracht [az aks create][az-aks-create] gebruikt om de service-principal automatisch te genereren, worden de referenties voor de service-principal naar het bestand `~/.azure/aksServicePrincipal.json` geschreven op de computer die wordt gebruikt om de opdracht uit te voeren.
- Wanneer u een AKS-cluster verwijdert dat is gemaakt met [az aks create][az-aks-create], wordt de automatisch gemaakte service-principal niet verwijderd.
    - Als u de service-principal wilt verwijderen, haalt u eerst het id voor de service-principal op met [az ad app list][az-ad-app-list]. In het volgende voorbeeld wordt eerst een query uitgevoerd voor het cluster met de naam *myAKSCluster* en wordt vervolgens het app-id verwijderd met [az ad app delete][az-ad-app-delete]. Vervang deze namen door uw eigen waarden:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Azure Active Directory-service-principals, raadpleegt u [Application and service principal objects][service-principal] (Toepassings- en service-principal-objecten)

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
