---
title: Understanding Azure digitale dubbels op rollen gebaseerd toegangsbeheer | Microsoft Docs
description: Meer informatie over verificatie in digitale dubbels met op rollen gebaseerd toegangsbeheer.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: 7a6d8565a0f85b4cb81d9f5f23b04fe6b2edc53e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324132"
---
# <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure van digitale dubbels kunnen nauwkeurig toegangsbeheer voor specifieke gegevens, resources en acties in uw ruimtelijke grafiek. Dit gebeurt door middel van gedetailleerde rol en beheer van machtigingen met de naam _Role-Based Access Control_. Toegangsbeheer op basis van rollen bestaat uit _rollen_, of het niveau van machtigingen, en _roltoewijzingen_, of de koppeling van een rol voor een gebruiker of apparaat.

Met behulp van Role-Based Access Control, kan machtiging worden verleend aan een gebruiker, een apparaat, een service-principal, een door de gebruiker gedefinieerde functie, alle gebruikers die behoren tot een domein of een tenant. De mate van toegang kan bovendien ook worden afgestemd.

Toegangsbeheer op basis van rollen is uniek omdat dat machtigingen worden overgenomen in de ruimtelijke grafiek.

## <a name="what-can-i-do-with-role-based-access-control"></a>Wat kan ik doen met op rollen gebaseerd toegangsbeheer?

Een ontwikkelaar kan Role-Based Access Control te gebruiken:

* Een gebruiker de mogelijkheid voor het beheren van apparaten voor een heel gebouw, of alleen voor een specifieke ruimte of floor verlenen.
* Verleen een beheerder wereldwijde toegang tot alle knooppunten van het ruimtelijke graph voor een hele grafiek of alleen voor een gedeelte van de grafiek.
* Een ondersteuning voor gespecialiseerde lezen toegang verlenen tot de grafiek, met uitzondering van toegangssleutels.
* Elk lid van een domein-leestoegang tot alle objecten voor graph verlenen.

## <a name="role-based-access-control-best-practices"></a>Aanbevolen procedures voor toegang op basis van rollen beheren

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Roldefinities

Een **roldefinitie** is een verzameling van machtigingen en wordt ook wel genoemd een **rol**. De roldefinitie geeft een lijst van de toegestane bewerkingen, met inbegrip van *maken*, *lezen*, *bijwerken*, en *verwijderen*. Het geeft ook aan welke objecttypen deze machtigingen toepassen.

De volgende rollen zijn beschikbaar in Azure, digitale dubbels:

* **Ruimte beheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor de opgegeven ruimte en alle knooppunten eronder. Algemene machtigingen.
* **Gebruikersbeheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor gebruikers en gebruiker gerelateerde objecten. Leesmachtigingen voor opslagruimten.
* **Apparaatbeheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor apparaten en apparaat-gerelateerde objecten. Leesmachtigingen voor opslagruimten.
* **Sleutel beheerder**: maken, lezen, bijwerken en verwijderen van de machtiging voor toegang tot sleutels. Leesmachtigingen voor opslagruimten.
* **Token van beheerder**: machtiging voor lezen en bijwerken voor toegang tot sleutels. Leesmachtigingen voor opslagruimten.
* **Gebruiker**: leesmachtigingen voor opslagruimten, sensoren en gebruikers, met inbegrip van de bijbehorende verwante objecten.
* **Ondersteuning voor gespecialiseerde**: leesmachtigingen voor alles, behalve de toegang tot sleutels.
* **Apparaat-installatieprogramma**: machtiging voor lezen en bijwerken van apparaten en sensoren, met inbegrip van de bijbehorende verwante objecten. Leesmachtigingen voor opslagruimten.
* **Gateway-apparaat**: machtiging voor sensoren maken. Leesmachtigingen voor apparaten en sensoren, verwante met inbegrip van de bijbehorende objecten.

>[!NOTE]
> *De volledige definities voor de bovenstaande kunnen worden opgehaald door het opvragen van de system/rollen API.*

### <a name="object-types"></a>Objecttypen

De `ObjectIdType` verwijst naar het type van de identiteit die is een functie die wordt gegeven. Naast de `DeviceId` en `UserDefinedFunctionId` typen, de typen komen overeen met een eigenschap van een Azure Active Directory (Azure AD)-object:
  
* De `UserId` type wijst een rol toe aan een gebruiker.
* De `DeviceId` type een rol toewijst aan een apparaat.
* De `DomainName` type wijst een rol toe aan een domein. Elke gebruiker met de opgegeven domeinnaam beschikt over de toegangsrechten van de bijbehorende rol.
* De `TenantId` type wijst een rol toe aan een tenant. Elke gebruiker die behoort tot de opgegeven Azure AD-tenant-ID beschikt over de toegangsrechten van de bijbehorende rol.
* De `ServicePrincipalId` type een rol toegewezen aan een service-principal-object-ID.
* De `UserDefinedFunctionId` type een rol wordt toegewezen aan een door de gebruiker gedefinieerde functie (UDF's).

> [!div class="nextstepaction"]
> [Query of de object-ID voor een gebruiker](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [De object-ID verkrijgen voor een service-principal](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [De object-ID voor een Azure AD-tenant ophalen](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Roltoewijzingen

Machtigingen zijn verleend aan een ontvanger door het maken van een roltoewijzing en ingetrokken door het verwijderen van een roltoewijzing. Een roltoewijzing Azure digitale dubbels wordt gekoppeld aan een object (gebruiker, Azure AD-tenant, enzovoort), deze rol, en een spatie. Vervolgens machtigingen op alle objecten die deel uitmaken van deze ruimte, met inbegrip van de gehele ruimtelijke grafiek eronder.

Bijvoorbeeld, een gebruiker krijgt een roltoewijzing met rol `DeviceInstaller` voor het hoofdknooppunt van een ruimtelijke grafiek, die een gebouw vertegenwoordigt. De gebruiker kan vervolgens lezen en bijwerken van apparaten die niet alleen voor dat knooppunt, maar alle andere onderliggende spaties in het gebouw.

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
