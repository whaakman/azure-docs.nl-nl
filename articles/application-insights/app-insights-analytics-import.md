---
title: Gegevens importeren met Analytics maken in Azure Application Insights | Microsoft Docs
description: Statische gegevens samen te voegen met app-telemetrie importeren, of importeren van een afzonderlijke gegevensstroom query's uitvoeren met Analytics.
services: application-insights
keywords: Open-schema, gegevens importeren
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: mbullwin
ms.openlocfilehash: cfcdf13f8aa4dfab9b361ccbb82ea4b2c3e2ca0d
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42055474"
---
# <a name="import-data-into-analytics"></a>Gegevens importeren in Analytics

Importeren van alle tabelgegevens in [Analytics](app-insights-analytics.md)om toe te voegen met een [Application Insights](app-insights-overview.md) telemetrie van uw app of zodat u ze als een afzonderlijke stroom kunt analyseren. Analytics is een krachtige querytaal die geschikt zijn voor het analyseren van grote volumes voorzien van een tijdstempel streams van telemetrie.
U kunt gegevens importeren in Analytics met behulp van uw eigen schema. Dit hoeft niet te gebruiken de standaard Application Insights-schema's, zoals aanvraag of trace.

U kunt importeren JSON of DSV (scheidingsteken gescheiden - komma, puntkomma of tabblad) bestanden.

> [!IMPORTANT]
> In dit artikel is **afgeschaft**. De aanbevolen methode van het ophalen van gegevens in Log Analytics is via de [Log Analytics-gegevensverzamelaar API.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api)

Er zijn drie situaties waarin importeren naar Analytics handig is:

* **Neem deel aan met app-telemetrie.** U kunt bijvoorbeeld een tabel met URL's van uw website wordt toegewezen aan beter leesbare paginatitels importeren. In Analytics kunt kunt u een dashboard-grafiek-rapport waarin de tien meest populaire pagina's in uw website maken. Nu kunt het de paginatitels in plaats van de URL's weergeven.
* **Correleren van telemetriegegevens van uw toepassing** met andere bronnen, zoals het netwerkverkeer, server-gegevens of CDN logboekbestanden.
* **Analyse van toepassing op een afzonderlijke gegevensstroom.** Application Insights Analytics is een krachtig hulpprogramma, dat goed met sparse, voorzien van een tijdstempel streams - veel beter dan SQL in veel gevallen werkt. Hebt u een dergelijke stroom van een andere bron, kunt u ze kunt analyseren met Analytics.

Het is eenvoudig om gegevens te verzenden naar uw gegevensbron. 

1. (Één keer) Het schema van uw gegevens in een gegevensbron definiëren.
2. (Tijd tot tijd) Uw gegevens uploaden naar Azure storage en de REST-API om te waarschuwen dat de nieuwe gegevens opname wacht aanroepen. De gegevens zijn binnen een paar minuten beschikbaar voor query's in Analytics.

De frequentie van het uploaden wordt gedefinieerd door u en hoe snel wilt u uw gegevens beschikbaar voor query's. Het is efficiënter om gegevens in grotere segmenten, maar niet groter zijn dan 1GB te uploaden.

