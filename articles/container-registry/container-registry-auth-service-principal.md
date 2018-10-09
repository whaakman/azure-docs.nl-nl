---
title: Azure Container Registry-verificatie met service-principals
description: Informatie over het bieden van toegang tot afbeeldingen in uw persoonlijke containerregister met behulp van een service-principal voor Azure Active Directory.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: danlep
ms.openlocfilehash: 30f0eb04b4b7d07785854e3079bc6656889edec6
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854476"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container Registry-verificatie met service-principals

U kunt een service-principal voor Azure Active Directory (Azure AD) gebruiken voor de containerinstallatiekopie `docker push` en `pull` toegang tot uw containerregister. U kunt toegang tot 'headless' services en toepassingen opgeven met behulp van een service-principal.

## <a name="what-is-a-service-principal"></a>Wat is een service-principal?

Azure AD *service-principals* bieden toegang tot Azure-resources binnen uw abonnement. U kunt zien van een service principal een gebruikers-id voor een service, waarbij 'service' is een toepassing, service of -platform die toegang tot de resources nodig heeft. U kunt een service-principal configureren met toegangsrechten binnen het bereik van alleen de resources die u opgeeft. U kunt vervolgens uw toepassing of service voor het gebruik van de service-principal-referenties voor toegang tot deze resources configureren.

In de context van Azure Container Registry, kunt u een Azure AD service-principal met pull, push als pull of de eigenaar van machtigingen aan uw persoonlijke Docker-register in Azure.

## <a name="why-use-a-service-principal"></a>Waarom een service-principal gebruiken?

Met behulp van een service-principal voor Azure AD, kunt u binnen het bereik toegang bieden aan uw persoonlijke containerregister. U kunt andere service-principals voor elk van uw toepassingen of services, elk met op maat gemaakte toegangsrechten naar uw register maken. En omdat het delen van referenties tussen services en toepassingen, kunt u voorkomen, kunt u roteren of intrekken van toegang voor alleen de service-principal (en dus de toepassing) u kiest.

Bijvoorbeeld, een service-principal waarmee deze installatiekopie kunt gebruiken in uw webtoepassing `pull` alleen toegang tot terwijl uw buildsysteem kan gebruikmaken van een service-principal die het biedt met zowel `push` en `pull` toegang. Als de ontwikkeling van uw toepassing verandert handen, kunt u de referenties voor service-principal draaien zonder gevolgen voor de build-systeem.

## <a name="when-to-use-a-service-principal"></a>Wanneer u een service-principal

U moet een service-principal gebruiken voor toegang tot het register in **' headless '-scenario's**. Dat wil zeggen, afbeeldingen een toepassing, service of -script dat moet push of pull-container in een geautomatiseerde of anderszins onbeheerd.

Voor afzonderlijke toegang tot een register, zoals wanneer u handmatig een containerinstallatiekopie naar uw werkstation ontwikkeling ophaalt moet in plaats daarvan u uw eigen [Azure AD-identiteit](container-registry-authentication.md#individual-login-with-azure-ad) voor toegang tot het register (bijvoorbeeld met [az acr aanmelding][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="sample-scripts"></a>Voorbeeldscripts

U vindt de voorgaande voorbeeldscripts voor Azure-CLI op GitHub, als en versies voor Azure PowerShell:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Volgende stappen

Zodra u een service-principal die u toegang hebt verleend aan het containerregister hebt, kunt u de referenties in uw toepassingen en services voor interactie met de registersleutel.

Terwijl afzonderlijke toepassingen configureren voor gebruik van referenties voor service-principal buiten het bereik van dit artikel is vindt u instructies voor sommige specifieke services en hier platforms:

* [Verifiëren met Azure Container Registry uit Azure Kubernetes Service (AKS)](container-registry-auth-aks.md)
* [Verifiëren met Azure Container Registry van Azure Container Instances (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login