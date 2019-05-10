---
title: Patronen voor Azure Stream Analytics-oplossing
description: Meer informatie over oplossingspatronen voor Azure Stream Analytics.
author: zhongc
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 4f4f22628d2c2a6beb7974aa9b776a2148a3fee0
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65238060"
---
# <a name="azure-stream-analytics-solution-patterns"></a>Patronen voor Azure Stream Analytics-oplossing

Net als vele andere services in Azure Stream Analytics is beste worden gebruikt met andere services om een grotere end-to-end oplossing te maken. In dit artikel wordt beschreven eenvoudige Azure Stream Analytics-oplossingen en verschillende architectonische patronen. U kunt voortbouwen op deze patronen om complexe oplossingen te ontwikkelen. De patronen die worden beschreven in dit artikel kunnen worden gebruikt in een groot aantal scenario's. Voorbeelden van scenario-specifieke patronen zijn beschikbaar op [Azure-architecturen](https://azure.microsoft.com/solutions/architecture/?product=stream-analytics).

## <a name="create-a-stream-analytics-job-with-a-real-time-dashboard"></a>Een Stream Analytics-taak maken met een realtime dashboard

Met Azure Stream Analytics gebruiksgemak gebruikt, kunt u snel paar van realtime-dashboards en meldingen. Een eenvoudige oplossing worden opgenomen gebeurtenissen van Event Hubs of IoT-Hub, en [-feeds van de Power BI-dashboard met een streaminggegevensset](/power-bi/service-real-time-streaming). Zie voor meer informatie de gedetailleerde zelfstudie [analyseren van gegevens van telefoongesprekken met Stream Analytics en resultaten te visualiseren in Power BI-dashboard](stream-analytics-manage-job.md).

![ASA Power BI-dashboard](media/stream-analytics-solution-patterns/pbidashboard.png)

Deze oplossing kan worden gebouwd in slechts enkele minuten in Azure portal. Er is dat geen uitgebreide codering betrokken en SQL-taal wordt gebruikt om de zakelijke logica.

Dit patroon van de oplossing voor realtime dashboard biedt de laagste latentie van de bron van de gebeurtenis naar het Power BI-dashboard in een browser. Azure Stream Analytics is het alleen Azure-service met deze ingebouwde functie.

## <a name="use-sql-for-dashboard"></a>Gebruik SQL voor dashboard

De Power BI-dashboard biedt een lage latentie, maar deze kan niet worden gebruikt om volledig uitgewerkte Power BI-rapporten te genereren. Een algemeen patroon voor rapportage is voor de eerste uitvoer van uw gegevens naar een SQL-database. Vervolgens gebruikt u Power BI SQL-connector op SQL-query voor de meest recente gegevens.

![ASA SQL-dashboard](media/stream-analytics-solution-patterns/sqldashboard.png)

Met behulp van SQL biedt database u meer flexibiliteit ten koste van de hogere latentie. Deze oplossing is geoptimaliseerd voor taken met latentievereisten die groter is dan een seconde. Met deze methode kunt u Power BI hulpprogramma voor verdere segment te maximaliseren en analyseren van de gegevens voor rapporten. U beschikt ook over de flexibiliteit van het gebruik van andere oplossingen voor dashboard, zoals Tableau.

SQL is niet een gegevensarchief met hoge doorvoer en de maximale doorvoer naar een SQL-database van Azure Stream Analytics is 24 MB/s. Als er gegevens tegen een hoger tarief wordt geproduceerd door de bronnen van gebeurtenissen in uw oplossing, moet u verwerkingslogica in Stream Analytics gebruiken om te beperken van de snelheid van de uitvoer naar SQL. Patroon van technieken zoals filteren, statistische functies in vensters, die overeenkomt met de tijdelijke joins en analytische functies kunnen worden gebruikt. De snelheid van de uitvoer naar SQL verder kan worden geoptimaliseerd met behulp van technieken die worden beschreven in [Azure Stream Analytics-uitvoer naar Azure SQL Database](stream-analytics-sql-output-perf.md).

## <a name="incorporate-real-time-insights-into-your-application-with-event-messaging"></a>Realtime-inzichten in uw toepassing met de gebeurtenis berichten opnemen

