---
title: C#zelfstudie over het gebruik van facetten kunnen helpen een navigatie - Azure Search
description: Deze zelfstudie bouwt voort op het project "Paginering - Azure Search zoekresultaten" facet navigatie toevoegen. Meer informatie over dat facetten kunnen worden gebruikt voor het eenvoudig Verfijn een zoekopdracht.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/20/2019
ms.openlocfilehash: 62326ad3bc5f2d740ce744819df559bce8658eb7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443785"
---
# <a name="c-tutorial-use-facets-to-aid-navigation---azure-search"></a>C#zelfstudie: Facetten gebruiken om u te helpen de navigatie - Azure Search

Facetten worden gebruikt om u te helpen bij het vinden, door de gebruiker voorzien van een set van links naar gebruiken om de zoekopdracht te verfijnen. Facetten zijn kenmerken van de gegevens (zoals de categorie, of een specifieke functie, van een hotel in onze voorbeeldgegevens).

Deze zelfstudie bouwt voort op het wisselbestand-project hebt gemaakt in de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) zelfstudie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Stel de eigenschappen van het model als _IsFacetable_
> * Facet navigatie toevoegen aan uw app

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

Hebben de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) project ingesteld en geactiveerd. Dit project kunt uw eigen versie of installeer deze via GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="set-model-properties-as-isfacetable"></a>Eigenschappen van de set-model als IsFacetable

In de volgorde voor de modeleigenschap van een zich bevinden in een zoekopdracht facet, moet het worden gelabeld met **IsFacetable**.

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

2. We zullen niet worden gewijzigd alle tags als onderdeel van deze zelfstudie, dus sluit het bestand hotel.cs ongewijzigd.

    > [!Note]
    > Een zoekopdracht facet genereert een fout als een veld in de zoekopdracht wordt aangevraagd, niet op de juiste wijze is gemarkeerd.


## <a name="add-facet-navigation-to-your-app"></a>Facet navigatie toevoegen aan uw app

In dit voorbeeld gaan we om in te schakelen van de gebruiker selecteert een categorie hotel of een openbare groenvoorzieningen, uit een lijst met koppelingen die aan de linkerkant van de resultaten worden weergegeven. De gebruiker wordt gestart door in te voeren sommige zoektekst, en vervolgens de resultaten van de zoekopdracht kunnen beperken door een categorie te selecteren en kunnen de resultaten verder beperken door het selecteren van een openbare groenvoorzieningen of kunnen ze de openbare groenvoorzieningen selecteren eerst (de volgorde is niet belangrijk).

Moeten we de controller om door te geven van de lijsten met facetten naar de weergave. We moeten onderhouden van de selecties voor de gebruiker tijdens de voortgang van de zoekopdracht en opnieuw gebruiken we tijdelijke opslag als het mechanisme voor het behouden van gegevens.

