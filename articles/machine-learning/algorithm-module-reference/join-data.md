---
title: 'Gegevens samenvoegen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de join-module van Join-gegevens in Azure Machine Learning-service om gegevenssets samen te voegen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7e814f5ea4bd47ceb0697e860c946039ce39ae1f
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518012"
---
# <a name="join-data"></a>Gegevens samenvoegen

In dit artikel wordt beschreven hoe u de **Join gegevens** module in de visuele interface van Azure Machine Learning-service om twee gegevenssets met behulp van een database-stijl join-bewerking samen te voegen.  

## <a name="how-to-configure-join-data"></a>Deelnemen aan gegevens configureren

Als u wilt uitvoeren van een join op twee gegevenssets, moeten ze zijn gerelateerd door een sleutelkolom. Samengestelde sleutels met behulp van meerdere kolommen worden ook ondersteund. 

1. Toevoegen van de gegevenssets die u wilt combineren en sleep de **deelnemen aan gegevens** module in uw experiment. 

    U vindt de module in de **gegevenstransformatie** categorie onder **manipulatie**.

1. Verbinding maken met de gegevenssets kunnen de **deelnemen aan gegevens** module. 
 
1. Selecteer **kolomkiezer lancering** -sleutelkolommen kiezen. Vergeet niet om de kolommen voor de linker- en invoer kiezen.

    Voor één sleutel:

    Selecteer een één sleutelkolom voor zowel invoer.
    
    Voor een samengestelde sleutel:

    Selecteer alle de sleutelkolommen van linkerinvoer en rechterinvoer in dezelfde volgorde. De **Join gegevens** module worden de tabellen samengevoegd wanneer alle sleutelkolommen overeenkomen. Schakel het selectievakje **dubbele vermeldingen toestaat en sla deze op volgorde van kolommen in de selectie** als de volgorde van kolommen is niet hetzelfde als de oorspronkelijke tabel. 

    ![column-selector](media/module/join-data-column-selector.png)


1. Selecteer de **identieke hoofdletters /** optie als u wilt behouden hoofdlettergevoeligheid op een kolom tekst join. 
   
1. Gebruik de **koppelingstype** vervolgkeuzelijst om op te geven hoe de gegevenssets moeten worden gecombineerd.  
  
    * **Inner Join**: Een *inner join* is de meest voorkomende joinbewerking. Deze retourneert de gecombineerde rijen alleen wanneer de waarden van de sleutelkolommen overeenkomen.  
  
    * **Left Outer Join**: Een *linker outer join* retourneert alle rijen uit de linkertabel rijen toegevoegd. Wanneer een rij in de linkertabel geen overeenkomende rijen in de rechtertabel heeft, bevat de geretourneerde rij ontbreken waarden voor alle kolommen die afkomstig uit de rechtertabel zijn. U kunt ook een vervangende waarde voor de ontbrekende waarden opgeven.  
  
    * **Volledige Outer Join**: Een *volledige outer join* retourneert alle rijen uit de linkertabel (**Tabel1**) en uit de rechtertabel (**table2**).  
  
         Het resultaat bevat voor elk van de rijen in een tabel waarvoor geen overeenkomende rijen in de andere is een rij met ontbrekende waarden.  
  
    * **Links puntkomma Join**: Een *left join semi* retourneert alleen de waarden uit de linkertabel wanneer de waarden van de sleutelkolommen overeenkomen.  

1. Voor de optie **rechts sleutelkolommen in gekoppelde tabel houden**:

    * Selecteer deze optie om de sleutels van beide invoertabellen weer te geven.
    * Hef de selectie alleen terugkeren de sleutelkolommen van de linkerinvoer.

1. Voer het experiment uit of selecteert u de gegevens van de Join-module en geselecteerd **geselecteerde uitvoeren** om uit te voeren van de join.

1. U kunt de resultaten, met de rechtermuisknop op de **deelnemen aan gegevens** > **resultaten gegevensset** > **Visualize**.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 