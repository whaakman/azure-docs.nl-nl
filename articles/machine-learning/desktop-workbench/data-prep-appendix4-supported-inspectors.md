---
title: Inspectors beschikbaar voor gegevensvoorbereiding van Azure Machine Learning ondersteund | Microsoft Docs
description: Dit document bevat een volledige lijst van inspectors beschikbaar voor gegevensvoorbereiding van Azure Machine Learning
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
ms.openlocfilehash: ef5f6f3dc7ae0c555b2afe000b54c443313800f1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645734"
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Inspectors ondersteund voor de preview van Azure Machine Learning gegevens voorbereiden
Dit document bevat een overzicht van de set inspectors die beschikbaar in deze Preview-versie zijn.

## <a name="the-halo-effect"></a>Het halo-effect 
Sommige inspectors ondersteuning voor het halo-effect. Dit effect maakt gebruik van twee verschillende kleuren om de wijziging visueel ten opzichte van een transformatie onmiddellijk weer te geven. De grijze vertegenwoordigt de waarde hebben voorafgaand aan de meest recente transformatie, en de blauwe ziet u de huidige waarde. Dit effect kan worden ingeschakeld en in de opties uitgeschakeld.

## <a name="graphical-filtering"></a>Grafische filteren 
Enkele van de inspectors ondersteuning voor het filteren van gegevens met behulp van de inspector als een editor. De inspector gebruikt als een editor omvat grafische elementen te selecteren en vervolgens met behulp van de werkbalk in de rechterbovenhoek van het venster inspector om te filteren in of uit de geselecteerde waarden. 

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
- Geen

## <a name="histogram"></a>Histogram 
Berekent en ziet u een histogram van één numerieke kolom. Het aantal buckets wordt berekend met behulp van de regel Scott's. De regel kan echter via de opties worden genegeerd.

Deze controle biedt ondersteuning voor het halo-effect.


### <a name="options"></a>Opties
- Minimum aantal buckets (dit geldt zelfs wanneer bucketing standaard is ingeschakeld)
- Standaardaantal buckets (regel Scott's) 
- Halo weergeven
- Kernel dichtheid plot overlay (Gaussiaans kernel) 
- Logaritmische schaal gebruiken


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via buckets, waaronder de enkelvoudige of meervoudige selectie buckets. Zoals eerder beschreven filters toepassen.

## <a name="value-counts"></a>Telt het aantal waarde
Deze controle geeft een Frequentietabel van de waarden voor de kolom die momenteel is geselecteerd. De standaardweergave is voor de eerste zes waarden. U kunt echter de limiet wijzigen op een willekeurig getal. U kunt ook de weergave te tellen van het laagste in plaats van de top instellen. Deze controle biedt ondersteuning voor het halo-effect.

### <a name="options"></a>Opties 
- Aantal bovenste waarden
- Aflopend
- Null/fout-waarden bevatten
- Halo weergeven
- Logaritmische schaal gebruiken


### <a name="actions"></a>Acties 
Deze controle biedt ondersteuning voor filteren via staven, waaronder de enkelvoudige of meervoudige selectie balken. Zoals eerder beschreven filters toepassen.

## <a name="box-plot"></a>BoxPlot 
Een vak whisker-grafiek van een numerieke kolom.

### <a name="options"></a>Opties 
- Groeperen op kolom

## <a name="scatter-plot"></a>Spreidingsplot
Een spreidingsdiagram voor twee numerieke kolommen. De gegevens wordt-voor betere prestaties. De grootte van de steekproef kan worden genegeerd in de opties.

### <a name="options"></a>Opties  
- X-as-kolom
- Y-as-kolom
- De grootte van steekproef
- Groeperen op kolom


## <a name="time-series"></a>Tijdreeks
Een lijndiagram met awareness tijd op de x-as.

### <a name="options"></a>Opties
- Datumkolom
- De numerieke kolommen
- De grootte van steekproef


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via een methode klikken en slepen select om een bereik in de grafiek te selecteren. Nadat u de selectie hebt voltooid, toepassen van filters zoals eerder is beschreven.


## <a name="map"></a>Kaart 
Een kaart met punten die worden getekend, ervan uitgaande dat de breedtegraad en lengtegraad zijn opgegeven. Breedtegraad moet eerst worden geselecteerd.

### <a name="options"></a>Opties
- Breedtegraad kolom
- Lengtegraad kolom
- Clustering voor
- Groeperen op kolom


### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren via de selectie op de kaart. Druk op de **Ctrl** sleutel, en klik en sleep met de muis om te vormen een vierkant rond de punten. Zoals eerder beschreven filters toepassen.

U kunt snel het formaat van de kaart om weer te geven alleen de mogelijke punten door te drukken de **E** aan de linkerkant van de kaart.


## <a name="pattern-frequency"></a>Patroon frequentie 

Deze controle wordt een lijst van patronen in de geselecteerde kolom met tekenreeksen. De patronen worden weergegeven met een reguliere expressie, zoals syntaxis. Aanwijzen van het patroon, ziet u de voorbeelden van waarden die worden vertegenwoordigd door dit patroon. De geschatte coverages in termen van percentage wordt ook weergegeven samen met de patronen.

![Afbeelding van het patroon inspector](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Opties
- Aantal bovenste waarden
- Aflopend
- Halo weergeven

### <a name="actions"></a>Acties
Deze controle biedt ondersteuning voor filteren op basis van patronen weergegeven. Druk op de **Ctrl** sleutel, en selecteer vervolgens de gevulde balken in patroon inspector. Zoals eerder beschreven filters toepassen. Als gevolg van de gebruiker acion, wordt een geavanceerd filterstap toegevoegd. U kunt zien en de gegenereerde Python-code wijzigen door de optie voor het bewerken van de stap Geavanceerd Filter aan te roepen.
