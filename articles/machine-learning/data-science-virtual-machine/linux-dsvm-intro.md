---
title: Maken van een CentOS Linux Data Science Virtual Machine
titleSuffix: Azure
description: Configureer en maak een Linux Data Science Virtual Machine in Azure voor het analyseren en machine learning.
services: machine-learning
documentationcenter: ''
author: gopitk
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
ms.author: gokuma
ms.openlocfilehash: 253934d450619ca67e429fbf396a5fed5b71a267
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081860"
---
# <a name="provision-a-linux-centos-data-science-virtual-machine-on-azure"></a>Een Linux CentOS Data Science Virtual Machine op Azure inrichten

De Linux Data Science Virtual Machine is een CentOS gebaseerde virtuele machine van Azure die wordt geleverd met een verzameling vooraf geïnstalleerde hulpprogramma's. Deze hulpprogramma's worden vaak gebruikt voor het uitvoeren van gegevensanalyse en machine learning. Er zijn belangrijke software-onderdelen opgenomen:

* Besturingssysteem: CentOS Linux-distributie.
* Microsoft R Server Developer Edition
* Anaconda Python-distributie (versie 2.7 en 3.5), met inbegrip van populaire analysebibliotheken
* JuliaPro - een gecureerde distributie van Julia-taal met populaire wetenschappelijke en data analytics-bibliotheken
* Standalone Spark-instantie en één knooppunt Hadoop (HDFS, Yarn)
* JupyterHub - een omgeving met meerdere gebruikers Jupyter-notebookserver ondersteunt R, Python, PySpark, Julia kernels
* Azure Opslagverkenner
* Azure-opdrachtregelinterface (CLI) voor het beheren van Azure-resources
* Database voor PostgresSQL
* Hulpprogramma's voor machine learning
  * [Cognitive Toolkit](https://github.com/Microsoft/CNTK): een deep learning toolkit voor software van Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine learning-systeem, zoals de online, hash, allreduce, kortingen, learning2search, actief, ondersteuning en interactieve training.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpprogramma voor snelle en nauwkeurige boosted structuur-implementatie.
  * [Rattle](https://togaware.com/rattle/) (de R analytische hulpprogramma om te leren eenvoudig): een hulpprogramma waarmee aan de slag met data analytics en machine learning in R eenvoudig, met een GUI-gebaseerde gegevens verkennen en modelleren met automatische R-code genereren.
* Azure SDK in Java, Python, node.js, Ruby, PHP
* Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure-services
* Ontwikkelprogramma's en editors (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi)


Gegevenswetenschap omvat het doorlopen van een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
1. Het bouwen en testen van modellen
1. De modellen voor gebruik in intelligente toepassingen implementeren

Gegevenswetenschappers verschillende hulpprogramma's gebruiken om deze taken te voltooien. Worden erg tijdrovend zijn om de juiste versies van de software zoeken en vervolgens om te downloaden, compileren kan, en deze versies installeren.

De Linux Data Science Virtual Machine kan deze last aanzienlijk vereenvoudigen. Gebruik dit voor uw analyticsproject een vliegende start geven. Hiermee kunt u taken in verschillende talen, waaronder R, Python, SQL, Java en C++. Eclipse biedt een IDE voor het ontwikkelen en testen van uw code die is eenvoudig te gebruiken. De Azure SDK opgenomen in de virtuele machine kunt u uw toepassingen kunt maken met verschillende services in Linux gebruiken voor het cloudplatform van Microsoft. Bovendien hebt u toegang tot andere talen zoals Ruby, Perl, PHP en node.js die vooraf zijn geïnstalleerd.

Er zijn geen kosten voor software voor deze data science VM-installatiekopie. U betaalt alleen de Azure-hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die u met de VM-installatiekopie inricht. Meer informatie over de compute-kosten te vinden in de [pagina met de vermelding op Azure Marketplace VM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de virtuele Machine voor Datatechnologie
Een [Ubuntu](dsvm-ubuntu-intro.md) installatiekopie is ook beschikbaar zijn, met veel van dezelfde hulpprogramma's als de installatiekopie van CentOS plus deep learning-frameworks. Een [Windows](provision-vm.md) installatiekopie is ook beschikbaar.

## <a name="prerequisites"></a>Vereisten
Voordat u een Linux Data Science Virtual Machine maken kunt, moet u het volgende hebt:

* **Een Azure-abonnement**: als u wilt aanvragen, Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/free/).
* **Een Azure storage-account**: maken van een Zie [maken van een Azure storage-account](../../storage/common/storage-quickstart-create-account.md). Als u niet gebruiken van een bestaand account wilt, kan het storage-account ook worden gemaakt als onderdeel van het proces voor het maken van de virtuele machine.

## <a name="create-your-linux-data-science-virtual-machine"></a>Maken van uw Linux Data Science Virtual Machine
Hier volgen de stappen voor het maken van een exemplaar van de Linux Data Science Virtual Machine:

1. Navigeer naar de virtuele machine weergeven op de [Azure-portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
1. Klik op **maken** (aan de onderkant) om de wizard.![ Configureer-data-science-vm](./media/linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
1. De volgende secties vindt u de invoer voor elk van de stappen in de wizard (geïnventariseerd aan de rechterkant van de afbeelding hierboven) gebruikt voor het maken van de Microsoft Data Science Virtual Machine. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:
   
   a. **Grondbeginselen van**:
   
   * **Naam**: naam van uw data science-server die u maakt.
   * **Gebruikersnaam**: eerste aanmelding bij het account ID.
   * **Wachtwoord**: eerste wachtwoord (u kunt openbare SSH-sleutel gebruiken in plaats van wachtwoord).
   * **Abonnement**: als u meer dan één abonnement hebt, selecteert u het een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht. U hebt privileges voor het maken van resources nodig voor dit abonnement.
   * **Resourcegroep**: U kunt een nieuwe groep maken of een bestaande groep gebruikt.
   * **Locatie**: Selecteer het datacenter die het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens, of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk.
   
   b. **Grootte**:
   
   * Selecteer een van de servertypen die voldoet aan de vereisten van het functionele en kostenbeperkingen. Selecteer **Alles weergeven** om te zien van meer opties van VM-grootten.
   
   c. **Instellingen voor**:
   
   * **Schijftype**: kies **Premium** als u liever een SSD (solid-state drive). Kies anders **Standard**.
   * **Storage-Account**: U kunt een nieuw Azure storage-account maken in uw abonnement, of gebruik een bestaande resourcegroep in dezelfde locatie die is gekozen voor de **basisbeginselen** stap van de wizard.
   * **Andere parameters**: In de meeste gevallen gebruikt u alleen de standaardwaarden. U moet overwegen niet-standaard waarden, de muisaanwijzer over de informatieve koppeling voor meer informatie over de specifieke velden.
   
   d. **Samenvatting**:
   
   * Controleer of alle informatie die u hebt ingevoerd juist is.
   
   e. **Kopen**:
   
   * Voor het starten van het inrichten, klikt u op **kopen**. Er wordt een koppeling gegeven naar de voorwaarden van de transactie. De virtuele machine heeft geen eventuele extra kosten buiten de rekenkracht voor de servergrootte van de die u hebt gekozen in de **grootte** stap.

De inrichting duurt ongeveer 10-20 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Toegang tot de Linux Data Science Virtual Machine
Nadat de virtuele machine is gemaakt, kunt u zich aanmelden toe met behulp van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de tekst shell-interface. In Windows kunt u een SSH-clienthulpprogramma zoals [Putty](http://www.putty.org) downloaden. Als u liever een grafische desktop (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go uitgevoerd aanzienlijk beter dan X11 doorsturen in de testfase. Het is raadzaam om met behulp van de client X2Go voor een grafische interface voor het bureaublad.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go client
De Linux-VM is al ingericht met X2Go-server en gereed voor clientverbindingen accepteren. Voor verbinding met het bureaublad van de Linux-VM-grafische, het volgende op de client te doen:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
1. De X2Go-client wordt uitgevoerd, en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: de hostnaam of IP-adres van uw Linux Data Science VM.
     * **Aanmelding**: de naam van de gebruiker op de Linux-VM.
     * **SSH-poort**: 22, de standaardwaarde accepteren.
     * **Sessietype**: Wijzig de waarde in XFCE. De Linux-VM ondersteunt momenteel alleen XFCE desktop.
   * **Tabblad Media**: als u niet nodig hebt om goede ondersteuning en afdrukken-client te gebruiken, kunt u deze uitschakelen.
   * **Gedeelde mappen**: als u mappen van uw clientmachines die gekoppeld is op de Linux-VM wilt, voegt u de mappen op de client computer die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of de XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u snelkoppelingen voor toepassingen in het menu en pictogrammen op het bureaublad voor veel van de hulpprogramma's.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Linux Data Science Virtual Machine
### <a name="microsoft-r-server"></a>Microsoft R Server
R is een van de meest populaire talen voor data-analyse en machine learning. Als u R gebruiken voor uw analyses wilt, heeft de virtuele machine Microsoft R Server (MEVR) met de Microsoft R Open (BHT) en Math Kernel-bibliotheek (MKL). De MKL optimaliseert wiskundige bewerkingen gebruikelijk in analytische algoritmen. BHT is 100 procent is compatibel met CRAN R en een van de R-bibliotheken die is gepubliceerd in CRAN kan worden geïnstalleerd op de BHT. MEVR biedt u schalen en uitoefening van R-modellen in webservices. U kunt uw R-programma's in een van de standaard-editors, zoals RStudio, vi, Emacs of gedit bewerken. Als u van de editor Emacs gebruikmaakt, is Houd er rekening mee dat de Emacs ERDRUKKEN (Emacs spreekt statistieken), vereenvoudigt pakket werken met R-bestanden in de editor Emacs vooraf geïnstalleerd.

Console te starten, R, typt u **R** in de shell. Hiermee gaat u naar een interactieve omgeving. Voor het ontwikkelen van uw R-programma, u gewoonlijk gebruikt een editor zoals Emacs of vi of gedit en voer de scripts in R. Met RStudio hebt u een volledige grafische IDE-omgeving voor het ontwikkelen van uw R-programma.

Er is ook een R-script voor installatie de [Top 20 R-pakketten](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) als u wilt. Met dit script kan worden uitgevoerd nadat u zich in de interactieve interface van R, die kan worden ingevoerd (zoals vermeld) door te typen **R** in de shell.  

### <a name="python"></a>Python
Voor het ontwikkelen met behulp van Python, is distributie Anaconda Python 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten. U kunt de standaard-teksteditors. Bovendien kunt u Spyder, een Python IDE die wordt geleverd met Anaconda Python-distributies. Spyder moet een grafische bureaublad of X11 doorsturen. Een snelkoppeling naar Spyder is opgegeven in het grafische bureaublad.

Aangezien we zowel Python 2.7 en 3.5 hebben, moet u de gewenste Python-versie (conda-omgeving) u werken wilt op in de huidige sessie om precies te activeren. Het activeringsproces stelt de variabele PATH op de gewenste versie van Python.

Voer de volgende opdracht uit vanuit de shell voor het activeren van de Python 2.7 conda-omgeving:

    source /anaconda/bin/activate root

Python 2.7 is geïnstalleerd op */anaconda/bin*.

Voor het activeren van de Python 3.5 conda-omgeving, voert u de volgende vanuit de shell:

    source /anaconda/bin/activate py35


Python 3.5 is geïnstalleerd op */anaconda/envs/py35/bin*.

Voor het aanroepen van een Python-interactieve sessie, typt u gewoon **python** in de shell. Als u zich op een grafische interface of X11 set up doorsturen hebt, kunt u typen **pycharm** de PyCharm Python IDE starten.

Voor het installeren van extra Python-bibliotheken, die u nodig hebt om uit te voeren ```conda``` of ````pip```` opdracht onder sudo en het volledige pad van de Python package manager (conda of pip) om te installeren op de juiste Python-omgeving opgeven. Bijvoorbeeld:

    sudo /anaconda/bin/pip install <package> #pip for Python 2.7
    sudo /anaconda/envs/py35/bin/pip install <package> #pip for Python 3.5
    sudo /anaconda/bin/conda install [-n py27] <package> #conda for Python 2.7, default behavior
    sudo /anaconda/bin/conda install -n py35 <package> #conda for Python 3.5


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
Een zelfstandig exemplaar van Apache Spark is vooraf geïnstalleerd op de Linux-DSVM om u te helpen bij het ontwikkelen van toepassingen voor Spark lokaal eerst voor testen en implementeren op grote clusters. U kunt de PySpark-programma's uitvoeren via de Jupyter-kernel. Wanneer u Jupyter openen en klik op de **nieuw** knop klikt, ziet u een lijst met beschikbare kernels. De 'Spark – Python' is de PySpark-kernel waarmee u Spark toepassingen bouwen met Python-taal. U kunt ook een Python IDE zoals PyCharm of Spyder gebruiken om te bouwen u Spark-programma. Aangezien dit is een zelfstandig exemplaar, de Spark-stack binnen de aanroepende clientprogramma wordt uitgevoerd. Hierdoor kunt u sneller en gemakkelijker problemen in vergelijking met het ontwikkelen van een Spark-cluster. 

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
U hebt een ruime keuze aan verschillende code-editors. Dit omvat vi/VIM, Emacs gEdit, PyCharm, RStudio, Eclipse en IntelliJ. gEdit, Eclipse, IntelliJ, RStudio en PyCharm grafische editors zijn en moet u zijn aangemeld met een grafische bureaublad om ze te gebruiken. Deze editors hebben bureaublad- en toepassingsservices snelkoppelingen in het menu te starten.

**VIM** en **Emacs** editors op basis van tekst zijn. Op Emacs, hebben we een add-on pakket met de naam Emacs spreekt statistieken (eldingen ONDERDRUKKEN) die werken met R gemakkelijker in de editor Emacs geïnstalleerd. Meer informatie kunt vinden op [ERDRUKKEN](http://ess.r-project.org/).

**Eclipse** is een open source, uitbreidbare IDE die ondersteuning biedt voor meerdere talen. De Java-ontwikkelaars-editie wordt het exemplaar geïnstalleerd op de virtuele machine. Er zijn invoegtoepassingen beschikbaar zijn voor verschillende veelgebruikte talen die kunnen worden geïnstalleerd om uit te breiden de omgeving. We hebben ook een invoegtoepassing geïnstalleerd in Eclipse met de naam **Azure Toolkit voor Eclipse**. Hiermee kunt u maken, ontwikkelen, testen en implementeren van Azure met behulp van de Eclipse-ontwikkelomgeving die ondersteuning biedt voor talen zoals Java-toepassingen. Er is ook een **Azure SDK voor Java** waarmee toegang tot verschillende Azure-services uit binnen een Java-omgeving. Meer informatie over Azure toolkit voor Eclipse kan worden gevonden op [Azure Toolkit voor Eclipse](../../azure-toolkit-for-eclipse.md).

**LaTex** wordt geïnstalleerd via de texlive-package samen met een invoegtoepassing Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) pakket vereenvoudigt het ontwerpen van uw documenten LaTex binnen Emacs.  

### <a name="databases"></a>Databases
#### <a name="postgres"></a>Postgres
De open-SourceDatabase **Postgres** is beschikbaar op de virtuele machine, met de services die worden uitgevoerd en initdb al voltooid. U moet nog steeds om databases en gebruikers te maken. Zie voor meer informatie de [Postgres documentatie](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafische SQL-client
**SQuirrel SQL**, een grafische SQL-client is opgegeven voor het verbinding maken met verschillende databases (zoals Microsoft SQL Server, Postgres en MySQL) en SQL-query's uitvoeren. U kunt dit uitvoeren vanaf een grafische bureaublad-sessiehost (met behulp van de client X2Go, bijvoorbeeld). Om aan te roepen SQuirrel SQL, kunt u starten vanaf het pictogram op het bureaublad of Voer de volgende opdracht op de shell.

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

Toegang krijgen tot **Postgres**:

* Gebruik van het pakket uit R: **RPostgreSQL**.
* Gebruik van Python: De **psycopg2** bibliotheek.

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

* **Microsoft Cognitive Toolkit** : een deep learning toolkit.
* **Vowpal Wabbit**: een leeralgoritme snel online.
* **xgboost**: een hulpprogramma waarmee u geoptimaliseerde, boosted algoritmen.
* **Python**: Anaconda Python wordt geleverd met machine learning-algoritmen met bibliotheken, zoals Scikit-informatie. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **R**: Er is een uitgebreide bibliotheek met machine learning-functies beschikbaar voor R. Sommige van de bibliotheken die vooraf geïnstalleerd zijn zijn, er lm glm, randomForest rpart. Andere bibliotheken kunnen worden geïnstalleerd door uit te voeren:
  
        install.packages(<lib name>)

Hier is aanvullende informatie over de eerste drie machine learning-hulpprogramma's in de lijst.

#### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
Dit is een open-source, deep learning toolkit. Het is een opdrachtregelprogramma (cntk), en is al in het pad.

Voor het uitvoeren van een eenvoudig voorbeeld, voert u de volgende opdrachten in de shell:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Zie voor meer informatie de sectie CNTK van [GitHub](https://github.com/Microsoft/CNTK), en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

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

* De [gegevenswetenschap op de Linux Data Science Virtual Machine](linux-dsvm-walkthrough.md) procedure ziet u hoe u enkele algemene datatechnologietaken met de Linux Data Science VM ingericht hier uitvoert. 
* Verken de verschillende hulpprogramma's voor data science op de virtuele machine voor datatechnologie door het uitproberen van de hulpprogramma's die worden beschreven in dit artikel. U kunt ook uitvoeren *dsvm-meer-info* op de shell binnen de virtuele machine voor een algemene inleiding tot en tips voor meer informatie over de hulpprogramma's geïnstalleerd op de virtuele machine.  
* Informatie over het bouwen van analytische oplossingen voor end-to-end systematisch met behulp van de [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
* Ga naar de [Cortana Analytics-galerie](http://gallery.cortanaanalytics.com) voor machine learning en data analytics-voorbeelden die gebruikmaken van de Cortana Analytics Suite.

