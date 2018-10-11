---
title: Meer informatie over het bieden van optionele claims voor uw Azure AD-toepassing | Microsoft Docs
description: Een handleiding voor het toevoegen van aangepaste of extra claims voor de SAML 2.0 en JSON Web Tokens (JWT)-tokens die zijn uitgegeven door Azure Active Directory.
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: c42e8978a94730669f3c3f879d1d26c4426bd9da
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079126"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app-public-preview"></a>Hoe: geef optioneel claims voor uw Azure AD-app (Preview-versie)

Deze functie wordt gebruikt door ontwikkelaars van toepassingen om op te geven welke claims dat ze willen in tokens die aan hun toepassing worden gestuurd. U kunt optioneel claims te gebruiken:
- Selecteer aanvullende claims in tokens voor uw toepassing moeten worden opgenomen.
- Het gedrag van bepaalde claims die Azure AD in tokens retourneert wijzigen.
- Toevoegen en toegang tot aangepaste claims voor uw toepassing. 

> [!Note]
> Deze mogelijkheid is momenteel in openbare preview. Wees voorbereid om terug te keren of eventuele wijzigingen te verwijderen. De functie is beschikbaar in alle Azure AD-abonnement tijdens de openbare preview. Wanneer de functie algemeen beschikbaar komt, kunnen sommige aspecten van de functie echter een Azure AD premium-abonnement vereist.

Zie voor een lijst van standaard claims en hoe ze worden gebruikt in tokens, de [basisprincipes van tokens die zijn uitgegeven door Azure AD](v1-id-and-access-tokens.md). 

Een van de doelstellingen van de [Azure AD v2.0-eindpunt](active-directory-appmodel-v2-overview.md) is token kleinere om optimale prestaties door clients.  Als gevolg hiervan meerdere claims voorheen opgenomen in de toegang en de ID-tokens zijn niet meer aanwezig zijn in v2.0-tokens en moeten worden gevraagd om specifiek op basis van de per toepassing.

  

**Tabel 1: toepassing**

| Accounttype | V1.0-eindpunt | V2.0-eindpunt  |
|--------------|---------------|----------------|
| Persoonlijke Microsoft-account  | N.V.T. - RPS Tickets in plaats daarvan worden gebruikt | Binnenkort ondersteuning |
| Azure AD-account          | Ondersteund                          | Ondersteund met aanvullende opmerkingen      |

