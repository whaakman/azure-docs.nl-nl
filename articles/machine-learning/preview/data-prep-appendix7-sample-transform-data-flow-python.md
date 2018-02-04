---
title: Voorbeeld van de transformatie gegevensstroom transformaties mogelijk met Azure Machine Learning gegevens voorbereiden | Microsoft Docs
description: Dit document bevat een reeks voorbeelden van gegevensstroom transformaties transformatie mogelijk met Azure Machine Learning gegevens voorbereiden
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
ms.date: 02/01/2018
ms.openlocfilehash: 8146c2a41a2b8fc241131a42ec74227795867609
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Voorbeeld van aangepaste gegevensstroom transformaties (Python) 
De naam van de transformatie in het menu is **transformeren gegevensstroom (Script)**. Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Frame transformeren
### <a name="create-a-new-column-dynamically"></a>Maak een nieuwe kolom dynamisch 
Een kolom dynamisch gemaakt (**city2**) en voor overeenstemming zorgt met meerdere verschillende versies van San Francisco op een van de bestaande kolom plaats.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Nieuwe statistische functies toevoegen
Maakt een nieuw frame met de eerste en laatste aggregaties voor de kolom score wordt berekend. Deze zijn gegroepeerd op de **risk_category** kolom.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize een kolom 
Opnieuw wordt geformuleerd de gegevens om te voldoen aan een formule voor het beperken van de uitschieters in een kolom.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Gegevensstroom transformeren
### <a name="fill-down"></a>Omlaag doorvoeren 
Omlaag doorvoeren vereist twee transformaties. Wordt ervan uitgegaan dat gegevens die op het volgende lijkt:


|Status         |Plaats       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Californië    |Los Angeles|
|              |San Diego  |
|              |San Jose   |
|Texas         |Dallas     |
|              |San Antonio|
|              |Houston    |

Maak eerst een kolom toevoegen (Script)-transformatie die de volgende code bevat:
```python
    row['State'] if len(row['State']) > 0 else None
```
Nu een transformatie-gegevensstroom (Script) transformatiebestand maken dat de volgende code bevat:
```python
    df = df.fillna( method='pad')
```

De gegevens ziet er nu als volgt uit:

|Status         |newState         |Plaats       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |Seattle    |
|Californië    |Californië    |Los Angeles|
|              |Californië    |San Diego  |
|              |Californië    |San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Min-max normalisatie
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```