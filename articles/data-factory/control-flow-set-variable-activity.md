---
title: Activiteit-variabele instellen in Azure Data Factory | Microsoft Docs
description: Meer informatie over het gebruik van de activiteit van de variabele instellen om in te stellen de waarde van een bestaande variabele gedefinieerd in een Data Factory-pijplijn
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 71abfdff629f36b278488851b546c7371353a4d9
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575287"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Activiteit-variabele instellen in Azure Data Factory

Gebruik de variabele instellen-activiteit om de waarde van een bestaande variabele van het type String, Bool of matrix die is gedefinieerd in een Data Factory-pijplijn instellen.

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Description | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit in de pijplijn | Ja
description | Een beschrijving van de werking van de activiteit | nee
type | Activiteitstype is SetVariable | ja
waarde | Tekenreeks van letterlijke waarde of expressie objectwaarde gebruikt voor het opgegeven variabele instellen | ja
variableName | Naam van de variabele die door deze activiteit worden ingesteld | ja


## <a name="next-steps"></a>Volgende stappen
Meer informatie over een gerelateerde controlestroomactiviteit ondersteund door Data Factory: 

- [Variabele activiteit toevoegen](control-flow-append-variable-activity.md)
