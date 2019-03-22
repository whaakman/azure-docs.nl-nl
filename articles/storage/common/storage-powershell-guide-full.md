---
title: Azure PowerShell gebruiken met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure PowerShell-cmdlets voor Azure Storage.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 08/16/2018
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 78eeb986be5380d9e34a42872cbbdeee37654cc0
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316993"
---
# <a name="using-azure-powershell-with-azure-storage"></a>Azure PowerShell gebruiken met Azure Storage

Azure PowerShell wordt gebruikt om te maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. Voor Azure Storage, kunnen deze cmdlets worden onderverdeeld in twee categorieën--het besturingselement en de gegevenslaag. Het besturingselement vlak-cmdlets worden gebruikt voor het beheren van het opslagaccount--voor het maken van de storage-accounts, eigenschappen instellen, storage-accounts verwijderen, draaien van de toegangssleutels, enzovoort. De data plane-cmdlets worden gebruikt voor het beheren van de gegevens die zijn opgeslagen *in* het storage-account. Bijvoorbeeld: blobs uploaden, het maken van bestandsshares en berichten aan een wachtrij toe te voegen.

In dit artikel bevat informatie over algemene bewerkingen met behulp van de cmdlets management vlak om opslagaccounts te beheren. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Lijst met storage-accounts
> * Een verwijzing naar een bestaand opslagaccount ophalen
> * Create a storage account
> * Eigenschappen van het opslagaccount instellen
> * Ophalen en de toegangssleutel opnieuw genereren
> * Toegang tot uw storage-account beveiligen
> * Inschakelen van Storage Analytics

In dit artikel vindt u koppelingen naar diverse andere artikelen in PowerShell voor opslag, zoals het inschakelen en toegang tot de Opslaganalyse, hoe u de data plane-cmdlets gebruikt en over toegang tot de Azure-onafhankelijke clouds, zoals China-Cloud, de Duitse Cloud en de overheid Cloud.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In deze oefening is de Azure PowerShell-module Az 0,7 of hoger vereist. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps).

Voor deze oefening hebt u de opdrachten kunt invoeren in een reguliere PowerShell-venster, of kunt u de [Windows PowerShell Integrated Scripting Environment (ISE)](/powershell/scripting/getting-started/fundamental/windows-powershell-integrated-scripting-environment--ise-) en typt u de opdrachten in een editor, en vervolgens een of meer opdrachten tegelijk als testen u doorlopen de voorbeelden. U kunt de rijen die u wilt uitvoeren en klikt u op geselecteerde uitvoeren alleen uitvoeren van deze opdrachten markeren.

