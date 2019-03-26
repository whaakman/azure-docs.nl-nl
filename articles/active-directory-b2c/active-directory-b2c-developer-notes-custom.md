---
title: Opmerkingen voor ontwikkelaars voor aangepast beleid - Azure Active Directory B2C | Microsoft Docs
description: Opmerkingen voor ontwikkelaars over het configureren en onderhouden van Azure AD B2C met aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: cf9c6f6a54c38f00e477e2a9d62e72ab5faccdef
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418921"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Opmerkingen voor ontwikkelaars voor aangepast beleid in Azure Active Directory B2C

Configuratie van aangepast beleid in Azure Active Directory B2C is nu algemeen beschikbaar. Deze methode van de configuratie is gericht op ontwikkelaars Geavanceerd Identiteitsbeheer voor het bouwen van complexe identiteitsoplossingen. Aangepaste beleidsregels maken de kracht van de Identiteitservaring-Framework beschikbaar in Azure AD B2C-tenants. Geavanceerde identiteit ontwikkelaars die gebruikmaken van aangepast beleid plannen moeten om te investeren enige tijd voltooien van de zelfstudies en naslaginformatie over documenten te lezen.

De meeste van de beschikbare opties voor aangepast beleid zijn nu algemeen beschikbaar, maar er zijn onderliggende mogelijkheden, zoals technische profieltypen en inhoudsdefinitie API's die zich in verschillende fasen van de levenscyclus van software. Nog veel meer afkomstig zijn. De volgende tabel geeft het niveau van de beschikbaarheid van een meer gedetailleerd niveau.  

## <a name="features-that-are-generally-available"></a>Functies die algemeen beschikbaar

- De auteur en te uploaden, aangepaste verificatie gebruiker reizen met behulp van aangepast beleid.  
    - Gebruikers reizen stapsgewijze als worden uitgewisseld tussen claims-providers wordt beschreven.  
    - Definieer Voorwaardelijke vertakkingen in gebruiker reizen.  
- Samenwerken met services REST API's in uw aangepaste verificatie gebruiker reizen.  
- Federeren met id-providers die compatibel met het protocol OpenIDConnect zijn.  
- Federeren met id-providers die aan het SAML 2.0-protocol voldoen.   

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>De verantwoordelijkheden van aangepast beleid functieset ontwikkelaars

Handmatige configuratie op lager niveau toegang verleent tot het onderliggende platform van Azure AD B2C en de resultaten bij het maken van een unieke, vertrouwen framework. Het aantal mogelijke permutaties van aangepaste id-providers, vertrouwensrelaties, integratie met externe services en stapsgewijze werkstromen vereisen een methodisch benadering voor het ontwerpen en configureren. 

Ontwikkelaars verbruikt de functieset aangepast beleid moeten voldoen aan de volgende richtlijnen:

- Vertrouwd raken met de taal van de configuratie van het aangepaste beleid en de sleutel/geheimen management. Zie voor meer informatie, [TrustFrameworkPolicy](trustframeworkpolicy.md). 
- Eigenaar worden van scenario's en aangepaste integraties. Documenteer uw werk en kennis van uw organisatie live site.  
- Methodisch scenariotests uitvoeren. 
- Ga als volgt software ontwikkel- en staging-best practices met een minimum van een ontwikkelings- en testomgeving en een productie-omgeving. 
- Blijf op de hoogte van nieuwe ontwikkelingen op het gebied van de id-providers en de services die u met integreert. Bijvoorbeeld, bijhouden van wijzigingen in de geheimen en van geplande en ongeplande wijzigingen in de service. 
- Actieve bewaking hebt ingesteld en de reactietijd van productie-omgevingen bewaken. Zie voor meer informatie over de integratie met Application Insights [Azure Active Directory B2C: Verzamelen van logboeken](active-directory-b2c-custom-guide-eventlogger-appins.md). 
- Neem contact op met e-mailadressen Houd in het Azure-abonnement bent en blijf te reageren op de Microsoft live-site-team e-mailberichten. 
- Maatregelen tijdig wanneer op de hoogte om dit te doen door het team van Microsoft live-site.

## <a name="terms-for-features-in-public-preview"></a>Voorwaarden voor functies in public preview

