---
title: Werken met de beheerde App Service Mobile Apps-clientbibliotheek (Windows | Microsoft Docs
description: Informatie over het gebruik van een .NET-client voor Azure App Service Mobile Apps met Windows en Xamarin-apps.
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2017
ms.author: crdun
ms.openlocfilehash: c80265432f4ee3120e3125b45712dc0e7a434708
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>De beheerde client gebruiken voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de beheerde client-bibliotheek voor Azure App Service Mobile Apps voor Windows en Xamarin-apps uitvoeren. Als u niet bekend met Mobile Apps bent, moet u eerst voltooien van de [Azure Mobile Apps Quick Start] [ 1] zelfstudie. In deze handleiding richten we op de beheerde client-side '-SDK. Zie voor meer informatie over de serverzijde SDK's voor mobiele Apps, de documentatie voor de [.NET Server SDK] [ 2] of de [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Referentiedocumentatie
De documentatie bij de client-SDK bevindt zich hier: [referentie voor Azure Mobile Apps .NET client][4].
Ook vindt u enkele voorbeelden van client in de [Azure-Samples GitHub-opslagplaats][5].

## <a name="supported-platforms"></a>Ondersteunde Platforms
De .NET-Platform ondersteunt de volgende platforms:

* Xamarin Android-versies voor API-19 tot en met 24 (KitKat via deze)
* Xamarin iOS-versies voor iOS 8.0 en hoger
* Universele Windows-Platform
* Windows Phone 8.1
* Windows Phone 8.0, met uitzondering van Silverlight-toepassingen

De verificatie 'server-flow' maakt gebruik van een webweergave voor de gebruikersinterface weergegeven.  Als het apparaat niet tot een WebView UI aanwezig is, worden andere methoden voor verificatie nodig.  Deze SDK is derhalve niet geschikt voor controle-type of op dezelfde manier beperkte apparaten.

## <a name="setup"></a>Het installatieprogramma en vereisten
Er wordt ervan uitgegaan dat u hebt al gemaakt en gepubliceerd van uw mobiele App back-end-project, waaronder ten minste één tabel.  In de code die wordt gebruikt in dit onderwerp wordt de tabel met de naam `TodoItem` en heeft de volgende kolommen: `Id`, `Text`, en `Complete`. Deze tabel is dezelfde tabel gemaakt wanneer u voltooit de [Azure Mobile Apps Quick Start][1].

Het bijbehorende client-side '-type getypte in C# is de volgende klasse:

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

De [JsonPropertyAttribute] [ 6] wordt gebruikt voor het definiëren van de *PropertyName* toewijzing tussen de velden van de client en de tabel.

Zie voor meer informatie over het maken van tabellen in uw back-end van Mobile Apps, de [onderwerp SDK voor .NET-Server] [ 7] of de [Node.js Server SDK onderwerp][8]. Als u uw back-end voor de mobiele App in de Azure-portal met behulp van de Quick Start hebt gemaakt, kunt u ook gebruiken de **gemakkelijk tabellen** instellen in de [Azure-portal].

### <a name="how-to-install-the-managed-client-sdk-package"></a>Procedure: de beheerde client SDK-pakket installeren
Gebruik een van de volgende methoden voor het installeren van de beheerde client SDK-pakket voor mobiele Apps van [NuGet][9]:

* **Visual Studio** met de rechtermuisknop op uw project, klikt u op **NuGet-pakketten beheren**, zoekt de `Microsoft.Azure.Mobile.Client` van het pakket en klik vervolgens op **installeren**.
* **Xamarin Studio** met de rechtermuisknop op uw project, klikt u op **toevoegen** > **NuGet-pakketten toevoegen**, zoekt de `Microsoft.Azure.Mobile.Client `van het pakket en klik vervolgens op **Add Package**.

In het bestand hoofdactiviteit onthouden om toe te voegen van de volgende **met** instructie:

```
using Microsoft.WindowsAzure.MobileServices;
```

### <a name="symbolsource"></a>Procedure: werken met symbolen in Visual Studio
De symbolen voor de naamruimte Microsoft.Azure.Mobile zijn beschikbaar op [SymbolSource][10].  Raadpleeg de [SymbolSource instructies] [ 11] SymbolSource integreren met Visual Studio.

## <a name="create-client"></a>De Mobile Apps-client maken
De volgende code maakt de [MobileServiceClient] [ 12] object dat wordt gebruikt voor toegang tot uw mobiele App back-end.

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Vervang in de bovenstaande code `MOBILE_APP_URL` met de URL van de back-end voor de mobiele App, die is gevonden in de blade voor uw mobiele App back-end in de [Azure-portal]. Het object MobileServiceClient moet een singleton.

## <a name="work-with-tables"></a>Werken met tabellen
De volgende sectie wordt het zoeken en records ophalen en de gegevens in de tabel wijzigen.  De volgende onderwerpen komen aan bod:

* [De tabelverwijzing van een maken](#instantiating)
* [Opvragen van gegevens](#querying)
* [Geretourneerde gegevens filteren](#filtering)
* [Geretourneerde gegevens sorteren](#sorting)
* [Gegevens weergeven op pagina 's](#paging)
* [Selecteer specifieke kolommen](#selecting)
* [Een record door-Id niet opzoeken](#lookingup)
* [Omgaan met niet-getypeerde query 's](#untypedqueries)
* [Gegevens invoegen](#inserting)
* [Bijwerken van gegevens](#updating)
* [Verwijderen van gegevens](#deleting)
* [Conflictoplossing en optimistische gelijktijdigheid](#optimisticconcurrency)
* [Het binden aan een Windows-gebruikersinterface](#binding)
* [De grootte van de pagina wijzigen](#pagesize)

### <a name="instantiating"></a>Procedure: een tabelverwijzing maken
De code die toegang heeft tot of gegevens wijzigt in een tabel van de back-end-functies aanroepen op de `MobileServiceTable` object. Een verwijzing naar de tabel verkrijgen door het aanroepen van de [GetTable] methode als volgt:

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Het geretourneerde object maakt gebruik van het type serialisatie-model. Een model met niet-getypeerde serialisatie wordt ook ondersteund. Het volgende voorbeeld [wordt een verwijzing naar een niet-getypeerde tabel]:

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Niet-getypeerde query's, moet u de onderliggende OData-query-tekenreeks opgeven.

### <a name="querying"></a>Procedure: een Query over gegevens van uw mobiele App
Deze sectie wordt beschreven hoe het uitgeven van query's voor de backend voor mobiele Apps, waaronder de volgende functionaliteit:

* [Geretourneerde gegevens filteren](#filtering)
* [Geretourneerde gegevens sorteren](#sorting)
* [Gegevens weergeven op pagina 's](#paging)
* [Selecteer specifieke kolommen](#selecting)
* [Opzoeken van gegevens door-ID](#lookingup)

> [!NOTE]
> Een server aangestuurde paginaformaat wordt afgedwongen om te voorkomen dat alle rijen worden geretourneerd.  Paginering voorkomt standaard aanvragen voor grote gegevenssets die een negatieve invloed hebben op de service.  U kunt meer dan 50 rijen gebruiken de `Skip` en `Take` methode, zoals beschreven in [gegevens retourneren op pagina's](#paging).

### <a name="filtering"></a>Hoe: Filter gegevens geretourneerd
De volgende code ziet u hoe u voor het filteren van gegevens door een `Where` -component in een query. Deze retourneert alle items uit `todoTable` waarvan `Complete` eigenschap is gelijk aan `false`. De [waar] functie is van toepassing een rij voor het filteren van predicaat voor de query op de tabel.

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

U kunt de URI van de aanvraag verzonden naar de back-end via bericht inspectie software, zoals browser ontwikkelhulpprogramma's weergeven of [Fiddler]. Als u de aanvraag-URI bekijkt, ziet u dat de queryreeks is gewijzigd:

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Deze OData-aanvraag wordt omgezet in een SQL-query door de SDK-Server:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

De functie die wordt doorgegeven aan de `Where` methode een willekeurig aantal voorwaarden kan hebben.

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

In dit voorbeeld zou worden vertaald naar een SQL-query door de SDK-Server:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Deze query kan ook worden gesplitst in meerdere componenten:

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

De twee methoden zijn equivalent en door elkaar worden gebruikt.  De vorige optie&mdash;van meerdere predicaten in één query cookievalidatie&mdash;is compact en aanbevolen.

De `Where` component ondersteunt bewerkingen die worden vertaald naar de OData-subset. Bewerkingen zijn onder andere:

* Relationele operators (==,! =, <, < =, >, > =),
* Rekenkundige operatoren (+, -, /, *, %),
* Number precision (Math.Floor, Math.Ceiling)
* Tekenreeks-functies (lengte, subtekenreeks, vervangen, IndexOf, StartsWith, EndsWith)
* Datumeigenschappen (jaar, maand, dag, uur, minuut, seconde)
* Toegang tot de eigenschappen van een object, en
* Expressies combineren van deze bewerkingen.

Wanneer u overweegt wat de SDK-Server ondersteunt, kunt u overwegen de [OData v3 documentatie].

### <a name="sorting"></a>Hoe: sorteren gegevens geretourneerd
De volgende code laat zien hoe u kunt gegevens sorteren door op te nemen een [OrderBy] of [OrderByDescending] functie in de query. Deze items uit retourneert `todoTable` gesorteerd oplopende door de `Text` veld.

```
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="paging"></a>Procedure: gegevens retourneren op pagina's
Standaard worden in de back-end alleen de eerste 50 rijen geretourneerd. U kunt het aantal geretourneerde rijen verhogen door het aanroepen van de [nemen] methode. Gebruik `Take` samen met de [overslaan] methode voor het aanvragen van een specifieke 'pagina' van de totale gegevensset die door de query. De volgende query wanneer uitgevoerd, retourneert de bovenste drie items in de tabel.

```
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

De volgende herziene query wordt de eerste drie resultaten overgeslagen en de volgende drie resultaten geretourneerd. Deze query wordt de tweede 'pagina"gegevens, waarbij het paginaformaat drie items is geproduceerd.

```
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

De [IncludeTotalCount] methode aanvragen het totale aantal voor *alle* de records die zou zijn geretourneerd, eventuele opgegeven paginering/limiet-component wordt genegeerd:

```
query = query.IncludeTotalCount();
```

In werkelijkheid-app kunt u query's die vergelijkbaar is met het vorige voorbeeld met een pagineringsbesturingselement of een vergelijkbare UI navigeren tussen pagina's.

> [!NOTE]
> Als u wilt overschrijven 50 Rijlimiet in een back-end voor de mobiele App, moet u ook toepassen de [EnableQueryAttribute] openbare methode GET en geef het gedrag van het wisselbestand. Wanneer de methode wordt toegepast, wordt het volgende stelt het maximumaantal rijen geretourneerd en 1000:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Hoe: specifieke kolommen selecteren
U kunt opgeven welke set eigenschappen moeten worden opgenomen in de resultaten door toe te voegen een [Selecteer] component aan uw query. Bijvoorbeeld toont de volgende code hoe om slechts één veld te selecteren en ook hoe selecteren en formatteren van meerdere velden:

```
// Select one field -- just the Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

Alle functies die tot nu toe beschreven additief, zodat we kunt houden chaining ze. Elke aanroep van de keten is van invloed op meer van de query. Een voorbeeld van meer:

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Hoe: opzoeken van gegevens door-ID
De [LookupAsync] functie kan worden gebruikt voor het opzoeken van objecten uit de database met een bepaalde-ID.

```
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Hoe: niet-getypeerde query's uitvoeren
Bij het uitvoeren van een query met een niet-getypeerde tabelobject, moet u expliciet de OData-query-tekenreeks opgeven door het aanroepen van [ReadAsync], zoals in het volgende voorbeeld:

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

U terughalen JSON-waarden die u, zoals een eigenschappenverzameling gebruiken kunt. Zie voor meer informatie over JToken en Newtonsoft Json.NET de [Json.NET] site.

### <a name="inserting"></a>Procedure: gegevens invoegen in een back-end voor de mobiele App
Alle clienttypen moeten bevatten een lid genaamd **Id**, dit is standaard een tekenreeks. Dit **Id** CRUD-bewerkingen uit te voeren is vereist en voor het offline synchroniseren. De volgende code laat zien hoe u de [InsertAsync] methode voor het invoegen van nieuwe rijen in een tabel. De parameter bevat de gegevens moet worden ingevoegd als .NET-object.

```
await todoTable.InsertAsync(todoItem);
```

Als een unieke aangepaste ID-waarde niet is opgenomen in de `todoItem` tijdens een insert-, een GUID wordt gegenereerd door de server.
U kunt de gegenereerde Id ophalen door het object te bekijken nadat de oproep retourneert.

Niet-getypeerde om gegevens te voegen, kunt u profiteren van Json.NET:

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Hier volgt een voorbeeld van een e-mailadres als een unieke tekenreeks-id:

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Werken met de id-waarden
Mobiele Apps biedt ondersteuning voor tekenreeks voor unieke aangepaste waarden voor de tabel **id** kolom. Een string-waarde zorgt ervoor dat toepassingen als u aangepaste waarden zoals e-mailadressen of gebruikersnamen voor de ID.  Tekenreeks-id's bieden u de volgende voordelen:

* Id's worden zonder dat een retouren naar de database gegenereerd.
* Records zijn gemakkelijker te samenvoegen uit verschillende tabellen of databases.
* Id-waarden kunnen beter geïntegreerd met een toepassing logica.

Wanneer de waarde van een tekenreeks-ID niet is ingesteld op een ingevoegde record, de back-end voor de mobiele App wordt gegenereerd voor een unieke waarde voor de ID. U kunt de [Guid.NewGuid] methode voor het genereren van uw eigen id-waarden op de client of in de back-end.

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Procedure: gegevens wijzigen in een back-end voor de mobiele App
De volgende code laat zien hoe u de [UpdateAsync] methode voor het bijwerken van een bestaande record met dezelfde ID met nieuwe informatie. De parameter bevat de gegevens worden bijgewerkt als .NET-object.

```
await todoTable.UpdateAsync(todoItem);
```

Voor het bijwerken van niet-getypeerde gegevens te profiteren van [Json.NET] als volgt:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Een `id` veld moet worden opgegeven bij het maken van een update. Maakt gebruik van de back-end de `id` veld om te bepalen welke rij bijwerken. De `id` veld kan worden verkregen van het resultaat van de `InsertAsync` aanroepen. Een `ArgumentException` treedt op als u probeert een item bijwerken zonder dat de `id` waarde.

### <a name="deleting"></a>Hoe: verwijderen van gegevens in een back-end voor de mobiele App
De volgende code laat zien hoe u de [DeleteAsync] methode om te verwijderen van een bestaand exemplaar. Het exemplaar wordt geïdentificeerd door de `id` veld is ingesteld op de `todoItem`.

```
await todoTable.DeleteAsync(todoItem);
```

Niet-getypeerde om gegevens te verwijderen, kunt u profiteren van Json.NET als volgt:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Wanneer u een aanvraag verwijderen, moet een ID worden opgegeven. Andere eigenschappen worden niet doorgegeven aan de service of op de service worden genegeerd. Het resultaat van een `DeleteAsync` aanroep is meestal `null`. De ID op te geven kan worden verkregen van het resultaat van de `InsertAsync` aanroepen. Een `MobileServiceInvalidOperationException` wordt gegenereerd wanneer u probeert te verwijderen van een item zonder op te geven de `id` veld.

### <a name="optimisticconcurrency"></a>Hoe: gebruikt optimistische gelijktijdigheid voor conflictoplossing
Twee of meer clients kunnen schrijven naar hetzelfde item op hetzelfde moment. Zonder conflictdetectie overschrijft de laatste schrijfbewerking alle vorige updates. **Optimistisch gelijktijdigheidbeheer** wordt ervan uitgegaan dat elke transactie kan worden doorgevoerd en daarom geen vergrendeling van elke resource gebruikt.  Voordat u een transactie doorvoert, controleert Optimistisch gelijktijdigheidbeheer of geen andere transactie heeft de gegevens worden gewijzigd. Als de gegevens zijn gewijzigd, wordt de committing transactie teruggedraaid.

Mobiele Apps biedt ondersteuning voor Optimistisch gelijktijdigheidbeheer door het bijhouden van wijzigingen in elk item met behulp van de `version` systeemkolom eigenschap die is gedefinieerd voor elke tabel in uw back-end voor de mobiele App. Telkens wanneer een record is bijgewerkt, Mobile Apps stelt de `version` eigenschap voor deze record op een nieuwe waarde. Tijdens de aanvraag voor elke update de `version` eigenschap van de record is opgenomen in de aanvraag wordt vergeleken met dezelfde eigenschap voor de record op de server. Als de versie die is doorgegeven met de aanvraag komt niet overeen met de back-end, wordt de clientbibliotheek leidt tot een `MobileServicePreconditionFailedException<T>` uitzondering. Het type dat is opgenomen in de uitzondering is het record van de back-end met de versie van de servers van de record. De toepassing vervolgens deze gegevens kunt gebruiken om te bepalen of uitvoeren van de updateaanvraag opnieuw met de juiste `version` waarde van de back-end wijzigingen doorvoeren.

Een kolom definiëren voor de klasse van de tabel voor de `version` systeemeigenschap optimistische gelijktijdigheid inschakelen. Bijvoorbeeld:

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

Toepassingen die gebruikmaken van niet-getypeerde tabellen optimistische gelijktijdigheid inschakelen door in te stellen de `Version` vlag op de `SystemProperties` van de tabel als volgt.

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

Naast het inschakelen van optimistische gelijktijdigheid, u moet ook catch-de `MobileServicePreconditionFailedException<T>` uitzondering in uw code bij het aanroepen van [UpdateAsync].  Los het conflict op door het toepassen van de juiste `version` aan de bijgewerkte record en de aanroep [UpdateAsync] aan de record opgelost. De volgende code toont het oplossen van een schrijfconflict eenmaal gedetecteerd:

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resolution between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

Zie voor meer informatie de [Offline synchroniseren van gegevens in Azure Mobile Apps] onderwerp.

### <a name="binding"></a>Hoe: Bind Mobile Apps-gegevens naar een Windows-gebruikersinterface
Deze sectie wordt beschreven hoe geretourneerde gegevensobjecten met behulp van UI-elementen in een Windows-app kunt weergeven.  De volgende voorbeeldcode koppelt aan de bron van de lijst met een query voor onvolledige items. De [MobileServiceCollection] maakt een mobiele Apps-bewuste binding-verzameling.

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Sommige besturingselementen in de beheerde runtime ondersteunt een interface aangeroepen [ISupportIncrementalLoading]. Deze interface kunt besturingselementen om extra gegevens vragen wanneer de gebruiker. Er is ingebouwde ondersteuning voor deze interface voor universele Windows-apps via [MobileServiceIncrementalLoadingCollection], die automatisch de aanroepen van de besturingselementen verwerkt. Gebruik `MobileServiceIncrementalLoadingCollection` in Windows-apps als volgt:

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

U kunt de nieuwe verzameling voor apps uit de Windows Phone 8 en 'Silverlight', gebruiken de `ToCollection` uitbreidingsmethoden op `IMobileServiceTableQuery<T>` en `IMobileServiceTable<T>`. Om gegevens te laden, roepen `LoadMoreItemsAsync()`.

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Wanneer u de verzameling gemaakt door het aanroepen van `ToCollectionAsync` of `ToCollection`, ophalen van een verzameling die kan worden gebonden aan de UI-besturingselementen.  Deze verzameling is paginering-bewust.  Omdat de verzameling gegevens worden geladen vanaf het netwerk, soms laden is mislukt. Voor het afhandelen van dergelijke fouten, overschrijven de `OnException` methode op `MobileServiceIncrementalLoadingCollection` voor het afhandelen van uitzonderingen ten gevolge van aanroepen naar `LoadMoreItemsAsync`.

U kunt als de tabel veel velden heeft, maar u wilt alleen enkele ervan in uw beheer weergeven. U kunt de richtlijnen in de vorige sectie '[specifieke kolommen selecteren](#selecting)' om bepaalde kolommen om weer te geven in de gebruikersinterface te selecteren.

### <a name="pagesize"></a>Het paginaformaat wijzigen
Azure Mobile Apps retourneert een maximum van 50 items per aanvraag standaard.  U kunt de grootte van het wisselbestand wijzigen door het verhogen van de maximale grootte van de client en de server.  Geef voor een verhoging van de grootte van de aangevraagde pagina `PullOptions` bij gebruik van `PullAsync()`:

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Ervan uitgaande dat u hebt aangebracht de `PageSize` gelijk is aan of groter zijn dan 100 binnen de server een aanvraag maximaal 100 items geretourneerd.

## <a name="#offlinesync"></a>Werken met Offline tabellen
Een lokale SQLite winkel gegevens offline tabellen gebruiken voor gebruik wanneer u offline bent.  Alle tabel bewerkingen worden uitgevoerd voor de lokale SQLite opslaan in plaats van het archief van de externe server.  Een offline als tabel wilt maken, moet u eerst uw project voorbereiden:

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet-pakketten beheren voor oplossing...** , zoek vervolgens naar en installeren de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket voor alle projecten in de oplossing.
2. (Optioneel) Ter ondersteuning van Windows-apparaten, een van de volgende SQLite-runtime-pakketten te installeren:

   * **Windows 8.1 Runtime:** installeren [SQLite voor Windows 8.1][3].
   * **Windows Phone 8.1:** installeren [SQLite voor Windows Phone 8.1][4].
   * **Universal Windows Platform** installeren [SQLite voor het Universal Windows][5].
3. (Optioneel). Voor Windows-apparaten, klikt u op **verwijzingen** > **verwijzing toevoegen...** , vouw de **Windows** map > **extensies**, schakelt u vervolgens de juiste **SQLite voor Windows** SDK samen met de **Visual C++ 2013-Runtime voor Windows** SDK.
    De namen van SQLite SDK verschillen met elke Windows-platform.

Voordat een tabelverwijzing kan worden gemaakt, moet het lokale archief worden voorbereid:

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Initialiseren van de wordt normaal uitgevoerd onmiddellijk nadat de client wordt gemaakt.  De **OfflineDbPath** moet een bestandsnaam hebben die geschikt zijn voor gebruik op alle platforms die u ondersteunt.  Als het pad een volledig gekwalificeerde pad is (dat wil zeggen, begint met een slash), wordt dat pad gebruikt.  Als het pad niet volledig opgegeven is, wordt het bestand in een platform-specifieke locatie geplaatst.

* Voor iOS en Android-apparaten is het standaardpad voor de map 'Persoonlijke bestanden'.
* Voor Windows-apparaten is het standaardpad de toepassingsspecifieke 'AppData'-map.

Een tabelverwijzing kan worden verkregen met de `GetSyncTable<>` methode:

```
var table = client.GetSyncTable<TodoItem>();
```

U hoeft niet te verifiëren voor het gebruik van een offline-tabel.  U hoeft alleen te verifiëren wanneer u met de back-endservice communiceert.

### <a name="syncoffline"></a>Synchroniseren van een Offline-tabel
Offline tabellen zijn niet gesynchroniseerd met de back-end standaard.  Synchronisatie is opgesplitst in twee delen.  U kunt wijzigingen afzonderlijk forceren nieuwe items worden gedownload.  Hier volgt een typisch synchronisatiemethode:

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
            //Use a different query name for each unique query in your program
            "allTodoItems",
            this.todoTable.CreateQuery());
    }
    catch (MobileServicePushFailedException exc)
    {
        if (exc.PushResult != null)
        {
            syncErrors = exc.PushResult.Errors;
        }
    }

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
                await error.CancelAndUpdateItemAsync(error.Result);
            }
            else
            {
                // Discard local change.
                await error.CancelAndDiscardItemAsync();
            }

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

Als het eerste argument voor `PullAsync` is null, en vervolgens de incrementele synchronisatie wordt niet gebruikt.  Elke synchronisatiebewerking haalt alle records.

De SDK voert een impliciete `PushAsync()` voordat binnenhalen van records.

Conflict verwerking wordt uitgevoerd op een `PullAsync()` methode.  U kunt maken met conflicten op dezelfde manier als online-tabellen.  Het conflict wordt gemaakt wanneer `PullAsync()` wordt aangeroepen in plaats van tijdens het invoegen, bijwerken of verwijderen. Als meerdere conflicten optreden, worden ze in een enkel MobileServicePushFailedException gebundeld.  Elke fout afzonderlijk verwerkt.

## <a name="#customapi"></a>Werken met een aangepaste API gebruiken
Een aangepaste API kunt u aangepaste eindpunten die serverfunctionaliteit weergeven die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API gebruiken, kunt u meer controle over messaging, hebben waaronder lezen en HTTP-bericht headers instellen en definiëren van een berichttekst de indeling dan JSON.

U een aangepaste API aanroepen door het aanroepen van een van de [InvokeApiAsync] methoden op de client. De volgende regel code verzendt bijvoorbeeld een POST-aanvraag naar de **completeAll** API op de back-end:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Dit formulier is een getypeerde methodeaanroep en vereist dat de **MarkAllResult** retourneren type is gedefinieerd. Getypte en niet-getypeerde methoden worden ondersteund.

De methode InvokeApiAsync() voegt toe '/ api /' aan de API die u oproepen wilt tenzij de API met begint een '/'.
Bijvoorbeeld:

* `InvokeApiAsync("completeAll",...)`/api/completeAll aanroepen op de back-end
* `InvokeApiAsync("/.auth/me",...)`/.auth/me aanroepen op de back-end

U kunt InvokeApiAsync gebruiken om aan te roepen eventuele WebAPI, met inbegrip van deze WebAPIs die niet zijn gedefinieerd met Azure Mobile Apps.  Wanneer u InvokeApiAsync() gebruikt, worden de juiste headers, met inbegrip van verificatieheaders, verzonden met de aanvraag.

## <a name="authentication"></a>Verificatie van gebruikers
Mobiele Apps biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen voor alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in server-scripts. Zie de zelfstudie [verificatie toevoegen aan uw app] voor meer informatie.

Twee verificatie stromen worden ondersteund: *client beheerd* en *server beheerd* stroom. De server beheerd stroom biedt de eenvoudigste verificatie-ervaring, is afhankelijk van de webinterface voor verificatie van de provider. De stroom client beheerd kan voor de diepgaande integratie met apparaatspecifieke mogelijkheden, zoals is afhankelijk van de provider-specifieke apparaat-specifieke SDK's.

> [!NOTE]
> U wordt aangeraden met behulp van een stroom client beheerd in uw productie-apps.

Als u verificatie instelt, moet u uw app registreren bij een of meer id-providers.  De id-provider genereert een client-ID en een clientgeheim voor uw app.  Deze waarden worden vervolgens in uw back-end ingesteld zodat Azure App Service-verificatie/autorisatie.  Voor meer informatie, volgt u de gedetailleerde instructies in de zelfstudie [verificatie toevoegen aan uw app].

De volgende onderwerpen worden in deze sectie behandeld:

* [Verificatie van client beheerd](#clientflow)
* [Beheerde server-verificatie](#serverflow)
* [Het verificatietoken opslaan in cache](#caching)

### <a name="clientflow"></a>Verificatie van client beheerd
Uw app kunt onafhankelijk Neem contact op met de id-provider en geef vervolgens het token geretourneerd tijdens het aanmelden met uw back-end. Deze stroom client kunt u voor een ervaring voor eenmalige aanmelding voor gebruikers of aanvullende gegevens ophalen van de id-provider. Stroom clientverificatie verdient de voorkeur voor het gebruik van de stroom van een server als de id-provider SDK een meer systeemeigen UX idee biedt en kunt u extra aanpassingen.

Voorbeelden zijn bedoeld voor de volgende patronen voor client-flow-verificatie:

* [Active Directory Authentication Library](#adal)
* [Facebook of Google](#client-facebook)
* [Live SDK](#client-livesdk)

#### <a name="adal"></a>Verificatie van gebruikers met de Active Directory Authentication Library
U kunt de Active Directory Authentication Library (ADAL) initiëren gebruikersverificatie van de client met behulp van Azure Active Directory-verificatie.

1. Instellen van uw mobiele app back-end voor AAD aanmelding door de [App Service configureren voor Active Directory-aanmelding] zelfstudie. Zorg ervoor dat de optionele stap voor het registreren van een systeemeigen clienttoepassing van voltooien.
2. Open het project in Visual Studio en Xamarin Studio en voeg een verwijzing naar de `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-pakket. Wanneer u zoekt, omvatten voorlopige versies.
3. De volgende code toevoegen aan uw toepassing op basis van het platform dat u gebruikt. Controleer de volgende vervangingen in elke:

   * Vervang **INSERT-instantie-hier** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure-portal].
   * Vervang **INSERT RESOURCE-ID hier** met de client-ID voor uw back-end voor de mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
   * Vervang **INSERT-CLIENT-ID-hier** met de client-ID die u hebt gekopieerd uit de native client-toepassing.
   * Vervang **INSERT-OMLEIDINGS-URI-hier** aan uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet er ongeveer als *https://contoso.azurewebsites.net/.auth/login/done*.

     Hier volgt de code die nodig zijn voor elk platform:

     **Windows:**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

     **Xamarin.iOS**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync(UIViewController view)
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

     **Xamarin.Android**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
    }
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ```

#### <a name="client-facebook"></a>Eenmalige aanmelding met behulp van een token van Facebook of Google
U kunt de stroom kunt gebruiken, zoals weergegeven in dit fragment voor Facebook of Google.

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

#### <a name="client-livesdk"></a>Eenmalige aanmelding met Microsoft-Account met de Live SDK
Om gebruikers te verifiëren, moet u uw app via de Microsoft-account Developer Center te registreren. Configureer de registratiedetails op uw mobiele App back-end. Als u wilt een registratie van Microsoft-account maken en te verbinden met uw back-end voor de mobiele App, voer de stappen in [registreren van uw app voor het gebruik van een Microsoft-account aanmelding]. Als u zowel de Windows Store en Windows Phone 8/Silverlight-versies van uw app hebt, registreert u eerst de Windows Store-versie.

De volgende code wordt geverifieerd met Live SDK en gebruikt het resulterende token aan te melden bij uw back-end voor de mobiele App.

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get the URL the Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create the authentication client for Windows Store using the service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create the authentication client for Windows Phone using the client ID of the registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request the authentication token from the Live authentication service.
        // The wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about the logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use the Microsoft account auth token to sign in to App Service.
            MobileServiceUser loginResult = await App.MobileService
                .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

            // Display a personalized sign-in greeting.
            string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
            var message = string.Format("You are now logged in - {0}", loginResult.UserId);
            var dialog = new MessageDialog(message, title);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
        else
        {
            session = null;
            var dialog = new MessageDialog("You must log in.", "Login Required");
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
}
```

Zie voor meer informatie de [Windows Live SDK] documentatie.

### <a name="serverflow"></a>Beheerde server-verificatie
Wanneer u de id-provider hebt geregistreerd, aanroepen de [LoginAsync] methode op [MobileServiceClient] met de [MobileServiceAuthenticationProvider] waarde van de provider. De volgende code initieert bijvoorbeeld een server stroom aanmelden via Facebook.

```
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

Als u van een id-provider dan Facebook gebruikmaakt, wijzigt u de waarde van [MobileServiceAuthenticationProvider] aan de waarde voor de provider.

In een stroom server beheert Azure App Service de OAuth-authenticatiestroom door de aanmeldingspagina van de geselecteerde provider weer te geven.  Nadat de id-provider retourneert, Azure App Service genereert een App Service-verificatietoken. De [LoginAsync] methode retourneert een [MobileServiceUser], waarmee u zowel de [UserId] van de geverifieerde gebruiker en de [MobileServiceAuthenticationToken], als een JSON web token (JWT). Dit token kan worden opgeslagen in de cache en opnieuw worden gebruikt totdat het verloopt. Zie voor meer informatie [het verificatietoken Caching](#caching).

### <a name="caching"></a>Het verificatietoken opslaan in cache
In sommige gevallen kan de aanroep van de methode aanmelding na de eerste geslaagde verificatie worden vermeden door op te slaan van het verificatietoken van de provider.  Windows Store en UWP-apps kunt gebruiken [PasswordVault] in de cache van het huidige verificatietoken na een geslaagde aanmelden, als volgt:

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

De gebruikers-id-waarde wordt opgeslagen als de gebruikersnaam van de referentie en het token is de opgeslagen als het wachtwoord. Op de volgende startende, kunt u controleren de **PasswordVault** voor in cache opgeslagen referenties. Het volgende voorbeeld wordt de in cache opgeslagen referenties wanneer ze worden gevonden, en anders probeert opnieuw te verifiëren met de back-end:

```
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

Wanneer u een gebruiker afmelden, moet u de opgeslagen referenties, ook als volgt verwijderen:

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin-apps gebruiken de [Xamarin.Auth] API's veilig opslaan van referenties in een **Account** object. Zie voor een voorbeeld van het gebruik van deze API's, de [AuthStore.cs] codebestand in de [ContosoMoments photo delen voorbeeld](https://github.com/azure-appservice-samples/ContosoMoments).

Als u client beheerd authenticatie gebruikt, kunt u ook het toegangstoken dat is verkregen van uw provider zoals Facebook- of Twitter cache. Dit token kan worden opgegeven om aan te vragen van een nieuw verificatietoken van de back-end als volgt:

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Pushmeldingen
De volgende onderwerpen vindt Pushmeldingen:

* [Registreren voor Pushmeldingen](#register-for-push)
* [Verkrijgen van een pakket-SID van de Windows Store](#package-sid)
* [Registreren bij platformoverschrijdende sjablonen](#register-xplat)

### <a name="register-for-push"></a>How to: registreren voor Pushmeldingen
De client voor mobiele Apps kunt u zich registreren voor pushmeldingen met Azure Notification Hubs. Als u registreert, krijgt u een koppeling die u uit de platform-specifieke Push Notification Service (PNS verkrijgt). Deze waarde samen met eventuele labels wordt vervolgens opgeven bij het maken van de registratie. De volgende code registreert uw Windows-app voor pushmeldingen met Windows Notification Service (WNS):

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Als u naar WNS pushen, dan u moet [verkrijgen van een pakket-SID van de Windows Store](#package-sid).  Zie voor meer informatie over Windows-apps, inclusief het registreren voor de sjabloon-registraties [pushmeldingen toevoegen aan uw app].

Aanvragen van de labels van de client wordt niet ondersteund.  Tag aanvragen zijn achtergrond verwijderd uit de inschrijving.
Indien gewenst kunt u uw apparaat registreren met labels, maakt u een aangepaste API die gebruikmaakt van de Notification Hubs-API voor het uitvoeren van de registratie namens jou.  [De aangepaste API aanroepen](#customapi) in plaats van de `RegisterNativeAsync()` methode.

### <a name="package-sid"></a>Hoe: verkrijgen van een pakket-SID van de Windows Store
Een pakket-SID is nodig voor het inschakelen van pushmeldingen in Windows Store-apps.  Voor het ontvangen van een pakket-SID, moet u uw toepassing registreren met de Windows Store.

Als u deze waarde:

1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project, klikt u op **Store** > **App aan de Store koppelen...** .
2. Klik in de wizard op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **een nieuwe appnaam reserveren**, klikt u vervolgens op **Reserve**.
3. Nadat de registratie van de app is gemaakt, selecteert u de appnaam, klikt u op **volgende**, en klik vervolgens op **koppelen**.
4. Meld u aan bij de [Windows-ontwikkelaarscentrum] met uw Microsoft-Account. Onder **mijn apps**, klikt u op de registratie van de app die u hebt gemaakt.
5. Klik op **appbeheer** > **App identiteit**, en schuif vervolgens omlaag naar het zoeken naar uw **pakket-SID**.

Vele toepassingen van de pakket-SID behandelen als een URI in dat geval moet u gebruiken *ms-app: / /* als schema. Noteer de versie van uw pakket-SID gevormd door samenvoeging van deze waarde als voorvoegsel.

Xamarin-apps nodig aanvullende code te kunnen registreren van een app die wordt uitgevoerd op de iOS of Android-platform. Zie het onderwerp voor uw platform voor meer informatie:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Hoe: Register push sjablonen om platformoverschrijdende meldingen te verzenden
Als u wilt registreren sjablonen, gebruiken de `RegisterAsync()` methode met de sjablonen als volgt:

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Uw sjablonen moet `JObject` van het type en kan meerdere sjablonen in de volgende JSON-indeling bevatten:

```
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

De methode **RegisterAsync()** accepteert ook secundaire tegels:

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Alle codes zijn te vertalen tijdens de registratie voor beveiliging. Zie [werken met de .NET-back-endserver SDK voor Azure Mobile Apps] labels toevoegen op installaties of -sjablonen in installaties.

Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, verwijzen naar de [Notification Hubs-API's].

## <a name="misc"></a>Diverse onderwerpen
### <a name="errors"></a>Hoe: afhandelen van fouten
Wanneer een fout in de back-end optreedt, de client SDK leidt tot een `MobileServiceInvalidOperationException`.  Het volgende voorbeeld ziet u hoe een uitzondering die is geretourneerd door de back-end afhandelen:

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

Een ander voorbeeld van moeten omgaan met de foutvoorwaarden vindt u in de [Mobile Apps bestanden voorbeeld]. De [LoggingHandler] voorbeeld bevat een handler van de gemachtigde logboekregistratie om de aanvragen worden ingediend bij de back-end te registreren.

### <a name="headers"></a>Hoe: aanpassen aanvraagheaders
Ter ondersteuning van uw specifieke app-scenario, moet u mogelijk communicatie met de back-end voor de mobiele App aanpassen. U wilt bijvoorbeeld een aangepaste header toevoegen aan elke uitgaande aanvraag of zelfs statuscodes antwoorden te wijzigen. U kunt een aangepaste [DelegatingHandler], zoals in het volgende voorbeeld:

```
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[verificatie toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline synchroniseren van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[pushmeldingen toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[registreren van uw app voor het gebruik van een Microsoft-account aanmelding]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[App Service configureren voor Active Directory-aanmelding]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[wordt een verwijzing naar een niet-getypeerde tabel]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[nemen]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Selecteer]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[overslaan]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Gebruikers-id]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[waar]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portal]: https://portal.azure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows-ontwikkelaarscentrum]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Notification Hubs-API's]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobile Apps bestanden voorbeeld]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 documentatie]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
