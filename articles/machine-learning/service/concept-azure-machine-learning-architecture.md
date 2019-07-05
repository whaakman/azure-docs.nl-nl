---
title: Architectuur en belangrijke concepten
titleSuffix: Azure Machine Learning service
description: Meer informatie over de architectuur, voorwaarden, concepten en werkstroom van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/15/2019
ms.custom: seodec18
ms.openlocfilehash: 2196e375db582202997b838d05c902db95b3a3ad
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461470"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Hoe werkt de Azure Machine Learning-service: Architectuur en concepten

Meer informatie over de architectuur, de concepten en de werkstroom voor Azure Machine Learning-service. De belangrijkste onderdelen van de service en de algemene werkstroom voor het gebruik van de service worden weergegeven in het volgende diagram:

![Azure Machine Learning-service-architectuur en werkstromen](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>Werkstroom

Deze reeks met de machine learning-model Werkstroom in het algemeen de volgende:

1. **Train**
    + Machine learning-scripts in Trainingen ontwikkelen **Python** of met de visuele interface.
    + Maken en configureren van een **compute-doel**.
    + **Verzenden van de scripts** naar de geconfigureerde compute-doel om uit te voeren in die omgeving. Tijdens de training, de scripts kunnen lezen of schrijven naar **gegevensopslag**. En worden de records van de uitvoering van opgeslagen als **wordt uitgevoerd** in de **werkruimte** en gegroepeerd onder **experimenten**.

1. **Pakket** : nadat een goede uitvoering wordt gevonden, Registreer het persistente model in de **model register**.

1. **Valideren** - **Query van het experiment** geregistreerde voor metrische gegevens van de huidige en eerdere uitvoeringen. Als de metrische gegevens een gewenste resultaat geven, lus terug naar stap 1 en ze opnieuw testen op uw scripts.

1. **Implementeren** -een scoring-script dat gebruikmaakt van het model te ontwikkelen en **het model implementeren** als een **webservice** in Azure of naar een **IoT Edge-apparaat**.

1. **Monitor** -Monitor **gegevens drift** tussen de trainingsgegevens voor gegevensset en Deductie van een geïmplementeerd model. Indien nodig, in een lus terug naar stap 1 om het opnieuw trainen het model met nieuwe trainingsgegevens.

## <a name="tools-for-azure-machine-learning"></a>Hulpprogramma's voor Azure Machine Learning 

Gebruik deze hulpprogramma's voor Azure Machine Learning:

+  Communiceren met de service in een Python-omgeving met de [Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
+ Automatiseer uw machine learning-activiteiten met de [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli).
+ Schrijf code in Visual Studio Code met [Azure Machine Learning VS Code-extensie](how-to-vscode-tools.md) 
+ Gebruik de [visuele interface (preview) voor Azure Machine Learning-service](ui-concept-visual-interface.md) om uit te voeren van de werkstroomstappen zonder code te schrijven.

## <a name="glossary-of-concepts"></a>Verklarende woordenlijst van concepten

+ <a href="#workspaces">Workspace</a>
+ <a href="#experiments">Experimenten</a>
+ <a href="#models">Models</a>
+ <a href="#run-configurations">Configuratie van de uitvoering</a>
+ <a href="#datasets-and-datastores">Gegevensset en gegevensopslag</a>
+ <a href="#compute-targets">COMPUTE-doelen</a>
+ <a href="#training-scripts">Trainingsscript</a>
+ <a href="#runs">Uitvoeren</a>
+ <a href="#github-tracking-and-integration">GIT bijhouden</a>
+ <a href="#snapshots">momentopname</a>
+ <a href="#activities">Activiteit</a>
+ <a href="#images">Installatiekopie</a>
+ <a href="#deployment">Implementatie</a>
+ <a href="#web-service-deployments">Webservices</a>
+ <a href="#iot-module-deployments">IoT-modules</a>
+ <a href="#ml-pipelines">ML-pijplijnen</a>
+ <a href="#logging">Logging</a>

> [!NOTE]
> Hoewel dit artikel definieert termen en concepten die worden gebruikt door Azure Machine Learning-service, worden niet gedefinieerd door termen en begrippen voor het Azure-platform. Zie voor meer informatie over Azure-platform terminologie, het [verklarende woordenlijst voor Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).


### <a name="workspaces"></a>Workspaces

[De werkruimte](concept-workspace.md) is de resource op het hoogste niveau voor Azure Machine Learning-service. Het biedt een centrale locatie voor het werken met alle artefacten die u maakt wanneer u Azure Machine Learning-service.

Een taxonomie van de werkruimte wordt weergegeven in het volgende diagram:

[![Taxonomie van werkruimte](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Zie voor meer informatie over werkruimten [wat is een Azure Machine Learning-werkruimte?](concept-workspace.md).

### <a name="experiments"></a>Experimenten

Een experiment is een groepering van veel worden uitgevoerd op een opgegeven-script. Altijd hoort bij een werkruimte. Wanneer u een uitvoering verzendt, kunt u de naam van een experiment opgeven. Informatie voor de uitvoering wordt onder dit experiment opgeslagen. Als u een uitvoering verzenden en geef de naam van een experiment die niet bestaat, wordt er automatisch een nieuw experiment met de zojuist opgegeven naam gemaakt.

Zie voor een voorbeeld van het gebruik van een experiment [Quick Start: Aan de slag met Azure Machine Learning-service](quickstart-run-cloud-notebook.md).

### <a name="models"></a>Modellen

De eenvoudigste is een model een stukje code dat een invoer en uitvoer wordt geproduceerd. Het maken van een machine learning-model omvat het selecteren van een algoritme, voorzien van deze gegevens en afstemmen van hyperparameters. Training is een iteratief proces dat een getraind model produceert, wat het model hebt geleerd tijdens de training ingekapseld.

Een model wordt geproduceerd door een uitvoering in Azure Machine Learning. U kunt ook een model dat buiten Azure Machine Learning wordt getraind. U kunt een model registreren in de werkruimte van een Azure Machine Learning-service.

Azure Machine Learning-service is framework-agnostische. Wanneer u een model maakt, kunt u alle populaire machine learning-frameworks, zoals Scikit-informatie, XGBoost PyTorch, TensorFlow en Chainer.

Zie voor een voorbeeld van een model te trainen, [zelfstudie: Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning Service](tutorial-train-models-with-aml.md).

De **model register** houdt van alle modellen in de werkruimte van uw Azure Machine Learning-service.

Modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren het register wordt ervan uitgegaan dat er een nieuwe versie beschikbaar is. De versie wordt verhoogd en het nieuwe model is geregistreerd onder dezelfde naam.

Als u het model registreert, kunt u aanvullende metagegevenstags bieden en vervolgens de codes gebruiken wanneer u een voor modellen zoekopdracht.

> [!TIP]
> Een geregistreerde model is een logische container voor een of meer bestanden die gezamenlijk uw model. Bijvoorbeeld, als u een model dat is opgeslagen in meerdere bestanden hebt, kunt u registreren deze als één model in uw Azure Machine Learning-werkruimte. Na de registratie, kunt u vervolgens downloaden of het geregistreerde model implementeren en ontvangen van alle bestanden die zijn geregistreerd.

U kunt een geregistreerde model dat wordt gebruikt door een actieve implementatie niet verwijderen.

Zie voor een voorbeeld van het registreren van een model [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md).

### <a name="run-configurations"></a>Configuraties uitvoeren

Een uitvoering configuratie is een set met instructies waarmee wordt gedefinieerd hoe een script moet worden uitgevoerd in een opgegeven compute-doel. De configuratie omvat een brede reeks definities van gedrag, zoals of een bestaande Python-omgeving of een Conda-omgeving die wordt afgeleid van een specificatie van het gebruik.

Een uitvoeren-configuratie kunt permanent worden opgeslagen in een bestand in de map waarin het trainingsscript, of kan worden samengesteld als een object in het geheugen en gebruikt voor het indienen van een uitvoering.

Bijvoorbeeld configuraties worden uitgevoerd, Zie [selecteren en gebruiken van een compute-doel aan uw model te trainen](how-to-set-up-training-targets.md).

### <a name="datasets-and-datastores"></a>Gegevenssets en gegevensopslag

**Azure Machine Learning gegevenssets** (preview) maken het gemakkelijker voor toegang tot en met uw gegevens kunt werken. Gegevenssets beheren van gegevens in verschillende scenario's zoals modeltraining en pijplijn maken. Met de Azure Machine Learning-SDK, kunt u toegang tot de onderliggende opslag, verkennen en gegevens voorbereiden, beheren van de levenscyclus van verschillende definities van de gegevensset en vergelijken tussen gegevenssets die worden gebruikt in training en in productie.

Gegevenssets biedt methoden voor het werken met gegevens in populaire indelingen, zoals het gebruik van `from_delimited_files()` of `to_pandas_dataframe()`.

Zie voor meer informatie, [maken en registreren Azure Machine Learning gegevenssets](how-to-create-register-datasets.md).  Zie voor meer voorbeelden van gegevenssets met de [voorbeeld notitieblokken](https://aka.ms/dataset-tutorial).

Een **gegevensopslag** is een abstractie van opslag via een Azure storage-account. Het gegevensarchief kan een Azure blob-container of een Azure-bestandsshare als back-end opslag gebruiken. Elke werkruimte heeft een standaard-gegevensarchief en u kunt aanvullende gegevensopslag registreren. De Python SDK-API of de Azure Machine Learning CLI gebruiken voor het opslaan en ophalen van bestanden van het gegevensarchief.

### <a name="compute-targets"></a>COMPUTE-doelen

Een [compute-doel](concept-compute-target.md) kunt u om op te geven van de compute-resource waar u uw service-implementatie uw trainingsscript of de host uitvoeren. Deze locatie mogelijk op uw lokale computer of een cloud-gebaseerde compute-resource. COMPUTE-doelen kunnen eenvoudig uw compute-omgeving wijzigen zonder uw code te wijzigen. 

Meer informatie over de [beschikbare compute-doelen voor training en implementatie](concept-compute-target.md). 

### <a name="training-scripts"></a>Trainingsscripts

Als u wilt een model te trainen, moet u de map waarin het trainingsscript en de bijbehorende bestanden opgeven. U geeft ook de naam van een experiment, die wordt gebruikt voor het opslaan van gegevens die zijn verzameld tijdens de training. Tijdens de training, de gehele map is gekopieerd naar de omgeving instrueren (compute-doel) en het script dat opgegeven door de configuratie van de uitvoering is gestart. Een momentopname van de map worden ook opgeslagen onder het experiment in de werkruimte.

Zie [Zelfstudie: Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning Service](tutorial-train-models-with-aml.md).

### <a name="runs"></a>Uitvoerbewerkingen

Een uitvoering is een record met de volgende informatie:

* Metagegevens over de uitvoering (tijdstempel, duur, enzovoort)
* Metrische gegevens die worden vastgelegd door het script
* Uitvoerbestanden autocollected door het experiment of expliciet geüpload door u
* Een momentopname van de map waarin uw scripts, voordat de uitvoering

U maken een uitvoering wanneer u een script voor een model te trainen verzendt. Een uitvoering kan nul of meer onderliggende uitvoeringen hebben. De op het hoogste niveau uitvoeren mogelijk bijvoorbeeld twee onderliggende-wordt uitgevoerd, die elk een eigen onderliggende uitvoeren mogelijk.

Zie voor een voorbeeld van het weergeven van uitvoeringen die worden geproduceerd door het trainen van een model [Quick Start: Aan de slag met Azure Machine Learning-service](quickstart-run-cloud-notebook.md).

### <a name="github-tracking-and-integration"></a>GitHub bijhouden en integratie

Wanneer u een training uitgevoerd waarbij de bronmap is een lokale Git-opslagplaats start, wordt informatie over de opslagplaats opgeslagen in de uitvoeringsgeschiedenis. Bijvoorbeeld, wordt de huidige doorvoer-ID voor de opslagplaats die vastgelegd als onderdeel van de geschiedenis. Dit werkt met uitvoeringen die zijn verzonden met behulp van een estimator, ML-pijplijn of script uitvoeren. Het werkt ook voor ingediend vanaf de SDK of de Machine Learning CLI worden uitgevoerd.

### <a name="snapshots"></a>Momentopnamen

Wanneer u een uitvoering verzendt, wordt de map die het script als een zip-bestand bevat en verzendt dit naar de compute-doel in Azure Machine Learning gecomprimeerd. Het zip-bestand wordt vervolgens opgehaald en wordt er in het script worden uitgevoerd. Azure Machine Learning worden ook het zip-bestand opgeslagen als een momentopname als onderdeel van de record die uitvoeren. Iedereen met toegang tot de werkruimte kan een uitvoerregistratie bladeren en downloaden van de momentopname.

> [!NOTE]
> Om te voorkomen dat onnodige bestanden worden opgenomen in de momentopname, moet u een bestand negeren (.gitignore of .amlignore). Plaats dit bestand in de momentopname-map en voeg de bestandsnamen moeten worden genegeerd in deze toe. Het bestand .amlignore maakt gebruik van dezelfde [syntaxis en patronen als het bestand .gitignore](https://git-scm.com/docs/gitignore). Als beide bestanden bestaat, wordt het bestand .amlignore voorrang.

### <a name="activities"></a>Activiteiten

Een activiteit vertegenwoordigt een langdurige bewerking. De volgende bewerkingen zijn voorbeelden van activiteiten:

* Maken of verwijderen van een compute-doel
* Een script uitgevoerd op een compute-doel

Activiteiten kunnen meldingen via de SDK of de web-UI bieden, zodat u eenvoudig de voortgang van deze bewerkingen kunt bewaken.

### <a name="images"></a>Installatiekopieën

Afbeeldingen bieden een manier om op betrouwbare wijze een model, samen met alle onderdelen die u wilt gebruiken van het model te implementeren. Een installatiekopie bevat de volgende items:

* Een model.
* Een scoring-script of toepassing. U gebruikt het script moet doorgeven van invoer voor het model en de uitvoer van het model geretourneerd.
* De afhankelijkheden die nodig zijn voor het model of scoring-script of de toepassing. U kunt bijvoorbeeld een Conda-omgeving-bestand met een lijst met Python-pakketafhankelijkheden opnemen.

Azure Machine Learning kunt twee soorten afbeeldingen maken:

* **FPGA image**: Wanneer u op een veld-programmable gate array in Azure implementeert gebruikt.
* **Docker-installatiekopie**: Wanneer u implementeert voor compute-doelen dan FPGA gebruikt. Voorbeelden zijn Azure Container Instances en Azure Kubernetes Service.

De Azure Machine Learning-service biedt een basisinstallatiekopie, wordt standaard gebruikt. U kunt ook uw eigen aangepaste installatiekopieën opgeven.

### <a name="image-registry"></a>Afbeelding register

Afbeeldingen zijn opgenomen in de catalogus de **installatiekopieregisters** in uw werkruimte. U kunt aanvullende metagegevenstags opgeven bij het maken van de afbeelding, zodat u kunt ze later om uw installatiekopie een query.

Zie voor een voorbeeld van het maken van een installatiekopie van een [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Zie voor een voorbeeld van het implementeren van een model met een aangepaste installatiekopie [over het implementeren van een model met behulp van een aangepaste Docker-installatiekopie](how-to-deploy-custom-docker-image.md).

### <a name="deployment"></a>Implementatie

Een implementatie is een instantie van uw model in een van beide een webservice die kan worden gehost in de cloud of een IoT-module voor implementaties van geïntegreerde apparaat.

#### <a name="web-service-deployments"></a>Webservice-implementaties

Een geïmplementeerde webservice kunt gebruiken voor Azure Container Instances, Azure Kubernetes Service of FPGA's. U maakt de service van uw model, scripts en bijbehorende bestanden. Deze worden ingekapseld in een afbeelding, waarmee u de runtime-omgeving voor de webservice. De afbeelding heeft een gelijke, HTTP-eindpunt dat scoring aanvragen die worden verzonden naar de webservice ontvangt.

Azure helpt u bij de implementatie van uw web-service controleren door het verzamelen van telemetrie van Application Insights- of model telemetrie, als u ervoor hebt gekozen om in te schakelen van deze functie. De telemetriegegevens die zijn alleen toegankelijk is voor u en deze opgeslagen in uw Application Insights en storage-account instanties.

Als u hebt ingeschakeld om automatisch schalen, schaalt Azure automatisch de implementatie.

Zie voor een voorbeeld van het implementeren van een model als een webservice, [implementeren een installatiekopie classificeringsmodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-deployments"></a>IoT-module-implementaties

Een geïmplementeerde IoT-module is een Docker-container met het model en bijbehorende script of toepassing en eventuele extra afhankelijkheden. U kunt deze modules implementeren met behulp van Azure IoT Edge op edge-apparaten.

Als u de bewaking hebt ingeschakeld, verzamelt telemetriegegevens op Azure uit het model in de Azure IoT Edge-module. De telemetriegegevens die zijn alleen toegankelijk is voor u en deze opgeslagen in uw exemplaar van storage-account.

Azure IoT Edge zorgt ervoor dat de module wordt uitgevoerd en deze controleert het apparaat waarop deze wordt gehost.

### <a name="ml-pipelines"></a>ML-pijplijnen

Gebruik van machine learning pijplijnen maken en beheren van werkstromen die samen te voegen voor machine learning-fasen. Een pijplijn kan bijvoorbeeld gegevens voor te bereiden, modeltraining modelimplementatie en Deductie/scoren fasen. Elke fase kan meerdere stappen, die kan worden uitgevoerd zonder toezicht in diverse compute-doelen omvatten.

Zie voor meer informatie over machine learning-pijplijnen met deze service [pijplijnen en Azure Machine Learning](concept-ml-pipelines.md).

### <a name="logging"></a>Logboekregistratie

Wanneer u uw oplossing ontwikkelt, gebruikt u de Azure Machine Learning Python SDK in uw Python-script aan te melden willekeurige metrische gegevens. Na de uitvoering query uitvoeren op de metrische gegevens om te bepalen of het model dat u wilt implementeren door de uitvoering zijn geproduceerd.

### <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met Azure Machine Learning-service, Zie:

* [Wat is Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Een Azure Machine Learning-service-werkruimte maken](setup-create-workspace.md)
* [Zelfstudie (deel 1): een model trainen](tutorial-train-models-with-aml.md)
