---
title: Grondige Learning en AI frameworks - Azure | Microsoft Docs
description: Deep Learning en AI-frameworks
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
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: cd8579fe29282f3875ecfddf67a34444e3de9ef5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Deep Learning en AI-frameworks
De [gegevens wetenschappelijke virtuele Machine](http://aka.ms/dsvm) (DSVM) en de [grondige Learning VM](http://aka.ms/dsvm/deeplearning) ondersteunt een aantal grondige learning frameworks om u te helpen bij het ontwikkelen van toepassingen met predictive analytics kunstmatige intelligentie (AI) en cognitieve mogelijkheden, zoals de installatiekopie en language understanding. 

Hier volgen de details op alle frameworks die beschikbaar is op de DSVM learning diep.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Microsoft cognitieve Toolkit (CNTK) in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.     |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | * Op een terminal: de juiste omgeving activeren en voer vervolgens Python. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map CNTK voor voorbeelden. |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorFlow in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).  |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.     |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | * Op een terminal: de juiste omgeving activeren en voer vervolgens Python. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map TensorFlow voor voorbeelden.  |

## <a name="horovod"></a>Horovod

|    |           |
| ------------- | ------------- |
| Wat is het?   | Distributed grondige learning framework voor TensorFlow      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Horovod in Python 3.5 is geïnstalleerd op [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Koppelingen naar voorbeelden      | [https://github.com/uber/horovod/tree/master/examples](https://github.com/uber/horovod/tree/master/examples)     |
| Bijbehorende hulpprogramma's op de DSVM      | TensorFlow      |
| Hoe gebruik / uitvoeren?    | Op een terminal: de juiste omgeving activeren en voer vervolgens Python. |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorFlow in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). |
| Koppelingen naar voorbeelden      | https://github.com/fchollet/keras/tree/master/examples      |
| Bijbehorende hulpprogramma's op de DSVM      | Microsoft cognitieve Toolkit, TensorFlow, Theano      |
| Hoe gebruik / uitvoeren?    | * Op een terminal: de juiste omgeving activeren en voer vervolgens Python. <br/>
 * In Jupyter: De voorbeelden downloaden vanaf de locatie van Github, verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), opent u de voorbeeld-map. |

## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe is geïnstalleerd in `/opt/caffe`.    |
| Overschakelen naar Python 2.7 | Voer `source activate root` uit. |
| Koppelingen naar voorbeelden      | Voorbeelden zijn opgenomen in `/opt/caffe/examples`.      |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe2      |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

X2Go aanmelden bij uw VM vervolgens start een nieuwe terminal en voer gebruiken

```
cd /opt/caffe/examples
source activate root
jupyter notebook
```

Een nieuw browservenster geopend met voorbeeldquery notitieblokken.

Binaire bestanden worden geïnstalleerd in /opt/caffe/build/install/bin.

Geïnstalleerde versie van Caffe Python 2.7 vereist en werkt niet met Python 3.5 standaard geactiveerd. Voer `source activate root` overschakelen Anaconda-omgeving. 

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe2 is geïnstalleerd in de [omgeving voor Python 2.7 (root) conda](dsvm-languages.md#python-linux-and-windows-server-2012-edition). De bron heeft `/opt/caffe2`. |
| Koppelingen naar voorbeelden      | Voorbeeldquery notitieblokken zijn opgenomen in JupyterHub. |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe      |
| Hoe gebruik / uitvoeren?    | * Op de terminal: Activeer de [hoofdmap Python-omgeving](dsvm-languages.md#python-linux-and-windows-server-2012-edition), start u Python en caffe2 importeren. <br/> * In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map Caffe2 voorbeeldquery notitieblokken vinden. Sommige laptops vereisen de hoofdmap Caffe2 moet worden ingesteld in de Python-code; Voer /opt/caffe2. |
| Notities maken | Caffe2 uit bron is gebaseerd op Linux en CUDA cuDNN en Intel MKL bevat. Het huidige doorvoeren is 0d9c0d48c6f20143d6404b99cc568efd29d5a4be die u hebt gekozen voor de stabiliteit op alle GPU's en voorbeelden getest. |

## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Chainer is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL en ChainerCV ook geïnstalleerd.   |
| Koppelingen naar voorbeelden      | Voorbeeldquery notitieblokken zijn opgenomen in JupyterHub. |
| Bijbehorende hulpprogramma's op de DSVM      | Caffe      |
| Hoe gebruik / uitvoeren?  | * Op de terminal: activeert de [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) omgeving, voeren _python_, Chainer vervolgens importeren. <br/>
* In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map Chainer voorbeeldquery notitieblokken vinden.


## <a name="deep-water"></a>Grondige Water

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework voor H2O      |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Grondige Water is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en is ook beschikbaar in `/dsvm/tools/deep_water`.   |
| Koppelingen naar voorbeelden      | Voorbeeldquery notitieblokken zijn opgenomen in JupyterHub.      |
| Bijbehorende hulpprogramma's op de DSVM      | H2O, mineraalwater      |

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

Grondige Water vereist CUDA 8 met cuDNN 5.1. Dit is niet in het pad van de bibliotheek standaard als andere frameworks grondige learning gebruik CUDA 9 en cuDNN 7. CUDA 8 + cuDNN 5.1 voor grondige Water gebruiken:

```
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
export CUDA_ROOT=/usr/local/cuda-8.0
```

Grondige Water gebruiken:
* Op de terminal: Activeer de [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) omgeving, voert u _python_. <br/>
* In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map deep_water voorbeeldquery notitieblokken vinden.

## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Wat is het?   | Grondige learning framework      |
| Ondersteunde DSVM edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | MXNet is geïnstalleerd in `C:\dsvm\tools\mxnet` in Windows en `/dsvm/tools/mxnet` op Linux. Python-bindingen in Python 3.5 zijn geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R-bindingen zijn ook geïnstalleerd op Ubuntu.   |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen.    |
| Bijbehorende hulpprogramma's op de DSVM      | Keras      |
| Hoe gebruik / uitvoeren?    | * Op een terminal: de juiste omgeving activeren en voer vervolgens Python. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map mxnet voor voorbeelden.  |
 | Notities maken | MXNet wordt samengesteld uit de bron op Linux. Deze versie bevat CUDA, cuDNN NCCL en MKL. |

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



## <a name="nvdia-smi"></a>NVDIA smi

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
| Hoe gebruik / uitvoeren?    | * Op een terminal activeren de versie van Python wilt (basis of py35), uitvoeren van python en theano importeren. <br/> 
* Selecteer de Python 2.7 of 3.5 kernel in Jupyter, en klik theano importeren.  
<br/>
U kunt een recente MKL fout omzeilen, moet u eerst de MKL threading laag instellen:<br/><br/>
_exporteren van MKL_THREADING_LAYER GNU =_
|



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
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | PyTorch is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks zijn opgenomen en voorbeelden kunnen worden gevonden in /dsvm/samples/pytorch.      |
| Bijbehorende hulpprogramma's op de DSVM      | Torch      |
| Hoe gebruik / uitvoeren | 
* Op een terminal: de juiste omgeving activeren en voer vervolgens Python. <br/>
 * In Jupyter: Verbinding maken met [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map PyTorch voor voorbeelden.  |

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

## <a name="tensorrt"></a>TensorRT

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een diepe Deductie server van NVIDIA leren. |
| Ondersteunde DSVM edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorRT wordt geïnstalleerd als een _apt_ pakket.   |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar [online](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).      |
| Bijbehorende hulpprogramma's op de DSVM      | TensorFlow verstrekken, MXNet Model Server  |



