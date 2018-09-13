---
title: Aan de slag met gegevens voor te bereiden voor Azure Machine Learning | Microsoft Docs
description: Dit is de introductiehandleiding voor gegevensvoorbereiding gedeelte met gegevens van workbench AML
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
ms.openlocfilehash: ffff3d274e093e9ce4aa32dee7b033bd1ed288bc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645994"
---
# <a name="getting-started-with-data-preparation"></a>Aan de slag met gegevensvoorbereiding

Welkom bij de aan de slag te gaan handleiding voor gegevensvoorbereiding. 

Voorbereiden van gegevens biedt een set hulpprogramma's voor het efficiënt verkennen, begrijpen en oplossen van problemen in de gegevens. Hiermee kunt u gebruiken voor gegevens in vele vormen en die gegevens omzetten in opgeschoonde gegevens die beter voor gebruik met downstream geschikt is.

Voorbereiden van gegevens is geïnstalleerd als onderdeel van de Azure Machine Learning Workbench-ervaring.  Voorbereiden van gegevens lokaal gebruiken of implementeren in een doelcluster en als een omgeving runtime of het uitvoeren van de cloud.

De runtime van de tijd ontwerp maakt gebruik van Python voor uitbreidbaarheid en is afhankelijk van verschillende Python-bibliotheken zoals Pandas. Zoals met andere onderdelen van Azure ML Workbench niet nodig om Python te installeren is, is het voor u geïnstalleerd. Maar als u nodig hebt voor het installeren van extra bibliotheken deze bibliotheken moeten worden geïnstalleerd in de map Azure ML Workbench Python niet uw gebruikelijke Python-map. Meer informatie over het installeren van pakketten vindt [hier](data-prep-python-extensibility-overview.md).

Een project is vereist voordat u gegevensvoorbereiding en kunt zodra dat project is gemaakt dat kunt u gegevens voorbereiden. 

