---
title: Een pijplijn maken met de API van Azure Log Analytics Data Collector | Microsoft Docs
description: De Log Analytics HTTP Data Collector-API kunt u POST-JSON-gegevens toevoegen aan de Log Analytics-opslagplaats vanaf een willekeurige client die de REST-API kunt aanroepen. Dit artikel wordt beschreven hoe u het uploaden van gegevens in bestanden opgeslagen in een automatische manier.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/09/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 180f1a39b92dd699fa114cb98a5842b0ab0dc89a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190535"
---
# <a name="create-a-data-pipeline-with-the-data-collector-api"></a>Een pijplijn maken met de Collector-API

De [Log Analytics Data Collector API](log-analytics-data-collector-api.md) kunt u een aangepaste gegevens importeren in Log Analytics. De enige vereisten zijn dat de gegevens JSON-indeling en splitsen in 30 MB of minder segmenten zijn. Dit is een volledig flexibele mechanisme dat kan worden aangesloten op tal van manieren: van gegevens rechtstreeks vanuit uw toepassing worden verzonden naar eenmalige ad-hoc uploadt. In dit artikel wordt een overzicht sommige beginpunten voor een veelvoorkomend scenario: de noodzaak om gegevens die zijn opgeslagen in bestanden op basis van reguliere, geautomatiseerde te uploaden. Terwijl de pijplijn die hier niet de meeste goed presterende of anders geoptimaliseerd, is dit bedoeld om te fungeren als een beginpunt voor het bouwen van een productie-pipeline van uw eigen.

## <a name="example-problem"></a>Voorbeeld van probleem
Voor de rest van dit artikel bespreken we paginaweergavegegevens in Application Insights. In ons scenario hypothetische willen we geografische gegevens verzameld standaard door de Application Insights-SDK met aangepaste gegevens met de populatie van elk land/regio in de hele wereld, met het doel om te identificeren waar we het meeste moet uitgaven correleren Marketing bedragen. 

