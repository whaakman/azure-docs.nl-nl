---
title: Azure Data Box gebruiken om gegevens te verzenden naar - warme, koude, archief - blok-blob-laag | Microsoft-Docs in gegevens
description: Hierin wordt beschreven hoe u met Azure Data Box gegevens verzenden naar een geschikte block blob storage-laag, zoals warm, koud of archief
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: dc84d5850656a0898fba328b652d86f3d8995bef
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58118371"
---
# <a name="use-azure-data-box-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Azure Data Box gebruiken om gegevens te verzenden naar de juiste Azure Storage blob-laag

Azure Data Box worden grote hoeveelheden gegevens naar Azure verplaatst door het verzenden van een eigen opslagapparaat. U vult van het apparaat met gegevens en retourneren. De gegevens van Data Box is geüpload naar een standaardlaag die is gekoppeld aan de storage-account. U kunt de gegevens vervolgens verplaatsen naar een andere opslaglaag.

Dit artikel wordt beschreven hoe de gegevens die door Data Box is geüpload kunnen worden verplaatst naar een blob-laag warm, koud of archief.  

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Kies de juiste opslaglaag voor uw gegevens

Azure-opslag kunnen drie verschillende lagen voor het opslaan van gegevens in de meest rendabele manier - warm, koud of archief. Hot storage-laag is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend. Hot storage heeft hogere opslagkosten dan Cool en Archive storage, maar de laagste toegangskosten.

Er is een Cool storage-laag voor minder vaak gebruikte gegevens die moeten worden bewaard voor minimaal 30 dagen. De opslagkosten voor de koude laag is lager dan die van de hot storage-laag, maar de kosten voor gegevenstoegang hoog in vergelijking met Hot-laag zijn.

De Azure Archive-laag is offline en biedt de laagste kosten voor opslag, maar ook de hoogste toegangskosten. Deze laag is bedoeld voor gegevens die in archiefopslag voor een minimum van 180 dagen achterblijven. Voor meer informatie over elk van deze lagen en het prijsmodel, gaat u naar [vergelijking van de opslaglagen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#comparison-of-the-storage-tiers).

De gegevens van de Data Box is geüpload naar een opslaglaag die is gekoppeld aan de storage-account. Wanneer u een opslagaccount maakt, geeft u de toegangslaag op warm of koud. Afhankelijk van het toegangspatroon van uw werkbelasting en de kosten, kunt u deze gegevens verplaatsen van de standaardlaag naar een andere opslaglaag.

U kunt alleen objectopslaggegevens uw in Blob storage of General Purpose v2 (GPv2-accounts). General Purpose v1 (GPv1)-accounts bieden geen ondersteuning voor opslaglagen. Als u de juiste opslaglaag voor uw gegevens, bekijk de overwegingen omtrent [Azure Blob-opslag: Premium, Hot, Cool en Archive storage-lagen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Instellen van een standaard blob-laag

De standaard blob-laag wordt opgegeven bij de storage-account wordt gemaakt in Azure portal. Nadat een opslagtype is geselecteerd als GPv2 of Blob storage, kan het kenmerk toegangslaag worden opgegeven. De warme laag is standaard geselecteerd.

De lagen kunnen niet worden opgegeven als u probeert te maken van een nieuw account wanneer een Data Box bestellen. Nadat het account is gemaakt, kunt u het account in de portal voor het instellen van de standaard-toegangslaag kunt wijzigen.

U kunt ook maken u een storage-account eerst met het opgegeven kenmerk toegangslaag. Bij het maken van de Data Box-order, selecteert u het bestaande storage-account. Voor meer informatie over het instellen van de standaard blob-laag tijdens het opslagaccount is gemaakt, gaat u naar [een storage-account maken in Azure portal](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Gegevens verplaatsen naar een niet-standaard-laag

Zodra de gegevens van Data Box is geüpload naar de standaardlaag, kunt u de gegevens te verplaatsen naar een niet-standaard-laag. Er zijn twee manieren om te verplaatsen van deze gegevens naar een niet-standaard-laag.

- **Azure Blob storage-levenscyclusbeheer** -kunt u een benadering op basis van beleid automatisch gegevens in lagen of aan het einde van de levenscyclus is verlopen. Ga voor meer informatie naar [beheer van de levenscyclus van Azure Blob storage](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Uitvoeren van scripts** -u kunt een script aanpak via Azure PowerShell gebruiken om in te schakelen laaginstelling op blobniveau. U kunt aanroepen de `SetBlobTier` bewerking naar de laag ingesteld voor de blob.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Azure PowerShell gebruiken om in te stellen van de blob-laag

Volgende stappen wordt beschreven hoe u kunt instellen de blob-laag naar archief met behulp van een Azure PowerShell-script.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden. Zorg ervoor dat uw actieve PowerShell 5.0 of hoger. Type:

   `$PSVersionTable.PSVersion`     

2. Meld u aan bij de Azure PowerShell. 

   `Login-AzureRmAccount`  

3. Definieer de variabelen voor de storage-account, toegangssleutel, container en de opslagcontext.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Alle blobs in de container ophalen.

    `$blobs = Get-AzureStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Stel de laag van alle blobs in de container naar archief.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Een voorbeeld van de uitvoer wordt hieronder weergegeven:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzureRmAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzureStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Als u de gegevens wilt archiveren op wilt opnemen, de standaardlaag van de account instellen op warm. Als de standaardlaag is ' Cool ', is er een 30-daagse vroege verwijdering boete als de gegevens direct naar archief wordt verplaatst.

## <a name="next-steps"></a>Volgende stappen

-  Meer informatie over het adres de [algemene gegevens die cloudlagen scenario's aan de regels van levenscyclus](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples)

