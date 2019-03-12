---
title: Activiteit in Azure Data Factory-variabele toevoegen | Microsoft Docs
description: Informatie over het instellen van de activiteit toevoeg-variabele een waarde toevoegen aan een bestaande matrixvariabele gedefinieerd in een Data Factory-pijplijn
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/09/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: a5efe946000eb00e65d314ae53d7136761e2109d
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575151"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Activiteit in Azure Data Factory-variabele toevoegen

Gebruik de variabele toevoegen-activiteit om een waarde toevoegen aan een bestaande matrixvariabele gedefinieerd in een Data Factory-pijplijn.

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Description | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit in de pijplijn | Ja
description | Een beschrijving van de werking van de activiteit | nee
type | Activiteitstype is AppendVariable | ja
waarde | Tekenreeks van letterlijke waarde of expressie object-waarde die wordt gebruikt om toe te voegen in de opgegeven variabele | ja
variableName | Naam van de variabele die wordt gewijzigd door de activiteit, de variabele moet van het type 'Array' | ja

## <a name="next-steps"></a>Volgende stappen
Meer informatie over een gerelateerde controlestroomactiviteit ondersteund door Data Factory: 

- [Activiteit-variabele instellen](control-flow-set-variable-activity.md)
