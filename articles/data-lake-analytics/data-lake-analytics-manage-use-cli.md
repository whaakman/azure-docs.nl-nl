---
title: Azure Data Lake Analytics met behulp van Azure-opdrachtregelinterface beheren
description: In dit artikel wordt beschreven hoe u de Azure CLI gebruiken om Data Lake Analytics-taken, gegevensbronnen en gebruikers te beheren.
services: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: e265a46533264bbb1d437edbfe1bbfb3306614ad
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044820"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-command-line-interface-cli"></a>Azure Data Lake Analytics beheren met de Azure-opdrachtregelinterface (CLI)

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informatie over het beheren van Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken met de Azure CLI. Voor onderwerpen over met andere hulpprogramma's, klikt u op het tabblad select bovenaan.


**Vereisten**

Voordat u deze zelfstudie begint, moet u de volgende bronnen:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* Azure CLI. Zie [Azure CLI installeren en configureren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

   * Download en installeer de **pre-release** [Azure CLI-hulpprogramma’s](https://github.com/MicrosoftBigData/AzureDataLake/releases) om deze demo te voltooien.

* Verifiëren met behulp van de `az login` opdracht en selecteer het abonnement dat u wilt gebruiken. Zie [Verbinding maken met een Azure-abonnement met Azure CLI](/cli/azure/authenticate-azure-cli) voor meer informatie over verificatie met een werk- of schoolaccount.

   ```azurecli
   az login
   az account set --subscription <subscription id>
   ```

   U kunt nu toegang tot de Data Lake Analytics en Data Lake Store-opdrachten. Voer de volgende opdracht om een lijst van de Data Lake Store en Data Lake Analytics-opdrachten:

   ```azurecli
   az dls -h
   az dla -h
   ```

## <a name="manage-accounts"></a>Accounts beheren

Voordat u een Data Lake Analytics-taken uitvoert, moet u een Data Lake Analytics-account hebben. In tegenstelling tot Azure HDInsight betaalt u niet voor een Analytics-account wanneer deze een taak niet wordt uitgevoerd. U betaalt alleen voor de tijd wanneer deze een taak wordt uitgevoerd.  Zie voor meer informatie, [overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Accounts maken

Voer de volgende opdracht om een Data Lake-account te maken 

   ```azurecli
   az dla account create --account "<Data Lake Analytics account name>" --location "<Location Name>" --resource-group "<Resource Group Name>" --default-data-lake-store "<Data Lake Store account name>"
   ```

### <a name="update-accounts"></a>Update-accounts

De volgende opdracht werkt de eigenschappen van een bestaand Data Lake Analytics-Account

   ```azurecli
   az dla account update --account "<Data Lake Analytics Account Name>" --firewall-state "Enabled" --query-store-retention 7
   ```

### <a name="list-accounts"></a>Lijst van accounts

Lijst met Data Lake Analytics-accounts binnen een specifieke resourcegroep

   ```azurecli
   az dla account list "<Resource group name>"
   ```

## <a name="get-details-of-an-account"></a>Details van een account ophalen

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

### <a name="delete-an-account"></a>Een account verwijderen

   ```azurecli
   az dla account delete --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

## <a name="manage-data-sources"></a>Gegevensbronnen beheren

Data Lake Analytics ondersteunt momenteel de volgende twee gegevensbronnen:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Wanneer u een Analytics-account maakt, moet u een Azure Data Lake Storage-account om te worden van het standaardopslagaccount opgeven. De standaard Data Lake storage-account wordt gebruikt voor het opslaan van taak metagegevens en taak auditlogboeken. Nadat u een Analytics-account hebt gemaakt, kunt u extra Data Lake Storage-accounts en/of Azure Storage-account toevoegen. 

### <a name="find-the-default-data-lake-store-account"></a>De standaard Data Lake Store-account zoeken

Vindt u de standaard Data Lake Store-account wordt gebruikt door het uitvoeren van de `az dla account show` opdracht. Standaardnaam van het account wordt vermeld onder de eigenschap defaultDataLakeStoreAccount.

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>"
   ```

### <a name="add-additional-blob-storage-accounts"></a>Aanvullende Blob storage-accounts toevoegen

   ```azurecli
   az dla account blob-storage add --access-key "<Azure Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Storage account name>"
   ```

> [!NOTE]
> Alleen Blob-opslag korte namen worden ondersteund. Gebruik geen FQDN-naam, bijvoorbeeld 'myblob.blob.core.windows.net'.
> 

### <a name="add-additional-data-lake-store-accounts"></a>Extra Data Lake Store-accounts toevoegen

De volgende opdracht werkt de opgegeven Data Lake Analytics-account met een extra Data Lake Store-account:

   ```azurecli
   az dla account data-lake-store add --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Data Lake Store account name>"
   ```

### <a name="update-existing-data-source"></a>Bestaande gegevensbron bijwerken

Een bestaande Blob storage-accountsleutel bijwerken:

   ```azurecli
   az dla account blob-storage update --access-key "<New Blob Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

### <a name="list-data-sources"></a>Lijst met gegevensbronnen:

Overzicht van de Data Lake Store-accounts:

   ```azurecli
   az dla account data-lake-store list --account "<Data Lake Analytics account name>"
   ```

Overzicht van de Blob storage-account:

   ```azurecli
   az dla account blob-storage list --account "<Data Lake Analytics account name>"
   ```

![Gegevensbron voor Data Lake Analytics-lijst](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Gegevensbronnen verwijderen:
Een Data Lake Store-account verwijderen:

   ```azurecli
   az dla account data-lake-store delete --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Azure Data Lake Store account name>"
   ```

Een Blob storage-account verwijderen:

   ```azurecli
   az dla account blob-storage delete --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

## <a name="manage-jobs"></a>Taken beheren
Voordat u een taak kunt maken, moet u een Data Lake Analytics-account hebben.  Zie voor meer informatie, [beheren van Data Lake Analytics-accounts](#manage-accounts).

### <a name="list-jobs"></a>Lijst met taken

   ```azurecli
   az dla job list --account "<Data Lake Analytics account name>"
   ```

   ![Gegevensbron voor Data Lake Analytics-lijst](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Informatie over

   ```azurecli
   az dla job show --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

### <a name="submit-jobs"></a>Verzenden van taken

> [!NOTE]
> De prioriteit is standaard van een taak is 1000 en de standaard-graad van parallelle uitvoering voor een taak is 1.
> 
   ```azurecli
   az dla job submit --account "<Data Lake Analytics account name>" --job-name "<Name of your job>" --script "<Script to submit>"
   ```

### <a name="cancel-jobs"></a>Taken annuleren
Gebruik de opdracht lijst zoeken naar de taak-id en gebruik vervolgens annuleren om te annuleren de taak.

   ```azurecli
   az dla job cancel --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
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

## <a name="see-also"></a>Zie ook
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

