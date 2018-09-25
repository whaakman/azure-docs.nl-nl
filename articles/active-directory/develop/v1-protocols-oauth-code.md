---
title: De OAuth 2.0-autorisatiecodestroom inzicht in Azure AD
description: Dit artikel wordt beschreven hoe u toegang tot webtoepassingen en web-API's in uw tenant met behulp van Azure Active Directory en OAuth 2.0 autoriseren met HTTP-berichten.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: bd9d3a677d9fea54331200258d4b9b8e07a54312
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956894"
---
# <a name="authorize-access-to-azure-active-directory-web-applications-using-the-oauth-20-code-grant-flow"></a>Toegang verlenen aan webtoepassingen voor Azure Active Directory met behulp van de OAuth 2.0-stroom voor het verlenen van code

Azure Active Directory (Azure AD) maakt gebruik van OAuth 2.0 waarmee u kunt toegang verlenen aan webtoepassingen en web-API's in uw Azure AD-tenant. Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een van onze [open source-bibliotheken](active-directory-authentication-libraries.md).

De OAuth 2.0-autorisatiecodestroom wordt beschreven in [sectie 4.1 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749#section-4.1). Het wordt gebruikt voor verificatie en autorisatie in de meeste toepassingstypen, met inbegrip van web-apps en systeemeigen geïnstalleerde apps.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## <a name="oauth-20-authorization-flow"></a>Stroom voor OAuth 2.0-autorisatie

Op hoog niveau, de volledige autorisatiestroom voor een toepassing een en ander er zo uit:

![Stroom voor OAuth-autorisatiecode](./media/v1-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## <a name="request-an-authorization-code"></a>Aanvragen van een autorisatiecode

De autorisatiecodestroom begint met de client zodat de gebruiker de `/authorize` eindpunt. In deze aanvraag geeft de client de machtigingen die nodig zijn om te verkrijgen van de gebruiker. U kunt het OAuth 2.0-autorisatie-eindpunt voor uw tenant krijgen door het selecteren van **App-registraties > eindpunten** in Azure portal.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%3A12345
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |Vereist |De toepassings-ID die aan uw app wordt toegewezen wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de Azure Portal. Klik op **Azure Active Directory** in de zijbalk services, klikt u op **App-registraties**, en kies de toepassing. |
| response_type |Vereist |Moet bevatten `code` voor de autorisatiecodestroom. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. Voor mobiele en systeemeigen apps, moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Optioneel |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app. Kan `query`, `fragment`, of `form_post`. `query` biedt de code als een queryreeks-parameter op uw omleidings-URI. Als u een ID-token met behulp van de impliciete stroom aanvragen, u niet gebruiken `query` zoals opgegeven in de [OpenID spec](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations). Als u alleen de code aanvragen, kunt u `query`, `fragment`, of `form_post`. `form_post` voert een bericht met de code op uw omleidings-URI. De standaardwaarde is `query` voor een codestroom.  |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [cross-site-aanvraag kunnen worden vervalst aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| Bron | Aanbevolen |De App-ID-URI van de doel-web-API (beveiligde resource). U kunt de URI van de App-ID vinden in de Azure Portal op **Azure Active Directory**, klikt u op **toepassingsregistraties**, van de toepassing openen **instellingen** pagina en klik vervolgens op  **Eigenschappen van**. Mogelijk ook een externe bron, zoals `https://graph.microsoft.com`. Dit is vereist in een van de autorisatie- of token aanvragen. Om ervoor te zorgen minder verificatie plaats vragen deze in de autorisatieaanvraag om ervoor te zorgen toestemming wordt ontvangen van de gebruiker. |
| scope | **genegeerd** | Voor apps in Azure AD v1, scopes statisch moeten worden geconfigureerd in de Azure-Portal onder de toepassingen **instellingen**, **vereiste machtigingen**. |
| prompt |Optioneel |Aangeven welk type tussenkomst van de gebruiker die is vereist.<p> Geldige waarden zijn: <p> *aanmelding*: de gebruiker moet worden gevraagd om u te verifiëren. <p> *select_account*: de gebruiker wordt gevraagd om te selecteren van een account dat eenmalige aanmelding op wordt onderbroken. De gebruiker kan Selecteer een bestaand account aangemeld, zijn referenties invoeren voor een account onthouden of wilt gebruiken een ander account kan worden overgeslagen. <p> *toestemming geven*: gebruikerstoestemming heeft gekregen, maar moet worden bijgewerkt. De gebruiker moet worden gevraagd om in te stemmen. <p> *admin_consent*: een beheerder moet worden gevraagd om in te stemmen namens alle gebruikers in hun organisatie |
| login_hint |Optioneel |Kan worden gebruikt om het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u hun gebruikersnaam tevoren weten. Vaak apps Gebruik deze parameter tijdens verificatie wordt uitgevoerd, de gebruikersnaam die al worden geëxtraheerd uit een vorige aanmelden met behulp van de `preferred_username` claim. |
| domain_hint |Optioneel |Biedt een aanwijzing over de tenant of het aanmeldingsdomein waarmee de gebruiker moet zich. De waarde van de domain_hint is een geregistreerde domein voor de tenant. Als de tenant met een on-premises directory is gefedereerd, is AAD wordt omgeleid naar de opgegeven tenant federation-server. |
| code_challenge_method | Aanbevolen    | De methode die wordt gebruikt om te coderen de `code_verifier` voor de `code_challenge` parameter. Een van `plain` of `S256`. Als uitgesloten, `code_challenge` wordt ervan uitgegaan dat als tekst zonder opmaak als `code_challenge` is opgenomen. Azure AAD v1.0 ondersteunt zowel `plain` en `S256`. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| code_challenge        | Aanbevolen    | Gebruikt voor het beveiligen van code autorisatietoekenningen via Proof-sleutel voor Code Exchange (PKCE) van een systeemeigen of een openbare client. Vereist als `code_challenge_method` is opgenomen. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

> [!NOTE]
> Als de gebruiker deel van een organisatie uitmaakt, kan een beheerder van de organisatie toestemming geven of weigeren namens de gebruiker of toestaan dat de gebruiker om toestemming. De gebruiker krijgt de mogelijkheid om in te stemmen alleen als de beheerder toestaat.
>
>

Op dit moment wordt de gebruiker gevraagd zijn referenties invoeren en instemmen met de machtigingen die zijn aangevraagd door de app in Azure Portal. Zodra de gebruiker verifieert en toestemming verleent, Azure AD stuurt een antwoord naar uw app op de `redirect_uri` adres in de aanvraag door de code.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde respons kan er als volgt:

```
GET  HTTP/1.1 302 Found
Location: http://localhost:12345/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschrijving |
| --- | --- |
| admin_consent |De waarde is True als een beheerder ingestemd met een toestemmingsprompt van de aanvraag. |
| Code |De autorisatiecode die de toepassing heeft aangevraagd. De toepassing kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. |
| session_state |Een unieke waarde die aangeeft van de sessie van de huidige gebruiker. Deze waarde is van een GUID, maar moet worden behandeld als een onduidelijke waarde die wordt doorgegeven zonder onderzoek. |
| state |Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. Het is raadzaam voor de toepassing om te controleren dat de provincie-waarden in de aanvraag en respons identiek zijn voordat u het antwoord. Hiermee kunt u detecteren [Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12) op basis van de client. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de toepassing deze op de juiste wijze kan verwerken.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een foutwaarde code gedefinieerd in de sectie 5.2 van de [OAuth 2.0 machtiging Framework](http://tools.ietf.org/html/rfc6749). De volgende tabel beschrijft de foutcodes die Azure AD als resultaat geeft. |
| error_description |Een gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld als geschikt voor eindgebruikers. |
| state |De statuswaarde is een willekeurig gegenereerde niet opnieuw gebruikt-waarde die wordt verzonden in de aanvraag en geretourneerd in het antwoord om te voorkomen dat de aanvraag voor cross-site kunnen worden vervalst (CSRF) aanvallen. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor endpoint-verificatiefouten
De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout in, zoals een ontbrekende vereiste parameter. |Los en verzend de aanvraag opnieuw. Dit is een fout, ontwikkeling en is meestal aangetroffen tijdens de eerste test. |
| unauthorized_client |De clienttoepassing is niet toegestaan om aan te vragen van een autorisatiecode. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| ACCESS_DENIED |Resource-eigenaar toestemming geweigerd |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet, tenzij de gebruiker toestemming heeft melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoord van het type in de aanvraag. |Los en verzend de aanvraag opnieuw. Dit is een fout, ontwikkeling en is meestal aangetroffen tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk bezet en kan de aanvraag te verwerken. |De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. |Dit geeft aan dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>De autorisatiecode gebruiken om aan te vragen van een toegangstoken
Nu dat u een autorisatiecode hebt verkregen en gemachtigd door de gebruiker, kun je de code voor een toegangstoken uit aan de gewenste resource, door te sturen van een POST-aanvraag naar de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%3A12345
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |Vereist |De toepassings-Id die aan uw app wordt toegewezen wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in Azure portal. De toepassings-Id wordt weergegeven in de instellingen van de app-registratie. |
| grant_type |Vereist |Moet `authorization_code` voor de autorisatiecodestroom. |
| Code |Vereist |De `authorization_code` die u in de vorige sectie hebt verkregen |
| redirect_uri |Vereist |Dezelfde `redirect_uri` waarde die is gebruikt voor het verkrijgen van de `authorization_code`. |
| client_secret |vereist voor web-apps is niet toegestaan voor openbare clients |Het toepassingsgeheim die u hebt gemaakt in de Azure-Portal voor uw app bij **sleutels**. Het kan niet worden gebruikt in een systeemeigen app (openbare client), omdat client_secrets op betrouwbare wijze kunnen niet worden opgeslagen op apparaten. Dit is vereist voor de web-apps en web-API's (alle vertrouwelijke clients), waarvoor de mogelijkheid om op te slaan de `client_secret` veilig op de server. De waarde voor client_secret moet URL gecodeerd voordat het wordt verzonden. |
| Bron | Aanbevolen |De App-ID-URI van de doel-web-API (beveiligde resource). U kunt de URI van de App-ID vinden in de Azure Portal op **Azure Active Directory**, klikt u op **toepassingsregistraties**, van de toepassing openen **instellingen** pagina en klik vervolgens op  **Eigenschappen van**. Mogelijk ook een externe bron, zoals `https://graph.microsoft.com`. Dit is vereist in een van de autorisatie- of token aanvragen. Om ervoor te zorgen minder verificatie plaats vragen deze in de autorisatieaanvraag om ervoor te zorgen toestemming wordt ontvangen van de gebruiker. Als in de autorisatieaanvraag en de tokenaanvraag, de resource' parameters moeten overeenkomen. | 
| code_verifier | Optioneel | De dezelfde code_verifier die is gebruikt voor het verkrijgen van de authorization_code. Vereist als PKCE is gebruikt in de autorisatieaanvraag voor het verlenen van code. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636)   |

U kunt de URI van de App-ID vinden in de Azure Portal op **Azure Active Directory**, klikt u op **toepassingsregistraties**, van de toepassing openen **instellingen** pagina en klik vervolgens op  **Eigenschappen van**.

### <a name="successful-response"></a>Geslaagde reactie
Azure AD-retourneert een [toegangstoken](access-tokens.md) na een geslaagde respons. Als u wilt netwerkaanroepen vanuit de clienttoepassing en hun bijbehorende Latentie minimaliseren, de clienttoepassing moet toegangstokens opslaan in cache voor de levensduur van het token dat is opgegeven in de OAuth 2.0-antwoord. Om te bepalen van de levensduur van tokens, gebruiken de `expires_in` of `expires_on` parameterwaarden.

Als een web-API-resource retourneert een `invalid_token` foutcode, dit kan duiden dat de resource heeft vastgesteld dat het token is verlopen. Als de client en de resource clock-tijd zijn verschillende (ook wel een 'tijdverschil'), is de resource overwegen het token verloopt voordat het token is verwijderd uit de clientcache. Als dit het geval is, schakelt u het token uit de cache, zelfs als deze nog steeds binnen hun berekende levensduur.

Een geslaagde respons kan er als volgt:

```
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parameter | Beschrijving |
| --- | --- |
| access_token |De aangevraagde [toegangstoken](access-tokens.md) als een ondertekende JSON Web Token (JWT). De app kan dit token gebruikt voor verificatie bij de beveiligde resource, zoals een web-API. |
| token_type |Geeft aan dat de waarde van het token. Het enige type die ondersteuning biedt voor Azure AD is Bearer. Zie voor meer informatie over het Bearer-tokens [Framework voor OAuth 2.0-autorisatie: Bearer Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De App-ID-URI van de web-API (beveiligde resource). |
| scope |Imitatie machtigingen verleend aan de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt aanvullende waarden registreren in Azure AD. |
| refresh_token |Een vernieuwingstoken OAuth 2.0. De app kunt dit token gebruiken om aanvullende toegangstokens verkrijgen nadat het huidige toegangstoken is verlopen. Vernieuwen van tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. |
| id_token |Een niet-ondertekende JSON Web Token (JWT) die een [ID-token](id-tokens.md). De app kan base64Url decoderen de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. |

Zie voor meer informatie over JSON-webtokens de [JWT IETF concept-specificatie](http://go.microsoft.com/fwlink/?LinkId=392344).   Voor meer informatie over `id_tokens`, Zie de [v1.0 OpenID Connect-stroom](v1-protocols-openid-connect-code.md).

### <a name="error-response"></a>Foutbericht
De eindpunt-fouten van token-uitgifte zijn HTTP-foutcodes, omdat het eindpunt van de token-uitgifte is door de client rechtstreeks aanroept. Naast de HTTP-statuscode, het eindpunt van de Azure AD-token-uitgifte ook een JSON-document met objecten die worden beschreven van de volgende fout geretourneerd.

Een voorbeeld-foutbericht kan er als volgt:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst van de STS-specifieke foutcodes die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens voor onderdelen helpen. |

#### <a name="http-status-codes"></a>HTTP-statuscodes
De volgende tabel bevat de HTTP-statuscodes die het token-uitgifte-eindpunt retourneert. In sommige gevallen wordt de foutcode is voldoende om te beschrijven van het antwoord, maar als er fouten zijn, moet u de bijbehorende JSON-document parseren en de foutcode te onderzoeken.

| HTTP-Code | Beschrijving |
| --- | --- |
| 400 |Standaard-HTTP-code. In de meeste gevallen gebruikt en wordt meestal veroorzaakt door een onjuist gevormde aanvraag. Los en verzend de aanvraag opnieuw. |
| 401 |Verificatie is mislukt. De aanvraag, bijvoorbeeld de waarde voor client_secret-parameter ontbreekt. |
| 403 |Autorisatie is mislukt. Bijvoorbeeld, is de gebruiker niet gemachtigd voor toegang tot de resource. |
| 500 |Er is een interne fout opgetreden bij de service. De aanvraag opnieuw. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt-fouten
| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout in, zoals een ontbrekende vereiste parameter. |Los en verzend de aanvraag opnieuw |
| invalid_grant |De autorisatiecode is ongeldig of is verlopen. |Probeer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client |De geverifieerde client is niet gemachtigd voor het gebruik van deze toekenningstype autorisatie. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| invalid_client |Clientverificatie is mislukt. |De referenties van de client zijn niet geldig. De beheerder van de toepassing om op te lossen, worden de referenties bijgewerkt. |
| unsupported_grant_type |De autorisatie-server biedt geen ondersteuning voor het toekenningstype autorisatie. |Wijzig het type verlenen in de aanvraag. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en worden gedetecteerd tijdens de eerste test. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. |Dit geeft aan dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| interaction_required |De aanvraag gebruikersinteractie is vereist. Bijvoorbeeld, is een aanvullende stap vereist. | In plaats van een niet-interactieve aanvraag, probeer het opnieuw met een interactieve autorisatieaanvraag voor dezelfde resource. |
| temporarily_unavailable |De server is tijdelijk bezet en kan de aanvraag te verwerken. |De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |

## <a name="use-the-access-token-to-access-the-resource"></a>Gebruik het toegangstoken voor toegang tot de resource
Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen voor Web-API's, door te nemen in de `Authorization` header. De [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) specificatie wordt uitgelegd hoe u toegang tot beveiligde bronnen met bearer-tokens in HTTP-aanvragen.

### <a name="sample-request"></a>Voorbeeld van een aanvraag
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Foutbericht
Beveiligde bronnen die HTTP-statuscodes voor RFC 6750 probleem implementeren. Als de aanvraag bevat geen verificatiereferenties of het token ontbreekt, wordt het antwoord bevat een `WWW-Authenticate` header. Wanneer een aanvraag mislukt, wordt de resource-server reageert met de HTTP-statuscode en een foutcode.

Hier volgt een voorbeeld van een mislukte reactie wanneer de clientaanvraag bevat geen het bearer-token:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Foutparameters
| Parameter | Beschrijving |
| --- | --- |
| authorization_uri |De URI (fysieke eindpunt) van de autorisatie-server. Deze waarde wordt ook gebruikt als een lookup-sleutel voor meer informatie over de server van een detectie-eindpunt. <p><p> De client moet worden gevalideerd dat de autorisatie-server vertrouwd wordt. Wanneer de resource is beveiligd door Azure AD, is het voldoende om te controleren dat de URL met begint https://login.microsoftonline.com of een andere hostnaam die ondersteuning biedt voor Azure AD. Een tenant-specifieke resource moet altijd de autorisatie-URI voor een tenant-specifieke retourneren. |
| error |Een foutwaarde code gedefinieerd in de sectie 5.2 van de [OAuth 2.0 machtiging Framework](http://tools.ietf.org/html/rfc6749). |
| error_description |Een gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld als geschikt voor eindgebruikers. |
| resource_id |Retourneert de unieke id van de resource. De clienttoepassing kan deze id gebruiken als de waarde van de `resource` parameter na het aanvragen van een token voor de resource. <p><p> Het is belangrijk voor de clienttoepassing om te controleren of deze waarde, anders een schadelijke service kan mogelijk veroorzaken een **kan leiden tot misbruik van bevoegdheden** aanval <p><p> De aanbevolen strategie voor het voorkomen van een aanval is om te controleren of de `resource_id` overeenkomt met de basis van de web-API-URL die wordt geopend. Bijvoorbeeld, als https://service.contoso.com/data wordt geopend, de `resource_id` htttps://service.contoso.com/ kan zijn. De clienttoepassing moet afwijzen een `resource_id` die begint niet met de basis-URL, tenzij er een betrouwbare alternatieve manier om te controleren of de id. |

#### <a name="bearer-scheme-error-codes"></a>Foutcodes voor Bearer-schema
De specificatie RFC 6750 definieert de volgende fouten voor resources die gebruikmaken van de WWW-Authenticate-header en Bearer-schema in het antwoord.

| HTTP-statuscode | Foutcode | Beschrijving | Clientactie |
| --- | --- | --- | --- |
| 400 |invalid_request |De aanvraag is niet grammaticaal correct. Het kan bijvoorbeeld worden een parameter ontbreekt of met behulp van dezelfde parameter twee keer. |Los de fout en probeer de aanvraag. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en bij de eerste test worden gedetecteerd. |
| 401 |invalid_token |Het toegangstoken ontbreekt, is ongeldig of is ingetrokken. De waarde van de parameter error_description biedt aanvullende informatie. |Een nieuw token aanvragen bij de autorisatieserver. Als het nieuwe token is mislukt, heeft een onverwachte fout opgetreden. Een foutbericht weergegeven voor de gebruiker en probeer het opnieuw na de willekeurige vertraging verzenden |
| 403 |insufficient_scope |Het toegangstoken bevat niet de imitatie-machtigingen vereist voor toegang tot de resource. |Een nieuwe autorisatieaanvraag verzenden naar het autorisatie-eindpunt. Als het antwoord de bereikparameter bevat, gebruikt u de waarde voor het bereik in de aanvraag voor de resource. |
| 403 |insufficient_access |Het onderwerp van het token beschikt niet over de machtigingen die nodig zijn voor toegang tot de resource. |De gebruiker naar een ander account gebruiken of om aan te vragen van machtigingen voor de opgegeven resource gevraagd. |

## <a name="refreshing-the-access-tokens"></a>De toegangstokens te vernieuwen

Toegangstokens tijdelijke zijn en moeten worden vernieuwd nadat ze zijn verlopen om door te gaan met het openen van bronnen. U kunt vernieuwen de `access_token` door het indienen van een andere `POST` aanvragen om te de `/token` eindpunt, maar op dit moment biedt de `refresh_token` in plaats van de `code`.

Vernieuwen van tokens geen opgegeven levensduur. De levensduur van vernieuwingstokens zijn meestal relatief lange. Echter, in sommige gevallen, vernieuwingstokens verlopen, worden ingetrokken of niet over voldoende bevoegdheden voor de gewenste actie. Uw toepassing moet verwachten en fouten die zijn geretourneerd door het eindpunt voor token-uitgifte correct verwerken.

Wanneer u een antwoord met een token vernieuwen-fout ontvangt, de huidige vernieuwingstoken negeren en aanvragen van een nieuwe autorisatiecode of toegangstoken. In het bijzonder wanneer met behulp van een vernieuwing token in de stroom-autorisatiecode verlenen als er een antwoord met de `interaction_required` of `invalid_grant` foutcodes, het vernieuwingstoken negeren en een nieuwe autorisatiecode aanvragen.

Een voorbeeld van een aanvraag naar de **tenantspecifieke** eindpunt (u kunt ook de **algemene** eindpunt) om een nieuwe toegang te krijgen met behulp van een vernieuwingstoken token er als volgt uitzien:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde respons token ziet er als:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parameter | Beschrijving |
| --- | --- |
| token_type |Het type token. De enige ondersteunde waarde is **bearer**. |
| expires_in |De resterende levensduur van het token in seconden. Een typische waarde is 3600 (1 uur). |
| expires_on |De datum en tijd waarop het token is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. |
| Bron |De beveiligde resource die het toegangstoken kan worden gebruikt voor toegang tot. |
| scope |Imitatie machtigingen verleend aan de toepassing native client. Standaard de machtiging is **user_impersonation**. De eigenaar van de doelresource kunt alternatieve waarden registreren in Azure AD. |
| access_token |Het nieuwe toegangstoken die is aangevraagd. |
| refresh_token |Een nieuwe OAuth 2.0-refresh_token die kunnen worden gebruikt om nieuwe toegangstokens wanneer het certificaat in het antwoord is verlopen. |

### <a name="error-response"></a>Foutbericht
Een voorbeeld-foutbericht kan er als volgt:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant. You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst van de STS-specifieke foutcodes die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens voor onderdelen helpen. |

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).
