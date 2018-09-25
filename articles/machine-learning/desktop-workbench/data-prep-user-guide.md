---
title: Uitgebreide handleiding voor het gebruik van Azure Machine Learning Data Preparations | Microsoft Docs
description: Dit document bevat een overzicht en meer informatie over het oplossen van problemen met gegevens met Azure Machine Learning Data Preparations
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 7536e67d0ae4973008c8acc91a99a7d0d286f9b8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988730"
---
# <a name="data-preparations-user-guide"></a>Gebruikershandleiding voor de voorbereidingen 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


De ervaring van de Azure Machine Learning Data Preparations biedt veel van de uitgebreide functionaliteit. In dit artikel worden de diepste onderdelen van de ervaring.

### <a name="step-execution-history-and-caching"></a>Stap worden uitgevoerd, geschiedenis en opslaan in cache 
Geschiedenis van stap voorbereidingen onderhoudt een reeks caches voor betere prestaties. Als u een stap te selecteren en dit bij een cache aankomt, deze niet opnieuw uit te voeren. Als u een blok schrijven aan het einde van de geschiedenis stap hebt en u spiegelen heen en weer op de stappen, maar geen wijzigingen aanbrengt, worden de voor schrijven wordt niet geactiveerd na de eerste keer. Een nieuwe Schrijf optreedt en de vorige versie, worden overschreven als u:

- Wijzigingen aanbrengen aan het blok schrijven.
- Een nieuwe transformatie-adresblok toevoegen en plaatst u deze boven het blok schrijven, waardoor een invalidatie worden gegenereerd.
- Wijzig de eigenschappen van een blok boven het blok schrijven, waardoor een invalidatie worden gegenereerd.
- Selecteer vernieuwen van een steekproef (dus alle caches ongeldig).

### <a name="error-values"></a>Foutwaarden

Gegevenstransformaties kunnen mislukken voor een invoerwaarde omdat deze waarde kan niet op de juiste wijze worden verwerkt. In het geval van bewerkingen van het type dwang mislukt de dwang bijvoorbeeld, als de ingevoerde tekenreeks-waarde kan niet worden geconverteerd naar het opgegeven doeltype. Een bewerking voor het afdwingen van type mogelijk een kolom van het tekenreekstype converteren naar een numerieke of Boolean-type of er wordt geprobeerd te dupliceren van een kolom die niet bestaat. (Deze fout treedt op als het resultaat van het verplaatsen van de *kolom X verwijderen* bewerking wordt uitgevoerd voordat de *dubbele kolom X* bewerking.)

In dergelijke gevallen levert Data Preparations een foutwaarde als de uitvoer. Foutwaarden geven aan dat een eerdere bewerking is mislukt voor de opgegeven waarde. Intern, ze worden behandeld als een eersteklas waardetype, maar het onderliggende type van een kolom niet van invloed op hun aanwezigheid, zelfs als een kolom alleen uit foutwaarden bestaat.

Foutwaarden zijn gemakkelijk te herkennen. Ze rood gemarkeerd en lezen van "Error". De reden voor de fout, Beweeg de muisaanwijzer over een foutwaarde om te zien van een beschrijving van het probleem bepalen.

Foutwaarden doorgeven. Na een fout wordt weergegeven, deze in de meeste gevallen wordt doorgegeven als een fout in de meeste bewerkingen. Er zijn drie manieren om te vervangen of verwijder ze:

* Vervangen
    -  Met de rechtermuisknop op een kolom en selecteer **vervangen door foutwaarden**. Vervolgens kunt u een vervangende waarde voor elke foutwaarde gevonden in de kolom.

* Verwijderen
    - Data Preparations bevat interactieve filters wilt behouden of verwijderen van foutwaarden.
    - Met de rechtermuisknop op een kolom en selecteer **filterkolom**. Als u wilt behouden of verwijderen van foutwaarden, maakt u een voorwaardelijke met de voorwaarde *"is een fout"* of *"is geen fout."*

