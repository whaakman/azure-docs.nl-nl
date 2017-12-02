---
title: Conceptueel overzicht van Azure Machine Learning-Model Management | Microsoft Docs
description: Dit document wordt uitgelegd concepten Model Management voor Azure Machine Learning.
services: machine-learning
author: nk773
ms.author: neerajkh, padou
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 0474e7ae77128a46fc6218de814913d07aa63d97
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-machine-learning-model-management"></a>Machine Learning Modelbeheer van Azure

Azure Machine Learning-Model Management kunt u beheren en implementeren van werkstromen en modellen van machine learning. 

Model Management biedt mogelijkheden voor:
- Model versiebeheer
- Bijhouden van modellen in productie
- Implementeren van modellen naar productie via berekenen voor AzureML-omgeving met [Azure Container Service](https://azure.microsoft.com/services/container-service/) en [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Docker-containers maken met de modellen en deze lokaal testen
- Geautomatiseerde model retraining
- Het opnemen van model telemetrie om bruikbare inzicht te krijgen. 

Azure Machine Learning-Model Management biedt een register van de modelversies. Het bevat ook geautomatiseerde werkstromen voor het verpakken en Machine Learning-containers als REST-API's implementeren. De modellen en hun runtime-afhankelijkheden zijn verpakt in op basis van Linux Docker-container met voorspelling API. 

Azure Machine Learning Compute-omgevingen help bij het instellen en beheren van schaalbare clusters voor het hosten van de modellen. De compute-omgeving is gebaseerd op Azure Container Services. Azure Container Services biedt automatische blootstelling van Machine Learning API's als REST-API-eindpunten met de volgende functies:

- Authentication
- Taakverdeling
- Automatische scale-out
- Versleuteling

Azure Machine Learning-Model Management biedt deze mogelijkheden via de CLI, API en de Azure-portal. 

Azure Machine Learning-model management gebruikt de volgende informatie:

 - Modelbestand of map met de modelbestanden
 - Implementatie van een functie score model Python-bestand gemaakt
 - Conda afhankelijkheidsbestand met runtime-afhankelijkheden
 - Keuze voor runtime-omgeving, en 
 - Schemabestand voor API-parameters 

Deze informatie wordt gebruikt bij het uitvoeren van de volgende acties:

- Registreren van een model
- Maken van een manifest dat wordt gebruikt bij het bouwen van een container
- Een Docker-container installatiekopie bouwen
- Een container implementeren in Azure Container Service
 
De volgende afbeelding toont een overzicht van hoe modellen zijn geregistreerd en geïmplementeerd in het cluster. 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Maken en beheren van modellen 
U kunt modellen met Azure Machine Learning-Model Management registreren voor het bijhouden van modelversies in productie. De service legt vereenvoudigen reproduceerbaarheid en beheeracties, afhankelijkheden en de bijbehorende informatie. U kunt model telemetrie met de opgegeven SDK vastleggen voor een beter inzicht in prestaties. Model telemetrie is in de gebruiker opgegeven opslag gearchiveerd. De model-telemetrie kan later worden gebruikt voor het model prestaties te analyseren, retraining en inzichten voor uw bedrijf krijgen.

## <a name="create-and-manage-manifests"></a>Maken en beheren van de manifesten 
Modellen vereisen aanvullende artefacten te implementeren in productie. Het systeem biedt de mogelijkheid voor het maken van een manifest dat model, afhankelijkheden, Deductie script (aka scoreprofiel script), voorbeeldgegevens, schema enzovoort omvat. Dit manifest fungeert als een recept om de installatiekopie van een Docker-container te maken. Ondernemingen kunnen automatisch manifest genereren, verschillende versies maken en beheren van hun manifesten. 

## <a name="create-and-manage-docker-container-images"></a>Maken en beheren van installatiekopieën van de Docker-container 
U kunt het manifest van de vorige stap voor het maken van installatiekopieën op basis van Docker-container in hun respectieve omgevingen. Installatiekopieën van het beperkte, op basis van Docker voorzien ondernemingen van de flexibiliteit voor het uitvoeren van deze installatiekopieën op de volgende compute-omgevingen:

- [Kubernetes op basis van Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- On-premises container-services
- Ontwikkelomgevingen
- IoT-apparaten

Deze beperkte Docker-installatiekopieën worden ingesloten met alle benodigde afhankelijkheden die zijn vereist voor het genereren van voorspellingen. 

## <a name="deploy-docker-container-images"></a>Installatiekopieën van de Docker-container implementeren 
U kunt met Azure Machine Learning-Model Management op basis van Docker-container afbeeldingen met één opdracht implementeren in Azure Container Service beheerd via ML Compute-omgeving. Deze implementaties worden gemaakt met een front-server de volgende functies biedt:

- Lage latentie voorspellingen op grote schaal
- Taakverdeling
- Automatische schaling van ML-eindpunten
- API-sleutel autorisatie
- API swagger-document

U kunt de omvang van de implementatie en de telemetrie via de volgende configuratie-instellingen beheren:

- Systeemregistratie en model telemetrie voor elke web service-niveau. Bij inschakeling alle stdout logboeken worden gestreamd naar [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Model telemetrie wordt in de opslag die u opgeeft gearchiveerd. 
- Limieten voor automatisch schalen en een gelijktijdigheid van taken. Het aantal geïmplementeerde containers op basis van de belasting binnen het bestaande cluster wordt automatisch vergroten door deze instellingen. Hiermee regelt u ook de doorvoer en consistentie van de voorspelling latentie.

## <a name="consumption"></a>Verbruik 
Azure Machine Learning-Model Management REST-API voor het geïmplementeerde model samen met de swagger-document gemaakt. U kunt geïmplementeerde modellen verbruiken door aanroepen van de REST-API's met API sleutel en model van de invoer voor de voorspellingen krijgt als onderdeel van de line-of-business-toepassingen. De voorbeeldcode is beschikbaar in GitHub voor Java, talen [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py), en C# voor het aanroepen van REST-API's. Azure Machine Learning-Model Management CLI biedt een eenvoudige manier om met deze REST-API's te werken. U kunt de API's via één CLI opdracht, een swagger-toepassingen, of met een curl verbruiken. 

## <a name="retraining"></a>Retraining 
Azure Machine Learning-Model Management biedt API's waarmee u kunt opnieuw trainen van modellen. U kunt ook de API's gebruiken bij het bijwerken van bestaande implementaties met bijgewerkte versies van het model. Als onderdeel van de wetenschappelijke werkstroom, kunt u het model maken in uw omgeving experimenteren. Vervolgens u het model met Model Management registreren en bijwerken van bestaande implementaties. Updates worden uitgevoerd met één opdracht UPDATE CLI. De opdracht UPDATE updates bestaande implementaties zonder de API-URL of de sleutel te wijzigen. De toepassingen het model blijven werken zonder codewijziging en betere voorspellingen nieuw model met krijgen.

De volledige werkstroom met een beschrijving van deze begrippen is vastgelegd in de volgende afbeelding:

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen 
- Welke gegevenstypen worden ondersteund? Kan ik matrices NumPy rechtstreeks als invoer voor de webservice?

   Als u schemabestand dat is gemaakt met behulp van generate_schema SDK biedt, kunt klikt u vervolgens u doorgeven NumPy en/of Pandas DF. U kunt ook een serialiseerbare JSON-invoer doorgeven. Afbeelding kan worden doorgegeven als ook binaire gecodeerde tekenreeks.

- De webservice ondersteuning voor meerdere invoer of andere invoer parseren? 

   Ja, kunt u meerdere invoer verpakt in een JSON-aanvraag als een woordenboek nemen. Elke invoer zou komen overeen met een sleutel één unieke woordenlijst.

- Is de aanroep geactiveerd door een aanvraag naar de web service een aanroep van blokkerende of een asynchrone aanroep?

   Als de service is gemaakt met behulp van de optie realtime als onderdeel van de CLI of API, is het een blokkeren/synchrone aanroep. Naar verwachting worden snel realtime. Hoewel blokkeren aan de clientzijde die u met behulp van asynchrone HTTP-bibliotheek aanroepen kunt om te voorkomen dat de client-thread.

- Hoeveel aanvragen de webservice tegelijk kan verwerken?

   Dit is afhankelijk van de cluster en web service schaal. U kunt uw service tot 100 x van replica's uitbreiden en vervolgens het gelijktijdig veel aanvragen kan verwerken. U kunt ook de maximum aantal gelijktijdige aanvragen per replica hogere service doorvoersnelheid configureren.

- Hoeveel aanvragen kan de webservice in de wachtrij?

   Het kan worden geconfigureerd. Standaard is ingesteld op 10 ~ per één replica, maar u kunt vergroten of verkleinen het aan de toepassingsvereisten van uw. Normaal gesproken verhogen deze het aantal aanvragen in de wachtrij verhoogt de doorvoer service maar maakt de latenties erger op hogere percentielen. Als u wilt de latenties consistent te houden, mag u wilt instellen op het in de wachtrij op een lage waarde (1-5) en verhoog het aantal replica's voor het afhandelen van de doorvoer. U kunt ook automatisch schalen aanbrengen het aantal replica's aanpassen automatisch op basis van load inschakelen. 

- Kan de dezelfde machine of het cluster worden gebruikt voor meerdere webservice-eindpunten?

   Absoluut. U kunt 100 x van de services-eindpunten uitvoeren in hetzelfde cluster. 

## <a name="next-steps"></a>Volgende stappen
Zie voor aan de slag met Model Management, [Configuring Model Management](deployment-setup-configuration.md).
