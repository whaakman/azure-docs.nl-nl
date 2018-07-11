---
title: Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory? | Microsoft Docs
description: Meer informatie over hoe voorwaarden worden gebruikt in Azure Active Directory voor voorwaardelijke toegang voor het activeren van een beleid.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 42792170593dbd94d0eae9b408c70f326891508a
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "36232380"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory? 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt bepalen hoe gemachtigde gebruikers toegang tot uw cloud-apps. In een beleid voor voorwaardelijke toegang definieert u het antwoord ('Doe dit') met de reden voor het activeren van uw beleid ('als dit gebeurt'). 

![Besturingselement](./media/active-directory-conditional-access-conditions/10.png)


In de context van voorwaardelijke toegang:

- "**Wanneer dit gebeurt**' heet **voorwaarden**. 
- "**Voert u deze**' heet **besturingselementen voor toegang**.

De combinatie van uw voorwaarden met de besturingselementen voor toegang vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Besturingselement](./media/active-directory-conditional-access-conditions/61.png)


Voorwaarden die u niet hebt geconfigureerd in een beleid voor voorwaardelijke toegang niet toegepast. Sommige omstandigheden [verplichte](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) beleid voor voorwaardelijke toegang toepassen op een omgeving. 

In dit artikel biedt een overzicht van de voorwaarden en hoe ze worden gebruikt in een beleid voor voorwaardelijke toegang. 

## <a name="users-and-groups"></a>Gebruikers en groepen

De voorwaarde voor gebruikers en groepen is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u een van beide select **alle gebruikers** of specifieke gebruikers en groepen selecteren.

![Besturingselement](./media/active-directory-conditional-access-conditions/111.png)

Wanneer u selecteert:

- **Alle gebruikers**, het beleid wordt toegepast op alle gebruikers in de map. Dit omvat gastgebruikers.

- **Gebruikers en groepen selecteren**, kunt u de volgende opties instellen:

    - **Alle gastgebruikers** -kunt u een beleid voor B2B-gastgebruikers. Dit probleem komt overeen met een gebruikersaccount met de *userType* kenmerk ingesteld op *Gast*. U kunt deze instelling gebruiken in gevallen waarin een beleid worden toegepast moet zodra het account is gemaakt in een stroom uitnodiging In Azure AD.

    - **Maprollen** -kunt u een beleid op basis van de roltoewijzing van de gebruiker. Deze voorwaarde ondersteunt directory-rollen zoals *hoofdbeheerder* of *wachtwoordbeheerder*.

    - **Gebruikers en groepen** -kunt u richten op specifieke sets van gebruikers. U kunt bijvoorbeeld een groep met alle leden van de HR-afdeling, wanneer u een HR-app hebt geselecteerd als cloud-app selecteren.

Een groep, worden deze elk type groep in Azure AD, met inbegrip van dynamische of toegewezen beveiligings- en -groepen

U kunt ook specifieke gebruikers of groepen uitsluiten van een beleid. Een gebruikelijk zijn service-accounts als uw beleid zorgt ervoor dat multi-factor authentication (MFA). 

Die gericht zijn op specifieke sets van gebruikers is handig voor de implementatie van een nieuw beleid. In een nieuw beleid, moet u alleen een eerste set met gebruikers voor het valideren van het gedrag van het beleid voor het doel. 



## <a name="cloud-apps"></a>Cloud-apps 

