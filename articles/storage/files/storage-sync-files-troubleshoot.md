---
title: Problemen met Azure File-synchronisatie (preview) | Microsoft Docs
description: Algemene problemen met het synchroniseren van Azure-bestand.
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
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 4f022bf227c8d460d014ea9bbc5dc426f0ada511
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Problemen met Azure File-synchronisatie (preview)
Gebruik Azure bestand Sync (preview) te centraliseren bestandsshares van uw organisatie in Azure-bestanden, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Azure File-synchronisatie transformeert Windows Server in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB en NFS FTPS gebruiken. U kunt zoveel caches als u over de hele wereld nodig hebben.

In dit artikel is ontworpen om u te helpen u problemen op te lossen die met uw Azure-bestand Sync-implementatie optreden kunnen. Er wordt ook beschreven hoe belangrijk logboeken verzamelen uit het systeem als een nadere analyse van het probleem vereist is. Als u het antwoord op uw vraag niet ziet, u kunt contact met ons opnemen via de volgende kanalen (in groeiende volgorde):

1. Het gedeelte met opmerkingen van dit artikel.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft ondersteuning. Maken van een nieuw ondersteuningsverzoek in de Azure-portal op de **Help** tabblad de **Help + ondersteuning** knop en selecteer vervolgens **nieuw ondersteuningsverzoek**.

## <a name="storage-sync-service-object-management"></a>Opslagbeheer Sync-Service-object
Als u een resource verplaatsen van een abonnement aan een ander abonnement doet, is bestand sync (opslag Sync-Service) geblokkeerd kunnen worden verplaatst. 

## <a name="agent-installation-and-server-registration"></a>Agent-installatie en server-registratie
<a id="agent-installation-failures"></a>**Oplossen van problemen met mislukte agentinstallaties**  
Voer de volgende opdracht in te schakelen logboekregistratie tijdens de installatie van de agent als de installatie van de Azure-bestand Sync-agent bij een opdrachtprompt met verhoogde bevoegdheid mislukt:

```
StorageSyncAgent.msi /l*v Installer.log
```

Bekijk installer.log om de oorzaak van de installatie is mislukt. 

> [!Note]  
> De agentinstallatie mislukt als de computer is ingesteld op Microsoft Update gebruiken en de Windows Update-service wordt niet uitgevoerd.

<a id="agent-installation-on-DC"></a>**Installatie van agent mislukt op Active Directory-domeincontroller** als u probeert en de synchronisatie-agent installeren op een Active Directory-domeincontroller waarbij de eigenaar van de PDC-functie op een Windows Server 2008 R2 of lager dan de versie van het besturingssysteem is, kunt u het probleem mogelijk bereikt waarbij de synchronisatie Agent niet worden geïnstalleerd.

Om op te lossen, draagt u de functie PDC naar een andere domein domeincontrollers met Windows Server 2012R2 of meer recente daarna synchronisatie te installeren.

<a id="agent-installation-websitename-failure"></a>**Installatie van agent mislukt vanwege de volgende fout: 'Opslag Sync-Agent is voortijdig beëindigd'**  
Dit probleem kan zich voordoen met versie 1.x-agent en of de IIS-website naam Standaard is gewijzigd. Om dit probleem omzeilen door onze 2.0.11+-agent te gebruiken.

<a id="server-registration-missing"></a>**Server niet wordt vermeld onder de geregistreerde servers in de Azure portal**  
Als een server niet wordt vermeld onder **servers geregistreerd** voor een Service-opslag-synchronisatie:
1. Aanmelden bij de server die u wilt registreren.
2. Open File Explorer en Ga naar de installatiemap van opslag Sync-Agent (de standaardlocatie is C:\Program Files\Azure\StorageSyncAgent). 
3. ServerRegistration.exe uitgevoerd en voltooi de wizard voor het registreren van de server met een opslag-Sync-Service.



<a id="server-already-registered"></a>**Serverregistratie het volgende bericht weergegeven tijdens de installatie van Azure bestand Sync-agent: 'deze server is al geregistreerd'** 

