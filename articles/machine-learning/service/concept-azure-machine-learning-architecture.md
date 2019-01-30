---
title: 'Machine learning in de cloud: Voorwaarden en -architectuur'
titleSuffix: Azure Machine Learning service
description: Meer informatie over de architectuur, -terminologie en concepten van Azure Machine Learning-service. Ook leert u over de algemene werkstroom van het gebruik van de service en de Azure-services die worden gebruikt door Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 751a1dc84f81b388a1fffb82cc3dfbc4996eed1f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249626"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Hoe werkt de Azure Machine Learning-service: Architectuur en concepten

In dit artikel beschrijft de architectuur en concepten voor Azure Machine Learning-service. De belangrijkste onderdelen van de service en de algemene werkstroom voor het gebruik van de service worden weergegeven in het volgende diagram: 

[![Azure Machine Learning-service-architectuur en werkstromen](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

De werkstroom volgt in het algemeen in deze volgorde:

1. Machine learning-scripts in Trainingen ontwikkelen **Python**.
1. Maken en configureren van een **compute-doel**.
1. **Verzenden van de scripts** naar de geconfigureerde compute-doel om uit te voeren in die omgeving. Tijdens de training, de scripts kunnen lezen of schrijven naar **gegevensopslag**. En worden de records van de uitvoering van opgeslagen als **wordt uitgevoerd** in de **werkruimte** en gegroepeerd onder **experimenten**.
1. **Query uitvoeren op het experiment** geregistreerde voor metrische gegevens van de huidige en eerdere uitvoeringen. Als de metrische gegevens een gewenste resultaat geven, lus terug naar stap 1 en ze opnieuw testen op uw scripts.
1. Nadat een goede uitvoering wordt gevonden, registreert u het persistente model in de **model register**.
1. Ontwikkel een scoring-script.
1. **Een installatiekopie maken** en registreren in de **installatiekopieregisters**. 
1. **De installatiekopie implementeert** als een **webservice** in Azure.


> [!NOTE]
> Hoewel dit artikel definieert termen en concepten die worden gebruikt door Azure Machine Learning-service, worden niet gedefinieerd door termen en begrippen voor het Azure-platform. Zie voor meer informatie over Azure-platform terminologie, het [verklarende woordenlijst voor Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Werkruimte

De werkruimte is de resource op het hoogste niveau voor Azure Machine Learning-service. Het biedt een centrale locatie voor het werken met alle artefacten die u maakt wanneer u Azure Machine Learning-service.

De werkruimte houdt een lijst met compute-doelen die u gebruiken kunt voor uw model te trainen. Het houdt ook een geschiedenis bij van de training wordt uitgevoerd, met inbegrip van Logboeken, metrische gegevens, uitvoer en een momentopname van uw scripts. U kunt deze informatie gebruiken om te bepalen welke run training levert het beste model.

U registreren modellen bij de werkruimte. U een geregistreerde model en scoring-scripts gebruiken om een installatiekopie te maken. Vervolgens kunt u de installatiekopie naar Azure Container Instances, Azure Kubernetes Service, of naar een veld-programmable gate array (FPGA) implementeren als een HTTP op basis van REST-eindpunt. U kunt ook de installatiekopie naar een Azure IoT Edge-apparaat implementeren als een module.

U kunt meerdere werkruimten maken en elke werkruimte kan worden gedeeld door meerdere personen. Wanneer u een werkruimte deelt, kunt u toegang tot deze kunt beheren door de volgende rollen toewijzen aan gebruikers:

* Eigenaar
* Inzender
* Lezer

Wanneer u een nieuwe werkruimte maakt, wordt automatisch verschillende Azure-resources die worden gebruikt door de werkruimte gemaakt:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Hiermee registreert u docker-containers die u tijdens de training en wanneer u een model implementeert gebruiken.
* [Azure-opslagaccount](https://azure.microsoft.com/services/storage/): Wordt gebruikt als de standaard gegevensopslag voor de werkruimte.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Winkels controlegegevens over uw modellen.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): Winkels geheimen die worden gebruikt door compute-doelen en andere gevoelige informatie die nodig is door de werkruimte.

> [!NOTE]
> Naast het maken van nieuwe versies, kunt u ook bestaande Azure-services gebruiken. 

Een taxonomie van de werkruimte wordt weergegeven in het volgende diagram:

[![Taxonomie van werkruimte](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.svg)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="model"></a>Model

De eenvoudigste is een model een stukje code dat een invoer en uitvoer wordt geproduceerd. Het maken van een machine learning-model omvat het selecteren van een algoritme, voorzien van deze gegevens en afstemmen van hyperparameters. Training is een iteratief proces dat een getraind model produceert, wat het model hebt geleerd tijdens de training ingekapseld.

Een model wordt geproduceerd door een uitvoering in Azure Machine Learning. U kunt ook een model dat buiten Azure Machine Learning wordt getraind. U kunt een model registreren in de werkruimte van een Azure Machine Learning-service.

Azure Machine Learning-service is framework-agnostische. Wanneer u een model maakt, kunt u alle populaire machine learning-frameworks, zoals Scikit-informatie, XGBoost PyTorch, TensorFlow, Chainer en Microsoft Cognitive Toolkit (voorheen bekend als CNTK).

Zie voor een voorbeeld van een model te trainen, [Quick Start: Maken van een werkruimte van de service Machine Learning](quickstart-get-started.md).

### <a name="model-registry"></a>Model-register

Het register model houdt van alle modellen in de werkruimte van uw Azure Machine Learning-service. 

Modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren het register wordt ervan uitgegaan dat er een nieuwe versie beschikbaar is. De versie wordt verhoogd en het nieuwe model is geregistreerd onder dezelfde naam.

Als u het model registreert, kunt u aanvullende metagegevenstags bieden en vervolgens de codes gebruiken wanneer u een voor modellen zoekopdracht.

U kunt modellen die worden gebruikt door een installatiekopie niet verwijderen.

Zie voor een voorbeeld van het registreren van een model [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md).

## <a name="image"></a>Installatiekopie

Afbeeldingen bieden een manier om op betrouwbare wijze een model, samen met alle onderdelen die u wilt gebruiken van het model te implementeren. Een installatiekopie bevat de volgende items:

* Een model.
* Een scoring-script of toepassing. U gebruikt het script moet doorgeven van invoer voor het model en de uitvoer van het model geretourneerd.
* De afhankelijkheden die nodig zijn voor het model of scoring-script of de toepassing. U kunt bijvoorbeeld een Conda-omgeving-bestand met een lijst met Python-pakketafhankelijkheden opnemen.

Azure Machine Learning kunt twee soorten afbeeldingen maken:

* **FPGA image**: Wanneer u op een veld-programmable gate array in Azure implementeert gebruikt.
* **Docker-installatiekopie**: Wanneer u implementeert voor compute-doelen dan FPGA gebruikt. Voorbeelden zijn Azure Container Instances en Azure Kubernetes Service.

Zie voor een voorbeeld van het maken van een installatiekopie van een [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Afbeelding register

Houdt het installatiekopieregister van de installatiekopieën die zijn gemaakt op basis van uw modellen. Wanneer u de installatiekopie maakt, kunt u aanvullende metagegevenstags bieden. METADATA-codes worden opgeslagen door het installatiekopieregister en kunt u zoeken om uw installatiekopie vinden.

## <a name="deployment"></a>Implementatie

Een implementatie is een instantie van uw installatiekopie in een van beide een webservice die kan worden gehost in de cloud of een IoT-module voor implementaties van geïntegreerde apparaat. 

### <a name="web-service"></a>Webservice

Een geïmplementeerde webservice kunt gebruiken voor Azure Container Instances, Azure Kubernetes Service of FPGA's. U maken de service van een afbeelding die uw model, scripts en bijbehorende bestanden bevat. De afbeelding heeft een gelijke, HTTP-eindpunt dat scoring aanvragen die worden verzonden naar de webservice ontvangt.

Azure helpt u bij de implementatie van uw web-service controleren door het verzamelen van telemetrie van Application Insights- of model telemetrie, als u ervoor hebt gekozen om in te schakelen van deze functie. De telemetriegegevens die zijn alleen toegankelijk is voor u en deze opgeslagen in uw Application Insights en storage-account instanties.

Als u hebt ingeschakeld om automatisch schalen, schaalt Azure automatisch de implementatie.

Zie voor een voorbeeld van het implementeren van een model als een webservice, [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>IoT-module

Een geïmplementeerde IoT-module is een Docker-container met het model en bijbehorende script of toepassing en eventuele extra afhankelijkheden. U kunt deze modules implementeren met behulp van Azure IoT Edge op Edge-apparaten. 

Als u de bewaking hebt ingeschakeld, verzamelt telemetriegegevens op Azure uit het model in de Azure IoT Edge-module. De telemetriegegevens die zijn alleen toegankelijk is voor u en deze opgeslagen in uw exemplaar van storage-account.

Azure IoT Edge zorgt ervoor dat de module wordt uitgevoerd en deze controleert het apparaat waarop deze wordt gehost.

## <a name="datastore"></a>Gegevensopslag

Een gegevensarchief is een abstractie storage via Azure storage-account. Het gegevensarchief kan een Azure blob-container of een Azure-bestandsshare als back-end opslag gebruiken. Elke werkruimte heeft een standaard-gegevensarchief en u kunt aanvullende gegevensopslag registreren. 

De Python SDK-API of de Azure Machine Learning CLI gebruiken voor het opslaan en ophalen van bestanden van het gegevensarchief. 

## <a name="run"></a>Voer

Een uitvoering is een record met de volgende informatie:

* Metagegevens over de uitvoering (tijdstempel, duur, enzovoort)
* Metrische gegevens die worden vastgelegd door het script
* Uitvoerbestanden autocollected door het experiment of expliciet geüpload door u
* Een momentopname van de map waarin uw scripts, voordat de uitvoering

U maken een uitvoering wanneer u een script voor een model te trainen verzendt. Een uitvoering kan nul of meer onderliggende uitvoeringen hebben. De op het hoogste niveau uitvoeren mogelijk bijvoorbeeld twee onderliggende-wordt uitgevoerd, die elk een eigen onderliggende uitvoeren mogelijk.

Zie voor een voorbeeld van het weergeven van uitvoeringen die worden geproduceerd door het trainen van een model [Quick Start: Aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

## <a name="experiment"></a>Experiment

Een experiment is een groepering van veel worden uitgevoerd op een opgegeven-script. Altijd hoort bij een werkruimte. Wanneer u een uitvoering verzendt, kunt u de naam van een experiment opgeven. Informatie voor de uitvoering wordt onder dit experiment opgeslagen. Als u een uitvoering verzenden en geef de naam van een experiment die niet bestaat, wordt er automatisch een nieuw experiment met de zojuist opgegeven naam gemaakt.

Zie voor een voorbeeld van het gebruik van een experiment [Quick Start: Aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

## <a name="pipeline"></a>Pijplijn

Gebruik van machine learning pijplijnen maken en beheren van werkstromen die samen te voegen voor machine learning-fasen. Een pijplijn kan bijvoorbeeld gegevens voor te bereiden, modeltraining modelimplementatie en inferentietaken fasen. Elke fase kan meerdere stappen, die kan worden uitgevoerd zonder toezicht in diverse compute-doelen omvatten.

Zie voor meer informatie over machine learning-pijplijnen met deze service [pijplijnen en Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>COMPUTE-doel

Een compute-doel is de compute-resource die u kunt uw trainingsscript uitgevoerd of host voor uw service-implementatie. De ondersteunde compute-doelen zijn: 

| COMPUTE-doel | Training | Implementatie |
| ---- |:----:|:----:|
| Uw lokale computer | ✓ | &nbsp; |
| Azure Machine Learning-Computing | ✓ | &nbsp; |
| Een virtuele Linux-machine in Azure</br>(zoals de Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; | &nbsp; |
| Azure Data Lake Analytics | ✓ | &nbsp; |
| Apache Spark voor HDInsight | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(Field-programmable gate array) | &nbsp; | ✓ |

COMPUTE-doelen zijn gekoppeld aan een werkruimte. COMPUTE-doelen dan de lokale computer worden gedeeld door gebruikers van de werkruimte.

### <a name="managed-and-unmanaged-compute-targets"></a>Beheerde en onbeheerde compute-doelen

* **Beheerde**: COMPUTE-doelen die worden gemaakt en beheerd door Azure Machine Learning-service. Deze compute-doelen zijn geoptimaliseerd voor workloads van machine learning. Azure Machine Learning-Computing is de enige beheerde compute-doel vanaf 4 December 2018. Aanvullende beheerde compute-doelen kunnen in de toekomst worden toegevoegd. 

    U kunt machine learning maken rekenprocessen rechtstreeks via de werkruimte met behulp van de Azure portal, de Azure Machine Learning-SDK of de Azure CLI. Alle andere compute-doelen moeten worden gemaakt buiten de werkruimte en vervolgens gekoppeld.

* **Niet-beheerde**: COMPUTE-doelen die zijn *niet* beheerd door Azure Machine Learning-service. Mogelijk moet u ze buiten Azure Machine Learning te maken en koppelt u ze aan uw werkruimte vóór gebruik. Niet-beheerde compute-doelen kunnen extra stappen moeten worden voor u te houden of om de prestaties voor machine learning-workloads te verbeteren.

Zie voor meer informatie over het selecteren van een compute-doel voor de training [selecteren en gebruiken van een compute-doel aan uw model te trainen](how-to-set-up-training-targets.md).

Zie voor meer informatie over het selecteren van een compute-doel voor de implementatie van de [Implementeer modellen met Azure Machine Learning-service](how-to-deploy-and-where.md).

## <a name="run-configuration"></a>Configuratie van de uitvoering

Een uitvoering configuratie is een set met instructies waarmee wordt gedefinieerd hoe een script moet worden uitgevoerd in een opgegeven compute-doel. De configuratie omvat een brede reeks definities van gedrag, zoals of een bestaande Python-omgeving of een Conda-omgeving die wordt afgeleid van een specificatie van het gebruik.

Een uitvoeren-configuratie kunt permanent worden opgeslagen in een bestand in de map waarin het trainingsscript, of kan worden samengesteld als een object in het geheugen en gebruikt voor het indienen van een uitvoering.

Bijvoorbeeld configuraties worden uitgevoerd, Zie [selecteren en gebruiken van een compute-doel aan uw model te trainen](how-to-set-up-training-targets.md).

## <a name="training-script"></a>Trainingsscript

Als u wilt een model te trainen, moet u de map waarin het trainingsscript en de bijbehorende bestanden opgeven. U geeft ook de naam van een experiment, die wordt gebruikt voor het opslaan van gegevens die zijn verzameld tijdens de training. Tijdens de training, de gehele map is gekopieerd naar de omgeving instrueren (compute-doel) en het script dat opgegeven door de configuratie van de uitvoering is gestart. Een momentopname van de map worden ook opgeslagen onder het experiment in de werkruimte.

Zie voor een voorbeeld [een werkruimte maken met Python](quickstart-get-started.md).

## <a name="logging"></a>Logboekregistratie

Wanneer u uw oplossing ontwikkelt, gebruikt u de Azure Machine Learning Python SDK in uw Python-script aan te melden willekeurige metrische gegevens. Na de uitvoering query uitvoeren op de metrische gegevens om te bepalen of het model dat u wilt implementeren door de uitvoering zijn geproduceerd. 

## <a name="snapshot"></a>Momentopname

Wanneer u een uitvoering verzendt, wordt de map die het script als een zip-bestand bevat en verzendt dit naar de compute-doel in Azure Machine Learning gecomprimeerd. Het zip-bestand wordt vervolgens opgehaald en wordt er in het script worden uitgevoerd. Azure Machine Learning worden ook het zip-bestand opgeslagen als een momentopname als onderdeel van de record die uitvoeren. Iedereen met toegang tot de werkruimte kan een uitvoerregistratie bladeren en downloaden van de momentopname.

## <a name="activity"></a>Activiteit

Een activiteit vertegenwoordigt een langdurige bewerking. De volgende bewerkingen zijn voorbeelden van activiteiten:

* Maken of verwijderen van een compute-doel
* Een script uitgevoerd op een compute-doel

Activiteiten kunnen meldingen via de SDK of de web-UI bieden, zodat u eenvoudig de voortgang van deze bewerkingen kunt bewaken.

## <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met Azure Machine Learning-service, Zie:

* [Wat is Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Snelstart: een werkruimte maken met Python](quickstart-get-started.md)
* [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)
