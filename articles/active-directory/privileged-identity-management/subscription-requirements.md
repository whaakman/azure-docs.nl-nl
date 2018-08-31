---
title: Abonnement-vereisten voor het gebruik van PIM - Azure | Microsoft Docs
description: Beschrijving van het abonnement en de licentievereisten duidelijk zijn Azure AD Privileged Identity Management (PIM) gebruiken.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 06/01/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1554895dcba0c09a3a2e19c284a1cd6f0416cfe1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190607"
---
# <a name="subscription-requirements-to-use-pim"></a>Abonnement-vereisten voor het gebruik van PIM

Azure AD Privileged Identity Management is beschikbaar als onderdeel van de Premium P2-editie van Azure AD. Zie voor meer informatie over andere functies van P2 en hoe deze zich verhoudt tot Premium P1, [Azure Active Directory-edities](../active-directory-editions.md).

>[!NOTE]
Wanneer Azure Active Directory (Azure AD) Privileged Identity Management in preview is, zijn er geen controles licentie voor een tenant om te proberen de service.  Nu dat Azure AD Privileged Identity Management is algemeen beschikbaar is bereikt, is er moet een proef- of betaald abonnement aanwezig zijn voor de tenant om door te gaan met Privileged Identity Management na December 2016.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Bevestig uw proef- of betaald abonnement

Als u niet zeker weet of uw organisatie heeft een proefversie of een abonnement heeft gekocht, kunt u vervolgens controleren of er een abonnement in uw tenant is met behulp van de opdrachten die zijn opgenomen in Azure Active Directory-Module voor Windows PowerShell V1. 
1. Open een PowerShell-venster.
2. Voer `Connect-MsolService` zich kan voordoen als een gebruiker in uw tenant.
3. Voer `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Met deze opdracht haalt een lijst van de abonnementen in uw tenant. Als er geen regels die zijn geretourneerd, moet u een aankoop van Azure AD Premium P2-proefversie, ophalen, een Azure AD Premium P2-abonnement of EMS E5-abonnement op Azure AD Privileged Identity Management gebruiken.  Als u een proef- en start met behulp van Azure AD Privileged Identity Management, lezen [aan de slag met Azure AD Privileged Identity Management](pim-getting-started.md).

Als deze opdracht retourneert een regel in welke SkuPartNumber is 'AAD_PREMIUM_P2' of 'EMSPREMIUM' en IsTrial 'True' is, dit geeft aan dat een Azure AD Premium P2-proefversie is aanwezig in de tenant.  Als de status van het abonnement is niet ingeschakeld en u hebt geen een Azure AD Premium P2- of EMS E5-abonnement kopen, moet klikt u vervolgens u een Azure AD Premium P2-abonnement of aanschaffen EMS E5-abonnement om door te gaan met behulp van Azure AD Privileged Identity Management.

Azure AD Premium P2 is beschikbaar via een [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), wordt de [Open Volume License-programma](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), en de [programma Cloud Solution Providers](https://partner.microsoft.com/cloud-solution-provider). Azure en Office 365-abonnees kunnen ook online Azure AD Premium P2 aanschaffen.  Meer informatie over prijzen van Azure AD Premium en hoe u online bestellen kan worden gevonden op [prijzen Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management is niet beschikbaar in de tenant

Azure AD Privileged Identity Management wordt niet langer beschikbaar is in uw tenant als:
- Uw organisatie is met behulp van Azure AD Privileged Identity Management wanneer deze in preview is en niet van Azure AD Premium P2-abonnement of EMS E5-abonnement kopen biedt.
- Uw organisatie heeft een Azure AD Premium P2- of EMS E5-proefversie die zijn verlopen.
- Uw organisatie heeft een aangeschafte abonnement dat is verlopen.

Wanneer een Azure AD Premium P2-abonnement of EMS E5-abonnement is verlopen, of een is organisatie die werkt met Azure AD Privileged Identity Management in de Preview-versie niet verkrijgen van Azure AD Premium P2- of EMS E5-abonnement:

- Permanente roltoewijzingen aan Azure AD-rollen niet worden gewijzigd.
- De extensie Azure AD Privileged Identity Management in Azure portal, evenals de Graph API-cmdlets en PowerShell-interfaces van Azure AD Privileged Identity Management, langer niet beschikbaar voor gebruikers bevoorrechte rollen activeren en beheren van bevoegdheden toegang tot of uitvoeren van beoordelingen van bevoorrechte rollen.
- In aanmerking komende roltoewijzingen van Azure AD-rollen worden verwijderd, omdat gebruikers wordt niet meer kunnen bevoorrechte rollen activeren.
- Alle actieve toegangsbeoordelingen van Azure AD-rollen wordt beëindigd en Azure AD Privileged Identity Management-configuratie-instellingen worden verwijderd.
- Azure AD Privileged Identity Management wordt niet meer e-mailberichten verzenden op wijzigingen aan toewijzingen van rol.

## <a name="next-steps"></a>Volgende stappen

- [PIM gebruiken](pim-getting-started.md)
- [Azure AD-maprollen die kunt u in PIM beheren](pim-roles.md)
