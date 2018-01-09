---
title: Inrichten van Windows gegevens wetenschappelijke virtuele Machine in Azure | Microsoft Docs
description: Configureren en een virtuele Machine van de gegevens wetenschappelijke in Azure maken voor analytics en machine learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: bradsev
ms.openlocfilehash: d71d8e44d0327515ed302c5c902ce87587e36c7d
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Inrichten van Windows gegevens wetenschappelijke virtuele Machine in Azure
De Microsoft Data wetenschappelijke virtuele Machine is een Windows Azure virtuele machine (VM) vooraf is geïnstalleerd en geconfigureerd met verschillende populaire hulpprogramma's die vaak worden gebruikt voor gegevensanalyse en machine learning. De hulpprogramma's zijn:

* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning-services/) Workbench
* [Microsoft ML Server](https://docs.microsoft.com/machine-learning-server/index) ontwikkelaarsversie
* Anaconda Python-distributie
* Jupyter-notebook (met R, Python, PySpark kernels)
* Visual Studio Community Edition
* Power BI Desktop
* SQL Server 2017 Developer Edition
* Standalone Spark-exemplaar voor lokale ontwikkeling en testen
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Machine learning en hulpprogramma's voor gegevensanalyse
  * Grondige Learning Frameworks: een uitgebreide reeks frameworks, met inbegrip AI [Microsoft cognitieve Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet, Keras zijn opgenomen op de virtuele machine.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snel machine learning-technieken zoals online, hash, allreduce, kortingen, learning2search, actief, ondersteunende system en interactieve daarvan te leren.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpprogramma dat snelle en nauwkeurige gestimuleerd structuur-implementatie.
  * [Rattle](http://rattle.togaware.com/) (de R analytische hulpprogramma voor meer informatie over eenvoudig): een hulpprogramma dat aan de slag met data analytics en machine learning in R eenvoudig met een GUI-gebaseerde gegevensverkenning en modellering met automatische R codegeneratie maakt.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : een visual gegevensanalyse en machine learning-software in Java.
  * [Apache inzoomen](https://drill.apache.org/): een schemavrije SQL Query-Engine voor Hadoop, NoSQL- en Cloud-opslag.  Biedt ondersteuning voor ODBC- en JDBC-interfaces om in te schakelen query NoSQL- en bestanden van de standaard BI-tools zoals Power BI, Excel, Tableau.
* Bibliotheken in R- en Python voor gebruik in Azure Machine Learning en andere Azure-services
* GIT, met inbegrip van Git Bash werken met broncodeopslagplaatsen, met inbegrip van GitHub, Visual Studio Team Services
* Windows-poorten van verschillende populaire Linux opdrachtregelprogramma's (inclusief awk, ype, perl, grep, zoeken, wget, curl enzovoort) toegankelijk zijn via de opdrachtprompt. 

Tijdens het doorzoeken van wetenschappelijke gegevens omvat sequentieel op een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
2. Maken en testen van modellen
3. De modellen voor verbruik in intelligente toepassingen implementeren

Gegevenswetenschappers gebruik tal van hulpprogramma's om deze taken te voltooien. Het kan erg tijd in beslag nemen om te zoeken naar de juiste versies van de software, en vervolgens downloaden en installeren ze zijn. De Microsoft Data wetenschappelijke virtuele Machine kunt deze last vereenvoudigen doordat een kant-en-klare-installatiekopie die kan worden ingericht op Azure met alle verschillende populaire hulpmiddelen vooraf is geïnstalleerd en geconfigureerd. 

De virtuele Machine voor Microsoft Data wetenschappelijke jump-starts uw project analytics. Hiermee kunt u om te werken op taken in verschillende talen, waaronder R, Python, SQL en C#. Visual Studio biedt een IDE voor het ontwikkelen en testen van uw code die eenvoudig te gebruiken. De Azure SDK opgenomen in de virtuele machine kunt u uw toepassingen bouwen met verschillende services op Microsoft cloud-platform. 

Er zijn geen kosten voor software voor deze gegevens wetenschappelijke VM-installatiekopie. U betaalt alleen voor de Azure gebruikskosten welke die afhankelijk zijn van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten vindt u in de sectie Pricing details op de [gegevens wetenschappelijke virtuele Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) pagina. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere versies van de gegevens wetenschappelijke virtuele Machine
Een [Ubuntu](dsvm-ubuntu-intro.md) installatiekopie is beschikbaar, evenals met veel vergelijkbare hulpmiddelen, plus een paar extra deep frameworks leren. Een [CentOS](linux-dsvm-intro.md) installatiekopie is ook beschikbaar. We bieden ook een [editie van Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) van de virtuele machine van de gegevens wetenschappelijke echter een aantal hulpprogramma's zijn alleen beschikbaar voor de editie Windows Server 2016.  Anders in dit artikel wordt ook toegepast op de editie Windows Server 2012.

## <a name="prerequisites"></a>Vereisten
Voordat u een Microsoft Data wetenschappelijke virtuele Machine maken kunt, moet u het volgende hebben:

* **Een Azure-abonnement**: Zie voor een [gratis proefversie van Azure ophalen](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Uw Microsoft Data wetenschappelijke virtuele Machine maken
Hier volgen de stappen voor het maken van een exemplaar van de Microsoft wetenschappelijke virtuele Machine gegevens:

1. Navigeer naar de virtuele machine op aanbieding [Azure-portal](https://portal.azure.com/#create/microsoft-ads.windows-data-science-vmwindows2016).
2. Selecteer de **maken** knop onderaan in een wizard moeten worden genomen.![ Configureer-gegevens-wetenschappelijke-vm](./media/provision-vm/configure-data-science-virtual-machine.png)
3. De wizard gebruikt voor het maken van de Microsoft Data wetenschappelijke virtuele Machine vereist **invoer** voor elk van de **bestaat uit vier stappen** opgesomd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer voor het configureren van deze stappen:
   
   1. **Basisinstellingen**
      
      1. **Naam**: naam van uw gegevens wetenschap-server die u maakt.
      2. **VM-schijftype**: Kies tussen SSD of harde schijf. Kies voor een GPU (NC-serie), **HDD** als het schijftype. 
      3. **Gebruikersnaam**: Admin-account aanmeldings-id.
      4. **Wachtwoord**: wachtwoord van de beheerder.
      5. **Abonnement**: als u meer dan één abonnement hebt, selecteert u een op waarop de machine is gemaakt en kosten in rekening gebracht.
      6. **Resourcegroep**: U kunt een nieuwe maken of een bestaande groep.
      7. **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is doorgaans het datacenter die de meeste van uw gegevens of heeft de dichtstbijzijnde fysieke voor snelste toegang tot het netwerk.
   2. **De grootte van**: Selecteer een van de servertypen die voldoet aan uw functionele vereisten en kostenbeperkingen. U kunt meer mogelijkheden van VM-formaten ophalen door 'Alles weergeven' selecteren.
   3. **Instellingen**:
      
      1. **Schijven beheerd**: Kies beheerd als u wilt dat Azure voor het beheren van de schijven voor de virtuele machine.  Anders moet u een nieuwe of exitsting storage-account opgeven. 
      2. **Andere parameters**: meestal zojuist hebt u de standaardwaarden. U kunt de muisaanwijzer op de informatief koppeling voor meer informatie over de specifieke velden voor het geval wilt u het gebruik van niet-standaard waarden.
   4. **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is en klik op **maken**. **Opmerking**: de VM beschikt niet over eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 

> [!NOTE]
> De inrichting duren ongeveer 10-20 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Toegang tot de Microsoft Data wetenschappelijke virtuele Machine
Nadat de virtuele machine is gemaakt, kunt u extern bureaublad in met behulp van de referenties van het Administrator-account die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 

Als uw virtuele machine is gemaakt en ingericht, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op deze. Er zijn start menu tegels en bureaubladpictogrammen voor veel van de hulpprogramma's. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Microsoft Data wetenschappelijke virtuele Machine

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning-Workbench is een bureaubladtoepassing en opdrachtregelinterface. De Workbench heeft ingebouwde gegevens voorbereiden die uw gegevens voorbereidende stappen leert u om ze uit te voeren. Het bevat ook projectbeheer, geschiedenis en laptop-integratie met uw productiviteit vergroot uitvoeren. U kunt profiteren van de beste open-source frameworks, met inbegrip van TensorFlow, cognitieve Toolkit, Spark ML en scikit-informatie over het ontwikkelen van uw modellen. Op de DSVM bieden we een bureaubladpictogram (InstallAMLFromLocal) uitpakken van de Azure Machine Learning-workbench lokaal in de map voor elke gebruiker % LOCALAPPDATA %. Elke gebruiker die moet worden gebruikt de Workbench moet doen een actie van dubbelklikken op het bureaubladpictogram InstallAMLFromLocal voor het installeren van hun exemplaar van de Workbench tijd. Azure Machine Learning ook maakt en gebruikt een per gebruiker Python-omgeving die in de map % LOCALAPPDATA%\amlworkbench\python wordt opgehaald.

### <a name="microsoft-ml-server-developer-edition"></a>Microsoft ML Server Developer Edition
Als u gebruiken van Microsoft enterprise-bibliotheken voor schaalbare R- of Python voor uw analyses wilt, heeft de virtuele machine Microsoft ML Server Developer edition (voorheen bekend als Microsoft R Server) geïnstalleerd. Microsoft ML Server is een grotendeels implementeerbare bedrijfsniveau analytics platform beschikbaar voor R- en Python en schaalbare, commercieel ondersteunde en veilig is. Ondersteuning biedt voor tal van big data-statistieken, voorspellende modellen en machine learning-mogelijkheden, ondersteunt ML-Server het volledige bereik van analytics – exploratie, analyse, visualisatie en modellering. Door en uitbreiden van de open source R- en Python, Microsoft ML Server is volledig compatibel met R / Python-scripts, functies en CRAN / pip / Conda-pakketten voor het analyseren van gegevens op enterprise schalen. Biedt ook een oplossing de beperkingen in het geheugen van de Open Source R door parallelle en gesegmenteerde verwerking van gegevens toe te voegen. Hiermee kunt u analyses uitvoeren op gegevens veel groter dan wat in het hoofdgeheugen past.  Visual Studio Community Edition opgenomen op de virtuele machine bevat de R-Tools voor Visual Studio en Python-tools voor Visual Studio-extensie die een volledige IDE biedt voor het werken met R- of Python. We bieden ook andere IDE ook zoals [RStudio](http://www.rstudio.com) en [PyCharm Community edition](https://www.jetbrains.com/pycharm/) op de virtuele machine. 

### <a name="python"></a>Python
Voor de ontwikkeling met behulp van Python, is distributie Anaconda Python 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de base Python samen met ongeveer 300 van de meest populaire math, engineering en gegevens analytics pakketten. U kunt Python-Tools voor Visual Studio (PTVS) die is geïnstalleerd in de Visual Studio 2015-Community-versie of een van de IDE gebundeld met Anaconda zoals niet-actief of Spyder gebruiken. U kunt een van deze door te zoeken op de zoekbalk starten (**Win** + **S** sleutel).

> [!NOTE]
> Om te verwijzen de Python-Tools voor Visual Studio op Anaconda Python 2.7 en 3.5, moet u aangepaste omgevingen voor elke versie te maken. Navigeer naar deze omgeving paden in de Visual Studio 2015 Community Edition stelt **extra** -> **Python Tools** -> **Python-omgevingen**en klik vervolgens op **+ aangepaste**. 
> 
> 

Anaconda Python 2.7 onder C:\Anaconda is geïnstalleerd en Anaconda Python 3.5 onder c:\Anaconda\envs\py35 is geïnstalleerd. Zie [documentatie bij PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) voor gedetailleerde stappen. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda-verdeling wordt ook geleverd met een Jupyter-notebook een omgeving voor het delen van code en -analyse. Een Jupyter-notebook-server is vooraf geconfigureerd met Python 2.7, Python 3.5, PySpark, Julia en R kernels. Er is een pictogram op het bureaublad met de naam 'Jupyter-Notebook' om te starten van de Jupyter-server en start de browser voor toegang tot de Notebook-server. 

> [!NOTE]
> Doorgaan als u certificaatwaarschuwingen. 
> 
> 

We hebben verschillende voorbeeldquery notitieblokken in Python en in R. verpakt De Jupyter-notebooks laten zien hoe werken met Microsoft ML Server, SQL Server-Services voor ML (In de database analytics), Python, Microsoft cognitieve ToolKit, Tensorflow en andere technologieën voor Azure wanneer u toegang Jupyter tot. Nadat u geverifieerd bij de Jupyter-notebook met het wachtwoord die u in een eerdere stap hebt gemaakt, kunt u de koppeling naar de voorbeelden bekijken op de startpagina van de notebook. 

### <a name="visual-studio-2017-community-edition"></a>Visual Studio 2017 Community edition
Visual Studio Community edition is geïnstalleerd op de virtuele machine. Het is een gratis versie van de populaire IDE van Microsoft die u voor evaluatiedoeleinden gebruikt en voor kleine teams gebruiken kunt. U kunt de licentievoorwaarden uitchecken [hier](https://www.visualstudio.com/support/legal/mt171547).  Visual Studio niet openen door te dubbelklikken op het bureaubladpictogram of de **Start** menu. U kunt ook zoeken naar programma's met **Win** + **S** en 'Visual Studio' in te voeren. Zodra er kunt u projecten in talen zoals C#, Python, R, node.js. Invoegtoepassingen zijn ook geïnstalleerd die werken met Azure-services zoals Azure Data Catalog, Azure HDInsight (Hadoop, Spark) en Azure Data Lake te vergemakkelijken. 

> [!NOTE]
> Mogelijk dat u een bericht waarin staat dat uw evaluatieperiode is verlopen. Voer de referenties van uw Microsoft-account of maak een nieuwe gratis account toegang krijgen tot de Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>2017 ontwikkelaarsversie van SQL Server
Een developer-versie van SQL Server 2017 met ML-Services worden uitgevoerd in de database analytics is beschikbaar op de virtuele machine in R- of Python. ML-Services bieden een platform voor het ontwikkelen en implementeren van intelligent toepassingen. U kunt uitgebreide en krachtige deze talen en de veel pakketten van de community modellen maken en het genereren van voorspellingen voor uw SQL Server-gegevens. Omdat ML-Services (In database) het R- en Python taal in de SQL-Server integreert, kunt u analytics dicht bij de gegevens behouden. Hierdoor is de kosten en beveiligingsrisico's met de verplaatsing van gegevens.

> [!NOTE]
> De ontwikkelaarsversie van SQL Server kan alleen worden gebruikt voor ontwikkeling en testdoeleinden. U moet een licentie uit te voeren in de productieomgeving. 
> 
> 

U hebt toegang tot de SQL-server starten **SQL Server Management Studio**. De naam van uw VM is ingevuld als de naam van de Server. Windows-verificatie gebruiken wanneer u bent aangemeld als de beheerder in Windows. Wanneer u naar SQL Server Management Studio kunt u andere gebruikers te maken, databases maken, gegevens importeren en SQL-query's uitvoeren. 

Als u wilt inschakelen In database-analyses met SQL ML Services, kunt u de volgende opdracht uitvoeren als een actie in SQL Server management studio na het aanmelden als beheerder van de tijd. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Verschillende Azure-hulpprogramma's worden geïnstalleerd op de virtuele machine:

* Er is een snelkoppeling op het bureaublad voor toegang tot de Azure SDK-documentatie. 
* **AzCopy**: gebruikt voor het verplaatsen van gegevens binnen en buiten uw Microsoft Azure Storage-Account. Gebruik Typ **Azcopy** achter de opdrachtprompt om te zien van het gebruik. 
* **Microsoft Azure Storage Explorer**: gebruikt voor het bladeren door de objecten die u hebt opgeslagen in uw Azure Storage-Account en de overdracht gegevens van en naar Azure storage. U kunt typen **Opslagverkenner** in zoeken of deze in het menu Start van Windows voor toegang tot dit hulpprogramma vinden. 
* **Adlcopy**: gebruikt om gegevens te verplaatsen naar Azure Data Lake. Gebruik Typ **adlcopy** in een opdrachtprompt. 
* **dtui**: gebruikt om gegevens te verplaatsen naar en van Azure Cosmos DB, een NoSQL-database in de cloud. Type **dtui** op de opdrachtprompt. 
* **Azure Data Factory-integratie Runtime**: kunt verplaatsing van gegevens tussen on-premises gegevensbronnen en cloud. Binnen hulpmiddelen zoals Azure Data Factory wordt gebruikt. 
* **Microsoft Azure Powershell**: een hulpprogramma dat wordt gebruikt voor het beheren van uw Azure-resources in de Powershell scripttaal is ook geïnstalleerd op de virtuele machine. 

### <a name="power-bi"></a>Power BI
Voor hulp bij het bouwen van dashboards en geweldige visualisaties de **Power BI Desktop** is geïnstalleerd. Met dit hulpprogramma kunt pull gegevens uit verschillende bronnen uw dashboards en rapporten, maken en deze publiceren in de cloud. Zie voor meer informatie, de [Power BI](http://powerbi.microsoft.com) site. Power BI desktop vindt u in het menu Start. 

> [!NOTE]
> U moet een Office 365-account voor toegang tot Power BI. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Aanvullende Microsoft-ontwikkelprogramma 's
De [ **Microsoft Web Platform Installer** ](https://www.microsoft.com/web/downloads/platform.aspx) kan worden gebruikt om te detecteren en andere Microsoft-ontwikkelprogramma's downloaden. Er is ook een snelkoppeling naar het hulpprogramma dat op het bureaublad van Microsoft Data wetenschappelijke virtuele Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de virtuele machine
| Item | Directory |
| --- | --- |
| Jupyter-notebook serverconfiguraties |C:\ProgramData\jupyter |
| Basismap van Jupyter-Notebook-voorbeelden |c:\dsvm\notebooks |
| Andere voorbeelden |c:\dsvm\samples |
| Anaconda (standaard: Python 2.7) |c:\Anaconda |
| Anaconda 3.5 Python-omgeving |c:\Anaconda\envs\py35 |
| Microsoft ML Server zelfstandige Python  | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Standaard-R-sessie (ML Server zelfstandig) |C:\Program Files\Microsoft\ML Server\R_SERVER |
| ML-Services voor SQL-database-exemplaar directory |C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER |
| Azure Machine Learning-Workbench (per gebruiker) | %localappdata%\amlworkbench | 
| Diverse hulpprogramma 's |c:\dsvm\tools |

> [!NOTE]
> Exemplaren van de Microsoft Data wetenschappelijke virtuele Machine gemaakt voordat 1.5.0 (vóór 3 September 2016) gebruikt een iets andere mapstructuur dan is opgegeven in de voorgaande tabel. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele volgende stappen om door te gaan uw leren en te verkennen. 

* Verken de verschillende beschikbare hulpprogramma wetenschappelijke gegevens op de virtuele machine voor gegevenswetenschap door te klikken op het startmenu en controleren van de hulpprogramma's die worden vermeld in het menu.
* Meer informatie over Azure Machine Learning-Services en -Workbench door naar het product te [pagina Quick Start- en zelfstudies](https://docs.microsoft.com/azure/machine-learning/preview/). 
* Navigeer naar **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** voor voorbeelden weergegeven met behulp van de bibliotheek RevoScaleR in R die ondersteuning biedt voor gegevensanalyse die op grote schaal enterprise.  
* Lees het artikel: [10 dingen die u op de virtuele Machine voor gegevenswetenschap doen kunt](http://aka.ms/dsvmtenthings)
* Informatie over het bouwen van end-to-end analyseoplossingen systematischer met behulp van de [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Ga naar de [AI-galerie van Azure](http://gallery.cortanaintelligence.com) voor machine learning en analytics voorbeelden die gebruikmaken van Azure Machine learning en verwante services in Azure. We hebben ook een pictogram opgegeven op de **Start** menu en op het bureaublad van de virtuele machine aan deze galerie.

