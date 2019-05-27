---
title: Stroomactiviteit gegevens uitvoeren in Azure Data Factory | Microsoft Docs
description: Het uitvoeren van de gegevens stromen van binnen een data factory-pijplijn.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: b0a6c6feae11f8daeed54c5e763dbff3aa711652
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153504"
---
# <a name="execute-data-flow-activity-in-azure-data-factory"></a>Stroomactiviteit gegevens uitvoeren in Azure Data Factory
De activiteit execute gegevens flow voor uw gegevensstroom ADF in Foutopsporing (sandbox) pijplijnuitvoeringen en geactiveerde pijplijnuitvoeringen gebruiken.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "dataflow1",
         "type": "DataFlowReference"
      },
        "compute": {
          "computeType": "General",
          "coreCount": 8,
      }
}

```

## <a name="type-properties"></a>Type-eigenschappen

* ```dataflow``` de naam van de flow-entiteit voor gegevens die u wilt uitvoeren
* ```compute``` Beschrijving van de uitvoeringsomgeving Spark
* ```coreCount``` is het aantal cores om toe te wijzen voor de uitvoering van deze activiteit van de gegevensstroom

![Uitvoeren van de gegevensstroom](media/data-flow/activity-data-flow.png "gegevensstroom uitvoeren")

### <a name="run-on"></a>Uitvoeren op

Kies de compute-omgeving voor deze uitvoering van de gegevensstroom. De standaardwaarde is de standaard Integratieruntime voor Azure automatisch oplossen. Met deze optie wordt de gegevensstroom op de Spark-omgeving in dezelfde regio als uw data factory uitgevoerd. Het rekentype wordt een cluster taak, wat betekent dat de compute-omgeving duurt enkele minuten om lid te worden.

### <a name="debugging-pipelines-with-data-flows"></a>Opsporen van fouten in pijplijnen met gegevensstromen

![Fouten opsporen in knop](media/data-flow/debugbutton.png "knop foutopsporing")

Gebruik de gegevens stromen foutopsporing gebruikmaken van een verwarmde cluster voor het testen van uw gegevensstromen interactief in een pijplijn foutopsporing uitvoeren. Gebruik de optie pijplijn fouten opsporen in uw gegevensstromen binnen een pijplijn testen.

### <a name="run-on"></a>Uitvoeren op

Dit is een verplicht veld waarmee wordt gedefinieerd welke Integration Runtime moet worden gebruikt voor de uitvoering van de activiteit gegevens stromen. Data Factory gebruikt standaard de standaard automatisch oplossen Azure Integration runtime. U kunt echter uw eigen Azure-Integratieruntimes die specifieke regio's definiëren, compute-type, kerngeheugens en TTL-waarde voor de uitvoering van de activiteit van de gegevensstroom maken.

De standaardinstelling voor uitvoeringen gegevensstroom is 8 kernen reken-en algemene met een TTL-waarde van 60 minuten.

U hebt controle over de Spark-uitvoeringsomgeving voor uw activiteiten gegevensstroom. In de [Azure integratieruntime](concepts-integration-runtime.md) zijn instellingen voor het instellen van het type compute (algemeen gebruik, geoptimaliseerd voor geheugen en geoptimaliseerde rekenkracht), aantal worker-kernen en time-to-live zodat deze overeenkomen met de engine voor het uitvoeren met uw compute gegevensstroom vereisten. Bovendien kunt TTL instelling u een warm-cluster dat is onmiddellijk beschikbaar voor taakuitvoeringen onderhouden.

![Azure Integratieruntime](media/data-flow/ir-new.png "Azure Integratieruntime")

### <a name="staging-area"></a>Faseringsgebied

Als u zijn uw gegevens zich in Azure Data Warehouse, moet u een tijdelijke locatie kiezen voor het laden van de batch Polybase.

## <a name="parameterized-datasets"></a>Gegevenssets met parameters

Als u geparameteriseerde gegevenssets gebruikt, zorg er dan voor dat de parameterwaarden instellen.

![Parameters voor stroom uitvoeren](media/data-flow/params.png "Parameters")

### <a name="debugging-parameterized-data-flows"></a>Foutopsporing met parameters gegevensstromen

U kunt alleen fouten opsporen gegevensstromen met geparameteriseerde gegevenssets van de pijplijn foutopsporing uitvoeren met behulp van de activiteit execute gegevensstroom. Interactieve debug-sessies in ADF-gegevensstroom op dit moment werken niet met geparameteriseerde gegevenssets. Uitvoeringen van pijplijn en foutopsporing wordt uitgevoerd, werkt met parameters.

Er is een goede gewoonte om te maken van de gegevensstroom met een statische gegevensset zodat u beschikt over de metagegevens van de volledige kolom doorgifte beschikbaar op het moment van ontwerp. Vervolgens de statische gegevensset vervangen door een gegevensset voor dynamische parameters, wanneer u uw stroom gegevenspijplijn operationeel maken.

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
