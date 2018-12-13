---
title: Veilige overdracht in Azure Storage vereisen | Microsoft Docs
description: Meer informatie over de functie 'Veilige overdracht vereist' voor Azure Storage en het inschakelen ervan.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 06/20/2017
ms.author: fryu
ms.component: common
ms.openlocfilehash: 72e0937e91a7287d240bbdb25996865f934d432d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876499"
---
# <a name="require-secure-transfer-in-azure-storage"></a>Veilige overdracht in Azure Storage vereisen

De optie 'Veilige overdracht vereist' verhoogt de beveiliging van uw storage-account doordat u aanvragen alleen aan het account van veilige verbindingen. Bijvoorbeeld, wanneer u gebeld REST-API's voor toegang tot uw opslagaccount, u moet verbinding maken met behulp van HTTPS. Aanvragen die HTTP gebruiken 'Veilige overdracht vereist' worden geweigerd.

Wanneer u de Azure Files-service gebruikt, wordt elke verbinding zonder versleuteling mislukt wanneer 'Veilige overdracht vereist' is ingeschakeld. Dit geldt ook voor scenario's die gebruikmaken van SMB 2.1, SMB 3.0 zonder versleuteling en bepaalde versies van de Linux SMB-client. 

De optie 'Veilige overdracht vereist' is standaard uitgeschakeld wanneer u een opslagaccount met de SDK maakt. En dit standaard ingeschakeld wanneer u een storage-account in Azure Portal maken.

> [!NOTE]
> Omdat Azure Storage biedt geen ondersteuning voor HTTPS voor aangepaste domeinnamen bieden, wordt deze optie wordt niet toegepast wanneer u een aangepaste domeinnaam. En klassieke opslagaccounts worden niet ondersteund.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>'Veilige overdracht vereist' inschakelen in Azure portal

U kunt inschakelen op de 'veilige overdracht vereist' instellen bij het maken van een opslagaccount in de [Azure-portal](https://portal.azure.com). U kunt deze ook inschakelen voor bestaande storage-accounts.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Veilige overdracht vereisen voor een nieuw opslagaccount

1. Open de **storage-account maken** deelvenster in de Azure-portal.
1. Onder **veilige overdracht vereist**, selecteer **ingeschakeld**.

  ![Blade opslagaccount maken](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Veilige overdracht vereisen voor een bestaand opslagaccount

1. Selecteer een bestaand opslagaccount in Azure portal.
1. In de opslag account menu in het deelvenster onder **instellingen**, selecteer **configuratie**.
1. Onder **veilige overdracht vereist**, selecteer **ingeschakeld**.

  ![Storage-account menu deelvenster](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>"Veilige overdracht vereist" inschakelen via een programma

Om te vereisen dat veilige overdracht via een programma, gebruikt u de instelling _supportsHttpsTrafficOnly_ in eigenschappen van het opslagaccount met de REST-API, hulpprogramma's of bibliotheken:

* [REST-API](https://docs.microsoft.com/rest/api/storagerp/storageaccounts) (versie: 12-01-2016)
* [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0) (versie: 4.1.0)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (versie: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (versie: 1.1.0)
* [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (versie: 6.3.0)
* [Python SDK](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (versie: 1.1.0)
* [Ruby-SDK](https://rubygems.org/gems/azure_mgmt_storage) (versie: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-powershell"></a>Inschakelen van 'Veilige overdracht vereist' instellen met PowerShell

In dit voorbeeld is de Azure PowerShell-module, versie 4.1 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Voer `Connect-AzureRmAccount` uit om een verbinding op te zetten met Azure.

 De volgende opdrachtregel gebruiken om te controleren of de instelling:

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

### <a name="enable-secure-transfer-required-setting-with-cli"></a>Inschakelen van 'Veilige overdracht vereist' instellen met CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 De volgende opdrachtregel gebruiken om te controleren of de instelling:

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
Azure Storage biedt een uitgebreide set met mogelijkheden voor beveiliging, die samen kunnen ontwikkelaars veilige toepassingen te bouwen. Voor meer informatie gaat u naar de [Storage-beveiligingshandleiding](storage-security-guide.md).
