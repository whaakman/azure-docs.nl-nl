---
title: Werken met de App Service Mobile Apps beheerde client-bibliotheek | Microsoft Docs
description: Meer informatie over het gebruik van de .NET-clientbibliotheek voor Azure App Service Mobile Apps met Windows en Xamarin-apps.
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 30eb458b9d0584b467479163d8312d7bd5456be1
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409731"
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>De beheerde client gebruiken voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u algemene scenario's met behulp van de beheerde-clientbibliotheek voor Azure App Service Mobile Apps voor Windows en Xamarin-apps uitvoert. Als u niet bekend bent met Mobile Apps, moet u overwegen eerst voltooien van de [Azure Mobile Apps quickstart] [ 1] zelfstudie. In deze handleiding, we ons richten op de beheerde client-side-SDK. Voor meer informatie over de server-side SDK's voor Mobile Apps, Zie de documentatie voor de [.NET Server SDK] [ 2] of de [Node.js Server SDK] [ 3].

## <a name="reference-documentation"></a>Referentiedocumentatie
De referentiedocumentatie voor de client SDK bevindt zich hier: [Naslaginformatie over Azure Mobile Apps .NET client][4].
U vindt hier ook enkele voorbeelden van de client in de [Azure-Samples-GitHub-opslagplaats][5].

## <a name="supported-platforms"></a>Ondersteunde Platforms
Het .NET-Platform ondersteunt de volgende platforms:

* Xamarin Android-versies voor API 19 tot en met 24 (KitKat via Nougat)
* Xamarin iOS-versies voor iOS 8.0 en hoger
* Universeel Windows-platform
* Windows Phone 8.1
* Windows Phone 8.0, met uitzondering van Silverlight-toepassingen

De verificatie 'server-stroom' gebruikmaakt van een webweergave voor de gebruikersinterface weergegeven.  Als het apparaat niet kan zijn om weer te geven van een UI WebView, zijn andere methoden voor verificatie nodig.  Deze SDK is dus niet geschikt is voor controle van het type of op dezelfde manier beperkte apparaten.

## <a name="setup"></a>Installatie en vereisten
Gaan we ervan uit dat u hebt al gemaakt en gepubliceerd uw Mobile App back-end-project, waaronder ten minste één tabel.  In de code die wordt gebruikt in dit onderwerp, de tabel met de naam `TodoItem` en heeft de volgende kolommen: `Id`, `Text`, en `Complete`. Deze tabel is dezelfde tabel gemaakt na voltooiing van de [Azure Mobile Apps quickstart][1].

Het bijbehorende getypte client-side '-type in C# is de volgende klasse:

```csharp
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

Zie voor meer informatie over het maken van tabellen in uw back-end van Mobile Apps, de [.NET Server SDK onderwerp] [ 7] of de [Node.js Server SDK onderwerp][8]. Als u de back-end van uw mobiele App in Azure portal met behulp van de Quick Start hebt gemaakt, kunt u ook gebruiken de **eenvoudige tabellen** instellen in de [Azure Portal].

### <a name="how-to-install-the-managed-client-sdk-package"></a>Procedure: De beheerde client-SDK-pakket installeren
Gebruik een van de volgende methoden voor het installeren van de beheerde client-SDK-pakket voor mobiele Apps van [NuGet][9]:

* **Visual Studio** met de rechtermuisknop op uw project, klikt u op **NuGet-pakketten beheren**, zoek de `Microsoft.Azure.Mobile.Client` pakket en klik vervolgens op **installeren**.
* **Xamarin Studio** met de rechtermuisknop op uw project, klikt u op **toevoegen** > **NuGet-pakketten toevoegen**, zoek de `Microsoft.Azure.Mobile.Client `verpakt en klik vervolgens op **pakket toevoegen** .

In het bestand van uw belangrijkste activiteit, vergeet niet om toe te voegen van de volgende **met behulp van** instructie:

```csharp
using Microsoft.WindowsAzure.MobileServices;
```

> [!NOTE]
> Alle ondersteuningspakketten waarnaar wordt verwezen in uw Android-project, moeten dezelfde versie hebben. De SDK heeft `Xamarin.Android.Support.CustomTabs` afhankelijkheid voor Android-platform, dus als uw project gebruikmaakt van nieuwere ondersteuning u pakketten moet dit pakket installeren met de vereiste versie rechtstreeks op het voorkomen van conflicten.

### <a name="symbolsource"></a>Procedures: Werken met foutopsporingssymbolen in Visual Studio
De symbolen voor de naamruimte Microsoft.Azure.Mobile zijn beschikbaar op [SymbolSource][10].  Raadpleeg de [SymbolSource instructies] [ 11] SymbolSource integreren met Visual Studio.

## <a name="create-client"></a>De Mobile Apps-client maken
De volgende code maakt de [MobileServiceClient] [ 12] -object dat wordt gebruikt voor toegang tot de back-end van uw mobiele App.

```csharp
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Vervang in de bovenstaande code `MOBILE_APP_URL` met de URL van de back-end van de mobiele App, die is gevonden in de blade voor uw mobiele App back-end in de [Azure Portal]. Het object MobileServiceClient moet een singleton.

