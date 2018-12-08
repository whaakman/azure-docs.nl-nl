---
title: Migreren van Workbench
titleSuffix: Azure Machine Learning service
description: Informatie over het upgraden of migreren naar de latere versie van Azure Machine Learning-service van een eerdere versie.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: haining
author: haining
ms.date: 09/24/2018
ms.openlocfilehash: cc60fd6a9d5f154d26fc9c495f190296453a0db0
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106648"
---
# <a name="migrate-from-workbench-to-the-latest-version-of-azure-machine-learning-service"></a>Migreren van de Workbench naar de nieuwste versie van Azure Machine Learning-service 

**Als u de Workbench-toepassing hebt geïnstalleerd en/of hebben experimenteermogelijkheden en Modelbeheer accounts bekijken, in dit artikel gebruiken om te migreren naar de nieuwste versie.**  Als u geen preview Workbench zijn geïnstalleerd, of een experimenten en/of Modelbeheer-account hebt, moet u niet verder niets te migreren.

## <a name="what-can-i-migrate"></a>Wat kan ik migreren?
De meeste artefacten die zijn gemaakt in de eerste preview van Azure Machine Learning-service worden opgeslagen in uw eigen lokale of cloudopslag. Deze artefacten wordt niet verwijderd. Als u wilt migreren, registreert u de artefacten opnieuw met de bijgewerkte Azure Machine Learning-service. 