* Een Python-expressie gebruiken voorwaardelijk foutwaarden bewerkingen uitvoeren. Zie voor meer informatie de [sectie over Python-extensies](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Steekproeven
Een bestand gegevensbronnen wordt in de onbewerkte gegevens van een of meer bronnen, vanuit het lokale bestandssysteem of een externe locatie. Het blok voorbeeld kunt u opgeven of u wilt werken met een subset van de gegevens door het genereren van voorbeelden. Besturingssysteem van een steekproef van de gegevens in plaats van een grote gegevensset vaak leidt tot betere prestaties wanneer u bewerkingen in latere stappen uitvoeren.

Voor elk bestand gegevensbronnen kunnen meerdere steekproeven worden gegenereerd en opgeslagen. Slechts één voorbeeld kan echter worden ingesteld als de actieve voorbeeld. U kunt maken, bewerken of verwijderen van de voorbeelden in de wizard gegevensbron of door de voorbeeld-blok te bewerken. Data Preparations bestanden die verwijzen naar een gegevensbron inherent gebruik het voorbeeld dat is opgegeven in het bestand gegevensbronnen.

Er zijn een aantal steekproeven strategieën beschikbaar is, elk met verschillende parameters, kunnen worden geconfigureerd.

#### <a name="top"></a>Boven
Deze strategie kan worden toegepast op lokale of externe bestanden. Het duurt de eerste N rijen (opgegeven door het aantal) in de gegevensbron.

#### <a name="random-n"></a>Willekeurige N 
Deze strategie kan alleen op lokale bestanden worden toegepast. Het duurt willekeurige N rijen (opgegeven door het aantal) in de gegevensbron. U kunt een specifieke seed om ervoor te zorgen dat de dezelfde steekproef wordt gegenereerd, mits Count ook hetzelfde is opgeven.

#### <a name="random-"></a>Willekeurige % 
Deze strategie kan worden toegepast op lokale of externe bestanden. In beide gevallen moeten een kans en een seed worden opgegeven, vergelijkbaar met de willekeurige N-strategie.

Voor voorbeelden van externe bestanden moeten aanvullende parameters worden opgegeven:

- Voorbeeld-generator 
  - Selecteer een Spark-cluster of extern Docker compute-doel moet worden gebruikt voor het genereren van het voorbeeld. De compute-doel moet worden gemaakt voor het project vooraf voor het in deze lijst wordt weergegeven. Volg de stappen in de sectie 'maken een nieuwe rekendoel' in [over het gebruik van GPU in Azure Machine Learning](how-to-use-gpu.md) compute-doelen te maken.
- Voorbeeld van opslag 
  - Geef een tussenliggende opslaglocatie voor het opslaan van de externe voorbeeld. Dit pad moet een andere map vanaf de locatie van het invoerbestand.

#### <a name="full-file"></a>Volledig bestand 
Deze strategie kan alleen op lokale bestanden, het volledige bestand houdend met de gegevensbron worden toegepast. Als het bestand te groot is is, kan deze optie toekomstige bewerkingen in de app vertragen. Wellicht meer geschikt voor gebruik in een andere samplingstrategie.


### <a name="fork-merge-and-append"></a>Splitsen, samenvoegen en toevoegen

Als u een filter op een gegevensset toepassen, de bewerking de gegevens gesplitst in twee resultatensets: één set vertegenwoordigt records die in het filter zou lukken, en een andere set is voor de records die niet voldoen aan. In beide gevallen kan de gebruiker kiezen welke resultatenset om weer te geven. De gebruiker kan de andere gegevensset negeren of plaats deze in een nieuwe gegevensstroom. De laatste optie wordt aangeduid als vertakking.

Een fork van: 
1. Selecteer een kolom, klik met de rechtermuisknop en selecteer de **Filter** kolom.

2. Onder **ik wil**, selecteer **rijen behouden** om de resultatenset die wordt doorgegeven bij het filter weer te geven.

3. Selecteer **rijen verwijderen** om de mislukte set weer te geven.

4. Na **voorwaarden**, selecteer **maken gegevensstroom met de rijen gefilterd uit** een fork van de resultaatset niet weergeven in een nieuwe gegevensstroom.


Met deze procedure wordt vaak gebruikt voor het scheiden van een set gegevens die extra voorbereiding vereist. Nadat u de Gevorkte gegevensset hebt voorbereid, is het gebruikelijk dat de gegevens samenvoegen met het resultaat ingesteld in de oorspronkelijke gegevensstroom. Als u wilt samenvoegen (de achterzijde van een bewerking fork) worden uitgevoerd, gebruikt u een van de volgende acties:

- **Rijen toevoegen**. Twee of meer gegevensstromen verticaal samenvoegen (row-wise). 
- **Kolommen toevoegen**. Twee of meer gegevensstromen horizontaal samenvoegen (column-wise).


>[!NOTE]
>Toevoeg-kolommen mislukt als er een conflict kolom optreedt.


Na een samenvoeging wordt verwezen naar een of meer gegevensstromen door de gegevensstroom van een bron. Data Preparations ontvangt u een melding met een melding in de rechterbenedenhoek van de app, onder de lijst met stappen.


Elke bewerking in de gegevensstroom waarnaar wordt verwezen, moet de gegevensstroom bovenliggende vernieuwen van het voorbeeld gebruikt van de stroom gegevens waarnaar wordt verwezen. In dat geval vervangt een bevestigingsdialoogvenster de gegevensstroom verwijzing melding in de rechterbenedenhoek. Het dialoogvenster bevestigt dat u moet de gegevensstroom om te synchroniseren met wijzigingen aan alle stromen van afhankelijkheid gegevens vernieuwen.

### <a name="list-of-appendices"></a>Lijst met bijlagen 
* [Ondersteunde gegevensbronnen](data-prep-appendix2-supported-data-sources.md)  
* [Ondersteunde transformaties](data-prep-appendix3-supported-transforms.md)  
* [Ondersteunde controles](data-prep-appendix4-supported-inspectors.md)  
* [Ondersteunde bestemmingen](data-prep-appendix5-supported-destinations.md)  
* [Voorbeeld van filterexpressies in Python](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Voorbeeld transformeren gegevensstroom expressies in Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Voorbeeld van gegevensbronnen in Python](data-prep-appendix8-sample-source-connections-python.md)  
* [Voorbeeld doelverbindingen in Python](data-prep-appendix9-sample-destination-connections-python.md)  
* [Voorbeeld-kolom voor gegevensstromen in Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  
