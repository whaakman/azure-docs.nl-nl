---
title: Verifiëren met Azure Container Registry van Azure Container Instances
description: Informatie over het bieden van toegang tot afbeeldingen in uw persoonlijke containerregister van Azure Container Instances met behulp van een service-principal voor Azure Active Directory.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: danlep
ms.openlocfilehash: 8a2d19a09233e510055e147fa1cf95dd4471768b
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390670"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Verifiëren met Azure Container Registry van Azure Container Instances

U kunt een service-principal voor Azure Active Directory (Azure AD) gebruiken voor toegang tot uw persoonlijke containerregisters in Azure Container Registry.

In dit artikel hebt u meer informatie over het maken en configureren van een service-principal voor Azure AD met *pull* machtigingen naar het register. Vervolgens start u een container in Azure Container Instances (ACI) die de installatiekopie ophaalt uit uw privéregister, met behulp van de service-principal voor verificatie.

## <a name="when-to-use-a-service-principal"></a>Wanneer u een service-principal

U moet een service-principal gebruiken voor verificatie van ACI in **' headless '-scenario's**, zoals toepassingen of services die containerinstanties in een geautomatiseerde of anderszins onbeheerd maken.

Bijvoorbeeld, als er een geautomatiseerd script dat wordt uitgevoerd elke nacht en maakt een [taakgebaseerde containerinstantie](../container-instances/container-instances-restart-policy.md) voor het verwerken van gegevens, kan deze een service-principal gebruiken met alleen pull machtigingen om het register te verifiëren. U kunt vervolgens de service-principal roteren of intrekken van de toegang volledig zonder gevolgen voor andere services en toepassingen.

Service-principals moeten ook worden gebruikt wanneer het register [gebruiker met beheerdersrechten](container-registry-authentication.md#admin-account) is uitgeschakeld.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Verifiëren met behulp van de service-principal

Om te starten in een container in Azure Container Instances met behulp van een service-principal, geef de ID voor `--registry-username`, en het bijbehorende wachtwoord voor `--registry-password`.

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

U vindt de voorgaande voorbeeldscripts voor Azure-CLI op GitHub, als en versies voor Azure PowerShell:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten aanvullende informatie over het werken met service-principals en ACR:

* [Azure Container Registry-verificatie met service-principals](container-registry-auth-service-principal.md)
* [Verifiëren met Azure Container Registry uit Azure Kubernetes Service (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
