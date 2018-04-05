---
title: Meer informatie over het optionele autorisatiebeslissingen aan uw Azure AD-toepassing | Microsoft Docs
description: Een handleiding voor het toevoegen van aangepaste of extra claims naar de door Azure Active Directory wordt uitgegeven tokens van SAML 2.0 en JSON Web Tokens (JWT).
documentationcenter: na
author: hpsin
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/15/2018
ms.author: hirsin
ms.custom: aaddev
ms.openlocfilehash: f9cc4f900428e1337fc9b9d428879d6527c60017
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="optional-claims-in-azure-ad-preview"></a>Optionele claims in Azure AD (preview)

Deze functie wordt gebruikt door ontwikkelaars van toepassingen om op te geven welke claims dat ze willen in tokens die worden verzonden naar de toepassing. U kunt optioneel claims te gebruiken:
-   Selecteer extra claims moeten worden opgenomen in de tokens voor uw toepassing.
-   Het gedrag van bepaalde claims die Azure AD in tokens retourneert wijzigen.
-   Voeg en toegang tot aangepaste claims voor uw toepassing. 

> [!Note]
> Deze mogelijkheid is momenteel in de openbare preview. Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen. De functie is beschikbaar in alle Azure AD-abonnement gedurende openbare preview. Wanneer de functie algemeen beschikbaar wordt, kunnen sommige aspecten van de functie echter een Azure AD premium-abonnement nodig.

Zie voor een overzicht van de standaard claims en hoe ze worden gebruikt in tokens de [basisprincipes van tokens die zijn uitgegeven door Azure AD](active-directory-token-and-claims.md). 

Een van de doelstellingen van de [Azure AD v2.0-eindpunt](active-directory-appmodel-v2-overview.md) is token kleinere optimale prestaties door clients te garanderen.  Als gevolg hiervan meerdere claims voorheen opgenomen in de toegang en de ID-tokens die niet meer aanwezig zijn in v2.0 tokens en moeten worden gevraagd specifiek op basis van de per toepassing.  

**Tabel 1: toepasbaarheid**

| Accounttype | V1.0 eindpunt                      | V2.0-eindpunt  |
|--------------|------------------------------------|----------------|
| MSA          | N.V.T. - RPS Tickets in plaats daarvan worden gebruikt | Ondersteuning wordt binnenkort |
| AAD          | Ondersteund                          | Ondersteund      |

## <a name="standard-optional-claims-set"></a>Standaard optionele claims instellen
De set optioneel claims die standaard beschikbaar zijn voor toepassingen om te gebruiken, worden hieronder vermeld.  Zie het toevoegen van aangepaste optionele claims voor uw toepassing [Directory uitbreidingen](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions)onderstaande. 

> [!Note]
>Het merendeel van deze claims kan worden opgenomen in JWTs, maar niet SAML-tokens, tenzij anders is aangegeven in de kolom Type Token.  Bovendien terwijl optionele claims worden alleen ondersteund voor AAD-gebruikers momenteel, wordt MSA ondersteuning toegevoegd.  Wanneer MSA optionele claims ondersteuning op het v2.0-eindpunt heeft, wordt de kolom Type gebruiker geven aan of een claim voor een gebruiker met AAD of MSA beschikbaar is.  

**Tabel 2: Claimset met optionele**

