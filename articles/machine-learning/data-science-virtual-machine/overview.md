---
title: Inleiding tot Azure Data Science Virtual Machine voor Linux en Windows | Microsoft Docs
description: Belangrijke analysescenario's en componenten voor Windows en Linux Data Science Virtual Machines.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 10/27/2017
ms.author: gokuma
ms.openlocfilehash: a6f085b0fa11f117b181714a3b92cdc3a7c9e9a9
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870669"
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Inleiding tot Azure Data Science Virtual Machine voor Linux en Windows

Data Science Virtual Machine (DSVM) is een aangepaste VM-installatiekopie in de Azure-cloud van Microsoft, die speciaal is gebouwd voor datatechnologie. DSVM bevat veel populaire gegevenswetenschap- en andere hulpprogramma's die vooraf zijn geïnstalleerd en geconfigureerd voor het snel ontwikkelen van intelligente toepassingen voor geavanceerde analyses. Deze is beschikbaar op Windows Server en Linux. We bieden de Windows-editie van DSVM op Server 2016 en Server 2012. We bieden Linux-edities van de DSVM op Ubuntu 16.04 LTS en CentOS 7.4.

In dit onderwerp wordt beschreven wat u kunt doen met de Data Science VM. Ook vindt u hier een overzicht van enkele van de belangrijkste scenario's voor het gebruik van de VM, een specificatie van de belangrijkste functies die beschikbaar zijn in de Windows- en Linux-versie en instructies over hoe u deze kunt gaan gebruiken.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Wat kan ik doen met de Data Science Virtual Machine?
Het doel van de Data Science Virtual Machine (DSVM) is om dataprofessionals van allerlei vaardigheidsniveaus en in allerlei branches te voorzien van een probleemloze, vooraf geconfigureerde en volledig geïntegreerde datatechnologieomgeving. In plaats van zelf een vergelijkbare werkruimte uit te rollen, kunt u een DSVM inrichten. Zo bespaart u dagen of zelfs _weken_ aan installatie-, configuratie- en pakketbeheerprocessen. Nadat uw DSVM is toegewezen, kunt u meteen aan uw datatechnologieproject gaan werken.

De Data Science VM is ontworpen en geconfigureerd om met een breed scala aan gebruiksscenario's te kunnen werken. U kunt uw omgeving omhoog of omlaag schalen naarmate de behoeften van uw project veranderen. U kunt de taal van uw voorkeur gebruiken om datatechnologietaken te programmeren en andere hulpprogramma's installeren om het systeem aan te passen aan uw specifieke behoeften.

## <a name="key-scenarios"></a>Belangrijke scenario's
In deze sectie wordt een aantal belangrijke scenario's besproken waarvoor de Data Science VM kan worden geïmplementeerd.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vooraf geconfigureerd analysebureaublad in de cloud
De Data Science VM biedt een basisconfiguratie voor datatechnologieteams die hun lokale bureaublad willen vervangen door een beheerd bureaublad in de cloud. Deze basisconfiguratie zorgt ervoor dat alle datawetenschappers in een team een consistente installatie hebben, zodat ze experimenten kunnen verifiëren en de samenwerking kunnen bevorderen. Deze basisconfiguratie verlaagt ook de kosten, doordat er minder systeembeheer nodig is en het minder tijd kost om de verschillende benodigde softwarepakketten voor geavanceerde analyses te evalueren, installeren en beheren.  

