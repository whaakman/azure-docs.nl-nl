---
title: Uitbreidbaarheid voor Python gebruiken met Azure Machine Learning Data Preparations | Microsoft Docs
description: Dit document bevat een overzicht en gedetailleerde voorbeelden van hoe u Python-code gebruikt om uit te breiden de functionaliteit van gegevens voor te bereiden
services: machine-learning
author: euangMS
ms.author: euang
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.openlocfilehash: a713f5fcde31e0e25de080a65b71209011ef551d
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645901"
---
# <a name="data-preparations-python-extensions"></a>Data Preparations Python-extensies
Als een manier om functionaliteit onderbrekingen tussen de ingebouwde functies invullen, bevat Azure Machine Learning Data Preparations uitbreidbaarheid op meerdere niveaus. In dit document beschrijven we de uitbreidbaarheid via Python-script. 

## <a name="custom-code-steps"></a>Aangepaste code stappen 
Data Preparations heeft de volgende aangepaste stappen waarin gebruikers code kunnen schrijven:

* Kolom toevoegen
* Geavanceerd filter
* Gegevensstroom transformeren
* Partitie transformeren

## <a name="code-block-types"></a>Typen van code blokkeren 
We ondersteunen twee typen van code blokkeren voor elk van deze stappen. Eerst wordt een bare Python-expressie die wordt uitgevoerd, wordt ondersteund. Ten tweede ondersteunen we een Python-Module wanneer een specifieke functie met een handtekening van bekende genoemd in de code die u opgeeft.

U kunt bijvoorbeeld een nieuwe kolom die het logboek van een andere kolom berekend in de volgende twee manieren toevoegen:

Expressie 

```python    
    math.log(row["Score"])
```

Module 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


De transformatie kolom toevoegen in de modus van de Module wordt verwacht dat een functie met de naam zoeken `newvalue` die een rij-variabele accepteert en retourneert de waarde voor de kolom. Deze module kan een aantal van de Python-code met andere functies, invoer, enzovoort bevatten.

De details van elk extensiepunt worden in de volgende secties besproken. 

## <a name="imports"></a>Invoer 
Als u het type expressie blokkeren, kunt u nog steeds toevoegen **importeren** instructies toe aan uw code. Ze allemaal moeten worden gegroepeerd op de belangrijkste regels van uw code.

Corrigeren 

```python
import math 
import numpy 
math.log(row["Score"])
```
 

Fout  

```python
import math  
math.log(row["Score"])  
import numpy
```
 
 
Als u de Module Bloktype gebruikt, kunt u de normale Python regels voor het gebruik van volgen de **importeren** instructie. 

## <a name="default-imports"></a>Standaard invoer
De volgende invoer zijn altijd inbegrepen en kan worden gebruikt in uw code. U hoeft niet opnieuw importeren. 

```python
import math  
import numbers  
import datetime  
import re  
import pandas as pd  
import numpy as np  
import scipy as sp
```
  

## <a name="install-new-packages"></a>Nieuwe pakketten installeren
Voor het gebruik van een pakket dat wordt niet standaard geïnstalleerd, moet u eerst om deze te installeren in de omgevingen die gebruikmaakt van Data Preparations. Deze installatie moet worden uitgevoerd op uw lokale computer, en ook op een compute-doelen die u uitvoeren wilt op.

U hebt uw om pakketten te installeren in een compute-doel, om de conda_dependencies.yml-bestand zich in de map aml_config onder de hoofdmap van uw project te wijzigen.

### <a name="windows"></a>Windows 
Als u de locatie op Windows zoekt, moet u de installatie van de app-specifieke van Python en de map scripts vinden. De standaardlocatie is:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts` 

Voer vervolgens een van de volgende opdrachten: 

`conda install <libraryname>` 

of 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
U vindt de locatie op een Mac, moet u de installatie van de app-specifieke van Python en de map scripts vinden. De standaardlocatie is: 

`/Users/<user>/Library/Caches/AmlWorkbench/Python/bin` 

Voer vervolgens een van de volgende opdrachten: 

`./conda install <libraryname>`

of 

`./pip install <libraryname>`

## <a name="use-custom-modules"></a>Gebruik aangepaste modules
Schrijf de volgende Python-code in transformeren gegevensstroom (Script)

```python
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction1
df = ExtensionFunction1(df)
```

In de kolom toevoegen (Script), stelt u Code Bloktype = Module en de volgende Python-code schrijven

```python 
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction2

def newvalue(row):
    return ExtensionFunction2(row)
```
Voor de uitvoering van verschillende wijst contexten (lokaal, Docker, Spark), u het absolute pad op de juiste plaats. U wilt gebruiken 'os.getcwd() + relativePath' bij het zoeken.


## <a name="column-data"></a>Kolomgegevens 
Kolomgegevens kunt toegankelijk vanaf een rij met behulp van een puntnotering of sleutel / waarde-notatie. Kolomnamen met spaties of speciale tekens kunnen niet worden geopend met behulp van een puntnotering. De `row` variabele moet altijd worden gedefinieerd in beide modi van Python-extensies (Module en expressie). 

Voorbeelden 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="add-column"></a>Kolom toevoegen 
### <a name="purpose"></a>Doel
De kolom toevoegen uitbreidingspunt kunt u Python voor het berekenen van een nieuwe kolom te schrijven. De code die u schrijft heeft toegang tot de volledige rij. Het moet een nieuwe waarde voor elke rij in de kolom retourneren. 

### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze uitbreidingspunt toevoegen met behulp van het blok kolom toevoegen (Script). Deze is beschikbaar op het hoogste niveau **transformaties** menu, ook als aan de **kolom** in het contextmenu. 

### <a name="syntax"></a>Syntaxis
Expressie

```python
    math.log(row["Score"])
