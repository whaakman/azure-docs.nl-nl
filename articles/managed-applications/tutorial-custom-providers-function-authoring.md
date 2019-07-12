---
title: Het ontwerpen van een RESTful-eindpunt voor aangepaste providers
description: In deze zelfstudie gaat over het ontwerpen van een RESTful-eindpunt voor aangepaste providers. Deze gaat informatie over het verwerken van aanvragen en antwoorden voor de ondersteunde RESTful HTTP-methoden.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 176e3b02cbda7577e306d86363cfe5b41335fb6e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800034"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Het ontwerpen van een RESTful-eindpunt voor aangepaste providers

Aangepaste providers kunnen u werkstromen in Azure aanpassen. Een aangepaste provider is een overeenkomst tussen Azure en een `endpoint`. In deze zelfstudie doorloopt het proces van het ontwerpen van een aangepaste provider RESTful `endpoint`. Als u niet bekend met aangepaste Providers van Azure bent, Zie [het overzicht van aangepaste resourceproviders](./custom-providers-overview.md).

In deze zelfstudie is onderverdeeld in de volgende stappen uit:

- Werken met aangepaste acties en aangepaste resources
- Aangepaste resources in de opslag partitioneren
- Ondersteuning voor aangepaste provider RESTful-methoden
- RESTful operations integreren

In deze zelfstudie bouwt op de volgende zelfstudies:

- [Azure Functions instellen voor Azure aangepaste Providers](./tutorial-custom-providers-function-setup.md)

> [!NOTE]
> Deze zelfstudie bouwt voort uit de vorige zelfstudie. Enkele van de stappen in deze zelfstudie werkt alleen als een Azure-functie is ingesteld om te werken met aangepaste providers.

## <a name="working-with-custom-actions-and-custom-resources"></a>Werken met aangepaste acties en aangepaste resources

In deze zelfstudie wordt de functie werkt als een RESTful-eindpunt voor onze aangepaste provider bijgewerkt. In Azure zijn-resources en acties gemodelleerd naar de eenvoudige RESTful-specificatie: PUT - maakt een nieuwe resource, GET (exemplaar) - worden opgehaald van een bestaande resource, DELETE - verwijdert een bestaande resource, POST - een actie starten en GET (verzameling) - geeft een lijst van alle bestaande resources. Voor deze zelfstudie gebruiken we Azure Tables als onze opslag, maar elke database of opslaggroep-service kan worden gebruikt.

## <a name="how-to-partition-custom-resources-in-storage"></a>Aangepaste resources in de opslag partitioneren

Omdat we een RESTful-service maakt, moet voor het opslaan van de gemaakte resources in de opslag. Voor Azure Table storage moeten we partitie- en recordsleutels sleutels voor onze gegevens genereren. Voor aangepaste providers, moeten de gegevens zijn gepartitioneerd met de aangepaste provider. Wanneer een binnenkomende aanvraag is verzonden naar de aangepaste provider, voegt de aangepaste provider toe de `x-ms-customproviders-requestpath` header aan uitgaande aanvraag naar de `endpoint`.

voorbeeld `x-ms-customproviders-requestpath` -header voor een aangepaste resource:

```
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{myResourceType}/{myResourceName}
```

Op basis van het bovenstaande voorbeeld `x-ms-customproviders-requestpath` header, kunnen we maken de partitionKey en rowKey voor onze opslag als het volgende:

Parameter | Template | Description
---|---
partitionKey | '{subscriptionId}:{resourceGroupName}:{resourceProviderName}' | De partitionKey is hoe de gegevens zijn gepartitioneerd. De meeste gevallen kunnen de gegevens moeten worden gepartitioneerd op het exemplaar van de aangepaste provider.
RowKey | '{myResourceType}:{myResourceName}' | De rowKey is de afzonderlijke id voor de gegevens. De meeste gevallen is dit is de naam van de resource.

Bovendien moeten we ook een nieuwe klasse voor het modelleren van onze aangepaste resource maken. In deze zelfstudie, zullen we toevoegen de `CustomResource` klasse aan onze functie, die is een algemene klasse die alle ingevoerde gegevens accepteert:

```csharp
// Custom Resource Table Entity
public class CustomResource : TableEntity
{
    public string Data { get; set; }
}
```

