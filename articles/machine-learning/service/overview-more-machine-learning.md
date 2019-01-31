---
title: Opties voor machine learning-producten vergelijken
titleSuffix: Microsoft
description: Vergelijk de verschillende producten van Microsoft om Machine Learning-modellen te bouwen, implementeren en beheren. Bepaal welke producten u wilt voor uw oplossing.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 1/17/2019
ms.openlocfilehash: d7c7c383f51d8bcc2b1691724e8f22b43782c5e8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242066"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Wat zijn de machine learning-producten van Microsoft?

Microsoft biedt tal van producten voor het bouwen, implementeren en beheren van Machine Learning-modellen. Vergelijk deze producten en kies wat u nodig hebt om uw machine learning-oplossingen zo effectief mogelijk te ontwikkelen.


**Cloud-opties**

De volgende opties zijn beschikbaar voor machine learning in de Azure-cloud.

| Cloud&nbsp;-opties | Wat is het? | Wat u ermee kunt doen |
|-|-|-|
| [Azure Machine Learning-service](#azure-machine-learning-services) | Beheerde cloudservice voor ML  | Modellen trainen, implementeren en beheren in Azure met behulp van Python en CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Visuele interface voor ML met slepen&ndash;en&ndash;neerzetten | Modellen bouwen en implementeren en ermee experimenteren met behulp van vooraf geconfigureerde algoritmen (Python en R)|
| [Azure Databricks](#azure-databricks) | Op Spark gebaseerd analyseplatform | Modellen en gegevenswerkstromen bouwen en implementeren |
| [Azure Cognitive Services](#azure-cognitive-services) | Azure-services met geïntegreerde AI- en ML-modellen | Eenvoudig intelligente functies aan uw apps toevoegen |
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | Virtuele machine met vooraf geïnstalleerde hulpprogramma 's voor data science | ML-oplossingen ontwikkelen in een vooraf geconfigureerde omgeving |

**On-premises opties**

De volgende opties zijn beschikbaar voor machine learning on-premises. On-premises servers kunnen ook in een virtuele machine in de cloud worden uitgevoerd.

| On-premises &nbsp;opties | Wat is het? | Wat u ermee kunt doen |
|-|-|-|
| [SQL Server Machine Learning-services](#sql-server-machine-learning-services) | Analyse-engine ingesloten in SQL | Modellen bouwen en implementeren in SQL Server |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Zelfstandige enterprise server voor voorspellende analyse | Modellen bouwen en implementeren met R en Python |

**Ontwikkelhulpprogramma's**

De volgende ontwikkelhulpprogramma's zijn beschikbaar voor machine learning.

| Ontwikkel&nbsp;hulpprogramma's | Wat is het? | Wat u ermee kunt doen |
|-|-|-|
| [ML.NET](#mlnet) | Open-source, platformoverschrijdende ML SDK | ML-oplossingen voor .NET-toepassingen ontwikkelen |
| [Windows ML](#windows-ml) | ML-platform voor Windows 10 | Getrainde modellen evalueren op een Windows 10-apparaat |







## <a name="azure-machine-learning-service"></a>Azure Machine Learning-service

[Azure Machine Learning service](overview-what-is-azure-ml.md) is een volledig beheerde cloudservice voor het trainen, implementeren en beheren van ML-modellen op schaal. De service ondersteunt open-source technologieën volledig, zodat u tienduizenden open-source Python-pakketten zoals TensorFlow, PyTorch en scikit-learn kunt gebruiken. Er zijn ook geavanceerde hulpprogramma's beschikbaar, zoals [Azure Notebooks](https://notebooks.azure.com/), [Jupyter Notebooks](http://jupyter.org) of de extensie [Azure Machine Learning voor Visual Studio Code](https://aka.ms/vscodetoolsforai). Deze maken het gemakkelijker om gegevens te verkennen en transformeren en vervolgens modellen te trainen en implementeren. Azure Machine Learning-service omvat functies die modelgeneratie automatiseren en afstemmen met gemak, efficiëntie en nauwkeurigheid.

Gebruik Azure Machine Learning service om ML-modellen te trainen, implementeren en beheren met behulp van Python en CLI op cloudschaal.

Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning-service](http://aka.ms/AMLFree).

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](../studio/what-is-ml-studio.md) biedt u een interactieve, visuele werkruimte waarmee u eenvoudig en snel modellen kunt bouwen, testen en implementeren met behulp van vooraf gebouwde machine learning-algoritmen. Machine Learning Studio publiceert modellen als webservices die eenvoudig kunnen worden gebruikt door aangepaste apps of BI-hulpprogramma's zoals Excel.
U hoeft niet te programmeren: u maakt uw Machine Learning-model door gegevenssets en analysemodules te verbinden op een interactief canvas en het model vervolgens met een paar klikken te implementeren.

Met Machine Learning Studio kunt u op elk moment modellen ontwikkelen en implementeren zonder dat er code vereist is.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) is een op Apache Spark gebaseerd analyseplatform, geoptimaliseerd voor het Microsoft Azure-platform voor cloudservices. Databricks is met Azure geïntegreerd, waardoor het installatie met één klik, gestroomlijnde werkstromen en een interactieve werkruimte biedt waarmee gegevenswetenschappers, gegevenstechnici en bedrijfsanalisten samen kunnen werken.
Gebruik Python-, R-, Scala- en SQL-code in webgebaseerde notebooks om query's uit te voeren op gegevens, gegevens te visualiseren en gegevens te modelleren.

Gebruik Databricks wanneer u wilt om samen te werken aan het bouwen van machine learning-oplossingen op Apache Spark.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

[Azure Cognitive Services](/azure/cognitive-services/welcome) is een set API's waarmee u apps kunt bouwen die gebruikmaken van natuurlijke communicatiemethoden. Dankzij deze API's kunnen apps zien, horen, spreken, begrijpen en de behoeften van gebruikers interpreteren, met slechts enkele regels code. Voeg eenvoudig intelligente functies toe aan uw apps, zoals: 

- Emotie- en stemmingsdetectie
- Beeld- en spraakherkenning
- Language Understanding (LUIS)
- Kennis en zoeken

Gebruik Cognitive Services om apps te ontwikkelen op allerlei apparaten en platforms. De API's worden voortdurend verbeterd, en zijn gemakkelijk in te stellen.

## <a name="azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine

[Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md) is een aangepaste VM-omgeving in de Azure-cloud van Microsoft, die speciaal is gebouwd voor datatechnologie. DSVM bevat veel populaire gegevenswetenschap- en andere hulpprogramma's die vooraf zijn geïnstalleerd en geconfigureerd voor het snel ontwikkelen van intelligente toepassingen voor geavanceerde analyses.

Data Science Virtual Machine wordt ondersteund als een doel voor Azure Machine Learning service.
Het is beschikbaar voor zowel Windows als Linux Ubuntu (Azure Machine Learning service wordt niet ondersteund op Linux CentOS).
Zie [Introduction to the Azure Data Science Virtual Machine](../data-science-virtual-machine/overview.md) (Inleiding voor Azure Data Science Virtual Machine) voor informatie over specifieke versies en een lijst met wat is inbegrepen.

Gebruik Data Science-VM wanneer u taken wilt uitvoeren of hosten op één knooppunt. Of als u uw verwerking op één computer extern omhoog wilt schalen.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning-services

[SQL Server Microsoft Machine Learning service](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) voegt statistische analyse, gegevensvisualisatie en predictive analytics toe in R en Python voor relationele gegevens in SQL Server-databases. R- en Python-bibliotheken van Microsoft bevatten geavanceerde modellering en ML-algoritmes in SQL Server die parallel en op schaal kunnen worden uitgevoerd.

Gebruik SQL Server Machine Learning Services als u ingebouwde AI en predictive analytics voor relationele gegevens in SQL Server nodig hebt.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) is een enterprise-server voor hosting en beheer van parallelle en gedistribueerde werkbelastingen van R- en Python-processen. Microsoft Machine Learning Server wordt uitgevoerd op Linux, Windows, Hadoop en Apache Spark en is ook beschikbaar op [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Het biedt een engine voor het uitvoeren van oplossingen die zijn gebouwd met behulp van [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) en [MicrosoftML-pakketten](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package). Ook breidt het open-source R en Python uit met ondersteuning voor hoogwaardige analyse, statistische analyse, machine learning en enorme gegevenssets. Deze functionaliteit wordt geboden via bedrijfseigen pakketten die op de server worden geïnstalleerd. Voor ontwikkeling kunt u gebruikmaken van IDE's zoals [R Tools voor Visual Studio](https://www.visualstudio.com/vs/rtvs/) en [Python Tools voor Visual Studio](https://www.visualstudio.com/vs/python/).

Gebruik Microsoft Machine Learning Server wanneer u modellen die zijn gemaakt met R en Python op een server wilt bouwen en operationeel maken, of R- en Python-training op grote schaal wilt distribueren op een Hadoop- of Spark-cluster.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) is een gratis, open-source en platformoverschrijdend machine learning-framework waarmee u aangepaste machine learning-oplossingen kunt bouwen en in uw .NET-toepassingen integreren.

Gebruik ML.NET wanneer u machine learning-oplossingen in uw .NET-toepassingen wilt integreren.

## <a name="windows-ml"></a>Windows ML

Met [Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) kunt u getrainde Machine Learning-modellen in uw toepassingen gebruiken, waarbij getrainde modellen lokaal op Windows 10-apparaten worden geëvalueerd.

Gebruik Windows ML als u getrainde Machine Learning-modellen in uw Windows-toepassingen wilt gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Zie [Microsoft AI-platform](https://www.microsoft.com/ai) voor meer informatie over alle Articifical Intelligence-ontwikkelingsproducten van Microsoft
- Zie [Microsoft AI School](https://aischool.microsoft.com/learning-paths) voor training over het ontwikkelen van AI-oplossingen
