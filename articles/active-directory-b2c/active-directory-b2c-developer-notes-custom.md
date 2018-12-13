---
title: Opmerkingen voor ontwikkelaars over het gebruik van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Opmerkingen voor ontwikkelaars over het configureren en onderhouden van Azure AD B2C met aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ea884f6fecc1e8a0de1f6a0f8a4daafcdf612e9f
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53272696"
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Opmerkingen bij de release voor de openbare preview van Azure Active Directory B2C aangepast beleid
De functieset aangepast beleid is nu beschikbaar voor evaluatie in openbare preview voor alle Azure Active Directory B2C (Azure AD B2C)-klanten. Deze functieset is gericht op ontwikkelaars Geavanceerd Identiteitsbeheer voor het bouwen van de meest complexe identiteitsoplossingen.  

Deze functieset vereist vandaag, ontwikkelaars het configureren van de Identity-Ervaringsframework rechtstreeks via het XML-bestand bewerken. Deze methode van de configuratie is een krachtige en complexe. Geavanceerde identiteit ontwikkelaars die gebruikmaken van de Identity-Ervaringsframework plannen moeten om te investeren enige tijd voltooien van de zelfstudies en naslaginformatie over documenten te lezen. 

## <a name="features-included-in-this-public-preview"></a>Functies die zijn opgenomen in deze openbare preview
Met de nieuwe functies geïntroduceerd in de openbare preview-versie, kunnen ontwikkelaars de volgende taken uitvoeren:<br>

* Maken en uploaden van aangepaste verificatie gebruiker reizen met behulp van aangepast beleid. 
   * Gebruikers reizen stapsgewijze als worden uitgewisseld tussen claims-providers wordt beschreven. 
   * Definieer Voorwaardelijke vertakkingen in gebruiker reizen. * Services REST API's in uw aangepaste verificatie gebruiker reizen integreren.  
* Federatie met id-providers die compatibel met de standaard OpenIDConnect zijn toevoegen. <br>
* Federatie met id-providers die aan het SAML 2.0-protocol voldoen toevoegen. 

## <a name="terms-of-the-public-preview"></a>Voorwaarden van de openbare preview