```

Module 

```python
def newvalue(row):  
     return math.log(row["Score"])
```
 

## <a name="advanced-filter"></a>Geavanceerd filter
### <a name="purpose"></a>Doel 
Het Geavanceerde Filter-extensiepunt kunt u een aangepast filter schrijven. U hebt toegang tot de hele rij en waar in uw code moet worden geretourneerd (inclusief de rij), of ONWAAR (uitsluiten de rij). 

### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze uitbreidingspunt toevoegen met behulp van het blok Geavanceerd Filter (Script). Deze is beschikbaar op het hoogste niveau **transformaties** menu. 

### <a name="syntax"></a>Syntaxis

Expressie

```python
    row["Score"] > 95
```

Module  

```python
def includerow(row):  
    return row["Score"] > 95
```
 

## <a name="transform-dataflow"></a>Gegevensstroom transformeren
### <a name="purpose"></a>Doel 
De gegevensstroom transformeren uitbreidingspunt kunt u volledig Transformeren de gegevensstroom. U hebt toegang tot een Pandas dataframe waarin alle kolommen en rijen die u wilt verwerken. Uw code moet een Pandas dataframe met de nieuwe gegevens retourneren. 

>[!NOTE]
>In Python, worden alle gegevens worden geladen in het geheugen zich in een Pandas dataframe als deze extensie wordt gebruikt. 
>
>In Spark worden alle gegevens worden verzameld op een enkele worker-knooppunt. Als de gegevens erg groot is, kan een werknemer onvoldoende geheugen uitvoeren. Gebruik deze zorgvuldig.

### <a name="how-to-use"></a>Gebruiksinstructies 
U kunt deze uitbreidingspunt toevoegen met behulp van het blok gegevensstroom transformeren (Script). Deze is beschikbaar op het hoogste niveau **transformaties** menu. 
### <a name="syntax"></a>Syntaxis 

Expressie

```python
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Module 

```python
def transform(df):  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()  
    return df
```
  

## <a name="transform-partition"></a>Partitie transformeren  
### <a name="purpose"></a>Doel 
De partitie transformeren uitbreidingspunt kunt u een partitie van de gegevensstroom transformeren. U hebt toegang tot een Pandas dataframe dat alle kolommen en rijen voor deze partitie bevat. Uw code moet een Pandas dataframe met de nieuwe gegevens retourneren. 

>[!NOTE]
>In Python, kunt u uiteindelijk met één partitie of meerdere partities, afhankelijk van de grootte van uw gegevens. In Spark, kunt u werkt met een dataframe waarin de gegevens voor een partitie op een bepaalde worker-knooppunt. In beide gevallen kan niet u wordt ervan uitgegaan dat u toegang tot de volledige gegevensset hebt. 


### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze uitbreidingspunt toevoegen met behulp van het blok partitie transformeren (Script). Deze is beschikbaar op het hoogste niveau **transformaties** menu. 

### <a name="syntax"></a>Syntaxis 

Expressie 

```python
    df['partition-id'] = index  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Module 

```python
def transform(df, index):
    df['partition-id'] = index
    df['index-column'] = range(1, len(df) + 1)
    df = df.reset_index()
    return df
```


## <a name="datapreperror"></a>DataPrepError  
### <a name="error-values"></a>Foutwaarden  
In Data Preparations bestaat het concept van foutwaarden. 

Het is mogelijk tegenkomen foutwaarden in aangepaste Python-code. Ze zijn exemplaren van een Python-klasse genaamd `DataPrepError`. Deze klasse verpakt een Python-uitzondering en een aantal eigenschappen heeft. De eigenschappen bevatten informatie over de fout is opgetreden tijdens het verwerken van de oorspronkelijke waarde, evenals de oorspronkelijke waarde. 


### <a name="datapreperror-class-definition"></a>Definitie van de klasse DataPrepError
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Het maken van een DataPrepError in de Data Preparations Python-webstructuur ziet er in het algemeen als volgt uit: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Gebruiksinstructies 
Het is mogelijk wanneer op een uitbreidingspunt voor het genereren van DataPrepErrors als retourwaarden met behulp van de vorige methode voor het maken van Python wordt uitgevoerd. Het is veel meer waarschijnlijk dat DataPrepErrors zijn opgetreden tijdens het verwerken van gegevens op het moment van een extensie. Op dit moment moet de aangepaste Python-code voor het afhandelen van een DataPrepError als een geldig gegevenstype.

#### <a name="syntax"></a>Syntaxis 
Expressie 
```python 
    if (isinstance(row["Score"], DataPrepError)): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
```python 
    if (hasattr(row["Score"], "originalValue")): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
Module 
```python 
def newvalue(row): 
    if (isinstance(row["Score"], DataPrepError)): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
``` 
```python 
def newvalue(row): 
    if (hasattr(row["Score"], "originalValue")): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
```  
