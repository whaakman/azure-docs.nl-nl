---
title: Python-uitbreidbaarheid gebruiken met Azure Machine Learning gegevens voorbereidingen | Microsoft Docs
description: Dit document bevat een overzicht en gedetailleerde voorbeelden van het gebruik van Python-code uit te breiden de functionaliteit van het voorbereiden van gegevens
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
ms.date: 09/07/2017
ms.openlocfilehash: 3c3864480d2fcba4f6d388d4e0d00b917cb62d2b
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="data-preparations-python-extensions"></a>Gegevensextensies voorbereidingen Python
Als een manier invullen van functionaliteit onderbrekingen tussen de ingebouwde functies bevat Azure Machine Learning gegevens voorbereidingen uitbreidbaarheid op meerdere niveaus. In dit document, geven we een overzicht op de uitbreidingsmogelijkheden via door de Python-script. 

## <a name="custom-code-steps"></a>Aangepaste code stappen 
Voorbereidingen voor gegevens heeft de volgende aangepaste stappen waarin gebruikers code kunnen schrijven:

* Bestand lezer *
* Schrijver *
* Kolom toevoegen
* Geavanceerd filter
* Gegevensstroom transformeren
* Partitie transformeren

* Deze stappen zijn momenteel niet ondersteund in een Spark-uitvoering.

## <a name="code-block-types"></a>Code blok typen 
Wij ondersteunen twee typen van de code blok voor elk van deze stappen. Eerst wordt een bare Python-expressie die wordt uitgevoerd, omdat wordt ondersteund. Wij ondersteunen tweede, een Python-Module waar noemen we een specifieke functie met een bekende handtekening in de code die u opgeeft.

U kunt bijvoorbeeld een nieuwe kolom die het logboek van een andere kolom berekend op de volgende twee manieren toevoegen:

Expressie 

```python    
    math.log(row["Score"])
```

Module 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


De transformatie kolom toevoegen in de modus van de Module wordt verwacht dat een functie aangeroepen zoeken `newvalue` die een rij-variabele accepteert en retourneert de waarde voor de kolom. Deze module kan de hoeveelheid Python-code met andere functies, invoer, enzovoort bevatten.

De details van elk extensiepunt worden in de volgende secties besproken. 

## <a name="imports"></a>Invoer 
Als u het blok Expressietype gebruikt, kunt u nog steeds toevoegen **importeren** instructies toe aan uw code. Al deze moeten worden gegroepeerd op de bovenste regels van uw code.

Corrigeer 

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
 
 
Als u het blok moduletype gebruikt, voert u de normale Python regels voor het gebruik van de **importeren** instructie. 

## <a name="default-imports"></a>Standaard-invoer
De volgende invoer zijn altijd opgenomen en kan worden gebruikt in uw code. U hoeft niet opnieuw importeren. 

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
Voor het gebruik van een pakket dat niet standaard geïnstalleerd, moet u eerst installeren in de omgevingen die gegevens voorbereidingen gebruikt. Deze installatie moet worden uitgevoerd op uw lokale machine en compute doelen die u uitvoeren wilt op.

U hebt voor het installeren van uw pakketten in een compute-doel, het conda_dependencies.yml bestand zich in de map aml_config onder de hoofdmap van uw project te wijzigen.

### <a name="windows"></a>Windows 
U vindt de locatie in Windows, de installatie van de app-specifiek van Python en de map scripts te vinden. De standaardlocatie is:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts.` 

Voer vervolgens een van de volgende opdrachten: 

`conda install <libraryname>` 

of 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
U vindt de locatie op een Mac, de installatie van de app-specifiek van Python en de map scripts te vinden. De standaardlocatie is: 

`/Users/<user>/Library/Caches/AmlWorkbench>/Python/bin` 

Voer vervolgens een van de volgende opdrachten: 

`./conda install <libraryname>`

of 

`./pip install <libraryname>`

## <a name="use-custom-modules"></a>Gebruik aangepaste modules
Schrijf python-code als volgt in transformeren gegevensstroom (Script):

```python
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction1
df = ExtensionFunction1(df)
```

In de kolom toevoegen (Script), Code Bloktype instellen = Module en python code volgend schrijven:

```python 
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction2

