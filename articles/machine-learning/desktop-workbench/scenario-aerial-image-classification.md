---
title: Afbeeldingsclassificatie | Microsoft Docs
description: Vindt u instructies voor werkelijk scenario voor afbeeldingsclassificatie
author: mawah
ms.author: mawah
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
ms.component: core
services: machine-learning
ms.workload: data-services
ms.date: 12/13/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 5ca47c8234239b56a2d829903828dda8220d53cb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967605"
---
# <a name="aerial-image-classification"></a>Afbeeldingsclassificatie van

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



In dit voorbeeld ziet u hoe u Azure Machine Learning Workbench gebruiken voor de coördinatie van gedistribueerde trainings- en uitoefening van modellen voor classificatie van afbeeldingen. We twee methoden gebruiken voor training: (i) verfijnen een deep neural network met een [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) GPU-cluster, en (ii) de [Microsoft Machine Learning voor Apache Spark (MMLSpark)](https://github.com/Azure/mmlspark) pakket naar parametriseer afbeeldingen met behulp van CNTK modellen dat en om te trainen met behulp van de afgeleide functies classificaties. We vervolgens de getrainde modellen in parallelle wijze toepassen op grote afbeelding sets in de cloud met een [Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) cluster, zodat we de snelheid van training en uitoefening schalen door toe te voegen of te verwijderen van worker-knooppunten.

In dit voorbeeld worden twee methoden om over te dragen learning, die gebruikmaakt van modellen om de kosten van training deep neural networks helemaal te vermijden dat gemarkeerd. Een deep neural network doorgaans retraining GPU compute vereist, maar kan leiden tot een grotere nauwkeurigheid wanneer de trainingsset groot genoeg is. Een eenvoudige classificatie op boommodel installatiekopieën training vereist geen GPU compute, is inherent snel en willekeurig schaalbare en minder parameters past. Deze methode is daarom een uitstekende keuze wanneer enkele voorbeelden van training beschikbaar zijn als het is vaak het geval is bij aangepaste gebruiksvoorbeelden. 

Het Spark-cluster gebruikt in dit voorbeeld heeft 40 worker-knooppunten en kosten ~$40/hr functioneren; de Batch AI-clusterbronnen acht GPU's omvatten en kosten ~$10/hr functioneren. In dit scenario voltooien duurt ongeveer drie uur. Wanneer u klaar bent, gaat u als volgt de instructies opschonen voor het verwijderen van de resources die u hebt gemaakt en stopt waarvoor kosten worden berekend.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats

De openbare GitHub-opslagplaats voor dit scenario praktijk bevat al het materiaal, waaronder voorbeelden van code, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>De aanvraag beschrijving gebruiken

In dit scenario trainen we machine learning-modellen voor het classificeren van het type van de grond wordt weergegeven in de lucht afbeeldingen van 224-meter x 224-meter-grafieken. Modellen voor classificatie van land gebruik kunnen worden gebruikt voor het bijhouden van urbanization, ontbossing, verlies van wetlands en andere belangrijke uitwerking trends met behulp van periodiek Luchtfoto beeldmateriaal verzameld. We hebt trainings- en validatie installatiekopie ingesteld op basis van beelden van de Verenigde Staten voorbereid Nationale landbouw beeldmateriaal programma en land labels die zijn gepubliceerd door het Amerikaanse gebruiken Nationale Land Cover-Database. Voorbeeld van de afbeeldingen in elk land gebruik klasse worden hier weergegeven:

![Voorbeeld van de regio's voor elk land wordt gebruikgemaakt van label](media/scenario-aerial-image-classification/example-labels.PNG)

Na de training en valideren van de classificatie-model, zullen we deze toepassen op Luchtfoto afbeeldingen spanning Middlesex regio, MA--startpagina van Microsoft van nieuw Engeland vlak van onderzoek en ontwikkeling (KLUNGELIG) Center--om te laten zien hoe deze modellen kunnen worden gebruikt voor de analyse van trends in de stad ontwikkeling.

Voor het produceren van een installatiekopie-classificatie gebruik van overdrachtsleren, gegevenswetenschappers vaak meerdere modellen met een aantal methoden maken en selecteer optimaal presterende model. Azure Machine Learning Workbench kunt helpen data scientists training coördinatie van verschillende rekenomgevingen, bijhouden en de prestaties van meerdere modellen en een gekozen model toepassen op grote gegevenssets in de cloud.

## <a name="scenario-structure"></a>Scenario-structuur

Image-gegevens en modellen dat in dit voorbeeld zijn ondergebracht in een Azure storage-account. Een Azure HDInsight Spark-cluster deze bestanden worden gelezen en formuleert een afbeelding classificeringsmodel MMLSpark gebruiken. Het getrainde model en de voorspellingen worden vervolgens geschreven naar het opslagaccount, waar ze kunnen worden geanalyseerd en gevisualiseerd door een Jupyter-notebook die lokaal wordt uitgevoerd. Azure Machine Learning Workbench coördineert de externe uitvoering van scripts op het Spark-cluster. Het houdt ook nauwkeurigheid metrische gegevens voor meerdere modellen die zijn getraind met verschillende methoden, zodat de gebruiker selecteert u de meeste presterende model.

![Overzicht voor de satelliet installatiekopie classificatie werkelijk scenario](media/scenario-aerial-image-classification/scenario-schematic.PNG)

Deze stapsgewijze instructies beginnen door u te begeleiden bij het maken en de voorbereiding van een Azure storage-account en een Spark-cluster, inclusief de overdracht en afhankelijkheid installatie van gegevens. Ze vervolgens wordt beschreven hoe u trainingstaken Start en de prestaties van de resulterende modellen. Ze ten slotte ziet u hoe u een gekozen model van toepassing op een grote afbeelding ingesteld op het Spark-cluster en analyseer de voorspellingsresultaten lokaal.


## <a name="set-up-the-execution-environment"></a>Stel de omgeving worden uitgevoerd

De volgende instructies leiden u door het proces van het instellen van de uitvoeringsomgeving voor dit voorbeeld.

### <a name="prerequisites"></a>Vereisten
- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar)
    - U maakt een HDInsight Spark-cluster met 40 worker-knooppunten (totale 168 cores). Zorg ervoor dat uw account voldoende beschikbare cores aan de hand van de 'Gebruik + quota' tabblad voor uw abonnement in Azure portal.
       - Als u minder cores beschikbaar hebt, kunt u de sjabloon van de HDInsight-cluster als u wilt verkleinen het aantal werknemers die zijn ingericht mag wijzigen. Instructies voor deze weergegeven onder de sectie 'De HDInsight Spark-cluster maken'.
    - In dit voorbeeld maakt u een Batch AI Training-cluster met twee NC6 (1 GPU, 6 vCPU) virtuele machines. Zorg ervoor dat uw account voldoende beschikbare cores in de regio VS-Oost aan de hand van de 'Gebruik + quota' tabblad voor uw abonnement in Azure portal.
- [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md)
    - Ga als volgt de [snelstartgids installeren en maken](../desktop-workbench/quickstart-installation.md) Azure Machine Learning Workbench installeren en experimenten en Modelbeheer-Accounts maken.
- [Batch AI](https://github.com/Azure/BatchAI) Python SDK en Azure CLI
    - Voer de volgende secties in het [Batch AI recepten Leesmij](https://github.com/Azure/BatchAI/tree/master/recipes):
        - 'Vereisten'
        - 'Maken en uw Azure Active Directory (AAD)-toepassing'
        - "BatchAI Resourceproviders registreren' (onder"uitvoeren recepten met behulp van Azure CLI")
        - "Azure Batch AI-Management-Client installeren"
        - "Azure Python-SDK installeren"
    - Record de client-ID, geheim en tenant-ID van de Azure Active Directory-toepassing die u moeten maken. U gebruikt deze referenties verderop in deze zelfstudie.
    - Op dit moment Azure Machine Learning Workbench en Azure Batch AI gebruiken van afzonderlijke recoveryforks van de Azure CLI. Voor de duidelijkheid verwijzen we naar de versie van de Workbench van de CLI als 'een CLI gestart vanuit Azure Machine Learning Workbench' en de algemene release-versie (waaronder Batch AI) als "Azure CLI."
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy), een gratis hulpprogramma voor het coördineren van bestandsoverdracht tussen Azure storage-accounts
    - Zorg ervoor dat de map met het AzCopy-uitvoerbare bestand op uw systeem padomgevingsvariabele bevindt. (Instructies over het aanpassen van omgevingsvariabelen zijn beschikbaar [hier](https://support.microsoft.com/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Een SSH-client. het is raadzaam [PuTTY](http://www.putty.org/).

In dit voorbeeld is getest op een Windows 10-PC; u moet kunnen worden uitgevoerd vanaf elke Windows-machine, met inbegrip van Azure Data Science Virtual Machines. De Azure CLI is geïnstalleerd vanaf een MSI-bestand volgens [deze instructies](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials). Kleine wijzigingen vereist (bijvoorbeeld wijzigingen in filepaths) kunnen worden uitgevoerd wanneer dit voorbeeld op macOS.

### <a name="set-up-azure-resources"></a>Azure-resources instellen

In dit voorbeeld is vereist voor een HDInsight Spark-cluster en Azure storage-account aan de relevante host-bestanden. Volg deze instructies voor het maken van deze resources in een nieuwe Azure-resourcegroep:

#### <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Luchtfoto Afbeeldingsclassificatie' en selecteer de sjabloon
5.  Klik op **Maken**.
 
#### <a name="create-the-resource-group"></a>De resourcegroep maken

1. Na het laden van uw project in Azure Machine Learning Workbench, open een opdrachtregelinterface (CLI) door te klikken op bestand -> opdrachtprompt openen.
    Deze versie van de CLI gebruiken voor het merendeel van de zelfstudie. (Indien vermeld, wordt u gevraagd om te openen van een andere versie van de CLI voor Batch AI-resources voorbereiden.)

1. Met de opdrachtregelinterface, meld u aan bij uw Azure-account met de volgende opdracht:

    ```
    az login
    ```

    U wordt gevraagd om naar een URL en een type in een opgegeven tijdelijke code; te gaan de website vraagt om de referenties van uw Azure-account.
    
1. Wanneer de aanmelding is voltooid, gaat u terug naar de CLI en voer de volgende opdracht uit om te bepalen welke Azure-abonnementen zijn beschikbaar voor uw Azure-account:

    ```
    az account list
    ```

    Met deze opdracht worden alle abonnementen die zijn gekoppeld aan uw Azure-account. Zoek de ID van het abonnement dat u wilt gebruiken. Schrijf de abonnements-ID indien vermeld in de volgende opdracht, klikt u vervolgens het actieve abonnement instellen door het uitvoeren van de opdracht:

    ```
    az account set --subscription [subscription ID]
    ```

1. De Azure-resources die zijn gemaakt in dit voorbeeld worden samen worden opgeslagen in een Azure-resourcegroep. Kies een unieke Resourcegroepnaam en schrijf deze waar van toepassing, uitvoeren van beide opdrachten voor het maken van de Azure-resourcegroep:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>Het opslagaccount maken

Nu maken we het opslagaccount dat hosts projectbestanden die moet worden gebruikt door de HDInsight Spark.

1. Kies een unieke opslagaccountnaam en schrijf deze indien vermeld in de volgende `set` opdracht en vervolgens een Azure storage-account maken door het uitvoeren van beide opdrachten:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. De opslagaccountsleutels lijst met de volgende opdracht:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Noteer de waarde van `key1` als de toegangssleutel van het opslagaccount in de volgende opdracht uit, voert u de opdracht voor het opslaan van de waarde.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Maak een bestandsshare met de naam `baitshare` in uw opslagaccount met de volgende opdracht:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. Laad in uw favoriete teksteditor de `settings.cfg` bestand van het Azure Machine Learning Workbench-project "Code" submap, en het storage-accountnaam en de sleutel invoegen, zoals wordt aangegeven. Opslaan en sluiten de `settings.cfg` bestand.
1. Als u hebt nog niet gedaan, downloadt en installeert de [AzCopy](http://aka.ms/downloadazcopy) hulpprogramma. Zorg ervoor dat het uitvoerbare bestand van AzCopy op het systeempad door te typen van 'AzCopy' en druk op Enter om weer te geven van de bijbehorende documentatie.
1. Geef de volgende opdrachten kunt u alle van de voorbeeldgegevens, dat modellen en model trainingsscripts kopiëren naar de juiste locaties in uw storage-account:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Verwacht dat de bestandsoverdracht naar duurt ongeveer één uur. Terwijl u wacht, kunt u doorgaan met de volgende sectie: u moet mogelijk een andere Command Line Interface via Workbench openen en er de tijdelijke variabelen definiëren.

#### <a name="create-the-hdinsight-spark-cluster"></a>De HDInsight Spark-cluster maken

Onze aanbevolen methode voor het maken van een HDInsight-cluster maakt gebruik van de HDInsight Spark-cluster resource manager-sjabloon opgenomen in de submap "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" van dit project.

1. De sjabloon voor HDInsight Spark-cluster is het bestand 'template.json' onder de submap "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning" van dit project. Standaard maakt de sjabloon een Spark-cluster met 40 worker-knooppunten. Als u dit nummer aanpassen moet, opent u de sjabloon in uw favoriete teksteditor en alle exemplaren van "40" vervangen door het aantal worker-knooppunt van uw keuze.
    - U tegenkomen-geheugen fouten later als het aantal worker-knooppunten die u kiest kleiner is. Bestrijden bij geheugenfouten, mag u de trainings- en uitoefening scripts uitvoeren op een subset van de beschikbare gegevens zoals verderop in dit document beschreven.
2. Kies een unieke naam en het wachtwoord voor het HDInsight-cluster en schrijft ze indien vermeld in de volgende opdracht: vervolgens het cluster te maken met behulp van de opdrachten:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

De implementatie van uw cluster duurt maximaal 30 minuten (met inbegrip van de uitvoering van inrichting en het script de actie).

### <a name="set-up-batch-ai-resources"></a>Batch AI-resources instellen

Terwijl u voor uw Storage-account bestandsoverdracht en implementatie van Spark-cluster om te voltooien wacht, kunt u de Batch AI Network File Server (NFS)- als GPU-cluster voorbereiden. Open de Azure CLI-prompt en voer de volgende opdracht uit:

```
az --version 
```

Bevestig dat `batchai` is in de lijst met geïnstalleerde modules. Zo niet, u gebruikt een andere Command Line Interface (bijvoorbeeld via de Workbench geopend).

Controleer vervolgens de registratie van de resourceprovider is voltooid. (Registratie van de provider duurt maximaal 15 minuten en is mogelijk nog steeds lopende als u onlangs voltooid de [Batch AI-installatie-instructies](https://github.com/Azure/BatchAI/tree/master/recipes).) Bevestig dat zowel 'Microsoft.Batch' en 'Microsoft.BatchAI' worden weergegeven met de status 'Registered' in de uitvoer van de volgende opdracht uit:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Als dat niet het geval is, voer de volgende provider registratie opdrachten en wacht ongeveer 15 minuten voor de registratie te voltooien.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

De volgende opdrachten wijzigen zodat de expressies tussen haakjes vervangen door de waarden die u eerder tijdens het maken van resource-groep en storage-account gebruikt. Sla vervolgens de waarden als variabelen door het uitvoeren van de opdrachten:
```
az account set --subscription [subscription ID]
set AZURE_RESOURCE_GROUP=[resource group name]
set STORAGE_ACCOUNT_NAME=[storage account name]
set STORAGE_ACCOUNT_KEY=[storage account key]
az configure --defaults location=eastus
az configure --defaults group=%AZURE_RESOURCE_GROUP%
```

Identificeren van de map met uw Azure Machine Learning-project (bijvoorbeeld `C:\Users\<your username>\AzureML\aerialimageclassification`). Vervang de waarde tussen haakjes door bestandspad van de map (met geen afsluitende backslash) en voer de opdracht uit:
```
set PATH_TO_PROJECT=[The filepath of your project's root directory]
```
U bent nu klaar om te maken van de Batch AI-resources die nodig zijn voor deze zelfstudie.

#### <a name="prepare-the-batch-ai-network-file-server"></a>Voorbereiden van de bestandsserver van de Batch AI-netwerk

De Batch AI-cluster heeft toegang tot uw trainingsgegevens op een netwerk-bestandsserver. Toegang tot de gegevens mogelijk several-fold sneller bij het openen van bestanden van een NFS vergeleken met een Azure-bestandsshare of een Azure Blob Storage.

1. De volgende opdracht om een netwerk-bestandsserver te maken:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p "Dem0Pa$$w0rd" --vm-size Standard_DS2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Controleer de Inrichtingsstatus van de bestandsserver van uw netwerk met de volgende opdracht:

    ```
    az batchai file-server list
    ```

    Wanneer de 'provisioningState' van de Server met de naam 'landuseclassifier' is 'geslaagd', is het gereed voor gebruik. Verwacht inrichten is ongeveer vijf minuten in beslag nemen.
1. Het IP-adres van uw NFS vinden in de uitvoer van de vorige opdracht (de eigenschap 'fileServerPublicIp' onder 'mountSettings'). Schrijven het IP-adres adres indien vermeld in de volgende opdracht en sla de waarde door het uitvoeren van de opdracht:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. Met behulp van uw favoriete SSH-hulpprogramma (het volgende voorbeeld maakt gebruik van opdracht [PuTTY](http://www.putty.org/)), uitvoeren van dit project `prep_nfs.sh` script op de NFS om over te dragen van de installatiekopie van het trainings- en validatie wordt er ingesteld.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Wees niet bezorgd als van de gegevens downloaden en uitpakken van de updates van de voortgang naar in de shell-venster dus snel schuift dat ze onleesbaar zijn.

Indien gewenst, kunt u bevestigen dat de gegevensoverdracht heeft soepel als gepland door te melden bij de bestandsserver met uw favoriete SSH-hulpprogramma en controleren van de `/mnt/data` mapinhoud. Vindt u twee mappen, training_images en validation_images, elk met met submappen met de naam op basis van land categorieën gebruiken.  De sets trainings- en validatie mag ~ 44 k en ~ 11 k afbeeldingen, respectievelijk.

#### <a name="create-a-batch-ai-cluster"></a>Een Batch AI-cluster maken

1. Maken van het cluster door de volgende opdracht:

    ```
    az batchai cluster create -n landuseclassifier2 -u demoUser -p "Dem0Pa$$w0rd" --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 --storage-account-name %STORAGE_ACCOUNT_NAME% 
    ```

1. Gebruik de volgende opdracht om te controleren of dat uw clusterinrichting status:

    ```
    az batchai cluster list
    ```

    Wanneer de toewijzingsstatus van de voor het cluster met de naam 'landuseclassifier' wijzigingen van de grootte van stabiel, is het mogelijk om te verzenden van taken. Taken starten echter niet wordt uitgevoerd totdat alle virtuele machines in het cluster nog over de status van de 'voorbereiden'. Als de eigenschap "fouten" van het cluster niet null zijn is, er is een fout opgetreden tijdens het maken van een cluster en mag niet worden gebruikt.

#### <a name="record-batch-ai-training-credentials"></a>Record Batch AI Training-referenties

Terwijl u op toewijzing van de cluster om uit te voeren wacht, opent u de `settings.cfg` -bestand van de submap "Code" van dit project in de teksteditor van uw keuze. De volgende variabelen met uw eigen referenties bijwerken:
- `bait_subscription_id` (de ID van de Azure-abonnement van 36 tekens)
- `bait_aad_client_id` (de Azure Active Directory-toepassing/client-ID die worden vermeld in de sectie "Vereisten")
- `bait_aad_secret` (de Azure Active Directory toepassingsgeheim vermeld in de sectie "Vereisten")
- `bait_aad_tenant` (de Azure Active Directory-tenant-ID die worden vermeld in de sectie "Vereisten")
- `bait_region` (op dit moment de enige optie is: VS-Oost)
- `bait_resource_group_name` (de resourcegroep waarin die u eerder hebt gekozen)

Als u deze waarden hebt toegewezen, moeten de gewijzigde regels van het bestand settings.cfg eruitzien zoals in de volgende tekst:

```
[Settings]
    # Credentials for the Azure Storage account
    storage_account_name = yoursaname
    storage_account_key = kpIXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXQ==
    
    # Batch AI training credentials
    bait_subscription_id = 0caXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX9c3
    bait_aad_client_id = d0aXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX7f8
    bait_aad_secret = ygSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX6I=
    bait_aad_tenant = 72fXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXb47
    bait_region = eastus
    bait_resource_group_name = yourrgname
```

Opslaan en sluiten `settings.cfg`.

U kunt de CLI-venster waarin u de Batch AI-opdrachten voor het maken van resource uitgevoerd nu sluiten. Alle verdere stappen in deze zelfstudie gebruikt u een gestart vanuit Azure Machine Learning Workbench CLI.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Voorbereiden van de uitvoeringsomgeving van de Azure Machine Learning Workbench

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Meld u aan het HDInsight-cluster als een Azure Machine Learning Workbench compute-doel

Nadat het maken van HDInsight-cluster voltooid is, meld u aan het cluster als een compute-doel voor uw project als volgt:

1.  Voert de volgende opdracht uit vanuit de Azure Machine Learning Command Line Interface:

    ```
    az ml computetarget attach cluster --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD%
    ```

    Deze opdracht voegt u twee bestanden `myhdi.runconfig` en `myhdi.compute`, aan van uw project `aml_config` map.

1. Open de `myhdi.compute` bestand in uw favoriete teksteditor. Wijzig de `yarnDeployMode: cluster` regel lezen `yarnDeployMode: client`, opslaan en sluiten van het bestand.
1. Voer de volgende opdracht om voor te bereiden voor gebruik in uw HDInsight-omgeving:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Lokale afhankelijkheden installeren

Open een CLI van Azure Machine Learning Workbench en installeer afhankelijkheden die nodig zijn voor lokale uitvoering door de volgende opdracht:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Gegevens verzamelen en begrijpen

In dit scenario maakt gebruik van openbaar beschikbare Luchtfoto beeldmateriaal gegevens uit de [nationale landbouw beeldmateriaal programma](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) resolutie van 1-meter. We hebben gegenereerd sets 224 pixel x 224 pixel PNG-bestanden uit de oorspronkelijke NAIP gegevens bijgesneden en gesorteerd op basis van land gebruik labels van de [nationale Land betrekking hebben op Database](https://www.mrlc.gov/nlcd2011.php). Een van voorbeeldafbeelding met het label 'Developed' wordt weergegeven op volledige grootte:

![De tegel van een voorbeeld van ontwikkelde land](media/scenario-aerial-image-classification/sample-tile-developed.png)

Klasse verdeeld sets met ~ 44 k en 11 k installatiekopieën worden gebruikt voor het trainen van het model en validatie, respectievelijk. We laten zien modelimplementatie op een installatiekopie van de k ~ 67 verdelen Middlesex regio, MA - startpagina van Microsoft nieuw Engeland onderzoek en ontwikkeling (KLUNGELIG) center ingesteld. Zie voor meer informatie over hoe deze sets installatiekopie zijn opgebouwd, de [perfect parallelle Afbeeldingsclassificatie git-opslagplaats](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Locatie van Middlesex regio, Massachusetts](media/scenario-aerial-image-classification/middlesex-ma.png)

Tijdens de installatie zijn de Luchtfoto installatiekopie die wordt gebruikt in dit voorbeeld overgebracht naar het opslagaccount dat u hebt gemaakt. De training, validatie en uitoefening afbeeldingen zijn alle 224 pixel x 224 pixel PNG-bestanden met een resolutie van één pixel per vierkante meter. De training en validatie-installatiekopieën hebben is georganiseerd in submappen op basis van hun naam land. (De labels van de grond gebruiken van de installatiekopieën uitoefening onbekend zijn en in veel gevallen niet-eenduidige; sommige van deze installatiekopieën meerdere land typen bevatten). Zie voor meer informatie over hoe deze sets installatiekopie zijn opgebouwd, de [perfect parallelle Afbeeldingsclassificatie git-opslagplaats](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

Voorbeeld van de afbeeldingen in uw Azure storage-account (optioneel) weergeven:
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar de naam van uw opslagaccount in de zoekbalk aan de bovenkant van het scherm. Klik op uw storage-account in de lijst met zoekresultaten.
2. Klik op de koppeling 'Blobs' in het hoofdvenster de storage-account.
3. Klik op de container met de naam "train." U ziet een lijst met mappen met de naam op basis van land gebruikt.
4. Klik op een van deze mappen laden van de lijst met installatiekopieën die deze bevat.
5. Klik op een andere afbeelding en downloaden om de installatiekopie weer te geven.
6. Als gewenst is, klikt u op de containers met de naam 'test' en 'middlesexma2016' om de inhoud ook weer te geven.

## <a name="modeling"></a>Modelleren

### <a name="training-models-with-azure-batch-ai"></a>Modellen met Azure Batch AI training

De `run_batch_ai.py` script in de submap "Code\02_Modeling" van het Workbench-project wordt gebruikt om uit te geven van een taak met Batch AI-Training. Deze taak retrains een afbeelding classificatie DNN gekozen door de gebruiker (AlexNet of ResNet 18 dat ImageNet). De diepte van de retraining kan ook worden opgegeven: te opnieuw trainen van alleen de laatste laag van het netwerk kan verminderen als paar training voorbeelden beschikbaar, zijn terwijl het hele netwerk aan te passen (of voor AlexNet, de volledig verbonden lagen) kan leiden tot meer model prestaties bij de trainingset voldoende is.

Wanneer de trainingstaak is voltooid, wordt het model (samen met een bestand met een beschrijving van de toewijzing tussen geheel getal-uitvoer van het model en de labels tekenreeks) en de voorspellingen van dit script opgeslagen naar blob-opslag. Logboekbestand van de taak BAIT wordt geparseerd om uit te pakken de timecourse van de fout percentage verbetering via de epoches training. De fout percentage verbetering timecourse is geregistreerd in van de Workbench AML uitvoeringsgeschiedenis functie voor later kunt bekijken.

Selecteer een naam voor het getrainde model, een modeltype dat en een retraining diepte. Schrijf die uw selecties indien vermeld in de volgende opdracht vervolgens opnieuw trainen start door het uitvoeren van een Azure ML Command Line Interface van de opdracht:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Verwacht dat de Azure Machine Learning uitvoeren om ongeveer een half uur te voltooien. U wordt aangeraden een paar vergelijkbare opdrachten (variëren de uitvoernaam van het model, het modeltype dat en de diepte van de retraining) uit te voeren zodat u de prestaties van modellen die zijn getraind met verschillende methoden kunt vergelijken.

### <a name="training-models-with-mmlspark"></a>Training-modellen met MMLSpark

De `run_mmlspark.py` script in de submap "Code\02_Modeling" van het Workbench-project wordt gebruikt met het trainen van een [MMLSpark](https://github.com/Azure/mmlspark) modellen voor classificatie van afbeeldingen. De eerste featurizes van het script op de training ingesteld afbeeldingen met behulp van een installatiekopie-classificatie DNN dat op de ImageNet-gegevensset (AlexNet of een ResNet 18-laag). Het script gebruikt vervolgens de installatiekopieën boommodel om een MMLSpark-model (een willekeurige forest of een model voor logistieke regressie) voor het classificeren van de afbeeldingen te trainen. De afbeelding testset is vervolgens boommodel en beoordeeld met het getrainde model. De nauwkeurigheid van de modelvoorspellingen voor de test wordt berekend en in het Azure Machine Learning Workbench van uitvoeringsgeschiedenis functie. Ten slotte worden het getrainde MMLSpark-model en de voorspellingen voor de test opgeslagen op BLOB storage.

Selecteer de naam van een unieke uitvoer-model voor het getrainde model, een modeltype dat en een type MMLSpark-model. Schrijf die uw selecties indien vermeld in de volgende opdrachtsjabloon vervolgens beginnen met het opnieuw trainen door het uitvoeren van een Azure ML Command Line Interface van de opdracht:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Een extra `--sample_frac` parameter kan worden gebruikt om te trainen en testen van het model met een subset van de beschikbare gegevens. Met behulp van een klein deel deze vereisten runtime en het geheugen ten koste van de nauwkeurigheid van het getrainde model. (Bijvoorbeeld, een uitvoering met `--sample_frac 0.1` wordt verwacht dat ongeveer twintig minuten duren.) Voor meer informatie over deze en andere parameters uitgevoerd `python Code\02_Modeling\run_mmlspark.py -h`.

Gebruikers worden aangemoedigd om uit te voeren met dit script meerdere keren met verschillende invoerparameters die zijn opgegeven. De prestaties van de resulterende modellen kan vervolgens worden vergeleken in Azure Machine Learning Workbench van Uitvoeringsgeschiedenis functie.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Met de functie Workbench Run History modelprestaties vergelijken

Nadat u hebt uitgevoerd twee of meer training wordt uitgevoerd van beide typen, gaat u naar de functie Uitvoeringsgeschiedenis in Workbench door te klikken op het klokpictogram langs de links in de menubalk. Selecteer `run_mmlspark.py` uit de lijst met scripts aan de linkerkant. Een deelvenster laadt de nauwkeurigheid van de test instellen voor alle uitvoeringen vergelijken. Voor meer details, schuif naar beneden en klik op de naam van een afzonderlijke run.

## <a name="deployment"></a>Implementatie

De naam van uw gewenste model invoegen in de volgende opdracht uit om toe te passen op een van uw getrainde modellen Luchtfoto afbeeldingen herhalen Middlesex regio, MA uitvoering op afstand gebruiken op HDInsight, en uitvoeren:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Een extra `--sample_frac` parameter kan worden gebruikt voor het operationeel maken van het model met een subset van de beschikbare gegevens. Met behulp van een klein deel deze vereisten runtime en het geheugen ten koste van de volledigheid voorspelling. Voor meer informatie over deze en andere parameters uitgevoerd `python Code\03_Deployment\batch_score_spark -h`.

Met dit script schrijft van het model voorspellingen naar uw opslagaccount. De voorspellingen kunnen worden onderzocht, zoals beschreven in de volgende sectie.

## <a name="visualization"></a>Visualisatie

De 'Model prediction analysis' Jupyter-notebook in de submap "Code\04_Result_Analysis" van het Workbench-project visualiseert van het model voorspellingen. Laden en voert u de notebook als volgt uit:
1. Open het project in Workbench en klik op de map pictogram in het menu links de mapweergave laden ("bestanden').
2. Navigeer naar de submap 'Code\04_Result_Analysis' en klik op de notebook met de naam "Model prediction analysis." Een weergave van de Preview-versie van de notebook moet worden weergegeven.
3. Klik op 'Start-Notebook Server' voor het laden van de notebook.
4. Voer de naam van het model waarvan u analyseren wilt, waar van toepassing de resultaten in de eerste cel.
5. Klik op ' cel Run -> alle ' voor het uitvoeren van alle cellen in het notitieblok.
6. Lezen, samen met de notebook voor meer informatie over de analyses en visualisaties die dit account.

## <a name="cleanup"></a>Opschonen
Wanneer u het voorbeeld hebt voltooid, wordt u aangeraden dat u alle resources die u hebt gemaakt door het uitvoeren van de volgende opdracht uit de Azure Command Line Interface verwijdert:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Verwijzingen

- [De opslagplaats perfect parallelle classificatie van afbeeldingen](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Beschrijving van gegevensset constructie van verkrijgbare beelden en labels
- [MMLSpark](https://github.com/Azure/mmlspark) GitHub-opslagplaats
   - Bevat aanvullende voorbeelden van modeltraining en -evaluatie met MMLSpark

## <a name="conclusions"></a>Conclusies

Azure Machine Learning Workbench helpt gegevensanalisten hun code op externe compute-doelen eenvoudig te implementeren. In dit voorbeeld lokale MMLSpark training code is geïmplementeerd voor uitvoering op afstand op een HDInsight-cluster en een lokale script gestart een trainingstaak op een Azure Batch AI GPU-cluster. Azure Machine Learning Workbench van uitvoeringsgeschiedenis functie bijgehouden van de prestaties van meerdere modellen en konden we de meest nauwkeurige model identificeren. Van de Workbench Jupyter notebooks functie heeft geholpen de onze modellen voorspellingen in een interactieve, grafische omgeving visualiseren.

## <a name="next-steps"></a>Volgende stappen
Naar verdiep u in dit voorbeeld:
- In Azure Machine Learning Workbench van Uitvoeringsgeschiedenis functie, klikt u op het tandwiel symbolen om te selecteren welke grafieken en metrische gegevens worden weergegeven.
- Bekijk de voorbeelden van scripts voor overzichten aanroepen van de `run_logger`. Controleer dat u begrijpt hoe alle gegevens wordt opgenomen.
- Bekijk de voorbeelden van scripts voor overzichten aanroepen van de `blob_service`. Controleer of u hoe getrainde modellen begrijpt en voorspellingen worden opgeslagen en opgehaald uit de cloud.
- Bekijk de inhoud van de containers in uw blob storage-account hebt gemaakt. Zorg ervoor dat u welke script begrijpt of opdracht verantwoordelijk is voor het maken van elke groep van bestanden.
- Wijzig de trainingsscript naar een ander type van de MMLSpark-model te trainen of te wijzigen van het model hyperparameters. De uitvoeringsgeschiedenis-functie gebruiken om te bepalen of uw wijzigingen verhoogd of verlaagd van nauwkeurigheid van het model.
