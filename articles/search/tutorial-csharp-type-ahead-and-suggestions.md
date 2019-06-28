---
title: C#zelfstudie over het automatisch aanvullen en suggesties - Azure Search
description: Deze zelfstudie bouwt voort op het project 'Paginering - Azure Search zoekresultaten' automatisch aanvullen en suggesties toe te voegen. Het doel is een uitgebreidere ervaring. Leer hoe u een vervolgkeuzelijst met suggesties met inline automatisch aanvullen van combineert.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: 01c0819fd0bf525739675ad756031cafc1a51673
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434697"
---
# <a name="c-tutorial-add-autocompletion-and-suggestions---azure-search"></a>C#zelfstudie: Automatisch aanvullen en suggesties - Azure Search toevoegen

Meer informatie over het implementeren van automatisch aanvullen (type-ahead en suggesties) wanneer een gebruiker wordt gestart in het zoekvak typt. In deze zelfstudie, wordt er afzonderlijk, automatisch aangevulde resultaten en resultaten van suggesties weergeven en een methode van het combineren van deze voor het maken van een rijkere gebruikerservaring weergeven. Een gebruiker mogelijk alleen naar het type van twee of drie sleutels om te zoeken van de resultaten die beschikbaar zijn. Deze zelfstudie bouwt voort op het wisselbestand-project hebt gemaakt in de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) zelfstudie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Suggesties toevoegen
> * De suggesties markering toevoegen
> * Automatisch aanvullen toevoegen
> * Automatisch aanvullen en suggesties combineren

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

Hebben de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) project ingesteld en geactiveerd. Dit project kunt uw eigen versie, die u in de vorige zelfstudie hebt voltooid of installeer deze via GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="add-suggestions"></a>Suggesties toevoegen

Laten we beginnen met het meest eenvoudige geval van aanbieding van alternatieven voor de gebruiker: een vervolgkeuzelijst met suggesties.

