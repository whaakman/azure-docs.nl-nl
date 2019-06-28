---
title: C#zelfstudie over het bestellen van resultaten - Azure Search
description: Deze zelfstudie bouwt voort op het project "Paginering - Azure Search zoekresultaten", om toe te voegen de volgorde van de lijst met zoekresultaten. Meer informatie over het bestellen van resultaten op een primaire eigenschap en voor de resultaten die de dezelfde primaire eigenschap, het bestellen van resultaten op een secundaire eigenschap. Tot slot leert u hoe u resultaten op basis van een scoringprofiel.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/21/2019
ms.openlocfilehash: 32e253b4e131d753ab6937d0aa2a49bda471e091
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466566"
---
# <a name="c-tutorial-order-the-results---azure-search"></a>C#zelfstudie: Volgorde van de resultaten - Azure Search

Accountpagina dit punt in onze serie zelfstudies, worden de resultaten geretourneerd en in de standaardvolgorde van een weergegeven. Dit is de volgorde waarin de gegevens zich bevinden, of misschien een standaard _scoringprofiel_ is gedefinieerd, die wordt gebruikt wanneer er geen bestellen parameters zijn opgegeven. In deze zelfstudie gaan we in hoe u kunt de resultaten op basis van een eigenschap primair is, en vervolgens voor de resultaten die de dezelfde primaire eigenschap, het bestellen van die selectie op een secundaire eigenschap. Als alternatief voor het ordenen op basis van numerieke waarden in het laatste voorbeeld laat zien hoe rangschikken op basis van een aangepaste scoringprofiel. We zullen ook verdiep u iets in de weergave van _complexe typen_.

Als u wilt vergelijken eenvoudig geretourneerde resultaten, dit project bouwt voort op het oneindig schuifbalkgebied project gemaakt de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) zelfstudie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Volgorde resultaten op basis van een eigenschap
> * Volgorde resultaten op basis van meerdere eigenschappen
> * Filteren van resultaten op basis van een afstand van een geografische punt
> * Volgorde-resultaten op basis van een scoringprofiel

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

