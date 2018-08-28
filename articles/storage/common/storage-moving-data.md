---
title: Verplaatsen van grote hoeveelheden gegevens naar/vanuit de cloudopslag in Azure | Microsoft Docs
description: Een overzicht van de verschillende methoden voor het verplaatsen van gegevens naar en van Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/26/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 1df237a65a8b5312b20de19a99399b3a3dd075ff
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049607"
---
# <a name="moving-data-to-and-from-azure-storage"></a>Gegevens verplaatsen naar en uit Azure Storage
Als u wilt verplaatsen van on-premises gegevens naar Azure Storage (of omgekeerd), zijn er verschillende manieren om dit te doen. De benadering die het meest voor u geschikt is afhankelijk van uw scenario. In dit artikel biedt een kort overzicht van de verschillende scenario's en de juiste aanbiedingen voor elk criterium.

## <a name="building-applications"></a>Toepassingen ontwikkelen
Als u een toepassing ontwikkelen op basis van de REST-API bouwt of een van onze veel clientbibliotheken is een uitstekende manier om gegevens te verplaatsen naar en van Azure Storage.

Azure Storage biedt uitgebreide clientbibliotheken voor veel populaire talen, waaronder .NET, Java, Android, Go, Xamarin, C++, Node.JS, PHP, Ruby, Python en iOS. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie [aan de slag met Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md) voor meer informatie.

Daarnaast bieden we ook de [Azure Storage-bibliotheek voor gegevensverplaatsing](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) die een bibliotheek is ontworpen voor high-performance kopiëren van gegevens naar en van Azure is. Raadpleeg onze bibliotheek voor gegevensverplaatsing [documentatie](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Snel weergeven van/interactie met uw gegevens
Als u wilt dat een eenvoudige manier om uw Azure Storage-gegevens terwijl hiervoor ook de mogelijkheid om te uploaden en downloaden van uw gegevens weer te geven, klikt u vervolgens kunt u overwegen een Azure Storage Explorer.

Bekijk onze lijst met [Azure Opslagverkenners](../storage-explorers.md) voor meer informatie.

## <a name="system-administration"></a>Systeembeheer
Als u vereist of meer vertrouwd met een opdrachtregel-hulpprogramma (bijvoorbeeld systeembeheerders bent), vindt hier u een aantal opties voor u te overwegen:

### <a name="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen voor high-performance kopiëren van gegevens naar en van Azure Storage. U kunt ook gegevens binnen een opslagaccount of tussen verschillende opslagaccounts kopiëren. AzCopy is beschikbaar op [Windows](storage-use-azcopy.md) en klik op [Linux](storage-use-azcopy-linux.md).

Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) of [gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md) voor meer informatie.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell is een module die cmdlets biedt voor het beheren van services op Azure. Het is een op taken gebaseerde opdrachtregelshell en scripttaal die speciaal is ontworpen voor systeembeheer.

Zie [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md) voor meer informatie.

### <a name="azure-cli"></a>Azure-CLI
Azure CLI bevat een set open-source, platformoverschrijdende opdrachten om te werken met Azure-services. Azure CLI is beschikbaar in Windows, OSX en Linux.

Zie [met de Azure CLI met Azure Storage](../storage-azure-cli.md) voor meer informatie.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Het verplaatsen van grote hoeveelheden gegevens met een traag netwerk
Een van de grootste uitdagingen die zijn gekoppeld aan het verplaatsen van grote hoeveelheden gegevens is de tijd voor overdracht. Als u wilt ophalen van gegevens naar/van Azure-opslag zonder u zorgen te maken over de kosten van netwerken of schrijven van code, is een geschikte oplossing met Azure Import/Export.

Zie [Azure Import/Export](../storage-import-export-service.md) voor meer informatie.

## <a name="backing-up-your-data"></a>Back-ups van uw gegevens
Als u nodig eenvoudig hebt back-up van uw gegevens naar Azure Storage, wordt Azure Backup is de manier om te gaan. Dit is een krachtige oplossing voor back-ups van on-premises gegevens en virtuele Azure-machines.

Zie [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) voor meer informatie.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Toegang tot uw gegevens on-premises en naar de cloud
Als u een oplossing nodig voor toegang tot uw gegevens on-premises en in de cloud, moet klikt u vervolgens u overwegen van het Azure-oplossing voor hybride cloudopslag, StorSimple. Deze oplossing bestaat uit een fysiek StorSimple-apparaat dat op intelligente wijze winkels vaak gebruikte gegevens op SSD's, af en toe gegevens gebruikt op HDD's en niet-actief/back-up/archivering van gegevens in Azure Storage.

Zie [StorSimple](../../storsimple/storsimple-overview.md) voor meer informatie.

## <a name="recovering-your-data"></a>Uw gegevens herstellen
Wanneer u on-premises werkbelastingen en toepassingen, moet u een oplossing waarmee uw bedrijf om door te gaan die wordt uitgevoerd in het geval van een noodgeval. Azure Site Recovery-replicatie, failovers en herstel van virtuele machines en fysieke servers worden verwerkt. Gerepliceerde gegevens worden opgeslagen in Azure Storage, zodat u kunt elimineert de noodzaak van een secundaire lokaal datacenter.

Zie [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) voor meer informatie.
### <a name="moving-data-faq"></a>Verplaatsen van gegevens Veelgestelde vragen over:
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>Kan ik migreren VHD's van de ene regio naar een andere zonder te kopiëren?
De enige manier om de VHD's kopiëren tussen regio is het kopiëren van gegevens tussen opslagaccounts voor elke regio. U kunt AZCopy gebruiken voor deze. Zie de gegevens overdragen met het AzCopy-opdrachtregelprogramma voor meer informatie. Voor zeer grote hoeveelheden gegevens kunt u ook Azure Import/Export. Zie [Azure Import/Export](https://docs.microsoft.com/azure/storage/storage-import-export-service) voor meer informatie.
