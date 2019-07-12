---
title: 'Classificatie: Vluchtvertragingen voorspellen'
titleSuffix: Azure Machine Learning service
description: In dit artikel laat u over het bouwen van een machine learning-model om te voorspellen van vertragingen van vertragingen van vluchten met behulp van de visuele slepen-en-neerzetten-interface en aangepaste R-code.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 07/02/2019
ms.openlocfilehash: 773e55fe4b5ca5acf27ba1765e5a16075f625187
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607635"
---
# <a name="sample-6---classification-predict-flight-delays-using-r"></a>Voorbeeld 6 - indeling: Voorspellen van vertragingen van vertragingen van vluchten met R

Dit experiment wordt gebruikgemaakt van historische vlucht en van Weergegevens te voorspellen als een geplande passagiersvlucht met meer dan 15 minuten worden uitgesteld.

Dit probleem kan worden gerealiseerd als een probleem is classificatie, voorspellen van twee klassen--vertraagd, of op tijd. Op een classificatie, dit met behulp van een groot aantal voorbeelden van historische flight data model maken.

Dit is de laatste experiment-grafiek:

[![Grafiek van het experiment](media/ui-sample-classification-predict-flight-delay/experiment-graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 6:

    ![Open het experiment](media/ui-sample-classification-predict-flight-delay/open-sample6.png)

## <a name="get-the-data"></a>De gegevens ophalen

Dit experiment maakt gebruik van de **vertragingen vluchtgegevens** gegevensset. Het onderdeel van de TranStats gegevens verzamelen van de Verenigde Staten 's Ministerie van transport. De gegevensset bevat vertraging vluchtgegevens van April tot oktober 2013. Voordat u de gegevens uploadt naar de visuele interface, het vooraf is verwerkt als volgt:

* Gefilterd op de 70 drukste luchthavens opnemen in de Verenigde Staten.
* Voor geannuleerde vluchten, relabeled als meer dan 15 minuten vertraagd.
* Omgereden vluchten gefilterd.
* Geselecteerde 14 kolommen.

Ter aanvulling van de vluchtgegevens van vertragingen van, de **weer gegevensset** wordt gebruikt. De weergegevens per uur weer op basis van land opmerkingen van NOAA bevat, en metingen uit luchthaven weer stations, die betrekking hebben op dezelfde periode van April-oktober 2013 vertegenwoordigt. Voordat u uploadt naar Azure ML visuele interface, het vooraf is verwerkt als volgt:

* Weerstation-id's zijn toegewezen aan de bijbehorende luchthaven id's.
* Weather stations die niet zijn gekoppeld aan de 70 drukste luchthavens zijn verwijderd.
* De datumkolom is splitsen in afzonderlijke kolommen: Jaar, maand en dag.
* 26 kolommen geselecteerd.

## <a name="pre-process-the-data"></a>De gegevens vooraf te verwerken

Een gegevensset moet meestal enkele vooraf verwerken voordat deze kan worden geanalyseerd.

![gegevens verwerken](media/ui-sample-classification-predict-flight-delay/data-process.png)

### <a name="flight-data"></a>Vluchtgegevens van vertragingen van

De kolommen **Carrier**, **OriginAirportID**, en **DestAirportID** als gehele getallen zijn opgeslagen. Echter nog categorische kenmerken, gebruikt u de **metagegevens bewerken** module converteert naar categorische.

![edit-metadata](media/ui-sample-classification-predict-flight-delay/edit-metadata.png)

Gebruik vervolgens de **Select Columns** module gegevensset moeten worden uitgesloten van de kolommen die mogelijk doel leakers zijn: **DepDelay**, **DepDel15**, **ArrDelay**, **geannuleerd**, **jaar**. 

Als u wilt deelnemen aan de records vlucht met records weer per uur, de geplande vertrektijd als een van de join-sleutels te gebruiken. Hiertoe de join, de kolom CSRDepTime moet worden afgerond op het dichtstbijzijnde uur wordt uitgevoerd door in de **R-Script uitvoeren** module. 

### <a name="weather-data"></a>Weergegevens

Kolommen met een groot deel van de ontbrekende waarden zijn uitgesloten met behulp van de **Projectkolommen** module. Deze kolommen bevatten alle stringwaarde kolommen: **ValueForWindCharacter**, **WetBulbFarenheit**, **WetBulbCelsius**, **PressureTendency**, **PressureChange**, **SeaLevelPressure**, en **StationPressure**.

De **Clean Missing Data** module wordt vervolgens toegepast op de overige kolommen verwijderen van rijen met ontbrekende gegevens.

Weather waarneming tijden worden naar boven afgerond op het dichtstbijzijnde volledig uur. Geplande vluchttijden en de tijden van de opmerking weer worden in de tegengestelde richting om te controleren of dat het model gebruikt alleen weer voor de overdracht afgerond. 

Sinds de weergegevens wordt gerapporteerd in de lokale tijd, tijdzone verschillen worden verwerkt door de kolommen van de tijdzone van de geplande vertrektijd en de weersomstandigheden waarneming af te trekken. Deze bewerkingen worden uitgevoerd met behulp van de **R-Script uitvoeren** module.

### <a name="joining-datasets"></a>Lid worden van gegevenssets

Flight records worden gekoppeld aan de weergegevens bij de oorsprong van de vlucht (**OriginAirportID**) met behulp van de **deelnemen aan gegevens** module.

 ![lid worden van vertragingen van vluchten en weer per oorsprong](media/ui-sample-classification-predict-flight-delay/join-origin.png)


Flight records worden gekoppeld aan de weergegevens met behulp van de bestemming van de vlucht (**DestAirportID**).

 ![Lid worden van vertragingen van vluchten en weer per doel](media/ui-sample-classification-predict-flight-delay/join-destination.png)

### <a name="preparing-training-and-test-samples"></a>Training en voorbeelden Test voorbereiden

De **Split Data** module splitst u de gegevens in April door September-records voor training en oktober registreert voor testen.

 ![Splitsen trainen en testen van gegevens](media/ui-sample-classification-predict-flight-delay/split.png)

Jaar, maand en tijdzone-kolommen zijn verwijderd uit de training gegevensset met behulp van de module Select Columns.

## <a name="define-features"></a>Functies definiëren

In machine learning-zijn functies afzonderlijke meetbare eigenschappen van iets waarin die u geïnteresseerd bent in. Zoeken naar een krachtige set functies, moet u basiskennis van experimenten en het domein. Bepaalde kenmerken zijn beter voor het voorspellen van het doel dan andere. Bovendien sommige functies mogelijk een nauwe correlatie met andere functies en nieuwe gegevens wordt niet toevoegen aan het model. Deze functies kunnen worden verwijderd.

U kunt voor het bouwen van een model, de beschikbare functies gebruiken of een subset van de functies selecteren.

## <a name="choose-and-apply-a-learning-algorithm"></a>Een leeralgoritme kiezen en toepassen

Maken van een model met de **Two-Class Logistic Regression** module en de service op de gegevensset training te trainen. 

Het resultaat van de **Train Model** -module is een getraind classificatie-model dat kan worden gebruikt om te beoordelen nieuwe voorbeelden om voorspellingen te maken. Gebruik de toets instellen voor het genereren van scores van het getrainde modellen. Gebruik vervolgens de **Evaluate Model** module te analyseren en de kwaliteit van de modellen te vergelijken.

Nadat u het experiment uitvoert, vindt u de uitvoer van de **Score Model** module door te klikken op de uitvoerpoort en selecteer **Visualize**. De uitvoer bevat de scored labels en de kansen voor de labels.

Als u wilt testen van de kwaliteit van de resultaten, voeg de **Evaluate Model** module naar het experimentcanvas en koppel de linkerinvoerpoort aan de uitvoer van de module Score Model. Voer het experiment uit en geef de uitvoer van de **Evaluate Model** -module, door te klikken op de uitvoerpoort en selecteer **Visualize**.

## <a name="evaluate"></a>Evalueren
Het model voor logistieke regressie heeft AUC van 0.631 op de testmachine instellen.

 ![evalueren](media/ui-sample-classification-predict-flight-delay/evaluate.png)

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 1: regressie: De prijs van een auto's voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voorbeeld 2: regressie: Algoritmen voor auto's voorspellen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
- [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Voorbeeld 5 - indeling: Verloop voorspellen](ui-sample-classification-predict-churn.md)
