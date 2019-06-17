---
title: 'De ontbrekende gegevens opschonen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Clean Missing Data in Azure Machine Learning-service te verwijderen, vervangen of ontbrekende waarden afleiden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: de81204219a102734f1820258a3c32e59a64c685
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028783"
---
# <a name="clean-missing-data-module"></a>Clean Missing Data-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module wilt verwijderen, vervangen of ontbrekende waarden afleiden. 

Gegevenswetenschappers vaak gegevens voor de ontbrekende waarden te controleren en vervolgens verschillende bewerkingen uitvoeren om te herstellen van de gegevens of nieuwe waarden invoegen. Het doel van dergelijke bewerkingen plaatsvindt reinigingstape is om te voorkomen dat problemen veroorzaakt door ontbrekende gegevens die zich voordoen kunnen bij het trainen van een model. 

Deze module biedt ondersteuning voor meerdere type van de bewerkingen voor "reinigen" ontbrekende waarden, met inbegrip van:

+ Ontbrekende waarden vervangen door een tijdelijke aanduiding, gemiddelde of een andere waarde
+ Verwijderen van alle rijen en kolommen met ontbrekende waarden
+ Voorbeeldgrootte waarden op basis van statistische methoden


Met behulp van deze module wordt niet gewijzigd voor de brongegevensset. In plaats daarvan wordt een nieuwe gegevensset in uw werkruimte die u in de volgende werkstroom gebruiken kunt gemaakt. U kunt ook de nieuwe, opgeschoonde gegevensset voor hergebruik opslaan.

Deze module voert ook een definitie van de transformatie die wordt gebruikt voor het opschonen van de ontbrekende waarden. U kunt deze transformatie opnieuw gebruiken voor andere gegevenssets die hetzelfde schema, met behulp van hebben de [transformatie toepassen](./apply-transformation.md) module.  

## <a name="how-to-use-clean-missing-data"></a>Het gebruik van Clean Missing Data

Deze module kunt u een reinigingstape bewerking definiëren. U kunt ook de reinigingsbewerking opslaan zodat u later op nieuwe gegevens toepassen kunt. Zie de volgende koppelingen voor een beschrijving van het maken en opslaan van een reinigingstape proces: 
 
+ Ontbrekende waarden vervangen
  
+ Een reinigingstape transformatie toepassen op nieuwe gegevens
 
> [!IMPORTANT]
> De reinigingstape methode die u gebruikt voor het verwerken van de ontbrekende waarden kan uw resultaten aanzienlijk beïnvloeden. Het is raadzaam dat u met verschillende methoden experimenteren. Houd rekening met zowel de reden voor gebruik van een specifieke methode en de kwaliteit van de resultaten.

### <a name="replace-missing-values"></a>Ontbrekende waarden vervangen  

Elke keer dat u de [Clean Missing Data](./clean-missing-data.md) module aan een set gegevens en de dezelfde reinigingsbewerking wordt toegepast op alle kolommen die u selecteert. Daarom gebruiken als u nodig hebt voor het opschonen van verschillende kolommen met verschillende methoden, afzonderlijke exemplaren van de module.

1.  Voeg de [Clean Missing Data](./clean-missing-data.md) module naar het experimentcanvas en verbinding maken met de gegevensset met ontbrekende waarden.  
  
2.  Voor **kolommen worden opgeschoond**, kiest u de kolommen met de ontbrekende waarden die u wilt wijzigen. U kunt meerdere kolommen, maar moet u dezelfde vervangingsmethode in alle geselecteerde kolommen. Daarom normaal gesproken moet u op te schonen tekenreekskolommen en numerieke kolommen afzonderlijk.

    Als u bijvoorbeeld om te controleren voor ontbrekende waarden in alle numerieke kolommen:

    1. Open de kolomkiezer en selecteer **met regels**.
    2. Voor **BEGIN WITH**, selecteer **geen kolommen**.

        U kunt ook beginnen met alle kolommen en kolommen vervolgens uitsluiten. In eerste instantie regels worden niet weergegeven als u eerst klikt u op **alle kolommen**, maar u kunt op **geen kolommen** en klik vervolgens op **alle kolommen** opnieuw te starten met alle kolommen en klikt u vervolgens filteren (uitsluiten)-kolommen die zijn gebaseerd op de naam, gegevenstype, of index van de kolommen.

    3. Voor **opnemen**, selecteer **kolomtype** uit de vervolgkeuzelijst en selecteer vervolgens **numerieke**, of een meer specifieke numeriek type. 
  
    Elke gereinigd of vervanging methode die u kiest moet zijn van toepassing op **alle** kolommen in de selectie. Als de gegevens in elke kolom niet compatibel met de opgegeven bewerking is, wordt de module wordt een fout geretourneerd en stopt het experiment.
  
