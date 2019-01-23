---
title: Sjablonen
description: Dit onderwerp vindt sjablonen voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 02473eb5649c7d201b6a54fd57faea997c1a21cc
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450231"
---
# <a name="templates"></a>Sjablonen

Sjablonen kunnen een clienttoepassing om op te geven van de precieze indeling van het bedrijf wil ontvangen van meldingen. Met behulp van sjablonen, kan een app meerdere verschillende voordelen, waaronder de volgende voordelen:

- Een platformonafhankelijke back-end
- Persoonlijke meldingen
- Onafhankelijkheid van de versie van de client
- Eenvoudig lokalisatie

Deze sectie bevat twee gedetailleerde voorbeelden van hoe u sjablonen voor het verzenden van platformonafhankelijke meldingen die gericht zijn op al uw apparaten op platforms en aanpassen van broadcast kennisgeving op elk apparaat.

## <a name="using-templates-cross-platform"></a>Met behulp van sjablonen cross-platform

Er is de standaardmethode voor het verzenden van pushmeldingen te verzenden, voor elk bericht dat moet worden verzonden, een specifieke nettolading platform notification Services (WNS, APNS). Als u wilt een waarschuwing naar APNS verzendt, is de nettolading van de bijvoorbeeld een JSON-object van de volgende notatie:

```json
{"aps": {"alert" : "Hello!" }}
```

Voor het verzenden van een soortgelijke toast-bericht op een Windows Store-toepassing, is de XML-nettolading als volgt uit:

```xml
<toast>
  <visual>
    <binding template=\"ToastText01\">
      <text id=\"1\">Hello!</text>
    </binding>
  </visual>
</toast>
```

U kunt vergelijkbare nettoladingen maken voor MPNS (Windows Phone) en FCM (Android)-platformen.

Deze vereiste zorgt ervoor dat de back-end voor het produceren van verschillende nettoladingen voor elk platform en effectief kunt u de back-end die verantwoordelijk is voor een deel van de presentatielaag van de app. Sommige problemen zijn lokalisatie en grafische lay-outs (met name voor Windows Store-apps die meldingen voor verschillende soorten tegels bevatten).

De functie van de sjabloon voor Notification Hubs kunt een client-app voor het maken van speciale registraties, sjabloonregistraties, met onder andere, naast de set-tags van een sjabloon met de naam. De functie van de sjabloon Notification Hubs kunt een client-app apparaten koppelen met sjablonen, of u met installaties werkt nu (bij voorkeur) of registraties. Gezien de voorgaande voorbeelden van nettolading, is de informatie die alleen platformonafhankelijke het werkelijke waarschuwingsbericht (Hallo!). Een sjabloon is een set met instructies voor de Notification Hub voor het formatteren van een bericht onafhankelijk is van platform voor de registratie van dat specifieke client-app. In het voorgaande voorbeeld wordt het bericht platformonafhankelijke één eigenschap: `message = Hello!`.

De volgende afbeelding illustreert het proces:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

De sjabloon voor de app-registratie van iOS-client is als volgt:

```json
{"aps": {"alert": "$(message)"}}
```

De bijbehorende sjabloon voor de Windows Store-client-app is:

```xml
<toast>
    <visual>
        <binding template=\"ToastText01\">
            <text id=\"1\">$(message)</text>
        </binding>
    </visual>
</toast>
```

U ziet dat het werkelijke bericht is vervangen door de expressie $(bericht). Deze expressie Hiermee geeft u de Notification Hub, wanneer deze een bericht naar deze bepaalde registratie verzendt, om te maken van een bericht die volgt op deze en switches in de algemene waarde.

Als u met installatiemodel werkt, bevat de sleutel van de "sjablonen" installatie van een JSON van meerdere sjablonen. Als u met registratie-model werkt, kunt de clienttoepassing meerdere rapporten maken als u wilt gebruiken, meerdere sjablonen. bijvoorbeeld, een sjabloon voor waarschuwingsberichten en een sjabloon voor updates van de tegel. Clienttoepassingen kunnen ook worden gecombineerd systeemeigen registraties (registraties en geen sjabloon) en sjabloonregistraties.

De Notification Hub verzendt een melding voor elke sjabloon zonder rekening te houden of ze deel uitmaken van dezelfde clientapp. Dit gedrag kan worden gebruikt voor de omzetting van platformonafhankelijke meldingen naar meer meldingen. Bijvoorbeeld, kan hetzelfde platform-onafhankelijke bericht met de Notification Hub worden naadloos omgezet in een toast-melding en een tegel, zonder dat de back-end moet op de hoogte van het. Sommige platformen (bijvoorbeeld iOS) kunnen meerdere meldingen samenvouwen op hetzelfde apparaat als ze worden verzonden in een korte periode.

## <a name="using-templates-for-personalization"></a>Met behulp van sjablonen voor persoonlijke instellingen