| Naam                     | Beschrijving                                                                                                                                                                                     | Type token | Gebruikerstype | Opmerkingen                                                                                                                                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `auth_time`                | De tijd wanneer de gebruiker laatste geverifieerd.  Zie OpenID Connect-specificatie.                                                                                                                                | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `tenant_region_scope`      | De regio van de resource-tenant                                                                                                                                                                   | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `signin_state`             | Meld u aan Status claim                                                                                                                                                                             | JWT        |           | 6 retourwaarden als vlaggen:<br> 'dvc_mngd': apparaat wordt beheerd<br> 'dvc_cmp': apparaat compatibel is<br> 'dvc_dmjd': apparaat is verbonden met het domein<br> 'dvc_mngd_app': apparaat wordt beheerd via MDM<br> 'inknownntwk': apparaat is binnen een bekende netwerk.<br> 'kmsi': houd mij ondertekend is gebruikt. <br> |
| `controls`                 | Meerdere waarden claim met de sessie-besturingselementen afgedwongen door beleidsregels voor voorwaardelijke toegang.                                                                                                       | JWT        |           | 3 waarden:<br> 'app_res': de app moet afdwingen meer granulaire beperkingen. <br> 'ca_enf': afdwinging van voorwaardelijke toegang werd uitgesteld en is nog steeds vereist. <br> 'no_cookie': dit token is onvoldoende voor het uitwisselen van voor een cookie in de browser. <br>                              |
| `home_oid`                 | Voor gastgebruikers, de object-ID van de gebruiker in de oorspronkelijke tenant van de gebruiker.                                                                                                                           | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `sid`                      | Sessie-ID voor de per sessie gebruiker signout gebruikt.                                                                                                                                                  | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `platf`                    | Apparaatplatform                                                                                                                                                                                 | JWT        |           | Beperkt tot beheerde apparaten die u kunnen controleren of het apparaattype.                                                                                                                                                                                                                              |
| `verified_primary_email`   | Afkomstig van de gebruiker PrimaryAuthoritativeEmail                                                                                                                                               | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `verified_secondary_email` | Afkomstig van de gebruiker SecondaryAuthoritativeEmail                                                                                                                                             | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `enfpolids`                | Afgedwongen beleid id's. Een lijst van het beleid voor id's die zijn geëvalueerd voor de huidige gebruiker.                                                                                                         | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `vnet`                     | VNET-aanduiding informatie.                                                                                                                                                                     | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `fwd`                      | IP-adres.  Voegt de oorspronkelijke IPv4-adres van de aanvragende client (binnen een VNET)                                                                                                       | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `ctry`                     | Land van gebruiker                                                                                                                                                                                  | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `tenant_ctry`              | Resource-tenant land                                                                                                                                                                       | JWT        |           |                                                                                                                                                                                                                                                                                         |
| `upn`                      | UserPrincipalName claim.  Hoewel deze claim automatisch geïnstalleerd wordt, kunt u dit opgeven als een optionele claim extra eigenschappen voor het wijzigen van het gedrag in het geval van de gebruiker gastbesturingssysteem koppelen. | JWT, SAML  |           | Aanvullende eigenschappen: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash`                                                                                                                                                                 |
### <a name="v20-optional-claims"></a>Optionele claims v2.0
Deze claims worden altijd opgenomen in v1.0 tokens, maar worden verwijderd uit het v2.0-tokens tenzij aangevraagd.  Deze claims zijn alleen van toepassing op JWTs (ID-tokens en toegangstokens).  

**Tabel 3: V2.0 alleen-lezen optionele claims**

| JWT Claim     | Naam                            | Beschrijving                                                                                                                    | Opmerkingen |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP-adres                      | Het IP-adres voor de client van aangemeld.                                                                                      |       |
| `onprem_sid`  | On-Premises beveiligings-id |                                                                                                                                |       |
| `pwd_exp`     | Wachtwoordverlooptijd        | De datum/tijd waarop het wachtwoord vervalt.                                                                                    |       |
| `pwd_url`     | URL van wijzigen wachtwoord             | Een URL die de gebruiker bezoeken kan om hun wachtwoord te wijzigen.                                                                        |       |
| `in_corp`     | Binnen bedrijfsnetwerk        | Geeft aan of de client is logboekregistratie in van het bedrijfsnetwerk. Als dat niet het geval is, wordt de claim niet meegeleverd                     |       |
| `nickname`    | Nickname                        | Een extra naam voor de gebruiker, het scheiden van voornaam of achternaam.                                                             |       |                                                                                                                |       |
| `family_name` | Achternaam                       | Biedt de laatste naam, achternaam of familienaam van de gebruiker, zoals gedefinieerd in de Azure AD-gebruiker-object. <br>"family_name":"Miller" |       |
| `given_name`  | Voornaam                      | De eerste biedt of als u ' ' naam van de gebruiker, zoals ingesteld op het gebruikersobject Azure AD.<br>"given_name": "Jaap"                   |       |

### <a name="additional-properties-of-optional-claims"></a>Aanvullende eigenschappen van optionele claims

Sommige optionele claims kunnen worden geconfigureerd om te wijzigen van de manier waarop die de claim wordt geretourneerd.  Deze extra eigenschappen worden meestal gebruikt om u te helpen bij de migratie van on-premises toepassingen met verschillende gegevens verwachtingen (bijvoorbeeld `include_externally_authenticated_upn_without_hash` helpt met clients die niet kunnen hashmarks verwerken (`#`) in de UPN)

