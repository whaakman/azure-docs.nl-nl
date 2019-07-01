---
title: C#Zelfstudie voor het aanroepen van Cognitive Services API's in een indexering pijplijn - Azure Search
description: In deze zelfstudie ziet u een voorbeeld van gegevensextractie, natuurlijke taal en AI-beeldverwerking in Azure Search indexeren voor gegevensextractie en transformatie.
manager: eladz
author: MarkHeff
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: maheff
ms.openlocfilehash: 7fc33f89797fe0e3cc3a9832c9dc084194cf6603
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485752"
---
# <a name="c-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>C#Zelfstudie: Cognitive Services API's aanroepen in een Azure Search indexeren van pijplijn

In deze zelfstudie leert u de mechanismen achter gegevensverrijking programmeren in Azure Search met behulp van *cognitieve vaardigheden*. Vaardigheden worden ondersteund door natuurlijke taalverwerking (NLP) en de installatiekopie van analysemogelijkheden in Cognitive Services. U kunt via de samenstelling van vaardigheden en configuratie, tekst en de vorm van een afbeelding of gescande documentbestand tekst extraheren. Ook kunt u de taal, entiteiten en sleuteltermen detecteren. Het eindresultaat is uitgebreide aanvullende inhoud in een Azure Search-index, die zijn gemaakt door een AI aangestuurde indexing-pijplijn.

In deze zelfstudie, kunt u de .NET SDK gebruiken om uit te voeren van de volgende taken:

> [!div class="checklist"]
> * Een indexeringspijplijn maken die voorbeeldgegevens onderweg naar een index verrijkt
> * Ingebouwde vaardigheden toepassen: optische tekenherkenning, tekst samenvoegen, taaldetectie, tekst splitsen, entiteiten herkennen, sleuteltermextractie
> * Vaardigheden aan elkaar koppelen door invoeren aan uitvoeren toe te wijzen in een set vaardigheden
> * Aanvragen uitvoeren en resultaten bekijken
> * De index en indexeerfuncties opnieuw instellen voor verdere ontwikkeling

Uitvoer is een volledige doorzoekbare index in Azure Search. U kunt de index uitbreiden met andere standaardfuncties, zoals [synoniemen](search-synonyms.md), [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [analyses](search-analyzers.md) en [filters](search-filters.md).

In deze zelfstudie wordt uitgevoerd op de gratis service, maar het aantal gratis transacties is beperkt tot 20 documenten per dag. Als u wilt meer dan eenmaal in deze zelfstudie wordt uitgevoerd in dezelfde dag, gebruikt u een kleiner bestand instellen, zodat u in meer uitvoeringen past.

> [!NOTE]
> Als u een bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of meer AI-algoritmen toe te voegen, moet u een factureerbare Cognitive Services-resource koppelen. Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/) . Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogramma's en gegevens worden gebruikt in deze zelfstudie. 

