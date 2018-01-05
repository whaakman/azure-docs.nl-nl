---
title: Voorbeeld filterexpressies mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een een reeks voorbeelden van filterexpressies mogelijk met Azure Machine Learning gegevens voorbereiden
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: ae70f5981c7ca533740968e0eea1b0b83df414fa
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
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
