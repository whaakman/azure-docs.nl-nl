---
title: C#zelfstudie over search resultaten paginering - Azure Search
description: Deze zelfstudie bouwt voort op het project 'Van uw eerste toepassing - Azure Search maken', met de keuze van de twee typen van paginering. De eerste maakt gebruik van een reeks paginaknoppen, evenals eerste, volgende, vorige, en laatste pagina knoppen. Het tweede wisselbestand-systeem maakt gebruik van oneindige schuiven, geactiveerd door het verplaatsen van een verticale schuifbalk aan de ondergrens.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: 7e6c433168b73c6b58d13d4698bed55d7c18ec58
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434631"
---
# <a name="c-tutorial-search-results-pagination---azure-search"></a>C#zelfstudie: Search-resultaten paginering - Azure Search

Meer informatie over het implementeren van twee verschillende wisselbestand-systemen, de eerste op basis van pagina's en de tweede op oneindige schuiven. Beide systemen van wisselbestand die veelvuldig worden gebruikt en selecteren van de juiste is afhankelijk van de ervaring van de gebruiker die u met de resultaten dat wilt. In deze zelfstudie wordt het wisselbestand-systemen in het project hebt gemaakt in de [ C# zelfstudie: Uw eerste app maken - Azure Search](tutorial-csharp-create-first-app.md) zelfstudie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Uw app met genummerde wisselbestand uit te breiden
> * Uw app met oneindige schuiven uit te breiden

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

