---
title: Met behulp van de taal aanpassing - Azure AD B2C | Microsoft Docs
description: Meer informatie over het aanpassen van de ervaring voor de taal.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/26/2018
ms.author: davidmu
ms.openlocfilehash: 7c72c1d43d9a5fa541c72a8ba7a5ccedeafdaaff
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Aanpassing van de taal met behulp

>[!NOTE]
>Deze functie is openbare preview.
>

Taal aanpassing kunt uw beleid voor verschillende talen aan de klantbehoeften van uw.  Microsoft biedt de vertalingen voor 36 talen (Zie [aanvullende informatie](#additional-information)), maar u kunt ook uw eigen vertalingen voor elke taal opgeven.  Zelfs als uw ervaring is alleen beschikbaar voor één taal, kunt u alle tekst op de pagina's aanpassen.  

## <a name="how-does-language-customization-work"></a>Hoe werkt taal aanpassing?
Taal aanpassing kunt u selecteren welke talen uw reis gebruiker is beschikbaar in.  Zodra de functie is ingeschakeld, kunt u de querytekenreeksparameter ui_locales, bieden van uw toepassing.  Wanneer u in Azure AD B2C aanroept, vertalen we uw pagina naar de landinstellingen die u hebt aangegeven.  Dit type configuratie krijgt u volledige controle over de talen in uw reis gebruiker en de taalinstellingen van de browser van de klant worden genegeerd. U kunt ook moet u mogelijk niet dat niveau van controle over welke talen Zie van uw klant.  Als u een ui_locales-parameter niet opgeeft, wordt de ervaring van de klant wordt bepaald door hun browserinstellingen.  U kunt nog steeds bepalen welke talen uw reis gebruiker wordt omgezet in door toe te voegen als een ondersteunde taal.  Als de browser van de klant is ingesteld op een taal weergeven die u niet wilt ondersteunen, wordt in plaats daarvan de taal die u hebt geselecteerd als een standaardwaarde in ondersteunde culturen weergegeven.

1. **UI-landinstellingen opgegeven taal** -wanneer u de aanpassing van de taal hebt ingeschakeld, de gebruiker reis wordt omgezet in de taal die u hier opgeeft
2. **Browser gevraagde taal** -als er geen gebruikersinterface-landinstellingen is opgegeven, worden omgezet naar de browser gevraagde taal, **als deze is opgenomen in de ondersteunde talen**
3. **De standaardtaal beleid** -als de browser een taal niet opgeven of deze bevat een die niet wordt ondersteund, worden omgezet naar de standaardtaal van beleid

>[!NOTE]
>Als u aangepaste gebruikerskenmerken gebruikt, moet u uw eigen vertalingen bieden. Zie '[aanpassen van uw tekenreeksen](#customize-your-strings)' voor meer informatie.
>

## <a name="support-uilocales-requested-languages"></a>Ondersteuning ui_locales aangevraagd talen 
Beleidsregels die zijn gemaakt vóór de release van de algemene beschikbaarheid van taal aanpassing moet eerst deze functie inschakelt.  Beleidsregels die zijn gemaakt na hebben taal aanpassing is standaard ingeschakeld.  Taal aanpassing van een beleid inschakelt, kunt u de taal van het traject van de gebruiker nu bepalen door de parameter ui_locales toe te voegen.
1. [Volg deze stappen om te navigeren naar de pagina van de B2C-functies in de Azure portal.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Navigeer naar een beleid dat u wilt inschakelen voor vertalingen.
3. Klik op **taal aanpassing**.  
4. Klik op **taal aanpassingen aanbrengen** bovenaan.
5. Lezen van het dialoogvenster en klik op "Ja".

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Selecteer welke talen in uw gebruiker reis zijn ingeschakeld 
Schakel een aantal talen voor de gebruiker reis worden vertaald wanneer de parameter ui_locales is niet opgegeven.
1. Zorg ervoor dat uw beleid heeft taal aanpassing' van de voorgaande instructies ingeschakeld.
2. Van uw **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer een taal die u wilt ondersteunen.
4. In het deelvenster properties schakelen **ingeschakeld** op Ja.  
5. Klik op **opslaan** aan de bovenkant van het eigenschappendeelvenster.

>[!NOTE]
>Als een parameter ui_locales niet is opgegeven, wordt klikt u vervolgens de pagina omgezet in de taal van de webbrowser van de klant alleen als deze is ingeschakeld
>

## <a name="customize-your-strings"></a>Uw tekenreeksen aanpassen
'Aanpassing taal' kunt u een willekeurige tekenreeks in uw reis gebruiker aanpassen.
1. Zorg ervoor dat uw beleid heeft taal aanpassing' van de voorgaande instructies ingeschakeld.
2. Van uw **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer de taal die u wilt aanpassen.
4. Selecteer de pagina die u wilt bewerken.
5. Klik op **downloaden standaardwaarden** (of **onderdrukkingen downloaden** als u deze taal eerder hebt bewerkt). 

Deze stappen geven een JSON-bestand dat u gebruiken kunt om te beginnen met het bewerken van uw tekenreeksen.

### <a name="changing-any-string-on-the-page"></a>Een willekeurige tekenreeks op de pagina wijzigen
1. Open het JSON-bestand dat is gedownload van de voorgaande instructies in een JSON-editor.
2. Zoek het element dat u wilt wijzigen.  U vindt de `StringId` van de tekenreeks die u zoekt, of zoek naar de `Value` u wilt wijzigen.
3. Update de `Value` kenmerk met wat u weergeven wilt.
4. Voor elke tekenreeks die u wilt wijzigen, moet u in-of uitschakelen `Override` naar **True**.
5. Sla het bestand en uploadt u uw wijzigingen (u kunt het besturingselement voor het uploaden vinden in de plaats waar u het JSON-bestand hebt gedownload). 

>[!IMPORTANT]
>Als u onderdrukken van een tekenreeks wilt, controleert u of in te stellen de `Override` van waarde naar `true`.  Als de waarde niet is gewijzigd, wordt de vermelding wordt genegeerd. 
>

### <a name="changing-extension-attributes"></a>Uitbreidingskenmerken wijzigen
Als u wilt wijzigen van de tekenreeks voor een aangepaste gebruikerskenmerk of wilt toevoegen aan de JSON, is het in de volgende indeling:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Vervang `<ExtensionAttribute>` met de naam van het aangepaste kenmerk.  

Vervang `<ExtensionAttributeValue>` met de nieuwe tekenreeks die moet worden weergegeven.

### <a name="using-localizedcollections"></a>Met behulp van LocalizedCollections
Als u voor een setlijst met waarden voor antwoorden wilt, moet u maken een `LocalizedCollections`.  Een `LocalizedCollections` is een matrix met `Name` en `Value` paren.  De `Name` is wat wordt weergegeven en de `Value` is wat wordt geretourneerd als de claim.  Om toe te voegen een `LocalizedCollections`, heeft de volgende indeling:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` de gebruiker die dit kenmerk is `LocalizedCollections` antwoord op
* `Name` de waarde weergegeven voor de gebruiker
* `Value` wordt geretourneerd als deze optie is geselecteerd in de claim is

### <a name="upload-your-changes"></a>Uw wijzigingen te uploaden
1. Nadat u de wijzigingen aan uw JSON-bestand hebt, gaat u terug naar uw B2C-tenant.
2. Van uw **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer de taal die u wilt bieden vertalingen.
4. Selecteer de pagina die u wilt bieden vertalingen.
5. Klik op het mappictogram en selecteer het JSON-bestand te uploaden.
6. Dit gewijzigd is automatisch aan uw beleid opgeslagen.

## <a name="using-page-ui-customization-with-language-customization"></a>Met behulp van pagina UI-aanpassing met taal aanpassing

Er zijn twee manieren om te lokaliseren uw HTML-inhoud.  Door het inschakelen van ['Aanpassing taal'](active-directory-b2c-reference-language-customization.md).  Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales`, naar het eindpunt.  De inhoudsserver kunt u deze parameter gebruiken voor aangepaste HTML-pagina's die een specifieke taal zijn gebonden.

We kunnen ook inhoud ophalen vanaf verschillende locaties op basis van de landinstellingen.  In uw eindpunt CORS is ingeschakeld, kunt u een mapstructuur naar host-inhoud voor specifieke talen instellen en noemen we je de juiste is als u het jokerteken `{Culture:RFC5646}`.  Bijvoorbeeld, als dit er als mijn aangepaste pagina URI:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Ik kan mijn pagina laden in `fr` en wanneer dit html en css-inhoud op te halen is, wordt het downloaden van:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="custom-locales"></a>Aangepaste landinstellingen

U kunt ook talen die Microsoft biedt momenteel geen vertalingen voor toevoegen.  U moet de vertalingen voor alle tekenreeksen in het beleid opgeven.

1. Van uw **beleid bewerken** pagina **taal aanpassing**.
2. Selecteer **aangepaste taal toevoegen** vanaf de bovenkant van de pagina.
3. Identificeren welke taal bieden u vertalingen voor door een geldige landinstelling-code in het deelvenster context dat wordt geopend.
4. U kunt voor elke pagina downloaden van een reeks overschrijvingen voor Engels en werkt op de vertalingen.
5. Als u klaar bent met de JSON-bestanden, kunt u ze kunt uploaden voor elke pagina.
6. Selecteer **inschakelen** en uw beleid voor deze taal voor uw gebruiker kan nu worden weergegeven.
7. Moet u taal opslaan nadat deze is ingeschakeld.

## <a name="additional-information"></a>Aanvullende informatie

### <a name="page-ui-customization-labels-are-persisted-as-your-first-set-of-overrides-once-language-customization-is-enabled"></a>Page UI aanpassing van labels zijn doorgevoerd als uw eerste set onderdrukkingen zodra 'Aanpassing taal' is ingeschakeld
Als u 'Aanpassing taal' inschakelt, worden de eerdere wijzigingen in met behulp van Page UI-aanpassing van labels vastgehouden in een JSON-bestand voor Engels (en).  U kunt doorgaan met het wijzigen van de labels en andere tekenreeksen door het uploaden van de taalbestanden in 'Aanpassing taal'.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft waakt over de meest recente vertalingen voor het gebruik bieden
We continu verbeteren vertalingen en bewaar deze op naleving voor u.  We identificeren fouten en wijzigingen in globale terminologie en dat de updates die werkt probleemloos in uw reis gebruiker.
### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen van rechts naar links
We momenteel niet zijn ter ondersteuning van rechts naar links worden geschreven, als u nodig hebt met deze functie moet stemmen voor deze functie op [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Vertalingen van de provider van sociale identiteit
We bieden de parameter ui_locales OIDC sociale aanmeldingen, maar ze worden niet herkend door sommige sociale identiteitsproviders, waaronder Facebook en Google. 
### <a name="browser-behavior"></a>Gedrag van de browser
Chrome en Firefox beide aanvragen voor de taal van hun instellen en als het een ondersteunde taal, deze wordt weergegeven voordat de standaardwaarde.  Rand momenteel geen een taal aanvraagt en wordt de meteen naar de standaardtaal.

### <a name="what-languages-are-supported"></a>Welke talen worden ondersteund?

| Taal              | Taalcode |
|-----------------------|---------------|
| Bengalees                | bn            |
| Tsjechisch                 | cs            |
| Deens                | da            |
| Duits                | de            |
| Grieks                 | el            |
| Nederlands               | nl            |
| Spaans               | es            |
| Fins               | fi            |
| Frans                | fr            |
| Gujarati              | Gu            |
| Hindi                 | hi            |
| Kroatisch              | uur            |
| Hongaars             | hu            |
| Italiaans               | it            |
| Japans              | ja            |
| Kanarees               | kn            |
| Koreaans                | ko            |
| Malayalam             | ml            |
| Mahratti               | MR            |
| Maleis                 | ms            |
| Noors, Bokmål      | nb            |
| Nederlands                 | nl            |
| Punjabi               | pa            |
| Pools                | pl            |
| Portugees - Brazilië   | pt-br         |
| Portugees - Portugal | pt-pt         |
| Roemeens              | ro            |
| Russisch               | ru            |
| Slowaaks                | SK            |
| Zweeds               | sv            |
| Tamil                 | DBC            |
| Telugu                | eren            |
| Thais                  | TH            |
| Turks               | TR            |
| Chinees - Vereenvoudigd  | zh-hans       |
| Chinees - Traditioneel | zh-hant       |
