---
title: Opnieuw een Machine Learning-Model te trainen | Microsoft Docs
description: Informatie over het opnieuw een model te trainen en bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 54655c33336ff3fef261024984947108d6f1df9c
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="retrain-a-machine-learning-model"></a>Opnieuw een Machine Learning-Model te trainen
Het model is getraind en opgeslagen als onderdeel van het proces van uitoefening van machine learning-modellen in Azure Machine Learning. U vervolgens worden gebruikt voor het maken van een predicative webservice. De webservice kan vervolgens worden gebruikt in websites, dashboards en mobiele apps. 

U maakt met behulp van Machine Learning modellen zijn meestal niet statisch. Zodra er nieuwe gegevens beschikbaar, of wanneer de gebruiker van de API hun eigen gegevens heeft moet het model worden retrained. 

Retraining kan vaak optreden. Met de functie programmatische Retraining API kunt u programmatisch opnieuw trainen van het model met behulp van de Retraining API's en de webservice bijwerken met het nieuwe getrainde model. 

Dit document worden de retraining beschreven en ziet u hoe de Retraining API's gebruiken.

## <a name="why-retrain-defining-the-problem"></a>Waarom retrain: het probleem definiëren
Een model wordt getraind met een verzameling van gegevens als onderdeel van de machine learning trainingsproces. U maakt met behulp van Machine Learning modellen zijn meestal niet statisch. Zodra er nieuwe gegevens beschikbaar, of wanneer de gebruiker van de API hun eigen gegevens heeft moet het model worden retrained.

In deze scenario's biedt een programmatische API een handige manier waarmee u of de consument van uw API's voor het maken van een client die u kunt op eenmalige of regelmatige basis retrain het model met hun eigen gegevens. Vervolgens kunnen ze evalueren van de resultaten van de retraining en bijwerken van de Web service-API voor het gebruik van het zojuist getrainde model.

> [!NOTE]
> Hebt u een bestaande Trainingsexperiment en een nieuwe webservice, wilt u mogelijk Retrain uitchecken met een bestaande voorspellende webservice in plaats van na de procedure beschreven in de volgende sectie.
> 
> 

## <a name="end-to-end-workflow"></a>End-to-end werkstroom
Het proces omvat de volgende onderdelen: een Trainingsexperiment en een Voorspellend Experiment gepubliceerd als een webservice. Om in te schakelen van een getraind model retraining, moet het Experiment Training als een webservice met de uitvoer van een getraind model worden gepubliceerd. Hierdoor kan API-toegang in het model voor retraining. 

De volgende stappen van toepassing op zowel nieuwe als klassieke Web services:

De eerste voorspellende webservice maken:

* Een trainingsexperiment maken
* Een web Voorspellend experiment maken
* Een predictive webservice implementeren

De webservice Retrain:

* Trainingsexperiment om toe te staan voor de retraining bijwerken
* De retraining-webservice implementeren
* De Batch-Service voor uitvoering van code gebruiken voor het model opnieuw trainen

Zie voor een overzicht van de voorgaande stappen, [Retrain Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie, [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

Als u een klassieke webservice geïmplementeerd:

* Een nieuw eindpunt op de voorspellende webservice maken
* De PATCH-URL en de code ophalen
* De URL van de PATCH voor het nieuwe eindpunt het retrained model verwijzen gebruiken 

Zie voor een overzicht van de voorgaande stappen, [opnieuw trainen een klassieke webservice](retrain-a-classic-web-service.md).

Als u in een klassieke webservice retraining moeilijkheden uitvoert, Zie [probleemoplossing retraining van een Azure Machine Learning klassieke webservice](troubleshooting-retraining-models.md).

Als u een nieuwe webservice geïmplementeerd:

* Aanmelden bij uw account voor Azure Resource Manager
* Definitie van de webservice ophalen
* Exporteren van de webservicedefinitie als JSON
* Bijwerken van de verwijzing naar de `ilearner` blob in de JSON
* De JSON importeren in de definitie van een Web-Service
* De webservice bijwerken met definitie van een nieuwe Web-Service

Zie voor een overzicht van de voorgaande stappen, [opnieuw trainen van de Machine Learning Management PowerShell-cmdlets met een nieuwe webservice](retrain-new-web-service-using-powershell.md).

Het proces voor het instellen van de retraining voor een klassieke webservice omvat de volgende stappen:

![Overzicht van het proces retraining][1]

Het proces voor het instellen van de retraining voor een nieuwe Web-service omvat de volgende stappen:

![Overzicht van het proces retraining][7]

## <a name="other-resources"></a>Meer informatie
* [Retraining en bijwerken van Azure Machine Learning-modellen met Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Groot aantal Machine Learning-modellen en web service-eindpunten maken van een experiment met behulp van PowerShell](create-models-and-endpoints-with-powershell.md)
* De [AML Retraining modellen met behulp van API's](https://www.youtube.com/watch?v=wwjglA8xllg) video ziet u hoe opnieuw trainen van Machine Learning-modellen in Azure Machine Learning gemaakt met behulp van de Retraining API's en PowerShell.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