Zie voor meer informatie over de storage-accounts, [Inleiding tot Storage](storage-introduction.md) en [over Azure storage-accounts](storage-create-storage-account.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

```powershell
Connect-AzAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Lijst van de storage-accounts in het abonnement

Voer de [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) cmdlet voor het ophalen van de lijst met opslagaccounts in het huidige abonnement.

```powershell
Get-AzStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Een verwijzing naar een opslagaccount ophalen

Vervolgens moet u een verwijzing naar een opslagaccount. U kunt een nieuw opslagaccount maken of een verwijzing naar een bestaand opslagaccount worden opgehaald. De volgende sectie ziet u beide methoden.

### <a name="use-an-existing-storage-account"></a>Een bestaand opslagaccount gebruiken

Als u wilt ophalen van een bestaand opslagaccount, moet u de naam van de resourcegroep en de naam van het storage-account. Stel de variabelen voor deze twee velden in en gebruik vervolgens de [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) cmdlet.

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
```

U hebt nu $storageAccount, die naar een bestaand opslagaccount verwijst.

### <a name="create-a-storage-account"></a>Create a storage account

Het volgende script toont het maken van een opslagaccount voor algemeen gebruik door middel [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Nadat u het account hebt gemaakt, haal de opslagaccountcontext op, die in de volgende opdrachten kan worden gebruikt in plaats van de verificatie met elke aanroep op te geven.

```powershell
# Get list of locations and select one.
Get-AzLocation | select Location
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzResourceGroup -Name $resourceGroup -Location $location

# Set the name of the storage account and the SKU name.
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context.
$ctx = $storageAccount.Context
```

Het script maakt gebruik van de volgende PowerShell-cmdlets:

*   [Get-AzLocation](/powershell/module/az.resources/get-azlocation) --haalt een lijst van de geldige locaties. Het voorbeeld wordt `eastus` voor de locatie.

*   [Nieuwe AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) --wordt een nieuwe resourcegroep. Een resourcegroep is een logische container waarin uw Azure-resources worden geïmplementeerd en beheerd. Onze heet `teststoragerg`.

*   [Nieuwe AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) --het storage-account wordt gemaakt. Het voorbeeld wordt `testpshstorage`.

De SKU-naam geeft het type van de replicatie voor de storage-account, zoals LRS (lokaal redundante opslag). Zie voor meer informatie over replicatie [Azure Storage-replicatie](storage-redundancy.md).

> [!IMPORTANT]
> De naam van uw opslagaccount moet uniek zijn binnen Azure en moet een kleine letter. Zie voor naamgevingsregels en beperkingen [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
>

U hebt nu een nieuw opslagaccount en een verwijzing naar deze.

## <a name="manage-the-storage-account"></a>Het storage-account beheren

Nu dat u een verwijzing naar een nieuw opslagaccount of een bestaand opslagaccount hebt, wordt de volgende sectie bevat enkele van de opdrachten die u gebruiken kunt voor het beheren van uw storage-account.

### <a name="storage-account-properties"></a>Eigenschappen van het opslagaccount

U kunt de instellingen voor een storage-account wijzigen met [Set AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Terwijl u de locatie van een storage-account of de resourcegroep waarin deze zich bevindt niet wijzigen, kunt u veel van de andere eigenschappen kunt wijzigen. Hieronder vindt u enkele van de eigenschappen die u kunt wijzigen met behulp van PowerShell.

* De **aangepast domein** toegewezen aan de storage-account.

* De **tags** toegewezen aan de storage-account. Labels worden vaak gebruikt voor het categoriseren van resources voor factureringsdoeleinden.

* De **SKU** is de replicatie-instelling voor de storage-account, zoals LRS voor lokaal redundante opslag. Bijvoorbeeld, u kunt wijzigen van standaard\_LRS standaard\_GRS of standaard\_RAGRS. Houd er rekening mee dat u standaard niet wijzigen\_ZRS- of Premium\_LRS naar andere SKU's, of wijzig andere SKU's in deze.

* De **toegangslaag** voor Blob storage-accounts. De waarde voor de toegangslaag is ingesteld op **hot** of **cool**, en kunt u uw kosten te minimaliseren door het selecteren van de toegangslaag die aansluit bij de manier waarop u de storage-account gebruiken. Zie voor meer informatie, [Hot, cool en archive storage-lagen](../blobs/storage-blob-storage-tiers.md).

* Alleen HTTPS-verkeer toestaan.

### <a name="manage-the-access-keys"></a>De toegangssleutels beheren

Een Azure Storage-account wordt geleverd met twee sleutels. Als u wilt ophalen van de sleutels gebruiken [Get-AzStorageAccountKey](/powershell/module/az.Storage/Get-azStorageAccountKey). In dit voorbeeld wordt de eerste sleutel opgehaald. Als u wilt een ophalen, gebruikt u `Value[1]` in plaats van `Value[0]`.

```powershell
$storageAccountKey = `
    (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Als u wilt de sleutel opnieuw genereren, gebruikt u [New-AzStorageAccountKey](/powershell/module/az.Storage/New-azStorageAccountKey).

```powershell
New-AzStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1
```

Als u wilt de andere sleutel opnieuw genereren, gebruikt u `key2` als de naam van de sleutel in plaats van `key1`.

Een van de sleutels opnieuw genereren en vervolgens opnieuw uit om te zien of de nieuwe waarde kunt ophalen.

> [!NOTE]
> U moet uitvoeren voordat u het opnieuw genereren van de sleutel voor een productie-storage-account een zorgvuldige planning. Een of beide sleutels opnieuw genereren, worden ongeldig de toegang voor elke toepassing met behulp van de sleutel die is gegenereerd. Ga voor meer informatie naar [Toegangssleutels](storage-account-manage.md#access-keys).


### <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen

Als u wilt verwijderen van een storage-account, gebruikt u [Remove-AzStorageAccount](/powershell/module/az.storage/Remove-azStorageAccount).

```powershell
Remove-AzStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> Wanneer u een opslagaccount verwijdert, worden alle activa die zijn opgeslagen in het account ook verwijderd. Als u een account per ongeluk verwijdert, wordt onmiddellijk contact opnemen met ondersteuning en opent u een ticket voor het herstellen van de storage-account. Herstel van uw gegevens kan niet worden gegarandeerd, maar soms werkt. Maak een nieuw opslagaccount niet met dezelfde naam als de oude heeft, totdat het ondersteuningsticket is opgelost.
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>Beveiliging van uw storage-account met behulp van vnet's en firewalls

Alle opslagaccounts zijn standaard toegankelijk is via een netwerk dat toegang tot internet heeft. U kunt echter netwerkregels zodat alleen de toepassingen van specifieke virtuele netwerken voor toegang tot een opslagaccount. Zie voor meer informatie, [configureren van Azure Storage-Firewalls en virtuele netwerken](storage-network-security.md).

Het artikel wordt beschreven hoe u kunt deze instellingen met de volgende PowerShell-cmdlets beheren:
* [Add-AzStorageAccountNetworkRule](/powershell/module/az.Storage/Add-azStorageAccountNetworkRule)
* [Update-AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset)
* [Remove-AzStorageAccountNetworkRule](https://docs.microsoft.com/powershell/module/az.storage/remove-azstorageaccountnetworkrule)

## <a name="use-storage-analytics"></a>Storage analytics gebruiken  

[Azure Storage Analytics](storage-analytics.md) bestaat uit [Storage Analytics Metrics](/rest/api/storageservices/about-storage-analytics-metrics) en [Storage Analytics logboekregistratie](/rest/api/storageservices/about-storage-analytics-logging).

**Storage Analytics Metrics** wordt gebruikt voor het verzamelen van metrische gegevens voor uw Azure storage-accounts die u gebruiken kunt voor het bewaken van de status van een storage-account. Metrische gegevens kan worden ingeschakeld voor blobs, bestanden, tabellen en wachtrijen.

**Storage Analytics logboekregistratie** serverzijde plaatsvindt en kunt u details vastleggen voor zowel geslaagde als mislukte aanvragen naar uw opslagaccount. Deze logboeken kunnen u details van lezen, schrijven en verwijderbewerkingen met betrekking tot uw tabellen, wachtrijen en blobs, evenals de redenen voor mislukte aanvragen bekijken. Logboekregistratie is niet beschikbaar voor Azure Files.

U kunt controleren met behulp van de [Azure-portal](https://portal.azure.com), PowerShell, of programmatisch met behulp van de storage-clientbibliotheek.

> [!NOTE]
> Hier kunt u één minuut analytics met behulp van PowerShell. Deze mogelijkheid is niet beschikbaar in de portal.
>

* Zie voor meer informatie over het inschakelen en weergeven van metrische gegevens van Storage-gegevens met behulp van PowerShell, [Storage analytics metrics](storage-analytics-metrics.md).

* Zie voor meer informatie over het inschakelen en ophalen van logboekregistratie voor opslag gegevens met behulp van PowerShell, [het inschakelen van logboekregistratie van opslag met behulp van PowerShell](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data) en [vinden van uw logboekgegevens Storage Logging](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data).

* Zie voor gedetailleerde informatie over het gebruik van metrische opslaggegevens en logboekregistratie van opslag aan het oplossen van opslagproblemen [bewaking, diagnose en het oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="manage-the-data-in-the-storage-account"></a>De gegevens in de storage-account beheren

Nu dat u hoe u begrijpt voor het beheren van uw storage-account met PowerShell, kunt u de volgende artikelen voor meer informatie over toegang tot de objecten in de storage-account.

* [Hoe u blobs beheren met PowerShell](../blobs/storage-how-to-use-blobs-powershell.md)
* [Over het beheren van bestanden met PowerShell](../files/storage-how-to-use-files-powershell.md)
* [Over het beheren van wachtrijen met PowerShell](../queues/storage-powershell-how-to-use-queues.md)
* [Azure Table storage bewerkingen uitvoeren met PowerShell](../../storage/tables/table-storage-how-to-use-powershell.md)

Azure Cosmos DB Table-API biedt premium-functies voor tabelopslag, zoals kant en klare wereldwijde distributie, leesbewerkingen met lage latentie en schrijfbewerkingen, automatische secundaire indexering en speciaal toegewezen doorvoer.

* Zie voor meer informatie, [Azure Cosmos DB Table-API](../../cosmos-db/table-introduction.md).

## <a name="independent-cloud-deployments-of-azure"></a>Onafhankelijke implementaties van Azure

De meeste mensen gebruiken openbare Azure-Cloud voor de globale Azure-implementatie. Er zijn ook enkele onafhankelijke implementaties van Microsoft Azure omwille van de onafhankelijkheid enzovoort. Onafhankelijke implementaties worden aangeduid als 'omgeving'. Dit zijn de beschikbare omgevingen:

* [Azure Government Cloud](https://azure.microsoft.com/features/gov/)
* [Azure China 21Vianet Cloud uitgevoerd door 21Vianet in China](http://www.windowsazure.cn/)
* [Azure German Cloud](../../germany/germany-welcome.md)

Zie voor informatie over hoe u toegang tot deze clouds en hun opslag met PowerShell, [opslag beheren in de Azure-onafhankelijke clouds met behulp van PowerShell](storage-powershell-independent-clouds.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resourcegroep en een opslagaccount voor deze oefening hebt gemaakt, kunt yous alle activa die u hebt gemaakt door het verwijderen van de resourcegroep verwijderen. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijderd het de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Volgende stappen

In dit artikel bevat informatie over algemene bewerkingen met behulp van de cmdlets management vlak om opslagaccounts te beheren. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Lijst met storage-accounts
> * Een verwijzing naar een bestaand opslagaccount ophalen
> * Create a storage account
> * Eigenschappen van het opslagaccount instellen
> * Ophalen en de toegangssleutel opnieuw genereren
> * Toegang tot uw storage-account beveiligen
> * Inschakelen van Storage Analytics

In dit artikel vindt u verwijzingen naar verschillende andere artikelen, zoals over het beheren van de gegevensobjecten, het inschakelen van de Storage Analytics en over toegang tot de Azure-onafhankelijke clouds, zoals China-Cloud, Duitse Cloud en Government-Cloud. Hier volgen enkele andere gerelateerde artikelen en resources ter referentie:

* [Azure Storage besturingselement vlak van het PowerShell-cmdlets](/powershell/module/az.storage/)
* [Azure Storage-gegevens vlak PowerShell-cmdlets](/powershell/module/azure.storage/)
* [Windows PowerShell-referentie](https://msdn.microsoft.com/library/ms714469.aspx)
