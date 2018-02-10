---
title: Voorwaarden in voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Meer informatie over hoe toewijzingen van voorwaardelijke toegang van Azure Active Directory worden gebruikt voor het activeren van een beleid.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 22cc43a861e2a9d1d0c508da362a5f0b48d0c268
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Voorwaarden in Azure Active Directory voorwaardelijke toegang 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt beheren hoe gemachtigde gebruikers toegang tot uw cloud-apps. In een beleid voor voorwaardelijke toegang definieert u het antwoord ('dit doen") met de reden voor activering van uw beleid ('wanneer dit gebeurt'). 

![Besturingselement](./media/active-directory-conditional-access-conditions/10.png)


In de context van voorwaardelijke toegang:

- "**Wanneer dit gebeurt**' heet **voorwaarden**. 
- "**Voert u deze**' heet **toegangscontroles**.

De combinatie van uw voorwaarden met uw toegangsbeheer vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Besturingselement](./media/active-directory-conditional-access-conditions/61.png)

In dit artikel biedt een overzicht van de voorwaarden en hoe ze worden gebruikt in een beleid voor voorwaardelijke toegang. 


## <a name="users-and-groups"></a>Gebruikers en groepen

De voorwaarde voor gebruikers en groepen is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u select **alle gebruikers** of specifieke gebruikers en groepen selecteren.

![Besturingselement](./media/active-directory-conditional-access-conditions/02.png)

Wanneer u selecteert:

- **Alle gebruikers**, het beleid wordt toegepast op alle gebruikers met in de map. Dit omvat gastgebruikers.

- **Selecteer gebruikers en groepen**, kunt u bepaalde groepen gebruikers, bijvoorbeeld alle leden van de afdeling Personeelszaken richten wanneer ze zich in een HR-app. 

- Een groep kan het zijn een type groep in Azure AD, met inbegrip van dynamische of toegewezen beveiligings- en -groepen.

U kunt ook bepaalde gebruikers of groepen uitsluiten van een beleid. Een veelvoorkomend gebruik geval zijn serviceaccounts als uw beleid zorgt ervoor dat multi-factor authentication-server. 

Die gericht is op bepaalde groepen gebruikers is nuttig voor de implementatie van een nieuw beleid. In een nieuw beleid, moet u het doel een eerste reeks gebruikers voor het valideren van het gedrag van het beleid. 



## <a name="cloud-apps"></a>Cloud-apps 

