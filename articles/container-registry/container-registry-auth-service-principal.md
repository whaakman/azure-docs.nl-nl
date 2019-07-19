---
title: Verificatie Azure Container Registry met een Service-Principal
description: Toegang bieden tot installatie kopieën in uw persoonlijke container register met behulp van een Azure Active Directory Service-Principal.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 12/13/2018
ms.author: danlep
ms.openlocfilehash: 97c45a009b155eea7bc61a9dd337090b9e3c1b42
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309945"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Verificatie Azure Container Registry met Service-principals

U kunt een service-principal voor Azure Active Directory (Azure AD) gebruiken om een `docker push` container `pull` installatie kopie te bieden en toegang te krijgen tot uw container register. Met behulp van een Service-Principal kunt u toegang bieden tot ' headless ' Services en toepassingen.

## <a name="what-is-a-service-principal"></a>Wat is een service-principal?

Azure AD- *service* -principals bieden toegang tot Azure-resources binnen uw abonnement. U kunt een Service-Principal beschouwen als een gebruikers-id voor een service, waarbij ' service ' elke toepassing, service of platform is die toegang nodig heeft tot de resources. U kunt een service-principal met toegangs rechten instellen die alleen zijn gericht op de resources die u opgeeft. Configureer vervolgens uw toepassing of service voor het gebruik van de referenties van de service-principal voor toegang tot deze resources.

In de context van Azure Container Registry kunt u een Azure AD-service-principal maken met pull-, push-en pull-toegang of andere machtigingen voor uw persoonlijke REGI ster in Azure. Zie [Azure container Registry rollen en machtigingen](container-registry-roles.md)voor een volledige lijst.

## <a name="why-use-a-service-principal"></a>Waarom een Service-Principal gebruiken?

Als u een service-principal voor Azure AD gebruikt, kunt u toegang tot uw persoonlijke container register verlenen. U kunt verschillende service-principals maken voor elk van uw toepassingen of services, elk met aangepaste toegangs rechten voor uw REGI ster. En omdat u het delen van referenties tussen services en toepassingen kunt voor komen, kunt u referenties draaien of de toegang intrekken voor alleen de Service-Principal (en dus de toepassing) die u kiest.

Uw webtoepassing kan bijvoorbeeld een Service-Principal gebruiken waarmee alleen installatie kopie `pull` wordt gemaakt, terwijl uw build-systeem een service-principal kan gebruiken die het met zowel `push` als `pull` toegang biedt. Als de ontwikkeling van uw toepassing wordt gewijzigd, kunt u de referenties van het Service principe draaien zonder dat dit van invloed is op het build-systeem.

## <a name="when-to-use-a-service-principal"></a>Wanneer moet u een Service-Principal gebruiken?

U moet een Service-Principal gebruiken om toegang te krijgen tot het REGI ster in **headless scenario's**. Dat wil zeggen, elke toepassing, service of script dat container installatie kopieën moet pushen of ophalen in een geautomatiseerde of op een andere manier zonder toezicht.

Voor individuele toegang tot een REGI ster, zoals het hand matig ophalen van een container installatie kopie naar uw ontwikkel werkstation, moet u in plaats daarvan uw eigen [Azure AD-identiteit](container-registry-authentication.md#individual-login-with-azure-ad) gebruiken voor toegang tot het REGI ster (bijvoorbeeld met [AZ ACR login][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="sample-scripts"></a>Voorbeeldscripts

U kunt de voor gaande voorbeeld scripts voor Azure CLI vinden op GitHub, evenals versies voor Azure PowerShell:

* [Azure-CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Volgende stappen

Zodra u een Service-Principal hebt die u toegang tot uw container register hebt verleend, kunt u de referenties in uw toepassingen en services gebruiken voor een headless REGI ster-interactie. U kunt de referenties van de service-principal van een Azure-service gebruiken die kan worden geverifieerd met een Azure container Registry. Voorbeelden zijn:

* [Verifiëren met Azure Container Registry van de Azure Kubernetes-service (AKS)](container-registry-auth-aks.md)
* [Verifiëren met Azure Container Registry van Azure Container Instances (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
