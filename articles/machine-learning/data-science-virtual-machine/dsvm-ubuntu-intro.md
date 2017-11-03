---
title: Inrichten van een Linux (Ubuntu) gegevens wetenschappelijke virtuele Machine in Azure | Microsoft Docs
description: Configureer en een gegevens wetenschappelijke virtuele Machine voor Linux (Ubuntu) maken op Azure doen analytics en machine learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev
ms.openlocfilehash: 557eab702175b1741cdcde62045947f2c031fbcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-the-data-science-virtual-machine-for-linux-ubuntu"></a>De gegevens wetenschappelijke virtuele Machine inrichten voor Linux (Ubuntu)

De gegevens wetenschappelijke virtuele Machine voor Linux is de installatiekopie van een virtuele Ubuntu-machine dat kunt u eenvoudig aan de slag met grondige learning in Azure. Hulpprogramma's voor grondige learning omvatten:

  * [Caffe](http://caffe.berkeleyvision.org/): een grondige learning framework gebouwd voor snelheid, expressivity en modulariteit
  * [Caffe2](https://github.com/caffe2/caffe2): een cross-platform-versie van Caffe
  * [Microsoft cognitieve Toolkit](https://github.com/Microsoft/CNTK): een diepe learning toolkit software van Microsoft Research
  * [H2O](https://www.h2o.ai/): een open source big data-platform en de grafische gebruikersinterface
  * [Keras](https://keras.io/): een op hoog niveau neurale netwerk API in Python voor Theano en TensorFlow
  * [MXNet](http://mxnet.io/): een flexibele en efficiënte grondige learning-bibliotheek veel taal bindingen
  * [NVIDIA cijfers](https://developer.nvidia.com/digits): een grafische systeem dat algemene beheertaken voor grondige learning vereenvoudigt
  * [TensorFlow](https://www.tensorflow.org/): een open source-bibliotheek voor machine intelligence van Google
  * [Theano](http://deeplearning.net/software/theano/): een Python-bibliotheek voor het definiëren en efficiënt evalueren wiskundige expressies met betrekking tot multi-dimensionale matrixen optimaliseren
  * [Torch](http://torch.ch/): een wetenschappelijke computing framework met brede ondersteuning voor machine learning-algoritmen
  * CUDA cuDNN en het stuurprogramma NVIDIA
  * Veel voorbeeld Jupyter-notebooks

Alle bibliotheken zijn de GPU-versies, hoewel ze ook op de CPU uitvoeren.

De gegevens wetenschappelijke virtuele Machine voor Linux bevat ook populaire hulpprogramma's voor gegevens wetenschap en ontwikkeling activiteiten, waaronder:

* Microsoft R Server Developer Edition met Microsoft R openen
* Anaconda Python-distributie (versies 2.7 en 3.5), met inbegrip van populaire gegevens analysebibliotheken
* JuliaPro - een curated verdeling van Julia taal met populaire wetenschappelijke en gegevens analytics-bibliotheken
* Standalone Spark-exemplaar en één knooppunt Hadoop (HDFS, Yarn)
* JupyterHub - een voor meerdere gebruikers Jupyter-notebook server R, Python, PySpark, Julia kernels ondersteunen
* Azure Opslagverkenner
* Azure-opdrachtregelinterface (CLI) voor het beheren van Azure-resources
* Machine learning-hulpprogramma 's
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snel machine learning-technieken zoals online, hash, allreduce, kortingen, learning2search, actief, ondersteunende system en interactieve leren
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpprogramma dat snelle en nauwkeurige gestimuleerd structuur-implementatie
  * [Rattle](http://rattle.togaware.com/): een grafisch hulpprogramma waarmee aan de slag met data analytics en machine learning-R-eenvoudig
  * [LightGBM](https://github.com/Microsoft/LightGBM): een snelle, gedistribueerde, hoge prestaties verloop versterking van framework
* Azure SDK in Java, Python, node.js, Ruby, PHP
* Bibliotheken in R- en Python voor gebruik in Azure Machine Learning en andere Azure-services
* Ontwikkelingsprogramma's en editors (RStudio, PyCharm, IntelliJ, Emacs, vim)


Tijdens het doorzoeken van wetenschappelijke gegevens omvat sequentieel op een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
2. Maken en testen van modellen
3. De modellen voor verbruik in intelligente toepassingen implementeren

Verschillende hulpprogramma's gegevenswetenschappers gebruiken om deze taken te voltooien. Behoorlijk tijd in beslag nemen om de juiste versies van de software en vervolgens om te downloaden, compileren kan en installeert deze versies.

De gegevens wetenschappelijke virtuele Machine voor Linux kunnen deze last aanzienlijk verminderen. Gebruik dit voor uw project analytics snel. Hiermee kunt u voor taken in verschillende talen, waaronder R, Python, SQL, Java en C++. De Azure SDK opgenomen in de virtuele machine kunt u uw toepassingen bouwen met behulp van verschillende services op Linux voor het Microsoft cloud-platform. Bovendien hebt u toegang tot de andere talen zoals Ruby, Perl, PHP en node.js die ook vooraf zijn geïnstalleerd.

Er zijn geen kosten voor software voor deze gegevens wetenschappelijke VM-installatiekopie. U betaalt alleen Azure hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten vindt u op de [VM aanbiedingspagina op Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de gegevens wetenschappelijke virtuele Machine
Een [CentOS](linux-dsvm-intro.md) installatiekopie is ook beschikbaar met veel van dezelfde hulpmiddelen als de Ubuntu-afbeelding. Een [Windows](provision-vm.md) installatiekopie is ook beschikbaar.

## <a name="prerequisites"></a>Vereisten
Voordat u een virtuele Machine van de gegevens wetenschappelijke voor Linux maken kunt, moet u een Azure-abonnement hebben. Zie voor een [gratis proefversie van Azure ophalen](https://azure.microsoft.com/free/).

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Uw gegevens wetenschappelijke virtuele Machine maken voor Linux
Hier volgen de stappen voor het maken van een exemplaar van de gegevens wetenschappelijke virtuele Machine voor Linux:

1. Navigeer naar de virtuele machine weergeven op de [Azure-portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu).
2. Klik op **maken** (aan de onderkant) online zetten van de wizard.![ Configureer-gegevens-wetenschappelijke-vm](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
3. De volgende secties bevatten de invoer voor elk van de stappen in de wizard (opgesomd aan de rechterkant van de voorgaande afbeelding) gebruikt voor het maken van de Microsoft Data wetenschappelijke virtuele Machine. Hier volgen de invoer voor het configureren van deze stappen:
   
   a. **Basisprincipes**:
   
   * **Naam**: naam van uw gegevens wetenschap-server die u maakt.
   * **Gebruikersnaam**: eerste account aanmelden ID.
   * **Wachtwoord**: eerste wachtwoord (u kunt openbare SSH-sleutel kunt gebruiken in plaats van wachtwoord).
   * **Abonnement**: als u meer dan één abonnement hebt, selecteert u een op waarop de machine is gemaakt en kosten in rekening gebracht. U moet bevoegdheden van de resource maken voor dit abonnement hebben.
   * **Resourcegroep**: U kunt een nieuwe maken of een bestaande groep.
   * **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is doorgaans het datacenter die de meeste van uw gegevens en of de dichtstbijzijnde fysieke voor snelste toegang tot het netwerk is.
   
   b. **De grootte van**:
   
   * Selecteer een van de servertypen die voldoet aan uw functionele vereisten en kostenbeperkingen. Selecteer **Alles weergeven** om meer opties van VM-formaten te bekijken. Selecteer een VM NC-klasse voor training GPU.
   
   c. **Instellingen**:
   
   * **Schijftype**: kies **Premium** als u liever een SSD-station (SSD). Kies anders **standaard**. Virtuele machines GPU vereisen een standaard schijf.
   * **Storage-Account**: U kunt een nieuw Azure storage-account maken in uw abonnement of gebruik een bestaande op dezelfde locatie die is gekozen op de **basisbeginselen** stap van de wizard.
   * **Andere parameters**: In de meeste gevallen gebruikt u alleen de standaardwaarden. U moet overwegen niet-standaard waarden, houd de muisaanwijzer boven de informatief koppeling voor meer informatie over de specifieke velden.
   
   d. **Samenvatting**:
   
   * Controleer of alle informatie die u hebt ingevoerd juist is.
   
   e. **Kopen**:
   
   * Voor het starten van de inrichting, klikt u op **kopen**. Een koppeling is met de voorwaarden van de transactie opgegeven. De virtuele machine heeft geen eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt gekozen in de **grootte** stap.

De inrichting duren ongeveer 5 tot 10 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Toegang tot de gegevens wetenschappelijke virtuele Machine voor Linux
Nadat de virtuele machine is gemaakt, kunt u aanmelden bij het gebruik van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de interface van de shell tekst. Op Windows, kunt u een SSH clienthulpprogramma zoals downloaden [Putty](http://www.putty.org). Als u liever een grafische bureaublad (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go beter presteert dan X11 doorsturen in de testfase. U wordt aangeraden met behulp van de client X2Go voor een grafische interface voor het bureaublad.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go client
De Linux-VM is al ingericht met X2Go server en gereed voor clientverbindingen accepteren. Voor verbinding met de grafische Linux VM-bureaublad, voer de volgende procedure op de client:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. De X2Go-client wordt uitgevoerd en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: de hostnaam of IP-adres van uw virtuele Linux-gegevens van wetenschappelijke machine.
     * **Aanmelding**: de gebruikersnaam op de Linux-VM.
     * **SSH-poort**: 22, de standaardwaarde behoudt.
     * **Sessietype**: Wijzig de waarde in XFCE. De Linux-VM ondersteunt momenteel alleen XFCE bureaublad.
   * **Tabblad Media**: U kunt uitschakelen geluid ondersteuning en de client als u hoeft niet te gebruiken.
   * **Gedeelde mappen**: als u mappen van uw clientcomputers die zijn gekoppeld aan de Linux-VM wilt, voegt u de client-machine-mappen die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u toepassingen menu, snelkoppelingen en bureaubladpictogrammen voor veel van de hulpprogramma's.

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Hulpprogramma's zijn geïnstalleerd op de gegevens wetenschappelijke virtuele Machine voor Linux
### <a name="deep-learning-libraries"></a>Grondige Learning bibliotheken

#### <a name="cntk"></a>CNTK
De Microsoft cognitieve Toolkit is een open-source, grondige learning-toolkit. Python-bindingen zijn beschikbaar in de hoofdmap en py35 Conda-omgevingen. Er wordt ook een opdrachtregelprogramma (cntk) dat zich al in het pad.

Voorbeeld Python-laptops zijn beschikbaar in JupyterHub. Als u wilt een voorbeeld van een eenvoudige uitvoeren op de opdrachtregel, voer de volgende opdrachten in de shell:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Zie voor meer informatie de sectie CNTK [GitHub](https://github.com/Microsoft/CNTK), en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe
Caffe is een grondige learning framework van de visie Berkeley en Learning Center. Het is beschikbaar in /opt/caffe. Voorbeelden vindt u in /opt/caffe/examples.

#### <a name="caffe2"></a>Caffe2
Caffe2 ligt een framework grondige learning van Facebook die is gebouwd op Caffe. Het is beschikbaar in Python 2.7 in de hoofdmap Conda omgeving. Als u wilt activeren, moet u het volgende uitvoeren via de shell:

    source /anaconda/bin/activate root

Sommige laptops voorbeeld zijn beschikbaar in JupyterHub.

#### <a name="h2o"></a>H2O
H2O is een snelle, in het geheugen, gedistribueerde machine learning en predictive analytics-platform. Een Python-pakket is geïnstalleerd in de hoofd- en py35 Anaconda-omgevingen. Een R-pakket is ook geïnstalleerd. Uitvoeren als u wilt starten H2O vanaf de opdrachtregel, `java -jar /dsvm/tools/h2o/current/h2o.jar`; er zijn verschillende [opdrachtregelopties](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) die u mogelijk wilt configureren. De Webgebruikersinterface stromen zijn toegankelijk door te bladeren naar http://localhost:54321 aan de slag. Voorbeeldquery notitieblokken zijn ook beschikbaar in JupyterHub.

#### <a name="keras"></a>Keras
Keras is een op hoog niveau neurale netwerk API in Python die kunnen worden uitgevoerd op de bovenkant van TensorFlow of Theano. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen. 

#### <a name="mxnet"></a>MXNet
MXNet is een grondige learning framework dat is ontworpen voor zowel efficiëntie en flexibiliteit. Het heeft R- en Python-bindingen op de DSVM. Voorbeeldquery notitieblokken zijn opgenomen in JupyterHub en voorbeeldcode is beschikbaar in /dsvm/samples/mxnet.

#### <a name="nvidia-digits"></a>NVIDIA CIJFERS
De NVIDIA grondige Learning GPU Training, cijfers, ook wel is een systeem voor het vereenvoudigen van algemene grondige learning taken zoals het beheren van gegevens, ontwerpen en training neural networks op systemen met GPU en bewaken van de prestaties in realtime met geavanceerde visualisatie. 

CIJFERS is beschikbaar als een service met de naam van cijfers. Start de service en blader naar http://localhost:5000 aan de slag.

CIJFERS, is ook geïnstalleerd als een Python-module in de hoofdmap Conda omgeving.

#### <a name="tensorflow"></a>TensorFlow
TensorFlow is van Google grondige learning-bibliotheek. Er is een open-source software-bibliotheek voor numerieke berekeningen met gegevensstroom grafieken. TensorFlow is beschikbaar in de py35 Python-omgeving en sommige voorbeeldquery notitieblokken zijn opgenomen in JupyterHub.

#### <a name="theano"></a>Theano
Theano is een Python-bibliotheek voor efficiënte numerieke berekeningen. Het is beschikbaar in de hoofdmap en py35 Python-omgevingen. 

#### <a name="torch"></a>Torch
Torch is een wetenschappelijke computing framework met brede ondersteuning voor machine learning-algoritmen. Is beschikbaar in de /dsvm/tools/torch en de interactieve sessie th en luarocks Pakketbeheer zijn beschikbaar op de opdrachtregel. Voorbeelden zijn beschikbaar in /dsvm/samples/torch.

PyTorch is ook beschikbaar in de hoofdmap Anaconda-omgeving. Voorbeelden zijn in /dsvm/samples/pytorch.

### <a name="microsoft-r-server"></a>Microsoft R Server
R is een van de meest populaire talen voor analyse van gegevens en machine learning. Als u R gebruiken voor uw analyses wilt, heeft de virtuele machine Microsoft R Server (MEVR) met de Microsoft R openen (BHT) en Math Kernel-bibliotheek (MKL). De MKL optimaliseert rekenkundige bewerkingen gebruikelijk in analytische algoritmen. BHT is 100 procent compatibel is met CRAN R en een van de gepubliceerd in CRAN R-bibliotheken kunnen worden geïnstalleerd op de BHT. MEVR biedt u schaalbaarheid en uitoefening van R-modellen in de web-services. U kunt uw R-programma's in een van de standaard-editors, zoals RStudio, vi of Emacs bewerken. Als u liever met behulp van de editor Emacs, is het vooraf worden geïnstalleerd. Het pakket Emacs eldingen ONDERDRUKKEN (Emacs spreekt statistieken) vereenvoudigt het werken met R-bestanden in de editor Emacs.

Console te starten R, typt u **R** in de shell. Hiermee gaat u naar een interactieve omgeving. Voor het ontwikkelen van uw programma R u doorgaans een editor zoals Emacs of vi gebruiken en voer vervolgens de scripts in R. Met RStudio hebt u een volledige grafische IDE-omgeving voor het ontwikkelen van uw R-programma.

Er is ook een R-script voor installatie de [Top 20 R-pakketten](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) als u wilt. Dit script kan worden uitgevoerd nadat u zich in de interface voor interactieve op R, die kan worden ingevoerd (zoals vermeld) door te typen **R** in de shell.  

### <a name="python"></a>Python
Voor de ontwikkeling met behulp van Python, is distributie Anaconda Python 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de base Python samen met ongeveer 300 van de meest populaire math, engineering en gegevens analytics pakketten. U kunt de standaard teksteditors gebruiken. Bovendien kunt u Spyder, een Python IDE die wordt meegeleverd met Anaconda Python-distributies. Spyder moet een grafische bureaublad of X11 doorsturen. Een snelkoppeling naar Spyder is opgegeven in de grafische bureaublad.

Aangezien we Python 2.7 zowel 3.5 hebben, moet u de gewenste Python-versie (conda environment) u werken wilt op in de huidige sessie om precies te activeren. Het activeringsproces stelt de variabele PATH op de gewenste versie van Python.

Voer de volgende opdracht uit vanuit de shell voor het activeren van de Python 2.7 conda omgeving:

    source /anaconda/bin/activate root

Python 2.7 is geïnstalleerd op *anaconda/bin*.

Voer het volgende vanuit de shell voor het activeren van de Python 3.5 conda omgeving:

    source /anaconda/bin/activate py35


Python 3.5 is geïnstalleerd op */anaconda/envs/py35/bin*.

Als u wilt een interactieve sessie Python aanroepen, typt u gewoon **python** in de shell. Als u X11 set bent doorgestuurd of zich op een grafische interface, typt u **pycharm** starten van de PyCharm Python IDE.

Voor het installeren van extra Python-bibliotheken, moet u uitvoeren ```conda``` of ````pip```` opdracht onder sudo en volledig pad van de Python package manager (conda of pip) te installeren op de juiste Python-omgeving te bieden. Bijvoorbeeld:

    sudo /anaconda/bin/pip install <package> #for Python 2.7 environment
    sudo /anaconda/envs/py35/bin/pip install <package> # for Python 3.5 environment


### <a name="jupyter-notebook"></a>Jupyter-notebook
De Anaconda-verdeling wordt ook geleverd met een Jupyter-notebook een omgeving voor het delen van code en -analyse. De Jupyter-notebook toegankelijk is via JupyterHub. U kunt aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord.

De Jupyter-notebook-server is vooraf is geconfigureerd met Python 2, 3 Python en R kernels. Er is een pictogram op het bureaublad met de naam 'Jupyter-Notebook' starten van de browser voor toegang tot de notebook-server. Als u van de virtuele machine via SSH of X2Go-client gebruikmaakt, kunt u ook bezoeken [https://localhost:8000 /](https://localhost:8000/) voor toegang tot de Jupyter-notebook-server.

> [!NOTE]
> Doorgaan als u certificaatwaarschuwingen.
> 
> 

U kunt de Jupyter-notebook server openen vanaf een willekeurige host. Typ *https://\<VM DNS-naam of IP-adres\>: 8000 /*

> [!NOTE]
> Poort 8000 wordt standaard in de firewall geopend wanneer de virtuele machine is ingericht.
> 
> 

We hebben verpakt voorbeeldquery notitieblokken--één in Python en één in R. Nadat u naar de Jupyter-notebook verifiëren met behulp van uw lokale Linux-gebruikersnaam en wachtwoord, kunt u de koppeling naar de voorbeelden bekijken op de startpagina van de notebook. U kunt een nieuwe notebook maken door het selecteren van **nieuw**, en vervolgens de juiste taal-kernel. Als er geen de **nieuw** klikken, klik op de **Jupyter** pictogram linksboven op om te gaan naar de startpagina van de notebook-server.

### <a name="apache-spark-standalone"></a>Apache Spark zelfstandige 
Een zelfstandig exemplaar van Apache Spark is voorgeïnstalleerd op de Linux DSVM voor hulp bij het ontwikkelen van Spark scala-toepassingen lokaal eerst voordat u gaat testen en implementeren op grote clusters. U kunt de PySpark-programma's uitvoeren via de Jupyter-kernel. Wanneer u Jupyter openen, klikt u op de **nieuw** knop en u ziet een lijst met beschikbare kernels. De 'Spark--Python' is de PySpark-kernel waarmee u Spark toepassingen bouwen met Python-taal. U kunt ook een Python IDE zoals PyCharm of Spyder gebruiken om u Spark programma samen te stellen. Aangezien dit een zelfstandig exemplaar is, de Spark-stack binnen het aanroepende clientprogramma wordt uitgevoerd. Hierdoor kunt u sneller en eenvoudiger oplossen van problemen in vergelijking met het ontwikkelen van een Spark-cluster. 

Een voorbeeld PySpark-notebook is beschikbaar op Jupyter die u in de map 'SparkML' onder de basismap van Jupyter ($HOME/notitieblokken/SparkML/pySpark vinden kunt). 

Als u in R voor Spark programmeert, kunt u Microsoft R Server SparkR of sparklyr gebruiken. 

Voordat u in de context voor Spark in Microsoft R Server, moet u een eenmaal setup stap om in te schakelen lokale één knooppunt Hadoop HDFS en Yarn-exemplaar te doen. Hadoop-services zijn standaard geïnstalleerd maar uitgeschakeld op de DSVM. Als u wilt inschakelen, moet u de volgende opdrachten uitvoeren als basis voor het eerst:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

U kunt de Hadoop stoppen gerelateerde services wanneer u hoeft ze niet door het uitvoeren van ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` een voorbeeld laat zien hoe te ontwikkelen en testen van MEVR in externe Spark-context (dit is het zelfstandige Spark-exemplaar op de DSVM) is opgegeven en beschikbaar in de `/dsvm/samples/MRS` de map. 

### <a name="ides-and-editors"></a>IDE en editors
U hebt een keuze uit verschillende code-editors. Dit omvat vi/VIM, Emacs PyCharm, RStudio en IntelliJ. IntelliJ, RStudio PyCharm grafische editors zijn en moet u zijn aangemeld met een grafische bureaublad met ze te gebruiken. Deze editors hebben desktop- en snelkoppelingen in het menu te starten.

**VIM** en **Emacs** editors op basis van tekst zijn. Op Emacs, hebben we een invoegtoepassing pakket aangeroepen Emacs spreekt statistieken (eldingen ONDERDRUKKEN) die werken met R gemakkelijker binnen de editor Emacs geïnstalleerd. Meer informatie kunt vinden op [eldingen ONDERDRUKKEN](http://ess.r-project.org/).

**LaTex** wordt geïnstalleerd door het pakket texlive samen met een invoegtoepassing Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) , dit vereenvoudigt het ontwerpen van uw documenten LaTex binnen Emacs pakket.  

### <a name="databases"></a>Databases

#### <a name="graphical-sql-client"></a>Grafische SQL-client
**SQuirrel SQL**, een grafische SQL-client is opgegeven voor het verbinding maken met verschillende databases (zoals Microsoft SQL Server en MySQL) en SQL-query's uitvoeren. U kunt dit uitvoeren vanaf een grafische bureaublad-sessiehost (met behulp van de client X2Go bijvoorbeeld). Om aan te roepen SQuirrel SQL, moet u deze starten vanuit het pictogram op het bureaublad of Voer de volgende opdracht op de shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Voordat u het eerste gebruik, stuurprogramma's en database aliassen hebt ingesteld. De JDBC-stuurprogramma's bevinden zich op:

*/usr/share/Java/jdbcdrivers*

Zie voor meer informatie [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Opdrachtregelprogramma's voor toegang tot Microsoft SQL Server
ODBC-stuurprogramma voor massaopslagapparaten voor SQL Server wordt ook geleverd met twee opdrachtregelprogramma's:

**BCP**: het hulpprogramma bcp bulksgewijs gegevens tussen een exemplaar van Microsoft SQL Server en een gegevensbestand in een door de gebruiker opgegeven indeling kopieert. Het hulpprogramma bcp kan worden gebruikt om te importeren van grote aantallen van nieuwe rijen in SQL Server-tabellen, of gegevens buiten tabellen exporteren naar gegevensbestanden. Om gegevens te importeren in een tabel, moet u een bestand-indeling gemaakt voor tabel of inzicht in de structuur van de tabel en de typen gegevens die geldig voor de kolommen zijn.

Zie voor meer informatie [verbinding te maken met bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**Sqlcmd**: Voer Transact-SQL-instructies met het hulpprogramma sqlcmd, evenals de procedures van systeem en scriptbestanden bij de opdrachtprompt. Dit hulpprogramma wordt ODBC Transact-SQL-batches uitvoeren.

Zie voor meer informatie [verbinding te maken met sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Er zijn bepaalde verschillen in dit hulpprogramma tussen Linux- en Windows-platforms. Zie de documentatie voor meer informatie.
> 
> 

#### <a name="database-access-libraries"></a>Database toegang bibliotheken
Er zijn bibliotheken beschikbaar in het R- en Python naar access-databases.

* In R, wordt de **RODBC** pakket of **dplyr** pakket kunt u een query of SQL-instructies uitvoeren op de databaseserver.
* In Python, de **pyodbc** -bibliotheek biedt toegang tot de database met behulp van ODBC als de onderliggende laag.  

### <a name="azure-tools"></a>Azure-hulpprogramma 's
De volgende Azure-hulpprogramma's worden geïnstalleerd op de virtuele machine:

* **Azure-opdrachtregelinterface**: de Azure CLI kunt u maken en beheren van Azure-resources via de shell-opdrachten. Als u wilt aanroepen van de Azure-hulpprogramma's, typt u gewoon **azure help**. Zie voor meer informatie de [documentatiepagina voor Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Storage Explorer**: Microsoft Azure Storage Explorer is een grafisch hulpprogramma dat wordt gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw Azure storage-account en voor het uploaden en downloaden van gegevens naar en van Azure blobs. U kunt Opslagverkenner openen vanaf het bureaublad snelkoppelingspictogram. U kunt het aanroepen van een shell-prompt door **StorageExplorer**. U moet zijn aangemeld vanaf een client X2Go of X11 set up doorgestuurd hebben.
* **Azure-bibliotheken**: Hieronder volgen enkele van de vooraf geïnstalleerde bibliotheken.
  
  * **Python**: betrekking hebben op de Azure-bibliotheken in Python die zijn geïnstalleerd **azure**, **azureml**, **pydocumentdb**, en **pyodbc**. Met de eerste drie bibliotheken, kunt u toegang tot Azure storage-services, Azure Machine Learning en Azure Cosmos-DB (een NoSQL-database in Azure). De vierde bibliotheek pyodbc (samen met het Microsoft ODBC-stuurprogramma voor SQL Server), kunt toegang tot SQL Server-, Azure SQL Database- en Azure SQL Data Warehouse met Python met behulp van een ODBC-interface. Voer **pip lijst** om te zien van de vermelde bibliotheken. Zorg dat u deze opdracht uitvoeren in zowel de Python 2.7 als 3.5 omgevingen.
  * **R**: betrekking hebben op de Azure-bibliotheken in R die zijn geïnstalleerd **AzureML** en **RODBC**.
  * **Java**: de lijst met Azure Java bibliotheken vindt u in de map **/dsvm/sdk/AzureSDKJava** op de virtuele machine. De belangrijkste bibliotheken zijn Azure opslag en beheer-API's, Azure Cosmos DB en JDBC-stuurprogramma's voor SQL Server.  

U hebt toegang tot de [Azure-portal](https://portal.azure.com) in een browser Firefox vooraf worden geïnstalleerd. In de Azure portal kunt u maken, beheren en bewaken van de Azure-resources.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde cloudservice waarmee u kunt maken, implementeren en delen van predictive analytics-oplossingen. U maken uw experimenten en modellen van Azure Machine Learning Studio. Deze toegankelijk is vanuit een webbrowser op de virtuele machine van de gegevens wetenschappelijke in via [Microsoft Azure Machine Learning](https://studio.azureml.net).

Als u zich bij Azure Machine Learning Studio aanmelden, hebt u toegang tot een canvas experimenteren waar kunt u een logische stroom voor de machine learning-algoritmen. U ook toegang hebben tot een Jupyter-notebook gehost op Azure Machine Learning en werkt naadloos samen met de experimenten in Machine Learning Studio. De machine learning-modellen die u hebt gemaakt door ze in een webinterface operationeel. Hierdoor kunnen clients in die zijn geschreven in een andere taal om aan te roepen voorspellingen van de machine learning-modellen. Zie voor meer informatie de [Machine Learning-documentatie](https://azure.microsoft.com/documentation/services/machine-learning/).

U kunt ook uw modellen in R- of Python op de virtuele machine maken en vervolgens te implementeren in productie in Azure Machine Learning. We bibliotheken hebt geïnstalleerd in R (**AzureML**) en Python (**azureml**) om deze functionaliteit te schakelen.

Zie voor meer informatie over het implementeren van modellen in R- en Python in Azure Machine Learning [tien dingen die u op de virtuele Machine voor gegevenswetenschap doen kunt](vm-do-ten-things.md) (met name de sectie ' bouwen van modellen met R- of Python en operationeel te maken met behulp van Azure Machine Learning').

> [!NOTE]
> Deze instructies zijn geschreven voor de Windows-versie van de gegevens wetenschappelijke virtuele machine. Maar de verstrekte informatie bevat over het implementeren van modellen op Azure Machine Learning is van toepassing op de Linux-VM.
> 
> 

### <a name="machine-learning-tools"></a>Machine learning-hulpprogramma 's
De virtuele machine wordt geleverd met een paar machine learning-hulpprogramma's en -algoritmen die zijn vooraf gecompileerd en vooraf lokaal worden geïnstalleerd. Deze omvatten:

* **Vowpal Wabbit**: een leeralgoritme snelle online.
* **xgboost**: een hulpprogramma waarmee geoptimaliseerde, gestimuleerd structuur algoritmen.
* **Rattle**: Er is geen R gebaseerde grafisch hulpprogramma voor het eenvoudig gegevensverkenning en modellering.
* **Python**: Anaconda Python wordt geleverd met machine learning-algoritmen met bibliotheken zoals Scikit meer. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **LightGBM**: een snelle, gedistribueerde, hoge prestaties verloop versterking van framework op basis van decision tree algoritmen.
* **R**: een uitgebreide bibliotheek met machine learning-functies is beschikbaar voor R. Sommige van de bibliotheken die vooraf geïnstalleerd zijn zijn lm, glm, randomForest, rpart. Andere bibliotheken kunnen worden geïnstalleerd door te voeren:
  
        install.packages(<lib name>)

Hier volgt een aantal aanvullende informatie over de eerste drie machine learning-hulpprogramma's in de lijst.

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit is een machine learning-besturingssysteem dat gebruikmaakt van technieken zoals online, hash, allreduce, kortingen, learning2search, actief, en interactieve daarvan te leren.

Als u wilt het hulpprogramma uitvoeren op een eenvoudige voorbeeld, het volgende doen:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Er zijn andere, groter demo's in die map. Zie voor meer informatie over een code [in deze sectie van GitHub](https://github.com/JohnLangford/vowpal_wabbit), en de [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dit is een tapewisselaar die is ontworpen en geoptimaliseerd voor algoritmen gestimuleerd (structuur). Het doel van deze bibliotheek is om de grenzen van de berekening van machines tot het uiterste nodig is voor grootschalige structuur versterking die uiterste schaalbare, draagbare en nauwkeurige.

Als een opdrachtregel, evenals een R-bibliotheek is opgegeven.

U kunt een interactieve R-sessie starten voor het gebruik van deze bibliotheek in R (eenvoudigweg te typen **R** in de shell), en laden van de bibliotheek.

Dit is een eenvoudig voorbeeld die kunt u in R prompt uitvoeren:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Als u wilt uitvoeren op de opdrachtregel xgboost, volgen hier de uit te voeren in de shell-opdrachten:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Een bestand .model is geschreven naar de opgegeven map. Informatie over dit voorbeeld demo vindt [op GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Zie voor meer informatie over xgboost de [xgboost documentatiepagina](https://xgboost.readthedocs.org/en/latest/), en de bijbehorende [GitHub-opslagplaats](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rammelaar
Rattle (de **R** **A**nalytical **T**lpmiddel **T**o **L**verdienen **E** asily) maakt gebruik van GUI gebaseerde gegevensverkenning en modellering. Deze geeft statistische en visuele overzichten van gegevens, transformaties gegevens die gemakkelijk kan worden gemodelleerd, zonder supervisie en onder supervisie modellen van de gegevens is gebaseerd, geeft de prestaties van modellen grafisch weergegeven, en nieuwe gegevens scores wordt ingesteld. Er wordt ook gegenereerd R-code, de bewerkingen in de gebruikersinterface die kunnen worden rechtstreeks in R uitvoeren of als uitgangspunt gebruikt voor verdere analyse te repliceren.

Als u wilt uitvoeren Rammelaar, moet u zich in een grafische aanmelden bureaubladsessie. Typ op de terminal ```R``` invoeren van de R-omgeving. Voer de volgende opdrachten bij de prompt R:

    library(rattle)
    rattle()

Nu een grafische interface wordt geopend met een reeks tabbladen. Hier volgen de snelstartstappen in Rammelaar die nodig zijn voor gebruik van een verzameling voorbeeldgegevens weer en een model bouwen. In sommige van de volgende stappen wordt u gevraagd automatisch installeren en laden van bepaalde vereiste R-pakketten die nog niet op het systeem.

> [!NOTE]
> Als u geen toegang tot het pakket installeert in de systeemmap (de standaardinstelling), ziet u mogelijk een prompt op uw R-consolevenster om pakketten te installeren op uw persoonlijke bibliotheek. Antwoord *y* als u deze aanwijzingen ziet.
> 
> 

1. Klik op **Uitvoeren**.
2. Een dialoogvenster weergegeven waarin u wordt gevraagd als u wilt gebruiken van de gegevensverzameling van voorbeeld weer. Klik op **Ja** laden in het voorbeeld.
3. Klik op de **Model** tabblad.
4. Klik op **Execute** voor het bouwen van een beslissingsstructuur.
5. Klik op **tekenen** om weer te geven van de beslissingsstructuur.
6. Klik op de **Forest** keuzerondje en klikt u op **Execute** voor het bouwen van een willekeurige forest.
7. Klik op de **Evaluate** tabblad.
8. Klik op de **risico** keuzerondje en klikt u op **Execute** om twee risico (cumulatief) prestaties waarnemingspunten weer te geven.
9. Klik op de **logboek** tabblad om de code genereren R voor voorgaande bewerkingen weer te geven.
   (Vanwege een fout in de huidige release van Rammelaar, moet u invoegen een  *#*  teken vóór *... dit logboek exporteren*  in de tekst van het logboek.)
10. Klik op de **exporteren** om op te slaan van het R-script-bestand met de naam *weather_script. R* naar de basismap.

U kunt afsluiten Rammelaar en R. U kunt nu het gegenereerde R-script wijzigen of ongewijzigd uit te voeren op elk gewenst moment om te worden herhaald alles die werd uitgevoerd in de gebruikersinterface Rattle gebruiken. Dit is een eenvoudige manier om snel analyses en machine learning in een eenvoudige grafische interface tijdens het automatisch genereren van code in R om te wijzigen en/of meer met name voor beginnende gebruikers in R.

## <a name="next-steps"></a>Volgende stappen
Hier ziet u hoe u kunt blijven uw leren en te verkennen:

* De [gegevenswetenschap op de virtuele Machine voor Linux van gegevens wetenschappelijke](linux-dsvm-walkthrough.md) overzicht toont u hoe u enkele algemene taken in het wetenschappelijke gegevens uitvoert met het Linux gegevens wetenschappelijke VM hier ingericht. 
* Bekijk de verschillende beschikbare hulpprogramma wetenschappelijke gegevens op de virtuele machine voor gegevenswetenschap aan het uitproberen van de hulpprogramma's beschreven in dit artikel. U kunt ook uitvoeren *dsvm meer gegevens* op de shell binnen de virtuele machine voor een algemene inleiding tot en koppelingen naar meer informatie over de hulpprogramma's geïnstalleerd op de virtuele machine.  
* Informatie over het bouwen van de end-to-end analyseoplossingen systematischer met behulp van de [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Ga naar de [Cortana-Analytics galerie](http://gallery.cortanaanalytics.com) voor machine learning en analytics voorbeelden die gebruikmaken van de Cortana-Analytics Suite.

