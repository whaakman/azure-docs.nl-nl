---
title: C#zelfstudie over het gebruik van facetten voor navigatie en netwerk-efficiëntie - Azure Search
description: Deze zelfstudie bouwt voort op het project "Paginering - Azure Search zoekresultaten", om toe te voegen facet zoekopdrachten. Meer informatie over dat facetten kunnen worden gebruikt in de navigatie en automatisch aanvullen.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/20/2019
ms.openlocfilehash: a81042869564533050fef42a983f2f8fb9bc7b23
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304655"
---
# <a name="c-tutorial-use-facets-for-navigation-and-network-efficiency---azure-search"></a>C#zelfstudie: Facetten voor navigatie en netwerk-efficiëntie - Azure Search gebruiken

Facetten hebben twee afzonderlijke toepassingen in Azure Search. Facetten kunnen worden gebruikt om te helpen navigatie door de gebruiker voorzien van een set met selectievakjes om te gebruiken om de zoekopdracht te verfijnen. Ze kunnen ook worden gebruikt voor het verbeteren van de efficiëntie van het netwerk, wanneer gebruikt in automatisch aanvullen. Facet zoekopdrachten zijn efficiënt, omdat ze slechts één keer voor elke pagina laden, in plaats van één keer voor elke toetsaanslag worden uitgevoerd. 

Facetten zijn kenmerken van de gegevens (zoals de categorie van een hotel in onze voorbeeldgegevens), en blijf relevant zijn voor de levensduur van een zoekopdracht.

In deze zelfstudie maakt twee projecten, één voor facet navigatie en de andere voor facet automatisch aanvullen. Beide projecten bouwen op het wisselbestand-project hebt gemaakt in de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) zelfstudie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Stel de eigenschappen van het model als _IsFacetable_
> * Facet navigatie toevoegen aan uw app
> * Facet automatisch aanvullen toevoegen aan uw app
> * Bepalen wanneer u het facet automatisch aanvullen gebruiken

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

Hebben de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) project ingesteld en geactiveerd. Dit kunt uw eigen versie of installeer deze via GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="set-model-fields-as-isfacetable"></a>Set-model velden als IsFacetable

In de volgorde voor de modeleigenschap van een zich bevinden in een zoekopdracht facet (navigatie of automatisch aanvullen), moet het worden gelabeld met **IsFacetable**.

1. Bekijk de **Hotel** klasse. **Categorie** en **Tags**, bijvoorbeeld worden gemarkeerd als **IsFacetable**, maar **HotelName** en **beschrijving** niet. 

    ```cs
    public partial class Hotel
    {
        [System.ComponentModel.DataAnnotations.Key]
        [IsFilterable]
        public string HotelId { get; set; }

        [IsSearchable, IsSortable]
        public string HotelName { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnLucene)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Category { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public bool? ParkingIncluded { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public DateTimeOffset? LastRenovationDate { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public double? Rating { get; set; }

        public Address Address { get; set; }

        [IsFilterable, IsSortable]
        public GeographyPoint Location { get; set; }

        public Room[] Rooms { get; set; }

    }
    ```

2. We zullen niet worden gewijzigd alle tags in deze zelfstudie. Een zoekopdracht facet genereert een fout als een veld in de zoekopdracht wordt aangevraagd, niet op de juiste wijze is gemarkeerd.


## <a name="add-facet-navigation-to-your-app"></a>Facet navigatie toevoegen aan uw app

In dit voorbeeld gaan we de gebruiker een of meer categorieën van hotel, selecteren in een lijst die aan de linkerkant van de resultaten worden weergegeven. Moeten we de controller de lijst met categorieën weten wanneer de app voor het eerst wordt uitgevoerd en deze lijst doorgeven aan de weergave moet worden weergegeven als het eerste scherm wordt weergegeven. Omdat elke pagina wordt weergegeven, moet om ervoor te zorgen dat we hebben de lijst met facetten en de huidige gebruikersselecties, om te worden doorgegeven aan de volgende pagina's onderhouden. Nogmaals, gebruiken we tijdelijke opslag als het mechanisme voor het behouden van gegevens.

