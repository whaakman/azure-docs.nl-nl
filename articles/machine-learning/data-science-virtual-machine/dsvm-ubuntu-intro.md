---
title: Maken van een Ubuntu Linux Data Science Virtual Machine
titleSuffix: Azure
description: Configureer en maak een Data Science Virtual Machine voor Linux (Ubuntu) in Azure voor het analyseren en machine learning.
services: machine-learning
documentationcenter: ''
author: gopitk
ms.author: gokuma
manager: cgronlun
ms.custom: seodec18
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: e3f17dd3717a57d184be7c9b8c73855c3fd2a768
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106767"
---
# <a name="provision-the-data-science-virtual-machine-for-linux-ubuntu"></a>De virtuele Machine voor Datatechnologie voor Linux (Ubuntu) inrichten

De virtuele Machine voor Datatechnologie voor Linux is een virtuele machine op basis van een Ubuntu-installatiekopie waarmee u eenvoudig aan de slag met machine learning, met inbegrip van deep learning, in Azure. Hulpprogramma's voor deep learning omvatten:

  * [Caffe](http://caffe.berkeleyvision.org/): een deep learning-frameworks die is gebouwd voor snelheid, expressivity en modulariteit mogelijk te maken
  * [Caffe2](https://github.com/caffe2/caffe2): een platformoverschrijdende-versie van Caffe
  * [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK): een deep learning toolkit voor software van Microsoft Research
  * [H2O](https://www.h2o.ai/): een open source big data-platform en de grafische gebruikersinterface
  * [Keras](https://keras.io/): een op hoog niveau neural network API in Python voor Theano en TensorFlow
  * [MXNet](http://mxnet.io/): een flexibele en efficiënte deep learning-bibliotheek met veel taalbindingen
  * [NVIDIA cijfers](https://developer.nvidia.com/digits): een grafische systeem dat algemene taken voor deep learning vereenvoudigt
  * [PyTorch](http://pytorch.org/): een Python-bibliotheek met ondersteuning voor dynamische netwerken op hoog niveau
  * [TensorFlow](https://www.tensorflow.org/): een open-source-bibliotheek voor machine-intelligentie van Google
  * [Theano](http://deeplearning.net/software/theano/): een Python-bibliotheek voor het definiëren en efficiënt evalueren van wiskundige expressies met betrekking tot multi-dimensionale matrixen optimaliseren
  * [Torch](http://torch.ch/): een wetenschappelijke computing framework met brede ondersteuning voor machine learning-algoritmen
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
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine learning-systeem, zoals de online, hash, allreduce, kortingen, learning2search, actief, ondersteuning en interactieve training
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpprogramma voor snelle en nauwkeurige boosted structuur-implementatie
  * [Rattle](https://togaware.com/rattle/): een grafisch hulpprogramma die aan de slag met gegevensanalyse en machine learning in R eenvoudig maakt
  * [LightGBM](https://github.com/Microsoft/LightGBM): een snelle, gedistribueerde, hoogwaardige verloop framework versterking
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

1. Navigeer naar de virtuele machine weergeven op de [Azure-portal](https://portal.azure.com/#create/microsoft-dsvm.linux-data-science-vm-ubuntulinuxdsvmubuntu). U mogelijk gevraagd om aan te melden bij uw Azure-account als u bent niet al aangemeld. 
1. Klik op **maken** (aan de onderkant) om de wizard.![ Configureer-data-science-vm](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
1. De volgende secties vindt u de invoer voor elk van de stappen in de wizard (geïnventariseerd aan de rechterkant van de afbeelding hierboven) gebruikt voor het maken van de Microsoft Data Science Virtual Machine. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:
   
   a. **Grondbeginselen van**:
   
   * **Naam**: naam van uw data science-server die u maakt.
   * **VM-schijftype**: kies **Premium SSD** als u liever een SSD (solid-state drive). Kies anders **Standard HDD**. 
   * **Gebruikersnaam**: eerste aanmelding bij het account ID.
   * **Wachtwoord**: eerste wachtwoord (u kunt openbare SSH-sleutel gebruiken in plaats van wachtwoord).
   * **Abonnement**: als u meer dan één abonnement hebt, selecteert u het een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht. U hebt privileges voor het maken van resources nodig voor dit abonnement.
   * **Resourcegroep**: U kunt een nieuwe groep maken of een bestaande groep gebruikt.
   * **Locatie**: Selecteer het datacenter die het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens, of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk.
   
   b. **Grootte**:
   
   * Selecteer een van de servertypen die voldoet aan de vereisten van het functionele en kostenbeperkingen. Selecteer een NC of de ND-klasse VM voor VM-exemplaren op basis van GPU. De [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) pagina geeft een lijst van de regio's met GPU's.
   
   c. **Instellingen voor**:
   
   * In de meeste gevallen kunt u alleen de standaardwaarden gebruiken. U moet overwegen niet-standaard waarden, de muisaanwijzer over de informatieve koppeling voor meer informatie over de specifieke velden.
   
   d. **Samenvatting**:
   
   * Controleer of alle informatie die u hebt ingevoerd juist is. Een koppeling volgen met de gebruiksvoorwaarden. De virtuele machine heeft geen eventuele extra kosten buiten de rekenkracht voor de servergrootte van de die u hebt gekozen in de **grootte** stap. Voor het starten van het inrichten, klikt u op **maken**. 
   
De inrichting duurt ongeveer 5 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Toegang tot de virtuele Machine voor Datatechnologie voor Linux

U kunt toegang tot de Ubuntu-DSVM drie methoden gebruiken:
1. SSH voor terminal-sessies
1. X2Go voor grafische sessies
1. JupyterHub en Jjupyterlab voor Jupyter-notebooks

### <a name="ssh"></a>SSH

Nadat de virtuele machine is gemaakt, kunt u zich aanmelden toe met behulp van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de tekst shell-interface. In Windows kunt u een SSH-clienthulpprogramma zoals [Putty](http://www.putty.org) downloaden. Als u liever een grafische desktop (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go beter dan X11 doorsturen tests uitgevoerd. Het is raadzaam om met behulp van de client X2Go voor een grafische interface voor het bureaublad.
> 
> 

### <a name="x2go"></a>X2Go
De Linux-VM is al ingericht met X2Go-server en gereed voor clientverbindingen accepteren. Voor verbinding met het bureaublad van de Linux-VM-grafische, voer de volgende procedure op de client:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
1. De X2Go-client wordt uitgevoerd, en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: de hostnaam of IP-adres van uw Linux Data Science VM.
     * **Aanmelding**: de naam van de gebruiker op de Linux-VM.
     * **SSH-poort**: 22, de standaardwaarde accepteren.
     * **Sessietype**: Wijzig de waarde in XFCE. De Linux-VM ondersteunt momenteel alleen XFCE desktop.
   * **Tabblad Media**: U kunt uitschakelen geluid ondersteuning en client afdrukken als u niet nodig hebt om ze te gebruiken.
   * **Gedeelde mappen**: als u mappen van uw clientmachines die gekoppeld is op de Linux-VM wilt, voegt u de mappen op de client computer die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of de XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u snelkoppelingen voor toepassingen in het menu en pictogrammen op het bureaublad voor veel van de hulpprogramma's.

### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub en Jjupyterlab

De Ubuntu-DSVM wordt uitgevoerd [JupyterHub](https://github.com/jupyterhub/jupyterhub), een Jupyter-server van meerdere gebruikers. Als u wilt verbinden, blader naar https://your-vm-ip:8000 op uw laptop of het bureaublad, voer de gebruikersnaam en het wachtwoord die u hebt gebruikt voor het maken van de virtuele machine en aanmelden. Veel voorbeeldnotitieblokken zijn beschikbaar voor u om te bladeren en uit te proberen.

Jjupyterlab, is de volgende generatie van Jupyter notebooks en JupyterHub, ook beschikbaar. Meld u aan bij JupyterHub voor toegang, en blader naar de URL https://your-vm-ip:8000/user/your-username/lab. U kunt Jjupyterlab instellen als de standaardserver voor de notebook door deze regel toe te voegen aan /etc/jupyterhub/jupyterhub_config.py:

    c.Spawner.default_url = '/lab'

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Hulpprogramma's zijn geïnstalleerd op de Data Science Virtual Machine voor Linux
### <a name="deep-learning-libraries"></a>Deep Learning-bibliotheken

#### <a name="cntk"></a>CNTK
De Microsoft Cognitive Toolkit is een open-source, deep learning toolkit. Python-bindingen zijn beschikbaar in de hoofdmap en py35 Conda-omgevingen. Het bevat ook een opdrachtregelprogramma (cntk) die zich al in het pad.

Voorbeeld van Python-laptops zijn beschikbaar in JupyterHub. Een eenvoudig voorbeeld op de opdrachtregel uitgevoerd, voert u de volgende opdrachten in de shell:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Zie voor meer informatie de sectie CNTK van [GitHub](https://github.com/Microsoft/CNTK), en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe
Caffe is een deep learning-structuur van de visie van Berkeley en Learning Center. Het is beschikbaar in /opt/caffe. Voorbeelden vindt u in /opt/caffe/examples.

#### <a name="caffe2"></a>Caffe2
Caffe2 is een deep learning-frameworks uit Facebook die is gebouwd op Caffe. Het is beschikbaar in Python 2.7 in de omgeving Conda-hoofdmap. Om deze te activeren, voert u de volgende vanuit de shell:

    source /anaconda/bin/activate root

Sommige laptops voorbeeld zijn beschikbaar in JupyterHub.

#### <a name="h2o"></a>H2O
H2O is een snelle, in het geheugen, gedistribueerde machine learning en predictive analytics-platform. Een Python-pakket is geïnstalleerd in de basis- en py35 Anaconda-omgevingen. Een R-pakket is ook geïnstalleerd. Uitvoeren voor het starten van H2O vanaf de opdrachtregel, `java -jar /dsvm/tools/h2o/current/h2o.jar`; er zijn verschillende [opdrachtregelopties](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) die u mogelijk wilt configureren. De gebruikersinterface van Flow Web kan worden geopend door te bladeren naar http://localhost:54321 aan de slag. Voorbeeldnotitieblokken zijn ook beschikbaar in JupyterHub.

#### <a name="keras"></a>Keras
Keras is een op hoog niveau neural network API in Python waarmee kan worden uitgevoerd op de hoogte van TensorFlow of Theano is. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen. 

#### <a name="mxnet"></a>MXNet
MXNet is een deep learning-structuur die is ontworpen voor zowel efficiëntie en flexibiliteit. R en Python-bindingen op de DSVM heeft. Voorbeeldnotitieblokken zijn opgenomen in JupyterHub en voorbeeldcode is beschikbaar in /dsvm/samples/mxnet.

#### <a name="nvidia-digits"></a>NVIDIA CIJFERS
De NVIDIA Deep Learning-GPU-Training, cijfers, ook wel is een systeem om algemene deep learning-taken zoals het beheren van gegevens, het ontwerpen en training neurale netwerken op GPU-systemen en bewaken van de prestaties in realtime met geavanceerde visualisatie te vereenvoudigen. 

CIJFERS is beschikbaar als een service met de naam van de cijfers. Start de service en blader naar http://localhost:5000 aan de slag.

CIJFERS is ook geïnstalleerd als een Python-module in de omgeving Conda-hoofdmap.

#### <a name="tensorflow"></a>TensorFlow
TensorFlow is deep learning-bibliotheek van Google. Het is een open source-software-bibliotheek voor numerieke berekeningen met behulp van gegevensstroomdiagrammen. TensorFlow is beschikbaar in de py35 Python-omgeving, en sommige voorbeeldnotitieblokken zijn opgenomen in JupyterHub.

#### <a name="theano"></a>Theano
Theano is een Python-bibliotheek voor efficiënte numerieke berekeningen. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen. 

#### <a name="torch"></a>Torch
Torch is een wetenschappelijke computing structuur met brede ondersteuning voor machine learning-algoritmen. Het is beschikbaar in /dsvm/tools/torch en de interactieve sessie th en luarocks Pakketbeheer zijn beschikbaar vanaf de opdrachtregel. Voorbeelden zijn beschikbaar in /dsvm/samples/torch.

PyTorch is ook beschikbaar in de hoofdmap Anaconda-omgeving. Voorbeelden zijn in /dsvm/samples/pytorch.

### <a name="microsoft-r-server"></a>Microsoft R Server
R is een van de meest populaire talen voor data-analyse en machine learning. Als u R gebruiken voor uw analyses wilt, heeft de virtuele machine Microsoft R Server (MEVR) met de Microsoft R Open (BHT) en Math Kernel-bibliotheek (MKL). De MKL optimaliseert wiskundige bewerkingen gebruikelijk in analytische algoritmen. BHT is 100 procent is compatibel met CRAN R en een van de R-bibliotheken die is gepubliceerd in CRAN kan worden geïnstalleerd op de BHT. MEVR biedt u schalen en uitoefening van R-modellen in webservices. U kunt uw R-programma's in een van de standaard-editors, zoals RStudio, vi of Emacs bewerken. Als u liever met behulp van de editor Emacs, is deze vooraf worden geïnstalleerd. Het pakket Emacs ERDRUKKEN (Emacs spreekt statistieken) vereenvoudigt het gebruik met R-bestanden in de editor Emacs.

Console te starten, R, typt u **R** in de shell. Hiermee gaat u naar een interactieve omgeving. Voor het ontwikkelen van uw R-programma, u doorgaans een editor zoals Emacs of vi gebruiken en voer de scripts in R. Met RStudio hebt u een volledige grafische IDE-omgeving voor het ontwikkelen van uw R-programma.

Er is ook een R-script voor installatie de [Top 20 R-pakketten](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) als u wilt. Met dit script kan worden uitgevoerd nadat u zich in de interactieve interface van R, die kan worden ingevoerd (zoals vermeld) door te typen **R** in de shell.  

### <a name="python"></a>Python
Anaconda Python is geïnstalleerd met Python 2.7 en 3,5-omgevingen. De 2,7 omgeving heet _hoofdmap_, en de 3,5 omgeving heet _py35_. Deze verdeling bevat de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten. 

De omgeving py35 is de standaardinstelling. Aan de hoofdmap (2.7)-omgeving te activeren:

    source activate root

De omgeving py35 opnieuw activeren:

    source activate py35

Voor het aanroepen van een Python-interactieve sessie, typt u gewoon **python** in de shell. 

Installeren van extra Python-bibliotheken met behulp van ```conda``` of ````pip```` . Voor pip, Hiermee activeert u de juiste omgeving eerst als u niet dat de standaardwaarde wilt:

    source activate root
    pip install <package>

Of geef het volledige pad naar het pip:

    /anaconda/bin/pip install <package>
    
Voor conda, moet u altijd de omgevingsnaam van de opgeven (_py35_ of _hoofdmap_):

    conda install <package> -n py35

Als u zich op een grafische interface of X11 set up doorsturen hebt, kunt u typen **pycharm** de PyCharm Python IDE starten. U kunt de standaard-teksteditors. Bovendien kunt u Spyder, een Python IDE die wordt geleverd met Anaconda Python-distributies. Spyder moet een grafische bureaublad of X11 doorsturen. Een snelkoppeling naar Spyder is opgegeven in de grafische desktop.s

### <a name="jupyter-notebook"></a>Jupyter Notebook
De distributie Anaconda wordt geleverd met een Jupyter-notebook, een omgeving voor het delen van code en -analyse. De Jupyter-notebook toegankelijk is via JupyterHub. U aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord.

De Jupyter-notebook-server is vooraf geconfigureerd met Python 2, 3 van Python en R-kernels. Er is een pictogram op het bureaublad met de naam 'Jupyter-Notebook' om te starten van de browser voor toegang tot de notebookserver. Als u van de virtuele machine via SSH of X2Go-client gebruikmaakt, kunt u ook bezoeken [ https://localhost:8000/ ](https://localhost:8000/) voor toegang tot de Jupyter-notebook-server.

> [!NOTE]
> Doorgaan als u certificaatwaarschuwingen.
> 
> 

U kunt de Jupyter-notebook-server openen vanaf elke host. Typ *https://\<VM DNS-naam of IP-adres\>: 8000 /*

> [!NOTE]
> Poort 8000 wordt standaard in de firewall geopend wanneer de virtuele machine is ingericht.
> 
> 

We hebben verpakt voorbeeldnotitieblokken--één in Python en één in R. U kunt de koppeling naar de voorbeelden op de startpagina van de notebook ziet nadat u voor de Jupyter-notebook verifiëren met uw lokale Linux-gebruikersnaam en wachtwoord. U kunt een nieuwe notebook maken door het selecteren van **nieuw**, en vervolgens de juiste taal-kernel. Als er geen de **nieuw** klikken, klik op de **Jupyter** pictogram links bovenin om te gaan naar de startpagina van de notebookserver.

### <a name="apache-spark-standalone"></a>Apache Spark-zelfstandig 
Een zelfstandig exemplaar van Apache Spark is vooraf geïnstalleerd op de Linux-DSVM om u te helpen bij het ontwikkelen van toepassingen voor Spark lokaal eerst voor testen en implementeren op grote clusters. U kunt de PySpark-programma's uitvoeren via de Jupyter-kernel. Wanneer u een Jupyter openen, klikt u op de **nieuw** knop en u ziet een lijst met beschikbare kernels. De 'Spark – Python' is de PySpark-kernel waarmee u Spark toepassingen bouwen met Python-taal. U kunt ook een Python IDE zoals PyCharm of Spyder gebruiken om te bouwen u Spark-programma. Aangezien dit is een zelfstandig exemplaar, de Spark-stack binnen de aanroepende clientprogramma wordt uitgevoerd. Hierdoor kunt u sneller en gemakkelijker problemen in vergelijking met het ontwikkelen van een Spark-cluster. 

Een voorbeeld PySpark-notebook wordt in Jupyter die u in de map 'SparkML' onder de basismap van Jupyter ($HOME/notitieblokken/SparkML/pySpark vinden kunt) verstrekt. 

Als u in R voor Spark programmeert, kunt u Microsoft R Server, SparkR of sparklyr gebruiken. 

Voordat u uitvoert in Spark-context in Microsoft R Server, moet u een setup-stap om in te schakelen van een lokale één knooppunt HDFS Hadoop en Yarn-instantie eenmaal te doen. Hadoop-services worden standaard geïnstalleerd maar uitgeschakeld op de DSVM. Als u wilt inschakelen, moet u de volgende opdrachten als root de eerste keer uitvoeren:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

U kunt de Hadoop stoppen gerelateerde services wanneer u ze niet nodig door uit te voeren ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` een voorbeeld laat zien hoe u kunt ontwikkelen en testen van MEVR in externe Spark-context (dit is de Spark-instantie van de zelfstandige versie van de DSVM) is opgegeven en beschikbaar in de `/dsvm/samples/MRS` de map. 

### <a name="ides-and-editors"></a>IDE's en editors
U hebt een ruime keuze aan verschillende code-editors. Dit omvat vi/VIM, Emacs, PyCharm, RStudio en IntelliJ. IntelliJ, RStudio PyCharm grafische editors zijn en moet u zijn aangemeld met een grafische bureaublad om ze te gebruiken. Deze editors hebben bureaublad- en toepassingsservices snelkoppelingen in het menu te starten.

**VIM** en **Emacs** editors op basis van tekst zijn. Op Emacs, hebben we een add-on pakket met de naam Emacs spreekt statistieken (eldingen ONDERDRUKKEN) die werken met R gemakkelijker in de editor Emacs geïnstalleerd. Meer informatie kunt vinden op [ERDRUKKEN](http://ess.r-project.org/).

**LaTex** wordt geïnstalleerd via de texlive-package samen met een invoegtoepassing Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) pakket vereenvoudigt het ontwerpen van uw documenten LaTex binnen Emacs.  

### <a name="databases"></a>Databases

#### <a name="graphical-sql-client"></a>Grafische SQL-client
**SQuirrel SQL**, een grafische SQL-client is opgegeven voor het verbinding maken met verschillende databases (zoals Microsoft SQL Server- en MySQL) en SQL-query's uitvoeren. U kunt dit uitvoeren vanaf een grafische bureaublad-sessiehost (met behulp van de client X2Go, bijvoorbeeld). Om aan te roepen SQuirrel SQL, kunt u starten vanaf het pictogram op het bureaublad of Voer de volgende opdracht op de shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Voordat u het eerste gebruik, stuurprogramma's en database-aliassen hebt ingesteld. Het JDBC-stuurprogramma's bevinden zich op:

*/usr/share/Java/jdbcdrivers*

Zie voor meer informatie, [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Opdrachtregelprogramma's voor toegang tot Microsoft SQL Server
Het pakket ODBC-stuurprogramma voor SQL Server wordt geleverd met twee opdrachtregelprogramma's:

**BCP**: het hulpprogramma bcp bulksgewijs worden gegevens gekopieerd tussen een exemplaar van Microsoft SQL Server en een bestand in een indeling met door gebruiker opgegeven. Het hulpprogramma bcp kan worden gebruikt om te importeren van grote aantallen nieuwe rijen in SQL Server-tabellen, of gegevens uit tabellen exporteren naar gegevensbestanden. Om gegevens te importeren in een tabel, moet u een bestandsindeling die is gemaakt voor deze tabel gebruiken of de structuur van de tabel en de typen gegevens die geldig voor de kolommen zijn begrijpen.

Zie voor meer informatie, [verbinding te maken met bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**Sqlcmd**: U kunt Transact-SQL-instructies met het hulpprogramma sqlcmd, evenals de systeemprocedures invoeren en scriptbestanden bij de opdrachtprompt. Dit hulpprogramma maakt gebruik van ODBC om uit te voeren van Transact-SQL-batches.

Zie voor meer informatie, [verbinding te maken met sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Er zijn enkele verschillen in dit hulpprogramma tussen Linux en Windows-platforms. Zie de documentatie voor meer informatie.
> 
> 

#### <a name="database-access-libraries"></a>Database toegang bibliotheken
Er zijn bibliotheken beschikbaar is in R en Python op access-databases.

* In R, de **RODBC** pakket of **dplyr** pakket kunt u query's uitvoeren of SQL-instructies uitvoeren op de databaseserver.
* In Python, de **pyodbc** -bibliotheek biedt toegang tot de database met ODBC-als de onderliggende laag.  

### <a name="azure-tools"></a>Azure-hulpprogramma 's
De volgende Azure-hulpprogramma's zijn geïnstalleerd op de virtuele machine:

* **Azure-opdrachtregelinterface**: de Azure CLI kunt u maken en beheren van Azure-resources via de shell-opdrachten. Voor het aanroepen van de Azure-hulpprogramma's, typt u gewoon **azure help**. Zie voor meer informatie de [documentatiepagina van Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Storage Explorer**: Microsoft Azure Storage Explorer is een grafische hulpprogramma dat wordt gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw Azure storage-account, en voor het uploaden en downloaden van gegevens naar en van Azure-blobs. U kunt Storage Explorer openen via het pictogram van de snelkoppeling op het bureaublad. U kunt het aanroepen van een shell-prompt door te typen **StorageExplorer**. U moet zijn aangemeld vanaf een client X2Go of X11 doorsturen set up hebben.
* **Azure-bibliotheken**: Hieronder volgen enkele van de vooraf geïnstalleerde-bibliotheken.
  
  * **Python**: met betrekking tot de Azure-bibliotheken in Python die zijn geïnstalleerd zijn **azure**, **azureml**, **pydocumentdb**, en **pyodbc**. Met de eerste drie bibliotheken, kunt u toegang tot Azure storage-services, Azure Machine Learning en Azure Cosmos DB (een NoSQL-database in Azure). De vierde servicebibliotheek pyodbc (samen met het Microsoft ODBC-stuurprogramma voor SQL Server), kunt u toegang tot SQL Server, Azure SQL Database en Azure SQL Data Warehouse met Python met behulp van een ODBC-interface. Voer **pip lijst** om te zien van de vermelde bibliotheken. Zorg dat u deze opdracht wordt uitgevoerd in de Python 2.7 en 3,5 omgevingen.
  * **R**: met betrekking tot de Azure-bibliotheken in R die zijn geïnstalleerd zijn **AzureML** en **RODBC**.
  * **Java**: de lijst met Azure Java-bibliotheken kan worden gevonden in de map **/dsvm/sdk/AzureSDKJava** op de virtuele machine. De sleutel-bibliotheken zijn Azure-opslag en beheer van API's, Azure Cosmos DB en JDBC-stuurprogramma's voor SQL Server.  

U hebt toegang tot de [Azure-portal](https://portal.azure.com) van de vooraf geïnstalleerde Firefox-browser. In de Azure-portal, kunt u maken, beheren en controleren van Azure-resources.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde cloudservice waarmee u kunt bouwen, implementeren en delen van predictive analytics-oplossingen. U bouwen uw experimenten en modellen in Azure Machine Learning Studio. Deze kan worden geopend via een webbrowser op de virtuele machine voor datatechnologie recentst [Microsoft Azure Machine Learning](https://studio.azureml.net).

Nadat u zich bij Azure Machine Learning Studio aanmelden, hebt u toegang tot een experimenten-canvas waar u een logische stroom voor de machine learning-algoritmen kunt bouwen. U ook toegang hebben tot een Jupyter-notebook die worden gehost op Azure Machine Learning en werkt naadloos samen met de experimenten in Machine Learning Studio. Operationeel maken van de machine learning-modellen die u hebt gemaakt door ze in een webservice-interface. Hierdoor kunnen clients die zijn geschreven in elke taal om aan te roepen voorspellingen op basis van de machine learning-modellen. Zie voor meer informatie de [documentatie voor Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

U kunt ook uw modellen in R of Python op de virtuele machine maken en vervolgens implementeren in productie met Azure Machine Learning. Hebben we bibliotheken geïnstalleerd in R (**AzureML**) en Python (**azureml**) om deze functionaliteit te schakelen.

Zie voor meer informatie over het implementeren van modellen in R en Python in Azure Machine Learning [tien dingen die u in de Data science Virtual Machine doen kunt](vm-do-ten-things.md) (met name de sectie ' modellen met behulp van R of Python maken en deze operationeel te maken met behulp van Azure Machine Learning').

> [!NOTE]
> Deze instructies zijn geschreven voor de Windows-versie van de Data Science VM. Maar de verstrekte informatie bevat over het implementeren van modellen op Azure Machine Learning is van toepassing op de Linux-VM.
> 
> 

### <a name="machine-learning-tools"></a>Hulpprogramma's voor machine learning
De virtuele machine wordt geleverd met enkele machine learning-hulpprogramma's en -algoritmen die zijn vooraf gecompileerde en vooraf lokaal zijn geïnstalleerd. Deze omvatten:

* **Vowpal Wabbit**: een leeralgoritme snel online.
* **xgboost**: een hulpprogramma waarmee u geoptimaliseerde, boosted algoritmen.
* **Rattle**: Er is geen R gebaseerd grafische hulpprogramma voor eenvoudig gegevens verkennen en modelleren.
* **Python**: Anaconda Python wordt geleverd met machine learning-algoritmen met bibliotheken, zoals Scikit-informatie. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **LightGBM**: een snelle, gedistribueerde, hoogwaardige verloop versterking framework op basis van algoritmen voor een beslissingsstructuur.
* **R**: Er is een uitgebreide bibliotheek met machine learning-functies beschikbaar voor R. Sommige van de bibliotheken die vooraf geïnstalleerd zijn zijn, er lm glm, randomForest rpart. Andere bibliotheken kunnen worden geïnstalleerd door uit te voeren:
  
        install.packages(<lib name>)

Hier is aanvullende informatie over de eerste drie machine learning-hulpprogramma's in de lijst.

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit is een machine learning-systeem dat gebruikmaakt van technieken zoals online, hash, allreduce, kortingen, learning2search, actief, en interactieve training.

Als u wilt het hulpprogramma uitvoeren op een eenvoudige voorbeeld, het volgende doen:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Er zijn andere, groter demo's in die map. Zie voor meer informatie over VW [in deze sectie van GitHub](https://github.com/JohnLangford/vowpal_wabbit), en de [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dit is een bibliotheek die is ontworpen en geoptimaliseerd voor boosted (structuur)-algoritmen. Het doel van deze bibliotheek is om de grenzen van de berekening van machines tot het uiterste dat nodig is voor grootschalige structuur versterking die schaalbare, draagbare en nauwkeurige.

Deze is geleverd als een opdrachtregel weergegeven, evenals een R-bibliotheek.

U kunt een interactieve R-sessie starten voor het gebruik van deze bibliotheek in R (door te typen **R** in de shell), en laad de bibliotheek.

Hier volgt een eenvoudig voorbeeld die u in de R-prompt uitvoeren kunt:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Als u wilt uitvoeren vanaf de opdrachtregel xgboost, vindt hier u de opdrachten uit te voeren in de shell:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Een bestand .model is geschreven naar de map die is opgegeven. Informatie over deze demo-voorbeeld vindt [op GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Zie voor meer informatie over xgboost, de [xgboost documentatiepagina](https://xgboost.readthedocs.org/en/latest/), en de bijbehorende [GitHub-opslagplaats](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rammelaar
Rattle (de **R** **A**analytische **T**lpmiddel **T**o **L**verdienen **E** asily) maakt gebruik van GUI-gebaseerde gegevens verkennen en modelleren. Deze geeft statistische en visuele samenvattingen van gegevens, transformaties die gemakkelijk kan worden gemodelleerd, zonder supervisie en onder supervisie modellen uit de gegevens is gebaseerd, geeft de prestaties van modellen grafische vorm en scores nieuwe gegevens worden ingesteld. Er wordt ook gegenereerd R-code voor het repliceren van de bewerkingen in de gebruikersinterface die kunnen worden rechtstreeks in R uitvoeren of als uitgangspunt gebruikt voor verdere analyse.

Als u wilt uitvoeren Rammelaar, moet u zich in een grafische aanmelden-bureaubladsessie. Typ op de terminal ```R``` in te voeren van de R-omgeving. Voer de volgende opdrachten bij de R-prompt:

    library(rattle)
    rattle()

Nu een grafische interface wordt geopend met een set tabbladen. Hier volgen de stappen snel aan de slag in Rammelaar die nodig zijn voor gebruik van een verzameling voorbeeldgegevens weer en een model te ontwikkelen. U wordt gevraagd in enkele van de volgende stappen uit om automatisch te installeren en sommige vereiste R-pakketten die nog niet op het systeem worden geladen.

> [!NOTE]
> Als u geen toegang tot het pakket installeren in de systeemmap (de standaardinstelling), ziet u mogelijk een prompt in uw R-console-venster om pakketten te installeren op uw persoonlijke bibliotheek. Antwoord *y* als u deze stappen ziet.
> 
> 

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

