---
title: Back-problemen oplossen met Azure virtual machines
description: Problemen oplossen met back-up en herstel van virtuele machines van Azure
services: backup
author: srinathvasireddy
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: srinathvasireddy
ms.openlocfilehash: 23137cd686bcdba59880ff705a43b16ced992b59
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66303981"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problemen oplossen met back-ups van virtuele Azure-machines
U kunt er zijn fouten opgetreden tijdens het gebruik van Azure Backup met de onderstaande informatie op te lossen:

## <a name="backup"></a>Backup
In deze sectie bevat informatie over back-upbewerking fout van Azure Virtual machine.

## <a name="copyingvhdsfrombackupvaulttakinglongtime---copying-backed-up-data-from-vault-timed-out"></a>CopyingVHDsFromBackUpVaultTakingLongTime - kopiëren van back-ups van gegevens uit de kluis is een time-out

Foutcode: CopyingVHDsFromBackUpVaultTakingLongTime <br/>
Foutbericht: Kopiëren van back-ups van gegevens uit de kluis is een time-out

Dit kan gebeuren vanwege onvoldoende opslagruimte op account IOPS aan voor de Backup-service gegevens overdraagt naar de kluis binnen de time-outperiode of van tijdelijke Opslagfouten. VM-back-up met behulp van deze configureren [aanbevolen procedures](backup-azure-vms-introduction.md#best-practices) en voer de back-upbewerking opnieuw uit.

## <a name="usererrorvmnotindesirablestate---vm-is-not-in-a-state-that-allows-backups"></a>UserErrorVmNotInDesirableState - VM is niet in een status waarin de back-ups.

Foutcode: UserErrorVmNotInDesirableState <br/>
Foutbericht: Virtuele machine is niet in een status waarin de back-ups.<br/>

De back-upbewerking is mislukt omdat de virtuele machine in de status mislukt. Voor geslaagde back-up van de virtuele machine moet de status actief, gestopt of gestopt (toewijzing opgeheven).

* Als de virtuele machine een tijdelijke status tussen is **met** en **afsluiten**, wacht totdat de status om te wijzigen. Vervolgens de back-uptaak activeert.
*  Als de virtuele machine een Linux-VM is en de module Security-Enhanced Linux-kernel gebruikt, sluit u het pad van de Azure Linux Agent **/var/lib/waagent** van het beveiligingsbeleid en zorg ervoor dat de back-up-extensie is geïnstalleerd.

## <a name="usererrorfsfreezefailed---failed-to-freeze-one-or-more-mount-points-of-the-vm-to-take-a-file-system-consistent-snapshot"></a>Kan de UserErrorFsFreezeFailed - niet blokkeren van een of meer koppelpunten van de virtuele machine naar een bestandssysteem consistente momentopname te maken

Foutcode: UserErrorFsFreezeFailed <br/>
Foutbericht: Kan een of meer koppelpunten van de VM niet stilzetten om een bestandssysteemconsistente momentopname te maken.

* Controleer de status van alle gekoppelde apparaten met behulp van de **tune2fs** opdracht, bijvoorbeeld **tune2fs -l/dev/sdb1 \\** .\| grep **bestandssysteem status**.
* Ontkoppel de apparaten die de systeemstatus van het bestand is niet opgeschoond waarvoor, met behulp van de **umount** opdracht.
* Een bestand system consistentiecontrole op deze apparaten worden uitgevoerd met behulp van de **fsck** opdracht.
* De apparaten opnieuw koppelen en voer de back-upbewerking opnieuw uit.</ol>

## <a name="extensionsnapshotfailedcom--extensioninstallationfailedcom--extensioninstallationfailedmdtc---extension-installationoperation-failed-due-to-a-com-error"></a>ExtensionSnapshotFailedCOM / ExtensionInstallationFailedCOM / ExtensionInstallationFailedMDTC - extensie installatie/per bewerking is mislukt vanwege een COM +-fout

Foutcode: ExtensionSnapshotFailedCOM <br/>
Foutbericht: De momentopnamebewerking is mislukt vanwege een COM +-fout

Foutcode: ExtensionInstallationFailedCOM  <br/>
Foutbericht: Extensiebewerking installatie is mislukt vanwege een COM +-fout

Foutcode: ExtensionInstallationFailedMDTC <br/>
Foutbericht: Installatie van de extensie is mislukt met de fout 'COM + is niet kan communiceren met de Microsoft Distributed Transaction Coordinator <br/>

De back-up is mislukt vanwege een probleem met Windows-service **COM + System** toepassing.  Volg deze stappen om dit probleem op te lossen:

* Windows-service starten/Start **COM + System Application** (vanaf een opdrachtprompt met verhoogde bevoegdheid **-net start COMSysApp**).
* Zorg ervoor dat **Distributed Transaction Coordinator** services wordt uitgevoerd als **netwerkservice** account. Als dit niet het geval is, wijzig dit in het run as- **netwerkservice** account in en start opnieuw op **COM + System Application**.
* Als aan de service opnieuw starten, opnieuw installeren **Distributed Transaction Coordinator** service door de onderstaande stappen te volgen:
    * Stop de MSDTC-service
    * Open een opdrachtprompt (cmd)
    * Opdracht uitvoeren ' msdtc-verwijderen "
    * ongedaan maken-opdracht ' msdtc-installeren "
    * Start de MSDTC-service
* Start de service Windows **COM + System Application**. Na de **COM + System Application** wordt gestart, activeert u een back-uptaak van de Azure-portal.</ol>

## <a name="extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state"></a>ExtensionFailedVssWriterInBadState - momentopname is mislukt omdat de VSS-schrijvers in orde zijn

Foutcode: ExtensionFailedVssWriterInBadState <br/>
Foutbericht: De momentopnamebewerking is mislukt omdat VSS-schrijvers in orde zijn.

Start opnieuw op VSS-schrijvers in orde. Voer vanuit een opdrachtprompt met verhoogde bevoegdheid ```vssadmin list writers```. De uitvoer bevat alle VSS-schrijvers en hun status. Voor elke VSS-schrijver met een status die geen **[1] stabiel**, zodat deze opnieuw starten van VSS-schrijver, voer de volgende opdrachten uit vanaf een opdrachtprompt met verhoogde bevoegdheid:

  * ```net stop serviceName```
  * ```net start serviceName```

## <a name="extensionconfigparsingfailure--failure-in-parsing-the-config-for-the-backup-extension"></a>ExtensionConfigParsingFailure - fout bij het parseren van de configuratie voor de Backup-extensie

Foutcode: ExtensionConfigParsingFailure<br/>
Foutbericht: Er is een fout opgetreden bij het parseren van de configuratie voor de back-upextensie.

Deze fout treedt op vanwege een gewijzigde machtigingen op de **als** directory: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.
Voer de volgende opdracht uit en controleer of dat machtigingen voor de **als** directory zijn standaard labels:**icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.

Standaardmachtigingen zijn als volgt:
* Iedereen: (R,W)
* BUILTIN\Administrators: (F)

Als er machtigingen in de **als** directory opgespoord die anders is dan de standaardinstellingen, volg deze stappen om de juiste machtigingen, het verwijderen van het certificaat en het activeren van de back-up:

1. Los van machtigingen op de **als** directory. Met behulp van Explorer security eigenschappen en instellingen voor geavanceerde beveiliging in de directory, kunt u machtigingen terug naar de standaardwaarden opnieuw instellen. Verwijder alle gebruikersobjecten, behalve de standaardinstellingen van de map en zorg ervoor dat de **iedereen** machtiging heeft speciale toegang als volgt:

    * Gegevens van de map/lezen lijst
    * Kenmerken lezen
    * Uitgebreide kenmerken lezen
    * Maken van bestanden/gegevens schrijven
    * Maken van mappen/gegevens toevoegen
    * Kenmerken schrijven
    * Uitgebreide kenmerken schrijven
    * De machtiging lezen
2. Verwijder alle certificaten die waar **verleend aan** is van het klassieke implementatiemodel of **Windows Azure CRP-Certificaatgenerator**:
    * [Certificaten op een lokale computer-console openen](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx).
    * Onder **persoonlijke** > **certificaten**, alle certificaten verwijderen waar **verleend aan** is van het klassieke implementatiemodel of **CRP van Windows Azure Certificaat Generator**.
3. Een virtuele machine back-uptaak activeert.

## <a name="extensionstuckindeletionstate---extension-state-is-not-supportive-to-backup-operation"></a>ExtensionStuckInDeletionState - status van de extensie is niet gericht op de back-upbewerking

Foutcode: ExtensionStuckInDeletionState <br/>
Foutbericht: Status van de extensie is niet gericht op de back-upbewerking

De back-up is mislukt vanwege inconsistente status van de back-up-uitbreiding. Volg deze stappen om dit probleem op te lossen:

* Zorg ervoor dat de gastagent geïnstalleerd en responsief is
* Ga van Azure-portal naar **Virtuele machine** > **Alle instellingen** > **Extensies**
* Selecteer de backup-extensie VmSnapshot of VmSnapshotLinux en klik op **Verwijderen**
* Probeer na verwijdering van de back-upextensie de back-upbewerking opnieuw
* De volgende back-upbewerking installeert de nieuwe extensie in de gewenste status

## <a name="extensionfailedsnapshotlimitreachederror---snapshot-operation-failed-as-snapshot-limit-is-exceeded-for-some-of-the-disks-attached"></a>ExtensionFailedSnapshotLimitReachedError - de momentopnamebewerking is mislukt omdat de limiet voor momentopnames is overschreden voor sommige van de schijven die zijn gekoppeld

Foutcode: ExtensionFailedSnapshotLimitReachedError  <br/>
Foutbericht: De momentopnamebewerking is mislukt omdat de limiet voor momentopnames is overschreden voor sommige van de schijven die zijn gekoppeld

De momentopnamebewerking is mislukt omdat de limiet voor momentopnames is overschreden voor sommige van de schijven die zijn gekoppeld. Voer de volgende probleemoplossingsstappen en probeer het opnieuw.

* Verwijder de blob-momentopnamen van de schijf die niet vereist. Wees voorzichtig met het schijf-blob niet verwijderen, alleen momentopname blobs moeten worden verwijderd.
* Als de functie voor voorlopig verwijderen is ingeschakeld op de VM-schijf Opslagaccounts, functie voor voorlopig verwijderen bewaren configureren zodat bestaande momentopnamen kleiner zijn dan de maximaal toegestane op elk moment zijn.
* Als Azure Site Recovery is ingeschakeld in de back-ups van virtuele machine, voer de onderstaande:

    * Zorg ervoor dat de waarde van **isanysnapshotfailed** is ingesteld op ONWAAR in /etc/azure/vmbackup.conf
    * Azure Site Recovery plannen op een ander tijdstip zodat er geen is de back-upbewerking conflict.

## <a name="extensionfailedtimeoutvmnetworkunresponsive---snapshot-operation-failed-due-to-inadequate-vm-resources"></a>ExtensionFailedTimeoutVMNetworkUnresponsive - momentopname is mislukt vanwege onvoldoende VM-resources.

Foutcode: ExtensionFailedTimeoutVMNetworkUnresponsive<br/>
Foutbericht: De momentopnamebewerking is mislukt vanwege onvoldoende VM-resources.

Back-upbewerking op de virtuele machine is mislukt vanwege vertragingen in netwerkaanroepen tijdens het uitvoeren van de momentopnamebewerking. U kunt dit probleem oplossen door stap 1 uit te voeren. Als het probleem zich blijft voordoen, probeert u stappen 2 en 3.

**Stap 1**: Momentopname maken via host

Voer vanaf een opdrachtprompt met verhoogde bevoegdheid (beheerder) de onderstaande opdracht uit:

```
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotMethod /t REG_SZ /d firstHostThenGuest /f
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v CalculateSnapshotTimeFromHost /t REG_SZ /d True /f
```

Deze opdracht zorgt ervoor dat de momentopnamen worden gemaakt via host in plaats van Guest. Probeer de back-upbewerking opnieuw.

**Stap 2**: Wijzig het back-upschema naar een tijd waarop de virtuele machine wordt minder belast (minder CPU-/ IOP's enz.)

**Stap 3**: Probeer [het vergroten van de virtuele machine](https://azure.microsoft.com/blog/resize-virtual-machines/) en probeer het opnieuw

## <a name="common-vm-backup-errors"></a>Veelvoorkomende fouten voor VM-back-up

| Foutdetails | Tijdelijke oplossing |
| ------ | --- |
| Foutcode: 320001<br/> Foutbericht: Kan de bewerking niet uitvoeren omdat de VM niet meer bestaat. <br/> <br/> Foutcode: 400094 <br/> Foutbericht: De virtuele machine bestaat niet <br/> <br/>  Een virtuele machine van Azure is niet gevonden.  |Deze fout treedt op wanneer de primaire virtuele machine is verwijderd, maar de back-upbeleid is nog steeds ziet er voor een virtuele machine back-up. U kunt deze fout oplossen door de volgende stappen uitvoeren: <ol><li> Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam **cloudservicenaam**,<br>**of**</li><li> De beveiliging van de virtuele machine met of zonder te verwijderen van de back-upgegevens stoppen. Zie voor meer informatie, [stoppen met het beveiligen van virtuele machines](backup-azure-manage-vms.md#stop-protecting-a-vm).</li></ol>|
| De virtuele machine is mislukte Inrichtingsstatus: <br>Opnieuw opstarten van de virtuele machine en controleer of dat de virtuele machine wordt uitgevoerd of afsluiten. | Deze fout treedt op wanneer een van de extensie-fouten in mislukte Inrichtingsstatus heeft de virtuele machine wordt geplaatst. Ga aan de lijst met extensies als er een uitbreiding is mislukt is, verwijdert u deze en probeer het opnieuw opstarten van de virtuele machine. Als alle extensies actief is, controleert u of de VM-Agent-service wordt uitgevoerd. Als dat niet het geval is, start de VM-Agent-service opnieuw. |
|Foutcode: UserErrorBCMPremiumStorageQuotaError<br/> Foutbericht: Kan de momentopname van de virtuele machine, vanwege onvoldoende beschikbare ruimte in het opslagaccount niet kopiëren | Voor premium-VM's op de VM-back-upstack V1 kopiëren we de momentopname van het naar het opslagaccount. Deze stap zorgt ervoor dat verkeer van de back-upbeheer, dat op de momentopname werkt, heeft geen beperkingen voor het aantal IOP's beschikbaar voor de toepassing met behulp van premium-schijven. <br><br>Het is raadzaam dat u alleen 50 procent van de totale opslagruimte voor de account 17,5 TB toewijzen. De Azure Backup-service kan de momentopname vervolgens kopiëren naar de storage-account en de overdracht van gegevens vanaf deze locatie gekopieerd in het opslagaccount naar de kluis. |
| Kan niet Microsoft Recovery Services-uitbreiding installeren als de virtuele machine wordt niet uitgevoerd <br>De VM-Agent is een vereiste voor de Azure Recovery Services-extensie. De Azure VM-Agent installeren en opnieuw starten van de bewerking voor de registratie. |<ol> <li>Controleer of de VM-Agent correct is geïnstalleerd. <li>Zorg ervoor dat de vlag aan de configuratie van de virtuele machine correct is ingesteld.</ol> Meer informatie over het installeren van de VM-Agent en hoe u de installatie van de VM-Agent te valideren. |
| De momentopnamebewerking is mislukt met de Volume Shadow Copy Service (VSS)-bewerkingsfout **dit station is vergrendeld door BitLocker-stationsversleuteling. U moet deze schijf van het Configuratiescherm ontgrendelen.** |BitLocker uitschakelen voor alle stations op de virtuele machine en controleer of de VSS-probleem is opgelost. |
| De VM zich niet in een status waarin de back-ups. |<ul><li>Als de virtuele machine een tijdelijke status tussen is **met** en **afsluiten**, wacht totdat de status om te wijzigen. Vervolgens de back-uptaak activeert. <li> Als de virtuele machine een Linux-VM is en de module Security-Enhanced Linux-kernel gebruikt, sluit u het pad van de Azure Linux Agent **/var/lib/waagent** van het beveiligingsbeleid en zorg ervoor dat de back-up-extensie is geïnstalleerd.  |
| De VM-Agent niet aanwezig op de virtuele machine: <br>Alle vereiste onderdelen en de VM-Agent installeren. Start de bewerking vervolgens opnieuw. |Meer informatie over [VM-Agent-installatie en het valideren van de installatie van de VM-Agent](#vm-agent). |
| Back-up is mislukt voor het blokkeren van een of meer koppelpunten van de virtuele machine naar een bestand system consistente momentopname te maken. | De volgende stap uitvoeren: <ul><li>Controleer de status van alle gekoppelde apparaten met behulp van de **'tune2fs'** opdracht. Een voorbeeld is **tune2fs -l/dev/sdb1 \\** .\| grep **bestandssysteem status**. <li>Ontkoppel de apparaten waarvoor de systeemstatus van het bestand is niet schoon met behulp van de **'umount'** opdracht. <li> Een bestand system consistentiecontrole op deze apparaten worden uitgevoerd met behulp van de **'fsck'** opdracht. <li> De apparaten opnieuw koppelen en probeer het back-up.</ol> |
| De momentopnamebewerking is mislukt vanwege fout bij het maken van een beveiligd netwerk communicatiekanaal. | <ol><li> Open de Register-Editor door te voeren **regedit.exe** in een modus met uitgebreide bevoegdheden. <li> Identificeer alle versies van .NET Framework aanwezig zijn in uw systeem. Ze zijn aanwezig in de hiërarchie van registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**. <li> Voor elke .NET Framework in de registersleutel aanwezig is, moet u de volgende sleutel toevoegen: <br> **SchUseStrongCrypto"=dword:00000001**. </ol>|
| De momentopnamebewerking is mislukt vanwege fout voor het installeren van Visual C++ Redistributable voor Visual Studio 2012. | Navigeer naar C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion en vcredist2012_x64 installeren.<br/>Zorg ervoor dat de registersleutelwaarde waarmee de installatie van de service is ingesteld op de juiste waarde. Dat wil zeggen, stel de **Start** waarde in de **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver** naar **3** en niet **4**. <br><br>Als u nog steeds problemen met de installatie hebt, start u de installatieservice opnieuw door te voeren **MSIEXEC/UNREGISTER** gevolgd door **MSIEXEC /REGISTER** vanaf een opdrachtprompt met verhoogde bevoegdheid.  |


## <a name="jobs"></a>Taken

| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Annulering wordt niet ondersteund voor dit taaktype: <br>Wacht totdat de taak is voltooid. |Geen |
| De taak niet in een is annuleerbaar status: <br>Wacht totdat de taak is voltooid. <br>**of**<br> De geselecteerde taak heeft geen is annuleerbaar status: <br>Wacht tot de taak is voltooid. |Is het waarschijnlijk dat de taak bijna voltooid is. Wacht totdat de taak is voltooid.|
| Back-up kan de taak niet annuleren omdat deze niet wordt uitgevoerd: <br>Annulering wordt alleen ondersteund voor taken worden uitgevoerd. Probeer het annuleren van een taak wordt uitgevoerd. |Deze fout treedt op vanwege een tijdelijke status. Wacht even en probeer de annuleringsbewerking. |
| Back-up annuleren van de taak is mislukt: <br>Wacht totdat de taak is voltooid. |Geen |

## <a name="restore"></a>Herstellen

| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Herstellen is mislukt vanwege een interne fout van de cloud. |<ol><li>De cloudservice waarnaar u wilt herstellen, is geconfigureerd met DNS-instellingen. U kunt controleren: <br>**$deployment = get-Azure-implementatie - servicenaam 'Servicenaam'-sleuf 'Productie' Get-AzureDns - DnsSettings $deployment. DnsSettings**.<br>Als **adres** is geconfigureerd, wordt de DNS-instellingen zijn geconfigureerd.<br> <li>De cloudservice waarnaar u wilt herstellen, is geconfigureerd met **ReservedIP**, en bestaande VM's in de cloudservice in de status ' gestopt '. U kunt controleren dat een service in de cloud een IP-adres is gereserveerd met behulp van de volgende PowerShell-cmdlets: **$deployment = Get-Azure-implementatie - servicenaam 'servicenaam'-sleuf 'Productie' $dep. ReservedIPName**. <br><li>U probeert te herstellen van een virtuele machine met de volgende speciale netwerkconfiguraties in dezelfde cloudservice: <ul><li>Virtuele machines in onder de load balancer-configuratie, interne en externe.<li>Virtuele machines met meerdere gereserveerde IP-adressen. <li>Virtuele machines met meerdere NIC's. </ul><li>Selecteer een nieuwe cloudservice in de gebruikersinterface of Zie [herstellen overwegingen met betrekking tot](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) voor virtuele machines met speciale netwerkconfiguraties.</ol> |
| De geselecteerde DNS-naam wordt al gebruikt: <br>Geef een andere DNS-naam en probeer het opnieuw. |Deze DNS-naam verwijst naar de naam van de cloudservice, gewoonlijk eindigt **. cloudapp.net**. Deze naam moet uniek zijn. Als u deze fout optreedt, moet u een andere VM-naam kiezen tijdens het terugzetten. <br><br> Deze fout wordt alleen voor gebruikers van de Azure-portal weergegeven. De herstelbewerking via PowerShell slaagt omdat deze alleen de schijven worden hersteld en de virtuele machine niet maken. De fout zal worden geconfronteerd bij de virtuele machine expliciet door u gemaakt is nadat de herstelbewerking van de schijf. |
| De opgegeven virtuele-netwerkconfiguratie is niet correct: <br>Geef de configuratie van een ander virtueel netwerk en probeer het opnieuw. |Geen |
| De opgegeven cloudservice wordt met behulp van een gereserveerde IP-adres dat komt niet overeen met de configuratie van de virtuele machine die wordt hersteld: <br>Geef een andere cloudservice die geen gebruik een gereserveerd IP-adres maakt. Of kies een ander herstelpunt kunt herstellen. |Geen |
| De service in de cloud heeft de limiet voor het aantal invoereindpunten bereikt: <br>Voer de bewerking opnieuw uit door een andere cloudservice op te geven of met behulp van een bestaand eindpunt. |Geen |
| De Recovery Services-kluis en de doelresources storage-account zich in twee verschillende regio's: <br>Zorg ervoor dat het opslagaccount dat is opgegeven in de herstelbewerking zich in dezelfde Azure-regio als de Recovery Services-kluis. |Geen |
| Het opslagaccount dat is opgegeven voor de herstelbewerking wordt niet ondersteund: <br>Alleen Basic of Standard storage-accounts met lokaal redundante of geografisch redundante replicatie-instellingen worden ondersteund. Selecteer een ondersteund opslagaccount. |Geen |
| Het type opslagaccount dat is opgegeven voor de herstelbewerking niet online is: <br>Zorg ervoor dat het opslagaccount dat is opgegeven in de herstelbewerking opnieuw online is. |Deze fout kan optreden vanwege een tijdelijke fout in Azure Storage of vanwege een storing. Kies een ander opslagaccount. |
| Het resourcegroepquotum is bereikt: <br>Sommige resourcegroepen verwijderen uit de Azure portal of neem contact op met ondersteuning van Azure om de limieten te verhogen. |Geen |
| Het geselecteerde subnet bestaat niet: <br>Selecteer een subnet dat zich voordoet. |Geen |
| De Backup-service heeft geen machtiging voor toegang tot resources in uw abonnement. |U kunt deze fout oplossen door eerst schijven herstellen met behulp van de stappen in [herstel de schijven van een back-up](backup-azure-arm-restore-vms.md#restore-disks). Gebruik de stappen in de PowerShell [maken van een VM van herstelde schijven](backup-azure-vms-automation.md#restore-an-azure-vm). |

## <a name="backup-or-restore-takes-time"></a>Back-up of herstel kost tijd
Als uw back-up meer dan 12 uur duurt, of herstel meer dan 6 uur duurt, controleert u [aanbevolen procedures](backup-azure-vms-introduction.md#best-practices) en [prestatie-overwegingen](backup-azure-vms-introduction.md#backup-performance)

## <a name="vm-agent"></a>VM-Agent
### <a name="set-up-the-vm-agent"></a>Instellen van de VM-Agent
Normaal gesproken is de VM-Agent al aanwezig in virtuele machines die zijn gemaakt op basis van de Azure-galerie. Maar virtuele machines die zijn gemigreerd vanuit on-premises datacenters geen de VM-Agent is geïnstalleerd. Voor deze virtuele machines moet de VM-Agent expliciet worden geïnstalleerd.

#### <a name="windows-vms"></a>Virtuele Windows-machines

* Download en installeer de [agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet een Administrator-bevoegdheden om de installatie te voltooien.
* Voor virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel, [bijwerken van de VM-eigenschap](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. Deze stap is niet vereist voor virtuele machines van Azure Resource Manager.

#### <a name="linux-vms"></a>Virtuele Linux-machines

* Installeer de nieuwste versie van de agent uit de opslagplaats voor distributie. Zie voor meer informatie over de naam van het pakket, de [Linux-Agent opslagplaats](https://github.com/Azure/WALinuxAgent).
* Voor virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel, [gebruiken deze blog](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) voor het bijwerken van de eigenschap van de virtuele machine en controleer of de agent is geïnstalleerd. Deze stap is niet vereist voor virtuele machines Resource Manager.

### <a name="update-the-vm-agent"></a>De VM-Agent bijwerken
#### <a name="windows-vms"></a>Virtuele Windows-machines

* Voor het bijwerken van de VM-Agent opnieuw de [binaire bestanden voor VM-Agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Voordat u de agent bijwerkt, zorg er dan voor dat er geen back-upbewerkingen optreden tijdens het bijwerken van de VM-Agent.

#### <a name="linux-vms"></a>Virtuele Linux-machines

* Volg de instructies in het artikel voor het bijwerken van de Linux VM-Agent, [bijwerken van de Linux VM-Agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    > [!NOTE]
    > Gebruik altijd de distributie van-opslagplaats om bij te werken van de agent.

    Download de agentcode van GitHub. Als de meest recente agent is niet beschikbaar voor uw distributie, neem dan contact op met de distributie-ondersteuning voor instructies over het verkrijgen van de meest recente agent. U kunt ook controleren op de meest recente [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) informatie in de GitHub-opslagplaats.

### <a name="validate-vm-agent-installation"></a>VM-Agent-installatie valideren

Controleer of de versie van de VM-Agent op Windows-VM's:

1. Aanmelden bij de virtuele machine van Azure en navigeer naar de map **C:\WindowsAzure\Packages**. U vindt de **WaAppAgent.exe** bestand.
2. Met de rechtermuisknop op het bestand en Ga naar **eigenschappen**. Selecteer vervolgens de **Details** tabblad. De **productversie** veld moeten 2.6.1198.718 of hoger.

## <a name="troubleshoot-vm-snapshot-issues"></a>Oplossen van problemen met VM-momentopname
Back-up van virtuele machine is afhankelijk van de van momentopnameopdrachten naar de onderliggende opslag worden uitgegeven. Geen toegang hebben tot opslag of vertragingen in de taak van een momentopname worden uitgevoerd, kan de back-uptaak mislukt veroorzaken. De volgende voorwaarden kunnen leiden tot momentopname taak mislukt:

- **Netwerktoegang tot opslag is geblokkeerd met behulp van NSG**. Meer informatie over het [toegang tot het netwerk tot stand brengen](backup-azure-arm-vms-prepare.md#establish-network-connectivity) naar Storage met behulp van een lijst met toegestane IP-adressen of via een proxyserver.
- **Virtuele machines met SQL Server-back-up geconfigureerd kunnen leiden tot de taak voor momentopname vertraagd**. VM-back-up maakt standaard een VSS volledige back-up op Windows-VM's. Virtuele machines waarop SQL Server wordt uitgevoerd met SQL Server back-up is geconfigureerd, kunnen er vertragingen momentopname. Als de momentopname vertragingen in het back-upfouten, stelt u de volgende registersleutel:

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

- **VM-status onjuist is gemeld, omdat de virtuele machine wordt afgesloten in RDP**. Als u de extern bureaublad gebruikt om de virtuele machine af te sluiten, moet u controleren of de status van de virtuele machine in de portal juist is. Als de status niet correct is, gebruikt u de **afsluiten** optie in het portaldashboard van de virtuele machine om de virtuele machine af te sluiten.
- **Als meer dan vier virtuele machines delen dezelfde cloudservice, de virtuele machines verdeeld over meerdere back-upbeleid**. De back-uptijden, dus er is geen dat back-ups van meer dan vier VM starten op hetzelfde moment spreiden. Probeer te scheiden van de begintijden in het beleid door ten minste een uur.
- **De virtuele machine wordt uitgevoerd op een hoog CPU of geheugen**. Als de virtuele machine wordt uitgevoerd op meer dan 90 procent, veel geheugen of CPU-gebruik, wordt de momentopname-taak in de wachtrij geplaatst en vertraagd. Uiteindelijk er een time-out optreedt. Als dit probleem treedt op, probeert u een on-demand back-up.

## <a name="networking"></a>Netwerken
Net als alle extensies van nodig back-up-extensies toegang tot het openbare internet om te werken. Toegang kunt krijgen tot het openbare internet, kan zich op verschillende manieren manifest:

* Installatie van de extensie kan mislukken.
* Back-upbewerkingen, zoals de schijfmomentopname van de kunnen mislukken.
* Weergeven van de status van de back-upbewerking kan mislukken.

De noodzaak om op te lossen openbare internet-adressen wordt besproken in [deze ondersteuning voor Azure-blog](https://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Controleer de DNS-configuratie voor het VNET en zorg ervoor dat de Azure-URI's kunnen worden opgelost.

Nadat de naamomzetting correct is uitgevoerd, moet toegang tot de Azure-IP-adressen ook worden opgegeven. Als u wilt deblokkeren toegang tot de Azure-infrastructuur, een van deze stappen te volgen:

- Lijst met IP-adresbereiken Azure datacenter toestaan:
   1. Lijst van alle [Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) om te worden in de acceptatielijst.
   1. Blokkering van de IP-adressen met behulp van de [New-NetRoute](https://docs.microsoft.com/powershell/module/nettcpip/new-netroute) cmdlet. Voer deze cmdlet uit binnen de Azure-VM in een PowerShell-venster met verhoogde bevoegdheid. Uitvoeren als beheerder.
   1. Als u een aanwezig is, voor toegang tot de IP-adressen hebt, kunt u regels toevoegen aan de NSG.
- Maak een pad voor HTTP-verkeer:
   1. Als u bepaalde netwerkbeperking opgesteld hebben, implementeert u een HTTP-proxy-server om het verkeer te routeren. Een voorbeeld is een netwerkbeveiligingsgroep. Zie de stappen voor het implementeren van een HTTP-proxyserver in [netwerkverbinding tot stand brengen](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   1. Als u een aanwezig is, om toegang te verlenen tot het internet van de HTTP-proxy hebt, kunt u regels toevoegen aan de NSG.

> [!NOTE]
> DHCP moet zijn ingeschakeld in de Gast voor IaaS-VM back-up om te werken. Als u een statisch privé IP-adres nodig hebt, kunt u deze via de Azure portal of PowerShell configureren. Zorg ervoor dat de DHCP-optie binnen de virtuele machine is ingeschakeld.
> Meer informatie over het instellen van een statisch IP-adres via PowerShell ophalen:
> - [Een statische interne IP-adres toevoegen aan een bestaande virtuele machine](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
> - [De toewijzingsmethode voor een privé IP-adres toegewezen aan een netwerkinterface wijzigen](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)
>
>
