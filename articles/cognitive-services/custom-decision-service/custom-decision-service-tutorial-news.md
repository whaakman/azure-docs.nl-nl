---
title: Personalisatie - cognitieve Azure-Services artikel | Microsoft Docs
description: Een zelfstudie voor het artikel personalisatie met Azure-Service voor het besluit van aangepaste, een cloud-gebaseerde API voor de contextuele besluitvorming.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 35d0567f81a23d4726461059eb6fd31e04228697
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345476"
---
# <a name="article-personalization"></a>Artikel persoonlijke instellingen

Deze zelfstudie richt zich op de selectie van artikelen op de voorpagina van een website aanpassen. De aangepaste besluit-Service is van invloed op *meerdere* , bijvoorbeeld een lijst met artikelen op de voorgrond pagina. De pagina is mogelijk een nieuwswebsite die alleen politiek en sport behandelt. Deze drie gerangschikte lijsten met artikelen wilt weergeven: politiek, sport, en recente.

## <a name="applications-and-action-sets"></a>Toepassingen en actie sets

Ga als volgt aanpassen aan uw scenario in het kader. Stel, we drie toepassingen, één voor elke lijst die wordt geoptimaliseerd: app-politiek, sport-app, en app-recente. Als u de artikelen kandidaat voor elke toepassing, er zijn twee sets van de actie: één voor politiek en één voor sport. De actie die is ingesteld voor app-recente wordt automatisch aangeleverd als een samenvoeging van de andere twee sets.

> [!TIP]
> Actie sets kunnen worden gedeeld door toepassingen in aangepaste besluit Service.

## <a name="prepare-action-set-feeds"></a>Actie set feeds voorbereiden

Aangepaste besluit Service verbruikt actie sets via RSS- of Atom-feeds door de klant. Bieden van twee feeds: één voor politiek en één voor sport. Stel dat ze worden aangeboden via `http://www.domain.com/feeds/<feed-name>`.

Elke feed bevat een lijst met artikelen. In de RSS, elk criterium wordt opgegeven door een `<item>` element als volgt:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

De volgorde van de artikelen voor u belangrijk is. Hiermee wordt de standaard-volgorde die bij benadering voor hoe de artikelen moeten worden geordend. De standaard-volgorde wordt vervolgens gebruikt voor vergelijking van de prestaties op de [dashboard](#performance-dashboard).

Zie voor meer informatie over de feedindeling de [API-referentiemateriaal](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Een nieuwe app te registreren

1. Meld u aan met uw [Microsoft-account](https://account.microsoft.com/account). Klik op het lint op **mijn Portal**.

2. Voor het registreren van een nieuwe toepassing, klikt u op de **nieuwe App** knop.

    ![Custom Decision Service-portal](./media/custom-decision-service-tutorial/portal.png)

3. Voer een unieke naam voor uw toepassing in de **App-ID** in het tekstvak. Als deze naam al gebruikt door een andere klant wordt, wordt u gevraagd u kiest een andere app-ID. Selecteer de **Geavanceerd** en voer de [verbindingsreeks](../../storage/common/storage-configure-connection-string.md) voor uw Azure storage-account. Normaal gesproken u hetzelfde opslagaccount gebruiken voor al uw toepassingen.

    ![Dialoogvenster Nieuwe app](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Nadat u alle drie de toepassingen in het bovenstaande scenario hebt geregistreerd, worden ze weergegeven:

    ![Lijst met apps](./media/custom-decision-service-tutorial/apps.png)

    U kunt later terugkeren naar deze lijst door te klikken op de **Apps** knop.

4. In de **nieuwe App** dialoogvenster geeft u een actie-feed. Actie-feeds kunnen ook worden opgegeven door te klikken op de **Feeds** knop en door te klikken op de **nieuw kanaal** knop. Voer een **naam** voor de nieuwe feed, voert u de **URL** dat zij worden geleverd en voer de **vernieuwen tijd**. De vernieuwingstijd geeft aan hoe vaak aangepaste besluit Service de feed moet vernieuwen.

    ![Dialoogvenster nieuwe feed](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Actie-feeds kunnen worden gebruikt door een willekeurige app, ongeacht waar ze zijn opgegeven. Nadat u beide feeds actie in een scenario opgeeft, worden deze weergegeven:

    ![Lijst met feeds](./media/custom-decision-service-tutorial/feeds.png)

    U kunt later terugkeren naar deze lijst door te klikken op de **Feeds** knop.

## <a name="use-the-apis"></a>Gebruik de API 's

De beslissing aangepaste Service rang artikelen via de API positie. Voeg de volgende code in de HTML-kop van de front-pagina voor het gebruik van deze API:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

Het HTTP-antwoord van de API rangschikking is een tekenreeks JSONP-indeling. Voor app-politiek, bijvoorbeeld de tekenreeks ziet eruit als:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

De browser voert deze tekenreeks als een aanroep van de `callback()` functie. De `data` argument in de `callback()` functie bevat de app-ID en de volgorde van URL's moet worden gerenderd. In het bijzonder `callback()` moet gebruiken `data.appId` onderscheid maken tussen de drie toepassingen. `eventId` wordt intern gebruikt door aangepaste besluit Service overeenkomen met de opgegeven positie met de bijbehorende Klik, indien van toepassing.

> [!TIP]
> `callback()` wellicht controleren elke actie feed voor nieuwheid met behulp van de `lastRefresh` veld. Als een bepaalde feed niet voldoende vers is `callback()` kunt negeren van de opgegeven volgorde, deze feed rechtstreeks aanroepen en de standaard volgorde geleverd door de feed gebruiken.

Zie voor meer informatie over specificaties en aanvullende opties die worden geleverd door de API rangschikking de [API-referentiemateriaal](custom-decision-service-api-reference.md).

De keuzes best gewaardeerd artikel van de gebruiker worden geretourneerd door de derden-API aanroept. Wanneer een keuze best gewaardeerd artikel wordt ontvangen, moet de volgende code worden aangeroepen op de front-pagina:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Met behulp van `appId` en `eventId` vereist in de code voor de afhandeling van Klik zorgvuldig te werk gaan. U kunt bijvoorbeeld implementeren de `callback()` werkt als volgt:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

In dit voorbeeld implementeert de `render()` functie voor het weergeven van een bepaald artikel voor een bepaalde toepassing. Deze functie, ingevoerd door de app-ID en het artikel (in de indeling van de positie API). De `onClick` parameter is de functie die moet worden aangeroepen vanuit `render()` voor het afhandelen van een klik. Controleert deze of de Klik op de bovenste sleuf. Vervolgens roept de API van derden met de juiste app-ID en de gebeurtenis-ID.

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg de [API-referentiemateriaal](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit voor opgegeven.