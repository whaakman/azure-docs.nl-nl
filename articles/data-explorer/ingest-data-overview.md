---
title: Gegevensopname met Azure Data Explorer
description: Meer informatie over de verschillende manieren waarop die u (load) gegevens in Azure Data Explorer opnemen kan
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 2/18/2019
ms.openlocfilehash: bd1cb101a3bf9a4da9c681169d2a5c999e56b1b3
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57438411"
---
# <a name="azure-data-explorer-data-ingestion"></a>Gegevensopname met Azure Data Explorer

Opname van gegevens is het proces dat wordt gebruikt voor het laden van records met gegevens uit een of meer bronnen maken of bijwerken van een tabel in Azure Data Explorer. Zodra die zijn opgenomen, wordt de gegevens beschikbaar voor query's. Het onderstaande diagram ziet de end-to-end-stroom voor het werken in Azure Data Explorer, met inbegrip van opname van gegevens.

![Gegevensstroom](media/ingest-data-overview/data-flow.png)

De Azure Data Explorer data management service, die verantwoordelijk voor opname van gegevens is, biedt de volgende functionaliteit:

1. **Gegevens pull**: Gegevens ophalen uit externe bronnen (Event Hubs) of aanvragen voor opname van een Azure-wachtrij lezen.

1. **Batchverwerking**: Batchgegevens doorgestuurd naar de dezelfde database en tabel opname doorvoer te optimaliseren.

1. **Validatie**: Voorbereidende validatie en indeling conversie indien nodig.

1. **Gegevensmanipulatie**: Overeenkomende schema, organiseren, indexering, codering en wanneer de gegevens worden gecomprimeerd.

1. **Persistentie punt in de stroom opname**: Opname-belasting van de engine voor het beheren en nieuwe pogingen bij tijdelijke fouten afhandelen.

1. **De gegevensopname doorvoeren**: Hiermee maakt de gegevens beschikbaar voor query's.

## <a name="ingestion-methods"></a>Opname-methoden

Azure Data Explorer ondersteunt verschillende methoden voor gegevensopname, elk met een eigen doelscenario's mogelijk, voordelen en nadelen. Azure Data Explorer biedt pijplijnen en connectors voor het algemene services, programmatische opname met behulp van SDK's en directe toegang tot de engine voor verkenning doeleinden.

### <a name="ingestion-using-pipelines-connectors-and-plugins"></a>Gegevensopname met pijplijnen, connectors en invoegtoepassingen

Azure Data Explorer ondersteunt op dit moment:

* Event Grid pijplijn, die kan worden beheerd met behulp van de wizard management in Azure portal. Zie voor meer informatie, [opnemen Azure Blobs in Azure Data Explorer](ingest-data-event-grid.md).

* Event Hub pijplijn, die kan worden beheerd met behulp van de wizard management in Azure portal. Zie voor meer informatie, [opnemen van gegevens van Event Hub in Azure Data Explorer](ingest-data-event-hub.md).

* Logstash-invoegtoepassing, Zie [opname van gegevens vanuit Logstash naar Azure Data Explorer](ingest-data-logstash.md).

* Kafka-connector, Zie [opnemen van gegevens van Kafka in Azure Data Explorer](ingest-data-kafka.md).

### <a name="ingestion-using-integration-services"></a>Gegevensopname met integratieservices

* Azure Data Factory (ADF), een volledig beheerde service voor gegevensintegratie voor analytische workloads in Azure, om gegevens te kopiëren naar en van Azure Data Explorer. Zie voor meer informatie, [gegevens kopiëren naar of van Azure Data Explorer met behulp van Azure Data Factory](/azure/data-factory/connector-azure-data-explorer).

### <a name="programmatic-ingestion"></a>Programmatische opname

Azure Data Explorer biedt SDK's die kunnen worden gebruikt voor query's en gegevens opnemen. Programmatische opname is geoptimaliseerd voor gegevensopname terugdringen omzet, door het minimaliseren van opslagtransacties tijdens en na de opname-proces.

**Beschikbare SDK's en open-source-projecten**:

Kusto biedt client-SDK die kan worden gebruikt voor het opnemen en opvragen van gegevens met:

* [Python SDK](/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](/azure/kusto/api/java/kusto-java-client-library)

* [Node-SDK](/azure/kusto/api/node/kusto-node-client-library)

