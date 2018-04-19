---
title: Structuur van de processjabloon Team gegevens wetenschap-projecten | Microsoft Docs
description: "Het instantiëren van het Team gegevens wetenschap proces (TDSP)-sjablonen in Azure Machine Learning die structuur voor samenwerking projecten"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: bradsev
ms.openlocfilehash: 0f7a6d0a58e2dcfd94e20a81ae12b42f238016c8
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Projecten met een structuur met de sjabloon Team gegevens wetenschappelijke processen

Dit document bevat instructies voor het maken van gegevens wetenschappelijke projecten in Azure Machine Learning met sjablonen Team gegevens wetenschap proces (TDSP). Deze sjablonen helpen de structuur projecten voor samenwerking en reproduceerbaarheid. 


## <a name="what-is-the-team-data-science-process"></a>Wat is Team Data Science Process?
De TDSP is een flexibele, herhalende, gegevens wetenschap-proces voor het uitvoeren van en geavanceerde analytics-oplossingen leveren. Het is ontworpen voor het verbeteren van de samenwerking en efficiëntie van gegevens wetenschappelijke teams binnen ondernemingen. Deze ondersteuning biedt voor deze doelstellingen met vier belangrijke onderdelen:

   * Een standaard [gegevens wetenschappelijke lifecycle](../team-data-science-process/lifecycle.md) definitie.
   * De structuur van een gestandaardiseerde project [project documentatie en de rapportage van sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Een infrastructuur en resources voor de projectuitvoering van het, zoals respectievelijk een berekenings- en infrastructuur en code-opslagplaatsen.
   * [Hulpprogramma's](https://github.com/Azure/Azure-TDSP-Utilities) voor gegevenswetenschap project taken, zoals:
      - Samenwerking versiebeheer
      - Code-revisie
      - Gegevensverkenning en modellering
      - Werk plannen

Zie voor een uitgebreidere bespreking van de TDSP de [Team gegevens wetenschappelijke procesoverzicht](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>Waarom moet u de structuur TDSP en sjablonen gebruiken?
Standaardisatie van de structuur, lifecycle en documentatie van de gegevens wetenschappelijke projecten is de sleutel te vergemakkelijken effectieve samenwerking op gegevens wetenschappelijke teams. Machine learning projecten maken met de sjabloon TDSP voor dergelijke een framework voor gecoördineerde werken als een team.

We eerder is uitgebracht een [GitHub-opslagplaats voor de structuur van TDSP-project en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) om u te helpen deze doelstellingen te bereiken. Maar was het niet mogelijk, tot op heden instantiëren van de structuur TDSP en sjablonen in een wetenschappelijke Gegevenshulpprogramma. Het is nu mogelijk te maken van een machine learning-project dat de TDSP structuur en documentatie sjablonen ServiceHost. 

## <a name="things-to-note-before-creating-a-new-project"></a>Let op voordat u een nieuw project maken
Bekijk de volgende items *voordat* u een nieuw project maakt:
* Bekijk de TDSP Machine Learning [sjabloon](https://aka.ms/tdspamlgithubrepo).
* De inhoud (met uitzondering van de wat is al aanwezig in de map "documenten") zijn moet kleiner zijn dan 25 MB groot vereist. Zie de opmerking na deze lijst.
* Het voorbeeld\_gegevensmap wordt alleen gebruikt voor kleine gegevensbestanden (minder dan 5 MB) waarmee u kunt uw code te testen of vroege ontwikkeling.
* Bestanden, zoals Word en PowerPoint-bestanden worden opgeslagen, kan de aanzienlijk verhogen voor de grootte van de map 'docs'. We adviseren dat u een gezamenlijke Wiki vinden [SharePoint](https://products.office.com/en-us/sharepoint/collaboration), of een andere bron samenwerking dergelijke bestanden op te slaan.
* Lees meer informatie over het verwerken van grote bestanden en de uitvoer in Machine Learning, [behouden blijven van wijzigingen en omgaan met grote bestanden](http://aka.ms/aml-largefiles).

> [!NOTE]
> Alle documentatie-gerelateerde inhoud (tekst, promoties, afbeeldingen en andere documentbestanden) die is *niet* gebruikt tijdens het uitvoeren van het project, anders dan het bestand readme.md moet zich bevinden in de map met de naam "documenten" (alle kleine letters). De map 'docs' is een speciale map genegeerd door de uitvoering van de Machine Learning, zodat de inhoud in deze map niet ophalen gekopieerd om doelen onnodig berekenen. Objecten in deze map ook meetellen niet voor de initiaal 25 MB voor de projectgrootte van het. De map 'docs' is bijvoorbeeld de plaats voor het opslaan van grote afbeelding-bestanden die nodig zijn in de documentatie. Deze bestanden nog steeds worden bijgehouden door Git via de uitvoeringsgeschiedenis. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>Exemplaar maken van de TDSP structuur en sjablonen uit de galerie met Machine Learning
Een nieuw project maakt met de structuur en documentatie sjablonen voor TDSP, voer de volgende procedures.

### <a name="create-a-new-project"></a>Een nieuw project maken
Open Azure Machine Learning voor het maken van een nieuw project. Onder **projecten** selecteren in het deelvenster linksboven het plusteken (**+**), en selecteer vervolgens **nieuw Project**.

![Nieuw project](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>Een nieuw TDSP gestructureerd-project maken
   1. Geef de parameters en de informatie in het relevante selectievakje of de lijst:

      - Projectnaam
      - Projectmap
      - Projectbeschrijving
      - Een leeg pad van de Git-opslagplaats
      - Naam van de werkruimte

   2. Klik in de **Search** Voer **TDSP**. 
   3. Wanneer de **structuur van een project met TDSP** optie wordt weergegeven, selecteert u dat de sjabloon. 
   4. Selecteer de **maken** knop uw nieuw project maken met een TDSP-structuur. Als u een leeg Git-opslagplaats opgeeft wanneer u het project (in de juiste tekstvak) maakt, wordt vervolgens die opslagplaats gevuld met de projectstructuur en inhoud na het maken van het project.

![Een TDSP-project maken](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>De structuur van het project TDSP onderzoeken
Nadat het nieuwe project is gemaakt, kunt u de structuur bestuderen (Zie het linkerdeelvenster in de volgende afbeelding). Deze bevat alle aspecten van gestandaardiseerde documentatie voor bedrijven begrijpen. Deze items zijn de fasen van de levenscyclus van TDSP, locatie van gegevens, definities en de architectuur van deze sjabloon documentatie. 

De structuur weergegeven is afgeleid van de TDSP-structuur die wordt gepubliceerd in [TDSP structuur, documenten en artefact projectsjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate), met een aantal wijzigingen. Bijvoorbeeld, verschillende van de documentsjablonen die worden samengevoegd in één markdown namelijk [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport). 

### <a name="project-folder-structure"></a>Mapstructuur van project
De projectsjabloon TDSP bevat de volgende mappen op het hoogste niveau:
   - **code**: code bevat.
   - **documenten**: bevat documentatie over het project (bijvoorbeeld markdown-bestanden en gerelateerde media).
   - **sample_data**: bevat **voorbeeld (klein)** gegevens die u voor vroege ontwikkeling of tests gebruiken kunt. Deze sets zijn doorgaans niet groter zijn dan enkele (5) MB. Deze map is niet beschikbaar voor volledige of grote gegevenssets.

![Voorbeeldgegevens](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>De structuur TDSP en sjablonen gebruiken
U moet de project-specifieke informatie toevoegen aan de structuur en sjablonen. U bent verwacht voor het vullen met de code en de informatie die nodig zijn uit te voeren en leveren van uw project. De [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) bestand is een sjabloon die u wilt wijzigen met informatie die relevant zijn voor uw project. Wordt geleverd met een reeks vragen die u helpen bij het invullen van de gegevens voor elk van de vier fasen van de [TDSP lifecycle](../team-data-science-process/lifecycle.md).

Een voorbeeld van een projectstructuur ziet er als tijdens de uitvoering of na voltooiing wordt weergegeven in het linkerdeelvenster in de volgende afbeelding. Dit project is afkomstig uit de [voorbeeldproject Team gegevens wetenschappelijke processen: classificeren inkomsten van VS telling gegevens in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) voorbeeldproject.

![De structuur van de voorbeeld-project](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Documenteer uw project
Raadpleeg de [TDSP documentatiesjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) voor meer informatie over het vastleggen van uw project. In de huidige sjabloon in de documentatie van Machine Learning TDSP, raden wij dat u ook de informatie in de [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) bestand. Deze sjabloon moet worden ingevuld met gegevens die specifiek is voor uw project. 

We bieden ook een [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings) sjabloon. Deze sjabloon kunt u geen gegevens die niet is opgenomen in het document primaire project, maar die nog steeds gebruiken om vast te leggen. 

### <a name="example-project-report"></a>Voorbeeld van een projectrapport
U krijgt een [project voorbeeldrapport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Dit projectrapport voor de [VS inkomsten classificatie-voorbeeldproject](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) laat zien hoe instantiëren en de sjabloon TDSP gebruiken voor een wetenschappelijke gegevensproject.

## <a name="next-steps"></a>Volgende stappen
Als u wilt uw kennis over het gebruik van de structuur TDSP en sjablonen in Machine Learning-projecten vergemakkelijken, bieden we enkele voorbeelden van de voltooide project in de documentatie voor Machine Learning:

- Zie voor een steekproef die het maken van een project TDSP in Machine Learning, [voorbeeldproject Team gegevens wetenschappelijke processen: classificeren inkomsten van ons telling gegevens in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Zie voor een steekproef die gebruikmaakt van grondige learning in natuurlijke taal (NLP) worden verwerkt in een project TDSP geïnstantieerd in Machine Learning, [biografie medische entiteit erkenning met behulp van natuurlijke taal verwerken met grondige learning](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

