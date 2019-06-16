---
title: Kies een realtime analyses en streaming van verwerkingstechnologie in Azure
description: Meer informatie over de juiste realtime analyses en streamingtechnologie verwerking die het bouwen van uw toepassing in Azure te kiezen.
author: zhongc
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 85d6ed80da93f90e6dc0feaee7081ee3f36f1bf9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242695"
---
# <a name="choose-a-real-time-analytics-and-streaming-processing-technology-on-azure"></a>Kies een realtime analyses en streaming van verwerkingstechnologie in Azure

Er zijn diverse services beschikbaar zijn voor realtime analyses en streaming verwerking op Azure. Dit artikel bevat informatie die u nodig hebt om te bepalen welke technologie is het meest geschikt is voor uw toepassing.

## <a name="when-to-use-azure-stream-analytics"></a>Wanneer u Azure Stream Analytics

Azure Stream Analytics is een aanbevolen service voor stream analytics op Azure. Het bedoeld voor een breed scala aan scenario's die zijn, maar niet beperkt tot:

* Dashboards voor gegevensvisualisatie
* Realtime [waarschuwingen](stream-analytics-set-up-alerts.md) van tijdelijke en ruimtelijke patronen en afwijkingen
* Extraction, Transformation, Loading (ETL)
* [Gebeurtenisbronnenpatroon](/azure/architecture/patterns/event-sourcing)
* [IoT Edge](stream-analytics-edge.md)

Toevoegen van een Azure Stream Analytics-taak aan uw toepassing is de snelste manier om streaminganalyse ophalen van en die worden uitgevoerd in Azure met behulp van de SQL-taal die u al kent. Azure Stream Analytics is een service van de taak, zodat u niet hoeven te besteden aan het beheren van clusters tijd, en u geen zorgen te hoeven maken over downtime met een SLA van 99,9% op het taakniveau van de. Facturering wordt ook uitgevoerd op het taakniveau opstartkosten laag te maken (één Streaming-eenheid), maar schaalbare (maximaal 192 Streaming-eenheden). Het is veel kostenefficiënter om uit te voeren van een paar Stream Analytics-taken dan het uitvoeren en onderhouden van een cluster.

Azure Stream Analytics is een uitgebreide out-of-the-box-ervaring. U kunt onmiddellijk profiteren van de volgende functies zonder eventuele extra instellingen:

* Ingebouwde tijdelijke operators, zoals [statistische functies in vensters](stream-analytics-window-functions.md), tijdelijke joins en tijdelijke analytische functies.
* Systeemeigen Azure [invoer](stream-analytics-add-inputs.md) en [uitvoer](stream-analytics-define-outputs.md) adapters
* Ondersteuning voor het wijzigen van trage [verwijzen naar gegevens](stream-analytics-use-reference-data.md) (ook wel bekend als een opzoektabellen), met inbegrip van worden samengevoegd met georuimtelijke referentiegegevens voor geofencing.
* Geïntegreerde oplossingen, zoals [Anomaliedetectie](stream-analytics-machine-learning-anomaly-detection.md)
* Meerdere tijdvensters in dezelfde query
* De mogelijkheid voor het opstellen van meerdere tijdelijke operators in willekeurige reeksen.
* Onder de 100 ms end-to-end latentie van invoer die binnenkomen in Event Hubs, om uit te voeren landingspagina in Event Hubs, met inbegrip van de vertraging in het netwerk van en naar Event Hubs, op een hoge doorvoer

## <a name="when-to-use-other-technologies"></a>Wanneer u andere technologieën

### <a name="you-need-to-input-from-or-output-to-kafka"></a>Moet u van invoer of uitvoer met Kafka

Azure Stream Analytics niet hebben een Apache Kafka-invoer of uitvoer van de adapter. Als u gebeurtenissen terechtkomen in hebt of wilt verzenden naar Kafka en u een vereiste om uit te voeren van uw eigen Kafka-cluster hebt, kunt u echter ook doorgaan met Stream Analytics door het verzenden van gebeurtenissen naar Event Hubs met behulp van de Event Hubs Kafka-API zonder te hoeven wijzigen van de afzender van de gebeurtenis. Als u uitvoeren van uw eigen Kafka-cluster wilt, kunt u Spark Structured Streaming, die volledig wordt ondersteund op [Azure Databricks](../azure-databricks/index.yml), of problemen met Storm op [Azure HDInsight](../hdinsight/storm/apache-storm-tutorial-get-started-linux.md).

### <a name="you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c"></a>Wilt u UDF's en UDA's en aangepaste deserializers schrijven in een andere taal dan JavaScript ofC#

Azure Stream Analytics ondersteunt gebruikersgedefinieerde functies (UDF's) of de gebruiker gedefinieerde aggregaties (UDA) in JavaScript voor cloudtaken en C# voor IoT Edge-taken. C#gebruiker gedefinieerde deserializers worden ook ondersteund. Als u wilt een deserializer, een UDF of een UDA implementeren in andere talen zoals Java of Python, kunt u Spark Structured Streaming. U kunt ook uitvoeren met de Event Hubs **EventProcessorHost** op uw eigen virtuele machines willekeurige streaming verwerken.

### <a name="your-solution-is-in-a-multi-cloud-or-on-premises-environment"></a>Uw oplossing is in een omgeving met meerdere cloud of on-premises

Azure Stream Analytics is eigen technologie van Microsoft en is alleen beschikbaar op Azure. Als u uw draagbare zijn voor Clouds of on-premises-oplossing nodig hebt, kunt u de open-source-technologieën, zoals Spark Structured Streaming of Storm.

## <a name="next-steps"></a>Volgende stappen

* [Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)
* [Een Stream Analytics-taak maken met behulp van Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Een Stream Analytics-taak maken met behulp van Visual Studio](stream-analytics-quick-create-vs.md)
* [Een Stream Analytics-taak maken met behulp van Visual Studio Code](quick-create-vs-code.md)