Hebben de [ C# zelfstudie: Uw eerste app maken - Azure Search](tutorial-csharp-create-first-app.md) project ingesteld en geactiveerd. Dit project kunt uw eigen versie of installeer deze via GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="extend-your-app-with-numbered-paging"></a>Uw app met genummerde wisselbestand uit te breiden

Genummerde wisselgeheugengebruik is het systeem wisselbestand van de keuze van de belangrijkste zoekmachines van internet en de meeste andere websites zoeken. Genummerde wisselbestand omvat doorgaans een optie 'vorige' en 'volgende' naast een bereik van de werkelijke pagina getallen. Ook kunnen een 'eerste pagina' en "laatste pagina" optie ook zijn beschikbaar. Deze opties bieden zeker besturingselement voor een gebruiker via het navigeren door op basis van een pagina resultaten.

Een systeem dat eerste, vorige, volgende bevat en laatste opties, samen met pagina's die niet worden gestart vanaf 1, wordt toegevoegd, maar in plaats daarvan rond de huidige pagina de gebruiker is op (dus bijvoorbeeld, als de gebruiker is op zoek op de pagina 10, misschien pagina getallen 8 9, 10, 11 en 12 worden weergegeven).

Het systeem is flexibel genoeg om toe te staan van het aantal cijfers zichtbare pagina moet worden ingesteld in een globale variabele.

Het systeem behandelen de pagina het meest linkse en meest rechtse aantal knoppen als speciale, wat betekent dat deze wordt geactiveerd voor het wijzigen van het bereik van pagina's weergegeven. Bijvoorbeeld, als pagina getallen 8, 9, 10, 11 en 12 worden weergegeven en de gebruiker op 8 klikt, weergegeven klikt u vervolgens het bereik van pagina's wijzigingen voor 6, 7, 8, 9 en 10. En er is een vergelijkbare verschuiving naar rechts als ze 12 geselecteerd.

### <a name="add-paging-fields-to-the-model"></a>Wisselbestand velden toevoegen aan het model

De basis-Zoek-pagina-oplossing open is.

1. Open het modelbestand SearchData.cs.

2. Eerst enkele globale variabelen toevoegen. Globale variabelen zijn in MVC gedefinieerd in hun eigen statische klasse. **ResultsPerPage** Hiermee stelt u het aantal resultaten per pagina. **MaxPageRange** bepaalt het aantal zichtbare pagina getallen in de weergave. **PageRangeDelta** bepaalt het aantal pagina's naar links of rechts de paginabereik moet worden verplaatst, als u het nummer van de meest linkse of rechtse hebt geselecteerd. Doorgaans is dit laatste getal rond de helft van **MaxPageRange**. Voeg de volgende code in de naamruimte.

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
        public static int MaxPageRange
        {
            get
            {
                return 5;
            }
        }

        public static int PageRangeDelta
        {
            get
            {
                return 2;
            }
        }
    }
    ```

    >[!Tip]
    >Als u dit project op een apparaat met een kleinere scherm, zoals een laptop uitvoert, overweeg dan om wijzigen **ResultsPerPage** naar 2.

3. Wisselbestand Voeg eigenschappen toe aan de **SearchData** klasse, bijvoorbeeld nadat de **Tekstdoorzoeken** eigenschap.

    ```cs
        // The current page being displayed.
        public int currentPage { get; set; }

        // The total number of pages of results.
        public int pageCount { get; set; }

        // The left-most page number to display.
        public int leftMostPage { get; set; }

        // The number of page numbers to display - which can be less than MaxPageRange towards the end of the results.
        public int pageRange { get; set; }

        // Used when page numbers, or next or prev buttons, have been selected.
        public string paging { get; set; }
    ```

### <a name="add-a-table-of-paging-options-to-the-view"></a>Een tabel van de opties voor serverpaginering toevoegen aan de weergave

1. Open het bestand index.cshtml, en voeg de volgende code direct voor de afsluitende &lt;/body&gt; tag. Deze nieuwe code is een tabel met de opties voor serverpaginering: eerste, vorige, 1, 2, 3, 4, 5, komende, afgelopen.

    ```cs
    @if (Model != null && Model.pageCount > 1)
    {
    // If there is more than one page of results, show the paging buttons.
    <table>
        <tr>
            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("|<", "Page", "Home", new { paging = "0" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">|&lt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("<", "Page", "Home", new { paging = "prev" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&lt;</p>
                }
            </td>

            @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
            {
                <td>
                    @if (Model.currentPage == pn)
                    {
                        // Convert displayed page numbers to 1-based and not 0-based.
                        <p class="pageSelected">@(pn + 1)</p>
                    }
                    else
                    {
                        <p class="pageButton">
                            @Html.ActionLink((pn + 1).ToString(), "Page", "Home", new { paging = @pn }, null)
                        </p>
                    }
                </td>
            }

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">", "Page", "Home", new { paging = "next" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">|", "Page", "Home", new { paging = Model.pageCount - 1 }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;|</p>
                }
            </td>
        </tr>
    </table>
    }
    ```

    We een HTML-tabel gebruiken om te dingen netjes uitlijnen. Maar alle actie afkomstig van is de @Html.ActionLink overzichten, elke aanroepen van de controller met een **nieuwe** model dat is gemaakt met verschillende vermeldingen dat moet worden de **wisselbestand** eigenschap die we eerder hebt toegevoegd.

    Opties voor de eerste en laatste pagina zenden geen tekenreeksen zoals 'first' en 'last', maar in plaats daarvan de juiste pagina's te verzenden.

2. Sommige wisselbestand-klassen toevoegen aan de lijst met HTML-stijlen in het bestand hotels.css. De **pageSelected** klasse is er voor het identificeren van de pagina die de gebruiker momenteel bekijkt is (door het aantal vet inschakelen) in de lijst van pagina's.

    ```html
        .pageButton {
            border: none;
            color: darkblue;
            font-weight: normal;
            width: 50px;
        }

        .pageSelected {
            border: none;
            color: black;
            font-weight: bold;
            width: 50px;
        }

        .pageButtonDisabled {
            border: none;
            color: lightgray;
            font-weight: bold;
            width: 50px;
        }
    ```

### <a name="add-a-page-action-to-the-controller"></a>De actie van een pagina toevoegen aan de controller

1. Open het bestand HomeController.cs en voeg de **pagina** actie. Deze actie reageert op een van de Paginaopties geselecteerd.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                switch (model.paging)
                {
                    case "prev":
                        page = (int)TempData["page"] - 1;
                        break;

                    case "next":
                        page = (int)TempData["page"] + 1;
                        break;

                    default:
                        page = int.Parse(model.paging);
                        break;
                }

                // Recover the leftMostPage.
                int leftMostPage = (int)TempData["leftMostPage"];

                // Recover the search text and search for the data for the new page.
                model.searchText = TempData["searchfor"].ToString();

                await RunQueryAsync(model, page, leftMostPage);

                // Ensure Temp data is stored for next call, as TempData only stored for one call.
                TempData["page"] = (object)page;
                TempData["searchfor"] = model.searchText;
                TempData["leftMostPage"] = model.leftMostPage;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

    De **RunQueryAsync** methode ziet nu een syntaxisfout, vanwege de derde parameter, die we eerst in een bit nagekeken worden.

    > [!Note]
    > De **TempData** aanroepen van een waarde opslaan (een **object**) in de tijdelijke opslag, maar deze opslag zich blijft voordoen voor _alleen_ één aanroep. Als er iets in tijdelijke gegevens opslaat, is beschikbaar voor de volgende aanroep van de actie van een domeincontroller, maar wordt hierna meest zeker worden voltooid door de aanroep van! Vanwege deze korte levensduur, slaan we de zoektekst en pagineringsopties back-ups maken in de tijdelijke opslag elke aanroep naar **pagina**.

2. De **Index(model)** actie behoeften bijgewerkt voor het opslaan van de tijdelijke variabelen en om toe te voegen van de meest linkse paginaparameter voor de **RunQueryAsync** aanroepen.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. De **RunQueryAsync** methode behoeften aanzienlijk bijgewerkt. We gebruiken de **overslaan**, **boven**, en **IncludeTotalResultCount** velden van de **SearchParameters** klasse om aan te vragen van slechts één pagina aan resultaten, beginnend bij de **overslaan** instelling. We moet er ook voor het berekenen van de variabelen wisselbestand voor onze weergeven. Vervang de volledige methode door de volgende code.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                   // Enter Hotel property names into this list so only these values will be returned.
                   // If Select is empty, all values will be returned, which can be inefficient.
                   Select = new[] { "HotelName", "Description" },
                   SearchMode = SearchMode.All,

                   // Skip past results that have already been returned.
                   Skip = page * GlobalVariables.ResultsPerPage,

                   // Take only the next page worth of results.
                   Top = GlobalVariables.ResultsPerPage,

                   // Include the total number of results.
                   IncludeTotalResultCount = true,
               };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // This variable communicates the total number of pages to the view.
            model.pageCount = ( (int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

            // This variable communicates the page number being displayed to the view.
            model.currentPage = page;

            // Calculate the range of page numbers to display.
            if (page == 0)
            {
                leftMostPage = 0;
            }
            else
               if (page <= leftMostPage)
            {
                // Trigger a switch to a lower page range.
                leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
            }
            else
            if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
            {
                // Trigger a switch to a higher page range.
                leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
            }
            model.leftMostPage = leftMostPage;

            // Calculate the number of page numbers to display.
            model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

            return View("Index", model);
        }
    ```

4. Tot slot moet een kleine wijziging aanbrengen in de weergave. De variabele **resultsList.Results.Count** bevat nu het aantal resultaten in één pagina (3 in ons voorbeeld), niet het totale aantal. Omdat we de **IncludeTotalResultCount** op true, wordt de variabele **resultsList.Count** bevat nu het totale aantal resultaten. Dus vinden waar het aantal resultaten wordt weergegeven in de weergave en dit wijzigen in de volgende code.

    ```cs
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>
    ```

    > [!Note]
    > Er is een treffer prestaties, maar meestal niet veel van een door in te stellen **IncludeTotalResultCount** op waar, als dit totaal moet worden berekend met Azure Search. Met complexe gegevenssets is er een waarschuwing dat de waarde geretourneerd is een _aanpassing_. Voor de Hotelgegevens van onze, wordt het nauwkeurig zijn.

### <a name="compile-and-run-the-app"></a>Compileren en uitvoeren van de app.

Selecteer nu **starten zonder foutopsporing** (of druk op F5).

1. Zoeken op tekst waarmee veel van de resultaten (zoals 'Wi-Fi'). Kunt u pagina netjes door de resultaten?

    ![Genummerde bladeren door 'groep' resultaten](./media/tutorial-csharp-create-first-app/azure-search-numbered-paging.png)

2. Probeer te klikken op de meest rechtse en hoger, uiterst linkse pagina's. Nummers van de pagina pas op de juiste wijze voor het midden van de pagina waarop die u zich op?

3. Zijn de opties 'first' en 'last' nuttig? Enkele populaire web-zoekopdrachten met deze opties en andere niet.

4. Ga naar de laatste pagina van de resultaten. De laatste pagina is de enige pagina die u mogelijk minder dan **ResultsPerPage** resultaten.

    ![Laatste pagina van "Wi-Fi" onderzoeken](./media/tutorial-csharp-create-first-app/azure-search-pool-last-page.png)

5. Typ 'plaats' en klik op zoeken. Er zijn geen opties voor paginering worden weergegeven als er minder dan één pagina de waarde van de resultaten.

    ![Zoeken naar "plaats"](./media/tutorial-csharp-create-first-app/azure-search-town.png)

Sla nu uit dit project en we proberen een alternatief voor deze vorm van paginering.

## <a name="extend-your-app-with-infinite-scrolling"></a>Uw app met oneindige schuiven uit te breiden

Oneindige schuiven wordt geactiveerd wanneer een gebruiker een verticale schuifbalk naar de laatste van de resultaten die worden weergegeven. In dit geval wordt een aanroep naar de server voor de volgende pagina van de resultaten uitgevoerd. Als er geen verdere resultaten, wordt er niets wordt geretourneerd en wordt de verticale schuifbalk wordt niet gewijzigd. Als er meer resultaten zijn, worden ze toegevoegd aan de huidige pagina en de wijzigingen van de balk schuiven om weer te geven dat meer resultaten beschikbaar zijn.

Wat belangrijk is dat de pagina die wordt weergegeven, niet, maar toegevoegd aan met de nieuwe resultaten vervangen. Een gebruiker kan altijd Bladeren back-up op de eerste resultaten van de zoekopdracht.

Voor het implementeren van oneindige schuiven, laten we beginnen met het project voordat een van de pagina schuiven elementen nummer zijn toegevoegd. Als u wilt, zorg er dus voor een ander exemplaar van de pagina basiszoekopdracht van GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

### <a name="add-paging-fields-to-the-model"></a>Wisselbestand velden toevoegen aan het model

1. Voeg eerst toe een **wisselbestand** eigenschap in op de **SearchData** klasse (in het modelbestand SearchData.cs).

    ```cs
        // Record if the next page is requested.
        public string paging { get; set; }
    ```

    Deze variabele is een tekenreeks, die 'volgende' bevat de volgende pagina van de resultaten moet worden verzonden of niet null zijn voor de eerste pagina van een zoekopdracht.

2. In hetzelfde bestand, en in de naamruimte, moet u een globale variabele klasse met één eigenschap toevoegen. Globale variabelen zijn in MVC gedefinieerd in hun eigen statische klasse. **ResultsPerPage** Hiermee stelt u het aantal resultaten per pagina. 

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
    }
    ```

### <a name="add-a-vertical-scroll-bar-to-the-view"></a>Toevoegen van een verticale schuifbalk aan de weergave

1. Ga naar de sectie van het bestand index.cshtml waarin de resultaten worden weergegeven (deze begint met de  **@if (Model! = null)** ).

2. Vervang de sectie met de volgende code. De nieuwe **&lt;div&gt;** sectie is om het gebied die moet worden schuifbare en voegt zowel een **overloop-y** kenmerk en een aanroep naar een **onscroll**functie, genaamd 'scrolled()', als volgt te werk.

    ```cs
        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>

            <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    // Display the hotel name and description.
                    @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
                }
            </div>
        }
    ```

3. Direct onder de lus, nadat de &lt;/div&gt; code, voegt u de **verschoven** functie.

    ```javascript
        <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 2) {
                                div.innerHTML += '\n<textarea class="box1">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2">' + data[i + 1] + '</textarea>';
                            }
                        });
                    }
                }
        </script>
    ```

    De **als** -instructie in het script hierboven tests om te zien als de gebruiker naar de onderkant van de verticale schuifbalk heeft geschoven. Als ze hebben, wordt een aanroep naar de **Start** controller is aangebracht in een actie met de naam **volgende**. Er is geen andere informatie nodig is door de netwerkcontroller, wordt de volgende pagina van de gegevens geretourneerd. Deze gegevens wordt vervolgens opgemaakt identieke HTML-stijlen te gebruiken als de oorspronkelijke pagina. Als er geen resultaten worden geretourneerd, niets wordt toegevoegd en dingen blijven als ze zijn.

### <a name="handle-the-next-action"></a>De volgende actie verwerken

Er zijn slechts drie acties die moeten worden verzonden naar de controller: de eerste uitvoering van de app wordt aangeroepen **index() heeft**, de eerste zoekopdracht door de gebruiker wordt aangeroepen **Index(model)** , en vervolgens de volgende vraagt om meer resultaten via **Next(model)** .

1. Open het bestand home controller en verwijder de **RunQueryAsync** methode uit de oorspronkelijke zelfstudie.

2. Vervang de **Index(model)** actie door de volgende code. Nu verwerkt de **wisselbestand** wanneer deze is null of is ingesteld op 'volgende' en de aanroep naar Azure Search worden verwerkt.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First call. Check for valid text input.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    page = 0;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Enter Hotel property names into this list so only these values will be returned.
                    // If Select is empty, all values will be returned, which can be inefficient.
                    Select = new[] { "HotelName", "Description" },
                    SearchMode = SearchMode.All,

                    // Skip past results that have already been returned.
                    Skip = page * GlobalVariables.ResultsPerPage,

                    // Take only the next page worth of results.
                    Top = GlobalVariables.ResultsPerPage,

                    // Include the total number of results.
                    IncludeTotalResultCount = true,
                };

                // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
                model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Net als bij de genummerde wisselbestand-methode, gebruiken we de **overslaan** en **boven** zoekinstellingen om aan te vragen van alleen de gegevens die we nodig hebben wordt geretourneerd.

3. Voeg de **volgende** actie aan de home-controller. Houd er rekening mee hoe deze retourneert een lijst, elke twee elementen toe te voegen aan de lijst met hotel: de naam van een hotel en een beschrijving van het hotel. Deze indeling is ingesteld zodat deze overeenkomen met de **verschoven** van functie gebruik van de geretourneerde gegevens in de weergave.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel name, then description, to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Als er een syntaxisfout op **lijst&lt;tekenreeks&gt;** , voegt u de volgende **met behulp van** richtlijn naar de kop van de controller-bestand.

    ```cs
    using System.Collections.Generic;
    ```

### <a name="compile-and-run-your-project"></a>Compileren en uitvoeren van uw project

Selecteer nu **starten zonder foutopsporing** (of druk op F5).

1. Voer een term die wordt veel van de resultaten geven (zoals 'groep') en test de verticale schuifbalk. Er een nieuwe pagina met resultaten geactiveerd?

    ![Oneindige scrollen door 'groep' resultaten](./media/tutorial-csharp-create-first-app/azure-search-infinite-scroll.png)

    > [!Tip]
    > Om ervoor te zorgen dat er een schuifbalk wordt weergegeven op de eerste pagina, de eerste pagina van de resultaten iets meer dan de hoogte van het gebied dat ze worden weergegeven in. In ons voorbeeld **.box1** heeft een hoogte van 30 pixels **.box2** heeft een hoogte van 100 pixels _en_ een ondermarge van 24 pixels. Elk item gebruikt dus 154 pixels. Drie vermeldingen duurt van 3 x 154 = 462 pixels. Om ervoor te zorgen dat een verticale schuifbalk wordt weergegeven, op het gebied voor de hoogte moet zijn ingesteld dat is kleiner dan 462 pixels, zelfs 461 werkt. Dit probleem doet zich alleen op de eerste pagina, daarna een schuifbalk wordt moet worden weergegeven. The line to update is: **&lt;div id="myDiv" style="width: 800px; height: 450px; overloop-y: schuiven;"onscroll="scrolled() "&gt;** .

2. Schuif omlaag helemaal tot aan de onderkant van de resultaten. U ziet hoe alle gegevens nu op de pagina met één weergave zijn. U kunt bladeren gaat helemaal terug naar de bovenkant zonder het activeren van de server-aanroepen.

Meer geavanceerde oneindig schuifbalkgebied systemen mogelijk gebruik van het muiswiel of vergelijkbare andere mechanisme voor het activeren van het laden van een nieuwe pagina met resultaten. We zullen niet duurt oneindige schuiven een verdere in deze zelfstudies, maar heeft een bepaalde charm toe wordt voorkomen dat er extra muisklikken, en kunt u andere opties voor verder onderzoek!

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Genummerde wisselgeheugengebruik is geschikt voor zoekopdrachten waar de volgorde van de resultaten enigszins willekeurige, dit houdt er mogelijk ook is iets van belang zijn voor uw gebruikers op de latere pagina's.
* Oneindige schuiven is goed als de volgorde van de resultaten met name van belang is. Bijvoorbeeld, als de resultaten worden gesorteerd op de afstand van het midden van de plaats van een bestemming.
* Genummerde paginering kunt u betere navigatie. Bijvoorbeeld, een gebruiker kunt Houd er rekening mee dat een interessante resultaat op pagina 6, is dat geen dergelijke eenvoudige verwijzing in oneindige schuiven bestaat.
* Oneindige schuiven heeft een eenvoudig beroep omhoog en omlaag schuiven met geen nauwkeurige paginanummers op te klikken.
* Een belangrijke functie van oneindige schuiven is dat resultaten worden toegevoegd aan een bestaande pagina, die pagina efficiënt is niet vervangen.
* Tijdelijke opslag zich blijft voordoen voor slechts één aanroep en moet opnieuw worden ingesteld voor het overbruggen van extra aanroepen.


## <a name="next-steps"></a>Volgende stappen

Wisselgeheugengebruik is van cruciaal belang voor zoekacties op internet. Met paging goed vallen, wordt de volgende stap is het bovendien is de gebruikerservaring verbeteren door toe te voegen, automatisch aangevulde zoekopdrachten.

> [!div class="nextstepaction"]
> [C#Zelfstudie: Automatisch aanvullen en suggesties - Azure Search toevoegen](tutorial-csharp-type-ahead-and-suggestions.md)
