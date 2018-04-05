---
title: Aanpassing van de taal in Azure AD B2C | Microsoft Docs
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
ms.openlocfilehash: 3d0f1f2ffd02873df2e2e7eab9894d9c3421b0f7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Aanpassing van de taal in Azure Active Directory B2C

>[!NOTE]
>Deze functie is openbare preview.
>

Aanpassing van de taal in Azure Active Directory B2C (Azure AD B2C) kunt u uw beleid voor verschillende talen aan de klantbehoeften van uw.  Microsoft biedt de vertalingen voor [36 talen](#supported-languages), maar u kunt ook uw eigen vertalingen voor elke taal opgeven. Zelfs als uw ervaring voor slechts één taal is opgegeven, kunt u alle tekst op de pagina's aanpassen.  

## <a name="how-language-customization-works"></a>De werking van taal aanpassing
Taal aanpassing kunt u selecteren welke talen uw reis gebruiker is beschikbaar in. Nadat de functie is ingeschakeld, kunt u de querytekenreeksparameter bieden `ui_locales`, van uw toepassing. Wanneer u in Azure AD B2C aanroept, wordt de pagina in de landinstellingen die u hebt aangegeven vertaald. Dit type configuratie krijgt u volledige controle over de talen in uw reis gebruiker en de taalinstellingen van de browser van de klant worden genegeerd. 

Mogelijk moet u niet dat niveau van controle over welke talen uw klant ziet. Als u niet bieden een `ui_locales` parameter van de klant ervaring wordt bepaald door hun browserinstellingen.  U kunt nog steeds bepalen welke talen uw reis gebruiker wordt omgezet in door toe te voegen als een ondersteunde taal. Als de browser van de klant is ingesteld om weer te geven van een taal die u niet wilt ondersteunen, wordt in plaats daarvan de taal die u hebt geselecteerd als een standaard in ondersteunde culturen weergegeven.

- **UI-landinstellingen opgegeven taal**: nadat u de aanpassing van de taal hebt ingeschakeld, de gebruiker reis wordt omgezet in de taal die hier wordt opgegeven.
- **Browser gevraagde taal**: als er geen `ui_locales` parameter is opgegeven, de gebruiker reis wordt omgezet in de taal browser aangevraagd *als de taal ondersteund*.
- **De standaardtaal beleid**: als de browser een taal niet opgeven of deze bevat een die niet wordt ondersteund, het traject van de gebruiker wordt omgezet in de standaardtaal van het beleid.

>[!NOTE]
>Als u aangepaste gebruikerskenmerken gebruikt, moet u uw eigen vertalingen bieden. Zie voor meer informatie [aanpassen van uw tekenreeksen](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Ondersteuning voor talen aangevraagde voor ui_locales 
Beleidsregels die zijn gemaakt vóór de algemene beschikbaarheid van taal aanpassing moeten eerst deze functie inschakelen. Beleidsregels die zijn gemaakt na hebben taal aanpassing is standaard ingeschakeld. 

Als u aanpassing van de taal van een beleid inschakelt, kunt u de taal van de gebruiker reis bepalen door toe te voegen de `ui_locales` parameter.
1. [Ga naar de pagina van de B2C-functies in de Azure portal](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Blader naar een beleid dat u wilt inschakelen voor vertalingen.
3. Selecteer **taal aanpassing**.  
4. Selecteer **taal aanpassingen aanbrengen**.
5. Lees de informatie in het dialoogvenster en selecteer **Ja**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Selecteer welke talen in uw gebruiker reis zijn ingeschakeld 
Inschakelen van een set van talen voor de gebruiker reis moeten worden omgezet in wanneer de `ui_locales` parameter is niet opgegeven.
1. Zorg ervoor dat uw beleid taal aanpassing van de voorgaande instructies ingeschakeld.
2. Van de **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer een taal die u wilt ondersteunen.
4. Wijzig in het eigenschappendeelvenster **ingeschakeld** naar **Ja**.  
5. Selecteer **opslaan** aan de bovenkant van het eigenschappendeelvenster.

>[!NOTE]
>Als een `ui_locales` parameter wordt opgegeven, wordt de pagina wordt omgezet in de taal van de webbrowser van de klant alleen als deze is ingeschakeld.
>

## <a name="customize-your-strings"></a>Uw tekenreeksen aanpassen
Taal aanpassing kunt u een willekeurige tekenreeks in uw reis gebruiker aanpassen.
1. Zorg ervoor dat uw beleid voor aanpassing van de taal van de voorgaande instructies ingeschakeld.
2. Van de **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer de taal die u wilt aanpassen.
4. Selecteer de pagina die u wilt bewerken.
5. Selecteer **downloaden standaardwaarden** (of **onderdrukkingen downloaden** als u deze taal eerder hebt bewerkt). 

Deze stappen geven een JSON-bestand dat u gebruiken kunt om te beginnen met het bewerken van uw tekenreeksen.

### <a name="change-any-string-on-the-page"></a>Een willekeurige tekenreeks op de pagina wijzigen
1. Open het JSON-bestand dat is gedownload van de voorgaande instructies in een JSON-editor.
2. Zoek het element dat u wilt wijzigen.  U vindt `StringId` voor de tekenreeks die u zoekt, of zoek naar de `Value` kenmerk dat u wilt wijzigen.
3. Update de `Value` kenmerk met wat u weergeven wilt.
4. Elke tekenreeks die u wilt wijzigen, wijzigt u `Override` naar `true`.
5. Sla het bestand en uploadt u uw wijzigingen. (U vindt het uploaden van besturingselement in de plaats waar u het JSON-bestand hebt gedownload.) 

>[!IMPORTANT]
>Als u onderdrukken van een tekenreeks wilt, controleert u of in te stellen de `Override` van waarde naar `true`.  Als de waarde niet is gewijzigd, wordt de vermelding wordt genegeerd. 
>

### <a name="change-extension-attributes"></a>Extensie kenmerken wijzigen
Als u wilt de tekenreeks voor een aangepaste gebruikerskenmerk wijzigen of u wilt toevoegen aan de JSON, is het in de volgende indeling:
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

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Een lijst met waarden bieden met behulp van LocalizedCollections
Als u voor een setlijst met waarden voor antwoorden wilt, moet u maken een `LocalizedCollections` kenmerk.  `LocalizedCollections` is een matrix met `Name` en `Value` paren. Om toe te voegen `LocalizedCollections`, gebruik de volgende notatie:

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

* `ElementId` de gebruiker die dit kenmerk is `LocalizedCollections` kenmerk is een reactie op.
* `Name` is de waarde die aan de gebruiker worden weergegeven.
* `Value` is wat de claim wordt geretourneerd wanneer deze optie is geselecteerd.

### <a name="upload-your-changes"></a>Uw wijzigingen te uploaden
1. Nadat u de wijzigingen aan uw JSON-bestand hebt voltooid, gaat u terug naar uw B2C-tenant.
2. Van de **beleid bewerken** pagina **taal aanpassing**.
3. Selecteer de taal die u vertalen wilt naar.
4. Selecteer de pagina waar u vertalingen bieden.
5. Selecteer de mappictogram en selecteer het JSON-bestand te uploaden.
 
De wijzigingen zijn opgeslagen in uw beleid voor automatisch.

## <a name="customize-the-page-ui-by-using-language-customization"></a>De pagina gebruikersinterface aanpassen met behulp van taal aanpassing

Er zijn twee manieren om te lokaliseren uw HTML-inhoud. Een manier is om in te schakelen [taal aanpassing](active-directory-b2c-reference-language-customization.md). Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales`, naar het eindpunt.  De inhoudsserver kunt u deze parameter gebruiken voor aangepaste HTML-pagina's die een specifieke taal zijn.

U kunt ook inhoud ophalen vanaf verschillende locaties op basis van de landinstellingen die wordt gebruikt. U kunt een mapstructuur naar host-inhoud voor specifieke talen instellen in uw eindpunt CORS ingeschakeld. U moet de juiste is aanroepen als u het jokerteken `{Culture:RFC5646}`.  Stel bijvoorbeeld dat dit uw aangepaste URI-pagina is:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
U kunt de pagina in laden `fr`. Wanneer de pagina HTML en CSS inhoud ophaalt, wordt het binnenhalen van:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-locales"></a>Aangepaste landinstellingen toevoegen

U kunt ook talen die Microsoft biedt momenteel geen vertalingen voor toevoegen. U moet de vertalingen voor alle tekenreeksen in het beleid opgeven.

1. Van de **beleid bewerken** pagina **taal aanpassing**.
2. Selecteer **aangepaste taal toevoegen** vanaf de bovenkant van de pagina.
3. Identificeren welke taal bieden u vertalingen voor door een geldige landinstelling-code in het deelvenster context dat wordt geopend.
4. U kunt voor elke pagina downloaden van een reeks overschrijvingen voor Engels en werkt op de vertalingen.
5. Nadat u met de JSON-bestanden bent klaar, kunt u ze kunt uploaden voor elke pagina.
6. Selecteer **inschakelen**, en uw beleid voor deze taal voor uw gebruikers kan nu worden weergegeven.
7. Sla de taal.

## <a name="additional-information"></a>Aanvullende informatie

### <a name="page-ui-customization-labels-as-overrides"></a>Page UI aanpassing van labels als onderdrukkingen
Wanneer u een taal aanpassing inschakelt, worden de eerdere wijzigingen in met behulp van pagina UI-aanpassing van labels vastgehouden in een JSON-bestand voor Engels (en). U kunt doorgaan met het wijzigen van de labels en andere tekenreeksen door het uploaden van de taalbestanden in de taal aanpassing.
### <a name="up-to-date-translations"></a>Up-to-date vertalingen
Microsoft hecht te bieden de meest recente vertalingen voor het gebruik. Microsoft continu verbetert vertalingen en laat ze naleving voor u. Microsoft wordt identificeren fouten en wijzigingen in globale terminologie en -updates die werkt naadloos in uw reis gebruiker.
### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen van rechts naar links
Microsoft biedt geen momenteel ondersteuning bieden voor v.r.n.l.-talen. Als u deze functie nodig hebt, neem stemmen voor op [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sociale identiteit provider vertalingen
Microsoft biedt de `ui_locales` OIDC parameter aan sociale aanmeldingen. Maar bepaalde sociale identiteitsproviders, waaronder Facebook en Google en ze niet naleven. 
### <a name="browser-behavior"></a>Gedrag van de browser
Chrome en Firefox die zowel een aanvraag voor de taal van hun instellen. Als het een ondersteunde taal, wordt het weergegeven voordat de standaardwaarde. Rand momenteel geen een taal aanvraagt en wordt de meteen naar de standaardtaal.

### <a name="supported-languages"></a>Ondersteunde talen

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
