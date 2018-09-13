---
title: Voorbeeld van de transformatie gegevensstroom transformaties mogelijk is met gegevensvoorbereiding van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een aantal voorbeelden van transformatie transformaties voor gegevensstromen mogelijk is met Azure Machine Learning-gegevensvoorbereiding
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
ms.openlocfilehash: ca780b51973a960caec3b9b7a80c8ba5621b5a0b
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646061"
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Voorbeeld van aangepaste transformaties voor gegevensstromen (Python) 
De naam van de transformatie in het menu is **gegevensstroom transformeren (Script)**. Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Frame transformeren
### <a name="create-a-new-column-dynamically"></a>Maak een nieuwe kolom dynamisch 
Hiermee maakt u een kolom dynamisch (**plaats2**) en voor overeenstemming zorgt met meerdere verschillende versies van San Francisco op een van de bestaande kolom plaats.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Nieuwe statistische functies toevoegen
Maakt een nieuw frame met de eerste en laatste statistische functies berekend voor de score-kolom. Deze zijn gegroepeerd op de **risk_category** kolom.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize een kolom 
Opnieuw wordt geformuleerd de gegevens om te voldoen aan een formule voor het verminderen van de uitschieters in een kolom.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Gegevensstroom transformeren
### <a name="fill-down"></a>Omlaag doorvoeren 

Omlaag doorvoeren vereist twee transformaties. Wordt ervan uitgegaan dat gegevens die lijkt op de volgende tabel:

|Status         |Plaats       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Breda   |
|              |Seattle    |
|Californië    |Los Angeles|
|              |San Diego  |
|              |San Jose   |
|Texas         |Dallas     |
|              |San Antonio|
|              |Houston    |

1. Maak een 'Toevoegen (Script) kolom' transformeren met behulp van de volgende code:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Maak een "Gegevensstroom transformeren (Script)"-transformatie die de volgende code bevat:
```python
    df = df.fillna( method='pad')
```

De gegevens er nu uitzien zoals in de volgende tabel:

|Status         |newState         |Plaats       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Breda   |
|              |Washington    |Seattle    |
|Californië    |Californië    |Los Angeles|
|              |Californië    |San Diego  |
|              |Californië    |San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Min-max normalisering
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```