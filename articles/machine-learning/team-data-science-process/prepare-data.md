---
title: Reinigen en voorbereiden van gegevens voor Azure Machine Learning | Microsoft Docs
description: Vooraf verwerken en gegevens naar de voorbereiding op deze machine learning opgeschoond.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 7f0c1f0f549e746cc99db3b47f6c90bb51145d5d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Taken om gegevens voor te bereiden voor verbeterde Machine Learning
Vooraf verwerken en het opruimen van gegevens zijn belangrijke taken die normaal gesproken plaatsvinden moeten voordat gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak veel ruis veroorzaken en onbetrouwbare mogelijk ontbreken waarden. Met deze gegevens voor modellering kan misleidende resultaten opleveren. Deze taken maken deel uit van het Team gegevens wetenschap proces (TDSP) en doorgaans als volgt een initiële exploratie van een gegevensset die wordt gebruikt om te detecteren en plannen van de vooraf verwerken vereist. Voor meer instructies voor het proces TDSP gedetailleerde, Zie de stappen in de [Team gegevens wetenschap proces](overview.md).

Vooraf verwerken en opschonen van taken, zoals de taak van de exploratie gegevens kunnen worden uitgevoerd in een groot aantal verschillende omgevingen, zoals SQL of Hive of Azure Machine Learning Studio, en met verschillende hulpprogramma's en talen, zoals het R- of Python, afhankelijk van waar de gegevens worden opgeslagen en hoe deze is geformatteerd. Omdat TDSP iteratieve aard, deze taken kunnen worden uitgevoerd op verschillende stappen in de werkstroom van het proces.

Dit artikel bevat verschillende gegevensverwerking concepten en taken beschreven die kunnen worden uitgevoerd vóór of na het ophalen van gegevens in Azure Machine Learning.