- We raden u aan de openbare preview-functies gebruiken voor evaluatiedoeleinden. 
- Service level agreements (Sla's) niet van toepassing op de openbare preview-functies.
- Ondersteuningsaanvragen voor openbare preview-functies kunnen worden ingediend via reguliere ondersteuningskanalen. 

## <a name="features-by-stage-and-known-issues"></a>Functies van de fase en bekende problemen

Aangepaste beleid/Identity-Ervaringsframework mogelijkheden zijn onder constante en snelle ontwikkeling. De volgende tabel bevat een overzicht van de functies en beschikbaarheid van onderdeel.

### <a name="identity-providers-tokens-protocols"></a>Identity Providers, Tokens, Protocols

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-------- | ----------- | ------- | -- | ----- |
| IDP-OpenIDConnect |  |  | X | Bijvoorbeeld, Google +.  |
| IDP-OAUTH2 |  |  | X | Bijvoorbeeld: Facebook.  |
| IDP-OAUTH1 (twitter) |  | X |  | Bijvoorbeeld, Twitter. |
| IDP-OAUTH1 (ex-twitter) |  |  |  | Niet ondersteund |
| IDP-SAML |  |   | X | Bijvoorbeeld, Salesforce, AD FS. |
| IDP-WSFED | X |  |  |  |
| Relying Party OAUTH1 |  |  |  | Wordt niet ondersteund. |
| Relying Party OAUTH2 |  |  | X |  |
| Relying Party OIDC |  |  | X |  |
| Relying Party SAML | X |  |  |  |
| Relying Party WSFED | X |  |  |  |
| REST-API met basic en authenticatie van clientcertificaat |  |  | X | Bijvoorbeeld, Azure Logic Apps. |

### <a name="component-support"></a>Ondersteuning voor onderdeel

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
| ------- | ----------- | ------- | -- | ----- |
| Azure Multi Factor Authentication |  |  | X |  |
| Azure Active Directory als de lokale map |  |  | X |  |
| Azure e-subsysteem voor e-mailverificatie |  |  | X |  |
| Ondersteuning voor meerdere talen|  |  | X |  |
| Predikaat validaties |  |  | X | Bijvoorbeeld: wachtwoordcomplexiteit. |
| Met behulp van externe e-serviceproviders | X |  |  |  |

### <a name="content-definition"></a>Inhoudsdefinitie

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
| ------- | ----------- | ------- | -- | ----- |
| Foutpagina, api.error |  |  | X |  |
| Pagina voor het selecteren van id-provider, api.idpselections |  |  | X |  |
| Selectie van de id-provider voor aanmelding, api.idpselections.signup |  |  | X |  |
| Wachtwoord bent vergeten, api.localaccountpasswordreset |  |  | X |  |
| Lokaal Account aanmelden, api.localaccountsignin |  |  | X |  |
| Lokaal Account aanmelden, api.localaccountsignup |  |  | X |  |
| MFA-pagina, api.phonefactor |  |  | X |  |
| Zelf-gecontroleerde sociaal account aanmelden, api.selfasserted |  |  | X |  |
| Zelf een profiel bijwerken door de bevestigde api.selfasserted.profileupdate |  |  | X |  |
| Geïntegreerde aanmelding of aanmeldingspagina, api.signuporsignin, met de parameter "disableSignup" |  |  | X |  |
| JavaScript / pagina contract |  | X |  |  |

### <a name="app-ief-integration"></a>App-IEF-integratie

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
| ------- | ----------- | ------- | -- | ----- |
| Query-tekenreeks parameter domain_hint |  |  | X | Service is beschikbaar als de claim, kunnen worden doorgegeven aan de id-provider. |
| Query-tekenreeks parameter login_hint |  |  | X | Service is beschikbaar als de claim, kunnen worden doorgegeven aan de id-provider. |
| JSON in UserJourney via client_assertion invoegen | X |  |  | Wordt afgeschaft. |
| JSON in UserJourney als id_token_hint invoegen |  | X |  | Go-zone voor forward benadering voor het doorgeven van JSON. |
| IDP TOKEN doorgeven aan de toepassing |  | X |  | Bijvoorbeeld, van Facebook naar de app. |

### <a name="session-management"></a>Sessiebeheer

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
| ------- | ----------- | ------- | -- | ----- |
| Sessie-Provider voor eenmalige aanmelding |  |  | X |  |
| Provider voor de sessie externe aanmelding |  |  | X |  |
| Provider voor SAML SSO-sessie |  |  | X |  |
| Standaardprovider SSO-sessie |  |  | X |  |

### <a name="security"></a>Beveiliging

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
|-------- | ----------- | ------- | -- | ----- |
| Beleid voor sleutels te genereren, handmatig, uploaden |  |  | X |  |
| Beleid voor sleutels - RSA/Cert geheimen |  |  | X |  |
| Beleid uploaden |  |  | X |  |

### <a name="developer-interface"></a>Developer-interface

| Functie | Ontwikkeling | Preview | Algemene beschikbaarheid | Opmerkingen |
| ------- | ----------- | ------- | -- | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| Application Insights-logboekbestanden met UserJourney |  | X |  | Gebruikt voor het oplossen van problemen tijdens de ontwikkeling.  |
| Application Insights-gebeurtenislogboeken (via indelingsstappen) |  | X |  | Gebruikt voor het bewaken van de gebruikersstromen in de productieomgeving. |

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over aangepaste beleidsregels en de verschillen met gebruikersstromen](active-directory-b2c-overview-custom.md).
