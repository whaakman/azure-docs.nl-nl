---
title: Hoe werkt Azure Machine Learning-service?
description: Meer informatie over de architectuur, -terminologie en concepten van Azure Machine Learning-service. Ook leert u over de algemene werkstroom van het gebruik van de service en de Azure-services die worden gebruikt door de Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 10/24/2018
ms.openlocfilehash: 0acf41cc0a2673ba665d1815b493df928fa4507d
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706803"
---
# <a name="how-the-azure-machine-learning-service-works-architecture-and-concepts"></a>De werking van de service Azure Machine Learning: architectuur en concepten

Dit document beschrijft de architectuur en concepten voor de Azure Machine Learning-service. Het volgende diagram ziet u de belangrijkste onderdelen van de service en de algemene werkstroom ziet u bij het gebruik van de service: 

[![Azure Machine Learning-Service-architectuur en werkstroom](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

De werkstroom in het algemeen als volgt:

1. Machine learning-scripts in Trainingen ontwikkelen __Python__.
1. Maken en configureren van een __compute-doel__.
1. __Verzenden van de scripts__ naar de geconfigureerde compute-doel om uit te voeren in die omgeving. Tijdens de training, slaat de compute-doel uitvoeren records die u wilt een __gegevensopslag__. Er records zijn opgeslagen in een __experimenteren__.
1. __Query uitvoeren op het experiment__ geregistreerde voor metrische gegevens van de huidige en eerdere uitvoeringen. Als de metrische gegevens niet aan de gewenste resultaat, lus terug naar stap 1 en ze opnieuw testen op uw scripts.
1. Als een goede uitvoering wordt gevonden, registreert u het persistente model in de __model register__.
1. Ontwikkel een scoring-script.
1. __Een installatiekopie maken__ en registreren in de __installatiekopieregisters__. 
1. __De installatiekopie implementeert__ als een __webservice__ in Azure.


[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

> [!NOTE]
> Hoewel dit document definieert termen en concepten die worden gebruikt door Azure Machine Learning, worden niet gedefinieerd door termen en begrippen voor het Azure-platform. Zie voor meer informatie over Azure-platform terminologie, het [verklarende woordenlijst voor Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Werkruimte

De werkruimte is de resource op het hoogste niveau voor de Azure Machine Learning-service. Het biedt een centrale locatie voor het werken met alle artefacten die u maakt bij het gebruik van Azure Machine Learning-Service.

De werkruimte houdt een lijst met compute-doelen die kan worden gebruikt om uw model te trainen. Het houdt ook een geschiedenis bij van de training wordt uitgevoerd, met inbegrip van Logboeken, metrische gegevens, uitvoer en een momentopname van uw scripts. Deze informatie wordt gebruikt om te bepalen welke run training levert het beste model.

Modellen zijn geregistreerd bij de werkruimte. Een geregistreerde model en score scripts worden gebruikt om een installatiekopie te maken. De installatiekopie kan vervolgens worden geïmplementeerd naar Azure Container Instances, Azure Kubernetes Service, of naar een veld-programmable gate array (FPGA) als een HTTP op basis van REST-eindpunt. Het kan ook worden geïmplementeerd in een Azure IoT Edge-apparaat als een module.

U kunt meerdere werkruimten maken en elke werkruimte kan worden gedeeld door meerdere personen. Wanneer u een werkruimte wilt delen, moet u toegang tot de werkruimte beheren door de volgende rollen toewijzen aan gebruikers:

* Eigenaar
* Inzender
* Lezer

Wanneer u een nieuwe werkruimte maakt, wordt automatisch verschillende Azure-resources die worden gebruikt door de werkruimte gemaakt:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) -docker-containers die worden gebruikt tijdens de training en bij het implementeren van een model registreert.
* [Azure Storage](https://azure.microsoft.com/services/storage/) : wordt gebruikt als de standaard gegevensopslag voor de werkruimte.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) - winkels controlegegevens over uw modellen.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) - winkels geheimen die worden gebruikt door compute-doelen en andere gevoelige informatie nodig voor de werkruimte.

> [!NOTE]
> In plaats van het maken van nieuwe versies, kunt u ook bestaande Azure-services gebruiken. 

Het volgende diagram wordt een taxonomie van de werkruimte:

[![Taxonomie van werkruimte](./media/concept-azure-machine-learning-architecture/taxonomy.png)](./media/concept-azure-machine-learning-architecture/taxonomy.png#lightbox)

## <a name="model"></a>Model

De eenvoudigste is een model een stukje code dat een invoer en uitvoer wordt geproduceerd. Het maken van een machine learning-model omvat het selecteren van een algoritme, voorzien van deze gegevens en afstemmen van hyperparameters. Training is een iteratief proces dat een getraind model produceert, wat het model hebt geleerd tijdens de training ingekapseld.

Een model wordt geproduceerd door een uitvoering in Azure Machine Learning. U kunt ook een getraind buiten Azure Machine Learning-model gebruiken. Een model kan worden geregistreerd onder een werkruimte van Azure Machine Learning-service.

Azure Machine Learning-Service is framework-agnostische. U kunt elk gewenst framework populaire machine learning kunt gebruiken bij het maken van een model, zoals scikit-informatie, xgboost, PyTorch, TensorFlow, Chainer en CNTK.

Zie voor een voorbeeld van een model te trainen, de [Quickstart: maken van een machine learning-werkruimte Service](quickstart-get-started.md) document.

### <a name="model-registry"></a>Model-register

Het register model houdt van alle modellen in de werkruimte van uw Azure Machine Learning-service. 

Modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren het register wordt ervan uitgegaan dat er een nieuwe versie beschikbaar is. De versie wordt verhoogd en het nieuwe model is geregistreerd onder de naam.

U kunt aanvullende metagegevenstags opgeven wanneer u het model registreren en vervolgens deze tags gebruiken bij het zoeken naar modellen.

U kunt modellen die worden gebruikt door een installatiekopie niet verwijderen.

Zie voor een voorbeeld van het registreren van een model, de [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md) document.

## <a name="image"></a>Installatiekopie

Afbeeldingen bieden een manier om op betrouwbare wijze een model, samen met alle onderdelen die nodig zijn voor het gebruik van het model te implementeren. Een installatiekopie bevat de volgende items:

* Een model.
* Een scoring-script of toepassing. Met dit script wordt gebruikt voor het doorgeven van invoer voor het model en de uitvoer van het model geretourneerd.
* Afhankelijkheden die nodig zijn voor het model of het scoring-script/toepassing.  U kunt bijvoorbeeld een Conda-omgeving-bestand met een lijst met Python-pakketafhankelijkheden opnemen.

Er zijn twee typen installatiekopieën die kunnen worden gemaakt met Azure Machine Learning:

* FPGA-installatiekopie: gebruikt bij het implementeren op een veld-programmable gate array in de Azure-cloud.
* Docker-installatiekopie: gebruikt bij het implementeren voor de compute-doelen dan FPGA. Bijvoorbeeld, Azure Container Instances en Azure Kubernetes Service.

Zie voor een voorbeeld van het maken van een afbeelding, de [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md) document.

### <a name="image-registry"></a>Afbeelding register

Houdt het installatiekopieregister van installatiekopieën gemaakt op basis van uw modellen. Wanneer de installatiekopie is gemaakt, kunt u aanvullende metagegevenstags bieden. Labels worden opgeslagen door het installatiekopieregister en de metagegevens kunnen worden opgevraagd om de afbeelding te zoeken.

## <a name="deployment"></a>Implementatie

Een implementatie is een instantie van uw installatiekopie in een van beide een webservice die kan worden gehost in de cloud of een IoT-Module voor implementaties van geïntegreerde apparaat. 

### <a name="web-service"></a>Webservice

Een geïmplementeerde webservice kunt gebruiken voor Azure Container Instances, Azure Kubernetes Service of veld-programmable gate arrays (FPGA).
De service wordt gemaakt van een installatiekopie die uw model, scripts en bijbehorende bestanden bevat. De afbeelding heeft een gelijke, HTTP-eindpunt dat scoring aanvragen verzonden naar de webservice ontvangt.

Azure helpt u bij de implementatie van uw Web-service controleren door het verzamelen van telemetrie van Application Insights en/of model telemetrie als u hebt gekozen voor het inschakelen van deze functie. De telemetrische gegevens is alleen toegankelijk is voor u, en die zijn opgeslagen in uw Application Insights en storage-account instanties.

Als u hebt ingeschakeld om automatisch schalen, schaalt Azure automatisch de implementatie.

Zie voor een voorbeeld van het implementeren van een model als een webservice, het [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md) document.

### <a name="iot-module"></a>IoT-Module

Een geïmplementeerde IoT-Module is een Docker-container met het model en bijbehorende script of toepassing en eventuele extra afhankelijkheden. Deze modules worden geïmplementeerd met behulp van Azure IoT Edge op edge-apparaten. 

Als u de bewaking hebt ingeschakeld, verzamelt telemetriegegevens op Azure uit het model in de Azure IoT Edge-module. De telemetrische gegevens is alleen toegankelijk is voor u, en opgeslagen in uw exemplaar van storage-account.

Azure IoT Edge zorgt ervoor dat de module wordt uitgevoerd en controleren van het apparaat waarop deze wordt gehost.

## <a name="datastore"></a>Gegevensopslag

Een gegevensarchief is een abstractie van opslag via een Azure Storage-Account. Het gegevensarchief kan een Azure blob-container of een Azure-bestandsshare als back-end-opslag gebruiken. Elke werkruimte heeft een standaard-gegevensarchief en u kunt aanvullende gegevensopslag kan registreren. 

De Python SDK-API of Azure Machine Learning CLI gebruiken voor het opslaan en ophalen van bestanden van het gegevensarchief. 

## <a name="run"></a>Voer

Een uitvoering is een record met de volgende informatie:

* Metagegevens over de uitvoering (tijdstempel, duur, enz.)
* Metrische gegevens die zijn vastgelegd door het script
* Uitvoer autocollected bestanden door het experiment of expliciet door u worden geüpload.
* Een momentopname van de map waarin uw scripts, voordat de uitvoering

Een uitvoering wordt gemaakt wanneer u een script voor een model te trainen verzendt. Een uitvoering kan nul of meer onderliggende uitvoeringen hebben. Zodat het op het hoogste niveau uitvoeren mogelijk twee onderliggende-wordt uitgevoerd, die mogelijk is hun eigen onderliggende wordt uitgevoerd.

Zie voor een voorbeeld van de weergave wordt geproduceerd door het trainen van een model uitgevoerd, de [Snelstartgids: aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

## <a name="experiment"></a>Experiment

Een experiment is een groepering van veel worden uitgevoerd op een bepaald script. Altijd hoort bij een werkruimte. Wanneer u een uitvoering verzendt, kunt u de naam van een experiment opgeven. Informatie voor de uitvoering wordt onder dit experiment opgeslagen. Als u een uitvoering verzenden en geef de naam van een experiment die niet bestaat, wordt er automatisch een nieuw experiment met die naam gemaakt.

Zie voor een voorbeeld van het gebruik van een experiment, het [Snelstartgids: aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

## <a name="pipeline"></a>Pijplijn

Machine learning-pijplijnen worden gebruikt voor het maken en beheren van werkstromen die samen te voegen machine learning-fasen. Een pijplijn kan bijvoorbeeld gegevens voor te bereiden, modeltraining modelimplementatie en inferentietaken fasen. Elke fase kan meerdere stappen, die kan worden uitgevoerd zonder toezicht in diverse compute-doelen omvatten.

Zie het artikel voor meer informatie over machine learning-pijplijnen met deze service [pijplijnen en Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>COMPUTE-doel

Een compute-doel is de compute-resource die is gebruikt voor het uitvoeren van uw trainingsscript of host voor uw service-implementatie. De ondersteunde compute-doelen zijn: 

| COMPUTE-doel | Training | Implementatie |
| ---- |:----:|:----:|
| Uw lokale computer | ✓ | &nbsp; |
| Een virtuele Linux-machine in Azure</br>(zoals de Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Batch AI-Cluster | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; | &nbsp; |
| Azure Data Lake Analytics | ✓ | &nbsp; |
| Apache Spark voor HDInsight | ✓ | &nbsp; |
| Azure Container Instance | ✓ | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(Field-programmable gate array) | &nbsp; | ✓ |

COMPUTE-doelen zijn gekoppeld aan een werkruimte. COMPUTE-doelen dan de lokale computer worden gedeeld door gebruikers van de werkruimte.

De meeste compute-doelen kunnen rechtstreeks via de werkruimte worden gemaakt met behulp van de Azure portal, Azure Machine Learning-SDK of Azure CLI. Als u de compute-doelen die zijn gemaakt door een ander proces (bijvoorbeeld, de Azure portal of Azure CLI) hebt, kunt u toevoegen (koppelen) deze naar uw werkruimte. Sommige compute-doelen moeten worden gemaakt buiten de werkruimte, en vervolgens gekoppeld.

Zie voor meer informatie over het selecteren van een compute-doel voor de training, de [selecteren en gebruiken van een compute-doel aan uw model te trainen](how-to-set-up-training-targets.md) document.

Zie voor meer informatie over het selecteren van een compute-doel voor de implementatie van de [Implementeer modellen met de service Azure Machine Learning](how-to-deploy-and-where.md) document.

## <a name="run-configuration"></a>Configuratie van de uitvoering

Een uitvoering configuratie is een set met instructies waarmee wordt gedefinieerd hoe een script moet worden uitgevoerd in een bepaalde compute-doel. Het bevat een brede set van definities van gedrag, zoals of een bestaande Python-omgeving gebruiken of een Conda-omgeving die is gebouwd op basis van de specificatie.

Een configuratie uitvoeren kan worden vastgehouden in een bestand in de map met uw trainingsscript of samengesteld als een object in het geheugen en gebruikt voor het indienen van een uitvoering.

Bijvoorbeeld configuraties worden uitgevoerd, Zie de [selecteren en gebruiken van een compute-doel aan uw model te trainen](how-to-set-up-training-targets.md) document.

## <a name="training-script"></a>Trainingsscript

Als u wilt een model te trainen, moet u de map waarin het trainingsscript en de bijbehorende bestanden opgeven. U geeft ook de naam van een experiment, die wordt gebruikt voor het opslaan van informatie die verzameld tijdens de training. Tijdens de training, de gehele map is gekopieerd naar de omgeving instrueren (compute-doel) en het script dat is opgegeven door de configuratie van de uitvoering is gestart. Een momentopname van de map worden ook opgeslagen onder het experiment in de werkruimte.

Zie voor een voorbeeld [een werkruimte maken met Python](quickstart-get-started.md)

## <a name="logging"></a>Logboekregistratie

Bij het ontwikkelen van uw oplossing, gebruikt u de Azure Machine Learning Python SDK in uw Python-script aan te melden willekeurige metrische gegevens. Na de uitvoering query uitvoeren op de metrische gegevens om te bepalen of de uitvoering geproduceerd het model dat u wilt implementeren. 

## <a name="snapshot"></a>Momentopname

Bij het indienen van een uitvoering, wordt de map die het script als een zip-bestand bevat en verzendt dit naar de compute-doel in Azure Machine Learning gecomprimeerd. Het ZIP-bestand vervolgens wordt uitgebreid en het script er wordt uitgevoerd. Azure Machine Learning worden ook het zip-bestand opgeslagen als een momentopname als onderdeel van de record die uitvoeren. Iedereen met toegang tot de werkruimte kan een uitvoerregistratie bladeren en downloaden van de momentopname.

## <a name="activity"></a>Activiteit

Een activiteit vertegenwoordigt een langdurige bewerking. De volgende bewerkingen zijn voorbeelden van activiteiten:

* Maken of verwijderen van een compute-doel
* Een script uitgevoerd op een compute-doel

Activiteiten kunnen meldingen via de SDK of de Webgebruikersinterface bieden, zodat u eenvoudig de voortgang van deze bewerkingen kunt bewaken.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen aan de slag met Azure Machine Learning:

* [Wat is de Azure Machine Learning service](overview-what-is-azure-ml.md)
* [Snelstart: een werkruimte maken met Python](quickstart-get-started.md)
* [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)
