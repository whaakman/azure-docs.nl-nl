---
title: Veilige overdracht in Azure Storage vereisen | Microsoft Docs
description: Meer informatie over de functie 'Beveiligde overdracht vereist' voor Azure Storage en hoe deze in te schakelen.
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: 797ac45a41cdf655e7465a01875a0394081c08a7
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="require-secure-transfer-in-azure-storage"></a>Veilige overdracht in Azure Storage vereisen

De optie "Veilig vereiste gegevensoverdracht" verhoogt de beveiliging van uw storage-account door alleen aanvragen toe aan het account van beveiligde verbindingen te laten. Bijvoorbeeld, wanneer u REST-API's belt voor toegang tot uw storage-account, moet u verbinden met behulp van HTTPS. "Veilig vereiste gegevensoverdracht" weigert aanvragen die gebruikmaken van HTTP.

Wanneer u de service Azure-bestanden gebruikt, wordt er een verbinding zonder versleuteling mislukt wanneer "Veilig vereiste gegevensoverdracht" is ingeschakeld. Dit geldt ook voor scenario's die gebruikmaken van SMB 2.1, SMB 3.0 zonder versleuteling en bepaalde versies van de Linux SMB-client. 

De optie "Veilig vereiste gegevensoverdracht" is standaard uitgeschakeld.

> [!NOTE]
> Omdat Azure Storage biedt geen ondersteuning voor HTTPS voor aangepaste domeinnamen, is deze optie niet toegepast wanneer u een aangepaste domeinnaam. En klassieke opslagaccounts worden niet ondersteund.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>"Veilig vereiste gegevensoverdracht" inschakelen in de Azure portal

U kunt inschakelen met de 'beveiligde overdracht vereist' instellen bij het maken van een opslagaccount in de [Azure-portal](https://portal.azure.com). U kunt deze ook inschakelen voor bestaande opslagaccounts.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Veilige overdracht voor een nieuw opslagaccount vereisen

1. Open de **storage-account maken** deelvenster in de Azure-portal.
1. Onder **vereiste gegevensoverdracht Secure**, selecteer **ingeschakeld**.

  ![Blade opslagaccount maken](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Veilige overdracht voor een bestaand opslagaccount vereisen

1. Selecteer een bestaand opslagaccount in de Azure-portal.
1. In de opslag account menu deelvenster onder **instellingen**, selecteer **configuratie**.
1. Onder **vereiste gegevensoverdracht Secure**, selecteer **ingeschakeld**.

  ![Storage-account menu deelvenster](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>"Veilig vereiste gegevensoverdracht" inschakelen via een programma

De instelling gebruiken om te vereisen veilige overdracht via een programma, _supportsHttpsTrafficOnly_ in eigenschappen van het opslagaccount met de REST-API, hulpprogramma's of bibliotheken:

* [REST-API](https://docs.microsoft.com/rest/api/storagerp/storageaccounts) (versie: 2016-12-01)
* [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0) (versie: 4.1.0)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (versie: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (versie: 1.1.0)
* [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (versie: 6.3.0)
* [Python SDK](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (versie: 1.1.0)
* [Ruby SDK](https://rubygems.org/gems/azure_mgmt_storage) (versie: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-powershell"></a>'Veilige overdracht vereist' instellen met PowerShell inschakelen

Dit voorbeeld is de Azure PowerShell-moduleversie 4.1 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Voer `Login-AzureRmAccount` geen verbinding maken met Azure.

 De volgende opdrachtregel gebruiken om te controleren van de instelling:

```powershell
> Get-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

De volgende opdrachtregel gebruiken om de instelling:

```powershell
> Set-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

### <a name="enable-secure-transfer-required-setting-with-cli"></a>'Veilige overdracht vereist' instellen met CLI inschakelen

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 De volgende opdrachtregel gebruiken om te controleren van de instelling:

```azurecli-interactive
> az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

De volgende opdrachtregel gebruiken om de instelling:

```azurecli-interactive
> az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only true
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": true,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Voor meer informatie gaat u naar de [opslag beveiligingshandleiding](storage-security-guide.md).