De tweede populairste gebruik van Stream Analytics is om realtime waarschuwingen te genereren. In dit patroon oplossing bedrijfslogica in Stream Analytics kan worden gebruikt voor het detecteren van [tijdelijke en ruimtelijke patronen](stream-analytics-geospatial-functions.md) of [afwijkingen](stream-analytics-machine-learning-anomaly-detection.md), en vervolgens waarschuwingen signalen produceren. In tegenstelling tot de dashboard-oplossing waarin Stream Analytics maakt gebruik van Power BI als een voorkeurs-eindpunt, kan een aantal tussenliggende gegevenssinks worden gebruikt. Deze sinks zijn Event Hubs, Service Bus en Azure Functions. Als de opbouwfunctie voor toepassing, moet u besluiten welke gegevenssink voor uw scenario het meest geschikt.

Downstream gebeurtenis consument logische moet worden geïmplementeerd voor het genereren van waarschuwingen in uw bestaande zakelijke werkstroom. Omdat u kunt aangepaste logica implementeren in Azure Functions, Functions de is de snelste manier waarop u deze integratie kunt uitvoeren. Een zelfstudie voor het gebruik van Azure-functie als de uitvoer voor een Stream Analytics-taak kunt u vinden in [Azure Functions uitvoeren vanuit Azure Stream Analytics-taken](stream-analytics-with-azure-functions.md). Azure Functions ondersteunt ook verschillende typen meldingen, inclusief tekst- en e-mailbericht. Logische App kan ook worden gebruikt voor deze integratie met Event Hubs tussen Stream Analytics en de logische App.

![ASA gebeurtenis berichten-app](media/stream-analytics-solution-patterns/eventmessagingapp.png)

Eventhubs, biedt aan de andere kant de meest flexibele integratiepunt. Veel andere services, zoals Azure Data Explorer en Time Series Insight, kunnen gebruikmaken van gebeurtenissen van Event Hubs. Services kunnen worden verbonden rechtstreeks naar de Event Hubs-sink van Azure Stream Analytics om de oplossing te voltooien. Eventhubs is ook de hoogste doorvoer berichtbroker beschikbaar zijn op Azure voor dergelijke scenario's voor gegevensintegratie.

## <a name="dynamic-applications-and-websites"></a>Dynamische toepassingen en websites

U kunt aangepaste realtime visualisaties, zoals een dashboard maken of kaartvisualisatie, met behulp van Azure Stream Analytics en Azure SignalR Service. Met SignalR, web-clients kunnen worden bijgewerkt en dynamische inhoud weergeven in realtime.

![Dynamische ASA-app](media/stream-analytics-solution-patterns/dynamicapp.png)

## <a name="incorporate-real-time-insights-into-your-application-through-data-stores"></a>Realtime-inzichten in uw toepassing via gegevensarchieven opnemen

De meeste webservices en web-apps moet u vandaag nog een verzoek/reactie-patroon gebruiken om te fungeren de presentatielaag. Het verzoek/reactie-patroon is eenvoudig te bouwen en kunnen eenvoudig worden geschaald met lage responstijd met behulp van een staatloze front-end- en schaalbare stores, zoals Cosmos DB.

Hoog gegevensvolume maakt vaak knelpunten in een CRUD-systeem. De [gebeurtenisbronnenpatroon oplossing](/azure/architecture/patterns/event-sourcing.md) wordt gebruikt om de knelpunten. Tijdelijke patronen en inzichten zijn ook moeilijk en inefficiënt om op te halen uit een traditionele gegevensarchief. Moderne hoog volume gegevensgestuurde toepassingen vaak vast een architectuur op basis van een gegevensstroom. Azure Stream Analytics als de Reken-engine voor gegevens in beweging is een linchpin in die architectuur.

![ASA event sourcing app](media/stream-analytics-solution-patterns/eventsourcingapp.png)

In dit patroon oplossing gebeurtenissen verwerkt en samengevoegd in de gegevensarchieven door Azure Stream Analytics. Niveau van de toepassing communiceert met gegevensarchieven die met behulp van de traditionele verzoek/reactie-patroon. Vanwege de Stream Analytics-mogelijkheid voor het verwerken van een groot aantal gebeurtenissen in realtime, de toepassing is zeer schaalbaar zonder de noodzaak voor het bulksgewijs kopiëren van de gegevenslaag van de store. De gegevenslaag van de store is in feite een gerealiseerde weergave in het systeem. [Azure Stream Analytics-uitvoer naar Azure Cosmos DB](stream-analytics-documentdb-output.md) wordt beschreven hoe Cosmos DB wordt gebruikt als de uitvoer van een Stream Analytics.

