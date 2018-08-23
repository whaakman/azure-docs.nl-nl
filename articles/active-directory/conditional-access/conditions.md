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
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 9feb6ef5b708813c2f73a70a930cabfd69dff114
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058124"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory? 

U kunt hoe gemachtigde gebruikers toegang tot uw cloud-apps beheren met behulp van [voorwaardelijke toegang van Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). In een beleid voor voorwaardelijke toegang, definieert u het antwoord ("Voer dit") met de reden voor het activeren van uw beleid ('als dit gebeurt'). 

![Reden en het antwoord](./media/conditions/10.png)


In de context van voorwaardelijke toegang, **wanneer dit gebeurt** heet een **voorwaarde**. **Klik hiervoor** heet een **toegangsbeheer**. De combinatie van de voorwaarden en de besturingselementen voor toegang vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Beleid voor voorwaardelijke toegang](./media/conditions/61.png)


Voorwaarden die u hebt nog niet hebt geconfigureerd in een beleid voor voorwaardelijke toegang niet toegepast. Sommige omstandigheden [verplichte](best-practices.md) beleid voor voorwaardelijke toegang toepassen op een omgeving. 

In dit artikel wordt een overzicht van de voorwaarden en hoe ze worden gebruikt in een beleid voor voorwaardelijke toegang. 

## <a name="users-and-groups"></a>Gebruikers en groepen

De voorwaarde voor gebruikers en groepen is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u een van beide select **alle gebruikers** of specifieke gebruikers en groepen selecteren.

![Gebruikers en groepen](./media/conditions/111.png)

Wanneer u selecteert **alle gebruikers**, het beleid wordt toegepast op alle gebruikers in de map, met inbegrip van gastgebruikers.

Wanneer u **gebruikers en groepen selecteren**, kunt u de volgende opties instellen:

* **Alle gastgebruikers** is gericht op een beleid voor B2B-gastgebruikers. Dit probleem komt overeen met een gebruikersaccount dat is de **userType** kenmerk ingesteld op **Gast**. U kunt deze instelling gebruiken wanneer een beleid worden toegepast moet zodra het account is gemaakt in een stroom uitnodiging in Azure AD.

* **Maprollen** is gericht op een beleid op basis van de roltoewijzing van de gebruiker. Deze voorwaarde biedt ondersteuning voor directory-rollen, zoals **hoofdbeheerder** of **wachtwoordbeheerder**.

* **Gebruikers en groepen** is gericht op specifieke sets van gebruikers. U kunt bijvoorbeeld een groep met alle leden van de HR-afdeling een HR-app is geselecteerd als de cloud-app selecteren. Een groep kan elk type groep in Azure AD, met inbegrip van dynamische of toegewezen beveiligings- en -groepen zijn.

U kunt ook specifieke gebruikers of groepen uitsluiten van een beleid. Een gebruikelijk is de service-accounts als uw beleid meervoudige verificatie (MFA dwingt). 

Die gericht zijn op specifieke sets van gebruikers is handig voor de implementatie van een nieuw beleid. In een nieuw beleid, moet u alleen een eerste set met gebruikers voor het valideren van het gedrag van het beleid voor het doel. 



## <a name="cloud-apps"></a>Cloud-apps 

