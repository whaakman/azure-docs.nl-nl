---
title: Inleiding tot Azure Files | Microsoft Docs
description: Een overzicht van Azure Files, een service waarmee u netwerkbestandsshares kunt maken en gebruiken in de cloud volgens het industriestandaard SMB-protocol.
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
ms.date: 09/19/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5a4a26957c115277e7558c210560777af63d2d0f
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="introduction-to-azure-files"></a>Kennismaking met Azure Files
Met Azure Files beschikt u over volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard [SMB-protocol (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (ook wel CIFS of Common Internet File System genoemd). Azure-bestandsshares kunnen gelijktijdig worden gekoppeld door on-premises of cloudimplementaties van Windows, Linux en macOS. Bovendien kunnen Azure-bestandsshares worden opgeslagen in de cache op Windows-servers met Azure-bestandssynchronisatie (preview) voor snelle toegang tot locaties waar de gegevens wordt gebruikt.

## <a name="videos"></a>Video's
| Kennismaken met Azure Files (27 min.) | Zelfstudie voor Azure Files (5 min.)  |
|-|-|
| [![Screencast van de video Inleiding tot Azure Files - klik om af te spelen.](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Screencast van de zelfstudie over Azure Files - klik om af te spelen.](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-files-is-useful"></a>Waarom Azure Files handig is
Azure-bestandsshares kunnen worden gebruikt voor het volgende:

* **Ter vervanging van of ter aanvulling op on-premises bestandsservers**:  
    Azure Files kan worden gebruikt om bestandsshares op traditionele on-premises bestandsservers of NAS-apparaten volledig te vervangen of aan te vullen. Populaire besturingssystemen, zoals Windows, Mac OS en Linux kunnen een Azure-bestandsshare rechtstreeks koppelen, waar ter wereld ze zich ook bevinden. Azure-bestandsshares kunnen ook met Azure-bestandssynchronisatie worden gerepliceerd naar Windows-Servers, on-premises of in de cloud, voor een krachtige en gedistribueerde opslag in de cache van gegevens waar deze worden gebruikt.

* **'Lift- en shift'-toepassingen**:  
    Met Azure Files kunnen toepassingen die een bestandsshare voor het opslaan van gegevens uit een bestandstoepassing verwachten eenvoudig worden 'gelift en geshift' naar de cloud. Azure Files biedt zowel het "klassieke" lift- en shift-scenario, waarbij zowel de toepassing als de gegevens worden verplaatst naar Azure, als het "hybride" lift- en shift-scenario, waarin de toepassingsgegevens worden verplaatst naar Azure Files en de toepassing nog steeds on-premises wordt uitgevoerd. 

* **Cloudontwikkeling vereenvoudigen**:  
    Azure Files kan ook op verschillende manieren worden gebruikt voor het vereenvoudigen van nieuwe cloudontwikkelingsprojecten. Bijvoorbeeld:
    * **Gedeelde toepassingsinstellingen**:  
        Een algemeen patroon voor gedistribueerde toepassingen is de aanwezigheid van configuratiebestanden op een centrale locatie waar ze toegankelijk zijn vanuit veel andere toepassingsexemplaren. Toepassingsexemplaren kunnen hun configuratie laden via de File REST-API, en mensen kunnen hier indien nodig toegang toe krijgen door de SMB-share lokaal te koppelen.

    * **Diagnostische gegevens delen**:  
        Een Azure-bestandsshare is een handige locatie voor cloudtoepassingen voor het wegschrijven van hun logboeken, metrische gegevens en crashdumps. Logboeken kunnen door de toepassingsexemplaren worden geschreven via de File REST-API en ontwikkelaars kunnen er toegang toe krijgen door de bestandsshare te koppelen met hun lokale computer. Dit biedt veel flexibiliteit, aangezien ontwikkelaars kunnen ontwikkelen in de cloud zonder hun bestaande tooling waarmee ze bekend zijn en graag werken te hoeven opgeven.

    * **Ontwikkelen/testen/fouten opsporen**:  
        Wanneer ontwikkelaars of beheerders op virtuele machines in de cloud werken, hebben ze vaak een set hulpprogramma's nodig. Het kopiëren van deze hulpprogramma's naar elke VM kan een tijdrovend proces zijn. Door een Azure-bestandsshare lokaal te koppelen aan de virtuele machines, kunnen de ontwikkelaar en beheerder indien nodig snel toegang krijgen tot hun hulpprogramma's.

## <a name="key-benefits"></a>Belangrijkste voordelen
* **Gedeelde toegang**. Azure-bestandsshares bieden ondersteuning voor de industrienorm SMB-protocol, wat betekent dat u uw lokale bestandsshares naadloos kunt vervangen door de Azure-bestandsshares zonder dat u zich zorgen hoeft te maken over de compatibiliteit van toepassingen. Het delen van een bestandssysteem op meerdere machines, toepassingen/exemplaren is een belangrijk voordeel van Azure Files voor toepassingen die gegevens moeten kunnen delen. 
* **Volledig beheerd**. Azure-bestandsshares kunnen worden gemaakt zonder de noodzaak om hardware of een besturingssysteem te beheren. Dit betekent dat u geen essentiële beveiligingsupgrades hoeft te installeren op het serverbesturingssysteem of defecte harde schijven moet vervangen.
* **Scripts en hulpprogramma’s**. PowerShell-cmdlets en Azure CLI kunnen worden gebruikt voor het maken, koppelen en beheren van Azure-bestandsshares als onderdeel van het beheer van Azure-toepassingen. U kunt Azure-bestandsshares maken en beheren met Azure Portal en Azure Storage Explorer. 
* **Flexibiliteit**. Azure Files is vanaf de grond opgebouwd om altijd beschikbaar te zijn. Het vervangen van on-premises bestandsshares door Azure Files betekent dat u niet langer problemen ondervindt door lokale stroomstoringen of netwerkstoringen. 
* **Vertrouwde programmeerbaarheid**. Toepassingen die worden uitgevoerd in Azure hebben toegang tot de gegevens in de share via [System I/O-API's](https://msdn.microsoft.com/library/system.io.file.aspx). Dat betekent dat ontwikkelaars bestaande code en vaardigheden kunnen inzetten voor het migreren van bestaande toepassingen. Naast System IO-API's kunt u [Azure Storage-clientbibliotheken](https://msdn.microsoft.com/library/azure/dn261237.aspx) of de [REST-API van Azure Storage](/rest/api/storageservices/file-service-rest-api) gebruiken.

## <a name="next-steps"></a>Volgende stappen
* [Azure-bestandsshare maken](storage-how-to-create-file-share.md)
* [Verbinding maken en koppelen in Windows](storage-how-to-use-files-windows.md)
* [Verbinding maken en koppelen in Linux](storage-how-to-use-files-linux.md)
* [Verbinding maken en koppelen in macOS](storage-how-to-use-files-mac.md)