3.  Voor **Minimum ontbrekende waarde verhouding**, geef het minimale aantal ontbrekende waarden vereist voor de bewerking die moet worden uitgevoerd.  
  
    Gebruik van deze optie in combinatie met **Maximum ontbrekende waarde verhouding** om de voorwaarden waaronder een reinigingstape bewerking wordt uitgevoerd op de gegevensset te definiëren. Als er te veel of te weinig rijen met waarden ontbrekende, kan de bewerking kan niet worden uitgevoerd. 
  
    Het getal dat u opgeeft vertegenwoordigt de **verhouding** van ontbrekende waarden aan alle waarden in de kolom. Standaard de **Minimum ontbrekende waarde verhouding** eigenschap is ingesteld op 0. Dit betekent dat de ontbrekende waarden worden opgeschoond, zelfs als er slechts één waarde ontbreekt. 

    > [!WARNING]
    > Deze voorwaarde moet worden voldaan door elke kolom in de volgorde voor de opgegeven bewerking om toe te passen. Bijvoorbeeld, wordt ervan uitgegaan dat u drie kolommen geselecteerd en stelt u de minimale verhouding van de ontbrekende waarden .2 of hoger (20%), maar slechts één kolom bevat de ontbrekende waarden 20%. In dit geval zou de bewerking opschonen gelden alleen voor de kolom met meer dan 20% van de ontbrekende waarden. Daarom zou de andere kolommen zijn niet gewijzigd.
    > 
    > Hebt u niet zeker weet of ontbrekende waarden zijn gewijzigd, selecteert u de optie **kolom van de indicator ontbrekende waarde genereren**. Een kolom wordt toegevoegd aan de gegevensset om aan te geven of elke kolom voldaan aan de opgegeven criteria voor de minimale en maximale bereiken.  
  
4. Voor **Maximum ontbrekende waarde verhouding**, geef het maximum aantal ontbrekende waarden die worden gebruikt voor de bewerking die kunnen moet worden uitgevoerd.   
  
    Bijvoorbeeld, kunt u uit te voeren van ontbrekende waarden te vervangen alleen als de 30% of minder rijen ontbreken waarden bevatten, maar laat u de waarden waar-is als meer dan 30% van de rijen ontbrekende waarden hebben.  
  
    U definieert het aantal als de verhouding van de ontbrekende waarden aan alle waarden in de kolom. Standaard de **Maximum ontbrekende waarde verhouding** is ingesteld op 1. Dit betekent dat de ontbrekende waarden worden opgeschoond, zelfs als 100% van de waarden in de kolom ontbreekt.  
  
   
  
5. Voor **reinigen modus**, selecteer een van de volgende opties voor het vervangen of verwijderen van de ontbrekende waarden:  
  
  
    + **Aangepaste vervangende waarde**: Gebruik deze optie om op te geven van een tijdelijke aanduiding invoegen (zoals een 0 of NA) die van toepassing op alle ontbrekende waarden. De waarde die u als vervanging opgeeft moet compatibel zijn met het gegevenstype van de kolom.
  
    + **Vervangen door de gemiddelde**: Berekent het gemiddelde van de kolom en het gemiddelde gebruikt als de waarde voor elke waarde ontbreekt in de kolom.  
  
        Geldt alleen voor kolommen met Integer, Double of Booleaanse gegevenstypen.  
  
    + **Vervangen door de mediaan**: Berekent de gemiddelde waarde van de kolom en de mediaanwaarde gebruikt als vervanging voor een ontbrekende waarde in de kolom.  
  
        Geldt alleen voor kolommen met gehele getallen of dubbele gegevenstypen. 
  
    + **Modus vervangen**: Berekent de modus voor de kolom en de modus gebruikt als de waarde voor elke waarde ontbreekt in de kolom.  
  
        Van toepassing op kolommen die Integer, Double, Booleaanse waarde of Categorisch gegevenstypen hebben. 
  
    + **Op Remove entire row**: Volledig verwijdert u een rij in de gegevensset die een of meer ontbrekende waarden. Dit is handig als de ontbrekende waarde kan worden beschouwd als willekeurig ontbreekt.  
  
    + **Verwijder de hele kolom**: Volledig verwijdert u een andere kolom in de gegevensset die een of meer ontbrekende waarden.  
  
    
  
