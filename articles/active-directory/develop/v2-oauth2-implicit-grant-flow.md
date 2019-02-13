---
title: Beveiligde toepassingen met één pagina met behulp van de Azure AD v2.0-impliciete stroom | Microsoft Docs
description: Webtoepassingen voor het bouwen met behulp van Azure AD v2.0-implementatie van de impliciete stroom voor apps van één pagina.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec47e6d52a3aef8533a3d16f0f81693b8f01f3cf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205049"
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protocollen - kuuroorden met behulp van de impliciete stroom

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Met het v2.0-eindpunt, kunt u gebruikers zich in uw apps met één pagina met zowel persoonlijke en werk of school-accounts van Microsoft. Één pagina en andere JavaScript-apps die worden uitgevoerd in een browser gezicht voornamelijk enkele interessante uitdagingen bij het verificatie:

* De beveiligingskenmerken van de van deze apps zijn aanzienlijk verschillen van traditionele server-gebaseerde webtoepassingen.
* Veel autorisatieservers en id-providers bieden geen ondersteuning voor CORS-aanvragen.
* Volledige pagina browser wordt omgeleid weg van de app worden met name Invasief tot de gebruikerservaring.

Voor deze toepassingen (AngularJS, Ember.js, React.js, enzovoort), Azure Active Directory (Azure AD) biedt ondersteuning voor de impliciete toekenning van OAuth 2.0-stroom. De impliciete stroom wordt beschreven in de [OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749#section-4.2). Het belangrijkste voordeel is dat de app om op te halen van tokens van Azure AD zonder uit te voeren van een back-endserver uitwisseling van verificatiegegevens. Hiermee wordt de app te melden bij de gebruiker, sessie en het verkrijgen van tokens, voor andere web-API's in de client JavaScript-code. Er zijn enkele belangrijke beveiligingsoverwegingen rekening moet houden bij het gebruik van de impliciete stroom specifiek ongeveer [client](https://tools.ietf.org/html/rfc6749#section-10.3) en [gebruikersimitatie](https://tools.ietf.org/html/rfc6749#section-10.3).

Als u de impliciete stroom en de Azure AD gebruiken wilt voor verificatie toevoegen aan uw JavaScript-app, raden we u de JavaScript-bibliotheek van open-source [msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js).

Als u liever niet te gebruiken van een bibliotheek in uw app met één pagina en protocolberichten uzelf te sturen, volgt u de algemene stappen hieronder.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en -functies worden ondersteund door het v2.0-eindpunt. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="protocol-diagram"></a>Protocol-diagram

Het volgende diagram toont hoe het gehele impliciete-aanmeldingsstroom uitziet en de volgende secties wordt elke stap in meer detail beschreven.

![OpenId Connect-banen](./media/v2-oauth2-implicit-grant-flow/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>De aanvraag voor aanmelding bij verzenden

In eerste instantie het tekenen van de gebruiker in uw app, kunt u sturen een [OpenID Connect](v2-protocols-oidc.md) autorisatie-aanvraag aan en ontvang een `id_token` van het v2.0-eindpunt.

> [!IMPORTANT]
> Om aan te vragen is een ID-token, dat de app-registratie in de [registratieportal](https://apps.dev.microsoft.com) ze beschikken over de **impliciete stroom toestaan** ingeschakeld voor de webclient. Als deze niet is ingeschakeld, een `unsupported_response` fout geretourneerd: **De opgegeven waarde voor de invoerparameter 'response_type' is niet toegestaan voor deze client. Verwachte waarde is 'code'**

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Als u wilt testen met behulp van de impliciete flow aanmelden, klikt u op <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank"> https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a> Na het aanmelden, moet uw browser worden omgeleid naar `https://localhost/myapp/` met een `id_token` in de adresbalk.
>

| Parameter |  | Description |
| --- | --- | --- |
| `tenant` | vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| `client_id` | vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app is toegewezen. |
| `response_type` | vereist |Moet bevatten `id_token` voor aanmelding OpenID Connect. Het kan ook het response_type bevatten `token`. Met behulp van `token` hier kunnen uw app ontvangt een toegangstoken onmiddellijk uit het geautoriseerde eindpunt zonder te hoeven maken van een tweede aanvraag naar het geautoriseerde eindpunt. Als u de `token` response_type, de `scope` parameter moet een scope die aangeeft welke resource voor het uitgeven van het token voor bevatten. |
| `redirect_uri` | Aanbevolen |De redirect_uri van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. |
| `scope` | vereist |Een door spaties gescheiden lijst van [scopes](v2-permissions-and-consent.md). Voor de OpenID Connect, moet deze het bereik bevatten `openid`, die wordt omgezet in de machtiging 'Aanmelden' in de gebruikersinterface voor toestemming. (Optioneel) u kunt ook om op te nemen de `email` of `profile` bereiken voor het verkrijgen van toegang tot aanvullende gegevens. U kunt ook andere bereiken in deze aanvraag voor het aanvragen van toestemming aan verschillende resources opnemen. |
| `response_mode` | optioneel |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app. De standaardwaarde als de aanvraag een id_token bevat te vragen voor een toegangstoken, maar het fragment. |
| `state` | Aanbevolen |Een waarde die is opgenomen in de aanvraag die wordt ook in het token antwoord geretourneerd. Een tekenreeks van de inhoud die u wenst dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [cross-site-aanvraag kunnen worden vervalst aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| `nonce` | vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die wordt opgenomen in de resulterende id_token als een claim. De app kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. Alleen vereist wanneer een id_token wordt aangevraagd. |
| `prompt` | optioneel |Geeft het type tussenkomst van de gebruiker die is vereist. De enige geldige waarden op dit moment zijn 'aanmelding', 'none', 'select_account', ' toestemming geven '. `prompt=login` wordt de gebruiker te dwingen zijn referenties invoeren voor deze aanvraag, zodat eenmalige aanmelding. `prompt=none` is het tegenovergestelde - dit zorgt ervoor dat de gebruiker niet wordt weergegeven met een interactieve prompt dan ook. Als de aanvraag kan niet op de achtergrond via eenmalige aanmelding worden voltooid, wordt het v2.0-eindpunt een fout geretourneerd. `prompt=select_account` Hiermee verzendt de gebruiker naar een accountkiezer waarin alle accounts die zijn opgeslagen in de sessie wordt weergegeven. `prompt=consent` het dialoogvenster OAuth wordt worden geactiveerd nadat de gebruiker zich heeft aangemeld, waarin de gebruiker wordt om de app-machtigingen te verlenen. |
| `login_hint`  |optioneel |Kan worden gebruikt om te vooraf vullen van het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker, als u bekend bent met hun gebruikersnaam vooraf. Vaak apps Gebruik deze parameter tijdens hernieuwde verificatie de gebruikersnaam die al worden geëxtraheerd uit een vorige aanmelden met behulp van de `preferred_username` claim.|
| `domain_hint` | optioneel |Een van `consumers` of `organizations`. Als opgenomen, wordt overgeslagen tijdens de detectie op basis van een e-mailadres van die gebruiker doorloopt op het v2.0-teken op de pagina, leidt tot een iets meer gestroomlijnde gebruikerservaring. Vaak apps wordt deze parameter gebruikt tijdens hernieuwde verificatie door te extraheren de `tid` claim uit de id_token. Als de `tid` claim waarde `9188040d-6c67-4c5b-b112-36a304b66dad` (de Microsoft-Account consument tenant), moet u `domain_hint=consumers`. Anders kunt u `domain_hint=organizations` tijdens de verificatie opnieuw. |

Op dit moment is de gebruiker gevraagd zijn referenties invoeren en de verificatie voltooien. Het v2.0-eindpunt wordt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Als de gebruiker heeft ingestemd met de **geen** van deze machtigingen de gebruiker akkoord gaan met de vereiste machtigingen wordt u gevraagd. Zie voor meer informatie, [machtigingen en toestemming apps met meerdere tenants](v2-permissions-and-consent.md).

Zodra de gebruiker verifieert en toestemming verleent, het v2.0-eindpunt wordt een antwoord naar uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons met `response_mode=fragment` en `response_type=id_token+token` er als volgt (met regeleinden voor een betere leesbaarheid):

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fuser.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Description |
| --- | --- |
| `access_token` |Opgenomen als `response_type` bevat `token`. Het toegangstoken dat de app is aangevraagd, in dit geval voor de Microsoft Graph. Het toegangstoken niet moet worden gedecodeerd of anders is gecontroleerd, moet dit worden beschouwd als een ondoorzichtige tekenreeks. |
| `token_type` |Opgenomen als `response_type` bevat `token`. Altijd `Bearer`. |
| `expires_in`|Opgenomen als `response_type` bevat `token`. Geeft het aantal seconden dat het token geldig is voor cachedoeleinden. |
| `scope` |Opgenomen als `response_type` bevat `token`. Geeft aan dat de bereik(en) waarvoor de access_token geldig zijn. Omvat mogelijk niet alle van de bereiken die is aangevraagd, als ze niet van toepassing op de gebruiker (in het geval van AAD-alleen bereiken worden aangevraagd zijn als een persoonlijk account wordt gebruikt om aan te melden). |
| `id_token` | Een ondertekende JSON Web Token (JWT). De app kan worden gedecodeerd de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. Zie voor meer informatie over id_tokens, de [ `id_token reference` ](id-tokens.md). <br> **Opmerking:** Alleen opgegeven als `openid` bereik is aangevraagd. |
| `state` |Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |

#### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| `error` |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="validate-the-idtoken"></a>De id_token valideren

Alleen een id_token ontvangen is niet voldoende om te verifiëren van de gebruiker. u moet ook de handtekening van het id_token valideren en controleer of de claims in het token op basis van de vereisten van uw app. Maakt gebruik van het v2.0-eindpunt [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen om te valideren de `id_token` in client-code, maar een gebruikelijk is voor het verzenden van de `id_token` naar een back-endserver en er de validatie uit te voeren. Nadat u de handtekening van het id_token hebt gevalideerd, zijn er enkele claims u moet om te controleren. Zie de [ `id_token` verwijzing](id-tokens.md) voor meer informatie, met inbegrip van [valideren van tokens](id-tokens.md#validating-an-idtoken) en [belangrijke informatie over de rollover van ondertekeningssleutel gebruiken](active-directory-signing-key-rollover.md). Het is raadzaam om gebruik van een bibliotheek voor het parseren en valideren van tokens: Er is ten minste één beschikbaar voor de meeste talen en platforms.

U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

* Ervoor te zorgen dat de gebruiker/organisatie is geregistreerd voor de app.
* Ervoor te zorgen dat de gebruiker heeft juiste autorisatie/bevoegdheden.
* Ervoor te zorgen dat een bepaalde sterkte van de verificatie is opgetreden, zoals meervoudige verificatie.

Nadat u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker en de claims in de id_token gebruiken om informatie over de gebruiker in uw app te verkrijgen. Deze informatie kan worden gebruikt voor het weergeven, records, persoonlijke instellingen, enzovoort.

## <a name="get-access-tokens"></a>Toegangstokens ophalen

Nu dat u hebt de gebruiker aangemeld bij uw app met één pagina, kunt u toegangstokens krijgen voor het aanroepen van web API's die zijn beveiligd door Azure AD, zoals de [Microsoft Graph](https://developer.microsoft.com/graph). Zelfs als u al een token met ontvangen de `token` response_type, kunt u deze methode gebruiken om te verkrijgen van tokens, voor aanvullende bronnen zonder te hoeven omleiden van de gebruiker zich opnieuw aanmelden.

In de normale OpenID Connect/OAuth-stroom, zou u dit doen door die een aanvraag verzendt naar het v2.0 `/token` eindpunt. Het v2.0-eindpunt biedt echter geen CORS-aanvragen, ondersteuning, zodat de AJAX-aanroepen voor het ophalen en vernieuwingstokens valt buiten de vraag. U kunt in plaats daarvan de impliciete stroom in een verborgen iframe gebruiken om op te halen van nieuwe-tokens voor andere web-API's: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

Zie voor meer informatie over de queryparameters in de URL, [het teken in de aanvraag verzenden](#send-the-sign-in-request).

> [!TIP]
> Probeer te kopiëren en plakken van de onderstaande aanvraag in een browsertabblad! (Vergeet niet om te vervangen de `login_hint` waarden met de juiste waarde voor de gebruiker)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2user.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint=your-username`
>

Dankzij de `prompt=none` parameter, dit verzoek op slagen of mislukken onmiddellijk en Ga terug naar uw toepassing. Een geslaagde reactie wordt verzonden naar uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons met `response_mode=fragment` ziet eruit als:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Description |
| --- | --- |
| `access_token` |Opgenomen als `response_type` bevat `token`. Het toegangstoken dat de app is aangevraagd, in dit geval voor de Microsoft Graph. Het toegangstoken niet moet worden gedecodeerd of anders is gecontroleerd, moet dit worden beschouwd als een ondoorzichtige tekenreeks. |
| `token_type` | Altijd `Bearer`. |
| `expires_in` | Geeft het aantal seconden dat het token geldig is voor cachedoeleinden. |
| `scope` | Geeft aan dat de bereik(en) waarvoor de access_token geldig zijn. Omvat mogelijk niet alle van de bereiken die is aangevraagd, als ze niet van toepassing op de gebruiker (in het geval van AAD-alleen bereiken worden aangevraagd zijn als een persoonlijk account wordt gebruikt om aan te melden). |
| `id_token` | Een ondertekende JSON Web Token (JWT). Opgenomen als `response_type` bevat `id_token`. De app kan worden gedecodeerd de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. Zie voor meer informatie over id_tokens, de [ `id_token` verwijzing](id-tokens.md). <br> **Opmerking:** Alleen opgegeven als `openid` bereik is aangevraagd. |
| `state` |Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |


#### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken. In het geval van `prompt=none`, een verwachte fout is:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Description |
| --- | --- |
| `error` |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Als u deze fout in de iframe-aanvraag ontvangt, moet de gebruiker interactief aanmelden weer naar een nieuw token opgehaald. U kunt in dit geval in een manier die zinvol is voor uw toepassing.

## <a name="validating-access-tokens"></a>Toegangstokens valideren

Wanneer u een access_token ontvangt, zorg er dan voor dat valideert de handtekening van het token, evenals de volgende claims. U kunt er ook voor kiezen om aanvullende claims op basis van uw scenario te valideren. 

* **doelgroep** claim, om ervoor te zorgen dat het token is bedoeld om u te krijgen tot uw app
* **verlener** claim, om te controleren of het token aan uw app is uitgegeven door het v2.0-eindpunt
* **niet voor** en **verlooptijd** claims, om te verifiëren dat het token niet is verlopen

Zie voor meer informatie over de claims aanwezig zijn in het toegangstoken, de [toegang tot tokenverwijzing](access-tokens.md)

## <a name="refreshing-tokens"></a>Vernieuwen van tokens

De impliciete toekenning biedt geen vernieuwingstokens. Beide `id_token`s en `access_token`s verloopt na een korte periode, zodat uw app moet worden voorbereid voor het vernieuwen van deze tokens periodiek. Beide typen token vernieuwen, kunt u uitvoeren de dezelfde verborgen iframe-aanvraag in het navolgende met behulp van de `prompt=none` parameter voor het beheren van Azure AD-gedrag. Als u wilt ontvangen van een nieuwe `id_token`, zorg ervoor dat u `response_type=id_token` en `scope=openid`, evenals een `nonce` parameter.

## <a name="send-a-sign-out-request"></a>Een afmelden aanvraag verzenden

De OpenIdConnect `end_session_endpoint` kan uw app een aanvraag verzenden naar het v2.0-eindpunt voor een gebruikerssessie beëindigen en Schakel cookies die zijn ingesteld door het v2.0-eindpunt. Volledig het tekenen van een gebruiker uit een web-App, moet uw app een eigen sessie met de gebruiker beëindigen (meestal met een token cache wissen of verwijderen van cookies) en leid de browser:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parameter |  | Description |
| --- | --- | --- |
| `tenant` |vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| `post_logout_redirect_uri` | Aanbevolen | De URL die de gebruiker moet worden geretourneerd aan na het afmelden is voltooid. Deze waarde moet overeenkomen met een van de omleidings-URI's die zijn geregistreerd voor de toepassing. Als niet is opgenomen, kan de gebruiker wordt een algemeen bericht met het v2.0-eindpunt worden weergegeven. |

## <a name="next-steps"></a>Volgende stappen

* Ga via de [MSAL JS voorbeelden](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) aan de slag coderen.
