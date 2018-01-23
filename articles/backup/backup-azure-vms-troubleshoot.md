---
title: Back-fouten met de virtuele machine van Azure oplossen | Microsoft Docs
description: Back-up en herstel van virtuele machines in Azure oplossen
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/21/2018
ms.author: trinadhk;markgal;jpallavi;
ms.openlocfilehash: d8840d2561e6102fe1679c36e981de6614b84d54
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problemen oplossen met back-ups van virtuele Azure-machines
U kunt er zijn fouten opgetreden tijdens het gebruik van Azure Backup met informatie die worden vermeld in de onderstaande tabel kunt oplossen.

## <a name="backup"></a>Back-up maken

### <a name="error-the-specified-disk-configuration-is-not-supported"></a>Fout: De opgegeven schijf-configuratie wordt niet ondersteund.

> [!NOTE]
> We hebben een private preview ter ondersteuning van back-ups voor virtuele machines met > 1TB schijven. Raadpleeg voor informatie [Private preview voor back-ondersteuning voor grote schijven VM](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

Momenteel schijfgrootten biedt geen ondersteuning voor Azure Backup [groter zijn dan 1023GB](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- Als u schijven hebt die groter zijn dan 1 TB, [koppelt u nieuwe schijven](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) met een grootte van minder dan 1 TB <br>
- Kopieer vervolgens de gegevens van de schijf die groter is dan 1TB, naar de zojuist gemaakte schijf of schijven met een grootte van minder dan 1 TB. <br>
- Zorg ervoor dat alle gegevens zijn gekopieerd en verwijder de schijven die groter zijn dan 1 TB
- Start de back-up.

| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Kan de bewerking niet uitvoeren omdat de VM niet meer bestaat. -Stop de beveiliging van virtuele machine zonder back-upgegevens te verwijderen. More details at http://go.microsoft.com/fwlink/?LinkId=808124 |Dit gebeurt wanneer de primaire virtuele machine wordt verwijderd, maar het back-upbeleid zoekt naar een virtuele machine blijft back-up. Deze fout oplossen: <ol><li> Maak de virtuele machine met dezelfde naam en hetzelfde Resourcegroepnaam [cloudservicenaam]<br>(OR)</li><li> Stop de beveiliging van virtuele machine met of zonder te verwijderen van de back-upgegevens. [Meer informatie](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Momentopname-bewerking is mislukt, omdat er geen verbinding met het netwerk op de virtuele machine - Zorg ervoor dat virtuele machine toegang tot het netwerk. Voor momentopnamen wilt voltooien, ofwel geaccepteerde Azure datacenter IP-adresbereiken of instellen van een proxyserver voor toegang tot het netwerk. Raadpleeg http://go.microsoft.com/fwlink/?LinkId=800034 voor meer informatie. Als u al proxyserver gebruikt, zorg ervoor dat de instellingen van proxyserver correct zijn geconfigureerd | Deze fout wordt gegenereerd wanneer u de uitgaande internetverbinding op de virtuele machine weigeren. Verbinding met Internet is vereist voor VM-momentopname extensie om een momentopname van onderliggende schijven van de virtuele machine. [Meer informatie](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) voor het oplossen van de momentopname storingen als gevolg van geblokkeerde netwerktoegang. |
| VM-agent is kan niet communiceren met de Azure Backup-Service. -Zorg ervoor dat de VM heeft verbinding met het netwerk en de VM-agent is de meest recente en wordt uitgevoerd. Raadpleeg voor meer informatie http://go.microsoft.com/fwlink/?LinkId=800034 |Deze fout wordt gegenereerd als er een probleem met de VM-Agent of netwerktoegang tot de Azure-infrastructuur is geblokkeerd op een bepaalde manier. [Meer informatie](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) over foutopsporing van de virtuele machine een momentopname van problemen.<br> Als de VM-agent is niet wordt veroorzaakt door problemen, start u de virtuele machine. Een onjuiste status voor de virtuele machine kan soms problemen veroorzaken en stelt deze 'slechte staat' opnieuw starten van de virtuele machine opnieuw. |
| De VM bevindt zich in een mislukte inrichten status: Start de virtuele machine en zorg ervoor dat de virtuele machine uitgevoerd of afsluiten van de status voor back-up wordt | Dit gebeurt wanneer een van de extensie fouten status van de virtuele machine zich in een mislukte Inrichtingsstatus leidt. Ga naar de lijst met extensies en zien of er een mislukte uitbreiding, verwijderen en probeer het opnieuw starten van de virtuele machine. Als alle uitbreidingen zijn uitgevoerd, controleert u of de VM-agent-service wordt uitgevoerd. Als dat niet het geval is, start de VM-agent-service opnieuw. | 
| VMSnapshot-uitbreiding is mislukt voor beheerde schijven - probeer de back-upbewerking. Als het probleem zich weer voordoet, volg de instructies in 'http://go.microsoft.com/fwlink/?LinkId=800034'. Als dat niet meer werkt, neem contact op met Microsoft ondersteuning | Deze fout als Azure Backup-service is mislukt voor het activeren van een momentopname. [Meer informatie](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) over foutopsporing van de virtuele machine een momentopname van problemen. |
| Kan niet worden gekopieerd de momentopname van de virtuele machine vanwege onvoldoende beschikbare ruimte in het opslagaccount - Zorg ervoor dat opslagaccount vrije schijfruimte die gelijk is aan de gegevens op de premium-opslag-schijven gekoppeld aan de virtuele machine heeft | In geval van een VM's voor premium kopiëren we de momentopname naar storage-account. Dit is om ervoor te zorgen dat managementverkeer van back-op momentopname werkt, niet worden gebruikt voor het beperken van het aantal IOPS beschikbaar voor de toepassing met premium-schijven. Microsoft raadt dat u slechts 50% van de totale opslagruimte voor de account toewijzen, zodat de Azure Backup-service de momentopname naar storage-account en overdracht gegevens vanaf deze locatie gekopieerd in opslagaccount naar de kluis kopiëren kan. | 
| Kan de bewerking niet uitvoeren omdat de VM-agent niet reageert niet |Deze fout wordt gegenereerd als er een probleem met de VM-Agent of netwerktoegang tot de Azure-infrastructuur is geblokkeerd op een bepaalde manier. Controleer de servicestatus van de VM-agent op services en of de agent wordt weergegeven in het Configuratiescherm voor Windows-VM's. Verwijder het programma uit besturingselement deelvenster en de agent opnieuw te installeren zoals is vermeld [hieronder](#vm-agent). Nadat de agent opnieuw is geïnstalleerd, activeert een ad-hoc back-up om te controleren. |
| Recovery services-uitbreiding is mislukt. -Zorg dat de meest recente agent van de virtuele machine aanwezig op de virtuele machine is en agent-service wordt uitgevoerd. Probeer de back-upbewerking en als het mislukt, neem dan contact op met Microsoft ondersteuning. |Deze fout wordt gegenereerd wanneer de VM-agent is verouderd. Raadpleeg de sectie 'De VM-Agent bijwerken' hieronder om te werken van de VM-agent. |
| Virtuele machine bestaat niet. -Geef Zorg ervoor dat de virtuele machine bestaat, of Selecteer een andere virtuele machine. |Dit gebeurt wanneer de primaire virtuele machine is verwijderd, maar blijft de back-upbeleid zoekt u naar een virtuele machine back-up uitvoeren. Deze fout oplossen: <ol><li> Maak de virtuele machine met dezelfde naam en hetzelfde Resourcegroepnaam [cloudservicenaam]<br>(OR)<br></li><li>Stop de beveiliging van de virtuele machine zonder te verwijderen van de back-upgegevens. [Meer informatie](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Opdrachtuitvoering is mislukt. -Een andere bewerking wordt momenteel uitgevoerd voor dit item. Wacht totdat de vorige bewerking is voltooid en probeer het vervolgens opnieuw |Een bestaande back-up op de virtuele machine wordt uitgevoerd en een nieuwe taak kan niet worden gestart terwijl de bestaande taak wordt uitgevoerd. |
| Kopiëren van virtuele harde schijven van de back-upkluis is een time-out - probeer de bewerking over enkele minuten opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. | Dit gebeurt als er een tijdelijke fout aan de opslag, of als Backup-service is niet voldoende IOPS van opslagaccount hosten van de virtuele machine om de overdracht van gegevens binnen time-outperiode kluis. Zorg ervoor dat u hebt gevolgd [Best practices](backup-azure-vms-introduction.md#best-practices) tijdens het instellen van back-up. Verplaats de VM naar een andere opslag account die is niet geladen en probeer het opnieuw back-up.|
| Back-up is mislukt met een interne fout: probeer de bewerking over enkele minuten opnieuw. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support |U kunt deze fout krijgen omwille van 2: <ol><li> Er is een tijdelijk probleem bij het openen van de VM-opslag. Controleer of [Azure Status](https://azure.microsoft.com/en-us/status/) om te zien of er een voortdurende problemen berekenen, opslag- of netwerken in de regio. Probeer de back-uptaak als het probleem opgelost is. <li>De oorspronkelijke virtuele machine is verwijderd en daarom het herstelpunt kan niet worden uitgevoerd. Aan de back-upgegevens om een verwijderde virtuele machine te bewaren, maar het verwijderen van de back-fouten: beveiliging opheffen van de virtuele machine en kies de optie om de gegevens te behouden. Deze actie worden de geplande back-uptaak en de terugkerende foutberichten gestopt. |
| De VM-agent is mislukt voor het installeren van de Azure Recovery Services-extensie voor het geselecteerde item - is een vereiste voor de Azure Recovery Services-extensie. De Azure VM-agent installeren en opnieuw starten van de bewerking voor de registratie |<ol> <li>Controleer of de VM-agent correct is geïnstalleerd. <li>Zorg dat de vlag op de VM-configuratie correct is ingesteld.</ol> [Lees meer](#validating-vm-agent-installation) over het installeren van de VM-agent en het valideren van de installatie van de VM-agent. |
| Installatie van de uitbreiding is mislukt met de fout 'COM + is kan niet communiceren met de Microsoft Distributed Transaction Coordinator |Dit betekent meestal dat de COM +-service niet wordt uitgevoerd. Neem contact op met Microsoft ondersteuning voor hulp bij het oplossen van dit probleem. |
| Momentopname-bewerking is mislukt met de VSS-fout voor bewerking 'dit station is vergrendeld door BitLocker-stationsversleuteling. U moet deze schijf van het Configuratiescherm ontgrendelen. |BitLocker uitschakelen voor alle stations op de virtuele machine en bekijk of de VSS-probleem is opgelost |
| Virtuele machine zich niet in een status waarin de back-ups. |<ul><li>Controleer of VM een tijdelijke status tussen die wordt uitgevoerd en computer afsluiten omlaag is. Als het is, wacht u totdat de VM-status in een van deze en geactiveerd back-up opnieuw. <li> Als de virtuele machine een Linux-VM en kernel-module [Security Enhanced Linux] gebruikt is, moet u de Linux-Agent pad uitsluiten (_/var/lib/waagent_) van beveiliging beleid Zorg ervoor dat de Backup-extensie wordt geïnstalleerd.  |
| Azure virtuele Machine is niet gevonden. |Dit gebeurt wanneer de primaire virtuele machine is verwijderd, maar blijft de back-upbeleid zoekt u naar een virtuele machine een back-up uitvoeren. Deze fout oplossen: <ol><li>Maak de virtuele machine met dezelfde naam en hetzelfde Resourcegroepnaam [cloudservicenaam] <br>(OR) <li> Schakel de beveiliging voor deze virtuele machine zodat de back-uptaken wordt niet gemaakt. </ol> |
| Agent van de virtuele machine is niet aanwezig op de virtuele machine - Installeer eventuele vereiste en de VM-agent en start de bewerking vervolgens opnieuw. |[Lees meer](#vm-agent) over de installatie van de VM-agent en het valideren van de installatie van de VM-agent. |
| Momentopname-bewerking is mislukt vanwege VSS-schrijvers in orde |U moet opnieuw opstarten (Volume Shadow copy Service) VSS-schrijvers in orde. Voer hiertoe vanaf een opdrachtprompt met verhoogde bevoegdheid _vssadmin lijst schrijvers_. Uitvoer bevat alle VSS-schrijvers en hun status. Voor elke VSS-schrijver waarvan de status is niet '[1] stabiel' opnieuw starten VSS-schrijver met opdrachten na vanaf een opdrachtprompt met verhoogde bevoegdheid:<br> _net stop serviceName_ <br> _net start serviceName_|
| Momentopname-bewerking is mislukt vanwege een fout bij het parseren van de configuratie |Dit gebeurt vanwege gewijzigde machtigingen op de map MachineKeys: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Voer onderstaande opdracht en Ga na of machtigingen voor map MachineKeys standaard waarden:<br>_icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Standaardmachtigingen zijn:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>Als u machtigingen op MachineKeys directory is anders dan standaard ziet, volgt u onderstaande stappen voor de juiste machtigingen, het verwijderen van het certificaat en het activeren van de back-up.<ol><li>Los machtigingen op de map MachineKeys op.<br>Met beveiligingseigenschappen Explorer en geavanceerde beveiligingsinstellingen voor de map machtigingen terug naar de standaardwaarden opnieuw instellen, verwijdert u eventuele extra (dan standaard) gebruikersobject vanuit de map en zorg ervoor dat de 'iedereen' machtigingen had speciale toegang voor:<br>-Map weergeven / gegevens lezen <br>-Kenmerken lezen <br>-Uitgebreide kenmerken lezen <br>-Bestanden maken / gegevens schrijven <br>-Mappen maken / gegevens toevoegen<br>-Kenmerken schrijven<br>-Uitgebreide kenmerken schrijven<br>-De machtiging lezen<br><br><li>Verwijder alle certificaten die met veld 'Verleend aan' = 'Azure Service Management voor uitbreidingen voor Windows' of 'Windows Azure CRP certificaat Generator'.<ul><li>[Open de console van certificaten (lokale computer)](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Alle certificaten verwijderen (-> certificaten onder Personal) met veld 'Verleend aan' = 'Azure Service Management voor uitbreidingen voor Windows' of 'Windows Azure CRP certificaat Generator'.</ul><li>Trigger VM back-up. </ol>|
| Validatie is mislukt, omdat virtuele machine is versleuteld met BEK alleen. Back-ups kunnen alleen worden ingeschakeld voor virtuele machines die zijn versleuteld met BEK en KEK-sleutel. |Virtuele machine moeten worden versleuteld met BitLocker-versleutelingssleutel en coderingssleutel Key. Daarna wordt de back-up moet worden ingeschakeld. |
| Azure Backup-Service beschikt niet over voldoende machtigingen voor Sleutelkluis voor back-up van versleuteld virtuele Machines. |Backup-service moet worden opgegeven deze machtigingen in PowerShell met stappen die worden vermeld in **back-up inschakelen** sectie van [PowerShell documentatie](backup-azure-vms-automation.md). |
|Installatie van een momentopname van extensie is mislukt met fout - de COM + is kan niet communiceren met de Microsoft Distributed Transaction Coordinator | Probeer te starten 'COM + System Application' van de windows-service (vanaf een opdrachtprompt met verhoogde bevoegdheid - _net start COMSysApp_). <br>Als het mislukt tijdens het starten, volgt u onderstaande stappen te volgen:<ol><li> Valideren dat de aanmeldingsaccount van 'Distributed Transaction Coordinator' service 'Netwerkservice'. Als dit niet het geval is, moet u wijzigen naar 'Netwerkservice', start deze service opnieuw en probeer het starten van service 'COM + System Application'.'<li>Als dit nog steeds niet kan starten, verwijderen/installeren service 'Distributed Transaction Coordinator' aan de hand van onderstaande stappen te volgen:<br> -De MSDTC-service stoppen<br> -Open een opdrachtprompt (cmd) <br> -Voer de opdracht ' msdtc-verwijderen " <br> -Voer de opdracht ' msdtc-installeren " <br> -De MSDTC-service starten<li>'COM + System Application' van de windows-service starten en nadat deze is gestart, activeert u back-up van de portal.</ol> |
|  Momentopname-bewerking is mislukt vanwege de COM +-fout | De aanbevolen actie is 'COM + System Application' van de windows-service opnieuw starten (vanaf een opdrachtprompt met verhoogde bevoegdheid - _net start COMSysApp_). Als het probleem zich blijft voordoen, start u de virtuele machine opnieuw. Als opnieuw opstarten van de virtuele machine niet helpt, probeer [verwijderen van de extensie VMSnapshot](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) en om de back-up handmatig te activeren. |
| Kan een of meer koppelpunten bevatten van de virtuele machine naar een consistente momentopname bestandssysteem blokkeren | Voer de volgende stappen uit: <ol><li>Controleer de status van het bestandssysteem van alle gekoppelde apparaten met behulp van _'tune2fs'_ opdracht.<br> Bijvoorbeeld: tune2fs -l/dev/sdb1 \| grep 'Bestandssysteem status' <li>Ontkoppelen van de apparaten voor welke bestandssysteem status niet met behulp van schone is _'umount'_ opdracht <li> Voer FileSystemConsistency uit op deze apparaten met behulp van _'fsck'_ opdracht <li> De apparaten opnieuw koppelen en probeer het back-up.</ol> |
| Momentopname-bewerking is mislukt vanwege fout bij maken van het communicatiekanaal beveiligd netwerk | <ol><Li> Open de Register-Editor door te voeren regedit.exe in een modus met uitgebreide bevoegdheden. <li> Alle versies van identificeren. NetFramework aanwezig is in het systeem. Ze zijn aanwezig in de hiërarchie van de registersleutel 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft' <li> Voor elk. NetFramework aanwezig is in de registersleutel, plaats de volgende sleutel: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| Momentopname-bewerking is mislukt vanwege fout in de installatie van Visual C++ Redistributable voor Visual Studio 2012 | Navigeer naar C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion en vcredist2012_x64 installeren. Zorg ervoor dat registersleutelwaarde voor het toestaan van de installatie van deze service is ingesteld op de juiste waarde dat wil zeggen de waarde van registersleutel _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ is ingesteld op 3 en niet 4. Als u problemen met de installatie nog steeds worden geconfronteerd, start u de installatieservice opnieuw door te voeren _MSIEXEC/UNREGISTER_ gevolgd door _MSIEXEC /REGISTER_ vanaf een opdrachtprompt met verhoogde bevoegdheid.  |


## <a name="jobs"></a>Taken
| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Annulering wordt niet ondersteund voor dit taaktype: wacht totdat de taak is voltooid. |None |
| De taak is niet in een status Annuleerbare - wacht totdat de taak is voltooid. <br>OF<br> De geselecteerde taak is niet in een status Annuleerbare - wacht tot de taak te voltooien. |De taak is naar alle waarschijnlijkheid bijna voltooid. Wacht totdat de taak is voltooid.|
| De taak annuleren niet omdat deze niet uitgevoerd is-deze annulering wordt alleen ondersteund voor taken die uitgevoerd worden. Annuleer van poging op een onderhanden taak. |Dit gebeurt als gevolg van een tijdelijke status. Wacht een paar minuten en probeer de annuleringsbewerking. |
| Annuleer de taak - is mislukt. Wacht totdat de taak is voltooid. |Geen |

## <a name="restore"></a>Herstellen
| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Terugzetten is mislukt vanwege een interne Cloud fout |<ol><li>Cloudservice die u probeert te herstellen is geconfigureerd met DNS-instellingen. U kunt controleren <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-sleuf 'Productie' Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Als er is een adres dat is geconfigureerd, betekent dit dat de DNS-instellingen zijn geconfigureerd.<br> <li>Cloudservice waarnaar u wilt herstellen, is geconfigureerd met ReservedIP en bestaande virtuele machines in de cloudservice wordt gestopt.<br>U kunt controleren dat een cloudservice is gereserveerde IP-adres via de volgende powershell-cmdlets:<br>$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Production" $dep.ReservedIPName <br><li>U probeert een virtuele machine te herstellen met de volgende speciale netwerkconfiguraties in dezelfde cloudservice. <br>-Virtuele machines onder load balancer-configuratie (intern en extern)<br>-Virtuele machines met meerdere gereserveerde IP 's<br>-Virtuele machines met meerdere NIC 's<br>Selecteer een nieuwe cloudservice in de gebruikersinterface of Raadpleeg [terugzetten overwegingen](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) voor virtuele machines met speciale netwerkconfiguraties.</ol> |
| De geselecteerde DNS-naam is al in gebruik: Geef een andere DNS-naam en probeer het opnieuw. |De DNS-naam verwijst naar de naam van de cloudservice (meestal eindigend. cloudapp.net). Dit moet uniek zijn. Als deze fout optreedt, moet u een andere VM-naam kiezen tijdens het terugzetten. <br><br> Deze fout wordt alleen weergegeven op gebruikers van de Azure-portal. De herstelbewerking via PowerShell slaagt omdat deze alleen worden hersteld van de schijven en de virtuele machine niet maken. De fout wordt worden geconfronteerd wanneer de virtuele machine expliciet door u is gemaakt nadat de herstelbewerking van de schijf. |
| De configuratie van de opgegeven virtuele netwerk is niet correct: Geef een ander virtueel netwerk-configuratie en probeer het opnieuw. |None |
| De opgegeven cloudservice maakt gebruik van een gereserveerd IP-adres die komt niet overeen met de configuratie van de virtuele machine wordt hersteld - Geef een andere cloudservice, die niet van gereserveerde IP-adres gebruikmaakt, of kies een ander herstelpunt van herstellen. |Geen |
| Cloudservice is bereikt voor het aantal invoer eindpunten: Voer de bewerking opnieuw uit door op te geven van een andere cloudservice of met behulp van een bestaand eindpunt. |None |
| Back-kluis en doel storage-account zijn in twee verschillende regio's: Zorg ervoor dat het storage-account opgegeven in restore-bewerking in dezelfde Azure-regio als de back-upkluis. |None |
| Storage-Account opgegeven voor de herstelbewerking niet is ondersteund - alleen Basic/Standard storage-accounts met lokaal redundante of geografisch redundante replicatie-instellingen worden ondersteund. Selecteer een ondersteunde storage-account |None |
| Type Opslagaccount dat is opgegeven voor de herstelbewerking is niet online - Zorg ervoor dat het storage-account opgegeven in restore-bewerking online is |Dit kan gebeuren vanwege een tijdelijke fout in Azure Storage of als gevolg van een storing. Kies een ander opslagaccount. |
| Resourcegroep quotum is bereikt - Verwijder een aantal resourcegroepen van Azure portal of neem contact op met ondersteuning van Azure naar de limieten verhogen. |None |
| Geselecteerde subnet bestaat niet: Selecteer een subnet dat bestaat |None |
| De Backup-service heeft geen toegang tot resources in uw abonnement. |Om op te lossen, eerste herstellen schijven met stappen die worden vermeld in de sectie **terugzetten een back-up schijven** in [kiezen VM restore-configuratie](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Daarna Gebruik PowerShell-stappen die worden vermeld [een virtuele machine maken van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) volledige virtuele machine maken van herstelde schijven. |

## <a name="backup-or-restore-taking-time"></a>Backup of Restore duurt
Als u uw backup(>12 hours) of terugzetten duurt time(>6 hours):
* Begrijpen [factoren die bijdragen tot back-uptijd](backup-azure-vms-introduction.md#total-vm-backup-time) en [factoren die bijdragen voor het herstellen van tijd](backup-azure-vms-introduction.md#total-restore-time).
* Zorg ervoor dat u volgt [back-up van best practices](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-the-vm-agent"></a>Instellen van de VM-Agent
Normaal gesproken is de VM-Agent al aanwezig in virtuele machines die zijn gemaakt van de Azure-galerie. Virtuele machines die worden gemigreerd van lokale datacenters moet echter niet de VM-Agent is geïnstalleerd. Voor deze virtuele machines moet de VM-Agent expliciet worden geïnstalleerd.

Voor VM's van Windows:

* Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren.
* Voor klassieke virtuele machines, [werk de eigenschap VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. Deze stap is niet vereist voor Resource Manager virtuele machines.

Voor virtuele Linux-machines:

* Meest recente installeren uit de opslagplaats voor distributie. We **aangeraden** installeren van de agent beperken tot de opslagplaats voor distributie. Raadpleeg voor informatie over de naam van het pakket, [opslagplaats voor Linux-agent](https://github.com/Azure/WALinuxAgent) 
* Voor klassieke virtuele machines, [werk de eigenschap VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. Deze stap is niet vereist voor Resource Manager virtuele machines.

### <a name="updating-the-vm-agent"></a>De VM-agent bijwerken
Voor VM's van Windows:

* Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. Echter, moet u ervoor zorgen dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-Agent wordt bijgewerkt.

Voor virtuele Linux-machines:

* Volg de instructies op [Linux VM-Agent bijwerken](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
We **aangeraden** bijwerken agent beperken tot de opslagplaats voor distributie. We raden niet downloaden van de agent-code van rechtstreeks github en wordt bijgewerkt. Als de nieuwste agent is niet beschikbaar voor uw distributiepunt, kunt contact met ondersteuning voor instructies over het installeren van de nieuwste agent distributie. U kunt de meest recente controleren [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) informatie in de github-opslagplaats.

### <a name="validating-vm-agent-installation"></a>Installatie van de VM-Agent valideren
Het controleren van de versie van het VM-Agent op VM's van Windows:

1. Meld u aan bij de virtuele machine van Azure en navigeer naar de map *C:\WindowsAzure\Packages*. Het bestand WaAppAgent.exe moet hier aanwezig zijn.
2. Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. Het veld productversie moet 2.6.1198.718 of hoger

## <a name="troubleshoot-vm-snapshot-issues"></a>VM-momentopname oplossen
VM-back-afhankelijk van de momentopnameopdrachten uitgeven naar de onderliggende opslag. Geen toegang hebben tot opslag- of vertragingen in uitvoering van een momentopname van de taak kan leiden tot de back-uptaak is mislukt. De volgende kan taakfout momentopname veroorzaken.

1. Netwerktoegang tot opslag is geblokkeerd met behulp van de NSG<br>
    Meer informatie over het [-netwerktoegang inschakelen](backup-azure-arm-vms-prepare.md#establish-network-connectivity) voor opslag met beide WhiteListing van IP-adressen of proxyserver.
2. Virtuele machines met Sql Server back-up is geconfigureerd, kunnen leiden tot vertragingen momentopname <br>
   Standaard VM back-up problemen VSS volledige back-up op VM's van Windows. Dit kan op virtuele machines die worden uitgevoerd op het Sql-Servers en als de back-up van Sql Server is geconfigureerd, vertraging veroorzaken in momentopname worden uitgevoerd. Stel volgende registersleutel als u mislukte back-ups vanwege momentopname problemen ondervindt.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. VM-status gerapporteerd onjuist, omdat virtuele machine wordt afgesloten in RDP.  <br>
   Als u op de virtuele machine in RDP hebt afgesloten, Controleer in de portal dat de status van de virtuele machine correct wordt weergegeven. Als dat niet het geval is, sluit de virtuele machine in de portal met de optie 'Afsluiten' in de VM-dashboard.
4. Als meer dan vier virtuele machines de dezelfde cloudservice delen, configureert u meerdere back-upbeleid voor het voorbereiden van de back-ups dus geen is meer dan vier VM-back-ups op hetzelfde moment worden gestart. Probeer de back-up begintijden een uur uit elkaar liggen tussen beleidsregels verspreiden.
5. Virtuele machine wordt uitgevoerd op veel CPU/geheugen.<br>
   Als de virtuele machine wordt uitgevoerd op een hoog CPU usage(>90%) en geheugen, momentopname taak in de wachtrij, vertraging en uiteindelijk time-out wordt opgehaald. Probeer het op aanvraag back-up in dergelijke situaties.

<br>

## <a name="networking"></a>Netwerken
Net als alle uitbreidingen Backup-extensie toegang nodig tot het openbare internet om te werken. Geen toegang hebben tot het openbare internet merkt op verschillende manieren:

* De extensie-installatie kan mislukken
* De back-upbewerkingen (zoals schijf-momentopname) kunnen mislukken
* Weergeven van de status van de back-upbewerking kan mislukken

De noodzaak voor het oplossen van openbare internetadressen is gelede [hier](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). U moet de DNS-configuratie voor het VNET controleren en ervoor te zorgen dat de Azure-URI's omgezet worden kunnen.

Zodra de naamomzetting correct is uitgevoerd, moet toegang tot de Azure-IP-adressen ook worden opgegeven. Als u wilt deblokkeren toegang tot de Azure-infrastructuur, volg een van deze stappen:

1. De Azure-datacenter geaccepteerde IP-adresbereiken.
   * De lijst met [Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) naar wilt plaatsen.
   * De IP-adressen met behulp van de blokkering opheffen de [nieuw NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet. Deze cmdlet in de Azure-machine worden uitgevoerd in een verhoogde PowerShell-venster (als Administrator uitvoeren).
   * Regels toevoegen aan het NSG (als u geïmplementeerd hebt) voor toegang tot de IP-adressen.
2. Maken van een pad voor de HTTP-verkeer
   * Als u hebt implementeren sommige netwerkbeperking aanwezig (een Netwerkbeveiligingsgroep, bijvoorbeeld) een HTTP-proxyserver voor het routeren van verkeer. Stappen voor het implementeren van een HTTP-proxyserver kunnen gevonden [hier](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   * Regels toevoegen aan het NSG (als u geïmplementeerd hebt) om toegang tot het INTERNET toestaat vanaf de HTTP-Proxy.

> [!NOTE]
> DHCP moet binnen het gastbesturingssysteem IaaS VM back-up werkt zijn ingeschakeld.  Als u een statisch privé IP-adres nodig hebt, moet u deze via het platform configureren. De DHCP-optie in de virtuele machine moet naar links worden ingeschakeld.
> Meer informatie weergeven over [instellen van een statische interne persoonlijke IP-adres](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
