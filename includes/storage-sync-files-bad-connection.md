---
title: bestand opnemen
description: bestand opnemen
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 359347e41264711a6ac0fa4d2dd0c3633590e917
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488022"
---
Deze fout kan optreden wanneer de Azure File Sync-service niet toegankelijk is vanaf de server is. U kunt deze fout oplossen door het uitvoeren van de volgende stappen uit:

1. Controleer of de service Windows `FileSyncSvc.exe` niet wordt geblokkeerd door uw firewall.
2. Controleer of poort 443 is geopend voor uitgaande verbindingen naar de Azure File Sync-service. U kunt dit doen met de `Test-NetConnection` cmdlet. De URL voor de `<azure-file-sync-endpoint>` tijdelijke aanduiding hieronder te vinden in de [Azure File Sync-proxy en firewall-instellingen](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall) document. 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Zorg ervoor dat de proxyconfiguratie werkt zoals verwacht is ingesteld. Dit kan worden gedaan met de `Get-StorageSyncProxyConfiguration` cmdlet. Meer informatie over het configureren van de proxyconfiguratie voor Azure File Sync kunt u vinden in de [Azure File Sync-proxy en firewall-instellingen](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Neem contact op met de netwerkbeheerder voor aanvullende hulp bij het oplossen van problemen verbinding met het netwerk.