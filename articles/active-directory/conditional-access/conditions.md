---
title: Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory? | Microsoft Docs
description: Meer informatie over hoe voorwaarden worden gebruikt in Azure Active Directory voor voorwaardelijke toegang voor het activeren van een beleid.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2018
ms.author: markvi
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 385f97b07f424d87c20561c8ab16e70be875c074
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196384"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory? 

U kunt bepalen hoe gebruikers toegang tot uw cloud-apps met behulp van [voorwaardelijke toegang van Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). In een beleid voor voorwaardelijke toegang, definieert u het antwoord ("Voer dit") met de reden voor het activeren van uw beleid ('als dit gebeurt'). 

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

* **Alle gastgebruikers** is gericht op een beleid voor B2B-gastgebruikers. Dit probleem komt overeen met een gebruikersaccount dat is de **userType** kenmerk ingesteld op **Gast**. Gebruik deze instelling als u een beleid moet worden toegepast zodra het account is gemaakt in een stroom uitnodiging in Azure AD.

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

- **Selecteer apps** tot specifieke doelservices in uw beleid. Bijvoorbeeld, u kunt vereisen dat gebruikers hebben een [compatibel apparaat](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) voor toegang tot SharePoint Online. Dit beleid wordt ook toegepast op andere services als ze toegang hebben tot SharePoint-inhoud. Een voorbeeld is Microsoft Teams. 

U kunt specifieke apps uitsluiten van een beleid. Deze apps zijn echter nog steeds afhankelijk van het beleid toegepast op de services die ze gebruiken. 



## <a name="sign-in-risk"></a>Aanmeldingsrisico

Een aanmeldingsrisico is een indicator van de kans (hoog, Gemiddeld of laag) die een aanmelding door de rechtmatige eigenaar van een gebruikersaccount is niet gemaakt. Azure AD berekent dat het niveau van aanmeldingsrisico tijdens een gebruiker zich sign-in. U kunt het niveau van de berekende aanmeldingsrisico gebruiken als voorwaarde in een beleid voor voorwaardelijke toegang.

![Aanmelden risiconiveaus](./media/conditions/22.png)

Voor het gebruik van dit probleem, moet u beschikken over [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable) ingeschakeld.
 
Algemene scenario's voor deze voorwaarde zijn sets beleidsregels met de volgende goed: 

- Blokkeren dat gebruikers met een hoog risico. Deze beveiliging wordt voorkomen dat gebruikers mogelijk niet-goedaardige toegang tot uw cloudapps. 
- Meervoudige verificatie vereisen voor gebruikers met een gemiddeld risico aanmelden. Door het afdwingen van multi-factor authentication, kunt u extra vertrouwen te geven dat de aanmelding wordt uitgevoerd door de rechtmatige eigenaar van een account opgeven.

Zie voor meer informatie, [toegang blokkeren als er een risico voor de sessie wordt gedetecteerd](app-sign-in-risk.md).  

## <a name="device-platforms"></a>Apparaatplatformen

Het apparaatplatform wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. Azure AD identificeert het platform met behulp van informatie geleverd door het apparaat, zoals de gebruikersagent. Deze informatie is niet geverifieerd. Het is raadzaam dat alle platforms een beleid toegepast hebben. Het beleid moet u de toegang blokkeren, moet voldoen aan beleid met Microsoft Intune-beleid of vereisen dat het apparaat worden toegevoegd aan een domein. De standaardwaarde is een beleid toepassen op alle platformen voor apparaten. 


![Apparaatplatforms configureren](./media/conditions/24.png)

