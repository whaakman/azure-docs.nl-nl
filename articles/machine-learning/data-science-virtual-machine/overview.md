---
title: Inleiding tot Azure Data wetenschappelijke virtuele Machine voor Linux en Windows | Microsoft Docs
description: Sleutel analytics scenario's en onderdelen voor Windows en Linux gegevens wetenschappelijke virtuele Machines.
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: gokuma
ms.openlocfilehash: 8ee4af162ddaa64d4dbe83bebbb93e22409f041d
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Inleiding tot Azure Data wetenschappelijke virtuele Machine voor Linux en Windows

De gegevens wetenschappelijke virtuele Machine (DSVM) is een aangepaste VM-installatiekopie op van Microsoft Azure-cloud die speciaal voor gegevenswetenschap doen. DSVM bevat veel populaire gegevenswetenschap- en andere hulpprogramma's die vooraf zijn geïnstalleerd en geconfigureerd voor het snel ontwikkelen van intelligente toepassingen voor geavanceerde analyses. Het is beschikbaar op Windows Server en op Linux. We bieden de Windows-editie van DSVM op Server 2016 en Server 2012. Linux-edities van de DSVM op 16.04-TNS Ubuntu en CentOS 7.4 bieden we.

In dit onderwerp wordt beschreven wat u kunt doen met de VM van de wetenschappelijke gegevens, bevat een overzicht van enkele van de belangrijkste scenario's voor het gebruik van de virtuele machine, itemizes van de belangrijkste functies beschikbaar op de Windows- en Linux-versies en bevat instructies over het gebruik ervan aan de slag.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Wat kan ik doen met de gegevens wetenschappelijke virtuele Machine?
Het doel van de gegevens wetenschappelijke virtuele Machine (DSVM) is om te voorzien van een omgeving met wrijving gratis, vooraf geconfigureerde en volledig geïntegreerd gegevens wetenschappelijke gegevens professionals op alle kwalificatieniveaus en in alle rollen. In plaats van een vergelijkbare werkruimte rolt zelf, kunt u een DSVM - zodat u dagen of zelfs inrichten _weken_ op de installatie, configuratie en beheerprocessen pakket. Nadat uw DSVM is toegewezen, kunt u meteen beginnen op uw project van de wetenschappelijke gegevens.

De wetenschappelijke gegevens VM is ontworpen en geconfigureerd voor het werken met een breed scala aan gebruiksscenario's. U kunt uw omgeving omhoog of omlaag schalen naar gelang de behoeften van uw project wijzigen. U zijn kunt gebruiken de voorkeurstaal aan programma gegevens wetenschappelijke taken. U kunt andere hulpprogramma's installeren en aanpassen van het systeem voor uw exacte behoeften.

## <a name="key-scenarios"></a>Belangrijke scenario 's
Deze sectie stelt een aantal belangrijke scenario's waarvoor de wetenschappelijke gegevens VM kan worden geïmplementeerd.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vooraf geconfigureerde analytics bureaublad in de cloud
De wetenschappelijke gegevens VM biedt een basislijnconfiguratie voor gegevens wetenschappelijke teams willen hun lokale bureaubladen vervangen door een beheerde cloud-bureaublad. Deze basislijn zorgt ervoor dat alle gegevenswetenschappers in een team een consistente configuratie waarmee experimenten controleren en promoveer samenwerking. Ook verlaagt de kosten door de sysadmin-belasting en opslaan op de tijd die nodig zijn om te evalueren, installeren en onderhouden van de verschillende softwarepakketten nodig voor geavanceerde analyses.  

### <a name="data-science-training-and-education"></a>Training en onderwijs voor datatechnologie
Enterprise opleiders en onderwijsinstellingen die leren gebruiken gegevenswetenschap klassen bieden meestal de installatiekopie van een virtuele machine om ervoor te zorgen dat hun studenten een consistente configuratie hebt en dat werken de voorbeelden niet zoals verwacht. De wetenschappelijke gegevens VM maakt u een omgeving op aanvraag met een consistente configuratie die de ondersteuning en incompatibiliteit uitdagingen vereenvoudigt. Gevallen waarin deze omgevingen moeten vaak worden gemaakt met name voor kortere training klassen, profiteren aanzienlijk.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastische capaciteit op aanvraag voor grootschalige projecten
Gegevens wetenschappelijke hackathons/wedstrijden of grootschalige gegevens modelleren en exploratie vereisen hardwarecapaciteit doorgaans voor korte duur uitgebreid. De wetenschappelijke gegevens VM kan helpen bij het repliceren van de omgeving van de wetenschappelijke gegevens snel op verzoek, op schaal uit servers waarmee experimenten vereisen krachtige computerbronnen om te worden uitgevoerd.

