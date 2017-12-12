---
title: Azure Active Directory voorwaardelijke toegang apparaatbeleid voor Office 365-services | Microsoft Docs
description: Meer informatie over het inrichten van apparaatbeleid voor voorwaardelijke toegang zodat bedrijfsbronnen meer beveiligd, met behoud van naleving van de gebruiker en toegang tot services.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 2b586aee9559b62d4cc8b21ab88ab193e61e7c14
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Active Directory voorwaardelijke toegang apparaatbeleid voor Office 365-services

Voorwaardelijke toegang is vereist voor meerdere werken. Hierbij een multi-factor geverifieerde gebruiker, een geverifieerde apparaat en een compatibel apparaat, van andere factoren. In dit artikel wordt voornamelijk zich richten op apparaat-voorwaarden die uw organisatie gebruiken kunt om u te helpen toegang tot Office 365-services beheren. 

Zakelijke gebruikers willen toegang tot Office 365-services zoals Exchange en SharePoint Online op het werk of school vanaf hun persoonlijke apparaten. Wilt u de toegang tot beveiligd zijn. U kunt beleid voor voorwaardelijke toegang apparaat om te helpen beter beveiligen van bedrijfsbronnen, bij het verlenen van toegang tot services voor gebruikers met compatibele apparaten inrichten. U kunt beleid voor voorwaardelijke toegang instellen op Office 365 in de portal voor Microsoft Intune voorwaardelijke toegang.

Azure Active Directory (Azure AD) worden afgedwongen beleid voor voorwaardelijke toegang om toegang tot Office 365-services te beveiligen. U kunt beleid voor voorwaardelijke toegang waardoor een gebruiker die gebruikmaakt van een niet-compatibele apparaten toegang krijgen tot Office 365-service maken. De gebruiker moet voldoen aan het bedrijfsbeleid apparaat om toegang tot de service te krijgen. Ook kunt u een beleid waarbij gebruikers hun apparaten toegang krijgen tot Office 365-service te registreren. Beleidsregels worden toegepast op alle gebruikers in een organisatie of beperkt tot enkele doelgroepen. U kunt meer doelgroepen toevoegen aan een beleid gedurende een bepaalde periode.

Een vereiste voor het afdwingen van beleidsregels voor apparaten is dat gebruikers hun apparaten bij de apparaatregistratieservice van Azure AD registreren moeten. U kunt ervoor kiezen om in te schakelen multi-factor authentication voor apparaten die geregistreerd bij de apparaatregistratieservice van Azure AD. Multi-factor authentication-server wordt aanbevolen voor de Azure Active Directory device registratieservice. Wanneer multi-factor authentication-server is ingeschakeld, worden gebruikers die hun apparaten bij de apparaatregistratieservice van Azure AD registreren gecontroleerd voor tweeledige verificatie.

## <a name="how-does-a-conditional-access-policy-work"></a>Hoe werkt een beleid voor voorwaardelijke toegang?

Wanneer een gebruiker aanvragen toegang tot Office 365-service van het platform van een ondersteund apparaat, wordt Azure AD verifieert de gebruiker en het apparaat. Azure AD verleent toegang tot de service alleen als de gebruiker aan het beleid dat is ingesteld voor de service voldoet. Gebruikers op apparaten die niet zijn ingeschreven krijgt instructies over het inschrijven en voldoen aan de voorwaarden voor toegang tot zakelijke Office 365-services. Gebruikers op iOS en Android-apparaten moeten hun apparaten inschrijven via de Intune-bedrijfsportal-toepassing. Wanneer een gebruiker een apparaat inschrijft, wordt het apparaat is geregistreerd bij Azure AD en ingeschreven voor beheer van apparaten en naleving. Voor mobile device management voor Office 365-services, moet u de apparaatregistratieservice van Azure AD gebruiken met Microsoft Intune. Inschrijving van apparaten is vereist voor gebruikers met toegang tot Office 365-services wanneer het apparaatbeleid wordt afgedwongen.

Wanneer een gebruiker is een apparaat inschrijft, wordt het apparaat vertrouwd wordt. Azure AD geeft de geverifieerde gebruiker één aanmelding toegang tot bedrijfstoepassingen. Azure AD worden afgedwongen beleid voor voorwaardelijke toegang om toegang te verlenen tot een service niet alleen de eerste keer dat de gebruiker toegang vraagt, maar telkens wanneer de gebruiker wordt verlengd een verzoek om toegang te krijgen. De gebruiker wanneer de aanmeldingsreferenties zijn gewijzigd, het apparaat is zoekgeraakt of gestolen, of de voorwaarden van het beleid wordt niet voldaan op het moment van de aanvraag voor certificaatvernieuwing toegang tot services geweigerd.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Apparaten registreren, moet u de apparaatregistratieservice van Azure AD gebruiken.

Wanneer on-premises gebruikers staan op het punt om te worden geverifieerd, Active Directory Federation Services (AD FS) (versie 1.0 en hoger) is vereist. Multi-factor authentication voor Workplace Join mislukt wanneer u de id-provider is niet geschikt voor multi-factor authentication-server. U kunt multi-factor authentication-server bijvoorbeeld niet gebruiken met AD FS 2.0. Zorg ervoor dat de on-premises AD FS werkt met meervoudige verificatie en een geldig multi-factor authenticatiemethode is voldaan voordat u multi-factor authentication voor de apparaatregistratieservice van Azure AD inschakelen. AD FS in Windows Server 2012 R2 heeft bijvoorbeeld de mogelijkheden van multi-factor authentication-server. Ook moet u instellen een extra geldig verificatie (MFA)-methode op de AD FS-server voordat u multi-factor authentication voor de apparaatregistratieservice van Azure AD inschakelen. Zie voor meer informatie over ondersteunde MFA-methoden in AD FS [extra verificatiemethoden configureren voor AD FS](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Volgende stappen

*   Zie voor antwoorden op veelgestelde vragen [voorwaardelijke toegang van Azure Active Directory Veelgestelde vragen over](active-directory-conditional-faqs.md).
