---
title: Kiezen tussen de cloud of server van Azure MFA| Microsoft Docs
description: 'Kies de Multi-Factor Authentication-beveiligingsoplossing die bij u past, door uzelf de vraag te stellen: ''Wat wil ik beveiligen en waar bevinden zich mijn gebruikers?''  Kies vervolgens de cloud, MFA-Server of AD FS.'
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: kgremban
ms.openlocfilehash: cc5a0a8ff789b878cd5afb62176ae8bcf3f80902
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Kies de Azure Multi-Factor Authentication-oplossing die bij u past
Omdat er verschillende versies van Azure Multi-Factor Authentication (MFA) zijn, moet u een aantal vragen beantwoorden om te kunnen beoordelen welke versie voor u de beste is.  Deze vragen zijn:

* [Wat wil ik beveiligen?](#what-am-i-trying-to-secure)
* [Waar bevinden de gebruikers zich?](#where-are-the-users-located)
* [Welke functies heb ik nodig?](#what-featured-do-i-need)

In de volgende secties vindt u richtlijnen die u helpen om elk van deze vragen te beantwoorden.

## <a name="what-am-i-trying-to-secure"></a>Wat wil ik beveiligen?
Om te bepalen welke oplossing voor verificatie in twee stappen de juiste is, moet eerst de vraag worden beantwoord wat u wilt beveiligen met een tweede verificatiemethode.  Is het een toepassing die deel uitmaakt van Azure?  Of een systeem met externe toegang?  Door te bepalen wat we willen beveiligen, kan de vraag worden beantwoord waar Multi-Factor Authentication moet worden ingeschakeld.  

| Wat wilt u beveiligen | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Eigen Microsoft-apps |● |● |
| SaaS-apps in de app-galerie |● |  |
| Webtoepassingen die zijn gepubliceerd via de toepassingsproxy van Azure AD |● |  |
| IIS-toepassingen die niet zijn gepubliceerd via toepassingsproxy van Azure AD | |● |
| Externe toegang zoals VPN, RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Waar bevinden de gebruikers zich?
Als we vervolgens kijken waar onze gebruikers zich bevinden, kunnen we bepalen wat de juiste oplossing is, in de cloud of on-premises via de MFA Server.

| Locatie van gebruikers | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD en on-premises AD dat gebruikmaakt van federatie met AD FS |● |● |
| Azure AD en on-premises AD dat gebruikmaakt van DirSync, Azure AD Sync, Azure AD Connect - geen wachtwoordsynchronisatie |● |● |
| Azure AD en on-premises AD dat gebruikmaakt van DirSync, Azure AD Sync, Azure AD Connect - met wachtwoordsynchronisatie |● | |
| On-premises Active Directory | |● |

## <a name="what-features-do-i-need"></a>Welke functies heb ik nodig?
De volgende tabel bevat een vergelijking van de functies van Multi-Factor Authentication die beschikbaar zijn in de cloud en de functies die beschikbaar zijn op de Multi-Factor Authentication-server.

| Functie | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Meldingen op mobiele app als een tweede factor | ● | ● |
| Een verificatiecode op mobiele app als een tweede factor | ● | ● |
| Telefoonoproep als tweede factor | ● | ● |
| Sms in één richting als tweede factor | ● | ● |
| Sms in twee richtingen als tweede factor | | ● |
| Hardwaretokens als tweede factor | | ● |
| App-wachtwoorden voor Office 365-clients die geen ondersteuning voor MFA bieden | ● | |
| Beheerdercontrole over verificatiemethoden | ● | ● |
| Pincodemodus | | ● |
| Fraudewaarschuwing |● | ● |
| MFA-rapporten |● | ● |
| Eenmalige toegang | | ● |
| Aangepaste begroeting voor telefoongesprekken | ● | ● |
| Aanpasbare nummerweergave voor telefoongesprekken | ● | ● |
| Goedgekeurde IP-adressen | ● | ● |
| MFA herinneren voor vertrouwde apparaten | ● | |
| Voorwaardelijke toegang | ● | ● |
| Cache |  | ● |

## <a name="next-steps"></a>Volgende stappen

U weet nu wat het verschil is tussen Azure Multi-Factor Authentication in de cloud en on-premises op de MFA-server. We kunnen daarom met een gerust hart Azure Multi-Factor Authentication gaan instellen en gebruiken. **Selecteer het pictogram dat bij uw scenario past**

<center>

[![MFA in de cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![MFA-server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
