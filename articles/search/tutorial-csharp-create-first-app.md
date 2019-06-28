---
title: C#zelfstudie voor het maken van uw eerste toepassing - Azure Search
description: Deze zelfstudie bevat een stapsgewijze handleiding voor het bouwen van uw eerste app voor Azure Search. De zelfstudie bevat zowel een koppeling naar een werkende app op GitHub en het volledige proces voor het volledig nieuwe app bouwen. Meer informatie over de essentiële onderdelen van Azure Search.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: d569437a3e6f6f05ddb9c6fa85f62c77ac51f72b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443807"
---
# <a name="c-tutorial-create-your-first-app---azure-search"></a>C#zelfstudie: Uw eerste app maken - Azure Search

Informatie over het maken van een webinterface voor query's en aanwezig zoekresultaten uit een index met behulp van Azure Search. Deze zelfstudie begint met een bestaande, gehoste index, zodat u zich richten kunt op het bouwen van een zoekpagina. De index bevat fictieve Hotelgegevens. Zodra u een standaard-pagina hebt, kunt u deze kunt verbeteren in de volgende lessen wisselbestanden, facetten en een, automatisch aangevulde ervaring op te nemen.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een ontwikkelomgeving instellen
> * Model-gegevensstructuren
> * Een webpagina maken
> * Methoden definiëren
> * De app testen

U leert ook hoe eenvoudig een search-aanroep is. De belangrijkste instructies in de code die u ontwikkelen worden ingekapseld in de volgende paar regels.

```cs
var parameters = new SearchParameters
{
    // Enter Hotel property names into this list, so only these values will be returned.
    Select = new[] { "HotelName", "Description" }
};

DocumentSearchResult<Hotel> results  = await _indexClient.Documents.SearchAsync<Hotel>("search text", parameters);
```

Deze één aanroep initieert een zoekopdracht van de Azure-gegevens en worden de resultaten geretourneerd.

![Zoeken naar 'groep'](./media/tutorial-csharp-create-first-app/azure-search-pool.png)


## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

