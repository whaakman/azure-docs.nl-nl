---
title: 'Metagegevens bewerken: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module metagegevens bewerken in Azure Machine Learning-service om te wijzigen van metagegevens die zijn gekoppeld aan de kolommen in een gegevensset.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: cfee607aca155b6cf68e5bddc40eb9c752df5e34
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028813"
---
# <a name="edit-metadata-module"></a>De metagegevens van module bewerken

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module metagegevens die zijn gekoppeld aan de kolommen in een gegevensset wijzigen. De waarde en het gegevenstype van de gegevensset wordt gewijzigd nadat u de **metagegevens bewerken** module. 
 
Wijzigingen in typische metagegevens zijn onder andere:
  
+ Booleaanse of numerieke kolommen te behandelen als categorische waarden  
  
+ Die aangeeft welke kolom bevat de *klasse* label of de waarden die u wilt categoriseren of voorspellen  
  
+ Kolommen markeren als functies
  
+ Wijzigen van datum/tijd-waarden naar een numerieke waarde of vice versa  
  
+ Naam van kolommen wijzigen
  
 Gebruik [bewerken metagegevens telkens wanneer u de definitie van een kolom, doorgaans wijzigen om te voldoen aan vereisten voor een downstream-module. Bijvoorbeeld, sommige modules kunnen werken alleen met specifieke gegevenstypen, of vereisen vlaggen op kolommen zoals `IsFeature` of `IsCategorical`.  
  
 Na het uitvoeren van de vereiste bewerking, kunt u de metagegevens in de oorspronkelijke staat herstellen. 
  
## <a name="configure-edit-metadata"></a>Configureren van metagegevens bewerken
  
1.  Toevoegen in Azure Machine Learning, [metagegevens bewerken](./edit-metadata.md) module naar het experimentcanvas en koppel de gegevensset die u wilt bijwerken. U vindt deze onder **gegevenstransformatie**, in de **bewerken** categorie.
  
2.  Klik op **Start de kolomkiezer** en kiest u de kolom of een set kolommen om te werken met. U kunt kolommen afzonderlijk op naam of index, of u kunt een groep van de kolommen op type.  
  
3.  Selecteer de **gegevenstype** optie als u moet een ander gegevenstype toewijzen aan de geselecteerde kolommen. Het wijzigen van de gegevens type nodig zijn voor bepaalde bewerkingen: bijvoorbeeld, als uw brongegevensset getallen verwerkt als tekst heeft, moet u ze op een numeriek gegevenstype voordat u wiskundige bewerkingen. 

    + De ondersteunde gegevenstypen zijn `String`, `Integer`, `Double`, `Boolean`, `DateTime`. 

    + Als er meerdere kolommen zijn geselecteerd, moet u de wijzigingen in metagegevens naar toepassen **alle** geselecteerde kolommen. Stel dat u de numerieke kolommen 2-3 kiezen. U kunt wijzigen naar gegevenstype string en naam wijzigen in één bewerking. U kan echter één kolom in een tekenreeksgegevenstype en een andere kolom wijzigen van een float-waarde in een geheel getal zijn.
  
    + Als u een nieuw gegevenstype niet opgeeft, wordt de kolommetagegevens van de ongewijzigd. 
    
    + De kolom van het type en de waarden worden gewijzigd nadat voeren de [metagegevens bewerken](./edit-metadata.md) bewerking. U kunt het oorspronkelijke gegevenstype op elk gewenst moment herstellen met behulp van [metagegevens bewerken](./edit-metadata.md) opnieuw instellen van het gegevenstype van kolom.  

    > [!NOTE]
    > Als u elk type getal en wijzigt de **datum-/** typt, laat u de **datum-/ tijdindeling** veld leeg. Op dit moment is het niet mogelijk om op te geven van de doel-gegevensindeling.  

      
4.  Selecteer de **Categorisch** optie om op te geven dat de waarden in de geselecteerde kolommen moeten worden behandeld als categorieën. 

    U kan bijvoorbeeld een kolom met de getallen 0,1 en 2, maar weet dat de getallen daadwerkelijk 'Roker", 'Niet-roker' en 'Onbekend betekent'. In dat geval door het markeren van de kolom als categorische kunt u ervoor zorgen dat de waarden niet worden gebruikt in berekeningen, alleen om gegevens te groeperen. 
  
5.  Gebruik de **velden** optie als u wilt wijzigen van de manier waarop Azure Machine Learning gebruikt de gegevens in een model.

    + **Functie**: Gebruik deze optie om een kolom markeren als een functie, voor gebruik met modules die worden uitgevoerd alleen op functie-kolommen. Standaard worden alle kolommen in eerste instantie beschouwd als functies.  
  
    + **Label**: Gebruik deze optie om het label (ook wel bekend als het voorspelbare kenmerk of doelvariabele) te markeren. Veel modules vereist ten minste één (en slechts één) labelkolom aanwezig zijn in de gegevensset. 
    
        In veel gevallen kan het afleiden van Azure Machine Learning dat een kolom een klasse-label bevat, maar door in te stellen deze metagegevens u ervoor zorgen kunt dat de kolom correct wordt geïdentificeerd. Deze optie wordt niet gewijzigd gegevenswaarden, alleen de manier waarop de gegevens worden verwerkt door enkele machine learning-algoritmen.
  

  
    > [!TIP]
    >  Gegevens die niet in de volgende categorieën passen bevatten?  Uw gegevensset kan bijvoorbeeld waarden zoals unieke id's die niet nodig als variabelen bevatten. Id's kunnen soms problemen bij gebruik in een model veroorzaken. 
    >   
    >  Gelukkig 'op de achtergrond' houdt Azure Machine Learning u al uw gegevens, zodat u niet hoeft te verwijderen van dergelijke kolommen uit de gegevensset. Als u uitvoeren van bewerkingen op een speciale set kolommen wilt, alleen alle andere kolommen tijdelijk verwijderen met behulp van de [Select Columns in Dataset](./select-columns-in-dataset.md) module. Later kunt u de kolommen in de gegevensset samenvoegen met behulp van de [kolommen toevoegen](./add-columns.md) module.  
  
6. Gebruik de volgende opties om te wissen van vorige selecties en metagegevens op de standaardwaarden herstellen.  
  
    + **Schakel functie**: Gebruik deze optie om de functievlag te verwijderen.  
  
         Omdat alle kolommen in eerste instantie worden behandeld als functies voor modules die wiskundige bewerkingen uitvoeren, moet u mogelijk op deze optie gebruiken om te voorkomen dat numerieke kolommen wordt behandeld als variabelen.
  
    + **Duidelijke label**: Gebruik deze optie om te verwijderen de **label** metagegevens van de opgegeven kolom.  
  
    + **Schakel score**: Gebruik deze optie om te verwijderen de **score** metagegevens van de opgegeven kolom.  
  
         De mogelijkheid om een kolom als een score expliciet markeren is momenteel niet beschikbaar in Azure Machine Learning. Bepaalde bewerkingen wordt echter resulteren in een kolom wordt gemarkeerd als een score intern. Een aangepaste R-module kan ook, score waarden uitvoer.
  
  
7.  Voor **nieuwe kolomnamen**, typt u de nieuwe naam van de geselecteerde kolom of kolommen.  
  
    + De namen van kolommen kunt alleen de tekens die worden ondersteund door de UTF-8-codering. Lege tekenreeksen, null-waarden of namen die alleen uit spaties zijn niet toegestaan.  
  
    + Wijzig de naam van meerdere kolommen, typt u de namen in als een door komma's gescheiden lijst in volgorde van de kolomindexen.  
  
    + De naam van alle geselecteerde kolommen moeten worden gewijzigd. U kunt geen weglaat of kolommen overslaan.  
  
  
8.  Voer het experiment uit.  

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 