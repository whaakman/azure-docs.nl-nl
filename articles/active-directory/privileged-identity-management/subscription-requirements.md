---
title: Privileged Identity Management - Azure-abonnementen | Microsoft Docs
description: Legt uit het abonnement en de vereisten voor het beheren en gebruiken van Azure AD Privileged Identity Management in uw tenant-licentieverlening
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: mwahl
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: b04544c6a1d288524783ed6d323146c0ef7bfe95
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Vereisten voor Azure Active Directory Privileged Identity Management abonnement

Azure AD Privileged Identity Management is beschikbaar als onderdeel van de Premium P2-editie van Azure AD. Zie voor meer informatie over andere functies van P2 en hoe deze zich verhoudt tot Premium P1 [Azure Active Directory-edities](../active-directory-editions.md).

>[!NOTE]
Wanneer u Azure Active Directory (Azure AD) Privileged Identity Management is in de preview, zijn er geen controles licentie voor een tenant om te proberen de service.  Nu dat Azure AD Privileged Identity Management is algemeen beschikbaar is bereikt, moet een proef- of betaald abonnement aanwezig zijn voor de tenant om door te gaan met Privileged Identity Management na December 2016.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Bevestig uw proef- of betaald abonnement

Als u niet zeker weet of uw organisatie beschikt over een proefversie of abonnement hebt aangeschaft, u controleren kunt of er een abonnement in uw tenant is met behulp van de opdrachten in Azure Active Directory-Module voor Windows PowerShell V1 opgenomen. 
1. Open een PowerShell-venster.
2. Voer `Connect-MsolService` om te verifiëren als een gebruiker in uw tenant.
3. Voer `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Met deze opdracht haalt een lijst van de abonnementen in uw tenant. Als er geen regels die zijn geretourneerd zijn, moet u voor het ophalen van een Azure AD Premium-P2 proef, voor aankoop een abonnement voor Azure AD Premium-P2 of EMS E5-abonnement op Azure AD Privileged Identity Management te gebruiken.  Als u een proefversie en begin met behulp van Azure AD Privileged Identity Management, lezen [aan de slag met Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

Als u deze opdracht retourneert een regel in welke SkuPartNumber is 'AAD_PREMIUM_P2' of 'EMSPREMIUM' en IsTrial 'True' is, betekent dit een proefversie van Azure AD Premium-P2 aanwezig in de tenant.  Als de status van het abonnement is niet ingeschakeld en u geen Azure AD Premium P2- of EMS E5-abonnement aanschaffen hebt, moet klikt u vervolgens u een abonnement voor Azure AD Premium-P2 of aanschaffen EMS E5-abonnement om door te gaan met Azure AD Privileged Identity Management.

Azure AD Premium-P2 is beschikbaar via een [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), wordt de [Open Volume License-programma](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), en de [programma Cloud Solution Providers](https://partner.microsoft.com/en-US/cloud-solution-provider). Azure en Office 365-abonnees zijn ook verkrijgbaar online Azure AD Premium-P2.  Meer informatie over prijzen voor Azure AD Premium en online bestellen kan worden gevonden op [Azure Active Directory prijzen](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management is niet beschikbaar in de tenant

Azure AD Privileged Identity Management wordt niet langer beschikbaar in uw tenant als:
- Uw organisatie is Azure AD Privileged Identity Management gebruikt bij het Preview-versie is en niet heeft aangeschaft Azure AD Premium P2-abonnement of EMS E5-abonnement.
- Uw organisatie heeft een Azure AD Premium-P2 of EMS E5 evaluatieversie die verlopen.
- Uw organisatie heeft een aangeschafte abonnement dat is verlopen.

Wanneer u een Azure AD Premium P2-abonnement of EMS E5-abonnement is verlopen, of een biedt organisatie die werkt met Azure AD Privileged Identity Management in preview Azure AD Premium P2- of EMS E5-abonnement niet ophalen:

- Permanente roltoewijzingen aan Azure AD-rollen niet worden gewijzigd.
- De extensie Azure AD Privileged Identity Management in de Azure-portal, evenals de Graph API-cmdlets en PowerShell-interfaces van Azure AD Privileged Identity Management wordt niet langer beschikbaar voor gebruikers bevoorrechte rollen activeren, het beheren van bevoorrechte toegang of het uitvoeren van toegang beoordelingen van bevoorrechte rollen.
- In aanmerking komende roltoewijzingen van Azure AD-rollen worden verwijderd, wanneer gebruikers zich niet langer bevoorrechte rollen activeren.
- Alle beoordelingen lopende toegang van Azure AD-rollen wordt beëindigd en Azure AD Privileged Identity Management-configuratie-instellingen worden verwijderd.
- Azure AD Privileged Identity Management wordt niet langer met het verzenden van e-mailberichten op wijzigingen aan toewijzingen van rollen.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md)
- [Rollen in Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-roles.md)