Hiermee maakt u een eenvoudige klasse op basis van `TableEntity`, die wordt gebruikt voor het opslaan van gegevens. De `CustomResource` klasse neemt over twee eigenschappen van `TableEntity`: partitionKey en rowKey.

## <a name="support-custom-provider-restful-methods"></a>Ondersteuning voor aangepaste provider RESTful-methoden

> [!NOTE]
> Als u de code niet rechtstreeks vanuit de zelfstudie kopieert, de inhoud van de reactie moet geldige JSON en stelt de `Content-Type` header als `application/json`.

Nu dat we gegevens partitioneren hebt, gaan we ondersteuning van de eenvoudige CRUD- en trigger methoden voor aangepaste resources en aangepaste acties. Aangezien aangepaste providers als een proxy fungeren, de aanvraag en respons moeten worden gemodelleerd en verwerkt door de RESTful `endpoint`. Volg de onderstaande codefragmenten voor het verwerken van de RESTful basisbewerkingen:

### <a name="trigger-custom-action"></a>Aangepaste actie van trigger

Voor aangepaste providers, een aangepaste actie wordt geactiveerd via `POST` aanvragen. Een aangepaste actie kan de hoofdtekst van de aanvraag die een set invoerparameters bevat (optioneel) accepteren. De actie moet en teruggaan weer terug een antwoord signally het resultaat van de actie, evenals of deze is geslaagd of mislukt. In deze zelfstudie, zullen we de methode toevoegen `TriggerCustomAction` aan onze functie:

```csharp
/// <summary>
/// Triggers a custom action with some side effect.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <returns>The http response result of the custom action.</returns>
public static async Task<HttpResponseMessage> TriggerCustomAction(HttpRequestMessage requestMessage)
{
    var myCustomActionRequest = await requestMessage.Content.ReadAsStringAsync();

    var actionResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    actionResponse.Content = myCustomActionRequest != string.Empty ? 
        new StringContent(JObject.Parse(myCustomActionRequest).ToString(), System.Text.Encoding.UTF8, "application/json") :
        null;
    return actionResponse;
}
```

De `TriggerCustomAction` methode accepteert een binnenkomende aanvraag en gewoon echoot back-het-antwoord met een code van de status geslaagd. 

### <a name="create-custom-resource"></a>Aangepaste resource maken

Voor aangepaste providers, een aangepaste resource is gemaakt via `PUT` aanvragen. De aangepaste provider accepteert een JSON-aanvraagtekst, die een set eigenschappen voor de aangepaste resource bevat. Resources in Azure, Ga als volgt een RESTful-model. De aanvraag-URL die is gebruikt voor het maken van een resource moet ook kunnen ophalen en verwijderen van de resource. In deze zelfstudie, zullen we de methode toevoegen `CreateCustomResource` om nieuwe resources te maken:

```csharp
/// <summary>
/// Creates a custom resource and saves it to table storage.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="azureResourceId">The parsed Azure resource Id.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the created custom resource.</returns>
public static async Task<HttpResponseMessage> CreateCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, ResourceId azureResourceId, string partitionKey, string rowKey)
{
    // Adds the Azure top-level properties.
    var myCustomResource = JObject.Parse(await requestMessage.Content.ReadAsStringAsync());
    myCustomResource["name"] = azureResourceId.Name;
    myCustomResource["type"] = azureResourceId.FullResourceType;
    myCustomResource["id"] = azureResourceId.Id;

    // Save the resource into storage.
    var insertOperation = TableOperation.InsertOrReplace(
        new CustomResource
        {
            PartitionKey = partitionKey,
            RowKey = rowKey,
            Data = myCustomResource.ToString(),
        });
    await tableStorage.ExecuteAsync(insertOperation);

    var createResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    createResponse.Content = new StringContent(myCustomResource.ToString(), System.Text.Encoding.UTF8, "application/json");
    return createResponse;
}
```

De `CreateCustomResource` methode werkt de inkomende aanvraag naar de Azure specifieke velden bevatten: `id`, `name`, en `type`. Dit zijn op het hoogste niveau van de eigenschappen die worden gebruikt door services in Azure. Ze kunnen de aangepaste provider om te integreren met andere services zoals Azure Policy, Azure Resource Manager-sjablonen en Azure-activiteitenlogboeken.

Eigenschap | Voorbeeld | Description
---|---|---
name | '{myCustomResourceName}' | De naam van de aangepaste resource.
type | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | De naamruimte van het type resource.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>{resourceTypeName}/{myCustomResourceName}' | De resource-ID.

