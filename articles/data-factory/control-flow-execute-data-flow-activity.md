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
ms.openlocfilehash: 24b27c16573a35b1d8749d7ff381fbef970f4bd0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471659"
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

### <a name="debugging-pipelines-with-data-flows"></a>Opsporen van fouten in pijplijnen met gegevensstromen

![Fouten opsporen in knop](media/data-flow/debugbutton.png "knop foutopsporing")

Gebruik de gegevens stromen foutopsporing gebruikmaken van een verwarmde cluster voor het testen van uw gegevensstromen interactief in een pijplijn foutopsporing uitvoeren. Gebruik de optie pijplijn fouten opsporen in uw gegevensstromen binnen een pijplijn testen.

### <a name="run-on"></a>Uitgevoerd op

Dit is een verplicht veld waarmee wordt gedefinieerd welke Integration Runtime moet worden gebruikt voor de uitvoering van de activiteit gegevens stromen. Data Factory gebruikt standaard de standaard automatisch oplossen Azure Integration runtime. U kunt echter uw eigen Azure-Integratieruntimes die specifieke regio's definiëren, compute-type, kerngeheugens en TTL-waarde voor de uitvoering van de activiteit van de gegevensstroom maken.

De standaardinstelling voor uitvoeringen gegevensstroom is 8 kernen reken-en algemene met een TTL-waarde van 60 minuten.

Kies de compute-omgeving voor deze uitvoering van de gegevensstroom. De standaardwaarde is de standaard Integratieruntime voor Azure automatisch oplossen. Met deze optie wordt de gegevensstroom op de Spark-omgeving in dezelfde regio als uw data factory uitgevoerd. Het rekentype wordt een cluster taak, wat betekent dat de compute-omgeving duurt enkele minuten om lid te worden.

U hebt controle over de Spark-uitvoeringsomgeving voor uw activiteiten gegevensstroom. In de [Azure integratieruntime](concepts-integration-runtime.md) zijn instellingen voor het instellen van het type compute (algemeen gebruik, geoptimaliseerd voor geheugen en geoptimaliseerde rekenkracht), aantal worker-kernen en time-to-live zodat deze overeenkomen met de engine voor het uitvoeren met uw compute gegevensstroom vereisten. Bovendien kunt TTL instelling u een warm-cluster dat is onmiddellijk beschikbaar voor taakuitvoeringen onderhouden.

![Azure Integratieruntime](media/data-flow/ir-new.png "Azure Integratieruntime")

> [!NOTE]
> De Integration Runtime-selectie in de activiteit gegevensstroom is alleen van toepassing op *geactiveerde uitvoeringen* van uw pijplijn. Foutopsporing van uw pijplijn met de gegevens stromen met foutopsporing wordt uitgevoerd op de standaardwaarde van 8-core Spark-cluster.

### <a name="staging-area"></a>Faseringsgebied

Als u zijn uw gegevens zich in Azure Data Warehouse, moet u een tijdelijke locatie kiezen voor het laden van de batch Polybase. De instellingen voor fasering zijn alleen van toepassing op Azure Data Warehouse-workloads.

## <a name="parameterized-datasets"></a>Gegevenssets met parameters

Als u geparameteriseerde gegevenssets gebruikt, zorg er dan voor dat de parameterwaarden instellen.

![Parameters voor stroom uitvoeren](media/data-flow/params.png "Parameters")

## <a name="parameterized-data-flows"></a>Met parameters gegevensstromen

Als u parameters in de gegevensstroom hebt, stelt u de dynamische waarden van uw stroom gegevensparameters hier in de sectie Parameters van de activiteit gegevensstroom uitvoeren. U kunt de ADF-pijplijn expressietaal (alleen voor de parameter tekenreekstypen) of de expressietaal gegevens Flow gebruiken de parameterwaarden met dynamische expressies of letterlijke statische waarden in te stellen.

![Voorbeeld van de gegevensstroom Parameter Execute](media/data-flow/parameter-example.png "Parameter voorbeeld")

### <a name="debugging-data-flows-with-parameters"></a>Gegevens voor foutopsporing stromen met parameters

Op dit moment kunt u gegevensstromen met parameters van de pijplijn foutopsporing uitvoeren met behulp van de activiteit execute gegevensstroom alleen fouten opsporen. Interactieve debug-sessies in ADF gegevensstroom is binnenkort beschikbaar. Uitvoeringen van pijplijn en foutopsporing wordt uitgevoerd, maar werkt met parameters.

Er is een goede gewoonte om te maken van de gegevensstroom met statische inhoud zodat u beschikt over de metagegevens van de volledige kolom doorgifte beschikbaar tijdens de ontwerpfase voor probleemoplossing. Vervolgens de statische gegevensset vervangen door een gegevensset voor dynamische parameters, wanneer u uw stroom gegevenspijplijn operationeel maken.

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
