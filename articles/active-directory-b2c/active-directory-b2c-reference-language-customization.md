---
title: 'Azure Active Directory B2C: Aanpassing van de taal met behulp | Microsoft Docs'
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: 3c7c49ee5fbd98762da0eef6f02e7c2f036453cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Aanpassing van de taal met behulp

>[!NOTE] 
>Deze functie is openbare preview.  Het is raadzaam dat u een testtenant gebruiken wanneer u deze functie.  We niet van plan bent van recente wijzigingen vanuit de evaluatieversie van in de algemene beschikbaarheid release, maar behoudt zich het recht om dergelijke wijzigingen voor het verbeteren van de functie te brengen.  Zodra u de functie uitproberen hebt gehad, geef feedback over uw ervaringen en hoe we kunt u beter een.  U kunt feedback via de Azure portal met de gezichtje face-hulpprogramma op de rechterbovenhoek.   Als er een zakelijke vereiste is om te worden gepubliceerd met behulp van deze functie tijdens de preview-fase laat ons weten uw scenario's en we u met de juiste hulp en ondersteuning opgeven kunnen.  U kunt contact met ons op [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Taal aanpassing kunt u uw gebruiker reis naar een andere taal om aan de klantbehoeften van uw te wijzigen.  We bieden vertalingen voor 36 talen (Zie [aanvullende informatie](#additional-information)).  Zelfs als uw ervaring is alleen beschikbaar voor één taal, kunt u alle tekst op de pagina's naar wens kunt aanpassen.  

## <a name="how-does-language-customization-work"></a>Hoe werkt taal aanpassing?
Taal aanpassing kunt u selecteren welke talen uw reis gebruiker is beschikbaar in.  Zodra de functie is ingeschakeld, kunt u de querytekenreeksparameter ui_locales, bieden van uw toepassing.  Wanneer u in Azure AD B2C aanroept, vertalen we uw pagina naar de landinstellingen die u hebt aangegeven.  Met behulp van het type configuratie, hebt u volledige controle over de talen in uw reis gebruiker en negeert de taalinstellingen van de browser van de klant. U kunt ook moet u mogelijk niet dat niveau van controle over welke talen Zie van uw klant.  Als u een ui_locales-parameter niet opgeeft, wordt de ervaring van de klant wordt bepaald door hun browserinstellingen.  U kunt nog steeds bepalen welke talen uw reis gebruiker wordt omgezet in door toe te voegen als een ondersteunde taal.  Als de browser van de klant is ingesteld op een taal weergeven die u niet wilt ondersteunen, wordt in plaats daarvan de taal die u hebt geselecteerd als een standaardwaarde in ondersteunde culturen weergegeven.

1. **UI-landinstellingen opgegeven taal** -wanneer u de aanpassing van de taal hebt ingeschakeld, de gebruiker reis wordt omgezet in de taal die u hier opgeeft
2. **Browser gevraagde taal** -als er geen gebruikersinterface-landinstellingen is opgegeven, worden omgezet naar de browser gevraagde taal, **als deze is opgenomen in de ondersteunde talen**
3. **De standaardtaal beleid** -als de browser een taal niet opgeven of deze bevat een die niet wordt ondersteund, worden omgezet naar de standaardtaal van beleid

>[!NOTE]
>Als u aangepaste gebruikerskenmerken gebruikt, moet u uw eigen vertalingen bieden. Zie '[aanpassen van uw tekenreeksen](#customize-your-strings)' voor meer informatie.
>

## <a name="support-uilocales-requested-languages"></a>Ondersteuning ui_locales aangevraagd talen 
Taal aanpassing van een beleid inschakelt, kunt u de taal van het traject van de gebruiker nu bepalen door de parameter ui_locales toe te voegen.
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Navigeer naar een beleid dat u wilt inschakelen voor vertalingen.
3. Klik op **taal aanpassing**.
4. Lees de waarschuwing zorgvuldig.  Eenmaal is ingeschakeld, kunt u 'Aanpassing taal' niet uitschakelen.
5. Schakel de functie en klik op **OK**.
6. Sla het beleid op de linkerbovenhoek van uw **beleid bewerken** blade.

