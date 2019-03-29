---
title: Back-problemen oplossen met Azure virtual machines
description: Problemen oplossen met back-up en herstel van virtuele machines van Azure
services: backup
author: srinathv
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: srinathv
ms.openlocfilehash: e5e84c22285d1cdec9678c8bf33dab1568d333cd
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621580"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problemen oplossen met back-ups van virtuele Azure-machines
U kunt fouten opgetreden tijdens het gebruik van Azure Backup met de gegevens die worden vermeld in de volgende tabel kunt oplossen:

| Foutdetails | Tijdelijke oplossing |
| ------ | --- |
| Back-up kan niet de bewerking niet uitvoeren omdat de virtuele machine (VM) niet meer bestaat: <br>De beveiliging van de virtuele machine zonder te verwijderen van back-upgegevens stoppen. Zie voor meer informatie, [stoppen met het beveiligen van virtuele machines](https://go.microsoft.com/fwlink/?LinkId=808124). |Deze fout treedt op wanneer de primaire virtuele machine is verwijderd, maar de back-upbeleid is nog steeds ziet er voor een virtuele machine back-up. U kunt deze fout oplossen door de volgende stappen uitvoeren: <ol><li> Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam **cloudservicenaam**,<br>**or**</li><li> De beveiliging van de virtuele machine met of zonder te verwijderen van de back-upgegevens stoppen. Zie voor meer informatie, [stoppen met het beveiligen van virtuele machines](https://go.microsoft.com/fwlink/?LinkId=808124).</li></ol> |
| De Azure Virtual Machine Agent (VM-Agent) kan niet communiceren met de Azure Backup-service: <br>Zorg ervoor dat de virtuele machine heeft een netwerkverbinding en de VM-agent is het laatste nieuws en uitgevoerd. Zie voor meer informatie, [Azure Backup-problemen oplossen-fout: Problemen met de agent of de extensie](https://go.microsoft.com/fwlink/?LinkId=800034). |Deze fout treedt op als er een probleem met de VM-Agent, of toegang tot het netwerk naar de Azure-infrastructuur is geblokkeerd op een bepaalde manier. Meer informatie over [problemen opsporen van fouten in VM momentopname](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup). <br><br>Als de VM-Agent niet wordt veroorzaakt door problemen, start u de VM opnieuw. Een onjuiste status voor de virtuele machine kan problemen veroorzaken en stelt de status opnieuw starten van de virtuele machine opnieuw. |
| De virtuele machine is mislukte Inrichtingsstatus: <br>Opnieuw opstarten van de virtuele machine en controleer of dat de virtuele machine wordt uitgevoerd of afsluiten. | Deze fout treedt op wanneer een van de extensie-fouten in mislukte Inrichtingsstatus heeft de virtuele machine wordt geplaatst. Ga aan de lijst met extensies als er een uitbreiding is mislukt is, verwijdert u deze en probeer het opnieuw opstarten van de virtuele machine. Als alle extensies actief is, controleert u of de VM-Agent-service wordt uitgevoerd. Als dat niet het geval is, start de VM-Agent-service opnieuw. |
| Back-up kan niet kopiëren van de momentopname van de virtuele machine vanwege onvoldoende beschikbare ruimte in de storage-account: <br>Zorg ervoor dat opslagaccount vrije schijfruimte die gelijk is aan de gegevens aanwezig zijn op de premium storage-schijven die zijn gekoppeld aan de virtuele machine heeft. | Voor premium-VM's op de VM-back-upstack V1 kopiëren we de momentopname van het naar het opslagaccount. Deze stap zorgt ervoor dat verkeer van de back-upbeheer, dat op de momentopname werkt, heeft geen beperkingen voor het aantal IOP's beschikbaar voor de toepassing met behulp van premium-schijven. <br><br>Het is raadzaam dat u alleen 50 procent van de totale opslagruimte voor de account 17,5 TB toewijzen. De Azure Backup-service kan de momentopname vervolgens kopiëren naar de storage-account en de overdracht van gegevens vanaf deze locatie gekopieerd in het opslagaccount naar de kluis. |
| Back-up kan de bewerking niet uitvoeren omdat de VM-Agent wordt reageert niet. |Deze fout treedt op als er een probleem met de VM-Agent of toegang tot het netwerk naar de Azure-infrastructuur is geblokkeerd op een bepaalde manier. Controleer de status van de VM-Agent-service in services en of de agent wordt weergegeven in programma's in het Configuratiescherm voor Windows-VM's. <br><br>Probeer te verwijderen van het programma in het Configuratiescherm en de agent opnieuw te installeren zoals beschreven in [VM-Agent](#vm-agent). Nadat u de agent opnieuw installeert, activeert u een ad-hoc back-up om deze te verifiëren. |
| De bewerking voor recovery services-extensie is mislukt: <br>Zorg ervoor dat de nieuwste VM-Agent aanwezig is op de virtuele machine en de VM-Agent-service wordt uitgevoerd. Probeer de back-upbewerking opnieuw. Als de back-upbewerking is mislukt, moet u contact op met Microsoft Support. |Deze fout treedt op wanneer de VM-Agent verouderd is. Raadpleeg los problemen met Azure virtuele machine back-up naar de VM-Agent bijwerken. |
| De virtuele machine bestaat niet: <br>Zorg ervoor dat de virtuele machine bestaat of Selecteer een andere virtuele machine. |Deze fout treedt op wanneer de primaire virtuele machine is verwijderd, maar de back-upbeleid is nog steeds ziet er voor een virtuele machine back-up. U kunt deze fout oplossen door de volgende stappen uitvoeren: <ol><li> Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam **cloudservicenaam**,<br>**or**<br></li><li>De beveiliging van de virtuele machine zonder te verwijderen van de back-upgegevens stoppen. Zie voor meer informatie, [stoppen met het beveiligen van virtuele machines](https://go.microsoft.com/fwlink/?LinkId=808124).</li></ol> |
| De opdracht uitvoeren: <br>Een andere bewerking wordt momenteel uitgevoerd voor dit item. Wacht totdat de vorige bewerking is voltooid. Voer de bewerking vervolgens opnieuw uit. |Een bestaande back-uptaak wordt uitgevoerd en een nieuwe taak kan niet worden gestart totdat de huidige taak is voltooid. |
| Het kopiëren van VHD's van de Recovery Services-kluis is een time-out: <br>Voer de bewerking over enkele minuten opnieuw uit. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. | Deze fout treedt op als er een tijdelijke fout aan de opslag, of als de Backup-service heeft voldoende opslagaccount IOPS gegevens overdraagt naar de kluis binnen de time-outperiode ontvangen. Zorg ervoor dat u de [aanbevolen procedures bij het configureren van uw virtuele machines](backup-azure-vms-introduction.md#best-practices). Uw virtuele machine verplaatsen naar een ander opslagaccount die niet is geladen en probeer de back-uptaak.|
| Back-up is mislukt met een interne fout: <br>Voer de bewerking over enkele minuten opnieuw uit. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |U kunt deze foutmelding krijgt om twee redenen: <ul><li> Er is een tijdelijk probleem bij het openen van de VM-opslag. Controleer de [status van Azure site](https://azure.microsoft.com/status/) om te zien of er Reken-, opslag- of netwerkproblemen in de regio. Nadat het probleem opgelost is, probeer de back-uptaak. <li> De oorspronkelijke virtuele machine is verwijderd en het herstelpunt kan niet worden uitgevoerd. Als u wilt de back-upgegevens bewaren voor een verwijderde virtuele machine, maar Verwijder de back-fouten, beveiliging opheffen van de virtuele machine en kies de optie om de gegevens te behouden. Deze actie worden de geplande back-uptaak en de terugkerende foutberichten gestopt. |
| Back-up is mislukt voor het installeren van de Azure Recovery Services-extensie op het geselecteerde item: <br>De VM-Agent is een vereiste voor de Azure Recovery Services-extensie. De Azure VM-Agent installeren en opnieuw starten van de bewerking voor de registratie. |<ol> <li>Controleer of de VM-Agent correct is geïnstalleerd. <li>Zorg ervoor dat de vlag aan de configuratie van de virtuele machine correct is ingesteld.</ol> Meer informatie over het installeren van de VM-Agent en hoe u de installatie van de VM-Agent te valideren. |
| Installatie van de extensie is mislukt met de fout **COM + is niet kan communiceren met de Microsoft Distributed Transaction Coordinator**. |Deze fout betekent meestal dat de COM +-service niet actief is. Neem contact op met Microsoft Support voor hulp bij het oplossen van dit probleem. |
| De momentopnamebewerking is mislukt met de Volume Shadow Copy Service (VSS)-bewerkingsfout **dit station is vergrendeld door BitLocker-stationsversleuteling. U moet deze schijf van het Configuratiescherm ontgrendelen.** |BitLocker uitschakelen voor alle stations op de virtuele machine en controleer of de VSS-probleem is opgelost. |
| De VM zich niet in een status waarin de back-ups. |<ul><li>Als de virtuele machine een tijdelijke status tussen is **met** en **afsluiten**, wacht totdat de status om te wijzigen. Vervolgens de back-uptaak activeert. <li> Als de virtuele machine een Linux-VM is en de module Security-Enhanced Linux-kernel gebruikt, sluit u het pad van de Azure Linux Agent **/var/lib/waagent** van het beveiligingsbeleid en zorg ervoor dat de back-up-extensie is geïnstalleerd.  |
| Een virtuele machine van Azure is niet gevonden. |Deze fout treedt op wanneer de primaire virtuele machine is verwijderd, maar de back-upbeleid is nog steeds ziet er voor de verwijderde virtuele machine. Corrigeer deze fout als volgt: <ol><li>Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam **cloudservicenaam**, <br>**or** <li> Schakel de beveiliging voor deze virtuele machine, zodat de back-uptaken wordt niet gemaakt. </ol> |
| De VM-Agent niet aanwezig op de virtuele machine: <br>Alle vereiste onderdelen en de VM-Agent installeren. Start de bewerking vervolgens opnieuw. |Meer informatie over [VM-Agent-installatie en het valideren van de installatie van de VM-Agent](#vm-agent). |
| De momentopnamebewerking is mislukt omdat de VSS-schrijvers in orde zijn. |Start opnieuw op VSS-schrijvers in orde. Voer vanuit een opdrachtprompt met verhoogde bevoegdheid ```vssadmin list writers```. De uitvoer bevat alle VSS-schrijvers en hun status. Voor elke VSS-schrijver met een status die geen **[1] stabiel**, zodat deze opnieuw starten van VSS-schrijver, voer de volgende opdrachten uit vanaf een opdrachtprompt met verhoogde bevoegdheid: <ol><li>```net stop serviceName``` <li> ```net start serviceName```</ol>|
| De momentopnamebewerking is mislukt vanwege een fout bij het parseren van de configuratie. |Deze fout treedt op vanwege een gewijzigde machtigingen op de **als** directory: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**. <br> Voer de volgende opdracht uit en controleer of deze machtigingen op de **als** directory zijn standaard die:<br>**icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**. <br><br>Standaardmachtigingen zijn als volgt: <ul><li>Iedereen: (R,W) <li>BUILTIN\Administrators: (F)</ul> Als er machtigingen in de **als** directory opgespoord die anders is dan de standaardinstellingen, volg deze stappen om de juiste machtigingen, het verwijderen van het certificaat en het activeren van de back-up: <ol><li>Los van machtigingen op de **als** directory. Met behulp van Explorer security eigenschappen en instellingen voor geavanceerde beveiliging in de directory, kunt u machtigingen terug naar de standaardwaarden opnieuw instellen. Verwijder alle gebruikersobjecten, behalve de standaardinstellingen van de map en zorg ervoor dat de **iedereen** machtiging heeft speciale toegang als volgt: <ul><li>Gegevens van de map/lezen lijst <li>Kenmerken lezen <li>Uitgebreide kenmerken lezen <li>Maken van bestanden/gegevens schrijven <li>Maken van mappen/gegevens toevoegen<li>Kenmerken schrijven<li>Uitgebreide kenmerken schrijven<li>De machtiging lezen </ul><li>Verwijder alle certificaten die waar **verleend aan** is van het klassieke implementatiemodel of **Windows Azure CRP-Certificaatgenerator**:<ol><li>[Certificaten op een lokale computer-console openen](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx).<li>Onder **persoonlijke** > **certificaten**, alle certificaten verwijderen waar **verleend aan** is van het klassieke implementatiemodel of **CRP van Windows Azure Certificaat Generator**.</ol> <li>Een virtuele machine back-uptaak activeert. </ol>|
| De Azure Backup-service beschikt niet over voldoende machtigingen voor Azure Key Vault voor back-up van versleutelde virtuele machines. |Om deze machtigingen in PowerShell bieden de Backup-service met behulp van de stappen in [maken van een VM van herstelde schijven](backup-azure-vms-automation.md). |
|Installatie van de momentopname-extensie is mislukt met de fout **COM + is niet kan communiceren met de Microsoft Distributed Transaction Coordinator**. | De Windows-service starten vanaf een opdrachtprompt met verhoogde bevoegdheid **COM + System Application**. Een voorbeeld is **net start COMSysApp**. Als de service niet kan worden gestart, klikt u vervolgens de volgende stappen uitvoeren:<ol><li> Zorg ervoor dat de aanmeldingsaccount van de service **Distributed Transaction Coordinator** is **netwerkservice**. Als dat niet het geval is, wijzigt u het account aanmelden op **netwerkservice** en start de service opnieuw. Probeer te starten **COM + System Application**.<li>Als **COM + System Application** wordt niet gestart, de volgende stappen uit om te verwijderen en installeer de service **Distributed Transaction Coordinator**: <ol><li>Stop de MSDTC-service. <li>Open een opdrachtprompt **cmd**. <li>Voer de opdracht ```msdtc -uninstall``` uit. <li>Voer de opdracht ```msdtc -install``` uit. <li>Start de MSDTC-service. </ol> <li>Start de service Windows **COM + System Application**. Na de **COM + System Application** wordt gestart, activeert u een back-uptaak van de Azure-portal.</ol> |
|  De momentopnamebewerking is mislukt vanwege een COM +-fout. | Het is raadzaam dat u de Windows-service opnieuw starten **COM + System Application** vanaf een opdrachtprompt met verhoogde bevoegdheid **net start COMSysApp**. Als het probleem zich blijft voordoen, start u de VM opnieuw. Als opnieuw opstarten van de virtuele machine niet wordt opgelost, probeert u [verwijderen van de VMSnapshot-extensie](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout) en de back-up handmatig activeren. |
| Back-up is mislukt voor het blokkeren van een of meer koppelpunten van de virtuele machine naar een bestand system consistente momentopname te maken. | De volgende stap uitvoeren: <ul><li>Controleer de status van alle gekoppelde apparaten met behulp van de **'tune2fs'** opdracht. Een voorbeeld is **tune2fs -l/dev/sdb1 \\** .\| grep **bestandssysteem status**. <li>Ontkoppel de apparaten waarvoor de systeemstatus van het bestand is niet schoon met behulp van de **'umount'** opdracht. <li> Een bestand system consistentiecontrole op deze apparaten worden uitgevoerd met behulp van de **'fsck'** opdracht. <li> De apparaten opnieuw koppelen en probeer het back-up.</ol> |
| De momentopnamebewerking is mislukt vanwege fout bij het maken van een beveiligd netwerk communicatiekanaal. | <ol><li> Open de Register-Editor door te voeren **regedit.exe** in een modus met uitgebreide bevoegdheden. <li> Identificeer alle versies van .NET Framework aanwezig zijn in uw systeem. Ze zijn aanwezig in de hiërarchie van registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**. <li> Voor elke .NET Framework in de registersleutel aanwezig is, moet u de volgende sleutel toevoegen: <br> **SchUseStrongCrypto"=dword:00000001**. </ol>|
| De momentopnamebewerking is mislukt vanwege fout voor het installeren van Visual C++ Redistributable voor Visual Studio 2012. | Navigeer naar C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion en vcredist2012_x64 installeren. Zorg ervoor dat de registersleutelwaarde waarmee de installatie van deze service is ingesteld op de juiste waarde. Dat wil zeggen, de waarde van de registersleutel **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver** is ingesteld op **3** en niet **4**. <br><br>Als u nog steeds problemen met de installatie hebt, start u de installatieservice opnieuw door te voeren **MSIEXEC/UNREGISTER** gevolgd door **MSIEXEC /REGISTER** vanaf een opdrachtprompt met verhoogde bevoegdheid.  |


## <a name="jobs"></a>Taken

| Foutdetails | Tijdelijke oplossing |
| --- | --- |
| Annulering wordt niet ondersteund voor dit taaktype: <br>Wacht totdat de taak is voltooid. |Geen |
| De taak niet in een is annuleerbaar status: <br>Wacht totdat de taak is voltooid. <br>**or**<br> De geselecteerde taak heeft geen is annuleerbaar status: <br>Wacht tot de taak is voltooid. |Is het waarschijnlijk dat de taak bijna voltooid is. Wacht totdat de taak is voltooid.|
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

- **Netwerktoegang tot opslag is geblokkeerd met behulp van NSG**. Meer informatie over het [toegang tot het netwerk tot stand brengen](backup-azure-arm-vms-prepare.md#establish-network-connectivity) naar Storage met behulp van een van beide in een whitelist opnemen van IP-adressen of via een proxyserver.
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

- De IP-bereiken van het Azure-datacentrum toevoegen aan de lijst met toegestane IP-adressen:
   1. Lijst van alle [Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) goedkeuring.
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