**Tabel 4: Waarden voor het configureren van standaard optionele claims**

| De naam van eigenschap                                     | Aanvullende eigenschapsnaam                                                                                                             | Beschrijving |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |             |
| | `include_externally_authenticated_upn`              | Bevat de Gast UPN die is opgeslagen in de resource-tenant.  Bijvoorbeeld: `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Dezelfde als hierboven, behalve dat de hashmarks (`#`) zijn vervangen door een onderstrepingsteken (`_`), bijvoorbeeld `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Geven dat de UPN-optioneel claim zonder een extra eigenschap verandert niet alle gedrag – om te zien van een nieuwe claim uitgegeven in het token, moet ten minste één van de aanvullende eigenschappen worden toegevoegd. 


#### <a name="additional-properties-example"></a>Voorbeeld van extra eigenschappen:

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

Dit object OptionalClaims zorgt ervoor dat het ID-token geretourneerd naar de client naar een andere upn met de extra thuis tenant en de tenant resourcegegevens bevatten.  

## <a name="configuring-optional-claims"></a>Optionele claims configureren

U kunt optioneel claims voor uw toepassing configureren door het wijzigen van het toepassingsmanifest (Zie onderstaand voorbeeld). Zie voor meer informatie de [inzicht in het manifest artikel voor Azure AD-toepassing](active-directory-application-manifest.md).

**Voorbeeldschema:**

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

Verklaart de optionele claims aangevraagd door een toepassing. Een toepassing kunt optionele claims moeten worden geretourneerd in elk van de drie typen tokens (ID token, token, SAML 2 toegangstoken) het kan ontvangen van de service voor beveiligingstokens configureren. De toepassing kunt configureren van een andere set optionele claims in elk type token wordt geretourneerd. De eigenschap OptionalClaims van de entiteit van de toepassing is een OptionalClaims-object.

**Tabel 5: OptionalClaims Type-eigenschappen**

| Naam        | Type                       | Beschrijving                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Verzameling (OptionalClaim) | De optionele claims geretourneerd in de ID van de JWT-token.     |
| `accessToken` | Verzameling (OptionalClaim) | De optionele claims geretourneerd in het JWT-toegangstoken. |
| `saml2Token`  | Verzameling (OptionalClaim) | De optionele claims geretourneerd in het SAML-token.       |


### <a name="optionalclaim-type"></a>Type OptionalClaim

Een optionele claim die zijn gekoppeld aan een toepassing of een service-principal bevat. De idToken accessToken en saml2Token eigenschappen van de [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) type is een verzameling OptionalClaim.
Als door een specifieke claim ondersteund, kunt u ook het gedrag van het gebruik van het veld AdditionalProperties OptionalClaim wijzigen.

**Tabel 6: OptionalClaim Type-eigenschappen**

| Naam                 | Type                    | Beschrijving                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | De naam van de optionele claim.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | De bron (directoryobject) van de claim. Er zijn vooraf gedefinieerde claims en de gebruiker gedefinieerde claims van extensie-eigenschappen. Als de bronwaarde null is, is de claim een vooraf gedefinieerde optionele claim. Als de bronwaarde gebruiker is, is de waarde in de eigenschap name de extensie-eigenschap van het gebruikersobject. |
| `essential`            | Edm.Boolean             | Als de waarde true is, is de claim die is opgegeven door de client nodig om een goede autorisatie-ervaring voor de specifieke taak die is aangevraagd door de eindgebruiker. De standaardwaarde is ingesteld op false.                                                                                                                 |
| `additionalProperties` | Verzameling (Edm.String) | Aanvullende eigenschappen van de claim. Als een eigenschap in deze verzameling bestaat, wijzigt u over de werking van de optionele claim die is opgegeven in de eigenschap name.                                                                                                                                                   |

