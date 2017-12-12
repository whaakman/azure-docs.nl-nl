---
title: Tien kunt u dingen op de virtuele Machine in Azure in gegevens wetenschappelijke doen | Microsoft Docs
description: Verschillende gegevensverkenning en modellering taak uitvoeren op de virtuele Machine voor gegevenswetenschap.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 622bb5971a6ad774e770f00d2d9f44999b844d12
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a>Tien dingen die u kunt doen in de virtuele wetenschapsgegevensmachine

Microsoft Data wetenschappelijke virtuele Machine (DSVM) is een krachtige gegevens wetenschappelijke ontwikkelingsomgeving waarmee u verschillende gegevens te verkennen en modellering taken uit te voeren. De omgeving wordt al geleverd gebouwd en gebundelde verschillende populaire gegevens analytics hulpprogramma's waarmee u gemakkelijk kunt snel aan de slag met uw analyse voor On-premises Cloud of hybride implementaties. De DSVM nauw met vele Azure-services en kan worden gelezen en verwerkt gegevens die al zijn opgeslagen in Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage of Azure Cosmos DB. Het kan ook gebruikmaken van andere hulpprogramma's voor webanalyse zoals Azure Machine Learning en Azure Data Factory.

In dit artikel zien we hoe u uw DSVM verschillende gegevens wetenschappelijke taken uitvoeren en communiceren met andere Azure-services gebruiken. Hier volgen enkele dingen die u op de DSVM kunt uitvoeren:

1. Gegevens verkennen en ontwikkelen van modellen lokaal op de Server in de Microsoft-R, Python met DSVM
2. Gebruik van een Jupyter-notebook om te experimenteren met uw gegevens in een browser met behulp van Python 2, 3, Python, Microsoft R een gereed enterprise-versie van R ontworpen voor schaalbaarheid en prestaties
3. Modellen gebouwd met behulp van R- en Python in Azure Machine Learning zodat clienttoepassingen toegang heeft tot uw via een eenvoudige webinterface services modellen operationeel maken
4. Uw Azure-resources met Azure-portal of Powershell beheren
5. Uw opslagruimte uitbreiden en delen van grootschalige gegevenssets / code in uw hele team door het maken van een Azure File storage als een koppelbaar station op uw DSVM
6. Code delen met uw team met behulp van GitHub en toegang tot uw opslagplaats met behulp van de vooraf geïnstalleerde Git clients - Git Bash Git GUI.
7. Toegang krijgen tot diverse Azure gegevens en analyse-services zoals Azure blob-opslag, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB Azure SQL Data Warehouse & databases
8. Maak rapporten en een dashboard met Power BI Desktop vooraf zijn geïnstalleerd op de DSVM en deze implementeren in de cloud
9. Dynamisch schalen uw DSVM om te voldoen aan de projectbehoeften van uw
10. Extra hulpprogramma's installeren op de virtuele machine   