Zie voor een lijst van de ondersteunde apparaatplatformen, [apparaat platform voorwaarde](technical-reference.md#device-platform-condition).


Een algemene use-case voor deze situatie doet zich een beleid dat beperkt de toegang tot uw cloud-apps naar [beheerde apparaten](require-managed-devices.md). Zie voor meer scenario's met inbegrip van de voorwaarde van apparaat-platform, [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](app-based-conditional-access.md).



## <a name="device-state"></a>Apparaatstatus

De apparaatstatusvoorwaarde sluit hybride die Azure AD gekoppelde apparaten en apparaten die zijn gemarkeerd als compatibel van beleid voor voorwaardelijke toegang. 


![Status van het apparaat configureren](./media/conditions/112.png)

Deze voorwaarde is handig wanneer een beleid van toepassing alleen op een niet-beheerd apparaat voor de sessiebeveiliging van aanvullende. Alleen afdwingen bijvoorbeeld de Microsoft Cloud App Security-sessiebeheer wanneer een apparaat niet-beheerde is. 

## <a name="locations"></a>Locaties

Met behulp van locaties, kunt u voorwaarden op basis van waar de geprobeerd om een verbinding is te definiëren. 

![Locaties configureren](./media/conditions/25.png)

Algemene scenario's voor deze voorwaarde zijn beleid met de volgende goed:

- Meervoudige verificatie vereisen voor gebruikers met toegang tot een service als ze uit het bedrijfsnetwerk bevinden.  

- Toegang blokkeren voor gebruikers met toegang tot een service van specifieke landen of regio's. 

Zie voor meer informatie, [wat is er met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>Client-apps

Standaard is een beleid voor voorwaardelijke toegang van toepassing op de volgende apps:

- **[Browser-apps](technical-reference.md#supported-browsers)**  -Browser-apps zijn onder andere websites met behulp van de SAML, WS-Federation en OpenID Connect web SSO-protocollen. Dit geldt ook voor elke website of web-service die is geregistreerd als een vertrouwelijke OAuth-client. Bijvoorbeeld, de Office 365 SharePoint-website. 

- **[Mobiele en bureaublad-apps die moderne authenticatie gebruiken](technical-reference.md#supported-mobile-applications-and-desktop-clients)**  -deze apps bevatten de Office-bureaubladtoepassingen en phone-apps. 


Bovendien kunt u zich richten op een beleid op specifieke client-apps die geen moderne verificatie, bijvoorbeeld:

- **[Exchange ActiveSync-clients](conditions.md#exchange-activesync-clients)**  : wanneer een beleid blokken met behulp van Exchange ActiveSync, betrokken gebruikers een enkel quarantaine-e met informatie krijgen over waarom ze worden geblokkeerd. Indien nodig, bevat het e-mailbericht instructies voor het inschrijven van hun apparaat bij Intune.

- **[Andere clients](block-legacy-authentication.md)**  -deze apps dus ook clients die gebruikmaken van basisverificatie met e-mailprotocollen zoals IMAP, MAPI-, pop-server, SMTP en oudere Office-apps die moderne verificatie wordt niet gebruikt. Zie voor meer informatie, [hoe moderne verificatie werkt voor client-apps voor Office 2013 en Office 2016](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016).

![Client-apps](./media/conditions/41.png)

Algemene scenario's voor deze voorwaarde zijn beleid met de volgende vereisten:

- **[Vereisen dat een beheerd apparaat](require-managed-devices.md)**  voor mobiele en desktoptoepassingen die gegevens naar een apparaat downloaden. Op hetzelfde moment, kunt u browsertoegang vanaf elk apparaat. In dit scenario wordt voorkomen dat documenten opslaan en synchroniseren met een niet-beheerde apparaat. Met deze methode kunt u de kans voor verlies van gegevens beperken als het apparaat is zoekgeraakt of gestolen.

- **[Vereisen dat een beheerd apparaat](require-managed-devices.md)**  voor apps met behulp van ActiveSync voor toegang tot Exchange Online.

- **[Verouderde verificatie blok](block-legacy-authentication.md)**  naar Azure AD (andere clients)

- Blokkeer de toegang van web-apps, maar zodat toegang vanaf mobiele en bureaubladtoepassingen.



### <a name="exchange-activesync-clients"></a>Exchange ActiveSync-clients

U kunt alleen selecteren **Exchange ActiveSync-clients** als:


- Microsoft Office 365 Exchange Online is de enige cloud-app die u hebt geselecteerd.

    ![Cloud-apps](./media/conditions/32.png)

- U hebt geen andere bepalingen die zijn geconfigureerd in een beleid. Echter, u het bereik van dit probleem is alleen van toepassing kunt verfijnen [ondersteunde platforms](technical-reference.md#device-platform-condition).
 
    ![Het beleid toepassen op ondersteunde platformen](./media/conditions/33.png)


Wanneer toegang is geblokkeerd omdat een [beheerd apparaat](require-managed-devices.md) is vereist, de betrokken gebruikers een enkel een e-mail ontvangt die bevat stappen waarmee ze om Intune te gebruiken. 

Als een goedgekeurde app vereist is, krijgt de betrokken gebruikers richtlijnen voor het installeren en gebruiken van de Outlook-client voor mobiele.

In andere gevallen, bijvoorbeeld, worden als MFA vereist is, de betrokken gebruikers geblokkeerd, omdat clients met behulp van basisverificatie bieden geen ondersteuning voor MFA.

U kunt deze instelling om gebruikers en groepen alleen richten. Het biedt geen ondersteuning voor gasten of rollen. Als een voorwaarde gast- of -rol is geconfigureerd, worden alle gebruikers geblokkeerd omdat voorwaardelijke toegang kan niet worden vastgesteld als het beleid van toepassing op de gebruiker of niet.


 Zie voor meer informatie:

- [SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
 
- [Azure Active Directory op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). 



## <a name="next-steps"></a>Volgende stappen

- Als u wilt weten hoe u een beleid voor voorwaardelijke toegang configureren, Zie [Quick Start: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

- Voor het configureren van beleid voor voorwaardelijke toegang voor uw omgeving, Zie de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md). 

