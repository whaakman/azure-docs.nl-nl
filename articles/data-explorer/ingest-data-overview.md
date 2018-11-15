---
title: Gegevensopname met Azure Data Explorer
description: Meer informatie over de verschillende manieren waarop die u (load) gegevens in Azure Data Explorer opnemen kan
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6c7d4d8d4a16e0679722f9de007870a7ec7554b0
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635996"
---
# <a name="azure-data-explorer-data-ingestion"></a>Gegevensopname met Azure Data Explorer

Opname van gegevens is het proces dat wordt gebruikt voor het laden van records met gegevens uit een of meer bronnen maken of bijwerken van een tabel in Azure Data Explorer. Zodra die zijn opgenomen, wordt de gegevens beschikbaar voor query's. Het onderstaande diagram ziet de end-to-end-stroom voor het werken in Azure Data Explorer, met inbegrip van opname van gegevens.

![Gegevensstroom](media/ingest-data-overview/data-flow.png)

De Azure Data Explorer data management service, die verantwoordelijk voor opname van gegevens is, biedt de volgende functionaliteit:

1. **Gegevens pull**: gegevens ophalen uit externe bronnen (Event Hubs) of aanvragen voor opname van een Azure-wachtrij lezen.

1. **Batchverwerking**: Batch-gegevens die binnenkomen op dezelfde database en tabel opname doorvoer te optimaliseren.

1. **Validatie**: voorbereidende validatie en indeling conversie indien nodig.

1. **Gegevensmanipulatie**: overeenkomstig schema, organiseren, indexering, codering en wanneer de gegevens worden gecomprimeerd.

1. **Persistentie punt in de stroom opname**: beheren opname belasting van de engine en verwerk nieuwe pogingen bij tijdelijke fouten.

1. **De gegevensopname doorvoeren**: maakt de gegevens beschikbaar voor query's.

## <a name="ingestion-methods"></a>Opname-methoden

Azure Data Explorer ondersteunt verschillende methoden voor gegevensopname, elk met een eigen doelscenario's mogelijk, voordelen en nadelen. Azure Data Explorer biedt pijplijnen en connectors voor het algemene services, programmatische opname met behulp van SDK's en directe toegang tot de engine voor verkenning doeleinden.

### <a name="ingestion-using-pipelines"></a>Gegevensopname met pijplijnen

Azure Data Explorer ondersteunt momenteel de Event Hub-pijplijn, die kan worden beheerd met behulp van de wizard management in Azure portal. Zie voor meer informatie, [Quick Start: opnemen van gegevens van Event Hub in Azure Data Explorer](ingest-data-event-hub.md).

### <a name="ingestion-using-connectors-and-plugins"></a>Gegevensopname met connectors en invoegtoepassingen
Azure Data Explorer ondersteunt momenteel de Logstash-invoegtoepassing. Zie voor meer informatie, [Logstash-uitvoer-invoegtoepassing voor Azure Data Explorer](https://github.com/Azure/logstash-output-kusto/blob/master/README.md).

### <a name="programmatic-ingestion"></a>Programmatische opname

Azure Data Explorer biedt SDK's die kunnen worden gebruikt voor query's en gegevens opnemen. Programmatische opname is geoptimaliseerd voor gegevensopname terugdringen omzet, door het minimaliseren van opslagtransacties tijdens en na de opname-proces.

**Beschikbare SDK's en open source-projecten**:

Kusto biedt client-SDK die kan worden gebruikt voor het opnemen en opvragen van gegevens met:

* [Python SDK](/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](/azure/kusto/api/java/kusto-java-client-library)

* [Node-SDK](/azure/kusto/api/node/kusto-node-client-library)

* [REST API](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Programmatische opname technieken**:

* Ophalen van gegevens via de Azure Data Explorer data management-service (hoge doorvoer en betrouwbare opname):

  * [**Batch-opname** ](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (geleverd door de SDK): de client de gegevens uploadt naar Azure Blob-opslag (aangegeven door de Azure Data Explorer data management-service) en een melding naar een Azure-wachtrij wordt gepost. Dit is de aanbevolen methode voor grote volumes, betrouwbare en goedkope gegevensopname.

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

Verwerkingstijd is afhankelijk van de gegevensgrootte doorgaans minder dan een paar seconden. Batchverwerking tijd standaard ingesteld op 5 minuten.

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

Voor organisaties met een bestaande infrastructuur op basis van een messaging-service zoals Event Hub is met behulp van een connector waarschijnlijk de meest geschikte oplossing. Opname in de wachtrij is geschikt voor grote hoeveelheden gegevens.

## <a name="supported-data-formats"></a>Ondersteunde gegevensindelingen voor

Voor alle opname methoden dan uit query opnemen, moet de gegevens in een van de van ondersteunde gegevensindelingen zijn geformatteerd zodat deze kan worden geparseerd door Azure Data Explorer.

* CSV, TSV, PSV, SCSV, STATUSVERKLARING
* JSON (regel gescheiden, meerdere regels) Avro
* ZIP- en GZIP 

> [!NOTE]
> Wanneer gegevens worden opgenomen, worden er gegevenstypen afgeleid op basis van de tabelkolommen doel. Als een record onvolledig is of een veld kan niet worden geparseerd als het vereiste type, wordt de bijbehorende tabelkolommen ingevuld met null-waarden.

## <a name="ingestion-recommendations-and-limitations"></a>Aanbevelingen voor gegevensopname en beperkingen
* De effectieve bewaarbeleid van opgenomen gegevens is afgeleid van beleid voor het bewaren van de database. Zie [bewaarbeleid](/azure/kusto/concepts/retentionpolicy) voor meer informatie. Ophalen van gegevens vereist **tabel ingestor** of **Database ingestor** machtigingen.
* Opname ondersteunt een maximale bestandsgrootte van 5GB. De aanbeveling is voor het opnemen van bestanden tussen 100MB en 1GB.

## <a name="schema-mapping"></a>Schematoewijzing

Schematoewijzing helpt deterministische wijze bronvelden gegevens binden aan tabelkolommen bestemming.

* [CSV-toewijzing](/azure/kusto/management/mappings?branch=master#csv-mapping) (optioneel) werkt met alle rangtelwoord op basis van indelingen en kan worden doorgegeven als de opdrachtparameter opname of [vooraf gemaakt in de tabel](/azure/kusto/management/tables?branch=master#create-ingestion-mapping) en waarnaar wordt verwezen, van de opdracht-parameter opnemen.
* [JSON-toewijzing](/azure/kusto/management/mappings?branch=master#json-mapping) (verplicht) en [Avro-toewijzing](/azure/kusto/management/mappings?branch=master#avro-mapping) (verplicht) kan worden doorgegeven als de opdrachtparameter opname of [vooraf gemaakt in de tabel](/azure/kusto/management/tables#create-ingestion-mapping) en waarnaar wordt verwezen, van de opdracht-parameter opnemen.

## <a name="next-steps"></a>Volgende stappen

[Snelstart: gegevens uit Event Hub opnemen in Azure Data Explorer](ingest-data-event-hub.md)

[Snelstart: gegevens opnemen met behulp van de Python-bibliotheek voor Azure Data Explorer](python-ingest-data.md)

[Snelstartgids: Opnemen van gegevens met behulp van het knooppunt voor Azure Data Explorer-bibliotheek](node-ingest-data.md)

