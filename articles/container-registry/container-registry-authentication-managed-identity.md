---
title: Azure Container Registry-verificatie met een beheerde identiteit
description: Bieden toegang tot afbeeldingen in uw persoonlijke containerregister met behulp van een gebruiker toegewezen of door het systeem toegewezen beheerde Azure-identiteit.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 01/16/2019
ms.author: danlep
ms.openlocfilehash: b09348e98a0dee85338cc9f20289d83b658eb719
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58338459"
---
# <a name="use-an-azure-managed-identity-to-authenticate-to-an-azure-container-registry"></a>Gebruik een door Azure identiteit beheerde worden geverifieerd bij een Azure container registry 

Gebruik een [beheerde identiteit voor de Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) te naar een Azure container registry verifiëren vanaf een andere Azure-resource, zonder dat u hoeft te bieden of registerreferenties beheren. Bijvoorbeeld instellen een gebruiker toegewezen of door het systeem toegewezen beheerde identiteit op een Linux-VM naar de containerinstallatiekopieën toegang vanuit uw container registry, net zo gemakkelijk als u een openbaar register gebruiken.

In dit artikel leert u meer informatie over beheerde identiteiten en hoe u:

> [!div class="checklist"]
> * Een gebruiker toegewezen of door het systeem toegewezen identiteit op een virtuele Azure-machine inschakelen
> * De identiteit toegang verlenen tot een Azure container registry
> * De beheerde identiteit gebruiken voor toegang tot het register en haal de containerinstallatiekopie van een 

Voor het maken van de Azure-resources in dit artikel is vereist dat u de Azure CLI versie 2.0.55 uitvoert of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

Voor het instellen van een container registry en een containerinstallatiekopie naar het pushen, moet u ook Docker lokaal geïnstalleerd hebben. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

## <a name="why-use-a-managed-identity"></a>Waarom een beheerde identiteit gebruiken?

Een beheerde identiteit voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt configureren [bepaalde Azure-resources](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), met inbegrip van virtuele machines, met een beheerde identiteit. Vervolgens gebruikt u de identiteit voor toegang tot andere Azure-resources, zonder het doorgeven van referenties in code of scripts.

Beheerde identiteiten zijn twee typen:

* *Gebruiker toegewezen identiteiten*, die u kunt toewijzen aan meerdere resources en voor zolang als uw wilt behouden. Gebruiker toegewezen identiteiten zijn momenteel in preview.

* Een *system beheerde identiteit*, die uniek is voor een specifieke bron, zoals een enkele virtuele machine en is geldig gedurende de levensduur van die resource.

Na het instellen van een Azure-resource met een beheerde identiteit, geeft u de identiteit van de toegang tot de gewenste naar een andere resource, net als bij alle beveiligings-principal. Bijvoorbeeld, een beheerde identiteit een rol toewijzen met pull, push als pull of andere machtigingen aan een persoonlijk register in Azure. (Zie voor een volledige lijst van register rollen [Azure Container Registry-rollen en machtigingen](container-registry-roles.md).) U kunt een identiteit toegang geven tot een of meer resources.

Gebruik vervolgens de identiteit te verifiëren met een [die ondersteuning biedt voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication), zonder eventuele referenties in uw code. Voor het gebruik van de identiteit voor toegang tot een Azure container registry van een virtuele machine, kunt u met Azure Resource Manager verifiëren. Kiezen hoe u verifieert met behulp van de beheerde identiteit, afhankelijk van uw scenario:

* [Een Azure AD-toegangstoken verkrijgen](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) programmatisch met behulp van HTTP- of REST-aanroepen