De volgende tabel en het artikel wordt uitgelegd wat u kunt doen met uw bestaande assets en resources vóór of na het verplaatsen van boven naar de nieuwste versie van Azure Machine Learning-service. U kunt ook echter ook doorgaan met de vorige versie en uw activa voor enige tijd ([Zie overgang ondersteuningstijdlijn](overview-what-happened-to-workbench.md#timeline)).

|Asset- of -resource in de oude versie|Kan ik migreren?|Acties|
|-----------------|:-------------:|-------------|
|Machine learning-modellen (zoals lokale bestanden)|Ja|Geen. Werkt net als voorheen.|
|Afhankelijkheden van modellen en schema's (zoals lokale bestanden)|Ja|Geen. Werkt net als voorheen.|
|Projecten|Ja|[De lokale map toevoegen aan nieuwe werkruimte](#projects).|
|Uitvoeringsgeschiedenis|Nee|[Downloadbare](#history) even.|
|Prep gegevensbestanden|Nee|[Voorbereiden van de gegevensset van elke grootte](#dataprep) voor modellen met behulp van de nieuwe Azure Machine Learning Data Prep SDK of gebruik van Azure Databricks.|
|COMPUTE-doelen|Nee|Deze registreren in de nieuwe werkruimte.|
|Geregistreerde modellen|Nee|Het model onder een nieuwe werkruimte opnieuw te registreren.|
|Geregistreerde manifesten|Nee|Geen. Manifesten bestaat niet meer als een concept in de nieuwe werkruimte.|
|Geregistreerde installatiekopieën|Nee|De implementatie van Docker-installatiekopie onder een nieuwe werkruimte opnieuw worden gemaakt.|
|Geïmplementeerde webservices|Nee|Geen. Ze nog steeds als gaat gebruiken-is <br/>of [implementeren met behulp van het meest recente versie](#services).|
|Experimenten en <br/>Modelbeheer-accounts|Nee|[Een werkruimte maken](#resources) in plaats daarvan.|
|Machine learning CLI & SDK|Nee|Gebruik de nieuwe [CLI](reference-azure-machine-learning-cli.md) en [SDK](https://aka.ms/aml-sdk) voor nieuwe werk.|


Meer informatie over [wat gewijzigd in deze release](overview-what-happened-to-workbench.md)?

>[!Warning]
>In dit artikel is niet beschikbaar voor gebruikers van Azure Machine Learning Studio. Het is voor Azure Machine Learning-service-Klanten die de toepassing Workbench (preview) hebt geïnstalleerd en/of experimenten en Modelbeheer-preview accounts hebben.

<a name="resources"></a>

## <a name="azure-resources"></a>Azure-resources

Resources, zoals uw experimenten-accounts, Modelbeheer-accounts en machine learning compute-omgeving kunnen niet worden gemigreerd naar de nieuwste versie van Azure Machine Learning-service. Zie de [tijdlijn](overview-what-happened-to-workbench.md#timeline) op hoe lang de activa blijven werken.

Aan de slag met de meest recente versie van het maken van een werkruimte van Azure Machine Learning-service in de [Azure-portal](quickstart-get-started.md). Het werkruimtedashboard van de portaal wordt alleen ondersteund in Edge, Chrome en Firefox.

Deze nieuwe werkruimte is de Serviceresource op het hoogste niveau en kunt u alle van de nieuwste functies van Azure Machine Learning-service gebruiken. Meer informatie over dit [werkruimte en architectuur](concept-azure-machine-learning-architecture.md).

<a name="projects"></a>

## <a name="projects"></a>Projecten

In plaats van dat uw projecten in een werkruimte in de cloud, projecten, zijn nu mappen op uw lokale computer in de nieuwste versie. Zie een diagram van de [meest recente architectuur](concept-azure-machine-learning-architecture.md). 

Om door te gaan met behulp van de lokale directory met uw bestanden en scripts, geef de naam van de map in de ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) Python-opdracht, of met behulp van de CLI-opdracht 'az ml project koppelen'.

Bijvoorbeeld:
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

<a name="services"></a>

## <a name="deployed-web-services"></a>Geïmplementeerde webservices

Opnieuw implementeren uw modellen met behulp van de nieuwe SDK of CLI naar de nieuwe implementatiedoelen voor het migreren van webservices. Er is niet nodig om uw oorspronkelijke scoring-bestand, modelbestanden bestand afhankelijkheden, omgevingsbestand en schema-bestanden te wijzigen. 

In de nieuwste versie, worden modellen als webservices met Azure Container Instances (ACI) of Azure Kubernetes Service (AKS)-clusters geïmplementeerd. 

Meer informatie in deze artikelen:
+ [Over het implementeren en waar](how-to-deploy-and-where.md)
+ [Zelfstudie: Implementeer modellen met Azure Machine Learning-service](tutorial-deploy-models-with-aml.md)

Wanneer [ondersteuning voor de eerdere CLI](overview-what-happened-to-workbench.md#timeline), kunt u zich niet voor het beheren van de webservices die u oorspronkelijk hebt geïmplementeerd met uw Modelbeheer-account. Deze webservices blijven echter werken voor, zolang het Azure Container Service (ACS) wordt nog steeds ondersteund.

<a name="history"></a>

## <a name="run-history-records"></a>Geschiedenisrecords uitvoeren

Terwijl u kan niet worden voortgezet om toe te voegen aan uw bestaande uitvoeringsgeschiedenis onder de oude werkruimte, kunt u de geschiedenis van hebt u met behulp van de eerdere CLI kunt exporteren. Wanneer [ondersteuning voor de eerdere CLI](overview-what-happened-to-workbench.md#timeline), kunt u zich niet meer deze uitvoeringsgeschiedenis exporteren.

Start uw modellen trainen en de uitvoeringsgeschiedenis met behulp van de nieuwe SDK en CLI bij te houden. U kunt meer te weten hoe u met de [zelfstudie: trainen van modellen met Azure Machine Learning-service](tutorial-train-models-with-aml.md).

Voor het exporteren van de uitvoeringsgeschiedenis met eerdere CLI:

```azurecli
#list all runs
az ml history list

#get details about a particular run
az ml history info

# download all artifacts of a run
az ml history download
```

<a name="dataprep"></a>

## <a name="data-preparation-files"></a>Gegevensbestanden voorbereiden
Gegevens voorbereiden bestanden zijn niet overdraagbaar zonder de Workbench. Maar u kunt nog steeds een grootte van de gegevensset voorbereiden voor het modelleren met behulp van de nieuwe Azure Machine Learning Data Prep SDK of Azure Databricks gebruiken voor big data-sets.  [Informatie over het verkrijgen van de SDK voor gegevensvoorbereiding](how-to-data-prep.md). 

## <a name="next-steps"></a>Volgende stappen

Voor een snelstart u voor het maken van een werkruimte, een project maken, een script uitvoeren en de uitvoeringsgeschiedenis van het script met de meest recente versie van Azure Machine Learning-service te verkennen, kunt u proberen [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

Voor een uitgebreidere ervaring met deze werkstroom, volgt u de volledige zelfstudie met gedetailleerde stappen voor training en implementeren van modellen met Azure Machine Learning-service. 

> [!div class="nextstepaction"]
> [Zelfstudie: Trainen en modellen implementeren](tutorial-train-models-with-aml.md)