> [!Important]
> Op dit moment apps die ondersteuning bieden voor zowel persoonlijke accounts en Azure AD (geregistreerd via de [portal voor app-registratie](https://apps.dev.microsoft.com)) optioneel claims kan niet worden gebruikt.  Apps die zijn geregistreerd voor alleen Azure AD met behulp van het v2.0-eindpunt kunnen beschikt echter over de optionele claims die ze in het manifest wordt aangevraagd.

## <a name="standard-optional-claims-set"></a>Standard optioneel claims instellen
De set optioneel claims die standaard beschikbaar is voor toepassingen om te gebruiken, worden hieronder vermeld.  Zie het toevoegen van aangepaste optionele claims voor uw toepassing [Mapextensies](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions)hieronder.  Houd er rekening mee dat bij het toevoegen van claims voor de **toegangstoken**, dit geldt voor toegangstokens aangevraagd *voor* de toepassing (een web-API), niet de *door* de toepassing.  Dit zorgt ervoor dat, ongeacht de toegang tot uw API-client de juiste gegevens aanwezig in het toegangstoken dat ze gebruiken is voor verificatie op basis van uw API.

> [!Note]
>De meeste van deze claims kan worden opgenomen in JWTs voor v1.0 en v2.0-tokens, maar geen SAML-tokens, tenzij anders is aangegeven in de kolom Type Token.  Bovendien, terwijl optioneel claims worden alleen ondersteund voor AAD-gebruikers momenteel, wordt MSA-ondersteuning toegevoegd.  Wanneer MSA optioneel claims ondersteuning op het v2.0-eindpunt heeft, wordt de kolom Type gebruiker geven als een claim beschikbaar voor een AAD- of MSA-gebruikers is.  

**Tabel 2: Claimset met optionele**

| Naam                        | Beschrijving   | Tokentype | Gebruikerstype | Opmerkingen  |
|-----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Tijd wanneer de gebruiker laatste geverifieerd.  Zie de OpenID Connect-specificatie.| JWT        |           |  |
| `tenant_region_scope`      | De regio van de resource-tenant | JWT        |           | |
| `signin_state`             | Meld u aan Status claim   | JWT        |           | waarden, retourneren 6 als vlaggen:<br> "dvc_mngd": het apparaat wordt beheerd<br> "dvc_cmp": apparaat is compatibel<br> "dvc_dmjd": apparaat is toegevoegd aan een domein<br> "dvc_mngd_app": het apparaat wordt beheerd via MDM<br> "inknownntwk": apparaat is een bekend netwerk van binnenuit.<br> "kmsi": houd Me aangemeld is gebruikt. <br> |
| `controls`                 | Meerdere waarden claim met de sessiebesturingselementen afgedwongen door het beleid voor voorwaardelijke toegang.  | JWT        |           | 3-waarden:<br> 'app_res': de app moet afdwingen meer granulaire beperkingen. <br> "ca_enf": afdwingen van voorwaardelijke toegang is uitgesteld en is nog steeds vereist. <br> "no_cookie": dit token is onvoldoende voor het uitwisselen van een cookie in de browser. <br>  |
| `home_oid`                 | Voor gastgebruikers, de object-ID van de gebruiker in de starttenant van de gebruiker.| JWT        |           | |
| `sid`                      | Sessie-ID die wordt gebruikt voor sessie-gebruiker die afmelden wordt weergegeven. | JWT        |           |         |
| `platf`                    | Apparaatplatform    | JWT        |           | Beperkt tot beheerde apparaten die u kunnen controleren of apparaattype.|
| `verified_primary_email`   | Afkomstig uit van de gebruiker PrimaryAuthoritativeEmail      | JWT        |           |         |
| `verified_secondary_email` | Afkomstig uit van de gebruiker SecondaryAuthoritativeEmail   | JWT        |           |        |
| `enfpolids`                | Afgedwongen beleid voor id's. Een lijst van het beleid voor id's die zijn geëvalueerd voor de huidige gebruiker.  | JWT |  |  |
| `vnet`                     | Informatie over VNET-aanduiding.    | JWT        |           |      |
| `fwd`                      | IP-adres.| JWT    |   | Het oorspronkelijke IPv4-adres van de aanvragende client (binnen een VNET) toegevoegd |
| `ctry`                     | Land van gebruiker | JWT |           | Azure AD-retourneert de `ctry` optionele claim als deze aanwezig is en de waarde van de claim een standaard twee letters landnummer, zoals FR, JP en SZ is. |
| `tenant_ctry`              | Land van de tenant van de resource | JWT | | |
| `xms_pdl`          | Gewenste gegevenslocatie   | JWT | | Voor meerdere geografische gebieden tenants is dit de 3-letterige code wordt weergegeven welke geografische regio als de gebruiker zich bevindt.  Zie voor meer informatie de [Azure AD Connect-documentatie over de gewenste gegevenslocatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation). <br> Bijvoorbeeld: `APC` voor Azië en Stille Oceaan. |
| `xms_pl`                   | Gebruiker gewenste taal  | JWT ||De gebruiker de taal, bij voorkeur als instellen.  Afkomstig uit de starttenant, in de Gast-scenario's.  LLE CC opgemaakt ("en-us '). |
| `xms_tpl`                  | Tenant van de taal van voorkeur| JWT | | De resource-tenant de taal, bij voorkeur als instellen.  Opgemaakte LLE ('en'). |
| `ztdid`                    | Zero-touch-implementatie-ID | JWT | | De apparaat-id die wordt gebruikt voor [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) |
| `acct`             | Accountstatus gebruikers in de tenant.   | JWT, SAML | | Als de gebruiker een lid van de tenant is, is de waarde `0`.  Als ze een gast zijn, is de waarde `1`.  |
| `upn`                      | UserPrincipalName claim.  | JWT, SAML  |           | Hoewel deze claim automatisch geïnstalleerd wordt, kunt u deze kunt opgeven als een optionele claim extra eigenschappen voor het wijzigen van het gedrag in het geval van de gebruiker Gast koppelen.  <br> Aanvullende eigenschappen: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash` |

### <a name="v20-optional-claims"></a>Optionele claims v2.0

Deze claims worden altijd in de tokens v1.0 opgenomen, maar niet opgenomen in v2.0-tokens, tenzij aangevraagd.  Deze claims zijn alleen van toepassing voor JWTs (ID-tokens en Tokens voor toegang).  

**Tabel 3: V2.0-alleen optioneel claims**

| JWT-Claim     | Naam                            | Beschrijving                                                                                                                    | Opmerkingen |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP-adres                      | De client heeft aangemeld vanaf het IP-adres.                                                                                      |       |
| `onprem_sid`  | On-Premises beveiligings-id |                                                                                                                                |       |
| `pwd_exp`     | Wachtwoordverlooptijd        | De datum en tijd waarop het wachtwoord is verlopen.                                                                                    |       |
| `pwd_url`     | URL van wijzigen wachtwoord             | Een URL die de gebruiker bezoeken kan om hun wachtwoord te wijzigen.                                                                        |       |
| `in_corp`     | Binnen bedrijfsnetwerk        | Signalen als de client is aangemeld vanuit het bedrijfsnetwerk bevinden. Als dat niet het geval is, is de claim niet opgenomen                     |       |
| `nickname`    | Bijnaam                        | Een andere naam voor de gebruiker te scheiden van de naam van eerste of laatste.                                                             |       |                                                                                                                |       |
| `family_name` | Achternaam                       | Biedt de achternaam, achternaam, of familienaam van de gebruiker zoals gedefinieerd in de Azure AD-gebruiker-object. <br>"family_name": "Kleefstra" |       |
| `given_name`  | Voornaam                      | De eerste biedt of als u ' ' naam van de gebruiker, zoals ingesteld op de Azure AD-gebruiker-object.<br>"given_name": "Frank"                   |       |

### <a name="additional-properties-of-optional-claims"></a>Aanvullende eigenschappen van optionele claims

Sommige optionele claims kunnen worden geconfigureerd voor het wijzigen van de manier waarop die de claim wordt geretourneerd.  De aanvullende eigenschappen worden voornamelijk gebruikt om te helpen bij de migratie van on-premises toepassingen met verschillende verwachtingen (bijvoorbeeld `include_externally_authenticated_upn_without_hash` helpt bij de clients die niet kunnen hashmarks verwerken (`#`) in de UPN)

**Tabel 4: Waarden voor het configureren van standard optioneel claims**

| Naam van eigenschap                                     | Aanvullende eigenschapsnaam                                                                                                             | Beschrijving |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |  Kan worden gebruikt voor SAML- en JWT antwoorden.            |
| | `include_externally_authenticated_upn`              | Bevat de UPN die is opgeslagen in de resource-tenant van de Gast.  Bijvoorbeeld: `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Hetzelfde als hierboven, met dien verstande dat de hashmarks (`#`) zijn vervangen door een onderstrepingsteken (`_`), bijvoorbeeld `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Op te geven dat de optionele upn-claim zonder een extra eigenschap verandert niet elk gedrag – als u wilt bekijken van een nieuwe claim uitgegeven in het token, moet ten minste één van de aanvullende eigenschappen worden toegevoegd. 

#### <a name="additional-properties-example"></a>Voorbeeld van de aanvullende eigenschappen

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Dit object OptionalClaims zorgt ervoor dat de ID-token dat is geretourneerd naar de client om op te nemen van een andere upn met de extra starttenant en informatie over de resource-tenant.  Hiermee wordt alleen gewijzigd de `upn` claim in het token als de gebruiker een gast in de tenant (die gebruikmaakt van een andere id-provider voor verificatie). 

## <a name="configuring-optional-claims"></a>Optionele claims configureren

U kunt optioneel claims voor uw toepassing configureren door het wijzigen van het toepassingsmanifest (Zie onderstaand voorbeeld). Zie voor meer informatie de [inzicht in de Azure AD application manifest artikel](reference-app-manifest.md).

**Voorbeeldschema:**

```json
"optionalClaims":  
   {
       "idToken": [
             { 
                   "name": "auth_time", 
                   "essential": false
              }
        ],
 "accessToken": [ 
             {
                    "name": "ipaddr", 
                    "essential": false
              }
        ],
"saml2Token": [ 
              { 
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>Type OptionalClaims

Verklaart de optionele claims aangevraagd door een toepassing. Een toepassing kunt configureren voor optionele claims moeten worden geretourneerd in elk van de drie typen van tokens (id-token, token, SAML 2 toegangstoken) kan ontvangen van de service voor beveiligingstokens. De toepassing kan een andere set optioneel claims moeten worden geretourneerd in elke tokentype configureren. De eigenschap OptionalClaims van de toepassing-entiteit is een OptionalClaims-object.

**Tabel 5: OptionalClaims Type-eigenschappen**

| Naam        | Type                       | Beschrijving                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Verzameling (OptionalClaim) | De optionele claims geretourneerd in de ID van de JWT-token.     |
| `accessToken` | Verzameling (OptionalClaim) | De optionele claims geretourneerd in de JWT-toegangstoken. |
| `saml2Token`  | Verzameling (OptionalClaim) | De optionele claims geretourneerd in het SAML-token.       |

### <a name="optionalclaim-type"></a>Type OptionalClaim

Bevat een optionele claim die zijn gekoppeld aan een toepassing of een service-principal. De idToken accessToken en saml2Token eigenschappen van de [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) type is een verzameling van OptionalClaim.
Als dit wordt ondersteund door een specifieke claim, kunt u ook het gedrag van het gebruik van het veld AdditionalProperties OptionalClaim wijzigen.

**Tabel 6: OptionalClaim Type-eigenschappen**

| Naam                 | Type                    | Beschrijving                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | De naam van de optionele claim.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | De bron (directory-object) van de claim. Er zijn vooraf gedefinieerde claims en de gebruiker gedefinieerde claims van extensie-eigenschappen. Als de waarde null is, is de claim een vooraf gedefinieerde optioneel claim. Als waarde voor de gebruiker is, is de waarde in de naameigenschap van de extensie-eigenschap van het gebruikersobject. |
| `essential`            | Edm.Boolean             | Als de waarde true is, is de claim die is opgegeven door de client die nodig zijn om te controleren of een goede autorisatie-ervaring voor de specifieke taak die is aangevraagd door de eindgebruiker. De standaardwaarde is false.                                                                                                                 |
| `additionalProperties` | Verzameling (Edm.String) | Aanvullende eigenschappen van de claim. Als een eigenschap in deze verzameling bestaat, wijzigt u over de werking van de optionele claim die in de eigenschap name is opgegeven.                                                                                                                                                   |
## <a name="configuring-custom-claims-via-directory-extensions"></a>Aangepaste claims via mapextensies configureren

Naast de standaard optioneel claims is ingesteld, kunnen tokens ook worden geconfigureerd om op te nemen van directory-schema-uitbreidingen (Zie de [Directory-schema-uitbreidingen artikel](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) voor meer informatie).  Deze functie is handig voor het koppelen van aanvullende informatie die uw app, bijvoorbeeld gebruiken kunt, een extra id of een belangrijke configuratie-optie die de gebruiker heeft ingesteld. 

> [!Note]
> Directory-schema-uitbreidingen zijn een functie van AAD alleen-lezen, dus als uw toepassing manifest van de aanvragen voor een aangepaste extensie- en een MSA-gebruiker meldt zich aan bij uw app, worden deze extensies, niet geretourneerd. 

### <a name="values-for-configuring-additional-optional-claims"></a>Waarden voor het configureren van aanvullende optionele claims

Gebruik de volledige naam van de extensie voor extensiekenmerken (in de indeling: `extension_<appid>_<attributename>`) in het toepassingsmanifest. De `<appid>` moet overeenkomen met de id van de aanvraag om de claim. 

Binnen de JWT, wordt deze claims worden verzonden met de volgende indeling: `extn.<attributename>`.

In het SAML-tokens, wordt deze claims worden verzonden met de volgende URI-indeling: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>Voorbeeld van de optionele claims

In deze sectie kan u helpen bij een scenario om te zien hoe u de functie optioneel claims voor uw toepassing kunt gebruiken.
Er zijn meerdere opties beschikbaar voor het bijwerken van de eigenschappen van de configuratie van de identiteit van een toepassing inschakelen en configureren van optionele claims:
-   U kunt het toepassingsmanifest kunt wijzigen. In het volgende voorbeeld wordt met deze methode gebruiken om uit te voeren van de configuratie. Lees de [inzicht krijgen in het document van Azure AD-toepassing-manifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) eerste voor een inleiding tot het manifest.
-   Het is ook mogelijk om te schrijven van een toepassing die gebruikmaakt van de [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) om bij te werken van uw toepassing. De [entiteit en complex type reference](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) in de Graph API reference guide kan u helpen bij de optionele claims configureren.

**Voorbeeld:** In het onderstaande voorbeeld wijzigt u een toepassingsmanifest om toe te voegen van claims voor toegang, -ID en SAML tokens die zijn bedoeld voor de toepassing.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Nadat u hebt geverifieerd, kiest u uw Azure AD-tenant door deze te selecteren in de rechterbovenhoek van de pagina.
1. Selecteer **Azure AD-extensie** in het linkernavigatievenster en klik op **App-registraties**.
1. De toepassing die u wilt configureren van optionele claims voor in de lijst en klik op het niet vinden.
1. Klik op de toepassingspagina **Manifest** om het manifest inline-editor te openen. 
1. U kunt het manifest met behulp van deze editor rechtstreeks bewerken. Het manifest volgt het schema voor de [Toepassingsentiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity), en het manifest één keer opgeslagen automatische-indelingen. Nieuwe elementen worden toegevoegd aan de `OptionalClaims` eigenschap.

      ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
      "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
      "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }
      ```
      In dit geval zijn verschillende optionele claims toegevoegd aan elk type token dat de toepassing kan ontvangen. De ID-tokens bevat nu de UPN voor federatieve gebruikers in het volledige formulier (`<upn>_<homedomain>#EXT#@<resourcedomain>`). De toegangstokens die andere clients voor deze toepassing vragen bevat nu de claim auth_time. De SAML-tokens bevat nu de skypeId directory-schema-uitbreiding (in dit voorbeeld wordt de app-ID voor deze app is ab603c56068041afb2f6832e2a17e237).  De SAML-tokens wordt weergegeven de Skype-ID als `extension_skypeId`.

1. Wanneer u klaar bent met het bijwerken van het manifest, klikt u op **opslaan** om op te slaan van het manifest

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de standaard claims geleverd door Azure AD.

- [ID-tokens](id-tokens.md)
- [Toegangstokens](access-tokens.md)