Naast het toevoegen van de eigenschappen, we ook het document opslaan met Azure Table Storage. 

### <a name="retrieve-custom-resource"></a>Aangepaste resource ophalen

Voor aangepaste providers, een aangepaste resource wordt opgehaald via `GET` aanvragen. De aangepaste provider wordt *niet* een JSON-aanvraagtekst accepteren. In het geval van `GET` aanvragen, de **eindpunt** moet gebruiken de `x-ms-customproviders-requestpath` koptekst naar de al gemaakte resource retourneren. In deze zelfstudie, zullen we de methode toevoegen `RetrieveCustomResource` om op te halen van bestaande resources:

```csharp
/// <summary>
/// Retrieves a custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the existing custom resource.</returns>
public static async Task<HttpResponseMessage> RetrieveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    var retrieveResponse = requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NotFound);

    retrieveResponse.Content = existingCustomResource != null ?
            new StringContent(existingCustomResource.Data, System.Text.Encoding.UTF8, "application/json"):
            null;
    return retrieveResponse;
}
```

In Azure, resources dienen een RESTful-model te volgen. De aanvraag-URL die de resource moet ook de resource retourneren als een `GET` aanvraag wordt uitgevoerd.

### <a name="remove-custom-resource"></a>Aangepaste resource verwijderen

Voor aangepaste providers, een aangepaste resource wordt verwijderd via `DELETE` aanvragen. De aangepaste provider wordt *niet* een JSON-aanvraagtekst accepteren. In het geval van `DELETE` aanvragen, de **eindpunt** moet gebruiken de `x-ms-customproviders-requestpath` koptekst van de gemaakte resource verwijderen. In deze zelfstudie, zullen we de methode toevoegen `RemoveCustomResource` om bestaande resources te verwijderen:

```csharp
/// <summary>
/// Removes an existing custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the result of the delete.</returns>
public static async Task<HttpResponseMessage> RemoveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    if (existingCustomResource != null) {
        var deleteOperation = TableOperation.Delete(existingCustomResource);
        await tableStorage.ExecuteAsync(deleteOperation);
    }

    return requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NoContent);
}
```

In Azure, resources dienen een RESTful-model te volgen. De aanvraag-URL die de resource moet ook de resource verwijderen als een `DELETE` aanvraag wordt uitgevoerd.

### <a name="list-all-custom-resources"></a>Lijst met alle aangepaste resources

Voor aangepaste providers, een lijst met bestaande aangepaste resources kan worden opgesomd via verzameling `GET` aanvragen. De aangepaste provider wordt *niet* een JSON-aanvraagtekst accepteren. In het geval van een verzameling `GET` aanvragen, de `endpoint` moet gebruiken de `x-ms-customproviders-requestpath` header opsommen van de gemaakte resources. In deze zelfstudie, zullen we de methode toevoegen `EnumerateAllCustomResources` opsommen van de bestaande resources.

```csharp
/// <summary>
/// Enumerates all the stored custom resources for a given type.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="resourceType">The resource type of the enumeration.</param>
/// <returns>The http response containing a list of resources stored under 'value'.</returns>
public static async Task<HttpResponseMessage> EnumerateAllCustomResources(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string resourceType)
{
    // Generate upper bound of the query.
    var rowKeyUpperBound = new StringBuilder(resourceType);
    rowKeyUpperBound[rowKeyUpperBound.Length - 1]++;

    // Create the enumeration query.
    var enumerationQuery = new TableQuery<CustomResource>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, partitionKey),
            TableOperators.And,
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, resourceType),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, rowKeyUpperBound.ToString()))));
    
    var customResources = (await tableStorage.ExecuteQuerySegmentedAsync(enumerationQuery, null))
        .ToList().Select(customResource => JToken.Parse(customResource.Data));

    var enumerationResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    enumerationResponse.Content = new StringContent(new JObject(new JProperty("value", customResources)).ToString(), System.Text.Encoding.UTF8, "application/json");
    return enumerationResponse;
}
```

> [!NOTE]
> De rijsleutel groter is dan en kleiner dan Azure Table-syntaxis is voor het uitvoeren van een query 'startswith' voor tekenreeksen. 

Voor het weergeven van alle bestaande resources genereren we een Azure Table-query die ervoor zorgt dat de bronnen bestaan onder onze aangepaste provider-partitie. De query en controleert of de rijsleutel wordt gestart met dezelfde `{myResourceType}`.

