---
title: Kenmerken van Azure Active Directory-tenant interactie | Microsoft Docs
description: Uw Azure Active-tenant tenants inzicht hebt in uw tenants als volledig onafhankelijke resource beheren
services: active-tenant
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 2c00f382546a0e5f1e9da9dc90777dcf9622ec94
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Begrijpen hoe meerdere tenants van Azure Active Directory gebruiken

In Azure Active Directory (Azure AD), is elke tenant een volledig onafhankelijke resource: een peer die logisch onafhankelijk van andere tenants die u beheert. Er is geen bovenliggende / onderliggende relatie tussen de tenants. Deze onafhankelijkheid tussen de tenants bevat resourceonafhankelijkheid, beheeronafhankelijkheid en synchronisatieonafhankelijkheid.

## <a name="resource-independence"></a>Resourceonafhankelijkheid
* Als u maken of verwijderen van een resource in een tenant, heeft dit geen invloed op alle bronnen in een andere tenant, met de gedeeltelijke uitzondering van externe gebruikers. 
* Als u een van uw domeinnamen met een tenant gebruikt, wordt deze niet gebruikt met een andere tenant.

## <a name="administrative-independence"></a>Beheeronafhankelijkheid
Als een beheergebruiker van de tenant 'Contoso' een testtenant 'Test' vervolgens maakt:

* Standaard wordt de gebruiker die een tenant maakt toegevoegd als een externe gebruiker in deze nieuwe tenant en de rol globale beheerder in deze tenant toegewezen.
* De beheerders van de tenant 'Contoso' hebben geen directe beheerdersrechten voor de tenant 'Test', tenzij de beheerder van 'Test' specifiek hun deze rechten verleent. Echter, 'Contoso' kunnen beheerders toegang tot de tenant 'Test' als ze bepalen dat het gebruikersaccount dat wordt gemaakt van 'Test'.
* Als u toevoegen/verwijderen een beheerdersrol voor een gebruiker in een tenant, heeft deze wijziging geen invloed op de beheerdersrollen die de gebruiker in een andere tenant heeft.

## <a name="synchronization-independence"></a>Synchronisatieonafhankelijkheid
U kunt elke Azure AD-tenant onafhankelijk zodanig gegevens worden gesynchroniseerd vanuit één exemplaar van een configureren:

* Het hulpprogramma Azure AD Connect om gegevens te synchroniseren met één AD-forest.
* De Azure Active tenant Connector voor Forefront Identity Manager, om gegevens te synchroniseren met een of meer on-premises-forests en/of niet-Azure AD-gegevensbronnen.

## <a name="add-an-azure-ad-tenant"></a>Toevoegen van een Azure AD-tenant
Als u wilt toevoegen van een Azure AD-tenant in de Azure portal, moet u zich aanmelden bij [de Azure-portal](https://portal.azure.com) met een account dat een globale beheerder van Azure AD, en aan de linkerkant, selecteert u **nieuw**.

> [!NOTE]
> In tegenstelling tot andere Azure-resources zijn uw tenants niet onderliggende resources van een Azure-abonnement. Als uw Azure-abonnement is geannuleerd of verlopen, kunt u nog steeds toegang tot uw tenant-gegevens met Azure PowerShell, de Azure Graph API of Office 365-beheercentrum. U kunt ook [een ander abonnement koppelen aan de tenant](active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Volgende stappen
Zie voor een breed overzicht van Azure AD-licentieverlening problemen en aanbevolen procedures [wat is Azure Active-tenant-licentieverlening?](active-directory-licensing-whatis-azure-portal.md).
