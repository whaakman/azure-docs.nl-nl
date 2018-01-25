---
title: Azure PowerShell gebruiken met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure PowerShell-cmdlets voor Azure Storage.
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: f94febfa1610795cd46b4315bbbbe56aa2bca861
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="using-azure-powershell-with-azure-storage"></a>Azure PowerShell gebruiken met Azure Storage

Azure PowerShell wordt gebruikt voor het maken en beheren van Azure-resources van de PowerShell-opdrachtregel of in scripts. Voor Azure Storage, worden deze cmdlets onderverdeeld in twee categorieën--het vlak van het besturingselement en het vlak van gegevens. Het besturingselement vlak-cmdlets worden gebruikt voor het beheren van het opslagaccount--voor het storage-accounts maken, eigenschappen instellen, storage-accounts te verwijderen, de sneltoetsen draaien, enzovoort. De gegevens vlak-cmdlets worden gebruikt voor het beheren van de opgeslagen gegevens *in* het storage-account. Bijvoorbeeld BLOB's uploaden, maken van bestandsshares en berichten toe te voegen aan een wachtrij.

Dit artikel bevat informatie over algemene bewerkingen met behulp van de vlak-cmdlets voor het beheren van storage-accounts. Procedures voor: 

> [!div class="checklist"]
> * Lijst met storage-accounts
> * Een verwijzing naar een bestaand opslagaccount ophalen
> * Een opslagaccount maken 
> * Eigenschappen van het opslagaccount instellen
> * Ophalen en de toegangssleutels opnieuw genereren
> * Toegang tot uw storage-account beveiligen 
> * Storage Analytics inschakelen

In dit artikel vindt u koppelingen naar diverse andere artikelen in PowerShell voor opslag, zoals het inschakelen en toegang tot de Opslaganalyse, hoe u de gegevens vlak-cmdlets en toegang tot de Azure onafhankelijke clouds zoals China Cloud, Duits Cloud en Government Cloud.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

In deze oefening vereist Azure PowerShell moduleversie 4.4 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 

Voor deze oefening kunt u de opdrachten in een reguliere PowerShell-venster typen of u kunt de [Windows PowerShell Integrated Scripting Environment (ISE)](/powershell/scripting/getting-started/fundamental/windows-powershell-integrated-scripting-environment--ise-) en typt u de opdrachten in een teksteditor, en vervolgens een of meer opdrachten tegelijk als testen u doorloopt de voorbeelden in. U kunt de rijen die u wilt uitvoeren en klik op geselecteerde uitvoeren naar NET uitvoeren van deze opdrachten markeren.

Zie voor meer informatie over opslagaccounts [Inleiding tot Storage](storage-introduction.md) en [over Azure storage-accounts](storage-create-storage-account.md).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Lijst van de storage-accounts in het abonnement

Voer de [Get AzureRMStorageAccount](/powershell/module/azurerm.resources/get-azurermstorageaccount) cmdlet voor het ophalen van de lijst met opslagaccounts in het huidige abonnement. 

```powershell
Get-AzureRMStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Een verwijzing naar een opslagaccount ophalen

Vervolgens moet u een verwijzing naar een opslagaccount. U kunt een nieuw opslagaccount maken of een verwijzing naar een bestaand opslagaccount ophalen. De volgende sectie bevat beide methoden. 

### <a name="use-an-existing-storage-account"></a>Een bestaand opslagaccount gebruiken 

Voor het ophalen van een bestaand opslagaccount, moet u de naam van de resourcegroep en de naam van het opslagaccount. Stel de variabelen voor deze twee velden en gebruik vervolgens de [Get AzureRmStorageAccount](/powershell/module/azurerm.storage/Get-AzureRmStorageAccount) cmdlet. 

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
```

U hebt nu $storageAccount die naar een bestaand opslagaccount wijst.

### <a name="create-a-storage-account"></a>Een opslagaccount maken 

