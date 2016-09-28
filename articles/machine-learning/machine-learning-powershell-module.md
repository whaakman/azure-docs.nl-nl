<properties
    pageTitle="PowerShell-module voor Machine Learning | Microsoft Azure"
    description="De PowerShell-module voor Azure Machine Learning is beschikbaar in de openbare voorbeeldmodus. Gebruik PowerShell om werkruimtes, experimenten, webservices en meer te maken en te beheren."
    keywords="experiment,lineaire regressie,machine learning-algoritmen,zelfstudie over machine learning,voorspellende modelleringstechnieken,gegevenswetenschapexperiment"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>


# PowerShell-module voor Microsoft Azure Machine Learning

De PowerShell-module voor Azure Machine Learning is een krachtig hulpprogramma waarmee u Windows PowerShell gebruikt voor het beheren van werkruimten, experimenten gegevenssets, web serivces en meer.

U kunt de documentatie bekijken en de module downloaden, samen met de volledige broncode, op [https://aka.ms/amlps](https://aka.ms/amlps). 

## Wat is de Machine Learning PowerShell-module?

De Machine Learning PowerShell-module is een .NET-based DLL-module die u de mogelijkheid biedt om de werkruimtes, experimenten, gegevenssets, webservices en webservice-eindpunten van Azure Machine Learning te beheren vanuit Windows PowerShell. Samen met de module kunt u de volledige broncode downloaden, deze omvat een duidelij kgescheiden [C# API-laag](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Dit betekent dat u naar deze DLL kunt verwijzen vanuit uw eigen .NET-project en Azure Machine Learning kunt beheren via .NET-code. Het DLL-bestand is bovendien afhankelijk van onderliggende REST-API's die u van rechtstreeks vanuit uw favoriete client kunt gebruiken.

## Wat kan ik doen met de PowerShell-module?

Hier volgen enkele van de taken die u met deze module PowerShell kunt uitvoeren. Bekijk de [volledige documentatie](https://aka.ms/amlps) voor deze en veel meer functies.

- Een nieuwe werkruimte inrichten met behulp van een beheercertificaat ([New AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Een JSON-bestand dat een experimentgrafiek weergeeft exporteren en importeren ([Export- AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [Import- AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Een experiment uitvoeren ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Een webservice buiten een voorspellend experiment maken ([New AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Een nieuw eindpunt op een gepubliceerde webservice maken ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Een RRS- en/of BES-webservice-eindpunt aanroepen ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) en [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier volgt een snel voorbeeld van het gebruik van PowerShell om een bestaand experiment uit te voeren:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Zie voor een uitgebreidere usecase dit artikel over het gebruik van de PowerShell-module om een zeer vaak gevraagde taak uit te voeren: [Met PowerShell veel Machine Learning-modellen en web service-eindpunten maken vanuit één experiment](machine-learning-create-models-and-endpoints-with-powershell.md).

## Hoe ga ik aan de slag?

Om te beginnen met de Machine Learning PowerShell downloadt u het [releasepakket](https://github.com/hning86/azuremlps/releases) van GitHub en volgt u de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). U moet de gedownloade / uitgepakte DLL deblokkeren en hem dan importeren in uw PowerShell-omgeving. De meeste cmdlets vereisen dat u de werkruimte-ID, het autorisatietoken van de werkruimte en de Azure-regio waarin de werkruimte zich bevindt opgeeft. De eenvoudigste manier om deze op te geven is in een standaard config.json-bestand, dat wordt in de installatie-instructies in detail wordt bespreken. Uiteraard kunt u de boomstructuur ook klonen en de code lokaal wijzigen / samenstellen met Visual Studio.

## Volgende stappen

De PowerShell-module wordt in deze voorbeeldperiode steeds verbeterd en uitgebreid. Houd de [Cortana Intelligence en Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) in de gaten voor meer nieuws en info.



<!--HONumber=Sep16_HO3-->


