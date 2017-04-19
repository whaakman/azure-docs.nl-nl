---
title: Aan de slag met Azure Data Lake Analytics met Azure CLI 2.0 | Microsoft Docs
description: 'Ontdek hoe u de Azure-opdrachtregelinterface 2.0 kunt gebruiken voor het maken van een Data Lake Analytics-account, het maken van een Data Lake Analytics-taak met U-SQL en het verzenden van de taak. '
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 109460cecc4e11c729203af97c9bf1c22b90e61a
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-cli-20-preview"></a>Zelfstudie: Aan de slag met Azure Data Lake Analytics met Azure CLI 2.0 (preview)
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Ontdek hoe u de Azure CLI 2.0 kunt gebruiken voor het maken van Azure Data Lake Analytics-accounts, het definiëren van Data Lake Analytics-taken in [U-SQL](data-lake-analytics-u-sql-get-started.md) en het verzenden van taken naar Data Lake Analytics-accounts. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

In deze zelfstudie gaat u een taak ontwikkelen die een bestand met door tabs gescheiden waarden (TSV) leest en converteert naar een bestand met door komma's gescheiden waarden (CSV). Als u dezelfde zelfstudie wilt volgen met andere ondersteunde hulpprogramma's, gebruikt u de vervolgkeuzelijst boven aan deze sectie.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Zie [Azure CLI installeren en configureren](https://docs.microsoft.com/cli/azure/install-azure-cli).
* **Data Lake Store/Analytics CLI 2.0 preview inschakelen**. Data Lake Store en Data Lake Analytics CLI 2.0 zijn momenteel alleen als preview beschikbaar. Voer de volgende opdrachten uit om beide in te schakelen:

    ```azurecli
    az component update --add dls
    az component update --add dla 
    ```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij uw Azure-abonnement:

```azurecli
az login
```

U wordt gevraagd naar een URL te gaan en een verificatiecode in te voeren.  Volg daarna de instructies om uw referenties in te voeren.

Wanneer u bent aangemeld, worden uw abonnementen weergegeven.

Een specifiek abonnement gebruiken:

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken
U hebt een Data Lake Analytics-account nodig voordat u taken kunt uitvoeren. Geef de volgende items op om een Data Lake Analytics-account te maken:

* **Azure-resourcegroep**. Er moet een Data Lake Analytics-account zijn gemaakt binnen een Azure-resourcegroep. Met [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u met de resources in uw toepassing als groep gebruiken. U kunt alle resources voor uw toepassing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking.  
  
    De bestaande resourcegroepen onder uw abonnement weergeven:
  
    ```azurecli
    az group list 
    ```

    Een nieuwe resourcegroep maken:
  
    ```azurecli
    az group create --name "<Resource Group Name>" --location "<Azure Location>"
    ```

* **Naam van Data Lake Analytics-account**. Elk Data Lake Analytics-account heeft een naam.
* **Locatie**. Gebruik een van de Azure-datacenters die ondersteuning bieden voor Data Lake Analytics.
* **Data Lake Store-standaardaccount**: elk Data Lake Store Analytics-account heeft een Data Lake Store-standaardaccount.
  
    De bestaande Data Lake Store-accounts weergeven:

    ```azurecli
    az dls account list
    ```
  
    Een nieuw Data Lake Store-account maken:
  
    ```azurecli
    az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
    ```

Gebruik de volgende syntaxis om een Data Lake Analytics-account te maken:

```azurecli
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Nadat u een account hebt gemaakt, kunt u de volgende opdrachten gebruiken om de accounts met de accountdetails weer te geven:

```azurecli
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Data Lake Store
In deze zelfstudie verwerkt u een aantal zoeklogboeken.  Het zoeklogboek kan worden opgeslagen in de Data Lake Store of Azure Blob-opslag. 

Azure Portal biedt een gebruikersinterface waarmee u een aantal voorbeeldbestanden kunt kopiëren naar het Data Lake Store-account, waaronder een zoeklogboekbestand. Zie [Brongegevens voorbereiden](data-lake-analytics-get-started-portal.md#prepare-source-data) om de gegevens te uploaden naar het Data Lake Store-standaardaccount.

Gebruik de volgende opdrachten om bestanden te uploaden met de CLI 2.0:

```azurecli
az dls file upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls file list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics heeft ook toegang tot Azure Blob-opslag.  Zie [De Azure CLI gebruiken met Azure Storage](../storage/storage-azure-cli.md) voor informatie over het uploaden van gegevens naar Azure Blob-opslag.

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics-taken verzenden
Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) voor meer informatie over U-SQL.

**Een Data Lake Analytics-taakscript maken**

Maak een tekstbestand met het volgende U-SQL-script en bewaar het tekstbestand op uw werkstation:
  
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();
  
Dit U-SQL-script leest het brongegevensbestand met **Extractors.Tsv()** en maakt vervolgens een CSV-bestand met **Outputters.Csv()**. 

Wijzig de twee paden niet, tenzij u het bronbestand naar een andere locatie kopieert.  Data Lake Analytics maakt de uitvoermap als deze nog niet bestaat.

Het is eenvoudiger om relatieve paden te gebruiken voor bestanden die zijn opgeslagen in Data Lake Store-standaardaccounts. Maar u kunt ook absolute paden gebruiken.  Bijvoorbeeld:

    adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

U moet absolute paden gebruiken om toegang te krijgen tot bestanden in gekoppelde Storage-accounts.  De syntaxis voor bestanden die zijn opgeslagen in het gekoppelde Azure Storage-account is:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
  
  > [!NOTE]
  > Azure Blob-containers met openbare blobs of machtigingen voor openbare containers worden momenteel niet ondersteund.      
  > 
  > 

**Taken verzenden**

Gebruik de volgende syntaxis om een taak te verzenden.

```azurecli
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Bijvoorbeeld:

```azurecli
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**Taken en taakdetails weergeven**

```azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**Taken annuleren**

```azurecli
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

##<a name="retrieve-job-results"></a>Taakresultaten ophalen

Wanneer een taak is voltooid, kunt u de volgende opdrachten gebruiken om de uitvoerbestanden weer te geven en te downloaden:

```azurecli
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" 
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Bijvoorbeeld:

```azurecli
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Volgende stappen

* Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
* Zie [Data Lake Analytics - az dla](https://docs.microsoft.com/cli/azure/dla) voor het naslagdocument van Data Lake Analytics CLI 2.0.
* Zie [Data Lake Store - az dla](https://docs.microsoft.com/cli/azure/dls) voor het naslagdocument van Data Lake Store CLI 2.0.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Zie [U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) om aan de slag te gaan met het ontwikkelen van U-SQL-toepassingen.
* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.
* Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor een overzicht van Data Lake Analytics.