Het volgende script toont het maken van een opslagaccounts voor algemeen gebruik voor het met [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Nadat u het account maakt, halen de context die kan worden gebruikt in de volgende opdrachten in plaats van geven de verificatie met elke aanroep.

```powershell
# Get list of locations and select one.
Get-AzureRmLocation | select Location 
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 

# Set the name of the storage account and the SKU name. 
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"
    
# Create the storage account.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context. 
$ctx = $storageAccount.Context
```

Het script maakt gebruik van de volgende PowerShell-cmdlets: 

*   [Get-AzureRmLocation](/powershell/module/azurerm.storage/Get-AzureRmLocation) --haalt een lijst met geldige locaties. In het voorbeeld wordt `eastus` voor de locatie.

*   [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup) --maakt een nieuwe resourcegroep. Een resourcegroep is een logische container waarin uw Azure-resources worden geïmplementeerd en beheerd. Onze heet `teststoragerg`. 

*   [Nieuwe AzureRmStorageAccount](/powershell/module/azurerm.resources/New-AzureRmStorageAcccount) --het werkelijke storage-account maakt. In het voorbeeld wordt `testpshstorage`.

De naam van de SKU geeft het type van de replicatie voor het opslagaccount, zoals LRS (lokaal redundante opslag). Zie voor meer informatie over replicatie [Azure Storage-replicatie](storage-redundancy.md).

> [!IMPORTANT]
> De naam van uw opslagaccount moet uniek zijn binnen Azure en moet een kleine letter. Zie voor naamconventies en beperkingen [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
> 

U hebt nu een nieuw opslagaccount en een verwijzing naar deze. 

## <a name="manage-the-storage-account"></a>Het opslagaccount beheren

Nu dat u een verwijzing naar een nieuw opslagaccount of een bestaand opslagaccount hebt, wordt de volgende sectie bevat enkele van de opdrachten die u gebruiken kunt voor het beheren van uw opslagaccount.

### <a name="storage-account-properties"></a>Eigenschappen van het opslagaccount

Gebruik de instellingen voor een opslagaccount te wijzigen [Set AzureRmStorageAccount](/powershell/module/azurerm.resources/Set-AzureRmStorageAccount). Terwijl u de locatie van een opslagaccount of de resourcegroep waarin deze zich bevindt niet wijzigen, kunt u veel van de overige eigenschappen kunt wijzigen. Hieronder vindt u enkele van de eigenschappen die u kunt wijzigen met behulp van PowerShell.

* De **aangepast domein** toegewezen aan het opslagaccount.

* De **labels** toegewezen aan het opslagaccount. Labels worden vaak gebruikt om resources te categoriseren voor facturatie.

* De **SKU** wordt de replicatie-instelling voor het opslagaccount, zoals LRS voor lokaal redundante opslag. U kunt bijvoorbeeld wijzigen van standaard\_LRS naar Standard\_GRS of standaard\_RAGRS. Opmerking u standaard niet wijzigen\_ZRS- of Premium\_LRS naar andere SKU's, of wijzig andere SKU's in deze.

* De **toegangslaag** voor Blob storage-accounts. De waarde voor de toegangslaag is ingesteld op **hot** of **cool**, en kunt u uw kosten te minimaliseren door het selecteren van de toegangslaag die in overeenstemming brengt met hoe u het opslagaccount gebruiken. Zie voor meer informatie [Hot, koel, en archiveren van opslaglagen](../blobs/storage-blob-storage-tiers.md).

* De storage-service versleutelingsinstelling voor blob-opslag-en/of opslag van bestanden. Zie voor meer informatie over SSE [Opslagversleuteling Service](storage-service-encryption.md).

* Alleen HTTPS-verkeer toestaan. 

### <a name="manage-the-access-keys"></a>De toegangssleutels beheren

Een Azure Storage-account wordt geleverd met twee sleutels. Voor het ophalen van de sleutels, gebruik [Get-AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/Get-AzureRmStorageAccountKey). In dit voorbeeld wordt de eerste sleutel opgehaald. U haalt een met `Value[1]` in plaats van `Value[0]`.

```powershell
$storageAccountKey = `
    (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Gebruiken om de sleutel opnieuw genereren, [nieuw AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccountKey). 

```powershell
New-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1 
```

Gebruiken om de andere sleutel opnieuw genereren, `key2` als de naam van de sleutel in plaats van `key1`.

Een van uw sleutels genereren en haal vervolgens opnieuw om te zien of de nieuwe waarde.

> [!NOTE] 
> U moet uitvoeren om een zorgvuldige planning vóór het opnieuw genereren van de sleutel voor een productie-opslagaccount. Opnieuw genereren van een of beide sleutels, de toegang voor elke toepassing met de sleutel die is opnieuw gegenereerd zijn ongeldig. Zie voor meer informatie [opslagtoegangssleutels opnieuw genereren](storage-create-storage-account.md#regenerate-storage-access-keys).


### <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen 

Gebruik voor het verwijderen van een opslagaccount [verwijderen AzureRmStorageAccount](/powershell/module/azurerm.storage/Remove-AzureRmStorageAccount).

```powershell
Remove-AzureRmStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> Wanneer u een opslagaccount verwijdert, worden alle activa opgeslagen in het account ook verwijderd. Als u een account per ongeluk hebt verwijderd, onmiddellijk contact op met ondersteuning en opent u een ticket voor het herstellen van het opslagaccount. Herstel van uw gegevens kan niet worden gegarandeerd, maar werkt soms. Maak een nieuw opslagaccount geen met dezelfde naam als de oude totdat de ondersteuningsticket is opgelost. 
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>Beveiligen van uw storage-account met behulp van vnet's en firewalls

Standaard zijn alle opslagaccounts toegankelijk zijn voor elk netwerk dat toegang tot internet heeft. U kunt echter regels zodat alleen toepassingen van specifieke virtuele netwerken voor toegang tot een opslagaccount netwerk configureren. Zie voor meer informatie [Azure Storage-Firewalls configureren en virtuele netwerken](storage-network-security.md). 

Het artikel laat zien hoe deze instellingen met de volgende PowerShell-cmdlets beheren:
* [Add-AzureRmStorageAccountNetworkRule](/powershell/module/AzureRM.Storage/Add-AzureRmStorageAccountNetworkRule)
* [Update-AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset)
* [Remove-AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.storage/remove-azurermstorage-account-networkrule)

## <a name="use-storage-analytics"></a>Storage analytics gebruiken  

[Azure Storage Analytics](storage-analytics.md) bestaat uit [metrische gegevens Storage Analytics](/rest/api/storageservices/about-storage-analytics-metrics) en [Storage Analytics logboekregistratie](/rest/api/storageservices/about-storage-analytics-logging). 

**Metrische gegevens Storage Analytics** wordt gebruikt voor het verzamelen van metrische gegevens voor uw Azure storage-accounts die u gebruiken kunt voor het bewaken van de status van een opslagaccount. Metrische gegevens kan worden ingeschakeld voor blobs, bestanden, tabellen en wachtrijen.

**Logboekregistratie van Storage Analytics** gebeurt serverzijde en kunt u de details van de records voor geslaagde en mislukte aanvragen voor uw opslagaccount. Deze logboeken kunnen u de details van lezen, schrijven en verwijderen van bewerkingen op tabellen, wachtrijen en blobs, evenals de redenen voor mislukte aanvragen. Logboekregistratie is niet beschikbaar voor Azure Files.

U kunt controleren met de [Azure-portal](https://portal.azure.com), PowerShell of programmatisch met behulp van de storage-clientbibliotheek. 

> [!NOTE]
> U kunt inschakelen minuut analytics met PowerShell. Deze mogelijkheid is niet beschikbaar in de portal.
>

* Zie voor meer informatie over het inschakelen en weergeven van metrische gegevens Storage-gegevens met behulp van PowerShell, [in Azure Storage inschakelen metrische gegevens en weer te geven metrische gegevens](storage-enable-and-view-metrics.md#how-to-enable-metrics-using-powershell).

* Zie voor meer informatie over het inschakelen en ophalen van gegevens van registratie van opslag met behulp van PowerShell, [opslag logboekregistratie met behulp van PowerShell inschakelen](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#how-to-enable-storage-logging-using-powershell) en [zoeken naar uw logboekgegevens opslag logboekregistratie](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#finding-your-storage-logging-log-data).

* Zie voor gedetailleerde informatie over het gebruik van opslag metrische gegevens en opslag logboekregistratie met opslag oplossen [bewaking, diagnose en het oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="manage-the-data-in-the-storage-account"></a>De gegevens in de storage-account beheren

Nu dat u hoe uw storage-account met PowerShell beheren begrijpt, kunt u de volgende artikelen voor meer informatie over het openen van de gegevensbron-objecten in het opslagaccount.

* [Het beheren van BLOB's met PowerShell](../blobs/storage-how-to-use-blobs-powershell.md)
* [Bestanden met PowerShell beheren](../files/storage-how-to-use-files-powershell.md)
* [Het beheren van wachtrijen met PowerShell](../queues/storage-powershell-how-to-use-queues.md)
* [Azure Table storage bewerkingen uitvoeren met PowerShell](../../cosmos-db/table-storage-how-to-use-powershell.md)

Premium-functies biedt Azure Cosmos DB tabel API voor tabelopslag zoals klare globale distribueren, lage latentie leesbewerkingen en schrijfbewerkingen, automatische secundaire indexeren en de speciale doorvoer. 

* Zie voor meer informatie [Azure Cosmos DB tabel API](../../cosmos-db/table-introduction.md). 
* Zie voor meer informatie over het gebruik van PowerShell voor Azure Cosmos DB tabel API-bewerkingen uitvoeren, [uitvoeren Azure Cosmos DB tabel-API-bewerkingen met PowerShell](../../cosmos-db/table-powershell.md).

## <a name="independent-cloud-deployments-of-azure"></a>Onafhankelijke cloudimplementaties van Azure

Openbare Azure-Cloud de meeste mensen gebruiken voor hun globale Azure-implementatie. Er zijn ook een aantal onafhankelijke implementaties van Microsoft Azure omwille van de onafhankelijkheid enzovoort. Deze onafhankelijke implementaties worden aangeduid als 'environments'. Dit zijn de beschikbare omgevingen:

* [Azure Government Cloud](https://azure.microsoft.com/features/gov/)
* [Azure China Cloud beheerd door 21Vianet in China](http://www.windowsazure.cn/)
* [Duitse Azure-Cloud](../../germany/germany-welcome.md)

Raadpleeg voor informatie over toegang tot deze clouds en hun opslag met PowerShell [Managing Storage in de Azure-onafhankelijke clouds met behulp van PowerShell](storage-powershell-independent-clouds.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resourcegroep en een opslagaccount voor deze oefening hebt gemaakt, kunt yous alle van de activa die u hebt gemaakt door het verwijderen van de resourcegroep verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijdert deze de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Volgende stappen

Dit artikel bevat informatie over algemene bewerkingen met behulp van de vlak-cmdlets voor het beheren van storage-accounts. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Lijst met storage-accounts
> * Een verwijzing naar een bestaand opslagaccount ophalen
> * Een opslagaccount maken 
> * Eigenschappen van het opslagaccount instellen
> * Ophalen en de toegangssleutels opnieuw genereren
> * Toegang tot uw storage-account beveiligen 
> * Storage Analytics inschakelen

In dit artikel vindt u verwijzingen naar verschillende andere artikelen, zoals het beheren van de gegevensbron objecten, het inschakelen van de Storage Analytics en toegang tot de Azure onafhankelijke clouds zoals China Cloud, Duits Cloud en de Cloud van de overheid. Hier volgen enkele meer gerelateerde artikelen en bronnen ter referentie:

* [Azure Storage besturingselement vlak PowerShell-cmdlets](/powershell/module/AzureRM.Storage/)
* [Azure Storage-gegevens vlak PowerShell-cmdlets](/powershell/module/azure.storage/)
* [Windows PowerShell-referentie](https://msdn.microsoft.com/library/ms714469.aspx)
