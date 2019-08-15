---
title: Taal aanpassing in Azure Active Directory B2C
description: Meer informatie over het aanpassen van de taal ervaring in uw gebruikers stromen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bced7a4b994172a1a2076149d6f25adb39c99b54
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015569"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Taal aanpassing in Azure Active Directory B2C

Door de taal aanpassing in Azure Active Directory B2C (Azure AD B2C) kan uw gebruikers stroom voorzien in verschillende talen die aan de behoeften van uw klant voldoen. Micro soft biedt de vertalingen voor [36 talen](#supported-languages), maar u kunt ook uw eigen vertalingen voor elke taal opgeven. Zelfs als uw ervaring slechts voor één taal wordt gegeven, kunt u tekst op de pagina's aanpassen.

## <a name="how-language-customization-works"></a>De werking van taal aanpassing

U gebruikt taal aanpassing om te selecteren in welke talen uw gebruikers stroom beschikbaar is. Nadat de functie is ingeschakeld, kunt u de query teken reeks parameter opgeven `ui_locales`, vanuit uw toepassing. Wanneer u Azure AD B2C aanroept, wordt uw pagina vertaald naar de land instellingen die u hebt opgegeven. Dit type configuratie geeft u volledige controle over de talen in uw gebruikers stroom en negeert de taal instellingen van de browser van de klant.

Mogelijk hebt u niet het controle niveau nodig over de talen die uw klant ziet. Als u geen `ui_locales` para meter opgeeft, wordt de gebruikers ervaring bepaald door de instellingen van de browser. U kunt nog steeds bepalen in welke talen uw gebruikers stroom wordt vertaald door deze toe te voegen als een ondersteunde taal. Als de browser van een klant is ingesteld om een taal weer te geven die u niet wilt ondersteunen, wordt in plaats daarvan de taal weer gegeven die u als standaard waarde hebt geselecteerd in ondersteunde cult uren.

* **gebruikers interface-land instellingen opgegeven taal**: Nadat u de taal aanpassing hebt ingeschakeld, wordt uw gebruikers stroom vertaald naar de taal die hier is opgegeven.
* Door **browser aangevraagde taal**: Als er `ui_locales` geen para meter is opgegeven, wordt uw gebruikers stroom vertaald naar de door de browser aangevraagde taal, *als de taal wordt ondersteund*.
* **Standaard taal van beleid**: Als de browser geen taal opgeeft, of als er een wordt opgegeven die niet wordt ondersteund, wordt de gebruikers stroom vertaald naar de standaard taal van de gebruikers stroom.

> [!NOTE]
> Als u aangepaste gebruikers kenmerken gebruikt, moet u uw eigen vertalingen opgeven. Zie [uw teken reeksen aanpassen](#customize-your-strings)voor meer informatie.

## <a name="support-requested-languages-for-ui_locales"></a>Ondersteuning voor aangevraagde talen voor ui_locales

Beleids regels die zijn gemaakt voor de algemene Beschik baarheid van taal aanpassing, moeten deze functie eerst inschakelen. Beleids regels en gebruikers stromen die zijn gemaakt nadat de taal aanpassing standaard is ingeschakeld.

Wanneer u taal aanpassing inschakelt voor een gebruikers stroom, kunt u de taal van de gebruikers stroom beheren door `ui_locales` de para meter toe te voegen.

1. Selecteer in uw Azure AD B2C-Tenant **gebruikers stromen**.
1. Klik op de gebruikers stroom die u wilt inschakelen voor vertalingen.
1. Selecteer **talen**.
1. Selecteer **taal aanpassing inschakelen**.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Selecteren welke talen in uw gebruikers stroom zijn ingeschakeld

Schakel een set talen in voor uw gebruikers stroom die moet worden omgezet naar wanneer dit wordt aangevraagd door de `ui_locales` browser zonder de para meter.

1. Zorg ervoor dat de taal aanpassing van uw gebruikers stroom is ingeschakeld vanuit de vorige instructies.
1. Selecteer op de pagina **talen** voor de gebruikers stroom een taal die u wilt ondersteunen.
1. Wijzig in het deel venster Eigenschappen de **instelling ingeschakeld** in **Ja**.
1. Selecteer **Opslaan** boven in het deel venster Eigenschappen.

>[!NOTE]
>Als er `ui_locales` geen para meter wordt gegeven, wordt de pagina alleen vertaald naar de browser taal van de klant als deze is ingeschakeld.
>

## <a name="customize-your-strings"></a>Uw teken reeksen aanpassen

Door taal aanpassing kunt u een wille keurige teken reeks in uw gebruikers stroom aanpassen.

1. Zorg ervoor dat de taal aanpassing voor uw gebruikers stroom is ingeschakeld in de vorige instructies.
1. Selecteer op de pagina **talen** voor de gebruikers stroom de taal die u wilt aanpassen.
1. Selecteer onder **bronnen bestanden op pagina niveau**de pagina die u wilt bewerken.
1. Selecteer **standaard waarden downloaden** (of **down loads negeren** als u deze taal eerder hebt bewerkt).

Deze stappen geven u een JSON-bestand dat u kunt gebruiken om te beginnen met het bewerken van uw teken reeksen.

### <a name="change-any-string-on-the-page"></a>Een wille keurige teken reeks op de pagina wijzigen

1. Open het JSON-bestand dat u hebt gedownload van de vorige instructies in een JSON-editor.
1. Zoek het element dat u wilt wijzigen. U kunt zoeken `StringId` naar de teken reeks die u zoekt, of zoeken naar het `Value` kenmerk dat u wilt wijzigen.
1. Werk het `Value` kenmerk bij met wat u wilt weer geven.
1. Voor elke teken reeks die u wilt wijzigen in, `Override` wijzigt `true`u in.
1. Sla het bestand op en upload uw wijzigingen. (U kunt het upload beheer vinden op dezelfde locatie als waar u het JSON-bestand hebt gedownload.)

> [!IMPORTANT]
> Als u een teken reeks moet overschrijven, moet u ervoor zorgen dat `Override` u de `true`waarde instelt op. Als de waarde niet wordt gewijzigd, wordt de vermelding genegeerd.

### <a name="change-extension-attributes"></a>Extensie kenmerken wijzigen

Als u de teken reeks voor een aangepast gebruikers kenmerk wilt wijzigen of een wilt toevoegen aan de JSON, heeft dit de volgende notatie:

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

Vervang `<ExtensionAttribute>` door de naam van uw aangepaste gebruikers kenmerk.

Vervang `<ExtensionAttributeValue>` door de nieuwe teken reeks die moet worden weer gegeven.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Een lijst met waarden opgeven met behulp van LocalizedCollections

Als u een lijst met waarden voor antwoorden wilt opgeven, moet u een `LocalizedCollections` kenmerk maken. `LocalizedCollections`is een matrix van `Name` en `Value` -paren. De volg orde van de items is de volg orde waarin ze worden weer gegeven. Gebruik de `LocalizedCollections`volgende indeling om toe te voegen:

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

* `ElementId`is het gebruikers kenmerk waarvan dit `LocalizedCollections` kenmerk een antwoord is.
* `Name`is de waarde die wordt weer gegeven voor de gebruiker.
* `Value`is wat wordt geretourneerd in de claim wanneer deze optie is geselecteerd.

### <a name="upload-your-changes"></a>Uw wijzigingen uploaden

1. Nadat u de wijzigingen in het JSON-bestand hebt voltooid, gaat u terug naar de B2C-Tenant.
1. Selecteer **gebruikers stromen** en klik op de gebruikers stroom die u wilt inschakelen voor vertalingen.
1. Selecteer **talen**.
1. Selecteer de taal waarin u wilt vertalen.
1. Selecteer de pagina waar u vertalingen wilt opgeven.
1. Selecteer het mappictogram en selecteer het JSON-bestand dat u wilt uploaden.

De wijzigingen worden automatisch opgeslagen in de stroom van uw gebruikers.

## <a name="customize-the-page-ui-by-using-language-customization"></a>De pagina gebruikers interface aanpassen met behulp van taal aanpassing

Er zijn twee manieren om uw HTML-inhoud te lokaliseren. Een manier is om [taal aanpassing](active-directory-b2c-reference-language-customization.md)in te scha kelen. Als u deze functie inschakelt, kunnen Azure AD B2C de OpenID Connect `ui-locales`Connect-para meter, naar uw eind punt door sturen. De inhouds server kan deze para meter gebruiken om aangepaste HTML-pagina's te bieden die specifiek zijn voor een specifieke taal.

U kunt ook inhoud vanaf verschillende locaties ophalen op basis van de land instelling die wordt gebruikt. In het eind punt waarvoor CORS is ingeschakeld, kunt u een mapstructuur instellen om inhoud voor specifieke talen te hosten. U belt het juiste nummer als u de Joker teken waarde `{Culture:RFC5646}`gebruikt. Stel dat dit uw aangepaste pagina-URI is:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```

U kunt de pagina laden in `fr`. Wanneer de pagina HTML-en CSS-inhoud haalt, wordt deze opgehaald uit:

```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Aangepaste talen toevoegen

U kunt ook talen toevoegen waarnaar micro soft momenteel geen vertalingen levert. U moet de vertalingen opgeven voor alle teken reeksen in de gebruikers stroom. Taal-en land codes zijn beperkt tot die in de ISO 639-1-standaard.

1. Selecteer in uw Azure AD B2C-Tenant **gebruikers stromen**.
2. Klik op de gebruikers stroom waaraan u aangepaste talen wilt toevoegen en klik vervolgens op **talen**.
3. Selecteer **aangepaste taal toevoegen** boven aan de pagina.
4. In het context venster dat wordt geopend, identificeert u in welke taal u de vertalingen levert door een geldige land instelling in te voeren.
5. Voor elke pagina kunt u een set onderdrukkingen downloaden voor Engels en werken aan de vertalingen.
6. Nadat u klaar bent met de JSON-bestanden, kunt u deze voor elke pagina uploaden.
7. Selecteer **inschakelen**en uw gebruikers stroom kan deze taal nu weer geven voor uw gebruikers.
8. Sla de taal op.

>[!IMPORTANT]
>Voordat u kunt opslaan, moet u de aangepaste talen inschakelen of overschrijvingen uploaden.

## <a name="additional-information"></a>Aanvullende informatie

### <a name="page-ui-customization-labels-as-overrides"></a>Labels voor pagina-UI aanpassen als onderdrukkingen

Wanneer u taal aanpassing inschakelt, worden uw vorige wijzigingen voor labels met de aanpassing van de pagina-UI persistent gemaakt in een JSON-bestand voor Engels (en). U kunt door gaan met het wijzigen van uw labels en andere teken reeksen door taal resources te uploaden bij het aanpassen van de taal.

### <a name="up-to-date-translations"></a>Up-to-date vertalingen

Micro soft streeft ernaar de meest recente vertalingen te leveren voor uw gebruik. Micro soft verbetert de vertalingen voortdurend en houdt ze op de naleving voor u. Micro soft identificeert bugs en wijzigingen in globale terminologie en maakt updates die naadloos werken in uw gebruikers stroom.

### <a name="support-for-right-to-left-languages"></a>Ondersteuning voor talen die van rechts naar links worden geschreven

Micro soft biedt momenteel geen ondersteuning voor talen die van rechts naar links worden geschreven. U kunt dit doen door aangepaste land instellingen te gebruiken en CSS te gebruiken voor het wijzigen van de manier waarop de teken reeksen worden weer gegeven. Als u deze functie nodig hebt, kunt u hieraan stemmen voor [Azure feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).

### <a name="social-identity-provider-translations"></a>Vertalingen van de sociale ID-provider

Micro soft biedt `ui_locales` de para meter OIDC aan sociale aanmeldingen. Maar sommige aanbieders van sociale identiteiten, waaronder Facebook en Google, voldoen niet aan deze providers.

### <a name="browser-behavior"></a>Browser gedrag

Chrome en Firefox hebben beide aanvragen voor de ingestelde taal. Als het een ondersteunde taal is, wordt deze weer gegeven vóór de standaard waarde. Micro soft Edge vraagt momenteel geen taal aan en gaat rechtstreeks naar de standaard taal.

## <a name="supported-languages"></a>Ondersteunde talen

Azure AD B2C biedt ondersteuning voor de volgende talen. De talen van de gebruikers stroom worden door Azure AD B2C verschaft. De multi-factor Authentication (MFA)-meldingen talen worden door [Azure MFA](../active-directory/authentication/concept-mfa-howitworks.md)verschaft.

| Taal              | Taalcode | Gebruikersstromen         | MFA-meldingen  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arabisch                | p.a.            | BxDxH                | :heavy_check_mark: |
| Bulgaars             | bg            | BxDxH                | :heavy_check_mark: |
| Bengalees                | bn            | :heavy_check_mark: | BxDxH                |
| Catalaans               | certificering            | BxDxH                | :heavy_check_mark: |
| Tsjechisch                 | cs            | :heavy_check_mark: | :heavy_check_mark: |
| Deens                | da            | :heavy_check_mark: | :heavy_check_mark: |
| Duits                | de            | :heavy_check_mark: | :heavy_check_mark: |
| Grieks                 | el            | :heavy_check_mark: | :heavy_check_mark: |
| Engels               | nl-NL            | :heavy_check_mark: | :heavy_check_mark: |
| Spaans               | es            | :heavy_check_mark: | :heavy_check_mark: |
| Estisch              | et            | BxDxH                | :heavy_check_mark: |
| Baskisch                | EU            | BxDxH                | :heavy_check_mark: |
| Fins               | fi            | :heavy_check_mark: | :heavy_check_mark: |
| Frans                | fr            | :heavy_check_mark: | :heavy_check_mark: |
| Galicisch              | boekhoud            | BxDxH                | :heavy_check_mark: |
| Gujarati              | gu            | :heavy_check_mark: | BxDxH                |
| Hebreeuws                | Hij            | BxDxH                | :heavy_check_mark: |
| Hindi                 | Hallo            | :heavy_check_mark: | :heavy_check_mark: |
| Kroatisch              | uur            | :heavy_check_mark: | :heavy_check_mark: |
| Hongaars             | hu            | :heavy_check_mark: | :heavy_check_mark: |
| Indonesisch            | id            | BxDxH                | :heavy_check_mark: |
| Italiaans               | it            | :heavy_check_mark: | :heavy_check_mark: |
| Japans              | ja            | :heavy_check_mark: | :heavy_check_mark: |
| Kazachs                | kk            | BxDxH                | :heavy_check_mark: |
| Kanarees               | kn            | :heavy_check_mark: | BxDxH                |
| Koreaans                | ko            | :heavy_check_mark: | :heavy_check_mark: |
| Litouws            | lt            | BxDxH                | :heavy_check_mark: |
| Lets               | lv            | BxDxH                | :heavy_check_mark: |
| Malayalam             | ml            | :heavy_check_mark: | BxDxH                |
| Marathi               | Dhr            | :heavy_check_mark: | BxDxH                |
| Maleis                 | ms            | :heavy_check_mark: | :heavy_check_mark: |
| Noors Bokmål      | nb            | :heavy_check_mark: | BxDxH                |
| Nederlands                 | nl            | :heavy_check_mark: | :heavy_check_mark: |
| Noors             | nee            | BxDxH                | :heavy_check_mark: |
| Punjabi               | PA            | :heavy_check_mark: | BxDxH                |
| Pools                | pl            | :heavy_check_mark: | :heavy_check_mark: |
| Portugees-Brazilië   | pt-br         | :heavy_check_mark: | :heavy_check_mark: |
| Portugees-Portugal | pt-pt         | :heavy_check_mark: | :heavy_check_mark: |
| Roemeens              | ro            | :heavy_check_mark: | :heavy_check_mark: |
| Russisch               | ru            | :heavy_check_mark: | :heavy_check_mark: |
| Slowaaks                | SK            | :heavy_check_mark: | :heavy_check_mark: |
| Sloveens             | lineaire            | BxDxH                | :heavy_check_mark: |
| Servisch-cyrillisch    | SR-Cryl-CS    | BxDxH                | :heavy_check_mark: |
| Servisch-Latijn       | SR-latn-cs    | BxDxH                | :heavy_check_mark: |
| Zweeds               | sv            | :heavy_check_mark: | :heavy_check_mark: |
| Tamil                 | &            | :heavy_check_mark: | BxDxH                |
| Telugu                | Ken            | :heavy_check_mark: | BxDxH                |
| Thais                  | e            | :heavy_check_mark: | :heavy_check_mark: |
| Turks               | formulieren            | :heavy_check_mark: | :heavy_check_mark: |
| Oekraïens             | rijk            | BxDxH                | :heavy_check_mark: |
| Vietnamees            | beschreven            | BxDxH                | :heavy_check_mark: |
| Chinees-vereenvoudigd  | zh-hans       | :heavy_check_mark: | :heavy_check_mark: |
| Chinees-traditioneel | zh-hant       | :heavy_check_mark: | :heavy_check_mark: |