In de echte toepassingen waarbij verwerking van logica complex en er is is de noodzaak om bij te werken bepaalde onderdelen van de logica onafhankelijk van elkaar, meerdere Stream Analytics-taken kunnen worden samengesteld, samen met Event Hubs als de tussenliggende gebeurtenis-broker.

![ASA complex event sourcing app](media/stream-analytics-solution-patterns/eventsourcingapp2.png)

Dit patroon verbetert de flexibiliteit en beheerbaarheid van het systeem. Hoewel de Stream Analytics garandeert exact eenmaal verwerken, is er echter een kleine kans is dat dubbele gebeurtenissen u in de tussenliggende Event Hubs komt mogelijk. Het is belangrijk voor de downstream Stream Analytics-taak voor gebeurtenissen met logische sleutels in een venster lookback Ontdubbeling. Zie voor meer informatie over de bezorging van gebeurtenissen, [gebeurtenis levering garanties](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) verwijzing.

## <a name="use-reference-data-for-application-customization"></a>Referentiegegevens gebruiken voor het aanpassen van de toepassing

De functie voor referentiegegevens Azure Stream Analytics is speciaal ontworpen voor aanpassing van de eindgebruikers, zoals waarschuwingsdrempel, regels, worden verwerkt en [geofences](geospatial-scenarios.md). Niveau van de toepassing kan de gewijzigde serverparameters accepteren en op te slaan in een SQL-database. De Stream Analytics-taak periodiek zoekt naar wijzigingen uit de database en de parameters aanpassen toegankelijk maakt via een verwijzing gegevens join. Zie voor meer informatie over het gebruik van referentiegegevens voor aanpassing van de toepassing [SQL referentiegegevens](sql-reference-data.md) en [verwijzing gegevens join](/stream-analytics-query/reference-data-join-azure-stream-analytics).

Dit patroon kan ook worden gebruikt voor het implementeren van een regels-engine waar de drempelwaarden van de regels uit de referentiegegevens worden gedefinieerd. Zie voor meer informatie over regels [configureerbare drempelwaarde gebaseerde regels in Azure Stream Analytics verwerkt](stream-analytics-threshold-based-rules.md).

![App met verwijzing ASA](media/stream-analytics-solution-patterns/refdataapp.png)

## <a name="add-machine-learning-to-your-real-time-insights"></a>Machine Learning toevoegen aan uw inzichten in real time

Azure Stream Analytics de ingebouwde [Anomaliedetectie model](stream-analytics-machine-learning-anomaly-detection.md) is een handige manier om te introduceren van Machine Learning aan uw realtime-toepassing. Zie voor een breder scala van Machine Learning nodig heeft, [Azure Stream Analytics kan worden geïntegreerd met Azure Machine Learning scoring service](stream-analytics-machine-learning-integration-tutorial.md).

Voor gevorderde gebruikers die willen online training en scoren opnemen in de dezelfde Stream Analytics-pijplijn in dit voorbeeld van hoe dit doen met [lineaire regressie](stream-analytics-high-frequency-trading.md).

![ASA Machine Learning-app](media/stream-analytics-solution-patterns/mlapp.png)

## <a name="near-real-time-data-warehousing"></a>In de buurt van real-time gegevensopslag

Een ander algemeen patroon is realtime data warehousing, ook wel streaming datawarehouse. Naast de gebeurtenissen die binnenkomen in Event Hubs en IoT-Hub vanuit uw toepassing, [Azure Stream Analytics op IoT Edge](stream-analytics-edge.md) kan worden gebruikt om te voldoen aan de gegevens worden opgeschoond, gegevensreductie, en gegevensopslag en forward behoeften. Stream Analytics op IoT Edge kunnen probleemloos beperking van de bandbreedte en verbindingsproblemen in het systeem worden verwerkt. De uitvoeradapter SQL kan worden gebruikt voor uitvoer naar SQL Data Warehouse; de maximale doorvoer is echter beperkt tot 10 MB/s.

