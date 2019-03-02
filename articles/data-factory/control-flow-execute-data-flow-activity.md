---
title: Stroomactiviteit gegevens uitvoeren in Azure Data Factory | Microsoft Docs
description: De activiteit execute gegevensstroom wordt uitgevoerd gegevensstromen.
services: data-factory
documentationcenter: ''
author: kromerm
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: 17bcb9efbf28901f241d44231bc5c71ae20380b5
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241609"
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

Als u ervoor een toegewezen IR kiest, kunt u een nieuwe Azure-IR maken met een vastgemaakte regio en compute-grootten die voldoen aan de vereisten van uw gegevensstroom. Deze optie wordt kringveld-up interactieve clusters, die lid is wordt onmiddellijk nadat de eerste taak is verzonden. Dit cluster blijft actief totdat de TTL verloopt nadat de laatste taak is uitgevoerd.

### <a name="compute-type"></a>Rekentype

U kunt kiezen voor algemene doeleinden, geoptimaliseerd voor berekenen of geoptimaliseerd voor geheugen, al naar gelang de vereisten van uw gegevensstroom.

### <a name="core-count"></a>Aantal kernen

Kies hoeveel cores u wilt toewijzen aan de taak. Voor kleinere taken werkt minder cores beter.

### <a name="staging-area"></a>Faseringsgebied

Als u zijn uw gegevens zich in Azure Data Warehouse, moet u een tijdelijke locatie kiezen voor het laden van de batch Polybase.

## <a name="parameterized-datasets"></a>Gegevenssets met parameters

Als u geparameteriseerde gegevenssets gebruikt, zorg er dan voor dat de parameterwaarden instellen.

![Parameters voor stroom uitvoeren](media/data-flow/params.png "Parameters")

### <a name="debugging-parameterized-data-flows"></a>Foutopsporing met parameters gegevensstromen

U kunt alleen fouten opsporen gegevensstromen met geparameteriseerde gegevenssets van de pijplijn foutopsporing uitvoeren met behulp van de activiteit execute gegevensstroom. Interactieve debug-sessies in ADF-gegevensstroom op dit moment werken niet met geparameteriseerde gegevenssets. Uitvoeringen van pijplijn en foutopsporing wordt uitgevoerd, werkt met parameters.

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
