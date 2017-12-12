---
title: 'Azure Active Directory B2C: Opmerkingen voor ontwikkelaars op het gebruik van aangepast beleid | Microsoft Docs'
description: Opmerkingen voor ontwikkelaars over het configureren en onderhouden van Azure AD B2C met aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/13/2017
ms.author: joroja
ms.openlocfilehash: 064ccec58406e08ae68320b59fcf2a96952a41ee
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Releaseopmerkingen voor openbare preview van Azure Active Directory B2C aangepast beleid
De functieset aangepast beleid is nu beschikbaar voor evaluatie van de openbare preview voor alle Azure Active Directory B2C (Azure AD B2C) klanten. Deze functieset is gericht op geavanceerde identiteit ontwikkelaars oplossingen die het meest complexe identiteit.  

Vandaag de dag, vereist deze functieset ontwikkelaars voor het configureren van het Framework identiteit ervaring rechtstreeks via XML-bestand bewerken. Deze methode van de configuratie is een krachtige en complexe. Geavanceerde identiteit ontwikkelaars met behulp van het kader van de gebruikerservaring identiteit moeten wilt investeren enige tijd voor het voltooien van de zelfstudies en verwijzing naar documenten te lezen. 

## <a name="features-included-in-this-public-preview"></a>Functies in deze openbare preview
Met de nieuwe functies geïntroduceerd in de openbare preview, kunnen ontwikkelaars de volgende taken uitvoeren:<br>

* Maken en uploaden aangepaste verificatie gebruiker trajecten met behulp van aangepast beleid. 
   * Gebruikers reizen stapsgewijze als kunnen worden uitgewisseld tussen claimproviders beschrijven. 
   * Definieer Voorwaardelijke vertakkingen in trajecten van de gebruiker. 
* REST-API-diensten in uw aangepaste verificatie gebruiker trajecten worden geïntegreerd.  
* Federatie met de id-providers die compatibel met de standaard OpenIDConnect zijn toevoegen. <br>
* Toevoegen van Federatie met de id-providers die aan het SAML 2.0-protocol voldoen. 

## <a name="terms-of-the-public-preview"></a>Voorwaarden van de openbare preview

* We raden u aan de nieuwe functies gebruiken voor evaluatiedoeleinden.<br>
* De nieuwe functies zijn niet bedoeld voor gebruik in een productieomgeving.<br>
* Service level agreements (Sla) niet van toepassing op de nieuwe functies. <br>
* Ondersteuningsaanvragen kunnen worden ingediend via reguliere ondersteuningskanalen. <br>
* Er is geen toegezegde opgegeven voor algemene beschikbaarheid.<br>
* Onze goeddunken, en voor welke reden dan ook, kunt Microsoft markeren en weigeren of scenario's en gebruiker trajecten die groter is dan het bereik van de Azure AD B2C product Freelance moeten fungeren als een klant identiteits- en toegangsbeheer (CIAM) beheerplatform beperken.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>De verantwoordelijkheden van aangepast beleid functieset ontwikkelaars
Handmatige beleidsconfiguratie lager niveau toegang verleent tot het onderliggende platform van Azure AD B2C en resulteert in het maken van een unieke, volledig aanpasbare vertrouwensrelatie-framework. Het aantal mogelijke permutaties met aangepaste identiteitsproviders, vertrouwensrelaties, integraties met externe services en stapsgewijze werkstromen hogere eisen op plaatsen gevorderde ontwikkelaars die ze gebruiken.

Om volledig profiteren van de openbare preview, is het raadzaam dat ontwikkelaars de functieset aangepast beleid gebruiken in overeenstemming zijn met de volgende richtlijnen:
* Vertrouwd raken met de taal van de configuratie van de identiteit ervaring Framework en het beheer van de sleutel/geheimen.
* Eigenaar worden van scenario's en aangepaste integraties.
* Methodisch scenariotests uitvoeren.
* Ga als volgt software ontwikkelings- en staging-best practices met ten minste één ontwikkelings- en testomgeving en een productie-omgeving.
* Blijf op de hoogte over nieuwe ontwikkelingen in de id-providers en services die u met integreert. Bijvoorbeeld: bijhouden van wijzigingen in geheimen en geplande en ongeplande wijzigingen in de service.
* Actieve bewaking hebt ingesteld en bewaak de reactiesnelheid van productieomgevingen.
* Neem contact op met e-mailadressen met actuele in de Azure-abonnement en blijven reageert op de Microsoft live-site-team e-mailberichten.
* Maatregelen tijdige wanneer aangeraden om dit te doen door het team van Microsoft live-site. 