1. Wijzig in het bestand index.cshtml de **TextBoxFor** instructie met de volgende.

    ```cs
     @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautosuggest" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

    De sleutel hier is dat we de ID van het zoekvak om hebt ingesteld **azureautosuggest**.

2. Na deze verklaring, na de afsluitende  **&lt;/div&gt;** , voer dit script.

    ```javascript
    <script>
        $("#azureautosuggest").autocomplete({
            source: "/Home/Suggest?highlights=false&fuzzy=false",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

    We hebben dit script verbonden met het zoekvak via dezelfde ID. Ook ten minste twee tekens is nodig voor het activeren van de zoekopdracht en noemen we de **voorstellen** actie in de oorspronkelijke controller met twee queryparameters: **markeert** en **fuzzy**, beide ingesteld op false in dit exemplaar.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Verwijzingen naar jquery-scripts toevoegen aan de weergave

De functie automatisch aanvullen is met de naam in het bovenstaande script is niet iets we onszelf schrijven hebben als deze beschikbaar in de bibliotheek jquery is. 

1. Voor toegang tot de bibliotheek jquery, wijzigt de &lt;head&gt; sectie van het bestand weergeven in de volgende code.

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Typeahead</title>
        <link href="https://code.jquery.com/ui/1.12.1/themes/start/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. We ook wilt verwijderen, of uit, een regel die verwijst naar een jquery in het bestand _Layout.cshtml (in de **weergaven/gedeelde** map). Zoek de volgende regels en opmerkingen bij de eerste scriptopdrachtregel, zoals wordt weergegeven. Deze wijziging voorkomt conflicterende verwijzingen naar jquery.

    ```html
    <environment include="Development">
        <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

    We kunnen nu de vooraf gedefinieerde automatisch aanvullen jquery-functies gebruiken.

### <a name="add-the-suggest-action-to-the-controller"></a>De aanbevolen actie toevoegen aan de controller

1. In de oorspronkelijke controller toevoegen de **voorstellen** actie (bijvoorbeeld na de **pagina** actie).

    ```cs
        public async Task<ActionResult> Suggest(bool highlights, bool fuzzy, string term)
        {
            InitSearch();

            // Setup the suggest parameters.
            var parameters = new SuggestParameters()
            {
                UseFuzzyMatching = fuzzy,
                Top = 8,
            };

            if (highlights)
            {
                parameters.HighlightPreTag = "<b>";
                parameters.HighlightPostTag = "</b>";
            }

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and simply searches the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", parameters);

            // Convert the suggest query results to a list that can be displayed in the client.
            List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();

            // Return the list of suggestions.
            return new JsonResult(suggestions);
        }
    ```

    De **boven** parameter geeft u het aantal resultaten om terug te keren (als u niets opgeeft, wordt de standaardwaarde is 5). Een _suggestie_ is opgegeven in de Azure-index, die wordt uitgevoerd wanneer de gegevens is ingesteld, en niet door een client-app, zoals in deze zelfstudie. In dit geval de suggestie wordt 'beveiligingsgroep' genoemd, en wordt gezocht in de **HotelName** veld - niets anders. 

    "Near missers in" zoeken bij benadering kunt moeten worden opgenomen in de uitvoer. Als de **markeert** parameter is ingesteld op true, vet weergegeven HTML-codes worden toegevoegd aan de uitvoer. We zullen deze twee parameters ingesteld op true in de volgende sectie.

2. U krijgt mogelijk sommige syntaxisfouten. Als dit het geval, voeg de volgende twee **met behulp van** instructies toe aan het begin van het bestand.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

3. De app uitvoeren. Krijgt u een scala aan opties wanneer u bijvoorbeeld 'IO'? Probeer nu 'pa'.

    ![Typen 'IO' ziet u twee suggesties](./media/tutorial-csharp-create-first-app/azure-search-suggest-po.png)

    U ziet dat de letters die u invoert _moet_ start een woord en niet alleen worden opgenomen in het woord.

4. Stel in het script weergeven **& fuzzy** true, en voer de app opnieuw uit. Voer nu 'IO'. U ziet dat de zoekopdracht wordt ervan uitgegaan dat u hebt één letter gegaan!
 
    ![Typen 'pa' zoeken bij benadering is ingesteld op true](./media/tutorial-csharp-create-first-app/azure-search-suggest-fuzzy.png)

    Als u geïnteresseerd bent, de [Lucene-querysyntaxis in Azure Search](https://docs.microsoft.com/azure/search/query-lucene-syntax) wordt de logica die is gebruikt voor fuzzy zoekopdrachten in detail beschreven.

## <a name="add-highlighting-to-the-suggestions"></a>De suggesties markering toevoegen

We kunnen het uiterlijk van de suggesties voor de gebruiker een beetje verbeteren door in te stellen de **markeert** parameter op waar. Echter, we eerst code toevoegen aan de weergave om de vette tekst weer te geven.

1. In de weergave (index.cshtml) toevoegen in het volgende script na het **azureautosuggest** script dat u de die hierboven zijn ingevoerd.

    ```javascript
    <script>
        var updateTextbox = function (event, ui) {
            var result = ui.item.value.replace(/<\/?[^>]+(>|$)/g, "");
            $("#azuresuggesthighlights").val(result);
            return false;
        };

        $("#azuresuggesthighlights").autocomplete({
            html: true,
            source: "/home/suggest?highlights=true&fuzzy=false&",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },
            select: updateTextbox,
            focus: updateTextbox
        }).data("ui-autocomplete")._renderItem = function (ul, item) {
            return $("<li></li>")
                .data("item.autocomplete", item)
                .append("<a>" + item.label + "</a>")
                .appendTo(ul);
        };
    </script>
    ```

2. De ID van het tekstvak nu wijzigen zodat deze als volgt wordt.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresuggesthighlights" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. De app opnieuw uitvoeren en u ziet nu de ingevoerde tekst vetgedrukt in de suggesties. Probeer daarom bijvoorbeeld 'pa'.
 
    ![Typen 'pa' met markeringen](./media/tutorial-csharp-create-first-app/azure-search-suggest-highlight.png)

4. De logica in de bovenstaande markeren script gebruikt, is geen betrouwbare. Als u een term die wordt weergegeven tweemaal in dezelfde naam invoert, zijn de resultaten van de vette zijn niet heel wat u wilt. Probeer 'maand'.

    Een van de ontwikkelaar moet beantwoorden vragen is dat wanneer is een script werkt 'goed genoeg', en wanneer moet kuren worden verholpen. We zullen niet worden duurt een verdere in deze zelfstudie te markeren, maar zoeken naar dat een exacte algoritme is iets te overwegen als duurt verder markeren.

## <a name="add-autocompletion"></a>Automatisch aanvullen toevoegen

Een andere variatie, dat wil zeggen enigszins afwijken van suggesties, wordt automatisch aanvullen (ook wel ', automatisch aangevulde' genoemd). Nogmaals, gaan we met de meest eenvoudige implementatie, voordat u verplaatsen naar de gebruikerservaring te verbeteren.

1. Voer het volgende script in de weergave, uw vorige scripts te volgen.

    ```javascript
    <script>
        $("#azureautocompletebasic").autocomplete({
            source: "/Home/Autocomplete",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

2. De ID van het tekstvak nu wijzigen zodat deze als volgt wordt.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocompletebasic" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. In de oorspronkelijke domeincontroller moeten we in te voeren de **automatisch aanvullen** actie, bijvoorbeeld onder de **voorstellen** actie.

    ```cs
        public async Task<ActionResult> AutoComplete(string term)
        {
            InitSearch();

            // Setup the autocomplete parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 6
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Convert the results to a list that can be displayed in the client.
            List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();

            // Return the list.
            return new JsonResult(autocomplete);
        }
    ```

    U ziet dat we dezelfde gebruiken *suggestie* functie, genaamd 'beveiligingsgroep', in het zoekvak automatisch aanvullen zoals we hebben gedaan voor suggesties (zodat we alleen automatisch aanvullen de namen van de hotel willen).

    Er zijn tal van **AutocompleteMode** instellingen gebruikt, en we **OneTermWithContext**. Raadpleeg [Azure automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) voor een beschrijving van het bereik van de opties.

4. De app uitvoeren. U ziet hoe het bereik van de opties worden weergegeven in de vervolgkeuzelijst één woorden. Probeer de woorden die beginnen met "opnieuw". U ziet hoe het aantal opties zoals meer letters zijn getypt vermindert.

    ![Typen met eenvoudige automatisch aanvullen](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocompletebasic.png)

    Als deze staat, is het suggesties-script dat u eerder hebt uitgevoerd, waarschijnlijk meer nuttige dan met dit script automatisch aanvullen. Als u automatisch aanvullen gebruikersvriendelijker, is het beste toegevoegd aan de zoekopdracht suggestie.

## <a name="combine-autocompletion-and-suggestions"></a>Automatisch aanvullen en suggesties combineren

Automatisch aanvullen en suggesties combineren is de meest complexe van onze opties, en biedt waarschijnlijk de beste gebruikerservaring. We willen is weergeven, inline met de tekst die wordt getypt, de eerste keuze van Azure Search voor autocompleting de tekst. We willen ook tal van suggesties als een vervolgkeuzelijst.

Er zijn bibliotheken die deze functionaliteit - ook wel 'inline automatisch aanvullen' of een vergelijkbare naam bieden. We gaan echter systeemeigen implementeren van deze functie, zodat u kunt zien wat er gebeurt. We gaan eerst werk op de controller starten in dit voorbeeld.

1. Er moet een actie toevoegen aan de domeincontroller die als resultaat slechts één resultaat voor automatisch aanvullen, samen met een opgegeven aantal suggesties geeft. Deze actie wordt gebeld **AutocompleteAndSuggest**. In de home-controller, moet u de volgende actie, na de andere nieuwe acties toevoegen.

    ```cs
        public async Task<ActionResult> AutocompleteAndSuggest(string term)
        {
            InitSearch();

            // Setup the type-ahead search parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 1,
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Setup the suggest search parameters.
            var sp = new SuggestParameters()
            {
                Top = 8,
            };

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and it searches only the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", sp);

            // Create an empty list.
            var results = new List<string>();

            if (autocompleteResult.Results.Count > 0)
            {
                // Add the top result for type-ahead.
                results.Add(autocompleteResult.Results[0].Text);
            }
            else
            {
                // There were no type-ahead suggestions, so add an empty string.
                results.Add("");
            }
            for (int n = 0; n < suggestResult.Results.Count; n++)
            {
                // Now add the suggestions.
                results.Add(suggestResult.Results[n].Text);
            }

            // Return the list.
            return new JsonResult(results);
        }
    ```

    Een optie voor automatisch aanvullen wordt geretourneerd aan de bovenkant van de **resultaten** lijst, gevolgd door alle suggesties.

2. Eerst in de weergave, implementeren we een truc, zodat een woord lichte grijze automatisch aanvullen rechts onder bolder tekst worden ingevoerd door de gebruiker wordt weergegeven. HTML-code bevat relatieve plaatsing voor dit doel. Wijziging de **TextBoxFor** instructie (en de omringende &lt;div&gt; instructies) voor de volgende waard die een tweede zoekvak geïdentificeerd als **onder** is direct onder onze normale zoekvak door deze zoekopdracht vak 39 pixels binnen korting op de standaardlocatie te!

    ```cs
    <div id="underneath" class="searchBox" style="position: relative; left: 0; top: 0">
    </div>

    <div id="searchinput" class="searchBoxForm" style="position: relative; left: 0; top: -39px">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocomplete" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

    We wijzigen de ID opmerking opnieuw, **azureautocomplete** in dit geval.

3. Ook in de weergave, voer het volgende script nadat alle de scripts die u tot nu toe hebt ingevoerd. Is er heel veel toe.

    ```javascript
    <script>
        $('#azureautocomplete').autocomplete({
            delay: 500,
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },

            // Use Ajax to set up a "success" function.
            source: function (request, response) {
                var controllerUrl = "/Home/AutoCompleteAndSuggest?term=" + $("#azureautocomplete").val();
                $.ajax({
                    url: controllerUrl,
                    dataType: "json",
                    success: function (data) {
                        if (data && data.length > 0) {

                            // Show the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = data[0];

                            // Remove the top suggestion as it is used for inline autocomplete.
                            var array = new Array();
                            for (var n = 1; n < data.length; n++) {
                                array[n - 1] = data[n];
                            }

                            // Show the drop-down list of suggestions.
                            response(array);
                        } else {

                            // Nothing is returned, so clear the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }
                });
            }
        });

        // Complete on TAB.
        // Clear on ESC.
        // Clear if backspace to less than 2 characters.
        // Clear if any arrow key hit as user is navigating the suggestions.
        $("#azureautocomplete").keydown(function (evt) {

            var suggestedText = document.getElementById("underneath").innerHTML;
            if (evt.keyCode === 9 /* TAB */ && suggestedText.length > 0) {
                $("#azureautocomplete").val(suggestedText);
                return false;
            } else if (evt.keyCode === 27 /* ESC */) {
                document.getElementById("underneath").innerHTML = "";
                $("#azureautocomplete").val("");
            } else if (evt.keyCode === 8 /* Backspace */) {
                if ($("#azureautocomplete").val().length < 2) {
                    document.getElementById("underneath").innerHTML = "";
                }
            } else if (evt.keyCode >= 37 && evt.keyCode <= 40 /* Any arrow key */) {
                document.getElementById("underneath").innerHTML = "";
            }
        });

        // Character replace function.
        function setCharAt(str, index, chr) {
            if (index > str.length - 1) return str;
            return str.substr(0, index) + chr + str.substr(index + 1);
        }

        // This function is needed to clear the "underneath" text when the user clicks on a suggestion, and to
        // correct the case of the autocomplete option when it does not match the case of the user input.
        // The interval function is activated with the input, blur, change, or focus events.
        $("#azureautocomplete").on("input blur change focus", function (e) {

            // Set a 2 second interval duration.
            var intervalDuration = 2000, 
                interval = setInterval(function () {

                    // Compare the autocorrect suggestion with the actual typed string.
                    var inputText = document.getElementById("azureautocomplete").value;
                    var autoText = document.getElementById("underneath").innerHTML;

                    // If the typed string is longer than the suggestion, then clear the suggestion.
                    if (inputText.length > autoText.length) {
                        document.getElementById("underneath").innerHTML = "";
                    } else {

                        // If the strings match, change the case of the suggestion to match the case of the typed input.
                        if (autoText.toLowerCase().startsWith(inputText.toLowerCase())) {
                            for (var n = 0; n < inputText.length; n++) {
                                autoText = setCharAt(autoText, n, inputText[n]);
                            }
                            document.getElementById("underneath").innerHTML = autoText;

                        } else {
                            // The strings do not match, so clear the suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }

                    // If the element loses focus, stop the interval checking.
                    if (!$input.is(':focus')) clearInterval(interval);

                }, intervalDuration);
        });
    </script>
    ```

    Let op de slimme gebruik van de **interval** functie op beide verwijdert u de onderliggende tekst wanneer deze niet meer overeenkomt met wat de gebruiker typt, en ook hetzelfde hoofdlettergebruik (hoofdletters of kleine) instellen als de gebruiker is te typen (zoals 'pa' wanneer komt overeen met 'PA', 'pA', 'Pa' zoeken), zodat de overlappende tekst leuk is.

    Lees de opmerkingen in het script naar een uitgebreidere inzicht te krijgen.

4. Tot slot moet een kleine aanpassing van twee HTML-klasse zodat ze transparant te maken. Voeg de volgende regel aan de **searchBoxForm** en **searchBox** klassen, in het bestand hotels.css.

    ```html
        background: rgba(0,0,0,0);
    ```

5. Nu de app uitvoeren. Voer 'pa' in het zoekvak. Krijgt u "palace' als het voorstel automatisch aanvullen, samen met twee hotels die 'pa' bevatten?

    ![Typen met inline automatisch aanvullen en suggesties](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocomplete.png)

6. Probeer de tab-toets om te accepteren van het voorstel automatisch aanvullen en probeert u suggesties met behulp van de pijltoetsen en de tab-toets en probeer het opnieuw met de muis en één klik. Controleer of dat het script al deze gevallen netjes verwerkt.

    Kunt u beslissen dat het is eenvoudiger om te laden in een bibliotheek die u deze functie biedt, maar u nu ten minste één manier weet om inline automatisch aanvullen om te werken.

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Automatisch aanvullen (ook bekend als ', automatisch aangevulde') en suggesties kunnen inschakelen voor de gebruiker naar het type van slechts een paar sleutels om te zoeken precies wat ze willen.
* Automatisch aanvullen en suggesties die samenwerken, kunnen een rijke gebruikerservaring bieden.
* Test altijd automatisch aanvullen werkt met alle soorten invoer.
* Met behulp van de **setInterval** functie is handig in te controleren en corrigeren van UI-elementen.

## <a name="next-steps"></a>Volgende stappen

In de volgende zelfstudie hebben we eens naar een andere manier om de gebruikerservaring te verbeteren facetten gebruiken om te beperken van zoekopdrachten met één klik.

> [!div class="nextstepaction"]
> [C#Zelfstudie: Facetten gebruiken om u te helpen de navigatie - Azure Search](tutorial-csharp-facets.md)


