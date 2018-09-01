---
title: Inrichten van de Windows Data Science Virtual Machine in Azure | Microsoft Docs
description: Configureren en een Data Science Virtual Machine in Azure maken voor analyse en machine learning.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: gokuma
ms.openlocfilehash: b01ef3701ffb46da57c52e5fe73828ec4252b074
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344757"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Inrichten van de Windows Data Science Virtual Machine van Azure
De Microsoft Data Science Virtual Machine (DSVM) is een installatiekopie van Windows Azure virtuele machine (VM). Het is vooraf geïnstalleerd en geconfigureerd met verschillende hulpprogramma's die worden gebruikt voor gegevensanalyse en machine learning. De volgende hulpprogramma's zijn opgenomen:

* [Azure Machine Learning](../service/index.yml) Workbench.
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer edition.
* Anaconda Python-distributie.
* Jupyter-Notebook met R, Python en PySpark-kernels.
* Microsoft Visual Studio Community.
* Microsoft Power BI desktop.
* Microsoft SQL Server 2017 Developer edition.
* Een zelfstandig Apache Spark-exemplaar voor lokale ontwikkeling en testen.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Machine learning en data analytics-hulpprogramma's:
  * Frameworks voor deep learning. Een grote verscheidenheid aan AI-frameworks zijn opgenomen op de virtuele machine: [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet, en Keras.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit). Een snelle machine learning-systeem die ondersteuning biedt voor technieken zoals online hashing, allreduce, kortingen, learning2search en actieve en interactieve training.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/). Een hulpprogramma waarmee u snel en nauwkeurige boosted structuur-implementatie.
  * [Rattle](https://togaware.com/rattle/), de R analytisch hulpmiddel voor meer informatie over eenvoudig. Een hulpprogramma waarmee u aan de slag met data analytics en machine learning in R. Het bevat GUI-gebaseerde gegevens verkennen en modelleren met automatische generatie van R-code.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/). Visual gegevensanalyse en machine learning-software in Java.
  * [Apache Drill](https://drill.apache.org/). Een schema's SQL query-engine voor Apache Hadoop-, NoSQL- en opslag in de cloud.  ODBC- en JDBC-interfaces worden ondersteund voor het uitvoeren van query's NoSQL en bestanden van standard BI-hulpprogramma's zoals Power BI, Microsoft Excel en Tableau.
* Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure-services.
* GIT, met inbegrip van Git Bash, om te werken met broncodeopslagplaatsen, zoals GitHub en Visual Studio Team Services. GIT biedt verschillende populaire Linux-opdrachtregelprogramma's die toegankelijk zijn zowel op Git Bash en een opdrachtprompt. Voorbeelden zijn awk, sed, perl, grep, zoeken, wget en curl.

Wetenschappelijke gegevens omvat het doorlopen van een reeks taken:

1. Zoeken, laden en gegevens voorverwerken.
1. Bouwen en testen van modellen.
1. De modellen voor gebruik in intelligente toepassingen te implementeren.

Gegevenswetenschappers gebruiken verschillende hulpprogramma's voor deze taken. Kan het zijn tijdrovend zijn om de juiste versies van software en download en installeer vervolgens deze. De Microsoft Data Science Virtual Machine bespaart u tijd door op te geven van de installatiekopie van een kant-en-klare die kan worden ingericht op Azure met enkele populaire hulpprogramma's vooraf geïnstalleerd en geconfigureerd. 

De Microsoft Data Science Virtual Machine jump-starts uw analyticsproject. U kunt werken aan taken in verschillende talen, waaronder van R, Python, SQL en C#. Visual Studio biedt een eenvoudig te gebruiken geïntegreerde ontwikkelomgeving (IDE) voor het ontwikkelen en testen van uw code. De Azure SDK is opgenomen in de virtuele machine. U kunt dus uw toepassingen bouwen met behulp van verschillende services op cloudplatform van Microsoft. 

Er zijn geen kosten voor software voor deze data science VM-installatiekopie. U betaalt alleen de kosten van het gebruik van Azure. Ze afhankelijk zijn van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten zijn de **prijsinformatie** sectie op de [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) pagina. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de Data Science Virtual Machine
* Een [Ubuntu](dsvm-ubuntu-intro.md) installatiekopie. Het bevat veel hulpprogramma's die vergelijkbaar is met de DSVM plus een aantal extra deep learning-frameworks. 
* Een [Linux CentOS](linux-dsvm-intro.md) installatiekopie.
* De [edition van Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) van de Data Science Virtual Machine. Een paar hulpprogramma's zijn alleen beschikbaar op de editie van Windows Server 2016. In dit artikel geldt in alle andere gevallen ook voor de editie van Windows Server 2012.

## <a name="prerequisite"></a>Vereiste
Voor het maken van een Microsoft Data Science Virtual Machine, moet u een Azure-abonnement hebben. Zie [Gratis proefversie van Azure ophalen](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Uw Microsoft Data Science-Machine maken
Volg deze stappen voor het maken van een exemplaar van de Microsoft Data Science Virtual Machine:

1. Navigeer naar de virtuele machine weergeven op de [Azure-portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). U mogelijk gevraagd om aan te melden bij uw Azure-account als u bent niet al aangemeld.
1. Selecteer de **maken** onder in de wizard een moeten worden genomen.

  ![Configureer-data-science-vm](./media/provision-vm/configure-data-science-virtual-machine.png) 

1. De wizard die wordt gemaakt van de Microsoft Data Science Virtual Machine vereist **invoer**. De volgende invoer nodig is voor het configureren van de stappen die aan de rechterkant van de afbeelding wordt weergegeven:

  a. **Grondbeginselen van**:

    i. **Naam**. De naam van de data science-server die u maakt.  

    II. **VM-schijftype**. Kies **SSD** of **HDD**. Kies voor een NC_v1 GPU-exemplaar, zoals NVidia Tesla R80 op basis van **HDD** als het schijftype.   

    III. **Gebruikersnaam**. De admin-account-ID aan te melden.   

    IV. **Wachtwoord**. Wachtwoord voor het beheerdersaccount.  

    v. **Abonnement**. Als u meer dan één abonnement hebt, selecteert u een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht.   

    VI. **Resourcegroep**. U kunt een nieuwe groep maken of een bestaande groep gebruikt.   

    VII. **Locatie**. Selecteer het datacenter die het meest geschikt is. Voor de snelste toegang tot het netwerk is het datacenter dat de meeste van uw gegevens of zich het dichtst bij uw fysieke locatie.   

  b. **Grootte**. Selecteer een van de servertypen die aansluit bij uw functionele vereisten en kostenbeperkingen. Voor meer opties van VM-grootten, selecteer **Alles weergeven**.  

  c. **Instellingen voor**:  

    i. **Beheerde schijven gebruiken**. Kies **beheerde** als u wilt dat Azure voor het beheren van de schijven voor de virtuele machine. Als dat niet het geval is, moet u een nieuwe of bestaande storage-account opgeven.  

    II. **Andere parameters**. U kunt de standaardwaarden gebruiken. Als u gebruiken niet-waarden wilt, Beweeg de muisaanwijzer over de informatieve koppeling voor meer informatie over de specifieke velden.  

  d. **Samenvatting**. Controleer of alle informatie die u hebt ingevoerd juist is. Selecteer **Maken**. 

> [!NOTE]
> * De virtuele machine beschikt niet over eventuele extra kosten buiten de compute-kosten voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 
> * De inrichting duurt ongeveer 10 tot 20 minuten. De status ervan weergegeven in de Azure portal.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Toegang tot de Microsoft Data Science Virtual Machine
Nadat de virtuele machine is gemaakt en ingericht, kunt u extern bureaublad in deze met behulp van de accountreferenties van de beheerder die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. U bent klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Veel van de hulpprogramma's hebben de tegels van het menu start en pictogrammen op het bureaublad. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Microsoft Data Science Virtual Machine

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer edition
U kunt Microsoft Enterprise Library voor schaalbare R of Python voor uw analyses gebruiken omdat de Machine Learning Server Developer edition is geïnstalleerd op de virtuele machine. Voorheen bekend als Microsoft R Server, is Machine Learning Server een analyseplatform grotendeels implementeerbare enterprise-klasse. Het is beschikbaar voor zowel R als Python en schaalbare, veilige en commercieel ondersteunde. 

Machine Learning-Server biedt ondersteuning voor verschillende big data-statistieken, voorspellende modellen en machine learning-taken. Deze biedt ondersteuning voor het volledige aanbod van analytics: verkenning, analyse, visualisatie en modellering. Door en uitbreiden van open-source R en Python, is de Machine Learning Server compatibel met R en Python-scripts en functies. Het is ook compatibel met CRAN, pip en Conda-pakketten voor het analyseren van gegevens op de grote schaal. 

Machine Learning Server lost de geheugenbeperkingen van open-source R door het toevoegen van parallelle en bloksgewijze verwerking van gegevens. U kunt dus analytics uitvoeren op de hoeveelheid gegevens groter zijn dan wat in het hoofdgeheugen past. Visual Studio Community is opgenomen op de virtuele machine. Heeft de R-hulpprogramma's voor Visual Studio en Python Tools voor Visual Studio (PTVS)-extensie die een IDE met volledige leveren voor het werken met R of Python. We bieden ook andere IDE's zoals [RStudio](http://www.rstudio.com) en [PyCharm Community edition](https://www.jetbrains.com/pycharm/) op de virtuele machine. 

### <a name="python"></a>Python
Voor het ontwikkelen met behulp van Python zijn-distributies Anaconda Python 2.7 en 3.6 geïnstalleerd. Deze distributies hebben de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten. U kunt PTVS, die is geïnstalleerd binnen Visual Studio Community 2017. Of u kunt een van de IDE's geleverd met Anaconda, zoals niet-actief of Spyder gebruiken. Zoek en start een van deze pakketten (Win + S).

> [!NOTE]
> Om te verwijzen de Python-Tools voor Visual Studio op Anaconda Python 2.7, die u wilt maken van aangepaste omgevingen voor elke versie. Deze omgeving paden instellen in Visual Studio 2017 Community, gaat u naar **extra** > **Python Tools** > **Python-omgevingen**. Selecteer vervolgens **+ aangepast**. 
> 
> 

Anaconda Python 3.6 is geïnstalleerd onder **C:\Anaconda**. Anaconda Python 2.7 is geïnstalleerd onder **c:\Anaconda\envs\python2**. Zie voor gedetailleerde stappen [documentatie bij PTVS](/visualstudio/python/installing-python-interpreters.md). 

### <a name="the-jupyter-notebook"></a>De Jupyter-Notebook
Anaconda distributie wordt geleverd met de Jupyter-Notebook, een omgeving voor het delen van code en -analyse. De Jupyter-Notebook-server is vooraf geconfigureerd met Python 2.7, Python 3.x PySpark, Julia en R-kernels. Start de Jupyter-server en start u de browser voor toegang tot de notebook-server, er wordt een pictogram op het bureaublad genoemd **Jupyter-Notebook**. 

We het pakket verschillende voorbeeldnotitieblokken in Python en R. Nadat u toegang Jupyter tot, geeft de notebooks laten zien hoe om te werken met de volgende technologieën:

* Machine Learning-Server.
* SQL Server Machine Learning-Services, analyses in databases. 
* Python.
* Microsoft Cognitive ToolKit.
* Tensorflow.
* Andere Azure-technologieën. 

U ziet de koppeling naar de voorbeelden op de startpagina van de notebook nadat u voor de Jupyter-Notebook verifiëren met behulp van het wachtwoord die u in een eerdere stap hebt gemaakt. 

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017
Visual Studio Community is geïnstalleerd op de virtuele machine. Het is een gratis versie van de populaire IDE van Microsoft die u voor evaluatiedoeleinden gebruikt en kleine teams gebruiken kunt. Zie de [licentievoorwaarden](https://www.visualstudio.com/support/legal/mt171547). 

Open Visual Studio door te dubbelklikken op het pictogram op het bureaublad of de **Start** menu. Zoeken naar programma's (Win + S), gevolgd door **Visual Studio**. U kunt daar projecten maken in talen zoals C#, Python, R en node.js. Geïnstalleerde invoegtoepassingen maken het handig om te werken met de volgende Azure-services:
* Azure Data Catalog
* Azure HDInsight Hadoop en Spark
* Azure Data Lake 

Er is ook een invoegtoepassing met de naam ```Visual Studio Tools for AI``` die naadloos kan worden geïntegreerd met Azure Machine Learning en helpt u snel build AI-toepassingen. 

> [!NOTE]
> Mogelijk is, krijgt u een bericht dat uw evaluatieperiode is verlopen. Voer de referenties van uw Microsoft-account. Of maak een nieuwe gratis account voor toegang tot Visual Studio Community. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition
Een developer-versie van SQL Server 2017 met Machine Learning-Services voor het uitvoeren van analyses in de database is beschikbaar op de virtuele machine in R of Python. Machine Learning-Services biedt een platform voor het ontwikkelen en implementeren van intelligente toepassingen. U kunt deze talen en veel pakketten van de community om te maken van modellen en voorspellingen voor uw SQL Server-gegevens genereren. Omdat de Machine Learning-Services, in de database, de R- en Python talen in SQL Server is geïntegreerd, kunt u analytics dicht bij de gegevens bewaren. Deze integratie elimineert de kosten en beveiligingsrisico's die zijn gekoppeld aan de verplaatsing van gegevens.

> [!NOTE]
> De editie van SQL Server Developer is alleen voor ontwikkelings- en testdoeleinden. U moet een licentie voor het in productie uitvoeren. 
> 
> 

U kunt toegang tot SQL Server door het starten van Microsoft SQL Server Management Studio. De naam van uw virtuele machine wordt ingevuld als de **servernaam**. Windows-verificatie gebruiken wanneer u zich aanmelden als de beheerder van Windows. Wanneer u zich in SQL Server Management Studio, kunt u maken van andere gebruikers, databases maken, gegevens importeren en uitvoeren van SQL-query's. 

Als u wilt inschakelen in database analyses met behulp van SQL-Machine Learning-Services, door de volgende opdracht uit als een eenmalige actie in SQL Server Management Studio uitvoeren nadat u zich hebt aangemeld als beheerder van de server: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Verschillende Azure-hulpprogramma's zijn geïnstalleerd op de virtuele machine:

* Een snelkoppeling op het bureaublad gaat u naar de Azure SDK-documentatie. 
* **AzCopy** wordt gebruikt om gegevens in en uit uw Azure Storage-account te verplaatsen. Als u wilt zien gebruik, voer **Azcopy** achter de opdrachtprompt. 
* Gebruik **Azure Storage Explorer** om te bladeren van de objecten die u in uw Azure Storage-account opslaat. Ook stuurt deze gegevens van en naar Azure Storage. Voor toegang tot dit hulpprogramma, kunt u **Opslagverkenner** in de **zoeken** veld. Of vinden op de Windows **Start** menu. 
* **Adlcopy** gegevens verplaatst naar Azure Data Lake. Als u wilt zien gebruik, voer **adlcopy** in een opdrachtprompt. 
* **dtui** verplaatst gegevens van en naar Azure Cosmos DB, een NoSQL-database in de cloud. Voer **dtui** in een opdrachtprompt. 
* **Azure Data Factory Integration Runtime** verplaatst gegevens tussen on-premises gegevensbronnen en de cloud. Het wordt gebruikt in hulpprogramma's zoals Azure Data Factory. 
* **Microsoft Azure PowerShell** is een hulpprogramma dat wordt gebruikt voor het beheren van uw Azure-resources in de PowerShell-scripttaal. Het ook geïnstalleerd op de virtuele machine. 

### <a name="power-bi"></a>Power BI
De **Power BI Desktop** is geïnstalleerd voor informatie over het bouwen van dashboards en visualisaties. Met dit hulpprogramma kunt gegevens ophalen uit verschillende bronnen, uw dashboards en rapporten maken en deze publiceren naar de cloud. Zie voor meer informatie de [Power BI](http://powerbi.microsoft.com) site. U kunt de Power BI desktop vinden op de **Start** menu. 

> [!NOTE]
> U hebt een Microsoft Office 365-account voor toegang tot Power BI nodig. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning Workbench is een bureaubladtoepassing en opdrachtregelinterface. De workbench heeft ingebouwde gegevensvoorbereiding waarmee u de stappen voor gegevensvoorbereiding leert terwijl u ze nemen. Het biedt ook projectbeheer, uitvoeringsgeschiedenis en integratie van notitieblokken om uw productiviteit te verbeteren. 

Kunt u open-source frameworks, waaronder TensorFlow, Cognitive Toolkit, Spark ML en scikit-informatie over het ontwikkelen van uw modellen. Op de DSVM, bieden we een pictogram voor het installeren van de Azure Machine Learning Workbench in de individuele gebruiker op het bureaublad **% LOCALAPPDATA %** directory. 

Elke gebruiker van de workbench moet een eenmalige actie ondernemen. Dubbelklik op de ```AzureML Workbench Setup``` pictogram op het bureaublad de workbench-exemplaar te installeren. Azure Machine Learning ook maakt en gebruikt een per gebruiker Python-omgeving die wordt opgehaald in de **%LOCALAPPDATA%\amlworkbench\python** directory.

## <a name="more-microsoft-development-tools"></a>Meer Microsoft-ontwikkelprogramma 's
De [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) wordt gebruikt om te zoeken en downloaden van andere Microsoft-ontwikkelhulpprogramma's. Er is ook een snelkoppeling naar het hulpprogramma dat is opgegeven op het bureaublad van Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de virtuele machine
| Item | Directory |
| --- | --- |
| Jupyter-Notebook server-configuraties | C:\ProgramData\jupyter |
| Jupyter-Notebook voorbeelden basismap | c:\dsvm\notebooks en c:\users\<gebruikersnaam > \notebooks |
| Andere voorbeelden | c:\dsvm\samples |
| Anaconda, standaard: Python 3.6 | c:\Anaconda |
| Anaconda Python 2.7 omgeving | c:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (zelfstandige versie)-Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Standaardexemplaar R, Machine Learning Server (zelfstandige versie) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL-Machine Learning-Services in de database-instantie directory | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Azure Machine Learning Workbench, per gebruiker | %localappdata%\amlworkbench | 
| Diverse hulpprogramma 's | c:\dsvm\tools |

> [!NOTE]
> Op de Windows Server 2012-versie van de editie van DSVM- en Windows Server 2016 vóór maart 2018 is de standaardomgeving Anaconda Python 2.7. De secundaire omgeving wordt Python 3.5, dat zich bevindt in **c:\Anaconda\envs\py35**. 
> 
> 

## <a name="next-steps"></a>Volgende stappen

* De hulpprogramma's op de virtuele machine voor datatechnologie verkennen door het selecteren van de **Start** menu.
* Meer informatie over Azure Machine Learning-Services en Workbench door naar het product te [snelstartgidsen en zelfstudies pagina](../service/index.yml). 
* Navigeer naar **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** voor voorbeelden die gebruikmaken van de bibliotheek RevoScaleR in R die ondersteuning biedt voor gegevensanalyse op de grote schaal.  
* Lees het artikel [tien dingen die u in de Data science Virtual Machine doen kunt](http://aka.ms/dsvmtenthings).
* Informatie over het bouwen van analytische oplossingen voor end-to-end systematisch met behulp van de [Team Data Science Process](../team-data-science-process/index.yml).
* Ga naar de [Azure AI Gallery](http://gallery.cortanaintelligence.com) services voor machine learning en data analytics-voorbeelden die gebruikmaken van Azure Machine Learning en gerelateerde gegevens in Azure. We hebben ook een pictogram voor deze galerie beschikbaar op de **Start** menu en het bureaublad van de virtuele machine.

