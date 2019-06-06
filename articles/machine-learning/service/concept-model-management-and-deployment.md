---
title: 'MLOps: Beheren, implementeren en bewaken van ML-modellen'
titleSuffix: Azure Machine Learning service
description: 'Informatie over het gebruik van Azure Machine Learning-Service voor MLOps: implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning-Service is getraind op uw lokale computer of uit andere bronnen kunt implementeren.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 0eaf48f57c3011222b71a63d703e1ccec7aca001
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692837"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Beheren, implementeren en bewaken van modellen met Azure Machine Learning-Service

In dit artikel leert u over het gebruik van Azure Machine Learning-service voor het beheren van de levenscyclus van uw modellen. Azure Machine Learning gebruikmaakt van een benadering voor Machine Learning-bewerkingen (MLOps), wat zorgt voor betere kwaliteit en consistentie van uw machine learning-oplossingen. Azure Machine Learning-Service biedt de volgende MLOps mogelijkheden:

* Integratie met Azure pijplijnen. Continue integratie en implementatie workflows definiëren voor uw modellen.
* Een model-register die meerdere versies van uw getrainde modellen onderhoudt.
* Validatie van het model. Automatisch uw getrainde modellen valideren en selecteer de optimale configuratie voor het implementeren van deze in productie.
* Uw modellen te implementeren als een webservice in de cloud, lokaal of op IoT Edge-apparaten.
* Prestaties van uw geïmplementeerde model, controleren, zodat u verbeteringen in de volgende versie van het model kunt uitbreiden.

U hoort meer over de concepten achter MLOps en hoe ze van toepassing op de Azure Machine Learning-service, de volgende video te bekijken.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="integration-with-azure-pipelines"></a>Integratie met Azure pijplijnen

Azure-pijplijnen kunt u een continue integratie-proces dat binnen een model maken. In een typisch scenario, wanneer een Gegevenswetenschapper controleert of een wijziging in de Git-opslagplaats voor een project, wordt de Azure-pijplijn gestart een training uitvoeren. De resultaten van de uitvoering kunnen vervolgens worden gecontroleerd om te zien van de prestatiekenmerken van het getrainde model. U kunt ook een pijplijn maken die het model als een webservice implementeert.

De [Azure Machine Learning-extensie](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) maakt het eenvoudiger om te werken met Azure-pijplijnen. Biedt de volgende verbeteringen voor Azure-pijplijnen:

* Kan werkruimte kunt selecteren bij het definiëren van een service-verbinding.
* Schakelt het uitbrengen van pijplijnen worden geactiveerd door getrainde modellen die zijn gemaakt in een pijplijn training.