def newvalue(row):
    return ExtensionFunction2(row)
```
Voor de uitvoering van de verschillende contexten (lokale computer, docker spark), wijst u het absolute pad op de juiste plaats. U wilt gebruiken 'os.getcwd() + relativePath' terug te vinden.


## <a name="column-data"></a>Kolomgegevens 
Kolomgegevens toegankelijk zijn vanuit een rij met puntnotatie of sleutel / waarde-notatie. Namen van kolommen die geen spaties of speciale tekens niet toegankelijk met puntnotatie. De `row` variabele moet altijd worden gedefinieerd in beide modi van Python-extensies (Module en expressie). 

Voorbeelden 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="file-reader"></a>Bestand lezer 
### <a name="purpose"></a>Doel 
Het bestand lezer extensiepunt kunt u volledig beheer het proces voor het lezen van een bestand in een gegevensstroom. Het systeem uw code aangeroepen en wordt doorgegeven in de lijst met bestanden die moet worden verwerkt. Uw code moet maken en een dataframe Pandas terug. 

>[!NOTE]
>Deze extensiepunt werkt in Spark niet. 


### <a name="how-to-use"></a>Gebruiksinstructies 
U toegang tot dit uitbreidingspunt van de **gegevensbron openen** wizard. Kies **bestand** op de eerste pagina, en kies vervolgens de bestandslocatie. Op de **bestandsparameters kiezen** pagina in de **bestandstype** vervolgkeuzelijst Kies **(Script) bestand met aangepaste**. 

Uw code krijgt een Pandas dataframe met de naam 'df' met informatie over de bestanden die u nodig hebt om te lezen. Als u kiest om een map met meerdere bestanden te openen, bevat de dataframe meer dan één rij.  

Deze dataframe heeft de volgende kolommen:

- Pad: Het bestand worden gelezen.
- PathHint: Vertelt u waar het bestand zich bevindt. Waarden: Lokale AzureBlobStorage en AzureDataLakeStorage.
- AuthenticationType: Het type verificatie gebruikt voor toegang tot het bestand. Waarden: None, SasToken en OAuthToken.
- AuthenticationValue: Bevat geen of het token moet worden gebruikt.

### <a name="syntax"></a>Syntaxis 
Expressie 

```python
    paths = df['Path'].tolist()  
    df = pd.read_csv(paths[0])
```


Module  
```python
PathHint = Local  
def read(df):  
    paths = df['Path'].tolist()  
    filedf = pd.read_csv(paths[0])  
    return filedf  
```
 

## <a name="writer"></a>schrijver 
### <a name="purpose"></a>Doel 
De Writer extensiepunt kunt u volledig beheer het proces van het schrijven van gegevens van een gegevensstroom. Het systeem uw code aangeroepen en wordt doorgegeven in een dataframe. Uw code kunt de dataframe schrijven van gegevens, maar u wilt gebruiken. 

>[!NOTE]
>De Writer extensiepunt werkt in Spark niet.


### <a name="how-to-use"></a>Gebruiksinstructies 
U kunt deze extensiepunt toevoegen met behulp van de schrijven (Script) gegevensstroomblok. Deze beschikbaar is op het hoogste niveau **transformaties** menu.

### <a name="syntax"></a>Syntaxis 
Expressie

```python
    df.to_csv('c:\\temp\\output.csv')
```

Module

```python
def write(df):  
    df.to_csv('c:\\temp\\output.csv')  
    return df
```
 
 
Dit blok aangepaste schrijven kan bestaan in het midden van een lijst met stappen. Als u een Module hebt gebruikt, moet uw schrijffunctie de dataframe die de invoer voor de stap die volgt retourneren. 

## <a name="add-column"></a>Kolom toevoegen 
### <a name="purpose"></a>Doel
Het extensiepunt kolom toevoegen kunt u Python voor het berekenen van een nieuwe kolom te schrijven. De code die u schrijft heeft toegang tot de volledige rij. Het moet een nieuwe waarde voor elke rij in de kolom retourneren. 

### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze extensiepunt toevoegen met behulp van het blok kolom toevoegen (Script). Deze beschikbaar is op het hoogste niveau **transformaties** menu, ook als aan de **kolom** contextmenu. 

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
Het extensiepunt Geavanceerde Filter kunt u een aangepast filter schrijven. U hebt toegang tot de hele rij en uw code moet True retourneren (inclusief de rij) of ONWAAR (uitsluiten de rij). 

### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze extensiepunt toevoegen met behulp van het blok Geavanceerde Filter (Script). Deze beschikbaar is op het hoogste niveau **transformaties** menu. 

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
De gegevensstroom transformeren extensiepunt kunt u de gegevensstroom volledig te transformeren. U hebt toegang tot een Pandas dataframe waarin alle kolommen en rijen dat u bent verwerken. Uw code moet een dataframe Pandas met de nieuwe gegevens retourneren. 

>[!NOTE]
>In Python is de gegevens in het geheugen worden geladen in een dataframe Pandas indien deze extensie wordt gebruikt. 
>
>In Spark, alle gegevens worden verzameld op een enkele werkrolknooppunt. Als de gegevens erg groot is, kan een werknemer uitgevoerd onvoldoende geheugen. Zorgvuldig gebruikmaken.

### <a name="how-to-use"></a>Gebruiksinstructies 
U kunt deze extensiepunt toevoegen met behulp van de transformatie (Script) gegevensstroomblok. Deze beschikbaar is op het hoogste niveau **transformaties** menu. 
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
De partitie transformeren extensiepunt kunt u een partitie van de gegevensstroom transformeren. U hebt toegang tot een Pandas dataframe waarin alle kolommen en rijen voor deze partitie. Uw code moet een dataframe Pandas met de nieuwe gegevens retourneren. 

>[!NOTE]
>In Python loopt u het risico met één partitie of meerdere partities, afhankelijk van de grootte van uw gegevens. In Spark, kunt u werkt met een dataframe waarin de gegevens voor een partitie op een gegeven werkrolknooppunt. In beide gevallen kan niet u wordt ervan uitgegaan dat u toegang tot de volledige gegevensset hebben. 


### <a name="how-to-use"></a>Gebruiksinstructies
U kunt deze extensiepunt toevoegen met behulp van het blok transformeren partitie (Script). Deze beschikbaar is op het hoogste niveau **transformaties** menu. 

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
In de voorbereidingen voor gegevens bestaat het concept van foutwaarden. 

Het is mogelijk tegenkomen foutwaarden in aangepaste Python-code. Ze zijn exemplaren van een Python-klasse aangeroepen `DataPrepError`. Deze klasse een Python-uitzondering verpakt en een aantal eigenschappen heeft. De eigenschappen bevatten informatie over de fout is opgetreden tijdens het verwerken van de oorspronkelijke waarde, evenals de oorspronkelijke waarde. 


### <a name="datapreperror-class-definition"></a>De klassendefinitie DataPrepError
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Het maken van een DataPrepError in het kader gegevens voorbereidingen Python ziet er in het algemeen als volgt uit: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Gebruiksinstructies 
Het is mogelijk wanneer Python wordt uitgevoerd op een extensiepunt DataPrepErrors als retourwaarden genereren met behulp van de methode voor maken van de vorige. Het is veel meer waarschijnlijk dat DataPrepErrors zijn opgetreden tijdens het verwerken van gegevens op het moment van een uitbreiding. Op dit moment moet de aangepaste Python-code voor het afhandelen van een DataPrepError als een geldig gegevenstype.

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
