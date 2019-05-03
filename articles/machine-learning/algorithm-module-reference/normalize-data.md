---
title: 'Gegevens normaliseren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module gegevens normaliseren in Azure Machine Learning-service om te zetten van een gegevensset via *normalisering*...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 95069bafa94770511c7ee40e82068960298fd6c5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029443"
---
# <a name="normalize-data-module"></a>Data-module normaliseren

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een gegevensset via transformeren *normalisering*.

Normalisatie is een techniek die vaak worden toegepast als onderdeel van de gegevens voor te bereiden voor machine learning. Het doel van normalisering is om te wijzigen van de waarden van de numerieke kolommen in de gegevensset te gebruiken een gemeenschappelijke schaal, zonder vervormen verschillen in het bereik van waarden of informatie verloren gaat. Normalisatie is ook vereist voor sommige algoritmen om de gegevens correct te modelleren.

Stel bijvoorbeeld dat de invoergegevensset bevat één kolom met waarden tussen 0 en 1 en een andere kolom met waarden die variëren van 10.000 tot 100.000. De grote verschil de *schaal* van de getallen kan problemen veroorzaken wanneer u de waarden als functies combineren tijdens het maken van modellering.

*Normalisering* vermijdt u deze problemen met het maken van nieuwe waarden die de algemene distributie en een ratio's in de brongegevens onderhouden terwijl de waarden in een schaalset die is toegepast op alle numerieke kolommen in het model gebruikt.

Deze module biedt verschillende opties voor het transformeren van numerieke gegevens:

- U kunt alle waarden wijzigen in 0-1 schalen of de waarden transformeert door vertegenwoordigen als percentiel ranks in plaats van absolute waarden.
- U kunt normalisering kunt toepassen op één kolom, of op meerdere kolommen in dezelfde gegevensset.
- Als u moet het experiment herhalen of dezelfde stappen normalisering van toepassing op andere gegevens, kunt u de stappen opslaan als een transformatie normalisering en toepassen op andere gegevenssets die hetzelfde schema hebben.

> [!WARNING]
> Sommige algoritmen is vereist dat gegevens worden genormaliseerd voor het trainen van een model. Andere algoritmen voeren hun eigen gegevens schalen of normalisering. Als u een machine learning-algoritme wordt gebruikt kiest bij het bouwen van een Voorspellend model, dus zorg ervoor dat u het bekijken van de vereisten voor gegevens van het algoritme voordat normalisering toepast op de trainingsgegevens.

##  <a name="configure-normalize-data"></a>Configureer gegevens normaliseren

U kunt slechts één normalisering methode toepassen op een tijdstip met behulp van deze module. Daarom wordt de dezelfde normalisatie-methode toegepast op alle kolommen die u selecteert. Voor het gebruik van normalisering van verschillende methoden, gebruikt u een tweede exemplaar van **gegevens normaliseren**.

1. Voeg de **gegevens normaliseren** module naar het experimentcanvas. U vindt de module In Azure Machine Learning, onder **gegevenstransformatie**, in de **schaal- en Reduce** categorie.

2. Verbinding maken met een gegevensset die ten minste één kolom van alle getallen bevat.

3. Gebruik de kolomkiezer om de numerieke kolommen normaliseren. Als u geen afzonderlijke kolommen standaard **alle** numerieke kolommen in de invoer zijn opgenomen en de dezelfde normalisatieproces wordt toegepast op alle geselecteerde kolommen. 

    Dit kan leiden tot vreemd resultaten als u numerieke kolommen die niet moeten worden genormaliseerd! Controleer altijd zorgvuldig de kolommen.

    Als er geen numerieke kolommen worden gedetecteerd, controleert u de metagegevens van de kolommen om te bevestigen dat het gegevenstype van de kolom een ondersteunde numeriek type.

    > [!TIP]
    > Om ervoor te zorgen dat de kolommen van een bepaald type worden opgegeven als invoer, probeert u het [Select Columns in Dataset](./select-columns-in-dataset.md) module voordat **gegevens normaliseren**.