Zie voor een voorbeeld van gegevensverkenning en vooraf verwerken die zijn uitgevoerd in Azure Machine Learning studio, de [vooraf verwerken van gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.

## <a name="why-pre-process-and-clean-data"></a>Waarom vooraf verwerken en de gegevens opschonen?
Concrete gegevens uit diverse bronnen worden verzameld en processen en onregelmatigheden of beschadigde gegevens inbreuk op de kwaliteit van de gegevensset mag bevatten. De standaard data quality-problemen die ontstaan zijn:

* **Onvolledig**: gegevens beschikt niet over kenmerken of met ontbrekende waarden.
* **Veel ruis veroorzaken**: gegevens foutieve records bevat of uitschieters.
* **Inconsistente**: gegevens bevat conflicterende records of afwijkingen.

Kwaliteitsgegevens is een vereiste voor kwaliteit voorspellende modellen. Als u wilt voorkomen 'garbagecollection in garbagecollection out' en kwaliteit van de gegevens te verbeteren en daarom model prestaties, is het noodzakelijk voor het uitvoeren van een health het scherm om problemen met de vroeg herkennen en bepaal de bijbehorende gegevens verwerken en de reinigingstape stappen.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Wat zijn enkele typische gegevens health schermen die worden gebruikt?
We kunnen de algemene kwaliteit van gegevens controleren door te controleren:

* Het aantal **records**.
* Het aantal **kenmerken** (of **functies**).
* Het kenmerk **gegevenstypen** (nominaal, rangtelwoord of continu).
* Het aantal **ontbrekende waarden**.
* **-Opmaak** van de gegevens.
  * Als de gegevens in TSV- of CSV, controleert u of de kolom scheidingstekens en een regel scheidingstekens altijd goed kolommen en lijnen scheiden.
  * Als de gegevens in de HTML-indeling of XML-indeling, controleert u of de gegevens is opgemaakt op basis van hun respectieve standaarden.
  * Bij het parseren van kan ook nodig zijn om gestructureerde gegevens ophalen uit semi-gestructureerde of ongestructureerde gegevens.
* **Inconsistente gegevensrecords**. Controleer het bereik van waarden zijn toegestaan. bijvoorbeeld als de gegevens studenten GPA, Controleer bevatten of de GPA in het opgegeven bereik is 0 spreken ~ 4.

Als u problemen met de gegevens gevonden **verwerken stappen** nodig die vaak omvat het opruimen van de ontbrekende waarden, gegevensnormalisatie, onderscheidende zijn, verwerking van tekst naar verwijderen en/of vervang ingesloten tekens die mogelijk van invloed op gegevens uitlijning gemengde gegevenstypen gemeenschappelijk velden en anderen.

**Azure Machine Learning verbruikt juist opgemaakte tabelgegevens**.  Als de gegevens zich al in tabelvorm, worden gegevens vooraf verwerken rechtstreeks met Azure Machine Learning in de Machine Learning Studio uitgevoerd.  Als gegevens niet in tabelvorm, als deze zich in de XML parseren mogelijk zijn vereist om de gegevens omzetten in tabelvorm.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Wat zijn enkele van de belangrijkste taken in gegevens vooraf verwerken?
* **Gegevens gereinigd**: invullen of ontbrekende waarden detecteren en veel ruis veroorzaken en uitschieters verwijderen.
* **Gegevenstransformatie**: normaliseren gegevens als dimensies en ruis wilt beperken.
* **Gegevens vermindering**: Sample gegevensrecords of kenmerken voor gemakkelijker de verwerking van gegevens.
* **Gegevens onderscheidende**: doorlopende kenmerken met bepaalde methoden van machine learning converteren naar categorische kenmerken voor eenvoudig te gebruiken.
* **Tekst reinigen**: Verwijder de ingesloten tekens waardoor de uitlijning van gegevens voor bijvoorbeeld ingesloten tabbladen in een gegevensbestand met door tabs gescheiden ingesloten voor nieuwe regels die mogelijk worden verbroken records, enzovoort.

De volgende secties bevatten informatie over enkele van de volgende stappen verwerken van gegevens.

## <a name="how-to-deal-with-missing-values"></a>Het omgaan met ontbrekende waarden?
Om door te gaan met ontbrekende waarden, is het beste eerst de reden voor de ontbrekende waarden beter wordt verwerkt het probleem te achterhalen. Typische ontbrekende waarde verwerking methoden zijn:

* **Verwijdering**: verwijderen van records met ontbrekende waarden
* **Vervanging testupdate worden uitgevoerd**: ontbrekende waarden vervangt door een dummy-waarde: bijvoorbeeld, *onbekende* voor categorische of 0 voor numerieke waarden.
* **Gemiddelde vervanging**: als de ontbrekende gegevens numerieke, de ontbrekende waarden vervangt het gemiddelde.
* **Regelmatige vervanging**: als de ontbrekende gegevens categorische, de ontbrekende waarden vervangt het meest voorkomende item
* **Regressie vervanging**: gebruik van een regressiemethode ontbrekende waarden vervangt door de waarden opgelost.  

## <a name="how-to-normalize-data"></a>Hoe kan ik gegevens normaliseren?
Gegevensnormalisatie schaalt opnieuw numerieke waarden aan een opgegeven bereik. Populaire gegevens normalisatie methoden zijn:

* **Min-Max normalisatie**: Lineair transformeren naar een bereik, spreken tussen 0 en 1, waarbij de min-waarde in 0 en max-waarde moet 1 wordt geschaald.
* **Z-score normalisatie**: gegevens op basis van gemiddelde en standaarddeviatie schalen: het verschil tussen de gegevens en het gemiddelde wordt gedeeld door de standaarddeviatie.
* **Decimale schalen**: schalen van de gegevens door het verplaatsen van het decimaalteken van de kenmerkwaarde.  

## <a name="how-to-discretize-data"></a>Hoe gegevens moeten worden onderscheiden?
Gegevens kunnen worden onderscheiden door doorlopende waarden converteren naar nominaal kenmerken of intervallen. Er zijn een aantal manieren om dit te doen:

* **Gelijke breedte Binning**: het bereik van alle mogelijke waarden van een kenmerk verdelen in groepen N van dezelfde grootte en de waarden die niet in een opslaglocatie toewijzen met het opslaglocatienummer.
* **Met Binning gelijk hoogte**: het bereik van alle mogelijke waarden van een kenmerk verdelen in groepen, N, elk met hetzelfde aantal exemplaren en vervolgens de waarden die niet in een opslaglocatie toewijzen met het opslaglocatienummer.  

## <a name="how-to-reduce-data"></a>Hoe om gegevens te reduceren?
Er zijn verschillende methoden om gegevensgrootte voor gemakkelijker de afhandeling van gegevens te beperken. Afhankelijk van de gegevensgrootte van en het domein, kunnen de volgende methoden worden toegepast:

* **Vastleggen van steekproeven**: voorbeeld van de records met gegevens en kiest u alleen de representatieve subset van de gegevens.
* **Kenmerk steekproeven**: alleen een subset van de belangrijkste kenmerken van de gegevens te selecteren.  
* **Aggregatie**: de gegevens verdelen in groepen en opslaan van de getallen voor elke groep. Bijvoorbeeld, kunnen de dagelijkse omzet getallen van een keten restaurant in de afgelopen 20 jaar worden samengevoegd voor maandelijkse omzet om de grootte van de gegevens te beperken.  

## <a name="how-to-clean-text-data"></a>Hoe tekst gegevens?
**Tekstvelden in tabelgegevens** kunnen tekens die van invloed op grenzen die kolommen uitlijning en/of record bevatten. Voor bijvoorbeeld tabbladen in een bestand met door tabs gescheiden oorzaak kolom uitlijning van gegevenstypen ingesloten en ingesloten opsplitsen nieuwe-regeltekens record regels. Onjuiste tekst codering verwerking tijdens het schrijven/lezen tekst leidt tot gegevensverlies, onbedoeld introductie van onleesbaar tekens, bijvoorbeeld nulwaarden en mogelijk ook invloed tekst te parseren. Zorgvuldige geparseerd en bewerkt mogelijk nodig om op te schonen tekstvelden voor de juiste uitlijning en/of extract gestructureerde gegevens van niet-gestructureerde of semi-gestructureerde gegevens.

**Gegevensverkenning** biedt een vroege weergave in de gegevens. Een aantal problemen kan ongedekte tijdens deze stap en bijbehorende methoden kunnen worden toegepast om die problemen op te lossen.  Het is belangrijk om te vragen zoals wat is er de oorzaak van het probleem en hoe het probleem mogelijk ontstaan. Dit helpt u ook bepalen op de gegevensverwerking stappen die worden uitgevoerd moeten om deze kunt oplossen. Het soort insights een wil worden afgeleid van de gegevens kan ook worden gebruikt om de prioriteit van de gegevensverwerking inspanning.

## <a name="references"></a>Verwijzingen
> *Gegevensanalyse: Concepten en technieken*, Third Edition Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber en Jian Pei
> 
> 