Zie voor meer informatie over het gebruik van Azure-pijplijnen met Azure Machine Learning, de [continue integratie en implementatie van ML-modellen met Azure-pijplijnen](/azure/devops/pipelines/targets/azure-machine-learning) artikel en de [Azure Machine Learning-Service MLOps](https://aka.ms/mlops) opslagplaats.

## <a name="convert-and-optimize-models"></a>Converteren en modellen te optimaliseren

Converteren van uw model [Open Neural Network-Exchange](https://onnx.ai) (ONNX) kan de prestaties verbeteren. Gemiddeld, het converteren naar de ONNX een 2 x toename van de prestaties opleveren.

Zie voor meer informatie over ONNX met Azure Machine Learning-service, de [maken en te versnellen ML-modellen](concept-onnx.md) artikel.

## <a name="register-models"></a>Modellen registreren

Registratie van het model kunt u om op te slaan en versie van uw modellen in de Azure-cloud in uw werkruimte. Het register model kunt u eenvoudig is om te organiseren en bijhouden van uw getrainde modellen.

> [!TIP]
> U kunt ook modellen die zijn getraind buiten de Azure Machine Learning-service registreren.
 
Geregistreerde modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren in het register wordt het versienummer verhoogd. U kunt ook aanvullende metagegevenstags opgeven tijdens de registratie die kan worden gebruikt bij het zoeken naar voor modellen. De Azure Machine Learning-service ondersteunt elk model dat worden kan geladen met behulp van Python 3.5.2 gebruikt of hoger.

U kunt modellen die worden gebruikt in een actieve implementatie niet verwijderen.

Zie voor meer informatie de sectie voor het model van registreren van [modellen implementeren](how-to-deploy-and-where.md#registermodel).

Zie voor een voorbeeld van het registreren van een model dat is opgeslagen in de indeling van pickle [zelfstudie: Een installatiekopie van classificatie-model te trainen](tutorial-deploy-models-with-aml.md).

## <a name="package-and-debug-models"></a>Pakket- en foutopsporing-modellen

Voordat u implementeert een model in productie, is het verpakt in een Docker-installatiekopie. In de meeste gevallen, het maken van installatiekopieën wordt automatisch uitgevoerd op de achtergrond tijdens de implementatie. Voor geavanceerde scenario's, kunt u de installatiekopie van het handmatig opgeven.

Als u problemen met de implementatie, kunt u implementeren op uw lokale ontwikkelomgeving voor probleemoplossing en foutopsporing.

Zie voor meer informatie, [modellen implementeren](how-to-deploy-and-where.md#registermodel) en [problemen met implementaties oplossen](how-to-troubleshoot-deployment.md).

## <a name="validate-and-profile-models"></a>Valideren en modellen-profiel

Azure Machine Learning-service kunt profilering gebruiken om te bepalen van de ideale CPU en geheugen instellingen moet worden gebruikt wanneer uw model implementeert. Validatie wordt uitgevoerd als onderdeel van dit proces, met behulp van de gegevens die u opgeeft voor de profilering proces.

## <a name="use-models"></a>Modellen gebruiken

Getrainde machine learning-modellen kan worden geïmplementeerd als webservices in de cloud of lokaal op uw ontwikkelomgeving. U kunt ook modellen implementeren op Azure IoT Edge-apparaten. Implementaties kunnen CPU, GPU of veld-programmable gate arrays (FPGA) gebruiken voor inferentietaken. U kunt ook modellen van Power BI gebruiken.

Wanneer u een model als een webservice of IoT Edge-apparaat, bieden u de volgende items:

* De modellen die worden gebruikt om gegevens verzonden naar de service/apparaat te beoordelen.
* Een post-script. Met dit script-aanvragen accepteert, gebruikt de modellen te beoordelen van de gegevens en een antwoord.
* Een bestand conda-omgeving die de afhankelijkheden die nodig is door het script modellen en het verkeer beschrijft.
* Eventuele extra activa, zoals tekst, gegevens, enzovoort die door het script modellen en het verkeer vereist zijn.

Deze elementen zijn verpakt in een Docker-installatiekopie en geïmplementeerd als een webservice of IoT Edge-module.

Desgewenst kunt u de volgende parameters om af te stemmen verder de implementatie:

* GPU inschakelen: Gebruikt voor het inschakelen van GPU-ondersteuning in de Docker-installatiekopie. De afbeelding moet worden gebruikt op Microsoft Azure-Services zoals Azure Container Instances, Azure Kubernetes Service, Azure Machine Learning-Computing of Azure Virtual Machines.
* Stappen voor extra docker-bestand: Een bestand met Docker voor extra stappen uitvoeren bij het maken van de Docker-installatiekopie uitvoeren.
* Basisinstallatiekopie: Een aangepaste installatiekopie gebruiken als de basisinstallatiekopie. Als u een aangepaste installatiekopie niet gebruikt, wordt de basisinstallatiekopie geleverd door de service Azure Machine Learning.

U ook opgeven de configuratie van het doelplatform voor implementatie. Bijvoorbeeld, de VM type adresfamilie, beschikbare geheugen en kernen bij het implementeren in Azure Kubernetes Service.

Wanneer de installatiekopie is gemaakt, worden ook de onderdelen die vereist zijn door Azure Machine Learning-Service toegevoegd. Bijvoorbeeld, assets die nodig zijn voor het uitvoeren van de webservice en communiceren met IoT Edge.

> [!NOTE]
> U kan wijzigen of de webserver of IoT Edge-onderdelen die worden gebruikt in de Docker-installatiekopie wijzigen. Azure Machine Learning-service maakt gebruik van de configuratie van een web-server en de IoT Edge-onderdelen die zijn getest en ondersteund door Microsoft.

### <a name="web-service"></a>Webservice

U kunt uw modellen in **webservices** compute-doelen met het volgende:

* Azure Container Instance
* Azure Kubernetes Service
* Lokale ontwikkelomgeving

Als u wilt het model als een webservice implementeert, moet u de volgende items:

* Het model of ensembles van modellen.
* Afhankelijkheden die zijn vereist voor het gebruik van het model. Bijvoorbeeld, een script dat aanvragen accepteert en roept het model, conda-afhankelijkheden, enzovoort.
* De configuratie van de implementatie die wordt beschreven hoe en waar het model te implementeren.

Zie voor meer informatie, [modellen implementeren](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>IoT Edge-apparaten


Kunt u modellen maken met IoT-apparaten via **Azure IoT Edge-modules**. IoT Edge-modules worden geïmplementeerd op een apparaat, waarmee Deductie, of het model scoren, op het apparaat.

Zie voor meer informatie, [modellen implementeren](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analyse

Microsoft Power BI ondersteunt het gebruik van machine learning-modellen voor gegevensanalyse. Zie voor meer informatie, [Azure Machine Learning-integratie in Power BI (Preview)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="monitor-and-collect-data"></a>Controleren en verzamelen van gegevens

Bewaking, krijgt u inzicht in welke gegevens worden verzonden naar het model en de voorspellingen die worden geretourneerd.

Deze informatie helpt u begrijpen hoe het model wordt gebruikt. De ingevoerde verzamelde gegevens zijn mogelijk ook nuttig in toekomstige versies van de training van het model.

Zie voor meer informatie, [het inschakelen van gegevensverzameling van model](how-to-enable-data-collection.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u modellen kunt implementeren](how-to-deploy-and-where.md) met de Azure Machine Learning-service. Zie voor een voorbeeld van de implementatie van [zelfstudie: Een afbeelding classificatie implementatiemodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Meer informatie over het maken van [continue integratie en implementatie van ML-modellen met Azure-pijplijnen](/azure/devops/pipelines/targets/azure-machine-learning). 

Meer informatie over het maken van client-toepassingen en services waar [een model geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md).
