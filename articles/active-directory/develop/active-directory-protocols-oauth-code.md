---
title: De OAuth 2.0-autorisatiecodestroom inzicht in Azure AD
description: Dit artikel wordt beschreven hoe u met HTTP-berichten toestaan van toegang tot webtoepassingen en web-API's in uw tenant met behulp van Azure Active Directory en OAuth 2.0.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 77df32710f17f8c5b749c39af9f6c64f0cc0b376
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure Active Directory
Azure Active Directory (Azure AD) maakt gebruik van OAuth 2.0 waarmee u toegang verlenen aan webtoepassingen en web-API's in uw Azure AD-tenant. Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een van onze open source-bibliotheken.

De OAuth 2.0-autorisatiecodestroom wordt beschreven in [sectie 4.1 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749#section-4.1). Het wordt gebruikt voor verificatie en autorisatie in de meeste toepassingstypen, met inbegrip van web-apps en systeemeigen apps hebben geïnstalleerd.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## <a name="oauth-20-authorization-flow"></a>Stroom van OAuth 2.0-autorisatie
Op een hoog niveau ziet de volledige autorisatie-stroom voor een toepassing er nogal zo:

![OAuth autorisatiecode stroom](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## <a name="request-an-authorization-code"></a>Aanvraag een autorisatiecode
De autorisatiecodestroom begint met de client voor het routeren van de gebruiker de `/authorize` eindpunt. De client wijst de machtigingen die te verkrijgen van de gebruiker moet op deze aanvraag. U kunt het OAuth 2.0-eindpunt voor uw tenant ophalen door het selecteren van **App registraties > eindpunten** in de Azure Portal.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |Vereist |De toepassings-Id toegewezen aan uw app bij de registratie met Azure AD. U kunt dit vinden in de Azure Portal. Klik op **Active Directory**, klikt u op de directory, kies de toepassing en klik op **configureren** |
| response_type |Vereist |Moet bevatten `code` voor de autorisatiecodestroom. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app.  Er moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen.  Voor mobiele en systeemeigen apps, moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query` of `form_post`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [voorkomen van aanvraagvervalsing op meerdere sites aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| resource |optioneel |De App ID URI van de web-API (beveiligde resource). Klik op de URI van de App-ID van de web-API, informatie in de Azure Portal **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |
| prompt |optioneel |Geef het type van de interactie van de gebruiker die is vereist.<p> Geldige waarden zijn: <p> *aanmelding*: de gebruiker moet worden gevraagd om te verifiëren. <p> *toestemming*: gebruiker toestemming heeft gekregen, maar moet worden bijgewerkt. De gebruiker moet worden gevraagd om toestemming. <p> *admin_consent*: een beheerder moet worden gevraagd om toestemming namens alle gebruikers in hun organisatie |
| login_hint |optioneel |Kan worden gebruikt voor het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u hun gebruikersnaam tevoren weten.  Vaak apps Gebruik deze parameter tijdens verificatie wordt uitgevoerd, de gebruikersnaam die al worden opgehaald uit een eerdere aanmelden met de `preferred_username` claim. |
| domain_hint |optioneel |Biedt een aanwijzing over de tenant of het domein dat de gebruiker gebruiken moet om aan te melden. De waarde van de domain_hint is een geregistreerd domein voor de tenant. Als de tenant aan een lokale directory is gefedereerd, is AAD wordt omgeleid naar de opgegeven tenant federation-server. |

> [!NOTE]
> Als de gebruiker deel van een organisatie uitmaakt, kan een beheerder van de organisatie toestemming geven of weigeren namens de gebruiker of toestaan van de gebruiker om toestemming. De gebruiker krijgt de mogelijkheid om toestemming alleen wanneer de beheerder toestaat.
>
>

Op dit punt wordt de gebruiker wordt gevraagd om hun referenties invoeren en instemming met de machtigingen die zijn aangegeven in de `scope` queryparameter. Als de gebruiker wordt geverifieerd en toestemming verleent, Azure AD stuurt een antwoord naar uw app op de `redirect_uri` adres in uw aanvraag.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie kan er als volgt uitzien:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschrijving |
| --- | --- |
| admin_consent |De waarde is waar als een beheerder wil een aanvraag-instemmingsprompt. |
| code |De autorisatiecode die de toepassing aangevraagd. De toepassing kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. |
| session_state |Een unieke waarde die de huidige gebruikerssessie identificeert. Deze waarde is een GUID, maar moet worden behandeld als een ondoorzichtige waarde die wordt doorgegeven zonder onderzoek. |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. Het is raadzaam voor de toepassing om te controleren dat de statuswaarden in de aanvraag en antwoord identiek zijn voordat u het antwoord. Hiermee kunt u detecteren [Cross-Site aanvragen kunnen worden vervalst (CSRF) aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12) op basis van de client. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de toepassing ze op de juiste wijze kan verwerken.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een foutwaarde code gedefinieerd in de sectie 5.2 van de [OAuth 2.0 autorisatie Framework](http://tools.ietf.org/html/rfc6749). De volgende tabel beschrijft de foutcodes die Azure AD als resultaat geeft. |
| error_description |Een gedetailleerdere beschrijving van de fout. Dit bericht is niet bedoeld als gebruiksvriendelijke door eindgebruikers. |
| state |De waarde van de status is een willekeurig gegenereerde niet opnieuw gebruikt waarde dat wordt verzonden in de aanvraag en geretourneerd in het antwoord om aanvraagvervalsing op meerdere sites (CSRF) aanvallen te voorkomen. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor autorisatie eindpunt fouten
De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw. Dit is een fout voor ontwikkeling en meestal wordt onderschept tijdens de eerste test. |
| unauthorized_client |De clienttoepassing is niet toegestaan om aan te vragen een autorisatiecode. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| ACCESS_DENIED |Resource-eigenaar toestemming geweigerd |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoordtype in de aanvraag. |Herstel en verzend de aanvraag opnieuw. Dit is een fout voor ontwikkeling en meestal wordt onderschept tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw proberen. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd doordat een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Dit betekent dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>De autorisatiecode gebruiken om aan te vragen van een toegangstoken
Nu dat u hebt aangeschaft een autorisatiecode en gemachtigd door de gebruiker, kunt u de code voor een toegangstoken op de gewenste resource inwisselen door te sturen een POST-aanvraag naar de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |Vereist |De toepassings-Id toegewezen aan uw app bij de registratie met Azure AD. U kunt dit vinden in de Azure portal. De toepassings-Id wordt weergegeven in de instellingen van de registratie van de app.  |
| grant_type |Vereist |Moet `authorization_code` voor de autorisatiecodestroom. |
| code |Vereist |De `authorization_code` die u in de vorige sectie hebt verkregen |
| redirect_uri |Vereist |Dezelfde `redirect_uri` waarde die is gebruikt voor het verkrijgen van de `authorization_code`. |
| client_secret |vereist voor web-apps |De toepassingsgeheim die u in de portal van de registratie van de app voor uw app hebt gemaakt.  Deze mag niet worden gebruikt in een eigen app omdat client_secrets betrouwbaar kunnen niet worden opgeslagen op apparaten.  Het is vereist voor de web-apps en web-API's die u de mogelijkheid hebt voor het opslaan van de `client_secret` veilig op de server. |
| resource |vereist als het opgegeven in de autorisatieaanvraag, anders optioneel |De App ID URI van de web-API (beveiligde resource). |

Klik op de App ID URI, informatie in de Azure-beheerportal **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**.

### <a name="successful-response"></a>Geslaagde reactie
Azure AD retourneert een toegangstoken na een geslaagde reactie. Om te beperken netwerk aanroepen vanuit de clienttoepassing en hun bijbehorende latentie, moet de clienttoepassing voor de levensduur van het token dat is opgegeven in het antwoord OAuth 2.0-toegangstokens cache. Om te bepalen van de levensduur van tokens, gebruiken de `expires_in` of `expires_on` parameterwaarden.

Als een web API-resource geeft een `invalid_token` foutcode: dit kan betekenen dat de resource heeft vastgesteld dat het token is verlopen. Als de klok client- en tijden zijn verschillende (bekend als een 'scheeftrekken keer'), is de resource overwegen het token is verlopen voordat het token van de clientcache is uitgeschakeld. Als dit het geval is, schakelt u het token uit de cache, zelfs als deze nog steeds binnen de berekende levensduur.

Een geslaagde reactie kan er als volgt uitzien:

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
| access_token |Het aangevraagde toegangstoken als een ondertekende JSON Web Token (JWT). De app kunt dit token gebruiken om de beveiligde resource, zoals een web-API te verifiëren. |
| token_type |Geeft de waarde van het type token. Het enige type dat ondersteuning biedt voor Azure AD is Bearer. Zie voor meer informatie over Bearer-tokens [OAuth2.0 autorisatie Framework: Bearer-Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| resource |De App ID URI van de web-API (beveiligde resource). |
| Bereik |Imitatie gemachtigd om de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt u aanvullende waarden registreren in Azure AD. |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt dit token gebruiken voor aanvullende toegangstokens verkrijgen nadat het huidige toegangstoken is verlopen.  Vernieuwen van tokens worden lange levensduur hebben en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd te behouden. |
| id_token |Een niet-ondertekende JSON Web Token (JWT). De app kan base64Url decoderen de segmenten van dit token informatie opvragen over de gebruiker die zich aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze niet verstandig deze beveiligingsgrenzen of autorisatie. |

### <a name="jwt-token-claims"></a>JWT-Token Claims
De JWT-token in de waarde van de `id_token` parameter in de volgende claims kan worden gedecodeerd:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Zie voor meer informatie over de JSON-webtokens de [JWT IETF conceptspecificatie](http://go.microsoft.com/fwlink/?LinkId=392344). Lees voor meer informatie over de typen tokens en claims [ondersteund Token en claimtypen](active-directory-token-and-claims.md)

De `id_token` parameter bevat de volgende claimtypen:

| Claimtype | Beschrijving |
| --- | --- |
| AUD |De doelgroep van het token. Wanneer het token wordt verleend aan een clienttoepassing, de doelgroep is de `client_id` van de client. |
| EXP |Verlooptijd. De tijd waarop het token verloopt. Voor het token geldig, de huidige datum en tijd moet kleiner dan of gelijk aan de `exp` waarde. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de geldigheid van het token is verlopen.|
| family_name |Van de gebruiker achternaam of achternaam. De toepassing kan deze waarde weergeven. |
| given_name |De voornaam van de gebruiker. De toepassing kan deze waarde weergeven. |
| IAT |Op tijdstip afgegeven. De tijd waarop de JWT is uitgegeven. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de tijd die het token is uitgegeven. |
| ISS |Identificeert de uitgever van het beveiligingstoken |
| NBF |Niet voor de tijd. De tijd waarop het token van kracht. Voor het token geldig, moet de huidige datum en tijd groter dan of gelijk aan de Nbf-waarde. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de tijd die het token is uitgegeven. |
| OID |Object-id (ID) van het gebruikersobject in Azure AD. |
| Sub |Token onderwerp-id. Dit is een permanente en onveranderbare id voor de gebruiker die het token wordt beschreven. Gebruik deze waarde in de cache logica. |
| TID |Tenant-id (ID) van de Azure AD-tenant die het token heeft uitgegeven. |
| unique_name |Een unieke id voor die kan worden weergegeven voor de gebruiker. Dit is meestal een UPN (user Principal name). |
| UPN |De UPN van de gebruiker. |
| ver |Versie. De versie van de JWT-token, doorgaans 1.0. |

### <a name="error-response"></a>Foutbericht
De uitgifte van tokens eindpunt fouten zijn foutcodes voor HTTP, omdat de client het eindpunt van de uitgifte van tokens rechtstreeks aanroept. Naast de HTTP-statuscode retourneert het eindpunt van de uitgifte van tokens Azure AD ook een JSON-document met objecten die de fout wordt beschreven.

Een voorbeeld-foutmelding kan uitzien:

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
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst met foutcodes STS-specifieke die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens over de onderdelen helpen. |

#### <a name="http-status-codes"></a>HTTP-statuscodes
De volgende tabel bevat de HTTP-statuscodes die het eindpunt van de uitgifte van tokens retourneert. In sommige gevallen kan de foutcode is voldoende om te beschrijven van het antwoord, maar als er fouten zijn, moet u de bijbehorende JSON-document te parseren en bekijk de foutcode.

| HTTP-Code | Beschrijving |
| --- | --- |
| 400 |Standaardcode voor HTTP. In de meeste gevallen gebruikt en wordt meestal veroorzaakt door een onjuist gevormde aanvraag. Herstel en verzend de aanvraag opnieuw. |
| 401 |Verificatie mislukt. De aanvraag is bijvoorbeeld de client_secret-parameter ontbreekt. |
| 403 |Autorisatie is mislukt. Bijvoorbeeld: de gebruiker heeft geen machtiging voor toegang tot de resource. |
| 500 |Een interne fout opgetreden bij de service. De aanvraag opnieuw proberen. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt fouten
| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw |
| invalid_grant |De autorisatiecode is ongeldig of is verlopen. |Probeer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client |De geverifieerde client is niet gemachtigd deze machtiging grant methode gebruiken. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| invalid_client |Clientverificatie is mislukt. |De clientreferenties zijn niet geldig. Als u wilt oplossen, werkt de beheerder van de toepassing de referenties. |
| unsupported_grant_type |De autorisatie-server biedt geen ondersteuning voor de machtiging grant-type. |Het type grant in de aanvraag wijzigen. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en worden gedetecteerd tijdens de eerste test. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Dit betekent dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| interaction_required |De aanvraag vereist gebruikersinteractie. Bijvoorbeeld, is een stap extra authenticatie vereist. | In plaats van een niet-interactieve aanvraag, probeer het opnieuw met een interactieve autorisatieaanvraag voor dezelfde resource. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd doordat een tijdelijke situatie. |

## <a name="use-the-access-token-to-access-the-resource"></a>Gebruik het toegangstoken voor toegang tot de bron
Nu dat u hebt gekregen een `access_token`, kunt u het token in hun aanvragen aan de Web-API's, door te nemen in de `Authorization` header. De [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) specificatie legt uit hoe u toegang tot beveiligde bronnen met bearer-tokens in HTTP-aanvragen.

### <a name="sample-request"></a>Voorbeeld van een aanvraag
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Foutbericht
Beveiligde bronnen en RFC 6750 probleem HTTP-statuscodes implementeren. Als de aanvraag geen verificatiereferenties bevat of het token ontbreekt, wordt het antwoord bevat een `WWW-Authenticate` header. Wanneer een aanvraag is mislukt, wordt de resource-server reageert met de HTTP-statuscode en een foutcode.

Hier volgt een voorbeeld van een mislukte reactie wanneer de aanvraag van de client geen bearer-token:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Foutparameters
| Parameter | Beschrijving |
| --- | --- |
| authorization_uri |De URI (fysieke eindpunt) van de autorisatie-server. Deze waarde wordt ook gebruikt als een zoeksleutel voor meer informatie over de server van een detectie-eindpunt. <p><p> De client moet valideren dat de autorisatie-server vertrouwd wordt. Wanneer de bron wordt beveiligd door Azure AD, is voldoende om te controleren dat de URL begint met https://login.microsoftonline.com of een andere hostnaam die ondersteuning biedt voor Azure AD. Een resource tenantspecifieke moet altijd een verificatie-URI van de tenant-specifieke retourneren. |
| error |Een foutwaarde code gedefinieerd in de sectie 5.2 van de [OAuth 2.0 autorisatie Framework](http://tools.ietf.org/html/rfc6749). |
| error_description |Een gedetailleerdere beschrijving van de fout. Dit bericht is niet bedoeld als gebruiksvriendelijke door eindgebruikers. |
| bron-id |Retourneert de unieke id van de resource. De clienttoepassing deze id kunt gebruiken als de waarde van de `resource` parameter na het aanvragen van een token voor de resource. <p><p> Het is belangrijk voor de clienttoepassing om te controleren of deze waarde, anders een schadelijke service mogelijk om te veroorzaken een **verhoging van bevoegdheden** aanval <p><p> Ter voorkoming van een aanval wordt u aangeraden om te controleren of de `resource_id` overeenkomt met het grondtal van de web-API-URL die wordt geopend. Bijvoorbeeld, als https://service.contoso.com/data wordt geopend, de `resource_id` htttps://service.contoso.com/ kan zijn. De clienttoepassing moet afwijzen een `resource_id` die begint niet met de basis-URL tenzij er een betrouwbare alternatieve manier om te controleren of de id. |

#### <a name="bearer-scheme-error-codes"></a>Foutcodes voor Bearer-schema
De specificatie RFC 6750 definieert de volgende fouten voor resources die gebruikmaken van de WWW-Authenticate-header en Bearer-schema in het antwoord.

| HTTP-statuscode | Foutcode | Beschrijving | Clientactie |
| --- | --- | --- | --- |
| 400 |invalid_request |De aanvraag is niet grammaticaal correct. Bijvoorbeeld, het mogelijk ontbreekt een parameter of met behulp van dezelfde parameter twee keer. |Los de fout en probeer de aanvraag. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en tests van de eerste worden gedetecteerd. |
| 401 |invalid_token |Het toegangstoken ontbreekt, is ongeldig of is ingetrokken. De waarde van de parameter error_description biedt aanvullende informatie. |Een nieuw token aangevraagd van de autorisatie-server. Als het nieuwe token is mislukt, heeft een onverwachte fout opgetreden. Een foutbericht voor de gebruiker en probeer het opnieuw na willekeurige vertraging verzenden. |
| 403 |insufficient_scope |Het toegangstoken bevat niet de imitatie-machtigingen vereist voor toegang tot de resource. |Een nieuwe autorisatieaanvraag verzenden naar het eindpunt voor autorisatie. Als het antwoord de bereikparameter bevat, gebruikt u de bereikwaarde in de aanvraag voor de resource. |
| 403 |insufficient_access |Het onderwerp van het token beschikt niet over de machtigingen die nodig zijn voor toegang tot de bron. |De gebruiker gevraagd naar een ander account of om aan te vragen van machtigingen voor de opgegeven bron. |

## <a name="refreshing-the-access-tokens"></a>De toegangstokens te vernieuwen
Toegangstokens tijdelijke zijn en moeten worden vernieuwd nadat ze zijn verlopen om door te gaan met het openen van bronnen. Vernieuwt u de `access_token` door het indienen van een andere `POST` aanvraag voor de `/token` eindpunt, maar deze tijd bieden de `refresh_token` in plaats van de `code`.

Vernieuwen van tokens hebben geen opgegeven levensduur. De levensduur van het vernieuwen van tokens zijn meestal relatief lange. Echter, in sommige gevallen vernieuwen van tokens verlopen, worden ingetrokken of niet over voldoende bevoegdheden voor de gewenste actie. Uw toepassing moet verwacht en fouten geretourneerd door het eindpunt van de uitgifte van tokens correct verwerkt.

Wanneer u een antwoord met een token fout vernieuwen ontvangt, de huidige vernieuwingstoken negeren en vraag een nieuwe autorisatiecode of toegangstoken. In het bijzonder wanneer met behulp van een vernieuwing token in de stroom Authorization Code Grant als er een antwoord met de `interaction_required` of `invalid_grant` foutcodes, het vernieuwingstoken negeren en vraag een nieuwe autorisatiecode.

Een voorbeeld van een aanvraag naar de **tenantspecifieke** eindpunt (u kunt ook de **algemene** eindpunt) om een nieuwe toegang te krijgen met behulp van een vernieuwingstoken token uitziet:

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
Een geslaagde reactie token, ziet er als:

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
| expires_in |De resterende levensduur van het token in seconden. Een typische waarde is 3600 (één uur). |
| expires_on |De datum en tijd waarop het token verloopt. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de verlooptijd. |
| resource |Geeft de beveiligde bron die het toegangstoken kan worden gebruikt voor toegang. |
| Bereik |Imitatie gemachtigd om de native client-toepassing. Standaard de machtiging is **user_impersonation**. De eigenaar van de doelbron kunt alternatieve waarden registreren in Azure AD. |
| access_token |Het nieuwe toegangstoken die is aangevraagd. |
| refresh_token |Een nieuwe OAuth 2.0-refresh_token die kunnen worden gebruikt om aan te vragen van nieuwe toegangstokens wanneer de structuur in voor deze reactie is verlopen. |

### <a name="error-response"></a>Foutbericht
Een voorbeeld-foutmelding kan uitzien:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
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
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst met foutcodes STS-specifieke die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens over de onderdelen helpen. |

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).
