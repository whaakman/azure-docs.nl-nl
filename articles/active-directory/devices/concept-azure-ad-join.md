---
title: Wat is een Azure AD toegevoegd apparaat?
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
ms.openlocfilehash: 4af3aea7218ea8792bb66188e8df7baf9f460b0b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462809"
---
# <a name="azure-ad-joined-devices"></a>Azure AD-gekoppelde apparaten

Azure AD join is bedoeld voor organisaties die willen cloud-first of alleen in de cloud. Elke organisatie kunt implementeren in Azure AD gekoppelde apparaten, ongeacht de grootte of bepaalde industrie voldoen. Azure AD join werkt zelfs in een hybride omgeving, toegang tot zowel cloud en on-premises apps en resources.

|   | Azure AD Join |
| --- | --- |
| **Definitie** | Alleen lid is van Azure AD-organisatie-account dat zich aanmeldt bij het apparaat |
| **Primaire doelgroep** | Geschikt voor beide alleen in de cloud en hybride organisaties. |
|   | Van toepassing op alle gebruikers in een organisatie |
| **Eigendom van het apparaat** | Organisatie |
| **Besturingssystemen** | Alle Windows 10-apparaten |
| **Inrichten** | Self-service: Windows OOBE- of -instellingen |
|   | Bulkinschrijving |
|   | Windows Autopilot |
| **Apparaat aanmeldopties** | Organisatie-accounts met behulp van: |
|   | Wachtwoord |
|   | Windows Hello voor Bedrijven |
|   | Beveiligingssleutels FIDO2.0 (preview) |
| **Apparaatbeheer** | Beheer van mobiele apparaten (voorbeeld: Microsoft Intune) |
|   | CO-beheer met Microsoft Intune en System Center Configuration Manager |
| **Belangrijkste mogelijkheden** | Eenmalige aanmelding tot zowel cloud en on-premises bronnen |
|   | Voorwaardelijke toegang via MDM-registratie en evaluatie van de MDM-naleving |
|   | Selfservice voor wachtwoordherstel en Windows Hello-PINCODE opnieuw instellen op het vergrendelingsscherm |
|   | Enterprise State Roaming via apparaten |

Azure AD gekoppelde apparaten voor het gebruik van een organisatie zijn ondertekend in Azure AD-account. Toegang tot resources in de organisatie kan verder worden beperkt op basis van die Azure AD-account en [beleid voor voorwaardelijke toegang](../conditional-access/overview.md) toegepast op de apparaat-id.

Beheerders kunnen beveiligen en verdere controle Azure AD-apparaten met behulp van Mobile Device Management (MDM)-hulpprogramma's, zoals Microsoft Intune of in scenario's met CO-beheer met behulp van System Center Configuration Manager toegevoegd. Deze hulpprogramma's bieden een mogelijkheid organisatie vereist configuraties, zoals het vereisen van opslag moeten worden versleuteld, wachtwoordcomplexiteit, software-installaties en software-updates af te dwingen. Beheerders kunnen organisatie toepassingen beschikbaar te maken in Azure AD gekoppelde apparaten met behulp van [System Center Configuration Manager en de Microsoft Store voor bedrijven](https://docs.microsoft.com/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business).

Azure AD join kan worden gerealiseerd met selfservice-opties, zoals de Out of Box Experience (OOBE), bulkinschrijving, of [Windows Autopilot](https://docs.microsoft.com/intune/enrollment-autopilot).

Azure AD gekoppelde apparaten gewoon nog steeds één aanmelding toegang tot de on-premises bronnen wanneer ze op het netwerk van de organisatie. Apparaten die toegevoegd aan Azure AD zijn kunnen nog steeds verifiëren met on-premises servers zoals bestand, afdrukken en andere toepassingen.

## <a name="scenarios"></a>Scenario's

Hoewel Azure AD-koppeling voornamelijk is bedoeld voor organisaties die geen on-premises Windows Server Active Directory-infrastructuur hebben, kunt u dit beslist gebruiken in scenario’s waarbij:

- U wilt overstappen op cloudgebaseerde infrastructuur met behulp van Azure AD en MDM zoals Intune.
- U geen on-premises domeinkoppeling kunt gebruiken, bijvoorbeeld als u mobiele apparaten zoals tablets en telefoons onder controle moet krijgen.
- Uw gebruikers voornamelijk toegang nodig hebben tot Office 365 of andere SaaS-apps die zijn geïntegreerd met Azure AD.
- U een groep gebruikers in Azure AD wilt beheren in plaats van in Active Directory. In dit scenario kunt toepassen, bijvoorbeeld op seizoenswerkers, aannemers of studenten.
- U koppelingsmogelijkheden wilt bieden aan medewerkers in externe filialen met beperkte on-premises infrastructuur.

U kunt Azure AD-gekoppelde apparaten configureren voor Windows 10-apparaten.

Het doel van Azure AD-gekoppelde apparaten is vereenvoudiging van:

- Windows-implementaties van apparaten die eigendom van het bedrijf zijn
- Toegang tot apps en bronnen van de organisatie vanaf elk Windows-apparaat
- Cloudgebaseerd beheer van apparaten die eigendom van het bedrijf zijn
- Gebruikers zich aanmelden op hun apparaten met hun Azure AD of gesynchroniseerde Active Directory werk- of schoolaccount.

![Azure AD-gekoppelde apparaten](./media/concept-azure-ad-join/azure-ad-joined-device.png)

Azure AD-koppeling kan via een van de volgende methoden worden geïmplementeerd:

- [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Bulkimplementatie](https://docs.microsoft.com/intune/windows-bulk-enroll)
- [Selfservice-ervaring](azuread-joined-devices-frx.md)

## <a name="next-steps"></a>Volgende stappen

- [Uw Azure AD join-implementatie plannen](azureadjoin-plan.md)
- [Over het beheren van de lokale groep administrators op de Azure AD gekoppelde apparaten](assign-local-admin.md)
- [Apparaat-id's met behulp van de Azure-portal beheren](device-management-azure-portal.md)
- [Verouderde apparaten beheren in Azure AD](manage-stale-devices.md)
