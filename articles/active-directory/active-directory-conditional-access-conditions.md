---
title: Wat zijn de voorwaarden van voorwaardelijke toegang van Azure Active Directory? | Microsoft Docs
description: Meer informatie over hoe voorwaarden van voorwaardelijke toegang van Azure Active Directory worden gebruikt voor het activeren van een beleid.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
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
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232380"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Wat zijn de voorwaarden van voorwaardelijke toegang van Azure Active Directory? 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt beheren hoe gemachtigde gebruikers toegang tot uw cloud-apps. In een beleid voor voorwaardelijke toegang definieert u het antwoord ('dit doen") met de reden voor activering van uw beleid ('wanneer dit gebeurt'). 

![Besturingselement](./media/active-directory-conditional-access-conditions/10.png)


In de context van voorwaardelijke toegang:

- "**Wanneer dit gebeurt**' heet **voorwaarden**. 
- "**Voert u deze**' heet **toegangscontroles**.

De combinatie van uw voorwaarden met uw toegangsbeheer vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Besturingselement](./media/active-directory-conditional-access-conditions/61.png)


Voorwaarden die u niet hebt geconfigureerd in een beleid voor voorwaardelijke toegang worden niet toegepast. Sommige voorwaarden [verplichte](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) beleid voor voorwaardelijke toegang toepassen op een omgeving. 

In dit artikel biedt een overzicht van de voorwaarden en hoe ze worden gebruikt in een beleid voor voorwaardelijke toegang. 

## <a name="users-and-groups"></a>Gebruikers en groepen

De voorwaarde voor gebruikers en groepen is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u select **alle gebruikers** of specifieke gebruikers en groepen selecteren.

![Besturingselement](./media/active-directory-conditional-access-conditions/111.png)

Wanneer u selecteert:

- **Alle gebruikers**, het beleid wordt toegepast op alle gebruikers in de map. Dit omvat gastgebruikers.

- **Selecteer gebruikers en groepen**, kunt u de volgende opties instellen:

    - **Alle gastgebruikers** -kunt u een beleid voor gastgebruikers B2B als doel. Deze voorwaarde overeenkomt met een gebruikersaccount met de *userType* -kenmerk ingesteld op *Gast*. U kunt deze instelling gebruiken in gevallen waarin een beleid moet worden toegepast als het account is gemaakt in een stroom uitnodiging In Azure AD.

    - **Directory-functies** -kunt u een beleid op basis van een gebruiker roltoewijzing. Deze voorwaarde ondersteunt directory-functies, zoals *hoofdbeheerder* of *wachtwoordbeheerder*.

    - **Gebruikers en groepen** -kunt u aan bepaalde groepen gebruikers doel. Selecteer bijvoorbeeld een groep met alle leden van de afdeling Personeelszaken wanneer er een HR-app als een cloud-app geselecteerd.

Een groep kan het type van de groep worden in Azure AD, met inbegrip van dynamische of toegewezen beveiligings- en -groepen

U kunt ook bepaalde gebruikers of groepen uitsluiten van een beleid. Een veelvoorkomend gebruik geval zijn serviceaccounts als uw beleid zorgt ervoor dat multi-factor authentication (MFA). 

Die gericht is op bepaalde groepen gebruikers is nuttig voor de implementatie van een nieuw beleid. In een nieuw beleid, moet u het doel een eerste reeks gebruikers voor het valideren van het gedrag van het beleid. 



## <a name="cloud-apps"></a>Cloud-apps 

