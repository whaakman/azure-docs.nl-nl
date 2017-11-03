---
title: Opslag beheren in het Azure onafhankelijk van clouds met Azure PowerShell | Microsoft Docs
description: Opslag in de Cloud in China, Cloud van de overheid en Duitse Cloud met Azure PowerShell beheren
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: robinsh
ms.openlocfilehash: 08e1af929d7ddc30c7dc149f6305ca1ca0bc22ae
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>Opslag in de Azure-onafhankelijke clouds met behulp van PowerShell beheren

Openbare Azure-Cloud de meeste mensen gebruiken voor hun globale Azure-implementatie. Er zijn ook een aantal onafhankelijke implementaties van Microsoft Azure omwille van de onafhankelijkheid enzovoort. Deze onafhankelijke implementaties worden aangeduid als 'environments'. De volgende lijst bevat de onafhankelijke clouds die momenteel beschikbaar is.

* [Azure Government Cloud](https://azure.microsoft.com/features/gov/)
* [Azure China Cloud beheerd door 21Vianet in China](http://www.windowsazure.cn/)
* [Duitse Azure-Cloud](../../germany/germany-welcome.md)

## <a name="using-an-independent-cloud"></a>Met behulp van een onafhankelijke cloud 

Voor het gebruik van Azure Storage in een van de onafhankelijke clouds, verbinding u met die cloud in plaats van openbare Azure. Een van de onafhankelijke clouds in plaats van openbare Azure gebruiken:

* U geeft de *omgeving* op waarmee verbinding wordt gemaakt.
* U vast en gebruikt de beschikbare regio's.
* U het juiste eindpunt achtervoegsel, dat van openbare Azure afwijkt gebruiken.

De voorbeelden moet Azure PowerShell-moduleversie 4.4.0 of hoger. Voer in een PowerShell-venster `Get-Module -ListAvailable AzureRM` de versie te vinden. Als niets wordt weergegeven, of u wilt bijwerken, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Voer de [Get-AzureEnvironment](/powershell/module/azure/Get-AzureRmEnvironment) cmdlet om te controleren van de beschikbare Azure-omgevingen:
   
```powershell
Get-AzureRmEnvironment
```

Aanmelden bij uw account dat toegang heeft tot de cloud die u wilt verbinden en de omgeving. Dit voorbeeld toont hoe u zich aanmeldt bij een account dat de Azure Government Cloud gebruikt.   

```powershell
Login-AzureRmAccount –Environment AzureUSGovernment
```

Voor toegang tot de Cloud China, gebruikt u de omgeving **AzureChinaCloud**. Gebruiken voor toegang tot de Cloud Duits, **AzureGermanCloud**.

Als u de lijst met locaties voor het maken van een opslagaccount of een andere resource nodig hebt, kunt u op dit moment de locaties die beschikbaar zijn voor de geselecteerde cloud met query [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

```powershell
Get-AzureRmLocation | select Location, DisplayName
```

De volgende tabel ziet de locaties voor de Duitse cloud geretourneerd.

|Locatie | Weergavenaam |
|----|----|
| germanycentral | Duitsland - centraal|
| germanynortheast | Duitsland - noordoost | 


## <a name="endpoint-suffix"></a>Achtervoegsel voor eindpunt

Het achtervoegsel eindpunt voor elk van deze omgevingen verschilt van het openbare Azure-eindpunt. Bijvoorbeeld, het achtervoegsel van de blob-eindpunt voor openbare Azure is **blob.core.windows.net**. Voor de overheid van de Cloud, het achtervoegsel voor de blob-eindpunt is **blob.core.usgovcloudapi.net**. 

### <a name="get-endpoint-using-get-azurermenvironment"></a>-Eindpunt met Get-AzureRMEnvironment ophalen 

Ophalen eindpunt achtervoegsel met [Get-AzureRMEnvironment](/powershell/module/azurerm.profile/get-azurermenvironment). Het eindpunt is de *StorageEndpointSuffix* eigenschap van de omgeving. De volgende codefragmenten laten zien hoe u dit doet. Deze opdrachten retourneren iets zoals 'core.cloudapp.net' of 'core.cloudapi.de', enzovoort. Toevoegen aan dit om de storage-service voor toegang tot deze service. "Queue.core.cloudapi.de" wordt bijvoorbeeld toegang tot de queue-service in de Cloud Duits.

Dit codefragment worden alle van de omgeving en het achtervoegsel eindpunt voor elk criterium opgehaald.

```powershell
Get-AzureRmEnvironment | select Name, StorageEndpointSuffix 
```

Deze opdracht retourneert de volgende resultaten.

| Naam| StorageEndpointSuffix|
|----|----|
|AzureChinaCloud | Core.chinacloudapi.CN|
| AzureCloud | Core.Windows.NET |
| AzureGermanCloud | Core.cloudapi.de|
| AzureUSGovernment | Core.usgov.cloudapi.NET |


Aanroepen voor het ophalen van alle eigenschappen voor het opgegeven milieu **Get-AzureRmEnvironment** en geef de cloudnaam van de. Dit codefragment retourneert een lijst met eigenschappen. Zoek naar **StorageEndpointSuffix** in de lijst. Het volgende voorbeeld is voor de Cloud Duits.

```powershell
Get-AzureRmEnvironment -Name AzureGermanCloud 
```

De resultaten zijn vergelijkbaar met het volgende:

|De naam van eigenschap|Waarde|
|----|----|
| Naam | AzureGermanCloud |
| EnableAdfsAuthentication | False |
| ActiveDirectoryServiceEndpointResourceI | http://Management.Core.cloudapi.de/ |
| GalleryURL | https://Gallery.cloudapi.de/ |
| ManagementPortalUrl | https://Portal.microsoftazure.de/ | 
| ServiceManagementUrl | https://Manage.Core.cloudapi.de/ |
| PublishSettingsFileUrl| https://Manage.microsoftazure.de/publishsettings/index |
| ResourceManagerUrl | http://Management.microsoftazure.de/ |
| SqlDatabaseDnsSuffix | . database.cloudapi.de |
| **StorageEndpointSuffix** | Core.cloudapi.de |
| ... | ... | 

Alleen de opslag eindpunt achtervoegsel eigenschap ophalen, de specifieke cloud ophalen en vraag voor slechts één eigenschap.

```powershell
$environment = Get-AzureRmEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix 
```

Hiermee wordt de volgende informatie.

```
Storage Endpoint Suffix = core.cloudapi.de
```

### <a name="get-endpoint-from-a-storage-account"></a>Eindpunt van een opslagaccount ophalen

U kunt ook de eigenschappen van een opslagaccount voor het ophalen van de eindpunten onderzoeken. Dit is handig als u al een opslagaccount in uw PowerShell-script; u kunt alleen het eindpunt dat u moet ophalen. 

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

Voor een opslagaccount in de Cloud Government wordt hiermee het volgende: 

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>Na het instellen van de omgeving

Vanaf hier voortaan, kunt u de dezelfde PowerShell gebruikt voor het beheren van uw storage-accounts en toegang krijgen tot het vlak van gegevens, zoals beschreven in het artikel [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resourcegroep en een opslagaccount voor deze oefening hebt gemaakt, kunt u alle activa door het verwijderen van de resourcegroep verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijdert deze de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

* [Persisting gebruikersaanmeldingen via PowerShell-sessies](/powershell/azure/context-persistence)
* [Azure Government-opslag](../../azure-government/documentation-government-services-storage.md)
* [Ontwikkelaarshandleiding voor Microsoft Azure Government](../../azure-government/documentation-government-developer-guide.md)
* [Opmerkingen voor ontwikkelaars voor Azure China toepassingen](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Azure Duitsland documentatie](../../germany/germany-welcome.md)