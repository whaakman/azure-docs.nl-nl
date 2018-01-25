---
title: Azure Container register verificatie met service-principals
description: "Informatie over het bieden van toegang tot afbeeldingen in het register privé-container met behulp van een Azure Active Directory-service-principal."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 97036ecabceb12b87b76c6ecb7e521157cbef827
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container register verificatie met service-principals

U kunt een Azure Active Directory (Azure AD) service-principal container afbeelding `docker push` en `pull` toegang tot het register van de container. U kunt toegang tot 'headless' services en toepassingen opgeven met behulp van een service-principal.

## <a name="what-is-a-service-principal"></a>Wat is een service-principal?

Azure AD *service-principals* bieden toegang tot Azure-resources in uw abonnement. U kunt zien van een service principal gebruikers-id voor een service waarbij 'service' is een toepassing, service of platform dat toegang tot de bronnen nodig heeft. U kunt een service-principal configureren met toegangsrechten binnen het bereik van alleen de resources die u opgeeft. Vervolgens kunt u uw toepassing of service voor het gebruik van de service-principal-referenties voor toegang tot deze bronnen.

In de context van Azure Container register, kunt u een Azure AD service-principal met pull, push als pull of de eigenaar van machtigingen in uw persoonlijke Docker-register in Azure.

## <a name="why-use-a-service-principal"></a>Waarom een service-principal gebruiken?

U kunt bereik toegang toe aan het register privé-container opgeven met behulp van een Azure AD-service-principal. U kunt andere service-principals voor elk van uw toepassingen of services, elk met rechten toe aan het register op maat gemaakte maken. En omdat het delen van referenties tussen services en toepassingen, kunt u voorkomen, kunt u referenties draaien of intrekken van toegang voor alleen het service-principal (en dus de toepassing) u kiest.

Bijvoorbeeld, een service-principal waarmee deze installatiekopie kunt gebruiken in uw webtoepassing `pull` alleen toegang tot bij uw build-systeem kunt gebruik maken van een service-principal waarmee deze beide `push` en `pull` toegang. Als de ontwikkeling van uw toepassing handen wijzigt, kunt u de service principle referenties draaien zonder de build-systeem.

## <a name="when-to-use-a-service-principal"></a>Het gebruik van een service-principal

U moet een service-principal gebruiken voor toegang tot het register in **headless scenario's**. Dat wil zeggen, afbeeldingen een toepassing, een service of een script dat moet push of pull-container geautomatiseerde of anderszins onbeheerd op.

Voor afzonderlijke toegang tot een register, zoals wanneer u handmatig een installatiekopie van een container pull naar uw werkstation ontwikkeling moet in plaats daarvan u uw eigen [Azure AD identity](container-registry-authentication.md#individual-login-with-azure-ad) voor toegang tot het register (bijvoorbeeld met [az acr aanmelding][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="next-steps"></a>Volgende stappen

Zodra u een service-principal die u toegang hebt verleend aan het register van de container hebt, kunt u de referenties in uw toepassingen en services voor interactie met de registersleutel.

Hoewel afzonderlijke toepassingen configureren voor service-principal referenties gebruiken buiten het bereik van dit artikel, vindt u instructies voor een aantal specifieke services en hier platforms:

* [Verificatie met Azure Container register van Azure Containerservice (AKS)](container-registry-auth-aks.md)
* [Verificatie met Azure Container register van exemplaren van de Azure-Container (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az_acr_login