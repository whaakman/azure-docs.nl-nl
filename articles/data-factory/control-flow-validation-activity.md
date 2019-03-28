---
title: Standaardactiviteit voor groepsvalidatie in Azure Data Factory | Microsoft Docs
description: De activiteit voor groepsvalidatie wordt uitvoering van de pijplijn niet verder voordat valideert de gekoppelde gegevensset aan bepaalde criteria die de gebruiker opgeeft.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 46447bdbea93d1f99c5682cf878c2035e6f49b78
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522947"
---
# <a name="validation-activity-in-azure-data-factory"></a>Standaardactiviteit voor groepsvalidatie in Azure Data Factory
U kunt een validatie in een pijplijn gebruiken om te controleren of de pijplijn blijft alleen kan worden uitgevoerd nadat deze is gevalideerd/de bijgevoegde gegevenssetverwijzing bestaat, dat deze voldoet aan de opgegeven criteria of time-out is bereikt.


## <a name="syntax"></a>Syntaxis

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de activiteit 'Validatie' | String | Ja |
type | Moet worden ingesteld op **validatie**. | String | Ja |
Gegevensset | Activiteit wordt de uitvoering blokkeren totdat ze deze gegevenssetverwijzing heeft gevalideerd bestaat en dat het voldoet aan de opgegeven criteria of time-out is bereikt. Gegevensset die opgegeven moet de eigenschap "MinimumSize" of "ChildItems" ondersteunen. | Gegevenssetverwijzing | Ja |
timeout | Hiermee geeft u de time-out op voor de activiteit die moet worden uitgevoerd. Als er geen waarde is opgegeven, is de standaardwaarde 7 dagen ("7.00:00:00"). De indeling is d.hh:mm:ss | String | Nee |
Slaapstand | Een vertraging in seconden tussen pogingen voor validatie. Als er geen waarde is opgegeven, is standaard de waarde 10 seconden. | Geheel getal | Nee |
childItems | Controleert of de map heeft onderliggende items. U kunt instellen op true: Valideren dat de map bestaat en dat deze items heeft. Geblokkeerd tot ten minste één item aanwezig in de map is of time-outwaarde is bereikt.-de waarde false: Valideren dat de map bestaat en of deze leeg is. Geblokkeerd totdat de map is leeg of totdat de time-waarde is bereikt. Als er geen waarde is opgegeven, worden de activiteit wordt geblokkeerd totdat de map bestaat of time-out is bereikt. | Booleaans | Nee |
minimumSize | Minimale grootte van een bestand in bytes. Als er geen waarde is opgegeven, de standaardwaarde is 0 bytes | Geheel getal | Nee |


## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund:

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
