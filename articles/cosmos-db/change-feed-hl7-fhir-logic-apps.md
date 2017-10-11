---
title: Feed voor HL7 FHIR resources - Azure Cosmos DB wijzigen | Microsoft Docs
description: "Informatie over het instellen van HL7 FHIR gezondheidszorg patiëntrecords met Azure Logic Apps, Azure Cosmos DB en Service Bus-wijzigingsmeldingen."
keywords: HL7 fhir
services: cosmos-db
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
ms.openlocfilehash: d2b50c0b6864af41fb9cfa051721c432772b228d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>Melding patiënten van HL7 FHIR gezondheidszorg recordwijzigingen met Logic Apps en Azure Cosmos-DB

Azure MVP Howard Edidin is onlangs neemt contact met een organisatie in de gezondheidszorg die nieuwe functionaliteit toevoegen aan hun patiënt portal wilden. Ze nodig zijn om meldingen te verzenden aan patiënten wanneer hun status record is bijgewerkt en deze patiënten kunnen abonneren op deze updates nodig. 

Dit artikel helpt bij de wijziging oplossing notification gemaakt voor deze organisatie in de gezondheidszorg met Azure Cosmos DB, Logic Apps en Service Bus-feed. 

## <a name="project-requirements"></a>Projectvereisten
- Providers verzenden dat HL7 geconsolideerde klinische Document architectuur (C-CDA) en documenten in XML-indeling. C CDA documenten omvatten vrijwel elke soort klinische document, inclusief klinische documenten zoals familie geschiedenisgegevens en immunisatie records, alsmede als administratieve werkstroom en financiële documenten. 
- C CDA documenten worden geconverteerd naar [HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON-indeling.
- Gewijzigde FHIR resource documenten worden verzonden via e-mail in JSON-indeling.

## <a name="solution-workflow"></a>Werkstroom van de oplossing 

Het project vereist op een hoog niveau de volgende werkstroomstappen: 
1. C CDA documenten niet converteren naar FHIR resources.
2. Terugkerende trigger wordt gedelegeerd voor het gewijzigde FHIR bronnen uitvoeren. 
2. Een aangepaste app FhirNotificationApi verbinding maken met Azure Cosmos DB en de query voor nieuwe of gewijzigde documenten aanroepen.
3. Het antwoord aan de Service Bus-wachtrij opslaan.
4. Poll op nieuwe berichten in de Service Bus-wachtrij.
5. E-mailmeldingen verzenden naar patiënten.

## <a name="solution-architecture"></a>Oplossingsarchitectuur
Deze oplossing vereist drie Logic Apps om te voldoen aan de bovenstaande vereisten en het voltooien van de werkstroom oplossing. De drie logische apps zijn:
1. **Toewijzing van HL7 FHIR app**: het document HL7 C-CDA ontvangt, getransformeerd tot de Resource FHIR en opgeslagen in Azure Cosmos DB.
2. **EHR app**: bevraagt de opslagplaats Azure Cosmos DB FHIR en het antwoord op een Service Bus-wachtrij wordt opgeslagen. Deze logica app gebruikmaakt van een [API-app](#api-app) nieuwe en gewijzigde documenten kunnen worden opgehaald.
3. **Proces melding app**: verzendt een e-mailbericht met de resource FHIR documenten in de hoofdtekst.

![De drie logische Apps gebruikt in deze oplossing HL7 FHIR gezondheidszorg](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a>Azure-services gebruikt voor de oplossing

#### <a name="azure-cosmos-db-documentdb-api"></a>Azure Cosmos DB DocumentDB API
Azure Cosmos-database is de opslagplaats voor de FHIR-bronnen zoals weergegeven in de volgende afbeelding.

![De Azure DB die Cosmos-account gebruikt in deze gezondheidszorg HL7 FHIR-zelfstudie](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Logische Apps verwerken het werkstroomproces. De volgende schermafbeeldingen weergeven de Logic apps die zijn gemaakt voor deze oplossing. 


1. **Toewijzing van HL7 FHIR app**: het document HL7 C-CDA ontvangen en te transformeren en een FHIR-resource met de Enterprise-integratiepakket voor Logic Apps. De Enterprise Integration Pack verwerkt de toewijzing van de C-CDA FHIR bronnen.

    ![De logische App gebruikt voor het ontvangen van HL7 FHIR gezondheidszorg records](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR app**: de opslagplaats Azure Cosmos DB FHIR vragen en in het antwoord op een Service Bus-wachtrij opslaan. De code voor de app GetNewOrModifiedFHIRDocuments is lager dan.

    ![De logische App gebruikt voor het zoeken van Azure DB die Cosmos](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **Proces melding app**: verzenden van een e-mailbericht met de resource FHIR documenten in de hoofdtekst.

    ![De logische App waarmee patiënt e-mailbericht met de resource HL7 FHIR worden verzonden in de hoofdtekst](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
De volgende afbeelding toont de patiënt wachtrij. De waarde van de Tag-eigenschap wordt gebruikt voor het onderwerp van e-mail.

![De Service Bus-wachtrij in deze zelfstudie HL7 FHIR gebruikt](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API-app
Een API-app verbindt met Azure Cosmos DB en query's voor nieuwe of gewijzigde FHIR documenten per resourcetype. Deze app heeft één domeincontroller **FhirNotificationApi** met een één bewerking **GetNewOrModifiedFhirDocuments**, Zie [bron voor de API-app](#api-app-source).

We gebruiken de [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) klasse van de Azure Cosmos DB DocumentDB .NET API. Zie voor meer informatie de [wijziging feed artikel](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments bewerking

**Invoer**
- Database-id
- CollectionId
- De naam van de HL7 FHIR brontype
- Booleaanse waarde: Starten vanaf het begin
- Int: Het aantal geretourneerde documenten

**Uitvoer**
- Succes: Statuscode: 200, antwoord: overzicht van documenten (JSON-matrix)
- Mislukt: Statuscode: 404, antwoord: ' geen documenten gevonden voor '*resourcenaam '* brontype '

<a id="api-app-source"></a>

**Bron voor de API-app**

```C#

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
                "from Last Run Date or Begining of Collection creation"
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

De volgende afbeelding laat zien hoe swagger tot is gebruikt voor het testen van de [FhirNotificationApi](#api-app-source).

![Het Swagger-bestand dat is gebruikt bij het testen van de API-app](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure-portaldashboard

De volgende afbeelding toont alle Azure-services voor deze oplossing uitgevoerd in de Azure-portal.

![De Azure-portal weer met alle services die worden gebruikt in deze zelfstudie HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Samenvatting

- U hebt geleerd dat Azure Cosmos DB heeft systeemeigen ondersteuning voor meldingen over nieuwe of gewijzigd documenten en hoe eenvoudig het is om te gebruiken. 
- Dankzij het gebruik van Logic Apps, kunt u werkstromen maken zonder een code te schrijven.
- Voor het verwerken van het distributiepunt voor de HL7 FHIR documenten met behulp van Azure Service Bus-wachtrijen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Cosmos DB, de [Azure DB die Cosmos-startpagina](https://azure.microsoft.com/services/cosmos-db/). Zie voor meer informatie over Logic Apps [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


