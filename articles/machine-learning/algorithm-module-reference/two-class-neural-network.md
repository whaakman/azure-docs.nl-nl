---
title: 'Two-Class Neural Network: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Two-Class Neural Network in Azure Machine Learning-service te maken van een neural network-model dat kan worden gebruikt voor het voorspellen van een doel dat slechts twee waarden heeft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7ea852fcd312c6f7b1b716278ed538b7accde5bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029218"
---
# <a name="two-class-neural-network-module"></a>Two-Class Neural Network-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een neural network-model dat kan worden gebruikt voor het voorspellen van een doel dat slechts twee waarden heeft.

Classificatie met behulp van neurale netwerken is een methode voor het leren met supervisie, en daarom vereist een *gelabeld gegevensset*, waaronder een labelkolom. U kunt bijvoorbeeld dit neural network-model gebruiken om te voorspellen binaire resultaten zoals of een patiënt is een bepaalde ziekten, of dat een virtuele machine is waarschijnlijk zal mislukken binnen een opgegeven periode.  

Nadat u het model gedefinieerd, service door op te geven van een label gegevensset en het model als invoer voor trainen [Train Model](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor de nieuwe invoer te voorspellen.

### <a name="more-about-neural-networks"></a>Meer informatie over het neurale netwerken

Een neuraal netwerk is een set met elkaar verbonden lagen. De invoer voor de eerste laag zijn en zijn verbonden met een laag voor uitvoer door een acyclische grafiek die bestaat uit gewogen randen en knooppunten.

U kunt meerdere verborgen lagen invoegen tussen de lagen van de invoer en uitvoer. Meest voorspellende taken kunnen u eenvoudig doen met slechts één of een paar verborgen lagen. Echter is recent onderzoek gebleken dat (DNN) deep neural networks met veel lagen van kracht in complexe taken, zoals afbeelding of spraakherkenning worden kunnen. De volgende lagen worden gebruikt om het toenemende niveaus van de diepte van de semantische model.

De relatie tussen de invoer en uitvoer wordt geleerd van het neurale netwerk op de ingevoerde gegevens. De richting van de grafiek opbrengsten uit de invoer door de verborgen laag en de uitvoer-laag. Alle knooppunten in een laag zijn verbonden met de gewogen randen voor knooppunten in de volgende laag.

Een waarde wordt berekend voor het berekenen van de uitvoer van het netwerk voor een bepaalde invoer, op elk knooppunt in de verborgen lagen en in de uitvoer-laag. De waarde is ingesteld door het berekenen van de gewogen som van de waarden van de knooppunten uit de vorige laag. Een functie activering wordt vervolgens toegepast op deze gewogen som.
  
## <a name="how-to-configure"></a>Configureren

1.  Voeg de **Two-Class Neural Network** module naar het experimentcanvas. U vindt deze module onder **Machine Learning**, **initialiseren**, in de **classificatie** categorie.  
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Selecteer deze optie als u al weet hoe u wilt configureren van het model.  

3.  Voor **verborgen laag specificatie**, selecteer het type van netwerkarchitectuur te maken.  
  
    -   **Volledig verbonden geval**: Maakt gebruik van de standaard neural network-architectuur voor twee klassen neurale netwerken als volgt gedefinieerd:
  
        -   Is een verborgen laag.
  
        -   De uitvoer-laag is volledig wordt verbonden met de verborgen laag en de verborgen laag volledig wordt verbonden met de invoer-laag.
  
        -   Het aantal knooppunten in de invoer-laag is gelijk aan het aantal functies in de trainingsgegevens.
  
        -   Het aantal knooppunten in de verborgen laag is ingesteld door de gebruiker. De standaardwaarde is 100.
  
        -   Het aantal knooppunten is gelijk aan het aantal klassen. Dit betekent dat alle invoer moeten worden toegewezen aan een van twee knooppunten in de uitvoer-laag voor een neural network twee klassen.

5.  Voor **leertempo**, definieer de grootte van de stap die bij elke herhaling, voordat u correctie worden genomen. Een grotere waarde op voor het leren van tarief kan leiden tot het model te convergeren sneller, maar het lokale minima kunt overschrijding.

6.  Voor **aantal iteraties learning**, geef het maximum aantal keren dat de trainingscases moet worden verwerkt door de algoritme.

7.  Voor **de initiële leerperiode gewicht diameter**, geef het gewicht van het knooppunt aan het begin van het leerproces.

8.  Voor **het momentum**, Geef een gewicht om toe te passen tijdens learning naar knooppunten uit de vorige iteraties  

10. Selecteer de **voorbeelden in willekeurige volgorde** optie als u wilt aanvragen in willekeurige volgorde tussen herhalingen. Als u deze optie uitschakelt, worden aanvragen verwerkt in dezelfde volgorde telkens wanneer die u het experiment uitvoert.
  
11. Voor **willekeurig getal seed**, typt u een waarde om te gebruiken als de seed.
  
     Een seed op te geven is waarde handig als u ervoor wilt zorgen herhaalbaarheid voor het hetzelfde experiment uitvoert.  Anders wordt de waarde van een system clock gebruikt als de seed, waardoor verschillende resultaten telkens wanneer die u het experiment uitvoert.
  
13. Een gegevensset met tags toevoegen aan het experiment en verbinding maken met een van de [trainingsmodules](module-reference.md).  
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](train-model.md) module.  
  
14. Voer het experiment uit.

## <a name="results"></a>Resultaten

Na de training is voltooid:

+ Voor een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training en andere parameters van het neurale netwerk, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md), en selecteer **Visualize**.  

+ Om een momentopname van het getrainde model hebt opgeslagen, met de rechtermuisknop op de **Trained model** uitvoer en selecteer **opslaan als getrainde Model**. Dit model wordt niet bijgewerkt op het hetzelfde experiment opeenvolgende is uitgevoerd.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