Een cloud-app is een websites of service. Dit omvat beveiligd door de toepassingsproxy van Azure websites. Zie voor een gedetailleerde beschrijving van de ondersteunde cloud-apps, [cloud-apps toewijzing](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

De cloud-apps-voorwaarde is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u select **alle cloud-apps** of Selecteer specifieke apps.

![Besturingselement](./media/active-directory-conditional-access-conditions/03.png)

U kunt selecteren:

- **Alle cloud-apps** basislijn-beleid moet worden toegepast op de hele organisatie. Een algemene gebruiksvoorbeeld voor deze selectie is een beleid dat multi-factor authentication-server is vereist bij het aanmelden risico wordt gedetecteerd voor alle cloud-Apps.

- Afzonderlijke cloud-apps tot specifieke services door het beleid voor doel. U kunt bijvoorbeeld vereisen dat gebruikers hebben een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor toegang tot SharePoint Online. Dit beleid geldt ook voor andere services wanneer ze toegang SharePoint-inhoud, bijvoorbeeld Microsoft-Teams tot. 

U kunt ook specifieke apps uitsluiten van een beleid; Deze apps zijn echter nog steeds onderworpen aan het beleid toegepast op ze toegang tot services. 



## <a name="sign-in-risk"></a>Aanmeldingsrisico

Een risico aanmelden is een indicator voor de kans (hoog, Gemiddeld of laag) op een aanmeldingspoging is niet uitgevoerd door de legitieme eigenaar van een gebruikersaccount. Azure AD berekent het risiconiveau aanmelden tijdens het aanmelden van een gebruiker. U kunt het risiconiveau voor berekende aanmelden als voorwaarde in een beleid voor voorwaardelijke toegang gebruiken. 

![Voorwaarden](./media/active-directory-conditional-access-conditions/22.png)

Voor het gebruik van deze voorwaarde hoeft te hebben [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) ingeschakeld.
 
Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Een gebruikers met een hoge aanmelden risico wilt voorkomen dat gebruikers mogelijk niet-rechtmatige toegang tot uw cloud-apps blokkeren. 
- Multi-factor authentication voor de gebruikers met een gemiddeld risico aanmelden vereist. Door het afdwingen van multi-factor authentication-server, kunt u meer vertrouwen dat de aanmeldingspagina wordt uitgevoerd door de legitieme eigenaar van een account opgeven.

Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie.  

## <a name="device-platforms"></a>Apparaatplatformen

Platform van het apparaat wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. Azure AD identificeert het platform met behulp van informatie die door het apparaat, zoals gebruikersagent. Omdat deze informatie niet geverifieerd is, is het raadzaam dat alle platforms een beleid is toegepast, door de toegang blokkeert, vereisen van naleving van de Intune-beleid of het vereisen van dat het apparaat zijn verbonden met het domein hebben. De standaardwaarde is beleid toepassen op alle platforms. 


![Voorwaarden](./media/active-directory-conditional-access-conditions/02.png)

Zie voor een volledige lijst met de ondersteunde apparaatplatforms [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Een algemeen gebruiksvoorbeeld voor deze voorwaarde is een beleid dat de toegang tot uw cloud-apps naar beperkt [vertrouwde apparaten](active-directory-conditional-access-policy-connected-applications.md#trusted-devices). Zie voor meer scenario's met inbegrip van de voorwaarde van de platform apparaat [voorwaardelijke toegang voor Azure Active Directory op basis van een app](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Locaties

Met locaties hebt u de optie voor het definiëren van de voorwaarden die zijn gebaseerd op waar een verbindingspoging van is gestart. 
     
![Voorwaarden](./media/active-directory-conditional-access-conditions/03.png)

Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Meervoudige verificatie vereisen voor gebruikers die toegang tot een service wanneer ze zich buiten het bedrijfsnetwerk.  

- Toegang blokkeren voor gebruikers die toegang tot een service van specifieke landen of regio's. 

Zie voor meer informatie [voorwaarden van de locatie van voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Client-apps

De voorwaarde van client-apps kunt u een beleid toepassen op verschillende typen toepassingen, zoals:

- Websites en services
- Mobiele apps en bureaubladtoepassingen. 

![Voorwaarden](./media/active-directory-conditional-access-conditions/04.png)

Een toepassing wordt ingedeeld als:

- Een website of de service als gebruikmaakt van de web-SSO-protocollen, SAML, WS-Fed of OpenID Connect voor een vertrouwelijk client.

- A een mobiele app of bureaubladtoepassing als de mobiele app OpenID Connect wordt gebruikt voor een native client.

Zie voor een volledige lijst van de ClientApps kunt u in uw beleid voor voorwaardelijke toegang, de [technische documentatie voor Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Vereist een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor mobiele en bureaublad-toepassingen die grote hoeveelheden gegevens op het apparaat downloaden en tegelijkertijd toegang tot de browser vanaf elk apparaat.

- De toegang tot webtoepassingen geblokkeerd, maar toegang toestaan via mobiele en bureaubladtoepassingen.

Naast het gebruik van web-SSO en moderne verificatieprotocollen, kunt u deze voorwaarde toepassen op e-toepassingen die gebruikmaken van Exchange ActiveSync, zoals de systeemeigen e-mailapps op de meeste smartphones. Op dit moment kunnen client-apps met behulp van verouderde protocollen moeten worden beveiligd met AD FS.

 Zie voor meer informatie:

- [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory op basis van een app voorwaardelijke toegang](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 

