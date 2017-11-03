---
title: 'Stap 2: Gegevens uploaden naar een Machine Learning-experiment | Microsoft Docs'
description: 'Stap 2 van het ontwikkelen van een voorspellende oplossing overzicht: het uploaden van openbare gegevens opgeslagen in Azure Machine Learning Studio.'
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 469c94f6115f99bc4cf067e9c8f0e55c64990358
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Kennismaken, stap 2: Bestaande gegevens uploaden naar een Azure Machine Learning-experiment
Dit is de tweede stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. **Bestaande gegevens uploaden**
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [Toegang tot de webservice](walkthrough-6-access-web-service.md)

- - -
Voor het ontwikkelen van een Voorspellend model voor kredietrisico, moeten we gegevens die u gebruiken kunt om te trainen en vervolgens het model te testen. Voor dit scenario gebruiken we 'UCI Statlog (Duitse tegoed gegevens) Data Set' uit de opslagplaats UC Irvine Machine Learning. U kunt deze hier vinden:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+credit+Data)</a>

We gebruiken het bestand met de naam **german.data**. Dit bestand downloaden naar uw lokale vaste schijf.  

De **german.data** gegevensset bevat rijen van 20 variabelen voor 1000 afgelopen aanvragers tegoed. Deze 20 variabelen vertegenwoordigen de gegevensset reeks functies (de *functie vector*), waarmee u identificatiekenmerken op voor elke aanvrager tegoed. Een extra kolom in elke rij vertegenwoordigt de aanvrager berekende kredietrisico, met 700 aanvrager aangeduid als een lage kredietrisico en 300 als een hoog risico.

De website UCI biedt een beschrijving van de kenmerken van de functie-vector voor deze gegevens. Dit omvat financiële gegevens, kredietgeschiedenis werkstatus en persoonlijke gegevens. Voor iedere aanvrager een binaire classificatie is opgegeven waarmee wordt aangegeven of ze zich een lage of hoge kredietrisico. 

We gebruiken deze gegevens om een predictive analytics-model te trainen. Als we bent klaar, is het model moet kunnen accepteren van een functie-vector voor een nieuwe afzonderlijke en voorspelt of hij of zij een lage of hoge kredietrisico is.  

Hier volgt een interessante twist. De beschrijving van de gegevensset op de website UCI noemt wat kost als we iemands kredietrisico misclassify.
Als het model een hoge kredietrisico voor iemand die daadwerkelijk een lage kredietrisico is voorspelt, heeft een misclassification doorgevoerd in het model.
Maar de omgekeerde misclassification vijf keer duurder aan de financiële instelling is: als het model wordt voorspeld een lage kredietrisico voor iemand die daadwerkelijk een hoge kredietrisico is.

Ja, willen we onze model te trainen, zodat de kosten van dit laatste type misclassification vijf keer hoger is dan de andere manier onjuiste.
Een eenvoudige manier om dit te doen bij het trainen van het model in onze experiment is door het dupliceren van (vijfmaal) die items die iemand met een hoge kredietrisico vertegenwoordigen. Klik, als het model ten onrechte iemand als een lage kredietrisico classificeert wanneer ze daadwerkelijk een hoog risico, het model geen die dezelfde misclassification vijf keer één keer voor elke kopie. Dit verhoogt de kosten van deze fout in de resultaten training.


## <a name="convert-the-dataset-format"></a>De dataset-indeling converteren
De oorspronkelijke gegevensset gebruikt een leeg gescheiden-indeling. Machine Learning Studio werkt beter met een bestand met door komma's gescheiden waarden (CSV), zodat we de gegevensset converteren moet door spaties vervangen door komma's.  

Er zijn veel manieren om deze gegevens te converteren. Een manier is door de volgende Windows PowerShell-opdracht:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Een andere manier is door de Unix-ype-opdracht:  

    sed 's/ /,/g' german.data > german.csv  

In beide gevallen is er een door komma's gescheiden versie van de gegevens in een bestand met de naam gemaakt **german.csv** die we in ons experiment kunt gebruiken.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Uploaden van de gegevensset naar Machine Learning Studio
Nadat de gegevens is geconverteerd naar een CSV-indeling, moeten we naar Machine Learning Studio te uploaden. 

1. Open de startpagina van de Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Klik op het menu ![Menu][1] in de linkerbovenhoek van het venster, klikt u op **Azure Machine Learning**, selecteer **Studio**, en meld u aan.

3. Klik op **+ nieuw** aan de onderkant van het venster.

4. Selecteer **GEGEVENSSET**.

5. Selecteer **vanuit het lokale bestand**.

    ![Voeg een gegevensset toe vanuit het lokale bestand][2]

6. In de **uploaden van een nieuwe gegevensset** dialoogvenster, klikt u op **Bladeren** en zoek de **german.csv** dat u hebt gemaakt.

7. Voer een naam voor de gegevensset. Voor dit scenario moet aanroepen 'UCI Duits creditcardgegevens'.

8. Selecteer voor gegevenstype **generieke CSV-bestand met geen koptekst (. nh.csv)**.

9. Een beschrijving toevoegen als u wilt.

10. Klik op de **OK** selectievakje is ingeschakeld.  

    ![Uploaden van de gegevensset][3]

Dit uploadt u de gegevens in een gegevensset-module die u in een experiment gebruiken kunt.

U kunt beheren gegevenssets die u naar Studio hebt geüpload door te klikken op de **GEGEVENSSETS** tabblad aan de linkerkant van het venster Studio.

![Gegevenssets beheren][4]

Zie voor meer informatie over het importeren van andere typen gegevens in een experiment [uw trainingsgegevens importeren in Azure Machine Learning Studio](import-data.md).

**Volgende stap: [een nieuw experiment maken](walkthrough-3-create-new-experiment.md)**

[1]: media/walkthrough-2-upload-data/menu.png
[2]: media/walkthrough-2-upload-data/add-dataset.png
[3]: media/walkthrough-2-upload-data/upload-dataset.png
[4]: media/walkthrough-2-upload-data/dataset-list.png
