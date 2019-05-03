---
title: 'Kolommen in gegevensset selecteren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de Select-Columns in Dataset module in Azure Machine Learning-service om te kiezen van een subset van kolommen die worden gebruikt in downstream-bewerkingen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: e7407f62bd3401411d56076b298bd8cd134ece62
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028093"
---
# <a name="select-columns-in-dataset-module"></a>Selecteer kolommen in gegevensset-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een subset van kolommen die worden gebruikt in downstream-bewerkingen. De module verwijderen de kolommen niet fysiek van de brongegevensset; in plaats daarvan wordt een subset van kolommen, net als een database gemaakt *weergave* of *projectie*.

Deze module is handig wanneer u nodig hebt om te beperken van de kolommen die beschikbaar zijn voor een downstream-bewerking, of als u wilt de grootte van de gegevensset beperken door het verwijderen van overbodige kolommen.

De kolommen in de gegevensset worden uitgevoerd in dezelfde volgorde als in de oorspronkelijke gegevens, zelfs als u ze in een andere volgorde opgeeft.

## <a name="how-to-use"></a>Gebruiksinstructies

Deze module heeft geen parameters. De kolomkiezer kunt u de kolommen wilt opnemen of uitsluiten kiezen.

### <a name="choose-columns-by-name"></a>Kolommen kiezen met de naam

Er zijn meerdere opties in de module voor het kiezen van kolommen met de naam: 

+ Filteren en zoeken

    Klik op de **op naam** optie.

    Als u een gegevensset die al is gevuld hebt verbonden, wordt een lijst met beschikbare kolommen moet worden weergegeven. Als er geen kolommen worden weergegeven, moet u mogelijk uitvoeren upstream modules om de lijst met kolommen weer te geven.

    Filter de lijst, typ in het zoekvak in. Bijvoorbeeld, als u de letter typt `w` in het zoekvak in de lijst is gefilterd om weer te geven van de namen van de kolommen die de letter bevatten `w`.

    Kolommen selecteren en klik op de knop pijl-rechts om de geselecteerde kolommen verplaatsen naar de lijst in het rechter deelvenster.

    + Als u wilt een continue reeks kolomnamen selecteren, drukt u op **Shift + klik**.
    + Afzonderlijke kolommen toevoegen aan de selectie, drukt u op **Ctrl + klikken**.

    Klik op de knop met het vinkje als u wilt opslaan en sluiten.

+ De namen gebruiken in combinatie met andere regels

    Klik op de **met regels** optie.
    
    Kies een regel, zoals het weergeven van de kolommen van een specifieke gegevens.

    Klik vervolgens op afzonderlijke kolommen van dat type met de naam, aan de selectielijst.

+ Typ of plak een door komma's gescheiden lijst met kolomnamen

    Als uw gegevensset breed is, is mogelijk eenvoudiger te gebruiken van indexen of afzonderlijk gegenereerd van lijsten met namen, in plaats van kolommen te selecteren. Ervan uitgaande dat u hebt de lijst van tevoren voorbereid:

    1. Klik op de **met regels** optie. 
    2. Selecteer **geen kolommen**, selecteer **opnemen**, en klik vervolgens in het tekstvak met het rode uitroepteken. 
    3. Plak of typ een door komma's gescheiden lijst met eerder gevalideerde kolomnamen. U kan niet opslaan in de module als elke kolom een ongeldige naam heeft, dus moet u om te controleren op de namen van de vooraf.
    
    U kunt deze methode ook gebruiken om op te geven van een lijst met kolommen met behulp van de indexwaarden. 

### <a name="choose-by-type"></a>Kies per type

Als u de **met regels** optie, kunt u meerdere voorwaarden toepassen op de kolomselecties die. Bijvoorbeeld, moet u mogelijk alleen functie kolommen van een numeriek gegevenstype worden opgehaald.

De **BEGIN WITH** optie bepaalt het beginpunt en is het belangrijk om te begrijpen van de resultaten. 

+ Als u selecteert de **alle kolommen** optie, alle kolommen worden toegevoegd aan de lijst. Vervolgens moet u de **uitsluiten** optie naar *verwijderen* kolommen die voldoen aan bepaalde voorwaarden. 

    U kunt bijvoorbeeld starten met alle kolommen en verwijder vervolgens de kolommen op naam of type.

+ Als u selecteert de **geen kolommen** optie, de lijst met kolommen aanvankelijk leeg. Vervolgens geeft u voorwaarden *toevoegen* kolommen aan de lijst. 

    Als u meerdere regels toepast, elke voorwaarde is **additieve**. Stel bijvoorbeeld dat u begint met geen kolommen en voeg een regel voor het ophalen van alle numerieke kolommen. In de prijs gegevensset voor auto's, waardoor de resultaten in 16 kolommen. Klik vervolgens u op de **+** Meld u aan een nieuwe voorwaarde toevoegen en selecteer **bevatten alle functies**. De resulterende gegevensset bevat de numerieke kolommen, plus alle functie kolommen, met inbegrip van sommige kolommen van de functie tekenreeks.

### <a name="choose-by-column-index"></a>Kies met de kolomindex

De kolomindex verwijst de volgorde van de kolom in de oorspronkelijke gegevensset.

+ Kolommen worden genummerd achter elkaar te beginnen bij 1.  
+ Als u een bereik van de kolommen, gebruikt u een afbreekstreepje. 
+ Open specificaties zoals `1-` of `-3` zijn niet toegestaan.
+ Dubbele index waarden (of kolomnamen) zijn niet toegestaan en kunnen leiden tot een fout.

Bijvoorbeeld, kan ervan uitgaande dat uw gegevensset ten minste acht kolommen bevat, plakken in een van de volgende voorbeelden om terug te keren meerdere niet-aaneengesloten kolommen: 

+ `8,1-4,6`
+ `1,3-8`
+ `1,3-6,4` 

het laatste voorbeeld resulteert niet in een fout. echter, wordt één exemplaar van de kolom `4`.



### <a name="change-order-of-columns"></a>Volgorde van kolommen wijzigen

De optie **dubbele vermeldingen toestaat en sla deze op volgorde van kolommen in de selectie** begint met een lege lijst en kolommen die u door de naam of index opgeeft toegevoegd. In tegenstelling tot andere opties, die kolommen in de 'natuurlijke volgorde"altijd worden geretourneerd, is deze optie levert de kolommen in de volgorde die u de naam, of ze. 

U kunt in een gegevensset met de kolommen Col1, Col2 Col3 en Kol4, bijvoorbeeld de volgorde van de kolommen en laat u uit kolom 2, door op te geven van de volgende lijsten:

+ `Col4, Col3, Col1`
+ `4,3,1`


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 