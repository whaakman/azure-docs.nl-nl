---
title: Aan de slag met gegevens voorbereiden voor Azure Machine Learning | Microsoft Docs
description: Dit is de introductiehandleiding voor de prep gegevenssectie van AML workbench
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 8f90bcb83233b1a2f1a5d342ee444e4b02d4927d
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="getting-started-with-data-preparation"></a>Aan de slag met gegevens voorbereiden

Welkom bij de gegevens voorbereiding gids aan de slag. 

Voorbereiden van gegevens biedt een set hulpprogramma's voor efficiënt verkennen, begrijpen en oplossen van problemen in de gegevens. Hiermee kunt u gebruiken voor gegevens in vele vormen en die gegevens transformeren naar gereinigd gegevens die beter voor downstream-gebruik geschikt is.

Voorbereiden van gegevens is geïnstalleerd als onderdeel van de ervaring van de Azure Machine Learning-Workbench.  Voorbereiden van gegevens lokaal gebruiken of implementeren op een doelcluster en in de cloud als een omgeving runtime of -uitvoering.

De tijd ontwerp runtime Python gebruikt voor uitbreidbaarheid en is afhankelijk van verschillende Python-bibliotheken zoals Pandas. Zoals met andere onderdelen van Azure ML-Workbench niet nodig om Python te installeren is, is dit voor u geïnstalleerd. Maar als u wilt installeren, aanvullende bibliotheken deze bibliotheken moeten worden geïnstalleerd in de Azure ML-Workbench Python-map niet uw gebruikelijke Python-directory. Meer informatie over het installeren van pakketten vindt [hier](data-prep-python-extensibility-overview.md).

Een project is vereist voordat u gegevens Prep, kunt zodra dat project is gemaakt dat kunt u gegevens voorbereiden. 

