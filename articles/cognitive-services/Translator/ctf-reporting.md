---
title: Microsoft Translator gezamenlijke vertaling Reporting Framework (CTF)
description: Het gebruik van de samenwerking vertaling Framework (CTF) rapportage.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: cefc630a82a56703ba4942bcad18f6e0a38b1ee5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344815"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>Het gebruik van samenwerking vertaling Framework (CTF) reporting

> [!NOTE]
> Deze methode is afgeschaft. Het is niet beschikbaar in V3.0 van de API van de tekst conversieprogramma.

> De samenwerking vertalingen Framework (CTF), voorheen beschikbaar voor V2.0 van de API van de tekst conversieprogramma is vanaf 1 februari 2018 afgeschaft. De functies AddTranslation en AddTranslationArray kunnen gebruikers inschakelen via de collectieve vertaling Framework correcties aan te brengen. Voorbeelden van nieuwe zin heeft niet geaccepteerd door deze twee functies na 31 januari 2018, en gebruikers een foutbericht. Deze functies zijn buiten gebruik gesteld en wordt niet vervangen. 

>Vergelijkbare functionaliteit is beschikbaar in de API conversieprogramma Hub, zodat u voor het bouwen van een aangepaste vertaling systeem met uw terminologie en stijl, en u kunt met behulp van de categorie-ID in de tekst conversieprogramma API aanroepen. Conversieprogramma Hub: [ https://hub.microsofttranslator.com ](https://hub.microsofttranslator.com). Conversieprogramma Hub API: [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger).

De samenwerking vertaling Framework (CTF) rapportage-API retourneert statistieken en de daadwerkelijke inhoud in het archief CTF. Deze API verschilt van de methode GetTranslations() omdat deze:
* Retourneert de vertaalde inhoud en het totale aantal alleen uit uw account (appId of Azure Marketplace-account).
* Retourneert de vertaalde inhoud en het totale aantal zonder een overeenkomst met de bron-zin.
* Retourneert de automatische vertaling (machinevertaling).

## <a name="endpoint"></a>Eindpunt
Het eindpunt van de CTF rapportage-API is http://api.microsofttranslator.com/v2/beta/ctfreporting.svc
                        

## <a name="methods"></a>Methoden
| Naam |    Beschrijving|
|:---|:---|
| GetUserTranslationCounts methode | Telt het aantal van de vertalingen die zijn gemaakt door de gebruiker worden opgehaald. |
| GetUserTranslations methode | Haalt de vertalingen die zijn gemaakt door de gebruiker. |

Deze methoden kunnen u:
* Ophalen van de volledige reeks gebruiker vertalingen en correcties aan te brengen onder uw account-ID voor downloaden.
* De lijst van de regelmatige inzenders verkrijgen. Zorg ervoor dat de juiste gebruikersnaam in AddTranslation() wordt verstrekt.
* Bouw een gebruikersinterface (UI) waarmee uw vertrouwde gebruikers wilt weergeven van alle beschikbare kandidaten, indien nodig, beperkt tot een deel van uw site, op basis van de voorvoegsels van URI.

> [!NOTE]
> De beide methoden zijn relatief langzaam en kostbaar proces. Het is raadzaam deze spaarzaam gebruiken.

## <a name="getusertranslationcounts-method"></a>GetUserTranslationCounts methode

Deze methode haalt de telling van vertalingen die zijn gemaakt door de gebruiker. Het bevat de lijst van de vertaling van tellingen gegroepeerd op de uriPrefix uit naar de gebruiker, minRating en maxRating aanvraagparameters.

**Syntaxis**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslationCount[]GetUserTranslationCounts(
           string appId,
           string uriPrefix,
           string from,
           string to,
           int? minRating,
           int? maxRating,
           string user, 
           string category
           DateTime? minDateUtc,
           DateTime? maxDateUtc,
           int? skip,
           int? take);
```

**Parameters**

| Parameter | Beschrijving |
|:---|:---|
| appId | **Vereist** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met 'Bearer' + ' ' + toegangstoken.|
| uriPrefix | **Optionele** een tekenreeks met het voorvoegsel van de URI van de vertaling.|
| uit | **Optionele** een tekenreeks voor de taalcode van de van Vertalingtekst. |
| tot | **Optionele** een tekenreeks voor de taalcode omzetten van de tekst in.|
| minRating| **Optionele** een integerwaarde voor de beoordeling van de minimale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen-10 en 10. De standaardwaarde is 1.|
| maxRating| **Optionele** een integerwaarde voor de beoordeling van de maximale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen-10 en 10. De standaardwaarde is 1.|
| Gebruiker | **Optionele** een tekenreeks die wordt gebruikt voor het filteren van het resultaat op basis van de afzender van het verzenden. |
| category| **Optionele** een tekenreeks met de categorie of het domein van de vertaling. Deze parameter ondersteunt alleen de standaardoptie algemeen.|
| minDateUtc| **Optionele** de datum uit als u wilt de vertalingen ophalen. De datum moet zich in de UTC-notatie. |
| maxDateUtc| **Optionele** de datum tot als u wilt de vertalingen ophalen. De datum moet zich in de UTC-notatie. |
| overslaan| **Optionele** het aantal resultaten dat u wilt overslaan op een pagina. Als u de overslaan op de eerste 20 rijen van de resultaten en de weergave van de resultaatrecord 21 wilt, bijvoorbeeld 20 voor deze parameter opgeven. De standaardwaarde voor deze parameter is 0.|
| duren | **Optionele** het aantal resultaten dat u wilt ophalen. Het maximum aantal elke aanvraag is 100. De standaardwaarde is 100.|

> [!NOTE]
> De aanvraagparameters overslaan en los het inschakelen paginering voor een groot aantal resultaatrecords.

**Retourwaarde**

De resultatenset bevat een matrix van de **UserTranslationCount**. Elke UserTranslationCount heeft de volgende elementen:

| Veld | Beschrijving |
|:---|:---|
| Count| Het aantal resultaten dat is opgehaald|
| Vanaf | De bron-taal|
| Waardering| De classificatie die wordt toegepast door de aanvrager in de methodeaanroep AddTranslation()|
| Handeling| De doel-taal|
| URI| De URI in de methodeaanroep AddTranslation() toegepast|
| Gebruiker| Naam van de gebruiker|

**Uitzonderingen**

| Uitzondering | Bericht | Voorwaarden |
|:---|:---|:---|
| ArgumentOutOfRangeException | De parameter '**maxDateUtc**'moet groter zijn dan of gelijk zijn aan'**minDateUtc**'.| De waarde van de parameter **maxDateUtc** is lager dan de waarde van de parameter **minDateUtc**.|
| TranslateApiException | IP-adres is via het quotum.| <ul><li>De limiet voor het aantal aanvragen per minuut is bereikt.</li><li>De aangevraagde grootte blijft beperkt 10000 tekens.</li><li>Een uur en een dagelijkse quotum beperken het aantal tekens dat de API van Microsoft Translator accepteert.</li></ul>|
| TranslateApiException | AppId is via het quotum.| De toepassings-ID overschreden de Uurlijkse of dagelijkse quotum.|

> [!NOTE]
> Het quotum wordt aangepast om ervoor te zorgen billijkheid door alle gebruikers van de service.

**Codevoorbeelden op GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>GetUserTranslations methode

Deze methode haalt de vertalingen die zijn gemaakt door de gebruiker. Het biedt dat de vertalingen gegroepeerd op de uriPrefix uit tot, gebruiker, en minRating en maxRating aanvraagparameters.

**Syntaxis**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslation[] GetUserTranslations (
            string appId,
            string uriPrefix,
            string from,
            string to,
            int? minRating,
            int? maxRating,
            string user, 
            string category
            DateTime? minDateUtc,
            DateTime? maxDateUtc,
            int? skip,
            int? take); 
```

**Parameters**

| Parameter | Beschrijving |
|:---|:---|
| appId | **Vereist** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met 'Bearer' + ' ' + toegangstoken.|
| uriPrefix| **Optionele** een tekenreeks met het voorvoegsel van de URI van de vertaling.|
| uit| **Optionele** een tekenreeks voor de taalcode van de van Vertalingtekst.|
| tot| **Optionele** een tekenreeks voor de taalcode omzetten van de tekst in.|
| minRating| **Optionele** een integerwaarde voor de beoordeling van de minimale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen-10 en 10. De standaardwaarde is 1.|
| maxRating| **Optionele** een integerwaarde voor de beoordeling van de maximale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen-10 en 10. De standaardwaarde is 1.|
| Gebruiker| **Optioneel. Een tekenreeks die wordt gebruikt voor het filteren van het resultaat op basis van de afzender van het verzenden**|
| category| **Optionele** een tekenreeks met de categorie of het domein van de vertaling. Deze parameter ondersteunt alleen de standaardoptie algemeen.| 
| minDateUtc| **Optionele** de datum uit als u wilt de vertalingen ophalen. De datum moet zich in de UTC-notatie.| 
| maxDateUtc| **Optionele** de datum tot als u wilt de vertalingen ophalen. De datum moet zich in de UTC-notatie.|
| overslaan| **Optionele** het aantal resultaten dat u wilt overslaan op een pagina. Als u de overslaan op de eerste 20 rijen van de resultaten en de weergave van de resultaatrecord 21 wilt, bijvoorbeeld 20 voor deze parameter opgeven. De standaardwaarde voor deze parameter is 0.|
| duren| **Optionele** het aantal resultaten dat u wilt ophalen. Het maximum aantal elke aanvraag is 100. De standaardwaarde is 50.|

> [!NOTE]
> De aanvraagparameters overslaan en los het inschakelen paginering voor een groot aantal resultaatrecords.

**Retourwaarde**

De resultatenset bevat een matrix van de **UserTranslation**. Elke UserTranslation heeft de volgende elementen:

| Veld | Beschrijving |
|:---|:---|
| CreatedDateUtc| De aanmaakdatum van de vermelding met AddTranslation()|
| Vanaf| De bron-taal|
| OriginalText| De van de bron-taaltekst die wordt gebruikt wanneer de aanvraag|
|Waardering |De classificatie die wordt toegepast door de aanvrager in de methodeaanroep AddTranslation()|
|Handeling|    De doel-taal|
|TranslatedText|    De vertaling verzonden in de methodeaanroep AddTranslation()|
|URI|   De URI in de methodeaanroep AddTranslation() toegepast|
|Gebruiker   |Naam van de gebruiker|

**Uitzonderingen**

| Uitzondering | Bericht | Voorwaarden |
|:---|:---|:---|
| ArgumentOutOfRangeException | De parameter '**maxDateUtc**'moet groter zijn dan of gelijk zijn aan'**minDateUtc**'.| De waarde van de parameter **maxDateUtc** is lager dan de waarde van de parameter **minDateUtc**.|
| TranslateApiException | IP-adres is via het quotum.| <ul><li>De limiet voor het aantal aanvragen per minuut is bereikt.</li><li>De aangevraagde grootte blijft beperkt 10000 tekens.</li><li>Een uur en een dagelijkse quotum beperken het aantal tekens dat de API van Microsoft Translator accepteert.</li></ul>|
| TranslateApiException | AppId is via het quotum.| De toepassings-ID overschreden de Uurlijkse of dagelijkse quotum.|

> [!NOTE]
> Het quotum wordt aangepast om ervoor te zorgen billijkheid door alle gebruikers van de service.

**Codevoorbeelden op GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)


