![Een schermopname van het dialoogvenster serverregistratie met de fout 'server is al geregistreerd' bericht](media/storage-sync-files-troubleshoot/server-registration-1.png)

Dit bericht wordt weergegeven als de server eerder met een opslag-Sync-Service is geregistreerd. Hef de registratie van de server van de huidige Storage Sync-Service en registreert u met een nieuwe opslag Sync-Service, de stappen die worden beschreven in [Hef de registratie van een server met het synchroniseren van Azure bestand](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Als de server niet wordt vermeld onder **servers geregistreerd** in de opslag Sync-Service op de server die u wilt uitschrijven Voer de volgende PowerShell-opdrachten:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Als de server deel van een cluster uitmaakt, kunt u de optionele *Reset StorageSyncServer - CleanClusterRegistration* -parameter voor de registratie van het cluster ook ongedaan gemaakt.

<a id="web-site-not-trusted"></a>**Bij het registreren van een server, zie ik talrijke 'website niet wordt vertrouwd' reacties. Waarom is dat?**  
Dit probleem treedt op wanneer de **verbeterde beveiliging van Internet Explorer** beleid is ingeschakeld tijdens de serverregistratie. Voor meer informatie over het correct uitschakelen de **verbeterde beveiliging van Internet Explorer** beleid, Zie [Windows-Server voorbereiden voor gebruik met Azure File-synchronisatie](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) en [Azure File implementeren Synchronisatie (preview)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Synchronisatie-groepsbeheer
<a id="cloud-endpoint-using-share"></a>**Cloud-eindpunt maken mislukt vanwege de volgende fout: 'De opgegeven Azure-bestandsshare is al in gebruik door een andere CloudEndpoint'**  
Dit probleem treedt op als de Azure-bestandsshare al gebruikt door een andere cloudeindpunt wordt. 

Als u dit bericht ziet en de Azure-bestandsshare momenteel niet gebruikt door een cloudeindpunt is, voert u de volgende stappen uit om te wissen van de metagegevens van de Azure-bestand synchronisatie op de Azure-bestandsshare:

> [!Warning]  
> Verwijderen van de metagegevens voor een Azure-bestandsshare die momenteel wordt gebruikt door een cloudeindpunt zorgt ervoor dat de Azure-bestand synchronisatiebewerkingen mislukken. 

1. In de Azure portal, gaat u naar uw Azure-bestandsshare.  
2. Met de rechtermuisknop op de Azure-bestandsshare en selecteer vervolgens **metagegevens bewerken**.
3. Met de rechtermuisknop op **SyncService**, en selecteer vervolgens **verwijderen**.

<a id="cloud-endpoint-authfailed"></a>**Cloud-eindpunt maken mislukt vanwege de volgende fout: "AuthorizationFailed"**  
Dit probleem treedt op als uw gebruikersaccount niet voldoende rechten voor het maken van een cloudeindpunt heeft. 

Uw gebruikersaccount moet de volgende Microsoft Authorization-machtigingen hebben voor het maken van een cloudeindpunt:  
* Lees: Roldefinitie ophalen
* Write: Maken of bijwerken van aangepaste roldefinitie
* Lees: Roltoewijzing ophalen
* Write: Roltoewijzing maken

De volgende ingebouwde rollen hebben de vereiste machtigingen voor Microsoft Authorization:  
* Eigenaar
* Beheerder voor gebruikerstoegang om te bepalen of uw gebruikersrol van het account de vereiste machtigingen heeft:  
1. Selecteer in de Azure-portal **resourcegroepen**.
2. Selecteer de resourcegroep waar het opslagaccount zich bevindt, en selecteer vervolgens **toegangsbeheer (IAM)**.
3. Selecteer de **rol** (bijvoorbeeld eigenaar of bijdrager) voor uw gebruikersaccount.
4. In de **Resource Provider** selecteert **Microsoft Authorization**. 
    * **Roltoewijzing** moet **lezen** en **schrijven** machtigingen.
    * **Roldefinitie** moet **lezen** en **schrijven** machtigingen.

<a id="server-endpoint-createjobfailed"></a>**Het maken van de endpoint mislukt, vanwege de volgende fout: 'MgmtServerJobFailed' (foutcode:-2134375898)**                                                                                                                    
Dit probleem treedt op als het pad naar het eindpunt van de server zich op het systeemvolume en cloud tiering is ingeschakeld. Cloud tiering wordt niet ondersteund op het systeemvolume. Een om servereindpunt te maken op het systeemvolume, cloud tiering bij het maken van de server het eindpunt niet uitschakelen.

<a id="server-endpoint-deletejobexpired"></a>**Server-eindpunt verwijderen is mislukt, vanwege de volgende fout: "MgmtServerJobExpired"**                
Dit probleem treedt op als de server offline is of geen verbinding met het netwerk. Als de server niet langer beschikbaar is, hef de registratie van de server in de portal die de server-eindpunten wordt verwijderd. Volg de stappen die worden beschreven in voor het verwijderen van de server-eindpunten [Hef de registratie van een server met het synchroniseren van Azure bestand](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

<a id="server-endpoint-provisioningfailed"></a>**Kan server eindpunt eigenschappenpagina te openen of cloud opslaglagenbeleid bijwerken**

Dit probleem kan optreden als een bewerking voor het beheer op de server het eindpunt is mislukt. Als de eigenschappenpagina van server-eindpunt niet wordt geopend in de Azure portal, kan servereindpunt van de met behulp van PowerShell-opdrachten van de server bijwerken los het probleem. 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzureRmStorageSyncServerEndpoint -SubscriptionId mysubguid -ResourceGroupName myrgname -StorageSyncServiceName storagesvcname -SyncGroupName mysyncgroup

# Update the free space percent policy for the server endpoint
Set-AzureRmStorageSyncServerEndpoint -Id serverendpointid -CloudTiering true -VolumeFreeSpacePercent 60
```

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Als ik een bestand rechtstreeks in mijn Azure-bestandsshare via SMB of via de portal gemaakt, hoe lang duurt het voor het bestand om te synchroniseren op de servers in de groep voor synchronisatie?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Synchronisatie mislukt op een server**  
Als de synchronisatie is mislukt op een server:
1. Controleer of een servereindpunt bestaat in de Azure portal voor de map die u wilt synchroniseren met een Azure-bestandsshare:
    
    ![Een schermopname van een groep voor synchronisatie met een cloudeindpunt en een servereindpunt in de Azure portal](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

2. Controleer de operationele en diagnostische-gebeurtenislogboeken op, zich onder Applications and Services\Microsoft\FileSync\Agent in Logboeken.
    1. Controleer of de server heeft verbinding met internet.
    2. Controleren of de service Azure File-synchronisatie wordt uitgevoerd op de server. Om dit te doen, opent u de Services MMC-module en controleren of de opslag Sync-Agent-service (FileSyncSvc) wordt uitgevoerd.

<a id="replica-not-ready"></a>**Synchronisatie mislukt vanwege de volgende fout: '0x80c8300f - de replica is niet klaar voor het uitvoeren van de vereiste bewerking'**  
Dit probleem wordt verwacht als u een cloudeindpunt maken en gebruiken van een Azure-bestandsshare die gegevens bevat. Wanneer de wijziging detectie-taak is voltooid op de Azure-bestandsshare (het kan tot 24 uur duren) uitgevoerd, moet synchronisatie starten correct werkt.


    > [!NOTE]
    > Azure File Sync periodically takes VSS snapshots to sync files that have open handles.

We momenteel geen ondersteuning voor resource verplaatsen naar een ander abonnement of, verplaatsen naar een andere Azure AD-tenant.  Als het abonnement wordt verplaatst naar een andere tenant, wordt de Azure-bestandsshare niet toegankelijk voor de service op basis van de wijziging in eigendom. Als de tenant is gewijzigd, moet u de server-eindpunten en het cloudeindpunt te verwijderen (Zie Sync groepsbeheer sectie voor meer informatie het reinigen van de Azure-bestandsshare moet opnieuw worden gebruikt) en opnieuw maken van de groep voor synchronisatie.

<a id="doesnt-have-enough-free-space"></a>**Deze computer beschikt niet over voldoende vrije ruimte-fout**  
Als de portal ziet u de status 'Deze computer beschikt niet over voldoende vrije ruimte' kan het probleem kan zijn dat kleiner dan 1 GB vrije ruimte op het volume blijft.  Bijvoorbeeld, als er een volume 1,5 GB, synchronisatie alleen kan worden .5GB gebruiken als u dit probleem op Neem Vouw de grootte van het volume dat wordt gebruikt voor het servereindpunt.

## <a name="cloud-tiering"></a>Cloudopslaglagen 
Er zijn twee paden op fouten in de cloud tiering:

- Bestanden kunnen niet worden servicetier, wat betekent dat dat Azure bestand Sync zonder succes probeert te laag van een bestand naar de Azure-bestanden.
- Bestanden kunnen niet worden ingetrokken, wat dat de Azure-bestand Sync bestandssysteemfilter betekent mislukt (StorageSync.sys) om gegevens wanneer een gebruiker attemptes voor toegang tot een bestand dat is gelaagde te downloaden.

Er zijn twee belangrijke klassen van fouten die kunnen ontstaan via beide pad is mislukt:

- Fouten van cloud-opslag
    - *Problemen met de servicebeschikbaarheid tijdelijke opslag*. Zie [Service Level Agreement (SLA) voor Azure Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) voor meer informatie.
    - *Azure-bestandsshare niet toegankelijk*. Deze fout gebeurt meestal wanneer u verwijdert de Azure-bestandsshare als het nog steeds een cloudeindpunt in een groep voor synchronisatie.
    - *Niet-toegankelijke opslagaccount*. Deze fout gebeurt meestal wanneer u het opslagaccount verwijderen terwijl er nog een Azure-bestandsshare die een cloudeindpunt in een groep voor synchronisatie. 
- Serverfouten 
    - *Azure bestand Sync bestandssysteemfilter (StorageSync.sys) is niet geladen*. Om te reageren op aanvragen lagen/intrekken, moet het bestandssysteemfilter van Azure bestand synchronisatie worden geladen. Het filter niet worden geladen om verschillende redenen kan gebeuren, maar de meest voorkomende reden is dat een beheerder verwijderd uit het geheugen deze handmatig. Het bestandssysteemfilter van Azure bestand synchronisatie moet worden geladen op altijd voor Azure bestand synchronisatie goed functie.
    - *Ontbreekt, is beschadigd of anderszins verbroken reparsepunt*. Een reparsepunt is een speciale gegevensstructuur van een bestand dat uit twee delen bestaat:
        1. Een reparsepunt code, waarmee wordt aangegeven dat het besturingssysteem dat het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys) moet mogelijk een aantal acties uitvoeren op IO naar het bestand. 
        2. Reparse-gegevens, waarmee wordt aangegeven dat het bestandssysteemfilter van de URI van het bestand op de gekoppelde cloud-eindpunt (de Azure-bestandsshare). 
        
        De meeste gevallen die een reparsepunt beschadigd kan raken is als een beheerder probeert om de code of gegevens ervan te wijzigen. 
    - *Verbindingsproblemen netwerk*. Om de servicelaag of intrekken van een bestand, moet de server verbinding met internet hebben.

De volgende secties geven aan hoe u problemen met cloud lagen en bepalen of een probleem een probleem met cloud-opslag of een probleem is.

<a id="files-fail-tiering"></a>**Problemen met bestanden die niet voldoen aan de servicelaag oplossen**  
Als bestanden niet te laag tot Azure-bestanden:

1. Controleer of de bestanden bestaan in de Azure-bestandsshare.

    > [!NOTE]
    > Een bestand moet worden gesynchroniseerd met een Azure-bestandsshare voordat tiers worden verdeeld.
2. Controleer de operationele en diagnostische-gebeurtenislogboeken op, zich onder Applications and Services\Microsoft\FileSync\Agent in Logboeken.
    1. Controleer of de server heeft verbinding met internet. 
    2. Controleer of de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
        - Bij een opdrachtprompt met verhoogde bevoegdheden uitvoeren `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

<a id="files-fail-recall"></a>**Problemen met bestanden die niet voldoen aan herinnerd oplossen**  
Als bestanden niet worden ingetrokken:
1. Controleer de operationele en diagnostische-gebeurtenislogboeken op, zich onder Applications and Services\Microsoft\FileSync\Agent in Logboeken.
    1. Controleer of de bestanden bestaan in de Azure-bestandsshare.
    2. Controleer of de server heeft verbinding met internet. 
    3. Controleer of de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
        - Bij een opdrachtprompt met verhoogde bevoegdheden uitvoeren `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

<a id="files-unexpectedly-recalled"></a>**Problemen met bestanden onverwacht ingetrokken op een server oplossen**  
Antivirussoftware, back-up en andere toepassingen die grote aantallen bestanden lezen veroorzaken onbedoelde teruggehaald tenzij ze het offline kenmerk overslaan respecteren en doorgaan met het lezen van de inhoud van deze bestanden. Overslaan van offlinebestanden voor producten die ondersteuning bieden voor deze optie helpt onbedoeld teruggehaald voorkomen tijdens bewerkingen, zoals antivirussoftware scans of back-uptaken.

Neem contact op met de softwareleverancier voor meer informatie over hoe de oplossing configureren voor het lezen van offlinebestanden overslaan.

Onbedoelde teruggehaald kunnen ook optreden in andere scenario's, zoals wanneer u bestanden in Verkenner bladert. Openen van een map met bestanden cloud lagen in Verkenner op de server kan resulteren in onbedoelde teruggehaald. Dit is zelfs waarschijnlijker als virussen te bestrijden is ingeschakeld op de server.

## <a name="general-troubleshooting"></a>Algemene probleemoplossing
Als u problemen met het synchroniseren van Azure-bestand op een server, starten door de volgende stappen:
1. Controleer de operationele en diagnostische logboeken in Logboeken.
    - Synchronisatie, tiering en terughalen problemen worden vastgelegd in de gebeurtenislogboeken diagnostische en operationele onder Applications and Services\Microsoft\FileSync\Agent.
    - Problemen met betrekking tot het beheren van een server (bijvoorbeeld, configuratie-instellingen) worden geregistreerd in de operationele en diagnostische logboeken onder Applications and Services\Microsoft\FileSync\Management.
2. Controleren of de service Azure File-synchronisatie wordt uitgevoerd op de server:
    - Open de MMC-module Services en controleren of de opslag Sync-Agent-service (FileSyncSvc) wordt uitgevoerd.
3. Controleer of de Azure-bestand Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
    - Bij een opdrachtprompt met verhoogde bevoegdheden uitvoeren `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestand stuurprogramma's voor bestandssysteemfilters worden vermeld.

Als het probleem niet is opgelost, moet u het hulpprogramma AFSDiag uitvoeren:
1. Maak een map waarin de uitvoer AFSDiag (bijvoorbeeld C:\Output) moet worden opgeslagen.
2. Open een PowerShell-venster met verhoogde bevoegdheden en voer vervolgens de volgende opdrachten (druk op Enter na elke opdracht):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Voer voor het traceerniveau Azure bestand Sync kernel mode **1** (tenzij anders vermeld, uitgebreidere traceringen maken), en druk op Enter.
4. Voer voor het traceerniveau Azure bestand Sync gebruiker-modus **1** (tenzij anders vermeld, uitgebreidere traceringen maken), en druk op Enter.
5. Reproduceer het probleem. Wanneer u klaar bent, voert u **D**.
6. Een ZIP-bestand met Logboeken en traceringsbestanden wordt opgeslagen naar de uitvoermap die u hebt opgegeven.

## <a name="see-also"></a>Zie ook
- [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md)
- [Oplossen van problemen in Windows Azure-bestanden](storage-troubleshoot-windows-file-connection-problems.md)
- [Problemen met Azure-bestanden in Linux](storage-troubleshoot-linux-file-connection-problems.md)
