---
title: PowerShell-module voor Machine Learning | Microsoft Docs
description: De PowerShell-module voor Azure Machine Learning is beschikbaar in de openbare voorbeeldmodus. PowerShell gebruiken om te maken en beheren van werkruimten, experimenten en webservices.
keywords: experiment,lineaire regressie,machine learning-algoritmen,zelfstudie over machine learning,voorspellende modelleringstechnieken,gegevenswetenschapexperiment
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: garye;haining
ms.openlocfilehash: 47d005e584b6cb87d27a77f56ff619d342481aaa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-module voor Microsoft Azure Machine Learning
De PowerShell-module voor Azure Machine Learning is een krachtig hulpprogramma waarmee u Windows PowerShell gebruiken voor het beheren van werkruimten, experimenten gegevenssets, klassieke webservices en meer.

U kunt de documentatie bekijken en de module downloaden, samen met de volledige broncode, op [https://aka.ms/amlps](https://aka.ms/amlps). 

> [!NOTE]
> De Azure Machine Learning PowerShell-module is momenteel in voorbeeldmodus. De module blijven worden verbeterd en uitgebreid in deze preview-periode. Let op de [Cortana Intelligence en Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) voor nieuws en informatie.

## <a name="what-is-the-machine-learning-powershell-module"></a>Wat is de Machine Learning PowerShell-module?
De Machine Learning PowerShell-module is een. Op basis van NET DLL-module die u kunt volledig Azure Machine Learning werkruimten, experimenten gegevenssets, klassieke webservices en klassieke webservice-eindpunten beheren vanuit Windows PowerShell. 

Samen met de module kunt u de volledige broncode foutloos gescheiden waaronder downloaden [C# API-laag](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). U kunt verwijzen naar dit dll-bestand van uw eigen .NET-project en beheren van Azure Machine Learning via .NET-code. Het DLL-bestand is bovendien afhankelijk van onderliggende REST-API's die u rechtstreeks vanuit uw favoriete client kunt gebruiken.

## <a name="what-can-i-do-with-the-powershell-module"></a>Wat kan ik doen met de PowerShell-module?
Hier volgen enkele van de taken die u met deze module PowerShell kunt uitvoeren. Bekijk de [volledige documentatie](https://aka.ms/amlps) voor deze en veel meer functies.

* Een nieuwe werkruimte inrichten met behulp van een beheercertificaat ([New AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Een JSON-bestand dat een experimentgrafiek weergeeft exporteren en importeren ([Export- AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [Import- AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Een experiment uitvoeren ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Een webservice buiten een voorspellend experiment maken ([New AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Maken van een eindpunt op een gepubliceerde webservice ([toevoegen AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Een RRS- en/of BES-webservice-eindpunt aanroepen ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) en [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier volgt een snel voorbeeld van het gebruik van PowerShell om een bestaand experiment uit te voeren:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Zie voor een uitgebreidere gebruiksvoorbeeld in dit artikel over het gebruik van de PowerShell-module voor het automatiseren van een taak vaak aangevraagd: [veel Machine Learning-modellen en web service-eindpunten maken van een experiment met behulp van PowerShell](create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?
Om te beginnen met de Machine Learning PowerShell downloadt u het [releasepakket](https://github.com/hning86/azuremlps/releases) van GitHub en volgt u de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). De instructies wordt uitgelegd hoe blokkering opheffen van de DLL gedownload/uitgepakt en vervolgens importeren in uw PowerShell-omgeving. De meeste cmdlets vereisen dat u de werkruimte-ID, het autorisatietoken van de werkruimte en de Azure-regio waarin de werkruimte zich bevindt opgeeft. De eenvoudigste manier om de waarden opgeven, is via een standaardbestand config.json. De instructies wordt ook uitgelegd hoe u deze bestanden te configureren. 

Als u wilt, kunt u de structuur van git klonen wijzigen van de code en compileert lokaal met behulp van Visual Studio.

## <a name="next-steps"></a>Volgende stappen
U vindt de volledige documentatie voor de PowerShell-module op [https://aka.ms/amlps](https://aka.ms/amlps). 

Bekijk de gedetailleerde gebruiksvoorbeeld voor een uitgebreid voorbeeld van het gebruik van de module in een Praktijkscenario [veel Machine Learning-modellen en web service-eindpunten maken van een experiment met behulp van PowerShell](create-models-and-endpoints-with-powershell.md).
