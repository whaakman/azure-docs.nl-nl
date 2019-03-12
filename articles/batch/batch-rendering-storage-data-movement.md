---
title: Opslag en verkeer voor de rendering - Azure Batch
description: Opties voor opslag en verkeer voor rendering van workloads
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 15f5fe6e74cbe8cbffff31114e8aa588ec4a7ebb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537997"
---
# <a name="storage-and-data-movement-options-for-rendering-asset-and-output-files"></a>Opslag en verkeer opties voor het renderen van activa- en uitvoer bestanden

Er zijn meerdere opties voor het maken van de scène en activa-bestanden beschikbaar voor de rendering-toepassingen op de pool-VM's:

* [Azure blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction):
  * Scène en asset bestanden worden geüpload BLOB storage vanuit een lokale bestandssysteem. Wanneer de toepassing wordt uitgevoerd door een taak, klikt u vervolgens de vereiste bestanden gekopieerd uit de blobopslag bij de virtuele machine zodat ze kunnen worden geopend door de renderingtoepassing. De uitvoerbestanden worden geschreven door de van renderingtoepassing op de VM-schijf en vervolgens gekopieerd naar blob-opslag.  Indien nodig, kunnen de uitvoerbestanden van blob-opslag worden gedownload naar een lokale bestandssysteem.
  * Azure blob-opslag is een eenvoudige en voordelige optie voor kleinere projecten.  Als alle asset bestanden zijn vereist op elke groep van virtuele machine, klikt u vervolgens wanneer het aantal en de grootte van de assetbestanden verhoogt zijn moet zorgvuldig worden uitgevoerd om te controleren of de bestandsoverdrachten zo efficiënt mogelijk.  
* Azure-opslag als een bestand met [blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux):
  * Voor virtuele Linux-machines kunt een storage-account worden weergegeven en als een bestandssysteem gebruikt bij het stuurprogramma van het virtuele bestandssysteem blobfuse wordt gebruikt.
  * Deze optie heeft als voordeel dat het is uiterst rendabele, als er geen virtuele machines zijn vereist voor het bestandssysteem, plus blobfuse opslaan in cache op de virtuele machines vermijdt u herhaalde downloads van dezelfde bestanden die meerdere jobs en taken.  Verplaatsing van gegevens is eveneens eenvoudig als de bestanden gewoon blobs zijn en standaard-API's en hulpprogramma's, zoals azcopy gebruikt, kunnen worden gebruikt om bestand tussen een on-premises bestandssysteem en de Azure storage te kopiëren.