## <a name="select-which-languages-your-user-journey-supports"></a>Selecteer de talen die ondersteuning biedt voor uw gebruiker reis 
Maak een lijst van toegestane talen voor de gebruiker reis worden vertaald wanneer de parameter ui_locales is niet opgegeven.
1. Zorg ervoor dat uw beleid heeft taal aanpassing' van de voorgaande instructies ingeschakeld.
2. Van uw **beleid bewerken** blade Selecteer **taal aanpassing**.
3. Gaat u naar uw **ondersteunde talen** blade.  Hier kunt u **taal toevoegen**.
4. Selecteer de talen die u wilt worden ondersteund.  

>[!NOTE]
>Als een parameter ui_locales niet is opgegeven, wordt klikt u vervolgens de pagina omgezet in de taal van de webbrowser van de klant alleen als op deze lijst
>

5. Klik op **Ok** onder
6. Sluit de **taal aanpassing** blade en **opslaan** uw beleid.

## <a name="customize-your-strings"></a>Uw tekenreeksen aanpassen
'Aanpassing taal' kunt u een willekeurige tekenreeks in uw reis gebruiker aanpassen.
1. Zorg ervoor dat uw beleid heeft taal aanpassing' van de voorgaande instructies ingeschakeld.
2. Van uw **beleid bewerken** blade Selecteer **taal aanpassing**.
3. Selecteer in het navigatiemenu links **inhoud downloaden**.
4. Selecteer de pagina die u wilt bewerken.
5. Selecteer de taal die u bewerken wilt voor in de vervolgkeuzelijst.
6. Klik op **Downloaden**. 

Deze stappen geven een JSON-bestand dat u gebruiken kunt om te beginnen met het bewerken van uw tekenreeksen.

### <a name="changing-any-string-on-the-page"></a>Een willekeurige tekenreeks op de pagina wijzigen
1. Open het JSON-bestand dat is gedownload van de voorgaande instructies in een JSON-editor.
2. Zoek het element dat u wilt wijzigen.  U vindt de `StringId` van de tekenreeks die u zoekt, of zoek naar de `Value` u wilt wijzigen.
3. Update de `Value` kenmerk met wat u weergeven wilt.
4. Sla het bestand en uploadt u uw wijzigingen.

### <a name="changing-extension-attributes"></a>Uitbreidingskenmerken wijzigen
Als u wilt wijzigen van de tekenreeks voor een aangepaste gebruikerskenmerk of wilt toevoegen aan de JSON, is het in de volgende indeling:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Als u onderdrukken van een tekenreeks wilt, controleert u of in te stellen de `Override` van waarde naar `true`.  Als de waarde niet is gewijzigd, wordt de vermelding wordt genegeerd. 
>

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
      "Override": false,
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
>[!IMPORTANT]
>Als u onderdrukken van een tekenreeks wilt, controleert u of in te stellen de `Override` van waarde naar `true`.  Als de waarde niet is gewijzigd, wordt de vermelding wordt genegeerd. 
>

* `ElementId`de gebruiker die dit kenmerk is `LocalizedCollections` antwoord op
* `Name`de waarde weergegeven voor de gebruiker
* `Value`wordt geretourneerd als deze optie is geselecteerd in de claim is

### <a name="upload-your-changes"></a>Uw wijzigingen te uploaden
1. Nadat u de wijzigingen aan uw JSON-bestand hebt, gaat u terug naar uw B2C-tenant.
2. Van uw **beleid bewerken** blade Selecteer **taal aanpassing**.
3. Selecteer in het navigatiemenu links **inhoud uploaden**.
4. Selecteer de pagina die u wilt uw wijzigingen voor uploaden.
5. Als u uploaden een taal die u eerder een JSON voor hebt opgegeven wilt, moet u het vorige item verwijderen.  U kunt het verwijderen door te klikken op de `...` aan de rechterkant van die taal en selecteer **verwijderen**.
6. Klik op **toevoegen** in de linkerbovenhoek.
7. Selecteer de taal van uw JSON-bestand.
8. Klik op de mapknop aan de rechterkant en bladert u naar uw JSON-bestand.
9. Klik op de **uploaden** knop aan de onderkant van de blade.
10. Ga terug naar uw **beleid bewerken** blade en klik op **opslaan**.

