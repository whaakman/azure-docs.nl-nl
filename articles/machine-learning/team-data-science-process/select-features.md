---
title: Functie selectie in de procedure voor wetenschappelijke gegevens Team | Microsoft Docs
description: Wordt het doel van functieselectie en vindt u voorbeelden van hun rol in de procedure voor de verbetering van gegevens van machine learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 878541f5-1df8-4368-889a-ced6852aba47
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: c69e7a7e86ccea2073209eabe123049e59a2b8e3
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Functies selecteren in Team Data Science Process (TDSP)
In dit artikel worden de doeleinden van functieselectie en voorbeelden gegeven van de rol in de procedure voor de verbetering van gegevens van machine learning. Deze voorbeelden zijn afkomstig van Azure Machine Learning Studio. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

De engineering en de selectie van functies maakt deel uit van het Team gegevens wetenschap proces (TDSP) die worden beschreven in het artikel [wat is het proces Team gegevens wetenschappelijke?](overview.md). Functie-engineering en een selectie maken deel uit van de **functies ontwikkelen** stap van de TDSP.

* **functie-engineering**: dit proces probeert extra relevante functies maken op basis van de bestaande onbewerkte functies in de gegevens, en predictive vermogen de learning-algoritme te vergroten.
* **functie selectie**: dit proces selecteert u de belangrijkste subset van de oorspronkelijke functies van de gegevens in een poging de dimensionaliteit van het probleem training verminderen.

Normaal gesproken **functie-engineering** als eerste toegepast voor het genereren van aanvullende functies, en vervolgens de **functie selectie** stap wordt uitgevoerd om de functies niet relevant, redundante of maximaal gecorreleerde elimineren.

## <a name="filter-features-from-your-data---feature-selection"></a>Functies van uw gegevens - Functieselectie filteren
Functieselectie is een proces dat meestal wordt toegepast voor de constructie van training gegevenssets voor taken zoals classificatie- of regressiemodel taken voorspellende modellen. Het doel is het selecteren van een subset van de functies van de oorspronkelijke gegevensset aan die de afmetingen verminderen met behulp van een minimale set van functies die de maximale hoeveelheid in de gegevens van de variantie vertegenwoordigt. Deze subset van functies wordt gebruikt voor het model trainen. Functieselectie fungeert twee hoofddoelen.

* Eerst Functieselectie vaak verhoogt de nauwkeurigheid van de classificatie doordat irrelevante, redundante of maximaal gecorreleerde functies.
* Ten tweede neemt het aantal functies, waardoor het model trainingsproces efficiënter af. Efficiëntie is vooral belangrijk voor overbrengen die dure te trainen zoals ondersteuning vector machines.

Hoewel Functieselectie zoeken naar Verminder het aantal onderdelen in de gegevensset die wordt gebruikt voor het model trainen, wordt deze niet verwezen door de term 'dimensionaliteit vermindering'. Functie selectiemethodes extraheren een subset van de oorspronkelijke functies in de gegevens niet wijzigen.  Dimensionaliteit vermindering methoden gebruiken engineering functies die u kunnen de oorspronkelijke functies transformeren en ze zo te wijzigen. Voorbeelden van dimensionaliteit vermindering methoden zijn Principal onderdeel analyse, canonieke correlatieanalyse en enkelvoud waarde afbreken.

Onder andere wordt één breed worden toegepast categorie van functie selectiemethoden in een context die onder supervisie 'filter gebaseerde Functieselectie' genoemd. Deze methoden toepassen voor het evalueren van de correlatie tussen elke functie en het kenmerk target een statistische meting om een score toewijzen aan elke functie. De functies zijn vervolgens door de score moet kunnen worden gebruikt om u te helpen bij het instellen van de drempelwaarde voor bewaren of verwijderen van een specifieke functie gerangschikt. Voorbeelden van de statistische metingen gebruikt in deze methoden zijn persoon correlatie, wederzijdse informatie en de test van de Chi-kwadraat.

In Azure Machine Learning Studio zijn er modules die zijn opgegeven voor de Functieselectie van de. Zoals u in de volgende afbeelding, deze modules bevatten [Functieselectie op basis van het Filter] [ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse][fisher-linear-discriminant-analysis].

![Voorbeeld van de functie-selectie](./media/select-features/feature-Selection.png)

Overweeg bijvoorbeeld het gebruik van de [Functieselectie op basis van het Filter] [ filter-based-feature-selection] module. Voor het gemak blijven in het voorbeeld van de analysestructuur tekst. Wordt ervan uitgegaan dat u maken van een regressiemodel wilt nadat een set van 256 functies zijn gemaakt via de [hash-functie] [ feature-hashing] -module is en of de antwoord-variabele de 'Col1' adresboek revisie classificaties met getal van 1 tot en met 5. Door in te stellen op 'Functie scoreprofiel methode' moet 'Pearson correlatie', 'doelkolom' als 'Col1' en het 'aantal gewenste functies' tot 50. Klik in de module [Functieselectie op basis van het Filter] [ filter-based-feature-selection] produceert een gegevensset met 50 functies samen met het kenmerk target 'Col1'. De volgende afbeelding ziet de stroom van dit experiment en de invoerparameters:

![Voorbeeld van de functie-selectie](./media/select-features/feature-Selection1.png)

De volgende afbeelding ziet de resulterende gegevenssets:

![Voorbeeld van de functie-selectie](./media/select-features/feature-Selection2.png)

Elke functie wordt berekend op basis van de correlatie Pearson tussen zichzelf en het kenmerk target 'Col1'. De functies van bovenste scores worden bewaard.

De bijbehorende scores van de geselecteerde onderdelen worden weergegeven in de volgende afbeelding:

![Voorbeeld van de functie-selectie](./media/select-features/feature-Selection3.png)

Door het toepassen van dit [Functieselectie op basis van het Filter] [ filter-based-feature-selection] module, 50 van 256 functies zijn geselecteerd omdat ze de meest gecorreleerde functies met de doelvariabele 'Col1' hebben op basis van de score 'Pearson correlatie'-methode.

## <a name="conclusion"></a>Conclusie
Functie-engineering en Functieselectie zijn twee meestal ontworpen en geselecteerde onderdelen verhoogt de efficiëntie van het trainingsproces die pogingen tot het extraheren van gegevens van de sleutel die is opgenomen in de gegevens. Ze verbeteren ook de kracht van deze modellen voor het classificeren van de invoergegevens nauwkeurig en meer krachtig voorspellen van resultaten van belang. Selectie van functie-engineering en kunnen ook zodat de learning meer rekenkundig tractable combineren. Dit gebeurt met verbeteren en vervolgens het aantal benodigde kalibreren of het model trainen functies te verminderen. Wiskundig spreken, zijn de functies die zijn geselecteerd voor het model trainen een minimale set van onafhankelijke variabelen die de patronen in de gegevens wordt uitgelegd en vervolgens de resultaten met succes voorspellen.

Het is niet altijd noodzakelijkerwijs Functieselectie techniek of functie uitvoeren. Of het nodig of niet is, is afhankelijk van de verzamelde gegevens, de algoritme die is geselecteerd en het doel van het experiment.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

