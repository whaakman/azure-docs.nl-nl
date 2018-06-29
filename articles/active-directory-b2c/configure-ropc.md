---
title: De resource-eigenaar wachtwoord referentiestroom configureren in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het configureren van de resource-eigenaar wachtwoord referentiestroom in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/24/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 073af4a57d55eb8b2f3608482159b57c7b408f3b
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102362"
---
# <a name="configure-the-resource-owner-password-credentials-flow-in-azure-ad-b2c"></a>De resource-eigenaar wachtwoord referentiestroom configureren in Azure AD B2C

De resource-eigenaar wachtwoord referenties (ROPC)-stroom is een standaard OAuth-authenticatiestroom waar de toepassing, ook wel bekend als de relying party, geldige referenties zoals gebruikersnaam en wachtwoord voor een token ID, toegangstoken en een vernieuwingstoken uitgewisseld. 

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.

In Azure Active Directory (Azure AD) B2C, zijn de volgende opties worden ondersteund:

- **Native Client**: interactie van de gebruiker tijdens de verificatie gebeurt wanneer de code wordt uitgevoerd op een apparaat aan de gebruiker. Het apparaat kan zijn van een mobiele toepassing die wordt uitgevoerd in een eigen besturingssysteem, zoals Android, of in een browser, zoals JavaScript, uitgevoerd.
- **Stroom voor openbare**: alleen gebruikersreferenties die worden verzameld door een toepassing worden verzonden in de API-aanroep. De referenties van de toepassing niet verzonden.
- **Nieuwe claims toe te voegen**: inhoud van de ID-tokens kunnen worden gewijzigd om nieuwe claims toe te voegen. 

De volgende stromen worden niet ondersteund:

- **Server-naar-server**: de computer van de beveiliging identiteit moet een betrouwbare IP-adres van de aanroeper (native client) verzameld als onderdeel van de interactie. Een aanroep van de API-serverzijde wordt alleen IP-adres van de server gebruikt. Als een dynamische drempelwaarde voor mislukte verificaties wordt overschreden, kan het identity protection systeem een herhaalde IP-adres als een aanvaller identificeren.
- **Stroom voor vertrouwelijke**: de client-ID is gevalideerd, maar het toepassingsgeheim is niet gevalideerd.

##  <a name="create-a-resource-owner-policy"></a>Maak een beleid voor resource-eigenaar

1. Meld u aan bij de Azure portal als hoofdbeheerder van uw Azure AD B2C-tenant.
2. Als u wilt overschakelen naar uw Azure AD B2C-tenant, selecteer de B2C-directory in de rechterbovenhoek van de portal.
3. Onder **beleid**, selecteer **Resource-eigenaar beleid**.
4. Geef een naam voor het beleid, zoals *ROPC_Auth*, en selecteer vervolgens **toepassingsclaims**.
5. Selecteer de toepassingsclaims die u nodig hebt voor uw toepassing, zoals *weergavenaam*, *e-mailadres*, en *identiteitsprovider*.
6. Selecteer **OK**, en selecteer vervolgens **Maken**.

   Vervolgens ziet u een eindpunt zoals in dit voorbeeld:

   `https://login.microsoftonline.com/yourtenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1A_ROPC_Auth`


## <a name="register-an-application"></a>Een toepassing registreren

1. Selecteer in de B2C-instellingen, **toepassingen**, en selecteer vervolgens **toevoegen**.
2. Voer een naam voor de toepassing zoals *ROPC_Auth_app*.
3. Selecteer **Nee** voor **Web App of Web-API**, en selecteer vervolgens **Ja** voor **Native client**.
4. Laat alle andere waarden zijn, en selecteer vervolgens **maken**.
5. Selecteer de nieuwe toepassing en noteer de toepassings-ID voor later gebruik.

## <a name="test-the-policy"></a>Het beleid testen

Gebruik uw favoriete toepassing van de API-ontwikkeling voor het genereren van een API-aanroep en bekijk de reactie voor foutopsporing van uw beleid. Bouw een aanroep als volgt met de informatie in de volgende tabel als de hoofdtekst van de POST-aanvraag:
- Vervang  *\<yourtenant.onmicrosoft.com >* met de naam van uw B2C-tenant.
- Vervang  *\<B2C_1A_ROPC_Auth >* met de volledige naam van uw beleid resource-eigenaar voor referenties.
- Vervang  *\<bef2222d56-552f-4a5b-b90a-1988a7d634c3 >* met de toepassings-ID van uw registratie.

