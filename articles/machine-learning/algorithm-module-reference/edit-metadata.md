---
title: 'Metagegevens bewerken: Modulereferentie'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module metagegevens bewerken in de Azure Machine Learning-service om te wijzigen van metagegevens die gepaard gaat met de kolommen in een gegevensset.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 282652adb917450c262e08bf10c3c6e537b829e7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072706"
---
# <a name="edit-metadata-module"></a>De metagegevens van module bewerken

Dit artikel beschrijft een module van de visuele interface (preview) voor de Azure Machine Learning-service.

Gebruik de module gegevens bewerken om te wijzigen van de metagegevens die gepaard gaat met de kolommen in een gegevensset. De waarde en het gegevenstype van de gegevensset wordt gewijzigd na het gebruik van de module metagegevens bewerken.

Wijzigingen in typische metagegevens zijn onder andere:
  
+ Booleaanse of numerieke kolommen als categorische waarden te behandelen.
  
+ Die aangeeft welke kolom bevat de **klasse** label of de waarden die u wilt categoriseren of voorspellen bevat.
  
+ Kolommen markeren als functies.
  
+ Het wijzigen van datum/tijd-waarden naar numerieke waarden of vice versa.
  
+ Wijzigen van kolommen.
  
 Telkens wanneer u de definitie van een kolom, doorgaans wijzigen om te voldoen aan vereisten voor een downstream-module met behulp van metagegevens bewerken. Bijvoorbeeld, sommige modules werken alleen met specifieke gegevenstypen of vlaggen voor de kolommen vereisen zoals `IsFeature` of `IsCategorical`.  
  
 Nadat u de vereiste bewerking niet uitvoeren, kunt u de metagegevens in de oorspronkelijke staat herstellen.
  
## <a name="configure-edit-metadata"></a>Configureren van metagegevens bewerken
  
1. De metagegevens bewerken-module toevoegen aan uw experiment in Azure Machine Learning, en verbinding maken met de gegevensset die u wilt bijwerken. U vindt de gegevensset onder **gegevenstransformatie** in de **bewerken** categorie.
  
1. Selecteer **Start de kolomkiezer** en kiest u de kolom of een set kolommen om te werken met. U kunt kolommen afzonderlijk op naam of index of kunt u een groep met kolommen op type.  
  
