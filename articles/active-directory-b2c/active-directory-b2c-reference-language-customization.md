---
title: Aanpassing van taal in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het aanpassen van de taal-ervaring.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: affd52352dcc745557dd66c61ccfa1e7a99dcdb7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442318"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Aanpassing van taal in Azure Active Directory B2C

Aanpassing van taal in Azure Active Directory B2C (Azure AD B2C) kunt u uw beleid voor verschillende talen aan de klantbehoeften van uw.  Microsoft biedt de vertalingen voor [36 talen](#supported-languages), maar u kunt ook uw eigen vertalingen voor elke taal opgeven. Zelfs als uw ervaring is opgegeven voor slechts één taal, kunt u alle tekst op de pagina's aanpassen.  

## <a name="how-language-customization-works"></a>De werking van de aanpassing van taal
Aanpassing van taal kunt u selecteren welke taal de gebruikersbeleving is beschikbaar in. Nadat de functie is ingeschakeld, kunt u de queryreeks-parameter, bieden `ui_locales`, van uw toepassing. Wanneer u in Azure AD B2C aanroepen, wordt de pagina wordt omgezet naar de landinstelling die u hebt aangegeven. Dit type configuratie hebt u volledige controle over de talen in de gebruikersbeleving en de taalinstellingen van de browser van de klant worden genegeerd. 

U moet mogelijk niet dat niveau van controle over welke talen uw klant ziet. Als u niet bieden een `ui_locales` parameter van de klant ervaring wordt bepaald door de instellingen van hun browser.  U kunt nog steeds beheren welke taal de gebruikersbeleving wordt omgezet naar door toe te voegen als een ondersteunde taal. Als de browser van een klant is ingesteld om weer te geven van een taal die u niet wilt ondersteunen, wordt in plaats daarvan de taal die u hebt geselecteerd als een standaard in ondersteunde culturen weergegeven.

- **UI-landinstellingen opgegeven taal**: nadat u de aanpassing van taal hebt ingeschakeld, de gebruikersbeleving wordt omgezet naar de taal die hier opgegeven.
- **Browser gevraagde taal**: als er geen `ui_locales` parameter is opgegeven, wordt de gebruikersbeleving wordt omgezet naar de taal browser aangevraagd *als de taal die wordt ondersteund*.
- **De standaardtaal voor beleid**: als de browser een taal niet opgeven, of deze bevat een die niet wordt ondersteund, de gebruikersbeleving wordt omgezet naar de standaardtaal van het beleid.

>[!NOTE]
>Als u aangepaste gebruikerskenmerken, moet u uw eigen vertalingen opgeven. Zie voor meer informatie, [aanpassen van uw tekenreeksen](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Aangevraagde talen ondersteuning voor ui_locales 
Beleidsregels die zijn gemaakt vóór de algemene beschikbaarheid van de aanpassing van taal moeten eerst deze functie inschakelt. Beleidsregels die zijn gemaakt na hebben aanpassing van taal is standaard ingeschakeld. 

Als u aanpassing van taal in een beleid inschakelt, kunt u de taal van de gebruikersbeleving bepalen door toe te voegen de `ui_locales` parameter.
1. [Ga naar de pagina van de B2C-functies in Azure portal](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Blader naar een beleid dat u wilt inschakelen voor vertalingen.
3. Selecteer **aanpassing van taal**.  
4. Selecteer **Schakel aanpassing van talen**.
5. Lees de informatie in het dialoogvenster en selecteer **Ja**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Selecteer welke talen in de gebruikersbeleving worden ingeschakeld 
Inschakelen van een reeks talen voor de gebruikersbeleving om te zetten wanneer aangevraagd door de browser zonder de `ui_locales` parameter.
1. Zorg ervoor dat uw beleid voor aanpassing van taal van de vorige instructies hebt ingeschakeld.
2. Uit de **beleid bewerken** weergeeft, schakelt **aanpassing van taal**.
3. Selecteer een taal die u wilt ondersteunen.
4. Wijzig in het eigenschappendeelvenster met **ingeschakeld** naar **Ja**.  
5. Selecteer **opslaan** aan de bovenkant van het deelvenster met eigenschappen.

>[!NOTE]
>Als een `ui_locales` parameter niet is opgegeven, wordt de pagina wordt omgezet naar de klant browsertaal alleen als deze is ingeschakeld.
>

## <a name="customize-your-strings"></a>De tekenreeksen aanpassen
Aanpassing van taal kunt u een willekeurige tekenreeks in de gebruikersbeleving aanpassen.
1. Zorg ervoor dat uw beleid voor aanpassing van taal van de vorige instructies hebt ingeschakeld.
2. Uit de **beleid bewerken** weergeeft, schakelt **aanpassing van taal**.
3. Selecteer de taal die u wilt aanpassen.
4. Selecteer de pagina die u wilt bewerken.
5. Selecteer **standaardwaarden downloaden** (of **overschrijvingen downloaden** als u deze taal eerder hebt bewerkt). 

Deze stappen kunnen u een JSON-bestand dat u gebruiken kunt om te beginnen met het bewerken van de tekenreeksen.

### <a name="change-any-string-on-the-page"></a>Een willekeurige tekenreeks op de pagina wijzigen
1. Open het JSON-bestand gedownload van de voorgaande instructies in een JSON-editor.
2. Zoek het element dat u wilt wijzigen.  U vindt `StringId` voor de tekenreeks die u zoekt, of zoek naar de `Value` kenmerk dat u wilt wijzigen.
3. Update de `Value` kenmerk met wat u weergeven wilt.
4. Voor elke tekenreeks is die u wilt wijzigen, wijzigt u `Override` naar `true`.
5. Sla het bestand en uploadt u uw wijzigingen. (U vindt het besturingselement voor het uploaden in de plaats waar u het JSON-bestand hebt gedownload.) 

>[!IMPORTANT]
>Als u overschrijven van een tekenreeks wilt, controleert u of om in te stellen de `Override` waarde die moet worden `true`.  Als de waarde is niet gewijzigd, wordt de vermelding wordt genegeerd. 
>

### <a name="change-extension-attributes"></a>Extensiekenmerken wijzigen
Als u wilt wijzigen van de tekenreeks voor een aangepaste gebruikerskenmerk of u wilt toevoegen aan de JSON, is het in de volgende indeling:
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

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Geef een lijst met waarden met behulp van LocalizedCollections
Als u voor een setlijst met waarden voor antwoorden wilt, moet u maken een `LocalizedCollections` kenmerk.  `LocalizedCollections` is een matrix van `Name` en `Value` paren. De volgorde voor de artikelen worden de volgorde die ze worden weergegeven.  Om toe te voegen `LocalizedCollections`, gebruik de volgende indeling:

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

* `ElementId` is de gebruiker van het kenmerk dat dit `LocalizedCollections` kenmerk is een reactie op.
* `Name` is de waarde die wordt weergegeven aan de gebruiker.
* `Value` is wordt geretourneerd als deze optie is geselecteerd in de claim.

### <a name="upload-your-changes"></a>Uw wijzigingen te uploaden
1. Nadat u de wijzigingen naar uw JSON-bestand hebt voltooid, gaat u terug naar uw B2C-tenant.
2. Uit de **beleid bewerken** weergeeft, schakelt **aanpassing van taal**.
3. Selecteer de taal die u omzetten wilt in.
4. Selecteer de pagina waar u vertalingen opgeven.
5. Selecteer het mappictogram en selecteer het JSON-bestand te uploaden.
 
De wijzigingen worden automatisch opgeslagen in uw beleid.

## <a name="customize-the-page-ui-by-using-language-customization"></a>De gebruikersinterface van de pagina aanpassen met behulp van taalaanpassing

Er zijn twee manieren om uw HTML-inhoud te lokaliseren. Één manier is om in te schakelen [aanpassing van taal](active-directory-b2c-reference-language-customization.md). Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales`, aan uw eindpunt.  Uw server voor webinhoud kunt u deze parameter gebruiken voor aangepaste HTML-pagina's die specifieke taal.

U kunt ook inhoud ophaalt uit verschillende plaatsen op basis van de landinstelling die wordt gebruikt. U kunt een mapstructuur op de host-inhoud voor specifieke talen instellen in uw eindpunt CORS is ingeschakeld. U moet de juiste is aanroepen als u het jokerteken `{Culture:RFC5646}`.  Bijvoorbeeld, wordt ervan uitgegaan dat dit de URI voor uw aangepaste pagina is:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
U kunt de pagina in laden `fr`. Wanneer de pagina HTML en CSS-inhoud haalt, wordt het binnenhalen van:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Aangepaste talen toevoegen

U kunt ook de talen die Microsoft op dit moment biedt geen vertalingen voor toevoegen. U moet de vertalingen voor de tekenreeksen in het beleid opgeven.  Taal en landinstellingen codes zijn beperkt tot die in de ISO 639-1-standaard. 

1. Uit de **beleid bewerken** weergeeft, schakelt **aanpassing van taal**.
2. Selecteer **aangepaste taal toevoegen** vanaf de bovenkant van de pagina.
3. Identificeren welke taal u bent vertalingen voor die door een geldige landinstelling-code in het deelvenster context die wordt geopend.
4. U kunt voor elke pagina downloaden van een set met onderdrukkingen voor Engels en werken op de vertalingen.
5. Als u klaar bent met de JSON-bestanden, kunt u ze kunt uploaden voor elke pagina.
6. Selecteer **inschakelen**, en deze taal voor uw gebruikers kan nu weergeven door het beleid.
7. Sla de taal.

>[!IMPORTANT]
>U moet de aangepaste talen inschakelen of overschrijvingen voor het uploaden voordat u kunt opslaan.
>

## <a name="additional-information"></a>Aanvullende informatie

### <a name="page-ui-customization-labels-as-overrides"></a>Page UI-aanpassing van labels als onderdrukkingen
Wanneer u aanpassing van taal inschakelt, worden uw vorige bewerkingen voor de labels met behulp van pagina voor UI-aanpassing voor Engels (en) persistent gemaakt in een JSON-bestand. U kunt de labels en andere tekenreeksen wijzigen door de taalbestanden in de aanpassing van taal uploaden.
### <a name="up-to-date-translations"></a>Recente vertalingen
Microsoft is vinden het belangrijk dat de meest recente vertalingen voor het gebruik. Microsoft verbetert de vertalingen continu en houdt u deze in acht genomen voor u. Microsoft identificeren bugs en wijzigingen in de algemene terminologie en -updates die werkt naadloos in de gebruikersbeleving maken.
### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen van rechts naar links
Microsoft biedt op dit moment geen ondersteuning voor talen van rechts naar links. U kunt dit doen met behulp van aangepaste landinstellingen en CSS gebruiken om te wijzigen van de manier waarop die de tekenreeksen worden weergegeven.  Als u deze functie nodig hebt, neem stemt u erop op [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Vertalingen van sociale id-provider
Microsoft biedt de `ui_locales` OIDC-parameter voor sociale aanmeldingen. Maar sommige sociale id-providers, zoals Facebook en Google en worden ze niet gehandhaafd. 
### <a name="browser-behavior"></a>Gedrag van de browser
Chrome en Firefox die beide voor hun eigen taal instellen aanvragen. Als dat een ondersteunde taal voor het geval is, wordt dit weergegeven voordat de standaardwaarde. Edge op dit moment heeft geen aanvragen voor een taal en gaat rechtstreeks naar de standaardtaal.

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
| Hindi                 | Hallo            |
| Kroatisch              | uur            |
| Hongaars             | hu            |
| Italiaans               | it            |
| Japans              | ja            |
| Kanarees               | kN            |
| Koreaans                | ko            |
| Malayalam             | ml            |
| Mahratti               | MR            |
| Maleis                 | ms            |
| Noors Bokmal      | nb            |
| Nederlands                 | nl            |
| Punjabi               | Pa            |
| Pools                | pl            |
| Portugees - Brazilië   | pt-br         |
| Portugees - Portugal | pt-pt         |
| Roemeens              | ro            |
| Russisch               | ru            |
| Slowaaks                | SK            |
| Zweeds               | sv            |
| Tamil                 | TA            |
| Telugu                | Ken            |
| Thais                  | e            |
| Turks               | TR            |
| Chinees - Vereenvoudigd  | zh-hans       |
| Chinees - Traditioneel | zh-hant       |