[Visual Studio installeren](https://visualstudio.microsoft.com/) om te gebruiken als de IDE.

### <a name="install-and-run-the-project-from-github"></a>Installeren en uitvoeren van het project vanuit GitHub

1. Zoek in het voorbeeld op GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Selecteer **klonen of downloaden** en maakt u uw persoonlijke lokale kopie van het project.
1. Met behulp van Visual Studio, gaat u naar, en open de oplossing voor de basis-Zoek-pagina en selecteer **starten zonder foutopsporing** (of druk op F5).
1. Typ in een bepaalde woorden (bijvoorbeeld "Wi-Fi", "view", "balk", 'van de vervangende domeinpagina') en bekijk de resultaten!

    ![Zoeken naar "Wi-Fi"](./media/tutorial-csharp-create-first-app/azure-search-wifi.png)

Hopelijk dit project goed wordt uitgevoerd, en u hebt Azure-app wordt uitgevoerd. Veel van de essentiële onderdelen voor meer geavanceerde zoekopdrachten zijn opgenomen in deze bepaalde Apps, dus is het een goed idee om te gaan via deze en maakt u deze stap voor stap opnieuw.

Als u dit project maken vanaf het begin en kan daarom te versterken van de onderdelen van Azure Search in gedachten, gaat u door de volgende stappen.

## <a name="set-up-a-development-environment"></a>Een ontwikkelomgeving instellen

1. Selecteer in Visual Studio 2017 of hoger, **nieuw/Project** vervolgens **ASP.NET Core-webtoepassing**. Geef het project een naam, zoals 'FirstAzureSearchApp'.

    ![Een cloudproject maken](./media/tutorial-csharp-create-first-app/azure-search-project1.png)

2. Nadat u hebt geklikt op **OK** voor dit projecttype, krijgt u een tweede set met opties die betrekking hebben op dit project. Selecteer **webtoepassing (Model-View-Controller)** .

    ![Het maken van een MVC-project](./media/tutorial-csharp-create-first-app/azure-search-project2.png)

3. Vervolgens in de **extra** in het menu **NuGet Package Manager** en vervolgens **NuGet-pakketten beheren voor oplossing...** . Er is een pakket die we wilt installeren. Selecteer de **Bladeren** tab en typt u 'Azure Search' in het zoekvak. Installeer **Microsoft.Azure.Search** wanneer deze wordt weergegeven in de lijst (versie 9.0.1, of hoger). U moet klikt u op via een paar extra dialoogvensters om de installatie te voltooien.

    ![Azure-bibliotheken toevoegen met behulp van NuGet](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

### <a name="initialize-azure-search"></a>Azure Search initialiseren

Voor dit voorbeeld gebruiken we openbaar beschikbare Hotelgegevens. Deze gegevens is een willekeurige verzameling 50 hotel fictieve namen en beschrijvingen, die worden gemaakt uitsluitend voor de demogegevens levert. Voor toegang tot deze gegevens, moet u een naam opgeven en de sleutel voor het.

1. Open het bestand appsettings.json in het nieuwe project en de standaardregels vervangen door de volgende naam en sleutel. De API-sleutel die wordt weergegeven niet is hier een voorbeeld van een sleutel, is _precies_ de sleutel die u nodig hebt voor toegang tot de Hotelgegevens. Het bestand appsettings.json moet er nu als volgt uitzien.

    ```cs
    {
        "SearchServiceName": "azs-playground",
        "SearchServiceQueryApiKey": "EA4510A6219E14888741FCFC19BFBB82"
    }
    ```

2. We niet klaar bent met dit bestand nog, selecteert u de eigenschappen voor dit bestand en wijzig de **naar uitvoermap kopiëren** instelt op **kopiëren indien nieuwer**.

    ![Kopiëren van de app-instellingen naar de uitvoer](./media/tutorial-csharp-create-first-app/azure-search-copy-if-newer.png)

## <a name="model-data-structures"></a>Model-gegevensstructuren

Modellen (C# klassen) worden gebruikt voor communicatie van gegevens tussen de client (weergeven), de server (de controller) en de Azure-cloud met behulp van de architectuur MVC (model, weergave, domeincontroller). Deze modellen weer normaal gesproken de structuur van de gegevens die wordt geopend. Moet u er ook een model voor het afhandelen van de communicatie view-controller.

1. Open de **modellen** map van uw project, met behulp van Solution Explorer, en u ziet één standaardmodel er: **ErrorViewModel.cs**.

2. Met de rechtermuisknop op de **modellen** map en selecteer **toevoegen** vervolgens **Nieuw Item**. Selecteer in het dialoogvenster dat verschijnt, **ASP.NET Core** vervolgens de eerste optie **klasse**. Naam van het bestand .cs Hotel.cs en klikt u op **toevoegen**. De inhoud van Hotel.cs vervangen door de volgende code. U ziet dat de **adres** en **ruimte** leden van de klasse, deze velden zijn klassen zelf, zodat we wordt deze ook modellen nodig.

    ```cs
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Spatial;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
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
    }
    ```

3. Doe hetzelfde voor het maken van een model voor de **adres** klasse, behalve de naam van het bestand Address.cs. Vervang de inhoud door het volgende.

    ```cs
    using Microsoft.Azure.Search;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Address
        {
            [IsSearchable]
            public string StreetAddress { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string City { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string StateProvince { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string PostalCode { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Country { get; set; }
        }
    }
    ```

4. En volg opnieuw de hetzelfde proces maken de **ruimte** klasse, het bestand Room.cs naam. Vervang de inhoud weer met de volgende.

    ```cs
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Room
        {
            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]

            public string Description { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
            [JsonProperty("Description_fr")]
            public string DescriptionFr { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string Type { get; set; }

            [IsFilterable, IsFacetable]
            public double? BaseRate { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string BedOptions { get; set; }

            [IsFilterable, IsFacetable]

            public int SleepsCount { get; set; }

            [IsFilterable, IsFacetable]
            public bool? SmokingAllowed { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string[] Tags { get; set; }
        }
    }
    ```

5. De set **Hotel**, **adres**, en **ruimte** klassen zijn wat is bekend in Azure als [ _complexe typen_ ](search-howto-complex-data-types.md), een belangrijke functie van Azure Search. Complexe typen kunnen worden veel niveaus van klassen en subklassen en veel meer complexe gegevensstructuren kunnen worden weergegeven dan het gebruik van inschakelen _eenvoudige typen_ (een klasse met alleen primitieve leden). We één meer model hoeven, dus gaat u door het proces voor het maken van een nieuwe modelklasse opnieuw, echter deze tijd aanroepen van de klasse SearchData.cs en vervang de standaardcode door het volgende.

    ```cs
    using Microsoft.Azure.Search.Models;

    namespace FirstAzureSearchApp.Models
    {
        public class SearchData
        {
            // The text to search for.
            public string searchText { get; set; }

            // The list of results.
            public DocumentSearchResult<Hotel> resultList;
        }
    }
    ```

    Deze klasse bevat de invoer van de gebruiker (**Tekstdoorzoeken**), en de uitvoer van de zoekopdracht (**resultList**). Het type van de uitvoer wordt kritieke, **DocumentSearchResult&lt;Hotel&gt;** , zoals dit type exact overeenkomt met de resultaten van de zoekopdracht en we moeten deze verwijzing in naar de weergave doorgeeft.



## <a name="create-a-web-page"></a>Een webpagina maken

Het project dat u hebt gemaakt wordt standaard een aantal client weergaven maken. De exacte weergaven, is afhankelijk van welke versie van Core .NET u (we gebruiken 2.1 in dit voorbeeld). Ze bevinden zich allemaal in de **weergaven** map van het project. U hoeft te wijzigen van het bestand Index.cshtml (in de **weergaven en thuis** map).

De inhoud van Index.cshtml in zijn geheel te verwijderen en opnieuw maken van het bestand in de volgende stappen uit.

1. We twee kleine afbeeldingen gebruiken in de weergave. U kunt uw eigen of kopiëren via de installatiekopieën van het GitHub-project: azure-logo.png en search.png. Deze twee installatiekopieën moeten worden geplaatst de **wwwroot/images** map.

2. De eerste regel van Index.cshtml moet verwijzen naar het model wordt gebruikt voor het communiceren van gegevens tussen de client (weergeven) en de server (de controller), is de **SearchData** model dat we hebben gemaakt. Deze regel toevoegen aan het bestand Index.cshtml.

    ```cs
    @model FirstAzureSearchApp.Models.SearchData
    ```

3. Het is standaard om Voer een titel voor de weergave, zodat de volgende regels:

    ```cs
    @{
        ViewData["Title"] = "Home Page";
    }
    ```

4. Voer na de titel, een verwijzing naar een HTML-stylesheet, die we gaan maken.

    ```cs
    <head>
        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

5. Nu op het vlees van de weergave. Een belangrijk te onthouden is dat de weergave voor het afhandelen van twee situaties. Deze moet de weergave in de eerste plaats omgaan met wanneer de app voor het eerst wordt gestart en de gebruiker nog geen zoektekst is opgegeven. Deze moet op de tweede plaats omgaan met de weergave van resultaten, naast het zoekvak, voor gebruik door de gebruiker. Voor het afhandelen van deze twee situaties, moeten we controleren of het model dat is opgegeven voor de weergave null of niet is. Een null-model geeft aan dat er zich in de eerste dag van de twee situaties (de eerste uitvoering van de app). Voeg het volgende toe aan het bestand Index.cshtml en lees de opmerkingen.

    ```cs
    <body>
    <h1 class="sampleTitle">
        <img src="~/images/azure-logo.png" width="80" />
        Hotels Search
    </h1>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        // Display the search text box, with the search icon to the right of it.
        <div class="searchBoxForm">
            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
        </div>

        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Results.Count) Results
            </p>

            @for (var i = 0; i < Model.resultList.Results.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
            }
        }
    }
    </body>
    ```

6. Ten slotte toevoegen we het opmaakmodel. In Visual Studio in de **bestand** menu Selecteer **nieuw/bestand** vervolgens **opmaakmodel** (met **algemene** gemarkeerde). Vervang de standaardcode door het volgende. Gaan we het zal niet worden in dit bestand in een meer details, zijn de stijlen standaard-HTML.

    ```html
    textarea.box1 {
        width: 648px;
        height: 30px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
    }

    textarea.box2 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }

    .sampleTitle {
        font: 32px/normal 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 32px;
        text-align: left;
    }

    .sampleText {
        font: 16px/bold 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 14px;
        text-align: left;
        height: 30px;
    }

    .searchBoxForm {
        width: 648px;
        box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 4px 0 rgba(0,0,0,.16);
        background-color: #fff;
        display: inline-block;
        border-collapse: collapse;
        border-spacing: 0;
        list-style: none;
        color: #666;
    }

    .searchBox {
        width: 568px;
        font-size: 16px;
        margin: 5px 0 1px 20px;
        padding: 0 10px 0 0;
        border: 0;
        max-height: 30px;
        outline: none;
        box-sizing: content-box;
        height: 35px;
        vertical-align: top;
    }

    .searchBoxSubmit {
        background-color: #fff;
        border-color: #fff;
        background-image: url(/images/search.png);
        background-repeat: no-repeat;
        height: 20px;
        width: 20px;
        text-indent: -99em;
        border-width: 0;
        border-style: solid;
        margin: 10px;
        outline: 0;
    }
    ```

7. Sla het bestand stylesheet als hotels.css, in de map wwwroot/css, samen met het standaardbestand site.css.

De weergave is voltooid. We stellen goed wordt uitgevoerd. De modellen en weergaven zijn uitgevoerd, kunt u alleen de controller nodespecid alles met elkaar verbinden.

## <a name="define-methods"></a>Methoden definiëren

We moeten aanpassen om de inhoud van de ene domeincontroller (**start Controller**), die standaard wordt gemaakt.

1. Open het bestand HomeController.cs en vervang de **met behulp van** instructies met de volgende stappen.

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using FirstAzureSearchApp.Models;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    ```