* Gebruik de [Azure SDK's](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md)

* [Meld u aan bij Azure CLI of PowerShell](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md) met de identiteit. 

## <a name="create-a-container-registry"></a>Een containerregister maken

Als u nog een Azure container registry hebt, een register maken en pushen een containerinstallatiekopie voorbeeld toe. Zie voor stappen [Quick Start: Een privé containerregister maken met de Azure CLI](container-registry-get-started-azure-cli.md).

In dit artikel wordt ervan uitgegaan dat u hebt de `aci-helloworld:v1` containerinstallatiekopie opgeslagen in het register. De voorbeelden gebruiken de registernaam van *myContainerRegistry*. Vervangen door uw eigen register en de namen van afbeeldingen in latere stappen.

## <a name="create-a-docker-enabled-vm"></a>Een Docker-functionaliteit-VM maken

Maak een Docker-functionaliteit Ubuntu-machine. U moet ook installeren de [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) op de virtuele machine. Als u al een virtuele machine van Azure, sla deze stap voor het maken van de virtuele machine.

Implementeer een standaard virtuele-machine met Ubuntu Azure [az vm maken][az-vm-create]. Het volgende voorbeeld wordt een virtuele machine met de naam *myDockerVM* in een bestaande resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Het duurt enkele minuten voordat de virtuele machine wordt gemaakt. Wanneer de opdracht is voltooid, noteer de `publicIpAddress` weergegeven door de Azure CLI. Dit adres gebruiken om te maken van SSH-verbindingen met de virtuele machine.

### <a name="install-docker-on-the-vm"></a>Docker installeren op de virtuele machine

Nadat de virtuele machine wordt uitgevoerd, maakt u een SSH-verbinding met de virtuele machine. Vervang *publicIpAddress* met het openbare IP-adres van uw virtuele machine.

```bash
ssh azureuser@publicIpAddress
```

Voer de volgende opdracht uit om Docker te installeren op de virtuele machine:

```bash
sudo apt install docker.io -y
```

Na de installatie voert u de volgende opdracht om te controleren of Docker correct wordt uitgevoerd op de virtuele machine:

```bash
sudo docker run -it hello-world
```

Uitvoer:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Azure-CLI installeren

Volg de stappen in [Azure CLI installeren met apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) de Azure CLI installeren op uw virtuele Ubuntu-machine. Zorg ervoor dat u versie 2.0.55 installeren voor dit artikel of hoger.

Sluit de SSH-sessie.

## <a name="example-1-access-with-a-user-assigned-identity"></a>Voorbeeld 1: Toegang met de identiteit van een gebruiker toegewezen

### <a name="create-an-identity"></a>Een identiteit maken

Een identiteit maken in uw abonnement met de [az-identiteit maken](/cli/azure/identity?view=azure-cli-latest#az-identity-create) opdracht. U kunt de dezelfde resourcegroep die u eerder hebt gebruikt om te maken van het containerregister of virtuele machine of een ander account gebruiken.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRId
```

Gebruik voor het configureren van de identiteit in de volgende stappen uit de [az identiteit show] [ az-identity-show] opdracht voor het opslaan van de identiteit van de bron- en service-principal-ID in de variabelen.

```azurecli
# Get resource ID of the user-assigned identity
userID=$(az identity show --resource-group myResourceGroup --name myACRId --query id --output tsv)

# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACRId --query principalId --output tsv)
```

Omdat u de ID van de identiteit in een latere stap nodig hebt wanneer u zich aanmeldt bij de CLI van uw virtuele machine, moet u de waarde weergeven:

```bash
echo $userID
```

De ID is van het formulier:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRId
```

### <a name="configure-the-vm-with-the-identity"></a>De virtuele machine configureren met de identiteit

De volgende [az vm identiteit toewijzen] [ az-vm-identity-assign] opdracht configureert uw Docker-VM met de identiteit van de gebruiker toegewezen:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM --identities $userID
```

### <a name="grant-identity-access-to-the-container-registry"></a>Identiteit toegang verlenen tot de container registry

Nu configureren voor de identiteit voor toegang tot uw containerregister. Voor het eerst gebruiken de [az acr show] [ az-acr-show] opdracht voor het ophalen van de resource-ID van het register:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Gebruik de [az roltoewijzing maken] [ az-role-assignment-create] opdracht aan de rol AcrPull toewijzen aan het register. Deze rol biedt [pull-machtigingen](container-registry-roles.md) in het register. Om te voorzien van beide pull en push-machtigingen, wijs de rol ACRPush toe.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>De identiteit van de toegang tot het register gebruiken

Voeg SSH toe aan de Docker-virtuele machine die geconfigureerd met de identiteit. Voer de volgende Azure CLI-opdrachten, met behulp van Azure CLI is geïnstalleerd op de virtuele machine.

Eerst, worden geverifieerd met de Azure CLI met [az login][az-login], met behulp van de identiteit die u hebt geconfigureerd op de virtuele machine. Voor <userID>, vervangen door de ID van de identiteit die u in de vorige stap hebt opgehaald. 

```azurecli
az login --identity --username <userID>
```

Vervolgens verifiëren naar het register met [az acr login][az-acr-login]. Wanneer u deze opdracht gebruikt, de CLI maakt gebruik van de Active Directory-token dat is gemaakt toen u uitvoerde `az login` naadloos verifiëren van de sessie met het containerregister. (Afhankelijk van instellingen van de virtuele machine, moet u mogelijk uitvoeren van deze opdracht en de docker-opdrachten met `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

U ziet een `Login succeeded` bericht. U kunt vervolgens uitvoeren `docker` opdrachten zonder referenties op te geven. Voer bijvoorbeeld [docker pull] [ docker-pull] voor pull de `aci-helloworld:v1` afbeelding, de naam van de aanmeldingsserver van het register op te geven. Naam van de aanmeldingsserver bestaat uit de naam van het containerregister (alle kleine letters) gevolgd door `.azurecr.io` - bijvoorbeeld `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="example-2-access-with-a-system-assigned-identity"></a>Voorbeeld 2: Toegang met een door het systeem toegewezen identiteit

### <a name="configure-the-vm-with-a-system-managed-identity"></a>De virtuele machine configureren met een systeem beheerde identiteit

De volgende [az vm identiteit toewijzen] [ az-vm-identity-assign] opdracht configureert uw Docker-VM met een door het systeem toegewezen identiteit:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM 
```

Gebruik de [az vm show] [ az-vm-show] -opdracht uit om een variabele met de waarde van `principalId` (de service principal-ID) van de identiteit van de virtuele machine, voor gebruik in latere stappen.

```azurecli-interactive
spID=$(az vm show --resource-group myResourceGroup --name myDockerVM --query identity.principalId --out tsv)
```

### <a name="grant-identity-access-to-the-container-registry"></a>Identiteit toegang verlenen tot de container registry

Nu configureren voor de identiteit voor toegang tot uw containerregister. Voor het eerst gebruiken de [az acr show] [ az-acr-show] opdracht voor het ophalen van de resource-ID van het register:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Gebruik de [az roltoewijzing maken] [ az-role-assignment-create] opdracht aan de rol AcrPull toewijzen aan de identiteit. Deze rol biedt [pull-machtigingen](container-registry-roles.md) in het register. Om te voorzien van beide pull en push-machtigingen, wijs de rol ACRPush toe.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>De identiteit van de toegang tot het register gebruiken

Voeg SSH toe aan de Docker-virtuele machine die geconfigureerd met de identiteit. Voer de volgende Azure CLI-opdrachten, met behulp van Azure CLI is geïnstalleerd op de virtuele machine.

De Azure CLI met eerst worden geverifieerd [az login][az-login], met behulp van de door het systeem toegewezen identiteit op de virtuele machine.

```azurecli
az login --identity
```

Vervolgens verifiëren naar het register met [az acr login][az-acr-login]. Wanneer u deze opdracht gebruikt, de CLI maakt gebruik van de Active Directory-token dat is gemaakt toen u uitvoerde `az login` naadloos verifiëren van de sessie met het containerregister. (Afhankelijk van instellingen van de virtuele machine, moet u mogelijk uitvoeren van deze opdracht en de docker-opdrachten met `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

U ziet een `Login succeeded` bericht. U kunt vervolgens uitvoeren `docker` opdrachten zonder referenties op te geven. Voer bijvoorbeeld [docker pull] [ docker-pull] voor pull de `aci-helloworld:v1` afbeelding, de naam van de aanmeldingsserver van het register op te geven. Naam van de aanmeldingsserver bestaat uit de naam van het containerregister (alle kleine letters) gevolgd door `.azurecr.io` - bijvoorbeeld `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het gebruik van beheerde identiteiten met Azure Container Registry en hoe u:

> [!div class="checklist"]
> * Inschakelen van een gebruiker toegewezen of door het systeem toegewezen identiteit in een Azure VM
> * De identiteit toegang verlenen tot een Azure container registry
> * De beheerde identiteit gebruiken voor toegang tot het register en haal de containerinstallatiekopie van een

* Meer informatie over [beheerde identiteiten voor een Azure-resources](/azure/active-directory/managed-identities-azure-resources/).


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-vm-show]: /cli/azure/vm#az-vm-show
[az-vm-identity-assign]: /cli/azure/vm/identity#az-vm-identity-assign
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
