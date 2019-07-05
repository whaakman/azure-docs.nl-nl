---
title: Wat zijn Azure AD, ingeschreven apparaten?
description: Meer informatie over hoe identiteit Apparaatbeheer kan u helpen om apparaten die toegang hebben tot bronnen in uw omgeving te beheren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e2a8cad7cd4410a95a6ebd60ada22de456737bf
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462757"
---
# <a name="azure-ad-registered-devices"></a>Azure AD-geregistreerde apparaten

Het doel van geregistreerd bij Azure AD-apparaten is voor uw gebruikers met ondersteuning voor het Bring Your Own Device (BYOD) of de scenario's voor mobiele apparaten. In deze scenario's kan een gebruiker toegang tot bronnen van van uw organisatie Azure Active Directory die worden beheerd met behulp van een persoonlijk apparaat.

|   | Geregistreerd bij Azure AD |
| --- | --- |
| **Definitie** | Geregistreerd bij Azure AD zonder organisatieaccount te melden bij het apparaat |
| **Primaire doelgroep** | Van toepassing op alle gebruikers met de volgende criteria: |
|   | Breng uw eigen apparaat (BYOD) |
|   | Mobiele apparaten |
| **Eigendom van het apparaat** | Gebruiker of organisatie |
| **Besturingssystemen** | Windows 10, iOS, Android en MacOS |
| **Inrichten** | Windows 10-instellingen |
|   | iOS/Android-bedrijfsportal-Portal of de Microsoft Authenticator-app |
|   | MacOS-bedrijfsportal-App |
| **Apparaat aanmeldopties** | Eindgebruikers lokale referenties |
|   | Wachtwoord |
|   | Windows Hello |
|   | PINCODE |
|   | Biometrie of patroon voor andere apparaten |
| **Apparaatbeheer** | Beheer van mobiele apparaten (voorbeeld: Microsoft Intune) |
|   | mobiel applicatiebeheer |
| **Belangrijkste mogelijkheden** | Eenmalige aanmelding bronnen in de cloud |
|   | Voorwaardelijke toegang wanneer ingeschreven bij Intune |
|   | Voorwaardelijke toegang via App-beveiligingsbeleid |
|   | Hiermee kunt telefoon aanmelden bij Microsoft Authenticator-app |

![Azure AD-geregistreerde apparaten](./media/concept-azure-ad-register/azure-ad-registered-device.png)

Azure AD ingeschreven apparaten bent aangemeld bij het gebruik van een lokaal account, zoals een Microsoft-account op een Windows 10-apparaat, maar ook een Azure AD-account dat is gekoppeld voor toegang tot resources van de organisatie hebben. Toegang tot resources in de organisatie kan worden verder beperkt op basis van die Azure AD-account en beleid voor voorwaardelijke toegang toegepast op de apparaat-id.

Beheerders kunnen beveiligen en verder te bepalen deze geregistreerd bij Azure AD-apparaten met behulp van Mobile Device Management (MDM)-hulpprogramma's, zoals Microsoft Intune. MDM biedt een manier om af te dwingen organisatie vereist configuraties, zoals het vereisen van opslag moeten worden versleuteld, wachtwoordcomplexiteit en beveiligingssoftware up-to-date blijft. 

Azure AD-registratie kan worden uitgevoerd bij het openen van een work-toepassing voor de eerste keer of handmatig via het menu instellingen voor Windows 10. 

## <a name="scenarios"></a>Scenario's

Een gebruiker in uw organisatie wil toegang tot hulpprogramma's voor e-mailbericht, vrije tijd en de inschrijving van de voordelen van hun Thuiscomputer reporting. Uw organisatie heeft deze hulpprogramma's achter een beleid voor voorwaardelijke toegang waarvoor toegang vanaf een compatibel apparaat met Intune. De gebruiker hun organisatieaccount wordt toegevoegd en wordt hun Thuiscomputer geregistreerd bij Azure AD en de vereiste Intune-beleid worden afgedwongen, zodat de gebruiker toegang hebben tot hun resources.

Een andere gebruiker wil toegang tot hun organisatie e-mail op hun persoonlijke Android-telefoon die geroot is. Uw bedrijf vereist een compatibel apparaat en een Intune-nalevingsbeleid voor elk geroote apparaten wilt blokkeren is gemaakt. De werknemers is toegang hebben tot resources van de organisatie op dit apparaat gestopt.

## <a name="next-steps"></a>Volgende stappen

- [Apparaat-id's met behulp van de Azure-portal beheren](device-management-azure-portal.md)
- [Verouderde apparaten beheren in Azure AD](manage-stale-devices.md)