### <a name="add-index-methods"></a>Index methoden toevoegen

Moeten we twee **Index** methoden, één waarbij er geen parameters (voor het geval wanneer de app voor het eerst wordt geopend) en één nemen van een model als een parameter (voor de gebruiker wanneer de zoektekst opgegeven heeft). De eerste dag van deze methoden wordt standaard gemaakt. 

1. Voeg de volgende methode toe na de standaardwaarde **index() heeft** methode.

    ```cs
        [HttpPost]
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the Azure Search call.
                await RunQueryAsync(model);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

    U ziet dat de **asynchrone** declaratie van de methode en de **await** aanroepen **RunQueryAsync**. Deze trefwoorden zorgen voor het maken van onze aanroepen asynchrone en Vermijd daarom blokkering van threads op de server.

    De **catch** blok maakt gebruik van de fout-model dat is gemaakt voor ons standaard.

### <a name="note-the-error-handling-and-other-default-views-and-methods"></a>Houd er rekening mee de foutafhandeling en andere standaardweergaven en -methoden

Afhankelijk van welke versie van .NET Core u gebruikt, een iets andere set standaard worden standaard weergaven gemaakt. Voor .NET Core 2.1 worden de standaardweergaven Index, over, Contact, Privacy en fout. Voor .NET Core 2.2, bijvoorbeeld zijn de standaardweergaven Index, Privacy en fout. In beide gevallen kunt u deze pagina standaard weergeven bij het uitvoeren van de app en onderzoeken hoe ze worden verwerkt in de controller.

Er wordt getest. de fout weergave later in deze zelfstudie.

In de GitHub-voorbeeld hebben we de niet-gebruikte weergaven en hun gekoppelde bewerkingen verwijderd.

### <a name="add-the-runqueryasync-method"></a>De methode RunQueryAsync toevoegen

De Azure Search-aanroep is ingekapseld in onze **RunQueryAsync** methode.

1. Enkele statische variabelen voor het instellen van de Azure-service en een aanroep om te starten op deze eerst toevoegen.

    ```cs
        private static SearchServiceClient _serviceClient;
        private static ISearchIndexClient _indexClient;
        private static IConfigurationBuilder _builder;
        private static IConfigurationRoot _configuration;

        private void InitSearch()
        {
            // Create a configuration using the appsettings file.
            _builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            _configuration = _builder.Build();

            // Pull the values from the appsettings.json file.
            string searchServiceName = _configuration["SearchServiceName"];
            string queryApiKey = _configuration["SearchServiceQueryApiKey"];

            // Create a service and index client.
            _serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(queryApiKey));
            _indexClient = _serviceClient.Indexes.GetClient("hotels");
        }
    ```

2. Voeg nu toe de **RunQueryAsync** methode zelf.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description" }
            };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // Display the results.
            return View("Index", model);
        }
    ```

    Bij deze methode wordt er eerst voor zorgen onze Azure-configuratie is gestart en vervolgens enkele zoekparameters instellen. De namen van de velden in de **Selecteer** parameter overeen met precies de namen van eigenschappen in de **hotel** klasse. Het is mogelijk te laat u uit de **Selecteer** parameter, in welk geval alle eigenschappen worden geretourneerd. Instellen van echter geen **Selecteer** parameters is inefficiënt als we alleen geïnteresseerd bent in een subset van de gegevens. De eigenschappen zijn we geïnteresseerd in opgeeft, zijn alleen deze eigenschappen worden geretourneerd.

    De asynchrone aanroep om te zoeken naar (**model.resultList = await _indexClient.Documents.SearchAsync&lt;Hotel&gt;(model.searchText, parameters);** ) is deze zelfstudie en de app weet alles over. De **DocumentSearchResult** klasse is een interessante en een goed idee (wanneer de app wordt uitgevoerd) is hier een onderbrekingspunt instellen en een foutopsporingsprogramma gebruiken om te controleren van de inhoud van **model.resultList**. U ziet, is het gemakkelijk, zodat u met de gegevens die u zoekt, en niet veel anders is.

