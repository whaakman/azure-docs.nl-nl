---
title: Voorbeeld van filterexpressies mogelijk is met het voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een aantal voorbeelden van filterexpressies mogelijk is met Azure Machine Learning-gegevensvoorbereiding
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: a960197c89e1d051de0e9b8b73cbab231982cf52
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216495"
---
# <a name="sample-of-filter-expressions-python"></a>Voorbeeld van filterexpressies (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Filteren met equivalentie testen
In alleen de rijen waarvoor de waarde van (numerieke) Col2 groter dan 4 is filteren. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Filteren met meerdere kolommen 
Filter alleen de rijen waar Col1 de waarde bevat **goede** en Col2 bevat de (numerieke) waarde 1. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Test-filter op basis van null
Filter in alleen de rijen waarin Col1 een null-waarde heeft. 
```python
    pd.isnull(row["Col1"])
```
