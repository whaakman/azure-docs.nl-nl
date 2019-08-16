---
title: Maken van een Windows Data Science Virtual Machine
titleSuffix: Azure
description: Configureren en een Data Science Virtual Machine in Azure maken voor analyse en machine learning.
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: quickstart
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 488dc7db01bd865268e143b68cdaccd989010912
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534930"
---
# <a name="provision-a-windows-data-science-virtual-machine-on-azure"></a>Een Windows-Data Science Virtual Machine inrichten in azure

Micro soft Windows Data Science Virtual Machine (DSVM) is een installatie kopie van een virtuele machine (VM) van Windows Server 2016 in azure die vooraf is geïnstalleerd en geconfigureerd met hulpprogram ma's voor gegevens analyse en machine learning.

## <a name="included-data-science-tools"></a>Inbegrepen data Science-hulpprogram ma's

De volgende hulpprogram ma's zijn opgenomen in een DSVM:

* [Azure machine learning-service](../service/index.yml) Python-SDK
* [Microsoft machine learning server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition
* Anaconda Python-distributie
* Jupyter Notebook met R-, python-en PySpark-kernels
* Micro soft Visual Studio Community
* Microsoft Power BI Desktop
* Microsoft SQL Server 2017 Developer Edition
* Een zelfstandig Apache Spark exemplaar voor lokale ontwikkeling en tests
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Machine learning en data analytics-hulpprogramma's:
  * Uitgebreide leer raamwerken: een uitgebreide set AI-Frameworks is beschikbaar op de VM. [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [tensor flow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet en Keras
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) -een snel machine learning systeem dat technieken zoals online hashing, allreduce, reducties, learning2search en actief en interactief onderwijs ondersteunt
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/) : een hulp programma dat snelle en nauw keurige structuur implementatie biedt
  * [Rattle](https://togaware.com/rattle/) : het R-analytische hulp programma waarmee u aan de slag gaat met gegevens analyse en machine learning in R. Het bevat de mogelijkheid om gegevens op basis van een gebruikers interface te verkennen en te model leren met automatische R-code generatie.
  * [Fridge](https://www.cs.waikato.ac.nz/ml/weka/) -Visual Data mining en machine learning software in Java
  * [Apache Drill](https://drill.apache.org/) -een SQL-query engine zonder schema voor Apache Hadoop-, NoSQL-en Cloud opslag. ODBC- en JDBC-interfaces worden ondersteund voor het uitvoeren van query's NoSQL en bestanden van standard BI-hulpprogramma's zoals Power BI, Microsoft Excel en Tableau.
* Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure-services
* GIT, met inbegrip van Git Bash, om te werken met broncodeopslagplaatsen, zoals GitHub en DevOps met Azure. GIT biedt verschillende populaire Linux-opdrachtregelprogramma's die toegankelijk zijn zowel op Git Bash en een opdrachtprompt. Voorbeelden zijn awk, sed, perl, grep, zoeken, wget en curl.
* Ontwikkelingsprogram ma's en editors (RStudio, PyCharm)

### <a name="about-data-science"></a>Over gegevens wetenschap

Wetenschappelijke gegevens omvat het doorlopen van een reeks taken:

1. Gegevens zoeken, laden en voorverwerken
1. Modellen bouwen en testen
1. De modellen implementeren voor gebruik in intelligente toepassingen

Gegevenswetenschappers gebruiken verschillende hulpprogramma's voor deze taken. Kan het zijn tijdrovend zijn om de juiste versies van software en download en installeer vervolgens deze. De DSVM bespaart tijd door een kant-en-klare installatie kopie op te geven die kan worden ingericht in azure met verschillende populaire hulpprogram ma's die vooraf zijn geïnstalleerd en geconfigureerd.

Met DSVM jump-start u uw analyse project. U kunt werken aan taken in verschillende talen, waaronder van R, Python, SQL en C#. Visual Studio biedt een eenvoudig te gebruiken geïntegreerde ontwikkelomgeving (IDE) voor het ontwikkelen en testen van uw code. De Azure SDK is opgenomen in de virtuele machine, zodat u uw toepassingen kunt bouwen met behulp van verschillende services op het Cloud platform van micro soft.

Er zijn geen kosten voor software voor deze data science VM-installatiekopie. U betaalt alleen de kosten van het gebruik van Azure. Ze afhankelijk zijn van de grootte van de virtuele machine die u inricht. Meer informatie over de compute-kosten zijn de **prijsinformatie** sectie op de [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) pagina.

### <a name="other-dsvm-versions"></a>Andere DSVM-versies

* Een [Ubuntu](dsvm-ubuntu-intro.md) installatiekopie. Het bevat veel hulpprogramma's die vergelijkbaar is met de DSVM plus een aantal extra deep learning-frameworks.
* Een [Linux CentOS](linux-dsvm-intro.md) installatiekopie.
* De [edition van Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) van de Data Science Virtual Machine. Een paar hulpprogramma's zijn alleen beschikbaar op de editie van Windows Server 2016. In dit artikel geldt in alle andere gevallen ook voor de editie van Windows Server 2012.

## <a name="prerequisite"></a>Vereiste

Voor het maken van een Microsoft Data Science Virtual Machine, moet u een Azure-abonnement hebben. Zie [Gratis proefversie van Azure ophalen](https://azure.com/free).

## <a name="create-your-dsvm"></a>Uw DSVM maken

Een DSVM-exemplaar maken:

1. Ga naar de vermelding van de virtuele machine op de [Azure Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). U wordt mogelijk gevraagd om u aan te melden bij uw Azure-account als u zich nog niet hebt aangemeld.
1. Selecteer de knop **maken** onderaan.

   ![Configureer-data-science-vm](./media/provision-vm/configure-data-science-virtual-machine.png)

1. U moet de volgende informatie invoeren voor het configureren van elk van de stappen die worden weer gegeven in het rechterdeel venster van de scherm opname:

   1. **Grondbeginselen van**:
      * **Naam**: de naam van de DSVM
      * **VM-schijf type**: **SSD** of **HDD**. Kies voor een NC_v1 GPU-exemplaar, zoals NVidia Tesla R80 op basis van **HDD** als het schijftype.
      * **Gebruikers naam**: de account-id van de beheerder
      * **Wacht woord**: het wacht woord voor het beheerders account
      * **Abonnement**: Als u meer dan één abonnement hebt, selecteert u een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht.
      * **Resourcegroep**. U kunt een nieuwe groep maken of een bestaande groep gebruikt.
      * **Locatie**. Selecteer het datacenter die het meest geschikt is. Voor de snelste toegang tot het netwerk is het datacenter dat de meeste van uw gegevens of zich het dichtst bij uw fysieke locatie.
   1. **Grootte**: Selecteer een van de servertypen die aansluit bij uw functionele vereisten en kostenbeperkingen. Voor meer opties van VM-grootten, selecteer **Alles weergeven**.
   1. **Instellingen voor**:  
      * **Beheerde schijven gebruiken**. Kies **beheerde** als u wilt dat Azure voor het beheren van de schijven voor de virtuele machine. Als dat niet het geval is, moet u een nieuwe of bestaande storage-account opgeven.  
      * **Andere parameters**. U kunt de standaardwaarden gebruiken. Als u niet-standaard waarden wilt gebruiken, houdt u de muis aanwijzer boven de koppeling voor informatie over de specifieke velden.
   1. **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is. Selecteer **Maken**.

> [!NOTE]
> * De virtuele machine heeft geen extra kosten als gevolg van de reken kosten voor de server grootte die u hebt gekozen in de stap **grootte** .
> * De inrichting duurt ongeveer 10 tot 20 minuten. U kunt de status van uw virtuele machine weer geven op de Azure Portal.

## <a name="how-to-access-the-dsvm"></a>Toegang tot de DSVM

Nadat de virtuele machine is gemaakt en ingericht, kunt u extern bureaublad in deze met behulp van de accountreferenties van de beheerder die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. U bent klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Veel van de hulpprogram ma's zijn toegankelijk via tegels en bureaublad pictogrammen in het start menu.

U kunt ook een Data Science VM aan Azure Notebooks koppelen om Jupyter-notebooks op de virtuele machine uit te voeren en de beperkingen van de gratis servicelaag over te slaan. Zie notebooks projects [-Compute-laag beheren en configureren](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier)voor meer informatie.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Microsoft Data Science Virtual Machine

Meer informatie over de hulpprogram ma's die zijn geïnstalleerd op de DSVM:

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer edition

U kunt Microsoft Enterprise Library voor schaalbare R of Python voor uw analyses gebruiken omdat de Machine Learning Server Developer edition is geïnstalleerd op de virtuele machine. Machine Learning Server is voorheen bekend als Microsoft R Server, het is een algemeen platform voor het analyseren van bedrijfs klasse. Het is beschikbaar voor zowel R als python. Het is ook schaalbaar, commercieel ondersteund en veilig.

Machine Learning-Server biedt ondersteuning voor verschillende big data-statistieken, voorspellende modellen en machine learning-taken. Deze biedt ondersteuning voor het volledige aanbod van analytics: verkenning, analyse, visualisatie en modellering. Door en uitbreiden van open-source R en Python, is de Machine Learning Server compatibel met R en Python-scripts en functies. Het is ook compatibel met CRAN, pip en Conda-pakketten voor het analyseren van gegevens op de grote schaal.

Machine Learning Server lost de geheugenbeperkingen van open-source R door het toevoegen van parallelle en bloksgewijze verwerking van gegevens. Dit betekent dat u analyses kunt uitvoeren op veel grotere gegevens dan wat past in het hoofd geheugen. Visual Studio Community is opgenomen op de virtuele machine. Het bevat de R-hulpprogram ma's voor Visual Studio-en Python Tools for Visual Studio-uitbrei dingen (PTVS) die een volledige IDE bieden voor het werken met R of python. We bieden ook andere IDE's zoals [RStudio](https://www.rstudio.com) en [PyCharm Community edition](https://www.jetbrains.com/pycharm/) op de virtuele machine.

### <a name="python"></a>Python

Voor de ontwikkeling met behulp van python worden Anaconda python-distributies 2,7 en 3,6 geïnstalleerd. Deze distributies hebben de basis Python samen met ongeveer 300 van de meest populaire math, engineering en data analytics-pakketten. U kunt PTVS, die is geïnstalleerd binnen Visual Studio Community 2017. Of u kunt een van de IDE's geleverd met Anaconda, zoals niet-actief of Spyder gebruiken. Zoek en start een van deze pakketten (Win + S).

> [!NOTE]
> Om te verwijzen de Python-Tools voor Visual Studio op Anaconda Python 2.7, die u wilt maken van aangepaste omgevingen voor elke versie. Deze omgeving paden instellen in Visual Studio 2017 Community, gaat u naar **extra** > **Python Tools** > **Python-omgevingen**. Selecteer vervolgens **+ aangepast**.

Anaconda Python 3.6 is geïnstalleerd onder **C:\Anaconda**. Anaconda Python 2.7 is geïnstalleerd onder **c:\Anaconda\envs\python2**. Zie voor gedetailleerde stappen [documentatie bij PTVS](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>De Jupyter-Notebook

Anaconda distributie wordt geleverd met de Jupyter-Notebook, een omgeving voor het delen van code en -analyse. De Jupyter-Notebook-server is vooraf geconfigureerd met Python 2.7, Python 3.x PySpark, Julia en R-kernels. Als u de Jupyter-server wilt starten en de browser voor toegang tot de notebook server wilt starten, gebruikt u het pictogram **Jupyter notebook** bureau blad.

We het pakket verschillende voorbeeldnotitieblokken in Python en R. Nadat u toegang Jupyter tot, geeft de notebooks laten zien hoe om te werken met de volgende technologieën:

* Machine Learning Server
* SQL Server Machine Learning Services, analyses in-data base
* Python
* Micro soft cognitieve ToolKit
* Tensorflow
* Andere Azure-technologieën

U vindt de koppeling naar de voor beelden op de start pagina van de notitie blok nadat u zich hebt geverifieerd voor de Jupyter Notebook met behulp van het wacht woord dat u in een eerdere stap hebt gemaakt.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

De DSVM omvat Visual Studio community. Dit is een gratis versie van de populaire IDE van micro soft die u kunt gebruiken voor evaluatie doeleinden en kleine teams. Zie de [licentievoorwaarden](https://www.visualstudio.com/support/legal/mt171547).

Open Visual Studio met behulp van het bureaublad pictogram of het menu **Start** . Zoeken naar programma's (Win + S), gevolgd door **Visual Studio**. U kunt daar projecten maken in talen zoals C#, Python, R en node.js. Geïnstalleerde invoegtoepassingen maken het handig om te werken met de volgende Azure-services:

* Azure Data Catalog
* Azure HDInsight Hadoop en Spark
* Azure Data Lake

Er is ook een invoeg toepassing met de naam **Azure machine learning voor Visual Studio code** die naadloos kan worden geïntegreerd in azure machine learning en waarmee u snel AI-toepassingen kunt bouwen.

> [!NOTE]
> Mogelijk is, krijgt u een bericht dat uw evaluatieperiode is verlopen. Voer de referenties van uw Microsoft-account. Of maak een nieuwe gratis account voor toegang tot Visual Studio Community.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition

De DSVM wordt geleverd met een ontwikkelaars versie van SQL Server 2017 met Machine Learning Services. Deze SQL Server Edition bevindt zich in R of python en kan in-data base-analyses uitvoeren. Machine Learning-Services biedt een platform voor het ontwikkelen en implementeren van intelligente toepassingen. U kunt deze talen en veel pakketten van de community om te maken van modellen en voorspellingen voor uw SQL Server-gegevens genereren. Omdat de Machine Learning-Services, in de database, de R- en Python talen in SQL Server is geïntegreerd, kunt u analytics dicht bij de gegevens bewaren. Deze integratie elimineert de kosten en beveiligingsrisico's die zijn gekoppeld aan de verplaatsing van gegevens.

> [!NOTE]
> De editie van SQL Server Developer is alleen voor ontwikkelings- en testdoeleinden. U moet een licentie voor het in productie uitvoeren.

U kunt toegang tot SQL Server door het starten van Microsoft SQL Server Management Studio. De naam van uw virtuele machine wordt ingevuld als de **servernaam**. Windows-verificatie gebruiken wanneer u zich aanmelden als de beheerder van Windows. Wanneer u zich in SQL Server Management Studio, kunt u maken van andere gebruikers, databases maken, gegevens importeren en uitvoeren van SQL-query's.

Als u wilt inschakelen in database analyses met behulp van SQL-Machine Learning-Services, door de volgende opdracht uit als een eenmalige actie in SQL Server Management Studio uitvoeren nadat u zich hebt aangemeld als beheerder van de server:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Vervang `%COMPUTERNAME%` door de naam van uw virtuele machine.

### <a name="azure"></a>Azure

Verschillende Azure-hulpprogramma's zijn geïnstalleerd op de virtuele machine:

* Een snelkoppeling op het bureaublad gaat u naar de Azure SDK-documentatie.
* Gebruik **AzCopy** om gegevens in en uit uw Azure Storage-account te kopiëren. Als u wilt zien gebruik, voer **Azcopy** achter de opdrachtprompt.
* Gebruik **Azure Storage Explorer** om te bladeren van de objecten die u in uw Azure Storage-account opslaat. Er worden ook gegevens gekopieerd van en naar Azure Storage. Als u dit hulp programma wilt openen, voert u **Storage Explorer** in het **Zoek** veld in. U kunt deze ook vinden in het menu **Start** van Windows.
* **Adlcopy** kopieert gegevens naar Azure data Lake. Als u wilt zien gebruik, voer **adlcopy** in een opdrachtprompt.
* **dtui** kopieert gegevens van en naar Azure Cosmos DB, een NoSQL-data base in de Cloud. Voer **dtui** in een opdrachtprompt.
* **Azure Data Factory Integration runtime** kopieert u gegevens tussen on-premises gegevens bronnen en de Cloud. Het wordt gebruikt in hulpprogramma's zoals Azure Data Factory.
* Gebruik **Microsoft Azure PowerShell** om uw Azure-resources te beheren in de Power shell-script taal. Het ook geïnstalleerd op de virtuele machine.

### <a name="power-bi"></a>Power BI

De DSVM wordt geleverd met **Power bi Desktop** geïnstalleerd om u te helpen bij het bouwen van Dash boards en visualisaties. Met dit hulpprogramma kunt gegevens ophalen uit verschillende bronnen, uw dashboards en rapporten maken en deze publiceren naar de cloud. Zie voor meer informatie de [Power BI](https://powerbi.microsoft.com) site. U kunt Power BI Desktop vinden in het menu **Start** .

> [!NOTE]
> U hebt een Microsoft Office 365-account voor toegang tot Power BI nodig.

### <a name="azure-machine-learning-service-python-sdk"></a>Azure Machine Learning-service Python-SDK

Gegevens wetenschappers en AI-ontwikkel aars gebruiken de Azure Machine Learning SDK voor python om machine learning werk stromen te bouwen en uit te voeren met de [Azure machine learning service](../service/overview-what-is-azure-ml.md). U kunt communiceren met de service in Jupyter-notebooks of een andere python IDE met behulp van open-source frameworks zoals tensor flow en scikit-learn.

Om te beginnen met de Python-SDK, Zie [Python gebruiken om aan de slag met Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

De Python-SDK is vooraf geïnstalleerd op de Microsoft Data Science Virtual Machine.

## <a name="more-microsoft-development-tools"></a>Meer Microsoft-ontwikkelprogramma 's

U kunt het [installatie programma van het micro soft](https://www.microsoft.com/web/downloads/platform.aspx) -webplatform gebruiken om andere ontwikkel Hulpprogramma's van micro soft te zoeken en te downloaden. Er is ook een snelkoppeling naar het hulp programma op het bureau blad van micro soft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de virtuele machine

| Item | Directory |
| --- | --- |
| Jupyter-Notebook server-configuraties | C:\ProgramData\jupyter |
| Jupyter-Notebook voorbeelden basismap | C:\dsvm\notebooks en c:\Users\\< gebruikers\>naam \notebooks |
| Andere voorbeelden | C:\dsvm\samples |
| Anaconda, standaard: Python 3,6 | C:\Anaconda |
| Anaconda Python 2.7 omgeving | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (zelfstandige versie)-Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Standaardexemplaar R, Machine Learning Server (zelfstandige versie) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL-Machine Learning-Services in de database-instantie directory | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Diverse hulpprogramma 's | C:\dsvm\tools |

> [!NOTE]
> Op de Windows Server 2012-versie van de editie van DSVM- en Windows Server 2016 vóór maart 2018 is de standaardomgeving Anaconda Python 2.7. De secundaire omgeving is python 3,5, te vinden op **C:\Anaconda\envs\py35**.

## <a name="next-steps"></a>Volgende stappen

* De hulpprogramma's op de virtuele machine voor datatechnologie verkennen door het selecteren van de **Start** menu.
* Meer informatie over Azure Machine Learning-service door te lezen [wat is Azure Machine Learning-service?](../service/overview-what-is-azure-ml.md) en uit te proberen de [snelstartgidsen en zelfstudies](../service/index.yml) die beschikbaar zijn.
* Ga in Verkenner naar **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** voor voor beelden die gebruikmaken van de RevoScaleR-bibliotheek in R die ondersteuning biedt voor gegevens analyse op de schaal van de onderneming.  
* Lees het artikel [tien dingen die u in de Data science Virtual Machine doen kunt](https://aka.ms/dsvmtenthings).
* Informatie over het bouwen van analytische oplossingen voor end-to-end systematisch met behulp van de [Team Data Science Process](../team-data-science-process/index.yml).
* Ga naar de [Azure AI Gallery](https://gallery.cortanaintelligence.com) services voor machine learning en data analytics-voorbeelden die gebruikmaken van Azure Machine Learning en gerelateerde gegevens in Azure. We hebben ook een pictogram voor deze galerie beschikbaar op de **Start** menu en het bureaublad van de virtuele machine.
