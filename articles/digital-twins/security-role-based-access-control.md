---
title: Informatie over Azure digitale dubbels op rollen gebaseerd toegangsbeheer | Microsoft Docs
description: Meer informatie over verificatie in digitale dubbels met op rollen gebaseerd toegangsbeheer.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014785"
---
# <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure van digitale dubbels kunnen nauwkeurig toegangsbeheer voor specifieke gegevens, resources en acties in uw ruimtelijke grafiek. Dit gebeurt door middel van gedetailleerde rol en beheer van machtigingen op rollen gebaseerd toegangsbeheer (RBAC) genoemd. RBAC bestaat uit _rollen_ en _roltoewijzingen_. Rollen bepalen het niveau van machtigingen. Een rol koppelen roltoewijzingen aan een gebruiker of apparaat.

Met RBAC kunt kan machtigingen worden verleend aan:

- Een gebruiker.
- Een apparaat.
- Een service-principal.
- Een door de gebruiker gedefinieerde functie. 
- Alle gebruikers die deel uitmaken van een domein. 
- Een tenant.
 
De mate van toegang kan ook worden afgestemd.

RBAC is uniek in de betreffende machtigingen worden overgenomen in de ruimtelijke grafiek.

## <a name="what-can-i-do-with-rbac"></a>Wat kan ik doen met op rollen gebaseerd toegangsbeheer?

Een ontwikkelaar kan RBAC om te gebruiken:

* Een gebruiker de mogelijkheid voor het beheren van apparaten voor een heel gebouw, of alleen voor een specifieke ruimte of een verdieping verlenen.
* Verleen een beheerder wereldwijde toegang tot alle knooppunten van het ruimtelijke graph voor een hele grafiek of alleen voor een gedeelte van de grafiek.
* Een ondersteuning voor gespecialiseerde lezen toegang verlenen tot de grafiek, met uitzondering van toegangssleutels.
* Elk lid van een domein-leestoegang tot alle objecten voor graph verlenen.

## <a name="rbac-best-practices"></a>Aanbevolen procedures voor RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Roldefinities

De roldefinitie van een is een verzameling van machtigingen en die een rol wordt ook wel genoemd. De rol de definitie van een lijst met de toegestane bewerkingen, waaronder maken, lezen, bijwerken en verwijderen. Het geeft ook aan welke objecttypen deze machtigingen toepassen.

De volgende rollen zijn beschikbaar in Azure, digitale dubbels:

* **Ruimte beheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor de opgegeven ruimte en alle knooppunten eronder. Algemene machtigingen.
* **Gebruikersbeheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor gebruikers en gebruiker gerelateerde objecten. Leesmachtigingen voor opslagruimten.
* **Apparaatbeheerder**: maken, lezen, bijwerken en verwijderen van machtigingen voor apparaten en apparaat-gerelateerde objecten. Leesmachtigingen voor opslagruimten.
* **Sleutel beheerder**: maken, lezen, bijwerken en verwijderen van de machtiging voor toegang tot sleutels. Leesmachtigingen voor opslagruimten.
* **Token van beheerder**: machtiging voor lezen en bijwerken voor toegang tot sleutels. Leesmachtigingen voor opslagruimten.
* **Gebruiker**: leesmachtigingen voor opslagruimten, sensoren en gebruikers, inclusief de bijbehorende verwante objecten.
* **Ondersteuning voor gespecialiseerde**: leesmachtigingen voor alles, behalve de toegang tot sleutels.
* **Apparaat-installatieprogramma**: update de machtigingen lezen en voor apparaten en sensoren, waaronder de bijbehorende verwante objecten. Leesmachtigingen voor opslagruimten.
* **Gateway-apparaat**: machtiging voor sensoren maken. Leesmachtigingen voor apparaten en sensoren, verwante inclusief de bijbehorende objecten.

>[!NOTE]
> Query uitvoeren op het systeem/rollen API om op te halen van de volledige definities voor de vorige rollen.

### <a name="object-types"></a>Objecttypen

De `ObjectIdType` verwijst naar het type van de identiteit die is toegewezen aan een rol. Naast de `DeviceId` en `UserDefinedFunctionId` typen, de typen komen overeen met een eigenschap van een Azure Active Directory (Azure AD)-object:
  
* De `UserId` type wijst een rol toe aan een gebruiker.
* De `DeviceId` type een rol toewijst aan een apparaat.
* De `DomainName` type wijst een rol toe aan een domein. Elke gebruiker met de opgegeven domeinnaam is de toegangsrechten van de bijbehorende rol.
* De `TenantId` type wijst een rol toe aan een tenant. Elke gebruiker die tot de opgegeven behoort Azure AD-tenant-ID heeft de toegangsrechten van de bijbehorende rol.
* De `ServicePrincipalId` type een rol toegewezen aan een service-principal-object-ID.
* De `UserDefinedFunctionId` type wijst een rol toe aan een gebruiker gedefinieerde functie (UDF's).

> [!div class="nextstepaction"]
> [Query of de object-ID voor een gebruiker](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [De object-ID verkrijgen voor een service-principal](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [De object-ID voor een Azure AD-tenant ophalen](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Roltoewijzingen

Om machtigingen te verlenen aan een ontvanger, een roltoewijzing te maken. Om in te trekken machtigingen, de roltoewijzing te verwijderen. Een roltoewijzing Azure digitale dubbels wordt gekoppeld aan een object, zoals een gebruiker of een Azure AD-tenant met een rol en een spatie. Machtigingen worden verleend tot alle objecten die deel uitmaken van deze ruimte. De ruimte omvat de volledige ruimtelijke grafiek eronder.

Bijvoorbeeld, een gebruiker een roltoewijzing met de rol wordt gegeven `DeviceInstaller` voor het hoofdknooppunt van een ruimtelijke grafiek, die een gebouw vertegenwoordigt. De gebruiker kan vervolgens lezen en bijwerken van apparaten voor dat knooppunt en alle andere onderliggende spaties in het gebouw.

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
