---
title: Functies selecteren in het Team Data Science Process | Microsoft Docs
description: Het doel van functieselectie wordt beschreven en vindt u voorbeelden van hun rol in de uitbreiding van de synchronisatiegegevens van machine learning.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: b439f7245dd09a2f8a7ffe5f3b3c5396786220af
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442369"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Functies selecteren in Team Data Science Process (TDSP)
Dit artikel worden de doeleinden van functieselectie en bevat voorbeelden van de rol in de uitbreiding van de synchronisatiegegevens van machine learning. Deze voorbeelden worden van Azure Machine Learning Studio getekend. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Het technische team en de selectie van functies maakt deel uit van het Team Data Science Process (TDSP) die worden beschreven in het artikel [wat is Team Data Science Process?](overview.md). Feature-engineering en selectie maken deel uit van de **functies ontwikkelen** stap van de TDSP.

* **functie-engineering**: dit proces probeert te maken van aanvullende desbetreffende functies van de bestaande onbewerkte functies in de gegevens en voorspellende power om het learning-algoritme te vergroten.
* **functies selecteren**: dit proces selecteert u de belangrijke subset van functies van de oorspronkelijke gegevens in een poging om te beperken de dimensionaliteit van het probleem training.

Normaal gesproken **functie-engineering** eerst wordt toegepast voor het genereren van extra functies, en vervolgens de **functies selecteren** stap wordt uitgevoerd om te voorkomen, niet van belang, redundante of zeer gecorreleerde functies.

## <a name="filter-features-from-your-data---feature-selection"></a>Filteren van de functies van uw gegevens - Functieselectie
Functies selecteren is een proces dat vaak wordt toegepast voor de bouw van trainingdatasets voor voorspellende modelleertaken zoals classificatie- of regressiemodel taken. Het doel is om te selecteren van een subset van de functies van de oorspronkelijke gegevensset die de afmetingen beperken met behulp van een minimale set functies voor de maximale hoeveelheid afwijking in de gegevens. Deze subset van functies wordt gebruikt om het model te trainen. Functieselectie heeft twee hoofddoelen.

* Eerst Functieselectie vaak verhoogt de nauwkeurigheid van de classificatie door het elimineren van irrelevante, redundante of nauw verband houden functies.
* Ten tweede neemt het aantal functies, waardoor het proces van de training model efficiënter af. Efficiëntie is vooral belangrijk voor cursisten die duur zijn in trainen zoals support vector machines.

Hoewel Functieselectie gezocht tot Verminder het aantal functies in de gegevensset die wordt gebruikt voor het model te trainen, wordt dit niet aangeduid met de term 'dimensionaliteitsvermindering'. Functie selectiemethodes Pak een subset van de oorspronkelijke functies in de gegevens zonder dat ze worden gewijzigd.  Dimensionaliteit vermindering methoden maken gebruik van Social engineering functies die kunnen transformeren van de oorspronkelijke functies en ze zo te wijzigen. Voorbeelden van dimensionaliteit vermindering methoden zijn Principal onderdeel analyse, correlatieanalyse van de canonieke en enkelvoudige waarde ontleding.

Onder andere, wordt een breed toegepaste categorie selectiemethodes functie in een context die onder supervisie 'functies op basis van een filter selecteren' genoemd. Door het evalueren van de correlatie tussen elke functie en het doelkenmerk, gelden deze methoden een statistische meting een score toewijzen aan elke functie. De functies worden vervolgens door de score, die kan worden gebruikt om u te helpen bij het instellen van de drempelwaarde voor bewaren of verwijderen van een specifieke functie gerangschikt. Voorbeelden van de statistische metingen gebruikt in deze methoden zijn persoon correlatie, wederzijdse informatie en de test van de Chi-kwadraat.

In Azure Machine Learning Studio zijn er modules die zijn opgegeven voor de functies selecteren. Zoals u in de volgende afbeelding, deze modules bevatten [Functieselectie op basis van een Filter] [ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse] [ fisher-linear-discriminant-analysis].

![Voorbeeld van de functie selecteren](./media/select-features/feature-Selection.png)

Bijvoorbeeld, kunt u het gebruik van de [Functieselectie op basis van een Filter] [ filter-based-feature-selection] module. Doorgaan met behulp van het voorbeeld van de analysestructuur tekst voor het gemak. Wordt ervan uitgegaan dat u maken van een regressiemodel wilt nadat een set van 256-functies zijn gemaakt via de [hash-functies] [ feature-hashing] -module en dat de variabele antwoord is de 'Col1' book revisie classificaties met variërend van 1 tot 5. Door in te stellen 'Functie scoring methode' moet 'Pearson correlatie', de kolom' doel' moet 'Col1', en het 'aantal gewenste functies' tot 50. Vervolgens de module [Functieselectie op basis van een Filter] [ filter-based-feature-selection] produceert een gegevensset met 50 functies samen met het doelkenmerk 'Col1'. De volgende afbeelding ziet u de stroom van dit experiment en de invoerparameters:

![Voorbeeld van de functie selecteren](./media/select-features/feature-Selection1.png)

De volgende afbeelding ziet de resulterende gegevenssets:

![Voorbeeld van de functie selecteren](./media/select-features/feature-Selection2.png)

Elke functie wordt berekend op basis van de Pearson-correlatie tussen zichzelf en het doelkenmerk 'Col1'. De functies van de bovenste scores worden bewaard.

De bijbehorende scores van de geselecteerde onderdelen worden weergegeven in de volgende afbeelding:

![Voorbeeld van de functie selecteren](./media/select-features/feature-Selection3.png)

Door toe te passen dit [Functieselectie op basis van een Filter] [ filter-based-feature-selection] 50 van 256 functies worden geselecteerd omdat ze de meest gecorreleerde functies met de doelvariabele "Col1"-module op basis van de scoring-methode 'Pearson correlatie'.

## <a name="conclusion"></a>Conclusie
Feature-engineering en Functieselectie zijn twee meestal ontworpen en geselecteerde onderdelen verhoogt de efficiëntie van de trainingsproces die pogingen tot het ophalen van gegevens van de sleutel die is opgenomen in de gegevens. Ze verbeteren de kracht van deze modellen voor het classificeren van de ingevoerde gegevens correct en om te voorspellen van uitkomsten van belang zijn meer databases robuuster worden ook. Feature-engineering en selectie kunnen ook zodat de learning meer rekenintensief tractable combineren. Dit gebeurt verbeteren en vervolgens het aantal functies die nodig zijn om te kalibreren of een model te trainen te verminderen. Wiskundig spreken, zijn de functies die zijn geselecteerd voor het model te trainen een minimale set van onafhankelijke variabelen die uitgelegd van de patronen in de gegevens en vervolgens de resultaten met succes voorspellen.

Het is niet altijd noodzakelijk om uit te voeren Functieselectie engineering of functie. Of het nodig of niet is, is afhankelijk van de gegevens die zijn verzameld, de algoritme die is geselecteerd en het doel van het experiment.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