### <a name="data-science-training-and-education"></a>Training en onderwijs voor datatechnologie
Zakelijke trainers en docenten die datatechnologie onderwijzen, werken meestal met een installatiekopie van een virtuele machine om ervoor te zorgen dat hun studenten een consistente configuratie hebben en de voorbeelden werken zoals verwacht. De Data Science VM maakt een omgeving op aanvraag met een consistente configuratie, waardoor zich minder problemen met ondersteuning en incompatibiliteit voordoen. In gevallen waarin deze omgevingen vaak moeten worden gemaakt, met name voor kortere trainingen, levert dit veel voordelen op.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Flexibele capaciteit op aanvraag voor grootschalige projecten
Datatechnologie-hackathons/-wedstrijden of grootschalige gegevensmodellen en grootschalig dataonderzoek vereisen uitgebreide hardwarecapaciteit, doorgaans voor een korte duur. Met de Data Science VM kan de datatechnologieomgeving snel op aanvraag worden gerepliceerd, op omhoog geschaalde servers waarop experimenten kunnen worden uitgevoerd die krachtige computerbronnen vereisen.

### <a name="short-term-experimentation-and-evaluation"></a>Experimenten en evaluatie op korte termijn
De Data Science VM kan worden gebruikt om hulpprogramma's als Microsoft ML Server, SQL Server, Visual Studio-hulpprogramma's, Jupyter, deep learning-/ML-toolkits en nieuwe hulpprogramma's die populair zijn in de community met minimale installatie-inspanningen te evalueren en ermee te gaan werken. Omdat de Data Science VM snel kan worden ingesteld, kan deze worden toegepast in andere gebruiksscenario's voor de korte termijn, zoals het repliceren van gepubliceerde experimenten, het uitvoeren van demo's, het volgen van procedures in onlinesessies en zelfstudies voor vergaderingen.

### <a name="deep-learning"></a>Deep learning
De Data Science VM kan worden gebruikt voor trainingsmodellen die gebruik maken van deep-learningalgoritmen op GPU-hardware (Graphics Processing Unit). DVSM maakt gebruik van de VM-schalingsmogelijkheden van Azure Cloud zodat u naar behoefte GPU-hardware in de cloud kunt gebruiken. Wanneer u grote modellen traint of snelle berekeningen nodig hebt, kunt u met behoud van dezelfde besturingssysteemschijf overschakelen naar een GPU-VM.  De Windows Server 2016-editie van DSVM wordt geleverd met vooraf geïnstalleerde GPU-stuurprogramma's, frameworks en GPU-versies van frameworks voor deep learning. In de Linux-editie is deep learning met behulp van de GPU ingeschakeld op zowel de CentOS- als de Ubuntu-DSVM. U kunt de Ubuntu-, CentOS- of Windows 2016-editie van Data Science VM implementeren op een virtuele Azure-machine die niet is gebaseerd op een GPU. Alle frameworks voor deep learning vallen dan terug op de CPU-modus. 

## <a name="whats-included-in-the-data-science-vm"></a>Wat is opgenomen in de Data Science VM?
In de Data Science VM zijn vele populaire hulpprogramma's voor datatechnologie en deep learning vooraf geïnstalleerd en geconfigureerd. Hiertoe behoren ook hulpprogramma's die het werken vergemakkelijken met verschillende Azure-gegevens- en analyseproducten, zoals Microsoft ML Server (R, Python) voor het bouwen van voorspellende modellen of SQL Server 2017 voor het verkennen van grootschalige gegevenssets. Ook zijn er allerlei andere hulpprogramma's uit de open-source community en van Microsoft in de VM opgenomen, evenals voorbeeldcode en notebooks. In de volgende tabel vindt u een specificatie en vergelijking van de belangrijkste componenten die zijn opgenomen in de Windows- en Linux-editie van de Data Science Virtual Machine.