Een cloud-app is een web-sites of de service. Dit omvat de websites die zijn beveiligd door de toepassingsproxy van Azure. Zie voor een gedetailleerde beschrijving van de ondersteunde cloud-apps, [toewijzing van apps in de cloud](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

De voorwaarde van cloud-apps is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u een van beide select **alle cloud-apps** of Selecteer specifieke apps.

![Besturingselement](./media/active-directory-conditional-access-conditions/03.png)

U kunt selecteren:

- **Alle cloud-apps** basislijn beleid moet worden toegepast op de hele organisatie. Een veelvoorkomende use-case voor deze selectie is een beleid dat multi-factor Authentication-verificatie is vereist wanneer aanmeldingsrisico wordt gedetecteerd voor alle cloud-Apps. Een beleid toegepast op **alle cloud-apps** geldt voor toegang tot alle web site en -services. Deze instelling is niet beperkt tot de cloud-apps die worden weergegeven op de **Selecteer Cloud-apps** lijst.

- Afzonderlijke cloud-apps voor specifieke services door het beleid doel. Bijvoorbeeld, u kunt vereisen dat gebruikers hebben een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor toegang tot SharePoint Online. Dit beleid wordt ook toegepast op andere services wanneer ze toegang SharePoint-inhoud tot, bijvoorbeeld Microsoft Teams. 

U kunt ook specifieke apps uitsluiten van een beleid; Deze apps zijn echter nog steeds afhankelijk van het beleid toegepast op services die ze gebruiken. 



## <a name="sign-in-risk"></a>Aanmeldingsrisico

Een aanmeldingsrisico is een indicator van de kans (hoog, Gemiddeld of laag) dat een poging tot aanmelden niet is uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount. Azure AD wordt het niveau van aanmeldingsrisico berekend tijdens de aanmelding van een gebruiker. U kunt het niveau van de berekende aanmeldingsrisico gebruiken als voorwaarde in een beleid voor voorwaardelijke toegang. 

![Voorwaarden](./media/active-directory-conditional-access-conditions/22.png)

Voor het gebruik van dit probleem, moet u beschikken over [Azure Active Directory Identity Protection](active-directory-identityprotection.md) ingeschakeld.
 
Algemene scenario's voor deze voorwaarde is voor apps die:

- Blokkeren dat gebruikers met een hoog risico's om te voorkomen dat gebruikers mogelijk niet-goedaardige toegang tot uw cloudapps. 
- Meervoudige verificatie vereisen voor gebruikers met een gemiddeld risico aanmelden. Door het afdwingen van multi-factor authentication, kunt u extra vertrouwen dat de aanmelding wordt uitgevoerd door de rechtmatige eigenaar van een account opgeven.

Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie.  

## <a name="device-platforms"></a>Apparaatplatformen

Het apparaatplatform wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. Azure AD identificeert het platform met behulp van informatie geleverd door het apparaat, zoals de gebruikersagent. Omdat deze informatie niet geverifieerd is, is het raadzaam dat alle platforms een beleid toegepast hebben, blokkeert de toegang, naleving van beleid van Intune vereisen of dat het apparaat worden toegevoegd aan een domein. De standaardwaarde is beleid toepassen op alle platformen voor apparaten. 


![Voorwaarden](./media/active-directory-conditional-access-conditions/24.png)

Zie voor een volledige lijst van de ondersteunde apparaatplatformen [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Een algemene use-case voor deze situatie doet zich een beleid dat beperkt de toegang tot uw cloud-apps naar [beheerde apparaten](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Zie voor meer scenario's met inbegrip van de voorwaarde van apparaat-platform, [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Apparaatstatus

De apparaatstatusvoorwaarde kan toegevoegd aan Hybrid Azure AD en apparaten gemarkeerd als compatibel moeten worden uitgesloten van een beleid voor voorwaardelijke toegang. Dit is handig wanneer een beleid alleen van toepassing op niet-beheerd apparaat voor de sessiebeveiliging van aanvullende. Alleen afdwingen bijvoorbeeld de Microsoft Cloud App Security-sessiebeheer wanneer een apparaat niet-beheerde is. 


![Voorwaarden](./media/active-directory-conditional-access-conditions/112.png)

Als u wilt dat de toegang voor niet-beheerde apparaten geblokkeerd, moet u implementeren [apparaat gebaseerde voorwaardelijke toegang](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Locaties

Met locaties hebt u de optie voor het definiëren van voorwaarden die zijn gebaseerd op waar een verbindingspoging uit is gestart. 
     
![Voorwaarden](./media/active-directory-conditional-access-conditions/25.png)

Algemene scenario's voor deze voorwaarde is voor apps die:

- Meervoudige verificatie vereisen voor gebruikers die toegang krijgen tot een service wanneer ze buiten het bedrijfsnetwerk bevinden vallen.  

- Toegang blokkeren voor gebruikers met toegang tot een service van specifieke landen of regio's. 

Zie voor meer informatie, [wat is er met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](active-directory-conditional-access-locations.md)


## <a name="client-apps"></a>Client-apps

De voorwaarde van client-apps kunt u een beleid toepassen op verschillende typen toepassingen, zoals:

- Websites en services
- Mobiele apps en bureaublad-toepassingen. 



Een toepassing wordt ingedeeld als:

- Een website of de service als deze gebruikmaakt van de web-SSO-protocollen, SAML, WS-Federation of OpenID Connect voor een vertrouwelijke client.

- A een mobiele app of een bureaubladtoepassing als de mobiele app OpenID Connect wordt gebruikt voor een native client.

Zie voor een volledige lijst van de client-apps die u in uw beleid voor voorwaardelijke toegang gebruiken kunt, [Client-apps voorwaarde](active-directory-conditional-access-technical-reference.md#client-apps-condition) in de technische documentatie voor Azure Active Directory voor voorwaardelijke toegang.

Algemene scenario's voor deze voorwaarde is voor apps die:

- Vereist een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor mobiele en desktoptoepassingen die grote hoeveelheden gegevens om het apparaat, downloadt terwijl browsertoegang vanaf elk apparaat.

- Blokkeer de toegang van web-apps, maar toegang van mobiele en bureaubladtoepassingen toestaan.

Naast het gebruik van web-SSO en moderne-verificatieprotocollen, kunt u deze voorwaarde toepassen op e-mailtoepassingen die gebruikmaken van Exchange ActiveSync, zoals de systeemeigen e-mailapps op de meeste smartphones. Client-apps met behulp van verouderde protocollen moeten op dit moment worden beveiligd met AD FS.

U kunt dit probleem alleen selecteren als **Office 365 Exchange Online** is de enige cloud-app die u hebt geselecteerd.

![Cloud-apps](./media/active-directory-conditional-access-conditions/32.png)

Selecteren **Exchange ActiveSync** als client-apps voorwaarde wordt alleen ondersteund als u geen andere voorwaarden in een beleid is geconfigureerd. U kunt echter het bereik van dit probleem is alleen van toepassing op ondersteunde platformen verfijnen.

 
![Ondersteunde platforms](./media/active-directory-conditional-access-conditions/33.png)

Deze voorwaarde toepassen op ondersteunde platformen is het equivalent naar alle apparaatplatforms in een [apparaat platform voorwaarde](active-directory-conditional-access-conditions.md#device-platforms).

![Ondersteunde platforms](./media/active-directory-conditional-access-conditions/34.png)


 Zie voor meer informatie:

- [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Verouderde verificatie  

Voorwaardelijke toegang is nu van toepassing op oudere Office-clients die geen ondersteuning voor moderne verificatie, evenals de clients die gebruikmaken van e-mailprotocollen zoals POP, IMAP, SMTP, enz. Hiermee kunt u het configureren van beleid, zoals **blokkeert de toegang van andere clients**.


![Verouderde verificatie](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Bekende problemen

- Configureren van een beleid voor **andere clients** Hiermee blokkeert u de hele organisatie van bepaalde clients, zoals SPConnect. Dit komt door deze oudere clients verifiëren op onverwachte manieren. Dit probleem is niet van toepassing op de primaire Office-toepassingen, zoals de oudere Office-clients. 

- Duurt maximaal 24 uur voor het beleid van kracht. 


#### <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wordt dit blokkeert Exchange Web Services (EWS)?**

Dat hangt ervan af op het verificatieprotocol die EWS wordt gebruikt. Als de toepassing EWS moderne verificatie gebruikt, wordt deze door de client-app 'mobiele apps en bureaubladclients' worden behandeld. Als de toepassing EWS basisverificatie wordt gebruikt, wordt deze door de "Andere clients" client-app worden behandeld.


**Welke besturingselementen kan ik gebruiken voor andere clients**

Elk besturingselement dat kan worden geconfigureerd voor 'Andere clients'. De ervaring van eindgebruikers zich echter toegang blokkeren voor alle aanvragen. "Andere clients" bieden geen ondersteuning voor besturingselementen zoals MFA, compatibel apparaat, lid van domein, enzovoort. 
 
**Welke voorwaarden kan ik gebruiken voor andere clients?**

Alle voorwaarden kunnen worden geconfigureerd voor 'Andere clients'.

**Ondersteunt Exchange ActiveSync alle voorwaarden en besturingselementen?**

Nee. Hier volgt een overzicht van ondersteuning voor Exchange ActiveSync (EAS):

- EAS biedt alleen ondersteuning voor gebruikers en groepen. Het biedt geen ondersteuning voor gasten, rollen. Als de voorwaarde van de Gast/host-rol is geconfigureerd, alle gebruikers geblokkeerd omdat kan niet worden bepaald als het beleid van toepassing op de gebruiker of niet.

- EAS werkt alleen met Exchange als de cloud-app. 

- EAS biedt geen ondersteuning voor een voorwaarde met uitzondering van de clientapp zelf.

- EAS kan worden geconfigureerd met een besturingselement (Alles behalve apparaatcompatibiliteit zal leiden tot blokkeren).

**Het beleid van toepassing op alle client-apps standaard voortaan?**

Nee. Er is geen wijziging in het standaardgedrag voor het beleid. Het beleid blijven toepassen op de browser en mobiele toepassingen/bureaublad-clients standaard.



## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 

