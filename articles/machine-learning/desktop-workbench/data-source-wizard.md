---
title: Azure-gegevensbron van de Wizard voor Azure Machine Learning | Microsoft Docs
description: De wizard gegevensbron van AML-workbench wordt uitgelegd
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: c74229504a43179673cc99ccff321b65e3f6ed4f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646366"
---
# <a name="data-source-wizard"></a>Wizard gegevensbron #

De Wizard gegevensbron is een snelle en eenvoudige manier om een gegevensset in Azure ML Workbench zonder code. Het is waar u ook een voorbeeld-strategie voor de gegevensset en gegevenstypen voor elke kolom kunt selecteren. 

## <a name="step-1-trigger-the-data-source-wizard"></a>Stap 1: De Wizard gegevensbron activeren ## 

Om gegevens in een project met behulp van de wizard gegevensbron. Selecteer de **+** knop naast het zoekvak in de gegevensweergave en kiest u gegevensbron toevoegen. 

![Gegevensbron toevoegen](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>Stap 2: Selecteer waar gegevens worden opgeslagen ##
Eerst, kunt u opgeven hoe uw gegevens is momenteel in. Deze kan worden opgeslagen in een plat bestand of een map, een parquet-bestand, een Excel-bestand of een database. Zie voor meer informatie, [gegevensbronnen ondersteund](data-prep-appendix2-supported-data-sources.md).

![Stap 1](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>Stap 3: Selecteer een gegevensbestand ##
Geef het pad voor een bestand/map. Kies in de vervolgkeuzelijst de locatie van de gegevens, wordt een lokaal bestandspad of de Azure Blob Storage. 

Het pad opgeven door deze in te typen of te klikken op de **bladeren...** om te bladeren. U kunt zoeken naar een map of een of meer bestanden.

Klik op **voltooien** accepteer de standaardwaarden voor de rest van de stappen of volgende om door te gaan met de volgende stap.


![Stap 4](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Stap 4: Bestandsparameters kiezen ##

Het bestand automatisch kan worden vastgesteld door de Wizard gegevensbron type, scheidingstekens en codering. Het zou ook een voorbeeld van hoe de gegevens eruit weergeven. U kunt ook een van deze parameters handmatig wijzigen. 

![Stap 5](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>Stap 5: Set-gegevenstypen voor kolommen ##

De Wizard gegevensbron detecteert automatisch de gegevenstypen van de kolommen van de gegevensset. Als er een missers, of u wilt afdwingen van een gegevenstype, kunt u handmatig het gegevenstype wijzigen. De **VOORBEELDGEGEVENS van uitvoer** kolom ziet u voorbeelden van hoe de gegevens eruit zien.

Voor een kolom die gegevensvoorbereiding afleidt datums bevat, u mogelijk gevraagd om te selecteren van de volgorde van de maand en dag in de notatie voor datum. Bijvoorbeeld 1/2/2013 kan vertegenwoordigen januari 2e (Selecteer voor dit *dag-maand*) of Feburary 1e (Selecteer *maand-dag*).

![Stap 6](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>Stap 6: Kies samplingstrategie voor gegevens ##

U kunt een of meer steekproeven strategieën voor de gegevensset opgeven, en kies een als de actieve strategie. De standaardwaarde is de Top 10000 rijen te laden. U kunt dit voorbeeld bewerken door te klikken op de **bewerken** knop op de werkbalk of een nieuwe strategie toevoegen door te klikken op + Nieuw. De strategieën die momenteel worden ondersteund zijn

-     Hoogste aantal rijen
-     Willekeurig aantal rijen
-     Willekeurige percentage van de rijen
-     Volledig bestand

U kunt zoveel steekproeven strategieën die u wilt, maar er slechts één die kan worden ingesteld op actief is tijdens het voorbereiden van uw gegevens kunt opgeven. U kunt een strategie voor de actieve worden door het selecteren van de strategie en klikt u op instellen als actief op de werkbalk.

Afhankelijk van waar de gegevens opgehaald, is het mogelijk dat sommige strategieën voorbeeld niet wordt ondersteund. Voor meer informatie over steekproeven kijken naar de sectie steekproeven in [dit document](data-prep-user-guide.md) 

![Stap 6](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>Stap 7: Pad kolom verwerken ##

Als het bestandspad belangrijke gegevens bevat, kunt u deze als de eerste kolom in de gegevensset is opgenomen. Deze optie is handig als u meerdere bestanden. U kunt anders niet is opgenomen.

![Stap 7](media/data-source-wizard/step6.png)

Nadat u op Voltooien klikt, wordt een nieuwe gegevensbron worden toegevoegd aan het project. U vindt deze onder de groep met gegevensbronnen in de gegevensweergave, of als een bestand dsource in de **bestandsweergave**.
