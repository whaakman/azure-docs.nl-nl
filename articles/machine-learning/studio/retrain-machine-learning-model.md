---
title: Opnieuw trainen van een model voor Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: Informatie over het opnieuw trainen van een model en het bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: e691913daabb832b2a3b51dac5d4a5b0e1f53871
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165069"
---
# <a name="retrain-an-azure-machine-learning-studio-model"></a>Opnieuw trainen van een Azure Machine Learning Studio-model
Uw model is getraind en opgeslagen als onderdeel van het proces voor uitoefening van machine learning-modellen in Azure Machine Learning. U vervolgens worden gebruikt om een voorspellende webservice te maken. De webservice kan vervolgens worden gebruikt in web sites, dashboards en mobiele apps. 

U maakt gebruik van Machine Learning modellen zijn doorgaans niet statisch. Zodra er nieuwe gegevens beschikbaar is of wanneer de gebruiker van de API hun eigen gegevens is moet het model opnieuw worden getraind. 

Opnieuw trainen kan vaak optreden. Met de functie programmatisch opnieuw trainen API kunt u programmatisch opnieuw trainen het model met behulp van de Retraining API's en de webservice bijwerken met het zojuist getrainde model. 

In dit document worden de retraining beschreven en ziet u hoe u kunt de Retraining API's gebruiken.

## <a name="why-retrain-defining-the-problem"></a>Waarom opnieuw trainen: het probleem definiëren
Een model is getraind met behulp van een set gegevens als onderdeel van de machine learning-cursussen proces. U maakt gebruik van Machine Learning modellen zijn doorgaans niet statisch. Zodra er nieuwe gegevens beschikbaar is of wanneer de gebruiker van de API hun eigen gegevens is moet het model opnieuw worden getraind.

In deze scenario's biedt een programmeer-API een handige manier om toe te staan u of de gebruiker van uw API's te maken van een client die u kunt op een eenmalige of regelmatige basis, opnieuw trainen het model met behulp van hun eigen gegevens. Ze kunnen vervolgens evalueren van de resultaten van het opnieuw trainen en bijwerken van de webservice-API voor het gebruik van het zojuist getrainde model.

> [!NOTE]
> Als u een bestaande Trainingsexperiment en een nieuwe webservice hebt, kunt u de Retrain uitchecken van een bestaande voorspellende webservice in plaats van de procedures die worden vermeld in het volgende gedeelte te volgen.
> 
> 

## <a name="end-to-end-workflow"></a>End-to-end werkstroom
Het proces omvat de volgende onderdelen: Een Opleidingsexperiment en een Voorspellend Experiment die zijn gepubliceerd als een webservice. Om in te schakelen opnieuw trainen van een getraind model, moet het Trainingsexperiment worden gepubliceerd als een webservice met de uitvoer van een getraind model. Hierdoor kunnen de API-toegang tot het model voor het opnieuw trainen. 

De volgende stappen gelden voor zowel nieuwe en klassieke webservices:

De eerste voorspellende webservice maken:

* Een opleidingsexperiment maken
* Een web Voorspellend experiment maken
* Een voorspellende webservice implementeren

De webservice opnieuw trainen:

* Update-trainingsexperiment om toe te staan voor het opnieuw trainen
* De retraining webservice implementeren
* Gebruik van de code Batch-Service voor uitvoering van het model trainen

Zie voor een overzicht van de voorgaande stappen, [opnieuw trainen Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

Als u een klassieke webservice geïmplementeerd:

* Een nieuw eindpunt op de voorspellende webservice maken
* De PATCH-URL en de code ophalen
* Gebruik van de PATCH-URL naar het nieuwe eindpunt voor het retrained model 

Als u een klassieke webservice opnieuw trainen problemen ondervindt, raadpleegt u [het oplossen van het opnieuw trainen van een Azure Machine Learning klassieke Web service](troubleshooting-retraining-models.md).

Als u een nieuwe webservice geïmplementeerd:

* Meld u aan bij uw Azure Resource Manager-account
* De definitie van de Web service ophalen
* De definitie van de Web Service exporteren als JSON
* Bijwerken van de verwijzing naar de `ilearner` blob in de JSON
* Het importeren van de JSON in de definitie van een Web-Service
* De webservice bijwerken met nieuwe Web-servicedefinitie

Zie voor een overzicht van de voorgaande stappen, [opnieuw trainen van een nieuwe webservice met behulp van de Machine Learning Management PowerShell-cmdlets](retrain-new-web-service-using-powershell.md).

Het proces voor het instellen van het opnieuw trainen van een klassieke webservice omvat de volgende stappen uit:

![Overzicht van het proces opnieuw trainen][1]

Het proces voor het instellen van het opnieuw trainen van een nieuwe Web-service omvat de volgende stappen uit:

![Overzicht van het proces opnieuw trainen][7]

## <a name="other-resources"></a>Meer informatie
* [Opnieuw trainen en bijwerken van Azure Machine Learning-modellen met Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Veel Machine Learning-modellen en web service-eindpunten maken van een experiment met PowerShell](create-models-and-endpoints-with-powershell.md)
* De [AML Retraining modellen met behulp van API's](https://www.youtube.com/watch?v=wwjglA8xllg) video ziet u hoe u Machine Learning-modellen die zijn gemaakt in Azure Machine Learning te trainen met behulp van de Retraining API's en PowerShell.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

