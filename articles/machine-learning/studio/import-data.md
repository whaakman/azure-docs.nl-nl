---
title: Gegevens importeren in Machine Learning Studio - Azure | Microsoft Docs
description: Hoe u uw gegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen. Meer informatie over welke gegevenstypen en opmaak van gegevens worden ondersteund.
keywords: gegevens, de indeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 2f8c1eb43fddb21a59d4f00fd86b08d3fb3608f4
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269954"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Uw trainingsgegevens vanuit verschillende gegevensbronnen importeren in Azure Machine Learning Studio

Als u uw eigen gegevens in Machine Learning Studio wilt ontwikkelen en trainen van een predictive analytics-oplossing, kunt u gegevens uit: 

* Een [ **lokaal bestand** ](import-data-from-local-file.md) -lokale gegevens vooraf laden van de harde schijf te maken van een gegevensset-module in uw werkruimte
* [**Online gegevensbronnen** ](import-data-from-online-sources.md) -gebruik de [importgegevens] [ import-data] module voor toegang tot gegevens op basis van een van verschillende online gegevensbronnen terwijl uw experiment wordt uitgevoerd
* [**Machine Learning Studio-experiment** ](import-data-from-an-experiment.md) -gegevens gebruiken die is opgeslagen als een gegevensset in Machine Learning Studio
* [**On-premises SQL Server-database** ](use-data-from-an-on-premises-sql-server.md) -gegevens uit een on-premises SQL Server-database gebruiken zonder dat het handmatig kopiëren van gegevens

> [!NOTE]
> Er zijn een aantal voorbeeldgegevenssets beschikbaar in Machine Learning Studio die u voor het trainen van gegevens gebruiken kunt. Zie voor meer informatie over deze [de voorbeeldgegevenssets gebruiken in Azure Machine Learning Studio](use-sample-datasets.md)).
> 
> 

In dit inleidende artikel wordt beschreven hoe u gegevens klaar voor gebruik in Machine Learning Studio ook en wordt beschreven welke gegevensindelingen en gegevenstypen worden ondersteund.

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Bereid gegevens voor gebruik in Azure Machine Learning Studio
Machine Learning Studio is ontworpen voor gebruik met rechthoekige of tabellaire gegevens, zoals tekstgegevens die is gescheiden of gestructureerde gegevens uit een database, maar in sommige gevallen niet-rechthoekige gegevens kunnen worden gebruikt.

Het is raadzaam als uw gegevens relatief opgeschoond zijn. Dat wil zeggen, moet u zorgen voor problemen zoals zonder aanhalingstekens tekenreeksen voordat u de gegevens naar uw experiment uploaden.

Er zijn echter modules beschikbaar in Machine Learning Studio waarmee manipuleren van gegevens in uw experiment. Afhankelijk van de machine learning-algoritmen die u wilt gebruiken, moet u mogelijk om te bepalen hoe u gegevens structurele problemen zoals ontbrekende waarden en sparse gegevens kunt verwerken en -modules die u bij die helpen kunnen er zijn. Zoeken in de **gegevenstransformatie** gedeelte van het modulepalet voor modules die deze functies uitvoeren.

U kunt op elk gewenst moment in uw experiment weergeven of downloaden van de gegevens die wordt geproduceerd door een module door te klikken op de uitvoerpoort. Afhankelijk van de module, kunnen er verschillende Downloadinstellingen beschikbaar of kunt u mogelijk de gegevens in uw webbrowser in Machine Learning Studio visualiseren.

## <a name="data-formats-and-data-types-supported"></a>Gegevens-indelingen en gegevenstypen ondersteund
U kunt verschillende soorten gegevens importeren in uw experiment, afhankelijk van welke mechanisme die u voor het importeren van gegevens en waar deze vandaan gebruiken:

* Tekst zonder opmaak (*.txt)
* Door komma's gescheiden waarden (CSV met een koptekst (.csv) of zonder) (. nh.csv)
* Door tabs gescheiden waarden (TSV met een koptekst (.tsv) of zonder) (. nh.tsv)
* Excel-bestand
* Azure-tabel
* Hive-tabel
* SQL-databasetabel
* OData-waarden
* SVMLight gegevens (.svmlight) (Zie de [SVMLight definitie](http://svmlight.joachims.org/) voor informatie over de)
* Kenmerk Relation File Format (ARFF)-gegevens (.arff) (Zie de [ARFF definitie](http://weka.wikispaces.com/ARFF) voor informatie over de)
* ZIP-bestand (.zip)
* R-object of de werkruimte-bestand (. RData)

Als u gegevens in een indeling zoals ARFF met metagegevens importeert, Machine Learning Studio maakt gebruik van deze metagegevens voor het definiëren van de kop en het gegevenstype van elke kolom.

Als u gegevens zoals TSV- of CSV-indeling die deze metagegevens bevat geen importeert, Machine Learning Studio het gegevenstype voor elke kolom door steekproeven van de gegevens. Als de gegevens ook geen kolomkoppen, biedt Machine Learning Studio standaardnamen.

U kunt expliciet opgeven of wijzigen van de koppen en gegevenstypen voor kolommen met behulp van de [metagegevens bewerken][edit-metadata].

De volgende **gegevenstypen** worden herkend door Machine Learning Studio:

* Reeks
* Geheel getal
* Double-waarde
* Booleaans
* DateTime
* TimeSpan

Machine Learning Studio maakt gebruik van een interne gegevenstype met de naam ***gegevenstabel*** om door te geven van gegevens tussen modules. U kunt uw gegevens expliciet converteren naar gegevenstabel opmaken met de [converteren naar gegevensset] [ convert-to-dataset] module.

Elke module die dan tabel hebben worden de gegevens converteren naar gegevenstabel op de achtergrond voordat deze wordt doorgegeven aan de volgende module.

Indien nodig, kunt u gegevens tabelindeling weer in CSV, TSV, ARFF of SVMLight indeling met behulp van andere modules conversie kunt converteren.
Zoeken in de **indeling Gegevensconversies** gedeelte van het modulepalet voor modules die deze functies uitvoeren.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
