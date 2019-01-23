---
title: Voorwaardelijke toegang van Azure Active Directory Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over voorwaardelijke toegang in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4fc2a5aced68ad27b87f409ff4220443648b9d1f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446048"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Voorwaardelijke toegang van Azure Active Directory Veelgestelde vragen

## <a name="which-applications-work-with-conditional-access-policies"></a>Welke toepassingen werken met beleid voor voorwaardelijke toegang?

Zie voor meer informatie over toepassingen die met beleid voor voorwaardelijke toegang werken [toepassingen en browsers die gebruikmaken van regels voor voorwaardelijke toegang in Azure Active Directory](technical-reference.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Beleid voor voorwaardelijke toegang afgedwongen voor B2B-samenwerking en gastgebruikers?

Beleid wordt afgedwongen voor gebruikers van business-to-business (B2B) samenwerking. Echter, in sommige gevallen kan een gebruiker niet mogelijk om te voldoen aan de beleidsvereisten. De organisatie van de gastgebruiker van een kan bijvoorbeeld geen ondersteuning voor meervoudige verificatie. 



## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Is een SharePoint Online-beleid ook toepassing op OneDrive voor bedrijven?

Ja. Een SharePoint Online-beleid geldt ook voor OneDrive voor bedrijven.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Waarom niet kan ik een beleid ingesteld op client-apps, zoals Word of Outlook?

Vereisten voor toegang tot een service Hiermee stelt u een beleid voor voorwaardelijke toegang. Deze wordt afgedwongen wanneer verificatie die service wordt uitgevoerd. Het beleid is niet rechtstreeks op een clienttoepassing ingesteld. In plaats daarvan wordt toegepast wanneer een client een service aanroept. Bijvoorbeeld een beleid instellen voor SharePoint is van toepassing op clients aanroepen van SharePoint. Een beleid dat is ingesteld op Exchange geldt voor Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Is een beleid voor voorwaardelijke toegang van toepassing op service-accounts?

Beleid voor voorwaardelijke toegang van toepassing op alle gebruikersaccounts. Dit geldt ook voor gebruikersaccounts die worden gebruikt als serviceaccounts. Vaak een serviceaccount dat wordt uitgevoerd zonder toezicht kan niet voldoen aan de vereisten van een beleid voor voorwaardelijke toegang. Meervoudige verificatie kan bijvoorbeeld vereist zijn. Service-accounts kunnen worden uitgesloten van een beleid met behulp van de instellingen voor beleid voor voorwaardelijke toegang. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Zijn er Graph-API's beschikbaar voor het configureren van beleid voor voorwaardelijke toegang?

Op dit moment geen. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Wat is het standaardbeleid voor uitsluiting voor niet-ondersteunde apparaatplatformen?

Beleid voor voorwaardelijke toegang worden op dit moment selectief toegepast op gebruikers van iOS- en Android-apparaten. Toepassingen op andere apparaatplatforms worden, standaard, niet beïnvloed door het beleid voor voorwaardelijke toegang voor iOS en Android-apparaten. Een tenantbeheerder wilt overschrijven het globale beleid voor het weigeren van toegang tot gebruikers op platforms die niet worden ondersteund.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Hoe werken beleid voor voorwaardelijke toegang voor Microsoft Teams?

Microsoft Teams is sterk afhankelijk van Exchange Online en SharePoint Online voor core productiviteit scenario's, zoals vergaderingen, agenda's en het delen van bestanden. Beleid voor voorwaardelijke toegang die zijn ingesteld voor deze cloud-apps van toepassing op Microsoft Teams-wanneer een gebruiker zich rechtstreeks in Microsoft Teams.

Microsoft Teams wordt ook ondersteund afzonderlijk als een cloud-app in Azure Active Directory-beleid voor voorwaardelijke toegang. Beleid voor voorwaardelijke toegang die zijn ingesteld voor een cloud-app van toepassing op Microsoft Teams-wanneer een gebruiker zich aanmeldt. Echter, zonder de juiste beleidsregels op andere apps, zoals Exchange Online en SharePoint Online gebruikers nog steeds mogelijk rechtstreeks toegang hebben tot deze resources.

Microsoft Teams-bureaublad-clients voor Windows en Mac ondersteuning voor moderne verificatie. Moderne verificatie maakt aanmelding op basis van de Azure Active Directory Authentication Library (ADAL) voor Microsoft Office-clienttoepassingen verschillende platforms.