+ [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van de voorbeeldgegevens. Zorg ervoor dat het opslagaccount zich in dezelfde regio als de Azure Search.

+ [Voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) bestaat uit een verzameling kleine bestanden van verschillende typen. 

+ [Visual Studio installeren](https://visualstudio.microsoft.com/) om te gebruiken als de IDE.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

Om te communiceren met uw Azure Search-service moet u de service-URL en een toegangssleutel. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

   ![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden

De verrijkingspijplijn haalt gegevens uit Azure-gegevensbronnen. Brongegevens moeten afkomstig zijn van een ondersteund type gegevensbron van een [Azure Search-indexeerfunctie](search-indexer-overview.md). Azure Table Storage wordt niet ondersteund voor cognitief zoeken. In dit voorbeeld gebruiken we blobopslag om meerdere inhoudstypen te laten zien.

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) voorbeeldgegevens bevatten. U kunt het niveau van de openbare toegang instellen op een van de geldige waarden. In deze zelfstudie wordt ervan uitgegaan dat de containernaam van de 'basic-demo-gegevens-pr'.

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk om te uploaden de [voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4).

   ![Bronbestanden in Azure-blobopslag](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Nadat de voorbeeldbestanden zijn geladen, haalt u de containernaam en een verbindingsreeks voor de Blob-opslag op. U kunt dat doen door te navigeren naar uw opslagaccount in Azure portal, selecteren **toegangssleutels**, en kopieer vervolgens de **Connection String** veld.

   De verbindingsreeks moet een URL zijn die er ongeveer als volgt uitziet:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Er zijn andere manieren om de verbindingsreeks op te geven, zoals een Shared Access Signature bieden. Zie [Indexing Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials) (Azure Blob Storage indexeren) voor meer informatie over referenties voor gegevensbronnen.

## <a name="set-up-your-environment"></a>Uw omgeving instellen

Beginnen met Visual Studio te openen en een nieuwe Console-App-project maken dat kan worden uitgevoerd op .NET Core.

### <a name="install-nuget-packages"></a>NuGet-pakketten installeren

De [Azure Search .NET SDK](https://aka.ms/search-sdk) bestaat uit een aantal clientbibliotheken waarmee u voor het beheren van uw indexen, gegevensbronnen, Indexeerfuncties en kennis en vaardigheden, ook als uploaden en beheren van documenten en het uitvoeren van query's, allemaal zonder dat u hoeft te bekommeren om de de details van HTTP en JSON. Deze clientbibliotheken worden gedistribueerd als een NuGet-pakketten.

Voor dit project, moet u voor het installeren van versie 9 van de `Microsoft.Azure.Search` NuGet-pakket en de meest recente `Microsoft.Extensions.Configuration.Json` NuGet-pakket.

Installeer de `Microsoft.Azure.Search` NuGet-pakket met behulp van de Package Manager console in Visual Studio. Openen van de Package Manager console Selecteer **extra** > **NuGet Package Manager** > **Package Manager Console**. Als u de opdracht uit te voeren, gaat u naar de [Microsoft.Azure.Search NuGet-pakketpagina](https://www.nuget.org/packages/Microsoft.Azure.Search)versie 9, en selecteer de Package Manager-opdracht kopiëren. In de Package Manager-console, moet u deze opdracht uitvoeren.

Voor het installeren van de `Microsoft.Extensions.Configuration.Json` NuGet-pakket in Visual Studio, selecteer **extra** > **NuGet Package Manager** > **NuGet-pakketten beheren voor oplossing...** . Selecteer Bladeren en zoeken naar de `Microsoft.Extensions.Configuration.Json` NuGet-pakket. Als u deze hebt gevonden, selecteert u het pakket, selecteert u uw project, Controleer of dat de versie is de laatste stabiele versie, en selecteer vervolgens installeren.

## <a name="add-azure-search-service-information"></a>Informatie over Azure Search-service toevoegen

Als u wilt verbinding maken met uw Azure Search-service moet u de informatie over de zoekservice toevoegen aan uw project. Klik met de rechtermuisknop op uw project in Solution Explorer en selecteer **toevoegen** > **Nieuw Item...**  . Noem het bestand `appsettings.json` en selecteer **toevoegen**. 

Dit bestand moet worden opgenomen in uw map met de uitvoer. Om dit te doen, klik met de rechtermuisknop op `appsettings.json` en selecteer **eigenschappen**. Wijzig de waarde van **naar uitvoermap kopiëren** naar **kopie van de nieuwere**.

Kopieer de onderstaande JSON in uw nieuwe JSON-bestand.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "SearchServiceQueryApiKey": "Put your query API key here",
  "AzureBlobConnectionString": "Put your Azure Blob connection string here",
}
```

Search-service en blob storage gegevens over uw account toevoegen.

U kunt de informatie over de zoekservice ophalen vanuit jouw accountpagina zoeken in Azure portal. De accountnaam is op de hoofdpagina en de sleutels kunnen worden gevonden door te selecteren **sleutels**.

U kunt de blob-verbindingsreeks ophalen door te navigeren naar uw opslagaccount in Azure portal, selecteren **toegangssleutels**, en vervolgens te kopiëren de **Connection String** veld.

## <a name="add-namespaces"></a>Naamruimten toevoegen

In deze zelfstudie maakt gebruik van veel verschillende typen in verschillende naamruimten. Als u wilt gebruiken die typen het volgende toevoegen aan `Program.cs`.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;
```

## <a name="create-a-client"></a>Een client maken

Maak een instantie van de `SearchServiceClient` klasse.

```csharp
IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
IConfigurationRoot configuration = builder.Build();
SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);
```

`CreateSearchServiceClient` maakt u een nieuwe `SearchServiceClient` met waarden die zijn opgeslagen in het configuratiebestand van de toepassing (appsettings.json).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string adminApiKey = configuration["SearchServiceAdminApiKey"];

   SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
   return serviceClient;
}
```

> [!NOTE]
> De klasse `SearchServiceClient` beheert verbindingen met uw zoekservice. Als u wilt voorkomen dat er te veel verbindingen worden geopend, kunt u proberen één instantie van `SearchServiceClient` in uw toepassing te delen, indien mogelijk. De bijbehorende methoden zijn thread-safe om delen in te schakelen.
> 
> 

## <a name="create-a-data-source"></a>Een gegevensbron maken

Maak een nieuwe `DataSource` exemplaar door het aanroepen van `DataSource.AzureBlobStorage`. `DataSource.AzureBlobStorage` vereist dat u de naam van de gegevensbron, verbindingsreeks en de naam van de blob-container opgeven.

Hoewel het niet wordt gebruikt in deze zelfstudie wordt ook een beleid voor voorlopig verwijderen gedefinieerd dat wordt gebruikt voor het identificeren van verwijderde blobs op basis van de waarde van een kolom voor voorlopig verwijderen. Het volgende beleid rekening gehouden met een blob moet worden verwijderd als er een eigenschap metadata `IsDeleted` met de waarde `true`.

```csharp
DataSource dataSource = DataSource.AzureBlobStorage(
    name: "demodata",
    storageConnectionString: configuration["AzureBlobConnectionString"],
    containerName: "basic-demo-data-pr",
    deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
        softDeleteColumnName: "IsDeleted",
        softDeleteMarkerValue: "true"),
    description: "Demo files to demonstrate cognitive search capabilities.");
```

Nu dat u hebt geïnitialiseerd de `DataSource` object, het maken van de gegevensbron. `SearchServiceClient` heeft een `DataSources`-eigenschap. Deze eigenschap bevat alle methoden die u maken wilt, weergeven, bijwerken of verwijderen van Azure Search-gegevensbronnen.

Voor een aanvraag is gelukt retourneert de methode voor de gegevensbron die is gemaakt. Als er een probleem met de aanvraag, zoals een ongeldige parameter, genereert de methode een uitzondering.

```csharp
try
{
    serviceClient.DataSources.CreateOrUpdate(dataSource);
}
catch (Exception e)
{
    // Handle the exception
}
```

Aangezien dit uw eerste aanvraag is, controleert u Azure Portal om te bevestigen dat de gegevensbron in Azure Search is gemaakt. Controleer op de dashboardpagina van de zoekservice of de tegel Gegevensbronnen een nieuw item heeft. U moet mogelijk een paar minuten wachten tot de portalpagina is vernieuwd.

  ![Tegel Gegevensbronnen in de portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Tegel Gegevensbronnen in de portal")

## <a name="create-a-skillset"></a>Een set vaardigheden maken

In deze sectie maakt definiëren u een reeks verrijking stappen die u wilt toepassen op uw gegevens. Elke stap verrijking heet een *vaardigheid* en de set van verrijking stappen een *vaardigheden*. In deze zelfstudie wordt gebruikgemaakt van [vooraf gedefinieerde cognitieve vaardigheden](cognitive-search-predefined-skills.md) voor de set vaardigheden:

+ [Optische tekenherkenning](cognitive-search-skill-ocr.md) gedrukte en handgeschreven tekst in afbeeldingen herkennen.

+ [Tekst samenvoegen](cognitive-search-skill-textmerger.md) tekst uit een verzameling van velden in één veld consolideren.

+ [Taaldetectie](cognitive-search-skill-language-detection.md) om de taal van de inhoud vast te stellen.

+ [Tekst splitsen](cognitive-search-skill-textsplit.md) grote inhoud opsplitsen in kleinere chunks voordat u de kwalificatie van de extractie sleuteluitdrukkingen en de entiteit opname-kwalificatie. Extractie van cruciale frasen en herkenning entiteit accepteren invoer van 50.000 tekens of minder. Enkele voorbeeldbestanden moeten worden opgesplitst om aan deze limiet te voldoen.

+ [Herkenning van entiteit](cognitive-search-skill-entity-recognition.md) voor het uitpakken van de namen van organisaties van de inhoud in de blob-container.

+ [Sleuteltermextractie](cognitive-search-skill-keyphrases.md) voor het ophalen van de belangrijkste sleuteltermen.

Tijdens de verwerking van initiële zorgt Azure Search elk document kunnen inhoud in verschillende bestandsindelingen lezen. Gevonden tekst uit het bronbestand wordt in een gegenereerd ```content```-veld geplaatst, één voor elk document. Als zodanig ingesteld als invoer voor ```"/document/content"``` gebruik van deze tekst. 

Uitvoeren kunnen aan een index worden toegewezen, als invoer worden gebruikt voor een downstream-vaardigheid, of beide zoals bij taalcode. Een taalcode is in de index handig om te filteren. Taalcode wordt als invoer door vaardigheden voor tekstanalyse gebruikt om de taalregels voor woordafbreking in te stellen.

Zie [How to define a skillset](cognitive-search-defining-skillset.md) (Een set vaardigheden definiëren) voor meer informatie over de grondbeginselen van vaardigheden.

### <a name="ocr-skill"></a>OCR-kwalificatie

De **OCR** vaardigheid haalt tekst uit afbeeldingen. Deze kwalificatie wordt ervan uitgegaan dat een veld normalized_images bestaat. Voor het genereren van dit veld, verderop in de zelfstudie we stellen de ```"imageAction"``` configuratie in de definitie van de indexeerfunctie te ```"generateNormalizedImages"```.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "image",
    source: "/document/normalized_images/*"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "text",
    targetName: "text"));

OcrSkill ocrSkill = new OcrSkill(
    description: "Extract text (plain and structured) from image",
    context: "/document/normalized_images/*",
    inputs: inputMappings,
    outputs: outputMappings,
    defaultLanguageCode: OcrSkillLanguage.En,
    shouldDetectOrientation: true);
```

### <a name="merge-skill"></a>Samenvoegen van vaardigheden

In deze sectie maakt u een **samenvoegen** dat het veld voor de document-inhoud met de tekst die is geproduceerd door de OCR-kwalificatie worden samengevoegd.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/content"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "itemsToInsert",
    source: "/document/normalized_images/*/text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "offsets",
    source: "/document/normalized_images/*/contentOffset"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "mergedText",
    targetName: "merged_text"));