![Facet navigatie gebruiken om een zoekactie van 'groep' te beperken](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="modify-the-searchdata-model"></a>Het model SearchData wijzigen

1. Open het bestand SearchData.cs en voeg deze extra **met behulp van** instructie. Om in te schakelen moet de **lijst&lt;tekenreeks&gt;**  samenstellen.

    ```cs
    using System.Collections.Generic;
    ```

2. In hetzelfde bestand, voeg de volgende regels aan de **SearchData** klasse. Een van de bestaande klasse-eigenschappen niet verwijderen, maar Voeg de volgende Constructormethoden en matrices met eigenschappen.

    ```cs
        public SearchData()
        {
        }

        // Constructor to initialize the list of facets sent from the controller.
        public SearchData(List<string> facets)
        {
            facetText = new string[facets.Count];

            for (int i = 0; i < facets.Count; i++)
            {
                facetText[i] = facets[i];
            }
        }

        // Array to hold the text for each facet.
        public string[] facetText { get; set; }

        // Array to hold the check box setting.
        public bool[] facetOn { get; set; }
    ```


### <a name="search-for-facets-on-the-first-index-call"></a>Zoeken naar facetten in de eerste aanroep van de Index

De oorspronkelijke domeincontroller moet een ingrijpende wijziging. De eerste aanroep **index() heeft** langer retourneert een weergave met geen andere manier verwerken. We willen de weergave voorzien van een volledige lijst van facetten en de eerste aanroep is de juiste is voor dat doel.

1. Open het bestand home-controller, en Voeg twee **met behulp van** instructies.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

2. Vervang nu de regels van de huidige **index() heeft** methode met een methode die uitgevoerd een facet zoeken naar hotels categorieën wordt. Als de zoekopdracht moet asynchroon worden uitgevoerd, moeten we de **Index** methode **asynchrone**.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search for up to 20 categories.
                // Field names specified here must be marked as "IsFacetable" in the model, or the search call will throw an exception.
                Facets = new List<string> { "Category,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to a list that can be displayed in the client.
            List<string> categories = searchResult.Facets["Category"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(categories);

            // Save the facet text for the next view.
            SaveFacets(model);

            // Render the view including the facets.
            return View(model);
        }
    ```

    Enkele punten om te weten hier. We de resultaten van de aanroep van de zoekopdracht om een lijst met tekenreeksen te converteren en vervolgens deze tekenreeksen facet worden toegevoegd aan een **SearchData** modellen voor communicatie met de weergave. Ook opslaan we deze tekenreeksen naar de tijdelijke opslag voordat ten slotte de weergave. Deze opslaan wordt gedaan zodanig dat deze lijst beschikbaar voor de volgende aanroep van de actie van een domeincontroller is.

3. We gaan de twee particuliere methoden voor het opslaan en terugzetten facetten aan het model en naar de tijdelijke opslag toevoegen.

    ```cs
        // Save the facet text to temporary storage, optionally saving the state of the check boxes.
        private void SaveFacets(SearchData model, bool saveChecks = false)
        {
            for (int i = 0; i < model.facetText.Length; i++)
            {
                TempData["facet" + i.ToString()] = model.facetText[i];
                if (saveChecks)
                {
                    TempData["faceton" + i.ToString()] = model.facetOn[i];
                }
            }
            TempData["facetcount"] = model.facetText.Length;
        }

        // Recover the facet text to a model, optionally recoving the state of the check boxes.
        private void RecoverFacets(SearchData model, bool recoverChecks = false)
        {
            // Create arrays of the appropriate length.
            model.facetText = new string[(int)TempData["facetcount"]];
            if (recoverChecks)
            {
                model.facetOn = new bool[(int)TempData["facetcount"]];
            }

            for (int i = 0; i < (int)TempData["facetcount"]; i++)
            {
                model.facetText[i] = TempData["facet" + i.ToString()].ToString();
                if (recoverChecks)
                {
                    model.facetOn[i] = (bool)TempData["faceton" + i.ToString()];
                }
            }
        }
    ```

### <a name="save-and-restore-facet-text-on-all-calls"></a>Opslaan en terugzetten facet tekst op alle aanroepen

1. De twee andere acties van de oorspronkelijke controller **Index (SearchData model)** en **pagina (SearchData model)** , beide moeten herstellen van de facetten voordat de aanroep van de zoekopdracht en ze opnieuw na de aanroep van de zoekopdracht opslaan. Wijzig de **Index (SearchData model)** voor deze twee aanroepen.

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

                // Recover the facet text.
                RecoverFacets(model);

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;

                // Facets
                SaveFacets(model, true);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

2. Nu Doe hetzelfde voor de **pagina (SearchData model)** methode. We hebben alleen de relevante code hieronder vermeld. Voeg de **RecoverFacets** en **SaveFacets** aangeroepen om de **RunQueryAsync** aanroepen.

    ```cs
                // Recover facet text and check marks.
                RecoverFacets(model, true);

                await RunQueryAsync(model, page, leftMostPage);

                // Save facets and check marks.
                SaveFacets(model, true);
    ```

### <a name="set-up-a-search-filter"></a>Instellen van een zoekfilter

Wanneer een gebruiker bepaalde facetten selecteert, bijvoorbeeld zeggen dat ze Klik op de **Budget** en **redmiddel en beveiligd-wachtwoordverificatie** categorieën en klik vervolgens alleen hotels die zijn opgegeven als een van deze twee categorieën moet worden geretourneerd in de resultaten. Voor het optimaliseren van een zoekopdracht op deze manier, moeten we het instellen van een _filter_.

1. In de **RunQueryAsync** methode, voeg de code aan het opgegeven model facet instellingen, het maken van een filtertekenreeks doorlopen. En het filter toe te voegen de **SearchParameters**, zoals wordt weergegeven in de volgende code.

    ```cs
            // Create a filter for selected facets.
            string selectedFacets = "";

            for (int f = 0; f < model.facetText.Length; f++)
            {
                if (model.facetOn[f])
                {
                    if (selectedFacets.Length > 0)
                    {
                        // If there is more than one selected facet, logically OR them together.
                        selectedFacets += " or ";
                    }
                    selectedFacets += "(Category eq \'" + model.facetText[f] + "\')";
                }
            }

            var parameters = new SearchParameters
            {
                // Facets: add the filter.
                Filter = selectedFacets,

                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description", "Category" },
                SearchMode = SearchMode.All,

                // Skip past results that have already been returned.
                Skip = page * GlobalVariables.ResultsPerPage,

                // Take only the next page worth of results.
                Top = GlobalVariables.ResultsPerPage,

                // Include the total number of results.
                IncludeTotalResultCount = true,
            };
    ```

    We hebben toegevoegd de **categorie** eigenschap aan de lijst met **Selecteer** items om te retourneren. Toevoegen van deze eigenschap is geen vereiste, maar op deze manier kunnen we verifiëren dat we correct filtert.

### <a name="define-a-few-additional-html-styles"></a>Een paar extra HTML-stijlen definiëren

De weergave gaat vereisen enkele belangrijke wijzigingen. 

1. Begin met het openen van het bestand hotels.css (in de map wwwroot/css) en de volgende klassen toevoegen.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 200px;
        background-color: lightgoldenrodyellow;
        display: normal;
        color: #666;
        margin: 10px;
    }
    ```

### <a name="add-a-list-of-facet-checkboxes-to-the-view"></a>Een lijst met selectievakjes facet toevoegen aan de weergave

Voor de weergave organiseren we de uitvoer in een tabel, zodat deze netjes de facetten aan de linkerkant en de resultaten aan de rechterkant. Open het bestand index.cshtml.

1. Vervang de volledige inhoud van het HTML- &lt;hoofdtekst&gt; tags, met de volgende code.

    ```cs
    <body>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        <table>
            <tr>
                <td></td>
                <td>
                    <h1 class="sampleTitle">
                        <img src="~/images/azure-logo.png" width="80" />
                        Hotels Search - Facet Navigation
                    </h1>
                </td>
            </tr>

            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it.-->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input value="" class="searchBoxSubmit" type="submit">
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">
                        <h5>Filter by Category:</h5>
                        <ul class="facetlist">
                            @for (var i = 0; i < Model.facetText.Length; i++)
                            {
                                <li> @Html.CheckBoxFor(m => m.facetOn[i], new { @id = "check" + i.ToString() }) @Model.facetText[i] </li>
                            }
                        </ul>
                    </div>
                </td>
                <td valign="top">
                    <div id="resultsplace">
                        @if (Model != null && Model.resultList != null)
                        {
                            // Show the result count.
                            <p class="sampleText">
                                @Html.DisplayFor(m => m.resultList.Count) Results
                            </p>

                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                // Display the hotel name and description.
                                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                                @Html.TextArea("desc", Model.resultList.Results[i].Document.Description + "\nCategory:  " +  Model.resultList.Results[i].Document.Category, new { @class = "box2" })
                            }
                        }
                    </div>
                </td>
            </tr>

            <tr>
                <td></td>
                <td valign="top">
                    @if (Model != null && Model.pageCount > 1)
                    {
                        // If there is more than one page of results, show the paging buttons.
                        <table>
                            <tr>
                                <td class="tdPage">
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

                                <td class="tdPage">
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
                                    <td class="tdPage">
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

                                <td class="tdPage">
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

                                <td class="tdPage">
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
                </td>
            </tr>
        </table>
    }
    </body>
    ```

    Let op het gebruik van de **CheckBoxFor** aanroep voor het vullen van de **facetOn** matrix met de selecties voor de gebruiker. We hebben ook de categorie van het hotel toegevoegd aan het einde van het hotel beschrijving. Deze tekst is eenvoudig om te bevestigen dat onze zoekfunctie correct functioneert. Niet veel anders is gewijzigd van eerdere zelfstudies, behalve dat we de uitvoer in een tabel zijn georganiseerd.

### <a name="run-and-test-the-app"></a>Uitvoeren en testen van de app.

1. De app uitvoeren en controleer of dat de lijst met facetten netjes wordt weergegeven aan de linkerkant.

2. Probeer een, twee, drie of meer selectievakjes in te schakelen en de resultaten controleren.

    ![Facet navigatie gebruiken om een zoekactie van "Wi-Fi" te beperken](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

3. Er is een lichte complicatie met facet navigatie. Wat er moet gebeuren als een gebruiker de selectie facet (selecteren of ongedaan maken de selectievakjes in te schakelen) wordt gewijzigd, maar vervolgens klikt op een van de opties voor paginering en niet de zoekbalk? In feite moet de selectie wordt gewijzigd starten een nieuwe zoekopdracht, als de huidige pagina's niet langer juist. U kunt ook de wijzigingen van de gebruiker kunnen worden genegeerd, en de volgende pagina van de resultaten worden vermeld, op basis van de oorspronkelijke facet selecties. We de laatste oplossing hebt gekozen in dit voorbeeld, maar misschien overwegen hoe u de vorige oplossing kan implementeren. Een nieuwe zoekopdracht misschien geactiveerd als de meest recente selectie van de gekozen facetten komt niet exact overeen met de selectie in de tijdelijke opslag?

Ons voorbeeld van facet navigatie is voltooid. Maar misschien u kunt ook overwegen hoe deze app kan worden uitgebreid. De lijst met facet kan worden uitgebreid naar andere facet kunnen velden (bijvoorbeeld **Tags**), zodat een gebruiker een scala aan opties, zoals een groep van toepassingen, Wi-Fi, balk, gratis vervangende domeinpagina, enzovoort kan selecteren. 

Het voordeel van het facet navigatie aan de gebruiker is dat ze hoeft niet te houden de dezelfde tekst in te voeren, de mogelijkheden van hun facet bewaard voor de levensduur van de huidige sessie met de app. Deze categorieën kunnen selecteren en mogelijk andere kenmerken, met één klik vervolgens op andere specifieke tekst zoeken.

Nu gaan we bekijken een ander gebruik van facetten.

## <a name="add-facet-autocompletion-to-your-app"></a>Facet automatisch aanvullen toevoegen aan uw app

Facet automatisch aanvullen werkt door een eerste zoekopdracht wanneer de app voor het eerst wordt uitgevoerd. Deze zoekopdracht wordt een lijst met facetten moet worden gebruikt als suggesties wanneer het typen van de gebruiker is verzameld.

![Typ "opnieuw" ziet u drie facetten](./media/tutorial-csharp-create-first-app/azure-search-facet-type-re.png)

We gebruiken de genummerde wisselbestand-app die u mogelijk in de tweede zelfstudie hebt voltooid als basis voor dit voorbeeld.

Voor het implementeren van facet automatisch aanvullen, hoeft we geen niet te wijzigen van de modellen (de gegevensklassen). We hoeft sommige script toevoegen aan de weergave en een actie met de domeincontroller.

### <a name="add-an-autocomplete-script-to-the-view"></a>Een script automatisch aanvullen in de weergave toevoegen

Als u wilt een facet zoeken initiëren, moeten we een query verzenden. De volgende JavaScript toegevoegd aan het bestand index.cshtml bevat de querylogica en de presentatie die we nodig hebben.

1. Zoek de **@Html.TextBoxFor(m = > m.searchText,...)** instructie en een unieke ID, vergelijkbaar met het volgende toe te voegen.

    ```cs
    <div class="searchBoxForm">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresearchfacets" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

2. Nu de volgende JavaScript toevoegen (na de afsluitende **&lt;/div&gt;** werkt prima hierboven).

    ```JavaScript
     <script>
            $(function () {
                $.getJSON("/Home/Facets", function (data) {

                    $("#azuresearchfacets").autocomplete({
                        source: data,
                        minLength: 2,
                        position: {
                            my: "left top",
                            at: "left-23 bottom+10"
                        }
                    });
                });
            });
        </script>
    ```

    U ziet dat het script roept de **facetten** actie in de home-controller, zonder andere parameters, wanneer een minimumlengte van twee tekens is bereikt.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Verwijzingen naar jquery-scripts toevoegen aan de weergave

De functie automatisch aanvullen is met de naam in het bovenstaande script is niet iets we onszelf schrijven hebben als deze beschikbaar in de bibliotheek jquery is. 

1. Voor toegang tot de bibliotheek jquery, vervangen de &lt;head&gt; sectie van het bestand weergeven door de volgende code.

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Facets demo</title>
        <link href="https://code.jquery.com/ui/1.10.4/themes/ui-lightness/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.10.4/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. We ook wilt verwijderen, of uit, een regel die verwijst naar een jquery in het bestand _Layout.cshtml (in de **weergaven/gedeelde** map). Zoek de volgende regels en opmerkingen bij de eerste scriptopdrachtregel, zoals wordt weergegeven. Door deze regel is verwijderd, we te voorkomen dat niet-eenduidige verwijzingen naar jquery.

    ```html
     <environment include="Development">
            <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
            <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
            <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

We kunnen nu de vooraf gedefinieerde automatisch aanvullen jquery-functies gebruiken.

### <a name="add-a-facet-action-to-the-controller"></a>Een actie facet aan de controller toevoegen

1. Open de startpagina controller en voeg de volgende twee **met behulp van** instructies toe aan de kop van het bestand.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

2. JavaScript in de weergave-triggers de **facetten** actie in de controller, dus gaan we die actie toevoegen aan de home-controller (bijvoorbeeld onder de **pagina** actie).

    ```cs
        public async Task<ActionResult> Facets()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search all Tags, but limit the total number to 100, and add up to 20 categories.
                // Field names specified here must be marked as "IsFacetable" in the model, or the search call will throw an exception.
                Facets = new List<string> { "Tags,count:100", "Category,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to two lists that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();
            List<string> categories = searchResult.Facets["Category"].Select(x => x.Value.ToString()).ToList();

            // Combine and return the lists.
            facets.AddRange(categories);
            return new JsonResult(facets);
        }
    ```

    U ziet dat we aanvragen van maximaal 100 facetten uit de **Tags** velden, en tot maximaal 20 van de **categorie** velden. De **aantal** vermeldingen zijn optioneel, als er is geen aantal is ingesteld. de standaardwaarde is 10.

    Moeten we twee lijsten, die vervolgens worden gecombineerd in één, omdat we gevraagd om twee velden waarin wordt gezocht (**Tags** en **categorie**). Als we hadden gevraagd voor drie velden waarin wordt gezocht, zouden we hebben drie lijsten combineren tot één, enzovoort.

    > [!NOTE]
    > Het is mogelijk om in te stellen van een of meer van de volgende parameters voor elk veld in een zoekopdracht facet: **aantal**, **sorteren**, **interval**, en **waarden**. Zie voor meer informatie, [facetnavigatie implementeren in Azure Search](https://docs.microsoft.com/azure/search/search-faceted-navigation).

### <a name="compile-and-run-your-project"></a>Compileren en uitvoeren van uw project

Het programma nu testen.

1. Probeer "fr" in het zoekvak typt, die verschillende resultaten moet worden weergegeven.

    !["Fr" typen ziet u drie facetten](./media/tutorial-csharp-create-first-app/azure-search-facet-type-fr.png)

2. Voeg nu een "o" voor "voor" en ziet u dat het bereik van de opties is verlaagd tot één.

3. Typ andere combinaties van twee letters en wat wordt weergegeven. U ziet dat wanneer u de server is *niet* die wordt aangeroepen. De facetten worden lokaal opgeslagen wanneer de app wordt gestart en nu wordt een aanroep alleen uitgevoerd op de server wanneer de gebruiker vraagt om een zoekopdracht.

## <a name="decide-when-to-use-a-facet-autocompletion-search"></a>Bepalen wanneer u het facet automatisch aanvullen zoeken

Het duidelijk verschil tussen het facet zoekopdrachten en andere zoekopdrachten, zoals suggesties en automatisch aanvullen, is dat de zoekopdracht facet _ontworpen_ alleen uit te voeren eenmaal als een pagina wordt geladen. De andere automatisch aanvullen zoekopdrachten _ontworpen_ moet worden aangeroepen nadat elke teken wordt ingevoerd. Facetten met opgeslagen op deze manier mogelijk veel aanroepen naar de server. 

Echter, wanneer moet automatisch aanvullen facet worden gebruikt?

Facet automatisch aanvullen is de beste wordt gebruikt:
* De belangrijkste reden is dat de prestaties van andere zoekopdrachten die aanroepen van de server elke toetsaanslag een probleem is.
* De facetten geretourneerd biedt gebruikers een lijst met opties van redelijke wanneer deze in een paar tekens typt.
* De facetten geretourneerd bieden een snelle manier om toegang tot meest, of in het ideale geval alle van de gegevens beschikbaar.
* Het maximale aantal's kunnen de meeste facetten moeten worden opgenomen. In de code, stellen we een maximum van 100 facetten voor **Tags** en 20 facetten voor **categorie**. De set maximumwaarden moet werken goed met de grootte van de gegevensset. Als er te veel mogelijke facetten zijn wordt knip-, klikt u vervolgens is misschien de zoekopdracht niet als nuttig beoordeeld als deze moet worden.

> [!NOTE]
> Hoewel het facet zoekopdrachten zijn ontworpen om te worden aangeroepen zodra per pagina laden, ze kunnen natuurlijk worden aangeroepen veel vaker worden gebruikt, is deze afhankelijk is van uw JavaScript. Even waar is dat automatisch aanvullen/suggestie zoekopdrachten minder vaak meer dan één keer per toetsaanslag kunnen worden uitgevoerd. Dit wordt weer bepaald door uw JavaScript, niet Azure Search. Facet zoeken is echter ontworpen voor slechts één keer per pagina worden aangeroepen als facetten door Azure Search zijn samengesteld uit de gezochte documenten met dit in gedachten. Het is raadzaam om naar het facet automatisch aanvullen zoekopdrachten beschouwen als een iets minder flexibel maar meer netwerk efficiënt vorm van een gebruiker hulp.

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Het is belangrijk om elk veld als markeren **IsFacetable**, als ze moeten worden opgenomen in de navigatie in de facet of automatisch aanvullen.
* Facet navigatie biedt een gebruiker met een eenvoudige en intuïtieve manier van het verkleinen van een zoekopdracht.
* Facet navigatie is best onderverdeeld in secties (categorieën van hotel), de functies van een hotel, prijsbereiken, enz., elke sectie met de juiste koptekst.
* Facet automatisch aanvullen is een efficiënte manier voor het ophalen van een nuttige ervaring zonder de herhaalde server aanroepen van andere zoekopdrachten automatisch aanvullen van.
* Facet automatisch aanvullen is een _alternatieve_ automatisch aanvullen/suggesties, niet een toevoeging.

## <a name="next-steps"></a>Volgende stappen

U hebt deze reeks C# zelfstudies - u moet hebben opgedaan waardevolle kennis van de Azure Search-API's.

Voor verdere verwijzing en zelfstudies, kunt u overwegen Bladeren [Microsoft Learn](https://docs.microsoft.com/learn/browse/?products=azure), of de andere zelfstudies in de [documentatie voor Azure Search](https://docs.microsoft.com/azure/search/).
