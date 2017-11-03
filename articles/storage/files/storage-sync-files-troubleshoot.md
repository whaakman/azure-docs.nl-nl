---
title: Problemen met Azure File-synchronisatie (preview) | Microsoft Docs
description: Algemene problemen met het synchroniseren van Azure-bestand
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 6bc5f2da2b8628671037b9257db746e73cd3afad
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Problemen met Azure File-synchronisatie (preview)
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

In dit artikel is ontworpen om u te helpen u problemen op te lossen met uw implementatie Azure File-synchronisatie aangetroffen. Deze handleiding mislukt, kunt u zien hoe belangrijk logboeken verzamelen uit het systeem voor een nadere analyse van de problemen. Als u het antwoord op uw vraag hier niet ziet, altijd bereiken ons via de volgende kanalen (in volgorde escaleren):

1. In de sectie met opmerkingen van dit artikel.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Voor het maken van een nieuwe ondersteuningsaanvraag gaat u naar 'Help + ondersteuning' tabblad in de Azure portal en klik op 'Nieuw ondersteuningsverzoek'.

## <a name="agent-installation-and-server-registration"></a>Agent-installatie en server-registratie
<a id="agent-installation-failures"></a>**Het oplossen van problemen met mislukte agentinstallaties**  
Als de installatie van de Azure-bestand Sync-agent is mislukt, moet u de volgende opdracht uitvoeren vanaf een opdrachtprompt met verhoogde bevoegdheid logboekregistratie tijdens de installatie van de agent in te schakelen:

```
StorageSyncAgent.msi /l*v Installer.log
```

Zodra de installatie mislukt, Controleer de installer.log om de oorzaak te bepalen. 

> [!Note]  
> De agentinstallatie mislukt als u Microsoft Update gebruiken en de Windows Update-service wordt niet uitgevoerd.

<a id="server-registration-missing"></a>**Server niet wordt vermeld onder de geregistreerde Servers in de Azure portal**  
Als een server niet wordt vermeld onder Servers geregistreerd voor een Service-opslag-synchronisatie, voert u de volgende stappen uit:
1. Meld u aan met de server die u wilt registreren.
2. Open Windows Verkenner en navigeer naar de installatiemap van de opslag Sync-Agent (standaardlocatie is `C:\Program Files\Azure\StorageSyncAgent`). 
3. Voer ServerRegistration.exe en volg de wizard om de server geregistreerd met een opslag-Sync-Service.

<a id="server-already-registered"></a>**Serverregistratie het volgende bericht weergegeven na de installatie van de Azure-bestand Sync-agent: 'deze server is al geregistreerd'**  
![Een schermopname van het dialoogvenster serverregistratie met de fout 'server is al geregistreerd' bericht](media/storage-sync-files-troubleshoot/server-registration-1.png)