MergeSkill mergeSkill = new MergeSkill(
    description: "Create merged_text which includes all the textual representation of each image inserted at the right location in the content field.",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    insertPreTag: " ",
    insertPostTag: " ");
```

### <a name="language-detection-skill"></a>Taal detecteren-kwalificatie

De **taaldetectie** vaardigheid detecteert de taal van de invoertekst en rapporten van een enkele taalcode voor elk document dat is ingediend bij de aanvraag. We gebruiken de uitvoer van de **taaldetectie** vaardigheden als onderdeel van de invoer voor de **tekst splitsen** kwalificatie.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "languageCode",
    targetName: "languageCode"));

LanguageDetectionSkill languageDetectionSkill = new LanguageDetectionSkill(
    description: "Detect the language used in the document",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="text-split-skill"></a>Tekst splitsen vaardigheid

De onderstaande **splitsen** vaardigheid wordt tekst splitsen op pagina's en beperkingen voor pagina 4000 tekens bevatten wordt gemeten door `String.Length`. Het algoritme probeert op te splitsen van de tekst in stukken verdeeld die maximaal `maximumPageLength` in grootte. In dit geval het algoritme voor het beste doen verbreken van de zin op een grens van een zin, zodat de grootte van het segment mogelijk iets minder dan `maximumPageLength`.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "textItems",
    targetName: "pages"));

SplitSkill splitSkill = new SplitSkill(
    description: "Split content into pages",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    textSplitMode: TextSplitMode.Pages,
    maximumPageLength: 4000);
```

