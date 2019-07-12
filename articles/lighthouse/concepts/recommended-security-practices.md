---
title: Aanbevolen beveiligingsprocedures voor Azure naar
description: Wanneer met behulp van Azure Resourcemanagement overgedragen, is het belangrijk dat beveiliging en toegangsbeheer.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 843b965e6ea74a7c11dc11459ff5d30ddbe5c987
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809867"
---
# <a name="recommended-security-practices"></a>Aanbevolen beveiligingsprocedures

Wanneer met behulp van Azure Resourcemanagement overgedragen, is het belangrijk dat beveiliging en toegangsbeheer. Gebruikers in uw tenant hebben rechtstreeks toegang tot abonnementen en resourcegroepen, dus moet u stappen ondernemen om te onderhouden van beveiliging van uw tenant. Moet u controleren of dat u alleen de toegang die nodig is om op doeltreffende uw klanten resources toestaan. Dit onderwerp bevat aanbevelingen voor het doen.

## <a name="require-azure-multi-factor-authentication"></a>Azure multi-factor Authentication vereisen

[Azure multi-factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (ook wel bekend als verificatie in twee stappen) helpt voorkomen dat aanvallers toegang krijgen tot een account doordat meerdere stappen voor verificatie. U moet meervoudige verificatie vereisen voor alle gebruikers in uw serviceprovider-tenant, met inbegrip van gebruikers die toegang tot resources van de klant.

Het is raadzaam dat u uw klanten voor het implementeren van Azure multi-factor Authentication in hun tenants ook vragen.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>Machtigingen toewijzen aan groepen met behulp van het principe van minimale bevoegdheden

Beheer om eenvoudiger te maken, wordt u aangeraden Azure AD-gebruikersgroepen voor elke rol die is vereist om uw klanten resources te beheren. Hiermee kunt u afzonderlijke gebruikers aan de groep indien nodig, in plaats van machtigingen worden toegewezen aan die gebruiker rechtstreeks toevoegen of verwijderen.

Bij het maken van de structuur van uw toestemming, moet u het principe van minimale bevoegdheden volgen zodat gebruikers hebben alleen de machtigingen die nodig zijn om hun taak te voltooien waardoor de kans dat onbedoeld fouten.

U wilt bijvoorbeeld een structuur als volgt gebruiken:

|Groepsnaam  |type  |principalId  |Roldefinitie  |Roldefinitie-ID  |
|---------|---------|---------|---------|---------|
|Architecten     |Gebruikersgroep         |\<principalId\>         |Inzender         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Beoordeling     |Gebruikersgroep         |\<principalId\>         |Lezer         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|VM-specialisten     |Gebruikersgroep         |\<principalId\>         |VM-Inzender         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Automation     |Service principal name (SPN)         |\<principalId\>         |Inzender         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Als u deze groepen hebt gemaakt, kunt u gebruikers kunt toewijzen, indien nodig. Alleen de gebruikers toevoegen die echt moeten toegang hebben. Zorg dat het lidmaatschap van regelmatig controleren en verwijderen van alle gebruikers die niet langer juiste of die nodig zijn om op te nemen.

Houd er rekening mee dat wanneer u [Onboarding van klanten via een openbare beheerde service-aanbieding](../how-to/publish-managed-services-offers.md), een groep (of gebruiker of service-principal) die u heeft dezelfde machtigingen voor elke klant die het abonnement koopt. Om te wijzen aan verschillende groepen om te werken met elke klant, moet u een aparte privé plan dat is afzonderlijk exclusief voor elke klant of de Onboarding van klanten met behulp van Azure Resource Manager-sjablonen publiceren. Bijvoorbeeld, kan u een openbare-plan met zeer beperkte toegang, en vervolgens samen met de klant direct voor de Onboarding van hun bronnen voor aanvullende toegang die met een aangepaste Azure-Resourcesjabloon aanvullende toegang verlenen, indien nodig.


## <a name="next-steps"></a>Volgende stappen

- [Implementeren van Azure multi-factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md).
- Meer informatie over [ervaringen voor cross-tenant](cross-tenant-management-experience.md).