Een cloud-app is een website of de service. Websites die zijn beveiligd door de Azure AD-toepassingsproxy zijn ook cloud-apps. Zie voor een gedetailleerde beschrijving van de ondersteunde cloud-apps, [cloud-apps toewijzingen](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#cloud-apps-assignments). 

De **cloud-apps** voorwaarde is verplicht in een beleid voor voorwaardelijke toegang. In het beleid, kunt u een van beide select **alle cloud-apps** of Selecteer specifieke apps.

![Cloud-apps opnemen](./media/conditions/03.png)

Selecteer:

- **Alle cloud-apps** basislijn beleid toe te passen op de hele organisatie. Gebruik deze selectie voor beleidsregels die meervoudige verificatie vereisen wanneer het aanmeldingsrisico wordt gedetecteerd voor alle cloud-Apps. Een beleid toegepast op **alle cloud-apps** geldt voor toegang tot alle websites en services. Deze instelling is niet beperkt tot de cloud-apps die worden weergegeven op de **apps selecteren** lijst. 

- Afzonderlijke cloud-apps voor specifieke services door het beleid doel. Bijvoorbeeld, u kunt vereisen dat gebruikers hebben een [compatibel apparaat](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) voor toegang tot SharePoint Online. Dit beleid wordt ook toegepast op andere services als ze toegang hebben tot SharePoint-inhoud. Een voorbeeld is Microsoft Teams. 

U kunt specifieke apps uitsluiten van een beleid. Deze apps zijn echter nog steeds afhankelijk van het beleid toegepast op de services die ze gebruiken. 



## <a name="sign-in-risk"></a>Aanmeldingsrisico

Een aanmeldingsrisico is een indicator van de kans dat een poging tot aanmelden is niet door de rechtmatige eigenaar van een gebruikersaccount gemaakt (hoog, Gemiddeld of laag). Azure AD berekent dat het niveau van aanmeldingsrisico tijdens een gebruiker zich sign-in. U kunt het niveau van de berekende aanmeldingsrisico gebruiken als voorwaarde in een beleid voor voorwaardelijke toegang.

![Aanmelden risiconiveaus](./media/conditions/22.png)

Voor het gebruik van dit probleem, moet u beschikken over [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable) ingeschakeld.
 
Algemene scenario's voor deze voorwaarde zijn sets beleidsregels met de volgende goed: 

- Blokkeren dat gebruikers met een hoog risico. Deze beveiliging wordt voorkomen dat gebruikers mogelijk niet-goedaardige toegang tot uw cloudapps. 
- Meervoudige verificatie vereisen voor gebruikers met een gemiddeld risico aanmelden. Door het afdwingen van multi-factor authentication, kunt u extra vertrouwen te geven dat de aanmelding wordt uitgevoerd door de rechtmatige eigenaar van een account opgeven.

Zie [Riskante aanmeldingen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) voor meer informatie.  

## <a name="device-platforms"></a>Apparaatplatformen

Het apparaatplatform wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. Azure AD identificeert het platform met behulp van informatie geleverd door het apparaat, zoals de gebruikersagent. Deze informatie is niet geverifieerd. Het is raadzaam dat alle platforms een beleid toegepast hebben. Het beleid moet u de toegang blokkeren, moet voldoen aan beleid met Microsoft Intune-beleid of vereisen dat het apparaat worden toegevoegd aan een domein. De standaardwaarde is een beleid toepassen op alle platformen voor apparaten. 


![Apparaatplatforms configureren](./media/conditions/24.png)

Zie voor een lijst van de ondersteunde apparaatplatformen, [apparaat platform voorwaarde](technical-reference.md#device-platform-condition).


Een algemene use-case voor deze situatie doet zich een beleid dat beperkt de toegang tot uw cloud-apps naar [beheerde apparaten](require-managed-devices.md). Zie voor meer scenario's met inbegrip van de voorwaarde van apparaat-platform, [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](app-based-conditional-access.md).



## <a name="device-state"></a>Apparaatstatus

De apparaatstatusvoorwaarde sluit hybride die Azure AD gekoppelde apparaten en apparaten die zijn gemarkeerd als compatibel van beleid voor voorwaardelijke toegang. Deze voorwaarde is handig wanneer een beleid van toepassing alleen op een niet-beheerd apparaat voor de sessiebeveiliging van aanvullende. Alleen afdwingen bijvoorbeeld de Microsoft Cloud App Security-sessiebeheer wanneer een apparaat niet-beheerde is. 


![Status van het apparaat configureren](./media/conditions/112.png)

Als u wilt dat de toegang voor niet-beheerde apparaten geblokkeerd, implementeren [apparaat gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).


## <a name="locations"></a>Locaties

Met behulp van locaties, kunt u voorwaarden op basis van waar de geprobeerd om een verbinding is te definiëren. 

![Locaties configureren](./media/conditions/25.png)

Algemene scenario's voor deze voorwaarde zijn beleid met de volgende goed:

- Meervoudige verificatie vereisen voor gebruikers met toegang tot een service als ze uit het bedrijfsnetwerk bevinden.  

- Toegang blokkeren voor gebruikers met toegang tot een service van specifieke landen of regio's. 

Zie voor meer informatie, [wat is er met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>Client-apps

Met behulp van de voorwaarde van client-apps, kunt u een beleid toepassen op verschillende typen toepassingen. Voorbeelden zijn websites, services, mobiele apps en bureaublad-toepassingen. 



Een toepassing wordt als volgt ingedeeld:

- Een website of de service als deze gebruikmaakt van web-SSO-protocollen, SAML, WS-Federation en OpenID Connect voor een vertrouwelijke client.

- Een mobiele app of een bureaubladtoepassing als de mobiele app OpenID Connect wordt gebruikt voor een native client.

Zie voor een lijst van de client-apps die u in uw beleid voor voorwaardelijke toegang gebruiken kunt, [Client-apps voorwaarde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#client-apps-condition) in de technische documentatie voor Azure Active Directory voor voorwaardelijke toegang.

Algemene scenario's voor deze voorwaarde zijn beleid met de volgende goed: 

- Vereist een [compatibel apparaat](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) voor mobiele en desktoptoepassingen die grote hoeveelheden gegevens om het apparaat downloadt. Op hetzelfde moment, kunt u browsertoegang vanaf elk apparaat.

- Blokkeer de toegang van web-apps, maar zodat toegang vanaf mobiele en bureaubladtoepassingen.

U kunt dit probleem op het web-SSO en moderne-verificatieprotocollen toepassen. U kunt deze ook toepassen op e-mailtoepassingen die gebruikmaken van Microsoft Exchange ActiveSync. Voorbeelden zijn de systeemeigen e-mailapps op de meeste smartphones. 

U kunt de client-apps-voorwaarde alleen selecteren als Microsoft Office 365 Exchange Online is de enige cloud-app die u hebt geselecteerd.

![Cloud-apps](./media/conditions/32.png)

Selecteren **Exchange ActiveSync** als een client apps voorwaarde wordt alleen ondersteund als er geen andere bepalingen die zijn geconfigureerd in een beleid. U kunt echter het bereik van dit probleem om toe te passen op ondersteunde platformen verfijnen.

 
![Het beleid toepassen op ondersteunde platformen](./media/conditions/33.png)

Deze voorwaarde toepassen op ondersteunde platformen is gelijk aan alle apparaatplatforms in een [apparaat platform voorwaarde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).

![Apparaatplatforms configureren](./media/conditions/34.png)


 Raadpleeg voor meer informatie de volgende artikelen:

- [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
 
- [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). 


### <a name="legacy-authentication"></a>Verouderde verificatie  

Voorwaardelijke toegang is nu van toepassing op oudere Microsoft Office-clients die geen ondersteuning voor moderne verificatie bieden. Dit geldt ook voor clients die gebruikmaken van e-mailprotocollen zoals POP, IMAP en SMTP. Met behulp van verouderde verificatie, kunt u beleid voor zoals **blokkeert de toegang van andere clients**.


![Client-apps configureren](./media/conditions/160.png)  


#### <a name="known-issues"></a>Bekende problemen

- Configureren van een beleid voor **andere clients** Hiermee blokkeert u de hele organisatie van bepaalde clients, zoals SPConnect. Dit blok treedt op omdat oudere clients worden geverifieerd op onverwachte manieren. Het probleem geldt niet voor belangrijke Office-toepassingen, zoals de oudere Office-clients. 

- Duurt maximaal 24 uur voor het beleid van kracht. 


#### <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V:** deze verificatie blokkeert Microsoft Exchange-webservices?

Dat hangt ervan af op het verificatieprotocol die gebruikmaakt van Exchange Web Services. Als de Exchange Web Services-toepassing maakt gebruik van moderne verificatie, wordt gedekt door de **mobiele apps en bureaubladclients** client-app. Basisverificatie wordt gedekt door de **andere clients** client-app.


**V:** welke besturingselementen kan ik gebruiken voor **andere clients**?

Elk besturingselement kan worden geconfigureerd voor **andere clients**. De eindgebruikerservaring zijn echter in alle gevallen de toegang is geblokkeerd. **Andere clients** bieden geen ondersteuning voor besturingselementen zoals MFA en compatibel apparaat lid van domein. 
 
**V:** welke voorwaarden kan ik gebruiken voor **andere clients**?

Een voorwaarde kan worden geconfigureerd voor **andere clients**.

**V:** Exchange ActiveSync biedt ondersteuning voor alle voorwaarden en besturingselementen?

Nee. De volgende lijst bevat een overzicht van ondersteuning voor Exchange ActiveSync: 

- Exchange ActiveSync ondersteunt alleen gebruikers en groepen. Het biedt geen ondersteuning voor gasten of rollen. Als een voorwaarde gast- of -rol is geconfigureerd, worden alle gebruikers worden geblokkeerd. Exchange ActiveSync worden alle gebruikers geblokkeerd omdat deze kan niet worden vastgesteld als het beleid van toepassing op de gebruiker of niet.

- Exchange ActiveSync werkt alleen met Microsoft Exchange Online als de cloud-app. 

- Exchange ActiveSync biedt geen ondersteuning voor een voorwaarde met uitzondering van de client-app zelf. 

- Exchange ActiveSync kan worden geconfigureerd met een besturingselement. Alle besturingselementen met uitzondering van het nalevingsbeleid voor apparaten leidt tot een blok.

**V:** zijn het beleid van toepassing op alle client-apps standaard voortaan?

Nee. Er is geen wijziging in het standaardgedrag voor het beleid. Het beleid blijven toepassen op de browser en mobiele toepassingen en bureaublad-clients standaard.



## <a name="next-steps"></a>Volgende stappen

- Als u wilt weten hoe u een beleid voor voorwaardelijke toegang configureren, Zie [Quick Start: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

- Voor het configureren van beleid voor voorwaardelijke toegang voor uw omgeving, Zie de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md). 