## <a name="work-with-tables"></a>Werken met tabellen
De volgende sectie wordt uitgelegd hoe om te zoeken en records ophalen en de gegevens in de tabel wijzigen.  De volgende onderwerpen komen aan bod:

* [Maken van een tabelverwijzing](#instantiating)
* [Gegevens op te vragen](#querying)
* [Geretourneerde gegevens filteren](#filtering)
* [Geretourneerde gegevens sorteren](#sorting)
* [Als resultaat de gegevens op pagina 's](#paging)
* [Specifieke kolommen selecteren](#selecting)
* [Een record door-Id opzoeken](#lookingup)
* [Omgaan met niet-getypeerde query 's](#untypedqueries)
* [Gegevens invoegen](#inserting)
* [Het bijwerken van gegevens](#updating)
* [Het verwijderen van gegevens](#deleting)
* [Conflictoplossing en functionaliteit voor optimistische gelijktijdigheid](#optimisticconcurrency)
* [Binding met een Windows-gebruikersinterface](#binding)
* [Formaat van de pagina wijzigen](#pagesize)

### <a name="instantiating"></a>Procedures: Maken van een tabelverwijzing
De code die toegang heeft tot of wijzigt u gegevens in de endtabel van een back-roept functies aan op de `MobileServiceTable` object. Een verwijzing naar de tabel ophalen door het aanroepen van de [GetTable] methode, als volgt te werk:

```csharp
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Het geretourneerde object maakt gebruik van het type serialisatie-model. Een niet-getypeerde serialisatie-model wordt ook ondersteund. Het volgende voorbeeld [Hiermee maakt u een verwijzing naar een niet-getypeerde tabel]:

```csharp
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Niet-getypeerde query's, moet u de onderliggende OData-query-tekenreeks opgeven.

### <a name="querying"></a>Procedures: Gegevens van uw mobiele App op te vragen
In deze sectie wordt beschreven hoe u query's uitgeven aan de backend voor mobiele Apps, waaronder de volgende functionaliteit:

* [Geretourneerde gegevens filteren](#filtering)
* [Geretourneerde gegevens sorteren](#sorting)
* [Als resultaat de gegevens op pagina 's](#paging)
* [Specifieke kolommen selecteren](#selecting)
* [Gegevens per-ID opzoeken](#lookingup)

> [!NOTE]
> Het formaat van een server gebaseerde pagina wordt afgedwongen om te voorkomen dat alle rijen worden geretourneerd.  Wisselbestand zorgt ervoor dat standaard aanvragen voor grote gegevenssets niet een negatieve invloed op de service.  Als u wilt meer dan 50 rijen retourneren, gebruikt u de `Skip` en `Take` methode, zoals beschreven in [als resultaat de gegevens op pagina's](#paging).

### <a name="filtering"></a>Procedures: Geretourneerde gegevens filteren
De volgende code laat zien hoe u gegevens filteren door een `Where` -component in een query. Deze retourneert alle items uit `todoTable` waarvan `Complete` eigenschap is gelijk aan `false`. De [waar] functie is van toepassing een rij predicaat om de query op de tabel te filteren.

```csharp
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

U kunt de URI van de aanvraag verzonden naar de back-end met behulp van bericht inspectie software, zoals browser ontwikkelhulpprogramma's weergeven of [Fiddler]. Als u de aanvraag-URI bekijkt, ziet u dat de queryreeks wordt gewijzigd:

```csharp
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Deze OData-aanvraag wordt omgezet in een SQL-query door de SDK-Server:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

De functie die wordt doorgegeven aan de `Where` methode een willekeurig aantal voorwaarden kan hebben.

```csharp
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

In dit voorbeeld zou worden vertaald naar een SQL-query door de SDK-Server:

```csharp
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Deze query kan ook worden gesplitst in meerdere componenten:

```csharp
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

De twee methoden zijn gelijkwaardig en door elkaar kunnen worden gebruikt.  De eerste optie&mdash;van het samenvoegen van meerdere predicaten in één query&mdash;compacter en aanbevolen is.

De `Where` component biedt ondersteuning voor bewerkingen die worden vertaald naar de subset OData. Bewerkingen zijn onder meer:

* Relationele operators (==,! =, <, < =, >, > =),
* Rekenkundige operators (+, -, /, *, %),
* Number precisie (Math.Floor, Math.Ceiling)
* Tekenreeksfuncties (lengte, subtekenreeks, vervangen, IndexOf, StartsWith, EndsWith),
* Datumeigenschappen van (jaar, maand, dag, uur, minuut, seconde)
* Toegang tot de eigenschappen van een object, en
* Expressies combineren van elk van deze bewerkingen.

Wanneer u overweegt wat de SDK-Server ondersteunt, kunt u overwegen de [OData v3-documentatie].

### <a name="sorting"></a>Procedures: Geretourneerde gegevens sorteren
De volgende code laat zien hoe u gegevens sorteren door op te nemen een [Sorteren op] of [OrderByDescending] functie in de query. Het resultaat van items uit `todoTable` door oplopend gesorteerd de `Text` veld.

```csharp
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="paging"></a>Procedures: Als resultaat de gegevens op pagina 's
De back-end retourneert standaard alleen de eerste 50 rijen. U kunt het aantal geretourneerde rijen verhogen door het aanroepen van de [nemen] methode. Gebruik `Take` samen met de [overslaan] methode om aan te vragen van een specifieke "page" van de totale gegevensset die wordt geretourneerd door de query. De volgende query, wanneer uitgevoerd, retourneert de bovenste drie items in de tabel.

```csharp
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

De volgende gewijzigde query wordt de eerste drie resultaten overgeslagen en worden de volgende drie resultaten geretourneerd. Deze query levert de tweede "page" van gegevens, waarbij de paginagrootte van de drie items is.

```csharp
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

De [IncludeTotalCount] methode vraagt het totale aantal voor *alle* de records die zou zijn geretourneerd, wordt een wisselbestand/limit-component opgegeven genegeerd:

```csharp
query = query.IncludeTotalCount();
```

In een echte wereld-app, kunt u query's die vergelijkbaar is met het voorgaande voorbeeld met een semafoon besturingselement of een vergelijkbare UI navigeren tussen pagina's.

> [!NOTE]
> Als u wilt overschrijven de limiet van 50-rijen in een back-end van de mobiele App, moet u ook toepassen de [EnableQueryAttribute] openbaar methode GET en geef het gedrag van het wisselbestand. Wanneer toegepast op de methode, wordt het volgende stelt het maximum aantal rijen geretourneerd naar 1000:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Procedures: Specifieke kolommen selecteren
U kunt opgeven welke set eigenschappen om op te nemen in de resultaten door toe te voegen een [Selecteren] component aan uw query. Bijvoorbeeld, toont de volgende code hoe u slechts één veld selecteert en ook hoe u kunt selecteren en formatteren van meerdere velden:

```csharp
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

Alle functies die beschreven tot nu toe zijn additief, zodat we kunnen houden chaining ze. Elke aanroep van de keten is van invloed op meer van de query. Een meer voorbeeld:

```csharp
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Procedures: Gegevens per-ID opzoeken
De [LookupAsync] functie kan worden gebruikt om te controleren of objecten uit de database met een bepaalde ID.

```csharp
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Procedures: Niet-getypeerde query's uitvoeren
Bij het uitvoeren van een tabelobject niet-getypeerde met een query uitvoeren, moet u expliciet de OData-query-tekenreeks opgeven door het aanroepen van [ReadAsync], zoals in het volgende voorbeeld:

```csharp
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

U krijgt terug JSON-waarden die u, zoals een eigenschappenverzameling gebruiken kunt. Zie voor meer informatie over JToken en Newtonsoft Json.NET de [Json.NET] site.

### <a name="inserting"></a>Procedures: Gegevens invoegen in een mobiele App back-end
Alle clienttypen moeten een lid met de naam bevatten **Id**, dit is standaard een tekenreeks. Dit **Id** CRUD-bewerkingen uit te voeren is vereist en voor het offline synchroniseren. De volgende code laat zien hoe u de [InsertAsync] methode voor het invoegen van nieuwe rijen in een tabel. De parameter bevat de gegevens moet worden ingevoegd als een .NET-object.

```csharp
await todoTable.InsertAsync(todoItem);
```

Als een unieke aangepaste ID-waarde niet is opgenomen in de `todoItem` tijdens een insert-, een GUID wordt gegenereerd door de server.
U kunt de gegenereerde Id ophalen door het inspecteren van het object nadat de aanroep retourneert.

Voor het invoegen van niet-getypeerde gegevens, kunt u profiteren van Json.NET:

```csharp
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Hier volgt een voorbeeld met behulp van een e-mailadres als een unieke tekenreeks-id:

```csharp
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Werken met de id-waarden
Mobile Apps biedt ondersteuning voor unieke tekenreeks van aangepaste waarden van de tabel **id** kolom. Een string-waarde zorgt ervoor dat toepassingen gebruiken van aangepaste waarden, zoals e-mailadressen of gebruikersnamen voor de ID.  Tekenreeks-id's bieden u de volgende voordelen:

* Id's worden gegenereerd zonder een retour naar de database.
* Records zijn gemakkelijker te samenvoegen uit andere tabellen of databases.
* Id-waarden kunnen beter integreren met de logica van een toepassing.

Als een tekenreeks-ID-waarde niet is ingesteld op een record ingevoegd, de mobiele App back-end wordt gegenereerd voor een unieke waarde voor de ID. U kunt de [Guid.NewGuid] methode voor het genereren van uw eigen id-waarden, op de client of in de back-end.

```csharp
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Procedures: Gegevens in een back-end van Mobile App wijzigen
De volgende code laat zien hoe u de [UpdateAsync] methode voor het bijwerken van een bestaande record met dezelfde ID met nieuwe gegevens. De parameter bevat de gegevens moeten worden bijgewerkt als een .NET-object.

```csharp
await todoTable.UpdateAsync(todoItem);
```

Voor het bijwerken van niet-getypeerde gegevens, u kunt profiteren van [Json.NET] als volgt:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Een `id` veld moet worden opgegeven bij het maken van een update. De back-end maakt gebruik van de `id` veld om te bepalen welke rij bij te werken. De `id` veld kan worden opgehaald uit het resultaat van de `InsertAsync` aanroepen. Een `ArgumentException` treedt op als u probeert een item bijwerken zonder op te geven de `id` waarde.

### <a name="deleting"></a>Procedures: Gegevens in een back-end van Mobile App verwijderen
De volgende code laat zien hoe u de [DeleteAsync] methode voor het verwijderen van een bestaand exemplaar. Het exemplaar wordt geïdentificeerd door de `id` veld is ingesteld op de `todoItem`.

```csharp
await todoTable.DeleteAsync(todoItem);
```

Niet-getypeerde om gegevens te verwijderen, kunt u profiteren van Json.NET als volgt:

```csharp
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Wanneer u een delete-aanvraag, moet een ID worden opgegeven. Andere eigenschappen worden niet doorgegeven aan de service of in de service worden genegeerd. Het resultaat van een `DeleteAsync` -aanroep is meestal `null`. De ID op om door te geven, kan worden opgehaald van het resultaat van de `InsertAsync` aanroepen. Een `MobileServiceInvalidOperationException` wordt gegenereerd wanneer u probeert te verwijderen van een item zonder op te geven de `id` veld.

### <a name="optimisticconcurrency"></a>Procedures: Optimistische gelijktijdigheid gebruiken voor conflictoplossing
Twee of meer clients kunnen schrijven naar hetzelfde item op hetzelfde moment. Zonder opsporing van conflicten, zou de laatste schrijfbewerking overschrijven alle voorgaande updates. **Optimistisch gelijktijdigheidbeheer** wordt ervan uitgegaan dat elke transactie kan worden doorgevoerd en daarom geen van een resource vergrendelen gebruikmaakt.  Voordat het doorvoeren van een transactie Optimistisch gelijktijdigheidbeheer controleert of er geen andere transactie heeft de gegevens worden gewijzigd. Als de gegevens is gewijzigd, de committing transactie teruggedraaid.

Mobile Apps biedt ondersteuning voor Optimistisch gelijktijdigheidbeheer door het bijhouden van wijzigingen in elk item met behulp van de `version` systeemkolom eigenschap die is gedefinieerd voor elke tabel in de back-end van uw mobiele App. Telkens wanneer een record wordt bijgewerkt, Mobile Apps-stelt de `version` eigenschap voor die record in een nieuwe waarde. Tijdens de aanvraag voor elke update de `version` eigenschap van de record die is opgenomen in de aanvraag wordt vergeleken met de dezelfde eigenschap van de record op de server. Als de versie die is doorgegeven met de aanvraag komt niet overeen met de back-end, wordt de clientbibliotheek leidt tot een `MobileServicePreconditionFailedException<T>` uitzondering. Het type dat is opgenomen in de uitzondering is de record van de back-end die de servers-versie van de record. De toepassing kan deze informatie vervolgens gebruiken om te bepalen of voor het uitvoeren van de updateaanvraag opnieuw uit met de juiste `version` waarde van de back-end wijzigingen wilt doorvoeren.

Een kolom definiëren voor de tabel-klasse voor de `version` systeemeigenschap om in te schakelen van optimistische gelijktijdigheid. Bijvoorbeeld:

```csharp
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

Toepassingen die gebruikmaken van niet-getypeerde tabellen optimistische gelijktijdigheid inschakelen door in te stellen de `Version` markering waarmee wordt aangegeven op de `SystemProperties` van de tabel als volgt te werk.

```csharp
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

Naast het inschakelen van optimistische gelijktijdigheid moet u ook moet catch de `MobileServicePreconditionFailedException<T>` uitzondering in uw code bij het aanroepen van [UpdateAsync].  Los het conflict op door het toepassen van de juiste `version` naar de bijgewerkte record en roep [UpdateAsync] met de record opgelost. De volgende code laat zien hoe om op te lossen een schrijfconflict eenmaal is gedetecteerd:

```csharp
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

Zie voor meer informatie de [Offlinesynchronisatie van gegevens in Azure Mobile Apps] onderwerp.

### <a name="binding"></a>Procedures: Mobile Apps-gegevens binden aan een Windows-gebruikersinterface
Deze sectie wordt beschreven hoe objecten van geretourneerde gegevens met behulp van UI-elementen in een Windows-app kunt weergeven.  De volgende voorbeeldcode koppelt aan de bron van de lijst met een query voor onvolledige items. De [MobileServiceCollection] maakt een verzameling met mobiele Apps-bewuste binding.

```csharp
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

Sommige besturingselementen in de beheerde runtime ondersteunt een interface met de naam [ISupportIncrementalLoading]. Deze interface kunt besturingselementen voor het aanvragen van extra gegevens wanneer de gebruiker. Er is ingebouwde ondersteuning voor deze interface voor universele Windows-apps via [MobileServiceIncrementalLoadingCollection], die verwerkt automatisch de aanroepen van de besturingselementen. Gebruik `MobileServiceIncrementalLoadingCollection` in Windows-apps als volgt te werk:

```csharp
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Als u de nieuwe verzameling op Windows Phone 8- en 'Silverlight' apps, gebruikt de `ToCollection` uitbreidingsmethoden op `IMobileServiceTableQuery<T>` en `IMobileServiceTable<T>`. Aanroepen om gegevens te laden, `LoadMoreItemsAsync()`.

```csharp
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Bij het gebruik van de verzameling die zijn gemaakt door het aanroepen van `ToCollectionAsync` of `ToCollection`, krijgt u een verzameling die kan worden gebonden aan een UI-besturingselementen.  Deze verzameling is wisselbestand-bewust.  Omdat de verzameling is het laden van gegevens vanaf het netwerk, mislukt het laden van soms. Voor het afhandelen van dergelijke fouten, overschrijven de `OnException` methode voor `MobileServiceIncrementalLoadingCollection` voor het afhandelen van uitzonderingen die voortvloeien uit aanroepen naar `LoadMoreItemsAsync`.

Als uw tabel veel velden heeft, maar u wilt alleen enkele ervan weer te geven in uw overwegen. U kunt de richtlijnen in de voorgaande sectie "[specifieke kolommen selecteren](#selecting)' om specifieke kolommen om weer te geven in de gebruikersinterface te selecteren.

### <a name="pagesize"></a>De grootte van de pagina wijzigen
Azure Mobile Apps retourneert een maximum van 50 items per aanvraag standaard.  U kunt de grootte van het wisselbestand wijzigen door de maximale grootte van de client en de server.  Als u wilt de aangevraagde pagina vergroten, geef `PullOptions` bij het gebruik van `PullAsync()`:

```csharp
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Ervan uitgaande dat u hebt aangebracht de `PageSize` gelijk aan of groter zijn dan 100 in de server, een aanvraag tot 100 items geretourneerd.

## <a name="#offlinesync"></a>Werken met Offline tabellen
Offline tabellen gebruik een lokale gegevens van de SQLite-archief op te slaan voor gebruik wanneer u offline bent.  Alle tabel bewerkingen worden uitgevoerd op basis van de lokale SQLite opslaan in plaats van de externe server-store.  Als u wilt een offline-tabel maken, moet u eerst uw project voorbereiden:

1. In Visual Studio met de rechtermuisknop op de oplossing > **NuGet-pakketten beheren voor oplossing...** , zoek en installeer de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket voor alle projecten in de oplossing.
2. (Optioneel) Ter ondersteuning van Windows-apparaten, moet u een van de volgende pakketten van de SQLite-runtime installeren:

   * **Windows 8.1-Runtime:** Installeer [SQLite voor Windows 8.1][3].
   * **Windows Phone 8.1:** Installeer [SQLite voor Windows Phone 8.1][4].
   * **Universal Windows Platform** installeren [SQLite voor het Universal Windows][5].
3. (Optioneel). Voor Windows-apparaten, klikt u op **verwijzingen** > **verwijzing toevoegen...** , vouw de **Windows** map > **extensies**, schakelt u de juiste **SQLite voor Windows** SDK samen met de **Visual C++ 2013-Runtime voor Windows** SDK.
    De namen van de SQLite SDK verschillen met elke Windows-platform.

Voordat een tabelverwijzing kan worden gemaakt, kan het lokale archief moet worden voorbereid:

```csharp
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Store-initialisatie is normaal uitgevoerd onmiddellijk nadat de client wordt gemaakt.  De **OfflineDbPath** moet een bestandsnaam geschikt zijn voor gebruik op alle platformen die u ondersteunt.  Als het pad een volledig gekwalificeerde pad is (dat wil zeggen, het begint al met een slash), wordt het opgegeven pad gebruikt.  Als het pad is niet volledig gekwalificeerd, wordt het bestand geplaatst op een platform-specifieke locatie.

* Voor iOS en Android-apparaten is het standaardpad naar de map 'Persoonlijke Files'.
* Voor Windows-apparaten is het standaardpad naar de map van de "AppData" toepassingsspecifieke.

Een tabelverwijzing kan worden verkregen met behulp van de `GetSyncTable<>` methode:

```csharp
var table = client.GetSyncTable<TodoItem>();
```

U hoeft niet te verifiëren voor het gebruik van een offline-tabel.  U hoeft alleen te verifiëren wanneer u met de back-endservice communiceert.

### <a name="syncoffline"></a>Synchroniseren van een Offline-tabel
Offline tabellen zijn niet gesynchroniseerd met de back-end standaard.  Synchronisatie wordt gesplitst in twee delen.  U kunt wijzigingen afzonderlijk forceren van het downloaden van nieuwe items.  Hier volgt een typische synchronisatiemethode:

```csharp
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

Als het eerste argument voor `PullAsync` null is, en vervolgens incrementele synchronisatie wordt niet gebruikt.  Elke synchronisatiebewerking haalt u alle records.

De SDK voert een impliciete `PushAsync()` voordat u records.

Conflict verwerking wordt uitgevoerd op een `PullAsync()` methode.  U kunt omgaan met conflicten op dezelfde manier als online-tabellen.  Het conflict wordt gemaakt wanneer `PullAsync()` heet in plaats van tijdens het invoegen, bijwerken of verwijderen. Als meerdere conflicten optreden, worden ze in een enkele MobileServicePushFailedException gebundeld.  Elke fout afzonderlijk afhandelen.

## <a name="#customapi"></a>Werken met een aangepaste API
Een aangepaste API kunt u aangepaste eindpunten die beschikbaar maken van server-functionaliteit die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API, kunt u hebben meer controle over berichten, zoals lezen en HTTP-bericht headers instellen en definiëren van een indeling van de hoofdtekst van bericht dan JSON.

U een aangepaste API oproept door het aanroepen van een van de [InvokeApiAsync] methoden op de client. De volgende regel code verzendt bijvoorbeeld een POST-aanvraag naar de **completeAll** API op de back-end:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Dit formulier is een getypeerde methodeaanroep en vereist dat de **MarkAllResult** retourneren type is gedefinieerd. Getypte en niet-getypeerde methoden worden ondersteund.

De methode InvokeApiAsync() voegt toe '/ api /' aan de API die u aanroepen wilt, tenzij de API met begint een slash (/).
Bijvoorbeeld:

* `InvokeApiAsync("completeAll",...)` /api/completeAll wordt op de back-end
* `InvokeApiAsync("/.auth/me",...)` /.auth/me wordt op de back-end

U kunt InvokeApiAsync gebruiken om aan te roepen een WebAPI, met inbegrip van deze WebAPIs die niet zijn gedefinieerd met Azure Mobile Apps.  Wanneer u InvokeApiAsync() gebruikt, worden de juiste headers, met inbegrip van verificatieheaders, verzonden met de aanvraag.

## <a name="authentication"></a>Verificatie van gebruikers
Mobile Apps biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in server-scripts. Zie de zelfstudie [Verificatie toevoegen aan uw app] voor meer informatie.

Twee verificatiestromen worden ondersteund: *client beheerd* en *server beheerd* stroom. De server beheerd stroom bevat de meest eenvoudige verificatie-ervaring, afhankelijk van de provider de webinterface voor verificatie. De stroom client beheerd kunt diepere integratie met de apparaat-specifieke mogelijkheden zoals deze is afhankelijk van de provider-specifieke apparaat-specifieke SDK's.

> [!NOTE]
> We raden het gebruik van een stroom softwareclient worden beheerd in uw productie-apps.

Als u verificatie instelt, moet u uw app registreren bij een of meer id-providers.  De id-provider wordt gegenereerd voor een client-ID en een clientgeheim voor uw app.  Deze waarden zijn wordt ingesteld in uw back-end op Azure App Service-verificatie/autorisatie inschakelen.  Voor meer informatie, volgt u de gedetailleerde instructies in de zelfstudie [Verificatie toevoegen aan uw app].

De volgende onderwerpen worden besproken in deze sectie:

* [Verificatie-client worden beheerd](#clientflow)
* [Beheerde server-verificatie](#serverflow)
* [Het verificatietoken opslaan in cache](#caching)

### <a name="clientflow"></a>Verificatie-client worden beheerd
Uw app kunt afzonderlijk contact opnemen met de id-provider en geef vervolgens het geretourneerde token tijdens het aanmelden met uw back-end. Met deze clientstroom kunt u voor een eenmalige aanmelding voor gebruikers of aanvullende gebruikersgegevens ophalen van de id-provider. Clientverificatie voor flow heeft de voorkeur voor het gebruik van een stroom server als de id-provider SDK een meer systeemeigen UX uiterlijk biedt en kunt u extra aanpassingen.

Voorbeelden zijn beschikbaar voor de volgende patronen van de client-flow-verificatie:

* [Active Directory-Verificatiebibliotheek](#adal)
* [Facebook of Google](#client-facebook)

#### <a name="adal"></a>Verificatie van gebruikers met de Active Directory-Verificatiebibliotheek
U kunt de Active Directory Authentication Library (ADAL) te initiëren gebruikersverificatie van de client met behulp van Azure Active Directory-verificatie gebruiken.

1. Uw mobiele app back-end voor AAD-aanmelding configureren door de [App Service configureren voor Active Directory-aanmelding] zelfstudie. Zorg ervoor dat u de optionele stap voor het registreren van een systeemeigen clienttoepassing.
2. Open uw project in Visual Studio of Xamarin Studio, en voeg een verwijzing naar de `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-pakket. Wanneer u zoekt, neemt u voorlopige versies.
3. Voeg de volgende code aan uw toepassing, op basis van het platform dat u gebruikt. In elk, moet u de volgende vervangingen:

   * Vervang **INSERT-instantie-HERE** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure Portal].
   * Vervang **INSERT-RESOURCE-ID-HERE** met de client-ID voor de back-end van uw mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
   * Vervang **INSERT-CLIENT-ID-HERE** met de client-ID die u hebt gekopieerd uit de toepassing native client.
   * Vervang **INSERT-OMLEIDINGS-URI-HERE** met van uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met *https://contoso.azurewebsites.net/.auth/login/done*.

     Hier volgt de code die nodig zijn voor elk platform:

     **Windows:**

    ```csharp
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

    ```csharp
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

    ```csharp
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

#### <a name="client-facebook"></a>Eenmalige aanmelding met een token van Facebook of Google
U kunt de stroom kunt gebruiken, zoals wordt weergegeven in dit fragment voor Facebook of Google.

```csharp
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

### <a name="serverflow"></a>Beheerde server-verificatie
Wanneer u uw id-provider hebt geregistreerd, aanroepen de [LoginAsync] methode voor het [MobileServiceClient] met de [MobileServiceAuthenticationProvider] waarde van de provider. Bijvoorbeeld, initieert de volgende code een server flow aanmelden via Facebook.

```csharp
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

Als u een id-provider dan Facebook gebruikt, wijzigt u de waarde van [MobileServiceAuthenticationProvider] naar de waarde voor de provider.

In een stroom server beheert Azure App Service de OAuth-verificatie-stroom door de aanmeldingspagina van de geselecteerde provider weer te geven.  Zodra de id-provider retourneert, Azure App Service genereert een App Service-verificatietoken. De [LoginAsync] methode retourneert een [MobileServiceUser], waarmee u zowel de [UserId] van de geverifieerde gebruiker en de [MobileServiceAuthenticationToken], als een JSON webtoken (JWT). Dit token kan worden opgeslagen in de cache en opnieuw worden gebruikt totdat het verloopt. Zie voor meer informatie, [Caching het verificatietoken](#caching).

### <a name="caching"></a>Het verificatietoken opslaan in cache
In sommige gevallen kan de aanroep naar de aanmeldingsmethode na de eerste authenticatie worden vermeden door op te slaan van het verificatietoken van de provider.  Microsoft Store- en UWP-apps kunt [PasswordVault] cache van het huidige verificatietoken na een geslaagde aanmelding, als volgt:

```csharp
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

De gebruikers-id-waarde wordt opgeslagen als de gebruikersnaam van de referentie en het token wordt de opgeslagen als het wachtwoord. Op de volgende startende bedrijven, kunt u controleren de **PasswordVault** voor referenties in de cache. Het volgende voorbeeld wordt in de cache opgeslagen referenties wanneer ze worden gevonden, en anders probeert opnieuw te verifiëren met de back-end:

```csharp
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

Wanneer u zich afmeldt een gebruiker, moet u de opgeslagen referenties, ook als volgt verwijderen:

```csharp
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin-apps gebruiken de [Xamarin.Auth] API's voor het veilig opslaan van referenties in een **Account** object. Zie voor een voorbeeld van het gebruik van deze API's, de [AuthStore.cs] codebestand in de [ContosoMoments foto's delen voorbeeld](https://github.com/azure-appservice-samples/ContosoMoments).

Wanneer u gebruikmaakt van verificatie softwareclient worden beheerd, kunt u ook de cache van het toegangstoken verkregen van de provider, zoals Facebook of Twitter. Dit token kan worden opgegeven voor het aanvragen van een nieuw verificatietoken van de back-end, als volgt:

```csharp
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Pushmeldingen verzenden
De volgende onderwerpen vindt u Pushmeldingen:

* [Registreren voor Pushmeldingen](#register-for-push)
* [Verkrijgen van een pakket-SID van de Microsoft Store](#package-sid)
* [Registreren met sjablonen voor meerdere platformen](#register-xplat)

### <a name="register-for-push"></a>Procedures: Registreren voor Pushmeldingen
De client met Mobile Apps kunt u zich registreren voor pushmeldingen kunt verzenden met Azure Notification Hubs. Wanneer u registreert, kunt u een ingang die u hebt verkregen van de platform-specifieke Push Notification Service (PNS) verkrijgen. Deze waarde samen met eventuele labels wordt vervolgens opgeven bij het maken van de registratie. De volgende code registreert uw Windows-app voor pushmeldingen met Windows Notification Service (WNS):

```csharp
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Als u voor WNS pusht, dan u moet [verkrijgen van een pakket-SID van de Microsoft Store](#package-sid).  Zie voor meer informatie over Windows-apps, inclusief het registreren voor de sjabloon-registraties [Pushmeldingen toevoegen aan uw app].

Aanvragen van tags van de client wordt niet ondersteund.  Tag-aanvragen zijn op de achtergrond verwijderd uit de inschrijving.
Als u wilt dat uw apparaat te registreren bij tags, maakt u een aangepaste API die gebruikmaakt van de Notification Hubs-API om uit te voeren van de registratie namens.  [De aangepaste API aanroepen](#customapi) in plaats van de `RegisterNativeAsync()` methode.

### <a name="package-sid"></a>Procedures: Verkrijgen van een pakket-SID van de Microsoft Store
Een pakket-SID is vereist voor het inschakelen van pushmeldingen in Microsoft Store-apps.  Voor het ontvangen van een pakket-SID, moet u uw toepassing registreren met de Microsoft Store.

Als u deze waarde:

1. In Visual Studio Solution Explorer met de rechtermuisknop op de Microsoft Store-app-project, klikt u op **Store** > **App aan de Store koppelen...** .
2. In de wizard, klikt u op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **een nieuwe appnaam reserveren**, klikt u vervolgens op **Reserve**.
3. Nadat de app-registratie is gemaakt, selecteert u de appnaam, klikt u op **volgende**, en klik vervolgens op **koppelen**.
4. Meld u aan bij de [Windows-ontwikkelaarscentrum] met uw Microsoft-Account. Onder **mijn apps**, klikt u op de appregistratie van de die u hebt gemaakt.
5. Klik op **appbeheer** > **identiteit App**, en schuif vervolgens omlaag naar zoeken naar uw **pakket-SID**.

Vele toepassingen van de pakket-SID behandelen als een URI, in welk geval u wilt gebruiken *ms-app: / /* als het schema. Noteer de versie van uw pakket-SID gevormd door het samenvoegen van deze waarde als een voorvoegsel.

Xamarin-apps vereisen aanvullende code om te kunnen registreren van een app die wordt uitgevoerd op het iOS- of Android-platformen. Zie voor meer informatie het onderwerp voor uw platform:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Procedures: Push-sjablonen voor het verzenden van platformonafhankelijke meldingen registreren
Gebruik voor het registreren van sjablonen, de `RegisterAsync()` methode met de sjablonen kunt als volgt te werk:

```csharp
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Uw sjablonen moet `JObject` van het type en kan meerdere sjablonen in de volgende JSON-indeling bevatten:

```csharp
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

```csharp
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Alle tags worden onmiddellijk verwijderd tijdens de registratie voor beveiliging. Tags toevoegen aan installaties of -sjablonen in installaties, Zie [werken met de .NET back-endserver SDK voor Azure Mobile Apps].

Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, raadpleegt u de [Notification Hubs-API 's].

## <a name="misc"></a>Diverse onderwerpen
### <a name="errors"></a>Procedures: Fouten verwerken
Wanneer er een fout optreedt in de back-end, de client SDK retourneert een `MobileServiceInvalidOperationException`.  Het volgende voorbeeld laat zien hoe voor het afhandelen van een uitzondering die is geretourneerd door de back-end:

```csharp
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

Een ander voorbeeld van het afhandelen van fouten kunt u vinden in de [Voorbeeld van mobiele Apps-bestanden]. De [LoggingHandler] voorbeeld biedt een handler voor het delegeren van logboekregistratie om de aanvragen worden ingediend bij de back-end te registreren.

### <a name="headers"></a>Procedures: Aanvraagheaders aanpassen
Ter ondersteuning van uw specifieke app-scenario, moet u mogelijk communicatie met de back-end van Mobile App aanpassen. U wilt bijvoorbeeld een aangepaste header toevoegen aan elke uitgaande aanvraag of antwoord-statuscodes zelfs wijzigen. U kunt een aangepaste [DelegatingHandler], zoals in het volgende voorbeeld:

```csharp
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
[4]: https://msdn.microsoft.com/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: https://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: https://github.com/SymbolSource/SymbolSource
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Verificatie toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Pushmeldingen toevoegen aan uw app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Register your app to use a Microsoft account login]: ../app-service/configure-authentication-provider-microsoft.md
[App Service configureren voor Active Directory-aanmelding]: ../app-service/configure-authentication-provider-aad.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/library/azure/jj554275(v=azure.10).aspx
[Hiermee maakt u een verwijzing naar een niet-getypeerde tabel]: https://msdn.microsoft.com/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/library/azure/jj871654(v=azure.10).aspx
[Sorteren op]: https://msdn.microsoft.com/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/library/azure/mt691741(v=azure.10).aspx
[nemen]: https://msdn.microsoft.com/library/azure/dn250574(v=azure.10).aspx
[Selecteren]: https://msdn.microsoft.com/library/azure/dn250569(v=azure.10).aspx
[Overslaan]: https://msdn.microsoft.com/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/library/azure/dn250536.(v=azure.10)aspx
[UserID]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[waar]: https://msdn.microsoft.com/library/azure/dn250579(v=azure.10).aspx
[Azure Portal]: https://portal.azure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: https://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows-ontwikkelaarscentrum]: https://dev.windows.com/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[PasswordVault]: https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: https://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Notification Hubs-API 's]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Voorbeeld van mobiele Apps-bestanden]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3-documentatie]: https://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: https://www.telerik.com/fiddler
[Json.NET]: https://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
