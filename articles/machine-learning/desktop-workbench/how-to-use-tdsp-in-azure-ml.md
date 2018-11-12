---
title: Structuur van projecten met de sjabloon voor Team Data Science Process | Microsoft Docs
description: Het instantiëren van sjablonen voor Team Data Science Process (TDSP) in Azure Machine Learning die structuur voor samenwerking projecten
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 8e13ba0bcbd24dcc3cd08a0d2886f1da9d85de79
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249911"
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Projecten structureren met de sjabloon voor Team Data Science Process

Dit document bevat instructies voor het maken van data science-projecten in een eerdere versie van Azure Machine Learning met behulp van sjablonen voor Team Data Science Process (TDSP). Deze sjablonen helpen aan projecten voor samenwerking en reproduceerbaarheid structuur. 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

## <a name="what-is-the-team-data-science-process"></a>Wat is Team Data Science Process?
De TDSP is een flexibele, iteratieve, data science-proces voor het uitvoeren van en het leveren van geavanceerde analyseoplossingen. Het is ontworpen voor het verbeteren van de samenwerking en efficiëntie van data-scienceteams in enterprise-organisaties. Deze biedt ondersteuning voor deze doelstellingen met vier belangrijke onderdelen:

   * Een standaard [levenscyclus van wetenschappelijke gegevens](../team-data-science-process/lifecycle.md) definitie.
   * Een gestandaardiseerde projectstructuur [project documentatie en de rapportage van sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Een infrastructuur en bronnen voor het project wordt uitgevoerd, zoals respectievelijk een reken- en infrastructuur en code-opslagplaatsen.
   * [Hulpprogramma's](https://github.com/Azure/Azure-TDSP-Utilities) voor datatechnologie project taken, zoals:
      - Gezamenlijke versiebeheer
      - Code-revisie
      - Gegevens verkennen en modelleren
      - Werk plannen

Zie voor een uitgebreidere bespreking van de TDSP, de [Team Data Science Process overzicht](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>Waarom zou u de TDSP-structuur en -sjablonen gebruiken?
Ordening van de structuur, de levenscyclus en de documentatie van data science-projecten is essentieel voor effectieve samenwerking voor teams van data science te vergemakkelijken. Machine learning-projecten maken met de TDSP-sjabloon om dergelijke een framework voor gecoördineerde teamwerk.

We eerder uitgebrachte een [GitHub-opslagplaats voor de TDSP-projectstructuur en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) om u te helpen deze doelstellingen te bereiken. Maar het is niet mogelijk, tot nu toe voor het starten van de TDSP-structuur en sjablonen in een data science-hulpprogramma. Het is nu mogelijk te maken van een machine learning-project waarmee een instantie van de TDSP-structuur en de documentatie van sjablonen. 

## <a name="things-to-note-before-creating-a-new-project"></a>Let voordat u een nieuw project maken
Bekijk de volgende items *voordat* u een nieuw project maken:
* Bekijk de TDSP Machine Learning [sjabloon](https://aka.ms/tdspamlgithubrepo).
* De inhoud (met uitzondering van de wat is al aanwezig in de map 'docs') moet kleiner zijn dan 25 MB in grootte. Zie de opmerking die volgt op deze lijst.
* Het voorbeeld\_gegevensmap is uitsluitend bedoeld voor kleine gegevensbestanden (minder dan 5 MB) waarmee u kunt testen van uw code of beginnen met vroege ontwikkelen.
* Opslaan van bestanden, zoals Word en PowerPoint-bestanden, kunt de aanzienlijk verhogen voor de grootte van de map 'docs'. We adviseren dat u een gezamenlijke Wiki [SharePoint](https://products.office.com/sharepoint/collaboration), of een andere gezamenlijke bron voor het opslaan van deze bestanden.
* Lees voor informatie over het verwerken van grote bestanden en de uitvoer in Machine Learning, [persistent maken van wijzigingen en het werken met grote bestanden](https://aka.ms/aml-largefiles).

> [!NOTE]
> Alle documentatie met betrekking tot inhoud (tekst, promoties, afbeeldingen en andere bestanden) die is *niet* gebruikt tijdens het uitvoeren van het project, dan het readme.md-bestand moet zich bevinden in de map met de naam 'docs' (zonder hoofdletters). De map 'docs' is een speciale map genegeerd door de uitvoering van Machine Learning, zodat de inhoud in deze map om onnodig compute-doelen niet gekopieerd. Objecten in deze map ook tellen niet mee voor het maximum van 25 MB voor de projectgrootte van het. De map 'docs', bijvoorbeeld, is de plaats voor het opslaan van grote afbeelding-bestanden die nodig zijn in de documentatie. Deze bestanden worden nog steeds door Git bijgehouden via de uitvoeringsgeschiedenis. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>Exemplaar maken van de TDSP-structuur en de sjablonen van de Machine Learning-galerie met sjablonen
Voer de volgende procedures voor het maken van een nieuw project met de TDSP-structuur en de documentatie van sjablonen.

### <a name="create-a-new-project"></a>Een nieuw project maken
Open Azure Machine Learning voor het maken van een nieuw project. Onder **projecten** linksboven in het deelvenster, selecteer het plusteken (**+**), en selecteer vervolgens **nieuw Project**.

![Nieuw project](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>Een nieuw gestructureerde TDSP-project maken
   1. Geef de parameters en de informatie in het relevante selectievakje of de lijst:

      - Projectnaam
      - Projectmap
      - Projectbeschrijving
      - Een leeg pad van de Git-opslagplaats
      - Naam van de werkruimte

   2. Klik in de **zoeken** Voer **TDSP**. 
   3. Wanneer de **een project structureren met TDSP** optie wordt weergegeven, selecteert u die sjabloon. 
   4. Selecteer de **maken** om te maken van het nieuwe project met een TDSP-structuur. Als u een lege Git-opslagplaats opgeeft bij het maken van het project (in het desbetreffende tekstvak), wordt vervolgens die opslagplaats gevuld met de projectstructuur en inhoud na het maken van het project.

![Een TDSP-project maken](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>Bekijk de structuur van de TDSP-project
Nadat het nieuwe project is gemaakt, kunt u de structuur bekijken (Zie het linkerdeelvenster in de volgende afbeelding). Deze bevat alle aspecten van gestandaardiseerde documentatie voor inzicht in het bedrijf. Deze items zijn de fasen van de TDSP-levenscyclus, gegevenslocatie, definities en de architectuur van deze sjabloon documentatie. 

De structuur die wordt weergegeven is afgeleid van de TDSP-structuur die is gepubliceerd in [TDSP-structuur, documenten en artefact projectsjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate), met enkele wijzigingen. Bijvoorbeeld, verschillende van de documentsjablonen die worden samengevoegd in één markdown, namelijk [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport). 

### <a name="project-folder-structure"></a>Project-mapstructuur
De sjabloon voor de TDSP-project bevat de volgende mappen op het hoogste niveau:
   - **code**: code bevat.
   - **Docs**: bevat de benodigde documentatie over het project (bijvoorbeeld markdown-bestanden en gerelateerde media).
   - **sample_data**: bevat **voorbeeld (klein)** gegevens die u voor vroege ontwikkeling en testen gebruiken kunt. Deze sets zijn doorgaans niet groter zijn dan enkele (5) MB. Deze map is niet beschikbaar voor volledige of grote gegevenssets.

![Voorbeeldgegevens](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>De TDSP-structuur en sjablonen gebruiken
U moet de project-specifieke informatie toevoegen aan de structuur en sjablonen. U bent verwacht voor het vullen van deze met de code en de informatie die nodig zijn om te worden uitgevoerd en het leveren van uw project. De [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) bestand is een sjabloon die u nodig hebt om te wijzigen met de informatie die relevant zijn voor uw project. Het wordt geleverd met een reeks vragen die u helpen bij Vul de gegevens voor elk van de vier fasen van de [TDSP-levenscyclus](../team-data-science-process/lifecycle.md).

Een voorbeeld van wat een projectstructuur ziet eruit als tijdens de uitvoering of na voltooiing wordt weergegeven in het linkerdeelvenster in de volgende afbeelding. Dit project is afkomstig uit de [voorbeeldproject Team Data Science Process: classificeren van inkomsten in VS-tellinggegevens in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) voorbeeldproject.

![De structuur van de voorbeeld-project](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Documenteer uw project
Raadpleeg de [TDSP documentatiesjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate) voor meer informatie over het vastleggen van uw project. In de huidige sjabloon in de documentatie voor Machine Learning TDSP, wordt aangeraden dat u de informatie in bevatten de [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) bestand. Deze sjabloon moet worden ingevuld met gegevens die specifiek is voor uw project. 

We bieden ook een [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings) sjabloon. U kunt deze sjabloon gebruiken om op te nemen van alle gegevens die niet is opgenomen in het primaire project-document, maar dat is nog steeds nuttig om het document. 

### <a name="example-project-report"></a>Voorbeeld van een projectrapport
U krijgt een [project voorbeeldrapport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Dit projectrapport voor de [VS inkomsten classificatie-voorbeeldproject](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) laat zien hoe u instantiëren en de TDSP-sjabloon gebruiken voor een data science-project.

## <a name="next-steps"></a>Volgende stappen
Uw kennis over het gebruik van de TDSP-structuur en sjablonen in Machine Learning-projecten in het kader, bieden we enkele voorbeelden van voltooide project in de documentatie voor Machine Learning:

- Zie voor een voorbeeld waarin wordt uitgelegd hoe een TDSP-project maken in Machine Learning, [voorbeeldproject Team Data Science Process: classificeren van inkomsten van ons tellinggegevens in Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Zie voor een voorbeeld dat gebruikmaakt van diepteleren in natuurlijke taal verwerken (NLP) in een project TDSP geïnstantieerd in Machine Learning, [biografie-medische entiteit herkenning van natuurlijke taal verwerken met deep learning](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

