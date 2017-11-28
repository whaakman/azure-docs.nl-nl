---
title: Grote hoeveelheden gegevens te verplaatsen naar/van de cloudopslag in Azure | Microsoft Docs
description: Een overzicht van de verschillende methoden voor het verplaatsen van gegevens naar en van Azure Storage.
services: storage
documentationcenter: 
author: JarrettRenshaw
manager: msmets
editor: tysonn
ms.assetid: 5e3947a9-d99b-4108-9d57-3eb67c03e7ba
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.openlocfilehash: db0f09433750a3af2d70039d780a25ad64bb4df1
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="moving-data-to-and-from-azure-storage"></a>Gegevens verplaatsen naar en uit Azure Storage
Als u wilt verplaatsen van on-premises gegevens naar Azure Storage (of andersom), zijn er verschillende manieren om dit te doen. De benadering die voor u het meest geschikt is afhankelijk van uw scenario. In dit artikel biedt een snel overzicht van verschillende scenario's en de juiste aanbiedingen voor elk criterium.

## <a name="building-applications"></a>Toepassingen maken
Als u een toepassing maakt, is het ontwikkelen van toepassingen met de REST-API of een van onze veel clientbibliotheken een uitstekende manier om gegevens te verplaatsen naar en van Azure Storage.

Azure Storage biedt uitgebreide clientbibliotheken voor .NET, iOS, Java, Android, Universal Windows Platform (UWP), Xamarin, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie [aan de slag met Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md) voor meer informatie.

Bovendien bieden wij ook de [Azure Storage-bibliotheek voor gegevensverplaatsing](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) dit is een bibliotheek is ontworpen voor high-performance kopiëren van gegevens naar en van Azure. Raadpleeg onze bibliotheek voor gegevensverplaatsing [documentatie](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Snel weergeven van/interactie met uw gegevens
Als u een eenvoudige manier om uw Azure Storage-gegevens weergeven en ook hebben de mogelijkheid om te uploaden en downloaden van uw gegevens wilt, klikt u vervolgens kunt u overwegen een Azure Storage Explorer.

Bekijk onze lijst met [Azure Storage Explorers](../storage-explorers.md) voor meer informatie.

## <a name="system-administration"></a>Systeembeheer
Als u vereisen of meer vertrouwd met een opdrachtregelprogramma (bijvoorbeeld systeembeheerders bent), zijn hier een aantal opties voor u te overwegen:

### <a name="azcopy"></a>AzCopy
AzCopy is een Windows-opdrachtregelprogramma dat is  ontworpen voor het high-performance kopiëren van gegevens van en naar Azure Storage. U kunt ook gegevens binnen een opslagaccount of tussen verschillende opslagaccounts kopiëren.

Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) voor meer informatie.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell is een module die cmdlets biedt voor het beheren van services op Azure. Het is een op taken gebaseerde opdrachtregelshell en scripttaal die speciaal is ontworpen voor systeembeheer.

Zie [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md) voor meer informatie.

### <a name="azure-cli"></a>Azure CLI
Azure CLI biedt een set van open-source platformoverschrijdende opdrachten voor het werken met Azure-services. Azure CLI is beschikbaar op Windows, OSX en Linux.

Zie [met de Azure CLI met Azure Storage](../storage-azure-cli.md) voor meer informatie.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Het verplaatsen van grote hoeveelheden gegevens met een langzaam netwerk
Een van de grootste uitdagingen die zijn gekoppeld aan het verplaatsen van grote hoeveelheden gegevens is de overdrachtstijd. Als u gegevens ophalen uit Azure Storage wilt zonder zorgen te hoeven maken over de kosten van netwerken of code te schrijven, is Azure Import/Export een passende oplossing.

Zie [Azure Import/Export](../storage-import-export-service.md) voor meer informatie.

## <a name="backing-up-your-data"></a>Back-ups van uw gegevens
Als u gewoon back-up van uw gegevens naar Azure Storage wilt, wordt Azure Backup is de manier om te gaan. Dit is een krachtige oplossing voor back-up on-premises gegevens en virtuele Azure-machines.

Zie [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) voor meer informatie.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Toegang tot uw gegevens on-premises als vanuit de cloud
Als u een oplossing nodig voor toegang tot uw gegevens on-premises en vanuit de cloud, moet vervolgens u met behulp van Azure hybride cloud opslagoplossing, StorSimple. Deze oplossing bestaat uit een fysiek StorSimple-apparaat dat op intelligente wijze winkels vaak gebruikte gegevens op SSD's en van tijd tot tijd gebruikt gegevens op HDD's en niet-actief/back-up/archivering van gegevens op Azure Storage.

Zie [StorSimple](../../storsimple/storsimple-overview.md) voor meer informatie.

## <a name="recovering-your-data"></a>Uw gegevens herstellen
Wanneer u lokale werkbelastingen en toepassingen hebt, moet u een oplossing waarmee uw bedrijf blijft uitvoeren in het geval van een noodgeval. Azure Site Recovery verwerkt replicatie, failovers en herstel van virtuele machines en fysieke servers. Gerepliceerde gegevens worden opgeslagen in Azure Storage, zodat u kunt een secundair datacenter ter plaatse overbodig.

Zie [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) voor meer informatie.
### <a name="moving-data-faq"></a>Veelgestelde vragen over gegevens te verplaatsen:
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>Kan ik migreren VHD's van de ene regio naar een andere zonder te kopiëren?
De enige manier om de VHD's kopiëren tussen regio heeft het kopiëren van de gegevens tussen de storage-accounts op elke regio. U kunt AZCopy gebruiken voor deze. Zie de overdracht van gegevens met het AzCopy-opdrachtregelprogramma voor meer informatie. Voor zeer grote hoeveelheden gegevens kunt u ook Azure Import/Export. Zie [Azure Import/Export](https://docs.microsoft.com/azure/storage/storage-import-export-service) voor meer informatie.
