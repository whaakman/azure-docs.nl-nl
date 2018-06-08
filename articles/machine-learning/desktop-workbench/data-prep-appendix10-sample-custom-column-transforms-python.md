---
title: Python steekproef voor het afleiden van nieuwe kolommen in Azure Machine Learning gegevens voorbereiden | Microsoft Docs
description: Dit document vindt voorbeelden van Python-code voor het maken van nieuwe kolommen in Azure Machine Learning gegevens voorbereiden.
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
ms.openlocfilehash: babe7b8dd1ea459fd3478d20dd7d69138c9ca163
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830092"
---
# <a name="sample-of-custom-column-transforms-python"></a>Voorbeeld van aangepaste kolom transformaties (Python) 
De naam van deze transformatie in het menu is **kolom toevoegen (Script)**.

Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Equivalentie testen en vervang waarden 
Als de waarde in Col1 minder dan 4 is, moet de nieuwe kolom die de waarde 1 hebben. Als de waarde in Col1 meer dan 4 is, heeft de nieuwe kolom die de waarde 2. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Huidige datum en tijd 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Typecasting 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Voor nullness evalueren 
Als Col1 een null-waarde bevat, markeert u de nieuwe kolom die als **onjuiste**. Als dat niet markeren als **goed.** 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Nieuwe berekende kolom 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Epoche berekening 
Het aantal seconden sinds de Unix-epoche (uitgaande Col1 is al een datum): 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Hash-waarde in de kolom in een nieuwe kolom
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