> [!NOTE]
> *Hebt u veel gegevensbronnen voor het analyseren van?* [*Overweeg het gebruik van* logstash *voor het verzenden van uw gegevens naar Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Voordat u begint

U hebt de volgende zaken nodig:

1. Een Application Insights-resource in Microsoft Azure.

 * Als u wilt analyseren van uw gegevens onafhankelijk van andere telemetrie [Maak een nieuwe Application Insights-resource](app-insights-create-new-resource.md).
 * Als u lid worden van of uw gegevens met de telemetrie van een app die al is ingesteld met Application Insights vergelijken, kunt u de resource voor die app gebruiken.
 * Inzender of eigenaar de toegang tot die resource.
 
2. Azure-opslag. U kunt uploaden naar Azure storage en Analytics uw gegevens worden opgehaald van daaruit. 

 * U wordt aangeraden dat u een speciale storage-account maken voor uw blobs. Als uw blobs kunnen worden gedeeld met andere processen, duurt het langer om ze in onze processen te lezen van uw blobs.


## <a name="define-your-schema"></a>Uw schema definiëren

Voordat u gegevens importeren kunt, moet u een *gegevensbron,* dat aangeeft dat het schema van uw gegevens.
U kunt maximaal 50 gegevensbronnen in een enkele Application Insights-resource hebben

1. Start de wizard gegevensbron. Gebruik de knop 'Nieuwe gegevensbron toevoegen'. Ook - Klik op de knop instellingen in de linkerbovenhoek rechts en kies 'Gegevensbronnen' in in het vervolgkeuzemenu.

    ![Nieuwe gegevensbron toevoegen](./media/app-insights-analytics-import/add-new-data-source.png)

    Geef een naam voor uw nieuwe gegevensbron.

2. Indeling van de bestanden die u uploadt definiëren.

    U kunt de indeling handmatig definiëren of upload een voorbeeldbestand.

    Als de gegevens zich bevinden in CSV-indeling, kan de eerste rij van het voorbeeld van kolomkoppen zijn. U kunt de veldnamen in de volgende stap wijzigen.

    Het voorbeeld moet ten minste 10 rijen of records van de gegevens bevatten.

    Veld of kolom namen moeten alfanumerieke namen (zonder spaties of leestekens) hebben.

    ![Upload een voorbeeldbestand](./media/app-insights-analytics-import/sample-data-file.png)


3. Controleer het schema dat je kunt de wizard. Als deze de typen van een voorbeeld van een afgeleid, moet u mogelijk de afgeleide typen van de kolommen aanpassen.

    ![Het afgeleide schema controleren](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Optioneel.) Upload de schemadefinitie van een. Zie de onderstaande indeling.

 * Selecteer een tijdstempel. Alle gegevens in Analytics moet beschikken over een tijdstempelveld. Type moet hebben `datetime`, maar dit hoeft te worden met de naam 'tijdstempel'. Als uw gegevens een kolom met een datum en tijd in de ISO-indeling heeft, kies deze optie als de timestamp-kolom. Kies anders 'als gegevens ontvangen' en het importproces een tijdstempelveld wordt toegevoegd.

5. Maak de gegevensbron.

### <a name="schema-definition-file-format"></a>Bestandsindeling van pakketdefinities schema

In plaats van het schema in de gebruikersinterface bewerken, kunt u de schemadefinitie laden uit een bestand. De indeling van de definitie van schema is als volgt: 

Indeling met scheidingstekens 
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

* Locatie: gescheiden bestand deze indeling is de positie van de toegewezen waarde. Voor JSON-indeling is de jpath van de toegewezen sleutel.
* Naam: de naam van de kolom die wordt weergegeven.
* Type: het gegevenstype van die kolom.
 
> [!NOTE]
> Voorbeeldgegevens is gebruikt en de bestandsindeling worden van elkaar gescheiden, moet de schemadefinitie alle kolommen toewijzen en nieuwe kolommen toevoegen aan het einde.
> 
> JSON staat gedeeltelijke toewijzing van de gegevens, dus de schemadefinitie met een JSON-indeling heeft geen om toe te wijzen van elke sleutel die is gevonden in de voorbeeldgegevens. Deze kunt kolommen die geen deel uitmaken van de voorbeeldgegevens ook toewijzen. 

## <a name="import-data"></a>Gegevens importeren

Voor het importeren van gegevens te uploaden naar Azure storage, een toegangssleutel voor het maken en breng een REST-API-aanroep.

![Nieuwe gegevensbron toevoegen](./media/app-insights-analytics-import/analytics-upload-process.png)

U kunt de volgende stappen handmatig uitvoeren of instellen van een geautomatiseerd systeem om dat te doen met regelmatige intervallen. U moet als volgt te werk voor elk gegevensblok dat u wilt importeren.

1. De gegevens naar uploaden [Azure blob-opslag](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * Bobs kunnen bestaan uit een grootte van maximaal 1GB niet gecomprimeerd. Grote blobs van honderden MB zijn ideaal vanuit het oogpunt van prestaties.
 * U kunt deze gecomprimeerd met Gzip om tijd en latentie voor de gegevens zijn beschikbaar voor query's te verbeteren. Gebruik de `.gz` bestandsnaamextensie.
 * Het is raadzaam een afzonderlijk opslagaccount voor dit doel gebruiken om te voorkomen dat aanroepen vanuit verschillende services vertraagde prestaties.
 * Bij het verzenden van gegevens in hoge frequentie, om de paar seconden, het verdient aanbeveling gebruik van meer dan één storage-account, voor betere prestaties.

 
2. [Maak een Shared Access Signature-sleutel voor de blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). De sleutel moet een vervalperiode van één dag hebben en leestoegang bieden.
3. Een REST-aanroep naar een melding van Application Insights die is in afwachting van gegevens maken.

 * Eindpunt: `https://dc.services.visualstudio.com/v2/track`
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

* `Blob URI with Shared Access Key`: U krijgt deze van de procedure voor het maken van een sleutel. Dit is specifiek voor de blob.
* `Schema ID`: De schema-ID die is gegenereerd voor uw schema opgegeven. De gegevens in deze blob moet voldoen aan het schema.
* `DateTime`: De tijd waarop de aanvraag is verzonden, UTC. We accepteren van deze indelingen: ISO8601 (zoals "01-01-2016 13:45:01"); RFC822 ("WO, 14 december 16 14:57:01 + 0000"); RFC850 ("woensdag, 14-december-16 14:57:00 UTC"); RFC1123 ("WO 14 december 2016 14:57:00 + 0000 ').
* `Instrumentation key` van uw Application Insights-resource.

De gegevens zijn beschikbaar in Analytics na een paar minuten.

## <a name="error-responses"></a>Foutberichten

* **400 Ongeldige aanvraag**: geeft aan dat de nettolading van de aanvraag ongeldig is. Controleren:
 * Juiste instrumentatiesleutel.
 * Geldige tijd-waarde. Dit moet de tijd in UTC nu.
 * De JSON van de gebeurtenis voldoet aan het schema.
* **403-verboden**: de blob die u hebt verzonden, is niet toegankelijk. Zorg ervoor dat de gedeelde toegangssleutel geldig is en niet is verlopen.
* **404 niet gevonden**:
 * De blob bestaat niet.
 * De bron-id is onjuist.

Meer gedetailleerde informatie vindt u in het antwoordbericht fout.


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

Gebruik deze code voor elke blob. 

```csharp
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Volgende stappen

* [Rondleiding door de Log Analytics-querytaal](app-insights-analytics-tour.md)
* Als u Logstash, gebruikt u de [Logstash-invoegtoepassing voor het verzenden van gegevens naar Application Insights](https://github.com/Microsoft/logstash-output-application-insights)
