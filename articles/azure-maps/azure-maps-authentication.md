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
ms.openlocfilehash: b97c303433eb8fadcda51257d37447f052ce4a3b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118794"
---
# <a name="authentication-with-azure-maps"></a>Verificatie met Azure Maps

Azure Maps ondersteunt twee manieren voor het verifiëren van aanvragen. Gedeelde sleutel of Azure Active Directory (Azure AD) bieden verschillende methoden voor het autoriseren van elke aanvraag die naar Azure Maps wordt verzonden. In dit artikel worden beide verificatiemethoden uitgelegd voor het implementeren van uw verificatie.

## <a name="shared-key-authentication"></a>Gedeelde sleutelverificatie

Gedeelde sleutelverificatie is voor elke aanvraag bij Azure Maps afhankelijk van het doorgeven van sleutels die door het Azure Maps-account zijn gegenereerd.  Er worden twee sleutels gegenereerd als uw Azure Maps-account wordt gemaakt.  Voor elke aanvraag bij Azure Maps-services moet de abonnementssleutel als een parameter aan de URL worden toegevoegd.

> [!Tip]
> U wordt aangeraden de sleutels regelmatig opnieuw te genereren. Er worden twee sleutels geleverd, zodat u met behulp van één sleutel verbonden kunt blijven terwijl u de andere sleutel opnieuw genereert. Als u de sleutels opnieuw genereert, dient u elke toepassing bij te werken die toegang heeft tot dit account om de nieuwe sleutels te kunnen gebruiken.

Zie [Verificatiedetails](https://aka.ms/amauthdetails) om de sleutels weer te geven.

## <a name="authentication-with-azure-active-directory-preview"></a>Verificatie met Azure Active Directory (preview-versie)

Azure Maps biedt nu integratie met [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) voor verificatie van aanvragen voor Azure Maps-services.  Azure AD biedt verificatie op basis van identiteit, waaronder [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) om toegang op gebruikers- of toepassingsniveau te verlenen aan Azure Maps-resources. In dit artikel worden de concepten en onderdelen van integratie met Azure Maps Azure AD uitgelegd.

## <a name="authentication-with-oauth-access-tokens"></a>Verificatie met OAuth-toegangstokens

Azure Maps accepteert **OAuth 2.0**-toegangstokens voor Azure AD-tenants die zijn gekoppeld aan een Azure-abonnement dat een Azure Maps-account bevat.  Azure Maps accepteert tokens voor:

* Azure AD-gebruikers 
* Toepassingen van derden die machtigingen gebruiken die door gebruikers zijn gedelegeerd
* Beheerde identiteiten voor Azure-resources

Azure Maps genereert een `unique identifier (client ID)` voor elk Azure Maps-account.  Als de client-id wordt gecombineerd met aanvullende parameters, kunnen uit Azure AD tokens worden aangevraagd door de onderstaande waarde op te geven:

```
https://login.microsoftonline.com
```
Zie [How To Manage Authentication](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication) (Verificatie beheren) voor meer informatie over het configureren van Azure AD en aanvraagtokens voor Azure Maps.

Zie [Basisinformatie over verificatie in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios) voor algemene informatie over het aanvragen van tokens uit Azure AD.

## <a name="requesting-azure-map-resources-with-oauth-tokens"></a>Azure Maps-resources aanvragen met OAuth-tokens

Zodra een token uit Azure AD is verkregen, kan een aanvraag naar Azure Maps worden verzonden waarbij de volgende twee vereiste aanvraagheaders zijn ingesteld:

| Aanvraagheader    |    Waarde    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorisatie     | Bearer-token eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` is de op het Azure Maps-account gebaseerde GUID die op de verificatiepagina van Azure Maps wordt weergegeven

Hieronder staat een voorbeeld van de Azure Maps-routeaanvraag, aangevraagd met het OAuth-token:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Zie [Verificatiedetails](https://aka.ms/amauthdetails) om de client-id weer te geven.

## <a name="control-access-with-role-based-access-control-rbac"></a>Toegang beheren met op rollen gebaseerde toegangsbeheer (RBAC)

Een belangrijke functie van Azure AD bestaat uit het beheren van de toegang tot beveiligde resources via RBAC. Zodra uw Azure Maps-account is gemaakt en u de Azure Maps Azure AD-toepassing registreert in uw Azure AD-tenant, kunt u via de portalpagina van het Azure Maps-account RBAC configureren voor een gebruiker, een toepassing of een Azure-resource. 

Azure Maps ondersteunt momenteel alleen toegangsbeheer voor afzonderlijke Azure AD-gebruikers, toepassingen of Azure-services via beheerde identiteiten voor Azure-resources.

![concept](./media/azure-maps-authentication/concept.png)

Zie [RBAC configureren voor Azure Maps](https://aka.ms/amrbac) om uw RBAC-instellingen te bekijken.

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Beheerde identiteiten voor Azure-resources en Azure Maps

[Beheerde identiteiten voor Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) bieden Azure-services (Azure App-service, Azure Functions, Virtual Machines, enz.) met een automatisch beheerde identiteit die kan worden geautoriseerd voor toegang tot Azure Maps-services.  

## <a name="next-steps"></a>Volgende stappen

* Zie [How To Manage Authentication](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication) (Verificatie beheren) voor meer informatie over het verifiëren van een toepassing met Azure AD en Azure Maps.

* Zie [Azure AD en Azure Maps Map Control](https://aka.ms/amaadmc) voor meer informatie over het verifiëren van Azure Maps, Map Control en Azure AD.