| **Hulpprogramma**                                                           | **Windows-editie** | **Linux-editie** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) met populaire pakketten die vooraf zijn geïnstalleerd   |J                      | J             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer-editie bevat, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) parallel en gedistribueerd high-performance framework (R & Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) - nieuwe geavanceerde algoritmen van Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R- en Python-operationalisering](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |J                      | J |
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) Pro-Plus met gedeelde activering - Excel, Word en PowerPoint   |J                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 met populaire pakketten die vooraf zijn geïnstalleerd    |J                      |J              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) met populaire pakketten voor Julia-taal vooraf geïnstalleerd                         |J                      |J              |
| Relationele databases                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Databasehulpprogramma's                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> *ODBC-/JDBC-stuurprogramma's| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (querytool), <br /> * bcp, sqlcmd <br /> *ODBC-/JDBC-stuurprogramma's|
| Schaalbare in-database analyses met SQL Server ML-services (R, Python) | J     |N              |
| **[Jupyter-Notebook Server](http://jupyter.org/) met de volgende kernels**                                  | J     | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (alleen Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | J |
| JupyterHub (notebookserver voor meerdere gebruikers)| N | J |
| JupyterLab (notebookserver voor meerdere gebruikers) | N | Y (alleen Ubuntu) |
| **Ontwikkelingsprogramma's, IDE en code-editors**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) met Git-plugin, Azure HDInsight (Hadoop), Data Lake, SQL Server Data-hulpprogramma's, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), en [R-hulpprogramma's voor Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim en Emacs | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git en GitBash | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* .Net Framework | J | N |
| PowerBI Desktop | J | N |
| SDK's voor toegang tot Azure en services van de Cortana Intelligence Suite | J | J |
| **Hulpprogramma's voor gegevensverplaatsing en -beheer** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Opslagverkenner | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Blob FUSE-stuurprogramma](https://github.com/Azure/azure-storage-fuse) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB-hulpprogramma voor gegevensmigratie](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Data Management Gateway](https://msdn.microsoft.com/library/dn879362.aspx): gegevens verplaatsen tussen on-premises en de cloud | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix-/Linux-opdrachtregelprogramma’s | J | J |
| [Apache Drill](http://drill.apache.org) voor dataonderzoek | J | J |
| **Hulpprogramma's voor Machine Learning** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integratie met [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](https://togaware.com/rattle/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (alleen Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/), [Deep Water](https://www.h2o.ai/deep-water/) | N | Y (alleen Ubuntu) |
| **Hulpprogramma's voor Deep Learning** <br>Alle hulpprogramma's werken op een GPU of CPU |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) (Windows 2016) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | Y (Windows 2016) | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Horovod](https://github.com/uber/horovod) | N | Y (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | Y (Windows 2016) | J|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Chainer](https://chainer.org/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](http://pytorch.org/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorRT](https://developer.nvidia.com/tensorrt) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA-, cuDNN-, NVIDIA-stuurprogramma](https://developer.nvidia.com/cuda-toolkit) | J | J |
| **Platform voor Big Data (alleen Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Spark](http://spark.apache.org/) Standalone | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | J |

## <a name="get-started"></a>Aan de slag

### <a name="windows-data-science-vm"></a>Een Windows Data Science VM
* Zie [De Windows Data Science Virtual Machine inrichten](provision-vm.md) voor meer informatie over het maken en gebruiken van een Windows DSVM. Zie [Tien dingen die u kunt doen op de Data Science Virtual Machine](vm-do-ten-things.md) voor meer informatie over het uitvoeren van verschillende taken die nodig zijn voor uw datatechnologieproject op de Windows DSVM.

### <a name="linux-data-science-vm"></a>Linux Data Science VM
* Zie [De Data Science Virtual Machine voor Linux (Ubuntu) inrichten](dsvm-ubuntu-intro.md) voor meer informatie over het maken en gebruiken van een Ubuntu DSVM. Zie [Een Linux CentOS Data Science Virtual Machine inrichten in Azure](linux-dsvm-intro.md) voor meer informatie over het maken en gebruiken van een CentOS DSVM.
* Zie [Datatechnologie op de Linux Data Science Virtual Machine](linux-dsvm-walkthrough.md) voor een overzicht waarin wordt getoond hoe u enkele algemene datatechnologietaken met de Linux-VM uitvoert (zowel CentOS en Ubuntu).

## <a name="next-steps"></a>Volgende stappen
[Azure-handleiding voor R-ontwikkelaars](../r-developers-guide.md)
