---
title: Gegevens importeren in Machine Learning Studio | Microsoft Docs
description: Hoe uw gegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen. Meer informatie over welke gegevenstypen en gegevensopmaak worden ondersteund.
keywords: gegevens, gegevensindeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: garye;bradsev
ms.openlocfilehash: eb22f516f298df9396ca809acaa9c8cb62589c2a
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Uw trainingsgegevens vanuit verschillende gegevensbronnen importeren in Azure Machine Learning Studio
Als u uw eigen gegevens in Machine Learning Studio te ontwikkelen en training van een predictive analytics-oplossing, kunt u het volgende doen: 

* uploaden van gegevens uit een **lokaal bestand** tevoren van de harde schijf voor het maken van een gegevensset-module in uw werkruimte
* toegang tot gegevens uit een van de **online gegevensbronnen** terwijl uw experiment wordt uitgevoerd met behulp van de [importgegevens] [ import-data] module 
* gegevens uit een andere Azure Machine learning **experimenteren** opgeslagen als een gegevensset
* gegevens uit een on-premises **SQL Server-database**

Elk van deze opties wordt beschreven in een van de onderwerpen in het menu hieronder. Deze onderwerpen beschreven hoe u gegevens importeert uit verschillende gegevensbronnen in Machine Learning Studio gebruiken. 

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Er zijn een aantal voorbeeldgegevenssets beschikbaar in Machine Learning Studio die u voor trainingsgegevens gebruiken kunt. Zie voor meer informatie over deze [de voorbeeldgegevenssets in Azure Machine Learning Studio gebruiken](use-sample-datasets.md)).
> 
> 

Dit inleidende onderwerp wordt uitgelegd hoe het gebruiksklaar maken van gegevens in Machine Learning Studio ook en wordt beschreven welke gegevensindelingen en gegevenstypen worden ondersteund. 

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Gegevens in Azure Machine Learning Studio klaar voor gebruik ophalen
Machine Learning Studio is ontworpen voor gebruik met rechthoekige of tabellaire gegevens, zoals tekstgegevens die gescheiden of gestructureerde gegevens uit een database, maar in sommige gevallen niet-rechthoekige gegevens mogen worden gebruikt.

Het is raadzaam als uw gegevens relatief schoon is. Dat wil zeggen, moet u zorgt voor problemen zoals zonder aanhalingstekens tekenreeksen voordat u de gegevens naar uw experiment uploaden.

Er zijn echter modules in Machine Learning Studio waarmee manipuleren van gegevens binnen uw experiment beschikbaar. Afhankelijk van de machine learning-algoritmen die u wilt gebruiken, moet u mogelijk bepalen hoe u gegevens structurele problemen zoals ontbrekende waarden en sparse gegevens hebt verwerkt en er zijn de modules die u bij die helpen kunnen. Zoeken in de **Data Transformation** sectie van het modulepalet voor modules die deze functies worden uitgevoerd.

U kunt op elk gewenst moment in uw experiment weergeven of downloaden van de gegevens die wordt geproduceerd door een module door te klikken op de uitvoerpoort. Mogelijk zijn er verschillende Downloadinstellingen beschikbaar, afhankelijk van de module, of u mogelijk de gegevens in uw webbrowser in Machine Learning Studio visualiseren.

## <a name="data-formats-and-data-types-supported"></a>Opmaak en gegevenstypen worden ondersteund
U kunt een aantal gegevenstypen in uw experiment importeren, afhankelijk van welke mechanisme u gegevens importeren en waar deze vandaan:

* Tekstbestand (.txt)
* Door komma's gescheiden waarden (CSV met een koptekst (.csv) of zonder) (. nh.csv)
* Door tabs gescheiden waarden (TSV met een koptekst (.tsv) of zonder) (. nh.tsv)
* Excel-bestand
* Azure-tabel
* Hive-tabel
* SQL-databasetabel
* OData-waarden
* SVMLight gegevens (.svmlight) (Zie de [SVMLight definitie](http://svmlight.joachims.org/) voor indeling informatie)
* Kenmerk Relation File Format (ARFF) gegevens (.arff) (Zie de [ARFF definitie](http://weka.wikispaces.com/ARFF) voor indeling informatie)
* ZIP-bestand (.zip)
* R-object of de werkruimte-bestand (. RData)

Als u gegevens in een indeling zoals ARFF met metagegevens importeert, wordt in Machine Learning Studio deze metagegevens gebruikt voor het definiëren van de kop en het gegevenstype van elke kolom.

Als u gegevens zoals TSV- of CSV-indeling waarin deze metagegevens niet importeert, Machine Learning Studio het gegevenstype voor elke kolom van de gegevens steekproeven. Als de gegevens ook geen kolomkoppen, biedt Machine Learning Studio standaardnamen.

U kunt expliciet opgeven of wijzig de koppen en gegevenstypen voor kolommen met behulp van de [metagegevens bewerken][edit-metadata].

De volgende **gegevenstypen** worden herkend door Machine Learning Studio:

* Reeks
* Geheel getal
* dubbele
* Boole-waarde
* Datum en tijd
* TimeSpan

Machine Learning Studio maakt gebruik van een interne gegevenstype aangeroepen ***gegevenstabel*** gegevens moeten worden doorgegeven tussen modules. U kunt uw gegevens expliciet converteren naar gegevenstabel indeling met behulp van de [converteren naar een gegevensset] [ convert-to-dataset] module.

Elke module die indelingen dan tabel accepteert worden de gegevens converteren naar gegevenstabel achtergrond voordat deze wordt doorgegeven aan de volgende module.

U kunt indien nodig, gegevenstabel naar CSV, TSV, ARFF, of SVMLight notatie met andere modules conversie converteren.
Zoeken in de **gegevens indeling conversies** sectie van het modulepalet voor modules die deze functies worden uitgevoerd.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
