---
title: Maken van een Ubuntu Linux Data Science Virtual Machine
titleSuffix: Azure
description: Configureer en maak een Data Science Virtual Machine voor Linux (Ubuntu) in Azure voor het analyseren en machine learning.
services: machine-learning
documentationcenter: ''
author: vijetajo
ms.author: vijetaj
manager: cgronlun
ms.custom: seodec18
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/13/2019
ms.openlocfilehash: 6d79bfa8f390d145bfd963b40497030140a3d135
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69013600"
---
# <a name="provision-the-data-science-virtual-machine-for-linux-ubuntu"></a>De virtuele Machine voor Datatechnologie voor Linux (Ubuntu) inrichten

De virtuele Machine voor Datatechnologie voor Linux is een virtuele machine op basis van een Ubuntu-installatiekopie waarmee u eenvoudig aan de slag met machine learning, met inbegrip van deep learning, in Azure. Hulpprogramma's voor deep learning omvatten:

* [Caffe](https://caffe.berkeleyvision.org/): Een diep leer raamwerk dat is gebouwd voor snelheid, expressivity en modulariteit
* [Caffe2](https://github.com/caffe2/caffe2): Een platformoverschrijdende versie van Caffe
* [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK): Een grondige learning software Toolkit van micro soft Research
* [H2O](https://www.h2o.ai/): Een open source big data-platform en Graphical User Interface
* [Keras](https://keras.io/): Een Neural-netwerk-API op hoog niveau in Python voor tensor flow, Microsoft Cognitive Toolkit en Theano
* [MXNet](https://mxnet.io/): Een flexibele en efficiënte diep gaande bibliotheek met veel taal bindingen
* [NVIDIA-CIJFERS](https://developer.nvidia.com/digits): Een grafisch systeem dat veelvoorkomende diep gaande taken vereenvoudigt
* [PyTorch](https://pytorch.org/): Een python-bibliotheek op hoog niveau met ondersteuning voor dynamische netwerken
* [Tensor flow](https://www.tensorflow.org/): Een open source-bibliotheek voor Machine Intelligence van Google
* [Theano](http://deeplearning.net/software/theano/): Een python-bibliotheek voor het definiëren, optimaliseren en efficiënt evalueren van mathematische expressies met meerdere dimensies
* [Torch](http://torch.ch/): Een weten schappelijk computing Framework met brede ondersteuning voor machine learning-algoritmen
* CUDA-, cuDNN- en het NVIDIA-stuurprogramma
* Veel voorbeeld Jupyter-notebooks

Alle bibliotheken zijn de GPU-versies, maar ze ook worden uitgevoerd op de CPU.

De virtuele Machine voor Datatechnologie voor Linux bevat ook populaire hulpprogramma's voor data science en ontwikkeling, met inbegrip van:

* Microsoft R Server Developer Edition met Microsoft R Open
* Anaconda Python-distributie (versie 2.7 en 3.5), met inbegrip van populaire analysebibliotheken
* JuliaPro - een gecureerde distributie van Julia-taal met populaire wetenschappelijke en data analytics-bibliotheken
* Standalone Spark-instantie en één knooppunt Hadoop (HDFS, Yarn)
* JupyterHub - een omgeving met meerdere gebruikers Jupyter-notebookserver ondersteunt R, Python, PySpark, Julia kernels
* Azure Opslagverkenner
* Azure-opdrachtregelinterface (CLI) voor het beheren van Azure-resources
* Hulpprogramma's voor machine learning
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Een snelle machine learning systeem dat technieken ondersteunt, zoals online, hashing, allreduce, reducties, learning2search, actief en interactief leren
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Een hulp programma dat snelle en nauw keurige structuur implementatie biedt
  * [Rattle](https://togaware.com/rattle/): Een grafisch hulp programma waarmee u snel aan de slag kunt met gegevens analyse en machine learning in R.
  * [LightGBM](https://github.com/Microsoft/LightGBM): Een snel, gedistribueerd, verlopend raam werk voor verloop versterking van prestaties
* Azure SDK in Java, Python, node.js, Ruby, PHP
* Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure-services
* Ontwikkelprogramma's en editors (RStudio, PyCharm, IntelliJ, Emacs, vim)

Gegevenswetenschap omvat het doorlopen van een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
1. Het bouwen en testen van modellen
1. De modellen voor gebruik in intelligente toepassingen implementeren

Gegevenswetenschappers verschillende hulpprogramma's gebruiken om deze taken te voltooien. Worden erg tijdrovend zijn om de juiste versies van de software zoeken en vervolgens om te downloaden, compileren kan, en deze versies installeren.

De virtuele Machine voor Datatechnologie voor Linux is, kan deze last aanzienlijk vereenvoudigen. Gebruik dit voor uw analyticsproject een vliegende start geven. Hiermee kunt u taken in verschillende talen, waaronder R, Python, SQL, Java en C++. De Azure SDK opgenomen in de virtuele machine kunt u uw toepassingen kunt maken met verschillende services in Linux gebruiken voor het cloudplatform van Microsoft. Bovendien hebt u toegang tot andere talen zoals Ruby, Perl, PHP en node.js die vooraf zijn geïnstalleerd.

Er zijn geen kosten voor software voor deze data science VM-installatiekopie. U betaalt alleen de Azure-hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten te vinden in de [pagina met de vermelding op Azure Marketplace VM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de virtuele Machine voor Datatechnologie

Een [CentOS](linux-dsvm-intro.md) installatiekopie is ook beschikbaar zijn, met veel van dezelfde hulpprogramma's als de Ubuntu-installatiekopie. Een [Windows](provision-vm.md) installatiekopie is ook beschikbaar.

## <a name="prerequisites"></a>Vereisten

Voordat u een virtuele Machine voor Datatechnologie voor Linux maken kunt, moet u een Azure-abonnement hebben. Als u wilt aanvragen, Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/free/).

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Maken van uw virtuele Machine voor Datatechnologie voor Linux

Hier volgen de stappen voor het maken van een exemplaar van de Data Science Virtual Machine voor Linux:

1. Navigeer naar de virtuele machine weergeven op de [Azure-portal](https://portal.azure.com/#create/microsoft-dsvm.linux-data-science-vm-ubuntulinuxdsvmubuntu). U wordt mogelijk gevraagd om u aan te melden bij uw Azure-account als u zich nog niet hebt aangemeld. 
1. Klik op **maken** om de wizard te openen.
    ![configure-data-Science-VM](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
1. De volgende secties bevatten de invoer voor elk van de stappen in de wizard die wordt gebruikt om de micro soft-Data Science Virtual Machine te maken. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:

    a. **Grondbeginselen van**:
    
    * **Abonnement**: Als u meer dan één abonnement hebt, selecteert u een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht. U hebt privileges voor het maken van resources nodig voor dit abonnement.
    * **Resourcegroep**: U kunt een nieuwe groep maken of een bestaande groep gebruikt.
    * **Naam van de virtuele machine**: De naam van uw data Science-server die u maakt.
    * **Regio**: Selecteer het Data Center dat het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens, of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk.
    * **Beschikbaarheidsopties**: Stel dit in als u deze virtuele machine wilt gebruiken in beschikbaarheids sets/zones else de standaard waarde laten.
    * **Installatie kopie**: De standaard waarde behouden
    * **Grootte**: Selecteer een van de servertypen die voldoet aan de vereisten van het functionele en kostenbeperkingen. Selecteer een VM van de NC of ND-serie voor op GPU gebaseerde VM-exemplaren. 
    * **Gebruikersnaam**: Gebruikers naam van beheerder
    * **Open bare SSH-sleutel**: Open bare RSA-sleutel in enkelvoudige-regel indeling (u kunt wacht woord gebruiken in plaats van SSH-sleutel).
    
    b. **Schijven**:
    
    * **Type besturingssysteem schijf**: Kies **Premium-SSD** als u de voor keur geeft aan een SSD (Solid-State Drive). Kies anders **Standard HDD**.
    
    c. Voor de rest van de instellingen kunt u gewoon de standaard waarden gebruiken. U moet overwegen niet-standaard waarden, de muisaanwijzer over de informatieve koppeling voor meer informatie over de specifieke velden.
    
    Selecteer ten slotte **controleren en maken**
    
    d. **Controleren en maken**:
    
    * Nadat de validatie is geslaagd, controleert u of alle gegevens die u hebt ingevoerd juist zijn. Een koppeling volgen met de gebruiksvoorwaarden. Er zijn geen extra kosten in rekening gebracht voor de virtuele machine die u hebt gekozen in de grootte-invoer. Voor het starten van het inrichten, klikt u op **maken**.
    
    De inrichting duurt ongeveer 5 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Toegang tot de virtuele Machine voor Datatechnologie voor Linux

U kunt toegang tot de Ubuntu-DSVM drie methoden gebruiken:

1. SSH voor terminal-sessies
1. X2Go voor grafische sessies
1. JupyterHub en Jjupyterlab voor Jupyter-notebooks

U kunt ook een Data Science VM aan Azure Notebooks koppelen om Jupyter-notebooks op de virtuele machine uit te voeren en de beperkingen van de gratis servicelaag over te slaan. Zie notebooks projects [-Compute-laag beheren en configureren](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier)voor meer informatie.

### <a name="ssh"></a>SSH

Nadat de virtuele machine is gemaakt, kunt u zich aanmelden toe met behulp van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de tekst shell-interface. In Windows kunt u een SSH-clienthulpprogramma zoals [Putty](https://www.putty.org) downloaden. Als u liever een grafische desktop (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go beter dan X11 doorsturen tests uitgevoerd. Het is raadzaam om met behulp van de client X2Go voor een grafische interface voor het bureaublad.

### <a name="x2go"></a>X2Go

De Linux-VM is al ingericht met X2Go-server en gereed voor clientverbindingen accepteren. Voor verbinding met het bureaublad van de Linux-VM-grafische, voer de volgende procedure op de client:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. De X2Go-client wordt uitgevoerd, en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: De hostnaam of het IP-adres van uw Linux-Data Science VM.
     * **Aanmelding**: Gebruikers naam op de virtuele Linux-machine.
     * **SSH-poort**: De standaard waarde is 25.
     * **Sessie type**: Wijzig de waarde in XFCE. De Linux-VM ondersteunt momenteel alleen XFCE desktop.
   * **Tabblad Media**: U kunt geluids ondersteuning en client afdrukken uitschakelen als u deze niet nodig hebt.
   * **Gedeelde mappen**: Als u mappen wilt van uw client machines die op de virtuele Linux-machine zijn gekoppeld, voegt u de client computer directory's toe die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of de XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u snelkoppelingen voor toepassingen in het menu en pictogrammen op het bureaublad voor veel van de hulpprogramma's.

### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub en Jjupyterlab

De Ubuntu-DSVM wordt uitgevoerd [JupyterHub](https://github.com/jupyterhub/jupyterhub), een Jupyter-server van meerdere gebruikers. Als u verbinding wilt maken, gaat\/u naar https:/Your-VM-IP: 8000 op uw laptop of desktop, voert u de gebruikers naam en het wacht woord in die u hebt gebruikt om de virtuele machine te maken en meldt u zich aan. Veel voorbeeldnotitieblokken zijn beschikbaar voor u om te bladeren en uit te proberen.

Jjupyterlab, is de volgende generatie van Jupyter notebooks en JupyterHub, ook beschikbaar. Voor toegang tot de service meldt u zich aan bij JupyterHub en bladert u\/naar de URL https:/Your-VM-IP: 8000/User/your-username/Lab. U kunt Jjupyterlab instellen als de standaard notebook server door deze regel toe te voegen aan */etc/jupyterhub/jupyterhub_config.py*:

```python
c.Spawner.default_url = '/lab'
```

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Hulpprogramma's zijn geïnstalleerd op de Data Science Virtual Machine voor Linux

### <a name="deep-learning-libraries"></a>Deep Learning-bibliotheken

#### <a name="cntk"></a>CNTK

De Microsoft Cognitive Toolkit is een open-source, deep learning toolkit. Python-bindingen zijn beschikbaar in de hoofdmap en py35 Conda-omgevingen. Het bevat ook een opdrachtregelprogramma (cntk) die zich al in het pad.

Voorbeeld van Python-laptops zijn beschikbaar in JupyterHub. Als u een basis voorbeeld wilt uitvoeren op de opdracht regel, voert u de volgende opdrachten uit in de shell:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Zie voor meer informatie de sectie CNTK van [GitHub](https://github.com/Microsoft/CNTK), en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe

Caffe is een deep learning-structuur van de visie van Berkeley en Learning Center. Het is beschikbaar in /opt/caffe. Voorbeelden vindt u in /opt/caffe/examples.

#### <a name="caffe2"></a>Caffe2

Caffe2 is een deep learning-frameworks uit Facebook die is gebouwd op Caffe. Het is beschikbaar in Python 2.7 in de omgeving Conda-hoofdmap. Als u deze wilt activeren, voert u de volgende opdracht uit vanuit de shell:

```bash
source /anaconda/bin/activate root
```

Sommige laptops voorbeeld zijn beschikbaar in JupyterHub.

#### <a name="h2o"></a>H2O

H2O is een snelle, in het geheugen, gedistribueerde machine learning en predictive analytics-platform. Een Python-pakket is geïnstalleerd in de basis- en py35 Anaconda-omgevingen. Een R-pakket is ook geïnstalleerd. Uitvoeren voor het starten van H2O vanaf de opdrachtregel, `java -jar /dsvm/tools/h2o/current/h2o.jar`; er zijn verschillende [opdrachtregelopties](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) die u mogelijk wilt configureren. De gebruikersinterface van Flow Web kan worden geopend door te bladeren naar http://localhost:54321 aan de slag. Voorbeeldnotitieblokken zijn ook beschikbaar in JupyterHub.

#### <a name="keras"></a>Keras

Keras is een Neural-netwerk-API op hoog niveau in python die kan worden uitgevoerd op tensor flow, Microsoft Cognitive Toolkit of Theano. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen.

#### <a name="mxnet"></a>MXNet

MXNet is een deep learning-structuur die is ontworpen voor zowel efficiëntie en flexibiliteit. R en Python-bindingen op de DSVM heeft. Voorbeeldnotitieblokken zijn opgenomen in JupyterHub en voorbeeldcode is beschikbaar in /dsvm/samples/mxnet.

#### <a name="nvidia-digits"></a>NVIDIA CIJFERS

De NVIDIA Deep Learning-GPU-Training, cijfers, ook wel is een systeem om algemene deep learning-taken zoals het beheren van gegevens, het ontwerpen en training neurale netwerken op GPU-systemen en bewaken van de prestaties in realtime met geavanceerde visualisatie te vereenvoudigen.

CIJFERS is beschikbaar als een service met de naam van de cijfers. Start de service en blader naar http://localhost:5000 aan de slag.

CIJFERS is ook geïnstalleerd als een Python-module in de omgeving Conda-hoofdmap.

#### <a name="tensorflow"></a>TensorFlow

TensorFlow is deep learning-bibliotheek van Google. Het is een open-source software bibliotheek voor numerieke berekening met behulp van gegevens stroom grafieken. TensorFlow is beschikbaar in de py35 Python-omgeving, en sommige voorbeeldnotitieblokken zijn opgenomen in JupyterHub.

#### <a name="theano"></a>Theano

Theano is een Python-bibliotheek voor efficiënte numerieke berekeningen. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen. 

#### <a name="torch"></a>Torch

Torch is een wetenschappelijke computing structuur met brede ondersteuning voor machine learning-algoritmen. Het is beschikbaar in /dsvm/tools/torch en de interactieve sessie th en luarocks Pakketbeheer zijn beschikbaar vanaf de opdrachtregel. Voorbeelden zijn beschikbaar in /dsvm/samples/torch.

PyTorch is ook beschikbaar in de hoofdmap Anaconda-omgeving. Voorbeelden zijn in /dsvm/samples/pytorch.

### <a name="microsoft-r-server"></a>Microsoft R Server

R is een van de meest populaire talen voor data-analyse en machine learning. Als u R gebruiken voor uw analyses wilt, heeft de virtuele machine Microsoft R Server (MEVR) met de Microsoft R Open (BHT) en Math Kernel-bibliotheek (MKL). De MKL optimaliseert wiskundige bewerkingen gebruikelijk in analytische algoritmen. BHT is 100 procent is compatibel met CRAN R en een van de R-bibliotheken die is gepubliceerd in CRAN kan worden geïnstalleerd op de BHT. MEVR biedt u schalen en uitoefening van R-modellen in webservices. U kunt uw R-programma's in een van de standaard-editors, zoals RStudio, vi of Emacs bewerken. Als u liever met behulp van de editor Emacs, is deze vooraf worden geïnstalleerd. Het pakket Emacs ERDRUKKEN (Emacs spreekt statistieken) vereenvoudigt het gebruik met R-bestanden in de editor Emacs.

Console te starten, R, typt u **R** in de shell. Met deze opdracht gaat u naar een interactieve omgeving. Voor het ontwikkelen van uw R-programma, u doorgaans een editor zoals Emacs of vi gebruiken en voer de scripts in R. Met RStudio hebt u een volledige grafische IDE-omgeving voor het ontwikkelen van uw R-programma.

Er is ook een R-script voor installatie de [Top 20 R-pakketten](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) als u wilt. Met dit script kan worden uitgevoerd nadat u zich in de interactieve interface van R, die kan worden ingevoerd (zoals vermeld) door te typen **R** in de shell.  

### <a name="python"></a>Python

Anaconda Python is geïnstalleerd met Python 2.7 en 3,5-omgevingen. De 2,7 omgeving heet _hoofdmap_, en de 3,5 omgeving heet _py35_. Deze verdeling bevat de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten.

De omgeving py35 is de standaardinstelling. Aan de hoofdmap (2.7)-omgeving te activeren:

```bash
source activate root
```

De omgeving py35 opnieuw activeren:

```bash
source activate py35
```

Voor het aanroepen van een Python-interactieve sessie, typt u gewoon **python** in de shell. 

Installeren van extra Python-bibliotheken met behulp van ```conda``` of ```pip``` . Voor pip, Hiermee activeert u de juiste omgeving eerst als u niet dat de standaardwaarde wilt:

```bash
source activate root
pip install <package>
```

Of geef het volledige pad naar het pip:

```bash
/anaconda/bin/pip install <package>
```

Voor conda, moet u altijd de omgevingsnaam van de opgeven (_py35_ of _hoofdmap_):

```bash
conda install <package> -n py35
```

Als u zich op een grafische interface of X11 set up doorsturen hebt, kunt u typen **pycharm** de PyCharm Python IDE starten. U kunt de standaard-teksteditors. Bovendien kunt u Spyder, een Python IDE die wordt geleverd met Anaconda Python-distributies. Spyder moet een grafische bureaublad of X11 doorsturen. Een snelkoppeling naar Spyder is opgegeven in het grafische bureaublad.

### <a name="jupyter-notebook"></a>Jupyter Notebook

De distributie Anaconda wordt geleverd met een Jupyter-notebook, een omgeving voor het delen van code en -analyse. De Jupyter-notebook toegankelijk is via JupyterHub. U aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord.

De Jupyter-notebook-server is vooraf geconfigureerd met Python 2, 3 van Python en R-kernels. Er is een pictogram op het bureaublad met de naam 'Jupyter-Notebook' om te starten van de browser voor toegang tot de notebookserver. Als u van de virtuele machine via SSH of X2Go-client gebruikmaakt, kunt u ook bezoeken [ https://localhost:8000/ ](https://localhost:8000/) voor toegang tot de Jupyter-notebook-server.

> [!NOTE]
> Doorgaan als u certificaatwaarschuwingen.

U kunt de Jupyter-notebook-server openen vanaf elke host. Typ *https://\<VM DNS-naam of IP-adres\>: 8000 /*

> [!NOTE]
> Poort 8000 wordt standaard in de firewall geopend wanneer de virtuele machine is ingericht. 

We hebben verpakt voorbeeldnotitieblokken--één in Python en één in R. U kunt de koppeling naar de voorbeelden op de startpagina van de notebook ziet nadat u voor de Jupyter-notebook verifiëren met uw lokale Linux-gebruikersnaam en wachtwoord. U kunt een nieuwe notebook maken door het selecteren van **nieuw**, en vervolgens de juiste taal-kernel. Als er geen de **nieuw** klikken, klik op de **Jupyter** pictogram links bovenin om te gaan naar de startpagina van de notebookserver.

### <a name="apache-spark-standalone"></a>Apache Spark-zelfstandig

Een zelfstandig exemplaar van Apache Spark is vooraf geïnstalleerd op de Linux-DSVM om u te helpen bij het ontwikkelen van toepassingen voor Spark lokaal eerst voor testen en implementeren op grote clusters. U kunt de PySpark-programma's uitvoeren via de Jupyter-kernel. Wanneer u een Jupyter openen, klikt u op de **nieuw** knop en u ziet een lijst met beschikbare kernels. De 'Spark – Python' is de PySpark-kernel waarmee u Spark toepassingen bouwen met Python-taal. U kunt ook een python IDE zoals PyCharm of Spyder gebruiken om uw Spark-programma te bouwen. In deze zelfstandige instantie wordt de Spark-stack uitgevoerd binnen het aanroepende client programma, waardoor het sneller en eenvoudiger is om problemen op te lossen in vergelijking met het ontwikkelen van een Spark-cluster.

Een voorbeeld PySpark-notebook wordt in Jupyter die u in de map 'SparkML' onder de basismap van Jupyter ($HOME/notitieblokken/SparkML/pySpark vinden kunt) verstrekt. 

Als u programmeert in R voor Spark, kunt u Microsoft R Server, Spark of sparklyr gebruiken. 

Voordat u uitvoert in Spark-context in Microsoft R Server, moet u een setup-stap om in te schakelen van een lokale één knooppunt HDFS Hadoop en Yarn-instantie eenmaal te doen. Hadoop-services worden standaard geïnstalleerd maar uitgeschakeld op de DSVM. Als u wilt inschakelen, moet u de volgende opdrachten als root de eerste keer uitvoeren:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

U kunt de Hadoop-gerelateerde services stoppen wanneer u deze niet nodig hebt door uit te voeren```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```

Een voor beeld van het ontwikkelen en testen van MEVR in de externe Spark-context (dat is de zelfstandige Spark-instantie op de DSVM) is beschikbaar in de */dsvm/samples/Mrs* -map.

### <a name="ides-and-editors"></a>IDE's en editors

U hebt de keuze uit verschillende code editors, waaronder VI/VIM, emacs, PyCharm, RStudio en IntelliJ. IntelliJ, RStudio PyCharm grafische editors zijn en moet u zijn aangemeld met een grafische bureaublad om ze te gebruiken. Deze editors hebben bureaublad- en toepassingsservices snelkoppelingen in het menu te starten.

**VIM** en **Emacs** editors op basis van tekst zijn. Op Emacs, hebben we een add-on pakket met de naam Emacs spreekt statistieken (eldingen ONDERDRUKKEN) die werken met R gemakkelijker in de editor Emacs geïnstalleerd. Meer informatie kunt vinden op [ERDRUKKEN](https://ess.r-project.org/).

**LaTex** wordt geïnstalleerd via de texlive-package samen met een invoegtoepassing Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) pakket vereenvoudigt het ontwerpen van uw documenten LaTex binnen Emacs.  

### <a name="databases"></a>Databases

#### <a name="graphical-sql-client"></a>Grafische SQL-client

**SQuirrel SQL**, een grafische SQL-client is opgegeven voor het verbinding maken met verschillende databases (zoals Microsoft SQL Server- en MySQL) en SQL-query's uitvoeren. U kunt SQuirrel SQL uitvoeren vanuit een grafische bureaublad sessie (bijvoorbeeld met behulp van de X2Go-client) met behulp van een bureaublad pictogram, of met behulp van de volgende opdracht in de shell:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

Voordat u het eerste gebruik, stuurprogramma's en database-aliassen hebt ingesteld. Het JDBC-stuurprogramma's bevinden zich op:

*/usr/share/Java/jdbcdrivers*

Zie voor meer informatie, [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Opdrachtregelprogramma's voor toegang tot Microsoft SQL Server

Het pakket ODBC-stuurprogramma voor SQL Server wordt geleverd met twee opdrachtregelprogramma's:

**bcp**: Met het BCP-hulp programma worden gegevens bulksgewijs gekopieerd tussen een exemplaar van Microsoft SQL Server en een gegevens bestand in een door de gebruiker opgegeven indeling. Het hulpprogramma bcp kan worden gebruikt om te importeren van grote aantallen nieuwe rijen in SQL Server-tabellen, of gegevens uit tabellen exporteren naar gegevensbestanden. Om gegevens te importeren in een tabel, moet u een bestandsindeling die is gemaakt voor deze tabel gebruiken of de structuur van de tabel en de typen gegevens die geldig voor de kolommen zijn begrijpen.

Zie voor meer informatie, [verbinding te maken met bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: U kunt Transact-SQL-instructies invoeren met het Sqlcmd-hulp programma, evenals systeem procedures en script bestanden bij de opdracht prompt. Dit hulpprogramma maakt gebruik van ODBC om uit te voeren van Transact-SQL-batches.

Zie voor meer informatie, [verbinding te maken met sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Er zijn enkele verschillen in dit hulpprogramma tussen Linux en Windows-platforms. Zie de documentatie voor meer informatie.

#### <a name="database-access-libraries"></a>Database toegang bibliotheken

Er zijn bibliotheken beschikbaar is in R en Python op access-databases.

* In R, de **RODBC** pakket of **dplyr** pakket kunt u query's uitvoeren of SQL-instructies uitvoeren op de databaseserver.
* In Python, de **pyodbc** -bibliotheek biedt toegang tot de database met ODBC-als de onderliggende laag.  

### <a name="azure-tools"></a>Azure-hulpprogramma 's

De volgende Azure-hulpprogramma's zijn geïnstalleerd op de virtuele machine:

* **Azure-opdracht regel interface**: Met de Azure CLI kunt u Azure-resources maken en beheren via shell-opdrachten. Voor het aanroepen van de Azure-hulpprogramma's, typt u gewoon **azure help**. Zie voor meer informatie de [documentatiepagina van Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Storage Explorer**: Microsoft Azure Storage Explorer is een grafisch hulp programma dat wordt gebruikt om te bladeren door de objecten die u in uw Azure-opslag account hebt opgeslagen, en om gegevens van en naar Azure-blobs te uploaden en te downloaden. U kunt Storage Explorer openen via het pictogram van de snelkoppeling op het bureaublad. U kunt het aanroepen van een shell-prompt door te typen **StorageExplorer**. U moet zijn aangemeld bij een X2Go-client of X11 door sturen hebben ingesteld.
* **Azure-bibliotheken**: Hier volgen enkele van de vooraf geïnstalleerde bibliotheken.
  
  * **Python**: De Azure-gerelateerde bibliotheken in python die zijn geïnstalleerd, zijn **Azure**, **azureml**, **pydocumentdb**en **pyodbc**. Met de eerste drie bibliotheken, kunt u toegang tot Azure storage-services, Azure Machine Learning en Azure Cosmos DB (een NoSQL-database in Azure). De vierde servicebibliotheek pyodbc (samen met het Microsoft ODBC-stuurprogramma voor SQL Server), kunt u toegang tot SQL Server, Azure SQL Database en Azure SQL Data Warehouse met Python met behulp van een ODBC-interface. Voer **pip lijst** om te zien van de vermelde bibliotheken. Zorg dat u deze opdracht wordt uitgevoerd in de Python 2.7 en 3,5 omgevingen.
  * **R**: De Azure-gerelateerde bibliotheken in R die zijn geïnstalleerd, zijn **AzureML** en **RODBC**.
  * **Java**: De lijst met Azure Java-bibliotheken vindt u in de map **/dsvm/SDK/AzureSDKJava** op de virtuele machine. De sleutel-bibliotheken zijn Azure-opslag en beheer van API's, Azure Cosmos DB en JDBC-stuurprogramma's voor SQL Server.  

U hebt toegang tot de [Azure-portal](https://portal.azure.com) van de vooraf geïnstalleerde Firefox-browser. In de Azure-portal, kunt u maken, beheren en controleren van Azure-resources.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning is een volledig beheerde cloudservice waarmee u kunt bouwen, implementeren en delen van predictive analytics-oplossingen. U bouwen uw experimenten en modellen in Azure Machine Learning Studio. Deze kan worden geopend via een webbrowser op de virtuele machine voor datatechnologie recentst [Microsoft Azure Machine Learning](https://studio.azureml.net).

Nadat u zich bij Azure Machine Learning Studio aanmelden, hebt u toegang tot een experimenten-canvas waar u een logische stroom voor de machine learning-algoritmen kunt bouwen. U ook toegang hebben tot een Jupyter-notebook die worden gehost op Azure Machine Learning en werkt naadloos samen met de experimenten in Machine Learning Studio. Operationeel maken van de machine learning-modellen die u hebt gemaakt door ze in een webservice-interface. Met inzet machine learning-modellen kunnen clients die in elke taal zijn geschreven, voor spellingen van deze modellen aanroepen. Zie voor meer informatie de [documentatie voor Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

U kunt ook uw modellen in R of Python op de virtuele machine maken en vervolgens implementeren in productie met Azure Machine Learning. Hebben we bibliotheken geïnstalleerd in R (**AzureML**) en Python (**azureml**) om deze functionaliteit te schakelen.

Zie voor meer informatie over het implementeren van modellen in R en Python in Azure Machine Learning [tien dingen die u in de Data science Virtual Machine doen kunt](vm-do-ten-things.md) (met name de sectie ' modellen met behulp van R of Python maken en deze operationeel te maken met behulp van Azure Machine Learning').

> [!NOTE]
> Deze instructies zijn geschreven voor de Windows-versie van de Data Science VM. Maar de verstrekte informatie bevat over het implementeren van modellen op Azure Machine Learning is van toepassing op de Linux-VM.

### <a name="machine-learning-tools"></a>Hulpprogramma's voor machine learning

De virtuele machine wordt geleverd met enkele machine learning-hulpprogramma's en -algoritmen die zijn vooraf gecompileerde en vooraf lokaal zijn geïnstalleerd. Deze omvatten:

* **Vowpal Wabbit**: Een Fast Online Learning-algoritme.
* **xgboost**: Een hulp programma dat geoptimaliseerde, gestimuleerde structuur algoritmen biedt.
* **Rattle**: Een R-gebaseerd grafisch hulp programma voor het snel verkennen en model leren van gegevens.
* **Python**: Anaconda python wordt geleverd met machine learning algoritmen met bibliotheken zoals Scikit. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **LightGBM**: Een snelle, gedistribueerde, hoogwaardige kleur overgang voor het verbeteren van het Framework op basis van de beslissings structuur algoritmen.
* **R**: Er is een uitgebreide bibliotheek met machine learning functies beschikbaar voor R. Enkele van de vooraf geïnstalleerde bibliotheken zijn LM, GLM, randomForest, rpart. Andere bibliotheken kunnen worden geïnstalleerd door uit te voeren:
  
        install.packages(<lib name>)

Hier is aanvullende informatie over de eerste drie machine learning-hulpprogramma's in de lijst.

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit is een machine learning-systeem dat gebruikmaakt van technieken zoals online, hash, allreduce, kortingen, learning2search, actief, en interactieve training.

Als u het hulp programma wilt uitvoeren op een eenvoudig voor beeld, gebruikt u de volgende opdrachten:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Er zijn andere, groter demo's in die map. Zie voor meer informatie over VW [in deze sectie van GitHub](https://github.com/JohnLangford/vowpal_wabbit), en de [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost

Dit is een bibliotheek die is ontworpen en geoptimaliseerd voor boosted (structuur)-algoritmen. Het doel van deze bibliotheek is om de grenzen van de berekening van machines tot het uiterste dat nodig is voor grootschalige structuur versterking die schaalbare, draagbare en nauwkeurige.

Deze is geleverd als een opdrachtregel weergegeven, evenals een R-bibliotheek.

U kunt een interactieve R-sessie starten voor het gebruik van deze bibliotheek in R (door te typen **R** in de shell), en laad de bibliotheek.

Hier volgt een eenvoudig voorbeeld die u in de R-prompt uitvoeren kunt:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

Als u wilt uitvoeren vanaf de opdrachtregel xgboost, vindt hier u de opdrachten uit te voeren in de shell:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Een bestand .model is geschreven naar de map die is opgegeven. Informatie over deze demo-voorbeeld vindt [op GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Zie voor meer informatie over xgboost, de [xgboost documentatiepagina](https://xgboost.readthedocs.org/en/latest/), en de bijbehorende [GitHub-opslagplaats](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rammelaar

Rattle (de **R** **A**analytische **T**lpmiddel **T**o **L**verdienen **E** asily) maakt gebruik van GUI-gebaseerde gegevens verkennen en modelleren. Deze geeft statistische en visuele samenvattingen van gegevens, transformaties die gemakkelijk kan worden gemodelleerd, zonder supervisie en onder supervisie modellen uit de gegevens is gebaseerd, geeft de prestaties van modellen grafische vorm en scores nieuwe gegevens worden ingesteld. Er wordt ook gegenereerd R-code voor het repliceren van de bewerkingen in de gebruikersinterface die kunnen worden rechtstreeks in R uitvoeren of als uitgangspunt gebruikt voor verdere analyse.

Als u wilt uitvoeren Rammelaar, moet u zich in een grafische aanmelden-bureaubladsessie. Typ op de terminal ```R``` in te voeren van de R-omgeving. Voer de volgende opdrachten bij de R-prompt:

```R
library(rattle)
rattle()
```

Nu een grafische interface wordt geopend met een set tabbladen. Hier volgen de Quick Start-stappen in Rattle die nodig zijn om een voor beeld weer te geven van een gegevens verzameling en een model te bouwen. U wordt gevraagd in enkele van de volgende stappen uit om automatisch te installeren en sommige vereiste R-pakketten die nog niet op het systeem worden geladen.

> [!NOTE]
> Als u geen toegang tot het pakket installeren in de systeemmap (de standaardinstelling), ziet u mogelijk een prompt in uw R-console-venster om pakketten te installeren op uw persoonlijke bibliotheek. Antwoord *y* als u deze stappen ziet.

1. Klik op **Uitvoeren**.
1. Een dialoogvenster verschijnt, waarin u wordt gevraagd als u wilt gebruiken van de voorbeeldgegevensset van weer. Klik op **Ja** laden in het voorbeeld.
1. Klik op de **Model** tabblad.
1. Klik op **Execute** een beslissingsstructuur maken.
1. Klik op **tekenen** om weer te geven van de beslissingsstructuur.
1. Klik op de **Forest** keuzerondje en klikt u op **Execute** aan het bouwen van een willekeurige forest.
1. Klik op de **evalueren** tabblad.
1. Klik op de **risico** keuzerondje en klikt u op **Execute** om twee risico (cumulatief) prestaties grafieken weer te geven.
1. Klik op de **Log** tabblad om de genereren R-code voor de voorgaande bewerkingen weer te geven.
   (Vanwege een fout in de huidige release van Rammelaar, u hoeft in te voegen een *#* teken vóór *... dit logboek exporteren*  in de tekst van het logboek.)
1. Klik op de **exporteren** om op te slaan van de R-script-bestand met de naam *weather_script. R* naar de basismap.

U kunt afsluiten Rammelaar en R. U kunt nu het gegenereerde R-script wijzigen of als het is om uit te voeren op elk gewenst moment om te worden herhaald alles wat u in de gebruikersinterface Rattle is uitgevoerd. Met name voor beginners in R is dit een eenvoudige manier om snel analyses en machine learning in een eenvoudig grafische interface, tijdens het automatisch genereren van code in R om te wijzigen en/of meer informatie.

## <a name="next-steps"></a>Volgende stappen

Hier ziet u hoe u kunt doorgaan met uw learning en verkennen:

* De [gegevenswetenschap op de virtuele Machine voor Datatechnologie voor Linux](linux-dsvm-walkthrough.md) procedure ziet u hoe u enkele algemene datatechnologietaken met de Linux Data Science VM ingericht hier uitvoert. 
* Verken de verschillende hulpprogramma's voor data science op de virtuele machine voor datatechnologie door het uitproberen van de hulpprogramma's die worden beschreven in dit artikel. U kunt ook uitvoeren *dsvm-meer-info* op de shell binnen de virtuele machine voor een algemene inleiding tot en tips voor meer informatie over de hulpprogramma's geïnstalleerd op de virtuele machine.  
* Informatie over het bouwen van analytische oplossingen voor end-to-end systematisch met behulp van de [Team Data Science Process](https://aka.ms/tdsp).
* Ga naar de [Azure AI Gallery](https://gallery.azure.ai/) voor machine learning en data analytics-voorbeelden die gebruikmaken van de Azure AI-services.
