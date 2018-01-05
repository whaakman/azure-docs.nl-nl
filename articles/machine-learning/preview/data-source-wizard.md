---
title: Azure Data Source-Wizard voor Azure Machine Learning | Microsoft Docs
description: De wizard gegevensbron van AML workbench wordt uitgelegd
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: ff0159facd693b83230c731eb7e76f0a9495fdf2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="data-source-wizard"></a>Wizard gegevensbron #

De Wizard gegevensbron is een snelle en gemakkelijke manier om te zorgen dat een gegevensset in Azure ML-Workbench zonder code. Het is waar u ook een voorbeeld-strategie voor de gegevensset kunt selecteren. 

## <a name="step-1-trigger-the-data-source-wizard"></a>Stap 1: De Wizard gegevensbron activeren ## 

Om gegevens in een project met de wizard gegevensbron. Selecteer de  **+**  knop naast het zoekvak in de gegevensweergave en kies gegevensbron toevoegen. 

![Gegevensbron toevoegen](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>Stap 2: Selecteer waar gegevens worden opgeslagen ##
Eerst opgeven hoe uw gegevens in. Het kan worden opgeslagen in een plat bestand of een map, een parketvloeren-bestand, een Excel-bestand of een database. Zie [gegevensbronnen ondersteund](data-prep-appendix2-supported-data-sources.md) voor meer informatie.

![Stap 1](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>Stap 3: Selecteer een gegevensbestand ##
Geef het bestandspad voor een bestand of de map. Kies uit de vervolgkeuzelijst de locatie van de gegevens – wordt een lokaal pad of een Azure Blob Storage. 

Geef het pad door deze te typen of te klikken op de **bladeren...** om te bladeren. U kunt bladeren naar een map of een of meer bestanden.

Klik op **voltooien** accepteer de standaardwaarden voor de rest van de stappen of volgende om door te gaan met de volgende stap.


![Stap 4](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Stap 4: Kies bestandsparameters ##

Het bestand automatisch kan worden vastgesteld door de Wizard gegevensbron type, scheidingstekens en codering. Deze ook weergegeven een voorbeeld van hoe de gegevens eruit. U kunt ook een van deze parameters handmatig wijzigen. 

![Stap 5](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>Stap 5: Gegevenstypen voor kolommen instellen ##

De Wizard gegevensbron detecteert automatisch de gegevenstypen van de gegevensset kolommen. Als het een Cachemissers of als u wilt afdwingen van een gegevenstype, kunt u handmatig het gegevenstype wijzigen. De **uitvoer VOORBEELDGEGEVENS** kolom ziet u voorbeelden van hoe de gegevens eruit.

![Stap 6](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>Stap 6: Kies steekproeven strategie voor gegevens ##

U kunt opgeven van een of meer steekproeven strategieën voor de gegevensset en kies een als de actieve strategie. De standaardwaarde is de hoogste 10000 rijen laden. U kunt dit voorbeeld bewerken door te klikken op de **bewerken** knop op de werkbalk of Voeg een nieuwe strategie toe door te klikken op + Nieuw. De strategieën die wordt ondersteund worden

-     Bovenste aantal rijen
-     Willekeurig aantal rijen
-     Willekeurige percentage van de rijen
-     Volledige

U kunt zoveel steekproeven strategieën die u wilt, maar er slechts één kunnen worden ingesteld op actief is bij het voorbereiden van uw gegevens kunt opgeven. U kunt een strategie voor de actieve worden door het selecteren van de strategie en klik vervolgens op instellen als actief is op de werkbalk.

Afhankelijk van waar de gegevens vandaan, is het mogelijk dat een aantal strategieën voorbeeld niet wordt ondersteund. Voor meer informatie over steekproef nemen, de sectie steekproeven in [dit document](data-prep-user-guide.md) 

![Stap 6](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>Stap 7: Pad kolom verwerking ##

Als het bestandspad belangrijke gegevens bevat, kunt u deze als de eerste kolom in de gegevensset opnemen. Dit is handig als u in meerdere bestanden meebrengen. U kunt anders niet is opgenomen.

![Stap 7](media/data-source-wizard/step6.png)

Nadat u op Voltooien klikt, wordt een nieuwe gegevensbron worden toegevoegd aan het project. U vindt deze onder de groep gegevensbronnen in de weergave of als een bestand .dsource in de **bestandsweergave**.