* Bestandssysteem of bestandsshare:
  * Afhankelijk van VM-besturingssysteem en prestaties/schaalvereisten, klikt u vervolgens opties zijn onder andere [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction), met behulp van een virtuele machine met gekoppelde schijven voor NFS, met behulp van meerdere virtuele machines met gekoppelde schijven voor een gedistribueerd bestandssysteem, zoals GlusterFS, of met behulp van een aanbieding van derden.
  * [Avere systemen](https://www.averesystems.com/) maakt nu deel uit van Microsoft en oplossingen in de nabije toekomst die ideaal voor grote, hoogwaardige rendering zijn hebben.  De oplossing Avere wordt een NFS op basis van Azure in- of SMB-cache moet worden gemaakt die wordt gebruikt in combinatie met blob-opslag of met on-premises NAS-apparaten.
  * Met een bestandssysteem bestanden kunnen worden gelezen of geschreven rechtstreeks naar het bestandssysteem of tussen een bestandssysteem en de pool-VM's kunnen worden gekopieerd.
  * Een gedeeld bestandssysteem kunt een groot aantal activa gedeeld tussen projecten en taken kunnen worden gebruikt met het weergeven van taken die alleen toegang tot wat vereist is.

## <a name="using-azure-blob-storage"></a>Met behulp van Azure blob-opslag

Een blob storage-account of een opslagaccount voor algemeen gebruik v2 moet worden gebruikt.  Deze twee storage-accounttypen kunnen worden geconfigureerd met aanzienlijk hogere limieten in vergelijking met een opslagaccount voor algemeen gebruik v1, zoals beschreven in [dit blogbericht](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).  Wanneer geconfigureerd, kunnen de hogere limieten veel betere prestaties en schaalbaarheid, met name wanneer er veel groep VM's toegang tot het opslagaccount.

### <a name="copying-files-between-client-and-blob-storage"></a>Kopiëren van bestanden tussen client en blob-opslag

Als u wilt kopiëren van bestanden en naar Azure storage, verschillende mechanismen kunnen worden gebruikt met inbegrip van de blob storage-API, de [Azure Storage-bibliotheek voor gegevensverplaatsing](https://github.com/Azure/azure-storage-net-data-movement), de azcopy-opdrachtregelprogramma voor [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) of [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux), [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), en [Azure Batch Explorer](https://azure.github.io/BatchExplorer/).

Bijvoorbeeld, kunnen met behulp van azcopy, alle activa in een map worden overgedragen als volgt:


`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /Y`

Alleen gewijzigde bestanden wilt kopiëren, kan de parameter /XO worden gebruikt:

`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /XO /Y`

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Invoeractivum bestanden kopiëren van blob-opslag naar virtuele machines van de Batch-pool

Er zijn een aantal verschillende manieren om te kopiëren van bestanden met de aanbevolen aanpak is bepaald door de grootte van de activa van de taak.
De eenvoudigste manier is het kopiëren van alle assetbestanden aan de groep virtuele machines voor elke taak:

* Als er bestanden uniek is voor een taak, maar zijn vereist voor de taken van een taak, een [jobvoorbereidingstaak](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask) kan worden opgegeven om alle bestanden te kopiëren.  De jobvoorbereidingstaak wordt één keer uitgevoerd wanneer de eerste taak wordt uitgevoerd op een virtuele machine, maar is niet opnieuw uitgevoerd voor de volgende taken.
* Een [jobvrijgevingstaak](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask) moet worden opgegeven voor het verwijderen van de bestanden per taak wanneer de taak is voltooid; dit voorkomt dat de VM-schijf ophalen gevuld door alle bestanden van de taak asset.
* Wanneer er meerdere taken met behulp van de activa van de hetzelfde, met alleen incrementele wijzigingen aan de activa voor elke taak worden vervolgens alle assetbestanden nog steeds gekopieerd, zelfs als slechts een subset zijn bijgewerkt.  Dit zou inefficiënt zijn wanneer er veel grote assetbestanden.

Wanneer assetbestanden worden hergebruikt tussen taken met alleen incrementele wijzigingen tussen taken, klikt u vervolgens is een efficiënter, maar iets bewerkelijker benadering voor het opslaan van assets in de gedeelde map op de virtuele machine en gewijzigde bestanden synchroniseren.

* De jobvoorbereidingstaak zou het exemplaar met behulp van azcopy met de parameter /XO naar de gedeelde map van virtuele machine wordt opgegeven door de omgevingsvariabele AZ_BATCH_NODE_SHARED_DIR uitvoeren.  Dit wordt alleen gewijzigde bestanden kopiëren naar elke virtuele machine.
* Ze moeten krijgen tot de grootte van alle assets om te controleren of dat ze past op de tijdelijke schijf van de pool-VM's.

Azure Batch heeft ingebouwde ondersteuning voor het kopiëren van bestanden tussen een storage-account en een Batch-pool VM's.  Taak [bronbestanden](https://docs.microsoft.com/rest/api/batchservice/job/add#resourcefile) kopiëren uit de opslag van bestanden naar de pool-VM's en kan worden opgegeven voor de jobvoorbereidingstaak.  Helaas, als er honderden bestanden het is mogelijk om een limiet en taken mislukken.  Wanneer er grote aantallen activa is het aanbevolen het gebruik van de azcopy-opdrachtregel in de jobvoorbereidingstaak, kunt u jokertekens gebruiken en heeft geen limiet.

### <a name="copying-output-files-to-blob-storage-from-batch-pool-vms"></a>Kopiëren van uitvoerbestanden naar het blob-opslag van virtuele machines van de Batch-pool

[Uitvoerbestanden](https://docs.microsoft.com/rest/api/batchservice/task/add#outputfile) bestanden van de gebruikte kopiëren uit een groep met virtuele machine naar de opslag kan zijn.  Een of meer bestanden kunnen worden gekopieerd van de virtuele machine naar een opgegeven opslagaccount als de taak is voltooid.  De gerenderde uitvoer moet worden gekopieerd, maar ook kan het wenselijk zijn voor het opslaan van logboekbestanden.

## <a name="using-a-blobfuse-virtual-file-system-for-linux-vm-pools"></a>Met behulp van een virtueel bestandssysteem blobfuse voor Linux-VM-pools

Blobfuse is een stuurprogramma van het virtuele bestandssysteem voor Azure Blob Storage, waarmee u toegang krijgt tot bestanden die zijn opgeslagen als blobs in een Storage-account via het bestandssysteem van Linux.

De pool, het bestandssysteem wanneer aan de slag kunnen koppelen of de koppeling kan gebeuren als onderdeel van een jobvoorbereidingstaak – een taak die alleen wordt uitgevoerd wanneer de eerste taak in een taak wordt uitgevoerd op een knooppunt.  Blobfuse kan worden geconfigureerd als u wilt gebruikmaken van zowel een RAM-schijf en de lokale SSD van virtuele machines voor het opslaan van bestanden, die aanzienlijk prestaties als meerdere taken op een knooppunt toegang tot een aantal dezelfde bestanden.

[Voorbeeldsjablonen zijn beschikbaar](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/vray/render-linux-with-blobfuse-mount) om uit te voeren van zelfstandige V-Ray met behulp van een bestandssysteem blobfuse wordt weergegeven en kan worden gebruikt als basis voor sjablonen voor andere toepassingen.

### <a name="accessing-files"></a>Toegang tot bestanden

Taken van Jobs opgeven paden voor de invoerbestanden en uitvoerbestanden met behulp van het gekoppelde bestandssysteem.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Invoeractivum bestanden kopiëren van blob-opslag naar virtuele machines van de Batch-pool

Als bestanden gewoon de blobs in Azure Storage en vervolgens de standaard blob-API's, hulpprogramma's zijn en gebruikersinterfaces kunnen worden gebruikt om bestanden kopiëren tussen een on-premises bestand systeem- en blob-opslag; bijvoorbeeld, azcopy Storage Explorer, Batch Explorer, enzovoort.

## <a name="using-azure-files-with-windows-vms"></a>Met behulp van Azure Files met Windows-VM 's

[Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het SMB-protocol.  Azure Files is gebaseerd op Azure blob-opslag; het is [betaalbare](https://azure.microsoft.com/pricing/details/storage/files/) en kunnen worden geconfigureerd met replicatie van gegevens naar een andere regio dus globaal redundante.  [Schalen van doelen](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#azure-files-scale-targets) , om te bepalen als Azure-bestanden moet worden gebruikt vanwege de grootte van de prognose en aantal assetbestanden moeten worden gecontroleerd.

Er is een [blogbericht](https://blogs.msdn.microsoft.com/windowsazurestorage/2014/05/26/persisting-connections-to-microsoft-azure-files/) en [documentatie](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) waarin wordt beschreven hoe u een Azure-bestandsshare koppelen.

### <a name="mounting-an-azure-files-share"></a>Een Azure-bestandsshare koppelen

Als u wilt gebruiken in Batch, moet een koppelbewerking telkens wanneer een taak wordt uitgevoerd als het is niet mogelijk om vast te leggen van de verbinding tussen taken worden uitgevoerd.  De eenvoudigste manier om dit te doen is met cmdkey referenties met behulp van de begintaak opgegeven in de configuratie van de pool persistent maken en vervolgens de bestandsshare voor elke taak koppelen.

Gebruik het voorbeeld van cmdkey in een pool-sjabloon (escape voor gebruik in JSON-bestand): Houd er rekening mee dat wanneer de cmdkey aanroep van de aanroep netgebruik scheiden, de gebruikerscontext voor de begintaak moet zijn hetzelfde als die voor het uitvoeren van de taken:

```
"startTask": {
  "commandLine": "cmdkey /add:storageaccountname.file.core.windows.net
    /user:AZURE\\markscuscusbatch /pass:storage_account_key",
  "userIdentity":{
    "autoUser": {
      "elevationLevel": "nonadmin",
      "scope": "pool"
    }
}
```

Voorbeeld van de taak taak vanaf de opdrachtregel:
```
"commandLine":"net use S:
  \\\\storageaccountname.file.core.windows.net\\rendering &
3dsmaxcmdio.exe -v:5 -rfw:0 -10 -end:10
  -bitmapPath:\"s:\\3dsMax\\Dragon\\Assets\"
  -outputName:\"s:\\3dsMax\\Dragon\\RenderOutput\\dragon.jpg\"
  -w:1280 -h:720
  \"s:\\3dsMax\\Dragon\\Assets\\Dragon_Character_Rig.max\""
```

### <a name="accessing-files"></a>Toegang tot bestanden

Taken van Jobs opgeven paden voor de invoerbestanden en uitvoerbestanden met behulp van het gekoppelde bestandssysteem, met behulp van een toegewezen station of een UNC-pad.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Invoeractivum bestanden kopiëren van blob-opslag naar virtuele machines van de Batch-pool

Azure-bestanden worden ondersteund door de belangrijkste API's en hulpprogramma's met Azure Storage ondersteuning; bijvoorbeeld azcopy, Azure CLI, Storage Explorer, Azure PowerShell, Batch Explorer, enzovoort.

[Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) is beschikbaar voor automatisch synchroniseren van bestanden tussen een on-premises bestandssysteem en een Azure-bestandsshare.

## <a name="next-steps"></a>Volgende stappen

Zie de gedetailleerde documentatie voor meer informatie over de opties voor opslag:

* [Azure blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
* [Blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux)
* [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)
