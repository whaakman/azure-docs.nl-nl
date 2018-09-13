---
title: Voorbeeld van Python voor het afleiden van nieuwe kolommen in Azure Machine Learning-gegevensvoorbereiding | Microsoft Docs
description: Dit document bevat voorbeelden van Python-code voor het nieuwe kolommen maken in Azure Machine Learning gegevens voor te bereiden.
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
ms.openlocfilehash: d9f8bb20325ff15b6ba67253de5e66d1d1d8e643
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643973"
---
# <a name="sample-of-custom-column-transforms-python"></a>Voorbeeld van aangepaste kolomtransformaties (Python) 
De naam van deze transformatie in het menu is **kolom toevoegen (Script)**.

Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Gelijkwaardige testen en vervang de waarden 
Als de waarde in Col1 minder dan 4 is, moet de waarde 1 hebben op de nieuwe kolom. Als de waarde in Col1 meer dan 4 is, heeft de nieuwe kolom die de waarde 2. 

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
Als Col1 een null-waarde bevat, markeert u vervolgens de nieuwe kolom als **ongeldige**. Zo niet, markeer deze als **goed.** 

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

## <a name="hash-a-column-value-into-a-new-column"></a>Hash van een waarde in de kolom in een nieuwe kolom
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