4. **Gebruik 0 voor constante kolommen wanneer dit selectievakje inschakelt**:  Selecteer deze optie als u een numerieke kolom bevat één onveranderlijke waarde. Dit zorgt ervoor dat dergelijke kolommen in normalisering bewerkingen niet worden gebruikt.

5. Uit de **transformatiemethode** vervolgkeuzelijst kiest u een enkele rekenkundige functie toe te passen op alle geselecteerde kolommen. 
  
    - **Zscore**: Converteert alle waarden naar een z-score.
    
      De waarden in de kolom worden getransformeerd met behulp van de volgende formule:  
  
      ![met behulp van z normalisering&#45;scores](media/module/aml-normalization-z-score.png)
  
      Afwijking van gemiddelde en standard worden afzonderlijk berekend voor elke kolom. Standaarddeviatie wordt gebruikt.
  
    - **MinMax**: De min-max normalisatiefunctie aangepast lineair toe aan elke functie voor het interval [0,1].
    
      Schaling aanpassen voor het interval [0,1] wordt uitgevoerd door de waarden van elke functie verandert, zodat de minimale waarde 0 is, maar vervolgens delen door de nieuwe maximale waarde (dit is het verschil tussen de oorspronkelijke waarden van de maximale en minimale).
      
      De waarden in de kolom worden getransformeerd met behulp van de volgende formule:  
  
      ![met behulp van de minimale waarde normalisering&#45;functie max](media/module/aml-normalization-minmax.png "AML_normalization minmax")  
  
    - **Logistieke**: De waarden in de kolom worden getransformeerd met behulp van de volgende formule:

      ![formule voor het normaliseren door logistieke functie](media/module/aml-normalization-logistic.png "AML_normalization logistieke")  
  
    - **Logaritmische**: Deze optie worden alle waarden geconverteerd naar een logaritmische schaal.
  
      De waarden in de kolom worden getransformeerd met behulp van de volgende formule:
  
      ![formule log&#45;normale distributie](media/module/aml-normalization-lognormal.png "AML_normalization logaritmische")
    
      Hier volgen de parameters van de distributie, empirisch uit de gegevens als maximale kans naar schatting voor elke kolom afzonderlijk berekend μ en σ.  
  
    - **TanH**: Alle waarden worden geconverteerd naar een hyperbolische tangens.
    
      De waarden in de kolom worden getransformeerd met behulp van de volgende formule:
    
      ![met de functie tanh normalisering](media/module/aml-normalization-tanh.png "AML_normalization tanh")

6. Voer het experiment uit en dubbelklik op de **gegevens normaliseren** -module en selecteer **geselecteerde uitvoeren**. 

## <a name="results"></a>Resultaten

De **gegevens normaliseren** module genereert twee uitvoer:

- Als u de getransformeerde waarden, met de rechtermuisknop op de module, selecteert u **getransformeerde gegevensset**, en klikt u op **Visualize**.

    Standaard worden getransformeerd en waarden in plaats. Als u vergelijken van de getransformeerde waarden naar de oorspronkelijke waarden wilt, gebruikt u de [kolommen toevoegen](./add-columns.md) module weer van de gegevenssets en bekijk de kolommen naast elkaar.

- Als u wilt de transformatie opslaan zodat u kunt dezelfde methode normalisering toepassen op een andere soortgelijke gegevensset, met de rechtermuisknop op de module, selecteert u **transformatiefunctie**, en klikt u op **opslaan als transformatie**.

    U kunt vervolgens de opgeslagen transformaties uit laden de **transformeert** groep van het navigatiedeelvenster links en dit toepassen op een gegevensset met hetzelfde schema met behulp van [. / transformatie toepassen](apply-transformation.md).  


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 