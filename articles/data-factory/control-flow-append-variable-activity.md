---
title: Activiteit in Azure Data Factory-variabele toevoegen | Microsoft Docs
description: Informatie over het instellen van de activiteit toevoeg-variabele een waarde toevoegen aan een bestaande matrixvariabele gedefinieerd in een Data Factory-pijplijn
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: 03652ce20d82565d5714cdc43a01a9e7c3074f6a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48904194"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Activiteit in Azure Data Factory-variabele toevoegen

Gebruik de variabele toevoegen-activiteit om een waarde toevoegen aan een bestaande matrixvariabele gedefinieerd in een Data Factory-pijplijn.

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit in de pijplijn | Ja
description | Een beschrijving van de werking van de activiteit | nee
type | Activiteitstype is AppendVariable | ja
waarde | Tekenreeks van letterlijke waarde of expressie object-waarde die wordt gebruikt om toe te voegen in de opgegeven variabele | ja
variableName | Naam van de variabele die wordt gewijzigd door de activiteit, de variabele moet van het type 'Array' | ja

## <a name="next-steps"></a>Volgende stappen
Meer informatie over een gerelateerde controlestroomactiviteit ondersteund door Data Factory: 

- [Activiteit-variabele instellen](control-flow-set-variable-activity.md)
