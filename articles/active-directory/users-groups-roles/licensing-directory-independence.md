---
title: Kenmerken van Azure Active Directory-tenant interactie | Microsoft Docs
description: Uw Azure Active-tenant-tenants inzicht hebt in uw tenants als volledig onafhankelijke resource beheren
services: active-tenant
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 85f7cddb7231bf9c5e45de87af3c922148f214be
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861878"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Inzicht in hoe meerdere Azure Active Directory-tenants gebruiken

In Azure Active Directory (Azure AD), wordt elke tenant een volledig onafhankelijke resource: een peer die logisch onafhankelijk van andere tenants die u beheert. Er is geen bovenliggende / onderliggende relatie tussen tenants. Deze onafhankelijkheid tussen tenants omvat resourceonafhankelijkheid, beheeronafhankelijkheid en synchronisatieonafhankelijkheid.

## <a name="resource-independence"></a>Resourceonafhankelijkheid
* Als u maakt of een resource in één tenant verwijdert, heeft dit geen invloed op alle bronnen in een andere tenant, gedeeltelijke met uitzondering van externe gebruikers. 
* Als u een van uw domeinnamen met één tenant gebruikt, kan deze niet worden gebruikt met een andere tenant.

## <a name="administrative-independence"></a>Beheeronafhankelijkheid
Als een beheergebruiker van de tenant 'Contoso' een testtenant 'Test', klikt u vervolgens maakt:

* Standaard wordt de gebruiker die een tenant maakt toegevoegd als een externe gebruiker in deze nieuwe tenant en de rol globale beheerder in deze tenant is toegewezen.
* De beheerders van de tenant 'Contoso' hebben geen directe beheerdersrechten voor de tenant 'Test', tenzij een beheerder van 'Test' ze deze bevoegdheden verleent. Beheerders van 'Contoso' kunnen echter toegang tot de tenant 'Test' beheren, als ze het gebruikersaccount dat wordt gemaakt van 'Test'.
* Als u toevoegen/verwijderen een beheerdersrol voor een gebruiker in één tenant, de wijziging heeft geen invloed op de beheerdersrollen die de gebruiker in een andere tenant heeft.

## <a name="synchronization-independence"></a>Synchronisatieonafhankelijkheid
U kunt elke Azure AD-tenant onafhankelijk zodanig gegevens worden gesynchroniseerd vanuit één exemplaar van een configureren:

* Het hulpprogramma Azure AD Connect om gegevens te synchroniseren met één AD-forest.
* De Azure Active tenant Connector voor Forefront Identity Manager, om gegevens te synchroniseren met een of meer on-premises-forests en/of niet-Azure AD-gegevensbronnen.

## <a name="add-an-azure-ad-tenant"></a>Een Azure AD-tenant toevoegen
Als u wilt toevoegen een Azure AD-tenant in Azure portal, moet u zich aanmelden bij [de Azure-portal](https://portal.azure.com) met een account dat een globale beheerder van Azure AD, en aan de linkerkant, selecteert u **nieuw**.

> [!NOTE]
> In tegenstelling tot andere Azure-resources zijn uw tenants geen onderliggende resources van een Azure-abonnement. Als uw Azure-abonnement is geannuleerd of verlopen, kunt u nog steeds toegang tot de gegevens van uw tenant met behulp van Azure PowerShell, de Azure Graph API of de Office 365-beheercentrum. U kunt ook [een ander abonnement koppelen aan de tenant](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Volgende stappen
Zie voor een breed overzicht van Azure AD-licentieverlening problemen en aanbevolen procedures, [wat is Azure Active-tenant-licentieverlening?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
