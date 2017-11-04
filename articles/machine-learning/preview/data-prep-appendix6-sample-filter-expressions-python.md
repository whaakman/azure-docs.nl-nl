---
title: Voorbeeld filterexpressies mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een een reeks voorbeelden van filterexpressies mogelijk met Azure Machine Learning gegevens voorbereiden
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: f8b18b2a94885a76df4026a29400f4d651cbcad4
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-filter-expressions-python"></a>Voorbeeld van filterexpressies (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Filteren met equivalentie test
In alleen de rijen waarvoor de waarde van Col2 (numeriek) groter dan 4 is filteren. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Filteren met meerdere kolommen 
Filteren in alleen die rijen Col1 waar de waarde bevat **goede** en Col2 bevat de (numeriek) waarde 1. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Test filter tegen null
Filter in alleen die rijen waar Col1 een null-waarde heeft. 
```python
    pd.isnull(row["Col1"])
```