6. De optie **vervangende waarde** is beschikbaar als u de optie hebt geselecteerd **aangepaste vervangende waarde**. Typ een nieuwe waarde kunt gebruiken als de waarde voor alle ontbrekende waarden in de kolom.  
  
    Houd er rekening mee dat u kunt deze optie alleen gebruiken in kolommen met de gegevenstypen Integer, Double, Booleaanse waarde of de datum. Voor de datumkolommen, de vervangende waarde kan ook worden ingevoerd als het aantal maatstreepjes van 100 nanoseconden sinds 1/1/0001 12:00 uur  
  
7. **Ontbrekende waarde indicatorkolom genereren**: Selecteer deze optie als u een indicatie wilt van of de waarden in de kolom voldaan aan de criteria voor de ontbrekende waarde reinigen uitvoeren. Deze optie is bijzonder nuttig wanneer u het instellen van een nieuwe reinigingsbewerking en controleren of dat deze werkt wilt zoals ontworpen.
  
8. Voer het experiment uit.

### <a name="results"></a>Resultaten

De module retourneert twee uitvoer:  

-   **Opgeschoonde gegevensset**: Als u die optie hebt geselecteerd, wordt een gegevensset bestaat uit de geselecteerde kolommen, met ontbrekende waarden als opgegeven samen met een indicatorkolom verwerkt.  

    Kolommen die niet is geselecteerd voor het opschonen worden ook 'doorgegeven via'.  
  
-  **Transformatie reinigen**: Een transformatie gebruikt voor het opschonen, die kan worden opgeslagen in uw werkruimte en kan worden toegepast op nieuwe gegevens later opnieuw.

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>Een opgeslagen reinigingsbewerking toepassen op nieuwe gegevens  

Als u reinigen operations vaak herhalen wilt, raden wij aan dat u uw recept opslaat voor het opschonen van gegevens als een *transformeren*, om te gebruiken met dezelfde gegevensset. Opslaan van een reinigingstape transformatie is vooral handig als u moet vaak opnieuw te importeren en vervolgens de gegevens die hetzelfde schema heeft opschonen.  
      
1.  Voeg de [transformatie toepassen](./apply-transformation.md) module naar het experimentcanvas.  
  
2.  De gegevensset die u wilt opschonen toe te voegen en de gegevensset verbinding met de rechter invoerpoort.  
  
3.  Vouw de **transformeert** groep in het linkerdeelvenster van de interface. Zoek de opgeslagen transformatie en sleep deze naar het experimentcanvas.  
  
4.  Verbinding maken met de opgeslagen transformatie aan de linkerinvoerpoort van [transformatie toepassen](./apply-transformation.md). 

    Wanneer u een opgeslagen transformatie toepast, kunt u de kolommen waarop de transformatie wordt toegepast niet selecteren. Dat komt doordat de transformatie is al gedefinieerd en wordt automatisch toegepast op de kolommen die zijn opgegeven in de oorspronkelijke bewerking.

    Stel echter dat u een transformatie op een subset van de numerieke kolommen hebt gemaakt. U kunt deze transformatie toepassen op een gegevensset van gemengde kolomtypen zonder een fout, omdat de ontbrekende waarden alleen in de overeenkomende kolommen die numerieke zijn gewijzigd.

6.  Voer het experiment uit.  

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 