We gebruiken een openbare gegevensbron, zoals de [ongedaan maken ter wereld populatie Prospects](https://esa.un.org/unpd/wpp/) voor dit doel. De gegevens hebben de volgende eenvoudige schema:

![Voorbeeld van eenvoudige schema](./media/log-analytics-create-pipeline-datacollector-api/example-simple-schema-01.png)

In ons voorbeeld veronderstellen we dat we een nieuw bestand met de meest recente jaar gegevens uploaden zodra deze beschikbaar.

## <a name="general-design"></a>Algemeen ontwerp
We gebruiken een klassieke ETL-type-logica te ontwerpen van onze pijplijn. De architectuur ziet er als volgt uit:

![Data collection pijplijn-architectuur](./media/log-analytics-create-pipeline-datacollector-api/data-pipeline-dataflow-architecture.png)

In dit artikel wordt niet uitgelegd hoe u gegevens maakt of [uploaden naar een Azure Blob Storage-account](../storage/blobs/storage-upload-process-images.md). In plaats daarvan kiezen we de stroom van zodra een nieuw bestand wordt geüpload naar de blob. Vanaf hier:

1. Een proces wordt gedetecteerd dat nieuwe gegevens zijn geüpload.  Ons voorbeeld wordt een [Azure Logic App](../logic-apps/logic-apps-overview.md), die beschikbaar is voor een trigger voor het detecteren van nieuwe gegevens naar een blob geüpload.

2. Een processor van deze nieuwe gegevens leest en converteert deze naar JSON, de indeling die is vereist door Log Analytics.  In dit voorbeeld gebruiken we een [Azure Function](../azure-functions/functions-overview.md) als een lichtgewicht, betaalbare manier voor het uitvoeren van onze verwerkingscode. De functie wordt gestart door de dezelfde logische App die we hebben gebruikt voor het detecteren van een van de nieuwe gegevens.

3. Ten slotte, als het JSON-object beschikbaar is, wordt het verzonden naar Log Analytics. Dezelfde logische App verzendt die gegevens naar Log Analytics met behulp van de ingebouwde in Log Analytics-gegevensverzamelaar-activiteit.

Terwijl de gedetailleerde instellingen van de blob-opslag, logische App of Azure-functie niet in dit artikel beschreven wordt, vindt gedetailleerde instructies u op de specifieke producten pagina's.

Voor het bewaken van deze pijplijn, gebruiken we Application Insights voor het bewaken van onze Azure-functie [hier details](../azure-functions/functions-monitoring.md), en de Log Analytics voor het bewaken van de logische App [hier details](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md). 

## <a name="setting-up-the-pipeline"></a>Instellen van de pijplijn
Zorg ervoor dat u hebt uw blob-container gemaakt en geconfigureerd om in te stellen de pijplijn. Evenzo, zorg ervoor dat de Log Analytics-werkruimte waar u wilt verzenden van de gegevens is gemaakt.

## <a name="ingesting-json-data"></a>JSON-gegevens opnemen
Ophalen van JSON-gegevens is heel eenvoudig met Logic Apps en omdat er geen transformatie plaatsvinden moet, kunnen we de hele pijplijn in één logische App encase. Als zowel de blob-container en de Log Analytics-werkruimte hebt geconfigureerd, een nieuwe logische App maken en deze als volgt configureren:

![Voorbeeld van logische apps-werkstroom](./media/log-analytics-create-pipeline-datacollector-api/logic-apps-workflow-example-01.png)

Uw logische App opslaan en doorgaan om dit te testen.

## <a name="ingesting-xml-csv-or-other-formats-of-data"></a>XML, CSV of andere indelingen van de gegevens opnemen
Logic Apps vandaag heeft geen ingebouwde mogelijkheden voor het eenvoudig XML, CSV of andere typen transformeren naar JSON-indeling. Daarom moet een andere manier gebruiken om te voltooien van deze transformatie. In dit artikel gebruiken we de serverless Computing-mogelijkheden van Azure Functions als een zeer lichte en kosten-vriendelijk manier hiervan. 

In dit voorbeeld wordt een CSV-bestand parseren, maar alle andere bestandstypen op dezelfde manier kan worden verwerkt. Het deserialiseren gedeelte van de Azure-functie in overeenstemming met de juiste logica voor uw specifieke gegevens van het type wijzigen.

1.  Maak een nieuwe Azure-functie, met behulp van de functie runtime v1- en op basis van gebruik wanneer u hierom wordt gevraagd.  Selecteer de **HTTP-trigger** sjabloon gericht op C# als uitgangspunt dat Hiermee configureert u de bindingen die we nodig hebt. 
2.  Uit de **bestanden weergeven** tabblad in het rechterdeelvenster, maak een nieuw bestand met de naam **project.json** en plak de volgende code uit de NuGet-pakketten die we gebruiken:

    ![Azure Functions-voorbeeld-project](./media/log-analytics-create-pipeline-datacollector-api/functions-example-project-01.png)
    
    ``` JSON
    {
      "frameworks": {
        "net46":{
          "dependencies": {
            "CsvHelper": "7.1.1",
            "Newtonsoft.Json": "11.0.2"
          }  
        }  
       }  
     }  
    ```

3. Schakel over naar **run.csx** in het rechter deelvenster en vervang de standaard-code met de volgende stappen. 

    >[!NOTE]
    >Voor uw project moet u de record model (de klasse 'PopulationRecord') vervangen door uw eigen gegevensschema.
    >

    ```   
    using System.Net;
    using Newtonsoft.Json;
    using CsvHelper;
    
    class PopulationRecord
    {
        public String Location { get; set; }
        public int Time { get; set; }
        public long Population { get; set; }
    }

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        string filePath = await req.Content.ReadAsStringAsync(); //get the CSV URI being passed from Logic App
        string response = "";

        //get a stream from blob
        WebClient wc = new WebClient();
        Stream s = wc.OpenRead(filePath);         

        //read the stream
        using (var sr = new StreamReader(s))
        {
            var csvReader = new CsvReader(sr);
    
            var records = csvReader.GetRecords<PopulationRecord>(); //deserialize the CSV stream as an IEnumerable
    
            response = JsonConvert.SerializeObject(records); //serialize the IEnumerable back into JSON
        }    

        return response == null
            ? req.CreateResponse(HttpStatusCode.BadRequest, "There was an issue getting data")
            : req.CreateResponse(HttpStatusCode.OK, response);
     }  
    ```

4. Sla uw functie.
5. Test de functie om ervoor te zorgen dat de code correct werkt. Schakel over naar de **testen** tabblad in het rechterdeelvenster de test als volgt configureren. Plaats een koppeling naar een blob met voorbeeldgegevens in de **aanvraagtekst** tekstvak. Nadat u hebt geklikt **uitvoeren**, moet u ziet JSON-uitvoer in de **uitvoer** vak:

    ![Functie-Apps testen van code](./media/log-analytics-create-pipeline-datacollector-api/functions-test-01.png)

Nu moeten we teruggaan en de logische App die we begonnen met het bouwen van eerder zodanig dat de gegevens die zijn opgenomen en geconverteerd naar JSON-indeling wijzigen.  Met behulp van ontwerper, configureren als volgt te werk en sla vervolgens uw logische App:

![Compleet voorbeeld van Logic Apps-werkstroom](./media/log-analytics-create-pipeline-datacollector-api/logic-apps-workflow-example-02.png)

## <a name="testing-the-pipeline"></a>De pijplijn testen
U kunt nu een nieuw bestand uploaden naar de blob die eerder hebt geconfigureerd en deze door uw logische App bewaakt. Snel, moet u ziet een nieuw exemplaar van de logische App beluisteren, naar uw Azure-functie aanroepen en de gegevens is naar Log Analytics te verzenden. 

>[!NOTE]
>Het kan duren voordat de gegevens worden weergegeven in Log Analytics de eerste keer dat u een nieuw gegevenstype verzendt tot 30 minuten.


## <a name="correlating-with-other-data-in-log-analytics-and-application-insights"></a>Correleren met andere gegevens in Log Analytics en Application Insights
Om te voltooien ons doel voor het correleren van gegevens van Application Insights pagina weergeven met de vullen met gegevens die we afkomstig zijn van onze aangepaste gegevensbron, voert u de volgende query uit uw Application Insights Analytics venster of Log Analytics-werkruimte:

``` KQL
app("fabrikamprod").pageViews
| summarize numUsers = count() by client_CountryOrRegion
| join kind=leftouter (
   workspace("customdatademo").Population_CL
) on $left.client_CountryOrRegion == $right.Location_s
| project client_CountryOrRegion, numUsers, Population_d
```

De twee gegevens moet worden weergegeven in de uitvoer nu lid geworden van bronnen.  

![Correleren geen lid meer gegevens in het voorbeeld van een zoekopdracht-resultaat](./media/log-analytics-create-pipeline-datacollector-api/correlating-disjoined-data-example-01.png)

## <a name="suggested-improvements-for-a-production-pipeline"></a>Voorgestelde verbeteringen voor een productie-pijplijn
In dit artikel weergegeven een prototype werken, de logica achter die kan worden gebruikt voor een echte oplossing voor productiekwaliteit. Voor een dergelijke kwaliteit van de productie oplossing, worden de volgende verbeteringen aanbevolen:

* Foutafhandeling toevoegen en logica in uw logische App en de functie voor opnieuw proberen.
* Voeg logica om ervoor te zorgen dat de limiet van 30MB/één Log Analytics-gegevensopname API-aanroep niet is overschreden. De gegevens splitsen in kleinere segmenten, indien nodig.
* Instellen van een beleid voor opschonen van de blob-opslag. Nadat is verzonden naar Log Analytics, tenzij u wilt de onbewerkte gegevens beschikbaar houden voor archiveringsdoeleinden, is er geen reden om door te gaan op te slaan. 
* Controleer of de controle is ingeschakeld voor de volledige pijplijn, trace-punten toe te voegen en een waarschuwing indien van toepassing.
* Maak gebruik van broncodebeheer voor het beheren van de code voor uw functie en de logische App.
* Zorg ervoor dat een juiste wijziging management-beleid wordt gevolgd, zodat als het schema wordt gewijzigd, de functie en Logic Apps dienovereenkomstig worden aangepast.
* Als u meerdere verschillende gegevenstypen uploaden wilt, ze te scheiden in afzonderlijke mappen in uw blobcontainer en logica voor de logica uit op basis van het gegevenstype ventilator maken. 


## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Data Collector API](log-analytics-data-collector-api.md) gegevens schrijven naar Log Analytics vanuit elke client REST-API.
