---
title: Wat is Azure Machine Learning? | Microsoft Docs
description: "Overzicht van Azure Machine Learning Experimenten en Modelbeheer, een geïntegreerde end-to-end gegevenswetenschapoplossing voor professionele gegevenswetenschappers waarmee ze geavanceerde analyseapplicaties kunnen ontwikkelen, ermee kunnen experimenteren en ze op cloudschaal kunnen implementeren."
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.openlocfilehash: 3bf9227a7ee432f036c57dd9d1c3807c7a867f3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-machine-learning"></a>Wat is Azure Machine Learning?

Azure Machine Learning is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse. Hiermee kunt gegevenswetenschappers gegevens voorbereiden, experimenten ontwikkelen en modellen implementeren op cloudschaal.

De belangrijkste onderdelen van Azure Machine Learning zijn:
- Azure Machine Learning Workbench
- Azure Machine Learning Experimenten-service
- Azure Machine Learning Modelbeheer-service
- Microsoft Machine Learning-bibliotheken voor Apache Spark (MMLSpark-bibliotheek)
- Visual Studio Code-hulpprogramma's voor AI

Samen helpen deze toepassingen en services het ontwikkelen en implementeren van uw gegevenswetenschapsprojecten aanzienlijk versnellen. 

![Concepten voor Azure Machine Learning](media/overview-what-is-azure-ml/aml-concepts.png)

## <a name="open-source-compatible"></a>Compatibel met open source

Azure Machine Learning biedt volledige ondersteuning voor open-source-technologieën. U kunt tienduizenden open-source Python-pakketten gebruiken, zoals de volgende machine learning-frameworks:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

U kunt uw experimenten uitvoeren in beheerde omgevingen zoals Docker-containers en Spark-clusters. U kunt ook geavanceerde hardware, zoals [virtuele machines met GPU in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu) (Engelstalig artikel), gebruiken om de uitvoering te versnellen.

Azure Machine Learning is gebouwd op basis van de volgende open-source-technologieën:

