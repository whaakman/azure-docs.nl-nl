---
title: Voorbeeld filterexpressies mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een een reeks voorbeelden van filterexpressies mogelijk met Azure Machine Learning gegevens voorbereiden
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 434da595762a077b94ff034325d5bdb156585884
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830357"
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
