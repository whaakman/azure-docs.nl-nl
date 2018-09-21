---
title: Andere machine learning-producten van Microsoft - Azure Machine Learning | Microsoft Docs
description: Naast Azure Machine Learning biedt Microsoft allerlei opties voor het bouwen, implementeren en beheren van machine learning-modellen.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: garyericson, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: overview
ms.date: 04/11/2018
ms.openlocfilehash: 138fbb64ca2d157059c952d9e82541cfac3ff597
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579051"
---
# <a name="other-machine-learning-products-and-services-from-microsoft"></a>Andere machine learning-producten en -services van Microsoft

Naast [Azure Machine Learning](overview-what-is-azure-ml.md) biedt Microsoft allerlei opties voor het bouwen, implementeren en beheren van machine learning-modellen. 
* SQL Server Machine Learning-services
* Microsoft Machine Learning Server
* Azure Data Science Virtual Machine
* Spark MLLib in HDInsight
* Batch AI Training-service
* Microsoft Cognitive Toolkit (CNTK)
* Azure Cognitive Services


## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning-services
Met [SQL Server Microsoft Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) kunt u machine learning-modellen uitvoeren, trainen en implementeren met behulp van R of Python. U kunt gegevens gebruiken die zich on-premises en in SQL Server-databases bevinden. 

Gebruik Microsoft Machine Learning Services als u modellen on-premises of binnen Microsoft SQL Server wilt trainen of implementeren. Met Machine Learning-services gebouwde modellen kunnen worden geïmplementeerd met Azure Machine Learning-modelbeheer. 

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server 
[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) is een enterprise-server voor hosting en beheer van parallelle en gedistribueerde werkbelastingen van R- en Python-processen. Microsoft Machine Learning-Server wordt uitgevoerd op Linux, Windows, Hadoop en Apache Spark. Het is ook beschikbaar is op [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Het biedt een engine voor het uitvoeren van oplossingen met behulp van [Microsoft Machine Learning-pakketten](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), en breidt open source R en Python uit met ondersteuning voor de volgende scenario's:

- Krachtige analyse
- Statistische analyse
- Machine learning
- Enorm grote gegevenssets

Extra functionaliteit wordt geboden via bedrijfseigen pakketten die op de server worden geïnstalleerd. Voor ontwikkeling kunt u gebruikmaken van IDE's zoals [R Tools voor Visual Studio](https://www.visualstudio.com/vs/rtvs/) en [Python Tools voor Visual Studio](https://www.visualstudio.com/vs/python/).

Gebruik Microsoft Machine Learning Server als u het volgende wilt doen:

- Met R en Python gebouwde modellen bouwen en implementeren op een server
- R- en Python-training op grote schaal distribueren op een Hadoop- of Spark-cluster

## <a name="azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine
[Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) is een aangepaste VM-omgeving in de Azure-cloud van Microsoft, die speciaal is gebouwd voor datatechnologie. DSVM bevat veel populaire gegevenswetenschap- en andere hulpprogramma's die vooraf zijn geïnstalleerd en geconfigureerd voor het snel ontwikkelen van intelligente toepassingen voor geavanceerde analyses. De Data Science-VM is beschikbaar in Windows Server-versies 2016 en 2012, en op een Linux-VM met Ubuntu 16.04 LTS en voor distributies op basis van OpenLogic CentOS 7.4. 

Gebruik Data Science-VM wanneer u taken wilt uitvoeren of hosten op één knooppunt. Of als u uw verwerking op één computer extern omhoog wilt schalen. Data Science Virtual Machine wordt ondersteund als doel voor zowel Azure Machine Learning Experimenten als Azure Machine Learning Modelbeheer. 

## <a name="spark-mllib-in-hdinsight"></a>Spark MLLib in HDInsight
Met [MLLib Spark in HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) kunt u modellen maken als onderdeel van Spark-taken die worden uitgevoerd op big data. Met Spark kunt u gegevens eenvoudig transformeren en voorbereiden en vervolgens het maken van modelen uitschalen in één taak. Modellen die zijn gemaakt via Spark MLLib kunnen worden geïmplementeerd, beheerd en bewaakt door Azure Machine Learning Modelbeheer. Trainingsuitvoeringen kunnen worden verzonden en beheerd met Azure Machine Learning Experimenten. Spark kan ook worden gebruikt voor het uitschalen van gegevensvoorbereidingstaken die zijn gemaakt in de Machine Learning Workbench. 

Gebruik Spark als u uw gegevensverwerking wilt uitschalen en modellen wilt maken als onderdeel van een gegevenspijplijn. U kunt Spark-taken maken in Scala, Java, Python of R. 

## <a name="batch-ai-training"></a>Batch AI Training 
[Azure Batch AI Training](https://aka.ms/batchaitraining) helpt u parallel experimenteren met uw AI-modellen met behulp van elk gewenst framework, en traint ze vervolgens op grote schaal over geclusterde GPU's. Beschrijf de taakvereisten en de configuratie die u wilt uitvoeren, dan doen wij de rest. 

Met Batch AI Training kunt u deep learning uitschalen over geclusterde GPU's, met behulp van frameworks zoals:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

Azure Machine Learning-Model Management kan worden gebruikt om modellen uit Batch AI Training te nemen om ze te implementeren, beheren controleren.  Batch AI Training wordt in de toekomst geïntegreerd met Azure Machine Learning Experimenten. 

## <a name="microsoft-cognitive-toolkit-cntk"></a>Microsoft Cognitive Toolkit (CNTK)
[Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) is een uniforme deep learning-werkset waarin neurale netwerken worden beschreven als calculatiestappen in een gerichte graaf. In deze gerichte graaf vertegenwoordigen bladknooppunten invoerwaarden of netwerkparameters, terwijl andere knooppunten matrixbewerkingen op hun invoer vertegenwoordigen. Met de Cognitive Toolkit kunt u gemakkelijk populaire modeltypen realiseren en combineren, zoals feed-forward DNN's, convolutionele netwerken (CNNs) en recurrente netwerken (RNN's/LSTM's). De toolkit implementeert leren met stochastische gradiëntdaling (SGD, terugpropagatie van fouten) met automatische differentiëring en parallellisatie over meerdere GPU's en servers.

Gebruik de Cognitive Toolkit wanneer u een model wilt opbouwen met behulp van deep learning.  De Cognitive Toolkit kan worden gebruikt in elk van de voorafgaande services.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services
[Azure Cognitieve Services](https://docs.microsoft.com/azure/#pivot=products&panel=ai) is een set van ongeveer 30 API's waarmee u apps kunt bouwen die gebruikmaken van natuurlijke communicatiemethoden. Dankzij deze API's kunnen apps zien, horen, spreken, begrijpen en de behoeften van gebruikers interpreteren, met slechts enkele regels code. Voeg eenvoudig intelligente functies toe aan uw apps, zoals: 

- Emotie- en stemmingsdetectie
- Beeld- en spraakherkenning
- Language Understanding (LUIS)
- Kennis en zoeken

Cognitive Services kan worden gebruikt om apps te ontwikkelen op allerlei apparaten en platforms. De API's worden voortdurend verbeterd, en zijn gemakkelijk in te stellen. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Volgende stappen

Lees het overzicht voor [Azure Machine Learning](overview-what-is-azure-ml.md).
