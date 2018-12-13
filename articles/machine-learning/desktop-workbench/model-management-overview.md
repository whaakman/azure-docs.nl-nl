---
title: Conceptueel overzicht van Azure Machine Learning Modelbeheer | Microsoft Docs
description: Dit document wordt uitgelegd Modelbeheer-concepten voor Azure Machine Learning.
services: machine-learning
author: chris-lauren
ms.author: clauren
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: b89e80051ec53649a3da8fadcf412c58c177525c
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53260205"
---
# <a name="azure-machine-learning-model-management"></a>Machine Learning Modelbeheer van Azure

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Azure Machine Learning Modelbeheer kunt u machine learning-werkstromen en modellen beheren en implementeren. 

Modelbeheer biedt mogelijkheden voor:
- Model-versiebeheer
- Tracerings-modellen in productie
- Implementeren van modellen in productie door middel van AzureML-Compute-omgeving met [Azure Container Service](https://azure.microsoft.com/services/container-service/) en [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Docker-containers met de modellen maken en deze lokaal testen
- Geautomatiseerde opnieuw trainen van modellen
- Model-telemetrie voor inzichten vast te leggen. 

Azure Machine Learning Modelbeheer voorziet in een register van modelversies. Het biedt ook geautomatiseerde werkstromen voor het verpakken en implementeren van Machine Learning-containers als REST-API's. De modellen en hun runtime-afhankelijkheden zijn verpakt in Docker op basis van Linux-container met de voorspellings-API. 

Azure Machine Learning-computeromgevingen helpen bij het instellen en beheren van schaalbare clusters voor het hosten van de modellen. De compute-omgeving is gebaseerd op Azure Container Services. Azure Container Services biedt automatische blootstelling van API's voor Machine Learning als REST-API-eindpunten met de volgende functies:

- Verificatie
- Taakverdeling
- Automatische scale-out
- Versleuteling

Azure Machine Learning Modelbeheer biedt deze mogelijkheden via de CLI, API en de Azure-portal. 

Azure Machine Learning-Modelbeheer maakt gebruik van de volgende informatie:

 - Modelbestand of map met de modelbestanden
 - Gebruiker heeft gemaakt voor het implementeren van een model scoren functie Python-bestand
 - Conda-afhankelijkheidsbestand runtime-afhankelijkheden weergeven
 - Keuze van runtime-omgeving, en 
 - Schema-bestand voor de API-parameters 

Deze informatie wordt gebruikt bij het uitvoeren van de volgende acties:

- Registreren van een model
- Het maken van een manifest die wordt gebruikt bij het bouwen van een container
- Het bouwen van een Docker-containerinstallatiekopie
- Een container te implementeren naar Azure Container Service
 
De volgende afbeelding toont een overzicht van hoe de modellen worden geregistreerd en geïmplementeerd in het cluster. 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Maken en beheren van modellen 
U kunt modellen met Azure Machine Learning Modelbeheer registreren voor het bijhouden van modelversies in de productieomgeving. De service vastgelegd voor het gebruiksgemak reproduceerbaarheid en beheeracties, afhankelijkheden en de bijbehorende informatie. Voor meer inzicht in prestaties kunt u de model-telemetrie met behulp van de opgegeven SDK vastleggen. Model-telemetrie is in de gebruiker opgegeven opslag gearchiveerd. De model-telemetrie kan later worden gebruikt voor het analyseren van modelprestaties, opnieuw trainen en inzichten te verkrijgen voor uw bedrijf.

## <a name="create-and-manage-manifests"></a>Maken en beheren van de manifesten 
Modellen nodig extra artefacten te implementeren in productie. Het systeem biedt de mogelijkheid voor het maken van een manifest van het model, afhankelijkheden, Deductie script (ook wel scoring-script), voorbeeldgegevens, schema enzovoort. Dit manifest fungeert als een recept om een Docker-containerinstallatiekopie te maken. Ondernemingen kunnen automatisch manifest genereren, verschillende versies maken en beheren van hun manifesten. 

## <a name="create-and-manage-docker-container-images"></a>Maken en beheren van Docker-containerinstallatiekopieën 
U kunt het manifest van de vorige stap gebruiken om te bouwen op basis van een Docker-containerinstallatiekopieën in hun respectieve omgevingen. De in containers, op basis van een Docker-installatiekopieën bevatten ondernemingen met de flexibiliteit voor het uitvoeren van deze installatiekopieën op de volgende omgevingen:

- [Kubernetes op basis van Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- On-premises containerservices
- U kunt ontwikkelomgevingen
- IoT-apparaten

Deze containers op basis van een Docker-installatiekopieën zijn op zichzelf staand met alle benodigde afhankelijkheden die zijn vereist voor het genereren van voorspellingen. 

## <a name="deploy-docker-container-images"></a>Installatiekopieën van Docker-container implementeren 
Met Machine Learning Modelbeheer van Azure, kunt u op basis van een Docker-containerinstallatiekopieën met slechts één opdracht implementeren op Azure Container Service worden beheerd door ML Compute-omgeving. Deze implementaties worden gemaakt met een front-endwebserver die de volgende functies biedt:

- Voorspellingen van de lage latentie op grote schaal
- Taakverdeling
- Automatisch schalen van ML-eindpunten
- Autorisatiebeleid API
- API-swagger-document

U kunt de schaal van de implementatie en de telemetrie via de volgende configuratie-instellingen beheren:

- Systeemregistratie en model telemetrie voor elke web service-niveau. Bij inschakeling alle stdout-logboeken worden gestreamd naar [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Model telemetrie wordt gearchiveerd in de opslag die u opgeeft. 
- Limieten voor automatisch schalen en gelijktijdigheid. Deze instellingen worden automatisch het aantal geïmplementeerde containers op basis van de belasting binnen het bestaande cluster verhogen. Ze bepalen ook de doorvoer en consistentie van voorspelling latentie.

## <a name="consumption"></a>Verbruik 
Azure Machine Learning Modelbeheer maakt REST-API voor het gedistribueerde model samen met de swagger-document. U kunt gebruiken van geïmplementeerde modellen door aanroepen van de REST-API's met API-sleutel en het model van de invoer voor de voorspellingen krijgt als onderdeel van de line-of-business-toepassingen. De voorbeeldcode is beschikbaar in GitHub voor talen Java, [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py), en C# voor het aanroepen van REST-API's. Azure Machine Learning-Model Management CLI biedt een eenvoudige manier om te werken met deze REST-API's. U kunt de API's met behulp van een enkele CLI-opdracht, een swagger-toepassingen, of curl verbruiken. 

## <a name="retraining"></a>Opnieuw trainen 
Azure Machine Learning Modelbeheer biedt API's die u gebruiken kunt voor uw modellen opnieuw trainen. U kunt ook de API's gebruiken voor het bijwerken van bestaande implementaties met bijgewerkte versies van het model. Als onderdeel van de werkstroom voor datatechnologie, maakt u het model opnieuw in uw experimenten-omgeving. Vervolgens u het model registreren bij Modelbeheer en bestaande implementaties van updates. Updates worden uitgevoerd met behulp van een eenmalige UPDATE CLI-opdracht. De opdracht UPDATE werkt bestaande implementaties zonder de API-URL of de sleutel te wijzigen. Het model de toepassingen blijven werken zonder codewijziging, en beginnen met het doen van betere voorspellingen met behulp van nieuwe model ophalen.

De volledige werkstroom met een beschrijving van deze concepten worden vastgelegd in de volgende afbeelding:

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen 
- **Welke gegevenstypen worden ondersteund? Kan ik matrices NumPy rechtstreeks als invoer voor de webservice?**

   Als u schemabestand dat is gemaakt met behulp van generate_schema SDK opgeeft, kunt klikt u vervolgens u doorgeven NumPy en/of Pandas DF. U kunt ook een JSON-invoer voor serialiseerbare doorgeven. U kunt installatiekopie doorgeven als ook binaire gecodeerde tekenreeks.

- **De webservice ondersteunen meerdere invoergegevens of parseren van verschillende soorten invoer?**

   Ja, kunt u meerdere invoergegevens verpakt in een JSON-aanvraag als een woordenboek uitvoeren. Elke invoer zou komen overeen met een sleutel één unieke woordenlijst.

- **Is de aanroep geactiveerd door een aanvraag voor de web service teruggebeld te blokkeren of een asynchrone aanroep?**

   Als service is gemaakt met behulp van de optie voor real-time als onderdeel van de CLI of API, is het een blokkeren/synchrone aanroep. Naar verwachting worden realtime snel. Hoewel blokkeren aan de clientzijde die u met behulp van asynchrone HTTP-bibliotheek aanroepen kunt om te voorkomen dat de client-thread.

- **Het aantal aanvragen voor de webservice tegelijk kan verwerken?**

   Dat hangt ervan af op de schaal van de service-cluster en het web. U kunt de service tot 100 x van replica's uitbreiden en vervolgens het gelijktijdig veel aanvragen kan verwerken. U kunt ook het maximum aantal gelijktijdige aanvraag per replica om service-doorvoer te vergroten.

- **Het aantal aanvragen kan de webservice wachtrij komen?**

   Het kan worden geconfigureerd. Standaard is ingesteld op ~ 10 per één replica, maar u kunt vergroten/verkleinen het aan de toepassingsvereisten voor uw. Meestal het vergroten het aantal aanvragen in de wachtrij verhoogt de netwerkdoorvoer service, maar Hiermee wordt de latenties nog erger op hogere percentielen. U kunt om te voorkomen dat de latenties consistente, wilt instellen op het in de wachtrij op een lage waarde (1-5), en het aantal replica's voor het afhandelen van de doorvoer te verhogen. U kunt ook automatisch schalen om te laten het aantal replica's aanpassen automatisch op basis van load inschakelen. 

- **Kan de dezelfde machine of het cluster worden gebruikt voor meerdere web service-eindpunten?**

   Absoluut. U kunt 100 x de prijs van services /-eindpunten in hetzelfde cluster uitvoeren. 

## <a name="next-steps"></a>Volgende stappen
Zie voor het aan de slag met Modelbeheer, [Modelbeheer configureren](deployment-setup-configuration.md).