### <a name="entity-recognition-skill"></a>Entiteit erkenning vaardigheid

Dit `EntityRecognitionSkill` instantie is ingesteld voor het herkennen van categorietype `organization`. De **entiteit erkenning** vaardigheid herkent ook categorietypen `person` en `location`.

U ziet dat het veld 'context' is ingesteld op ```"/document/pages/*"``` met een sterretje, wat betekent dat de stap verrijking wordt aangeroepen voor elke pagina onder ```"/document/pages"```.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
    
List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "organizations",
    targetName: "organizations"));

List<EntityCategory> entityCategory = new List<EntityCategory>();
entityCategory.Add(EntityCategory.Organization);
    
EntityRecognitionSkill entityRecognitionSkill = new EntityRecognitionSkill(
    description: "Recognize organizations",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings,
    categories: entityCategory,
    defaultLanguageCode: EntityRecognitionSkillLanguage.En);
```

### <a name="key-phrase-extraction-skill"></a>Sleuteluitdrukkingen extraheren vaardigheid

Net als de `EntityRecognitionSkill` -exemplaar dat zojuist is gemaakt, de **sleutel vindt er sleuteltermextractie plaats** kwalificatie voor elke pagina van het document wordt genoemd.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "keyPhrases",
    targetName: "keyPhrases"));

KeyPhraseExtractionSkill keyPhraseExtractionSkill = new KeyPhraseExtractionSkill(
    description: "Extract the key phrases",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="build-and-create-the-skillset"></a>Maken van de vaardigheden

Bouw de `Skillset` met behulp van de vaardigheden die u hebt gemaakt.

```csharp
List<Skill> skills = new List<Skill>();
skills.Add(ocrSkill);
skills.Add(mergeSkill);
skills.Add(languageDetectionSkill);
skills.Add(splitSkill);
skills.Add(entityRecognitionSkill);
skills.Add(keyPhraseExtractionSkill);