De oneindig schuifbalkgebied versie van de [ C# zelfstudie: Search-resultaten paginering - Azure Search](tutorial-csharp-paging.md) project ingesteld en geactiveerd. Dit project kunt uw eigen versie of installeer deze via GitHub: [Eerste app maken](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="order-results-based-on-one-property"></a>Volgorde resultaten op basis van een eigenschap

Wanneer we bestellen resultaten op basis van één eigenschap, bijvoorbeeld hotel classificatie, willen we niet alleen de geordende resultaten, we willen dat ook bevestigen dat de volgorde juist is. Als we order niet op classificatie uitvoeren, moeten we de classificatie met andere woorden, weergegeven in de weergave.

In deze zelfstudie, zullen we ook toevoegen iets meer aan de weergave van resultaten, de goedkoopste ruimte-frequentie en de frequentie van ruimte duurste, voor elke hotel. Als we dieper ingaan op volgorde, we ook waarden om ervoor te zorgen wat we op volgorde wilt toevoegen, wordt ook weergegeven in de weergave.

Er is niet nodig om te wijzigen van de modellen te ordenen inschakelen. De weergave en controller nodig bijgewerkt. Open de startpagina controller eerst.

### <a name="add-the-orderby-property-to-the-search-parameters"></a>De eigenschap OrderBy toevoegen aan de zoekparameters

1. Alle die nodig is voor de order-resultaten op basis van een enkele numerieke eigenschap, is het instellen van de **OrderBy** parameter de naam van de eigenschap. In de **Index (SearchData model)** methode, voeg de volgende regel toe aan de zoekparameters.

    ```cs
        OrderBy = new[] { "Rating desc" },
    ```

    >[!Note]
    > De standaard wordt oplopend, maar u kunt toevoegen **asc** met de eigenschap om dit te verduidelijken. Aflopende volgorde is opgegeven door toe te voegen **desc**.

2. Nu de app uitvoeren en geef een algemene zoekterm. De resultaten kunnen of mogelijk niet in de juiste volgorde, noch als de ontwikkelaar, niet de gebruiker beschikt over een eenvoudige manier om de resultaten te controleren.

3. Laten we deze duidelijk de resultaten worden gesorteerd op classificatie. Vervang eerst de **Vak1** en **Vak2** klassen in het bestand hotels.css met de volgende klassen (deze klassen zijn de nieuwe licenties we nodig hebben voor deze zelfstudie).

    ```html
    textarea.box1A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box1B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box2A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box2B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box3 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }
    ```

    >[!Tip]
    >Browsers meestal CSS-bestanden in de cache en dit kan leiden tot een oude CSS-bestand dat wordt gebruikt en uw bewerkingen genegeerd. Een goede manier ronde dit is het toevoegen van een querytekenreeks met een versieparameter op de koppeling. Bijvoorbeeld:
    >
    >```html
    >   <link rel="stylesheet" href="~/css/hotels.css?v1.1" />
    >```
    >
    >Het versienummer bijwerken als u denkt dat een oude CSS-bestand wordt gebruikt door uw browser.

4. Toevoegen de **waardering** eigenschap in op de **Selecteer** parameter in de **Index (SearchData model)** methode.

    ```cs
    Select = new[] { "HotelName", "Description", "Rating"},
    ```

5. Open de weergave (index.cshtml) en vervang de rendering-lus ( **&lt;!--tonen de hotel.--&gt;** ) met de volgende code.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

6. De classificatie moet beschikbaar zowel in de eerste pagina en in de volgende pagina's die worden aangeroepen via de oneindige schuiven. Voor de laatste van deze twee situaties, moeten we beide bijwerken de **volgende** actie in de controller en de **verschoven** functie in de weergave. Beginnen met de domeincontroller, wijzigt de **volgende** methode naar de volgende code. Deze code maakt en de classificatie-tekst communiceert.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";

                // Add three strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

7. Nu bijwerken de **verschoven** functie in de weergave om de tekst van de beoordeling weer te geven.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 3) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 2] + '</textarea>';
                            }
                        });
                    }
                }
            </script>

    ```

8. Voer nu de app opnieuw uit. Zoeken naar een algemene term, zoals 'Wi-Fi', en controleer of dat de resultaten worden gesorteerd op aflopende volgorde van hotel classificatie.

    ![Ordenen op basis van classificatie](./media/tutorial-csharp-create-first-app/azure-search-orders-rating.png)

    U ziet dat verschillende hotels een identieke classificatie, en dus hun weergave in de weergave opnieuw de volgorde waarin de gegevens worden gevonden is, die willekeurig is.

    Voordat we naar een tweede niveau van de ordening kunt toevoegen, nu code toevoegen om het bereik van de tarieven van de ruimte weer te geven. We zijn deze code toevoegen aan beide show extraheren van gegevens uit een _complexe type_, en ook zodat we bespreken op basis van de prijs bestellen resultaten (goedkoopste eerste misschien).

### <a name="add-the-range-of-room-rates-to-the-view"></a>Het bereik van de tarieven van de ruimte aan de weergave toevoegen

1. Eigenschappen met de snelheid van de goedkoopste en meest kostbare ruimte aan het model Hotel.cs toevoegen.

    ```cs
        // Room rate range
        public double cheapest { get; set; }
        public double expensive { get; set; }
    ```

2. Berekenen van de tarieven van de ruimte aan het einde van de **Index (SearchData model)** actie in de home-controller. De berekeningen toe te voegen na de opslag van tijdelijke gegevens.

    ```cs
                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    // Calculate room rates.
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
    ```

3. Toevoegen de **ruimten** eigenschap in op de **Selecteer** parameter in de **Index (SearchData model)** actiemethode van de controller.

    ```cs
     Select = new[] { "HotelName", "Description", "Rating", "Rooms" },
    ```

4. Wijzig de lus rendering in de weergave om het bereik van het tarief voor de eerste pagina van de resultaten weer te geven.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

5. Wijzig de **volgende** methode in de home-controller om te communiceren van het bereik van de snelheid, voor de volgende pagina's van resultaten.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

6. Update de **verschoven** functie in de weergave voor het afhandelen van de kamer classificeert tekst.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 4) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

7. De app uitvoeren en controleer of dat de bereiken van de snelheid ruimte worden weergegeven.

    ![Ruimte tarief bereiken weergeven](./media/tutorial-csharp-create-first-app/azure-search-orders-rooms.png)

De **OrderBy** eigenschap van de zoekparameters accepteert een vermelding zoals **Rooms.BaseRate** voor de goedkoopste ruimte snelheid, zelfs als de ruimten zijn al gesorteerd op basis van tarief (die ze niet zijn). Om hotels in de verzameling voorbeeldgegevens, gesorteerd op de snelheid van ruimte, weer te geven moet u de resultaten in de oorspronkelijke controller sorteren en deze resultaten verzenden naar de weergave in de gewenste volgorde.

## <a name="order-results-based-on-multiple-values"></a>Volgorde resultaten op basis van meerdere waarden

Nu de vraag is hoe u onderscheid maken tussen hotels met de dezelfde classificatie. Een goede manier zou om op basis van de laatste keer dat het hotel is renovated zijn. Met andere woorden, hoe meer recent het hotel is renovated, hoe hoger het hotel wordt weergegeven in de resultaten.

1. Als u wilt een tweede niveau voor het ordenen van toevoegen, wijzigen de **OrderBy** en **Selecteer** eigenschappen in de **Index (SearchData model)** methode om op te nemen de  **LastRenovationDate** eigenschap.

    ```cs
    OrderBy = new[] { "Rating desc", "LastRenovationDate desc" },
    Select = new[] { "HotelName", "Description", "Rating", "Rooms", "LastRenovationDate" },
    ```

    >[!Tip]
    >Een willekeurig aantal eigenschappen kan worden ingevoerd op de **OrderBy** lijst. Als hotels had de dezelfde classificatie en renovatie datum, kan de eigenschap van een derde onderscheid maken tussen deze worden ingevoerd.

2. Nogmaals, moeten we zien de renovatie datum in de weergave alleen dat u er zeker van te zijn dat de volgorde juist is. Voor dergelijke een ding als een renovatie, zijn waarschijnlijk slechts het jaar is vereist. Wijzig de lus rendering in de weergave in de volgende code.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

3. Wijzig de **volgende** methode in de home-controller om door te sturen het jaargedeelte van de datum van laatste renovatie.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";
                var lastRenovatedText = $"Last renovated: {model.resultList.Results[n].Document.LastRenovationDate.Value.Year}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(lastRenovatedText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Wijzig de **verschoven** functie in de weergave om de renovatie tekst weer te geven.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 5) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

5. De app uitvoeren. Zoeken op een algemene term, zoals "groep" of "view", en controleer of hotels met de dezelfde classificatie worden nu weergegeven in aflopende volgorde van renovatie datum.

    ![Sorteren op datum renovatie](./media/tutorial-csharp-create-first-app/azure-search-orders-renovation.png)

## <a name="filter-results-based-on-a-distance-from-a-geographical-point"></a>Filteren van resultaten op basis van een afstand van een geografische punt

Classificatie en renovatie datum, zijn voorbeelden van eigenschappen die het beste in aflopende volgorde worden weergegeven. Een alfabetische lijst is een voorbeeld van een goed gebruik van oplopende volgorde (bijvoorbeeld, als er slechts een is **OrderBy** en deze eigenschap is ingesteld op **HotelName** en vervolgens een alfabetische volgorde zou worden weergegeven. ). Echter voor onze voorbeeldgegevens zou afstand van een geografische dat zijn meer.

Als u wilt weergeven van resultaten op basis van geografische afstand, zijn de verschillende stappen vereist.

1. Filter alle hotels die zich buiten een opgegeven radius van de willekeurig moment door te voeren van een filter met lengtegraad, breedtegraad en RADIUS-parameters. Lengtegraad wordt eerst aan de functie voor het gegeven. RADIUS is in kilometer zijn verwijderd.

    ```cs
        // "Location" must match the field name in the Hotel class.
        // Distance (the radius) is in kilometers.
        // Point order is Longitude then Latitude.
        Filter = $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') le {model.radius}",
    ```

2. Het bovenstaande filter heeft _niet_ volgorde van de resultaten op basis van op afstand, alleen de uitschieters verwijderd. Als u wilt de resultaten bestellen, voer een **OrderBy** instellen die Hiermee geeft u de methode geoDistance.

    ```cs
    OrderBy = new[] { $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') asc" },
    ```

3. Hoewel de resultaten zijn geretourneerd door Azure Search met behulp van een filter afstand, is het de berekende afstand tussen de gegevens en het opgegeven punt _niet_ geretourneerd. Deze waarde in de weergave of de controller opnieuw als u wilt weergeven in de resultaten.

    De volgende code wordt de afstand tussen de twee lat/ion punten berekend.

    ```cs
        const double EarthRadius = 6371;

        public static double Degrees2Radians(double deg)
        {
            return deg * Math.PI / 180;
        }

        public static double DistanceInKm( double lat1,  double lon1, double lat2, double lon2)
        {
            double dlon = Degrees2Radians(lon2 - lon1);
            double dlat = Degrees2Radians(lat2 - lat1);

            double a = (Math.Sin(dlat / 2) * Math.Sin(dlat / 2)) + Math.Cos(Degrees2Radians(lat1)) * Math.Cos(Degrees2Radians(lat2)) * (Math.Sin(dlon / 2) * Math.Sin(dlon / 2));
            double angle = 2 * Math.Atan2(Math.Sqrt(a), Math.Sqrt(1 - a));
            return angle * EarthRadius;
        }
    ```

4. U hebt nu deze concepten met elkaar verbinden. Deze fragmenten zijn echter zo ver onze zelfstudie gaat, het bouwen van dat een kaart-app bij wijze van oefening wordt gegeven voor de lezer. Als u dit voorbeeld verder, houd rekening met het invoeren van de naam van een plaats met een radius of zoeken naar een punt op een kaart en selecteren van een radius. Zie de volgende bronnen voor het onderzoeken van deze opties verder:

* [Azure Maps-documentatie](https://docs.microsoft.com/azure/azure-maps/)
* [Zoeken naar een adres met behulp van de search-service van Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address)

## <a name="order-results-based-on-a-scoring-profile"></a>Volgorde-resultaten op basis van een scoringprofiel

De voorbeelden die tot nu toe in de zelfstudie wordt getoond hoe rangschikken op basis van numerieke waarden (classificatie, renovatie datum, de geografische afstand), biedt een _exacte_ proces voor het ordenen van. Echter lenen enkele zoekopdrachten en enkele gegevens niet voor een eenvoudige vergelijking tussen de twee elementen. Azure Search bevat het concept van _scoren_. _Scoreprofielen_ kan worden opgegeven voor een set gegevens die kan worden gebruikt voor het leveren van meer complexe en kwalitatief vergelijkingen, die moeten worden het nuttigst wanneer, bijvoorbeeld eerst vergelijken van gegevens op basis van tekst om te bepalen die moet worden weergegeven.

Scoreprofielen zijn niet gedefinieerd door gebruikers, maar doorgaans door beheerders van een gegevensset. Verschillende scoreprofielen zijn ingesteld op de gegevens hotels. Laten we kijken hoe een scoringprofiel wordt gedefinieerd en probeer het schrijven van code om op te zoeken.

### <a name="how-scoring-profiles-are-defined"></a>Hoe scoring-profielen zijn gedefinieerd

Laten we kijken naar drie voorbeelden van scoreprofielen en houd rekening met hoe de _moet_ van invloed op de volgorde van de resultaten. Als een app-ontwikkelaar, u deze profielen geen schrijven, ze worden geschreven door de Gegevensbeheerder van de, echter is het handig om te kijken naar de syntaxis.

1. Dit is de standaardinstelling scoringprofiel voor de gegevensset hotels gebruikt wanneer u geen opgeeft **OrderBy** of **ScoringProfile** parameter. Dit profiel verhoogt de _score_ voor een hotel als de zoektekst in de hotelnaam, beschrijving of lijst met labels (faciliteiten) aanwezig is. U ziet hoe het gewicht van de beoordelingsresultaten voorkeur voor bepaalde velden. Als de zoektekst wordt weergegeven in een ander veld niet wordt vermeld, is er een gewicht van 1. Natuurlijk, hoe hoger de score eerder een resultaat wordt weergegeven in de weergave.

     ```cs
    {
            "name": "boostByField",
            "text": {
                "weights": {
                    "HotelName": 2,
                    "Description": 1.5,
                    "Description_fr": 1.5,
                    "Tags": 3
                }
            }
        }

    ```

2. De volgende scoringprofiel verhoogt de score aanzienlijk, als een opgegeven parameter bevat een of meer van de lijst met labels (die we bellen 'faciliteiten'). Het belangrijkste punt van dit profiel is die een parameter _moet_ worden opgegeven, die de tekst bevat. Als de parameter leeg is of niet is opgegeven, is een fout wordt gegenereerd.
 
    ```cs
            {
            "name": "boostAmenities",
            "functions": [
                {
                    "type": "tag",
                    "fieldName": "Tags",
                    "boost": 5,
                    "tag": {
                        "tagsParameter": "amenities"
                    }
                }
            ]
        }
    ```

3. In dit derde voorbeeld geeft de classificatie een aanzienlijke verbetering tot de score. De laatste renovated datum wordt ook de score, maar alleen als die gegevens binnen 730 dagen (2 jaar valt) verhogen van de huidige datum.

    ```cs
            {
            "name": "renovatedAndHighlyRated",
            "functions": [
                {
                    "type": "magnitude",
                    "fieldName": "Rating",
                    "boost": 20,
                    "interpolation": "linear",
                    "magnitude": {
                        "boostingRangeStart": 0,
                        "boostingRangeEnd": 5,
                        "constantBoostBeyondRange": false
                    }
                },
                {
                    "type": "freshness",
                    "fieldName": "LastRenovationDate",
                    "boost": 10,
                    "interpolation": "quadratic",
                    "freshness": {
                        "boostingDuration": "P730D"
                    }
                }
            ]
        }

    ```

    Nu, laat het ons Kijk als deze profielen als we denken dat zou moeten werken.

### <a name="add-code-to-the-view-to-compare-profiles"></a>Voeg code toe aan de weergave om te vergelijken van profielen

1. Open het bestand index.cshtml en vervang de &lt;hoofdtekst&gt; sectie met de volgende code.

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
                        Hotels Search - Order Results
                    </h1>
                </td>
            </tr>
            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it. -->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
                    </div>

                    <div class="searchBoxForm">
                        <b>&nbsp;Order:&nbsp;</b>
                        @Html.RadioButtonFor(m => m.scoring, "Default") Default&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "RatingRenovation") By numerical Rating&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "boostAmenities") By Amenities&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "renovatedAndHighlyRated") By Renovated date/Rating profile&nbsp;&nbsp;
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.facetText != null)
                        {
                            <h5 class="facetheader">Amenities:</h5>
                            <ul class="facetlist">
                                @for (var c = 0; c < Model.facetText.Length; c++)
                                {
                                    <li> @Html.CheckBoxFor(m => m.facetOn[c], new { @id = "check" + c.ToString() }) @Model.facetText[c] </li>
                                }

                            </ul>
                        }
                    </div>
                </td>
                <td>
                    @if (Model != null && Model.resultList != null)
                    {
                        // Show the total result count.
                        <p class="sampleText">
                            @Html.DisplayFor(m => m.resultList.Count) Results <br />
                        </p>

                        <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                            <!-- Show the hotel data. -->
                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                                var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                                var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);
                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel details.
                                @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                                @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                                @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
                                @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
                            }
                        </div>

                        <script>
                            function scrolled() {
                                if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                                    $.getJSON("/Home/Next", function (data) {
                                        var div = document.getElementById('myDiv');

                                        // Append the returned data to the current list of hotels.
                                        for (var i = 0; i < data.length; i += 5) {
                                            div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                            div.innerHTML += '<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                            div.innerHTML += '<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                                        }
                                    });
                                }
                            }
                        </script>
                    }
                </td>
            </tr>
        </table>
    }
    </body>
    ```

2. Open het bestand SearchData.cs en vervang de **SearchData** klasse met de volgende code.

    ```cs
    public class SearchData
    {
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

        // Array to hold the text for each amenity.
        public string[] facetText { get; set; }

        // Array to hold the setting for each amenitity.
        public bool[] facetOn { get; set; }

        // The text to search for.
        public string searchText { get; set; }

        // Record if the next page is requested.
        public string paging { get; set; }

        // The list of results.
        public DocumentSearchResult<Hotel> resultList;

        public string scoring { get; set; }       
    }
    ```

3. Open het bestand hotels.css, en voeg de volgende HTML-klassen toe.

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

### <a name="add-code-to-the-controller-to-specify-a-scoring-profile"></a>Voeg code toe aan de domeincontroller een scoringprofiel opgeven

1. Open het bestand home-controller. Voeg de volgende **met behulp van** instructie (om u te helpen met het maken van een lijst met).

    ```cs
    using System.Linq;
    ```

2.  In dit voorbeeld moet de eerste aanroep **Index** naar iets meer dan alleen de eerste weergave kan retourneren. De methode is nu zoekt van maximaal 20 faciliteiten om weer te geven in de weergave.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search for up to 20 amenities.
                Facets = new List<string> { "Tags,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to a list that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(facets);

            // Save the facet text for the next view.
            SaveFacets(model, false);

            // Render the view including the facets.
            return View(model);
        }
    ```

3. Moeten we twee particuliere methoden voor de facetten opslaan naar de tijdelijke opslag, en voor ze kunt herstellen vanuit de tijdelijke opslag en vullen van een model.

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

4. We moeten de **OrderBy** en **ScoringProfile** parameters naar behoefte. Vervang de bestaande **Index (SearchData model)** methode met de volgende stappen.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Recover the facet text, and the facet check box settings.
                    RecoverFacets(model, true);

                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First search with text. 
                    // Recover the facet text, but ignore the check box settings, and use the current model settings.
                    RecoverFacets(model,false);

                    // First call. Check for valid text input, and valid scoring profile.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    if (model.scoring == null)
                    {
                        model.scoring = "Default";
                    }
                    page = 0;
                }

                // Set empty defaults for ordering and scoring parameters.
                var orderby = new List<string>();
                string profile = "";
                var scoringParams = new List<ScoringParameter>();

                // Set the ordering based on the user's radio button selection.
                switch (model.scoring)
                {
                    case "RatingRenovation":
                        orderby.Add("Rating desc");
                        orderby.Add("LastRenovationDate desc");
                        break;

                    case "boostAmenities":
                        {
                            profile = model.scoring;
                            var setAmenities = new List<string>();

                            // Create a string list of amenities that have been clicked.
                            for (int a = 0; a < model.facetOn.Length; a++)
                            {
                                if (model.facetOn[a])
                                {
                                    setAmenities.Add(model.facetText[a]);
                                }
                            }
                            if (setAmenities.Count > 0)
                            {
                                // Only set scoring parameters if there are any.
                                var sp = new ScoringParameter("amenities", setAmenities);
                                scoringParams.Add(sp);
                            }
                            else
                            {
                                // No amenities selected, so set profile back to default.
                                profile = "";
                            }
                        }
                        break;

                    case "renovatedAndHighlyRated":
                        profile = model.scoring;
                        break;

                    default:
                        break;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Set the ordering/scoring parameters.
                    OrderBy = orderby,
                    ScoringProfile = profile,
                    ScoringParameters = scoringParams,

                    // Select the data properties to be returned.
                    Select = new[] { "HotelName", "Description", "Tags", "Rooms", "Rating", "LastRenovationDate" },
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
                TempData["scoring"] = model.scoring;
                SaveFacets(model,true);

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Lees de opmerkingen voor elk van de **overschakelen** selecties.

5. We hoeft niet te wijzigen naar de **volgende** actie, als u de aanvullende code voor de vorige sectie op volgorde op basis van meerdere eigenschappen voltooid.

### <a name="run-and-test-the-app"></a>Uitvoeren en testen van de app.

1. De app uitvoeren. Hier ziet u een volledige set voorzieningen in de weergave.

2. Voor het bestellen geeft selecteren 'door een numerieke score' u de numerieke volgorde die u al in deze zelfstudie hebt geïmplementeerd met renovatie datum hotels van gelijk classificatie kiezen.

![Bestellen 'strand"op basis van classificatie](./media/tutorial-csharp-create-first-app/azure-search-orders-beach.png)

3. Probeer nu het profiel 'door faciliteiten'. Verschillende selecties van faciliteiten en controleer of dat hotels met deze voorzieningen wordt gevraagd om de lijst met resultaten.

![Bestellen 'strand"op basis van profiel](./media/tutorial-csharp-create-first-app/azure-search-orders-beach-profile.png)

4. Probeer de 'Door Renovated datum/classificatie profiel' om te zien of u krijgt van wat u verwacht. Alleen onlangs renovated hotels deze krijgt een _webdocumenten_ boost.

### <a name="resources"></a>Resources

Zie voor meer informatie de volgende [scoreprofielen toevoegen aan een Azure Search-index](https://docs.microsoft.com/azure/search/index-add-scoring-profiles).

## <a name="takeaways"></a>Opgedane kennis

Houd rekening met de volgende takeaways van dit project:

* Gebruikers wordt verwacht dat de zoekresultaten worden besteld, meest relevante eerst.
* Gegevens moet gestructureerd zodat bestellen is eenvoudig. We konden niet sorteren op 'goedkoopste' eerst eenvoudig, omdat de gegevens is niet correct gestructureerd als u wilt inschakelen om te worden uitgevoerd zonder extra code te bestellen.
* Er zijn te bestellen, onderscheid maken tussen de resultaten die dezelfde waarde op een hoger niveau voor het ordenen van hebben veel niveaus.
* Is het vanzelfsprekend bepaalde resultaten worden gesorteerd in oplopende volgorde (bijvoorbeeld afstand weg van een punt), en sommige in aflopende volgorde (bijvoorbeeld Gast beoordeling).
* Scoreprofielen kan worden gedefinieerd als numerieke vergelijkingen niet beschikbaar zijn of niet genoeg voor een gegevensset Smart. Elk resultaat scoren help te bestellen en op intelligente wijze de resultaten weergeven.

## <a name="next-steps"></a>Volgende stappen

U hebt deze reeks C# zelfstudies - u moet hebben opgedaan waardevolle kennis van de Azure Search-API's.

Voor verdere verwijzing en zelfstudies, kunt u overwegen Bladeren [Microsoft Learn](https://docs.microsoft.com/learn/browse/?products=azure), of de andere zelfstudies in de [documentatie voor Azure Search](https://docs.microsoft.com/azure/search/).