## <a name="configuring-custom-claims-via-directory-extensions"></a>Aangepaste claims via directory-uitbreidingen configureren

Naast de standaard optionele claims is ingesteld, kunnen tokens ook worden geconfigureerd om op te nemen van directory-schema-uitbreidingen (Zie de [Directory-schema-uitbreidingen artikel](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) voor meer informatie).  Deze functie is handig voor het koppelen van aanvullende informatie die uw app: bijvoorbeeld gebruiken kunt, een extra id of belangrijke configuratie-optie die de gebruiker heeft ingesteld. 

> [!Note]
> Directory-schema-uitbreidingen zijn een functie AAD alleen-lezen, dus als uw toepassing aanvragen manifest een aangepaste extensie en een gebruiker beheerde Serviceaccounts in uw app registreert, worden deze extensies niet geretourneerd. 

### <a name="values-for-configuring-additional-optional-claims"></a>Waarden voor het configureren van aanvullende optionele claims 

Gebruik de volledige naam van de uitbreiding voor uitbreidingskenmerken, (in de indeling: `extension_<appid>_<attributename>`) in het toepassingsmanifest. De `<appid>` moet overeenkomen met de id van de aanvraag om de claim. 

Binnen de JWT wordt deze claims worden verzonden met de volgende indeling: `extn.<attributename>`.

Binnen de SAML-tokens, wordt deze claims worden verzonden met de volgende URI-indeling: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>Voorbeeld van de optionele claims

In deze sectie kunt u een scenario om te zien hoe u de functie optionele claims kunt gebruiken voor uw toepassing doorlopen.
Er zijn meerdere opties beschikbaar voor het bijwerken van de eigenschappen van een toepassing identiteit configuratie inschakelen en configureren van de optionele claims:
-   U kunt het toepassingsmanifest wijzigen. Het volgende voorbeeld wordt deze methode gebruiken om uit te voeren van de configuratie. Lees de [inzicht in de Azure AD-toepassing-manifestdocument](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) eerste voor een inleiding tot het manifest.
-   Het is ook mogelijk om te schrijven van een toepassing die gebruikmaakt van de [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) uw toepassing bijwerken. De [entiteit en het complexe typeverwijzing](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) in de grafiek API reference handleiding kan u helpen bij het configureren van de optionele claims.

**Voorbeeld:** In het onderstaande voorbeeld wijzigt u een toepassingsmanifest om toe te voegen claims voor toegang, -ID en SAML tokens die zijn bedoeld voor de toepassing.
1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Nadat u hebt geverifieerd, kiest u uw Azure AD-tenant selecteren in de rechterbovenhoek van de pagina.
3.  Selecteer **Azure AD-extensie** uit het linkernavigatievenster en klik op **App registraties**.
4.  De toepassing die u wilt configureren optionele claims voor in de lijst en klikt u op de gevonden.
5.  Klik op de toepassingspagina **Manifest** om het manifest inline-editor te openen. 
6.  U kunt het manifest met behulp van deze editor rechtstreeks bewerken. Het manifest volgt het schema voor de [Toepassingsentiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity), en het manifest wordt eenmaal opgeslagen automatisch-indelingen. Nieuwe elemets wordt toegevoegd aan de `OptionalClaims` eigenschap.

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
      In dit geval er verschillende optionele claims zijn toegevoegd voor elk type token dat de toepassing kan ontvangen. De ID-tokens bevat nu de UPN voor federatieve gebruikers in de volledige vorm (`<upn>_<homedomain>#EXT#@<resourcedomain>`). De toegangstokens ontvangt nu de claim auth_time. De SAML-tokens bevat nu de skypeId directory-schema-uitbreiding (in dit voorbeeld wordt de app-ID voor deze app is ab603c56068041afb2f6832e2a17e237).  De SAML-tokens zal gebruiken de Skype-ID als `extension_skypeId`.

7.  Wanneer u klaar bent voor het bijwerken van het manifest, klikt u op **opslaan** om op te slaan van het manifest


## <a name="related-content"></a>Gerelateerde inhoud
* Meer informatie over de [standaard claims](active-directory-token-and-claims.md) verstrekt door Azure AD. 
