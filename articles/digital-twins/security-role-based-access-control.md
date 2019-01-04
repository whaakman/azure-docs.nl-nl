---
title: Informatie over Azure digitale dubbels op rollen gebaseerd toegangsbeheer | Microsoft Docs
description: Meer informatie over verificatie in digitale dubbels met op rollen gebaseerd toegangsbeheer.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: lyrana
ms.openlocfilehash: c1e48f6cb7ca4f26b8cafc31605bc9441ed047d3
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807598"
---
# <a name="role-based-access-control-in-azure-digital-twins"></a>Op rollen gebaseerd toegangsbeheer in Azure, digitale dubbels

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

- Een gebruiker de mogelijkheid voor het beheren van apparaten voor een heel gebouw, of alleen voor een specifieke ruimte of een verdieping verlenen.
- Verleen een beheerder wereldwijde toegang tot alle knooppunten van het ruimtelijke graph voor een hele grafiek of alleen voor een gedeelte van de grafiek.
- Een ondersteuning voor gespecialiseerde lezen toegang verlenen tot de grafiek, met uitzondering van toegangssleutels.
- Elk lid van een domein-leestoegang tot alle objecten voor graph verlenen.

## <a name="rbac-best-practices"></a>Aanbevolen procedures voor RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Roldefinities

De roldefinitie van een is een verzameling van machtigingen en andere kenmerken die deel uitmaken van een rol. Een roldefinitie geeft een lijst van de toegestane bewerkingen, waaronder *maken*, *lezen*, *UPDATE*, en *verwijderen* dat een met die object rol kan uitvoeren. Het geeft ook waarop object typen machtigingen toepassen op.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

>[!NOTE]
> Query uitvoeren op het systeem/rollen API om op te halen van de volledige definities voor de vorige rollen.
> Voor meer informatie lezen [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md#all).

### <a name="object-identifier-types"></a>Object-id typen

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

>[!TIP]
> Meer informatie over het verlenen van machtigingen aan uw service-principal door te lezen [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md#grant).

De volgende artikelen voor referentie-documentatie wordt beschreven:

- Hoe u [Query of de object-ID voor een gebruiker](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0).
- Hoe u [verkrijgen van de object-ID voor een service-principal](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1).
- Hoe u [de object-ID ophalen voor een Azure AD-tenant](../active-directory/develop/quickstart-create-new-tenant.md).

## <a name="role-assignments"></a>Roltoewijzingen

Een roltoewijzing Azure digitale dubbels wordt gekoppeld aan een object, zoals een gebruiker of een Azure AD-tenant met een rol en een spatie. Machtigingen worden verleend tot alle objecten die deel uitmaken van deze ruimte. De ruimte omvat de volledige ruimtelijke grafiek eronder.

Bijvoorbeeld, een gebruiker een roltoewijzing met de rol wordt gegeven `DeviceInstaller` voor het hoofdknooppunt van een ruimtelijke grafiek, die een gebouw vertegenwoordigt. De gebruiker kan vervolgens lezen en bijwerken van apparaten voor dat knooppunt en alle andere onderliggende spaties in het gebouw.

Om machtigingen te verlenen aan een ontvanger, een roltoewijzing te maken. Om in te trekken machtigingen, de roltoewijzing te verwijderen.

>[!IMPORTANT]
> Meer informatie over de roltoewijzingen vindt [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over het maken en beheren van roltoewijzingen Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
