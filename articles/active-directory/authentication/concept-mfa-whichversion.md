---
title: Azure MFA-Server of -Service, On-premises of in de cloud? - Azure Active Directory
description: Als een Azure AD-beheerder, die ik nodig om te begrijpen welke versie van MFA ik moet implementeren?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a099fa8d223643e5b339d35c0ff5cf7a5589049c
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372242"
---
# <a name="which-version-of-azure-mfa-is-right-for-my-organization"></a>Welke versie van Azure MFA is geschikt voor mijn organisatie?

Voordat u waar en hoe bepalen kunt bij het implementeren van multi-factor Authentication (MFA), moet u drie algemene vragen beantwoorden.

* [Wat wil ik beveiligen?](#what-am-i-trying-to-secure)
* [Waar bevinden de gebruikers zich?](#where-are-the-users-located)
* [Welke functies heb ik nodig?](#what-features-do-i-need)

Elk van de volgende secties vindt u informatie op basis waarvan u de voorgaande vragen te beantwoorden.

## <a name="what-am-i-trying-to-secure"></a>Wat wil ik beveiligen?

Als u wilt bepalen welke de juiste verificatie in twee stappen verificatie-oplossing, eerst u moet de vraag worden beantwoord wat u wilt beveiligen met een extra authenticatiefactor. Is het een toepassing die deel uitmaakt van Azure? Of een systeem met externe toegang? Door te bepalen wat u wilt beveiligen, kunt u de vraag waar multi-factor Authentication moet worden ingeschakeld.

| Wat wilt u beveiligen | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Eigen Microsoft-apps |● |● |
| SaaS-apps in de app-galerie |● |  |
| Webtoepassingen die zijn gepubliceerd via de toepassingsproxy van Azure AD |● |  |
| IIS-toepassingen die niet zijn gepubliceerd via toepassingsproxy van Azure AD | |● |
| Externe toegang zoals VPN, RDG met behulp van de NPS-extensie of een bestaande NPS-Server | ● | ● |

## <a name="where-are-the-users-located"></a>Waar bevinden de gebruikers zich?

Vervolgens kunt u bepalen waar gebruikers van uw organisatie zich bevindt helpt om te bepalen wat de juiste oplossing moet worden gebruikt, in de cloud of on-premises via de MFA-Server.

| Locatie van gebruikers | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD en on-premises AD dat gebruikmaakt van federatie met AD FS |● |● |
| Azure AD en on-premises AD met behulp van Azure AD Connect - geen synchronisatie van wachtwoordhashes of Pass through-verificatie |● |● |
| Azure AD en on-premises AD dat gebruikmaakt van Azure AD Connect - met wachtwoord-hash-synchronisatie of Pass through-verificatie |● | |
| On-premises Active Directory | |● |

## <a name="what-features-do-i-need"></a>Welke functies heb ik nodig?

De volgende tabel bevat een vergelijking van de functies van Multi-Factor Authentication die beschikbaar zijn in de cloud en de functies die beschikbaar zijn op de Multi-Factor Authentication-server.

| Functie | MFA in de cloud | MFA-server |
| --- |:---:|:---:|
| Meldingen op mobiele app als een tweede factor | ● | ● |
| Een verificatiecode op mobiele app als een tweede factor | ● | ● |
| Telefoonoproep als tweede factor | ● | ● |
| Sms in één richting als tweede factor | ● | ● |
| Hardwaretokens als tweede factor | ● (openbare preview) | ● |
| App-wachtwoorden voor Office 365-clients die geen ondersteuning voor MFA bieden | ● | |
| Beheerdercontrole over verificatiemethoden | ● | ● |
| Pincodemodus | | ● |
| Fraudewaarschuwing | ● | ● |
| MFA-rapporten | ● | ● |
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

[![Keuze voor Azure MFA in de cloud](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [ ![Keuze  voor Azure MFA-Server on-premises  ](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