Nu in voor het moment van waarheid.

### <a name="test-the-app"></a>De app testen

Nu gaan we controleren correct of de app wordt uitgevoerd.

1. Selecteer **foutopsporing/starten zonder foutopsporing** of druk op F5. Als u hebt dingen correct gecodeerd, krijgt u de eerste weergave in de Index.

     ![De app te openen](./media/tutorial-csharp-create-first-app/azure-search-index.png)

2. Voer tekst in, zoals 'strand' (of de tekst die wordt geleverd aan waarmee u rekening moet) en klik op het zoekpictogram. U moet enkele resultaten krijgt.

     ![Zoeken naar "strand"](./media/tutorial-csharp-create-first-app/azure-search-beach.png)

3. Voer 'vijfsterren'. Houd er rekening mee hoe je geen resultaten krijgt. Een meer geavanceerde zoekopdracht zou 'vijfsterren' behandelen als een synoniem voor 'luxe' en de resultaten worden geretourneerd. Het gebruik van synoniemen is beschikbaar in Azure Search, maar we niet worden die betrekking op het in de eerste zelfstudies hebben zullen.
 
4. Voer 'hot' Als zoektekst. Dit gebeurt _niet_ items met het woord "Hotels" in deze retourneren. Onze zoeken is alleen zoeken naar volledige woorden, hoewel enkele resultaten worden geretourneerd.

