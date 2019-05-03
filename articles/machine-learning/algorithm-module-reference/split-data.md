---
title: 'Gegevens splitsen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Split Data in Azure Machine Learning-service naar een gegevensset opdelen in twee verschillende sets.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7395280ed9a2e9dcb94a081f0b3bf10a28da719
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029398"
---
# <a name="split-data-module"></a>Module split Data

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een gegevensset opdelen in twee verschillende sets.

Deze module is bijzonder nuttig wanneer u nodig hebt voor het scheiden van gegevens in trainings- en testsets. U kunt de manier waarop dat gegevens ook worden verdeeld. Sommige opties ondersteunen willekeurig toepassen van gegevens. anderen zijn geschikt voor een bepaald gegevenstype of modeltype.

## <a name="how-to-configure"></a>Configureren

> [!TIP]
> U moet voordat u de splitsen modus lezen van alle opties om te bepalen van het type splitsen.
> Als u de modus voor splitsen wijzigt, alle andere opties kunnen opnieuw worden ingesteld.

1. Voeg de **Split Data** module aan uw experiment in de interface. U vindt deze module onder **gegevenstransformatie**, in de **voorbeeld en Split** categorie.

2. **Modus splitsen**: Kies een van de volgende modi, afhankelijk van het type gegevens die u hebt en hoe u wilt het verdelen. Elke splitsen modus heeft verschillende opties. Klik op de volgende onderwerpen voor gedetailleerde instructies en voorbeelden. 

    - **Rijen splitsen**: Gebruik deze optie als u alleen wilt verdelen van de gegevens uit twee delen. Kunt u het percentage van de gegevens in elke splitsing te plaatsen, maar standaard 50 tot 50 in de gegevens worden verdeeld.

        U kunt ook de selectie van rijen in elke groep een willekeurige en gebruiken van de toepassing stratificatie steekproeven. In de toepassing stratificatie steekproeven, moet u een één kolom met gegevens waarvoor u de waarden die moeten worden evenredig worden verdeeld tussen de twee resultaat gegevenssets wilt selecteren.  

    - **Reguliere expressie Split** Kies deze optie als u uw gegevensset delen wilt door het testen van één kolom voor een waarde.

        Bijvoorbeeld, als u sentiment analyseren, kan u controleren op de aanwezigheid van de naam van een bepaald product in een tekstveld en deelt u de gegevensset in rijen met de naam van het doel-product en gebruikers.

    - **Relatieve Expression gesplitste**:  Gebruik deze optie als u wilt een voorwaarde op een kolom met toepast. Het getal kan worden een datum/tijd-veld, een kolom met leeftijd of dollar bedragen of zelfs een percentage. Bijvoorbeeld, als u wilt delen van uw gegevensset, afhankelijk van de kosten van de artikelen, groep mensen door leeftijd adresbereiken of afzonderlijke gegevens door een datum.

### <a name="split-rows"></a>Rijen splitsen
1.  Voeg de [Split Data](./split-data.md) module aan uw experiment in de interface, en verbinding maken met de gegevensset die u wilt splitsen.
  
2.  Voor **splitsen modus**, kiest u **rijen splitsen**. 

3.  **Fractie van rijen in de eerste uitvoergegevensset**. Gebruik deze optie om te bepalen hoeveel rijen u gaat u naar de uitvoer van de eerste (links). Alle andere rijen wordt omgeleid naar de uitvoer van de tweede (rechts).

    De verhouding tussen vertegenwoordigt het percentage van de rijen die worden verzonden naar de eerste uitvoergegevensset, zodat u moet een decimaal getal tussen 0 en 1 typen.
     
     Bijvoorbeeld, als u 0,75 als de waarde typt, kan de gegevensset zou worden gesplitst met behulp van een verhouding 75:25 met 75% van de rijen zijn verzonden naar de eerste uitvoergegevensset en 25% naar de tweede uitvoergegevensset verzonden.
  
4. Selecteer de **Randomized splitsen** optie als u wilt een willekeurige selectie van de gegevens in de twee groepen. Dit is de aanbevolen optie bij het maken van trainings- en testset gegevenssets.

5.  **Random Seed**: Typ een niet-negatief geheel getal-waarde voor het initialiseren van de pseudo-willekeurige volgorde van de exemplaren moeten worden gebruikt. Deze standaard seed wordt gebruikt in alle modules die willekeurige getallen genereren. 

     Een seed op te geven, maakt de resultaten in het algemeen reproduceerbare. Als u herhalen van de resultaten van een splitsbewerking wilt, moet u een seed voor de generator van willekeurige getallen opgeven. Anders is de random seed standaard ingesteld op 0, wat betekent dat de eerste seed-waarde wordt opgehaald uit de systeemklok. Als gevolg hiervan de verdeling van gegevens mogelijk enigszins telkens wanneer die u een splitsing uitvoeren. 

6. **Toepassing stratificatie split**: Deze optie instelt op **waar** om ervoor te zorgen dat de twee uitvoergegevenssets bevatten een representatieve steekproef van de waarden in de *strata kolom* of *indeling sleutelkolom*. 

    Met toepassing stratificatie Samples, worden de gegevens worden verdeeld, zodat elke uitvoergegevensset ongeveer hetzelfde percentage van de doel-waarde opgehaald. Bijvoorbeeld, mogelijk u ervoor wilt zorgen dat uw trainings- en testsets grofweg worden verdeeld met betrekking tot het resultaat of met inachtneming ot sommige andere kolom, zoals geslacht.

7. Voer het experiment uit.


## <a name="regular-expression-split"></a>Reguliere expressie splitsen

1.  Toevoegen de [Split Data](./split-data.md) module naar het experimentcanvas en koppel ze als invoer voor de gegevensset die u wilt splitsen.  
  
2.  Voor **splitsen modus**, selecteer **reguliere expressie split**.

3. In de **reguliere expressie** typt u een geldige reguliere expressie. 
  
   De reguliere expressie moet syntaxis met gewone uitdrukkingen Python volgen.


4. Voer het experiment uit.

    Op basis van de reguliere expressie die u opgeeft, de gegevensset is onderverdeeld in twee sets met rijen: rijen met waarden die overeenkomen met de expressie en alle overige rijen. 

## <a name="relative-expression-split"></a>Relatieve expressie splitsen.

1. Toevoegen de [Split Data](./split-data.md) module naar het experimentcanvas en koppel ze als invoer voor de gegevensset die u wilt splitsen.
  
2. Voor **splitsen modus**, selecteer **relatieve expression gesplitste**.
  
3. In de **relationele expressie** typt u een expressie die een bewerking voor tekenreeksvergelijking, op één kolom uitvoert:


 - De numerieke kolommen:
    - De kolom bevat de nummers van elk type zijn numerieke gegevens, met inbegrip van datum/tijd-gegevenstypen.

    - De expressie kan verwijzen naar een maximum van de naam van één kolom.

    - Gebruik het teken en-teken (&) voor de AND-bewerking en gebruik de pipe (|) teken voor de OR-bewerking.

    - De volgende operators worden ondersteund: `<`, `>`, `<=`, `>=`, `==`, `!=`

    - U kunt geen bewerkingen groeperen met behulp van `(` en `)`.

 - Kolom met tekenreeksen: 
    - De volgende operators worden ondersteund: `==`, `!=`



4. Voer het experiment uit.

    De expressie verdeelt de gegevensset in twee sets met rijen: rijen met waarden die voldoen aan de voorwaarde, en alle overige rijen.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 