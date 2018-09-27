---
title: Voorbeelden en procedures voor de Data Science Virtual Machine - Azure | Microsoft Docs
description: Voorbeelden en procedures voor de Data Science Virtual Machine.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: a1f15b805d2f27152d9ba85608ce0dc1d1aac21e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392562"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Voorbeelden van de Data Science Virtual Machines (DSVM)

De Dsvm bevat een uitgebreide set met voorbeeldcode, zowel in de vorm van Jupyter Notebooks, evenals de scripts in talen zoals Python en R.    
> [!NOTE]
> Raadpleeg [toegang Jupyter](#access-jupyter) sectie voor meer informatie over het uitvoeren van Jupyter Notebooks op uw DSVM.

## <a name="quick-reference-of-samples"></a>Naslaggids van voorbeelden
| Voorbeelden van categorie | Beschrijving | Locaties |
| ------------- | ------------- | ------------- |
| **R** taal  | Voorbeelden **R** waarin wordt uitgelegd scenario's zoals het maken van een verbinding met Azure-cloud-gegevens worden opgeslagen, Open Source R vergelijken en Microsoft R & tot het operationaliseren van modellen op Microsoft R Server of SQL Server. <br/> [Schermopname](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| **Python** taal  | Voorbeelden **Python** waarin wordt uitgelegd scenario's zoals verbinding maken met Azure-cloud-gegevensarchieven en werken met **Azure Machine Learning**.  <br/> [Schermopname](#python-language) | <br/>`~notebooks` <br/><br/>|
| **Julia** taal  | Voorbeeld van een **Julia** die details Plotting in Julia, deep learning in Julia, C en Python aanroepen vanuit Julia enzovoort. <br/> [Schermopname](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **Azure Machine Learning** AzureML  | Bouw ML en deep learning-modellen met **Azure Machine Learning** -Service en implementeert modellen overal. Maak gebruik van mogelijkheden zoals ML geautomatiseerde, intelligente hyper parameter afstemmen, Modelbeheer, gedistribueerd training. <br/> [Schermopname](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| **PyTorch** notitieblokken  | Deep Learning-voorbeelden met behulp van **PyTorch** op basis van neurale netwerken. Er zijn tal van notitieblokken, variërend van beginner tot geavanceerde scenario's.  <br/> [Schermopname](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| **TensorFlow**  | Meerdere verschillende voorbeelden voor Neurale netwerken en technieken die worden geïmplementeerd met behulp van de **TensorFlow** framework. <br/> [Schermopname](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| **CNTK** <br/> (Microsoft Cognitive Toolkit)  | Deep learning-voorbeelden die zijn gepubliceerd door de Cognitive Toolkit-team van Microsoft.  <br/> [Schermopname](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **caffe2** | Deep Learning-voorbeelden met behulp van **caffe2** op basis van neurale netwerken. Er zijn verschillende notitieblokken ontworpen om te leren kennen van gebruikers met caffe2 en hoe u effectief, inclusief voorbeelden zoals installatiekopie vooraf verwerken, het maken van gegevensset, regressie, en met behulp van vooraf modellen getrainde. <br/> [Schermopname](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| **H2O**   | Op basis van een Python-voorbeelden met behulp van **H2O** voor een groot aantal Praktijkscenario problemen. <br/> [Schermopname](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| **SparkML** taal  | Voorbeeld met behulp van functies en mogelijkheden van Spark van **MLlib** toolkit via **pySpark** en **MMLSpark - Microsoft Machine Learning voor Apache Spark** op **Apache Spark-2.x**.  <br/> [Schermopname](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| **XGBoost** | Standard Machine Learning-voorbeelden in **XGBoost** voor scenario's zoals classificatie, regressie enzovoort. <br/> [Schermopname](#xgboost) | <br/>**Windows**:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Toegang tot Jupyter 

U kunt Jupyter openen door te klikken op de `Jupyter` pictogram in het menu bureaublad of toepassing. U kunt ook Jupyter op Linux-edities van de DSVM op afstand via een webbrowser openen door naar de pagina **`https://<Full Domain Name or IP Address of the DSVM>:8000`** op Ubuntu.

Zie de schermafbeelding uitzondering toevoegen en Jupyter-toegang inschakelen via de browser.


![Jupyter-uitzondering inschakelen](./media/ubuntu-jupyter-exception.png)


Meld u aan met hetzelfde wachtwoord als voor uw aanmelding bij voor de DSVM.
<br/>

**Jupyter-startpagina**
<br/>![Jupyter-startpagina](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R-taal 
<br/>![R-voorbeelden](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python-taal
<br/>![Voorbeelden van Python](./media/python-language-samples.png)<br/>

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