### <a name="short-term-experimentation-and-evaluation"></a>Korte termijn experimenteren en evaluatie
De VM van de wetenschappelijke gegevens kunnen worden gebruikt om te evalueren of informatie over hulpprogramma's zoals Microsoft ML-Server, SQL Server, Visual Studio tools, Jupyter, diep learning / ML toolkits en nieuwe hulpprogramma's populaire in community met minimale inspanning setup. Aangezien de wetenschappelijke gegevens VM kan snel worden ingesteld, kan deze worden toegepast in andere op korte termijn gebruiksscenario's zoals het repliceren van gepubliceerde experimenten, demo's, volgende leidraden in sessies online of telefonische vergaderingen zelfstudies wordt uitgevoerd.

### <a name="deep-learning"></a>Grondige learning
De virtuele machine voor gegevenswetenschap kan worden gebruikt voor training model grondige learning-algoritmen met op GPU (Graphics processing unit) gebaseerde hardware. DSVM kunt met behulp van een VM schalen hebben van Azure-cloud, u op basis van GPU hardware gebruiken in de cloud volgens nodig. Een kunt overschakelen naar een VM op basis van GPU bij het trainen van grote modellen of Snelle berekeningen moet terwijl dezelfde schijf met het besturingssysteem.  De editie Windows Server 2016 van DSVM is voorgeïnstalleerd met GPU-stuurprogramma's en frameworks GPU-versies van grondige learning frameworks. Deep learning in GPU is ingeschakeld op de Linux op de CentOS en Ubuntu DSVMs. U kunt de editie Ubuntu, CentOS of Windows 2016 van gegevens wetenschappelijke VM implementeren naar Azure virtuele machine niet op basis van GPU in dat geval alle grondige learning frameworks terugval naar de CPU-modus wordt. 

## <a name="whats-included-in-the-data-science-vm"></a>Wat opgenomen in de VM van de wetenschappelijke gegevens?
De virtuele Machine van de gegevens wetenschap heeft veel populaire gegevenswetenschap en deep hulpmiddel al geïnstalleerd en geconfigureerd. Dit omvat ook hulpprogramma's waarmee u gemakkelijk kunt werken met diverse Azure-gegevens en analyse producten. U kunt verkennen en bouwen van voorspellende modellen op grote gegevenssets met behulp van de Server van de ML Microsoft (R, Python) of met behulp van SQL Server 2017. Een host met andere hulpprogramma's van de open-source-community en van Microsoft zijn ook opgenomen, evenals voorbeeld code en laptops. De volgende tabel itemizes en vergelijkt de belangrijkste onderdelen die deel uitmaken van de Windows- en Linux-edities van de gegevens wetenschappelijke virtuele Machine.


