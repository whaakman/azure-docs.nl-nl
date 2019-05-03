---
title: 'Gegevens handmatig invoeren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module gegevens handmatig invoeren in Azure Machine Learning-service te maken van een kleine gegevensset door waarden te typen. De gegevensset kan meerdere kolommen hebben.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ee15b6fb7160ece907d55e790b0ae38ee458ab96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028603"
---
# <a name="enter-data-manually-module"></a>Voer de gegevens handmatig module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een kleine gegevensset maken door waarden te typen. De gegevensset kan meerdere kolommen hebben.
  
Deze module kan nuttig zijn bij dergelijke scenario's:  
  
- Genereren van een kleine set waarden voor het testen  
  
- Het maken van een korte lijst met labels
  
- Een lijst met kolomnamen om in te voegen in een gegevensset te typen

## <a name="enter-data-manually"></a>Gegevens handmatig invoeren 
  
1.  Voeg de [gegevens handmatig invoeren](./enter-data-manually.md) module naar het experimentcanvas. U vindt deze module in de **gegevensinvoer en uitvoer** categorie in Azure Machine Learning. 
  
2.  Voor **DataFormat**, selecteer een van de volgende opties. Deze opties te bepalen hoe de gegevens die u opgeeft moet worden geanalyseerd. De vereisten voor elke indeling aanzienlijk verschillen, dus zorg ervoor dat de verwante onderwerpen lezen.  
  
    -   **ARFF**. De kenmerkrelatie bestandsindeling, die wordt gebruikt door Weka.   
  
    -   **CSV**. Indeling met door komma's gescheiden waarden. Zie voor meer informatie, [converteren naar CSV](./convert-to-csv.md).  
  
    -   **SVMLight**. Een indeling die wordt gebruikt door Vowpal Wabbit en andere machine learning-frameworks.  
  
    -   **TSV**. Indeling met door tabs gescheiden waarden.

     Als u een indeling kiezen en biedt geen gegevens die aan de indeling-specificaties voldoet, wordt een runtime-fout optreedt.
  
3.  Klik in de **gegevens** in het tekstvak om te beginnen met het invoeren van gegevens. De volgende indelingen vereisen speciale aandacht:  
  
    - **CSV**:  Voor het maken van meerdere kolommen in een door komma's gescheiden tekst plakken of meerdere kolommen met komma's tussen de velden van het type.
  
        Als u selecteert de **HasHeader** optie, kunt u de eerste rij met waarden als de kolomkop te klikken.  
  
        Als u deze optie, de namen van kolommen, Col1, Col2, enzovoort, worden gebruikt. U kunt toevoegen of wijzigen van kolommen met behulp van de naam [metagegevens bewerken](./edit-metadata.md).  
  
    - **TSV**: Voor het maken van meerdere kolommen in door tabs gescheiden tekst plakken of meerdere kolommen met de tabbladen tussen de velden van het type.  
  
        Als u selecteert de **HasHeader** optie, kunt u de eerste rij met waarden als de kolomkop te klikken.  
  
        Als u deze optie, de namen van kolommen, Col1, Col2, enzovoort, worden gebruikt. U kunt toevoegen of wijzigen van kolommen met behulp van de naam [metagegevens bewerken](./edit-metadata.md).  
  
    -   **ARFF**:  In een bestaand ARFF format-bestand te plakken. Als u waarden rechtstreeks typt, moet u de optionele header en het vereiste kenmerkvelden toevoegen aan het begin van de gegevens. 
    
        De volgende rijen van de kop- en -kenmerk kunnen bijvoorbeeld worden toegevoegd aan een eenvoudige lijst. De kolomkop zou worden `SampleText`.
    
        ```text
        % Title: SampleText.ARFF  
        % Source: Enter Data module  
        @ATTRIBUTE SampleText STRING  
        @DATA  
        \<type first data row here>  
        ```

    -   **SVMLight**: Typ of plak in waarden met behulp van de SVMLight-indeling.  
  
        Het volgende voorbeeld vertegenwoordigt bijvoorbeeld de eerste paar regels van de gegevensset bloeddonatie SVMight indeling:  
  
        ```text  
        # features are [Recency], [Frequency], [Monetary], [Time]  
        1 1:2 2:50 3:12500 4:98   
        1 1:0 2:13 3:3250 4:28   
        ```  
  
        Bij het uitvoeren van de [gegevens handmatig invoeren](./enter-data-manually.md) -module, deze regels worden geconverteerd naar een gegevensset van kolommen en index waarden als volgt te werk:  
  
        |Col1|Col2|Col3|Col4|Labels|  
        |-|-|-|-|-|  
        |0.00016|0.004|0.999961|0.00784|1|  
        |0|0.004|0.999955|0.008615|1|  
  
4.  Druk op ENTER na elke rij, om een nieuwe regel te beginnen.  
  
     **Zorg ervoor dat druk op ENTER na de laatste rij.** 
     
     Als u op ENTER drukt is meerdere keren om toe te voegen meerdere lege rijen afsluitende, is de laatste rij leeg bijgesneden is verwijderd, maar andere lege rijen worden behandeld als de ontbrekende waarden.  
  
     Als u rijen met ontbrekende waarden maakt, kunt u altijd filteren ze later opnieuw.  
  
5.  Met de rechtermuisknop op de module en selecteer **geselecteerde uitvoeren** voor het parseren van de gegevens en deze te laden in uw werkruimte als een gegevensset.  
  
     Als u de gegevensset, klikt u op de uitvoerpoort en selecteer **Visualize**.  
## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 