![ASA Data Warehousing](media/stream-analytics-solution-patterns/datawarehousing.png)

Een manier voor het verbeteren van de doorvoer met sommige negatieve gevolgen voor de latentie is voor het archiveren van de gebeurtenissen in Azure Blob-opslag, en vervolgens [ze importeren in SQL Data Warehouse met Polybase](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md). U moet handmatig samen te voegen uitvoer van Stream Analytics op blob-opslag en invoer van blob-opslag naar SQL Data Warehouse door [archiveren van de gegevens op timestamp](stream-analytics-custom-path-patterns-blob-storage-output.md) en periodiek te importeren.

In dit gebruikspatroon wordt Azure Stream Analytics gebruikt als een bijna realtime ETL-engine. Nieuwe worden binnenkomende gebeurtenissen continu omgezet en opgeslagen voor gebruik van downstream analytics-service.

![ASA hoge doorvoer, Data Warehousing](media/stream-analytics-solution-patterns/datawarehousing2.png)

## <a name="archiving-real-time-data-for-analytics"></a>Archiveren van realtime gegevens voor analyse

De meeste activiteiten van data science en analyse nog steeds offline gebeuren. Gegevens kunnen worden gearchiveerd door Azure Stream Analytics via Azure Data Lake Store Gen2 uitvoer en Parquet uitvoerindeling op te geven. Deze mogelijkheid Hiermee verwijdert u de ergernis om gegevens rechtstreeks in Azure Data Lake Analytics, Azure Databricks en Azure HDInsight-feed. Azure Stream Analytics wordt gebruikt als een bijna realtime ETL-engine in deze oplossing. U kunt gearchiveerde gegevens in Data Lake verkennen met behulp van verschillende compute-engines.