Dit bericht wordt weergegeven als de server eerder met een opslag-Sync-Service is geregistreerd. Volg de stappen om de registratie van de server met de huidige Storage Sync-Service en registreren met een nieuwe opslag Sync-Service, [Hef de registratie van een server met het synchroniseren van Azure bestand](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Als de server niet wordt vermeld onder de geregistreerde Servers in de Storage-Sync-Service, voert u de volgende PowerShell-opdrachten op de server die u wilt registratie:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Als de server deel van een cluster uitmaakt, is er een optionele `Reset-StorageSyncServer -CleanClusterRegistration` parameter die ook de registratie van het cluster verwijdert.

<a id="web-site-not-trusted"></a>**Bij het registreren van een server die ik talrijke 'website niet wordt vertrouwd' reacties, waarom krijgen?**  
Deze fout treedt op omdat de **verbeterde beveiliging van Internet Explorer** beleid is ingeschakeld tijdens de serverregistratie. Voor meer informatie over het correct uitschakelen de **verbeterde beveiliging van Internet Explorer** beleid, Zie [voorbereiden Windows-Servers voor gebruik met het synchroniseren van Azure bestand](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) en [Azure File implementeren Synchronisatie (preview)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Groep voor synchronisatie management
<a id="cloud-endpoint-using-share"></a>**Mislukt het maken van cloud-eindpunt met de volgende fout: 'De opgegeven Azure-bestandsshare is al in gebruik door een andere CloudEndpoint'**  
Deze fout treedt op als de Azure-bestandsshare al gebruikt door een andere Cloud-eindpunt wordt. 

Als u dit foutbericht ontvangt en de Azure-bestandsshare momenteel niet gebruikt door een Cloudeindpunt is, voert u de volgende stappen hieronder om te wissen van de metagegevens van de Azure-bestand synchronisatie op de Azure-bestandsshare:

> [!Warning]  
> De metagegevens voor een Azure-bestandsshare is momenteel in gebruik door een cloudeindpunt verwijdert, wordt Azure bestand synchronisatiebewerkingen mislukken. 

1. Navigeer naar uw Azure-bestandsshare in de Azure Portal.  
2. Klik met de rechtermuisknop op de Azure-bestandsshare en selecteer **metagegevens bewerken**
3. Klik met de rechtermuisknop op SyncService en selecteer **verwijderen**.

<a id="cloud-endpoint-authfailed"></a>**Cloud-eindpunt maken is mislukt met fout: AuthorizationFailed**  
Dit probleem treedt op als uw gebruikersaccount niet voldoende rechten voor het maken van een Cloudeindpunt heeft. 

Uw gebruikersaccount moet de volgende Microsoft Authorization-machtigingen hebben voor het maken van een Cloudeindpunt:  
* Lees: Roldefinitie ophalen
* Write: Maken of bijwerken van aangepaste roldefinitie
* Lees: Roltoewijzing ophalen
* Write: Roltoewijzing maken

De volgende ingebouwde rollen hebben de juiste machtigingen voor Microsoft Authorization:  
* Eigenaar
* Beheerder van gebruikerstoegang

Om te bepalen of uw gebruikersrol van het account de juiste machtigingen heeft, het volgende doen:  
* Klik op **resourcegroepen** in de Azure portal
* Selecteer de resourcegroep waar het opslagaccount zich bevindt en klik op **toegangsbeheer (IAM)**
* Klik op de **rol** (bijvoorbeeld eigenaar, bijdrager) voor uw gebruikersaccount.
* In de **Resource Provider** selecteert **Microsoft Authorization** 
* **Roltoewijzing** moet **lezen** en **schrijfmachtigingen**
* **Roldefinitie** moet **lezen** en **schrijfmachtigingen**

<a id="cloud-endpoint-deleteinternalerror"></a>**Verwijderen van een eindpunt is mislukt met fout: MgmtInternalError**  
Dit probleem kan optreden als de Azure File share of storage-account is verwijderd voordat u het eindpunt van de Cloud verwijdert. Dit probleem wordt opgelost in een toekomstige update en het Cloud-eindpunt kan worden verwijderd.

Als u wilt voorkomen dat dit probleem optreedt, kan het eindpunt Cloud vóór het verwijderen van de Azure File share of storage-account verwijderen.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Ik heb een bestand rechtstreeks in mijn Azure-bestandsshare via SMB of via de portal gemaakt. Hoe lang duurt het tot het bestand is gesynchroniseerd met de servers in de groep voor synchronisatie?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Het oplossen van synchronisatie niet werkt op een server**  
Als de synchronisatie niet op een server, het volgende doen:
- Controleer of dat een eindpunt van de Server bestaat in de Azure portal voor de map die u wilt synchroniseren met een Azure-bestandsshare:
    
    ![Een schermopname van een groep voor synchronisatie met een Cloud- en eindpunt van de Server in de Azure portal](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Controleer met behulp van Logboeken, zich in de gebeurtenislogboeken van de operationele en diagnose `Applications and Services\Microsoft\FileSync\Agent`.
- Bevestig op dat de server verbinding heeft met internet.
- Controleer of de Azure-bestand-Sync-service wordt uitgevoerd op de server door het openen van de Services MMC-module en controleer of dat de opslag Sync-Agent-service (FileSyncSvc) wordt uitgevoerd.

<a id="replica-not-ready"></a>**Synchronisatie mislukt met fout: 0x80c8300f - de replica is niet gereed voor de vereiste bewerking niet uitvoeren**  
Deze fout kan worden verwacht wanneer u een Cloudeindpunt maken en gebruiken van een Azure-bestandsshare die gegevens bevat. Wanneer de wijziging detectie is voltooid op de Azure-bestandsshare (kan tot 24 uur duren), moet synchronisatie eerst goed werkt.

<a id="broken-sync-files"></a>**Het oplossen van afzonderlijke bestanden niet kunnen synchroniseren**  
Als u afzonderlijke bestanden niet kunnen synchroniseren, voert u het volgende:
- Bekijk de gebeurtenislogboeken van de operationele en diagnose onder `Applications and Services\Microsoft\FileSync\Agent` in Logboeken
- Controleer of dat er zijn geen open ingangen in het bestand
    - Opmerking: Azure File-synchronisatie wordt periodiek momentopnamen VSS bestanden met open ingangen synchroniseren

## <a name="cloud-tiering"></a>Cloudopslaglagen 
<a id="files-fail-tiering"></a>**Het oplossen van bestanden die niet voldoen aan de laag**  
Als bestanden niet laag tot Azure-bestanden, moet u de volgende stappen uitvoeren:

- Controleer of dat de bestanden zich in de Azure-bestandsshare
    - Opmerking: Een bestand moet worden gesynchroniseerd naar een Azure-bestandsshare voordat tiers worden verdeeld
- Controleer de gebeurtenislogboeken operationele en diagnose, zich onder `Applications and Services\Microsoft\FileSync\Agent` in Logboeken
- Bevestig op dat de server verbinding heeft met internet 
- Controleer of dat de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys & StorageSyncGuard.sys) worden uitgevoerd
    - Open een opdrachtprompt met verhoogde bevoegdheid, voert u `fltmc` en controleer of de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

<a id="files-fail-recall"></a>**Bestanden die niet voldoen aan herinnerd oplossen**  
Als bestanden niet worden ingetrokken, moet u de volgende stappen uitvoeren:
- Controleer de gebeurtenislogboeken operationele en diagnose, zich onder `Applications and Services\Microsoft\FileSync\Agent` in Logboeken
- Controleer of dat de bestanden bestaat in de Azure-bestandsshare
- Bevestig op dat de server verbinding heeft met internet 
- Controleer of dat de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys & StorageSyncGuard.sys) worden uitgevoerd
    - Open een opdrachtprompt met verhoogde bevoegdheid, voert u `fltmc` en controleer of de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

<a id="files-unexpectedly-recalled"></a>**Het oplossen van bestanden wordt onverwacht teruggehaald op een server**  
Antivirusprogramma's, back-up en andere toepassingen die grote aantallen bestanden worden gelezen, wordt ongewenste teruggehaald tenzij ze het kenmerk offline respecteren en overslaan van de inhoud van die bestanden lezen. Offlinebestanden voor deze producten die ondersteuning bieden voor het overslaan voorkomt ongewenste teruggehaald bij het uitvoeren van bewerkingen zoals antivirusscans of back-uptaken.

Neem contact op met de softwareleverancier met betrekking tot het configureren van de oplossing moet worden overgeslagen offline bestanden lezen.

Verder ongewenste teruggehaald kunnen zich voordoen in andere scenario's zoals het bladeren door de bestanden in Verkenner. Openen van een map met bestanden cloud lagen in Verkenner op de server kan leiden tot ongewenste terughalen, meer als antivirus is ingeschakeld op de server.

## <a name="general-troubleshooting"></a>Algemene probleemoplossing
Als u problemen met het synchroniseren van Azure-bestand op een server, starten door het uitvoeren van het volgende:
- Bekijk de gebeurtenislogboeken diagnose en operationele in Logboeken
    - Synchroniseren, Tiering en terughalen problemen worden vastgelegd in de gebeurtenislogboeken diagnostische en operationele onder`Applications and Services\Microsoft\FileSync\Agent`
    - Problemen met het beheren van een server (bijvoorbeeld, configuratie-instellingen) worden vastgelegd in de gebeurtenislogboeken diagnostische en operationele onder`Applications and Services\Microsoft\FileSync\Management`
- Controleer of dat de service Azure File-synchronisatie wordt uitgevoerd op de server
    - Open de MMC-module Services en controleer of dat de opslag Sync-Agent-service (FileSyncSvc) wordt uitgevoerd
- Controleer of dat de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys & StorageSyncGuard.sys) worden uitgevoerd
    - Open een opdrachtprompt met verhoogde bevoegdheid, fltmc uitvoeren en controleer of dat de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

Als het probleem niet is opgelost als u de bovenstaande stappen hebt uitgevoerd, voert u het hulpprogramma AFSDiag door de volgende stappen uit te voeren:
1. Maak een map die wordt gebruikt voor het opslaan van de uitvoer AFSDiag (bijvoorbeeld c:\output).
2. Open een PowerShell-venster met verhoogde bevoegdheid en voer de volgende opdrachten (hits enter na elke opdracht):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Voer 1 (tenzij opgegeven uitgebreidere traceringen maken) voor het traceerniveau Azure bestand Sync kernel-modus en op ENTER drukken.
4. Voer 1 (tenzij opgegeven uitgebreidere traceringen maken) voor het traceerniveau Azure bestand Sync gebruiker-modus en op ENTER drukken.
5. Reproduceer het probleem en druk op D wanneer u klaar bent.
6. Een ZIP-bestand met Logboeken en traceringsbestanden bevindt zich in de uitvoermap die is opgegeven.

## <a name="see-also"></a>Zie ook
- [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md)
- [Oplossen van problemen in Windows Azure-bestanden](storage-troubleshoot-windows-file-connection-problems.md)
- [Problemen met Azure-bestanden in Linux](storage-troubleshoot-linux-file-connection-problems.md)