Ga naar de sectie van de gegevens van het project door het pictogram van de gegevens te selecteren ![data source pictogram](media/data-prep-getting-started/data-source-icon.png) aan de linkerkant van het scherm.  Klik op '+' opnieuw aan bij **een gegevensbron toevoegen**. De Wizard gegevensbron moet starten en voegt een **gegevensbron** (.dsource)-bestand aan het project na het voltooien van de wizard. De weergave van de gegevens is standaard het raster. Boven het raster is het ook mogelijk om te selecteren van de weergave voor metrische gegevens. In de weergave voor metrische gegevens, worden samenvattende statistieken weergegeven.  Bekijk de samenvattende statistieken en klikt u op **voorbereiden** aan de bovenkant van het scherm. [Meer informatie over de Wizard gegevensbron](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Bouwstenen van gegevens voor te bereiden ##
### <a name="the-package"></a>Het pakket ###

Een pakket is de primaire container voor uw werk. Een pakket is het artefact dat is opgeslagen op en geladen van de schijf. Tijdens het werken in de client het pakket is voortdurend automatisch opgeslagen op de achtergrond. 

Het resultaat van een pakket kan worden verkend, Python of via een Jupyter-Notebook.

Een pakket kan worden uitgevoerd op meerdere runtimes waaronder lokale Python, Spark (inclusief in Docker) en HDInsight.

Een pakket bevat een of meer gegevensstromen die zijn de stappen en transformaties die worden toegepast op de gegevens.

Een pakket kan een ander pakket gebruiken als een gegevensbron (aangeduid als een referentie-gegevensstroom).

### <a name="the-dataflow"></a>De gegevensstroom ###
Een gegevensstroom heeft een bron en optionele transformeert die door een reeks stappen en optionele bestemmingen zijn gerangschikt. Alle bronnen en vóór de transformaties tot en met de geselecteerde stap te klikken op een stap opnieuw worden uitgevoerd.  De getransformeerde gegevens via deze stap wordt weergegeven in het raster. Stappen kunnen worden toegevoegd, verplaatst en binnen een gegevensstroom door de stap-lijst verwijderd.

De stappenlijst aan de rechterkant van de client kan worden geopend en gesloten om meer ruimte op het scherm.

Meerdere gegevensstromen kunnen tegelijkertijd bestaan in de gebruikersinterface, elke gegevensstroom wordt weergegeven als een tabblad in de gebruikersinterface.

### <a name="the-source"></a>De bron
Een bron is waar de gegevens vandaan komen en de indeling is in. Een gegevenspakket Prep bronnen altijd de gegevens uit een andere gegevens Flow(Data Source). Deze referentie gegevensstroom met de informatie is. Elke bron is de ervaring van een andere gebruiker toe te staan dat moet worden geconfigureerd. De bron produceert een "Vierkant" / tabellaire weergave van de gegevens. Als de brongegevens is oorspronkelijk "niet-aaneengesloten rechts", wordt klikt u vervolgens de structuur genormaliseerd als "Vierkant". [Bijlage 2 biedt de huidige lijst van ondersteunde bronnen](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>De transformatie ###
Transformaties gebruiken voor gegevens in een bepaalde indeling, het uitvoeren van een bewerking op de gegevens (zoals het wijzigen van het gegevenstype), en vervolgens het produceren van gegevens in de nieuwe indeling. Elke transformatie heeft een eigen gebruikersinterface en behavior(s). Koppeling van de verschillende transformaties samen door stappen te volgen in de gegevensstroom is de kern van de functionaliteit van de gegevens voor te bereiden. [Bijlage 3 vindt u de huidige lijst van ondersteunde transformaties](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>De inspector ###

Inspectors visualisaties van de gegevens zijn en beter inzicht te krijgen van de gegevens beschikbaar zijn.  Kennis van de gegevens en gegevens problemen kwaliteit kunt die u welke acties (transformaties bepalen) moeten worden genomen. Sommige Inspectors ondersteuning voor acties die transformaties te genereren. Bijvoorbeeld, kunt het aantal waarden Inspector u een waarde te selecteren en vervolgens een filter wilt die waarde opnemen of uitsluiten van die waarde toepassen. Inspectors kunnen ook context bieden voor transformaties. Bijvoorbeeld door een of meer kolommen te selecteren, verandert de mogelijke transformaties die kunnen worden toegepast.

Een kolom mogelijk meerdere Inspectors op elk gewenst moment in de tijd (bijvoorbeeld Kolomstatistieken en een Histogram). Er kan ook worden exemplaren van een Inspector in meerdere kolommen. Bijvoorbeeld, kunnen alle numerieke kolommen histogrammen hebben op hetzelfde moment.

Inspectors worden weergegeven in de profilering ook aan de onderkant van het scherm.  Maximaliseer inspectors om ze groter binnen het belangrijkste gedeelte van de inhoud te bekijken. Het gegevensraster beschouwen als de standaard-inspector. Alle Inspector kan worden uitgebreid naar het gebied met de belangrijkste inhoud. Inspectors binnen de belangrijkste inhoudsgebied geminimaliseerd tot de profilering goed. Een Inspector aanpassen door te klikken op het potloodpictogram in de Inspector. Opnieuw wilt rangschikken Inspectors binnen de goed met slepen en neerzetten.

Sommige Inspectors ondersteuning voor een 'Halo'-modus. Deze modus wordt de waarde of de status weergegeven voordat de laatste transformatie is toegepast. De oude waarde grijs wordt weergegeven met de huidige waarde op de voorgrond en ziet u de impact van een transformatie. [Bijlage 4 bevat de huidige lijst van ondersteunde controles](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>De bestemming
 Een doel is waar u write-/ exportservice van de gegevens nadat u deze in een gegevensstroom hebt voorbereid. Een bepaalde gegevensstroom kan meerdere doelen hebben. Elk doel heeft de ervaring van een andere gebruiker toe te staan dat moet worden geconfigureerd. De bestemming verbruikt gegevens in een indeling "Vierkant" / in tabelvorm en schrijft deze naar een locatie in een opgegeven indeling. [Bijlage 5 bevat de huidige lijst van ondersteunde bestemmingen](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Met behulp van gegevens voor te bereiden ###
Voorbereiden van gegevens wordt ervan uitgegaan dat een eenvoudige vijf stappen methodologie/benadering voor het voorbereiden van gegevens.

#### <a name="step-1-ingestion"></a>Stap 1: opname ####
Gegevens importeren voor het voorbereiden van gegevens met behulp van de **gegevensbron toevoegen** -optie in de project-weergave.  Alle initiële opnemen van gegevens wordt verwerkt door de Wizard gegevensbron.

#### <a name="step-2-understandprofile-the-data"></a>Stap 2: Inzicht in of het profiel de gegevens ####

Kijk eerst in de Data Quality Bar aan de bovenkant van elke kolom. Groen geeft de rijen met waarden. Grijze vertegenwoordigt rijen met een ontbrekende waarde, null, enzovoort. Groen geeft aan foutwaarden. Beweeg de muisaanwijzer over de balk knopinfo met het exacte aantal rijen in elk van de drie buckets ophalen. De Data Quality Bar gebruikt een logaritmische schaal dus altijd de werkelijke getallen om op te halen van een ruwe uiterlijk voor het volume van de ontbrekende gegevens controleren.

![Kolommen](media/data-prep-getting-started/columns.png)

Gebruik vervolgens een combinatie van andere Inspectors plus het raster gegevenskenmerk beter te begrijpen.  Start formuleren hypothesen over het voorbereiden van gegevens vereist zijn voor verdere analyse. De meeste inspectors werken op één kolom of een klein aantal kolommen.  

Is het waarschijnlijk dat verschillende Inspectors over meerdere kolommen nodig zijn om te begrijpen van de gegevens. U kunt verschillende Inspectors in de profilering en schuiven. In de bron, kunt u ook Inspectors verplaatsen naar de kop van de lijst om te zien in het gebied direct kan worden weergegeven.

![Inspectors](media/data-prep-getting-started/inspectors.PNG)

Verschillende controles zijn beschikbaar voor continue vs categorische variabelen/kolommen. Het menu Inspector Hiermee en opties, afhankelijk van het type variabelen/kolommen die u hebt uitgeschakeld.

Als u werkt met grote gegevenssets met veel kolommen, wordt een pragmatische benadering van het werken met subsets aangeraden. Deze benadering omvat richten op een klein aantal kolommen (bijvoorbeeld 5-10), deze voorbereiden en vervolgens werken via de overige kolommen. De inspector grid biedt ondersteuning voor verticale partitionering van kolommen en zijn er dus als u meer dan 300 kolommen hebt, moet u "page" behandelen.
 

#### <a name="step-3-transform-the-data"></a>Stap 3: De gegevens te transformeren ####
Transformaties wijzigen van de gegevens en de uitvoering van de gegevens voor de ondersteuning van de huidige werkmap hypothese toestaan. Transformaties weergegeven als de stappen in de lijst met stap aan de rechterkant. Het is mogelijk te 'tijd reizen' in de lijst met stap door te klikken op elk willekeurig moment in de lijst met stap.

Een groen pictogram aan de linkerkant van een bepaalde stap geeft aan dat deze is uitgevoerd en de gegevens is inclusief de uitvoering van de transformatie. Een verticale balk aan de linkerkant van de stap geeft de huidige status van de gegevens in de controles.

![stappen](media/data-prep-getting-started/steps.PNG)

Probeer kleine regelmatige wijzigingen aanbrengen in de gegevens en als u wilt valideren (stap 4) na elke wijziging de hypothese zich verder ontwikkelt.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>Stap 4: Controleer of de impact van de transformatie. 
Bepaal of de hypothese juist is. Als dit correct is, de volgende hypothese ontwikkelen en Herhaal stap 2-3 voor het nieuwe knooppunt. Als er fouten zijn, klikt u vervolgens ongedaan maken van de laatste transformatie en ontwikkelen van een nieuwe hypothese en Herhaal stap 2-3.

De belangrijkste manier om te bepalen als de transformatie had de juiste effect is het gebruik van de controles. Bestaande gebruiken. Gebruik Inspectors met het Halo-effect ingeschakeld of meerdere Inspectors als u wilt weergeven van de gegevens op te starten krijgt punten in de tijd.

![Halo inspector](media/data-prep-getting-started/halo1.PNG) ![Halo inspector](media/data-prep-getting-started/halo2.PNG)

Als u wilt een transformatie ongedaan kunt maken, gaat u de lijst met stappen aan de rechterkant van de gebruikersinterface. (Het deelvenster van de lijst met stappen mogelijk moet worden nu weer uit. Als u wilt openen, klikt u op de dubbele punthaak die verwijst naar links). Selecteer in het deelvenster de transformatie die is uitgevoerd die u wilt ongedaan maken. Selecteer de vervolgkeuzelijst aan de rechterkant van de UI-blok. Selecteer een **bewerken** aanbrengen of **verwijderen** de transformatie verwijderen uit de lijst met stappen en de gegevensstroom.

#### <a name="step-5-output"></a>Stap 5: uitvoer 
Wanneer u klaar bent met uw gegevens voor te bereiden, kunt u de gegevensstroom naar uitvoer schrijven. Een gegevensstroom kan veel uitvoer hebben. U kunt in het menu transformaties selecteren die u wilt dat de gegevensset worden geschreven als uitvoer. U kunt ook de uitvoer van de bestemming selecteren. 

## <a name="list-of-appendices"></a>Lijst met bijlagen 
[Bijlage 2 - ondersteunde gegevensbronnen](data-prep-appendix2-supported-data-sources.md)  
[Bijlage 3: ondersteunde transformaties](data-prep-appendix3-supported-transforms.md)  
[Bijlage 4 - ondersteunde controles](data-prep-appendix4-supported-inspectors.md)  
[Bijlage 5: ondersteunde bestemmingen](data-prep-appendix5-supported-destinations.md)  
[Bijlage 6 - voorbeeld van filterexpressies in Python](data-prep-appendix6-sample-filter-expressions-python.md)  
[Bijlage 7 - voorbeeld transformeren gegevensstroom expressies in Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Bijlage 8 - voorbeeld-gegevensbronnen in Python](data-prep-appendix8-sample-source-connections-python.md)  
[Bijlage 9 - voorbeeld doelverbindingen in Python](data-prep-appendix9-sample-destination-connections-python.md)  
[Bijlage 10 - voorbeeld-kolom voor gegevensstromen in Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  

## <a name="see-also"></a>Zie ook

[Zelfstudie geavanceerde gegevensvoorbereiding](tutorial-bikeshare-dataprep.md)