`https://login.microsoftonline.com/<yourtenant.onmicrosoft.com>/<B2C_1A_ROPC_Auth>/oauth2/v2.0/token`

| Sleutel | Waarde |
| --- | ----- |
| gebruikersnaam | leadiocl@outlook.com |
| wachtwoord | Passxword1 |
| grant_type | wachtwoord |
| scope | openid \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > offline_access |
| client_id | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| response_type | token id_token |

*Client_id* is de waarde die u eerder hebt genoteerd als de toepassings-ID. *Offline_access* is optioneel als u wilt een vernieuwingstoken dat is ontvangen. 

De werkelijke POST-aanvraag ziet er als volgt:

```
POST /yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

username=leadiocl%40trashmail.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```


Een geslaagde reactie met offline toegang lijkt op het volgende voorbeeld:

```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJpc3MiOiJodHRwczovL3RlLmNwaW0ud2luZG93cy5uZXQvZjA2YzJmZTgtNzA5Zi00MDMwLTg1ZGMtMzhhNGJmZDllODJkL3YyLjAvIiwiZXhwIjoxNTEzMTMwMDc4LCJuYmYiOjE1MTMxMjY0NzgsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsIm9pZCI6IjNjM2I5NjljLThjNDktNGUxMS1hNGVmLWZkYjJmMzkyZjA0OSIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF6cCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsInZlciI6IjEuMCIsImlhdCI6MTUxMzEyNjQ3OH0.MSEThYZxCS4SevBw3-3ecnVLUkucFkehH-gH-P7SFcJ-MhsBeQEpMF1Rzu_R9kUqV3qEWKAPYCNdZ3_P4Dd3a63iG6m9TnO1Vt5SKTETuhVx3Xl5LYeA1i3Slt9Y7LIicn59hGKRZ8ddrQzkqj69j723ooy01amrXvF6zNOudh0acseszt7fbzzofyagKPerxaeTH0NgyOinLwXu0eNj_6RtF9gBfgwVidRy9OzXUJnqm1GdrS61XUqiIUtv4H04jYxDem7ek6E4jsH809uSXT0iD5_4C5bDHrpO1N6pXSasmVR9GM1XgfXA_IRLFU4Nd26CzGl1NjbhLnvli2qY4A", 
    "token_type": "Bearer", 
    "expires_in": "3600", 
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6IjEuMCJ9.aJ_2UW14dh4saWTQ0jLJ7ByQs5JzIeW_AU9Q_RVFgrrnYiPhikEc68ilvWWo8B20KTRB_s7oy_Eoh5LACsqU6Oz0Mjnh0-DxgrMblUOTAQ9dbfAT5WoLZiCBJIz4YT5OUA_RAGjhBUkqGwdWEumDExQnXIjRSeaUBmWCQHPPguV1_5wSj8aW2zIzYIMbofvpjwIATlbIZwJ7ufnLypRuq_MDbZhJkegDw10KI4MHJlJ40Ip8mCOe0XeJIDpfefiJ6WQpUq4zl06NO7j8kvDoVq9WALJIao7LYk_x9UIT-3d0W0eDBHGSRcNgtMYpymaN9ltx6djcEesXNn4CFnWG3g.y6KKeA9EcsW9zW-g.TrTSgn4WBt18gezegxihBla9SLSTC3YfDROQsL9K4yX4400FKlTlf-2l9CnpGTEdWXVi7sIMHCl8S4oUiXd-rvY2mn_NfDrbbVJfgKp1j7Nnq9FFyeJEFcP_FtUXgsNTG9iwfzWox04B1d845qNRWiS9N8BhAAAIdz5N0ChHuOxsVOC0Y_Ly3DNe-JQyXcq964M6-jp3cgi4UqMxT837L6pLY5Ih_iPsSfyHzstsFeqyUIktnzt1MpTlyW-_GDyFK1S-SyV8PPQ7phgFouw2jho1iboHX70RlDGYyVmP1CfQzKE_zWxj3rgaCZvYMWN8fUenoiatzhvWkUM7dhqKGjofPeL8rOMkhl6afLLjObzhUg3PZFcMR6guLjQdEwQFufWxGjfpvaHycZSKeWu6-7dF8Hy_nyMLLdBpUkdrXPob_5gRiaH72KvncSIFvJLqhY3NgXO05Fy87PORjggXwYkhWh4FgQZBIYD6h0CSk2nfFjR9uD9EKiBBWSBZj814S_Jdw6HESFtn91thpvU3hi3qNOi1m41gg1vt5Kh35A5AyDY1J7a9i_lN4B7e_pknXlVX6Z-Z2BYZvwAU7KLKsy5a99p9FX0lg6QweDzhukXrB4wgfKvVRTo.mjk92wMk-zUSrzuuuXPVeg", 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJleHAiOjE1MTMxMzAwNzgsIm5iZiI6MTUxMzEyNjQ3OCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly90ZS5jcGltLndpbmRvd3MubmV0L2YwNmMyZmU4LTcwOWYtNDAzMC04NWRjLTM4YTRiZmQ5ZTgyZC92Mi4wLyIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsImFjciI6ImIyY18xYV9yZXNvdXJjZW93bmVydjIiLCJpYXQiOjE1MTMxMjY0NzgsImF1dGhfdGltZSI6MTUxMzEyNjQ3OCwib2lkIjoiM2MzYjk2OWMtOGM0OS00ZTExLWE0ZWYtZmRiMmYzOTJmMDQ5IiwiYXRfaGFzaCI6Ikd6QUNCTVJtcklwYm9OdkFtNHhMWEEifQ.iAJg13cgySsdH3cmoEPGZB_g-4O8KWvGr6W5VzRXtkrlLoKB1pl4hL6f_0xOrxnQwj2sUgW-wjsCVzMc_dkHSwd9QFZ4EYJEJbi1LMGk2lW-PgjsbwHPDU1mz-SR1PeqqJgvOqrzXo0YHXr-e07M4v4Tko-i_OYcrdJzj4Bkv7ZZilsSj62lNig4HkxTIWi5Ec2gD79bPKzgCtIww1KRnwmrlnCOrMFYNj-0T3lTDcXAQog63MOacf7OuRVUC5k_IdseigeMSscrYrNrH28s3r0JoqDhNUTewuw1jx0X6gdqQWZKOLJ7OF_EJMP-BkRTixBGK5eW2YeUUEVQxsFlUg" 
} 
```

