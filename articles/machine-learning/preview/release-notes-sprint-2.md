---
title: Azure ML-Workbench release-opmerkingen voor sprint 2 December 2017
description: In dit document worden de updates voor de sprint 2-release van Azure ML
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 6fede727a36b0f3243975bf23df2ef53baf1b15c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="sprint-2---december-2017"></a>Sprint 2 - december 2017 

#### <a name="version-number-01171115263"></a>Versienummer: 0.1.1711.15263

>Dit is hoe u kunt [Zoek het versienummer](https://docs.microsoft.com/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Welkom bij de derde update van Azure Machine Learning-Workbench. Deze update bevat verbeteringen in de workbench-app, de opdrachtregelinterface (CLI) en de back-end-services. Hartelijk dank voor het verzenden van Messenger en frowns. Veel van de volgende updates worden gedaan als directe resultaten van uw feedback. 

## <a name="notable-new-features"></a>Belangrijke nieuwe functies
- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](https://docs.microsoft.com/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 
- [Grondige kennis op Spark met MMLSpark met GPU-ondersteuning](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Alle AML containers zijn compatibel met Azure IoT randapparaten wanneer geïmplementeerd (geen extra stappen vereist)](http://aka.ms/aml-iot-edge-blog)
- Geregistreerd model lijst en details weergaven beschikbaar Azure-portal
- Toegang tot compute-doelen met SSH-sleutel gebaseerde verificatie naast gebruikersnaam/wachtwoord gebaseerde toegang. 
- Nieuwe patroon frequentie Inspector in de gegevens voorbereiden ervaring. 

## <a name="detailed-updates"></a>Gedetailleerde Updates
Hier volgt een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

### <a name="installer"></a>Installatieprogramma
- Installatieprogramma kan self update zodat die fouten oplossingen en nieuwe functies kunnen worden ondersteund zonder dat gebruiker opnieuw te installeren

### <a name="workbench-authentication"></a>Workbench verificatie
- Meerdere oplossingen voor verificatiesysteem. Laat ons weten als u nog steeds problemen met aanmelden ondervinden.
- UI-wijzigingen die makkelijker te vinden de Manager van de Proxy-instellingen.

### <a name="workbench"></a>Workbench
- Weergave van alleen-lezen bestand heeft nu lichte blauwe achtergrond
- Verplaatste knop Bewerken rechts detecteerbaar meer.
- 'dsource', 'dprep' en 'ipynb' bestandsindelingen kunnen nu worden gerenderd onbewerkte tekst zonder opmaak
- De workbench heeft nu een nieuwe bewerken ervaring dat de gebruiker naar het gebruik van externe IDE voor scripts bewerken en Workbench alleen gebruiken voor het bewerken van de bestandstypen die u een rijke bewerken-ervaring (zoals laptops, gegevensbronnen, voorbereiding gegevenspakketten hebt)
- Bij het laden van de lijst met werkruimten en projecten waaraan de gebruiker toegang tot heeft is aanzienlijk sneller nu

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Een patroon frequentie Inspector om weer te geven van de tekenreeks patronen in een kolom. U kunt ook uw gegevens met behulp van deze patronen filteren. Hiermee geeft u een weergave vergelijkbaar met de waarde telt inspector. Het verschil is dat patroon frequentie het aantal van de unieke patronen van de gegevens in plaats van het aantal unieke gegevens toont. U kunt ook filteren in- of alle rijen die voldoen aan een bepaald patroon.

![Afbeelding van patroon frequentie inspector op nummer Product](media/release-notes-sprint-2/pattern-inspector-product-number.png)

- Verbeterde prestaties bij het aanbevelen van rand aanvragen om te bekijken in de transformatie 'kolom afgeleid door voorbeeld'

- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](https://docs.microsoft.com/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 

![Afbeelding van het maken van een nieuwe SQL server-gegevensbron](media/release-notes-sprint-2/sql-server-data-source.png)

- 'In een oogopslag'-weergave van het aantal rijen en kolommen ingeschakeld

![Afbeelding van de kolomaantal rijen in een respijtperiode](media/release-notes-sprint-2/row-col-count.png)

- Gegevens prep is ingeschakeld in alle compute-contexten
- Gegevensbronnen die gebruikmaken van een SQL Server-database zijn ingeschakeld in alle compute-contexten
- Gegevens voorbereiden raster kolommen kunnen worden gefilterd door het gegevenstype
- Opgelost probleem met het converteren van meerdere kolommen tot datum
- Probleem opgelost die gebruiker kan uitvoerkolom selecteren als een bron in kolom afgeleid door voorbeeld als gebruiker gewijzigd uitvoerkolomnaam in de geavanceerde modus.

### <a name="job-execution"></a>Uitvoeren van taak
U kunt nu maken en toegang tot een remotedocker of cluster compute-doel SSH-sleutel verificatie op basis van deze stappen te volgen:
- Een compute-doel met de volgende opdracht in de CLI koppelen

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
[!NOTE] -k optie (of---azureml-ssh-sleutel gebruiken) in de opdracht geeft om te genereren en gebruiken van SSH-sleutel.

- Azure ML-Workbench gaat genereren van een openbare sleutel en uitvoer die in de console. Meld u aan bij de compute-doelserver met behulp van de dezelfde gebruikersnaam en ~/.ssh/authorized_keys toevoegen aan deze openbare sleutel.

- U kunt dit doel compute voorbereiden en deze gebruiken voor uitvoering en Azure ML-Workbench gebruikt deze sleutel voor verificatie.  

Zie voor meer informatie over het maken van de compute-doelen [configureren van Azure Machine Learning experimenteren Service](https://docs.microsoft.com/azure/machine-learning/preview/experimentation-service-configuration)

### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools voor AI
- Ondersteuning toegevoegd voor [Visual Studio Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Opdrachtregelinterface (CLI)
- Toegevoegd `az ml datasource create` kan naar het bestand voor een gegevensbron maken vanaf de opdrachtregel

### <a name="model-management-and-operationalization"></a>Model Management en uitoefening
- [Alle AML containers zijn compatibel met Azure IoT randapparaten wanneer geoperationaliseerd (geen extra stappen vereist)](http://aka.ms/aml-iot-edge-blog) 
- Verbeteringen van foutberichten in de CLI o16n
- Oplossingen voor problemen in het beheerportal model UX  
- Consistente letter hoofdlettergebruik voor model management kenmerken in de detailpagina
- Score berekenen aanroepen time-out realtime ingesteld op 60 seconden
- Geregistreerd model lijst en details weergaven die beschikbaar zijn in de Azure portal

![details van het model in de portal](media/release-notes-sprint-2/model-list.jpg)

![overzicht van het model in de portal](media/release-notes-sprint-2/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Deep Learning in Spark met [GPU-ondersteuning](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Ondersteuning voor Resource Manager-sjablonen voor eenvoudig resources implementeren
- Ondersteuning voor het ecosysteem SparklyR
- [AZTK-integratie](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Voorbeeldprojecten
- [IRIS](https://github.com/Azure/MachineLearningSamples-Iris) en [MMLSpark](https://github.com/Azure/mmlspark) voorbeelden bijgewerkt met de nieuwe versie van Azure ML-SDK

## <a name="breaking-changes"></a>BREKEN WIJZIGINGEN
- Gepromoveerd de `--type` in switch `az ml computetarget attach` aan een onderliggende opdracht. 

    - `az ml computetarget attach --type remotedocker`is nu`az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster`is nu`az ml computetarget attach cluster`