- [Jupyter Notebook](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Het omvat ook de open-source technologieën van Microsoft zelf, zoals de [Machine Learning-bibliotheek van Microsoft voor Apache Spark](https://github.com/Azure/mmlspark) en Cognitive Toolkit.

Bovendien profiteert u van enkele van de meest geavanceerde, beproefde en betrouwbare machine learning-technologieën die zijn ontwikkeld door Microsoft voor het oplossen van grootschalige problemen. Deze zijn zwaar beproefd in veel Microsoft-producten, zoals:

- Windows
- Bing
- Xbox
- Office
- SQL Server

Hier volgen enkele van de Microsoft-machine learning-technologieën in Azure Machine Learning:

- [PROSE](https://microsoft.github.io/prose/) (PROgram Synthesis using Examples, programmasynthese met behulp van voorbeelden)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench
Azure Machine Learning Workbench bestaat uit een bureaubladtoepassing plus opdrachtregelprogramma's, en wordt ondersteund op Windows en macOS. Met de Workbench kunt u machine learning-oplossingen beheren gedurende de gehele gegevenswetenschappelijke levenscyclus:

- Gegevensopname en -voorbereiding
- Modelontwikkeling en experimentbeheer
- Modelimplementatie in verschillende doelomgevingen

Hier volgen de belangrijkste functies die Azure Machine Learning Workbench biedt:

- Hulpprogramma voor gegevensvoorbereiding dat gegevenstransformatielogica kan leren op basis van voorbeelden.
- Gegevensbronabstractie die toegankelijk is via UX en Python-code.
- Python-SDK voor het aanroepen van visueel samengestelde gegevensvoorbereidingspakketten.
- Ingebouwde Jupyter Notebook-service en client-UX.
- Controle en beheer van experimenten via uitvoeringsgeschiedenisweergaven.
- Op rollen gebaseerd toegangsbeheer voor delen en samenwerking via Azure Active Directory.
- Automatische projectmomentopnamen voor elke uitvoering, en expliciet versiebeheer dat mogelijk wordt gemaakt door systeemeigen Git-integratie. 
- Integratie met populaire Python IDE's.

Raadpleeg voor meer informatie de volgende Engelstalige artikelen:
- [Data Preparation User Guide](data-prep-user-guide.md)
- [Using Git with Azure Machine Learning](using-git-ml-project.md)
- [Using Jupyter Notebook in Azure Machine Learning](how-to-use-jupyter-notebooks.md)
- Roaming and Sharing
- [Run History Guide](how-to-use-run-history-model-metrics.md)
- [IDE Integration](how-to-configure-your-IDE.md)

## <a name="azure-machine-learning-experimentation-service"></a>Azure Machine Learning Experimenten-service
De Experimenten-service verzorgt de uitvoering van machine learning-experimenten. De service ondersteunt ook de Workbench door projectbeheer, Git-integratie, toegangsbeheer, roaming en delen. 

Via een eenvoudige configuratie kunt u uw experimenten uitvoeren in tal van compute-omgevingen:

- Lokale installatie
- Lokale Docker-container
- Docker-container op een externe VM
- Uitschaalbaar Spark-cluster in Azure

De Experimenten-service construeert virtuele omgevingen om ervoor te zorgen dat uw script geïsoleerd kan worden uitgevoerd met reproduceerbare resultaten. De gegevens van de uitvoeringsgeschiedenis worden vastgelegd en de geschiedenis wordt visueel aan u gepresenteerd. U kunt gemakkelijk het beste model selecteren uit de uitvoeringen van uw experiment. 

Raadpleeg voor meer informatie [Experimentation Execution Configuration](experiment-execution-configuration.md) (Configuratie voor het uitvoeren van experimenten).

## <a name="azure-machine-learning-model-management-service"></a>Azure Machine Learning Modelbeheer-service

Met de Modelbeheer-service kunnen gegevenswetenschappers en ontwikkelteams voorspellende modellen implementeren in allerlei verschillende omgevingen. Modelversies en afkomst worden getraceerd, van trainingsuitvoeringen tot implementaties. Modellen worden opgeslagen, geregistreerd en beheerd in de cloud. 

Met behulp van eenvoudige CLI-opdrachten kunt u uw model, scorescripts en afhankelijkheden containeriseren in Docker-installatiekopieën. Deze installatiekopieën worden geregistreerd in uw eigen Docker-register, gehost in Azure (Azure Container Registry). Ze kunnen op betrouwbare wijze worden geïmplementeerd naar de volgende doelen:

- Lokale machines
- On-premises servers
- De cloud
- IoT Edge-apparaten

Kubernetes uitgevoerd in ACS (Azure Container Service) wordt gebruikt voor scale-out-implementatie in de cloud. Modeluitvoeringstelemetrie wordt in AppInsights vastgelegd voor visuele analyse. 

Raadpleeg voor meer informatie over de Modelbeheer-service [Overzicht van Modelbeheer](model-management-overview.md)


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning-bibliotheek voor Apache Spark

De [MMLSpark](https://github.com/Azure/mmlspark)-bibliotheek (Microsoft Machine Learning-bibliotheek voor Apache Spark) is een open source Spark-pakket met hulpmiddelen voor deep learning en gegevenswetenschap voor Apache Spark. De bibliotheek integreert [Spark Machine Learning-pijplijnen](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) met de [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) en [OpenCV](http://opencv.org/)-bibliotheek. Met MMLSpark kunt u snel krachtige, zeer schaalbare voorspellende en analytische modellen maken voor grote gegevenssets met afbeeldingen en tekst. Enkele hoogtepunten zijn:

- Eenvoudig afbeeldingen uit HDFS opnemen in Spark DataFrame
- Afbeeldingsgegevens voorbewerken met behulp van transformaties uit OpenCV
- Parametriseer afbeeldingen met vooraf getrainde neurale deep learning-netwerken (DNN’s) met behulp van de Microsoft Cognitive Toolkit 
- Vooraf getrainde bidirectionele LSTM's van Keras gebruiken voor extractie van medische entiteiten
- Op DNN gebaseerde afbeeldingsclassificatiemodellen trainen op N-serie GPU-VM's in Azure
- Vrije-tekstgegevens parametriseren met behulp van handige API's op primitieven in SparkML via één transformator
- Gemakkelijk classificatie- en regressiemodellen trainen via impliciete parametrisatie van gegevens
- Een uitgebreide set metrische evaluatiegegevens berekenen, waaronder metrische gegevens per instantie

Voor meer informatie raadpleegt u [Using MMLSpark in Azure Machine Learning](how-to-use-mmlspark.md) (MMLSpark gebruiken in Azure Machine Learning).

## <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code-hulpprogramma's voor AI
Visual Studio Code-hulpprogramma's voor AI is een extensie in Visual Studio Code voor het bouwen, testen en implementeren van deep learning- en AI-oplossingen. Het heeft veel integratiepunten met Azure Machine Learning, waaronder:
- Een uitvoeringsgeschiedenis voor het weergeven van de prestaties van trainingsuitvoeringen en geregistreerde metrische gegevens.
- Een galerieweergave waarin gebruikers kunnen bladeren en nieuwe projecten opstarten met de Microsoft Cognitive Toolkit, TensorFlow en vele andere frameworks voor deep learning. 
- Een verkennerweergave voor het selecteren van compute-doelen om uit te voeren met uw scripts.
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Wat zijn de machine learning-opties van Microsoft?
Behalve Azure Machine Learning zijn er tal van opties in Azure voor het bouwen, implementeren en beheren van machine learning-modellen. 
* Microsoft Machine Learning Services in SQL Server
* Microsoft Machine Learning Server
* Data Science Virtual Machine
* Spark MLLib in HDInsight
* Batch AI Training-service
* Microsoft Cognitive Toolkit
* Microsoft Cognitive Services


### <a name="microsoft-machine-learning-services-in-sql-server"></a>Microsoft Machine Learning-services in SQL Server
Met [Microsoft Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) kunt u machine learning-modellen uitvoeren, trainen en implementeren met behulp van R of Python. U kunt gegevens gebruiken die zich on-premises en in SQL Server-databases bevinden. 

Gebruik Microsoft Machine Learning Services als u modellen on-premises of binnen Microsoft SQL Server wilt trainen of implementeren. Met Machine Learning-services gebouwde modellen kunnen worden geïmplementeerd met Azure Machine Learning-modelbeheer. 

### <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server 
[Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) is een enterprise-server voor hosting en beheer van parallelle en gedistribueerde werkbelastingen van R- en Python-processen. Microsoft Machine Learning-Server wordt uitgevoerd op Linux, Windows, Hadoop en Apache Spark. Het is ook beschikbaar is op [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Het biedt een engine voor het uitvoeren van oplossingen met behulp van [Microsoft Machine Learning-pakketten](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), en breidt open source R en Python uit met ondersteuning voor de volgende scenario's:

- krachtige analyse
- statistische analyse
- machine learning
- enorm grote gegevenssets

Functionaliteit met toegevoegde waarde wordt geboden via bedrijfseigen pakketten die bij de server worden geïnstalleerd. Voor ontwikkeling kunt u gebruikmaken van IDE's zoals [R Tools voor Visual Studio](https://www.visualstudio.com/vs/rtvs/) en [Python Tools voor Visual Studio](https://www.visualstudio.com/vs/python/).

Gebruik Microsoft Machine Learning Server als u het volgende wilt doen:

- Met R en Python gebouwde modellen bouwen en implementeren op een server
- R- en Python-training op grote schaal distribueren op een Hadoop- of Spark-cluster

### <a name="data-science-virtual-machine"></a>Data Science Virtual Machine
[Data Science Virtual Machine (DSVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) is een aangepaste VM-installatiekopie op de Azure-cloud van Microsoft, die speciaal is gebouwd voor gegevenswetenschap. DSVM bevat veel populaire gegevenswetenschap- en andere hulpprogramma's die vooraf zijn geïnstalleerd en geconfigureerd voor het snel ontwikkelen van intelligente toepassingen voor geavanceerde analyses. Het is beschikbaar op Windows Server en op Linux. We bieden de Windows-editie van DSVM op Server 2016 en Server 2012. We bieden de Linux-versie van de DSVM op Ubuntu 16.04 LTS en op Linux-distributies op basis van OpenLogic 7.2 CentOS. 

Gebruik Data Science Virtual Machine wanneer u uw taken wilt uitvoeren op hosten op één knooppunt. Of als u uw verwerking op één computer extern omhoog wilt schalen. Data Science Virtual Machine wordt ondersteund als doel voor zowel Azure Machine Learning Experimenten als Azure Machine Learning Modelbeheer. 

### <a name="spark-mllib-in-hdinsight"></a>Spark MLLib in HDInsight
Met [MLLib Spark in HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) kunt u modellen maken als onderdeel van Spark-taken die worden uitgevoerd op big data. Met Spark kunt u gegevens eenvoudig transformeren en voorbereiden en vervolgens het maken van modelen uitschalen in één taak. Modellen die zijn gemaakt via Spark MLLib kunnen worden geïmplementeerd, beheerd en bewaakt door Azure Machine Learning Modelbeheer. Trainingsuitvoeringen kunnen worden verzonden en beheerd met Azure Machine Learning Experimenten. Spark kan ook worden gebruikt voor het uitschalen van gegevensvoorbereidingstaken die zijn gemaakt in de Machine Learning Workbench. 

Gebruik Spark als u uw gegevensverwerking wilt uitschalen en modellen wilt maken als onderdeel van een gegevenspijplijn. U kunt Spark-taken maken in Scala, Java, Python of R. 

### <a name="batch-ai-training"></a>Batch AI Training 
[Azure Batch AI Training](https://aka.ms/batchaitraining) helpt u parallel experimenteren met uw AI-modellen met behulp van elk gewenst framework, en traint ze vervolgens op grote schaal over geclusterde GPU's. Beschrijf de taakvereisten en de configuratie die u wilt uitvoeren, dan doen wij de rest. 

Met Batch AI Training kunt u deep learning uitschalen over geclusterde GPU's, met behulp van frameworks zoals:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

Azure Machine Learning-Model Management kan worden gebruikt om modellen uit Batch AI Training te nemen om ze te implementeren, beheren controleren.  Batch AI Training wordt in de toekomst geïntegreerd met Azure Machine Learning Experimenten. 

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
[Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) is een uniforme deep learning-werkset waarin neurale netwerken worden beschreven als calculatiestappen in een gerichte graaf. In deze gerichte graaf vertegenwoordigen bladknooppunten invoerwaarden of netwerkparameters, terwijl andere knooppunten matrixbewerkingen op hun invoer vertegenwoordigen. Met de Cognitive Toolkit kunt u gemakkelijk populaire modeltypen realiseren en combineren, zoals feed-forward DNN's, convolutionele netwerken (CNNs) en recurrente netwerken (RNN's/LSTM's). De toolkit implementeert leren met stochastische gradiëntdaling (SGD, terugpropagatie van fouten) met automatische differentiëring en parallellisatie over meerdere GPU's en servers.

Gebruik de Cognitive Toolkit wanneer u een model wilt opbouwen met behulp van deep learning.  De Cognitive Toolkit kan worden gebruikt in elk van de voorafgaande services.

### <a name="microsoft-cognitive-services"></a>Microsoft Cognitive Services
Microsoft Cognitieve Services is een set van 30 API's waarmee u apps kunt bouwen die gebruikmaken van natuurlijke communicatiemethoden. Dankzij deze API's kunnen uw apps zien, horen, spreken, begrijpen en onze behoeften interpreteren, met slechts enkele regels code. Voeg eenvoudig intelligente functies toe aan uw apps, zoals: 

- Emotie- en stemmingsdetectie
- Beeld- en spraakherkenning
- Taal begrijpen
- Kennis en zoeken

Microsoft Cognitive Services kan worden gebruikt om apps te ontwikkelen op allerlei apparaten en platforms. De API's worden voortdurend verbeterd, en zijn gemakkelijk in te stellen. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Volgende stappen
* [Azure Machine Learning installeren en maken](quickstart-installation.md)