> [!NOTE]
> Aanvullende gebruikskosten voor veel van de opslag en analyses services van aanvullende gegevens die worden vermeld in dit artikel worden toegepast. Raadpleeg de [prijzen van Azure](https://azure.microsoft.com/pricing/) pagina voor meer informatie.
> 
> 

**Vereisten**

* U moet een Azure-abonnement. U kunt zich aanmelden voor een gratis proefversie [hier](https://azure.microsoft.com/free/).
* Instructies voor het inrichten van een Data wetenschappelijke virtuele Machine op de Azure-portal zijn beschikbaar op [maken van een virtuele machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Gegevens verkennen en ontwikkelen van modellen Microsoft R Server of Python
Talen zoals R- en Python kunt u uw gegevensanalyse rechts op de DSVM doen.

Voor R, kunt u een IDE 'Revolution R Enterprise 8.0' die kan worden gevonden in het startmenu of het bureaublad. Microsoft heeft opgegeven, aanvullende bibliotheken boven op de Open source/CRAN-R om in te schakelen schaalbare analytics en de mogelijkheid om gegevens groter is dan de geheugengrootte die is toegestaan door het uitvoeren van parallelle gesegmenteerde analyse te analyseren. U kunt ook installeren met een R-IDE van uw keuze soortgelijke [RStudio](https://www.rstudio.com/products/rstudio-desktop/).

Voor Python, kunt u een IDE zoals Visual Studio Community Edition waarvoor de Python Tools for Visual Studio (PTVS)-extensie die vooraf zijn geïnstalleerd. Een basic Python 2.7 is standaard geconfigureerd op PTVS (zonder een bibliotheek analytics, zoals SciKit, Pandas). Om in te schakelen Anaconda Python 2.7 en 3.5, moet u het volgende doen:

* Maken van aangepaste omgevingen voor elke versie door te navigeren naar **extra** -> **Python Tools** -> **Python-omgevingen** en vervolgens te klikken op '**+ aangepaste**' in de Visual Studio 2015 Community Edition
* Geef een beschrijving en stel de omgeving paden als voorvoegsel *c:\anaconda* voor Anaconda Python 2.7 of *c:\anaconda\envs\py35* voor Anaconda Python 3.5
* Klik op **automatische detectie** en vervolgens **toepassen** om op te slaan van de omgeving.

Hier ziet u hoe de instellingen van de aangepaste omgeving eruitziet in Visual Studio.

![PTVS Setup](./media/vm-do-ten-things/PTVSSetup.png)

Zie de [documentatie bij PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) voor meer informatie over het maken van Python-omgevingen.

Nu u worden ingesteld om een nieuwe Python-project te maken. Navigeer naar **bestand** -> **nieuw** -> **Project** -> **Python** en selecteer het type van Python-toepassing die u wilt maken. U kunt de Python-omgeving voor het huidige project instellen op de gewenste versie (Anaconda 2.7 of 3.5): met de rechtermuisknop op de **Python-omgeving**, selecteer **Python-omgevingen toevoegen/verwijderen**, en selecteer vervolgens de gewenste omgeving om te koppelen aan het project. U vindt meer informatie over het werken met PTVS op de product- [documentatie](https://github.com/Microsoft/PTVS/wiki) pagina.

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2. Met behulp van een Jupyter-Notebook om te verkennen en uw gegevens met Python of R model
De Jupyter-Notebook is een krachtige omgeving met een browser gebaseerde 'IDE' voor gegevensverkenning en modellering. U kunt Python 2, 3 Python of R (Open Source en de Microsoft-R-Server) gebruiken in een Jupyter-Notebook.

Starten van de Jupyter-Notebook Klik op het pictogram van het menu start / pictogram op het bureaublad met de titel **Jupyter-Notebook**. Op de DSVM u kunt ook bladeren naar ' https://localhost:9999 / "voor toegang tot de Notebook Jupiter. Als wordt u gevraagd een wachtwoord, gebruikt u instructies in de ***het maken van een sterk wachtwoord op de server Jupyter-notebook*** sectie van de [inrichten van de virtuele Machine voor Microsoft Data wetenschappelijke](provision-vm.md) onderwerp voor een sterk wachtwoord voor toegang tot de Jupyter-notebook maken. 

Nadat u de notebook hebt geopend, ziet u een map met een paar voorbeeld-notitieblokken die vooraf verpakte in de DSVM zijn. U kunt nu:

* Klik op de laptop om te zien van de code.
* elke cel uitvoeren door te drukken **SHIFT + ENTER**.
* de volledige notebook uitvoeren door te klikken op **cel** -> **uitvoeren**
* Maak een nieuwe notebook door te klikken op het pictogram Jupyter (bovenste linkerhoek) **nieuw** knop aan de rechterkant en vervolgens de notebook-taal (ook wel bekend als kernels) te kiezen.   

> [!NOTE]
> Momenteel ondersteuning wordt voor Python 2.7, Python 3.5 en R. De kernel R ondersteunt programmering in zowel Open-source R, evenals de onderneming schaalbare Microsoft R Server.   
> 
> 

Zodra u de notebook u vindt worden uw gegevens, het model bouwen, testen van het model met behulp van uw keuze van bibliotheken.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. Gebruik R of Python en Operationalize ervan met Azure Machine Learning modellen bouwen
Zodra u hebt gemaakt en gevalideerd van uw model is de volgende stap meestal te implementeren in productie. Hierdoor kan de client toepassingen aan te roepen de modelvoorspellingen op een realtime of op basis van de batch-modus. Azure Machine Learning biedt een mechanisme voor een model ingebouwd in R- of Python operationeel.

Wanneer u het Azure Machine Learning-model operationeel maken, wordt een webservice weergegeven waarmee clients REST-aanroepen die in de invoerparameters doorgeven en ontvangen van voorspellingen uit het model als uitvoer.   

> [!NOTE]
> Als u hebt nog niet aangemeld voor Azure Machine Learning, kunt u een gratis werkruimte of een standaard werkruimte verkrijgen in via de [Azure Machine Learning Studio](https://studio.azureml.net/) startpagina en te klikken op 'aan de slag'.   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Build en Python operationeel modellen
Hier volgt een codefragment in een Jupyter-Notebook van Python die voortbouwt een eenvoudige model met behulp van de bibliotheek SciKit meer ontwikkeld.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

De methode gebruikt voor het implementeren van uw modellen python Azure Machine Learning loopt van de voorspelling van het model in een functie en wordt deze verfraaid met kenmerken die worden geleverd door de vooraf geïnstalleerde Azure Machine Learning python-bibliotheek waarbij aanduiding van uw Azure Machine Learning-werkruimte-ID, API-sleutel en de invoer en parameters geretourneerd.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Een client kan nu bellen naar de webservice. Er zijn gemak wrappers die samenstellen van de REST-API-aanvragen. Hier volgt een voorbeeld van code gebruiken voor de webservice.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> De Azure Machine Learning-bibliotheek wordt alleen ondersteund voor Python 2.7 momenteel.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Build en operationeel R modellen
U kunt implementeren op de gegevens wetenschappelijke virtuele Machine of elders naar Azure Machine Learning gebouwd op een manier die lijkt op hoe deze wordt uitgevoerd voor Python R-modellen. Haar als volgt:

* Maak een bestand settings.json om uw werkruimte-ID en auth token zoals weergegeven in het volgende codevoorbeeld.
* een wrapper schrijven voor het model functie voorspellen.
* Roep ```publishWebService``` in de bibliotheek Azure Machine Learning om door te geven in de functie-wrapper.  

Hier volgt de procedure en code codefragmenten die kunnen worden gebruikt voor het instellen, maken, publiceren en gebruiken van een model als een webservice in Azure Machine Learning.

#### <a name="setup"></a>Instellen
1. De Machine Learning-R-pakket installeren door te typen ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE of uw R-IDE.
2. Download RTools van [hier](https://cran.r-project.org/bin/windows/Rtools/). U moet het zip-hulpprogramma in het pad (en de benoemde zip.exe) naar uw R-pakket in Machine Learning operationeel.
3. Maak een bestand settings.json onder de map ```.azureml``` onder de basismap en voer de parameters uit uw Azure Machine Learning-werkruimte:

Settings.JSON bestandsstructuur:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Een model in R bouwen en deze publiceren in Azure Machine Learning
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Het model dat is geïmplementeerd in Azure Machine Learning gebruiken
Voor het verbruik van het model van een clienttoepassing gebruiken we de Azure Machine Learning-bibliotheek om de gepubliceerde webservice met behulp van de naam te zoeken de `services` API-aanroep om te bepalen van het eindpunt. Vervolgens u alleen roept de `consume` werken en in het kader van de gegevens om te worden voorspeld.
De volgende code wordt gebruikt voor het gebruiken van het model is gepubliceerd als een Azure Machine Learning-webservice.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Meer informatie over de Azure Machine Learning-R-bibliotheek vindt [hier](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Uw Azure-resources met Azure-portal of Powershell beheren
De DSVM wordt niet alleen kunt u voor het bouwen van uw oplossing analytics lokaal op de virtuele machine, maar er wordt een ook hebt u toegang tot services op Microsoft Azure-cloud. Azure biedt verschillende compute, storage, gegevens analytics-services en andere services die u kunt beheren en toegang tot van uw DSVM.

Voor het beheren van uw Azure-abonnement en cloud-resources kunt u gebruikmaken van uw browser en wijs de [Azure-portal](https://portal.azure.com). U kunt ook Azure Powershell gebruiken voor het beheren van uw Azure-abonnement en resources via een script.
U kunt Azure Powershell uitvoeren vanuit een snelkoppeling op het bureaublad of vanuit het startmenu met de titel 'Microsoft Azure Powershell'. Raadpleeg [documentatie voor Microsoft Azure Powershell](../../powershell-azure-resource-manager.md) voor meer informatie over hoe u uw Azure-abonnement en de resources met behulp van Windows Powershell-scripts kunt beheren.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Uitbreiden van uw opslagruimte met een gedeelde bestandssysteem
Gegevenswetenschappers kunnen delen grote gegevenssets, code of andere bronnen in het team. De DSVM zelf is ongeveer 70GB beschikbare ruimte. Als u wilt uw opslag uitbreidt, kunt u de Azure File-Service en een koppelen op de DSVM of toegang tot dit via een REST-API.   

> [!NOTE]
> De maximale ruimte van de share Azure File-Service is 5TB en afzonderlijke maximale bestandsgrootte is 1TB.   
> 
> 

U kunt Azure Powershell gebruiken voor het maken van een share Azure File-Service. Hier wordt het script worden uitgevoerd met Azure PowerShell om een Azure-service bestandsshare te maken.

    # Authenticate to Azure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


Nu dat u een Azure-bestandsshare gemaakt hebt, kunt u deze koppelen in een virtuele machine in Azure. Het is raadzaam dat de virtuele machine zich in dezelfde Azure-Datacenter als het opslagaccount om latentie en gegevens overdracht kosten te voorkomen. Hier volgen de opdrachten voor het koppelen van het station op de DSVM die u op Azure Powershell uitvoeren kunt.

    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Nu kunt u dit station openen, net als een normale station op de virtuele machine.

## <a name="6-share-code-with-your-team-using-github"></a>6. Code delen met uw team met behulp van GitHub
GitHub is een code-opslagplaats vindt u een groot aantal voorbeeldcode en bronnen voor verschillende hulpprogramma's met behulp van verschillende technologieën die worden gedeeld door de community van ontwikkelaars. Deze Git gebruikt als de technologie volgen en versies van de codebestanden opslaan. GitHub is ook een platform waarmee u uw eigen opslagplaats opslaan van de gedeelde code en de documentatie van uw team, versiebeheer implementeren en ook beheren die toegang hebben tot weergeven en bijdragen code kunt maken. Ga naar de [GitHub help-pagina's](https://help.github.com/) voor meer informatie over het gebruik van Git. U kunt GitHub gebruiken als een van de manieren om samenwerken met uw team, gebruikt u code die is ontwikkeld door de community en code bijdragen terug naar de community.

De DSVM komt al geladen met clienthulpprogramma's op zowel opdrachtregelprogramma als goed GUI voor toegang tot de GitHub-opslagplaats. Het opdrachtregelprogramma werken met Git en GitHub, Git Bash heet. Visual Studio is geïnstalleerd op de DSVM heeft de Git-uitbreidingen. U vindt opstarten pictogrammen voor deze hulpprogramma's in het startmenu en het bureaublad.

Code downloaden van een GitHub-opslagplaats die u gebruikt de ```git clone``` opdracht. Voorbeeld voor het downloaden van wetenschappelijke gegevensopslagplaats gepubliceerd door Microsoft in de huidige map u de volgende opdracht kunt uitvoeren wanneer u naar ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

In Visual Studio kunt u de dezelfde kopieerbewerking doen. De volgende schermafbeelding ziet u hoe voor toegang tot Git en GitHub-hulpprogramma's in Visual Studio.

![GIT in Visual Studio](./media/vm-do-ten-things/VSGit.PNG)

U vindt meer informatie over het werken met uw GitHub-opslagplaats van verschillende bronnen beschikbaar op github.com met Git. De [referentieoverzicht](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is een nuttig verwijzing.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Toegang krijgen tot diverse Azure services voor gegevens en analyse
### <a name="azure-blob"></a>Azure Blob
Azure-blob is een betrouwbare, voordelige cloudopslag voor gegevens van grote of kleine. Laat het ons bekijken hoe u gegevens naar Azure-Blob en toegang tot gegevens die zijn opgeslagen in een Azure-Blob verplaatsen kunt.

**Vereiste**

* **Maken van uw Azure Blob storage-account van [Azure-portal](https://portal.azure.com).**

![Create_Azure_Blob](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Bevestig dat het vooraf geïnstalleerde opdrachtregelhulpprogramma AzCopy is gevonden op ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. U kunt de map waarin zich de azcopy.exe aan de omgevingsvariabele PATH om te voorkomen dat het pad van de volledige opdracht te typen bij het uitvoeren van dit hulpprogramma kunt toevoegen. Raadpleeg voor meer informatie over AzCopy hulpprogramma [documentatie van AzCopy](../../storage/common/storage-use-azcopy.md)
* Start het hulpprogramma Azure Storage Explorer. Kan worden gedownload vanaf [Microsoft Azure Storage Explorer](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

**Gegevens van virtuele machine verplaatsen naar Azure Blob: AzCopy**

Om gegevens te verplaatsen tussen uw lokale bestanden en de blob-opslag, kunt u AzCopy gebruiken in een opdrachtregel of PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Vervang **C:\myfolder** naar het pad waar het bestand wordt opgeslagen, **mystorageaccount** aan uw accountnaam blob storage **mycontainer** naar de containernaam **opslagaccountsleutel** naar uw toegangssleutel voor blob-opslag. U vindt de referenties van het opslagaccount in [Azure-portal](https://portal.azure.com).

![StorageAccountCredential_v2](./media/vm-do-ten-things/StorageAccountCredential_v2.png)

AzCopy-opdracht uitvoeren in PowerShell of vanaf een opdrachtprompt. Hier volgt een voorbeeld van syntaxis van AzCopy-opdracht:

    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Nadat u uw AzCopy opdracht uitvoeren om te kopiëren naar een Azure-blob ziet u uw bestand wordt in Azure Storage Explorer binnenkort.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Gegevens van virtuele machine verplaatsen naar Azure Blob: Azure Opslagverkenner**

U kunt ook gegevens uit het lokale bestand uploaden in uw virtuele machine met behulp van Azure Storage Explorer:

* Als u wilt gegevens uploaden naar een container, selecteer de doelcontainer en klik op de **uploaden** knop.![ In Opslagverkenner uploaden](./media/vm-do-ten-things/storage-accounts.png)
* Klik op de **...**  rechts van de **bestanden** Selecteer een of meerdere bestanden voor het uploaden van het bestandssysteem en klikt u op **uploaden** om te beginnen met de bestanden te uploaden.![ Uploaden van bestanden naar de blob](./media/vm-do-ten-things/upload-files-to-blob.png)

**Gegevens lezen uit Azure Blob: leesmodule Machine Learning**

In Azure Machine Learning Studio kunt u een **gegevens importeren-module** gegevens lezen uit de blob.

![AML_ReaderBlob_Module_v3](./media/vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Gegevens lezen uit Azure Blob: Python ODBC**

U kunt **BlobService** bibliotheek gegevens rechtstreeks vanuit de blobs in een Jupyter-Notebook of Python lezen.

Vereiste pakketten eerst importeren:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Vervolgens worden in de referenties van uw Azure Blob-account en gegevens uit Blob lezen:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

De gegevens worden als een gegevensframe gelezen:

![IPNB_data_readin](./media/vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Storage is een opslagplaats hyperschaal voor big data-analyses workloads en compatibel is met Hadoop Distributed File System (HDFS). Deze werkt met het Hadoop-ecosysteem en Azure Data Lake Analytics. Laten we zien hoe u kunt gegevens in Azure Data Lake Store verplaatsen en uitvoeren van analytics met Azure Data Lake Analytics.

**Vereiste**

* Maken van uw Azure Data Lake Analytics in [Azure-portal](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* De **Azure Data Lake Tools** in **Visual Studio** gevonden op deze [koppeling](https://www.microsoft.com/download/details.aspx?id=49504) is al geïnstalleerd op de Visual Studio Community Edition die zich op de virtuele machine. Na het starten van de Visual Studio en logboekregistratie in uw Azure-abonnement, ziet u uw Azure Data Analytics-account en de opslag in het linkerdeelvenster van Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Gegevens van virtuele machine verplaatsen naar Data Lake: Azure Data Lake Explorer**

U kunt **Azure Data Lake Explorer** voor het uploaden van gegevens van de lokale bestanden in de virtuele Machine naar Data Lake storage.

![Azure_Data_Lake_UploadData](./media/vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

U kunt ook een pijplijn gegevens om de verplaatsing van gegevens naar of van het gebruik van Azure Data Lake productionize maken de [Azure gegevens Factory(ADF)](https://azure.microsoft.com/services/data-factory/). We u verwijzen naar dit [artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) pijplijnen om u te begeleiden bij de stappen voor het bouwen van de gegevens.

**Gegevens lezen uit Azure Blob naar Data Lake: U-SQL**

Als uw gegevens zich in Azure Blob-opslag bevindt, kunt u gegevens rechtstreeks lezen van Azure storage-blob in U-SQL-query. Controleer of dat uw blob storage-account is gekoppeld aan uw Azure Data Lake voordat het samenstellen van de U-SQL-query. Ga naar **Azure-portal**, zoeken van uw Azure Data Lake Analytics-dashboard, klikt u op **gegevensbron toevoegen**, selecteer opslagtype moet **Azure Storage** en sluit u uw Azure Storage-accountnaam en-sleutel. U zich vervolgens om te verwijzen naar de gegevens die zijn opgeslagen in het opslagaccount.

![Voer storage-account en een sleutel](./media/vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

In Visual Studio kunt u lezen van gegevens uit blob storage, sommige gegevensmanipulatie doen, functie-engineering en de resulterende uitvoergegevens naar Azure Data Lake of Azure Blob Storage. Wanneer u verwijst naar de gegevens in blob storage, gebruiken **wasb: / /**; wanneer u verwijst naar de gegevens in Azure Data Lake gebruik **swbhdfs: / /**

![Gegevensframe](./media/vm-do-ten-things/USQL_Read_Blob_v2.PNG)

U kunt de volgende U-SQL-query's in Visual Studio:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Nadat de query wordt verzonden naar de server, wordt een diagram die de status van uw taak weergegeven.

![Diagram van de status van taak](./media/vm-do-ten-things/USQL_Job_Status.PNG)

**Opvragen van gegevens in Data Lake: U-SQL**

Nadat de gegevensset wordt ingenomen in Azure Data Lake, kunt u [U-SQL-taal](../../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) doorzoeken en Verken de gegevens. U-SQL-taal is vergelijkbaar met T-SQL, maar sommige functies van C# combineert zodat gebruikers aangepaste modules, gebruiker gedefinieerde functies en enzovoort schrijven kunnen. U kunt de scripts in de vorige stap.

Nadat de query wordt verzonden naar de server, tripdata_summary. CSV vindt u enkele ogenblikken in **Azure Data Lake Explorer**, u mogelijk voorbeeld van de gegevens door met de rechtermuisknop op het bestand.

![Bestand in Azure Data Lake Explorer](./media/vm-do-ten-things/USQL_create_summary.png)

Het om bestandsinformatie te bekijken:

![Samenvatting](./media/vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-Clusters
Azure HDInsight is een beheerde Apache Hadoop, Spark, HBase en Storm-service in de cloud. U kunt eenvoudig werken met Azure HDInsight-clusters van de gegevens wetenschappelijke virtuele machine.

**Vereiste**

* Maken van uw Azure Blob storage-account van [Azure-portal](https://portal.azure.com). Dit opslagaccount wordt gebruikt voor het opslaan van gegevens voor HDInsight-clusters.

![Azure Blob storage-account maken](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Azure HDInsight Hadoop-Clusters van aanpassen [Azure-portal](../team-data-science-process/customize-hadoop-cluster.md)
  
  * Het opslagaccount met uw HDInsight-cluster gemaakt wanneer deze wordt gemaakt, moet u koppelen. Dit opslagaccount wordt gebruikt voor toegang tot gegevens die kunnen worden verwerkt binnen het cluster.

![Koppelen aan de storage-account is gemaakt met HDInsight-cluster](./media/vm-do-ten-things/Create_HDI_v4.PNG)

* U moet inschakelen **RAS** met het hoofdknooppunt van het cluster nadat deze is gemaakt. Vergeet niet de RAS-referenties die u hier opgeeft (anders dan die zijn opgegeven voor het cluster bij het maken ervan): u nodig hebt in de volgende procedure.

![Externe toegang inschakelen](./media/vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Maak een Azure Machine Learning-werkruimte. Uw Machine Learning-experimenten worden opgeslagen in deze Machine Learning-werkruimte. De gemarkeerde opties selecteren in de Portal, zoals wordt weergegeven in de volgende schermafbeelding:

![Een Azure Machine Learning-werkruimte maken](./media/vm-do-ten-things/Create_ML_Space.PNG)

* Voer vervolgens de parameters voor uw werkruimte

![Geef parameters van Machine Learning-werkruimte](./media/vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Uploaden van gegevens met behulp van de IPython Notebook. Vereiste pakketten eerst te importeren, plug-in referenties, het maken van een database in uw opslagaccount en laden van gegevens naar HDI-clusters.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create the connection to Hive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Ook kunt u dit [scenario](../team-data-science-process/hive-walkthrough.md) NYC Taxi gegevens te uploaden naar HDI-cluster. Belangrijke stappen omvatten:
  
  * AzCopy: gecomprimeerde CSV van openbare blobs downloaden naar uw lokale map
  * AzCopy: uploaden uitgepakte CSV van vanuit een lokale map voor het HDI-cluster
  * Meld u aan bij het hoofdknooppunt van Hadoop-cluster en bereid voor experimentele data-analyse

Nadat de gegevens zijn geladen voor HDI-cluster, kunt u uw gegevens in Azure Storage Explorer controleren. En u hebt een database nyctaxidb in HDI-cluster gemaakt.

**Gegevensverkenning: Hive-query's in Python**

Omdat de gegevens zich in de Hadoop-cluster, kunt u het pakket pyodbc verbinding maken met Hadoop-Clusters en query-database met Hive exploratie doen en functie-engineering. De bestaande tabellen die in de vereiste stap is gemaakt, kunt u weergeven.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Bestaande tabellen weergeven](./media/vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Bekijk het aantal records in elke maand en de frequenties van Gekantelde of niet in de tabel reis:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Grafische voorstelling van het aantal records in elke maand](./media/vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Grafiek van de frequenties tip](./media/vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

We kunnen ook de afstand tussen ophalen locatie en dropoff locatie te berekenen en vervolgens vergelijken met de afstand reis.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Tabel ophalen en dropoff](./media/vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Grafiek van ophalen/dropoff afstand op reis afstand](./media/vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Nu gaan we een (% 1) omlaag actieve set van gegevens voorbereiden voor modellering. We kunnen deze gegevens gebruiken in Machine Learning-leesmodule.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of the join into the preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Na enige tijd ziet u dat de gegevens zijn geladen in Hadoop-clusters:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabel met gegevens](./media/vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Gegevens lezen uit HDI met Machine Learning: leesmodule**

U kunt ook de **lezer** module in Machine Learning Studio toegang tot de database in Hadoop-cluster. Sluit de referenties van uw HDI-clusters en Azure Storage-Account zodat build ing machine learning-modellen met database in HDI-clusters.

![Eigenschappen van de module Reader](./media/vm-do-ten-things/AML_Reader_Hive.PNG)

De gegevensset scored kan vervolgens worden weergegeven:

![Scored gegevensset weergeven](./media/vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse & databases
Azure SQL Data Warehouse is een elastische datawarehouse als een service met SQL Server-ervaring bedrijfsniveau.

U kunt uw Azure SQL Data Warehouse inrichten door de instructies in dit [artikel](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Nadat u uw Azure SQL Data Warehouse inricht, kunt u dit [scenario](../team-data-science-process/sqldw-walkthrough.md) wilt uploaden van gegevens, exploratie en modellering met behulp van de gegevens in de SQL Data Warehouse.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos-database is een NoSQL-database in de cloud. Dit kunt u werken met documenten, zoals JSON en kunt u op te slaan en de documenten opvragen.

U moet de volgende stappen per vereisten voor toegang tot Azure Cosmos DB via de DSVM uitvoeren.

1. Installeer de Azure-SDK voor Python Cosmos DB (uitvoeren ```pip install pydocumentdb``` vanaf een opdrachtprompt)
2. Maak een Azure DB die Cosmos-account en een database van [Azure-portal](https://portal.azure.com)
3. 'Azure Cosmos DB hulpprogramma voor migratie' downloaden van [hier](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) en uitpakken naar een map van uw keuze
4. Importeren van JSON-gegevens (Vulkaan gegevens) opgeslagen op een [openbare blob](https://cahandson.blob.core.windows.net/samples/volcano.json) naar Cosmos DB met de volgende opdrachtparameters naar het migratiehulpprogramma voor (dtui.exe uit de map waar u het hulpprogramma voor migratie DB Cosmos geïnstalleerd). Geef de bron en doel-locatie met de volgende parameters:
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[sleutel]; Database = Vulkaan /t.Collection:volcano1

Zodra u de gegevens importeert, kunt u gaat u naar Jupyter en opent u de notebook met de titel *DocumentDBSample* die python-code voor toegang tot Azure Cosmos DB en voer enkele eenvoudige query bevat. U meer informatie over Cosmos DB via de service [documentatiepagina](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. Maak rapporten en een dashboard met Power BI Desktop
Laat het ons visualiseren het Vulkaan JSON-bestand dat we eerder in het voorgaande voorbeeld Cosmos DB in Power BI om visual inzicht in de gegevens te krijgen. Gedetailleerde stappen zijn beschikbaar in de [Power BI-artikel](../../cosmos-db/powerbi-visualize.md). Hier volgen de hoofdstappen:

1. Power BI Desktop openen en "Get-Data" doen. Geef de URL als: https://cahandson.blob.core.windows.net/samples/volcano.json
2. U ziet het JSON-records geïmporteerd als een lijst
3. De lijst converteren naar een tabel zodat Power BI met dezelfde werken kunt
4. Vouw de kolommen door te klikken op het uitvouwpictogram (de knop met het pictogram 'pijl-links en een pijl naar rechts' aan de rechterkant van de kolom)
5. U ziet dat de locatie een veld '-Record is. Vouw de record en selecteert u alleen de coördinaten. Coördinaat is een lijstkolom
6. Voeg een nieuwe kolom de coördinaat lijstkolom converteren naar een aparte LatLong-kolom van door komma's, de twee elementen in het lijstveld coördinaat is met de formule cookievalidatie ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Vervolgens zet u de ```Elevation``` kolom decimalen en selecteer de **sluiten** en **toepassen**.

In plaats van de vorige stappen, kunt u de volgende code dat scripts buiten de stappen in de geavanceerde Editor in Power BI waarmee u de gegevenstransformaties schrijven in een querytaal plakken.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



U hebt nu de gegevens in uw Power BI-gegevensmodel. Uw Power BI-bureaublad ziet er als volgt:

![Power BI Desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

U kunt beginnen met het maken van rapporten en visualisaties met behulp van het gegevensmodel. U kunt de stappen in deze [Power BI-artikel](../../cosmos-db/powerbi-visualize.md#build-the-reports) om een rapport te maken. Het eindresultaat is een rapport dat op het volgende lijkt.

![Power BI Desktop rapportweergave - Power BI-connector](./media/vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. Dynamisch schalen uw DSVM om te voldoen aan de projectbehoeften van uw
U kunt schalen omhoog en omlaag de DSVM om te voldoen aan de projectbehoeften van uw. Als u niet nodig hebt voor het gebruik van de virtuele machine in de 's avonds of in het weekend, kunt u alleen afsluiten de VM van de [Azure-portal](https://portal.azure.com).

> [!NOTE]
> U kosten compute als u alleen de knop besturingssysteem op de virtuele machine gebruikt.  
> 
> 

Als u wilt verwerken van een grootschalige analyse en vindt u een groot aantal VM-grootten in termen van CPU-kernen, geheugencapaciteit en schijftypen (inclusief Solid-State-schijven) en die voldoen aan uw berekenings- en de behoeften van meer CPU, geheugen of schijfruimte capaciteit nodig. De volledige lijst met virtuele machines samen met de Uurlijkse compute prijscategorie is beschikbaar op de [prijzen van Azure virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/) pagina.

Op dezelfde manier als uw behoefte VM verwerkingscapaciteit vermindert (bijvoorbeeld: u een primaire werkbelasting verplaatst naar een Hadoop- of een Spark-cluster), kunt u het cluster in terugschroeven de [Azure-portal](https://portal.azure.com) en zal de instellingen van uw VM-instantie. Hier volgt een schermafbeelding.

![Instellingen voor VM-exemplaar](./media/vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Extra hulpprogramma's installeren op de virtuele machine
We hebben verschillende hulpprogramma's die we zijn ervan overtuigd kunnen op veel van de algemene gegevens analytics behoeften en die moet u tijd besparen door te voorkomen dat u hoeft te installeren en configureren van uw omgevingen één voor één adres en u geld besparen door betaalt alleen voor resources dat u geleverd.

U kunt gebruikmaken van andere Azure-gegevens en analyse-services in dit artikel voor het verbeteren van uw omgeving analytics profiel. We begrijpen dat in sommige gevallen de behoeften van uw mogelijk extra hulpprogramma's, waaronder sommige eigen hulpprogramma's van derden. U hebt volledige beheerderstoegang op de virtuele machine voor het installeren van nieuwe hulpprogramma's die u nodig hebt. U kunt ook extra pakketten installeren in Python en R die niet vooraf zijn geïnstalleerd. Voor Python gebruikt u een ```conda``` of ```pip```. Voor R kunt u de ```install.packages()``` in het R-console of gebruik van de IDE en kies '**pakketten** -> **installatiepakketten...** ".

## <a name="summary"></a>Samenvatting
Dit zijn slechts enkele dingen die u op de Microsoft wetenschappelijke virtuele Machine gegevens kunt doen. Er zijn veel meer kunt u stappen ondernemen om deze een effectieve analytics-omgeving.