Met behulp van sjablonen in een ander voordeel is de mogelijkheid Notification Hubs gebruiken om uit te voeren van per-registratie van persoonlijke instellingen voor meldingen. Neem bijvoorbeeld een app weer die een tegel met de weersomstandigheden op een specifieke locatie weergeeft. Een gebruiker kan kiezen tussen graden Celsius of Fahrenheit en een prognose van één of vijf dagen. Met behulp van sjablonen, de installatie van elke client-app kunt registreren voor de vereiste indeling (1 dag Celsius, 1 dag Fahrenheit, 5 dagen Celsius, 5 dagen Fahrenheit), en de back-end van een enkel bericht dat de gegevens die zijn vereist voor het vullen van deze sjablonen bevat verzenden (bijvoorbeeld een voorspellen met graden Celsius en Fahrenheit vijf dagen).

De sjabloon voor de prognose eendaagse met Celsius temperaturen is als volgt:

```xml
<tile>
  <visual>
    <binding template="TileWideSmallImageAndText04">
      <image id="1" src="$(day1_image)" alt="alt text"/>
      <text id="1">Seattle, WA</text>
      <text id="2">$(day1_tempC)</text>
    </binding>  
  </visual>
</tile>
```

Het bericht verzonden naar de Notification Hub bevat de volgende eigenschappen:

```html
<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>
```

Met behulp van dit patroon, verzendt de back-end alleen een enkel bericht zonder hoeft op te slaan van de opties voor specifieke persoonlijke instellingen voor de appgebruikers. De volgende afbeelding ziet u dit scenario:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Informatie over het registreren van sjablonen

Als u wilt registreren met behulp van de installatie-model (bij voorkeur), of de registratie-sjablonen, Zie [registratiebeheer](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Sjabloon expressietaal

Sjablonen zijn beperkt tot XML of JSON-document-indelingen. Ook, u alleen expressies kunt plaatsen op bepaalde plaatsen; bijvoorbeeld, knooppunt kenmerken of waarden voor XML, de tekenreeks die eigenschapswaarden voor JSON.

De volgende tabel ziet u de taal die is toegestaan in sjablonen:

| Expressie       | Description |
| ---------------- | --- |
| $(prop)          | Verwijzing naar de gebeurteniseigenschap van een met de opgegeven naam. Eigenschapnamen zijn niet hoofdlettergevoelig. Deze expressie wordt omgezet naar de waarde van de tekst of een lege tekenreeks als de eigenschap niet aanwezig is. |
| $(prop, n)       | Zoals hierboven, maar de tekst expliciet is afgekapt op n tekens, bijvoorbeeld $(titel, 20) clips de inhoud van de titeleigenschap 20 tekens. |
| . (prop, n)       | Zoals hierboven, maar de tekst wordt voorafgegaan door de drie punten als deze wordt afgekapt. De totale grootte van de afgekapte tekenreeks en het achtervoegsel wordt niet meer dan n tekens. . (title, 20) met een eigenschap van 'Dit is de van titelregel' leidt tot **dit is de titel...** |
| %(Prop)          | Vergelijkbaar zijn met $(name), behalve dat de uitvoer URI-codering is. |
| #(prop)          | In JSON-sjablonen gebruikt (bijvoorbeeld: voor iOS en Android-sjablonen).<br><br>Deze functie werkt precies hetzelfde als $(prop) eerder hebt opgegeven, behalve wanneer gebruikt in de JSON-sjablonen (bijvoorbeeld een Apple-sjablonen). In dit geval, als deze functie niet wordt omgeven door '{','}' (bijvoorbeeld 'myJsonProperty': '#(naam)'), en dit resulteert in een getal in Javascript-indeling, bijvoorbeeld regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*)) (\.&#91;0-9&#93;+)? ((e&#124;E) (+&#124;-)? &#91;0-9&#93;+)?, en vervolgens de JSON-uitvoer een getal is.<br><br>Bijvoorbeeld ' badge: '#(naam)' wordt de badge': 40 (en niet 40). |
| 'text' of 'tekst' | Een letterlijke tekenreeks. Letterlijke waarden bevatten willekeurige tekst tussen enkele of dubbele aanhalingstekens. |
| expr1 + expr2    | De operator voor tekenreekssamenvoeging lid worden van twee expressies in een enkele tekenreeks. |

De expressies kunnen dit een van de voorgaande formulieren.

Bij het gebruik van de samenvoeging met de volledige expressie moet tussen `{}`. Bijvoorbeeld `{$(prop) + ‘ - ’ + $(prop2)}`.

Bijvoorbeeld, is de volgende sjabloon niet een geldig XML-sjabloon:

```xml
<tile>
  <visual>
    <binding $(property)>
      <text id="1">Seattle, WA</text>
    </binding>  
  </visual>
</tile>
```

Zoals eerder uitgelegd bij het gebruik van samengevoegd, moeten expressies tussen accolades worden geplaatst. Bijvoorbeeld:

```xml
<tile>
  <visual>
    <binding template="ToastText01">
      <text id="1">{'Hi, ' + $(name)}</text>
    </binding>  
  </visual>
</tile>
```