| **Hulpprogramma**                                                           | **Windows Edition** | **Linux Edition** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) met populaire pakketten die vooraf zijn geïnstalleerd   |J                      | J             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) ontwikkelaarsversie bevat, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) framework (R & Python) voor parallelle en gedistribueerde hoge prestaties<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) -nieuwe de nieuwste ML algoritmen van Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R en Python uitoefening](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |J                      | J |
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) professionele Plus met gedeelde activering - Excel, Word en PowerPoint   |J                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 met populaire pakketten die vooraf zijn geïnstalleerd    |J                      |J              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) met populaire pakketten voor Julia taal vooraf is geïnstalleerd                         |J                      |J              |
| Relationele databases                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Ontwikkelaarsversie| [PostgreSQL](https://www.postgresql.org/)(alleen CentOS) |
| Database-hulpprogramma 's                                                       | * SQL Server Management Studio <br/>* SQL Server integratieservices<br/>* [BCP sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC-stuurprogramma 's| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (query hulpprogramma wordt uitgevoerd), <br /> * bcp sqlcmd <br /> * ODBC/JDBC-stuurprogramma 's|
| Schaalbare in database-analyses met SQL Server ML-services (R, Python) | J     |N              |
| **[Jupyter-Notebook Server](http://jupyter.org/) met de volgende kernels**                                  | J     | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (alleen Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | J |
| JupyterHub (meerdere gebruikers notitieblokken server)| N | J |
| **Ontwikkelingsprogramma's, IDE en Code-editors**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) > met Git Plugin, Azure HDInsight (Hadoop), Data Lake, SQL Server Data tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), en [R-hulpprogramma's voor het visuele element Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim en Emacs | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git en GitBash | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* .Net Framework | J | N |
| PowerBI Desktop | J | N |
| SDK's voor toegang tot Azure en Cortana Intelligence Suite van services | J | J |
| **Verplaatsing van gegevens en beheerprogramma 's** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Opslagverkenner | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [BLOB ZEKERING stuurprogramma](https://github.com/Azure/azure-storage-fuse) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Hulpprogramma voor gegevensmigratie DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Data Management Gateway](https://msdn.microsoft.com/library/dn879362.aspx): gegevens verplaatsen tussen OnPrem en Cloud | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix/Linux-opdrachtregelprogramma 's | J | J |
| [Apache inzoomen](http://drill.apache.org) voor gegevensverkenning | J | J |
| **Machine Learning-hulpprogramma 's** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integratie met [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (alleen Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (alleen Ubuntu) |
| **Op basis van GPU Deep leren werken met hulpprogramma 's** |Windows Server 2016 edition  | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft cognitieve Toolkit (voorheen bekend als CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | J | J|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](http://pytorch.org/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow levering](https://www.tensorflow.org/serving/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, CUDNN, Nvidia stuurprogramma](https://developer.nvidia.com/cuda-toolkit) | J | J |
| **Platform voor big Data (alleen Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Spark](http://spark.apache.org/) zelfstandige | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | J |



## <a name="get-started-with-the-windows-data-science-vm"></a>Aan de slag met Windows gegevens wetenschappelijke VM
* Maak een instantie van de gewenste editie van Windows DSVM door te navigeren naar
  * [DSVM op basis van Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.windows-data-science-vm)
  
  of 
  * [Windows Server 2012 based DSVM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) 
* Klik op de **IT ophalen nu** knop.
* Meld u aan de virtuele machine van het externe bureaublad met de referenties die u hebt opgegeven toen u de virtuele machine hebt gemaakt.
* Als u wilt detecteren en de hulpprogramma's starten, klikt u op de **Start** menu.

## <a name="get-started-with-the-linux-data-science-vm"></a>Aan de slag met de Linux-VM wetenschappelijke van gegevens
* Maak een instantie van de gewenste versie van Linux DSVM door te navigeren naar 
  * [Ubuntu DSVM gebaseerd](http://aka.ms/dsvm/ubuntu)

  of

  * [DSVM op basis van centOS](http://aka.ms/dsvm/centos)

  
* Klik op de **nu downloaden** knop.
* Meld u aan de virtuele machine van een SSH-client, zoals Putty of SSH-opdracht, met de referenties die u hebt opgegeven toen u de virtuele machine hebt gemaakt.
* Voer in de shell-prompt dsvm meer informatie.
* Voor een grafische bureaublad, downloadt de client X2Go voor uw clientplatform [hier](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) en volg de instructies in het document Linux gegevens wetenschappelijke VM [Linux gegevens wetenschap-Machine inrichten](linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Volgende stappen
### <a name="for-the-windows-data-science-vm"></a>Voor de Windows-Gegevenswetenschap VM
* Zie voor meer informatie over het uitvoeren van specifieke hulpprogramma's beschikbaar op de Windows-versie [inrichten van de virtuele Machine voor Microsoft Data wetenschappelijke](provision-vm.md) en
* Zie voor meer informatie over het uitvoeren van verschillende taken die nodig zijn voor uw project van de wetenschappelijke gegevens op de virtuele machine van Windows [tien dingen die u op de virtuele Machine voor gegevenswetenschap doen kunt](vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Voor de Linux-Gegevenswetenschap VM
* Zie voor meer informatie over het uitvoeren van specifieke hulpprogramma's beschikbaar op de Linux-versie [Linux gegevens wetenschap-Machine inrichten](linux-dsvm-intro.md).
* Zie voor een overzicht waarin wordt getoond hoe u enkele algemene gegevens wetenschappelijke uitvoert met de Linux-VM [gegevenswetenschap op de Linux wetenschappelijke virtuele Machine gegevens](linux-dsvm-walkthrough.md).