* [REST API](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Programmatische opname technieken**:

* Ophalen van gegevens via de Azure Data Explorer data management-service (hoge doorvoer en betrouwbare opname):

    [**Batch-opname** ](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (geleverd door de SDK): de client de gegevens uploadt naar Azure Blob-opslag (aangegeven door de Azure Data Explorer data management-service) en een melding naar een Azure-wachtrij wordt gepost. Opname van de batch is de aanbevolen methode voor grote volumes, betrouwbare, en goedkope gegevensopname.

* Ophalen van gegevens rechtstreeks in de Azure Data Explorer-engine (meest geschikt voor verkenning en ontwikkelen van prototypen):

  * **Inline-opname**: besturings (.ingest inline) met in-band-gegevens is bedoeld voor ad-hoc voor testdoeleinden.

  * **Opname van query**: besturings (.set, .set-of-toe te voegen, .set of vervangen) die naar de resultaten van query verwijst wordt gebruikt voor het genereren van rapporten of kleine tijdelijke tabellen.

  * **Opnemen uit de opslag**: besturings (.ingest in) met gegevens die extern zijn opgeslagen (bijvoorbeeld Azure Blob Storage) kunt u efficiënt bulksgewijs opnemen van gegevens.

**Latentie van verschillende methoden**:

| Methode | Latentie |
| --- | --- |
| **Inline-opname** | Onmiddellijk |
| **Opname van query** | Uitvoeren van query's + verwerkingstijd |
| **Opnemen uit de opslag** | Downloadtijd + verwerkingstijd |
| **Opname in de wachtrij** | Batchverwerking tijd + verwerkingstijd |
| |

Verwerkingstijd is afhankelijk van de gegevensgrootte, minder dan een paar seconden. Batchverwerking tijd standaard ingesteld op 5 minuten.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>De meest geschikte methode voor de opname te kiezen

Voordat u begint met het opnemen van gegevens, moet u uzelf de volgende vragen stellen.

* Waar mijn gegevens opgeslagen? 
* Wat is de gegevensindeling en kan worden gewijzigd? 
* Wat zijn de vereiste velden moet worden gezocht? 
* Wat is de verwachte gegevensvolume en uit te voeren? 
* Hoeveel gebeurtenistypen worden verwacht (weergegeven als het aantal tabellen)? 
* Hoe vaak de gebeurtenissenschema naar verwachting wijzigen? 
* Hoeveel knooppunten worden de gegevens genereren? 
* Wat is de bron-OS? 
* Wat zijn de latentievereisten? 
* Kan een van de bestaande beheerde opname-pijplijnen worden gebruikt? 

Voor organisaties met een bestaande infrastructuur die zijn gebaseerd op een messaging-service zoals Event Hub, is met behulp van een connector waarschijnlijk het meest geschikte oplossing. Opname in de wachtrij is geschikt voor grote hoeveelheden gegevens.

## <a name="supported-data-formats"></a>Ondersteunde gegevensindelingen voor

Voor alle opname methoden dan uit query opnemen, moet u de gegevens opmaken zodat deze kan worden geparseerd door Azure Data Explorer. De van ondersteunde gegevensindelingen zijn:

* CSV, TSV, PSV, SCSV, SOH
* JSON (regel gescheiden, meerdere regels) Avro
* ZIP- en GZIP 

> [!NOTE]
> Wanneer gegevens worden opgenomen, worden er gegevenstypen afgeleid op basis van de tabelkolommen doel. Als een record onvolledig is of een veld kan niet worden geparseerd als het vereiste type, wordt de bijbehorende tabelkolommen ingevuld met null-waarden.

## <a name="ingestion-recommendations-and-limitations"></a>Aanbevelingen voor gegevensopname en beperkingen

* De effectieve bewaarbeleid van opgenomen gegevens is afgeleid van beleid voor het bewaren van de database. Zie [bewaarbeleid](/azure/kusto/concepts/retentionpolicy) voor meer informatie. Ophalen van gegevens vereist **tabel ingestor** of **Database ingestor** machtigingen.
* Opname ondersteunt een maximale bestandsgrootte van 5 GB. De aanbeveling is voor het opnemen van bestanden tussen 100 MB en 1 GB.

## <a name="schema-mapping"></a>Schematoewijzing

Schematoewijzing helpt bronvelden gegevens binden aan tabelkolommen bestemming.

* [CSV-toewijzing](/azure/kusto/management/mappings?branch=master#csv-mapping) (optioneel) werkt met alle op basis van het rangtelwoord voor indelingen. Kan worden uitgevoerd met behulp van de opdrachtparameter opname of [vooraf gemaakt in de tabel](/azure/kusto/management/tables?branch=master#create-ingestion-mapping) en waarnaar wordt verwezen, van de opdracht-parameter opnemen.
* [JSON-toewijzing](/azure/kusto/management/mappings?branch=master#json-mapping) (verplicht) en [Avro-toewijzing](/azure/kusto/management/mappings?branch=master#avro-mapping) (verplicht) kan worden uitgevoerd met behulp van de opdrachtparameter opnemen. Ze kunnen ook worden [vooraf gemaakt in de tabel](/azure/kusto/management/tables#create-ingestion-mapping) en waarnaar wordt verwezen, van de opdracht-parameter opnemen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Opnemen van gegevens van Event Hub in Azure Data Explorer](ingest-data-event-hub.md)

> [!div class="nextstepaction"]
> [Opname van gegevens met behulp van Event Grid-abonnement in Azure Data Explorer](ingest-data-event-grid.md)

> [!div class="nextstepaction"]
> [Opnemen van gegevens van Kafka in Azure Data Explorer](ingest-data-kafka.md)

> [!div class="nextstepaction"]
> [Opname van gegevens met behulp van de Azure Data Explorer Python-bibliotheek](python-ingest-data.md)

> [!div class="nextstepaction"]
> [Opname van gegevens met behulp van het knooppunt voor Azure Data Explorer-bibliotheek](node-ingest-data.md)

> [!div class="nextstepaction"]
> [Opname van gegevens met behulp van de Azure Data Explorer .NET Standard SDK (Preview)](net-standard-ingest-data.md)

> [!div class="nextstepaction"]
> [Opname van gegevens vanuit Logstash naar Azure Data Explorer](ingest-data-logstash.md)
