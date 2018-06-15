---
title: Verificatie met Azure Container register van exemplaren van de Azure-Container
description: Informatie over het bieden van toegang tot afbeeldingen in het register privé-container van exemplaren van Azure Container met behulp van een Azure Active Directory-service-principal.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: marsma
ms.openlocfilehash: daa9c098de0c410bd4033cc62ee911631eb3b634
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33768223"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Verificatie met Azure Container register van exemplaren van de Azure-Container

U kunt een service-principal voor Azure Active Directory (Azure AD) gebruiken voor toegang tot uw registers privé-container in Azure Container register.

In dit artikel leert u hoe u kunt maken en configureren van een Azure AD-service-principal met *pull* machtigingen toe aan het register. Start vervolgens een container in Azure Container exemplaren (ACI) waarmee de installatiekopie wordt opgehaald uit het persoonlijke register met behulp van de service-principal voor verificatie.

## <a name="when-to-use-a-service-principal"></a>Het gebruik van een service-principal

U moet een service-principal gebruiken voor verificatie van ACI in **headless scenario's**, zoals toepassingen of services die containerexemplaren in geautomatiseerde of anderszins onbeheerd maken.

Bijvoorbeeld, als er een geautomatiseerde script dat wordt uitgevoerd elke nacht en maakt een [instantie container taakgebaseerde](../container-instances/container-instances-restart-policy.md) voor het verwerken van bepaalde gegevens, kan deze een service-principal gebruiken met alleen pull (lezer) machtigingen om het register te verifiëren. U kunt vervolgens draaien referenties voor de service-principal of volledig de toegang intrekken zonder andere services en toepassingen.

Service-principals moeten ook worden gebruikt wanneer het register [gebruiker met beheerdersrechten](container-registry-authentication.md#admin-account) is uitgeschakeld.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Verifiëren met behulp van de service-principal

Start een container in Azure Container-exemplaren die gebruikmaken van een service-principal, geef de ID voor `--registry-username`, en het bijbehorende wachtwoord voor `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Voorbeeldscripts

U vindt de voorgaande voorbeelden van scripts voor Azure CLI op GitHub, als de juist versies voor Azure PowerShell:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten aanvullende informatie over het werken met de service-principals en ACR:

* [Azure Container register verificatie met service-principals](container-registry-auth-service-principal.md)
* [Verificatie met Azure Container register van Azure Kubernetes-Service (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
