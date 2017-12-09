---
title: Azure Data Lake Analytics beheren met Azure-opdrachtregelinterface | Microsoft Docs
description: Informatie over het beheren van Data Lake Analytics-accounts, gegevensbronnen, taken en gebruikers met Azure CLI
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 142a5baf0ffb0425856520fadccee27f3fc0517f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Azure Data Lake Analytics beheren met Azure-opdrachtregelinterface (CLI)
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informatie over het beheren van Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken met de Azure CLI. Als u wilt zien management onderwerpen met een ander hulpprogramma, klikt u op het tabblad select bovenaan.


**Vereisten**

Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI**. Zie [Azure CLI installeren en configureren](../cli-install-nodejs.md).
  * Download en installeer de **pre-release** [Azure CLI-hulpprogramma’s](https://github.com/MicrosoftBigData/AzureDataLake/releases) om deze demo te voltooien.
* **Verificatie**, met de volgende opdracht:
  
        azure login
    Zie [Verbinding maken met een Azure-abonnement met Azure CLI](/cli/azure/authenticate-azure-cli) voor meer informatie over verificatie met een werk- of schoolaccount.
* **Overschakelen naar de modus Azure Resource Manager** met de volgende opdracht:
  
        azure config mode arm

**Overzicht van de Data Lake Store en Data Lake Analytics-opdrachten:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Accounts beheren
Voordat u een Data Lake Analytics-taken uitvoert, moet u een Data Lake Analytics-account hebben. In tegenstelling tot Azure HDInsight betaalt niet u voor een Analytics-account wanneer deze een taak niet wordt uitgevoerd.  U betaalt alleen voor de tijd wanneer deze een taak wordt uitgevoerd.  Zie voor meer informatie [overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Accounts maken
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Bijwerken van accounts
De volgende opdracht werkt u de eigenschappen van een bestaande Data Lake Analytics-Account

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Lijst van accounts
Lijst met Data Lake Analytics-accounts 

    azure datalake analytics account list

Lijst met Data Lake Analytics-accounts binnen een specifieke resourcegroep

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Details van een specifieke Data Lake Analytics-account ophalen

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Data Lake Analytics-accounts te verwijderen
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Account gegevensbronnen beheren
Data Lake Analytics ondersteunt momenteel de volgende gegevensbronnen:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Wanneer u een Analytics-account maakt, moet u een Azure Data Lake Storage-account moet het standaardopslagaccount toewijzen. Het standaardopslagaccount voor ADL wordt gebruikt voor het opslaan van taak metagegevens en taak controlelogboeken. Nadat u een Analytics-account hebt gemaakt, kunt u extra Data Lake Storage-accounts en/of Azure Storage-account toevoegen. 

### <a name="find-the-default-adl-storage-account"></a>Het standaardopslagaccount voor ADL vinden
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

De waarde wordt vermeld onder eigenschappen: datalakeStoreAccount:name.

### <a name="add-additional-azure-blob-storage-accounts"></a>Extra Azure Blob storage-accounts toevoegen
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Alleen Blob storage korte namen worden ondersteund.  Gebruik geen FQDN-naam, bijvoorbeeld 'myblob.blob.core.windows.net'.
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>Extra Data Lake Store-accounts toevoegen
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] is een optionele schakeloptie om aan te geven of de Data Lake wordt toegevoegd het Data Lake-standaardaccount. 

### <a name="update-existing-data-source"></a>Bestaande gegevensbron bijwerken
Een bestaande Data Lake Store-account de standaardprovider instellen:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

Een bestaande sleutel voor Blob storage-account bijwerken:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Lijst van gegevensbronnen:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Gegevensbron voor Data Lake Analytics-lijst](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Gegevensbronnen verwijderen:
Een Data Lake Store-account verwijderen:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

Een Blob storage-account verwijderen:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Taken beheren
U moet een Data Lake Analytics-account hebben voordat u een taak kunt maken.  Zie voor meer informatie [beheren Data Lake Analytics-accounts](#manage-accounts).

### <a name="list-jobs"></a>Taken weergeven
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Gegevensbron voor Data Lake Analytics-lijst](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Taakdetails van de ophalen
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>Verzenden van taken
> [!NOTE]
> De laagste prioriteit van een taak is 1000 en de standaard graad van parallelle uitvoering voor een taak is 1.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Taken annuleren
Gebruik de opdracht lijst zoeken naar de taak-id en gebruik vervolgens annuleren om te annuleren de taak.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Catalogus beheren
De U-SQL-catalogus wordt gebruikt om gegevens en code structuur, zodat ze kunnen worden gedeeld door de U-SQL-scripts. De catalogus kunt de beste prestaties mogelijk met gegevens in Azure Data Lake. Zie [U-SQL-catalogus gebruiken](data-lake-analytics-use-u-sql-catalog.md) voor meer informatie.

### <a name="list-catalog-items"></a>Lijst met catalogusitems
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

De typen bestaan database, schema, assembly, externe gegevensbron, tabel, functie met tabelwaarden of tabelstatistieken.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>ARM-groepen gebruiken
Toepassingen bestaan in het algemeen uit meerdere onderdelen, bijvoorbeeld een web-app, database, databaseserver, opslag en services van derden. Met Azure Resource Manager (ARM) kunt u de resources in uw toepassing gebruiken als groep, die we een Azure-resourcegroep noemen. U kunt alle resources voor uw toepassing implementeren, bijwerken, bewaken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. U kunt facturering voor uw organisatie verduidelijken door de samengevoegde kosten voor de hele groep weer te geven. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 

Een Data Lake Analytics-service kan de volgende onderdelen bevatten:

* Azure Data Lake Analytics-account
* Vereiste standaard Azure Data Lake Storage-account
* Aanvullende Azure Data Lake Storage-accounts
* Extra Azure Storage-accounts

U kunt al deze onderdelen onder een ARM-groep zodat ze gemakkelijker te beheren.

![Azure Data Lake Analytics-account en opslag](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Een Data Lake Analytics-account en de afhankelijke storage-accounts moeten in dezelfde Azure-datacenter worden geplaatst.
De ARM-groep kan echter in een ander datacenter worden geplaatst.  

## <a name="see-also"></a>Zie ook
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