## <a name="features-by-stage-and-known-issues"></a>Functies door de fase en bekende problemen
Aangepast beleid/identiteit ervaring Framework mogelijkheden zijn onder constante en snelle ontwikkeling.  Deze tabel is een index van de beschikbaarheid van functies/componenten.

Stel vragen in de Stack Overflow op [aka.ms/aadb2cso](http://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>Protocollen voor identiteit Providers, Tokens.
Interfaces met externe onderdelen en toepassingen

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| IDP OpenIDConnect |  | x |  | bijvoorbeeld, Google + |
| IDP OAUTH2 |  | x |  | bijvoorbeeld, Facebook  |
| IDP OAUTH1 |  | x |  | bijvoorbeeld, Twitter |
| IDP SAML |  | x |  | bijvoorbeeld Salesforce, AD FS |
| IDP WSFED | x |  |  |  |
| Relying Party OAUTH |  | x |  |  |
| Relying Party OIDC |  | x |  |  |
| Relying Party SAML | x |  |  |  |
| Relying Party WSFED | x |  |  |  |
| REST-API met basic en belastingdienst verificatie |  | x |  | bijvoorbeeld: Azure Functions |


### <a name="component-support"></a>Ondersteuning voor onderdeel


| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi Factor Authentication |  | x |  |  |
| Azure Active Directory als de lokale directory |  | x |  |  |
| Azure e-subsysteem voor 2FA |  | x |  |  |
| Ondersteuning voor meerdere talen|  | x |  |  |
| Wachtwoordcomplexiteit | x |  |  |  |


### <a name="content-definition"></a>De definitie van inhoud

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Foutpagina, api.error |  | x |  |  |
|   De pagina selectie IDP, api.idpselections |  | x |  |  |
|   IDP selectie voor aanmelding, api.idpselections.signup |  | x |  |  |
|   Wachtwoord vergeten api.localaccountpasswordreset |  | x |  |  |
|   Lokaal Account aanmelden, api.localaccountsignin |  | x |  |  |
|   Lokaal Account aanmelden, api.localaccountsignup |  | x |  |  |
|   MFA-pagina, api.phonefactor |  | x |  |  |
|   Zelf die wordt beweerd-bijvoorbeeld sociale account sig-up api.selfasserted |  | x |  |  |
|   Zelf Profielupdate die wordt beweerd api.selfasserted.profileupdate |  | x |  |  |
|   Geïntegreerde aanmelding of aanmeldingspagina, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>App-IEF-integratie
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Query-tekenreeks parameter id_token_hint | x |  |  |  |
| Query-tekenreeks parameter domain_hint |  | x |  | beschikbaar als de claim, kan worden doorgegeven aan IDP |
| Query-tekenreeks parameter login_hint |  | x |  | beschikbaar als de claim, kan worden doorgegeven aan IDP |
| JSON invoegen in UserJourney via client_assertion | x |  |  | afgeschaft |
| JSON invoegen in UserJourney als id_token_hint | x |  |  | Ga doorsturen benadering JSON doorgeven |


### <a name="session-management"></a>Sessiebeheer

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------|-------------|---------|----|-------|
| Sessie-Provider voor eenmalige aanmelding |  | x |  |  |
| Sessie-Provider voor externe aanmelding |  | x |  |  |
| Provider voor eenmalige aanmelding voor SAML-sessie |  | x |  |  |


### <a name="security"></a>Beveiliging
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| Beleid sleutels genereren, handmatig uploaden |  | x |  |  |
| Beleid sleutels - RSA/Cert geheimen |  | x |  |  |


### <a name="developer-interface"></a>Developer-interface
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| Azure Portal-IEF UX |  | x |  |  |
| Application Insights UserJourney Logboeken  |  | x |  |  |
| Application Insights-gebeurtenislogboeken |x|  |  |  |



## <a name="next-steps"></a>Volgende stappen
[Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
