---
title: Conceptueel overzicht van Azure Machine Learning preview-functies | Microsoft Docs
description: Conceptueel overzicht van de preview-functies van Azure Machine Learning, zoals abonnementen, accounts, -werkruimten, projecten, enz.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: ea9da6f23fd08c09f9e805519487648480816f35
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning - concepten

Dit artikel worden gedefinieerd en beschrijft de concepten die u weten moet om het gebruik van Azure Machine Learning. 

![Hiërarchie van de concepten van](media/overview-general-concepts/hierarchy.png)

- **Abonnement:** een Azure-abonnement biedt u toegang tot bronnen in Azure. Omdat Azure Machine Learning is nauw geïntegreerd met compute, storage, en vele andere Azure-resources en services, Workbench is vereist dat elke gebruiker toegang tot een geldige Azure-abonnement heeft. Gebruikers moeten ook voldoende machtigingen in dat abonnement om resources te maken hebben.


- **Account voor experimenteren:** experimenteren-account is een Azure-resource vereist voor Azure ML en een facturering medium. Het bevat uw werkruimten die op zijn beurt projecten bevatten. U kunt meerdere gebruikers, aangeduid als toevoegen _seats_, aan een account experimenteren. U moet toegang hebben tot een account experimenteren om te kunnen gebruiken van Azure ML-Workbench experimenten uitvoeren. 


- **Account van beheerserver model** de account van een model-management is ook een Azure-resource vereist voor Azure ML voor het beheren van modellen. U kunt het registreren van modellen en manifesten, beperkte webservices bouwen en deze lokaal of in de cloud te implementeren. Het is ook de facturering drager van Azure ML.


- **Werkruimte:** een werkruimte is de primaire component voor het delen en samenwerking in Azure ML. Projecten zijn gegroepeerd in een werkruimte. Een werkruimte kan vervolgens worden gedeeld met meerdere gebruikers die zijn toegevoegd aan het experimenteren-account.


- **Project:** In Azure Machine Learning, een project is een logische container voor al het werk wordt gedaan om een probleem te verhelpen. Deze toegewezen aan een enkel bestand-map op uw lokale vaste schijf en kunt u bestanden of mappen die u wilt deze sub. Een project kan eventueel worden gekoppeld aan een Git-opslagplaats voor broncodebeheer en samenwerking.  

- **Experiment:** In Azure ML een experiment een of meer code bronbestand(en) die kunnen worden uitgevoerd vanaf een enkele invoerpunt is. Taken zoals gegevensopname, functie-engineering, training model of model evaluatie kan bevatten. Op dit moment alleen PySpark-experimenten of Azure ML Python ondersteunt.


- **Model:** In Azure Machine Learning, modellen verwijzen naar het product van een machine learning-experiment. Ze zijn recepten die wanneer correct toegepast op gegevens genereren voorspelde waarden. Modellen kunnen worden geïmplementeerd op test of productie-omgevingen en gebruikt voor score berekenen voor nieuwe gegevens. Eenmaal in productie, modellen worden gecontroleerd op prestaties en afwijking en retrained zoals vereist. 

- **COMPUTE doel:** een compute-doel is de compute-resource die u configureert voor het uitvoeren van uw experimenten. Het kan zijn op uw lokale computer (Windows of Mac OS), Docker-container op uw lokale computer of op een Linux-VM worden uitgevoerd op Azure of een HDInsight Spark-cluster.


- **Voer:** de experimenteren Service definieert een uitvoeren als de levensduur van de uitvoering van een experiment in een compute-doel. Azure ML automatisch gegevens van elke uitvoering worden vastgelegd en geeft de volledige geschiedenis van een bepaalde experiment in de vorm van uitvoeringsgeschiedenis.

- **Omgeving:** In Azure Machine Learning, een omgeving met een specifieke rekenbron die wordt gebruikt voor het implementeren en beheren van uw modellen aangegeven. Het kan zijn op uw lokale computer, een Linux VM op Azure of een Kubernetes-cluster in Azure Container Service wordt uitgevoerd, afhankelijk van de context en de configuratie. Het model wordt gehost in een Docker-container in deze omgevingen wordt uitgevoerd en weergegeven als een REST-API-eindpunt.


- **Beheerde model:** Model Management kunt u modellen als webservices implementeren, beheren van verschillende versies van uw modellen en de prestaties en metrische gegevens controleren. Beheerde modellen zijn geregistreerd met een account voor Azure Machine Learning-Model Management.

- **Manifesten:** als het Model-Management-systeem een model in productie implementeert, bevat een manifest dat, model, afhankelijkheden, scoreprofiel script, voorbeeldgegevens en schema bevatten kan. Het manifestbestand is het recept gebruikt voor het maken van de installatiekopie van een Docker-container. Model Management gebruikt, kunt u de manifesten automatisch genereren, verschillende versies maken en beheren van deze manifesten. 


- **Afbeeldingen:** kunt u manifesten genereren (en opnieuw genereren) Docker-installatiekopieën. Beperkte Docker-installatiekopieën maken flexibiliteit in de cloud, op lokale computers of op IoT-apparaat worden uitgevoerd. Installatiekopieën zelfstandig en alle afhankelijkheden die zijn vereist voor scorefuncties nieuwe gegevens met modellen bevatten. 

- **Services:** Model Management kunt u modellen als webservices implementeren. De logica van de webservice en de afhankelijkheden worden ingekapseld in een afbeelding. Elke webservice is een set van containers op basis van de afbeelding gereed voor aanvragen voor een opgegeven URL. Een webservice wordt beschouwd als een enkele implementatie.
