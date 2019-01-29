---
title: Aanpassing van taal in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het aanpassen van de taal-ervaring.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 040f9b00b7f282789d8a63b9bbeb8ee7edfc779e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164143"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Aanpassing van taal in Azure Active Directory B2C

Aanpassing van taal in Azure Active Directory B2C (Azure AD B2C) kunt u uw beleid voor verschillende talen aan de klantbehoeften van uw.  Microsoft biedt de vertalingen voor [36 talen](#supported-languages), maar u kunt ook uw eigen vertalingen voor elke taal opgeven. Zelfs als uw ervaring is opgegeven voor slechts één taal, kunt u alle tekst op de pagina's aanpassen.  

## <a name="how-language-customization-works"></a>De werking van de aanpassing van taal
Aanpassing van taal kunt u selecteren welke taal de gebruikersstroom is beschikbaar in. Nadat de functie is ingeschakeld, kunt u de queryreeks-parameter, bieden `ui_locales`, van uw toepassing. Wanneer u in Azure AD B2C aanroepen, wordt de pagina wordt omgezet naar de landinstelling die u hebt aangegeven. Dit type configuratie hebt u volledige controle over de talen in uw beleid en de taalinstellingen van de browser van de klant worden genegeerd. 

U moet mogelijk niet dat niveau van controle over welke talen uw klant ziet. Als u niet bieden een `ui_locales` parameter van de klant ervaring wordt bepaald door de instellingen van hun browser.  U kunt nog steeds beheren welke taal de gebruikersstroom wordt omgezet naar door toe te voegen als een ondersteunde taal. Als de browser van een klant is ingesteld om weer te geven van een taal die u niet wilt ondersteunen, wordt in plaats daarvan de taal die u hebt geselecteerd als een standaard in ondersteunde culturen weergegeven.

- **UI-landinstellingen opgegeven taal**: Nadat u de aanpassing van taal hebt ingeschakeld, wordt de gebruikersstroom wordt omgezet naar de taal die hier opgegeven.
- **Browser gevraagde taal**: Als er geen `ui_locales` parameter is opgegeven, wordt de gebruikersstroom wordt omgezet naar de taal browser aangevraagd *als de taal die wordt ondersteund*.
- **De standaardtaal voor beleid**: Als de browser een taal niet opgeven, of deze bevat een die niet wordt ondersteund, wordt de gebruikersstroom wordt omgezet naar de standaardtaal van de gebruiker-stroom.

>[!NOTE]
>Als u aangepaste gebruikerskenmerken, moet u uw eigen vertalingen opgeven. Zie voor meer informatie, [aanpassen van uw tekenreeksen](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Aangevraagde talen ondersteuning voor ui_locales 
Beleidsregels die zijn gemaakt vóór de algemene beschikbaarheid van de aanpassing van taal moeten eerst deze functie inschakelt. Beleid en de gebruikersstromen die zijn gemaakt na hebben aanpassing van taal is standaard ingeschakeld. 

Als u aanpassing van taal op een beleid inschakelt, kunt u de taal van de gebruikersstroom bepalen door toe te voegen de `ui_locales` parameter.
1. Selecteer in uw Azure AD B2C-tenant, **gebruikersstromen**.
2. Klik op de gebruikersstroom die u wilt inschakelen voor vertalingen.
3. Selecteer **talen**.  
4. Selecteer **Schakel aanpassing van talen**.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Selecteer welke talen in de gebruikersstroom worden ingeschakeld 
Inschakelen van een reeks talen voor de gebruikersstroom om te zetten wanneer aangevraagd door de browser zonder de `ui_locales` parameter.
1. Zorg ervoor dat de gebruikersstroom aanpassing van taal van de vorige instructies hebt ingeschakeld.
2. Op de **talen** pagina voor de gebruikersstroom, selecteert u een taal die u wilt ondersteunen.
3. Wijzig in het eigenschappendeelvenster met **ingeschakeld** naar **Ja**.  
4. Selecteer **opslaan** aan de bovenkant van het deelvenster met eigenschappen.

>[!NOTE]
>Als een `ui_locales` parameter niet is opgegeven, wordt de pagina wordt omgezet naar de klant browsertaal alleen als deze is ingeschakeld.
>

## <a name="customize-your-strings"></a>De tekenreeksen aanpassen
Aanpassing van taal kunt u een willekeurige tekenreeks in de gebruikersstroom aanpassen.
1. Zorg ervoor dat de gebruikersstroom aanpassing van taal van de vorige instructies hebt ingeschakeld.
2. Op de **talen** pagina voor de gebruikersstroom, selecteer de taal die u wilt aanpassen.
3. Onder **pagina-niveau-resourcebestanden**, selecteer de pagina die u wilt bewerken.
4. Selecteer **standaardwaarden downloaden** (of **overschrijvingen downloaden** als u deze taal eerder hebt bewerkt).

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
2. Selecteer **gebruikersstromen** en klikt u op de gebruikersstroom die u wilt inschakelen voor vertalingen.
3. Selecteer **talen**.
4. Selecteer de taal die u omzetten wilt in.
5. Selecteer de pagina waar u vertalingen opgeven.
6. Selecteer het mappictogram en selecteer het JSON-bestand te uploaden.
 
De wijzigingen zijn opgeslagen in de gebruikersstroom automatisch.

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

U kunt ook de talen die Microsoft op dit moment biedt geen vertalingen voor toevoegen. U moet de vertalingen voor alle tekenreeksen in de gebruikersstroom opgeven.  Taal en landinstellingen codes zijn beperkt tot die in de ISO 639-1-standaard. 

1. Selecteer in uw Azure AD B2C-tenant, **gebruikersstromen**.
2. Klik op de gebruikersstroom waar u aangepaste talen toevoegen en klik vervolgens op **talen**.
3. Selecteer **aangepaste taal toevoegen** vanaf de bovenkant van de pagina.
4. Identificeren welke taal u bent vertalingen voor die door een geldige landinstelling-code in het deelvenster context die wordt geopend.
5. U kunt voor elke pagina downloaden van een set met onderdrukkingen voor Engels en werken op de vertalingen.
6. Als u klaar bent met de JSON-bestanden, kunt u ze kunt uploaden voor elke pagina.
7. Selecteer **inschakelen**, en de gebruikersstroom kan nu weergeven voor deze taal voor uw gebruikers.
8. Sla de taal.

>[!IMPORTANT]
>U moet de aangepaste talen inschakelen of overschrijvingen voor het uploaden voordat u kunt opslaan.
>

## <a name="additional-information"></a>Aanvullende informatie

### <a name="page-ui-customization-labels-as-overrides"></a>Page UI-aanpassing van labels als onderdrukkingen
Wanneer u aanpassing van taal inschakelt, worden uw vorige bewerkingen voor de labels met behulp van pagina voor UI-aanpassing voor Engels (en) persistent gemaakt in een JSON-bestand. U kunt de labels en andere tekenreeksen wijzigen door de taalbestanden in de aanpassing van taal uploaden.
### <a name="up-to-date-translations"></a>Recente vertalingen
Microsoft is vinden het belangrijk dat de meest recente vertalingen voor het gebruik. Microsoft verbetert de vertalingen continu en houdt u deze in acht genomen voor u. Microsoft identificeren bugs en wijzigingen in de algemene terminologie en -updates die werkt naadloos in uw beleid maken.
### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen van rechts naar links
Microsoft biedt op dit moment geen ondersteuning voor talen van rechts naar links. U kunt dit doen met behulp van aangepaste landinstellingen en CSS gebruiken om te wijzigen van de manier waarop die de tekenreeksen worden weergegeven.  Als u deze functie nodig hebt, neem stemt u erop op [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Vertalingen van sociale id-provider
Microsoft biedt de `ui_locales` OIDC-parameter voor sociale aanmeldingen. Maar sommige sociale id-providers, zoals Facebook en Google en worden ze niet gehandhaafd. 
### <a name="browser-behavior"></a>Gedrag van de browser
Chrome en Firefox die beide voor hun eigen taal instellen aanvragen. Als dat een ondersteunde taal voor het geval is, wordt dit weergegeven voordat de standaardwaarde. Microsoft Edge wordt momenteel heeft geen aanvragen voor een taal en gaat rechtstreeks naar de standaardtaal.

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
| Kanarees               | kN            |
| Koreaans                | ko            |
| Malayalam             | ml            |
| Mahratti               | MR            |
| Maleis                 | ms            |
| Noors Bokmal      | nb            |
| Nederlands                 | nl            |
| Punjabi               | pa            |
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