Skillset skillset = new Skillset(
    name: "demoskillset",
    description: "Demo skillset",
    skills: skills);
```

Maak de vaardigheden in uw search-service.

```csharp
try
{
    serviceClient.Skillsets.CreateOrUpdate(skillset);
}
catch (Exception e)
{
    // Handle exception
}
```

## <a name="create-an-index"></a>Een index maken

In dit gedeelte kunt u het indexschema definiëren door op te geven welke velden in de doorzoekbare index moeten worden opgenomen en de zoekkenmerken voor elk veld op te geven. Velden hebben een type en kunnen kenmerken opnemen die bepalen hoe het veld wordt gebruikt (doorzoekbaar, sorteerbaar enzovoort). Veldnamen in een index hoeven niet identiek te zijn aan de veldnamen in de bron. In een latere stap voegt u veldverwijzingen in een indexeerfunctie toe om bron-doelvelden te verbinden. Definieer voor deze stap de index met behulp van veldnaamconventies die relevant zijn voor uw zoektoepassing.

In deze oefening worden de volgende velden en veldtypen gebruikt:

| field-names: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="create-demoindex-class"></a>DemoIndex klasse maken

De velden voor deze index worden gedefinieerd met behulp van een modelklasse. Elke eigenschap van de modelklasse heeft kenmerken die het zoekgerelateerde gedrag van het bijbehorende indexveld bepalen. 

Gaan we de modelklasse toevoegen aan een nieuwe C# bestand. Klik met de rechtermuisknop op uw project en selecteer **toevoegen** > **Nieuw Item...** , selecteer "Class" en noem het bestand `DemoIndex.cs`en selecteer vervolgens **toevoegen**.

Zorg ervoor dat om aan te geven dat u wilt gebruiken vanaf de `Microsoft.Azure.Search` en `Microsoft.Azure.Search.Models` naamruimten.

```csharp
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
```

Voeg de onder de klassedefinitie model te `DemoIndex.cs` en deze opnemen in dezelfde naamruimte waar u de index maken.

```csharp
// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public class DemoIndex
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsSearchable, IsSortable]
    public string Id { get; set; }

    [IsSearchable]
    public string Content { get; set; }

    [IsSearchable]
    public string LanguageCode { get; set; }

    [IsSearchable]
    public string[] KeyPhrases { get; set; }

    [IsSearchable]
    public string[] Organizations { get; set; }
}
```

Nu dat u een modelklasse hebt gedefinieerd, back-ups maken `Program.cs` kunt u vrij eenvoudig een indexdefinitie maken. De naam voor de index is 'demoindex'.

```csharp
var index = new Index()
{
    Name = "demoindex",
    Fields = FieldBuilder.BuildForType<DemoIndex>()
};
```

Tijdens het testen merkt u misschien dat u probeert te maken van de index meer dan één keer. Als gevolg hiervan, controleert u of de index die u gaat maken al bestaat voordat wordt geprobeerd om deze te maken.

```csharp
try
{
    bool exists = serviceClient.Indexes.Exists(index.Name);

    if (exists)
    {
        serviceClient.Indexes.Delete(index.Name);
    }

    serviceClient.Indexes.Create(index);
}
catch (Exception e)
{
    // Handle exception
}
```

Zie [Create Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index maken (Azure Search REST API)) voor meer informatie over het definiëren van een index.

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Een indexeerfunctie maken, velden toewijzen en transformaties uitvoeren

Tot nu toe hebt u een gegevensbron, een set vaardigheden en een index gemaakt. Deze drie onderdelen gaan deel uitmaken van een [indexeerfunctie](search-indexer-overview.md) die de onderdelen combineert in één meervoudige bewerking. Als u deze in een indexeerfunctie wilt combineren, moet u veldtoewijzingen definiëren.

+ De fieldMappings worden verwerkt voordat de vaardigheden, toewijzing van de bronvelden uit de gegevensbron naar de doelvelden in een index. Als de veldnamen en typen identiek aan beide uiteinden zijn, zijn er is geen toewijzing is vereist.

+ De outputFieldMappings worden verwerkt na de vaardigheden, verwijst naar een sourceFieldNames die niet tot documenten kraken of verrijking gemaakt. De targetFieldName is een veld in een index.

Naast het installeren van de invoer voor uitvoer, kunt u ook veldtoewijzingen gebruiken om af te vlakken gegevensstructuren. Zie voor meer informatie, [verrijkt velden toewijzen aan een doorzoekbare index](cognitive-search-output-field-mapping.md).

```csharp
IDictionary<string, object> config = new Dictionary<string, object>();
config.Add(
    key: "dataToExtract",
    value: "contentAndMetadata");
