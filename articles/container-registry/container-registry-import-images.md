---
title: Importeren van containerinstallatiekopieën naar Azure Container Registry
description: Importeer containerinstallatiekopieën naar een Azure container registry met behulp van Azure-API's, zonder dat Docker-opdrachten uit te voeren.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 02/06/2019
ms.author: danlep
ms.openlocfilehash: a4187176548a674525d0be0d06bc2557de196af0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885661"
---
# <a name="import-container-images-to-a-container-registry"></a>Importeren van containerinstallatiekopieën naar een containerregister

U kunt eenvoudig containerinstallatiekopieën (kopie) importeren naar een Azure container registry, zonder met behulp van Docker-opdrachten. Bijvoorbeeld, afbeeldingen importeren uit een register ontwikkeling met een productie-register of basisinstallatiekopieën kopiëren van een openbaar register.

Azure Container Registry verwerkt een aantal algemene scenario's voor het kopiëren van afbeeldingen van een bestaand register:

* Importeren uit een openbaar register

* Importeren uit een andere Azure-containerregister, in dezelfde of een ander Azure-abonnement

* Importeren uit een niet-Azure privé containerregister

Afbeelding importeren in een Azure container registry biedt de volgende voordelen boven het gebruik van Docker-CLI-opdrachten:

* Omdat uw clientomgeving niet een lokale Docker-installatie hoeft, importeert u een containerinstallatiekopie, ongeacht de ondersteunde OS-type.

* Wanneer u meerdere architectuur afbeeldingen (zoals de officiële Docker-installatiekopieën) importeert, installatiekopieën voor alle architecturen en platformen die zijn opgegeven in de lijst met manifest gekopieerd.

Als u wilt importeren containerinstallatiekopieën, in dit artikel is vereist dat u de Azure CLI in Azure Cloud Shell uitvoert of lokaal (versie 2.0.55 of hoger aanbevolen). Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

> [!NOTE]
> Als u identiek zijn containerinstallatiekopieën verdelen over meerdere Azure-regio's moet, Azure Container Registry biedt ook ondersteuning voor [geo-replicatie](container-registry-geo-replication.md). Door geo-replicatie een register (Premium-SKU vereist), kunt u meerdere regio's met dezelfde naam voor de installatiekopie en tag van een enkel register fungeren.
>

## <a name="prerequisites"></a>Vereisten

Als u nog een Azure container registry hebt, maakt u een register. Zie voor stappen [Quick Start: Een privé containerregister maken met de Azure CLI](container-registry-get-started-azure-cli.md).

Voor het importeren van een installatiekopie naar een Azure container registry, moet je identiteit schrijftoegang hebt tot het doelregister (ten minste de rol van Inzender). Zie [Azure Container Registry-rollen en machtigingen](container-registry-roles.md). 

## <a name="import-from-a-public-registry"></a>Importeren uit een openbaar register

### <a name="import-from-docker-hub"></a>Importeren uit Docker Hub

Gebruik bijvoorbeeld de [az acr importeren] [ az-acr-import] opdracht voor het importeren van de architectuur met meerdere `hello-world:latest` installatiekopie vanuit Docker Hub naar een register met de naam *myregistry*. Omdat `hello-world` een officiële afbeelding van Docker Hub, deze afbeelding is in de standaard `library` opslagplaats. Naam van de opslagplaats en eventueel een code opnemen in de waarde van de `--source` installatiekopieparameter. (U kunt eventueel herkennen een installatiekopie van een door het manifest digest in plaats van door code, waarmee een specifieke versie van een installatiekopie wordt gegarandeerd.)
 
```azurecli
az acr import --name myregistry --source docker.io/library/hello-world:latest --image hello-world:latest
```

U kunt controleren dat meerdere manifesten gekoppeld aan deze installatiekopie door het uitvoeren van zijn de `az acr repository show-manifests` opdracht:

```azurecli
az acr repository show-manifests --name myregistry --repository hello-world
```

Het volgende voorbeeld worden geïmporteerd van een openbare installatiekopie van het `tensorflow` opslagplaats in Docker Hub:

```azurecli
az acr import --name myregistry --source docker.io/tensorflow/tensorflow:latest-gpu --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Importeren uit Microsoft Container Registry

Importeert bijvoorbeeld de meest recente Windows Server Core-installatiekopie uit de `windows` opslagplaats in Microsoft Container Registry.

```azurecli
az acr import --name myregistry --source mcr.microsoft.com/windows/servercore:latest --image servercore:latest
```

## <a name="import-from-another-azure-container-registry"></a>Importeren uit een andere Azure-containerregister

U kunt een installatiekopie importeren vanuit een andere Azure container registry met behulp van geïntegreerde Azure Active Directory-machtigingen.

* Uw identiteit moet Azure Active Directory-machtigingen te lezen uit het register bron (rol van lezer) en te schrijven naar het doelregister (rol van Inzender) hebben.

* Het register kan zich in hetzelfde of een ander Azure-abonnement in dezelfde Active Directory-tenant.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Importeren uit een register in hetzelfde abonnement

Importeert bijvoorbeeld de `aci-helloworld:latest` afbeelding uit een bron-register *mysourceregistry* naar *myregistry* in hetzelfde Azure-abonnement.

```azurecli
az acr import --name myregistry --source mysourceregistry.azurecr.io/aci-helloworld:latest --image hello-world:latest
```

Het volgende voorbeeld worden een installatiekopie van een geïmporteerd door manifest digest (SHA-256-hash, weergegeven als `sha256:...`) in plaats van door-tag:

```azurecli
az acr import --name myregistry --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Importeren uit een register in een ander abonnement

In het volgende voorbeeld *mysourceregistry* zich in een ander abonnement uit *myregistry* in dezelfde Active Directory-tenant. Geef de resource-ID van het register van de bron met de `--registry` parameter.
 
```azurecli
az acr import --name myregistry --source mysourceregistry.azurecr.io/aci-helloworld:latest --image hello-world:latest --registry /subscriptions/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Importeren uit een register met behulp van referenties voor service-principal

Als u wilt importeren vanuit een register dat u geen toegang tot met behulp van machtigingen voor Active Directory, kunt u referenties voor service-principal gebruiken (indien beschikbaar). Geef de toepassings-id en het wachtwoord van een Active Directory [service-principal](container-registry-auth-service-principal.md) die ACRPull toegang heeft tot het register van de bron. Met behulp van een service-principal is handig voor het bouwen en andere zonder toezicht systemen die nodig hebt voor het importeren van installatiekopieën naar het register.

```azurecli
az acr import --name myregistry --source sourceregistry.azurecr.io/sourcerepo/sourceimage:tag --image targetimage:tag --username <SP_App_ID> –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Importeren uit een niet-Azure privé containerregister

Een afbeelding van een persoonlijk register importeren door het opgeven van referenties die pull-toegang tot het register inschakelen. Haal bijvoorbeeld een afbeelding uit een persoonlijk dockerregister: 

```azurecli
az acr import --name myregistry --source docker.io/sourcerepo/sourceimage:tag --image sourceimage:tag --username <username> --password <password>
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het importeren van containerinstallatiekopieën naar een Azure container registry uit een openbaar register of een ander persoonlijk register. Voor de installatiekopie van de aanvullende opties voor het importeren, raadpleegt u de [az acr importeren] [ az-acr-import] opdrachten. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli