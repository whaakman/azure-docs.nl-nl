---
title: 'R-Script uitvoeren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module R-Script uitvoeren in Azure Machine Learning-service R-code uit te voeren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bfddcd3db4825ea1875474aa16544aa15412bdea
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518051"
---
# <a name="execute-r-script"></a>R-Script uitvoeren

In dit artikel wordt beschreven hoe u de **R-Script uitvoeren** module voor het uitvoeren van R-code in uw experiment visuele interface.

Met R, kunt u taken die worden momenteel niet ondersteund door de bestaande modules, zoals uitvoeren: 
- Aangepaste gegevenstransformaties maken
- Gebruik uw eigen metrische gegevens om te evalueren van voorspellingen
- Bouw modellen met behulp van de algoritmen die niet zijn geïmplementeerd als zelfstandige modules in de visuele interface

## <a name="r-version-support"></a>Ondersteuning voor R-versie

De visuele interface voor Azure Machine Learning-service maakt gebruik van de distributie CRAN (Comprehensive R Archive Network) van R. De huidige gebruikte versie is CRAN 3.5.1.

## <a name="supported-r-packages"></a>Ondersteunde R-pakketten

De R-omgeving is vooraf worden geïnstalleerd met meer dan 100 pakketten. Zie de sectie voor een volledige lijst [vooraf geïnstalleerde R-pakketten](#pre-installed-r-packages).

U kunt ook de volgende code toevoegen aan een **R-Script uitvoeren** module en om te zien van de geïnstalleerde pakketten.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  dataframe1 <- data.frame(installed.packages())
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="installing-r-packages"></a>R-pakketten installeren
Voor het installeren van extra R-pakketten, gebruikt u de `install.packages()` methode. Zorg ervoor dat de CRAN-opslagplaats opgeven. Pakketten zijn geïnstalleerd voor elke **R-Script uitvoeren** -module, en worden niet gedeeld met andere **R-Script uitvoeren** modules.

In dit voorbeeld laat zien hoe Zoo installeren:
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  
  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")
  library(zoo)
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="how-to-configure-execute-r-script"></a>Het configureren van R-Script uitvoeren

De **R-Script uitvoeren** -module bevat voorbeeldcode die u als uitgangspunt gebruiken kunt. Het configureren van de **R-Script uitvoeren** -module, een set met invoer en de code wordt uitgevoerd.

![R-module](media/module/execute-r-script.png)

Gegevenssets die zijn opgeslagen in de visuele interface worden automatisch geconverteerd naar een gegevensframe R wanneer met deze module wordt geladen.

1.  Voeg de **R-Script uitvoeren** module naar het experimentcanvas.

    > [!NOTE]
    > Alle gegevens doorgegeven aan de **R-Script uitvoeren** module wordt geconverteerd naar de R `data.frame` indeling.

1. Verbinding maken met een invoer nodig is voor het script. Invoer zijn optioneel en kunnen gegevens en extra R-code bevatten.

    * **Dataset1**: Verwijzen naar de eerste invoer als `dataframe1`. De invoergegevensset moet zijn opgemaakt als een CSV, TSV, ARFF of u verbinding kunt maken met een Azure Machine Learning-gegevensset.

    * **Dataset2**: Verwijzen naar de tweede invoer als `dataframe2`. Deze gegevensset ook moet zijn opgemaakt als een CSV, TSV, ARFF-bestand, of als een Azure Machine Learning-gegevensset.

    * **Script bundel**: De derde invoer accepteert ZIP-bestanden. Het ZIP-bestand kan bevatten meerdere bestanden en meerdere bestandstypen.

1. In de **R-script** in het tekstvak typt of plakt u geldige R-script.

    Om u aan de slag te helpen de **R-Script** tekstvak is vooraf gevuld met voorbeeldcode, die u kunt bewerken of vervangen.

    * Het script moet een functie met de naam bevatten `azureml_main`, dit is het toegangspunt dat voor deze module.

    * De functie voor het item mag maximaal twee invoerargumenten: `Param<dataframe1>` en `Param<dataframe2>`
 
    > [!NOTE]
    >  Bestaande R-code moet mogelijk kleine wijzigingen om uit te voeren in een experiment visuele interface. Bijvoorbeeld, moet invoergegevens die u in CSV-indeling opgeeft worden expliciet geconverteerd naar een gegevensset voordat u deze in uw code gebruiken kunt. Gegevens en in de kolom typen in de taal R gebruikt verschillen ook op een aantal punten uit de gegevens en in de kolom typen gebruikt in de visuele interface.

1.  **Random Seed**: Typ een waarde om te gebruiken in de R-omgeving als de random seed-waarde. Deze parameter is gelijk aan aanroepen `set.seed(value)` in R-code.  

1. Voer het experiment uit.  

## <a name="results"></a>Resultaten

De **R-Script uitvoeren** modules kunnen meerdere uitvoer retourneren, maar deze moeten worden opgegeven als R gegevensframes. Gegevensframes worden automatisch geconverteerd naar de visuele interface gegevenssets voor compatibiliteit met andere modules.

Standard berichten en fouten van R keert terug naar het logboek van de module.

## <a name="sample-scripts"></a>Voorbeeldscripts

Er zijn veel manieren waarop u uw experiment uitbreiden kunt met behulp van aangepaste R-script.  Deze sectie bevat voorbeelden van code voor algemene taken.


### <a name="add-r-script-as-an-input"></a>R-script als invoer toevoegen

De **R-Script uitvoeren** module biedt ondersteuning voor willekeurige R-script-bestanden als invoer. Om dit te doen, moeten deze worden geüpload naar uw werkruimte als onderdeel van het ZIP-bestand.

1. Als u wilt uploaden een ZIP-bestand met R-code naar uw werkruimte, klikt u op **nieuw**, klikt u op **gegevensset**, en selecteer vervolgens **vanuit het lokale bestand** en de **Zip-bestand**optie.  

1. Controleer of het ZIP-bestand is beschikbaar in de **opgeslagen gegevenssets** lijst.

1.  Verbinding maken met de gegevensset in de **Script bundel** poort invoer.

1. Alle bestanden die zijn opgenomen in het ZIP-bestand zijn tijdens runtime experiment beschikbaar. 

    Als het script bundle-bestand een mapstructuur bevat, wordt de structuur behouden. Echter, moet u uw code voor het toevoegen van de map wijzigen **. / Script bundelen** naar het pad.

### <a name="process-data"></a>Gegevens verwerken

Het volgende voorbeeld laat zien hoe om te schalen en invoergegevens normaliseren:

```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.
# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.
  series <- dataframe1$width
  # find the maximum and minimum values of width column in dataframe1
  max_v <- max(series)
  min_v <- min(series)
  # calculate the scale and bias
  scale <- max_v - min_v
  bias <- min_v / dis
  # apply min-max normalizing
  dataframe1$width <- dataframe1$width / scale - bias
  dataframe2$width <- dataframe2$width / scale - bias
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
 ```

### <a name="read-a-zip-file-as-input"></a>Een ZIP-bestand te lezen als invoer

In dit voorbeeld laat zien hoe u een gegevensset in een ZIP-bestand gebruiken als invoer voor de **R-Script uitvoeren** module.

1. Maken van het bestand met CSV-indeling en geef deze de naam 'mydatafile.csv'.
1. Maak een ZIP-bestand en het CSV-bestand toevoegen aan het archief.
1. Upload het ZIP-bestand naar uw Azure Machine Learning-werkruimte. 
1. Verbinding maken met de resulterende gegevensset in de **ScriptBundle** invoer van uw **R-Script uitvoeren** module.
1. Met behulp van de volgende code aan de CSV-gegevens lezen van het ZIP-bestand.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  mydataset<-read.csv("./Script Bundle/mydatafile.csv",encoding="UTF-8");  
  # Return datasets as a Named List
  return(list(dataset1=mydataset, dataset2=dataframe2))
}
```

### <a name="replicate-rows"></a>Rijen repliceren

In dit voorbeeld laat zien hoe positieve records in een gegevensset om het voorbeeld in balans repliceren:

```R
azureml_main <- function(dataframe1, dataframe2){
  data.set <- dataframe1[dataframe1[,1]==-1,]  
  # positions of the positive samples
  pos <- dataframe1[dataframe1[,1]==1,]
  # replicate the positive samples to balance the sample  
  for (i in 1:20) data.set <- rbind(data.set,pos)  
  row.names(data.set) <- NULL
  # Return datasets as a Named List
  return(list(dataset1=data.set, dataset2=dataframe2))
}
```

### <a name="pass-r-objects-between-execute-r-script-modules"></a>Pass R objecten tussen modules van de R-Script uitvoeren

U kunt R objecten doorgeven tussen exemplaren van de **R-Script uitvoeren** module met behulp van het mechanisme voor interne serialisatie. In dit voorbeeld wordt ervan uitgegaan dat u wilt verplaatsen van het R-object met de naam `A` tussen twee **R-Script uitvoeren** modules.

1. Toevoegen van de eerste **R-Script uitvoeren** module naar uw experiment en typ de volgende code in de **R-Script** tekstvak in op het maken van een geserialiseerde object `A` als tabel met de uitvoer van een kolom in de module:  
  
    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      # some codes generated A
      
      serialized <- as.integer(serialize(A,NULL))  
      data.set <- data.frame(serialized,stringsAsFactors=FALSE)

      return(list(dataset1=data.set, dataset2=dataframe2))
    }
    ```

    De expliciete conversie naar het type integer gebeurt omdat de serialisatie-functie gegevens uit in de R voert- `Raw` -indeling, dit wordt niet ondersteund door de visuele interface.

1. Voeg een tweede exemplaar van de **R-Script uitvoeren** -module, en deze verbinden met de uitvoerpoort van de vorige module.

1. Typ de volgende code in de **R-Script** in het tekstvak om op te halen object `A` uit de invoertabel van gegevens. 

    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      A <- unserialize(as.raw(dataframe1$serialized))  
      # Return datasets as a Named List
      return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

## <a name="pre-installed-r-packages"></a>Vooraf geïnstalleerde R-pakketten

De huidige lijst met vooraf geïnstalleerde R-pakketten die worden gebruikt:

|              |            | 
|--------------|------------| 
| Pakket      | Version    | 
| askpass      | 1.1        | 
| assertthat   | 0.2.1      | 
| backports    | 1.1.4      | 
| Base         | 3.5.1      | 
| base64enc    | 0.1-3      | 
| BH           | 1.69.0-1   | 
| bindr        | 0.1.1      | 
| bindrcpp     | 0.2.2      | 
| bitops       | 1.0-6      | 
| Opstarten         | 1.3-22     | 
| bezem        | 0.5.2      | 
| callr        | 3.2.0      | 
| caret-teken        | 6.0-84     | 
| caTools      | 1.17.1.2   | 
| cellranger   | 1.1.0      | 
| Klasse        | 7.3-15     | 
| cli          | 1.1.0      | 
| clipr        | 0.6.0      | 
| Cluster      | 2.0.7-1    | 
| codetools    | 0.2-16     | 
| ColorSpace   | 1.4-1      | 
| compiler     | 3.5.1      | 
| crayon       | 1.3.4      | 
| curl         | 3.3        | 
| Data.Table   | 1.12.2     | 
| datasets     | 3.5.1      | 
| DBI          | 1.0.0      | 
| dbplyr       | 1.4.1      | 
| digest       | 0.6.19     | 
| dplyr        | 0.7.6      | 
| e1071        | 1.7-2      | 
| evalueren     | 0.14       | 
| fansi        | 0.4.0      | 
| forcats      | 0.3.0      | 
| Foreach      | 1.4.4      | 
| refererende      | 0.8-71     | 
| fs           | 1.3.1      | 
| gdata        | 2.18.0     | 
| algemene typen     | 0.0.2      | 
| ggplot2      | 3.2.0      | 
| glmnet       | 2.0-18     | 
| Lijm         | 1.3.1      | 
| gower        | 0.2.1      | 
| gplots       | 3.0.1.1    | 
| grafische afbeeldingen     | 3.5.1      | 
| grDevices    | 3.5.1      | 
| raster         | 3.5.1      | 
| gtable       | 0.3.0      | 
| gtools       | 3.8.1      | 
| haven        | 2.1.0      | 
| highr        | 0.8        | 
| hms          | 0.4.2      | 
| htmltools    | 0.3.6      | 
| httr         | 1.4.0      | 
| ipred        | 0.9-9      | 
| iterators    | 1.0.10     | 
| jsonlite     | 1.6        | 
| KernSmooth   | 2.23-15    | 
| knitr        | 1.23       | 
| labeling     | 0.3        | 
| andere      | 0.20-38    | 
| lava         | 1.6.5      | 
| lazyeval     | 0.2.2      | 
| lubridate    | 1.7.4      | 
| magrittr     | 1.5        | 
| markdown     | 1          | 
| VOOR MASSAOPSLAG         | 7.3-51.4   | 
| Matrix       | 1.2-17     | 
| Methoden      | 3.5.1      | 
| mgcv         | 1.8-28     | 
| MIME         | 0.7        | 
| ModelMetrics | 1.2.2      | 
| modelr       | 0.1.4      | 
| munsell      | 0.5.0      | 
| nlme         | 3.1-140    | 
| nnet         | 7.3-12     | 
| numDeriv     | 2016.8-1.1 | 
| openssl      | 1.4        | 
| parallel     | 3.5.1      | 
| De pilaar       | 1.4.1      | 
| pkgconfig    | 2.0.2      | 
| plogr        | 0.2.0      | 
| plyr         | 1.8.4      | 
| prettyunits  | 1.0.2      | 
| processx     | 3.3.1      | 
| prodlim      | 2018.04.18 | 
| Wordt uitgevoerd     | 1.2.2      | 
| ps           | 1.3.0      | 
| purrr        | 0.3.2      | 
| quadprog     | 1.5-7      | 
| quantmod     | 0.4-15     | 
| R6           | 2.4.0      | 
| RandomForest | 4.6-14     | 
| RColorBrewer | 1.1-2      | 
| Rcpp         | 1.0.1      | 
| RcppRoll     | 0.3.0      | 
| readr        | 1.3.1      | 
| readxl       | 1.3.1      | 
| recepten      | 0.1.5      | 
| rematch      | 1.0.1      | 
| reprex       | 0.3.0      | 
| reshape2     | 1.4.3      | 
| reticulate   | 1.12       | 
| rlang        | 0.4.0      | 
| rmarkdown    | 1.13       | 
| ROCR         | 1.0-7      | 
| rpart        | 4.1-15     | 
| rstudioapi   | 0.1        | 
| rvest        | 0.3.4      | 
| kan worden geschaald       | 1.0.0      | 
| selectr      | 0.4-1      | 
| Ruimtelijke      | 7.3-11     | 
| splines      | 3.5.1      | 
| SQUAREM      | 2017.10-1  | 
| Statistieken        | 3.5.1      | 
| stats4       | 3.5.1      | 
| stringi      | 1.4.3      | 
| stringr      | 1.3.1      | 
| survival     | 2.44-1.1   | 
| sys          | 3.2        | 
| tcltk        | 3.5.1      | 
| tibble       | 2.1.3      | 
| tidyr        | 0.8.3      | 
| tidyselect   | 0.2.5      | 
| tidyverse    | 1.2.1      | 
| timeDate     | 3043.102   | 
| tinytex      | 0.13       | 
| hulpprogramma's        | 3.5.1      | 
| tseries      | 0.10-47    | 
| TTR          | 0.23-4     | 
| Utf8         | 1.1.4      | 
| utils        | 3.5.1      | 
| vctrs        | 0.1.0      | 
| viridisLite  | 0.3.0      | 
| whisker      | 0.3-2      | 
| withr        | 2.1.2      | 
| xfun         | 0.8        | 
| xml2         | 1.2.0      | 
| xts          | 0.11-2     | 
| yaml         | 2.2.0      | 
| zeallot      | 0.1.0      | 
| Zoo          | 1.8-6      | 

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 