## <a name="integrate-restful-operations"></a>RESTful operations integreren

Zodra de RESTful methoden worden toegevoegd aan de functie, kunnen we de belangrijkste bijwerken `Run` methode voor het aanroepen van de functies voor het afhandelen van de verschillende REST-aanvragen:

```csharp
/// <summary>
/// Entry point for the Azure Function webhook and acts as the service behind a custom provider.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="log">The logger.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <returns>The http response for the custom Azure API.</returns>
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log, CloudTable tableStorage)
{
    // Get the unique Azure request path from request headers.
    var requestPath = req.Headers.GetValues("x-ms-customproviders-requestpath").FirstOrDefault();

    if (requestPath == null)
    {
        var missingHeaderResponse = req.CreateResponse(HttpStatusCode.BadRequest);
        missingHeaderResponse.Content = new StringContent(
            new JObject(new JProperty("error", "missing 'x-ms-customproviders-requestpath' header")).ToString(),
            System.Text.Encoding.UTF8, 
            "application/json");
    }

    log.LogInformation($"The Custom Provider Function received a request '{req.Method}' for resource '{requestPath}'.");

    // Determines if it is a collection level call or action.
    var isResourceRequest = requestPath.Split('/').Length % 2 == 1;
    var azureResourceId = isResourceRequest ? 
        ResourceId.FromString(requestPath) :
        ResourceId.FromString($"{requestPath}/");

    // Create the Partition Key and Row Key
    var partitionKey = $"{azureResourceId.SubscriptionId}:{azureResourceId.ResourceGroupName}:{azureResourceId.Parent.Name}";
    var rowKey = $"{azureResourceId.FullResourceType.Replace('/', ':')}:{azureResourceId.Name}";

    switch (req.Method)
    {
        // Action request for an custom action.
        case HttpMethod m when m == HttpMethod.Post && !isResourceRequest:
            return await TriggerCustomAction(
                requestMessage: req);

        // Enumerate request for all custom resources.
        case HttpMethod m when m == HttpMethod.Get && !isResourceRequest:
            return await EnumerateAllCustomResources(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                resourceType: rowKey);

        // Retrieve request for a custom resource.
        case HttpMethod m when m == HttpMethod.Get && isResourceRequest:
            return await RetrieveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Create request for a custom resource.
        case HttpMethod m when m == HttpMethod.Put && isResourceRequest:
            return await CreateCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                azureResourceId: azureResourceId,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Remove request for a custom resource.
        case HttpMethod m when m == HttpMethod.Delete && isResourceRequest:
            return await RemoveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Invalid request received.
        default:
            return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}
``` 

De bijgewerkte `Run` methode bevat nu de `tableStorage` Invoerbinding die is toegevoegd voor Azure Table storage. Het eerste deel van de methode wordt nu lezen de `x-ms-customproviders-requestpath` kop- en gebruik de `Microsoft.Azure.Management.ResourceManager.Fluent` bibliotheek parseren van de waarde als een resource-ID. De `x-ms-customproviders-requestpath` header is verzonden door de aangepaste provider en geeft het pad van de inkomende aanvraag. Met behulp van de geparseerde resource-ID, kunnen we nu genereren partitionKey en rowKey om de gegevens te zoeken of aangepaste resources opslaan.

Naast het toevoegen van de methoden en klassen, moeten we werken met de methoden voor de functie. Voeg het volgende toe aan het begin van het bestand:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"
#r "../bin/Microsoft.Azure.Management.ResourceManager.Fluent.dll"

using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Configuration;
using System.Text;
using System.Threading;
using System.Globalization;
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Als je verloren tijdens elk gewenst moment van deze zelfstudie, het voltooide voorbeeld kan worden gevonden op de [aangepaste provider C# RESTful-eindpunt verwijzing](./reference-custom-providers-csharp-endpoint.md). Nadat de functie voltooid is, sla de URL van de functie die kan worden gebruikt om de functie niet starten omdat deze wordt gebruikt in latere zelfstudies.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebben we een RESTful-eindpunt om te werken met Azure aangepaste Provider geschreven `endpoint`. Ga naar het volgende artikel voor meer informatie over het maken van een aangepaste provider.

- [Zelfstudie: Het maken van een aangepaste provider](./tutorial-custom-providers-create.md)
