---
title: Grondige Learning en AI frameworks - Azure | Microsoft Docs
description: Deep Learning en AI-frameworks
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
ms.openlocfilehash: 89a8cde0dbb7fe7ccfbb6d068411aaf5488c532f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Deep Learning en AI-frameworks
De [gegevens wetenschappelijke virtuele Machine](http://aka.ms/dsvm) (DSVM) en de [grondige Learning VM](http://aka.ms/dsvm/deeplearning) ondersteunt een aantal grondige learning frameworks om u te helpen bij het ontwikkelen van toepassingen met predictive analytics kunstmatige intelligentie (AI) en cognitieve mogelijkheden, zoals de installatiekopie en language understanding. 

Hier volgen de details op alle frameworks die beschikbaar is op de DSVM learning diep.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Microsoft cognitieve Toolkit (CNTK) is geïnstalleerd in Python 2.7 in de _hoofdmap_ omgeving, evenals Python 3.5, in de _py35_ omgeving.   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.     |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | Jupyter openen en vervolgens zoekt u naar de map CNTK  |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Op Linux, TensorFlow is geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving. In Windows, Tensorflow in Python 3.5 is geïnstalleerd (_py35_) omgeving.  |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.     |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | Jupyter openen en vervolgens zoekt u naar de map TensorFlow.  |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Keras is geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving.   |
| Koppelingen naar voorbeelden      | https://github.com/fchollet/keras/tree/master/examples      |
| Bijbehorende hulpprogramma's op de DSVM      | Microsoft Cognitive Toolkit, TensorFlow, Theano      |
| Hoe gebruik / uitvoeren?    | De voorbeelden van de locatie van Github downloaden, kopiëren naar een map onder ~/notebooks en openen in Jupyter   |




## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe is geïnstalleerd in `/opt/caffe`.    |
| Koppelingen naar voorbeelden      | Voorbeelden zijn opgenomen in `/opt/caffe/examples`.      |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe2      |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

X2Go aanmelden bij uw VM vervolgens start een nieuwe terminal en voer gebruiken

```
cd /opt/caffe/examples
jupyter notebook
```

Een nieuw browservenster geopend met voorbeeldquery notitieblokken.

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe2 is geïnstalleerd in `/opt/caffe2`. Het is ook beschikbaar voor Python 2.7 (_hoofdmap_) conda omgeving.     |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen     |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe      |
| Hoe gebruik / uitvoeren?    | Jupyter openen en vervolgens gaat u naar de map Caffe2 voorbeeldquery notitieblokken vinden. Sommige laptops vereisen de hoofdmap Caffe2 moet worden ingesteld in de Python-code; Voer /opt/caffe2.   |


## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Chainer is geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving. ChainerRL en ChainerCV ook geïnstalleerd.   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.      |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe      |

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

Op een terminal activeren de gewenste versie van Python (_hoofdmap_ of _py35_), voert _python_, Chainer vervolgens te importeren. Selecteer de Python 2.7 of 3.5 kernel in Jupyter, en klik Chainer importeren.


## <a name="deep-water"></a>Grondige Water

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework voor H2O      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Grondige Water is geïnstalleerd in `/dsvm/tools/deep_water`.   |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar via de grondige Water-server.      |
| Bijbehorende hulpprogramma's op de DSVM      | H2O, mineraalwater      |

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

Verbinding maken met de virtuele machine met behulp van X2Go. Op een terminal server grondige Water te starten:

    java -jar /dsvm/tools/deep_water/h2o.jar

Open een browser vervolgens en verbinding maken met `http://localhost:54321`.



## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | MXNet is geïnstalleerd in `C:\dsvm\tools\mxnet` in Windows en `/dsvm/tools/mxnet` op Linux. Python-bindingen zijn geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving. R-bindingen zijn ook geïnstalleerd.   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.    |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | Jupyter openen en vervolgens zoekt u naar de map mxnet  |

## <a name="nvidia-digits"></a>NVIDIA CIJFERS

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-systeem van NVIDIA snel grondige learning-modellen trainen      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | CIJFERS is geïnstalleerd in `/dsvm/tools/DIGITS` en beschikbaar is aangeroepen voor een service _cijfers_.   |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

Meld u aan de virtuele machine met X2Go bij. Start de service op een terminal:

    sudo systemctl start digits

De service duurt ongeveer een minuut om te starten. Start een webbrowser en navigeer naar `http://localhost:5000`.



## <a name="nvdia-smi"></a>nvdia-smi

|    |           |
| ------------- | ------------- |
| Wat is het?   | NVIDIA hulpprogramma voor het uitvoeren van query's GPU-activiteit      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _NVIDIA smi_ is beschikbaar in het systeempad bevindt.   |
| Hoe gebruik / uitvoeren? | Start een opdrachtprompt (op Windows) of een terminal (op Linux) en voer vervolgens _nvidia smi_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Theano is geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving.   |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | Op een terminal activeren de versie van Python wilt (basis of py35), uitvoeren van python en theano importeren. Selecteer de Python 2.7 of 3.5 kernel in Jupyter, en klik theano importeren.  |



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Torch is geïnstalleerd in `/dsvm/tools/torch`. PyTorch is geïnstalleerd in Python 2.7 (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving.   |
| Koppelingen naar voorbeelden      | Voorbeelden van torch bevinden zich op `/dsvm/samples/torch`. Voorbeelden van PyTorch bevinden zich op `/dsvm/samples/pytorch`.      |


## <a name="pytorch"></a>PyTorch

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | PyTorch is geïnstalleerd in de Python 3.5 (_py35_) omgeving.   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen en voorbeelden kunnen worden gevonden in /dsvm/samples/pytorch.      |
| Bijbehorende hulpprogramma's op de DSVM      | Torch      |

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

Voer vanaf een terminal _python_, torch vervolgens te importeren. In Jupyter, selecteert u de kernel Python 3.5 en torch importeren.


## <a name="mxnet-model-server"></a>MXNet Model Server

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een server voor het maken van HTTP-eindpunten voor MXNet en ONNX modellen      |
| Ondersteunde DSVM edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _mxnet-model-server_ is beschikbaar op op de terminal.   |
| Koppelingen naar voorbeelden      | Up-to-date voorbeelden zoeken op de [MXNet Model serverpagina](https://github.com/awslabs/mxnet-model-server).    |
| Bijbehorende hulpprogramma's op de DSVM      | MXNet      |

## <a name="tensorflow-serving"></a>TensorFlow levering

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een server uit te voeren inferencing op een model TensorFlow      |
| Ondersteunde DSVM edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _tensorflow_model_server_ is beschikbaar op de terminal.   |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar [online](https://www.tensorflow.org/serving/).      |
| Bijbehorende hulpprogramma's op de DSVM      | TensorFlow      |