![Facet navigatie gebruiken om een zoekactie van 'groep' te beperken](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="add-filter-strings-to-the-searchdata-model"></a>Tekenreeksen toevoegen aan het model SearchData

1. Open het bestand SearchData.cs en voeg tekenreekseigenschappen toe aan de **SearchData** klasse voor het opslaan van het facet tekenreeksen.

    ```cs
        public string categoryFilter { get; set; }
        public string amenityFilter { get; set; }
    ```

### <a name="add-the-facet-action-method"></a>De actie Facet methode toevoegen

De oorspronkelijke domeincontroller moet een nieuwe actie, **Facet**, en updates van de bestaande **Index** en **pagina** acties, evenals de updates voor de **RunQueryAsync**  methode.

1. Open het bestand home controller en voeg de **met behulp van** instructie waarmee de **lijst&lt;tekenreeks&gt;**  samenstellen.

    ```cs
    using System.Collections.Generic;
    ```

2. Vervang de **Index (SearchData model)** actiemethode.

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
                await RunQueryAsync(model, 0, 0, "", "");
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. Vervang de **pagina (SearchData model)** actiemethode.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                // Calculate the page that should be displayed.
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

                // Recover the filters.
                string catFilter = TempData["categoryFilter"].ToString();
                string ameFilter = TempData["amenityFilter"].ToString();

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Search for the new page.
                await RunQueryAsync(model, page, leftMostPage, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

4. Voeg een **Facet (SearchData model)** actiemethode worden geactiveerd wanneer de gebruiker op de koppeling van een facet klikt. Het model bevat een zoekfilter categorie of een openbare groenvoorzieningen zoekfilter. Misschien toe te voegen na de **pagina** actie.

    ```cs
        public async Task<ActionResult> Facet(SearchData model)
        {
            try
            {
                // Filters set by the model override those stored in temporary data.
                string catFilter;
                string ameFilter;
                if (model.categoryFilter != null)
                {
                    catFilter = model.categoryFilter;
                } else
                {
                    catFilter = TempData["categoryFilter"].ToString();
                }

                if (model.amenityFilter != null)
                {
                    ameFilter = model.amenityFilter;
                } else
                {
                    ameFilter = TempData["amenityFilter"].ToString();
                }

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Initiate a new search.
                await RunQueryAsync(model, 0, 0, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

### <a name="set-up-the-search-filter"></a>Instellen van het zoekfilter

Wanneer een gebruiker een bepaalde facet selecteert, bijvoorbeeld ze Klik op de **redmiddel en beveiligd-wachtwoordverificatie** categorie en klik vervolgens alleen hotels die zijn opgegeven als deze categorie in de resultaten moet worden geretourneerd. Als u wilt een zoekopdracht op deze manier beperken, moet voor het instellen van een _filter_.

1. Vervang de **RunQueryAsync** methode met de volgende code. Primair, het gebruikt een filtertekenreeks categorie en een filtertekenreeks openbare groenvoorzieningen en stelt de **Filter** parameter van de **SearchParameters**.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage, string catFilter, string ameFilter)
        {
            InitSearch();

            string facetFilter = "";

            if (catFilter.Length > 0 && ameFilter.Length > 0)
            {
                // Both facets apply.
                facetFilter = $"{catFilter} and {ameFilter}"; 
            } else
            {
                // One, or zero, facets apply.
                facetFilter = $"{catFilter}{ameFilter}";
            }

            var parameters = new SearchParameters
            {
                Filter = facetFilter,

                // Return information on the text, and number, of facets in the data.
                Facets = new List<string> { "Category,count:20", "Tags,count:20" },

                // Enter Hotel property names into this list, so only these values will be returned.
                Select = new[] { "HotelName", "Description", "Category", "Tags" },

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
            model.pageCount = ((int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

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

            // Ensure Temp data is stored for the next call.
            TempData["page"] = page;
            TempData["leftMostPage"] = model.leftMostPage;
            TempData["searchfor"] = model.searchText;
            TempData["categoryFilter"] = catFilter;
            TempData["amenityFilter"] = ameFilter;

            // Return the new view.
            return View("Index", model);
        }
    ```

    We hebben toegevoegd de **categorie** en **Tags** eigenschappen aan de lijst met **Selecteer** items om te retourneren. Deze toevoeging is geen vereiste voor facet navigatie om te werken, maar we gebruiken deze gegevens om te controleren dat we correct filtert.

### <a name="add-lists-of-facet-links-to-the-view"></a>Lijsten van facet koppelingen naar de weergave toevoegen

De weergave gaat vereisen enkele belangrijke wijzigingen. 

1. Begin met het openen van het bestand hotels.css (in de map wwwroot/css) en de volgende klassen toevoegen.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;    
    }
    ```

2. Voor de weergave wordt de uitvoer indelen in een tabel, om te worden uitgelijnd netjes het facet geeft een lijst aan de linkerkant en de resultaten aan de rechterkant. Open het bestand index.cshtml. Vervang de volledige inhoud van het HTML- &lt;hoofdtekst&gt; tags, met de volgende code.

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

                        @if (Model != null && Model.resultList != null)
                        {
                            List<string> categories = Model.resultList.Facets["Category"].Select(x => x.Value.ToString()).ToList();

                            if (categories.Count > 0)
                            {
                                <h5 class="facetheader">Category:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < categories.Count; c++)
                                    {
                                        var facetLink = $"{categories[c]} ({Model.resultList.Facets["Category"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { categoryFilter = $"Category eq '{categories[c]}'" }, null)
                                        </li>
                                    }
                                </ul>
                            }

                            List<string> tags = Model.resultList.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

                            if (tags.Count > 0)
                            {
                                <h5 class="facetheader">Amenities:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < tags.Count; c++)
                                    {
                                        var facetLink = $"{tags[c]} ({Model.resultList.Facets["Tags"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { amenityFilter = $"Tags/any(t: t eq '{tags[c]}')" }, null)
                                        </li>
                                    }
                                </ul>
                            }
                        }
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
                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);

                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nCategory: {Model.resultList.Results[i].Document.Category}";
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel name and description.
                                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box2" })
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

    Let op het gebruik van de **Html.ActionLink** aanroepen. Deze aanroep communiceert geldige tekenreeksen met de domeincontroller, wanneer de gebruiker een facet-koppeling. 

### <a name="run-and-test-the-app"></a>Uitvoeren en testen van de app.

Het voordeel van het facet navigatie aan de gebruiker is dat ze zoekopdrachten met één klik, die we in de volgende reeks weergeven kunt kunnen beperken.

1. De app, het type 'luchthaven' worden uitgevoerd als de zoektekst. Controleer of dat de lijst met facetten netjes wordt weergegeven aan de linkerkant. Deze facetten zijn alle die betrekking hebben op hotels waarvoor 'luchthaven' in hun tekstgegevens met een telling van hoe vaak ze voorkomen.

    ![Facet navigatie gebruiken om een zoekactie van "luchthaven" te beperken](./media/tutorial-csharp-create-first-app/azure-search-facet-airport.png)

2. Klik op de **redmiddel en beveiligd-wachtwoordverificatie** categorie. Controleer of dat alle resultaten worden in deze categorie.

    ![Verkleinen om te zoeken naar "Redmiddel en beveiligd-wachtwoordverificatie"](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras.png)

3. Klik op de **ontbijt** openbare groenvoorzieningen. Controleer of dat alle resultaten zijn nog steeds in de categorie "Redmiddel en beveiligd-wachtwoordverificatie" met de geselecteerde openbare groenvoorzieningen.

    ![Verkleinen om te zoeken naar "ontbijt"](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras-cb.png)

4. Probeer een andere categorie, klikt u vervolgens een openbare groenvoorzieningen, selecteren en de beperkende resultaten te bekijken. Probeer de andere manier om een openbare groenvoorzieningen en vervolgens een categorie.

    >[!Note]
    > Wanneer u een selectie is gemaakt in een lijst facet (zoals categorie) overschrijft alle vorige selectie in de lijst met categorieën.

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Het is belangrijk om het markeren van elke eigenschap als **IsFacetable**, als ze moeten worden opgenomen in facet navigatie.
* Facet navigatie biedt een gebruiker met een eenvoudige en intuïtieve manier van het verkleinen van een zoekopdracht.
* Facet navigatie is best onderverdeeld in secties (categorieën van hotel), faciliteiten van een hotel, prijsbereiken, classificatie bereiken, enz., elke sectie met de juiste koptekst.

## <a name="next-steps"></a>Volgende stappen

In de volgende zelfstudie kijken we resultaten ordenen. Tot nu, worden de resultaten gerangschikt gewoon in de volgorde waarin ze bevinden zich in de database.

> [!div class="nextstepaction"]
> [C#zelfstudie: Volgorde van de resultaten - Azure Search](tutorial-csharp-orders.md)
