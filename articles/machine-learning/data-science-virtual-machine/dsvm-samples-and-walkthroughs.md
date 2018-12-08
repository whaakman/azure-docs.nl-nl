---
title: Voorbeelden en procedures voor virtuele Machines voor Datatechnologie - Azure | Microsoft Docs
description: Meer informatie over de voorbeelden en procedures die laten zien u hoe u algemene taken en scenario's met de Data Science Virtual Machine.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: 64be6af340aa02c6c0b094013d2cbd286286aca7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101983"
---
# <a name="samples-on-data-science-virtual-machines"></a>Voorbeelden van virtuele Machines voor Datatechnologie

Azure Data Science Virtual Machines bevat een uitgebreide set met voorbeeldcode. De voorbeeldcode is in de vorm van Jupyter notebooks en scripts in talen zoals Python en R. 
> [!NOTE]
> Zie voor meer informatie over het uitvoeren van Jupyter notebooks op uw virtuele machines voor datatechnologie, de [toegang Jupyter](#access-jupyter) sectie.

## <a name="quick-reference-of-samples"></a>Naslaggids van voorbeelden
| Voorbeelden van categorie | Description | Locaties |
| ------------- | ------------- | ------------- |
| R-taal  | Voorbeelden in R uitgelegd scenario's zoals verbinding maken met Azure-cloud-gegevensarchieven. Ze ook wordt uitgelegd hoe u wilt vergelijken van open-source R en Microsoft-R. En ze wordt uitgelegd hoe u modellen op Microsoft R Server of SQL Server. <br/> [R-taal](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Python-taal  | Voorbeelden van Python wordt uitgelegd scenario's, zoals het verbinden met Azure-cloud-gegevensarchieven en werken met Azure Machine Learning.  <br/> [Python-taal](#python-language) | <br/>`~notebooks` <br/><br/>|
| Julia-taal  | Voorbeeld in Julia waarin wordt uitgelegd plotten en deep learning in Julia. Ook wordt uitgelegd aanroepende C en Python van Julia. <br/> [Julia-taal](#julia-language) |<br/> Windows:<br/> `~notebooks/Julia_notebooks`<br/><br/> Linux:<br/> `~notebooks/julia`<br/><br/> |
| Azure Machine Learning  | Machine learning en deep learning-modellen met Machine Learning bouwen. Implementeer modellen overal. Gebruik automatische machine learning en intelligente hyperparameter afstemmen. Modelbeheer- en gedistribueerde training ook gebruiken. <br/> [Machine Learning](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| PyTorch-laptops  | Deep learning-voorbeelden die gebruikmaken van PyTorch gebaseerde neurale netwerken. Notitieblokken de variëren van beginner tot geavanceerde scenario's.  <br/> [PyTorch-laptops](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| TensorFlow  |  Voorbeelden voor verschillende neurale netwerken en technieken die worden geïmplementeerd met behulp van de TensorFlow-framework. <br/> [TensorFlow](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| Microsoft Cognitive Toolkit <br/>   | Deep learning-voorbeelden die zijn gepubliceerd door de Cognitive Toolkit-team van Microsoft.  <br/> [Cognitive Toolkit](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> Linux:<br/> `~notebooks/CNTK`<br/> <br/>|
| caffe2 | Deep learning-voorbeelden die gebruikmaken van caffe2 gebaseerde neurale netwerken. Gebruikers vertrouwd verschillende notitieblokken met caffe2 en hoe u effectief te gebruiken. Voorbeelden zijn onder meer het maken van het vooraf verwerken en gegevensset installatiekopieën. Ze bevatten ook regressie en het gebruik van vooraf getrainde modellen. <br/> [caffe2](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| H2O   | Op basis van een Python-voorbeelden die gebruikmaken van H2O Praktijkscenario problemen. <br/> [H2O](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| SparkML taal  | Voorbeelden die gebruikmaken van functies van de toolkit MLLib Spark met pySpark en MMLSpark--Microsoft Machine Learning voor Apache Spark op Apache Spark 2.x.  <br/> [SparkML taal](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| XGBoost | Standard machine learning-voorbeelden in XGBoost voor scenario's zoals classificatie- en regressiemodellen. <br/> [XGBoost](#xgboost) | <br/>Windows:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Toegang tot Jupyter 

Voor toegang tot Jupyter, selecteer de `Jupyter` pictogram in het menu bureaublad of toepassing. U kunt ook Jupyter openen op Linux-versies van virtuele Machines voor Datatechnologie. U kunt toegang krijgen tot op afstand via een webbrowser recentst `https://<Full Domain Name or IP Address of the DSVM>:8000` op Ubuntu.

Voor het toevoegen van uitzonderingen en toegang tot Jupyter beschikbaar maken via een browser, Zie de volgende schermafbeelding.


![Jupyter-uitzondering inschakelen](./media/ubuntu-jupyter-exception.png)


Meld u aan met hetzelfde wachtwoord als voor uw aanmelding bij voor Data Science Virtual Machines.
<br/>

**Jupyter thuis**
<br/>![Jupyter thuis](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R-taal 
<br/>![R-voorbeelden](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python-taal
<br/>![Python-voorbeelden](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia-taal 
<br/>![Julia-voorbeelden](./media/julia-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![AzurekML-voorbeelden](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![PyTorch-voorbeelden](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![TensorFlow-voorbeelden](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![Voorbeelden van CNTK](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>caffe2 
<br/>![caffe2-voorbeelden](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Voorbeelden van H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![SparkML-voorbeelden](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![XGBoost-voorbeelden](./media/xgboost-samples.png)<br/>