* We raden u aan het gebruik van de nieuwe functies voor evaluatiedoeleinden.<br>
* De nieuwe functies zijn niet bedoeld voor gebruik in een productieomgeving.<br>
* Service level agreements (Sla's) niet van toepassing op de nieuwe functies. <br>
* Ondersteuningsaanvragen kunnen worden ingediend via reguliere ondersteuningskanalen. <br>
* Er is geen toegezegde datum voor algemene beschikbaarheid.<br>
* Op naar eigen goeddunken, en voor welke reden dan ook, kunt Microsoft markeren en weigeren of scenario's en gebruiker reizen die groter zijn dan het bereik van het Handvest van de Azure AD B2C-product om te fungeren als een klant identiteits- en toegangsbeheer (CIAM) beheerplatform beperken.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>De verantwoordelijkheden van aangepast beleid functieset ontwikkelaars
Handmatige configuratie op lager niveau toegang verleent tot het onderliggende platform van Azure AD B2C en resulteert in het maken van een unieke, volledig aanpasbare vertrouwensrelatie-framework. De mogelijke permutaties van aangepaste id-providers, vertrouwensrelaties, integratie met externe services en stapsgewijze werkstromen plaats hogere eisen op het gebruiken van deze ontwikkelaars.

Om volledig te profiteren van de openbare preview, is het raadzaam dat ontwikkelaars de functieset aangepast beleid gebruiken in overeenstemming zijn met de volgende richtlijnen:
* Vertrouwd raken met de taal van de configuratie van de Identity-Ervaringsframework en het beheer van de sleutel/geheimen.
* Eigenaar worden van scenario's en aangepaste integraties.
* Methodisch scenariotests uitvoeren.
* Ga als volgt software ontwikkel- en staging-best practices met een minimum van een ontwikkelings- en testomgeving en een productie-omgeving.
* Blijf op de hoogte van nieuwe ontwikkelingen op het gebied van de id-providers en de services die u met integreert. Bijvoorbeeld, bijhouden van wijzigingen in de geheimen en van geplande en ongeplande wijzigingen in de service.
* Actieve bewaking hebt ingesteld en de reactietijd van productie-omgevingen bewaken.
* Neem contact op met e-mailadressen Houd in het Azure-abonnement bent en blijf te reageren op de Microsoft live-site-team e-mailberichten.
* Maatregelen tijdig wanneer op de hoogte om dit te doen door het team van Microsoft live-site. 

## <a name="features-by-stage-and-known-issues"></a>Functies van de fase en bekende problemen
Aangepaste beleid/Identity-Ervaringsframework mogelijkheden zijn onder constante en snelle ontwikkeling.  Deze tabel is een index van de beschikbaarheid van de functies/component.

Stel vragen op Stack Overflow op [https://aka.ms/aadb2cso](https://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>ID-Providers, Tokens, protocollen
Interfaces met externe onderdelen en toepassingen

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| IDP-OpenIDConnect |  | x |  | bijvoorbeeld, Google + |
| IDP-OAUTH2 |  | x |  | bijvoorbeeld, Facebook  |
| IDP-OAUTH1 |  | x |  | bijvoorbeeld, Twitter |
| IDP-SAML |  | x |  | bijvoorbeeld, Salesforce, AD FS |
| IDP-WSFED | x |  |  |  |
| Relying Party OAUTH |  | x |  |  |
| Relying Party OIDC |  | x |  |  |
| Relying Party SAML | x |  |  |  |
| Relying Party WSFED | x |  |  |  |
| REST-API met verificatie op basis- en -certificaat aanvragen basis |  | x |  | bijvoorbeeld, Azure Functions |


### <a name="component-support"></a>Ondersteuning voor onderdeel


| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi Factor Authentication |  | x |  |  |
| Azure Active Directory als de lokale map |  | x |  |  |
| Azure e-subsysteem voor 2FA |  | x |  |  |
| Ondersteuning voor meerdere talen|  | x |  |  |
| Wachtwoordcomplexiteit | x |  |  |  |


### <a name="content-definition"></a>Inhoudsdefinitie

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Foutpagina, api.error |  | x |  |  |
|   Pagina voor het selecteren van id-provider, api.idpselections |  | x |  |  |
|   Selectie van de id-provider voor aanmelding, api.idpselections.signup |  | x |  |  |
|   Wachtwoord bent vergeten, api.localaccountpasswordreset |  | x |  |  |
|   Lokaal Account aanmelden, api.localaccountsignin |  | x |  |  |
|   Lokaal Account aanmelden, api.localaccountsignup |  | x |  |  |
|   MFA-pagina, api.phonefactor |  | x |  |  |
|   Zelf een door de bevestigde-bijvoorbeeld sociaal account aanmelden, api.selfasserted |  | x |  |  |
|   Zelf een profiel bijwerken door de bevestigde api.selfasserted.profileupdate |  | x |  |  |
|   Geïntegreerde aanmelding of aanmeldingspagina, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>App-IEF-integratie
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Query-tekenreeks parameter id_token_hint | x |  |  |  |
| Query-tekenreeks parameter domain_hint |  | x |  | beschikbaar is als de claim, kunnen worden doorgegeven aan de id-provider |
| Query-tekenreeks parameter login_hint |  | x |  | beschikbaar is als de claim, kunnen worden doorgegeven aan de id-provider |
| JSON in UserJourney via client_assertion invoegen | x |  |  | wordt afgeschaft |
| JSON in UserJourney als id_token_hint invoegen | x |  |  | go-zone voor forward benadering voor het doorgeven van JSON |


### <a name="session-management"></a>Sessiebeheer

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------|-------------|---------|----|-------|
| Sessie-Provider voor eenmalige aanmelding |  | x |  |  |
| Provider voor de sessie externe aanmelding |  | x |  |  |
| Provider voor SAML SSO-sessie |  | x |  |  |


### <a name="security"></a>Beveiliging
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| Beleid voor sleutels te genereren, handmatig, uploaden |  | x |  |  |
| Beleid voor sleutels - RSA/Cert geheimen |  | x |  |  |


### <a name="developer-interface"></a>Developer-interface
| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|---------------------------------------------|-------------|---------|----|-------|
| Azure Portal-IEF UX |  | x |  |  |
| Application Insights-logboekbestanden met UserJourney  |  | x |  |  |
| Application Insights-gebeurtenislogboeken |x|  |  |  |



## <a name="next-steps"></a>Volgende stappen
[Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
