---
title: Een upgrade uitvoeren naar een opslagaccount voor algemeen gebruik v2 - Azure Storage | Microsoft Docs
description: Upgrade uitvoeren naar storage-accounts voor algemeen gebruik v2.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: 8a0b823a12178df56417b05de28c2125ec155829
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45740815"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Een upgrade uitvoert naar een opslagaccount voor algemeen gebruik v2

Opslagaccounts voor algemeen gebruik v2 ondersteuning voor de nieuwste opslagfuncties van Azure en alle functionaliteit van voor algemeen gebruik v1 en Blob storage-accounts. Voor algemeen gebruik v2-accounts worden aanbevolen voor de meeste scenario's voor opslag. Voor algemeen gebruik v2-accounts bieden de laagste per GB capaciteit prijzen voor Azure Storage, evenals de bedrijfstak van concurrerende transactieprijzen in.

Een upgrade naar een opslagaccount voor algemeen gebruik v2 in uw voor algemeen gebruik v1- of Blob storage-accounts is eenvoudig. U kunt upgraden met behulp van de Azure portal, PowerShell of Azure CLI. Upgraden van een account kan niet ongedaan worden gemaakt en kan leiden tot kosten voor facturering.

## <a name="upgrade-using-the-azure-portal"></a>Upgraden met Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar uw opslagaccount.
3. In de **instellingen** sectie, klikt u op **configuratie**.
4. Klik onder **Soort account** op **Upgrade**.
5. Typ bij **Upgrade bevestigen** de naam van uw account. 
6. Klik op **Upgrade** aan de onderkant van de blade.

## <a name="upgrade-with-powershell"></a>Een upgrade uitvoeren met PowerShell

Als u wilt een account voor algemeen gebruik v1 upgraden naar een algemeen gebruik v2-account met behulp van PowerShell, moet u eerst PowerShell voor het gebruik van de meest recente versie van bijwerken de **AzureRm.Storage** module. Zie [Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van PowerShell. 

Daarna roept u de volgende opdracht uit om het account, vervangen door de naam van de resourcegroep en storage-account te werken:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Een upgrade uitvoeren met Azure CLI

Als u wilt een account voor algemeen gebruik v1 upgraden naar een algemeen gebruik v2-account met behulp van Azure CLI, moet u eerst de meest recente versie van Azure CLI installeren. Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor meer informatie over het installeren van de CLI. 

Daarna roept u de volgende opdracht uit om het account, vervangen door de naam van de resourcegroep en storage-account te werken:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>Geef een toegangslaag voor blob-gegevens

Voor algemeen gebruik v2-accounts ondersteunen alle Azure-opslagservices en gegevensobjecten, maar toegangslagen zijn alleen beschikbaar voor blok-blobs in Blob-opslag. Wanneer u een naar een opslagaccount voor algemeen gebruik v2 upgrade, kunt u een toegangslaag opgeven voor uw blob-gegevens. 

Laag kunnen u de meest rendabele opslag op basis van uw Verwachte gebruikspatronen kiezen. Blok-blobs kunnen worden opgeslagen in een hot, cool of archive-laag. Zie voor meer informatie over toegangslagen [Azure Blob storage: Hot, cool en archive storage-lagen](../blobs/storage-blob-storage-tiers.md).

Standaard een nieuw opslagaccount wordt gemaakt in de hot Storage-toegangslaag en een opslagaccount voor algemeen gebruik v1 is bijgewerkt naar de hot Storage-toegangslaag. Als u welke toegangslaag moet worden gebruikt voor uw na de gegevensupgrade verkennen weet, kunt u uw scenario. Er zijn twee scenario's voor normale gebruiker voor het migreren naar een algemeen gebruik v2-account:

* U beschikt over een bestaand opslagaccount voor algemeen gebruik v1 en wilt een wijziging evalueren in een opslagaccount voor algemeen gebruik v2, met de juiste opslaglaag voor blob-gegevens.
* U hebt besloten om een algemeen gebruik v2-opslagaccount gebruiken of al hebt en wilt evaluearen of u de laag hot of cool storage voor blob-gegevens gebruiken moet.

In beide gevallen is de eerste prioriteit schat de kosten voor opslag, toegang tot en werken met uw gegevens die zijn opgeslagen in een opslagaccount voor algemeen gebruik v2 en vergelijkt die met uw huidige kosten.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Schat de kosten voor uw huidige verbruikspatroon

Voor een schatting van de kosten van het opslaan en toegang tot blobgegevens in een opslagaccount voor algemeen gebruik v2 in een bepaalde laag, uw bestaande gebruikspatroon evalueren of een schatting maken van het verwachte gebruikspatroon. Doorgaans zijn de volgende gegevens hiervoor van belang:

* Uw Blob storage verbruik, in gigabytes, met inbegrip van:
    - Hoeveel gegevens worden opgeslagen in het opslagaccount?
    - Hoe verandert het gegevensvolume op maandbasis; worden oude gegevens voortdurend vervangen door nieuwe gegevens?
* Het primaire toegangspatroon voor uw Blob storage-gegevens, met inbegrip van:
    - Hoeveel gegevens worden gelezen en geschreven naar het storage-account? 
    - Het aantal leesbewerkingen ten opzichte van schrijf-bewerkingen plaatsvinden op de gegevens in de storage-account?

Om te beslissen over de beste toegangslaag voor uw behoeften, kan het handig zijn om te bepalen hoeveel capaciteit uw blob-gegevens wordt gebruikt en hoe die gegevens wordt gebruikt. 

Voor het verzamelen van gebruiksgegevens voor uw storage-account vóór de migratie, kunt u controleren het opslagaccount door middel [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor logboekregistratie uitvoert en metrische gegevens voor Azure-services, waaronder Azure Storage biedt. 

Schakel metrische gegevens over capaciteit in Azure Monitor voor het bewaken van gegevens over het verbruik voor blobs in uw storage-account. Capaciteit metrische gegevens vastleggen gegevens over hoeveel opslagruimte de blobs in uw account dagelijks gebruikt. Capaciteit metrische gegevens kan worden gebruikt om te schatten van de kosten voor opslag van gegevens in de storage-account. Als u wilt weten hoe Blob storage-capaciteit is prijs voor elk type account, Zie [prijzen voor blok-blob](https://azure.microsoft.com/pricing/details/storage/blobs/).

Voor het controleren van gegevenstoegangspatronen voor Blob-opslag, moet u de metrische gegevens van een transactie in Azure Monitor inschakelen. U kunt filteren op andere Azure Storage-bewerkingen om te schatten hoe vaak elke wordt genoemd. Voor meer informatie over hoe verschillende typen transacties worden in rekening gebracht voor blok en toevoeg-blobs voor elk type account, Zie [prijzen voor blok-blob](https://azure.microsoft.com/pricing/details/storage/blobs/).  

Zie voor meer informatie over het verzamelen van metrische gegevens van Azure Monitor [metrische gegevens van Azure Storage in Azure Monitor](storage-metrics-in-azure-monitor.md).

## <a name="next-steps"></a>Volgende stappen

- [Een opslagaccount maken](storage-quickstart-create-account.md)
- [Azure storage-accounts beheren](storage-account-manage.md)