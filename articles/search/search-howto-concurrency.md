---
title: Gelijktijdige schrijfbewerkingen naar bronnen - Azure Search beheren
description: Optimistische gelijktijdige uitvoering gebruiken om te voorkomen dat halverwege lucht conflicten op updates of verwijderingen aan Azure Search-indexen, Indexeerfuncties en gegevensbronnen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/21/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4599498918b7a01a1207f20135c26924c6758eb8
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499422"
---
# <a name="how-to-manage-concurrency-in-azure-search"></a>Gelijktijdigheid van taken in Azure Search beheren

Bij het beheren van Azure Search-resources zoals indexen en gegevensbronnen, is het belangrijk om bij te werken resources veilig, met name als resources gelijktijdig worden gebruikt door verschillende onderdelen van uw toepassing. Wanneer twee clients gelijktijdig een resource zonder coördinatie bijwerkt, zijn racevoorwaarden mogelijk. Om dit te voorkomen, Azure Search biedt een *optimistische gelijktijdigheidsmodel*. Er zijn geen vergrendelingen op een resource. Er is in plaats daarvan een ETag voor elke resource waarmee de resourceversie, zodat u ze kunt moesten maken aanvragen die voorkomen per ongeluk dat worden overschreven.

> [!Tip]
> Conceptuele code in een [voorbeeld C# oplossing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) wordt uitgelegd hoe gelijktijdigheidsbeheer werkt in Azure Search. De code maakt voorwaarden waarmee u gelijktijdigheidsbeheer kunt aanroepen. Lezen van de [codefragment hieronder](#samplecode) is waarschijnlijk voldoende voor de meeste ontwikkelaars, maar als u wilt uitvoeren, bewerken appsettings.json als naam van de service en een admin api-sleutel wilt toevoegen. Een service-URL van de opgegeven `http://myservice.search.windows.net`, de servicenaam van de is `myservice`.

## <a name="how-it-works"></a>Hoe werkt het?

Optimistische gelijktijdigheid wordt geïmplementeerd via access voorwaarde controleert in API-aanroepen naar indexen, indexeerfuncties, gegevensbronnen en synonymmap, die vervolgens resources schrijven.

Alle resources hebben een [ *entity-tag (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) waarmee de object-versie-informatie. De ETag die is eerst inschakelt, kunt u voorkomen dat gelijktijdige updates in een werkstroom (get, lokaal wijzigen, bijwerken) door ervoor te zorgen dat de resource ETag komt overeen met de lokale kopie.

+ De REST-API maakt gebruik van een [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) in de aanvraagheader.
+ De .NET SDK stelt de ETag via een accessCondition-object, instellen van de [If-Match | De header If-Match-geen](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) op de resource. Een object overnemen van [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) een accessCondition-object.

Telkens wanneer u een resource bijwerkt, wordt automatisch de ETag gewijzigd. Wanneer u gelijktijdigheidsbeheer implementeert, zich staat u alleen een voorwaarde in de updateaanvraag die de externe resource toe aan de ETag die hetzelfde is als de kopie van de resource die u hebt gewijzigd op de client zijn vereist. Als de externe resource al is gewijzigd in een gelijktijdige proces, de ETag niet overeenkomen met de voorwaarde en de aanvraag mislukken met HTTP 412. Als u de .NET SDK, dit zich voordoet als een `CloudException` waar de `IsAccessConditionFailed()` uitbreidingsmethode retourneert ' True '.

> [!Note]
> Er is slechts één mechanisme voor gelijktijdigheid van taken. Het wordt altijd gebruikt, ongeacht welke API wordt gebruikt voor updates van de resource.

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Use cases en voorbeeldcode

De volgende code ziet u accessCondition controleert op belangrijke update-bewerkingen:

+ Een update mislukt als de resource niet meer bestaat
+ Een update mislukt als de resourceversie wordt gewijzigd

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>Voorbeeldcode van [DotNetETagsExplainer programma](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Search has an associated ETag that keeps track of which version
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a>ontwerppatroon

Een ontwerppatroon voor het implementeren van optimistische gelijktijdigheid bevatten een lus die pogingen van de voorwaarde toegang controleren, een test voor de voorwaarde toegang en eventueel een bijgewerkte resource opgehaald voordat u de wijzigingen opnieuw toepassen.

Dit codefragment ziet u het toevoegen van een synonymmap, die vervolgens naar een index die al bestaat. Deze code is afkomstig van de [synoniem (preview) C# voorbeeld voor Azure Search](search-synonyms-tutorial-sdk.md).

Het codefragment wordt de index "hotels", controleert de Objectversie voor een updatebewerking, een uitzondering genereert als de voorwaarde is mislukt, en vervolgens probeert om opnieuw de bewerking (maximaal drie keer), beginnen met het ophalen van de index van de server naar de meest recente versie.

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a>Volgende stappen

Controleer de [synoniemen C# voorbeeld](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) voor meer context over het veilig bijwerken van een bestaande index.

Probeer een van de volgende voorbeelden om op te nemen ETags of AccessCondition objecten wijzigen.

+ [REST-API-voorbeeld op GitHub](https://github.com/Azure-Samples/search-rest-api-getting-started)
+ [SDK voor .NET-voorbeeld op GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started). Deze oplossing omvat het 'DotNetEtagsExplainer'-project met de code die in dit artikel worden gepresenteerd.

## <a name="see-also"></a>Zie ook

[Veelvoorkomende HTTP-headers voor aanvraag en respons](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)
[HTTP-statuscodes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)
[Index-bewerkingen (REST-API)](https://docs.microsoft.com/rest/api/searchservice/index-operations)
