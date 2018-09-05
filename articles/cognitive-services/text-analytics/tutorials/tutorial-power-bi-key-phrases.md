---
title: Text Analytics met Power BI - Azure Cognitive Services | Microsoft Docs
description: Leer hoe u Text Analytics gebruikt om belangrijke woordgroepen te extraheren uit tekst die in Power BI is opgeslagen.
services: cognitive-services
author: luiscabrer
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: tutorial
ms.date: 3/07/2018
ms.author: luisca
ms.openlocfilehash: 2cdb93d44218627efdcb0360d8cf4a4eeeca177a
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42889264"
---
# <a name="text-analytics-with-power-bi"></a>Text Analytics met Power BI

Microsoft Power BI distilleert organisatiegegevens in schitterende rapporten, die u in uw hele organisatie kunt distribueren voor sneller, grondiger inzicht. De Text Analytics-service, onderdeel van Cognitive Services in Microsoft Azure, kan de belangrijkste woordgroepen uit tekst extraheren via zijn Key Phrases-API. Samen kunnen deze hulpmiddelen u helpen snel te zien waar uw klanten het over hebben en wat ze ervan vinden.

In deze zelfstudie ziet u hoe u Power BI Desktop en de Key Phrases-API kunt integreren om de belangrijkste woordgroepen uit klantenfeedback te extraheren via een aangepaste Power Query-functie. We maken ook een woordwolk van deze woordgroepen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

