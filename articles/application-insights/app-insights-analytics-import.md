---
title: Uw gegevens importeren in analyses in Azure Application Insights | Microsoft Docs
description: Statische gegevens samen te voegen met de app telemetrie importeren of een afzonderlijke gegevensstroom query met Analytics importeren.
services: application-insights
keywords: Open-schema, gegevens importeren
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 963e5cfd929f57b34dcb045df82b64f870e897e2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="import-data-into-analytics"></a>Gegevens importeren in Analytics

Importeer tabellaire gegevens in [Analytics](app-insights-analytics.md)om toe te voegen met een [Application Insights](app-insights-overview.md) telemetrie van uw app of dat u deze als een afzonderlijke stream kunt analyseren. Analytics is een krachtige querytaal die geschikt is voor het analyseren van grote hoeveelheden voorzien van een tijdstempel streams telemetrie.

U kunt gegevens importeren in Analytics met uw eigen schema. Heeft geen gebruik van de standaard Application Insights-schema's zoals aanvraag of trace.

U kunt importeren JSON of DSV (scheidingsteken gescheiden waarden - komma, puntkomma of tab) bestanden.

Er zijn drie situaties waarin u importeert in Analytics handig is:

* **Voeg met app telemetrie.** U kunt bijvoorbeeld een tabel met URL's van uw website wordt toegewezen aan beter leesbaar pagina's importeren. In Analytics, kunt u een dashboard grafiekrapport de tien meest populaire pagina's in uw website. Nu kunt deze pagina's in plaats van de URL's weergeven.
* **Telemetrie van uw toepassing correleren** met andere bronnen zoals netwerkverkeer, server-gegevens of CDN logboekbestanden.
* **Analyse van toepassing op een afzonderlijke gegevensstroom.** Application Insights Analytics is een krachtig hulpprogramma dat goed samen met sparse, voorzien van een tijdstempel streams - veel beter dan SQL in veel gevallen werkt. Als u een dergelijke stroom van een andere bron hebt, kunt u deze kunt analyseren met Analytics.

Verzenden van gegevens naar de gegevensbron is eenvoudig. 

1. (Eenmaal) Definieer de planning van uw gegevens in een gegevensbron.
2. (Periodiek) Uw gegevens uploaden naar Azure storage en de REST-API om contact met ons opnemen die nieuwe gegevens opname wacht aanroepen. De gegevens zijn beschikbaar voor de query in Analytics binnen een paar minuten.

De frequentie van het uploaden wordt gedefinieerd door u en hoe snel wilt u uw gegevens beschikbaar voor query's. Het is efficiënter uploaden van gegevens in grotere segmenten, maar niet groter zijn dan 1GB.

