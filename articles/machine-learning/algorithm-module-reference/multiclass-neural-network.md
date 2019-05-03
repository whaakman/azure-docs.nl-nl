---
title: 'Multiklassen Neural Network: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Multiklasse Neural Network in Azure Machine Learning-service te maken van een neural network-model dat kan worden gebruikt om een doel met meerdere waarden te voorspellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7bef225c001ebd9bbb9a45c8bcc301cfb49edb6
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029383"
---
# <a name="multiclass-neural-network-module"></a>Multiklassen Neural Network-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een neural network-model dat kan worden gebruikt om een doel met meerdere waarden te voorspellen. 

Neurale netwerken van dit type kunnen bijvoorbeeld worden gebruikt in complexe computer vision-taken, zoals het cijfer of letter erkenning documentclassificatie en patroonherkenning.

Classificatie met behulp van neurale netwerken is een methode voor het leren met supervisie, en daarom vereist een *gelabeld gegevensset* die een labelkolom bevat.

U kunt het model te trainen door te geven van het model en de label-gegevensset als invoer voor [Train Model](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor de nieuwe invoer voorbeelden te voorspellen.  

## <a name="about-neural-networks"></a>Over neurale netwerken

Een neuraal netwerk is een set met elkaar verbonden lagen. De invoer voor de eerste laag zijn en zijn verbonden met een laag voor uitvoer door een acyclische grafiek die bestaat uit gewogen randen en knooppunten.

U kunt meerdere verborgen lagen invoegen tussen de lagen van de invoer en uitvoer. Meest voorspellende taken kunnen u eenvoudig doen met slechts één of een paar verborgen lagen. Echter is recent onderzoek gebleken dat (DNN) deep neural networks met veel lagen van kracht in complexe taken, zoals afbeelding of spraakherkenning worden kunnen. De volgende lagen worden gebruikt om het toenemende niveaus van de diepte van de semantische model.

De relatie tussen de invoer en uitvoer wordt geleerd van het neurale netwerk op de ingevoerde gegevens. De richting van de grafiek opbrengsten uit de invoer door de verborgen laag en de uitvoer-laag. Alle knooppunten in een laag zijn verbonden met de gewogen randen voor knooppunten in de volgende laag.

Een waarde wordt berekend voor het berekenen van de uitvoer van het netwerk voor een bepaalde invoer, op elk knooppunt in de verborgen lagen en in de uitvoer-laag. De waarde is ingesteld door het berekenen van de gewogen som van de waarden van de knooppunten uit de vorige laag. Een functie activering wordt vervolgens toegepast op deze gewogen som.

## <a name="configure-multiclass-neural-network"></a>Multiklassen Neural Network configureren

1. Voeg de **Multiklasse Neural Network** module aan uw experiment in de interface. U vindt deze module onder **Machine Learning**, **initialiseren**, in de **classificatie** categorie.

2. **Trainer aanmaakmodus**: Gebruik deze optie om op te geven hoe u wilt dat het model te trainen:

    - **Eén Parameter**: Selecteer deze optie als u al weet hoe u wilt configureren van het model.

    

3. **Verborgen laag specificatie**: Selecteer het type van de netwerkarchitectuur te maken.

    - **Volledig verbonden geval**: Selecteer deze optie om een model met behulp van de standaard neural network-architectuur te maken. Voor multiklassen neural network-modellen zijn de standaardwaarden als volgt uit:

        - Een verborgen laag
        - De uitvoer-laag is volledig wordt verbonden met de verborgen laag.
        - De verborgen laag is volledig wordt verbonden met de invoer-laag.
        - Het aantal knooppunten in de invoer-laag wordt bepaald door het aantal functies in de trainingsgegevens.
        - Het aantal knooppunten in de verborgen laag kan worden ingesteld door de gebruiker. De standaardwaarde is 100.
        - Het aantal knooppunten in de uitvoer-laag is afhankelijk van het aantal klassen.
  
   

5. **Aantal verborgen knooppunten**: Deze optie kunt u het aantal verborgen knooppunten in de standaard-architectuur aanpassen. Typ het aantal verborgen knooppunten. De standaardwaarde is een verborgen laag met 100 knooppunten.

6. **Het leertempo**: Definieer de grootte van de stap die bij elke herhaling, voordat u correctie worden genomen. Een grotere waarde op voor het leren van tarief kan leiden tot het model te convergeren sneller, maar het lokale minima kunt overschrijding.

7. **Aantal herhalingen learning**: Geef het maximum aantal keren dat de trainingscases moet worden verwerkt door de algoritme.

8. **De initiële leerperiode gewicht diameter**: Geef het gewicht van het knooppunt aan het begin van het leerproces.

9. **De groei voor**: Geef een gewicht om toe te passen tijdens learning naar knooppunten uit de vorige herhalingen.
  
11. **Voorbeelden in willekeurige volgorde**: Selecteer deze optie om de aanvragen in willekeurige volgorde tussen herhalingen.

    Als u deze optie uitschakelt, worden aanvragen verwerkt in dezelfde volgorde telkens wanneer die u het experiment uitvoert.

12. **Willekeurig getal seed**: Typ een waarde om te gebruiken als de seed, als u ervoor wilt zorgen herhaalbaarheid voor het hetzelfde experiment uitvoert.

14. Verbinding maken met een gegevensset training en een van de [trainingsmodules](module-reference.md): 

    - Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u [Train Model](train-model.md).  
  

## <a name="results"></a>Resultaten

Na de training is voltooid:

- Voor een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training en andere parameters van het neurale netwerk, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md) en selecteer **Visualize**.  

- Om een momentopname van het getrainde model hebt opgeslagen, met de rechtermuisknop op de **Trained model** uitvoer en selecteer **opslaan als getrainde Model**. Dit model wordt niet bijgewerkt op het hetzelfde experiment opeenvolgende is uitgevoerd.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 