![ASA offline analytics](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="use-reference-data-for-enrichment"></a>Referentiegegevens gebruiken voor verrijking

Gegevensverrijking is vaak een vereiste voor ETL-engines. Azure Stream Analytics ondersteunt gegevensverrijking met [verwijzen naar gegevens](stream-analytics-use-reference-data.md) van zowel SQL database en Azure Blob-opslag. Gegevensverrijking kan worden gedaan voor gegevens terechtkomen in zowel Azure Data Lake en SQL Data Warehouse.

![ASA offline analyse met gegevensverrijking](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="operationalize-insights-from-archived-data"></a>Operationeel inzicht in gearchiveerde gegevens

Als u het patroon offline analytics met de bijna realtime toepassing combineren patroon, kunt u een feedback-lus. De feedback-lus kunt de toepassing automatisch wordt aangepast voor het wijzigen van patronen in de gegevens. Deze feedback-lus kan worden net zo eenvoudig als het wijzigen van de drempelwaarde voor waarschuwingen, of een stuk complexer is zoals Machine Learning-modellen opnieuw trainen. Dezelfde oplossingsarchitectuur kan worden toegepast op beide ASA-taken in de cloud en op IoT Edge worden uitgevoerd.

![ASA insights uitoefening](media/stream-analytics-solution-patterns/insightsoperationalization.png)

## <a name="how-to-monitor-asa-jobs"></a>Het bewaken van de ASA-jobs

Een Azure Stream Analytics-taak kan 24/7 voor het verwerken van inkomende gebeurtenissen continu in realtime worden uitgevoerd. De garantie voor actieve tijdsduur is van cruciaal belang voor de status van de algemene toepassing. Stream Analytics is de enige streaming analytics-service in de bedrijfstak biedt een [gegarandeerde beschikbaarheid van 99,9%](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/), worden er nog steeds bepaalde mate van uitvaltijd. Stream Analytics heeft de afgelopen jaren geïntroduceerd metrische gegevens, logboeken en taakstatussen in overeenstemming met de status van de taken. Ze allemaal zijn opgehaald via de Azure Monitor-service en verder kunnen worden geëxporteerd naar OMS. Zie voor meer informatie, [inzicht in Stream Analytics-taak controleren en bewaken van query's](stream-analytics-monitoring.md).

![ASA controleren](media/stream-analytics-solution-patterns/monitoring.png)

Er zijn twee belangrijke dingen om te controleren:

- [Taak de status mislukt](job-states.md)

    Allereerst omdat, moet u om te controleren of dat de taak wordt uitgevoerd. Zonder de taak in de status running doorbrengt, zijn er geen nieuwe metrische gegevens of de logboeken worden gegenereerd. Taken kunnen wijzigen in een mislukte status om verschillende redenen, waaronder met een hoog niveau voor SU-gebruik (dat wil zeggen, onvoldoende resources genoemd).

- [Watermerk vertraging metrische gegevens](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/)

    Met deze metriek weerspiegelt hoe ver achter de verwerking van de pijplijn is in kloktijd (seconden). Enkele van de vertraging wordt toegeschreven aan de inherente verwerkingslogica. Als gevolg hiervan is het veel belangrijker dan de bewaking van de absolute waarde bewaking van de stijgende trend. De vertraging stabiele status moet worden opgelost door het ontwerp van uw toepassing niet door bewaking of waarschuwingen.

Als de mislukt, activiteitenlogboeken en [diagnoselogboeken](stream-analytics-job-diagnostic-logs.md) zijn de aanbevolen plaatsen om te beginnen met zoeken naar fouten.

## <a name="build-resilient-and-mission-critical-applications"></a>Bouw robuuste en essentiële toepassingen

Ongeacht de SLA-garantie van Azure Stream Analytics en hoe zorg ervoor dat uitvoert u de end-to-end toepassing, storingen optreden. Als uw toepassing essentiële is, moet u bij uitval worden voorbereid om te herstellen zonder problemen.

Voor waarschuwingen over toepassingen, is het belangrijkste dat voor het detecteren van de volgende waarschuwing. U kunt kiezen om het opnieuw opstarten van de taak van de huidige tijd wanneer herstelt, worden eerdere waarschuwingen worden genegeerd. De taak start tijd semantiek zijn door de eerste keer in de uitvoer, niet de eerste invoertijd. De invoer wordt een juiste hoeveelheid tijd om te garanderen dat de eerste uitvoer op de opgegeven tijd is voltooid en de juiste achterwaartse teruggespoeld. U wordt niet ophalen van gedeeltelijke statistische functies en waarschuwingen onverwacht activeren als gevolg hiervan.

U kunt ook uitvoer starten vanuit een zekere mate van de tijd in het verleden. Bewaarbeleid voor zowel Event Hubs en IoT-Hub houdt een redelijk tijdsbestek van gegevens kunnen worden verwerkt in het verleden. De verhouding is hoe snel kunt u achterhalen op de huidige tijd en start tijdige nieuwe waarschuwingen worden gegenereerd. Gegevens verliest de waarde ervan snel na verloop van tijd, dus is het belangrijk om snel aan de huidige tijd dingen. Er zijn twee manieren om snel dingen:

- Meer bronnen (SU) inrichten als bouwen.
- Opnieuw opstarten na de huidige tijd.

De tijd is opnieuw te starten vanaf de huidige eenvoudig te achterhalen, met de verhouding van het bewaren van een onderbreking tijdens de verwerking. Opnieuw te starten op deze manier kan mogelijk OK voor scenario's, waarschuwingen, maar problematisch voor scenario's voor dashboard en is een niet-starter voor archivering en datawarehousescenario's.

Meer bronnen kan het proces versnellen, maar het effect dat een piek van de snelheid verwerking van complexe is.

- De test is dat uw taak schaalbaar naar een groter aantal su's is. Niet alle query's die zijn schaalbaar. U moet om ervoor te zorgen dat uw query is [geparallelliseerd](stream-analytics-parallelization.md).

- Controleer of er voldoende partities in de upstream-Event-Hubs of IoT-Hub die u meer Doorvoereenheden (Doorvoereenheden) voor het schalen van de invoer doorvoer kunt toevoegen. Denk eraan dat elke Event Hubs-Doorvoereenheden loadniveau tegen een tarief van de uitvoer van 2 MB/s.

- Zorg ervoor dat u voldoende resources in de uitvoerlocaties (dat wil zeggen, SQL-Database, Cosmos DB) hebt ingericht, zodat ze niet de piek voordoet in de uitvoer, die soms kunt beperken ervoor zorgen dat het systeem te blokkeren.

Het belangrijkste dat wordt verwacht dat de wijziging van de verwerking van tarief, deze scenario's voordat u gaat naar de productie testen en klaar voor het schalen van de verwerking correct tijdens de hersteltijd fout.

In het scenario voor extreme dat binnenkomende gebeurtenissen alle zijn vertraagd, [is het mogelijk alle de vertraagde gebeurtenissen worden verwijderd](stream-analytics-time-handling.md) als u een latere binnenkomen venster voor de taak hebt toegepast. Het verwijderen van de gebeurtenissen kan lijken te zijn van een verwarrende gedrag aan het begin; considering Stream Analytics een engine voor verwerking in realtime is, verwacht het inkomende gebeurtenissen moet dicht bij de kloktijd. Het heeft verwijderen van gebeurtenissen die deze beperkingen schenden.

### <a name="backfilling-process"></a>Backfilling proces

Gelukkig kan het vorige patroon voor het archiveren van gegevens worden gebruikt voor het verwerken van deze te late gebeurtenissen zonder problemen. Het idee is dat de taak archiveren binnenkomende gebeurtenissen worden verwerkt in de aankomsttijd en gebeurtenissen in het juiste moment bucket in Azure Blob of Azure Data Lake Store met hun tijd van de gebeurtenis-archieven. Het maakt niet uit hoe laat een gebeurtenis wordt ontvangen, wordt nooit verwijderd. Deze komt altijd u in de bucket van het juiste moment. Tijdens het herstellen is het mogelijk te verwerken van de gearchiveerde gebeurtenissen en backfill de resultaten naar de store naar keuze.

![ASA backfill](media/stream-analytics-solution-patterns/backfill.png)

Het proces backfill heeft met een offline-systeem, dat waarschijnlijk een andere programmeermodel dan Azure Stream Analytics heeft voor batchverwerking worden uitgevoerd. Dit betekent dat u hebt de volledige verwerkingslogica opnieuw te implementeren.

Voor backfilling is het toch belangrijk ten minste tijdelijk inrichten die meer resource die u wilt de uitvoer PUT voor het afhandelen van hogere doorvoer dan de actieve status moet verwerken.

|Scenario's  |Opnieuw worden gestart vanaf nu alleen  |Het opnieuw opstarten van tijd voor het laatst geëindigd |Opnieuw worden gestart vanaf nu + backfill met gearchiveerde gebeurtenissen|
|---------|---------|---------|---------|
|**Dashboarding**   |Hiermee maakt u tussenruimte    |OK voor korte onderbreking    |Gebruik voor lange onderbreking |
|**Waarschuwingen**   |Aanvaardbaar |OK voor korte onderbreking    |Niet nodig |
|**Event sourcing app** |Aanvaardbaar |OK voor korte onderbreking    |Gebruik voor lange onderbreking |
|**Gegevensopslag**   |Verlies van gegevens  |Aanvaardbaar |Niet nodig |
|**Offline analytics**  |Verlies van gegevens  |Aanvaardbaar |Niet nodig|

## <a name="putting-it-all-together"></a>Alles samenvoegen

Het is niet moeilijk te imagine dat alle hierboven genoemde oplossingspatronen kunnen worden gecombineerd in een complexe end-to-end-systeem. Het gecombineerde systeem kunt opnemen dashboards, waarschuwingen, event sourcing toepassing, gegevensopslag en offline analysemogelijkheden.

De sleutel is het ontwerpen van uw systeem in samenstelbare patronen, zodat elk subsysteem kan worden gebouwd, getest, upgrade hebt uitgevoerd, en onafhankelijk te herstellen.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een verscheidenheid aan met behulp van Azure Stream Analytics-oplossingspatronen gezien. Hierna kunt u zich verder in de materie verdiepen en uw eerste Stream Analytics-taak maken:

* [Een Stream Analytics-taak maken via Azure Portal](stream-analytics-quick-create-portal.md).
* [Create a Stream Analytics job by using Azure PowerShell](stream-analytics-quick-create-powershell.md) (Een Stream Analytics-taak maken met behulp van Azure PowerShell).
* [Create a Stream Analytics job by using Visual Studio](stream-analytics-quick-create-vs.md) (Een Stream Analytics-taak maken met behulp van Visual Studio).