> [!div class="checklist"]
> * Microsoft Power BI Desktop. [Download dit gratis](https://powerbi.microsoft.com/get-started/).
> * Een Microsoft Azure-account. [Start een gratis proefversie](https://azure.microsoft.com/free/) of [meld u aan](https://portal.azure.com/).
> * Een toegangssleutel voor Text Analytics. [Meld u aan](../../cognitive-services-apis-create-account.md) en [haal uw sleutel op](../how-tos/text-analytics-how-to-access-key.md).
> * Opmerkingen van klanten. [Download onze voorbeeldgegevens](https://aka.ms/cogsvc/ta) of gebruik uw eigen.

## <a name="loading-customer-data"></a>Klantgegevens laden

Om aan de slag te gaan, opent u Power BI Desktop en laadt u het CSV-bestand (door komma’s gescheiden waarden) **FabrikamComments.csv**. Dit bestand vertegenwoordigt een hele dag aan hypothetische activiteit op een ondersteuningsforum van een fictief klein bedrijf.

> [!NOTE]
> Power BI kan gegevens gebruiken uit vele verschillende bronnen, zoals Facebook of een SQL-database. Meer informatie in [Facebook-integratie met Power BI](https://powerbi.microsoft.com/integrations/facebook/) en [SQL Server-integratie met Power BI](https://powerbi.microsoft.com/integrations/sql-server/).

Ga in het hoofdvenster van Power BI Desktop naar de groep Externe gegevens op het Start-lint. Kies **Tekst/csv** in de vervolgkeuzelijst **Gegevens ophalen** in deze groep.

![[De knop Gegevens ophalen]](../media/tutorials/power-bi/get-data-button.png)

Het dialoogvenster Openen verschijnt. Navigeer naar de map Downloads, of welke map dan ook het bestand met voorbeeldgegevens bevat. Klik op `FabrikamComments.csv` en klik vervolgens op de knop **Openen**. Het dialoogvenster CSV-import verschijnt.

![[Het dialoogvenster CSV-import]](../media/tutorials/power-bi/csv-import.png)

In het dialoogvenster CSV-import kunt u verifiëren dat Power BI Desktop de tekenset, het scheidingsteken, de veldnamenrijen en de kolomtypen correct heeft gedetecteerd. Deze gegevens kloppen allemaal, dus we klikken op **Laden**.

Om de geladen gegevens te zien, schakelt u naar de gegevensweergave met de knop Gegevensweergave aan de linkerkant van de Power BI-werkruimte. Er wordt een tabel geopend die onze gegevens bevat, net zoals in Microsoft Excel.

![[De eerste weergave van de geïmporteerde gegevens]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="preparing-the-data"></a>De gegevens voorbereiden

U moet uw gegevens in Power BI Desktop mogelijk transformeren voordat ze klaar zijn om te worden verwerkt door de Key Phrases-service.

Onze gegevens bevatten bijvoorbeeld zowel een veld `subject` als een veld `comment`. We moeten tekst in allebei de velden beschouwen, niet alleen in `comment`, wanneer we belangrijke woordgroepen extraheren. De functie Kolommen samenvoegen in Power BI Desktop maakt deze taak gemakkelijk.

Open de Query-editor in het hoofdvenster van Power BI Desktop door te klikken op **Query’s bewerken** in de groep Externe gegevens op het Start-lint. 

![[De groep Externe gegevens op het Start-lint]](../media/tutorials/power-bi/edit-queries.png)

Selecteer ‘FabrikamComments’ in de lijst Query’s aan de linkerkant van het venster als dit nog niet is geselecteerd.

Selecteer nu zowel de kolom `subject` als `comment` in de tabel. U moet mogelijk horizontaal scrollen om deze kolommen te zien. Klik eerst op de kolomkop `subject`, houd vervolgens de Ctrl-toets ingedrukt en klik op de kolomkop `comment`.

![[Samen te voegen velden selecteren]](../media/tutorials/power-bi/select-columns.png)

Klik op **Kolommen samenvoegen** in de groep Tekstkolommen op het lint Transformeren. Het dialoogvenster Kolommen samenvoegen verschijnt.

![[Velden samenvoegen via het dialoogvenster Kolommen samenvoegen]](../media/tutorials/power-bi/merge-columns.png)

Kies in het dialoogvenster Kolommen samenvoegen Tab als het scheidingsteken en klik vervolgens op **OK**. Klaar!

U kunt ook overwegen lege berichten te verwijderen via het filter Leeg verwijderen of niet-afdrukbare tekens te verwijderen via de transformatie Opschonen. Als uw gegevens een kolom bevatten zoals de kolom `spamscore` in ons voorbeeldbestand, kunt u ‘spam’-opmerkingen overslaan met behulp van een getalfilter.

## <a name="understanding-the-api"></a>De API begrijpen

De Key Phrases-API kan maximaal duizend tekstdocumenten per HTTP-aanvraag verwerken. Power BI verwerkt records echter liever met één tegelijk, dus onze aanroepen naar de API zullen slechts één document bevatten. [De API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) vereist de volgende velden voor elk document dat wordt verwerkt.

| | |
| - | - |
| `id`  | Een unieke ID voor dit document in de aanvraag. Het antwoord bevat dit veld ook. Zo kunt u bij het verwerken van meerdere documenten de geëxtraheerde belangrijke woordgroepen gemakkelijk koppelen aan het document waaruit ze kwamen. Wij verwerken één document per aanvraag, dus we weten al aan welk document het antwoord is gekoppeld, en `id` kan hetzelfde zijn in elke aanvraag.|
| `text`  | De tekst die moet worden verwerkt. We halen deze uit de kolom `Merged` die we hebben gemaakt, die de gecombineerde onderwerpregel en opmerkingstekst bevat. De Key Phrases-API vereist dat deze gegevens niet langer dan ongeveer 5.000 tekens zijn.|
| `language` | De code die de taal vertegenwoordigt waarin het document is geschreven. Al onze berichten zijn in het Engels, dus we kunnen de taal `en` vastleggen in onze query.|

We moeten deze velden combineren in een JSON-document (JavaScript Object Notation) voor verzending naar de Key Phrases-API. Het antwoord van deze aanvraag is ook een JSON-document, en we moeten dit parseren en de belangrijke woordgroepen eruit vissen.

## <a name="creating-a-custom-function"></a>Een aangepaste functie maken

We zijn nu klaar om de aangepaste functie te maken die Power BI en Text Analytics zal integreren. Power BI Desktop roept de functie aan voor elke rij van de tabel en maakt een nieuwe kolom met de resultaten.

De functie ontvangt de te verwerken tekst als een parameter. Het converteert gegevens naar en uit de vereiste JavaScript Object Notation (JSON) en maakt de HTTP-aanvraag naar het eindpunt van de Key Phrases-API. Nadat de functie het antwoord heeft geparseerd, retourneert deze een tekenreeks die een door komma’s gescheiden lijst met de geëxtraheerde belangrijke woordgroepen bevat.

> [!NOTE]
> Aangepaste Power BI Desktop-functies worden geschreven in de [formuletaal Power Query M](https://msdn.microsoft.com/library/mt211003.aspx), of kort gezegd gewoon ‘M’. M is een functionele programmeertaal die op [F#](https://docs.microsoft.com/dotnet/fsharp/) is gebaseerd. U hoeft echter geen programmeur te zijn om deze studieles te kunnen voltooien; de benodigde code staat hieronder.

U moet zich nog steeds in het venster Query-editor bevinden. Klik op het Start-lint op **Nieuwe bron** (in de groep Nieuwe query) en kies **Lege query** in de vervolgkeuzelijst. 

Er verschijnt een nieuwe query, aanvankelijk Query1 genaamd, in de lijst Query’s. Dubbelklik op deze query en wijzig de naam in `KeyPhrases`.

Open nu het venster Geavanceerde editor door te klikken op **Geavanceerde editor** in de groep Query op het Start-lint. Verwijder de code die al in dat venster staat, en plak de volgende code. 

> [!NOTE]
> In de onderstaande voorbeelden gaan we ervan uit dat het eindpunt zich bevindt op https://westus.api.cognitive.microsoft.com.  Text Analytics ondersteunt het maken van een abonnement in 13 verschillende regio’s. Als u zich in een andere regio voor de service hebt geregistreerd, gebruik dan het eindpunt voor de regio die u hebt geselecteerd. Dit moet op de pagina Overzicht in de Azure-portal staan wanneer u uw Text Analytics-abonnement selecteert.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Plak ook uw API-sleutel voor Text Analytics, opgehaald uit het Microsoft Azure-dashboard, in de regel die begint met `apikey`. (Zorg dat u de aanhalingstekens voor en na de sleutel laat staan.) Klik vervolgens op **Gereed**.

## <a name="using-the-function"></a>De functie gebruiken

We kunnen de aangepaste functie nu gebruiken om de belangrijke woordgroepen in elk van onze klantopmerkingen op te halen en in een nieuwe kolom in de tabel op te slaan. 

Terwijl u zich nog steeds in de Query-editor bevindt, schakelt u terug naar de FabrikamComments-query, gaat u naar het lint Kolom toevoegen en klikt u op de knop **Aangepaste functie aanroepen** in de groep Algemeen.

![[Knop Aangepaste functie aanroepen]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

In het dialoogvenster Aangepaste functie aanroepen voert u `keyphrases` als naam van de nieuwe kolom in. Kies onze aangepaste functie, `KeyPhrases`, als de Functiequery. 

Er verschijnt een nieuw veld in het dialoogvenster, waarin wordt gevraagd welk veld we aan onze functie willen doorgeven als de parameter `text`. Kies `Merged` (de kolom die we eerder hebben gemaakt door het onderwerp- en berichtveld samen te voegen) uit de vervolgkeuzelijst.

![[Een aangepaste functie aanroepen]](../media/tutorials/power-bi/invoke-custom-function.png)

Klik ten slotte op **OK**.

Als alles klaar is, roept Power BI onze functie eenmaal voor elke rij in onze tabel aan, waarbij de Key Phrases-query’s worden uitgevoerd en de nieuwe kolom aan de tabel wordt toegevoegd. Maar voordat dat gebeurt, moet u mogelijk verificatie- en privacyinstellingen specificeren.

## <a name="authentication-and-privacy"></a>Verificatie en privacy

Nadat u het dialoogvenster Aangepaste functie aanroepen hebt gesloten, verschijnt er mogelijk een banner waarin u wordt gevraagd op te geven hoe er verbinding moet worden gemaakt met het Key Phrases-eindpunt.

![[referentiebanner]](../media/tutorials/power-bi/credentials-banner.png)

Klik op **Referenties bewerken**, zorg dat Anoniem is geselecteerd in het dialoogvenster en klik vervolgens op **Verbinding maken**. 

> [!NOTE]
> Waarom anoniem? De Text Analytics-service verifieert u met behulp van de API-sleutel, zodat Power BI zelf geen referenties voor de HTTP-aanvraag hoeft te bieden.

![[verificatie instellen op anoniem]](../media/tutorials/power-bi/access-web-content.png)

Als u de banner Referenties bewerken zelfs ziet nadat u anonieme toegang hebt gekozen, bent u mogelijk vergeten uw API-sleutel te plakken. Controleer de aangepaste functie `KeyPhrases` in de Geavanceerde editor om te zien of de API-sleutel daar staat.

Vervolgens verschijnt er mogelijk een banner waarin u wordt gevraagd informatie op te geven over de privacy van uw gegevensbronnen. 

![[privacybanner]](../media/tutorials/power-bi/privacy-banner.png)

Klik op **Doorgaan** en kies Openbaar voor elk van de gegevensbronnen in het dialoogvenster. Klik vervolgens op **Opslaan**.

![[privacy van gegevensbronnen instellen]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="creating-the-word-cloud"></a>De woordwolk maken

Zodra u eventueel verschenen banners hebt afgehandeld, klikt u op **Sluiten en toepassen** op het Start-lint om de Query-editor te sluiten.

Het duurt even voordat Power BI Desktop de benodigde HTTP-aanvragen heeft gemaakt. Voor elke rij in de tabel bevat de nieuwe kolom `keyphrases` de belangrijke woordgroepen die de Key Phrases-API in de tekst heeft gedetecteerd. 

Laten we deze kolom gebruiken om een woordwolk te genereren. Om aan de slag te gaan, klikt u in het hoofdvenster van Power BI Desktop op de knop Rapport, links van de werkruimte.

> [!NOTE]
> Waarom zou u geëxtraheerde belangrijke woordgroepen gebruiken om een woordwolk te genereren, in plaats van de volledige tekst van elke opmerking? De belangrijke woordgroepen bieden ons de *belangrijke* woorden uit onze klantopmerkingen, niet alleen de *meest voorkomende* woorden. Bovendien is de woordgrootte in de resulterende wolk niet vertekend door het veelvuldig gebruik van een woord in een relatief klein aantal opmerkingen.

Als de aangepaste visual Word Cloud nog niet is geïnstalleerd, installeer deze dan. Klik in het deelvenster Visualisaties rechts van de werkruimte op de drie puntjes (**...**) en kies **Importeren vanuit Marketplace**. Zoek vervolgens naar ‘cloud’ en klik op de knop **Toevoegen** naast de Word Cloud-visual. Power BI installeert de Word Cloud-visual en laat u weten dat deze succesvol is geïnstalleerd.

![[een aangepaste visual toevoegen]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Laten we nu onze woordwolk maken!

![[Word Cloud-pictogram in deelvenster Visualisaties]](../media/tutorials/power-bi/visualizations-panel.png)

Klik eerst op het Word Cloud-pictogram in het deelvenster Visualisaties. Er verschijnt een nieuw rapport in de werkruimte. Sleep het veld `keyphrases` in het deelvenster Velden naar het veld Categorie in het deelvenster Visualisaties. De woordwolk verschijnt in het rapport.

Ga nu naar de pagina Opmaak van het deelvenster Visualisaties. Schakel **Standaard stopwoorden** in de categorie Stopwoorden in om korte, algemene woorden zoals ’van’ uit de cloud te elimineren. 

![[standaard stopwoorden activeren]](../media/tutorials/power-bi/default-stop-words.png)

Schakel even verderop in dit deelvenster **Tekst draaien** en **Titel** uit.

![[focusmodus activeren]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Klik op het hulpmiddel Focusmodus in het rapport om onze woordwolk beter te bekijken. Het hulpmiddel breidt de woordwolk uit om de hele werkruimte te vullen, zoals hieronder weergegeven.

![[een woordwolk]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Meer Text Analytics-services

De Text Analytics-service, een van de Cognitive Services die worden aangeboden door Microsoft Azure, biedt ook sentimentanalyse en taaldetectie. Met name de taaldetectie is handig als uw klantenfeedback niet allemaal in het Engels is.

Beide van deze andere API’s lijken heel erg op de Key Phrases-API. Er kunnen dus vrijwel identieke aangepaste functies worden gebruikt om ze met Power BI Desktop te integreren. Maak gewoon een lege query en plak de toepasselijke code hieronder in de Geavanceerde editor, net zoals u eerder hebt gedaan. (Vergeet uw toegangssleutel niet!) Gebruik de functie vervolgens, net zoals eerder, om een nieuwe kolom aan de tabel toe te voegen.

De functie Sentimentanalyse hieronder retourneert een score die aangeeft hoe positief het in de tekst uitgedrukte sentiment is.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Hier zijn twee versies van een taaldetectiefunctie. De eerste retourneert de ISO-taalcode (bijv. `en` voor het Engels), terwijl de tweede de ‘vriendelijke’ naam (bijv. `English`) retourneert. U merkt wellicht dat alleen de laatste regel van de hoofdtekst verschilt tussen de twee versies.

```fsharp
// Returns the two-letter language code (e.g. en for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (e.g. English) of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Tot slot is hier een variant van de Key Phrases-functie die we al hebben laten zien, die de woordgroepen retourneert als een lijstobject in plaats van als één tekenreeks met door komma’s gescheiden woordgroepen. 

> [!NOTE]
> Het retourneren van één tekenreeks vereenvoudigde ons voorbeeld van een woordwolk. Een lijst is echter een flexibelere indeling om te werken met de geretourneerde woordgroepen in Power BI. U kunt lijstobjecten in Power BI Desktop bewerken via de groep Gestructureerde kolom op het lint Transformeren in de Query-editor.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Volgende stappen

Leer meer over de Text Analytics-service, de formuletaal Power Query M of Power BI.

> [!div class="nextstepaction"]
> [Naslaginformatie over de Text Analytics-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

> [!div class="nextstepaction"]
> [Naslaginformatie over Power Query M](//msdn.microsoft.com/library/mt211003.aspx)

> [!div class="nextstepaction"]
> [Documentatie voor Power BI](//powerbi.microsoft.com/documentation/powerbi-landing-page/)