Een cloud-app is een websites of service. Dit omvat beveiligd door de toepassingsproxy van Azure websites. Zie voor een gedetailleerde beschrijving van de ondersteunde cloud-apps, [cloud-apps toewijzing](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

De cloud-apps-voorwaarde is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u select **alle cloud-apps** of Selecteer specifieke apps.

![Besturingselement](./media/active-directory-conditional-access-conditions/03.png)

U kunt selecteren:

- **Alle cloud-apps** basislijn-beleid moet worden toegepast op de hele organisatie. Een algemene gebruiksvoorbeeld voor deze selectie is een beleid dat multi-factor authentication-server is vereist bij het aanmelden risico wordt gedetecteerd voor alle cloud-Apps. Een beleid toegepast op **alle cloud-apps** geldt voor toegang tot alle website en services. Deze instelling is niet beperkt tot de cloud-apps die worden weergegeven op de **Selecteer Cloud-apps** lijst.

- Afzonderlijke cloud-apps tot specifieke services door het beleid voor doel. U kunt bijvoorbeeld vereisen dat gebruikers hebben een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor toegang tot SharePoint Online. Dit beleid geldt ook voor andere services wanneer ze toegang SharePoint-inhoud, bijvoorbeeld Microsoft-Teams tot. 

U kunt ook specifieke apps uitsluiten van een beleid; Deze apps zijn echter nog steeds onderworpen aan het beleid toegepast op ze toegang tot services. 



## <a name="sign-in-risk"></a>Aanmeldingsrisico

Een risico aanmelden is een indicator voor de kans (hoog, Gemiddeld of laag) op een aanmeldingspoging is niet uitgevoerd door de legitieme eigenaar van een gebruikersaccount. Azure AD berekent het risiconiveau aanmelden tijdens het aanmelden van een gebruiker. U kunt het risiconiveau voor berekende aanmelden als voorwaarde in een beleid voor voorwaardelijke toegang gebruiken. 

![Voorwaarden](./media/active-directory-conditional-access-conditions/22.png)

Voor het gebruik van deze voorwaarde hoeft te hebben [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) ingeschakeld.
 
Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Gebruikers met een risico hoge aanmelden om te voorkomen dat gebruikers mogelijk niet-rechtmatige toegang tot uw cloud-apps blokkeren. 
- Meervoudige verificatie vereisen voor gebruikers met een gemiddeld risico aanmelden. Door het afdwingen van multi-factor authentication-server, kunt u meer vertrouwen dat de aanmeldingspagina wordt uitgevoerd door de legitieme eigenaar van een account opgeven.

Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie.  

## <a name="device-platforms"></a>Apparaatplatformen

Platform van het apparaat wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. Azure AD identificeert het platform met behulp van informatie die door het apparaat, zoals gebruikersagent. Omdat deze informatie niet geverifieerd is, is het raadzaam dat alle platforms een beleid is toegepast, door de toegang blokkeert, vereisen van naleving van de Intune-beleid of het vereisen van dat het apparaat zijn verbonden met het domein hebben. De standaardwaarde is beleid toepassen op alle platforms. 


![Voorwaarden](./media/active-directory-conditional-access-conditions/24.png)

Zie voor een volledige lijst met de ondersteunde apparaatplatforms [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Een algemeen gebruiksvoorbeeld voor deze voorwaarde is een beleid dat de toegang tot uw cloud-apps naar beperkt [beheerde apparaten](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Zie voor meer scenario's met inbegrip van de voorwaarde van de platform apparaat [voorwaardelijke toegang voor Azure Active Directory op basis van een app](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Apparaatstatus

De voorwaarde van de status van apparaten kan die lid zijn van hybride Azure AD en apparaten gemarkeerd als compatibel moeten worden uitgesloten van een beleid voor voorwaardelijke toegang. Dit is handig wanneer een beleid moet alleen van toepassing op een onbeheerd apparaat om extra sessiebeveiliging te bieden. Bijvoorbeeld alleen afdwingen voor het beheer van de Microsoft Cloud App Security-sessies een apparaat is onbeheerd. 


![Voorwaarden](./media/active-directory-conditional-access-conditions/112.png)

Als u toegang blokkeren voor onbeheerde apparaten wilt, moet u implementeren [voorwaardelijke toegang op basis van apparaten](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Locaties

Met locaties hebt u de optie voor het definiëren van de voorwaarden die zijn gebaseerd op waar een verbindingspoging van is gestart. 
     
![Voorwaarden](./media/active-directory-conditional-access-conditions/25.png)

Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Meervoudige verificatie vereisen voor gebruikers die toegang tot een service wanneer ze zich buiten het bedrijfsnetwerk.  

- Toegang blokkeren voor gebruikers die toegang tot een service van specifieke landen of regio's. 

Zie voor meer informatie [wat is de locatie-voorwaarde in voorwaardelijke toegang van Azure Active Directory?](active-directory-conditional-access-locations.md)


## <a name="client-apps"></a>Client-apps

De voorwaarde van client-apps kunt u een beleid toepassen op verschillende typen toepassingen, zoals:

- Websites en services
- Mobiele apps en bureaubladtoepassingen. 



Een toepassing wordt ingedeeld als:

- Een website of de service als gebruikmaakt van de web-SSO-protocollen, SAML, WS-Fed of OpenID Connect voor een vertrouwelijk client.

- A een mobiele app of bureaubladtoepassing als de mobiele app OpenID Connect wordt gebruikt voor een native client.

Zie voor een volledige lijst van de ClientApps kunt u in uw beleid voor voorwaardelijke toegang [Client apps voorwaarde](active-directory-conditional-access-technical-reference.md#client-apps-condition) in de technische documentatie voor Azure Active Directory voorwaardelijke toegang.

Algemene gebruiksvoorbeelden voor deze voorwaarde zijn beleidsregels die:

- Vereist een [compatibel apparaat](active-directory-conditional-access-policy-connected-applications.md) voor mobiele en bureaublad-toepassingen die grote hoeveelheden gegevens op het apparaat downloaden en tegelijkertijd toegang tot de browser vanaf elk apparaat.

- De toegang tot webtoepassingen geblokkeerd, maar toegang toestaan via mobiele en bureaubladtoepassingen.

Naast het gebruik van web-SSO en moderne verificatieprotocollen, kunt u deze voorwaarde toepassen op e-toepassingen die gebruikmaken van Exchange ActiveSync, zoals de systeemeigen e-mailapps op de meeste smartphones. Op dit moment kunnen client-apps met behulp van verouderde protocollen moeten worden beveiligd met AD FS.

U kunt deze voorwaarde alleen selecteren als **Office 365 Exchange Online** is de enige cloud-app die u hebt geselecteerd.

![Cloud-apps](./media/active-directory-conditional-access-conditions/32.png)

Selecteren **Exchange ActiveSync** als client-apps voorwaarde wordt alleen ondersteund als er geen andere voorwaarden in een beleid dat is geconfigureerd. U kunt echter het bereik van dit probleem is alleen van toepassing op ondersteunde platforms afbakenen.

 
![Ondersteunde platforms](./media/active-directory-conditional-access-conditions/33.png)

Deze voorwaarde toepassen alleen op ondersteunde platforms komt overeen met alle apparaatplatforms in een [apparaat platform voorwaarde](active-directory-conditional-access-conditions.md#device-platforms).

![Ondersteunde platforms](./media/active-directory-conditional-access-conditions/34.png)


 Zie voor meer informatie:

- [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory op basis van een app voorwaardelijke toegang](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Verouderde verificatie  

Voorwaardelijke toegang is nu van toepassing op oudere Office-clients die moderne verificatie niet ondersteunen, evenals de clients die gebruikmaken van e-protocollen zoals POP-, IMAP-, SMTP-, enzovoort. Hiermee kunt u voor het configureren van beleid zoals **blokkeert de toegang van andere clients**.


![Verouderde verificatie](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Bekende problemen

- Een beleid configureert voor **andere clients** blokkeert de hele organisatie van bepaalde clients zoals SPConnect. Dit komt door deze oudere clients verifiëren op onverwachte manieren. Dit probleem is niet van toepassing op de primaire Office-toepassingen zoals de oudere Office-clients. 

- Het kan tot 24 uur voor het beleid van kracht duren. 


#### <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wordt dit Exchange Web Services (EWS) blokkeren?**

Dit is afhankelijk van het verificatieprotocol dat EWS wordt gebruikt. Als de toepassing EWS moderne verificatie gebruikt, zal het worden gedekt door de client-app 'mobiele apps en bureaublad-clients'. Als de toepassing EWS met behulp van basisverificatie, zal het worden gedekt door de client-app 'Andere clients'.


**Welke besturingselementen kan ik gebruiken voor andere clients**

Een besturingselement kan worden geconfigureerd voor 'Andere clients'. De eindgebruiker zich echter toegang blokkeren voor alle aanvragen. 'Andere clients' bieden geen ondersteuning voor besturingselementen zoals MFA compatibel apparaat, domein, enzovoort. 
 
**Welke voorwaarden kan ik gebruiken voor andere clients?**

Alle voorwaarden kunnen worden geconfigureerd voor 'Andere clients'.

**Ondersteunt Exchange ActiveSync alle voorwaarden en besturingselementen?**

Nee. Hier volgt een samenvatting van ondersteuning voor Exchange ActiveSync (EAS):

- EAS ondersteunt alleen gebruikers en groepen. Deze biedt geen ondersteuning voor gast, rollen. Als gast/rol voorwaarde is geconfigureerd, worden alle gebruikers worden geblokkeerd omdat als het beleid voor de gebruiker gelden moet kan niet worden bepaald.

- EAS werkt alleen als de cloud-app met Exchange. 

- EAS biedt geen ondersteuning voor elke voorwaarde met uitzondering van de clientapp zelf.

- EAS kan worden geconfigureerd met een besturingselement (Alles behalve apparaatcompatibiliteit zal leiden tot block).

**Zijn de beleidsregels van toepassing op alle client-apps standaard voortaan?**

Nee. Er is geen wijziging in het standaardgedrag voor het beleid. Het beleid blijven standaard toegepast op de browser en mobiele toepassingen/bureaublad-clients.



## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 