> [!NOTE]
> *Hebt u veel gegevensbronnen analyseren?* [*Overweeg het gebruik van* logstash *voor het verzenden van uw gegevens naar Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Voordat u begint

U hebt de volgende zaken nodig:

1. Een Application Insights-resource in Microsoft Azure.

 * Als u wilt analyseren van uw gegevens afzonderlijk van andere telemetrie [Maak een nieuwe Application Insights-resource](app-insights-create-new-resource.md).
 * Als u lid worden van of uw gegevens met de telemetrie van een app die al is ingesteld met Application Insights vergelijken, kunt u de bron voor die app gebruiken.
 * Inzender of eigenaar toegang tot deze resource.
 
2. Azure-opslag. U kunt uploaden naar Azure storage en Analytics uw gegevens ontvangt van daaruit. 

 * U wordt aangeraden dat u een speciale storage-account maken voor uw blobs. Als uw blobs worden gedeeld met andere processen, duurt het langer voor onze processen uw blobs te lezen.


## <a name="define-your-schema"></a>Uw schema definiëren

Voordat u gegevens importeren kunt, moet u een *gegevensbron* waarmee het schema van uw gegevens.
U kunt maximaal 50 gegevensbronnen in een enkele Application Insights-bron hebben

1. Start de wizard gegevensbron. Gebruik de knop 'Een nieuwe gegevensbron toevoegen'. U kunt ook - klikt u op de knop instellingen in rechts bovenhoek en kies 'Gegevensbronnen' in het vervolgkeuzemenu.

    ![Nieuwe gegevensbron toevoegen](./media/app-insights-analytics-import/add-new-data-source.png)

    Geef een naam voor de nieuwe gegevensbron.

2. Definieer de indeling van de bestanden die u zult uploaden.

    U kunt de indeling handmatig definiëren of u een voorbeeld-bestand uploaden.

    Als de gegevens zich in de CSV-indeling, zijn de eerste rij van de voorbeeld-kolomkoppen. U kunt de veldnamen in de volgende stap.

    Het voorbeeld moet ten minste 10 rijen of records van de gegevens bevatten.

    Kolom- of veldnamen moeten alfanumeriek namen (zonder spaties of leestekens) hebben.

    ![Een voorbeeld-bestand uploaden](./media/app-insights-analytics-import/sample-data-file.png)


3. Controleer het schema die is verkregen met de wizard. Als deze de typen van een voorbeeld van een afgeleid, moet u mogelijk de afgeleide typen van de kolommen aanpassen.

    ![Bekijk de afgeleid schema](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Optioneel.) Upload een schemadefinitie. Zie de onderstaande notatie.

 * Selecteer een tijdstempel. Alle gegevens in Analytics moet beschikken over een tijdstempelveld. Type moet hebben `datetime`, maar heeft geen naam 'tijdstempel'. Als uw gegevens een kolom met een datum en tijd in de ISO-indeling heeft, kies deze optie als de timestamp-kolom. Kies anders 'als gegevens ontvangen' en het importproces een timestamp-veld wilt toevoegen.

5. De gegevensbron maken.

### <a name="schema-definition-file-format"></a>Bestandsindeling van pakketdefinities schema

In plaats van het schema in de gebruikersinterface bewerkt, kunt u de schemadefinitie laden uit een bestand. De indeling van de definitie schema is als volgt: 

Gescheiden indeling 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

JSON-indeling 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Elke kolom wordt geïdentificeerd door de locatie, de naam en het type. 

* Locatie – is voor het bestand met scheidingstekens hiervan de positie van de toegewezen waarde. Voor JSON-indeling is de jpath van de toegewezen sleutel.
* Naam: de naam van de kolom die wordt weergegeven.
* Type: het gegevenstype van die kolom.
 
Als een voorbeeldgegevens gebruikt en bestandsindeling worden gescheiden, moet de schemadefinitie alle kolommen toewijzen en nieuwe kolommen toevoegen aan het einde. 

JSON staat gedeeltelijke toewijzing van de gegevens, dus de schemadefinitie van JSON-indeling heeft geen elke sleutel die is gevonden in een voorbeeldgegevens toewijzen. Deze kolommen die geen deel uitmaken van de voorbeeldgegevens kan ook worden toegewezen. 

## <a name="import-data"></a>Gegevens importeren

Om gegevens te importeren, uploaden naar Azure storage, een toegangssleutel voor het maken en breng een REST-API-aanroep.

![Nieuwe gegevensbron toevoegen](./media/app-insights-analytics-import/analytics-upload-process.png)

U kunt het volgende proces handmatig uitvoeren of een geautomatiseerde systeem instellen om dat te doen met regelmatige tussenpozen. U moet als volgt te werk voor elk blok van gegevens die u wilt importeren.

1. De gegevens te uploaden [Azure blob-opslag](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * BLOBs kunnen worden elke maximaal 1GB ongecomprimeerde omvang. Grote blobs honderden MB zijn ideaal vanuit het oogpunt van prestaties van.
 * U kunt het comprimeren met Gzip uploadtijd en de latentie voor de gegevens beschikbaar zijn voor de query te verbeteren. Gebruik de `.gz` bestandsnaamextensie.
 * Het is raadzaam een afzonderlijke opslagaccount voor dit doel gebruiken om te voorkomen dat verzoeken van andere services prestaties te vertragen.
 * Tijdens het verzenden van gegevens met hoge frequentie, om de paar seconden het verdient aanbeveling gebruik van meer dan één opslagaccount, voor betere prestaties.

 
2. [Maak een Shared Access Signature-sleutel voor de blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). De sleutel moet een vervalperiode van één dag hebben en leestoegang te bieden.
3. Een REST-aanroep op de hoogte van Application Insights die wacht op de gegevens wilt maken.

 * Eindpunt:`https://dc.services.visualstudio.com/v2/track`
 * HTTP-methode: POST
 * De nettolading van:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

De tijdelijke aanduidingen zijn:

* `Blob URI with Shared Access Key`: U krijgt deze van de procedure voor het maken van een sleutel. Het is specifiek voor de blob.
* `Schema ID`: De schema-ID gegenereerd voor uw gedefinieerd schema. De gegevens in deze blob moeten voldoen aan het schema.
* `DateTime`: De UTC-tijd waarop de aanvraag is verzonden. We accepteren van deze indelingen: ISO8601 (zoals ' 01-01-2016 13:45:01 '); RFC822 (' Wed, 14 december 16 14:57:01 + 0000 '); RFC850 ("woensdag, 14-december-16 14:57:00 UTC"); RFC1123 (' Wed 14 december 2016 14:57:00 + 0000 ').
* `Instrumentation key`van uw Application Insights-resource.

De gegevens zijn beschikbaar in Analytics na een paar minuten.

## <a name="error-responses"></a>Foutberichten

* **400 onjuiste aanvraag**: geeft aan dat de nettolading van de aanvraag ongeldig is. Controleren:
 * Juiste instrumentatiesleutel.
 * Geldige tijd-waarde. Deze moet de tijd nu in UTC.
 * De JSON van de gebeurtenis voldoet aan het schema.
* **403-verboden**: de blob die u hebt verzonden, is niet toegankelijk. Zorg ervoor dat de gedeelde toegangssleutel geldig is en niet is verlopen.
* **404 niet gevonden**:
 * De blob bestaat niet.
 * De bron-id is onjuist.

Meer gedetailleerde informatie is beschikbaar in het antwoordbericht van de fout.


## <a name="sample-code"></a>Voorbeeldcode

Deze code gebruikt de [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-pakket.

### <a name="classes"></a>Klassen

```csharp
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a>Gegevens opnemen

Deze code gebruiken voor elke blob. 

```csharp
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Volgende stappen

* [Rondleiding van de querytaal Log Analytics](app-insights-analytics-tour.md)
* Als u Logstash, gebruikt u de [Logstash invoegtoepassing om gegevens te verzenden naar Application Insights](https://github.com/Microsoft/logstash-output-application-insights)
