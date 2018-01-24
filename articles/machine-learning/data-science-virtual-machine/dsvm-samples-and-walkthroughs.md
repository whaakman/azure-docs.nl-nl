---
title: Voorbeelden en scenario's voor de gegevens wetenschappelijke virtuele Machine, Azure | Microsoft Docs
description: Voorbeelden en scenario's voor de gegevens wetenschappelijke virtuele Machine.
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: fb9a7e900c489b3d22594dfcb6e1faa736c3c490
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Voorbeelden op de gegevens wetenschappelijke virtuele Machines (DSVM)

De DSVMs worden geleverd met voorbeelden van opgenomen volledig gewerkt-out in de vorm van Jupyter-Notebooks en sommige die niet zijn gebaseerd op Jupyter. U kunt de Jupyter openen door te klikken op de `Jupyter` pictogram in het menu desktop of toepassing.  
> [!NOTE]
> Raadpleeg [toegang Jupyter](#access-jupyter) sectie Jupyter-Notebooks op uw DSVM inschakelen.

## <a name="quick-reference-of-samples"></a>Naslaginformatie van voorbeelden
| Voorbeelden categorie | Beschrijving | Locaties |
| ------------- | ------------- | ------------- |
| **R** taal  | Voorbeelden in **R** uitleg over scenario's zoals verbinding maken met Azure-cloud-gegevens opslaat, Open Source-R vergelijken en Microsoft R & operationele modellen op Microsoft R Server of SQL Server. <br/> [Schermopname](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| **Python** Language  | Voorbeelden in **Python** scenario's zoals verbinding te maken met Azure-cloud gegevensarchieven en werken met uitleg over **Azure Machine Learning**.  <br/> [Schermopname](#python-language) | <br/>`~notebooks` <br/><br/>|
| **Julia** taal  | Voorbeeld **Julia** die worden beschreven die Plotting in Julia, diep in Julia leren, C en Python aanroept vanuit Julia enzovoort. <br/> [Schermopname](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/>`~notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **CNTK** <br/> (Microsoft cognitieve Toolkit)  | Deep learning voorbeelden die zijn gepubliceerd door het team cognitieve Toolkit bij Microsoft.  <br/> [Schermopname](#cntk) | <br/>**Windows**:<br/> `~notebooks/CNTK/Tutorials`<br/><br/>`~/samples/CNTK-Samples-2-0/Examples`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **MXNet** laptops  | Diep Learning steekproeven gebruik **MXNet** gebaseerde neural netwerken. Er zijn tal van notitieblokken variërend van beginnende tot geavanceerde scenario's.  <br/> [Schermopname](#mxnet) | <br/>`~notebooks/mxnet`<br/> <br/>|
| **Azure Machine Learning** AzureML  | Interactie met **Azure Machine Learning** Studio en het maken van de webservice eindpunten van lokaal getraind modellen voor cloud-gebaseerde scoreprofiel werkstromen. <br/> [Schermopname](#azureml) | <br/>`~notebooks/azureml`<br/> <br/>|
| **caffe2** | Diep Learning steekproeven gebruik **caffe2** gebaseerde neural netwerken. Er zijn verschillende notitieblokken ontworpen om te leren kennen van gebruikers met caffe2 en het gebruik ervan effectief, inclusief voorbeelden dat voorverwerking maken van de gegevensset, regressie, en met behulp van de installatiekopie vooraf modellen getraind. <br/> [Schermopname](#caffe2) | <br/>`~notebooks/caffe2`<br/><br/> |
| **H2O**   | Gebruik op basis van een Python-voorbeelden **H2O** talrijke: Praktijkscenario problemen. <br/> [Schermopname](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| **SparkML** taal  | Voorbeeld voor het gebruik van functies en mogelijkheden van de Spark **MLlib** toolkit via **pySpark 2.0** op **Apache Spark 2.0**.  <br/> [Schermopname](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/><br/> |
| **MMLSpark** taal  | Een aantal voorbeelden gebruik **MMLSpark - Microsoft-Machine Learning voor Apache Spark**, dit is een framework dat biedt een aantal grondige learning en gegevens wetenschappelijke hulpprogramma's voor **Apache Spark**. <br/> [Schermopname](#sparkml) | <br/>`~notebooks/MMLSpark`<br/><br/> |
| **TensorFlow**  | Meerdere verschillende Neural Network-voorbeelden en technieken die zijn geïmplementeerd met behulp van de **TensorFlow** framework. <br/> [Schermopname](#tensorflow) | <br/>`~notebooks/tensorflow`<br/><br/> |
| **XGBoost** | Standaard Machine Learning-voorbeelden in **XGBoost** voor scenario's zoals classificatie, regressie enzovoort. <br/> [Schermopname](#xgboost) | <br/>`~samples/xgboost/demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Toegang Jupyter 

Ga naar Jupyter thuis via  **`https://localhost:9999`**  op Windows of  **`https://localhost:8000`**  op Ubuntu.


### <a name="enabling-jupyter-access-from-browser"></a>Het inschakelen van Jupyter toegang vanuit Browser

**Windows DSVM**

Voer  **`Jupyter SetPassword`**  van de snelkoppeling op het bureaublad en volg de prompt set/uw wachtwoord opnieuw instellen voor Jupyter en starten van de Jupyter verwerken. 
<br/>![De Jupyter-uitzondering toestaan](./media/jupyter-setpassword.png)<br/>
U hebt toegang tot Jupyter Home zodra de Jupyter-proces is gestart op de virtuele machine in via  **`https://localhost:9999`**  van uw browser. Zie schermafbeelding uitzondering toevoegen en inschakelen van Jupyter toegang via de browser
<br/>![De Jupyter-uitzondering toestaan](./media/windows-jupyter-exception.png)<br/>
Aanmelden met het nieuwe wachtwoord dat u zojuist hebt ingesteld.
<br/>
**Linux DSVM**

U toegang hebt tot Jupyter-startpagina op de virtuele machine in via  **`https://localhost:8000`**  van uw browser. Zie schermafbeelding uitzondering toevoegen en inschakelen van Jupyter toegang via de browser.
<br/>![De Jupyter-uitzondering toestaan](./media/ubuntu-jupyter-exception.png)<br/>
Aanmelden met hetzelfde wachtwoord als uw aanmelding voor de DSVM.
<br/>

**Jupyter-startpagina**
<br/>![Jupyter-startpagina](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R taal 
<br/>![R-voorbeelden](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python Language
<br/>![Python-voorbeelden](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia taal 
<br/>![Julia voorbeelden](./media/julia-samples.png)<br/>

## <a name="cntk"></a>CNTK 
<br/>![Voorbeelden van CNTK](./media/cntk-samples2.png)<br/>
<br/>![Voorbeelden van CNTK](./media/cntk-samples.png)<br/>

## <a name="mxnet"></a>MXNet
<br/>![Voorbeelden van MXnet](./media/mxnet-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![AzurekML Samples](./media/azureml-samples.png)<br/>

## <a name="caffe2"></a>caffe2 
<br/>![Voorbeelden van caffe2](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Voorbeelden van H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Voorbeelden van SparkML](./media/sparkml-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Voorbeelden van TensorFlow](./media/tensorflow-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Voorbeelden van XGBoost](./media/xgboost-samples.png)<br/>