Navigeer naar het gedeelte met gegevens van het project door het selecteren van het pictogram gegevens ![data source-pictogram](media/data-prep-getting-started/data-source-icon.png) aan de linkerkant van het scherm.  Klik op '+' opnieuw aan bij **toevoegen van een gegevensbron**. De Wizard gegevensbron moet starten en voegt een **gegevensbron** (.dsource)-bestand aan het project na het voltooien van de wizard. De weergave van de gegevens is standaard het raster. Boven het raster is het ook mogelijk om te selecteren van metrische gegevens weergeven. Metrische gegevens in de weergave worden samenvatting statistieken weergegeven.  Nadat de samenvattende statistieken bekeken, klikt u op **voorbereiden** aan de bovenkant van het scherm. [Meer informatie over de Wizard gegevensbron](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Bouwstenen van voorbereiden van gegevens ##
### <a name="the-package"></a>Het pakket ###

Een pakket is de primaire container voor uw werk. Een pakket is de artefacten die wordt opgeslagen naar en van de schijf worden geladen. Tijdens het werken binnen de client het pakket is voortdurend automatisch opgeslagen op de achtergrond. 

Het resultaat van een pakket kunnen worden verkend in Python of via een Jupyter-Notebook.

Een pakket kan worden uitgevoerd op meerdere runtimes waaronder lokale Python, Spark (inclusief in Docker) en HDInsight.

Een pakket bevat een of meer Dataflows die de stappen en transformaties toegepast op de gegevens zijn.

Een pakket mag een ander pakket gebruiken als een gegevensbron (aangeduid als een verwijzing gegevensstroom).

### <a name="the-dataflow"></a>De gegevensstroom ###
Een gegevensstroom heeft een bron en optionele transformeert die door een reeks stappen en optionele bestemmingen worden gerangschikt. Te klikken op een stap uitvoert de bronnen en transformaties voorafgaand aan en inclusief de geselecteerde stap opnieuw.  De getransformeerde gegevens via deze stap wordt weergegeven in het raster. Stappen kunnen worden toegevoegd, verplaatst en verwijderd binnen een gegevensstroom door de lijst van de stap.

De stappenlijst aan de rechterkant van de client kan worden geopend en gesloten om meer ruimte op het scherm.

Meerdere Dataflows kunnen bestaan in de gebruikersinterface tegelijk, elke gegevensstroom wordt weergegeven als een tabblad in de gebruikersinterface.

### <a name="the-source"></a>De bron
Een bron is waar de gegevens vandaan komen en de indeling is in. Een gegevenspakket Prep gegevensbronnen altijd de gegevens uit een andere gegevens Flow(Data Source). Deze verwijzing gegevensstroom met de informatie is. Elke bron heeft een andere gebruikerservaring toe te staan dat moet worden geconfigureerd. De bron produceert een 'rechthoekige' / tabellarische weergave van de gegevens. Als de brongegevens heeft oorspronkelijk 'niet-aaneengesloten rechts', klikt u vervolgens de structuur genormaliseerd als "rechthoekige." [Bijlage 2 bevat de huidige lijst met ondersteunde gegevensbronnen](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>De transformatie ###
Transformaties gebruiken voor gegevens in een bepaalde indeling, sommige bewerking uitvoeren op de gegevens (zoals het wijzigen van het gegevenstype) en vervolgens het produceren van gegevens in de nieuwe indeling. Elke transformatie heeft een eigen gebruikersinterface en behavior(s). Verschillende transformaties samen Chaining via de stappen in de gegevensstroom vormt de kern van de functionaliteit van voorbereiden van gegevens. [Bijlage 3 bevat de huidige lijst van ondersteunde transformaties](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>De inspector ###

Inspectors visualisaties van de gegevens zijn en beter inzicht te krijgen van de gegevens beschikbaar zijn.  Kennis van de gegevens en gegevens problemen kwaliteit kunt die u welke acties (transformaties bepalen) moeten worden genomen. Sommige Inspectors ondersteuning voor acties die genereren van transformaties. Het aantal Inspector kunt u een waarde te selecteren en vervolgens een filter wilt die waarde opnemen of uitsluiten van die waarde toepassen. Inspectors biedt ook context voor transformaties. Bijvoorbeeld een of meer kolommen te selecteren, verandert de mogelijke transformaties die kunnen worden toegepast.

Een kolom mogelijk meerdere Inspectors op elk gewenst moment in de tijd (bijvoorbeeld Kolomstatistieken en een Histogram). Er kan ook worden exemplaren van een Inspector in meerdere kolommen. Alle numerieke kolommen kunnen bijvoorbeeld histogrammen hebben op hetzelfde moment.

Inspectors worden weergegeven in het samenstellen van profiel ook aan de onderkant van het scherm.  Het maximaliseren van inspectors om ze in het hoofdgedeelte van de inhoud groter te bekijken. Het gegevensraster beschouwen als de standaard-inspector. Alle Inspector kan worden uitgebreid naar het hoofdgedeelte van de inhoud. Inspectors binnen het hoofdgedeelte van de inhoud wordt geminimaliseerd tot het samenstellen van profiel goed. Een Inspector aanpassen door te klikken op het potloodpictogram binnen de Inspector. Bestellen Inspectors binnen de ook met behulp van slepen en neerzetten.

Sommige Inspectors ondersteuning voor een 'Halo'-modus. Deze modus wordt de waarde of de status weergegeven voordat de laatste transformatie is toegepast. De oude waarde grijs wordt weergegeven met de huidige waarde op de voorgrond en geeft de invloed van een transformatie. [Bijlage 4 bevat de huidige lijst met ondersteunde Inspectors](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>De bestemming
 Een doel is waar u schrijven/exporteren de gegevens nadat u deze hebt voorbereid in een gegevensstroom. Een opgegeven gegevensstroom kan meerdere doelen hebben. Elk doel heeft een andere gebruikerservaring toe te staan dat moet worden geconfigureerd. De bestemming gegevens in een indeling 'rechthoekige' / in tabelvorm verbruikt en schrijft deze naar een locatie in een bepaalde indeling. [Bijlage 5 bevat de huidige lijst met ondersteunde bestemmingen](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Met behulp van de gegevens voorbereiden ###
Voorbereiden van gegevens wordt ervan uitgegaan dat een basic vijf stap methodologie/benadering van voorbereiden van gegevens.

#### <a name="step-1-ingestion"></a>Stap 1: opname ####
Importeren van gegevens voor het voorbereiden van gegevens met behulp van de **gegevensbron toevoegen** optie binnen de project-weergave.  Alle initiële opname van gegevens wordt verwerkt door de Wizard gegevensbron.

#### <a name="step-2-understandprofile-the-data"></a>Stap 2: Begrijpen of het profiel de gegevens ####

Kijk eerst in de Data Quality-balk aan het begin van elke kolom. Groen geeft de rijen met waarden. Grijs vertegenwoordigt rijen met een ontbrekende waarde, null, enzovoort. Groen geeft aan foutwaarden. Beweeg de muisaanwijzer over de balk knopinfo met het exacte aantal rijen in elk van de drie buckets ophalen. Kwaliteit gegevensbalk gebruikt een logaritmische schaal Controleer dus altijd de werkelijke getallen naar een ruwe idee krijgen voor het volume gegevens ontbreken.

![Kolommen](media/data-prep-getting-started/columns.png)

Gebruik een combinatie van andere Inspectors plus het raster vervolgens gegevenskenmerk beter te begrijpen.  Start formuleren hypothesen over het voorbereiden van gegevens voor verdere analyse is vereist. De meeste inspectors werken op één kolom of een klein aantal kolommen.  

Is het waarschijnlijk dat verschillende Inspectors over meerdere kolommen nodig zijn om te begrijpen van de gegevens. U kunt verschillende Inspectors in de profilering goed schuiven. Binnen de ook kunt u ook Inspectors verplaatsen naar de kop van de lijst om te zien in het onmiddellijk zichtbare gebied.

![Inspectors](media/data-prep-getting-started/inspectors.PNG)

Verschillende controles zijn beschikbaar voor continue tegenover categorische variabelen/kolommen. Het menu Inspector Hiermee en opties, afhankelijk van het type van variabelen/kolommen die u hebt uitgeschakeld.

Als u werkt met grote gegevenssets die veel kolommen hebben, wordt een pragmatic benadering van het werken met subsets aangeraden. Deze aanpak omvat te focussen op een klein aantal kolommen (bijvoorbeeld 5-10), het voorbereiden van deze en vervolgens via de overige kolommen werken. Het raster inspector biedt ondersteuning voor verticale partities van kolommen en kan dus als u meer dan 300 kolommen hebt, moet u "pagina' doorheen.
 

#### <a name="step-3-transform-the-data"></a>Stap 3: De gegevens transformeren ####
Transformaties wijzigen van de gegevens en de uitvoering van de gegevens voor de ondersteuning van de huidige werkende hypothese toestaan. Transformaties weergegeven als de stappen in de lijst stap aan de rechterkant. Kan worden 'tijd reizen' in de lijst met stap door te klikken op elk willekeurig punt in de lijst van de stap.

Een groen pictogram aan de linkerkant van een bepaalde stap geeft aan dat deze is uitgevoerd en de gegevens de uitvoering van de transformatie wordt. Een verticale balk aan de linkerkant van de stap geeft de huidige status van de gegevens in de inspecteurs.

![stappen](media/data-prep-getting-started/steps.PNG)

Probeer kleine regelmatige wijzigingen aanbrengen in de gegevens en als u wilt valideren (stap 4) na elke wijziging de hypothese ontwikkeld.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>Stap 4: Controleer of de impact van de transformatie. 
Bepaal of de hypothese juist is. Als dit correct zijn de volgende hypothese ontwikkelen en Herhaal stappen 2-3 voor de nieuwe knop. Als er fouten zijn, klikt u vervolgens ongedaan maken van de laatste transformatie en ontwikkelen van een nieuwe hypothese en Herhaal stappen 2-3.

De belangrijkste manier om te bepalen of de transformatie de juiste invloed had is het gebruik van de controles. Gebruik bestaande. Gebruik Inspectors met de Halo effect ingeschakeld of meerdere Inspectors om weer te geven van de gegevens op te starten gegeven punten in de tijd.

![Halo inspector](media/data-prep-getting-started/halo1.PNG) ![Halo inspector](media/data-prep-getting-started/halo2.PNG)

Als u wilt een transformatie ongedaan kunt maken, gaat u de lijst met aan de rechterkant van de gebruikersinterface. (Het paneel stappen lijst moet mogelijk worden verschijnt weer uit. Om deze te openen, klikt u op de dubbele punthaak die verwijst naar links). Selecteer in het deelvenster de transformatie die is uitgevoerd die u wilt ongedaan maken. Selecteer de vervolgkeuzelijst aan de rechterkant van de UI-blok. Selecteer een **bewerken** wijzigingen aanbrengen of **verwijderen** de transformatie verwijderen uit de lijst met stappen en de gegevensstroom.

#### <a name="step-5-output"></a>Stap 5: uitvoer 
Als u klaar bent met het voorbereiden van uw gegevens kunt u de gegevensstroom naar uitvoer schrijven. Een gegevensstroom kan veel uitvoer hebben. U kunt in het menu transformaties selecteren die u wilt dat de gegevensset die wordt opgeslagen als uitvoer. U kunt ook de uitvoer doel selecteren. 

## <a name="list-of-appendices"></a>Lijst met bijlagen 
[Bijlage 2 - ondersteunde gegevensbronnen](data-prep-appendix2-supported-data-sources.md)  
[Bijlage 3 - ondersteund transformeert](data-prep-appendix3-supported-transforms.md)  
[Bijlage 4 - ondersteund Inspectors](data-prep-appendix4-supported-inspectors.md)  
[Bijlage 5 - ondersteund bestemmingen](data-prep-appendix5-supported-destinations.md)  
[Bijlage 6 - voorbeeld filterexpressies in Python](data-prep-appendix6-sample-filter-expressions-python.md)  
[Bijlage 7 - voorbeeld transformeren gegevensstroom expressies in Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Bijlage 8 - voorbeeld gegevensbronnen in Python](data-prep-appendix8-sample-source-connections-python.md)  
[Bijlage 9 - voorbeeld bestemming verbindingen in Python](data-prep-appendix9-sample-destination-connections-python.md)  
[Bijlage 10 - voorbeeld kolom transformaties in Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  

## <a name="see-also"></a>Zie ook

[Geavanceerde voorbereiding-zelfstudie](tutorial-bikeshare-dataprep.md)