---
title: Voor bronnen HL7 FHIR - Azure Cosmos DB-wijzigingenfeed
description: Meer informatie over het instellen van wijzigingsmeldingen voor HL7 FHIR gezondheidszorg patiëntendossiers met Azure Logic Apps, Azure Cosmos DB en Service Bus.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: sngun
ms.openlocfilehash: 0ff92ad58cc8b7206b7061c88f8aadbb701870f0
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54044515"
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>De hoogte te brengen patiënten van HL7 FHIR gezondheidszorg record wordt gewijzigd met Logic Apps en Azure Cosmos DB

Azure MVP Howard Edidin is onlangs contact wordt opgenomen door een organisatie in de gezondheidszorg die wilde nieuwe functies toevoegen aan hun patiënten-portal. Ze nodig zijn om meldingen te verzenden aan patiënten wanneer hun status-record is bijgewerkt, en ze patiënten kunnen zich abonneren op deze updates nodig zijn. 

Dit artikel helpt bij de notification-oplossing die zijn gemaakt voor deze organisatie in de gezondheidszorg met Azure Cosmos DB, Logic Apps en Service Bus-wijzigingenfeed. 

## <a name="project-requirements"></a>Projectvereisten
- Providers verzenden dat HL7 geconsolideerde klinische Document-architectuur (C-CDA)-documenten in XML-indeling. C-CDA documenten bevat vrijwel elk type klinische document, inclusief klinische documenten, zoals de geschiedenis van familie en immunisatie records, ook als administratieve werkstroom en financiële documenten. 
- C-CDA documenten worden geconverteerd naar [HL7 FHIR Resources](https://hl7.org/fhir/2017Jan/resourcelist.html) in JSON-indeling.
- Gewijzigde FHIR resource documenten worden verzonden via e-mail in JSON-indeling.

## <a name="solution-workflow"></a>Werkstroom van de oplossing 

Op hoog niveau vereist het project de volgende stappen uit voor werkstroom: 
1. C-CDA documenten converteren naar FHIR-resources.
2. Voer terugkerende trigger polling voor gewijzigde FHIR-resources. 
2. Een aangepaste app, FhirNotificationApi, verbinding maken met Azure Cosmos DB en query's uitvoeren voor nieuwe of gewijzigde documenten aanroepen.
3. Het antwoord naar de Service Bus-wachtrij opslaan.
4. Poll voor nieuwe berichten in de Service Bus-wachtrij.
5. E-mailmeldingen verzenden aan patiënten.

## <a name="solution-architecture"></a>Oplossingsarchitectuur
Deze oplossing vereist drie Logic Apps om te voldoen aan de bovenstaande vereisten en de werkstroom van de oplossing te voltooien. De drie logische apps zijn:
1. **Toewijzing van HL7 FHIR app**: Het document HL7 C-CDA ontvangt, getransformeerd naar de Resource FHIR en slaat deze op Azure Cosmos DB.
2. **EHR app**: Query's van de Azure Cosmos DB FHIR-opslagplaats en slaat het antwoord op een Service Bus-wachtrij. Deze logische app maakt gebruik van een [API-app](#api-app) om nieuwe en gewijzigde documenten te halen.
3. **Proces melding app**: Verzendt een e-mailbericht met de resource FHIR documenten in de hoofdtekst.

![De drie logische Apps gebruikt in deze gezondheidszorg HL7 FHIR-oplossing](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a>Azure-services die in de oplossing

#### <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API
Azure Cosmos DB is de opslagplaats voor de FHIR-resources, zoals wordt weergegeven in de volgende afbeelding.

![De Azure Cosmos DB-account hebt gebruikt in deze gezondheidszorg HL7 FHIR-zelfstudie](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Logische Apps worden verwerkt het werkstroomproces. De volgende schermafbeeldingen tonen de Logic apps gemaakt voor deze oplossing. 


1. **Toewijzing van HL7 FHIR app**: Het document HL7 C-CDA ontvangen en transformeren naar een FHIR-resource met behulp van de Enterprise Integration Pack voor logische Apps. Het Enterprise Integration Pack verwerkt de toewijzing van de C-CDA naar FHIR-resources.

    ![De logische App die wordt gebruikt voor het ontvangen van gezondheidszorg HL7 FHIR-records](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR app**: Query uitvoeren op de Azure Cosmos DB FHIR-opslagplaats en sla het antwoord op een Service Bus-wachtrij. De code voor de app GetNewOrModifiedFHIRDocuments is lager dan.

    ![De logische App gebruikt om aan te vragen uit Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **Proces melding app**: Een e-mail verzenden met behulp van de resource FHIR documenten in de hoofdtekst.

    ![De logische App die de e-mailbericht met de HL7 FHIR-resource in de hoofdtekstaanvraag verzendt](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
De volgende afbeelding ziet u de patiënt wachtrij. De waarde van de eigenschap Tag wordt gebruikt voor het e-mailonderwerp.

![De Service Bus-wachtrij die wordt gebruikt in deze zelfstudie HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API-app
Een API-app verbindt met Azure Cosmos DB en query's voor nieuwe of gewijzigde FHIR-documenten op resourcetype. Deze app heeft één domeincontroller **FhirNotificationApi** met een één bewerking **GetNewOrModifiedFhirDocuments**, Zie [bron voor API-app](#api-app-source).

We gebruiken de [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) klasse vanuit de Azure Cosmos DB SQL .NET API. Zie voor meer informatie de [wijzigingenfeed artikel](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments bewerking

**invoer**
- Database-id
- CollectionId
- De naam van het Type HL7 FHIR-Resource
- Booleaanse waarde: Vanaf begin starten
- Int: Het aantal geretourneerde documenten

**Uitvoer**
- Geslaagd: Statuscode: 200, antwoord: Lijst met documenten (JSON-matrix)
- Fout: Statuscode: 404, antwoord: "Er zijn geen documenten gevonden voor '*resourcenaam '* resourcetype"

<a id="api-app-source"></a>

**Bron voor de API-app**

```csharp

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets the new or modified FHIR documents from Last Run Date 
            ///     or create date of the collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Beginning of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();  
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-the-fhirnotificationapi"></a>De FhirNotificationApi testen 

De volgende afbeelding ziet u hoe de swagger is gebruikt voor het testen van de [FhirNotificationApi](#api-app-source).

![Het Swagger-bestand dat is gebruikt voor het testen van de API-app](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure portal-dashboard

De volgende afbeelding ziet u alle Azure-Services voor deze oplossing die wordt uitgevoerd in Azure portal.

![De Azure-portal geeft alle services die worden gebruikt in deze zelfstudie HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Samenvatting

- U hebt geleerd dat Azure Cosmos DB biedt systeemeigen ondersteuning voor meldingen voor nieuw of gewijzigd documenten en hoe eenvoudig het is om te gebruiken. 
- Door gebruik te maken van logische Apps, kunt u werkstromen maken zonder een code te schrijven.
- Met behulp van Azure Service Bus-wachtrijen voor het afhandelen van de verdeling van de HL7 FHIR-documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Cosmos DB, de [startpagina van Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Zie voor meer informatie over Logic Apps [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


