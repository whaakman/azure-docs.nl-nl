---
title: Ondersteund inspectors beschikbaar met Azure Machine Learning gegevens voorbereiden | Microsoft Docs
description: Dit document bevat een volledige lijst van inspecteurs beschikbaar voor Azure Machine Learning gegevens voorbereiden
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
ms.date: 09/11/2017
ms.openlocfilehash: ff5fcbc6df8cb07e0b98b877f20d981d6bef5117
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Inspectors ondersteund voor de preview van Azure Machine Learning gegevens voorbereiden
Dit document bevat een overzicht van de reeks inspectors die beschikbaar in deze preview zijn.

## <a name="the-halo-effect"></a>Het effect halo 
Sommige inspectors ondersteuning voor het effect halo. Daartoe maakt gebruik van twee verschillende kleuren om de wijziging van een transformatie visueel onmiddellijk weer te geven. De grijze vertegenwoordigt de waarde hebben voorafgaand aan de meest recente transformatie, en de blauwe wordt de huidige waarde. Hiertoe worden ingeschakeld en in de opties uitgeschakeld.

## <a name="graphical-filtering"></a>Grafische filteren 
Sommige van de inspecteurs ondersteuning voor het filteren van gegevens met behulp van de inspector als een editor. Met behulp van de inspector als een editor omvat een grafische elementen te selecteren en vervolgens via de werkbalk in de rechterbovenhoek van het venster inspector te filteren in of uit de geselecteerde waarden. 

## <a name="column-statistics"></a>Kolomstatistieken
Deze controle biedt voor numerieke kolommen tal van verschillende statistieken over de kolom. Statistieken zijn onder andere de volgende metingen: 
- Minimum
- Lagere kwartiel
- Mediaan
- Bovenste kwartiel
- Maximum
- Gemiddeld
- Standaarddeviatie


### <a name="options"></a>Opties 
- None

## <a name="histogram"></a>Histogram 
Wordt berekend en een histogram van een enkele numerieke kolom wordt weergegeven. Het aantal buckets dat wordt berekend met behulp van Scott regel. De regel kan echter via de opties worden genegeerd.

Deze controle ondersteunt het effect halo.


### <a name="options"></a>Opties
- Minimum aantal buckets (dit geldt zelfs wanneer bucketing standaard is ingeschakeld)
- Standaardaantal buckets (van Scott regel) 
- Halo weergeven
- Kernel dichtheid tekent overlay (Gaussiaans kernel) 


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via buckets, met inbegrip van enkelvoudige of meervoudige selectie buckets. Zoals eerder beschreven filters toepassen.

## <a name="value-counts"></a>Telt het aantal waarde
Deze controle geeft een Frequentietabel van waarden voor de kolom die momenteel is geselecteerd. De standaardweergave is voor de eerste zes waarden. U kunt echter de limiet wijzigen op een willekeurig getal. U kunt ook de weergave voor het tellen van onderaf in plaats van de top instellen. Deze controle ondersteunt het effect halo.

### <a name="options"></a>Opties 
- Aantal bovenste waarden
- Aflopend
- Null/fout waarden bevatten
- Halo weergeven


### <a name="actions"></a>Acties 
Deze controle biedt ondersteuning voor filteren via staven, met inbegrip van enkelvoudige of meervoudige selectie balken. Zoals eerder beschreven filters toepassen.

## <a name="box-plot"></a>BoxPlot 
Een whisker BoxPlot van een numerieke kolom.

### <a name="options"></a>Opties 
- Groeperen op kolom

## <a name="scatter-plot"></a>Spreiding van de grafiek
Spreidingsgrafiek tekent voor twee numerieke kolommen. De gegevens wordt-oogpunt van prestaties. Grootte van de steekproef kan worden genegeerd in de opties.

### <a name="options"></a>Opties  
- X-as-kolom
- Y-as-kolom
- De grootte van steekproef
- Groeperen op kolom


## <a name="time-series"></a>Tijdreeks
Een lijndiagram waarin de afhankelijkheid van de tijd op de x-as.

### <a name="options"></a>Opties
- Datumkolom
- Numerieke kolom
- De grootte van steekproef


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via een methode klikken en slepen select om een bereik op de grafiek te selecteren. Nadat u de selectie hebt voltooid, toepassen van filters zoals eerder beschreven.


## <a name="map"></a>Kaart 
Een kaart met punten die worden getekend, ervan uitgaande dat de breedtegraad en lengtegraad zijn opgegeven. Breedtegraad moet eerst worden geselecteerd.

### <a name="options"></a>Opties
- Breedtegraad kolom
- Lengtegraad kolom
- Clustering voor
- Groeperen op kolom


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via de selectie op de kaart. Druk op de **Ctrl** sleutel, en klik en sleep met de muis naar het formulier een vierkant rond de punten. Zoals eerder beschreven filters vervolgens toepassen.

U kunt snel het formaat van de kaart zodanig dat alleen de mogelijke punten door op de **E** aan de linkerkant van de kaart.


## <a name="pattern-frequency"></a>Patroon frequentie 

Deze controle wordt een lijst met patronen in de geselecteerde kolom in de tekenreeks. De patronen worden weergegeven met een reguliere expressie zoals syntaxis. Muiswijzer op het patroon, ziet u de voorbeelden van waarden die worden vertegenwoordigd door dit patroon. De geschatte coverages in termen van percentage wordt ook weergegeven samen met de patronen.

![Afbeelding van het patroon inspector](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Opties
- Aantal bovenste waarden
- Aflopend
- Halo weergeven

### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren op basis van patronen weergegeven. Druk op de **Ctrl** sleutel en selecteer vervolgens de gevulde balken in patroon inspector. Zoals eerder beschreven filters vervolgens toepassen. Als gevolg van de acion gebruiker wordt een stap geavanceerde filter toegevoegd. U kunt zien en de gegenereerde Python-code wijzigen door het aanroepen van de optie voor het bewerken van de geavanceerde Filter-stap.
