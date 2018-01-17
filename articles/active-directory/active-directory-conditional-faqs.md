---
title: Voorwaardelijke toegang van Azure Active Directory Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over voorwaardelijke toegang in Azure Active Directory.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: e89550763209bc2d56dbc5dd374239ff404ccb11
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Voorwaardelijke toegang van Azure Active Directory Veelgestelde vragen

## <a name="which-applications-work-with-conditional-access-policies"></a>Welke toepassingen werken met beleid voor voorwaardelijke toegang?

Zie voor meer informatie over toepassingen die met beleid voor voorwaardelijke toegang werken [toepassingen en browsers die gebruikmaken van regels voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Afgedwongen beleid voor voorwaardelijke toegang voor B2B-samenwerking en gastgebruikers?

Beleid wordt afgedwongen voor business-to-business (B2B) samenwerking gebruikers. Echter, in sommige gevallen kan een gebruiker niet mogelijk om te voldoen aan de beleidsvereisten. De organisatie van de gastgebruiker van een mogelijk bijvoorbeeld geen ondersteuning voor multi-factor authentication-server. 



## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Een SharePoint Online-beleid ook van toepassing op OneDrive voor bedrijven?

Ja. Een SharePoint Online-beleid geldt ook voor OneDrive voor bedrijven.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Waarom niet kan ik een beleid op ClientApps, zoals Word of Outlook instellen?

Beleid voor voorwaardelijke toegang stelt vereisten voor het openen van een service. Het wordt afgedwongen wanneer verificatie die service optreedt. Het beleid is niet rechtstreeks op een clienttoepassing ingesteld. In plaats daarvan wordt toegepast wanneer een client een service aanroept. Een beleid dat is ingesteld op SharePoint geldt bijvoorbeeld voor clients voor het aanroepen van SharePoint. Een beleid dat is ingesteld op Exchange geldt voor Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Is een beleid voor voorwaardelijke toegang van toepassing op service-accounts?

Beleid voor voorwaardelijke toegang van toepassing op alle gebruikersaccounts. Dit omvat de gebruikersaccounts die worden gebruikt als serviceaccounts. Vaak een serviceaccount dat wordt uitgevoerd zonder toezicht kan niet voldoen aan de vereisten van een beleid voor voorwaardelijke toegang. Multi-factor authentication-server zijn vereist. Service-accounts kunnen worden uitgesloten van een beleid met behulp van de instellingen voor beleid voor voorwaardelijke toegang. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Graph API's beschikbaar zijn voor beleid voor voorwaardelijke toegang configureren?

Op dit moment niet. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Wat is het standaardbeleid voor uitsluiting voor niet-ondersteunde platforms?

Beleid voor voorwaardelijke toegang worden op dit moment selectief afgedwongen voor gebruikers van iOS- en Android-apparaten. Toepassingen op andere apparaatplatforms, standaard niet worden beïnvloed door het beleid voor voorwaardelijke toegang voor iOS en Android-apparaten. Een tenantbeheerder kunt kiezen voor het onderdrukken van het globaal beleid niet toestaan van toegang tot gebruikers op platforms die niet worden ondersteund.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Hoe werken beleidsregels voor voorwaardelijke toegang voor Microsoft-Teams?

Microsoft-Teams is sterk afhankelijk van Exchange Online en SharePoint Online voor core productiviteit scenario's, zoals vergaderingen, agenda's en bestanden delen. Beleid voor voorwaardelijke toegang die zijn ingesteld voor deze cloud-apps van toepassing op Microsoft-Teams wanneer een gebruiker zich rechtstreeks in de Microsoft-Teams.

Microsoft-Teams wordt ook ondersteund afzonderlijk als een cloud-app in Azure Active Directory-beleid voor voorwaardelijke toegang. Beleid voor voorwaardelijke toegang die zijn ingesteld voor een cloud-app van toepassing op Microsoft-Teams wanneer een gebruiker zich aanmeldt. Echter, zonder de juiste beleidsregels op andere apps zoals Exchange Online en SharePoint Online gebruikers nog steeds mogelijk rechtstreeks toegang hebben tot deze bronnen.

Microsoft-Teams bureaublad-clients voor Windows en Mac ondersteuning voor moderne verificatie. Moderne verificatie brengt op aanmelden op basis van de Azure Active Directory Authentication Library (ADAL) voor Microsoft Office-clienttoepassingen in verschillende platforms.