---
title: Wat is een hybride Azure AD toegevoegd apparaat?
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
ms.openlocfilehash: 5c57180ba10322cb790c05b3f8f48043ca08b545
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462744"
---
# <a name="hybrid-azure-ad-joined-devices"></a>Hybride Azure AD-gekoppelde apparaten

Al meer dan tien jaar gebruiken vele organisaties de domeinkoppeling naar hun on-premises Active Directory om:

- IT-afdelingen vanaf een centrale locatie apparaten te laten beheren die eigendom van het bedrijf zijn.
- Gebruikers zich bij hun apparaat te laten aanmelden met hun Active Directory-werkaccount of -schoolaccount.

Organisaties met een on-premises voetafdruk zijn meestal afhankelijk van imaging-methoden om apparaten in te richten, en gebruiken vaak **System Center Configuration Manager (SCCM)** of **groepsbeleid (GP)** om ze te beheren.

Als uw omgeving een on-premises AD-voetafdruk heeft en u ook wilt profiteren van de mogelijkheden die Azure Active Directory biedt, kunt u hybride Azure AD-gekoppelde apparaten implementeren. Deze apparaten zijn apparaten die zijn toegevoegd aan uw on-premises Active Directory en geregistreerd bij uw Azure Active Directory.

|   | Hybrid Azure AD Join |
| --- | --- |
| **Definitie** | Verbonden met on-premises AD en Azure AD-organisatie-account dat zich aanmeldt bij het apparaat |
| **Primaire doelgroep** | Geschikt voor hybride organisaties met bestaande on-premises AD-infrastructuur |
|   | Van toepassing op alle gebruikers in een organisatie |
| **Eigendom van het apparaat** | Organisatie |
| **Besturingssystemen** | Windows 10, 8.1 en 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 en 2019 |
| **Inrichten** | Windows 10, Windows Server 2016/2019 |
|   | Lid van domein door IT en autojoin via Azure AD Connect of AD FS-configuratie |
|   | Lid van domein door Windows Autopilot en autojoin via Azure AD Connect of AD FS-configuratie |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, WindowsServer 2012 en Windows Server 2008 R2 - vereisen MSI |
| **Apparaat aanmeldopties** | Organisatie-accounts met behulp van: |
|   | Wachtwoord |
|   | Windows Hello voor bedrijven voor Win10 |
| **Apparaatbeheer** | Groepsbeleid |
|   | System Center Configuration Manager zelfstandige of CO-beheer met Microsoft Intune |
| **Belangrijkste mogelijkheden** | Eenmalige aanmelding tot zowel cloud en on-premises bronnen |
|   | Voorwaardelijke toegang via lid van domein of via Intune als dezelfde beheerd |
|   | Selfservice voor wachtwoordherstel en Windows Hello-PINCODE opnieuw instellen op het vergrendelingsscherm |
|   | Enterprise State Roaming via apparaten |

![Hybride Azure AD-gekoppelde apparaten](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Scenario's

Hybride, gebruikt Azure AD gekoppelde apparaten als:

- U Win32-apps op deze apparaten hebt geïmplementeerd die afhankelijk zijn van Active Directory-machineverificatie.
- Wilt u doorgaan met het groepsbeleid gebruiken voor het beheren van apparaatconfiguratie.
- Wilt u echter ook doorgaan met de bestaande installatiekopieën oplossingen te implementeren en configureren van apparaten.
- U moet ondersteuning voor downlevel Windows 7 en 8.1 naast Windows 10-apparaten

## <a name="next-steps"></a>Volgende stappen

- [Uw implementatie van hybride Azure AD-deelname plannen](hybrid-azuread-join-plan.md)
- [Apparaat-id's met behulp van de Azure-portal beheren](device-management-azure-portal.md)
- [Verouderde apparaten beheren in Azure AD](manage-stale-devices.md)