## <a name="additional-information"></a>Aanvullende informatie
### <a name="recommendations-for-language-customization"></a>Aanbevelingen voor 'Taal aanpassing'
Het is raadzaam om alleen plaatsen in de gegevens aan uw resources taal voor tekenreeksen die u expliciet wilt vervangen.  We afdwingen een maximale grootte voor het bestand dat is gecompileerd buiten alle JSON-vertalingen.  Als uw bestanden te groot is, heeft dit gevolgen voor de prestaties van uw reis gebruiker.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Page UI aanpassing van labels worden verwijderd zodra 'Aanpassing taal' is ingeschakeld
Wanneer u 'Aanpassing taal' hebt ingeschakeld, wordt uw vorige bewerkingen voor met Page UI-aanpassing van labels zijn verwijderd, met uitzondering van aangepaste kenmerken.  Deze wijziging wordt gedaan om te voorkomen van conflicten in waar u uw tekenreeksen kunt bewerken.  U kunt doorgaan met het wijzigen van de labels en andere tekenreeksen door het uploaden van de taalbestanden in 'Aanpassing taal'.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft waakt over de meest recente vertalingen voor het gebruik bieden
We continu verbeteren vertalingen en bewaar deze op naleving voor u.  We identificeren fouten en wijzigingen in globale terminologie en dat de updates die werkt probleemloos in uw reis gebruiker.
### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen van rechts naar links
Er is geen ondersteuning voor talen van rechts naar links, als u nodig hebt met deze functie moet stemmen voor deze functie op [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Vertalingen van de provider van sociale identiteit
We bieden de parameter ui_locales OIDC sociale aanmeldingen, maar ze worden niet herkend door sommige sociale identiteitsproviders, waaronder Facebook en Google. 
### <a name="browser-behavior"></a>Gedrag van de browser
Chrome en Firefox beide aanvragen voor de taal van hun instellen en als het een ondersteunde taal, deze wordt weergegeven voordat de standaardwaarde.  Rand momenteel geen een taal aanvraagt en wordt de meteen naar de standaardtaal.
### <a name="known-issues"></a>Bekende problemen
* Het uploaden van taalbronnen voor de MFA-pagina in een beleid profiel bewerken is momenteel niet beschikbaar.
* `LocalizedCollections`voor waarden niet worden gegenereerd wanneer dat nodig is voor het reactietype
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Wat gebeurt er als ik wil een taal die niet wordt ondersteund?
We van plan bent om een uitbreiding van deze functie die u kunt een JSON-resource naar 'aangepaste talen' uploaden.  De functie kunt u de naam- en taalinstellingen code opgeven voor elke taal en geef *alle* de vertalingen voor die taal.  Als u deze functie moet, stuur ons uw scenario op [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Welke talen worden ondersteund?

| Taal              | Taalcode |
|-----------------------|---------------|
| Bengaals                | bn            |
| Tsjechisch                 | cs            |
| Deens                | da            |
| Duits                | de            |
| Grieks                 | el            |
| Nederlands               | en            |
| Spaans               | es            |
| Fins               | fi            |
| Frans                | fr            |
| Gujarati              | Gu            |
| Hindi                 | Hallo            |
| Kroatisch              | HR            |
| Hongaars             | hu            |
| Italiaans               | it            |
| Japans              | ja            |
| Kannada               | kN            |
| Koreaans                | ko            |
| Malajalam             | ml            |
| Marathi               | MR            |
| Maleis                 | MS            |
| Noors, Bokmål      | nb            |
| Nederlands                 | nl            |
| Punjabi               | Pa            |
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
