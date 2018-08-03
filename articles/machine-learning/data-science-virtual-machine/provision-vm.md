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
ms.date: 09/10/2017
ms.author: gokuma
ms.openlocfilehash: b749d8a904bc40eba3346cc03d9274236380c80d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449755"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Inrichten van de Windows Data Science Virtual Machine van Azure
De Microsoft Data Science Virtual Machine is een installatiekopie van de virtuele machine (VM) Windows Azure vooraf zijn geïnstalleerd en geconfigureerd met verschillende populaire hulpprogramma's die vaak worden gebruikt voor gegevensanalyse en machine learning. De hulpprogramma's zijn:

* [Azure Machine Learning](../service/index.yml) Workbench
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition
* Anaconda Python-distributie
* Jupyter-notebook (met R, Python, PySpark-kernels)
* Visual Studio Community-editie
* Power BI Desktop
* SQL Server 2017 Developer Edition
* Standalone Spark-instantie voor lokale ontwikkeling en testen
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Machine learning en Data Analytics-hulpprogramma 's
  * Deep Learning Frameworks: een grote verscheidenheid aan AI-frameworks waaronder [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet, Keras zijn opgenomen op de virtuele machine.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine learning-systeem, zoals de online, hash, allreduce, kortingen, learning2search, actief, ondersteuning en interactieve training.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpprogramma voor snelle en nauwkeurige boosted structuur-implementatie.
  * [Rattle](http://rattle.togaware.com/) (de R analytische hulpprogramma om te leren eenvoudig): een hulpprogramma dat aan de slag met gegevensanalyse en machine learning in R eenvoudig maakt. Het bevat GUI-gebaseerde gegevens verkennen en modelleren met automatische generatie van R-code.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : een visuele data-gegevensanalyse en machine learning-software in Java.
  * [Apache Drill](https://drill.apache.org/): een schemavrije SQL Query-Engine voor Hadoop-, NoSQL- en opslag in de Cloud.  Biedt ondersteuning voor ODBC- en JDBC-interfaces om in te schakelen van een query uitvoeren op NoSQL en bestanden van standard BI-hulpprogramma's zoals Power BI, Excel, Tableau.
* Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure-services
* GIT, met inbegrip van Git Bash om te werken met broncodeopslagplaatsen met inbegrip van GitHub, Visual Studio Team Services en biedt verschillende populaire Linux-opdrachtregelprogramma's (met inbegrip van awk, sed, perl, grep, zoeken, wget, curl, enzovoort) beschikbaar zowel in de git bash en de opdracht prompt. 

Gegevenswetenschap omvat het doorlopen van een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
1. Het bouwen en testen van modellen
1. De modellen voor gebruik in intelligente toepassingen implementeren

Gegevenswetenschappers gebruiken diverse hulpprogramma's om deze taken te voltooien. Kan het zijn heel veel tijd in beslag op de juiste versies van de software en download en installeer vervolgens deze. De Microsoft Data Science Virtual Machine, kan deze last vereenvoudigen door te voorzien in een kant-en-klare-installatiekopie die kan worden ingericht in Azure met alle verschillende populaire hulpprogramma's vooraf geïnstalleerd en geconfigureerd. 

De Microsoft Data Science Virtual Machine jump-starts uw analyticsproject. Hiermee kunt u taken in verschillende talen, waaronder van R, Python, SQL en C#. Visual Studio biedt een IDE voor het ontwikkelen en testen van uw code die is eenvoudig te gebruiken. De Azure SDK opgenomen in de virtuele machine kunt u uw toepassingen met behulp van verschillende services op het cloudplatform van Microsoft. 

Er zijn geen kosten voor software voor deze data science VM-installatiekopie. U betaalt alleen voor het gebruik van Azure-kosten die afhankelijk zijn van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten te vinden in de sectie met prijzen in de [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) pagina. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de virtuele Machine voor Datatechnologie
Een [Ubuntu](dsvm-ubuntu-intro.md) installatiekopie is beschikbaar, ook met veel vergelijkbaar hulpprogramma's plus een aantal extra deep learning-frameworks. Een [CentOS](linux-dsvm-intro.md) installatiekopie is ook beschikbaar. We bieden ook een [edition van Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) van de virtuele machine voor datatechnologie echter een paar hulpprogramma's zijn alleen beschikbaar op de editie van Windows Server 2016.  In dit artikel geldt in alle andere gevallen ook voor de editie van Windows Server 2012.

## <a name="prerequisites"></a>Vereisten
Voordat u een Microsoft Data Science Virtual Machine maken kunt, moet u het volgende hebt:

* **Een Azure-abonnement**: als u wilt aanvragen, Zie [gratis proefversie van Azure ophalen](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Uw Microsoft Data Science-Machine maken
Volg deze stappen voor het maken van een exemplaar van de Microsoft Data Science Virtual Machine:

1. Navigeer naar de virtuele machine weergeven op [Azure-portal](https://portal.azure.com/#create/microsoft-ads.windows-data-science-vmwindows2016).
1. Selecteer de **maken** onder in de wizard een moeten worden genomen.![ Configureer-data-science-vm](./media/provision-vm/configure-data-science-virtual-machine.png)
1. De wizard die wordt gebruikt voor het maken van de Microsoft Data Science Virtual Machine vereist **invoer** voor elk van de **vier stappen** geïnventariseerd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:
   
   1. **Basisinstellingen**
      
      1. **Naam**: naam van uw data science-server die u maakt.
      1. **VM-schijftype**: kiezen tussen SSD of HDD. Kies voor NC_v1 GPU exemplaar (NVidia Tesla R80 gebaseerd), **HDD** als het schijftype. 
      1. **Gebruikersnaam**: Admin-account aanmeldings-id.
      1. **Wachtwoord**: wachtwoord voor beheerdersaccount.
      1. **Abonnement**: als u meer dan één abonnement hebt, selecteert u het een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht.
      1. **Resourcegroep**: U kunt een nieuwe groep maken of een bestaande groep gebruikt.
      1. **Locatie**: Selecteer het datacenter die het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk.
   1. **Grootte**: Selecteer een van de servertypen die voldoet aan de vereisten van het functionele en kostenbeperkingen. U kunt meer opties van VM-grootten opvragen 'Alles weergeven' selecteren.
   1. **Instellingen voor**:
      
      1. **Managed Disks gebruiken**: Kies beheerd als u wilt dat Azure voor het beheren van de schijven voor de virtuele machine.  Anders moet u een nieuwe of bestaande storage-account opgeven. 
      1. **Andere parameters**: meestal alleen u de standaardwaarden. Als u uitvoeren met behulp van niet-standaard waarden wilt, Beweeg de muisaanwijzer over de informatieve koppeling voor meer informatie over de specifieke velden.
    a. **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is en klik op **maken**. **Houd er rekening mee**: de virtuele machine heeft geen eventuele extra kosten buiten de rekenkracht voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 

> [!NOTE]
> De inrichting duurt ongeveer 10-20 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Toegang tot de Microsoft Data Science Virtual Machine
Nadat de virtuele machine is gemaakt, kunt u extern bureaublad in met behulp van de accountreferenties van de beheerder die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 

Als uw virtuele machine is gemaakt en ingericht, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op deze. Zijn er tegels van het menu start en pictogrammen op het bureaublad voor veel van de hulpprogramma's. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Microsoft Data Science Virtual Machine



### <a name="microsoft-ml-server-developer-edition"></a>Microsoft ML Server Developer Edition
Als u gebruiken van Microsoft enterprise-bibliotheken voor schaalbare R of Python voor uw analyses wilt, heeft de virtuele machine Microsoft ML Server Developer edition (voorheen bekend als Microsoft R Server) geïnstalleerd. Microsoft ML Server is een breed implementeerbare hoogwaardige analyseplatform beschikbaar voor zowel R als Python en schaalbare, commercieel ondersteunde en veilig is. Ondersteuning biedt voor tal van big data-statistieken, voorspellende modellen en machine learning-mogelijkheden, ML-Server biedt ondersteuning voor het volledige bereik van analytics – verkenning, analyse, visualisatie en modellering. Door en uitbreiden van open source R en Python, Microsoft ML Server is volledig compatibel met R / Python-scripts, functies en CRAN / pip / Conda-pakketten voor het analyseren van gegevens in zakelijk schalen. Het biedt ook de geheugenbeperkingen van Open Source R oplossing door parallelle en bloksgewijze gegevensverwerking toe te voegen. Hiermee kunt u analyses uitvoeren op gegevens veel groter is dan wat in het hoofdgeheugen past.  Visual Studio Community Edition opgenomen op de virtuele machine bevat de R-hulpprogramma's voor Visual Studio en Python tools voor Visual Studio-uitbreiding die voorzien in een IDE met volledige voor het werken met R of Python. We bieden ook andere IDE's ook zoals [RStudio](http://www.rstudio.com) en [PyCharm Community edition](https://www.jetbrains.com/pycharm/) op de virtuele machine. 

### <a name="python"></a>Python
Voor het ontwikkelen met behulp van Python, is distributie Anaconda Python 2.7 en 3.6 geïnstalleerd. Deze verdeling bevat de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten. U kunt Python Tools voor Visual Studio (PTVS) die is geïnstalleerd in de Visual Studio 2017 Community edition of een van de IDE's geleverd met Anaconda, zoals niet-actief of Spyder gebruiken. U kunt een van deze door te zoeken op de zoekbalk starten (**Win** + **S** sleutel).

> [!NOTE]
> Om te verwijzen de Python-Tools voor Visual Studio op Anaconda Python 2.7, die u wilt maken van aangepaste omgevingen voor elke versie. Deze omgeving paden instellen in de Visual Studio 2017 Community Edition, gaat u naar **extra** -> **Python Tools** -> **Python-omgevingen**en klik vervolgens op **+ aangepast**. 
> 
> 

Anaconda Python 3.6 onder C:\Anaconda is geïnstalleerd en onder c:\Anaconda\envs\python2 Anaconda Python 2.7 is geïnstalleerd. Zie [documentatie bij PTVS](/visualstudio/python/installing-python-interpreters.md) voor gedetailleerde stappen. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda distributie wordt geleverd met een Jupyter-notebook, een omgeving voor het delen van code en -analyse. Een Jupyter-notebook-server is vooraf geconfigureerd met Python 2.7, Python 3.x, PySpark, Julia en R-kernels. Er is een pictogram op het bureaublad met de naam 'Jupyter-Notebook' om te starten van de Jupyter-server en start de browser voor toegang tot de Notebook-server. 

We hebben verschillende voorbeeldnotitieblokken in Python en R. verpakt De Jupyter-notebooks laten zien hoe om te werken met Microsoft ML Server, SQL Server ML-Services (In-database analyses), Python, Microsoft Cognitive ToolKit, Tensorflow en andere Azure-technologieën, wanneer u toegang Jupyter tot. U kunt de koppeling naar de voorbeelden zien op de startpagina van de notebook nadat u zich bij de Jupyter-notebook met behulp van het wachtwoord die u hebt gemaakt in een eerdere stap verifieert. 

### <a name="visual-studio-2017-community-edition"></a>Visual Studio 2017 Community edition
Visual Studio Community edition is geïnstalleerd op de virtuele machine. Het is een gratis versie van de populaire IDE van Microsoft die u voor evaluatiedoeleinden gebruikt en voor kleine teams gebruiken kunt. U kunt de licentievoorwaarden bekijken [hier](https://www.visualstudio.com/support/legal/mt171547).  Open Visual Studio door te dubbelklikken op het pictogram op het bureaublad of de **Start** menu. U kunt ook zoeken voor programma's met **Win** + **S** en 'Visual Studio' in te voeren. Zodra u er projecten in talen zoals C#, Python, R, node.js maken kunt. Invoegtoepassingen worden ook geïnstalleerd waarmee u gemakkelijk om te werken met Azure-services zoals Azure Data Catalog, Azure HDInsight (Hadoop, Spark) en Azure Data Lake. Er is nu ook een invoegtoepassing met de naam ```Visual Studio Tools for AI``` die naadloos kan worden geïntegreerd met Azure Machine Learning en helpt u snel build AI-toepassingen. 

> [!NOTE]
> Krijgt u mogelijk een bericht waarin staat dat uw evaluatieperiode is verlopen. Voer de referenties van uw Microsoft-account of maak een nieuwe gratis account voor toegang tot de Community Edition van Visual Studio. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition
Een developer-versie van SQL Server 2017 met ML-Services voor het uitvoeren van analyses in de database is beschikbaar op de virtuele machine in R of Python. ML-Services biedt een platform voor het ontwikkelen en implementeren van intelligente toepassingen. Kunt u de uitgebreide en krachtige deze talen en het aantal pakketten van de community om te maken van modellen en voorspellingen voor uw SQL Server-gegevens genereren. Omdat de R- en Python-taal in de SQL Server ML-Services (In-database) is geïntegreerd, kunt u analytics dicht bij de gegevens bewaren. Dit elimineert de kosten en beveiligingsrisico's die zijn gekoppeld aan de verplaatsing van gegevens.

> [!NOTE]
> De developer-editie van SQL Server kan alleen worden gebruikt voor ontwikkelings- en testdoeleinden. U moet een licentie voor het in productie uitvoeren. 
> 
> 

U kunt toegang tot de SQL-server door te starten **SQL Server Management Studio**. De naam van uw virtuele machine wordt ingevuld als de servernaam. Windows-verificatie wanneer u bent aangemeld als de beheerder van Windows gebruiken. Wanneer u SQL Server Management Studio kunt u maken van andere gebruikers, databases maken, gegevens importeren en uitvoeren van SQL-query's. 

Als u wilt In-database analyses met SQL ML-Services hebt ingeschakeld, kunt u de volgende opdracht uitvoeren als een eenmalige bewerking in SQL Server management studio na het aanmelden als beheerder van de server. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Verschillende Azure-hulpprogramma's zijn geïnstalleerd op de virtuele machine:

* Er is een snelkoppeling op het bureaublad voor toegang tot de Azure SDK-documentatie. 
* **AzCopy**: gebruikt voor het verplaatsen van gegevens in en uit uw Microsoft Azure Storage-Account. Gebruik Typ **Azcopy** achter de opdrachtprompt om te zien van het gebruik. 
* **Microsoft Azure Storage Explorer**: gebruikt voor het bladeren door de objecten die u hebt opgeslagen in uw Azure Storage-Account en de overdracht van gegevens van en naar Azure storage. U kunt typen **Opslagverkenner** in zoeken of deze in het menu Start van Windows voor toegang tot dit hulpprogramma vinden. 
* **Adlcopy**: gebruikt voor het verplaatsen van gegevens naar Azure Data Lake. Gebruik Typ **adlcopy** in een opdrachtprompt. 
* **dtui**: gebruikt om gegevens te verplaatsen naar en van Azure Cosmos DB, een NoSQL-database in de cloud. Type **dtui** op de opdrachtprompt. 
* **Azure Data Factory Integration Runtime**: kunt verplaatsen van gegevens tussen on-premises gegevensbronnen en de cloud. Het wordt gebruikt in hulpprogramma's zoals Azure Data Factory. 
* **Microsoft Azure Powershell**: een hulpprogramma dat wordt gebruikt voor het beheren van uw Azure-resources in de Powershell scripttaal is ook geïnstalleerd op uw virtuele machine. 

### <a name="power-bi"></a>Power BI
Waarmee u dashboards en geweldige visualisaties, de **Power BI Desktop** is geïnstalleerd. Met dit hulpprogramma kunt gegevens ophalen uit verschillende bronnen, uw dashboards en rapporten maken en deze publiceren naar de cloud. Zie voor meer informatie, de [Power BI](http://powerbi.microsoft.com) site. Power BI desktop vindt u in het menu Start. 

> [!NOTE]
> U moet een Office 365-account voor toegang tot Power BI. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning Workbench is een bureaubladtoepassing en opdrachtregelinterface. De Workbench heeft ingebouwde gegevensvoorbereiding waarmee u de stappen voor gegevensvoorbereiding leert terwijl u ze uitvoert. Het biedt ook projectbeheer, uitvoeringsgeschiedenis en integratie van notitieblokken ter ondersteuning van uw productiviteit. U kunt profiteren van de beste open source-frameworks, waaronder TensorFlow, Cognitive Toolkit, Spark ML en scikit-meer informatie over het ontwikkelen van uw modellen. We bieden een pictogram voor het installeren van de Azure Machine Learning workbench in de map van de individuele gebruiker % LOCALAPPDATA % op het bureaublad op de DSVM. Elke gebruiker die moet worden gebruikt de Workbench moet doen van een eenmalige bewerking van het te dubbelklikken op de ```AzureML Workbench Setup``` pictogram op het bureaublad om hun exemplaar van de Workbench te installeren. Azure Machine Learning is ook maakt en gebruikt een per gebruiker Python-omgeving die in de map % LOCALAPPDATA%\amlworkbench\python wordt opgehaald.

## <a name="additional-microsoft-development-tools"></a>Aanvullende Microsoft-ontwikkelprogramma 's
De [ **Microsoft Web Platform Installer** ](https://www.microsoft.com/web/downloads/platform.aspx) kan worden gebruikt om te detecteren en andere Microsoft-ontwikkeltools downloaden. Er is ook een snelkoppeling naar het hulpprogramma dat is opgegeven op het bureaublad van Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de virtuele machine
| Item | Directory |
| --- | --- |
| Jupyter-notebook server-configuraties |C:\ProgramData\jupyter |
| Jupyter-Notebook voorbeelden basismap |c:\dsvm\notebooks en c:\users\<gebruikersnaam > \notebooks|
| Andere voorbeelden |c:\dsvm\samples |
| Anaconda (standaard: Python 3.6) |c:\Anaconda |
| Anaconda Python 2.7 omgeving |c:\Anaconda\envs\python2 |
| Microsoft ML Server zelfstandige Python  | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Standaardexemplaar R (ML Server zelfstandig) |C:\Program Files\Microsoft\ML Server\R_SERVER |
| ML-Services voor SQL database-exemplaar directory |C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Azure Machine Learning Workbench (per gebruiker) | %localappdata%\amlworkbench | 
| Diverse hulpprogramma 's |c:\dsvm\tools |

> [!NOTE]
> In Windows Server 2012-editie van de editie van DSVM- en Windows Server 2016 vóór maart 2018 is de standaardomgeving Anaconda Python 2.7. De secundaire omgeving wordt Python 3.5 dat zich bevindt in c:\Anaconda\envs\py35. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele volgende stappen om door te gaan uw leren en te verkennen. 

* Verken de verschillende hulpprogramma's voor data science in de data science VM door te klikken op het startmenu en controleren van de hulpprogramma's die worden vermeld in het menu.
* Meer informatie over Azure Machine Learning-Services en Workbench door naar het product te [snelstartgidsen en zelfstudies pagina](../service/index.yml). 
* Navigeer naar **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** voor voorbeelden met behulp van de bibliotheek RevoScaleR in R die ondersteuning biedt voor gegevensanalyse op grote schaal.  
* Lees het artikel: [10 dingen die u op de virtuele Machine voor datatechnologie doen kunt](http://aka.ms/dsvmtenthings)
* Informatie over het bouwen van end-to-end analytische oplossingen systematisch met behulp van de [Team Data Science Process](../team-data-science-process/index.yml).
* Ga naar de [Azure AI Gallery](http://gallery.cortanaintelligence.com) services voor machine learning en data analytics-voorbeelden die gebruikmaken van Azure Machine learning en gerelateerde gegevens in Azure. We bieden ook een pictogram op de **Start** menu en klik op het bureaublad van de virtuele machine aan deze galerie.

