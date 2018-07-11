---
title: Deep Learning en AI-frameworks - Azure | Microsoft Docs
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
ms.openlocfilehash: 891059a440189112c834f3402725781a6b4a3960
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952821"
---
# <a name="deep-learning-and-ai-frameworks"></a>Deep Learning en AI-frameworks
De [Data Science Virtual Machine](http://aka.ms/dsvm) (DSVM) en de [Deep Learning VM](http://aka.ms/dsvm/deeplearning) ondersteunt een aantal frameworks voor deep learning bij het opbouwen van kunstmatige intelligentie (AI)-toepassingen met Voorspellend en cognitieve functionaliteit, zoals afbeeldingen en language understanding. 

Hier volgen de details voor alle deep learning-frameworks beschikbaar is op de DSVM.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Microsoft Cognitive Toolkit (CNTK) in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).   |
| Koppelingen naar voorbeelden      | Voorbeeld van Jupyter notebooks zijn opgenomen.     |
| Meer hulpprogramma's op de DSVM      | Keras      |
| Hoe kan ik gebruik / uitvoeren?    | * Op een terminal: Python Voer vervolgens de juiste omgeving activeren. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map CNTK voor voorbeelden. |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorFlow in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).  |
| Koppelingen naar voorbeelden      | Voorbeeld van Jupyter notebooks zijn opgenomen.     |
| Meer hulpprogramma's op de DSVM      | Keras      |
| Hoe kan ik gebruik / uitvoeren?    | * Op een terminal: Python Voer vervolgens de juiste omgeving activeren. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map TensorFlow voor voorbeelden.  |

## <a name="horovod"></a>Horovod

|    |           |
| ------------- | ------------- |
| Wat is het?   | Distributed deep learning-frameworks voor TensorFlow      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Horovod in Python 3.5 is geïnstalleerd op [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Koppelingen naar voorbeelden      | [https://github.com/uber/horovod/tree/master/examples](https://github.com/uber/horovod/tree/master/examples)     |
| Meer hulpprogramma's op de DSVM      | TensorFlow      |
| Hoe kan ik gebruik / uitvoeren?    | Op een terminal: Python Voer vervolgens de juiste omgeving activeren. |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Wat is het?   | Op hoog niveau deep learning-API      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorFlow in Python 3.5 is geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). |
| Koppelingen naar voorbeelden      | https://github.com/fchollet/keras/tree/master/examples      |
| Meer hulpprogramma's op de DSVM      | Microsoft Cognitive Toolkit, TensorFlow, Theano      |
| Hoe kan ik gebruik / uitvoeren?    | * Op een terminal: Python Voer vervolgens de juiste omgeving activeren. <br/>
 * In Jupyter: Voorbeelden downloaden uit de Github-locatie, verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), opent u de voorbeeld-map. |

## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe is geïnstalleerd in `/opt/caffe`.    |
| Overschakelen naar Python 2.7 | Voer `source activate root` uit. |
| Koppelingen naar voorbeelden      | Voorbeelden zijn opgenomen in `/opt/caffe/examples`.      |
| Meer hulpprogramma's op de DSVM      | Caffe2      |
### <a name="how-to-use--run-it"></a>Hoe kan ik gebruik / uitvoeren?  

X2Go Meld u aan bij uw virtuele machine, en vervolgens start een nieuwe terminal en voer gebruiken

```
cd /opt/caffe/examples
source activate root
jupyter notebook
```

Er wordt een nieuw browservenster geopend met voorbeeldnotitieblokken.

Binaire bestanden zijn in /opt/caffe/build/install/bin geïnstalleerd.

