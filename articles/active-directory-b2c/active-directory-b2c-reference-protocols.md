---
title: 'Azure Active Directory B2C: Verificatieprotocollen | Microsoft Docs'
description: Apps maken via de protocollen die worden ondersteund door Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5e407d0a-73a2-4d74-ac81-3aa6c31ddcee
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: eb5d0da5b723c5fef81f7d74936ae30d4366385c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Verificatieprotocollen
Azure Active Directory B2C (Azure AD B2C) biedt identiteit als een service voor uw apps door de ondersteuning van twee standaardprotocollen: OpenID Connect en OAuth 2.0. De service is compatibel met standaarden, maar de twee implementaties van deze protocollen subtiele verschillen kunnen hebben. 

De informatie in deze handleiding is handig als u uw code schrijven door rechtstreeks verzenden en HTTP-aanvragen voor de verwerking, in plaats van via een open-source-bibliotheek. Het is raadzaam dat u deze pagina te lezen voordat u Duik in de details van elk protocol dat specifieke. Maar als u al bekend met Azure AD B2C bent, gaat u meteen naar [de verwijzing naar protocol handleidingen](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>De basisbeginselen
Elke app die gebruikmaakt van Azure AD B2C moet worden geregistreerd in uw B2C-directory in de [Azure-portal](https://portal.azure.com). Tijdens het registratieproces van de app worden enkele waarden verzameld en toegewezen aan uw app:

* Een **toepassings-id** die de app op unieke wijze identificeert.
* Een **omleidings-URI** of **pakket-id** die kunnen worden gebruikt om een directe antwoorden terug naar uw app.
* Enkele andere scenariospecifieke waarden. Informatie voor meer informatie over [voor de registratie van uw toepassing](active-directory-b2c-app-registration.md).

Nadat u uw app registreert, wordt deze door verzoeken te sturen naar het eindpunt communiceert met Azure Active Directory (Azure AD):

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Bijna alle OAuth en OpenID Connect stromen zijn vier partijen betrokken in de exchange:

![OAuth 2.0-functies](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* De **autorisatie server** is het Azure AD-eindpunt. Veilig verwerkt iets betrekking hebben op gebruikersgegevens en -toegang. Ook verwerkt de vertrouwensrelaties tussen de partijen in een stroom. Het is verantwoordelijk voor het controleren van de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en uitgeven van tokens. Het is ook bekend als de id-provider.

* De **resource-eigenaar** is meestal de eindgebruiker. Is de partij die eigenaar is van de gegevens en hieraan het vermogen om toe te staan van derden voor toegang tot die gegevens of de resource.

* De **OAuth client** wordt uw app. Die wordt geïdentificeerd door de toepassing-ID. Dit is meestal de partij die eindgebruikers interactie met hebben. Deze tokens ook-aanvragen van de autorisatie-server. De resource-eigenaar moet de machtiging client toegang tot de bron.

* De **bronserver** is waar de resource of de gegevens zich bevindt. Deze vertrouwt de autorisatie-server veilig verifiëren en autoriseren van de OAuth-client. Access-bearer-tokens worden ook gebruikt om ervoor te zorgen dat toegang tot een bron kan worden toegekend.

## <a name="policies"></a>Beleidsregels
Azure AD B2C-beleidsregels zijn weliswaar, de belangrijkste functies van de service. Azure AD B2C breidt de standaard OAuth 2.0 en OpenID Connect-protocollen door de introductie van beleid. Hiermee kunnen Azure AD B2C om uit te voeren veel meer dan een eenvoudige verificatie en autorisatie. 

Beleid beschrijven consumer identiteitservaringen, met inbegrip van registreren, aanmelden, volledig en profiel bewerken. Beleidsregels kunnen worden gedefinieerd in een administratieve gebruikersinterface. Ze kunnen worden uitgevoerd met behulp van een speciale queryparameter in HTTP-aanvragen voor verificatie. 

Beleidsregels zijn niet standaard functies van OAuth 2.0 en OpenID Connect, dus u moet de tijd om te begrijpen. Zie voor meer informatie de [Naslaggids voor Azure AD B2C beleid](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokens
De Azure AD B2C-implementatie van OAuth 2.0 en OpenID Connect wordt uitgebreid gebruikgemaakt van bearer-tokens, bearer-tokens die worden weergegeven als JSON webtokens (JWTs). Een bearer-token is een lichtgewicht beveiligingstoken die de 'bearer' toegang tot een beveiligde bron verleent.

De houder is een partij die het token kan opleveren. Azure AD een partij moet eerst worden geverifieerd voordat er een bearer-token kan ontvangen. Maar als de vereiste stappen niet genomen worden voor het beveiligen van het token in overdracht en opslag, kan deze kan worden onderschept en gebruikt door een onbedoelde partij.

Sommige beveiligingstokens ingebouwde mechanismen die voorkomen dat onbevoegden gebruiken ze hebben, maar bearer-tokens hebben geen dit mechanisme. Zij moeten worden overgebracht in een beveiligd kanaal, zoals een transport layer security (HTTPS). 

Als een bearer-token buiten een beveiligd kanaal wordt verzonden, kunnen een kwaadwillende party een man-in-the-middle-aanval kunt gebruiken om het token verkrijgen en gebruiken om onbevoegde toegang te krijgen tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer bearer-tokens worden opgeslagen of voor later gebruik opgeslagen. Altijd voor zorgen dat uw app verzendt en bearer-tokens worden opgeslagen op een veilige manier.

Zie voor aanvullende bearer-token beveiligingsoverwegingen [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende typen tokens die worden gebruikt in Azure AD B2C zijn beschikbaar in [de Azure AD-tokenverwijzing](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protocollen
Wanneer u klaar bent om te controleren van bepaalde aanvragen voorbeeld, kunt u beginnen met een van de volgende zelfstudies. Elke komt overeen met een bepaalde verificatiescenario. Als u hulp bij het bepalen welke stroom geschikt is voor u nodig hebt, kijk dan eens [de typen apps die u maken kunt met behulp van Azure AD B2C](active-directory-b2c-apps.md).

* [Mobiele en systeemeigen toepassingen bouwen met behulp van OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
* [Web-apps bouwen met behulp van OpenID Connect](active-directory-b2c-reference-oidc.md)
* [Apps van één pagina met behulp van de impliciete OAuth 2.0-stroom maken](active-directory-b2c-reference-spa.md)

