---
title: Protocollen voor verificatie in Azure Active Directory B2C | Microsoft Docs
description: Over het bouwen van apps direct met behulp van de protocollen die worden ondersteund door Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 19f3318c8bcaf634447285cab2951a89abf13a67
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837474"
---
# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C:-Verificatieprotocollen
Azure Active Directory B2C (Azure AD B2C) biedt identiteit als een service voor uw apps door de ondersteuning van twee protocollen volgens de industrienorm: OpenID Connect en OAuth 2.0. De service is compatibel met de standaarden, maar de twee implementaties van deze protocollen subtiele verschillen kunnen hebben. 

De informatie in deze handleiding is handig als u uw code schrijven door rechtstreeks verzenden en verwerken van HTTP-aanvragen, in plaats van met behulp van een open source-bibliotheek. Het is raadzaam dat u deze pagina hebt gelezen voordat u de details van elk protocol dat door specifieke duiken. Maar als u al bekend met Azure AD B2C bent, gaat u rechtstreeks naar [de naslaghandleidingen protocol](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>De basisbeginselen
Elke app die gebruikmaakt van Azure AD B2C moet worden geregistreerd in uw B2C-directory in de [Azure-portal](https://portal.azure.com). Tijdens het registratieproces van de app worden enkele waarden verzameld en toegewezen aan uw app:

* Een **toepassings-id** die de app op unieke wijze identificeert.
* Een **omleidings-URI** of **pakket-id** die kunnen worden gebruikt om antwoorden naar uw app terug te regelen.
* Enkele andere scenariospecifieke waarden. Informatie voor meer informatie over [informatie over het registreren van uw toepassing](active-directory-b2c-app-registration.md).

Nadat u uw app hebt geregistreerd, wordt deze door het verzenden van aanvragen naar het eindpunt communiceert met Azure Active Directory (Azure AD):

```
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token
```

In bijna alle OAuth en OpenID Connect-stromen zijn de vier partijen die betrokken zijn bij de exchange:

![OAuth 2.0-rollen](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* De **autorisatieserver** is het Azure AD-eindpunt. Veilig verwerkt alles met betrekking tot gebruikersinformatie en -toegang. Ook verwerkt deze de vertrouwensrelaties tussen de partijen in een stroom. Het is verantwoordelijk voor het controleren van de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en uitgeven van tokens. Het is ook wel bekend als de id-provider.

* De **resource-eigenaar** is doorgaans de eindgebruiker. Is de partij die eigenaar is van de gegevens en het vermogen om toe te staan van derden voor toegang tot die gegevens of bron heeft.

* De **OAuth-client** is uw app. Die wordt geïdentificeerd door de toepassings-ID. Dit is meestal de partij die eindgebruikers interactie met hebben. Ook vraagt deze tokens uit de autorisatie-server. De resource-eigenaar moet de client-machtiging voor toegang tot de resource te verlenen.

* De **bronserver** is waarin de resource of de gegevens zich bevindt. Deze vertrouwt de autorisatieserver veilig verifiëren en autoriseren van de OAuth-client. Access-bearer-tokens worden ook gebruikt om ervoor te zorgen dat toegang tot een resource kan worden verleend.

## <a name="policies-and-user-flows"></a>Beleid en de gebruiker stromen
Azure AD B2C-beleid zijn weliswaar, de belangrijkste functies van de service. Azure AD B2C breidt de standaardprotocollen OAuth 2.0 en OpenID Connect door de introductie van beleid. Azure AD B2C om uit te voeren veel meer dan een eenvoudige verificatie en autorisatie kunt. 

Instellen om u te helpen de meest algemene taken voor identiteit, bevat vooraf gedefinieerde, configureerbare beleidsregels met de naam van de Azure AD B2C-portal **gebruikersstromen**. Gebruikersstromen beschrijven consumentervaringen van identiteit, met inbegrip van gebruikersregistratie, aanmelding, volledig en profiel bewerken. Gebruikersstromen kunnen worden gedefinieerd in een gebruikersinterface met beheerdersrechten. Ze kunnen worden uitgevoerd met behulp van een speciale queryparameter in HTTP-aanvragen voor verificatie. 

Beleid en de gebruikersstromen kunnen niet standard-functies van OAuth 2.0 en OpenID Connect, dus moet u rekening houden met de tijd om te begrijpen. Zie voor meer informatie de [Naslaggids voor Azure AD B2C gebruiker stroom](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokens
De Azure AD B2C-implementatie van OAuth 2.0 en OpenID Connect maakt uitgebreid gebruik van bearer-tokens, met inbegrip van bearer-tokens die worden weergegeven als JSON-webtokens (JWTs). Een bearer-token is een lichtgewicht beveiligingstoken die de "bearer" toegang tot een beveiligde bron verleent.

De houder is een partij die het token kan opleveren. Azure AD een partij moet eerst worden geverifieerd voordat het bearer-token kan ontvangen. Maar als de vereiste stappen zijn niet in gebruik voor het beveiligen van de token in overdracht en opslag, kan worden onderschept en die worden gebruikt door een onbedoelde partij.

Sommige beveiligingstokens ingebouwde mechanismen die voorkomen dat onbevoegden gebruiken ze hebben, maar bearer-tokens geen dit mechanisme. Zij moeten worden overgebracht in een beveiligd kanaal, zoals een transport layer security (HTTPS). 

Als een bearer-token buiten een beveiligd kanaal wordt verzonden, kunnen een schadelijke partij een man-in-the-middle-aanval kunt gebruiken om de token verkrijgen en gebruiken om ongeoorloofde toegang verlenen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer bearer-tokens worden opgeslagen of in de cache voor later gebruik opgeslagen. Altijd voor zorgen dat uw app worden verzonden en bearer-tokens worden opgeslagen op een veilige manier.

Zie voor aanvullende bearer token beveiligingsoverwegingen [RFC 6750 sectie 5](https://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende typen tokens die worden gebruikt in Azure AD B2C zijn beschikbaar in [de naslaginformatie over voor Azure AD-tokens](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protocollen
Wanneer u klaar bent om de aanvragen van een voorbeeld bekijken, kunt u beginnen met een van de volgende zelfstudies. Elke komt overeen met een bepaalde verificatiescenario. Als u hulp te bepalen welke stroom is geschikt voor u nodig hebt, Bekijk [de typen apps die u bouwen kunt met behulp van Azure AD B2C](active-directory-b2c-apps.md).

* [Mobiele en systeemeigen toepassingen kunt maken met behulp van OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
* [Web-apps bouwen met behulp van OpenID Connect](active-directory-b2c-reference-oidc.md)
* [Bouw apps van één pagina met behulp van de impliciete flow OAuth 2.0](active-directory-b2c-reference-spa.md)