Geïnstalleerde versie van Caffe vereist Python 2.7 en werkt niet met Python 3.5 standaard geactiveerd. Voer `source activate root` om over te schakelen Anaconda-omgeving. 

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Caffe2 is geïnstalleerd in de [conda-omgeving voor Python 2.7 (root)](dsvm-languages.md#python-linux-and-windows-server-2012-edition). De bron is in `/opt/caffe2`. |
| Koppelingen naar voorbeelden      | Voorbeeldnotitieblokken zijn opgenomen in JupyterHub. |
| Meer hulpprogramma's op de DSVM      | Caffe      |
| Hoe kan ik gebruik / uitvoeren?    | * Op de terminal: activeert de [basis Python-omgeving](dsvm-languages.md#python-linux-and-windows-server-2012-edition), start u Python en caffe2 importeren. <br/> * In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map Caffe2 voorbeeldnotitieblokken vinden. Sommige laptops vereisen de hoofdmap Caffe2 moet worden ingesteld in de Python-code; Voer /opt/caffe2. |
| Opmerkingen bij de bouwen | Caffe2 van bron is gebaseerd op Linux en CUDA-, cuDNN- en Intel MKL bevat. De huidige doorvoer is 0d9c0d48c6f20143d6404b99cc568efd29d5a4be, die is gekozen voor de stabiliteit van alle GPU's en voorbeelden getest. |

## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Chainer is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL en ChainerCV opdrachtprompts ook geïnstalleerd.   |
| Koppelingen naar voorbeelden      | Voorbeeldnotitieblokken zijn opgenomen in JupyterHub. |
| Meer hulpprogramma's op de DSVM      | Caffe      |
| Hoe kan ik gebruik / uitvoeren?  | * Op de terminal: activeert de [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) omgeving, worden uitgevoerd _python_, chainer vervolgens te importeren. <br/>
* In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map Chainer voorbeeldnotitieblokken vinden.


## <a name="deep-water"></a>Diep Water

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks voor H2O      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Diep Water is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en is ook beschikbaar in `/dsvm/tools/deep_water`.   |
| Koppelingen naar voorbeelden      | Voorbeeldnotitieblokken zijn opgenomen in JupyterHub.      |
| Meer hulpprogramma's op de DSVM      | H2O, Sparkling Water      |

### <a name="how-to-use--run-it"></a>Hoe kan ik gebruik / uitvoeren?  

CUDA 8 moet diep Water waarbij cuDNN 5.1. Dit is niet in het bibliotheekpad standaard als andere frameworks voor deep learning CUDA-9 en cuDNN-7. CUDA 8 + cuDNN 5.1 gebruiken voor Deep Water:

```
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
export CUDA_ROOT=/usr/local/cuda-8.0
```

Diep Water gebruiken:
* Op de terminal: activeert de [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) omgeving, voert u _python_. <br/>
* In JupyterHub: [verbinding maken met JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), navigeer naar de map deep_water voorbeeldnotitieblokken vinden.

## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | MXNet is geïnstalleerd in `C:\dsvm\tools\mxnet` op Windows en `/dsvm/tools/mxnet` op Linux. Python-bindingen in Python 3.5 zijn geïnstalleerd op [Linux en Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) en Python 3.6 op [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R-bindingen worden ook geïnstalleerd op Ubuntu.   |
| Koppelingen naar voorbeelden      | Voorbeeld van Jupyter notebooks zijn opgenomen.    |
| Meer hulpprogramma's op de DSVM      | Keras      |
| Hoe kan ik gebruik / uitvoeren?    | * Op een terminal: Python Voer vervolgens de juiste omgeving activeren. <br/>
 * In Jupyter: Verbinding maken met [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) of [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map mxnet voor voorbeelden.  |
 | Opmerkingen bij de bouwen | MXNet wordt samengesteld uit de bron op Linux. Deze versie bevat CUDA-, cuDNN-, NCCL en MKL. |

## <a name="nvidia-digits"></a>NVIDIA CIJFERS

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-systeem van NVIDIA snel deep learning-modellen te trainen      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | CIJFERS is geïnstalleerd in `/dsvm/tools/DIGITS` en vindt u een service met de naam _cijfers_.   |
### <a name="how-to-use--run-it"></a>Hoe kan ik gebruik / uitvoeren?  

Meld u aan bij de virtuele machine met X2Go. Start de service op een terminal:

    sudo systemctl start digits

De service duurt ongeveer een minuut om te starten. Start een webbrowser en navigeer naar `http://localhost:5000`.



## <a name="nvidia-smi"></a>NVIDIA-smi

|    |           |
| ------------- | ------------- |
| Wat is het?   | NVIDIA-hulpprogramma voor het uitvoeren van query's GPU-activiteit      |
| Ondersteunde DSVM-edities      | Windows, Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _NVIDIA-smi_ is beschikbaar in het systeempad.   |
| Hoe kan ik gebruik / uitvoeren? | Start een opdrachtprompt (op Windows) of een terminal (op Linux) en voer vervolgens _nvidia-smi_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Theano in Python 2.7 is geïnstalleerd (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving.   |
| Meer hulpprogramma's op de DSVM      | Keras      |
| Hoe kan ik gebruik / uitvoeren?    | * Op een terminal activeren de Python-versie u wilt (root of py35), uitvoeren van python en theano importeren. <br/> 
* In Jupyter, selecteert u de Python 2.7 of 3,5 kernel en theano importeren.  
<br/>
Als tijdelijke oplossing voor een recente MKL bug, moet u eerst de MKL threading laag instellen:<br/><br/>
_exporteren van MKL_THREADING_LAYER GNU =_
|



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Torch is geïnstalleerd in `/dsvm/tools/torch`. PyTorch in Python 2.7 is geïnstalleerd (_hoofdmap_), evenals Python 3.5 (_py35_) omgeving.   |
| Koppelingen naar voorbeelden      | Torch-voorbeelden bevinden zich op `/dsvm/samples/torch`. PyTorch-voorbeelden bevinden zich op `/dsvm/samples/pytorch`.      |


## <a name="pytorch"></a>PyTorch

|    |           |
| ------------- | ------------- |
| Wat is het?   | Deep learning-frameworks      |
| Ondersteunde DSVM-edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | PyTorch is geïnstalleerd in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Koppelingen naar voorbeelden      | Voorbeeld van Jupyter notebooks zijn opgenomen en kunnen ook voorbeelden te vinden in /dsvm/samples/pytorch.      |
| Meer hulpprogramma's op de DSVM      | Torch      |
| Over het gebruik / uitvoeren | 
* Op een terminal: Python Voer vervolgens de juiste omgeving activeren. <br/>
 * In Jupyter: Verbinding maken met [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), open vervolgens de map PyTorch voor voorbeelden.  |

## <a name="mxnet-model-server"></a>MXNet Model-Server

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een server om HTTP-eindpunten voor MXNet en u kunt ONNX-modellen te maken      |
| Ondersteunde DSVM-edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _mxnet-model-server_ is beschikbaar op de terminal.   |
| Koppelingen naar voorbeelden      | Recente voorbeelden Zoek op de [MXNet Model serverpagina](https://github.com/awslabs/mxnet-model-server).    |
| Meer hulpprogramma's op de DSVM      | MXNet      |

## <a name="tensorflow-serving"></a>TensorFlow-servers

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een server inferentietaken uitgevoerd op een TensorFlow-model      |
| Ondersteunde DSVM-edities      | Linux     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | _tensorflow_model_server_ is beschikbaar op de terminal.   |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar [online](https://www.tensorflow.org/serving/).      |
| Meer hulpprogramma's op de DSVM      | TensorFlow      |

## <a name="tensorrt"></a>TensorRT

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een deep learning-server van NVIDIA Deductie. |
| Ondersteunde DSVM-edities      | Ubuntu     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | TensorRT wordt geïnstalleerd als een _apt_ pakket.   |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar [online](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).      |
| Meer hulpprogramma's op de DSVM      | TensorFlow levert, MXNet Model-Server  |



