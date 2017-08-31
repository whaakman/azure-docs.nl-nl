---
title: Inleiding tot Azure File Storage | Microsoft Docs
description: Inleiding tot Azure File Storage, waarmee netwerkbestandsshares beschikbaar worden gesteld in Microsoft Cloud
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 498af5cffb76e026c9a87127cab238f0f23b668a
ms.contentlocale: nl-nl
ms.lasthandoff: 08/21/2017

---

# <a name="introduction-to-azure-file-storage"></a>Inleiding tot Azure File Storage

Azure File Storage biedt netwerkbestandsshares volgens de industrienormen met het [SMB-protocol (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) en [CIFS (Common Internet File System)](https://technet.microsoft.com/library/cc939973.aspx). Azure-bestandsshares kunnen gelijktijdig worden gekoppeld door virtuele machines van Azure en on-premises implementaties van Windows, macOS of Linux. Een opslagaccount voor algemeen gebruikt biedt toegang tot Azure File Storage, Azure Blob Storage en Azure Queue Storage.

## <a name="videos"></a>Video's
| Inleiding tot Azure File storage (27 minuten) | Azure File Storage-zelfstudie (5 minuten)  |
|-|-|
| [![Screencast van de video Inleiding tot Azure File Storage - klik om af te spelen.](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Screencast van de zelfstudie over Azure File Storage - klik om af te spelen.](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Waarom Azure File Storage handig is

Azure File Storage biedt u de mogelijkheid om Windows Server-, Linux-, of NAS-bestandsservers die lokaal of in de cloud worden gehost te vervangen door een besturingssysteemvrije cloudbestandsshare. Azure File Storage biedt de volgende voordelen:

* **Gedeelde toegang** Azure-bestandsshares bieden ondersteuning voor de industrienorm SMB-protocol, wat betekent dat u uw lokale bestandsshares naadloos kunt vervangen door de Azure-bestandsshares zonder dat u zich zorgen hoeft te maken over de compatibiliteit van toepassingen. De mogelijkheid om een bestandsshare te gebruiken vanaf verschillende computers en vanuit diverse toepassingen/exemplaren, is een belangrijk voordeel van Azure File Storage.

* **Volledig beheerd** Azure-bestandsshares kunnen worden gemaakt zonder dat het nodig is om hardware of een besturingssysteem te beheren. Dit betekent dat u zich geen zorgen hoeft te maken over het patchen van de OS-server met essentiële beveiligingsupgrades of het vervangen van defecte harde schijven.

* **Scripts en hulpprogramma's** PowerShell-cmdlets en de Azure-CLI zijn beschikbaar voor het maken, koppelen en beheren van Azure-bestandsshares als onderdeel van het beheer van Azure-toepassingen. U kunt Azure-bestandsshares maken en beheren met behulp van [Azure Portal](https://portal.azure.com) en [Azure Storage Explorer](https://storageexplorer.com). 

* **Robuust** Azure File Storage is vanaf de grond opgebouwd om altijd beschikbaar te zijn. Het vervangen van on-premises bestandsshares door Azure File Storage betekent dat u niet langer problemen ondervindt door lokale stroomstoringen of netwerkstoringen. 

* **Vertrouwd programmeren** Toepassingen die worden uitgevoerd in Azure hebben toegang tot de gegevens in de share via [API's voor I/O van het bestandssysteem](https://msdn.microsoft.com/library/system.io.file.aspx). Dat betekent dat ontwikkelaars bestaande code en vaardigheden kunnen inzetten voor het migreren van bestaande toepassingen. Naast API's voor I/O van het bestandssysteem kunt u elke Azure Storage-clientbibliotheek gebruiken, zoals die voor [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), of de [REST-API van Azure Storage](/rest/api/storageservices/file-service-rest-api).

Azure-bestandsshares kunnen worden gebruikt voor het volgende:

* **On-premises bestandsservers vervangen** Azure File Storage kan worden gebruikt om bestandsshares op traditionele on-premises bestandsservers of NAS-apparaten volledig te vervangen. Populaire besturingssystemen, zoals Windows, Mac OS en Linux kunnen gemakkelijk een Azure-bestandsshare koppelen, waar ter wereld ze zich maar bevinden.

* **'Lift- en shift'-toepassingen**

    Met Azure File Storage kunt u toepassingen die gebruikmaken van on-premises bestandsshares gemakkelijk 'liften en shiften' om gegevens tussen onderdelen van de toepassing te delen. Om dit te realiseren, maakt elke virtuele machine verbinding met de bestandsshare en kan deze vervolgens bestanden lezen en schrijven op dezelfde manier als dit zou gebeuren op een on-premises bestandsshare.

* **Cloudontwikkeling vereenvoudigen**
    
    Azure File Storage kan op een aantal verschillende manieren worden gebruikt voor het vereenvoudigen van nieuwe cloudontwikkelingsprojecten.
    
    * **Gedeelde toepassingsinstellingen**
    
        Een algemene patroon voor gedistribueerde toepassingen is de aanwezigheid van configuratiebestanden op een centrale locatie waar ze toegankelijk zijn vanuit veel andere VM's. Dergelijke configuratiebestanden kunnen nu worden opgeslagen op een Azure-bestandsshare en worden gelezen door alle exemplaren van een toepassing. Deze instellingen kunnen ook worden beheerd via de REST-interface waarmee u wereldwijd toegang krijgt tot de configuratiebestanden.

    * **Diagnostische gegevens delen**
    
        Een Azure-bestandsshare kan ook worden gebruikt om diagnostische bestanden op te slaan, zoals logboeken, metrische gegevens en crashdumps. Als bestandshares beschikbaar zijn via zowel de SMB- als de REST-interface, kunt u toepassingen bouwen of gebruikmaken van tal van analysehulpprogramma's voor het verwerken en analyseren van de diagnostische gegevens.

    * **Ontwikkelen/testen/fouten opsporen**

        Wanneer ontwikkelaars of beheerders op virtuele machines in de cloud werken, hebben ze vaak een set hulpprogramma's nodig. Het installeren en distribueren van deze hulpprogramma's op elke virtuele machine waar ze nodig zijn, kan een tijdrovend proces zijn. Met Azure File Storage kan een ontwikkelaar of beheerder zijn favoriete hulpprogramma’s opslaan op een bestandsshare, waarmee eenvoudig verbinding kan worden gemaakt vanaf elke virtuele machine.
        
## <a name="how-does-it-work"></a>Hoe werkt het?

Het beheren van Azure-bestandsshares is veel eenvoudiger dan het beheren van bestandsshares op locatie. Het volgende diagram illustreert de constructie van Azure File Storage-management:

![Bestandsstructuur](./media/storage-files-introduction/files-concepts.png)

* **Opslagaccount** Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Share** Een File Storage-share is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bovenliggende share. Een account kan een onbeperkt aantal shares bevatten en een bestandsshare kan een onbeperkt aantal bestanden bevatten, tot de totale capaciteit van 5 TB van de bestandsshare.

* **Map** Een optionele hiërarchie van mappen.

* **Bestand** Een bestand in de share. Een bestand mag maximaal 1 TB groot zijn.

* **URL-indeling** Bestanden kunnen worden opgevraagd met de volgende URL-indeling:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```

## <a name="next-steps"></a>Volgende stappen

* [Azure-bestandsshare maken](storage-how-to-create-file-share.md)
* [Verbinding maken en koppelen in Windows](storage-how-to-use-files-windows.md)
* [Verbinding maken en koppelen in Linux](storage-how-to-use-files-linux.md)
* [Verbinding maken en koppelen in macOS](storage-how-to-use-files-mac.md)
* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen in Windows](storage-troubleshoot-windows-file-connection-problems.md)   
* [Problemen oplossen in Linux](storage-troubleshoot-linux-file-connection-problems.md)   

<!-- Rena I would remove any articles from here that are more than a year old. - Robin-->
### <a name="conceptual-articles-and-videos"></a>Conceptuele artikelen en video's
* [Azure File Storage: een naadloos SMB-bestandssysteem voor Windows en Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Hulpprogramma-ondersteuning voor Azure File Storage
* [AzCopy gebruiken met Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [De Azure CLI gebruiken met Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blogberichten
* [Azure File storage is now generally available (Azure File Storage is nu algemeen beschikbaar)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage (Een kijkje achter de schermen van Azure File Storage)](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (Introductie van Microsoft Azure File-service)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Gegevens migreren naar Azure File Storage](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Naslaginformatie
* [Naslaginformatie over de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Naslaginformatie over REST API voor bestandsservices](http://msdn.microsoft.com/library/azure/dn167006.aspx)

