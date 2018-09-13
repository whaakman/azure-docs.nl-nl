---
title: Conceptueel overzicht van Azure Machine Learning-previewfuncties | Microsoft Docs
description: Een conceptueel overzicht van de preview-functies van Azure Machine Learning, zoals abonnementen, accounts, werkruimten, projecten, enzovoort.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: f63b9c077e64b642adfd8c7eed5026563eb6319a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643792"
---
# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning - concepten

In dit artikel worden gedefinieerd en beschrijft de concepten die u moet weten om te gebruiken van Azure Machine Learning. 

![Hiërarchie van concepten](media/overview-general-concepts/hierarchy.png)

- **Abonnement:** een Azure-abonnement biedt u toegang tot resources in Azure. Omdat Azure Machine Learning is nauw geïntegreerd met compute, opslag, en vele andere Azure-resources en services, wordt Workbench is vereist dat elke gebruiker toegang tot een geldige Azure-abonnement heeft. Gebruikers moeten ook in dat abonnement voldoende machtigingen om resources te maken hebben.


- **Experimenten-Account:** experimenten-account is een Azure-resource door Azure ML en een facturering voertuig vereist. Het bevat uw werkruimten, dit op zijn beurt projecten bevatten. U kunt meerdere gebruikers, aangeduid als toevoegen _seats_, tot een experimenten-account. U moet toegang hebben tot een experimenten-account om Azure ML Workbench gebruiken voor het uitvoeren van experimenten. 


- **Modelbeheeraccount** een Modelbeheer-account is ook een Azure-resource door Azure ML is vereist voor het beheren van modellen. U kunt het registreren van de manifesten en modellen, beperkte webservices bouwen en deze lokaal of in de cloud te implementeren. Het is ook het facturering voertuig van Azure ML.


- **Werkruimte:** een werkruimte is het primaire onderdeel voor het delen en samenwerking in Azure ML. Projecten worden gegroepeerd in een werkruimte. Een werkruimte kan vervolgens worden gedeeld met meerdere gebruikers die zijn toegevoegd aan het experimenten-account.


- **Het project:** In Azure Machine Learning, een project is een logische container voor al het werk wordt uitgevoerd om een probleem te verhelpen. Het wordt toegewezen aan één map op uw lokale schijf, en u kunt er alle bestanden of submappen aan toevoegen die u wilt. Een project kan eventueel worden gekoppeld aan een Git-opslagplaats voor broncodebeheer en samenwerking.  

- **Experiment:** In Azure ML, een experiment is een of meer code bronbestand(en) die kunnen worden uitgevoerd vanaf een afzonderlijke vermelding punt. Deze kan taken, zoals gegevensopname, feature-engineering, trainen van het model of model evaluatie bevatten. Op dit moment Azure ML biedt ondersteuning voor Python of PySpark alleen experimenten.


- **Model:** modellen In Azure Machine Learning, Raadpleeg het product van een machine learning-experiment. Ze zijn recepten die correct wordt toegepast op gegevens, de voorspelde waarden genereren. Modellen kunnen worden geïmplementeerd voor test- of productieomgeving omgevingen, en gebruikt voor het scoren van nieuwe gegevens. Eenmaal in de productieomgeving, modellen kunnen worden gecontroleerd op prestaties en drift en retrained zoals vereist. 

- **COMPUTE-doel:** een compute-doel is de compute-resource die u configureert voor het uitvoeren van de experimenten nodig hebben. Het kan zijn op uw lokale computer (Windows of Mac OS), een Docker-container die wordt uitgevoerd op uw lokale computer of in een Linux-VM op Azure of een HDInsight Spark-cluster.


- **Uitvoeren:** de experimenten-Service definieert een uitvoeren als de levensduur van de uitvoering van een experiment in een compute-doel. Azure ML automatisch gegevens van elke uitvoering worden vastgelegd en presenteert de volledige geschiedenis van een bepaalde experiment in de vorm van de uitvoeringsgeschiedenis.

- **Omgeving:** In Azure Machine Learning, een omgeving geeft aan een specifieke rekenbron die wordt gebruikt voor het implementeren en beheren van modellen. Kan het zijn uw lokale computer, een Linux-VM op Azure of een Kubernetes-cluster in Azure Container Service wordt uitgevoerd, afhankelijk van de context en de configuratie. Het model is die wordt gehost in een Docker-container die wordt uitgevoerd in deze omgevingen en weergegeven als een REST-API-eindpunt.


- **Beheerde model:** Model Management kunt u modellen als webservices implementeren, verschillende versies van uw modellen beheren en bewaken van de prestaties en metrische gegevens. Beheerde modellen zijn geregistreerd bij een Azure Machine Learning Modelbeheer-account.

- **Manifesten:** wanneer het Model-Management-systeem een model in productie implementeert, bevat een manifest dat model, afhankelijkheden, scoring-script, voorbeeldgegevens en schema kan omvatten. Het manifestbestand is het recept gebruikt voor het maken van een Docker-container-installatiekopie. Met behulp van Modelbeheer, kunt u de manifesten automatisch te genereren, verschillende versies maken en beheren van deze manifesten. 


- **Afbeeldingen:** kunt u manifesten genereren (en opnieuw worden gegenereerd) Docker-installatiekopieën. Beperkte Docker-installatiekopieën maken flexibiliteit om uit te voeren ze in de cloud, op lokale machines of op IoT-apparaat. Installatiekopieën zijn op zichzelf staand en alle afhankelijkheden die zijn vereist voor het scoren van nieuwe gegevens met modellen. 

- **Services:** Model Management kunt u modellen als webservices implementeren. De logica van de webservice en de afhankelijkheden worden ingekapseld in een installatiekopie. Elke webservice is een set van containers op basis van de afbeelding gereed om aanvragen voor een opgegeven URL. Een webservice wordt geteld als één implementatie.
