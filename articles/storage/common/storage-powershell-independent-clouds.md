---
title: Azure PowerShell gebruiken voor het beheren van opslag in de Azure onafhankelijke clouds | Microsoft Docs
description: Opslag in de China-Cloud, de Government-Cloud en de Duitse Cloud met behulp van Azure PowerShell beheren
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 75a3dcb5aeb3e30da570eb57d0d1495710624e54
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060578"
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>Beheren van opslag in de Azure-onafhankelijke clouds met behulp van PowerShell

De meeste mensen gebruiken openbare Azure-Cloud voor de globale Azure-implementatie. Er zijn ook enkele onafhankelijke implementaties van Microsoft Azure omwille van de onafhankelijkheid enzovoort. Onafhankelijke implementaties worden aangeduid als 'omgeving'. De volgende lijst bevat de onafhankelijke clouds die momenteel beschikbaar is.

* [Azure Government-Cloud](https://azure.microsoft.com/features/gov/)
* [Azure China-Cloud uitgevoerd door 21Vianet in China](http://www.windowsazure.cn/)
* [Azure-Duitse Cloud](../../germany/germany-welcome.md)

## <a name="using-an-independent-cloud"></a>Met behulp van een onafhankelijke cloud 

Voor het gebruik van Azure Storage in een van de onafhankelijke clouds, moet u die cloud in plaats van openbare Azure-verbinden. Gebruik een van de onafhankelijke clouds in plaats van openbare Azure:

* U geeft de *omgeving* waarmee u verbinding wilt.
* U bepalen en de beschikbare regio's.
* U gebruik het juiste eindpunt-achtervoegsel, die van openbare Azure afwijkt.

De voorbeelden moet Azure PowerShell-moduleversie 4.4.0 of hoger. Voer in een PowerShell-venster `Get-Module -ListAvailable AzureRM` de versie te vinden. Als niets wordt weergegeven, of u wilt upgraden, Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Voer de [Get-AzureRmEnvironment](/powershell/module/servicemanagement/azurerm.profile/get-azurermenvironment) cmdlet om te controleren van de beschikbare Azure-omgevingen:
   
```powershell
Get-AzureRmEnvironment
```

Aanmelden bij uw account dat toegang heeft tot de cloud die u wilt verbinden en de omgeving. Dit voorbeeld laat zien hoe u zich aanmelden bij een account dat gebruikmaakt van de Azure Government-Cloud.   

```powershell
Connect-AzureRmAccount –Environment AzureUSGovernment
```

Voor toegang tot de China-Cloud, gebruikt u de omgeving **AzureChinaCloud**. Gebruiken voor toegang tot de Duitse Cloud, **AzureGermanCloud**.

Als u de lijst met locaties te maken van een storage-account of een andere resource nodig hebt, kunt u op dit moment de locaties die beschikbaar zijn voor het gebruik van de geselecteerde cloud query [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

```powershell
Get-AzureRmLocation | select Location, DisplayName
```

De volgende tabel ziet u de locaties die worden geretourneerd voor de Duitse cloud.

|Locatie | Weergavenaam |
|----|----|
| germanycentral | Duitsland - centraal|
| germanynortheast | Duitsland - noordoost | 


## <a name="endpoint-suffix"></a>Achtervoegsel van eindpunt

Het achtervoegsel van het eindpunt voor elk van deze omgevingen wijkt af van het openbare Azure-eindpunt. Bijvoorbeeld, het achtervoegsel van het blob-eindpunt voor openbare Azure-is **blob.core.windows.net**. Voor de Government-Cloud, het achtervoegsel van het blob-eindpunt is **blob.core.usgovcloudapi.net**. 

### <a name="get-endpoint-using-get-azurermenvironment"></a>Met behulp van Get-AzureRMEnvironment-eindpunt ophalen 

Ophalen van het eindpunt achtervoegsel via [Get-AzureRMEnvironment](/powershell/module/azurerm.profile/get-azurermenvironment). Het eindpunt is de *StorageEndpointSuffix* eigenschap van de omgeving. De volgende codefragmenten laten zien hoe u dit doet. Deze opdrachten retourneren iets, zoals 'core.cloudapp.net' of 'core.cloudapi.de', enzovoort. Voeg dit toe aan de storage-service voor toegang tot deze service. "Queue.core.cloudapi.de" wordt bijvoorbeeld toegang tot de queue-service in de Duitse Cloud.

Dit codefragment worden alle van de omgevingen en het achtervoegsel van het eindpunt voor elk criterium opgehaald.

```powershell
Get-AzureRmEnvironment | select Name, StorageEndpointSuffix 
```

Met deze opdracht worden de volgende resultaten geretourneerd.

| Naam| StorageEndpointSuffix|
|----|----|
| AzureChinaCloud | core.chinacloudapi.cn|
| AzureCloud | core.windows.net |
| AzureGermanCloud | core.cloudapi.de|
| AzureUSGovernment | Core.usgovcloudapi.NET |

Als u wilt alle van de eigenschappen voor de opgegeven omgeving ophalen, aanroepen **Get-AzureRmEnvironment** en geef de cloudnaam. Dit codefragment retourneert een lijst met eigenschappen. Zoek naar **StorageEndpointSuffix** in de lijst. Het volgende voorbeeld is voor de Duitse Cloud.

```powershell
Get-AzureRmEnvironment -Name AzureGermanCloud 
```

De resultaten zijn vergelijkbaar met het volgende:

|De naam van eigenschap|Waarde|
|----|----|
| Naam | AzureGermanCloud |
| EnableAdfsAuthentication | False |
| ActiveDirectoryServiceEndpointResourceI | http://management.core.cloudapi.de/ |
| GalleryURL | https://gallery.cloudapi.de/ |
| ManagementPortalUrl | https://portal.microsoftazure.de/ | 
| ServiceManagementUrl | https://manage.core.cloudapi.de/ |
| PublishSettingsFileUrl| https://manage.microsoftazure.de/publishsettings/index |
| ResourceManagerUrl | http://management.microsoftazure.de/ |
| SqlDatabaseDnsSuffix | .database.cloudapi.de |
| **StorageEndpointSuffix** | core.cloudapi.de |
| ... | ... | 

Alleen de opslag-achtervoegsel eindpunteigenschap ophalen, de specifieke cloud ophalen en vragen om alleen die een eigenschap.

```powershell
$environment = Get-AzureRmEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix 
```

Dit retourneert de volgende informatie.

```
Storage Endpoint Suffix = core.cloudapi.de
```

### <a name="get-endpoint-from-a-storage-account"></a>Eindpunt van een opslagaccount ophalen

U kunt ook de eigenschappen van een storage-account om op te halen van de eindpunten. Dit is handig als u al een storage-account in uw PowerShell-script; u kunt alleen het eindpunt dat u moet ophalen. 

```powershell
# Get a reference to the storage account.
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"
$storageAccount = Get-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
  # Output the endpoints.
Write-Host "blob endpoint = " $storageAccount.PrimaryEndPoints.Blob 
Write-Host "file endpoint = " $storageAccount.PrimaryEndPoints.File
Write-Host "queue endpoint = " $storageAccount.PrimaryEndPoints.Queue
Write-Host "table endpoint = " $storageAccount.PrimaryEndPoints.Table
```

Voor een opslagaccount in de Government-Cloud retourneert deze het volgende: 

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>Na het instellen van de omgeving

Vanaf hier voortaan, kunt u de dezelfde PowerShell gebruikt voor het beheren van uw opslagaccounts en toegang tot het gegevensvlak zoals beschreven in het artikel [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resourcegroep en een opslagaccount voor deze oefening hebt gemaakt, kunt u alle activa die door het verwijderen van de resourcegroep verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijderd het de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

* [Gebruikersaanmeldingen behouden tussen PowerShell-sessies](/powershell/azure/context-persistence)
* [Azure Government-opslag](../../azure-government/documentation-government-services-storage.md)
* [Ontwikkelaarshandleiding voor Microsoft Azure Government](../../azure-government/documentation-government-developer-guide.md)
* [Opmerkingen voor ontwikkelaars voor Azure China toepassingen](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Documentatie voor Azure Duitsland](../../germany/germany-welcome.md)
