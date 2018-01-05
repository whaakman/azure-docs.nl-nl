---
title: Gedetailleerde richtlijnen voor het gebruik van Azure Machine Learning gegevens voorbereidingen | Microsoft Docs
description: Dit document bevat een overzicht en meer informatie over het oplossen van problemen met Azure Machine Learning gegevens voorbereidingen gegevens
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 9bcdd539c199086e0f48c1172853ff00cc1617f8
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="data-preparations-user-guide"></a>Gebruikershandleiding voor de voorbereidingen 
De Azure Machine Learning gegevens voorbereidingen ervaring biedt een groot aantal uitgebreide functionaliteit. Dit artikel wordt de diepste onderdelen van de gebruikerservaring.

### <a name="step-execution-history-and-caching"></a>Stap worden uitgevoerd, geschiedenis en opslaan in cache 
Geschiedenis van stap voorbereidingen onderhoudt een reeks met caches voor betere prestaties. Als u een stap selecteren en dit bij een cache aankomt, niet het opnieuw uitvoeren. Als u een blok schrijven aan het einde van de geschiedenis van de stap hebt en u spiegelen heen en weer over de stappen, maar geen wijzigingen aanbrengt, worden de voor schrijven is niet geactiveerd na de eerste keer. Een nieuwe schrijven deze gebeurtenis treedt op als het vorige logboek wordt overschreven als u:

- Wijzigingen aanbrengen in het blok schrijven.
- Toevoegen van een nieuw blok met transformatie en plaatst u deze boven het blok schrijven, waardoor een cache ongeldig maken worden gegenereerd.
- Wijzig de eigenschappen van een blok boven het blok schrijven, waardoor een cache ongeldig maken worden gegenereerd.
- Selecteer het vernieuwen van een steekproef (dus alle caches ongeldig).

### <a name="error-values"></a>Foutwaarden

Gegevenstransformaties kunnen mislukken voor een invoerwaarde omdat deze waarde kan niet op de juiste wijze worden verwerkt. Bijvoorbeeld, in het geval van bewerkingen van het type afdwingen mislukt de afdwingen als de waarde voor de invoer kan niet worden geconverteerd naar het opgegeven doeltype. De bewerking van een type afdwingen mogelijk een kolom van het tekenreekstype converteren naar een numeriek gegevenstype of Boolean-type of bij het dupliceren van een kolom die niet bestaat. (Deze fout treedt op als gevolg van het verplaatsen van de *kolom X verwijdert* bewerking voordat u de *dubbele kolom X* bewerking.)

In dergelijke gevallen levert gegevens voorbereidingen een foutwaarde als uitvoer. Foutwaarden geven aan dat een eerdere bewerking is mislukt voor de opgegeven waarde. Intern maakt ze worden behandeld als een uitstekende waardetype, maar het onderliggende type van een kolom niet van invloed op de aanwezigheid zelfs als een kolom, geheel uit foutwaarden bestaat.

Foutwaarden zijn gemakkelijk te herkennen. Ze zijn rood gemarkeerd en lezen "Error". De reden voor de fout, houd de muis boven een foutwaarde als u een tekstbeschrijving van het probleem bepalen.

Foutwaarden doorgeven. Na een fout wordt weergegeven, deze in de meeste gevallen wordt doorgegeven als een fout via de meeste bewerkingen. Er zijn drie manieren om te vervangen of verwijder ze:

* Vervangen
    -  Een kolom met de rechtermuisknop en selecteer **foutwaarden vervangen**. Vervolgens kunt u een vervangende waarde voor elke foutwaarde gevonden in de kolom.

* Verwijderen
    - Gegevens voorbereidingen interactieve filters wilt behouden of verwijderen van foutwaarden bevat.
    - Een kolom met de rechtermuisknop en selecteer **filterkolom**. Als u wilt behouden of verwijderen van foutwaarden, maakt u een voorwaardelijke met de voorwaarde *'is de fout'* of *"is geen fout."*

