---
title: Azure Files Sync oplossen | Microsoft Docs
description: Oplossen van veelvoorkomende problemen met Azure File Sync.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/25/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 376ebcbc17cc9f5c797c2985fe3c0784f5036600
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752089"
---
# <a name="troubleshoot-azure-file-sync"></a>Problemen met Azure Files Sync oplossen
Gebruik Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, NFS en FTPS gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

In dit artikel is ontworpen om u te helpen u problemen op te lossen die met uw Azure File Sync-implementatie optreden mogelijk. Er wordt ook beschreven hoe u belangrijk om Logboeken te verzamelen uit het systeem als een nadere analyse van het probleem vereist is. Als u het antwoord op uw vraag niet ziet, u kunt contact met ons opnemen via de volgende kanalen (in steeds sneller groeiende volgorde):

1. [Azure Storage Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
2. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
3. Microsoft Ondersteuning. Maken van een nieuwe ondersteuningsaanvraag in de Azure-portal op de **Help** tabblad de **Help en ondersteuning** knop en selecteer vervolgens **nieuwe ondersteuningsaanvraag**.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="im-having-an-issue-with-azure-file-sync-on-my-server-sync-cloud-tiering-etc-should-i-remove-and-recreate-my-server-endpoint"></a>Ik ondervind een probleem met Azure File Sync op mijn server (sync, cloud cloudlagen, enz.). Moet ik verwijderen en opnieuw maken van mijn servereindpunt?
[!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## <a name="agent-installation-and-server-registration"></a>De registratie van de installatie en server van de agent
<a id="agent-installation-failures"></a>**Fouten bij de installatie van de agent oplossen**  
Voer de volgende opdracht uit om in te schakelen logboekregistratie tijdens de installatie van de agent als de installatie van de Azure File Sync-agent is mislukt, bij een opdrachtprompt met verhoogde bevoegdheid:

```
StorageSyncAgent.msi /l*v AFSInstaller.log
```

Bekijk installer.log om te bepalen van de oorzaak van de installatie is mislukt.

<a id="agent-installation-on-DC"></a>**De installatie van agent mislukt op Active Directory-domeincontroller**  
Als u probeert de sync-agent installeren op een Active Directory-domeincontroller waar de eigenaar van de PDC-functie in Windows Server 2008 R2 of onder de versie van het besturingssysteem is, kan u bereikt voor het probleem waarbij de sync-agent niet worden geïnstalleerd.

Om op te lossen, kunt u de rol van PDC overdragen naar een andere domeincontroller met Windows Server 2012 R2 of een meer recente, installeer de synchronisatie.

<a id="server-registration-missing"></a>**Server wordt niet vermeld onder geregistreerde servers in Azure portal**  
Als een server niet wordt vermeld onder **servers geregistreerd** voor een opslagservice voor synchronisatie:
1. Aanmelden bij de server die u wilt registreren.
2. Open File Explorer en ga vervolgens naar de installatiemap van de opslag-Sync-Agent (de standaardlocatie is C:\Program Files\Azure\StorageSyncAgent). 
3. ServerRegistration.exe worden uitgevoerd en voltooi de wizard voor het registreren van de server met een Opslagsynchronisatieservice.

<a id="server-already-registered"></a>**Registratie van de server weergegeven het volgende bericht tijdens de installatie van Azure File Sync-agent: "Deze server is al geregistreerd"** 

![Een schermafbeelding van de serverregistratie-dialoogvenster met de fout "server is al geregistreerd" bericht](media/storage-sync-files-troubleshoot/server-registration-1.png)

Dit bericht wordt weergegeven als de server eerder is geregistreerd bij een Opslagsynchronisatieservice. Als u wilt de server van de huidige Opslagsynchronisatieservice de registratie ongedaan maken en vervolgens met een nieuwe Opslagsynchronisatieservice te registreren, voltooi de stappen die worden beschreven in [registratie ongedaan maken van een server met Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Als de server niet wordt vermeld onder **servers geregistreerd** in de Opslagsynchronisatieservice op de server die u de registratie ongedaan maken wilt, voer de volgende PowerShell-opdrachten:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Als de server deel van een cluster uitmaakt, kunt u de optionele *Reset-StorageSyncServer - CleanClusterRegistration* parameter ook verwijderen van de registratie van de cluster.

<a id="web-site-not-trusted"></a>**Als ik een server registreert, zie ik talrijke 'website niet vertrouwd' antwoorden. Waarom is dat?**  
Dit probleem treedt op wanneer de **verbeterde beveiliging van Internet Explorer** beleid is ingeschakeld tijdens de serverregistratie. Voor meer informatie over het correct uitschakelen de **verbeterde beveiliging van Internet Explorer** beleid, Zie [Windows-Server voorbereiden voor gebruik met Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) en [over het implementeren van Azure File Synchronisatie](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Synchronisatie-groepsbeheer
<a id="cloud-endpoint-using-share"></a>**Cloud-eindpunt maken mislukt vanwege de volgende fout: "De opgegeven Azure-bestandsshare is al in gebruik door een andere CloudEndpoint"**  
Dit probleem treedt op als de Azure-bestandsshare al in gebruik door een andere cloudeindpunt. 

Als u dit bericht ziet en de Azure-bestandsshare momenteel niet gebruikt door een cloudeindpunt is, voert u de volgende stappen uit om te wissen van de metagegevens van de Azure File Sync op de Azure-bestandsshare:

> [!Warning]  
> Verwijderen van de metagegevens van een Azure-bestandsshare die momenteel in gebruik door een cloudeindpunt zorgt ervoor dat Azure File Sync-bewerkingen mislukken. 

1. In de Azure-portal, gaat u naar uw Azure-bestandsshare.  
2. Met de rechtermuisknop op de Azure-bestandsshare en selecteer vervolgens **metagegevens bewerken**.
3. Met de rechtermuisknop op **SyncService**, en selecteer vervolgens **verwijderen**.

<a id="cloud-endpoint-authfailed"></a>**Cloud-eindpunt maken mislukt vanwege de volgende fout: "AuthorizationFailed"**  
Dit probleem treedt op als uw gebruikersaccount niet beschikt over voldoende rechten voor het maken van een cloudeindpunt. 

Voor het maken van een cloudeindpunt, moet uw gebruikersaccount de volgende Microsoft Authorization-machtigingen hebben:  
* Lezen: Roldefinitie ophalen
* Schrijven: Een aangepaste roldefinitie maken of bijwerken
* Lezen: Roltoewijzing ophalen
* Schrijven: Roltoewijzing maken

De volgende ingebouwde rollen hebben de vereiste machtigingen voor Microsoft Authorization:  
* Eigenaar
* Beheerder van gebruikerstoegang

Om te bepalen of uw gebruikersrol van het account de vereiste machtigingen heeft:  
1. Selecteer in de Azure portal, **resourcegroepen**.
2. Selecteer de resourcegroep waar het opslagaccount zich bevindt, en selecteer vervolgens **toegangsbeheer (IAM)**.
3. Selecteer de **roltoewijzingen** tabblad.
4. Selecteer de **rol** (bijvoorbeeld, de eigenaar of Inzender) voor uw gebruikersaccount.
5. In de **Resourceprovider** in de lijst met **Microsoft Authorization**. 
    * **Roltoewijzing** moet **lezen** en **schrijven** machtigingen.
    * **Roldefinitie** moet **lezen** en **schrijven** machtigingen.

<a id="server-endpoint-createjobfailed"></a>**Het servereindpunt mislukt vanwege de volgende fout: 'MgmtServerJobFailed' (foutcode:-2134375898)**  
Dit probleem treedt op als het pad van server-eindpunt op het systeemvolume en cloud tiering is ingeschakeld. Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.

<a id="server-endpoint-deletejobexpired"></a>**Het servereindpunt mislukt vanwege de volgende fout: "MgmtServerJobExpired"**                
Dit probleem treedt op als de server offline is of geen verbinding met het netwerk. Als de server niet meer beschikbaar is, registratie van de server in de portal die de servereindpunten wordt verwijderd. Als u wilt verwijderen van de servereindpunten, volgt u de stappen die worden beschreven in [registratie ongedaan maken van een server met Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

<a id="server-endpoint-provisioningfailed"></a>**Kan niet open server eindpunt-eigenschappenpagina of bijwerken van de beleidsinstelling voor lagen cloud**  
Dit probleem kan optreden als een bewerking voor het beheer van het servereindpunt is mislukt. Als de eigenschappenpagina van server-eindpunt niet wordt geopend in de Azure-portal, servereindpunt met behulp van PowerShell-opdrachten van de server bijwerken dit probleem mogelijk opgelost. 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzureRmStorageSyncServerEndpoint `
    -SubscriptionId mysubguid `
    -ResourceGroupName myrgname `
    -StorageSyncServiceName storagesvcname `
    -SyncGroupName mysyncgroup

# Update the free space percent policy for the server endpoint
Set-AzureRmStorageSyncServerEndpoint `
    -Id serverendpointid `
    -CloudTiering true `
    -VolumeFreeSpacePercent 60
```
<a id="server-endpoint-noactivity"></a>**Servereindpunt heeft een status van 'Geen activiteit' of 'In behandeling' en de status van de server op de blade geregistreerde servers "Weergegeven als offline"**  

Dit probleem kan optreden als de Monitor voor het synchroniseren van opslag-proces wordt niet uitgevoerd of de server kan niet communiceren met de Azure File Sync-service vanwege een proxy of firewall.

Gebruik een of meer van de volgende stappen om dit probleem op te lossen:

1. Open Taakbeheer op de server en controleer of het Storage Sync Monitor-proces (AzureStorageSyncMonitor.exe) actief is. Als het proces niet wordt uitgevoerd, start u om te beginnen de server opnieuw op. Als de server opnieuw wordt opgestart, wordt het probleem niet verhelpen, een upgrade naar de meest recente Azure File Sync [agentversie](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes).
2. Controleer of de Firewall en Proxy-instellingen correct zijn geconfigureerd:
    - Als de server zich achter een firewall bevindt, controleert u of uitgaand verkeer via poort 443 is toegestaan. Als de firewall verkeer tot specifieke domeinen beperkt, controleert u of de domeinen die worden vermeld in de Firewall [documentatie](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#firewall) toegankelijk zijn.
    - Als de server zich achter een proxy, de brede, door het machine of app-specifieke proxy-instellingen configureren met de volgende stappen in de Proxy [documentatie](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#proxy).

<a id="endpoint-noactivity-sync"></a>**Servereindpunt heeft een status van "Geen activiteit" en de status van de server op de blade geregistreerde servers is 'Online'**  

De status van een server-eindpunt van 'Geen activiteit' betekent dat het servereindpunt is niet geregistreerd voor synchronisatie-activiteit in de afgelopen twee uur.

Een servereindpunt kan synchronisatieactiviteiten niet aanmelden voor de volgende redenen:

- De server heeft een actieve sessie VSS-synchronisatie (SnapshotSync). Wanneer een synchronisatiesessie VSS actief voor een servereindpunt is, kunnen een synchronisatiesessie start totdat de VSS-synchronisatiesessie is voltooid andere servereindpunten op hetzelfde volume niet starten.

    Huidige synchronisatie-activiteit op een server, Zie [hoe controleer ik de voortgang van een huidige synchronisatiesessie?](#how-do-i-monitor-the-progress-of-a-current-sync-session).

- De server heeft het maximum aantal gelijktijdige synchronisatiesessies bereikt. 
    - Agent-versie 4.x en hoger: Limiet is afhankelijk van beschikbare systeemresources.
    - Agentversie 3.x: 2 active synchronisatiesessies per processor- of maximaal 8 active synchronisatiesessies per server.

> [!Note]  
> Als de status van de server op de blade geregistreerde servers 'Offline weergegeven' is, voert u de stappen die zijn beschreven de [servereindpunt heeft een status van 'Geen activiteit' of 'In behandeling' en de status van de server op de blade geregistreerde servers "Weergegeven als offline" ](#server-endpoint-noactivity) sectie.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Als ik een bestand rechtstreeks in mijn Azure-bestandsshare via SMB of via de portal gemaakt, hoe lang duurt om het bestand te synchroniseren met servers in de groep voor synchronisatie?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="serverendpoint-pending"></a>**Serverstatus-eindpunt is in behandeling voor enkele uren**  
Dit probleem wordt verwacht als u een cloudeindpunt maken en gebruiken van een Azure-bestandsshare die gegevens bevat. De wijziging opsomming taak scant voor wijzigingen in de Azure-bestandsshare moet voltooien voordat de bestanden kunnen worden gesynchroniseerd tussen de eindpunten van de cloud en server. De tijd voor het voltooien van de taak is afhankelijk van de grootte van de naamruimte in de Azure-bestandsshare. De status van eindpunt moet worden bijgewerkt nadat de wijziging opsomming-taak is voltooid.

### <a id="broken-sync"></a>Hoe controleer ik health sync?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Binnen elke groep voor synchronisatie, kunt u inzoomen op de afzonderlijke servereindpunten voor de status van de laatste voltooide synchronisatiesessies. Een kolom met groene status en de bestanden niet synchroniseren waarde 0 geven aan dat synchronisatie werkt zoals verwacht. Als dit niet het geval is, ziet u hieronder voor een lijst met algemene synchronisatiefouten en verwerken van bestanden die niet worden gesynchroniseerd. 

![Een schermafbeelding van de Azure-portal](media/storage-sync-files-troubleshoot/portal-sync-health.png)

# <a name="servertabserver"></a>[Server](#tab/server)
Ga naar de logboekbestanden met telemetrie van de server, die kunnen worden gevonden in de Event Viewer op `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`. Gebeurtenis 9102 komt overeen met de synchronisatiesessie van een voltooide. Zoek de meest recente gebeurtenis met ID 9102 voor de meest recente status van synchronisatie. SyncDirection kunt u zien als deze sessie een uploaden of downloaden was. Als de HResult 0 is, zijn de synchronisatiesessie is voltooid. Een niet-nul-HResult betekent dat er een fout opgetreden tijdens de synchronisatie is; Zie hieronder voor een lijst met veelvoorkomende fouten. Als de PerItemErrorCount groter dan 0 is, betekent dit dat sommige bestanden of mappen is niet juist gesynchroniseerd. Het is mogelijk dat een HResult 0, maar een PerItemErrorCount die groter is dan 0.

Hieronder volgt een voorbeeld van een uploaden is voltooid. Om het te kort te houden, worden slechts enkele van de waarden in elke 9102 gebeurtenis hieronder vermeld. 

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: 0, 
SyncFileCount: 2, SyncDirectoryCount: 0,
AppliedFileCount: 2, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0,
TransferredFiles: 2, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Daarentegen een mislukte upload als volgt uitzien:

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: -2134364065,
SyncFileCount: 0, SyncDirectoryCount: 0, 
AppliedFileCount: 0, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0, 
TransferredFiles: 0, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Soms synchronisatiesessies algemene mislukken of een niet-nul-PerItemErrorCount hebt maar nog steeds aanbrengen voortgang, met enkele bestanden is gesynchroniseerd. Dit kunt u zien in de toegepast * velden (AppliedFileCount, AppliedDirCount AppliedTombstoneCount en AppliedSizeBytes), laat u weten welk deel van de sessie is voltooid. Als u meerdere synchronisatiesessies in een rij die mislukken, maar een toenemende toegepast * aantal ziet, moet u synchronisatietijd om te proberen het opnieuw voordat u een ondersteuningsticket openen geven.

---

### <a name="how-do-i-monitor-the-progress-of-a-current-sync-session"></a>Hoe controleer ik de voortgang van een huidige synchronisatiesessie?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
In de groep voor synchronisatie, gaat u naar het desbetreffende servereindpunt en kijken naar het gedeelte activiteit van de synchronisatie om te zien van het aantal bestanden geüpload of gedownload in de huidige synchronisatiesessie. Houd er rekening mee dat deze status wordt vertraagd circa 5 minuten, en als uw synchronisatiesessie klein genoeg is om te worden voltooid binnen deze periode is deze niet kan worden gerapporteerd in de portal. 

# <a name="servertabserver"></a>[Server](#tab/server)
Zoekt u naar de meest recente 9302 in de telemetrie in het gebeurtenislogboek op de server (Ga in de logboeken voor toepassingen en Services Logs\Microsoft\FileSync\Agent\Telemetry). Deze gebeurtenis geeft aan dat de status van de huidige synchronisatiesessie. TotalItemCount geeft aan hoeveel bestanden die moeten worden gesynchroniseerd, AppliedItemCount het aantal bestanden die tot nu toe zijn gesynchroniseerd en PerItemErrorCount het aantal bestanden die niet worden gesynchroniseerd (Zie hieronder voor het omgaan met dit).

```
Replica Sync Progress. 
ServerEndpointName: <CI>sename</CI>, SyncGroupName: <CI>sgname</CI>, ReplicaName: <CI>rname</CI>, 
SyncDirection: Upload, CorrelationId: {AB4BA07D-5B5C-461D-AAE6-4ED724762B65}. 
AppliedItemCount: 172473, TotalItemCount: 624196. AppliedBytes: 51473711577, 
TotalBytes: 293363829906. 
AreTotalCountsFinal: true. 
PerItemErrorCount: 1006.
```
---

### <a name="how-do-i-know-if-my-servers-are-in-sync-with-each-other"></a>Hoe weet ik of mijn servers synchroon met elkaar zijn?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Voor elke server in een bepaalde synchronisatiegroep, zorg ervoor dat:
- De tijdstempels voor de laatste synchronisatie heeft geprobeerd voor zowel uploaden en downloaden zijn recente.
- De status is groen voor zowel uploaden en downloaden.
- In het veld synchronisatieactiviteiten wordt zeer weinig of geen bestanden om te synchroniseren.
- Het veld niet synchroniseren van bestanden is 0 voor zowel uploaden en downloaden.

# <a name="servertabserver"></a>[Server](#tab/server)
Bekijk de voltooide synchronisatiesessies, die zijn gemarkeerd door 9102 gebeurtenissen in het gebeurtenislogboek van de telemetrie voor elke server (in de logboeken, gaat u naar `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`). 

1. Op elke server, die u wilt Zorg ervoor dat het meest recente uploaden en downloaden van sessies is voltooid. Om dit te doen, Controleer of de HResult en PerItemErrorCount 0 voor zowel uploaden en downloaden zijn (het veld SyncDirection geeft aan of een bepaalde sessie een sessie uploaden of downloaden). Houd er rekening mee dat als u een synchronisatiesessie laatst voltooide niet ziet, is het waarschijnlijk dat een synchronisatiesessie wordt momenteel uitgevoerd, die kan worden verwacht als u zojuist hebt toegevoegd of gewijzigd van een grote hoeveelheid gegevens.
2. Wanneer een server volledig up-to-date zijn met de cloud is en geen wijzigingen heeft in synchronisatie in beide richtingen, ziet u synchronisatiesessies leeg zijn. Deze worden aangeduid met het uploaden en downloaden van gebeurtenissen in waarin alle de synchronisatie * velden (SyncFileCount, SyncDirCount, SyncTombstoneCount en SyncSizeBytes) nul zijn, wat betekent dat er is niets om te synchroniseren. Houd er rekening mee dat deze leeg synchronisatiesessies niet op hoog verloop servers optreden kunnen als er altijd iets nieuws te synchroniseren is. Als er geen synchronisatie-activiteit, moeten ze elke 30 minuten plaatsvinden. 
3. Als alle servers bijgewerkt met de cloud zijn, wat betekent dat de recente uploaden en van downloadsessies leeg synchronisatiesessies zijn, is het kunnen zeggen met redelijke zekerheid dat het systeem als geheel gesynchroniseerd is. 
    
Houd er rekening mee dat als u rechtstreeks in uw Azure-bestandsshare wijzigingen, Azure File Sync niet detecteert deze wijziging pas wijzigen-inventarisatie wordt uitgevoerd, die elke 24 uur wordt uitgevoerd. Het is mogelijk dat een server is bijgewerkt met de cloud wanneer er in feite een recente wijzigingen die u rechtstreeks in de Azure-bestandsshare ontbreekt wordt bijvoorbeeld. 

---

### <a name="how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing"></a>Hoe kan ik zien of er zijn specifieke bestanden of mappen die niet worden gesynchroniseerd?
Als uw PerItemErrorCount op de server of -bestanden synchroniseren niet meegeteld in de portal zijn groter dan 0 voor een bepaalde synchronisatiesessie, waardoor dat sommige items niet kunnen synchroniseren. Bestanden en mappen kunnen hebben kenmerken die te voorkomen ze kunnen worden gesynchroniseerd dat. Deze kenmerken kunnen worden permanente en expliciete actie om te synchroniseren, bijvoorbeeld niet-ondersteunde tekens verwijderen uit het bestand of map naam hervatten is vereist. Ze kunnen ook worden tijdelijk is, wat betekent dat het bestand of map wordt automatisch hervat synchroniseren; bestanden met open ingangen wordt synchronisatie bijvoorbeeld automatisch hervat wanneer het bestand wordt gesloten. Wanneer u de Azure File Sync engine detecteert een probleem optreedt, een foutenlogboek wordt geproduceerd die kan worden geparseerd om de items die momenteel niet synchroniseert correct weer te geven.

Als u wilt zien deze fouten, voer de **FileSyncErrorsReport.ps1** PowerShell-script (te vinden in de map voor agentinstallatie van de Azure File Sync-agent) bestanden identificeren die kunnen niet synchroniseren vanwege open ingangen wordt niet ondersteund tekens of andere problemen. Het itempad-veld ziet u de locatie van het bestand ten opzichte van de hoofdmap van de synchronisatie. Zie de lijst met algemene synchronisatiefouten hieronder voor herstelstappen.

#### <a name="troubleshooting-per-filedirectory-sync-errors"></a>Per bestand/map synchronisatiefouten oplossen
**ItemResults logboek - synchronisatiefouten per item**  
| HRESULT | HRESULT (decimaal) | Fouttekenreeks | Probleem | Herstel |
|---------|-------------------|--------------|-------|-------------|
| 0x80c80207 | -2134375929 | ECS_E_SYNC_CONSTRAINT_CONFLICT | Wijziging van een bestand of directory kan niet nog worden gesynchroniseerd omdat een afhankelijke map nog niet is gesynchroniseerd. Dit item wordt gesynchroniseerd nadat de afhankelijke wijzigingen zijn gesynchroniseerd. | U hoeft geen actie te ondernemen. |
| 0x7B | 123 | ERROR_INVALID_NAME | De naam van het bestand of map is ongeldig. | Wijzig de naam van het bestand of map in kwestie. Zie [verwerking van niet-ondersteunde tekens](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) voor meer informatie. |
| 0x8007007b | -2147024773 | STIERR_INVALID_DEVICE_NAME | De naam van het bestand of map is ongeldig. | Wijzig de naam van het bestand of map in kwestie. Zie [verwerking van niet-ondersteunde tekens](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) voor meer informatie. |
| 0x80c80018 | -2134376424 | ECS_E_SYNC_FILE_IN_USE | Een bestand kan niet worden gesynchroniseerd omdat deze gebruikt wordt. Het bestand worden gesynchroniseerd wanneer deze niet meer gebruikt wordt. | U hoeft geen actie te ondernemen. Azure File Sync maakt een tijdelijke VSS-momentopname eenmaal per dag op de server om bestanden te synchroniseren met open ingangen. |
| 0x80c8031d | -2134375651 | ECS_E_CONCURRENCY_CHECK_FAILED | Een bestand is gewijzigd, maar de wijziging nog niet is gedetecteerd door synchronisatie. Synchronisatie wordt hersteld nadat deze wijziging wordt gedetecteerd. | U hoeft geen actie te ondernemen. |
| 0x80c8603e | -2134351810 | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED | Het bestand kan niet worden gesynchroniseerd omdat de limiet voor het delen van Azure bestand is bereikt. | U lost dit probleem, Zie [u de Azure file share storage limiet bereikt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#-2134351810) sectie in de gids voor probleemoplossing. |
| 0x80070005 | -2147024891 | E_ACCESSDENIED | Deze fout kan optreden als het bestand is versleuteld met een niet-ondersteunde oplossing (zoals NTFS EFS) of het bestand een status in behandeling heeft. | Als het bestand is versleuteld met een niet-ondersteunde oplossing, ontsleutelen van het bestand en een ondersteunde encryption-oplossing te gebruiken. Zie voor een lijst van ondersteuningsoplossingen, [versleutelingsoplossingen](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#encryption-solutions) sectie in de handleiding voor capaciteitsplanning. Als het bestand in een verwijdering status in behandeling is, wordt het bestand verwijderd zodra alle geopende bestandsingangen zijn gesloten. |
| 0x20 | 32 | ERROR_SHARING_VIOLATION | Een bestand kan niet worden gesynchroniseerd omdat deze gebruikt wordt. Het bestand worden gesynchroniseerd wanneer deze niet meer gebruikt wordt. | U hoeft geen actie te ondernemen. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Een bestand is gewijzigd tijdens de synchronisatie; dit bestand moet daarom opnieuw worden gesynchroniseerd. | U hoeft geen actie te ondernemen. |

#### <a name="handling-unsupported-characters"></a>Verwerking van niet-ondersteunde tekens
Als de **FileSyncErrorsReport.ps1** PowerShell-script bevat fouten vanwege niet-ondersteunde tekens (0x7b-foutcodes en 0x8007007b), moet u Verwijder of wijzig de naam van de tekens op fouten van de respectieve bestandsnamen. PowerShell wordt deze tekens als vraagtekens of lege rechthoeken waarschijnlijk afdrukken, omdat de meeste van deze tekens geen standaard visuele codering hebben. De [evaluatieprogramma](storage-sync-files-planning.md#evaluation-tool) kan worden gebruikt voor het identificeren van de tekens die niet worden ondersteund.

De onderstaande tabel bevat alle van de Azure File Sync biedt nog geen ondersteuning voor unicode-tekens.

| Tekenset | Aantal tekens |
|---------------|-----------------|
| <ul><li>0x0000009D (opdracht osc besturingssysteem)</li><li>0x00000090 (DC's apparaatreeks-besturingselement)</li><li>0x0000008F (shift drie één ss3)</li><li>0x00000081 (hoog octet standaardoptie)</li><li>0x0000007F (del verwijderen)</li><li>0x0000008D (ri Omgekeerde regelinvoer)</li></ul> | 6 |
| 0x0000FDD0 - 0x0000FDEF (Arabisch presentatie formulieren-a) | 32 |
| 0x0000FFF0 - 0x0000FFFF (aanbiedingen) | 16 |
| <ul><li>0x0001FFFE - 0x0001FFFF = 2 (noncharacter)</li><li>0x0002FFFE - 0x0002FFFF = 2 (noncharacter)</li><li>0x0003FFFE - 0x0003FFFF = 2 (noncharacter)</li><li>0x0004FFFE - 0x0004FFFF = 2 (noncharacter)</li><li>0x0005FFFE - 0x0005FFFF = 2 (noncharacter)</li><li>0x0006FFFE - 0x0006FFFF = 2 (noncharacter)</li><li>0x0007FFFE - 0x0007FFFF = 2 (noncharacter)</li><li>0x0008FFFE - 0x0008FFFF = 2 (noncharacter)</li><li>0x0009FFFE - 0x0009FFFF = 2 (noncharacter)</li><li>0x000AFFFE - 0x000AFFFF = 2 (noncharacter)</li><li>0x000BFFFE - 0x000BFFFF = 2 (noncharacter)</li><li>0x000CFFFE - 0x000CFFFF = 2 (noncharacter)</li><li>0x000DFFFE - 0x000DFFFF = 2 (noncharacter)</li><li>0x000EFFFE - 0x000EFFFF = 2 (ongedefinieerd)</li><li>0x000FFFFE - 0x000FFFFF = 2 (aanvullende persoonlijk gebruiksgebied)</li></ul> | 30 |
| 0x0010FFFE, 0x0010FFFF | 2 |

### <a name="common-sync-errors"></a>Algemene synchronisatiefouten
<a id="-2147023673"></a>**De synchronisatiesessie is geannuleerd.**  
| | |
|-|-|
| **HRESULT** | 0x800704c7 |
| **HRESULT (decimaal)** | -2147023673 | 
| **Fouttekenreeks** | ERROR_CANCELLED |
| **Herstel is vereist** | Nee |

Synchronisatiesessies mislukken om diverse redenen, met inbegrip van de server wordt opnieuw opgestart of bijgewerkt, VSS-momentopnamen, enzovoort. Hoewel deze fout opvolging vereist is lijkt, is het veilig om deze fout negeren, tenzij deze gedurende een periode van enkele uren actief blijft.

<a id="-2147012889"></a>**Een verbinding met de service kan niet worden gemaakt.**    
| | |
|-|-|
| **HRESULT** | 0x80072EE7 |
| **HRESULT (decimaal)** | -2147012889 | 
| **Fouttekenreeks** | WININET_E_NAME_NOT_RESOLVED |
| **Herstel is vereist** | Ja |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134376372"></a>**Aanvraag van de gebruiker wordt beperkt door de service.**  
| | |
|-|-|
| **HRESULT** | 0x80c8004c |
| **HRESULT (decimaal)** | -2134376372 |
| **Fouttekenreeks** | ECS_E_USER_REQUEST_THROTTLED |
| **Herstel is vereist** | Nee |

Er is geen actie vereist. de server probeert het opnieuw. Als deze fout zich blijft langer dan een paar uur voordoen, moet u een ondersteuningsaanvraag maken.

<a id="-2134364065"></a>**Synchronisatie geen toegang tot de Azure-bestandsshare in de cloudeindpunt opgegeven.**  
| | |
|-|-|
| **HRESULT** | 0x80c8305f |
| **HRESULT (decimaal)** | -2134364065 |
| **Fouttekenreeks** | ECS_E_CANNOT_ACCESS_EXTERNAL_STORAGE_ACCOUNT |
| **Herstel is vereist** | Ja |

Deze fout treedt op omdat de Azure File Sync-agent heeft geen toegang de Azure-bestandsshare, dit komt mogelijk tot doordat de Azure-bestandsshare of de storage-account waarop deze niet meer bestaat. U kunt deze fout oplossen door het uitvoeren van de volgende stappen uit:

1. [Controleer of dat het opslagaccount bestaat.](#troubleshoot-storage-account)
2. [Controleer of dat het opslagaccount bevat geen netwerkregels.](#troubleshoot-network-rules)
3. [Zorg ervoor dat de Azure-bestandsshare bestaat.](#troubleshoot-azure-file-share)
4. [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount.](#troubleshoot-rbac)

<a id="-2134364064"></a><a id="cannot-resolve-storage"></a>**Naam van het opslagaccount die wordt gebruikt, kan niet worden omgezet.**  
| | |
|-|-|
| **HRESULT** | 0x80C83060 |
| **HRESULT (decimaal)** | -2134364064 |
| **Fouttekenreeks** | ECS_E_STORAGE_ACCOUNT_NAME_UNRESOLVED |
| **Herstel is vereist** | Ja |

1. Controleer dat u de DNS-naam van de opslag van de server kan omzetten.

    ```PowerShell
    Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 443
    ```
2. [Controleer of dat het opslagaccount bestaat.](#troubleshoot-storage-account)
3. [Controleer of dat het opslagaccount bevat geen netwerkregels.](#troubleshoot-network-rules)

<a id="-1906441138"></a>**Synchronisatie mislukt vanwege een probleem met de synchronisatiedatabase.**  
| | |
|-|-|
| **HRESULT** | 0x8e5e044e |
| **HRESULT (decimaal)** | -1906441138 |
| **Fouttekenreeks** | JET_errWriteConflict |
| **Herstel is vereist** | Ja |

Deze fout treedt op wanneer er een probleem met de interne database die wordt gebruikt door Azure File Sync. Als dit probleem optreedt, maak een ondersteuningsaanvraag en wij nemen contact met u zodat u kunt dit probleem oplossen.

<a id="-2134364053"></a>**De versie van de Azure File Sync-agent geïnstalleerd op de server wordt niet ondersteund.**  
| | |
|-|-|
| **HRESULT** | 0x80C8306B |
| **HRESULT (decimaal)** | -2134364053 |
| **Fouttekenreeks** | ECS_E_AGENT_VERSION_BLOCKED |
| **Herstel is vereist** | Ja |

Deze fout treedt op als de versie van de Azure File Sync-agent geïnstalleerd op de server wordt niet ondersteund. Dit probleem op te lossen [upgrade]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#upgrade-paths) naar een [ondersteunde agentversie]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#supported-versions).

<a id="-2134351810"></a>**U bereikt de Azure file share-opslaglimiet.**  
| | |
|-|-|
| **HRESULT** | 0x80c8603e |
| **HRESULT (decimaal)** | -2134351810 |
| **Fouttekenreeks** | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED |
| **Herstel is vereist** | Ja |

Deze fout treedt op wanneer de limiet voor opslag van Azure file share is bereikt, dit kan gebeuren als een quotum voor een Azure-bestandsshare wordt toegepast, of als het-gebruik hoger is dan de limieten voor een Azure-bestandsshare. Zie voor meer informatie de [huidige limieten voor een Azure-bestandsshare](storage-files-scale-targets.md).

1. Navigeer naar de groep voor synchronisatie binnen de Opslagsynchronisatieservice.
2. Selecteer het cloudeindpunt in de groep voor synchronisatie.
3. Noteer de naam van de Azure file share in het geopende deelvenster.
4. Selecteer het gekoppelde opslagaccount. Als deze koppeling is mislukt, is het opslagaccount waarnaar wordt verwezen, verwijderd.

    ![Een schermafbeelding van het detailpaneel van cloud-eindpunt met een koppeling naar het opslagaccount.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

5. Selecteer **bestanden** om de lijst met bestandsshares weer te geven.
6. Klik op de drie puntjes aan het einde van de rij voor de Azure-bestandsshare waarnaar wordt verwezen door de cloudeindpunt.
7. Controleer de **gebruik** lager is dan de **quotum**. Houd er rekening mee, tenzij een alternatieve quotum is opgegeven, kan het quotum, komt overeen met de [maximale grootte van de Azure-bestandsshare](storage-files-scale-targets.md).

    ![Een schermafbeelding van de Azure eigenschappen voor bestandsdeling.](media/storage-sync-files-troubleshoot/file-share-limit-reached-1.png)

Als de share vol is en een quotum is niet ingesteld, is een mogelijke manier om dit probleem is opgelost zodat elke submap van de huidige servereindpunt in een eigen servereindpunt in hun eigen afzonderlijke synchronisatiegroepen. Op deze manier, elke submap worden gesynchroniseerd naar afzonderlijke Azure-bestandsshares.

<a id="-2134351824"></a>**De Azure-bestandsshare kan niet worden gevonden.**  
| | |
|-|-|
| **HRESULT** | 0x80c86030 |
| **HRESULT (decimaal)** | -2134351824 |
| **Fouttekenreeks** | ECS_E_AZURE_FILE_SHARE_NOT_FOUND |
| **Herstel is vereist** | Ja |

Deze fout treedt op wanneer de Azure-bestandsshare niet toegankelijk is. Om op te lossen:

1. [Controleer of dat het opslagaccount bestaat.](#troubleshoot-storage-account)
2. [Zorg ervoor dat de Azure-bestandsshare bestaat.](#troubleshoot-azure-file-share)

Als de Azure-bestandsshare is verwijderd, moet u een nieuwe bestandsshare maken en vervolgens opnieuw maken van de groep voor synchronisatie. 

<a id="-2134364042"></a>**Synchronisatie is onderbroken terwijl dit Azure-abonnement is onderbroken.**  
| | |
|-|-|
| **HRESULT** | 0x80C83076 |
| **HRESULT (decimaal)** | -2134364042 |
| **Fouttekenreeks** | ECS_E_SYNC_BLOCKED_ON_SUSPENDED_SUBSCRIPTION |
| **Herstel is vereist** | Ja |

Deze fout treedt op wanneer het Azure-abonnement is onderbroken. Synchronisatie wordt worden ingeschakeld wanneer het Azure-abonnement wordt hersteld. Zie [waarom is mijn Azure-abonnement uitgeschakeld en hoe ik het opnieuw activeren?](../../billing/billing-subscription-become-disable.md) voor meer informatie.

<a id="-2134364052"></a>**Het opslagaccount heeft een firewall of virtuele netwerken die zijn geconfigureerd.**  
| | |
|-|-|
| **HRESULT** | 0x80c8306c |
| **HRESULT (decimaal)** | -2134364052 |
| **Fouttekenreeks** | ECS_E_MGMT_STORAGEACLSNOTSUPPORTED |
| **Herstel is vereist** | Ja |

Deze fout treedt op wanneer de Azure-bestandsshare ontoegankelijk vanwege een storage-account firewall is of omdat het opslagaccount bij een virtueel netwerk hoort. Azure File Sync heeft nog geen ondersteuning voor deze functie. Om op te lossen:

1. [Controleer of dat het opslagaccount bestaat.](#troubleshoot-storage-account)
2. [Controleer of dat het opslagaccount bevat geen netwerkregels.](#troubleshoot-network-rules)

Deze regels om op te lossen dit probleem te verwijderen. 

<a id="-2134375911"></a>**Synchronisatie mislukt vanwege een probleem met de synchronisatiedatabase.**  
| | |
|-|-|
| **HRESULT** | 0x80c80219 |
| **HRESULT (decimaal)** | -2134375911 |
| **Fouttekenreeks** | ECS_E_SYNC_METADATA_WRITE_LOCK_TIMEOUT |
| **Herstel is vereist** | Nee |

Deze fout meestal zelf wordt omgezet, en kan zich voordoen als er zijn:

* Een groot aantal bestandswijzigingen op de servers in de groep voor synchronisatie.
* Een groot aantal fouten op afzonderlijke bestanden en mappen.

Als deze fout zich blijft langer dan een paar uur voordoen, maak een ondersteuningsaanvraag en wij nemen contact met u zodat u kunt dit probleem oplossen.

<a id="-2146762487"></a>**De server kan niet tot stand brengen van een beveiligde verbinding. De cloudservice heeft een onverwachte certificaat ontvangen.**  
| | |
|-|-|
| **HRESULT** | 0x800b0109 |
| **HRESULT (decimaal)** | -2146762487 |
| **Fouttekenreeks** | CERT_E_UNTRUSTEDROOT |
| **Herstel is vereist** | Ja |

Deze fout kan optreden als uw organisatie van een afsluitende SSL-proxy gebruikmaakt of als kwaadwillenden het verkeer tussen uw server en de Azure File Sync-service wordt onderschept. Als u er zeker van bent dat dit is normaal (omdat uw organisatie van een proxy wordt beëindigd SSL gebruikmaakt), kunt u overslaan certificaatcontrole met een onderdrukking register.

1. Maak de registerwaarde SkipVerifyingPinnedRootCertificate.

    ```PowerShell
    New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Azure\StorageSync -Name SkipVerifyingPinnedRootCertificate -PropertyType DWORD -Value 1
    ```

2. Start de synchronisatieservice op de geregistreerde server.

    ```PowerShell
    Restart-Service -Name FileSyncSvc -Force
    ```

Als u deze registerwaarde instelt, accepteert de Azure File Sync-agent elk lokaal vertrouwd SSL-certificaat tijdens de gegevensoverdracht tussen de server en de cloudservice.

<a id="-2147012894"></a>**Een verbinding met de service kan niet worden gemaakt.**  
| | |
|-|-|
| **HRESULT** | 0x80072EE2 |
| **HRESULT (decimaal)** | -2147012894 |
| **Fouttekenreeks** | WININET_E_TIMEOUT |
| **Herstel is vereist** | Ja |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134375680"></a>**Synchronisatie mislukt vanwege een probleem met verificatie.**  
| | |
|-|-|
| **HRESULT** | 0x80c80300 |
| **HRESULT (decimaal)** | -2134375680 |
| **Fouttekenreeks** | ECS_E_SERVER_CREDENTIAL_NEEDED |
| **Herstel is vereist** | Ja |

Deze fout kan worden veroorzaakt door:

- Servertijd is onjuist
- Het servereindpunt is verwijderd
- Gebruikt voor verificatie het certificaat is verlopen. 
    Als u wilt controleren of het certificaat is verlopen, moet u de volgende stappen uitvoeren:  
    1. Open de MMC-module Certificaten, selecteer computeraccount en navigeer naar certificaten (lokale Computer) \Personal\Certificates.
    2. Controleer of het certificaat voor clientverificatie is verlopen.

Als de servertijd juist is, voer de volgende stappen uit om het probleem te verhelpen:

1. Controleer of de Azure File Sync-agent versie 4.0.1.0 of hoger is geïnstalleerd.
2. Voer de volgende PowerShell-opdrachten op de server:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
    Login-AzureRmStorageSync -SubscriptionID <guid> -TenantID <guid>
    Reset-AzureRmStorageSyncServerCertificate -SubscriptionId <guid> -ResourceGroupName <string> -StorageSyncServiceName <string>
    ```

<a id="-1906441711"></a><a id="-2134375654"></a><a id="doesnt-have-enough-free-space"></a>**Het volume waar het servereindpunt bevindt is onvoldoende schijfruimte.**  
| | |
|-|-|
| **HRESULT** | 0x8e5e0211 |
| **HRESULT (decimaal)** | -1906441711 |
| **Fouttekenreeks** | JET_errLogDiskFull |
| **Herstel is vereist** | Ja |
| | |
| **HRESULT** | 0x80c8031a |
| **HRESULT (decimaal)** | -2134375654 |
| **Fouttekenreeks** | ECS_E_NOT_ENOUGH_LOCAL_STORAGE |
| **Herstel is vereist** | Ja |

Deze fout treedt op omdat het volume is gevuld. Deze fout treedt doorgaans op omdat bestanden buiten het servereindpunt ruimte vrij op het volume. Maak ruimte vrij op het volume dat door het toevoegen van extra servereindpunten, bestanden te verplaatsen naar een ander volume of het vergroten van het volume het servereindpunt is op.

<a id="-2134364145"></a><a id="replica-not-ready"></a>**De service is nog niet klaar om te synchroniseren met dit servereindpunt.**  
| | |
|-|-|
| **HRESULT** | 0x80c8300f |
| **HRESULT (decimaal)** | -2134364145 |
| **Fouttekenreeks** | ECS_E_REPLICA_NOT_READY |
| **Herstel is vereist** | Nee |

Deze fout treedt op omdat er wijzigingen in de Azure-bestandsshare rechtstreeks en detectie van de wijziging uitgevoerd wordt. Synchronisatie begint wanneer de detectie van de wijziging is voltooid.

[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="-2134375877"></a><a id="-2134375908"></a><a id="-2134375853"></a>**Synchronisatie is mislukt vanwege problemen met veel afzonderlijke bestanden.**  
| | |
|-|-|
| **HRESULT** | 0x80c8023b |
| **HRESULT (decimaal)** | -2134364145 |
| **Fouttekenreeks** | ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED |
| **Herstel is vereist** | Ja |
| | |
| **HRESULT** | 0x80c8021c |
| **HRESULT (decimaal)** | -2134375908 |
| **Fouttekenreeks** | ECS_E_SYNC_METADATA_KNOWLEDGE_LIMIT_REACHED |
| **Herstel is vereist** | Ja |
| | |
| **HRESULT** | 0x80c80253 |
| **HRESULT (decimaal)** | -2134375853 |
| **Fouttekenreeks** | ECS_E_TOO_MANY_PER_ITEM_ERRORS |
| **Herstel is vereist** | Ja |

In gevallen waarbij er veel per bestand synchronisatiefouten, synchronisatiesessies mislukken kunnen beginnen. Zie voor het oplossen van deze status [per bestand/map synchronisatiefouten oplossen]().

> [!NOTE]
> Azure File Sync maakt een tijdelijke VSS-momentopname eenmaal per dag op de server om bestanden te synchroniseren met open ingangen.

<a id="-2134376423"></a>**Synchronisatie mislukt vanwege een probleem met het pad naar de eindpunt.**  
| | |
|-|-|
| **HRESULT** | 0x80c80019 |
| **HRESULT (decimaal)** | -2134376423 |
| **Fouttekenreeks** | ECS_E_SYNC_INVALID_PATH |
| **Herstel is vereist** | Ja |

Controleer of het pad bestaat, op een lokaal NTFS-volume staat en geen reparsepunt of bestaand servereindpunt is.

<a id="-2134375817"></a>**Synchronisatie is mislukt omdat de versie van het filter-stuurprogramma niet compatibel met de agent-versie is**  
| | |
|-|-|
| **HRESULT** | 0x80C80277 |
| **HRESULT (decimaal)** | -2134375817 |
| **Fouttekenreeks** | ECS_E_INCOMPATIBLE_FILTER_VERSION |
| **Herstel is vereist** | Ja |

Deze fout treedt op omdat Cloud Tiering filter (StorageSync.sys) versie van het stuurprogramma geladen niet compatibel met de service opslag-Sync-Agent (FileSyncSvc is). Als de Azure File Sync-agent is geüpgraded, start opnieuw op de server om de installatie te voltooien. Als de fout zich voordoen blijft, verwijdert u de agent, de server opnieuw opstarten en de Azure File Sync-agent opnieuw installeren.

<a id="-2134376373"></a>**De service is momenteel niet beschikbaar.**  
| | |
|-|-|
| **HRESULT** | 0x80c8004b |
| **HRESULT (decimaal)** | -2134376373 |
| **Fouttekenreeks** | ECS_E_SERVICE_UNAVAILABLE |
| **Herstel is vereist** | Nee |

Deze fout treedt op omdat de Azure File Sync-service niet beschikbaar is. Deze fout wordt automatisch oplossen wanneer de Azure File Sync-service omdat beschikbaar opnieuw.

<a id="-2134375922"></a>**Synchronisatie mislukt vanwege een tijdelijk probleem met de synchronisatiedatabase.**  
| | |
|-|-|
| **HRESULT** | 0x80c8020e |
| **HRESULT (decimaal)** | -2134375922 |
| **Fouttekenreeks** | ECS_E_SYNC_METADATA_WRITE_LEASE_LOST |
| **Herstel is vereist** | Nee |

Deze fout treedt op vanwege een intern probleem opgetreden met de synchronisatiedatabase. Deze fout wordt automatisch oplossen wanneer de Azure File Sync wanneer synchronisatie opnieuw probeert. Als deze fout zich blijft voor een periode uitbreiden voordoen, maak een ondersteuningsaanvraag en wij nemen contact met u zodat u kunt dit probleem oplossen.

### <a name="common-troubleshooting-steps"></a>Algemene stappen voor probleemoplossing
<a id="troubleshoot-storage-account"></a>**Controleer of dat het opslagaccount bestaat.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Navigeer naar de groep voor synchronisatie binnen de Opslagsynchronisatieservice.
2. Selecteer het cloudeindpunt in de groep voor synchronisatie.
3. Noteer de naam van de Azure file share in het geopende deelvenster.
4. Selecteer het gekoppelde opslagaccount. Als deze koppeling is mislukt, is het opslagaccount waarnaar wordt verwezen, verwijderd.
    ![Een schermafbeelding van het detailpaneel van cloud-eindpunt met een koppeling naar het opslagaccount.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell
# Variables for you to populate based on your configuration
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"
$region = "<Az_Region>"
$resourceGroup = "<RG_Name>"
$syncService = "<storage-sync-service>"
$syncGroup = "<sync-group>"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# Log into the Azure account and put the returned account information
# in a reference variable.
$acctInfo = Connect-AzAccount

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = [System.String[]]@()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the " + `
        " selected Azure Region or the region is mistyped.")
}

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = [System.String[]]@()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    throw [System.Exception]::new("The provided resource group $resourceGroup does not exist.")
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region

# Check to make sure the provided Storage Sync Service
# exists.
$syncServices = [System.String[]]@()

Get-AzureRmStorageSyncService -ResourceGroupName $resourceGroup | ForEach-Object {
    $syncServices += $_.DisplayName
}

if ($storageSyncServices -notcontains $syncService) {
    throw [System.Exception]::new("The provided Storage Sync Service $syncService does not exist.")
}

# Check to make sure the provided Sync Group exists
$syncGroups = [System.String[]]@()

Get-AzureRmStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $syncService | ForEach-Object {
    $syncGroups += $_.DisplayName
}

if ($syncGroups -notcontains $syncGroup) {
    throw [System.Exception]::new("The provided sync group $syncGroup does not exist.")
}

# Get reference to cloud endpoint
$cloudEndpoint = Get-AzureRmStorageSyncCloudEndpoint `
    -ResourceGroupName $resourceGroup `
    -StorageSyncServiceName $storageSyncService `
    -SyncGroupName $syncGroup

# Get reference to storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object { 
    $_.Id -eq $cloudEndpoint.StorageAccountResourceId
}

if ($storageAccount -eq $null) {
    Write-Host "The storage account referenced in the cloud endpoint does not exist."
}
```
---

<a id="troubleshoot-network-rules"></a>**Controleer of dat het opslagaccount bevat geen netwerkregels.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Eenmaal in de storage-account, selecteert u **Firewalls en virtuele netwerken** aan de linkerkant van het opslagaccount.
2. In de storage-account, de **zodat toegang vanaf alle netwerken** keuzerondje moet worden geselecteerd.
    ![Een schermafbeelding van de storage-account firewall- en regels uitgeschakeld.](media/storage-sync-files-troubleshoot/file-share-inaccessible-2.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell
if ($storageAccount.NetworkRuleSet.DefaultAction -ne 
    [Microsoft.Azure.Commands.Management.Storage.Models.PSNetWorkRuleDefaultActionEnum]::Allow) {
    Write-Host ("The storage account referenced contains network " + `
        "rules which are not currently supported by Azure File Sync.")
}
```
---

<a id="troubleshoot-azure-file-share"></a>**Zorg ervoor dat de Azure-bestandsshare bestaat.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Klik op **overzicht** op de tabel links om terug te keren naar de pagina van het belangrijkste opslagaccount.
2. Selecteer **bestanden** om de lijst met bestandsshares weer te geven.
3. Controleer of de bestandsshare waarnaar wordt verwezen door de cloudeindpunt wordt weergegeven in de lijst met bestandsshares (u moet hebben opgemerkt dit in stap 1 hierboven).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $cloudEndpoint.StorageAccountShareName -and
    $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    Write-Host "The Azure file share referenced by the cloud endpoint does not exist"
}
```
---

<a id="troubleshoot-rbac"></a>**Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount.**  
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Klik op **toegangsbeheer (IAM)** op de linker inhoudsopgave.
1. Klik op de **roltoewijzingen** tabblad aan de lijst met de gebruikers en toepassingen (*service-principals*) die toegang hebben tot uw storage-account.
1. Controleer of **hybride File Sync-Service** wordt weergegeven in de lijst met de **Reader en gegevenstoegang** rol. 

    ![Een schermafbeelding van de service-principal voor hybride File Sync-Service in het tabblad voor het beheer van toegang van de storage-account](media/storage-sync-files-troubleshoot/file-share-inaccessible-3.png)

    Als **hybride File Sync-Service** niet wordt weergegeven in de lijst, voer de volgende stappen uit:

    - Klik op **Add**.
    - In de **rol** veld **Reader en toegang tot gegevens**.
    - In de **Selecteer** veld, typt u **hybride File Sync-Service**, selecteer de rol en klikt u op **opslaan**.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell    
$foundSyncPrincipal = $false
Get-AzRoleAssignment -Scope $storageAccount.Id | ForEach-Object { 
    if ($_.DisplayName -eq "Hybrid File Sync Service") {
        $foundSyncPrincipal = $true
        if ($_.RoleDefinitionName -ne "Reader and Data Access") {
            Write-Host ("The storage account has the Azure File Sync " + `
                "service principal authorized to do something other than access the data " + `
                "within the referenced Azure file share.")
        }

        break
    }
}

if (!$foundSyncPrincipal) {
    Write-Host ("The storage account does not have the Azure File Sync " + `
                "service principal authorized to access the data within the " + ` 
                "referenced Azure file share.")
}
```
---

### <a name="how-do-i-prevent-users-from-creating-files-containing-unsupported-characters-on-the-server"></a>Hoe ik voorkomen dat gebruikers het maken van bestanden met niet-ondersteunde tekens op de server?
U kunt [bestand Bestandsserverbronbeheer (FSRM) bestandscontroles](https://docs.microsoft.com/windows-server/storage/fsrm/file-screening-management) voor blok-bestanden met niet-ondersteunde tekens in hun namen worden gemaakt op de server. U moet dit doen met behulp van PowerShell als de meeste van de niet-ondersteunde tekens niet afdrukbare zijn en dus moet u eerst gecast van hun hexadecimale opmerkingen als tekens.

Maakt een groep van FSRM-bestand met de [cmdlet New-FsrmFileGroup](https://docs.microsoft.com/powershell/module/fileserverresourcemanager/new-fsrmfilegroup). In dit voorbeeld definieert de groep bevat slechts twee van de niet-ondersteunde tekens, maar u kunt opnemen als veel van de tekens zo nodig in uw groep.

```PowerShell
New-FsrmFileGroup -Name "Unsupported characters" -IncludePattern @(("*"+[char]0x00000090+"*"),("*"+[char]0x0000008F+"*"))
```

Als u de groep van een FSRM-bestand hebt gedefinieerd, kunt u het venster van een FSRM-bestand met de cmdlet New-FsrmFileScreen maken.

```PowerShell
New-FsrmFileScreen -Path "E:\AFSdataset" -Description "Filter unsupported characters" -IncludeGroup "Unsupported characters"
```

> [!Important]  
> Houd er rekening mee dat bestandscontroles alleen moeten worden gebruikt voor het blokkeren van het maken van de tekens die niet wordt ondersteund door Azure File Sync. Als bestandscontroles worden gebruikt in andere scenario's, synchronisatie voortdurend probeert te downloaden van de bestanden uit de Azure-bestandsshare naar de server en worden geblokkeerd vanwege de bestandscontrole, wat resulteert in hoge gegevensverkeer. 

## <a name="cloud-tiering"></a>Cloudopslaglagen 
Er zijn twee paden voor fouten in de cloud tiering:

- Bestanden kunnen niet naar tier, wat betekent dat dat Azure File Sync zonder succes probeert te laag van een bestand naar Azure Files.
- Bestanden kunnen niet worden ingetrokken, wat dat het bestandssysteemfilter van Azure File Sync betekent (StorageSync.sys) mislukt om gegevens te downloaden wanneer een gebruiker probeert te krijgen tot een bestand dat is gelaagde.

Er zijn twee belangrijke klassen van fouten die kunnen ontstaan via het pad van de fout:

- Fouten voor cloud-opslag
    - *Problemen met de servicebeschikbaarheid tijdelijke opslag*. Zie voor meer informatie de [Service Level Agreement (SLA) voor Azure Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).
    - *Niet-toegankelijke Azure-bestandsshare*. Deze fout doet zich doorgaans voor wanneer u de Azure-bestandsshare verwijderen wanneer deze nog steeds een cloudeindpunt in een synchronisatiegroep wordt.
    - *Niet-toegankelijke opslagaccount*. Deze fout doet zich doorgaans voor wanneer u het opslagaccount niet verwijderen terwijl er nog steeds een Azure-bestandsshare die een cloudeindpunt in een groep voor synchronisatie is. 
- Serverfouten 
    - *Azure File Sync bestandssysteemfilter (StorageSync.sys) is niet geladen*. Om te reageren op aanvragen in lagen/intrekken, moet het bestandssysteemfilter van Azure File Sync worden geladen. Het filter niet worden geladen kan gebeuren om verschillende redenen, maar de meest voorkomende reden is dat een beheerder verwijderd uit het geheugen deze handmatig. Het bestandssysteemfilter van Azure File Sync moet worden geladen op alle tijden voor Azure File Sync correct functioneren.
    - *Ontbreekt, is beschadigd of anderszins verbroken reparsepunt*. Een reparsepunt is een speciale gegevensstructuur op een bestand dat uit twee onderdelen bestaat:
        1. Een reparsepunt code, waarmee wordt aangegeven dat het besturingssysteem dat het bestandssysteemfilter van Azure File Sync (StorageSync.sys) moet mogelijk bepaalde actie wordt uitgevoerd op de i/o naar het bestand. 
        2. Reparse-gegevens, waarmee wordt aangegeven dat het bestandssysteemfilter van de URI van het bestand op de gekoppelde cloud-eindpunt (de Azure-bestandsshare). 
        
        De meest voorkomende manier die een reparsepunt beschadigd kan raken is als een beheerder probeert om de code of de gegevens te wijzigen. 
    - *Netwerkverbindingsproblemen*. Om te laag of intrekken van een bestand, moet de server verbinding met internet hebben.

De volgende secties leest u hoe u cloud cloudlagen problemen oplossen en bepalen of een probleem een probleem met de cloud-opslag of een serverfout is.

<a id="monitor-tiering-activity"></a>**Warmtemeting activiteit op een server controleren**  
U kunt controleren cloudlagen activiteit op een server met gebeurtenis-ID 9003, 9016 en 9029 in het logboek voor telemetrie (te vinden onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken).

- Gebeurtenis-ID 9003 biedt foutverdeling voor een servereindpunt. Totaal aantal fouten, ErrorCode, enzovoort. Let op: één gebeurtenis wordt geregistreerd per foutcode.
- Gebeurtenis-ID 9016 bevat de resultaten van het dupliceren van een volume. Vrije ruimte procent is, bijvoorbeeld ghosted aantal bestanden in de sessie voor het aantal bestanden kan niet ghost, enzovoort.
- Gebeurtenis-ID 9029 biedt de sessiegegevens dupliceren van een servereindpunt. Bijvoorbeeld, aantal bestanden die worden uitgevoerd in de sessie, aantal bestanden in de sessie, aantal bestanden gelaagd al lagen, enzovoort.

<a id="monitor-recall-activity"></a>**Intrekken-activiteit op een server controleren**  
U kunt controleren intrekken-activiteit op een server met gebeurtenis-ID 9005 9006, 9009 en 9059 in het logboek voor telemetrie (te vinden onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken).

- Gebeurtenis-ID 9005 biedt betrouwbaarheid van terugroeping voor een servereindpunt. Bijvoorbeeld: totaal aantal unieke bestanden zijn geopend, totale aantal unieke bestanden met mislukte toegang, enzovoort.
- Gebeurtenis-ID 9006 biedt foutverdeling intrekken voor een servereindpunt. Totaal aantal mislukte aanvragen, ErrorCode, enzovoort. Let op: één gebeurtenis wordt geregistreerd per foutcode.
- Gebeurtenis-ID 9009 biedt intrekken sessie-informatie voor een servereindpunt. Bijvoorbeeld, DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed, enzovoort.
- Gebeurtenis-ID 9059 biedt distributie van de toepassing het intrekken van een servereindpunt. Bijvoorbeeld, ShareId, toepassingsnaam en TotalEgressNetworkBytes.

<a id="files-fail-tiering"></a>**Problemen met bestanden die niet voldoen aan naar tier oplossen**  
Als bestanden niet naar tier naar Azure Files:

1. Bekijk de telemetrie, de operationele en diagnostische logboeken, zich bevindt onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken. 
    1. Controleer of dat de bestanden aanwezig zijn in de Azure-bestandsshare.

    > [!NOTE]
    > Een bestand moet worden gesynchroniseerd met een Azure-bestandsshare voordat tiers worden verdeeld.

    2. Controleer of dat de server verbinding heeft met internet. 
    3. Controleer of dat de Azure File Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
        - Voer bij een opdrachtprompt met verhoogde bevoegdheid `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestandssysteemfilters worden weergegeven.

> [!NOTE]
> Een gebeurtenis-ID 9003 eenmaal per uur in het gebeurtenislogboek telemetrie wordt geregistreerd als een bestand niet naar tier (een gebeurtenis wordt geregistreerd per foutcode). De operationele en diagnostische logboeken van de gebeurtenis moet worden gebruikt wanneer u aanvullende informatie nodig hebt om een probleem vast te stellen.

<a id="files-fail-recall"></a>**Problemen met bestanden die niet voldoen aan herinnerd oplossen**  
Als bestanden niet worden ingetrokken:
1. Bekijk de telemetrie, de operationele en diagnostische logboeken, zich bevindt onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken.
    1. Controleer of dat de bestanden aanwezig zijn in de Azure-bestandsshare.
    2. Controleer of dat de server verbinding heeft met internet. 
    3. Open de Services MMC-module en controleer of dat de service opslag-Sync-Agent (FileSyncSvc) wordt uitgevoerd.
    4. Controleer of dat de Azure File Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
        - Voer bij een opdrachtprompt met verhoogde bevoegdheid `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestandssysteemfilters worden weergegeven.

> [!NOTE]
> Een gebeurtenis-ID 9006 één keer per uur in het gebeurtenislogboek telemetrie wordt geregistreerd als een bestand niet intrekken (één gebeurtenis wordt geregistreerd per foutcode). De operationele en diagnostische logboeken van de gebeurtenis moet worden gebruikt wanneer u aanvullende informatie nodig hebt om een probleem vast te stellen.

<a id="files-unexpectedly-recalled"></a>**Problemen met bestanden onverwacht ingetrokken op een server oplossen**  
Antivirus-, back-up- en andere toepassingen die grote aantallen bestanden lezen ertoe leiden dat onbedoelde terughalen, tenzij ze het kenmerk overslaan offline respecteren en het lezen van de inhoud van die bestanden overslaan. Als u offlinebestanden overslaat voor producten die ondersteuning bieden voor deze optie, voorkomt u dat er onbedoeld wordt teruggehaald tijdens bijvoorbeeld antivirusscans en back-uptaken.

Vraag uw softwareleverancier hoe de oplossing kan worden geconfigureerd zodat offlinebestanden niet meer worden gelezen.

Onbedoelde terugroepingen kunnen ook optreden in andere scenario's, zoals wanneer u, bestanden in Verkenner bladert worden. Als u in Verkenner op de server een map opent met bestanden in cloudlagen, kan dat resulteren in onbedoeld terughalen. Dit is nog waarschijnlijker als er een antivirusoplossing is ingeschakeld op de server.

> [!NOTE]
>Gebeurtenis-ID 9059 in het gebeurtenislogboek telemetrie gebruiken om te bepalen welke toepassingen wordt veroorzaakt door terughalen. Deze gebeurtenis biedt distributie van de toepassing intrekken voor het servereindpunt van een en eenmaal per uur wordt geregistreerd.

## <a name="general-troubleshooting"></a>Algemene probleemoplossing
Als u problemen met Azure File Sync op een server, kunt u beginnen met de volgende stappen:
1. Bekijk de telemetrie, de operationele en diagnostische logboeken in Logboeken.
    - Synchroniseren, opslaglagen, en terughalen problemen worden geregistreerd in de telemetrie, diagnose- en operationele Logboeken onder logboeken toepassingen en Services\Microsoft\FileSync\Agent.
    - Problemen met betrekking tot het beheren van een server (bijvoorbeeld, configuratie-instellingen) worden geregistreerd in de operationele en diagnostische logboeken onder logboeken toepassingen en Services\Microsoft\FileSync\Management.
2. Controleer of dat de Azure File Sync-service wordt uitgevoerd op de server:
    - Open de Services MMC-module en controleer of de service opslag-Sync-Agent (FileSyncSvc) wordt uitgevoerd.
3. Controleer of dat de Azure File Sync filterstuurprogramma's (StorageSync.sys en StorageSyncGuard.sys) worden uitgevoerd:
    - Voer bij een opdrachtprompt met verhoogde bevoegdheid `fltmc`. Controleer of de StorageSync.sys en StorageSyncGuard.sys bestandssysteemfilters worden weergegeven.

Als het probleem niet is opgelost, moet u het hulpprogramma AFSDiag uitvoeren:
1. Maak een map waarin de uitvoer AFSDiag (bijvoorbeeld C:\Output) moet worden opgeslagen.
2. Open een PowerShell-venster met verhoogde bevoegdheid en voer de volgende opdrachten (druk op Enter na elke opdracht):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Voer voor het traceerniveau Azure File Sync-kernel-modus **1** (tenzij anders vermeld, uitgebreidere traceringen maken), en druk op Enter.
4. Voer voor het traceerniveau Azure File Sync gebruiker-modus **1** (tenzij anders vermeld, uitgebreidere traceringen maken), en druk op Enter.
5. Reproduceer het probleem. Wanneer u klaar bent, voert u **D**.
6. Een ZIP-bestand met Logboeken en logboekbestanden voor tracering wordt opgeslagen in de uitvoermap die u hebt opgegeven.

## <a name="see-also"></a>Zie ook
- [Monitor voor Azure File Sync](storage-sync-files-monitoring.md)
- [Veelgestelde vragen over Azure Files](storage-files-faq.md)
- [Problemen met Azure Files in Windows oplossen](storage-troubleshoot-windows-file-connection-problems.md)
- [Problemen met Azure Files oplossen in Linux](storage-troubleshoot-linux-file-connection-problems.md)
