---
title: Azure Active Directory-risicogebeurtenissen | Microsoft Docs
description: In dit onderwerp vindt u een gedetailleerd overzicht van wat risicogebeurtenissen zijn.
services: active-directory
keywords: beveiliging voor Azure active directory-identiteit, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
author: MarkusVi
manager: mtillman
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 59c8932f7676a5388413baf2edb5d9e259769f93
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory-risicogebeurtenissen

De meeste beveiligingslekken plaatsvinden als aanvallers toegang krijgen tot een omgeving met de identiteit van een gebruiker te stelen. Detectie van verdachte identiteiten is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodiek voor het detecteren van verdachte acties die zijn gekoppeld aan uw gebruikersaccounts. Elk gedetecteerd verdachte actie is opgeslagen in een record aangeroepen *risicogebeurtenis*.

Azure Active Directory detecteert op dit moment zes typen risicogebeurtenissen die:

- [Gebruikers met gelekte referenties](#leaked-credentials) 
- [Aanmeldingen vanaf anonieme IP-adressen](#sign-ins-from-anonymous-ip-addresses) 
- [Onmogelijke reis naar ongewone locaties](#impossible-travel-to-atypical-locations) 
- [Aanmeldingen vanaf geïnfecteerde apparaten](#sign-ins-from-infected-devices) 
- [Aanmeldingen vanaf IP-adressen met verdachte activiteiten](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Aanmeldingen vanaf onbekende locaties](#sign-in-from-unfamiliar-locations) 


![Risicogebeurtenis](./media/active-directory-reporting-risk-events/91.png)

Het voor een risicogebeurtenis gedetecteerde dat u inzicht is gekoppeld aan uw abonnement Azure AD. Met de Azure AD Premium P2-editie, moet u de meest gedetailleerde informatie over alle onderliggende detecties ophalen. Met de editie Azure AD Premium-P1 detecties die niet wordt gedekt door uw licenties worden weergegeven als de risicogebeurtenis **aanmelden met extra risico gedetecteerd**.


In dit onderwerp biedt u een gedetailleerd overzicht van welke gebeurtenissen van de risico's zijn en hoe u ze kunt gebruiken om de identiteiten van uw Azure AD te beschermen.


## <a name="risk-event-types"></a>Risicogebeurtenistypen

De eigenschap voor type gebeurtenis risico is dat een id voor de verdachte actie een record van de gebeurtenis risico is gemaakt.  
Microsoft continue investeringen in het detectieproces leiden tot:

- Verbeteringen in de nauwkeurigheid van de detectie van de bestaande risicogebeurtenissen 
- Nieuwe typen van de risico's gebeurtenissen die in de toekomst worden toegevoegd

### <a name="leaked-credentials"></a>Gelekte aanmeldingsreferenties

Wanneer cybercriminelen inbreuk geldige wachtwoorden van legitieme gebruikers, wordt in de criminelen vaak deze referenties delen. Dit gebeurt gewoonlijk door ze te posten openbaar op de donker web- of -sites of door handel of de referenties op de zwarte markt verkopen. De Microsoft gelekte referenties service verkrijgt gebruikersnaam / wachtwoord paren door de bewaking van openbare en donkere websites en door met:

- Onderzoekers
- Justitie en politie
- Microsoft Security-teams
- Andere betrouwbare bronnen 

Wanneer de service verkrijgt gebruikersnaam / wachtwoord paren, worden ze gecontroleerd tegen de huidige geldige referenties AAD-gebruikers. Wanneer een overeenkomst is gevonden, betekent dit dat het wachtwoord van een gebruiker is aangetast, en een *gelekte referenties risicogebeurtenis* wordt gemaakt.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Dit type risico gebeurtenis identificeert gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door mensen die u wilt verbergen, IP-adres van het apparaat en kan worden gebruikt voor kwade bedoelingen.


### <a name="impossible-travel-to-atypical-locations"></a>Onmogelijke reis naar ongewone locaties

Dit type risico gebeurtenis identificeert twee aanmeldingen die afkomstig zijn van geografisch verafgelegen locaties, waarbij ten minste één van de locaties mogelijk ook ongebruikelijk is voor de gebruiker, krijgt het verleden gedrag. Tussen verschillende andere factoren rekening deze machine learning-algoritme gehouden met de tijd tussen de twee aanmeldingen en de tijd die nodig zou hebben voor de gebruiker van de eerste locatie naar de tweede, die aangeeft dat een andere gebruiker is met behulp van dezelfde gaan de referenties.

De algoritme wordt genegeerd voor de hand liggende 'valse positieven' bijdragen aan de voorwaarden onmogelijke reis, zoals VPN-verbindingen en de locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie. Het systeem heeft een initiële learning periode van 14 dagen gedurende welke aan een nieuwe gebruiker aanmelden gedrag leert. 

### <a name="sign-in-from-unfamiliar-locations"></a>Aanmelden vanaf onbekende locaties

Dit type risico gebeurtenis uit het verleden aanmelden locaties overweegt (IP, breedtegraad / lengtegraad en ASN) om te bepalen van de nieuwe / onbekende locaties. Het systeem wordt informatie opgeslagen over de voorgaande locaties die worden gebruikt door een gebruiker en deze 'bekend' locaties overweegt. De risicogebeurtenis wordt geactiveerd wanneer de aanmeldingspagina plaatsvindt vanaf een locatie die nog niet in de lijst met vertrouwde sites. Het systeem heeft een initiële learning periode van 30 dagen, waarover biedt het geen nieuwe locaties als onbekende locaties vlag. Aanmeldingen vanaf bekende apparaten en de locaties die geografisch zich dicht bij een vertrouwde locatie worden ook negeert door het systeem. 

### <a name="sign-ins-from-infected-devices"></a>Aanmeldingen vanaf geïnfecteerde apparaten

Dit type risico gebeurtenis identificeert aanmeldingen vanaf apparaten geïnfecteerd met malware die bekend zijn bij actief communiceren met een bot-server. Dit wordt bepaald door de IP-adressen van het apparaat van de gebruiker op basis van IP-adressen die verbonden met een bot-server zijn. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Aanmeldingen van IP-adressen met verdachte activiteit
Dit type risico gebeurtenis identificeert IP-adressen waaruit een groot aantal mislukte aanmeldingspogingen zijn gezien, bij verschillende gebruikersaccounts gedurende een korte periode. Dit komt overeen met verkeerspatronen van IP-adressen die door aanvallers worden gebruikt en een sterke indicator dat accounts al zijn, of moeten worden aangetast. Dit is een machine learning-algoritme dat wordt genegeerd voor de hand liggende '*false positieven*', zoals IP-adressen die regelmatig worden gebruikt door andere gebruikers in de organisatie.  Het systeem heeft een initiële learning periode van 14 dagen waar deze leert het gedrag aanmelden van een nieuwe gebruiker en de nieuwe tenant.


## <a name="detection-type"></a>Detectietype

De eigenschap van het type detectie is een indicator (realtime of Offline) voor het tijdsbestek detectie van een risicogebeurtenis.  
Op dit moment worden de meeste risicogebeurtenissen gedetecteerd offline in na verwerking bewerking nadat de risicogebeurtenis heeft plaatsgevonden.

De volgende tabel bevat de hoeveelheid tijd die nodig is voor een type detectie worden weergegeven in een gerelateerde rapport:

| Detectie van Type | Reporting latentie |
| --- | --- |
| Realtime | 5 tot 10 minuten |
| Offline | 2 tot 4 uur |


Voor de gebeurtenis risico typen die Azure Active Directory wordt gedetecteerd, zijn de detectie-typen:

| Gebeurtenistype risico | Detectie van Type |
| :-- | --- | 
| [Gebruikers met gelekte referenties](#leaked-credentials) | Offline |
| [Aanmeldingen vanaf anonieme IP-adressen](#sign-ins-from-anonymous-ip-addresses) | Realtime |
| [Onmogelijke reis naar ongewone locaties](#impossible-travel-to-atypical-locations) | Offline |
| [Aanmeldingen vanaf onbekende locaties](#sign-in-from-unfamiliar-locations) | Realtime |
| [Aanmeldingen vanaf geïnfecteerde apparaten](#sign-ins-from-infected-devices) | Offline |
| [Aanmeldingen vanaf IP-adressen met verdachte activiteiten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Offline|


## <a name="risk-level"></a>Risiconiveau

De eigenschap van het risico van een risicogebeurtenis is een indicator (hoog, Gemiddeld of laag) voor de ernst en het vertrouwen van een risicogebeurtenis. Deze eigenschap kunt u bepalen welke acties die u moet uitvoeren. 

De ernst van de risicogebeurtenis vertegenwoordigt de sterkte van het signaal identiteit inbreuk te voorspellen.  
Het vertrouwen is een indicator voor de mogelijkheid van valse positieven. 

Bijvoorbeeld: 

* **Hoge**: hoge betrouwbaarheid en hoge urgentie risicogebeurtenis. Deze gebeurtenissen zijn sterk indicatoren die de identiteit van de gebruiker er inbreuk is gemaakt en alle gebruikersaccounts die van invloed op onmiddellijk moeten worden hersteld.

* **Gemiddeld**: hoog dreigingsniveau, maar lagere vertrouwen risicogebeurtenis, of vice versa. Deze gebeurtenissen zijn mogelijk schadelijke en van invloed op gebruikersaccounts moeten worden hersteld.

* **Lage**: lage vertrouwen en laag dreigingsniveau risicogebeurtenis. Deze gebeurtenis kan niet een directe actie is vereist, maar in combinatie met andere risicogebeurtenissen kan bieden een sterke aanwijzing dat de identiteit is geknoeid.

![Risiconiveau](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Gelekte aanmeldingsreferenties

Gelekte referenties risicogebeurtenissen die zijn geclassificeerd als een **hoge**, omdat ze een duidelijke aanwijzing is bieden dat de gebruikersnaam en wachtwoord beschikbaar voor een aanvaller zijn.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Het risiconiveau voor dit type risico gebeurtenis is **gemiddeld** omdat een anoniem IP-adres niet een sterke indicatie van inbreuk op een account is.  
Het is raadzaam dat u direct contact opnemen met de gebruiker om te controleren of als ze anonieme IP-adressen gebruikten.


### <a name="impossible-travel-to-atypical-locations"></a>Onmogelijke reis naar ongewone locaties

Onmogelijke reis is meestal een goede indicatie die een hacker kon is aanmelden. ONWAAR-positieven kunnen echter optreden wanneer een gebruiker op reis gaat met behulp van een nieuw apparaat of via een VPN die doorgaans niet wordt gebruikt door andere gebruikers in de organisatie. Een andere bron van false positieven is voor toepassingen die onjuist server IP-adressen als client-IP-adressen, zodat de vormgeving van aanmeldingen die hebben plaatsgevonden in het datacenter waarin de toepassing van back-end wordt gehost (dit zijn vaak Microsoft-datacenters die geven de vormgeving van aanmeldingen plaatsvinden van Microsoft die eigendom zijn van IP-adressen). Als gevolg van deze ONWAAR-positieven, het risiconiveau voor deze risicogebeurtenis is **gemiddeld**.

> [!TIP]
> U kunt de hoeveelheid gemelde ONWAAR-positieven voor dit type van de gebeurtenis risico verkleinen door te configureren [locaties met de naam](active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Aanmelden vanaf onbekende locaties

Onbekende locaties kunnen bieden een sterke aanwijzing dat een aanvaller gebruikmaken van de identiteit van een gestolen is. ONWAAR-positieven kunnen optreden wanneer een gebruiker op reis gaat, wordt geprobeerd om een nieuw apparaat of met behulp van een nieuwe VPN. Als gevolg van deze fout-positieven het risiconiveau voor dit gebeurtenistype is **gemiddeld**.

### <a name="sign-ins-from-infected-devices"></a>Aanmeldingen vanaf geïnfecteerde apparaten

Deze risicogebeurtenis identificeert IP-adressen, niet de apparaten van gebruikers. Als meerdere apparaten zich achter een enkel IP-adres en alleen bepaalde worden beheerd door een bot-netwerk, aanmeldingen vanaf andere apparaten mijn trigger deze gebeurtenis onnodig, dit is de reden voor het classificeren van deze risicogebeurtenis als **laag**.  

U wordt aangeraden dat u contact op met de gebruiker en onderzoeken van apparaten van de gebruiker. Het is ook mogelijk dat een gebruiker persoonlijk apparaat is geïnfecteerd, of zoals eerder gezegd, dat iemand anders is met behulp van een geïnfecteerde apparaten van hetzelfde IP-adres als de gebruiker. Geïnfecteerde apparaten zijn vaak geïnfecteerd met malware die nog niet is geïdentificeerd door de antivirussoftware, en kan ook duiden op als ongeldige gebruikers gewoontes op die het apparaat worden geïnfecteerd kunnen veroorzaakt.

Zie voor meer informatie over het adres malware-infecties de [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Aanmeldingen van IP-adressen met verdachte activiteit

Het is raadzaam dat u contact op met de gebruiker om te controleren of als ze daadwerkelijk aangemeld vanaf een IP-adres dat is gemarkeerd als verdacht. Het risiconiveau voor dit gebeurtenistype is '**gemiddeld**' omdat meerdere apparaten mogelijk achter hetzelfde IP-adres, terwijl er mag slechts enkele die verantwoordelijk is voor de verdachte activiteit worden. 


 
## <a name="next-steps"></a>Volgende stappen

Risico's vormen de basis voor het beveiligen van uw Azure AD-identiteiten. Azure AD kan momenteel zes risicogebeurtenissen detecteren: 


| Gebeurtenistype risico | Risiconiveau | Detectie van Type |
| :-- | --- | --- |
| [Gebruikers met gelekte referenties](#leaked-credentials) | Hoog | Offline |
| [Aanmeldingen vanaf anonieme IP-adressen](#sign-ins-from-anonymous-ip-addresses) | Middelgroot | Realtime |
| [Onmogelijke reis naar ongewone locaties](#impossible-travel-to-atypical-locations) | Middelgroot | Offline |
| [Aanmeldingen vanaf onbekende locaties](#sign-in-from-unfamiliar-locations) | Middelgroot | Realtime |
| [Aanmeldingen vanaf geïnfecteerde apparaten](#sign-ins-from-infected-devices) | Laag | Offline |
| [Aanmeldingen vanaf IP-adressen met verdachte activiteiten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Middelgroot | Offline|

Waar vind u de risicogebeurtenissen die in uw omgeving zijn gedetecteerd
Er zijn twee plaatsen waar u gerapporteerde risicogebeurtenissen bekijken:

 - **Rapportage van Azure AD** -Risicogebeurtenissen die deel uitmaken van Azure AD-beveiligingsgroep rapporten. Zie voor meer informatie de [gebruikers op risico beveiligingsrapport](active-directory-reporting-security-user-at-risk.md) en de [riskant aanmeldingen beveiligingsrapport](active-directory-reporting-security-risky-sign-ins.md).

 - **Azure AD Identity Protection** -risico's zijn ook deel uit van [Azure Active Directory: Identity Protection van](active-directory-identityprotection.md) rapportagemogelijkheden.
    

Tijdens de detectie van risico's al een belangrijk aspect vertegenwoordigt van het beveiligen van identiteiten, hebt u ook de optie ze handmatig wegnemen of zelfs automatische antwoorden implementeren door het beleid voor voorwaardelijke toegang configureren. Zie voor meer informatie van [Azure Active Directory: Identity Protection van](active-directory-identityprotection.md).
 
