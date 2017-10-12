---
title: Aan de slag met Azure Data Lake Analytics met Azure CLI 2.0 | Microsoft Docs
description: 'Ontdek hoe u de Azure-opdrachtregelinterface 2.0 kunt gebruiken voor het maken van een Data Lake Analytics-account, het maken van een Data Lake Analytics-taak met U-SQL en het verzenden van de taak. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: fe2b84aac718ff5eddd4d73b5dc2120362952c1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Aan de slag met Azure Data Lake Analytics met Azure CLI 2.0
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

In deze zelfstudie ontwikkelt u een taak voor het lezen van een TSV-bestand (door tabs gescheiden waarden) en het converteren ervan naar een CSV-bestand (door komma's gescheiden waarden). Als u dezelfde zelfstudie wilt volgen met andere ondersteunde hulpprogramma's, gebruikt u de vervolgkeuzelijst boven aan deze sectie.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Zie [Azure CLI installeren en configureren](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij uw Azure-abonnement:

```
azurecli
az login
```

U wordt gevraagd naar een URL te gaan en een verificatiecode in te voeren.  Volg daarna de instructies om uw referenties in te voeren.

Wanneer u bent aangemeld, worden uw abonnementen weergegeven.

Een specifiek abonnement gebruiken:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken
U hebt een Data Lake Analytics-account nodig voordat u taken kunt uitvoeren. Geef de volgende items op om een Data Lake Analytics-account te maken:

* **Azure-resourcegroep**. Er moet een Data Lake Analytics-account zijn gemaakt binnen een Azure-resourcegroep. Met [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u met de resources in uw toepassing als groep gebruiken. U kunt alle resources voor uw toepassing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking.  

De bestaande resourcegroepen onder uw abonnement weergeven:

```
az group list
```

Een nieuwe resourcegroep maken:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Naam van Data Lake Analytics-account**. Elk Data Lake Analytics-account heeft een naam.
* **Locatie**. Gebruik een van de Azure-datacenters die ondersteuning bieden voor Data Lake Analytics.
* **Data Lake Store-standaardaccount**: elk Data Lake Store Analytics-account heeft een Data Lake Store-standaardaccount.

De bestaande Data Lake Store-accounts weergeven:

```
az dls account list
```

Een nieuw Data Lake Store-account maken:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Gebruik de volgende syntaxis om een Data Lake Analytics-account te maken:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Nadat u een account hebt gemaakt, kunt u de volgende opdrachten gebruiken om de accounts met de accountdetails weer te geven:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Data Lake Store
In deze zelfstudie verwerkt u een aantal zoeklogboeken.  Het zoeklogboek kan worden opgeslagen in de Data Lake Store of Azure Blob-opslag.

Azure Portal biedt een gebruikersinterface waarmee u een aantal voorbeeldbestanden kunt kopiëren naar het Data Lake Store-account, waaronder een zoeklogboekbestand. Zie [Brongegevens voorbereiden](data-lake-analytics-get-started-portal.md) om de gegevens te uploaden naar het Data Lake Store-standaardaccount.

Gebruik de volgende opdrachten om bestanden te uploaden met CLI 2.0:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics heeft ook toegang tot Azure Blob-opslag.  Zie [De Azure CLI gebruiken met Azure Storage](../storage/common/storage-azure-cli.md) voor informatie over het uploaden van gegevens naar Azure Blob-opslag.

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics-taken verzenden
Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) voor meer informatie over U-SQL.

**Een Data Lake Analytics-taakscript maken**

Maak een tekstbestand met het volgende U-SQL-script en bewaar het tekstbestand op uw werkstation:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Dit U-SQL-script leest het brongegevensbestand met **Extractors.Tsv()** en maakt vervolgens een CSV-bestand met **Outputters.Csv()**.

Wijzig de twee paden niet, tenzij u het bronbestand naar een andere locatie kopieert.  Data Lake Analytics maakt de uitvoermap als deze nog niet bestaat.

Het is eenvoudiger om relatieve paden te gebruiken voor bestanden die zijn opgeslagen in Data Lake Store-standaardaccounts. Maar u kunt ook absolute paden gebruiken.  Bijvoorbeeld:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

U moet absolute paden gebruiken om toegang te krijgen tot bestanden in gekoppelde Storage-accounts.  De syntaxis voor bestanden die zijn opgeslagen in het gekoppelde Azure Storage-account is:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Azure Blob-containers met openbare blobs worden niet ondersteund.      
> Azure Blob-containers met openbare containers worden niet ondersteund.      
>

**Taken verzenden**

Gebruik de volgende syntaxis om een taak te verzenden.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Bijvoorbeeld:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**Taken en taakdetails weergeven**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**Taken annuleren**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Taakresultaten ophalen

Wanneer een taak is voltooid, kunt u de volgende opdrachten gebruiken om de uitvoerbestanden weer te geven en te downloaden:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Bijvoorbeeld:

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a>Pijplijnen en herhalingen

**Meer informatie over pijplijnen en herhalingen**

Gebruik de `az dla job pipeline`-opdrachten om de pijplijngegevens te zien voor eerder verzonden taken.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Gebruik de `az dla job recurrence`-opdrachten om de herhalingsgegevens te zien voor eerder verzonden taken.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla) voor het naslagdocument van Data Lake Analytics CLI 2.0.
* Zie [Data Lake Store](https://docs.microsoft.com/cli/azure/dls) voor het naslagdocument van Data Lake Store CLI 2.0.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