1. Selecteer de **gegevenstype** optie als u moet een ander gegevenstype toewijzen aan de geselecteerde kolommen. Mogelijk moet u het gegevenstype voor bepaalde bewerkingen wijzigen. Bijvoorbeeld, als uw brongegevensset getallen verwerkt als tekst heeft, moet u ze op een numeriek gegevenstype voordat u wiskundige bewerkingen.

    + De ondersteunde gegevenstypen zijn **tekenreeks**, **geheel getal**, **dubbele**, **Booleaanse**, en **datum-/** .

    + Als u meerdere kolommen selecteert, moet u de wijzigingen in metagegevens naar toepassen *alle* geselecteerde kolommen. Stel dat u twee of drie numerieke kolommen kiezen. U kunt ze wijzigen naar een tekenreeks gegevenstype en de naam wijzigen in één bewerking. U kan echter één kolom in een tekenreeksgegevenstype en een andere kolom wijzigen van een float-waarde in een geheel getal zijn.
  
    + Als u een nieuw gegevenstype niet opgeeft, wordt de kolommetagegevens van de ongewijzigd.

    + De kolom van het type en de waarden wordt gewijzigd nadat u de metagegevens bewerken-bewerking niet uitvoeren. U kunt het oorspronkelijke gegevenstype op elk gewenst moment herstellen met behulp van metagegevens bewerken opnieuw instellen van het gegevenstype van kolom.  

    > [!NOTE]
    > Als u elk type getal en wijzigt de **datum-/** typt, laat u de **datum-/ tijdindeling** veld leeg. Op dit moment is het niet mogelijk om op te geven van de doel-gegevensindeling.  

1. Selecteer de **Categorisch** optie om op te geven dat de waarden in de geselecteerde kolommen moeten worden behandeld als categorieën.

    Bijvoorbeeld, u mogelijk hebt u een kolom met de getallen van 0, 1 en 2, maar weet dat de getallen daadwerkelijk betekent "Roker," 'Niet-roker' en 'Onbekend'. In dat geval door de kolom als categorische markeert u ervoor zorgen dat de waarden worden gebruikt alleen om gegevens te groeperen en niet in de berekeningen.
  
1. Gebruik de **velden** optie als u wilt wijzigen van de manier waarop Azure Machine Learning gebruikt de gegevens in een model.

    + **Functie**: Gebruik deze optie om een kolom markeren als een functie in modules die worden uitgevoerd alleen op functie-kolommen. Standaard worden alle kolommen in eerste instantie beschouwd als functies.  
  
    + **Label**: Gebruik deze optie om het markeren van het label, ook wel bekend als het voorspelbare kenmerk of target-variabele wordt. Veel modules vereist dat die precies één labelkolom is aanwezig in de gegevensset.

        In veel gevallen afleiden Azure Machine Learning dat een kolom een klasse-label bevat. Door in te stellen deze metagegevens, kunt u ervoor zorgen dat de kolom correct wordt geïdentificeerd. Deze optie instelt, wordt gegevenswaarden niet gewijzigd. Alleen de manier waarop dat sommige machine learning-algoritmen verwerking van de gegevens verandert.
  
    > [!TIP]
    > Hebt u gegevens die niet in deze categorieën past? Uw gegevensset kan bijvoorbeeld waarden zoals unieke id's die niet handig als variabelen zijn bevatten. Deze id's kunnen soms problemen bij gebruik in een model veroorzaken.
    >
    > Azure Machine Learning kunt u gelukkig houdt op al uw gegevens, zodat u hoeft deze kolommen verwijderen uit de gegevensset. Als u uitvoeren van bewerkingen op een speciale set kolommen wilt, alleen alle andere kolommen tijdelijk verwijderen met behulp van de [Select Columns in Dataset](select-columns-in-dataset.md) module. Later kunt u de kolommen in de gegevensset samenvoegen met behulp van de [kolommen toevoegen](add-columns.md) module.  
  
1. Gebruik de volgende opties om te wissen van vorige selecties en metagegevens op de standaardwaarden herstellen.  
  
    + **Schakel functie**: Gebruik deze optie om de functievlag te verwijderen.  
  
         Alle kolommen worden in eerste instantie behandeld als functies. Voor modules die wiskundige bewerkingen uitvoeren, moet u mogelijk gebruik van deze optie om te voorkomen dat numerieke kolommen wordt behandeld als variabelen.
  
    + **Duidelijke label**: Gebruik deze optie om te verwijderen de **label** metagegevens van de opgegeven kolom.  
  
    + **Schakel score**: Gebruik deze optie om te verwijderen de **score** metagegevens van de opgegeven kolom.  
  
         U markeren momenteel niet expliciet een kolom als een score in Azure Machine Learning. Bepaalde bewerkingen wordt echter resulteren in een kolom wordt gemarkeerd als een score intern. Een aangepaste R-module kan ook, score waarden uitvoer.

1. Voor **nieuwe kolomnamen**, typ de nieuwe naam van de geselecteerde kolom of kolommen.  
  
    + De namen van kolommen kunt alleen de tekens die worden ondersteund door UTF-8-codering. Lege tekenreeksen, null-waarden of namen die alleen uit spaties bestaan zijn niet toegestaan.  
  
    + Wijzig de naam van meerdere kolommen, geef de naam als een door komma's gescheiden lijst in volgorde van de indexen die kolom.  
  
    + De naam van alle geselecteerde kolommen moeten worden gewijzigd. U kunt geen weglaat of kolommen overslaan.  
  
1. Voer het experiment uit.  

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) naar de Azure Machine Learning-service.