config.Add(
    key: "imageAction",
    value: "generateNormalizedImages");

List<FieldMapping> fieldMappings = new List<FieldMapping>();
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "metadata_storage_path",
    targetFieldName: "id",
    mappingFunction: new FieldMappingFunction(
        name: "base64Encode")));
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "content",
    targetFieldName: "content"));

List<FieldMapping> outputMappings = new List<FieldMapping>();
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/organizations/*",
    targetFieldName: "organizations"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/keyPhrases/*",
    targetFieldName: "keyPhrases"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/languageCode",
    targetFieldName: "languageCode"));

Indexer indexer = new Indexer(
    name: "demoindexer",
    dataSourceName: dataSource.Name,
    targetIndexName: index.Name,
    description: "Demo Indexer",
    skillsetName: skillSet.Name,
    parameters: new IndexingParameters(
        maxFailedItems: -1,
        maxFailedItemsPerBatch: -1,
        configuration: config),
    fieldMappings: fieldMappings,
    outputFieldMappings: outputMappings);

try
{
    bool exists = serviceClient.Indexers.Exists(indexer.Name);

    if (exists)
    {
        serviceClient.Indexers.Delete(indexer.Name);
    }

    serviceClient.Indexers.Create(indexer);
}
catch (Exception e)
{
    // Handle exception
}
```

Verwacht dat het maken van de indexeerfunctie duurt even de tijd om te voltooien. De gegevensset is klein, maar analytische vaardigheden zijn berekeningsintensief. Sommige vaardigheden, zoals afbeeldingsanalyse, zijn langlopend.

> [!TIP]
> Het maken van een indexeerfunctie roept de pijplijn aan. Als er problemen zijn met het bereiken van de gegevens, het toewijzen van invoeren en uitvoeren of de volgorde van bewerkingen, worden ze in deze fase weergegeven.

### <a name="explore-creating-the-indexer"></a>Het maken van de indexeerfunctie verkennen

De code stelt ```"maxFailedItems"``` op-1, geeft dit de indexing-engine worden fouten genegeerd tijdens het importeren van gegevens. Dit is nuttig omdat de demo-gegevensbron maar een paar documenten bevat. Voor een grotere gegevensbron zou u de waarde op groter dan 0 instellen.

Ook ziet u de ```"dataToExtract"``` is ingesteld op ```"contentAndMetadata"```. Deze instructie geeft de indexeerfunctie de opdracht om automatisch de inhoud van verschillende bestandsindelingen te extraheren, evenals metagegevens voor elk bestand.

Wanneer inhoud wordt uitgepakt, kunt u instellen dat `imageAction` tekst ophaalt uit afbeeldingen die in de gegevensbron worden gevonden. De ```"imageAction"``` ingesteld op ```"generateNormalizedImages"``` configuratie, gecombineerd met de OCR-vaardigheden en tekst samenvoegen vaardigheid vertelt de indexeerfunctie tekst ophalen uit de afbeeldingen (bijvoorbeeld het woord 'stop' van een verkeer Stop-teken) en dit als onderdeel van het veld inhoud insluiten. Dit gedrag is van toepassing op zowel de afbeeldingen die zijn ingesloten in de documenten (zoals een afbeelding in een pdf-bestand) als afbeeldingen die zijn gevonden in de gegevensbron, zoals een JPG-bestand.

## <a name="check-indexer-status"></a>De status van de indexeerfunctie controleren

Nadat de indexeerfunctie is gedefinieerd, wordt deze automatisch uitgevoerd wanneer u de aanvraag verzendt. Afhankelijk van welke cognitieve vaardigheden u hebt gedefinieerd, kan het indexeren langer duren dan verwacht. Als u wilt weten of de indexeerfunctie nog steeds wordt uitgevoerd, gebruikt u de `GetStatus` methode.

```csharp
try
{
    IndexerExecutionInfo demoIndexerExecutionInfo = serviceClient.Indexers.GetStatus(indexer.Name);

    switch (demoIndexerExecutionInfo.Status)
    {
        case IndexerStatus.Error:
            Console.WriteLine("Indexer has error status");
            break;
        case IndexerStatus.Running:
            Console.WriteLine("Indexer is running");
            break;
        case IndexerStatus.Unknown:
            Console.WriteLine("Indexer status is unknown");
            break;
        default:
            Console.WriteLine("No indexer information");
            break;
    }
}
catch (Exception e)
{
    // Handle exception
}
```

`IndexerExecutionInfo` Hiermee geeft u de huidige status en de uitvoering van de geschiedenis van een indexeerfunctie.

Waarschuwingen komen veel voor bij sommige combinaties van bronbestand en vaardigheid en wijzen niet altijd op een probleem. In deze zelfstudie zijn de waarschuwingen onschadelijk (bijvoorbeeld geen tekstinvoeren van de JPEG-bestanden).
 
## <a name="query-your-index"></a>Een query uitvoeren in uw index

Nadat de indexeren is voltooid, kunt u query's die de inhoud van afzonderlijke velden retourneren kunt uitvoeren. Standaard retourneert Azure Search de 50 hoogste resultaten. De voorbeeldgegevens zijn beperkt, dus de standaardinstelling werkt prima. Als u echter werkt met grotere gegevenssets, moet u mogelijk parameters opnemen in de queryreeks om meer resultaten te retourneren. Zie [How to page results in Azure Search](search-pagination-page-layout.md) (Resultaten genereren in Azure Search) voor instructies.

Voer als verificatiestap query's uit op de index voor alle velden.

```csharp
DocumentSearchResult<DemoIndex> results;

ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*");
}
catch (Exception e)
{
    // Handle exception
}
```

`CreateSearchIndexClient` maakt u een nieuwe `SearchIndexClient` met waarden die zijn opgeslagen in het configuratiebestand van de toepassing (appsettings.json). Houd er rekening mee dat de search service API-query-sleutel wordt gebruikt en niet de Administrator-code.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

De uitvoer is het schema van de index met de naam, het type en de kenmerken van elk veld.

Verzend een tweede query voor `"*"` om alle inhoud te retourneren van één veld, zoals `organizations`.

```csharp
SearchParameters parameters =
    new SearchParameters
    {
        Select = new[] { "organizations" }
    };

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*", parameters);
}
catch (Exception e)
{
    // Handle exception
}
```

Herhaal dit voor aanvullende velden: inhoud, languageCode Sleutelzinnen en organisaties in deze oefening. U kunt meerdere velden retourneren via `$select` met behulp van een door komma's gescheiden lijst.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Opnieuw instellen en uitvoeren

In de eerste experimentele fasen van de ontwikkeling is de meest praktische aanpak voor ontwerp iteraties verwijderen van de objecten uit Azure Search en ervoor zorgen dat uw code opnieuw opbouwen. Resourcenamen zijn uniek. Na het verwijderen van een object kunt u het opnieuw maken met dezelfde naam.

In deze zelfstudie zorgt duurde controleren op bestaande Indexeerfuncties en indexen en deze worden verwijderd als ze al bestond, zodat u uw code opnieuw kunt uitvoeren.

U kunt de portal ook gebruiken om te verwijderen van indexen, Indexeerfuncties en kennis en vaardigheden.

Naarmate uw code meer vormt krijgt, is het raadzaam om uw herbouwstrategie te verfijnen. Zie [How to rebuild an index](search-howto-reindex.md) (Een index herbouwen) voor meer informatie.

## <a name="takeaways"></a>Opgedane kennis

De basisstappen voor het bouwen van een verrijkt indexeren pijplijn het maken van onderdelen in deze zelfstudie wordt gedemonstreerd: een gegevensbron, vaardigheden, index en indexeerfunctie.

[Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) zijn geïntroduceerd, samen met de definitie van de set vaardigheden en het mechanisme voor het koppelen van vaardigheden via in- en uitvoeren. U hebt ook geleerd dat `outputFieldMappings` vereist is in de definitie van de indexeerfunctie voor de routering van verrijkte waarden uit de pijplijn naar een doorzoekbare index op een Azure Search-service.

Tot slot hebt u geleerd hoe u resultaten kunt testen en het systeem opnieuw kunt instellen voor verdere iteraties. U hebt geleerd dat het uitvoeren van query's op de index de uitvoer retourneert die is gemaakt door de pijplijn voor verrijkte indexering. U hebt ook geleerd hoe u de status van de indexeerfunctie kunt controleren en welke objecten u moet verwijderen voordat u een pijplijn opnieuw uitvoert.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is om de resourcegroep met de Azure Search-service en Azure Blob service te verwijderen. Ervan uitgaande dat u beide services in dezelfde groep hebt geplaatst, verwijdert u de resourcegroep om alle inhoud ervan permanent te verwijderen, waaronder de services en alle opgeslagen inhoud die u voor deze zelfstudie hebt gemaakt. De naam van de resourcegroep staat in de portal op de pagina Overzicht van elke service.

## <a name="next-steps"></a>Volgende stappen

De pijplijn uitbreiden of aanpassen met aangepaste vaardigheden. Door een aangepaste vaardigheid te maken en deze toe te voegen aan een set vaardigheden, kunt u zelfgeschreven tekst- of afbeeldingsanalyse integreren.

> [!div class="nextstepaction"]
> [Voorbeeld: een aangepaste vaardigheid maken](cognitive-search-create-custom-skill-example.md)