## <a name="redeem-a-refresh-token"></a>Een vernieuwingstoken inwisselen

Bouw een POST-aanroep zoals hieronder wordt weergegeven met de informatie in de volgende tabel als de hoofdtekst van de aanvraag:

`https://login.microsoftonline.com/<yourtenant.onmicrosoft.com>/<B2C_1A_ROPC_Auth>/oauth2/v2.0/token`

| Sleutel | Waarde |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| Bron | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| refresh_token | eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3... |

*Client_id* en *resource* zijn de waarden die u eerder hebt genoteerd als de toepassings-ID. *Refresh_token* is het token dat u in de eerder genoemde authentication-oproep ontvangen.

## <a name="implement-with-your-preferred-native-sdk-or-use-app-auth"></a>Met uw voorkeur systeemeigen SDK implementeren of App-verificatie gebruiken

De Azure AD B2C-implementatie voldoet aan de standaarden voor openbare resource eigenaar wachtwoord clientreferenties OAuth 2.0 en moet compatibel zijn met de meeste client-SDK's. We hebben deze stroom uitgebreid, getest in productie met AppAuth voor iOS- en AppAuth voor Android. Zie voor de meest recente informatie [systeemeigen App SDK voor OAuth 2.0 en OpenID Connect implementatie van de moderne best practices](https://appauth.io/).

Voorbeelden van werken die zijn geconfigureerd voor gebruik met Azure AD B2C vanuit GitHub downloaden [voor Android](https://aka.ms/aadb2cappauthropc) en [voor iOS](https://aka.ms/aadb2ciosappauthropc).