5. Probeer andere woorden: 'groep', "Zonneschijn", "weergeven" en wat. Hier ziet u Azure Search werkt de eenvoudigste, maar nog steeds ervan te overtuigen dat niveau.

## <a name="test-edge-conditions-and-errors"></a>Situaties waarbij een rand en fouten

Het is belangrijk om te controleren dat onze functies foutafhandeling werkt zoals het hoort, zelfs wanneer dingen perfect werkt. 

1. In de **Index** methode na de **probeer {** aanroepen, voert u de regel **Throw nieuwe Exception()** . Deze uitzondering forceert een foutmelding wanneer we op tekst zoeken.

2. De app uitvoeren, geeft 'bar' als tekst zoeken en klik op het zoekpictogram. De uitzondering moet resulteren in de weergave van de fout.

     ![Afdwingen dat een fout](./media/tutorial-csharp-create-first-app/azure-search-error.png)

    > [!Important]
    > Het wordt beschouwd als een beveiligingsrisico voor interne fout getallen in de foutpagina's retourneren. Als uw app is bedoeld voor algemeen gebruik, doet u bepaalde onderzoek naar veilige en aanbevolen procedures van wat u wilt retourneren wanneer er een fout optreedt.

3. Verwijder **Throw nieuwe Exception()** wanneer u tevreden bent de foutafhandeling werkt behoren.

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Een Azure Search-aanroep is beknopt en eenvoudig met het interpreteren van de resultaten.
* Asynchrone aanroepen een kleine hoeveelheid complexiteit toevoegen aan de domeincontroller, maar zijn de aanbevolen procedure als u van plan bent om kwaliteitsapps te ontwikkelen.
* Deze app uitgevoerd in een eenvoudige tekst te zoeken, gedefinieerd door de wat is ingesteld in **searchParameters**. Deze een klasse kan echter worden gevuld met veel leden die verfijning aan een zoekopdracht toevoegen. Niet veel extra werk is nodig om deze app aanzienlijk krachtiger maken.

## <a name="next-steps"></a>Volgende stappen

Als u wilt bieden de beste gebruikerservaring met behulp van Azure Search, moet u meer functies toevoegen met name voor het wisselbestand (ofwel met behulp van pagina getallen of oneindige schuiven) en automatisch aanvullen/suggesties. We moet ook rekening houden met meer geavanceerde zoekparameters (bijvoorbeeld geografische zoekopdrachten hotels binnen een opgegeven radius van een bepaald moment en zoekresultaten volgorde).

Wij hebben oog voor volgende stappen in een reeks zelfstudies. Laten we beginnen met het wisselbestand.

> [!div class="nextstepaction"]
> [C#Zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md)


