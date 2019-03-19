---
title: Verificatie met Azure Maps | Microsoft Docs
description: Verificatie voor het gebruik van Azure Maps-services.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9dfe4024607e106565984d6d49de94d793bf7a8f
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010409"
---
# <a name="authentication-with-azure-maps"></a>Verificatie met Azure Maps

Azure kaarten biedt ondersteuning voor twee manieren om aanvragen te verifiëren: Gedeelde sleutel en Azure Active Directory (Azure AD). In dit artikel wordt uitgelegd deze verificatiemethoden als richtlijn voor uw implementatie.

## <a name="shared-key-authentication"></a>Gedeelde sleutelverificatie

Gedeelde sleutelverificatie doorgegeven sleutels die worden gegenereerd door een Azure kaarten-account met elke aanvraag voor Azure-kaarten.  Er worden twee sleutels gegenereerd als uw Azure Maps-account wordt gemaakt. De abonnementssleutel moet worden toegevoegd als een parameter aan de URL voor elke aanvraag naar Azure kaarten-services.

> [!Tip]
> U wordt aangeraden de sleutels regelmatig opnieuw te genereren. Krijgt u twee sleutels zodat u verbindingen met een sleutel onderhouden kunt bij het opnieuw genereren van de andere. Wanneer u uw sleutels opnieuw genereren, moet u alle toepassingen die toegang het account tot voor het gebruik van de nieuwe sleutels bijwerken.

Zie voor meer informatie over het weergeven van uw sleutels [verificatie details weergeven](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Verificatie met Azure Active Directory (preview-versie)

Azure Maps nu biedt [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) -integratie voor de verificatie van aanvragen voor Azure kaarten-services. Azure AD biedt verificatie op basis van identiteit, met inbegrip van [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview), gebruiker- of toepassingsniveau toegang tot resources van Azure Maps. De volgende secties kunt u informatie over de concepten en de onderdelen van Azure kaarten-integratie met Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Verificatie met OAuth-toegangstokens

Azure Maps accepteert **OAuth 2.0** toegang krijgen tot tokens voor Azure AD-tenants die zijn gekoppeld aan een Azure-abonnement met een Azure kaarten-account. Azure Maps accepteert tokens voor:

* Azure AD-gebruikers. 
* Partnertoepassingen die gebruikmaken van machtigingen die zijn overgedragen door gebruikers.
* Beheerde identiteiten voor Azure-resources.

Azure Maps genereert een *unieke id (client-ID)* voor elke Azure kaarten-account. Wanneer u deze client-ID met extra parameters combineert, kunt u tokens van Azure AD aanvragen door de volgende waarde op te geven:

```
https://login.microsoftonline.com
```
Zie voor meer informatie over het configureren van Azure AD en aanvragen van tokens voor Azure-kaarten [verificatie beheren in Azure Maps](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Raadpleeg voor algemene informatie over aanvragen tokens van Azure AD [wat is verificatie?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Aanvraag voor resources van de Azure-kaart met OAuth-tokens

Nadat een token uit Azure AD is ontvangen, kan een aanvraag naar Azure Maps worden verzonden met de volgende set van de twee vereiste aanvraag-headers:

| Aanvraagheader    |    Value    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorisatie     | Bearer-token eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` is de Azure-kaarten-account-GUID die wordt weergegeven op de pagina Azure Maps-verificatie.

Hier volgt een voorbeeld van de aanvraag van een Azure-kaarten-route die gebruikmaakt van een OAuth-token:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Zie voor meer informatie over het weergeven van uw client-ID [verificatie details weergeven](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Toegangsbeheer met RBAC

Met Azure AD kunt die u toegang tot beveiligde bronnen met behulp van RBAC. Nadat u uw Azure kaarten-account maakt en uw Azure Maps Azure AD-toepassing in uw Azure AD-tenant registreren, kunt u RBAC instellen voor een gebruiker, toepassing of Azure-resource op de pagina van Azure kaarten-account-portal.

Azure kaarten biedt ondersteuning voor lezen-toegangsbeheer voor afzonderlijke Azure AD-gebruikers, toepassingen en Azure-services via beheerde identiteiten voor Azure-resources.

![Azure Maps-gegevenslezer (preview-versie)](./media/azure-maps-authentication/concept.png)

Zie voor meer informatie over het weergeven van uw RBAC-instellingen [RBAC configureren voor Azure-kaarten](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Beheerde identiteiten voor een Azure-resources en Azure-kaarten

[Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) bieden Azure-services (Azure App Service, Azure Functions, Azure Virtual Machines, enzovoort) met een automatisch beheerde identiteit die kan worden geautoriseerd voor toegang tot Azure kaarten-services.  

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over het verifiëren van een toepassing met Azure AD en Azure Maps, Zie [verificatie beheren in Azure Maps](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Zie voor meer informatie over het verifiëren van de Azure Maps Map Control en Azure AD, [Azure Maps Map Control gebruiken](https://aka.ms/amaadmc).