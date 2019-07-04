---
title: Wat is de apparaat-id in Azure Active Directory?
description: Meer informatie over hoe identiteit Apparaatbeheer kan u helpen om de apparaten die toegang hebben tot bronnen in uw omgeving te beheren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d38ca8bdf93ff201b3f5842f4cb0e8409dcd0c3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481664"
---
# <a name="what-is-a-device-identity"></a>Wat is een apparaat-id?

Met de komst van apparaten van alle vormen en grootten en het concept Bring Your Own Device (BYOD), worden de IT-professionals geconfronteerd met twee enigszins andere doelen:

- Toestaan dat eindgebruikers productief waar en wanneer
- Beveiligen van de activa van de organisatie

Ter bescherming van deze middelen, moet IT-personeel eerst de apparaat-id's te beheren. IT-personeel kunnen gebruikmaken van de apparaat-id met hulpprogramma's, zoals Microsoft Intune om ervoor te zorgen standaarden voor beveiliging en naleving wordt voldaan. Azure Active Directory (Azure AD) kunt single sign-on bij apparaten, apps en services vanaf een willekeurige plaats via deze apparaten.

- Uw gebruikers krijgen toegang tot activa van uw organisatie die ze nodig hebben. 
- Uw IT-medewerkers krijgen de besturingselementen die ze nodig hebben voor het beveiligen van uw organisatie.

Apparaat-identiteitsbeheer vormt de basis voor [apparaat gebaseerde voorwaardelijke toegang](../conditional-access/require-managed-devices.md). Met beleid voor voorwaardelijke toegang op basis van apparaat, kunt u ervoor zorgen dat toegang tot bronnen in uw omgeving is alleen mogelijk met beheerde apparaten.

## <a name="getting-devices-in-azure-ad"></a>Ophalen van apparaten in Azure AD

Als u een apparaat in Azure AD, hebt u meerdere opties:

- **Geregistreerd bij Azure AD**
   - Apparaten die geregistreerd bij Azure AD zijn zijn doorgaans persoonlijk eigendom zijn of mobiele apparaten en bent aangemeld bij met een persoonlijk Microsoft-account of een andere lokale account.
      - Windows 10
      - iOS
      - Android
      - MacOS
- **Azure AD-domein**
   - Apparaten die toegevoegd aan Azure AD zijn eigendom zijn van een organisatie, en bent aangemeld bij met een Azure AD-account die behoren tot deze organisatie. Ze bestaan alleen in de cloud.
      - Windows 10 
- **lid is van hybride Azure Active Directory**
   - Apparaten die zijn toegevoegd aan hybrid Azure AD join eigendom zijn van een organisatie, en bent aangemeld bij met een Azure AD-account die behoren tot deze organisatie. Ze bestaan in de cloud en 
      - Windows 7, 8.1 en 10
      - WindowsServer 2008 of hoger

![Apparaten die worden weergegeven in de blade Azure AD-apparaten](./media/overview/azure-ad-devices-all-devices-overview.png)

## <a name="device-management"></a>Apparaatbeheer

Apparaten in Azure AD kunnen worden beheerd via Mobile Device Management (MDM)-hulpprogramma's zoals Microsoft Intune, System Center Configuration Manager, Group Policy (hybrid Azure AD join), hulpprogramma's voor Mobile Application Management (MAM) of andere hulpprogramma's van derden.

## <a name="resource-access"></a>Toegang tot resources

Registreren en deze koppelen aan uw gebruikers naadloze eenmalige aanmelding (SSO) in de cloud-resources en beheerders de mogelijkheid om toe te passen van beleid voor voorwaardelijke toegang tot deze bronnen te verlenen. 

Apparaten die toegevoegd aan Azure AD zijn of hybride Azure AD lid van het belangrijkste voordeel van eenmalige aanmelding voor uw organisatie on-premises bronnen, evenals cloud-bronnen. Meer informatie vindt u in het artikel [hoe eenmalige aanmelding voor on-premises resources werkt in Azure AD gekoppelde apparaten](azuread-join-sso.md).

## <a name="device-security"></a>Beveiligde apparaten

- **Azure AD ingeschreven apparaten** gebruikmaken van een account dat wordt beheerd door de gebruiker, dit account is een Microsoft-account of een ander lokaal beheerde referenties die zijn beveiligd met een of meer van de volgende.
   - Wachtwoord
   - PINCODE
   - Patroon
   - Windows Hello
- **Azure AD join of hybride Azure AD gekoppelde apparaten** gebruikmaken van een organisatie-account in Azure AD die worden beveiligd met een of meer van de volgende.
   - Wachtwoord
   - Windows Hello voor Bedrijven

## <a name="provisioning"></a>Inrichten

Ophalen van apparaten in Azure AD kan worden gedaan in een manier die zelf of in een gecontroleerde inrichtingsproces door beheerders.

## <a name="summary"></a>Samenvatting

Met identiteit Apparaatbeheer in Azure AD, kunt u het volgende doen:

- Het vereenvoudigen van halen en het beheer van apparaten in Azure AD
- Uw gebruikers gemakkelijke toegang tot de cloudgebaseerde bronnen van uw organisatie bieden

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure AD ingeschreven apparaten](concept-azure-ad-register.md)
- Meer informatie over [Azure AD gekoppelde apparaten](concept-azure-ad-join.md)
- Meer informatie over [hybride Azure AD gekoppelde apparaten](concept-azure-ad-join-hybrid.md)
- Als u een overzicht van hoe u voor het beheren van apparaat-id's in Azure portal, Zie [beheren van apparaat-id's met behulp van de Azure-portal](device-management-azure-portal.md).
- Zie voor meer informatie over voorwaardelijke toegang op basis van apparaat, [beleid voor het apparaat gebaseerde voorwaardelijke toegang van Azure Active Directory configureren](../conditional-access/require-managed-devices.md).
