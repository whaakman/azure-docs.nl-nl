---
title: Functie-engineering in gegevenswetenschap | Microsoft Docs
description: Worden de doeleinden van functie-engineering en voorbeelden gegeven van de rol in de procedure voor de verbetering van gegevens van machine learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: c0933fec67bbcfc2a8c450c1e2472440cf38f338
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="feature-engineering-in-data-science"></a>Functie-engineering in gegevenswetenschap
In dit onderwerp worden de doeleinden van functie-engineering en voorbeelden gegeven van de rol in de procedure voor de verbetering van gegevens van machine learning. De voorbeelden die wordt gebruikt ter illustratie van dit proces zijn afkomstig van Azure Machine Learning Studio. 

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Dit **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u functies voor gegevens in verschillende omgevingen maken. Deze taak is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Functie engineering pogingen tot het verhogen van de voorspellende kracht van learning-algoritmen door het maken van de functies van ruwe gegevens die helpen bij het leerproces vergemakkelijken. De engineering en de selectie van functies is een onderdeel van de TDSP die worden beschreven in de [wat is de levensduur van het Team gegevens wetenschap proces?](overview.md) Functie-engineering en een selectie maken deel uit van de **functies ontwikkelen** stap van de TDSP. 

* **functie-engineering**: dit proces probeert extra relevante functies maken op basis van de bestaande onbewerkte functies in de gegevens, en de voorspellende kracht van het leeralgoritme verhogen.
* **functie selectie**: dit proces selecteert u de belangrijkste subset van de oorspronkelijke functies van de gegevens in een poging de dimensionaliteit van het probleem training verminderen.

Normaal gesproken **functie-engineering** als eerste toegepast voor het genereren van aanvullende functies, en vervolgens de **functie selectie** stap wordt uitgevoerd om de functies niet relevant, redundante of maximaal gecorreleerde elimineren.

De trainingsgegevens gebruikt in machine learning kan vaak worden verbeterd door extractie van functies van de onbewerkte gegevens verzameld. Een voorbeeld van een engineering-functie in de context van het leren van het classificeren van de afbeeldingen van geschreven tekens is het maken van iets dichtheid kaart gemaakt op basis van het gegevenstype raw bit distributie. Deze kaart kan helpen bij de randen van de tekens efficiënter dan gewoon rechtstreeks met de onbewerkte distributie vinden.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="creating-features-from-your-data---feature-engineering"></a>Maken van de functies van uw gegevens - functie-Engineering
De trainingsgegevens bestaat uit een matrix die bestaan uit voorbeelden (records of opmerkingen die zijn opgeslagen in rijen), die allemaal een aantal functies (variabelen of velden die zijn opgeslagen in de kolommen). De functies die zijn opgegeven in het ontwerp voor experimentele worden kenmerkend zijn voor de patronen in de gegevens verwacht. Hoewel veel van de velden onbewerkte gegevens kunnen rechtstreeks worden opgenomen in de geselecteerde functieset gebruikt voor het model trainen, is het vaak het geval dat extra (Engineering) functies worden gemaakt op basis van de functies in de onbewerkte gegevens moeten voor het genereren van een uitgebreide training-gegevensset.

Wat voor soort functies moet worden gemaakt voor het verbeteren van de gegevensset bij het trainen van een model? Engineering functies waarmee u de training kunt bieden informatie die beter de patronen in de gegevens onderscheidt. We verwachten dat de nieuwe functies extra informatie die niet duidelijk vastgelegde of gemakkelijk zichtbaar in de oorspronkelijke of een bestaande functieset. Maar dit proces is er een illustratie. Audio- en productief beslissingen vereist vaak sommige expertise in verschillende domeinen.

Bij het starten met Azure Machine Learning, is het eenvoudigste om te leren van dit proces concrete invulling te geven met behulp van voorbeelden in de Studio. Twee voorbeelden worden hier weergegeven:

