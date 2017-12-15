---
title: Classificatie van de installatiekopie van de lucht | Microsoft Docs
description: Bevat instructies voor het werkelijk scenario van de installatiekopie van de lucht classificatie
author: mawah
ms.author: mawah
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
services: machine-learning
ms.date: 12/13/2017
ms.openlocfilehash: 57b81dfb2cb58fb43d4c420e8ce58c0c226316df
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="aerial-image-classification"></a>De installatiekopie van de lucht classificatie

In dit voorbeeld laat zien hoe Azure Machine Learning Workbench coördineren gedistribueerde trainings- en uitoefening van afbeelding classificatiemodellen kunnen gebruiken. We twee methoden gebruiken voor training: i een diepe neurale netwerk met verfijnen een [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) GPU-cluster, en (ii) met de [Microsoft Machine Learning voor Apache Spark (MMLSpark)](https://github.com/Azure/mmlspark) pakket naar featurize afbeeldingen met pretrained CNTK modellen en training van classificaties van de afgeleide functies. We vervolgens getraind modellen in parallelle wijze toepassen op grote afbeelding sets in de cloud met behulp van een [Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) cluster, zodat we de snelheid van training en uitoefening schalen door het toevoegen of verwijderen van worker-knooppunten.

In dit voorbeeld worden twee manieren om over te dragen learning die gebruikmaakt van pretrained modellen om te voorkomen dat de kosten van training diep neural networks helemaal gemarkeerd. Een diep neurale netwerk wordt doorgaans retraining GPU compute vereist, maar kan leiden tot nauwkeuriger wanneer trainingset groot genoeg is. Een eenvoudige classificatie op featurized installatiekopieën training vereist geen GPU compute, is inherent snel en willekeurig schaalbare en minder parameters past. Deze methode is daarom een uitstekende keuze wanneer enkele voorbeelden van training beschikbaar zijn--is vaak het geval voor aangepaste gebruiksvoorbeelden. 

Het Spark-cluster gebruikt in dit voorbeeld heeft 40 worker-knooppunten en kosten ~$40/hr te oefenen; de Batch AI-clusterbronnen acht GPU's bevatten en kosten ~$10/hr werken. Deze stapsgewijze Kennismaking voltooien duurt ongeveer drie uur. Wanneer u klaar bent, de instructies opruimen verwijderen van de resources die u hebt gemaakt en stoppen steeds kosten.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

De openbare GitHub-opslagplaats voor dit scenario werkelijkheid bevat alle materialen, inclusief codevoorbeelden, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>Case beschrijving gebruiken

In dit scenario trainen we machine learning-modellen voor het classificeren van het type van de grond wordt weergegeven in de lucht afbeeldingen van 224 meter x 224 meter waarnemingspunten. Land gebruik classificatiemodellen kunnen kunnen worden gebruikt voor het bijhouden van urbanization, ontbossing, verlies van wetlands en andere belangrijke milieu trends regelmatig met lucht beelden verzameld. We hebt trainings- en validatie installatiekopie sets op basis van beelden van de Verenigde Staten voorbereid Nationale land beelden programma en land labels die zijn gepubliceerd door het Amerikaanse gebruiken National Land behandeld Database. Installatiekopieën van het voorbeeld in elk land gebruik klasse worden hieronder weergegeven:

![Voorbeeld van de regio's voor elk land label gebruiken](media/scenario-aerial-image-classification/example-labels.PNG)

Na de trainings- en valideren van het model classificatie wordt we toepassen op lucht installatiekopieën spanning Middlesex regio, MA--startpagina van Microsoft van nieuw Engeland Research & Center ontwikkeling (KLUNGELIG)--te laten zien hoe deze modellen kunnen worden gebruikt om trends in stedelijk bestuderen ontwikkeling.

Voor het produceren van een installatiekopie classificatie met transfer learning gegevenswetenschappers vaak meerdere modellen met een aantal methoden maken en selecteer de meest zodat model. Azure Machine Learning Workbench kan helpen bij het gegevens verzameld training coördineren in omgevingen met verschillende rekenscenario, volgen en te vergelijken de prestaties van meerdere modellen en een gekozen model van toepassing op grote gegevenssets in de cloud.

## <a name="scenario-structure"></a>Scenario-structuur

In dit voorbeeld bevinden image-gegevens en pretrained modellen zich op een Azure storage-account. Een Azure HDInsight Spark-cluster deze bestanden leest en Hiermee wordt een installatiekopie van classificatie-model met MMLSpark. Het getrainde model en de voorspellingen worden vervolgens geschreven naar het opslagaccount, waar ze kunnen worden geanalyseerd en weergegeven door een Jupyter-notebook die lokaal wordt uitgevoerd. Azure Machine Learning Workbench coördineert externe uitvoering van scripts op het Spark-cluster. Het houdt ook nauwkeurigheid metrische gegevens voor meerdere modellen getraind met verschillende methoden, zodat de gebruiker het meest zodat model selecteren.

![Overzicht voor de lucht installatiekopie classificatie werkelijk scenario](media/scenario-aerial-image-classification/scenario-schematic.PNG)

De [Stapsgewijze instructies](https://github.com/MicrosoftDocs/azure-docs-pr/tree/release-ignite-aml-v2/articles/machine-learning/) beginnen met het leidt u door het maken en de voorbereiding van een Azure storage-account en Spark-cluster, inclusief de overdracht en afhankelijkheid installatie van gegevens. Ze beschrijven vervolgens training taken starten en de prestaties van de resulterende modellen vergelijken. Ten slotte ze te laten zien hoe een gekozen model toepassen op een set grote afbeelding op het Spark-cluster en de resultaten van de voorspelling lokaal te analyseren.


## <a name="set-up-the-execution-environment"></a>Instellen van de uitvoeringsomgeving

De volgende instructies leiden u door het proces voor het instellen van de uitvoeringsomgeving voor dit voorbeeld.

### <a name="prerequisites"></a>Vereisten
- Een [Azure-account](https://azure.microsoft.com/en-us/free/) (gratis proefversies zijn beschikbaar)
    - Maakt u een HDInsight Spark-cluster met 40 worker-knooppunten (168 kernen totale). Zorg ervoor dat uw account voldoende beschikbare kernen aan de hand van 'Gebruik + quota' tabblad voor uw abonnement in Azure-portal.
       - Als u minder cores beschikbaar hebt, kunt u de sjabloon HDInsight-cluster als u wilt verkleinen het aantal werknemers ingericht mag wijzigen. Instructies hiervoor worden weergegeven onder de sectie 'De HDInsight Spark-cluster maken'.
    - Dit voorbeeld maakt u een Batch AI Training-cluster met twee NC6 (1 GPU, 6 vCPU) virtuele machines. Zorg ervoor dat uw account voldoende beschikbare kernen in de regio VS-Oost aan de hand van 'Gebruik + quota' tabblad voor uw abonnement in Azure-portal.
- [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md)
    - Ga als volgt de [installeren en het maken van de Quick Start](quickstart-installation.md) voor het installeren van de Azure Machine Learning-Workbench en experimenteren en Model Management Accounts maken.
- [Batch-AI](https://github.com/Azure/BatchAI) Python SDK en Azure CLI 2.0
    - Voltooi de volgende secties in de [Batch AI recepten Leesmij](https://github.com/Azure/BatchAI/tree/master/recipes):
        - 'Vereisten'
        - 'Maken en uw toepassing met Azure Active Directory (AAD)'
        - 'Registreren BatchAI Resourceproviders' (onder ' uitvoeren met behulp van Azure CLI 2.0' recepten)
        - "Azure Batch AI-Management-Client installeren"
        - "Azure Python SDK installeren"
    - De client-ID-record, geheim en tenant-ID van de Azure Active Directory-toepassing die wordt u omgeleid naar maken. U gebruikt deze referenties verderop in deze zelfstudie.
    - Op dit moment van schrijven gebruik Azure Machine Learning Workbench en Azure Batch AI van afzonderlijke recoveryforks van de Azure CLI 2.0. Ter verduidelijking is de Workbench-versie van de CLI als 'een CLI van Azure Machine Learning Workbench gestart' en de algemene release-versie (waaronder Batch AI) 'Azure CLI 2.0'.
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy), wordt er een gratis hulpprogramma voor het coördineren van bestandsoverdracht tussen Azure storage-accounts
    - Zorg ervoor dat de map met het AzCopy uitvoerbare bestand op uw systeem pad-omgevingsvariabele. (Instructies over het wijzigen van omgevingsvariabelen zijn beschikbaar [hier](https://support.microsoft.com/en-us/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Een SSH-client. het is raadzaam [PuTTY](http://www.putty.org/).

In dit voorbeeld is getest op een Windows 10-PC; u moet mogelijk vanaf alle Windows-computers, met inbegrip van virtuele Machines van Azure Data wetenschappelijke uitvoeren. De Azure CLI 2.0 is geïnstalleerd vanaf een MSI volgens [deze instructies](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials). Kleine wijzigingen vereist (bijvoorbeeld wijzigingen in filepaths) kunnen worden uitgevoerd wanneer dit voorbeeld op Mac OS.

### <a name="set-up-azure-resources"></a>Azure-resources instellen

In dit voorbeeld vereist een HDInsight Spark-cluster en een Azure storage-account op de relevante hostbestanden. Volg deze instructies voor het maken van deze resources in een nieuwe Azure-resourcegroep:

#### <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Lucht installatiekopie classificatie' en selecteer de sjabloon
5.  Klik op **Maken**.
 
#### <a name="create-the-resource-group"></a>De resourcegroep maken

1. Na het laden van uw project in Azure Machine Learning Workbench, open een opdrachtregelinterface (CLI) door te klikken op bestand -> opdrachtprompt openen.
    Deze versie van de CLI gebruiken voor het merendeel van de zelfstudie. (Indien aangegeven, wordt u gevraagd een andere versie van de CLI om voor te bereiden AI Batch-resources te openen.)

1. Met de opdrachtregelinterface, meld u aan bij uw Azure-account met de volgende opdracht:

    ```
    az login
    ```

    U wordt gevraagd te gaat u naar een URL en een type in de opgegeven tijdelijke code; de website vraagt om de referenties van uw Azure-account.
    
1. Wanneer de aanmelding is voltooid, terug naar de CLI en voer de volgende opdracht om te bepalen welke Azure-abonnementen beschikbaar zijn voor uw Azure-account:

    ```
    az account list
    ```

    Met deze opdracht worden alle abonnementen die zijn gekoppeld aan uw Azure-account. De ID van het abonnement dat u wilt gebruiken gevonden. Schrijven van de abonnements-ID Stel aangegeven in de volgende opdracht vervolgens actief abonnement door de opdracht wordt uitgevoerd:

    ```
    az account set --subscription [subscription ID]
    ```

1. De Azure-resources in dit voorbeeld hebt gemaakt, worden samen worden opgeslagen in een Azure-resourcegroep. Kies een unieke resource-groepsnaam en schrijven indien aangegeven, vervolgens beide opdrachten voor het maken van de Azure-resourcegroep wordt uitgevoerd:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>De storage-account maken

We maken nu het opslagaccount dat hosts projectbestanden die moet worden gebruikt door de HDInsight Spark.

1. Kies de naam van een uniek account en schrijf deze indien aangegeven in de volgende `set` opdracht en vervolgens een Azure storage-account maken door het uitvoeren van beide opdrachten:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. De opslagaccountsleutels lijst met de volgende opdracht:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Leg de waarde van `key1` als de opslagsleutel in de volgende opdracht, voer vervolgens de opdracht voor het opslaan van de waarde.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Maken van een bestandsshare met de naam `baitshare` in uw opslagaccount met de volgende opdracht:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. In uw favoriete teksteditor laden de `settings.cfg` bestand van de submap 'Code' van het Azure Machine Learning Workbench project en plaatst u de naam van het opslagaccount en sleutel aangegeven. Sla op en sluit de `settings.cfg` bestand.
1. Als u dit nog niet hebt gedaan, downloadt en installeert de [AzCopy](http://aka.ms/downloadazcopy) hulpprogramma. Zorg ervoor dat het uitvoerbare bestand van AzCopy op uw systeempad door te voeren 'AzCopy' en druk op Enter om weer te geven van de bijbehorende documentatie.
1. Voert de volgende opdrachten om te kopiëren alle de voorbeeldgegevens, pretrained modellen en model training scripts naar de juiste locaties in uw opslagaccount:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Bestandsoverdracht naar ongeveer één uur duren voordat verwacht. Terwijl u wacht, kunt u doorgaan met de volgende sectie: moet u mogelijk een andere opdrachtregelinterface via Workbench openen en tijdelijke variabelen definiëren.

#### <a name="create-the-hdinsight-spark-cluster"></a>De HDInsight Spark-cluster maken

De aanbevolen methode voor het maken van een HDInsight-cluster maakt gebruik van de HDInsight Spark-cluster resource manager-sjabloon opgenomen in de submap 'Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning' van dit project.

1. De sjabloon HDInsight Spark-cluster is het bestand 'template.json' in de submap 'Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning' van dit project. De sjabloon maakt standaard een Spark-cluster met 40 worker-knooppunten. Als u dit nummer aanpassen moet, opent u de sjabloon in uw favoriete teksteditor en Vervang alle exemplaren van "40" met het aantal worker-knooppunt van uw keuze.
    - U tegenkomen-geheugen fouten later als het aantal worker-knooppunten die u kiest kleiner is. Bestrijden geheugenfouten, kunt u de trainings- en uitoefening scripts uitvoeren op een subset van de beschikbare gegevens zoals verderop in dit document worden beschreven.
2. Kies een unieke naam en het wachtwoord voor het HDInsight-cluster en schrijf deze indien aangegeven in de volgende opdracht: maken van het cluster vervolgens door uitgifte van de opdrachten:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Implementatie van het cluster kan tot 30 minuten (inclusief de actie uitvoeren voor inrichting en het script) duren.

### <a name="set-up-batch-ai-resources"></a>AI Batch-resources instellen

Terwijl u voor uw Storage-account bestandsoverdracht en Spark-cluster-implementatie wacht te voltooien, kunt u het cluster Batch AI Server NFS (Network File) en GPU voorbereiden. Open een opdrachtprompt Azure CLI 2.0 en voer de volgende opdracht:

```
az --version 
```

Bevestig dat `batchai` is in de lijst met geïnstalleerde modules. Als dit niet het geval is, kan u een andere opdrachtregelinterface (bijvoorbeeld een geopend via de Workbench).

Controleer vervolgens de registratie van de provider is voltooid. (Registratie van de provider nodig is tot vijftien minuten en is mogelijk nog steeds lopende als u onlangs voltooid de [Batch AI-installatie-instructies](https://github.com/Azure/BatchAI/tree/master/recipes).) Zorg ervoor dat zowel 'Microsoft.Batch' en 'Microsoft.BatchAI' worden weergegeven met de status 'Geregistreerd' in de uitvoer van de volgende opdracht:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Als dit niet het geval is, voer de volgende provider registratie opdrachten en wacht ~ 15 minuten om registratie te voltooien.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

Wijzig de volgende opdrachten de expressies tussen haakjes vervangt door de waarden die u eerder tijdens het maken van resource groep en storage account gebruikt. De waarden als variabelen vervolgens opslaan door het uitvoeren van de opdrachten:
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
U bent nu klaar om te maken van de Batch AI-bronnen nodig voor deze zelfstudie.

#### <a name="prepare-the-batch-ai-network-file-server"></a>De Batch netwerk AI-bestandsserver voorbereiden

Uw Batch-AI-cluster heeft toegang tot uw trainingsgegevens op een netwerkserver. Toegang tot gegevens mogelijk several-fold sneller bij het openen van bestanden van een NFS versus een Azure-bestandsshare of Azure Blob Storage.

1. Geef de volgende opdracht voor het maken van een netwerkserver:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p Dem0Pa$$w0rd --vm-size Standard_DS2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Controleer de status van de inrichting van uw netwerk-bestandsserver met de volgende opdracht:

    ```
    az batchai file-server list
    ```

    Wanneer de 'provisioningState' van de Server met de naam 'landuseclassifier' 'geslaagd' is, is deze klaar voor gebruik. Verwachten inrichten is ongeveer vijf minuten duren.
1. Het IP-adres van uw NFS vinden in de uitvoer van de vorige opdracht (de eigenschap 'fileServerPublicIp' onder 'mountSettings'). Wegschrijven naar het IP-adres adres indien aangegeven in de volgende opdracht en vervolgens de waarde door het uitvoeren van de opdracht opslaan:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. Met behulp van uw favoriete SSH-hulpprogramma (voorbeeld van de volgende opdracht gebruikt [PuTTY](http://www.putty.org/)), uitvoeren van dit project `prep_nfs.sh` script op basis van de NFS om over te dragen van de installatiekopie van het trainings- en validatie wordt er ingesteld.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Niet bezorgd worden als van de gegevens downloaden en uitpakken van de updates van de voortgang in het venster shell dus snel schuift dat ze onleesbaar zijn.

Indien gewenst, kunt u bevestigen dat de gegevensoverdracht is verder als gepland door aanmelden bij de bestandsserver met uw favoriete SSH-hulpprogramma en het controleren van de `/mnt/data` mapinhoud. Vindt u twee mappen, training_images en validation_images, elk met een naam volgens de grond submappen met categorieën gebruiken.  De training en validatie sets ~ 44 moeten bevatten k en ~ 11 k afbeeldingen, respectievelijk.

#### <a name="create-a-batch-ai-cluster"></a>Een Batch AI-cluster maken

1. Het cluster hebt gemaakt door de volgende opdracht:

    ```
    az batchai cluster create -n landuseclassifier2 -u demoUser -p Dem0Pa$$w0rd --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 --storage-account-name %STORAGE_ACCOUNT_NAME% 
    ```

1. Gebruik de volgende opdracht om te controleren van dat uw clusterinrichting status:

    ```
    az batchai cluster list
    ```

    Wanneer de status van de toewijzing voor het cluster met de naam 'landuseclassifier' wijzigingen van de grootte van stabiel, is het mogelijk om taken te verzenden. Taken starten echter niet uitgevoerd totdat alle VM's in het cluster nog de status 'voorbereiden'. Als de Fouteneigenschap '' van het cluster niet null is, is een fout opgetreden tijdens het maken van het cluster en mag niet worden gebruikt.

#### <a name="record-batch-ai-training-credentials"></a>Record Batch AI Training referenties

Terwijl u op toewijzing van de cluster wacht wilt voltooien, opent u de `settings.cfg` bestand uit de submap 'Code' van dit project in de teksteditor van uw keuze. De volgende variabelen met uw eigen referenties bijwerken:
- `bait_subscription_id`(uw 36 tekens Azure-abonnement-ID)
- `bait_aad_client_id`(de Azure Active Directory-toepassing/client-ID in de sectie 'Vereisten' genoemd)
- `bait_aad_secret`(de Azure Active Directory-toepassingsgeheim in de sectie 'Vereisten' genoemd)
- `bait_aad_tenant`(de Azure Active Directory-tenant-ID in de sectie 'Vereisten' genoemd)
- `bait_region`(op dit moment van schrijven, de enige optie is: eastus)
- `bait_resource_group_name`(de resourcegroep die u eerder hebt gekozen)

Zodra u deze waarden hebt toegewezen, moeten de gewijzigde regels van het bestand settings.cfg zijn vergelijkbaar met de volgende tekst:

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

U kunt het waarop u de Batch AI-opdrachten voor het maken van resource uitgevoerd CLI-venster nu sluiten. Alle verdere stappen in deze zelfstudie gebruiken een CLI van Azure Machine Learning Workbench gestart.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Voorbereiden van de uitvoeringsomgeving van Azure Machine Learning Workbench

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Het HDInsight-cluster als het doel van een Azure Machine Learning Workbench compute registreren

Zodra de HDInsight-cluster maken is voltooid, registreert u het cluster als een compute-doel voor uw project als volgt:

1.  Geef de volgende opdracht uit de Azure Machine Learning opdrachtregelinterface:

    ```
    az ml computetarget attach cluster --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD%
    ```

    Deze opdracht voegt u twee bestanden: `myhdi.runconfig` en `myhdi.compute`, aan uw project `aml_config` map.

1. Open de `myhdi.compute` bestand in uw favoriete teksteditor. Wijzig de `yarnDeployMode: cluster` regel lezen `yarnDeployMode: client`, opslaan en sluit het bestand.
1. Voer de volgende opdracht voor het voorbereiden van uw HDInsight-omgeving voor gebruik:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Lokale afhankelijkheden installeren

Open een CLI van Azure Machine Learning Workbench en afhankelijkheden die nodig zijn voor lokale uitvoering door de volgende opdracht installeren:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Gegevens verzamelen en begrijpen

Dit scenario gebruikt openbaar beschikbaar vanuit de lucht beelden gegevens uit de [nationale land beelden programma](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) 1 meter resolutie. We hebt gegenereerd sets 224 pixel x 224 pixel PNG-bestanden uit de oorspronkelijke NAIP gegevens bijgesneden en gesorteerd op basis van labels van de grond gebruiken van de [nationale Land hebben betrekking op Database](https://www.mrlc.gov/nlcd2011.php). Een voorbeeld met het label 'Developed' afbeelding op volledige grootte:

![Een voorbeeld-tegel ontwikkelde land](media/scenario-aerial-image-classification/sample-tile-developed.png)

Klasse met gelijke taakverdeling sets ~ 44 k en 11 k installatiekopieën worden gebruikt voor model trainings- en validatie, respectievelijk. Ziet u implementatie op een installatiekopie van de k ~ 67 model verdelen Middlesex regio, MA--startpagina van Microsoft New Engeland onderzoek en ontwikkeling (KLUNGELIG) center ingesteld. Zie voor meer informatie over hoe deze installatiekopie sets zijn opgebouwd, de [perfect parallelle installatiekopie classificatie git-opslagplaats](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Locatie van Middlesex regio, Massachusetts](media/scenario-aerial-image-classification/middlesex-ma.png)

Tijdens de installatie zijn de lucht afbeelding die wordt gebruikt in dit voorbeeld worden overgedragen naar het opslagaccount dat u hebt gemaakt. De training, validatie en uitoefening installatiekopieën zijn alle 224 pixel x 224 pixel PNG-bestanden met een resolutie van één pixel per vierkante meter. De trainings- en validatie afbeeldingen hebt is georganiseerd in submappen op basis van hun gebruik land label. (De labels land gebruiken van de installatiekopieën uitoefening onbekend zijn en in veel gevallen dubbelzinnige; sommige van deze installatiekopieën meerdere land typen bevatten.) Zie voor meer informatie over hoe deze installatiekopie sets zijn opgebouwd, de [perfect parallelle installatiekopie classificatie git-opslagplaats](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

Weergeven in voorbeeld afbeeldingen in uw Azure storage-account (optioneel):
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoek de naam van uw opslagaccount in de zoekbalk aan de bovenkant van het scherm. Klik op uw opslagaccount in de zoekresultaten.
2. Klik op de koppeling 'Blobs' in het hoofdvenster de storage-account.
3. Klik op de container met de naam "train." U ziet een lijst met mappen een naam volgens de grond gebruiken.
4. Klik op een van deze mappen laden van de lijst met afbeeldingen bevat.
5. Klik op een afbeelding en downloaden om de installatiekopie weer te geven.
6. Indien gewenst, klik op de containers met de naam 'test' en 'middlesexma2016' ook hun inhoud weer te geven.

## <a name="modeling"></a>Modelleren

### <a name="training-models-with-azure-batch-ai"></a>Training modellen met Azure Batch AI

De `run_batch_ai.py` script in de submap 'Code\02_Modeling' van het project Workbench is gebruikt voor het uitgeven van een taak met Batch AI Training. Deze taak retrains een installatiekopie classificatie DNN gekozen door de gebruiker (AlexNet of ResNet 18 pretrained op ImageNet). De diepte van de retraining kan ook worden opgegeven: te retraining alleen de laatste beveiligingslaag van het netwerk kan verminderen wanneer u enkele voorbeelden van training beschikbaar zijn, tijdens het gehele netwerk aan te passen (of voor AlexNet, de volledig verbonden lagen) kunnen leiden tot groter model prestaties bij de trainingset voldoende is.

Wanneer de taak training is voltooid, wordt het model (samen met een bestand met een beschrijving van de toewijzing tussen geheel getal-uitvoer van het model en de labels tekenreeks) en de voorspellingen van dit script opgeslagen naar blob storage. Logboekbestand van de taak LOKAAS wordt geparseerd om op te halen van de timecourse van verbetering van de fout snelheid via de training epoches. De fout snelheid verbetering timecourse is geregistreerd in AML-Workbench uitvoeringsgeschiedenis functie voor later kunt bekijken.

Selecteer een naam voor het getrainde model, een pretrained modeltype en retraining diepte. Schrijven die uw selecties aangegeven in de volgende opdracht stelt u de retraining door het uitvoeren van de opdracht van een Azure ML opdrachtregelinterface:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Verwacht dat het Azure Machine Learning uitvoeren om ongeveer een half uur te voltooien. Het is raadzaam dat u enkele vergelijkbare opdrachten (variëren de modelnaam uitvoer, het modeltype pretrained en de diepte van de retraining) uitvoert, zodat u de prestaties van modellen getraind met verschillende methoden kunt vergelijken.

### <a name="training-models-with-mmlspark"></a>Training modellen met MMLSpark

De `run_mmlspark.py` script in de submap 'Code\02_Modeling' van het project Workbench wordt gebruikt voor het trainen een [MMLSpark](https://github.com/Azure/mmlspark) model voor classificatie van de installatiekopie. De eerste featurizes van het script op de training ingesteld afbeeldingen met een installatiekopie classificatie die DNN pretrained op de gegevensset ImageNet (AlexNet of een ResNet 18-laag). Het script gebruikt vervolgens de installatiekopieën van het featurized voor het trainen van een model MMLSpark (een willekeurige forest of een logistic regressiemodel) voor het classificeren van de installatiekopieën. De afbeelding testset is vervolgens featurized en berekend met het getrainde model. De nauwkeurigheid van de voorspellingen van het model van de testset is berekend en geregistreerd in Azure Machine Learning-Workbench van uitvoeringsgeschiedenis functie. Ten slotte het getrainde MMLSpark model en de voorspellingen op de testset opgeslagen naar de blob storage.

Selecteer een naam voor het model van unieke uitvoer voor uw getrainde model, een pretrained modeltype en een modeltype MMLSpark. Schrijven die uw selecties aangegeven in de volgende opdrachtsjabloon, stelt u de retraining door het uitvoeren van de opdracht van een Azure ML opdrachtregelinterface:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Een extra `--sample_frac` parameter kan worden gebruikt om te trainen en testen van het model met een subset van beschikbare gegevens. Een kort voorbeeld fractie neemt runtime en geheugen vereisten ten koste van de trained model nauwkeurig af. (Bijvoorbeeld, een uitvoering met `--sample_frac 0.1` wordt verwacht dat ongeveer twintig minuten duren.) Voor meer informatie over deze en andere parameters uitgevoerd `python Code\02_Modeling\run_mmlspark.py -h`.

Gebruikers wordt aangeraden dit script meerdere keren uitvoeren met andere parameters voor invoer. De prestaties van de resulterende modellen kan vervolgens worden vergeleken in Azure Machine Learning-Workbench van geschiedenisfunctie uitvoeren.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Vergelijken model prestaties met behulp van de geschiedenisfunctie Workbench uitvoeren

Nadat u hebt uitgevoerd twee of meer training wordt uitgevoerd van beide typen, gaat u naar de functie Geschiedenis uitvoeren in de Workbench door te klikken op het klokpictogram op de balk menu links. Selecteer `run_mmlspark.py` uit de lijst met scripts aan de linkerkant. Een deelvenster laadt de nauwkeurigheid van de test instellen voor alle sessies te vergelijken. Voor meer details, schuif naar beneden en klik op de naam van een afzonderlijke uitvoering.

## <a name="deployment"></a>Implementatie

Als een van uw getraind modellen wilt lucht installatiekopieën herhalen Middlesex regio, MA uitvoering op afstand met op HDInsight toepassen, de gewenste modelnaam invoegen in de volgende opdracht en deze uitvoeren:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Een extra `--sample_frac` parameter kan worden gebruikt voor het model met een subset van beschikbare gegevens operationeel. Met behulp van een kort voorbeeld fractie verlaagt runtime en geheugen vereisten ten koste van de voorspelling volledigheid. Voor meer informatie over deze en andere parameters uitgevoerd `python Code\03_Deployment\batch_score_spark -h`.

Dit script schrijft de voorspellingen van het model in uw opslagaccount. De voorspellingen kunnen worden onderzocht, zoals beschreven in de volgende sectie.

## <a name="visualization"></a>Visualisatie

De 'Model prediction analysis' Jupyter-notebook in de submap 'Code\04_Result_Analysis' van het project Workbench visualiseren van een model voorspellingen. Laden en voert u de notebook als volgt:
1. Open het project in de Workbench en klik op de map pictogram langs in het menu links de mapweergave laden ('bestanden').
2. Navigeer naar de submap 'Code\04_Result_Analysis' en klik op de notebook met de naam "Model prediction analysis." Een preview-weergave van de notebook moet worden weergegeven.
3. Klik op 'Start-Notebook Server' als u wilt de notebook te laden.
4. Voer de naam van het model waarvan u analyseren wilt waar de resultaten in de eerste cel.
5. Klik op ' cel -> uitvoeren alle ' alle cellen in de notebook uitvoeren.
6. Gelezen samen met de notebook voor meer informatie over de analyses en het geeft visualisaties.

## <a name="cleanup"></a>Opruimen
Wanneer u het voorbeeld hebt voltooid, wordt u aangeraden dat u alle resources die u hebt gemaakt door het uitvoeren van de volgende opdracht uit de Azure opdrachtregelinterface verwijdert:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Verwijzingen

- [De opslagplaats perfect parallelle installatiekopie classificatie](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Beschrijving van gegevensset constructie van gratis beelden en labels
- [MMLSpark](https://github.com/Azure/mmlspark) GitHub-opslagplaats
   - Bevat aanvullende voorbeelden gegeven van het model trainen en evalueren met MMLSpark

## <a name="conclusions"></a>Conclusie trekt

Azure Machine Learning Workbench helpt gegevenswetenschappers eenvoudig implementeren hun code op externe compute-doelen. In dit voorbeeld lokale MMLSpark training code is geïmplementeerd voor uitvoering op afstand op een HDInsight-cluster en een lokale script gestart een training-taak op een Azure Batch AI GPU-cluster. Azure Machine Learning-Workbench van uitvoeringsgeschiedenis functie bijgehouden van de prestaties van meerdere modellen en geholpen bij ons identificeren van de meest nauwkeurige model. De Workbench Jupyter-notebooks functie heeft geholpen visualiseren voorspellingen onze modellen in een interactieve, grafische omgeving.

## <a name="next-steps"></a>Volgende stappen
Meteen dieper in dit voorbeeld:
- In functie van Azure Machine Learning-Workbench van geschiedenis uitvoeren, klikt u op de symbolen tandwielpictogram om te selecteren welke grafieken en metrische gegevens worden weergegeven.
- Bekijk de voorbeeldscripts voor overzichten aanroepen van de `run_logger`. Controleer dat u begrijpt hoe elke metrische gegevens worden geregistreerd.
- Bekijk de voorbeeldscripts voor overzichten aanroepen van de `blob_service`. Controleer of u hoe getraind modellen begrijpt en voorspellingen worden opgeslagen en opgehaald uit de cloud.
- Bekijk de inhoud van de containers in blob storage-account hebt gemaakt. Zorg ervoor dat u welke script begrijpt of opdracht verantwoordelijk is voor het maken van elke groep bestanden.
- Wijzig het script training voor het trainen van een ander MMLSpark modeltype of het model hyperparameters wijzigen. Gebruik de functie uitvoeringsgeschiedenis te bepalen of uw wijzigingen verhoogd of verlaagd nauwkeurigheid van het model.
