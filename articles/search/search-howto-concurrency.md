---
title: Het beheren van gelijktijdige schrijft naar resources in Azure Search
description: Optimistische gelijktijdigheid gebruiken om te voorkomen dat Force Feedback conflicten op updates of verwijderingen aan de Azure Search indexen, Indexeerfuncties en gegevensbronnen.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: heidist
ms.openlocfilehash: aee1b7376d4829e3e2f5a232525e3c3cb4df9d8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-concurrency-in-azure-search"></a>Het beheren van gelijktijdigheid van taken in Azure Search

Bij het beheren van Azure Search-resources, zoals indexen en gegevensbronnen, is het belangrijk om bij te werken bronnen veilig, vooral als bronnen gelijktijdig door verschillende onderdelen van uw toepassing worden geopend. Wanneer twee clients gelijktijdig een resource zijn zonder coördinatie bijwerkt, zijn racetoestanden mogelijk. Om dit te voorkomen, Azure Search biedt een *optimistische gelijktijdigheidsmodel*. Er zijn geen vergrendelingen voor een bron. Er is in plaats daarvan een ETag voor elke bron die u de resourceversie, identificeert zodat u kunt aanvragen dat per ongeluk voorkomen opgesteld worden overschreven.

> [!Tip]
> Conceptuele code in een [C#-oplossing steekproef](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) wordt uitgelegd hoe gelijktijdigheidbeheer werkt in Azure Search. De code maakt voorwaarden die gelijktijdigheidbeheer aanroepen. Lezen van de [onderstaande codefragment](#samplecode) is waarschijnlijk voldoende voor de meeste ontwikkelaars, maar als u wilt uitvoeren, bewerken appsettings.json als de servicenaam en een admin api-sleutel wilt toevoegen. Een service-URL van de opgegeven `http://myservice.search.windows.net`, de servicenaam is `myservice`.

## <a name="how-it-works"></a>Hoe werkt het?

Optimistische gelijktijdigheid van taken is geïmplementeerd via access voorwaarde controleert in API-aanroepen schrijven naar indexen, indexeerfuncties gegevensbronnen en synonymMap resources. 

Alle resources hebben een [ *entiteitscode (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) waarmee object versie-informatie. De ETag eerst inschakelt, kunt u voorkomen dat gelijktijdige wijzigingen in een typische werkstroom (ophalen, lokaal wijzigen, bijwerken) door ervoor te zorgen ETag komt overeen met de bron van de lokale kopie. 

+ De REST-API maakt gebruikt een [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) op de aanvraagheader.
+ De .NET SDK stelt de ETag via een accessCondition-object, instellen van de [If-Match | De header If-Match-geen](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) op de bron. Een object dat is overgenomen van [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) heeft een accessCondition-object.

Telkens wanneer u een resource bijwerkt, wordt de ETag automatisch gewijzigd. Wanneer u gelijktijdigheid management implementeert, wordt u alleen een voorwaarde plaatsen op de updateaanvraag die de externe bron hebben de dezelfde ETag als kopie van de resource die u hebt gewijzigd op de client vereist. Als een gelijktijdige proces al de externe bron is gewijzigd, wordt de ETag niet overeenkomt met de voorwaarde en de aanvraag mislukt met HTTP 412. Als u de .NET SDK, dit zich voordoet als een `CloudException` waar de `IsAccessConditionFailed()` uitbreidingsmethode ' true ' geretourneerd.

> [!Note]
> Er is slechts één mechanisme voor gelijktijdigheid van taken. Het wordt altijd gebruikt ongeacht welke API wordt gebruikt voor updates van de resource. 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Gebruiksvoorbeelden en voorbeeldcode

De volgende code toont de belangrijkste updatebewerkingen accessCondition controleert:

+ Een update mislukken als de bron niet meer bestaat
+ Een update mislukt als de resourceversie wordt gewijzigd

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>Voorbeeldcode uit [DotNetETagsExplainer programma](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

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

## <a name="design-pattern"></a>Ontwerp

Een ontwerppatroon voor de uitvoering van optimistische gelijktijdigheid bevatten een lus die probeert de voorwaarde voor toegang opnieuw controleren, een test voor de voorwaarde voor toegang en eventueel een bijgewerkte resource haalt voordat u de wijzigingen opnieuw toepassen. 

Dit codefragment ziet u de toevoeging van een synonymMap naar een index die al bestaat. Deze code is van de [synoniem (preview) C#-zelfstudie voor Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk). 

Het fragment opgehaald van de index "hotels", controleert de Objectversie van het op een updatebewerking, er een uitzondering gegenereerd als de voorwaarde is mislukt, en vervolgens probeert om opnieuw de bewerking (maximaal drie keer), beginnen met een index voor het ophalen van de server naar de nieuwste versie.

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

Controleer de [synoniemen C#-voorbeeld](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) voor meer context voor het veilig bijwerken van een bestaande index.

Probeer een van de volgende voorbeelden om op te nemen ETags of AccessCondition objecten wijzigen.

+ [REST-API-voorbeeld op Github](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ [Voorbeeld van de .NET SDK op Github](https://github.com/Azure-Samples/search-dotnet-getting-started). Deze oplossing bevat de 'DotNetEtagsExplainer'-project met de code in dit artikel.

## <a name="see-also"></a>Zie ook

  [Veelvoorkomende HTTP-headers voor aanvraag en -antwoord](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)    
  [HTTP-statuscodes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index-bewerkingen (REST-API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)