* Een voorbeeld van een regressie [voorspelling van het aantal fiets huren](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) in een bewaakte experiment waarbij de doelwaarden bekend zijn
* Een tekst analysemodel classificatie voorbeeld met [hash-functies](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-adding-temporal-features-for-regression-model"></a>Voorbeeld 1: Tijdelijke functies toe te voegen voor regressiemodel
We gebruiken de experiment 'voorspellen van de vraag van bikes' in Azure Machine Learning Studio om te laten zien hoe u functies voor een taak regressie samenstellen. Het doel van dit experiment is het voorspellen van de vraag naar de fietsen, dat wil zeggen, het aantal fiets huren binnen een specifieke maand/dag/uur. De gegevensset ' fiets verhuur UCI gegevensset ' wordt gebruikt als de onbewerkte gegevens van de invoer. Deze gegevensset is gebaseerd op echte gegevens van het bedrijf kapitaal Bikeshare die een fiets verhuur-netwerk in Washington DC in de Verenigde Staten heeft. De gegevensset geeft het aantal fiets huren binnen een specifieke uur van een dag van het jaar 2011 en jaar 2012 en bevat 17379 rijen en kolommen 17. De onbewerkte functieset bevat weer voorwaarden (vochtigheid-temperatuur/o-snelheid) en het type van de dag (vakantiedag/werkdag). Het veld te voorspellen is 'cnt', een aantal die staat voor de huren fiets binnen een specifieke uur en die variëren van 1 tot 977 bereiken.

Met het doel van het construeren van effectieve functies in de trainingsgegevens, vier regressie modellen zijn gebouwd met behulp van dezelfde algoritme maar met vier verschillende training gegevenssets. De vier gegevenssets vertegenwoordigen de dezelfde onbewerkte invoergegevens, maar met een toenemend aantal functies ingesteld. Deze functies zijn gegroepeerd in vier categorieën:

1. A = weer + vakantiedag + weekdag + functies voor het weekend voor de voorspelde dag
2. B = aantal eventueel een gehuurd zijn in elk van de vorige 12 uur
3. C = aantal eventueel een gehuurd zijn in elk van de afgelopen 12 dagen op hetzelfde uur
4. D = aantal eventueel een gehuurd zijn in elk van de vorige 12 weken op hetzelfde uur en op dezelfde dag

Naast de functie set A, die al bestaan in de oorspronkelijke onbewerkte gegevens, worden de drie sets van functies gemaakt via de functie technisch proces. Functie ingesteld B opnamen recente aanvraag voor de bikes. Functie ingesteld C opnamen de vraag naar bikes op een bepaald uur. D opnamen aanvraag voor bikes functie ingesteld op bepaalde uur en bepaalde dag van de week. De vier training gegevenssets die elke omvat set-functie A, A + B, A + B + C en A + B + C + D, respectievelijk.

In het Azure Machine Learning-experiment worden deze vier training gegevenssets via vier vertakkingen van voorverwerkte invoergegevensset gevormd. Behalve de linkerkant de meeste vertakking elk van deze vertakkingen bevat een [R-Script uitvoeren](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) module, waarin een verzameling afgeleide functies (functie ingesteld B en C D) zijn respectievelijk samengesteld en toegevoegd aan de geïmporteerde gegevensset. De volgende afbeelding ziet het R-script gebruikt voor het maken van functieset B in de tweede links vertakking.

![functies maken](./media/create-features/addFeature-Rscripts.png)

Het vergelijken van de prestatieresultaten van de vier modellen worden samengevat in de volgende tabel. De beste resultaten worden weergegeven door functies A + B + C. Opmerking de frequentie van fouten neemt af wanneer extra functies zijn opgenomen in de trainingsgegevens. Er wordt gecontroleerd met onze vermoeden dat de functieset B, C aanvullende relevante informatie voor de taak regressie bieden. Maar de D-functie toe te voegen lijkt niet te bieden een extra verlaging van de frequentie van fouten.

![resultaat-vergelijking](./media/create-features/result1.png)

## <a name="example2"></a>Voorbeeld 2: Het maken van functies in analysemodel tekst
Functie-engineering is breed in taken met betrekking tot mijnbouw-tekst, zoals document indeling en het gevoel analyse toegepast. Bijvoorbeeld, als we classificeren van documenten in verscheidene categorieën wilt, is een typische veronderstelling dat de word/zinnen opgenomen in één categorie van het document zijn minder waarschijnlijk optreden in een andere doc-categorie. In een andere woorden kan de frequentie van de distributie woorden/woordgroepen kenmerkend zijn voor verschillende documentcategorieën. In toepassingen met tekst analysemodel, omdat er afzonderlijke stukjes tekstinhoud gewoonlijk gebruikt als de invoergegevens de functie engineering proces nodig voor het maken van de functies met betrekking tot woord of woordgroep frequenties.

Voor deze taak, een techniek aangeroepen **hash-functie** efficiënt functies van willekeurige tekst om in te veranderen indexen wordt toegepast. In plaats van elke functie tekst (woorden/zinnen) koppelen aan een specifieke index, deze methode werkt door het toepassen van een hash-functie op de functies en hun hash-waarden als indexen rechtstreeks.

In Azure Machine Learning is een [hash-functie](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) module die u maakt deze woord of woordgroep functies gemakkelijk. Volgende afbeelding toont een voorbeeld van het gebruik van deze module. Invoergegevensset bestaat uit twee kolommen: de adresboek classificatie tussen 1 en 5, en de inhoud van de werkelijke controleren. Het doel van dit [hash-functies](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) -module is voor het ophalen van een aantal nieuwe functies die tonen de frequentie van het exemplaar van de overeenkomstige tekst / r-zin(nen) binnen de bepaalde adresboek bekijken. Voor het gebruik van deze module, moeten we de volgende stappen uit:

* Selecteer eerst de kolom waarin de invoertekst ('Col2' in dit voorbeeld).
* Ten tweede instellen 'Hashing bitsize' op 8, wat betekent dat 2 ^ 8 = 256 functies wordt gemaakt. Het woord of fase in de tekst wordt naar 256 indexen worden opgedeeld. De parameter 'Hashing bitsize' bereik van 1 tot en met 31. De tekst / r-zin(nen) minder waarschijnlijk worden opgedeeld in dezelfde index als dat het een groter aantal zijn.
* Derde, stel de parameter 'N-grams' op 2. Dit haalt de frequentie van het exemplaar van unigrams (een functie voor elke één woord) en bigrams (een functie voor elk paar aangrenzende woorden) uit de invoertekst. De parameter 'N-grams' bereik van 0 tot en met 10, geeft het maximum aantal opeenvolgende woorden om naar te worden opgenomen in een functie.  

![Module 'Functie Hashing'](./media/create-features/feature-Hashing1.png)

De volgende afbeelding ziet u hoe deze nieuw uiterlijk functie zoals.

![Voorbeeld 'Functie Hashing'](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Conclusie
Engineering en geselecteerde functies verhoogt de efficiëntie van het trainingsproces die probeert om op te halen van de belangrijkste gegevens in de gegevens. Ze verbeteren ook de kracht van deze modellen voor het classificeren van de invoergegevens nauwkeurig en meer krachtig voorspellen van resultaten van belang. Selectie van functie-engineering en kunnen ook zodat de learning meer rekenkundig tractable combineren. Dit gebeurt met verbeteren en vervolgens het aantal benodigde kalibreren of het model trainen functies te verminderen. Wiskundig spreken, zijn de functies die zijn geselecteerd voor het model trainen een minimale set van onafhankelijke variabelen die de patronen in de gegevens wordt uitgelegd en vervolgens de resultaten met succes voorspellen.

Houd er rekening mee dat het is niet altijd noodzakelijkerwijs Functieselectie techniek of functie uitvoeren. Hiermee wordt aangegeven of het nodig is of niet, is afhankelijk van de gegevens die we hebben of verzamelen, het algoritme dat we kiezen en het doel van het experiment.