* Gebruik een Python-expressie te foutwaarden voorwaardelijk bewerken. Zie voor meer informatie de [sectie over Python-extensies](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Steekproeven
Een bestand gegevensbronnen wordt in de onbewerkte gegevens uit een of meer bronnen, vanuit het lokale bestandssysteem of een externe locatie. Het blok voorbeeld kunt u opgeven of u wilt werken met een subset van de gegevens steekproeven te genereren. Besturingssysteem van een steekproef van de gegevens in plaats van een grote gegevensset vaak leidt tot betere prestaties wanneer u bewerkingen in latere stappen uitvoeren.

Voor elk bestand gegevensbronnen kunnen meerdere voorbeelden worden gegenereerd en opgeslagen. Slechts één voorbeeld kan echter worden ingesteld als het actieve voorbeeld. U kunt maken, bewerken of verwijderen van de voorbeelden in de wizard gegevensbron of bij het bewerken van het voorbeeld-blok. Gegevens voorbereidingen bestanden die naar een gegevensbron inherent verwijzen gebruik het voorbeeld dat is opgegeven in het bestand gegevensbronnen.

Er zijn een aantal steekproeven strategieën beschikbaar is, elk met verschillende configureerbare parameters.

#### <a name="top"></a>Boven
Deze strategie kan worden toegepast op lokale of externe bestanden. Het duurt de eerste N rijen (opgegeven op het aantal) in de gegevensbron.

#### <a name="random-n"></a>Willekeurige N 
Deze strategie kan worden alleen toegepast op lokale bestanden. Het duurt willekeurige N rijen (opgegeven op het aantal) in de gegevensbron. U kunt een specifieke seed om ervoor te zorgen dat de dezelfde steekproef wordt gegenereerd, mits de telling is ook dezelfde opgeven.

#### <a name="random-"></a>Willekeurige % 
Deze strategie kan worden toegepast op lokale of externe bestanden. In beide gevallen moet een kans en een seed zijn opgegeven, vergelijkbaar met de willekeurige N-strategie.

Extra parameters moeten worden opgegeven voor voorbeelden van externe bestanden:

- Voorbeeld generator 
  - Selecteer een Spark-cluster of extern Docker compute-doel moet worden gebruikt voor het genereren van de steekproef. De compute-doel moet worden gemaakt voor het project vooraf om te worden weergegeven in deze lijst. Volg de stappen in de sectie 'maken een nieuw compute-doel' in [GPU gebruiken in Azure Machine Learning](how-to-use-gpu.md) compute-doelen maken.
- Voorbeeld-opslag 
  - Geef een locatie voor de tussenliggende opslag voor het opslaan van het externe voorbeeld. Dit pad moet een andere map vanaf de locatie van het bestand voor invoer.

#### <a name="full-file"></a>Volledige 
Deze strategie kan worden alleen toegepast op lokale bestanden, het volledige bestand houdend met de gegevensbron. Als het bestand te groot is, kan deze optie toekomstige acties in de app vertragen. Wellicht meer geschikt is voor gebruik in een strategie voor verschillende steekproeven.


### <a name="fork-merge-and-append"></a>Vertakken, samenvoegen en toevoegen

Als u een filter via een gegevensset toepassen, de bewerking de gegevens gesplitst in twee resultatensets: één set vertegenwoordigt records die in het filter slagen, en een andere set is voor de records die niet voldoen aan. In beide gevallen kan de gebruiker kiezen welke resultaatset om weer te geven. De gebruiker kan de andere gegevensset negeren of deze in een nieuwe gegevensstroom te plaatsen. De laatste optie aangeduid als vertakken.

Om te vertakken: 
1. Selecteer een kolom, klik met de rechtermuisknop en selecteer de **Filter** kolom.

2. Onder **I wilt**, selecteer **rijen behouden** om de resultatenset die wordt doorgegeven van het filter weer te geven.

3. Selecteer **rijen verwijderen** om de mislukte set weer te geven.

4. Na **voorwaarden**, selecteer **maken gegevensstroom met de rijen gefilterd uit** naar de resultaatset niet weergeven in een nieuwe gegevensstroom vertakken.


Deze procedure wordt vaak gebruikt voor het scheiden van een set gegevens die aanvullende voorbereidende stappen vereist. Nadat u de gegevensset Gevorkte wrangle, wordt het meestal de gegevens worden samengevoegd met de resultaatset in de oorspronkelijke gegevensstroom. Als u wilt samenvoegen (het omgekeerde van een bewerking fork), een van de volgende acties te gebruiken:

- **Rijen toevoegen**. Samenvoegen van twee of meer gegevensstromen verticaal (row-wise). 
- **Kolommen toevoegen**. Samenvoegen van twee of meer gegevensstromen horizontaal (column-wise).


>[!NOTE]
>Kolommen mislukt toevoegen als een kolom conflict ontstaat.


Na een merge-bewerking, worden een of meer gegevensstromen verwezen door een bron-gegevensstroom. Gegevens voorbereidingen meldt een melding in de rechterbenedenhoek van de app, onder de lijst met stappen.


Een bewerking voor de stroom gegevens waarnaar wordt verwezen moet de bovenliggende gegevensstroom vernieuwen van de steekproef gebruikt vanuit de stroom gegevens waarnaar wordt verwezen. In dat geval wordt een bevestigingsvenster vervangen melding verwijzing voor de stroom van gegevens in de rechterbenedenhoek. Het dialoogvenster wordt bevestigd dat u wilt vernieuwen van de gegevensstroom om te synchroniseren met wijzigingen in een afhankelijkheid gegevensstromen.

### <a name="list-of-appendices"></a>Lijst met bijlagen 
* [Ondersteunde gegevensbronnen](data-prep-appendix2-supported-data-sources.md)  
* [Ondersteunde transformaties](data-prep-appendix3-supported-transforms.md)  
* [Ondersteunde inspectors](data-prep-appendix4-supported-inspectors.md)  
* [Ondersteunde bestemmingen](data-prep-appendix5-supported-destinations.md)  
* [Voorbeeld filterexpressies in Python](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Voorbeeld transformatie gegevensstroom expressies in Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Voorbeeld van gegevensbronnen in Python](data-prep-appendix8-sample-source-connections-python.md)  
* [Voorbeeld bestemming verbindingen in Python](data-prep-appendix9-sample-destination-connections-python.md)  
* [Voorbeeld kolom transformaties in Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  
