---
title: Azure Migrate ondersteuningsmatrix voor VMware-evaluatie en migratie
description: Geeft een overzicht van instellingen voor de ondersteuning en beperkingen voor evaluatie en migratie van VMware-machines naar Azure met behulp van de service Azure Migrate.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: raynew
ms.openlocfilehash: 567a6582e193208a7ff4aa37bafefe1dec4f4e8f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811581"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>Ondersteuningsmatrix voor VMware-evaluatie en migratie

U kunt de [Azure Migrate-service](migrate-overview.md) om te beoordelen en migreren van machines naar de Microsoft Azure-cloud. In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen voor het beoordelen en migreren van on-premises VMware-machines.


## <a name="vmware-scenarios"></a>Scenario's voor VMware

De tabel bevat een overzicht van ondersteunde scenario's voor virtuele VMware-machines.

**Implementatie** | **Details** 
--- | --- 
**On-premises VMware-VM's beoordelen** | [Instellen van](tutorial-prepare-vmware.md) uw eerste evaluatie.<br/><br/> [Voer](scale-vmware-assessment.md) een grootschalige evaluatie.
**Virtuele VMware-machines migreren** | U kunt migreren met behulp van migratie zonder agent, met enkele beperkingen of gebruik een migratie op basis van een agent. [Meer informatie](server-migrate-overview.md)


    


## <a name="azure-migrate-projects"></a>Azure Migrate-projecten

**Ondersteuning** | **Details**
--- | ---
Azure-machtigingen | U moet machtigingen voor Inzender of eigenaar in het abonnement voor het maken van een Azure Migrate-project.
VMware-beperkingen  | Evalueer tot 35.000 VMware-VM's in een enkel project.

Een project kan bestaan zowel VMware-VM's en Hyper-V-machines, totdat de evaluatie.

## <a name="assessment-vmware-server-requirements"></a>Evaluatie van de VMware-serververeisten

Deze tabel bevat een overzicht van ondersteuning voor evaluatie en beperkingen voor VMware-virtualisatieservers.

**Ondersteuning** | **Details**
--- | ---
**vCenter-server** | VMware-VM's die u wilt beoordelen, moeten worden beheerd door een of meer vCenter-Servers met 5.5, 6.0, 6.5 of 6.7.

## <a name="assessment-vcenter-server-permissions"></a>Evaluatie van de vCenter-servermachtigingen

Voor evaluatie moet u een alleen-lezen-account voor de vCenter-Server.

## <a name="assessment-appliance-requirements"></a>Vereisten voor evaluatie-apparaat

**Ondersteuning** | **Details**
--- | ---
**ESXi** | De virtuele machine van het apparaat moet worden geïmplementeerd op een ESXi-host met versie 5.5 of later.
**Azure Migrate-project** | Een apparaat kan worden gekoppeld aan een enkel project.
**vCenter Server** | Een apparaat kan maximaal 10.000 VMware-VM's op een vCenter-Server te detecteren.<br/> Een apparaat kan verbinding maken met een vCenter-Server.


## <a name="assessment-url-access-requirements"></a>Vereisten voor evaluatie-URL

Het apparaat Azure Migrate moet internetverbinding met internet.

- Wanneer u het apparaat implementeert, heeft Azure Migrate een controle van de netwerkverbinding tot de URL's in de onderstaande tabel wordt samengevat.
- Als u een URL-gebaseerde firewall.proxy, kunt u toegang tot deze URL's, ervoor te zorgen dat de proxy wordt omgezet een CNAME-records hebt ontvangen tijdens het opzoeken van de URL's.

**URL** | **Details**  
--- | --- |
*.portal.azure.com  | Navigeer naar de Azure Migrate in Azure portal.
*.windows.net | Meld u aan bij uw Azure-abonnement.
*.microsoftonline.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate.
management.azure.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate.
dc.services.visualstudio.com | App-logboeken die worden gebruikt voor het controleren van interne te uploaden.
*.vault.azure.net | Geheimen in de Azure Key Vault beheren.
*.servicebus.windows.net | Communicatie tussen het apparaat en de service Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Verbinding maken met Azure Migrate-service-URL's.
*.blob.core.windows.net | Gegevens uploaden naar storage-accounts.


## <a name="assessment-port-requirements"></a>Evaluatie-port requirements for Windows

**apparaat** | **verbinding**
--- | --- 
Apparaat | Binnenkomende verbindingen op TCP-poort 3389 voor verbindingen met extern bureaublad op het apparaat toestaan.<br/> Binnenkomende verbindingen op poort 44368 voor externe toegang tot de toestel-management-app met behulp van de URL: https://<appliance-ip-or-name>:44368 <br/>Uitgaande verbindingen op poort 443 voor het verzenden van metagegevens voor de detectie en prestaties voor het migreren van Azure.
vCenter-server | Binnenkomende verbindingen op TCP-poort 443 zodat het apparaat voor het verzamelen van configuratie- en prestatiemetagegevens voor evaluaties. <br/> Het apparaat verbinding maakt met vCenter op poort 443 standaard. Als de vCenter-server op een andere poort luistert, kunt u de poort wijzigen bij het instellen van detectie.


## <a name="agentless-migration-vmware-server-requirements"></a>Zonder agent migratie-VMware-serververeisten

Deze tabel bevat een overzicht van ondersteuning voor evaluatie en beperkingen voor VMware-virtualisatieservers.

**Ondersteuning** | **Details**
--- | ---
**vCenter-server** | VMware-VM's die u migreert met behulp van de migratie van een zonder agent moet worden beheerd door een of meer vCenter-Servers met 5.5, 6.0, 6.5 of 6.7.

## <a name="agentless-migration-vcenter-server-permissions"></a>Machtigingen voor zonder agent migratie-vCenter-Server

**Machtigingen** | **Details**
--- | --- 
Datastore.Browse | Toestaan dat bladeren van VM-logboekbestanden om op te lossen momentopname maken en verwijderen.
Datastore.LowLevelFileOperations | Toestaan dat bewerkingen voor lezen/schrijven/verwijderen/wijzigen in de browser gegevensopslag, om op te lossen momentopname maken en verwijderen.
VirtualMachine.Configuration.DiskChangeTracking | Inschakelen of uitschakelen van bijhouden van VM-schijven, voor het ophalen van de gewijzigde blokken van gegevens tussen momentopnamen
VirtualMachine.Configuration.DiskLease | Lease schijfbewerkingen voor een virtuele machine, om te lezen van de schijf met behulp van de VMware vSphere virtuele schijf Development Kit (VDDK) toestaan.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Openen van een schijf op een virtuele machine, om te lezen van de schijf met behulp van de VDDK toestaan.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Hiermee kunt u lezen bewerkingen op bestanden die zijn gekoppeld aan een virtuele machine, om te downloaden van de logboeken en problemen oplossen als de fout optreedt. 
VirtualMachine.SnapshotManagement.* | Toestaan dat het maken en beheren van VM-momentopnamen voor replicatie. 
Virtuele Machine.Interaction.Power uitschakelen | Toestaan dat de virtuele machine wordt uitgeschakeld tijdens de migratie naar Azure.


## <a name="agentless-migration-vmware-vm-requirements"></a>Vereisten voor zonder agent migratie-VMware-VM 's

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde besturingssystemen** | [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) en [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) besturingssystemen die worden ondersteund door Azure kan worden gemigreerd met migratie zonder agent.
**Vereiste wijzigingen voor Azure** | Sommige virtuele machines mogelijk moeten worden gewijzigd zodat ze in Azure uitvoeren kunnen. Met Azure Migrate kunt deze wijzigingen automatisch voor de volgende besturingssystemen:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> -Debian 7,8<br/><br/> Voor andere besturingssystemen moet u aanpassingen handmatig vóór de migratie. De relevante artikelen bevatten instructies over hoe u dit doet.
**Opstarten voor Linux** | Als er zich bevinden op een specifieke partitie, moet zich bevinden op de besturingssysteemschijf en niet worden verdeeld over meerdere schijven.<br/> Als bevinden maakt deel uit van de hoofdpartitie (/), moet klikt u vervolgens de partitie '/' zich op de schijf met het besturingssysteem en andere schijven zijn niet verdeeld.
**Opstarten via UEFI** | Virtuele machines met opstarten via UEFI worden niet ondersteund voor migratie.
**Versleutelde schijven/volumes** | Virtuele machines met versleutelde schijven/volumes worden niet ondersteund voor migratie.
**RDM/passthrough-schijven** | Als virtuele machines RDM of passthrough-schijven hebt, wordt deze schijven wordt niet gerepliceerd naar Azure.
**NFS** | NFS-volumes die zijn gekoppeld als volumes op de virtuele machines wordt niet worden gerepliceerd.
**Doelschijf** | VM's kunnen alleen worden gemigreerd naar managed disks (standard HHD, premium SSD) in Azure.


## <a name="agentless-migration-appliance-requirements"></a>Vereisten voor migratie-apparaat zonder agent


**Ondersteuning** | **Details**
--- | ---
**ESXi** | De virtuele machine van het apparaat moet worden geïmplementeerd op een ESXi-host met versie 5.5 of later.
**Azure Migrate-project** | Een apparaat kan worden gekoppeld aan een enkel project.
**vCenter Server** | Een apparaat kan maximaal 10.000 VMware-VM's op een vCenter-Server te detecteren.<br/> Een apparaat kan verbinding maken met een vCenter-Server.
**VDDK** | Als u een zonder agent-migratie met Azure Migrate-servermigratie uitvoert, moet de VMware vSphere virtuele schijf Development Kit (VDDK) worden geïnstalleerd op de virtuele machine van het apparaat.

## <a name="agentless-migration-url-access-requirements"></a>Vereisten voor zonder agent migratie-URL

Het apparaat Azure Migrate moet internetverbinding met internet.

- Wanneer u het apparaat implementeert, heeft Azure Migrate een controle van de netwerkverbinding tot de URL's in de onderstaande tabel wordt samengevat.
- Als u een URL-gebaseerde firewall.proxy, kunt u toegang tot deze URL's, ervoor te zorgen dat de proxy wordt omgezet een CNAME-records hebt ontvangen tijdens het opzoeken van de URL's.

**URL** | **Details**  
--- | --- 
*.portal.azure.com | Navigeer naar de Azure Migrate in Azure portal.
*.windows.net | Meld u aan bij uw Azure-abonnement.
*.microsoftonline.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate.
management.azure.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate.
dc.services.visualstudio.com | App-logboeken die worden gebruikt voor het controleren van interne te uploaden.
*.vault.azure.net | Geheimen in de Azure Key Vault beheren.
*.servicebus.windows.net | Communicatie tussen het apparaat en de service Azure Migrate.
*.discoverysrv.windowsazure.com<br/> *.migration.windowsazure.com<br/> *.hypervrecoverymanager.windowsazure.com | Verbinding maken met Azure Migrate-service-URL's.
*.blob.core.windows.net | Gegevens uploaden naar storage-accounts.


## <a name="agentless-migration-port-requirements"></a>Migratie-port requirements for zonder agent

**apparaat** | **verbinding**
--- | --- 
Apparaat | Uitgaande TCP-poort 3389 voor gerepliceerde gegevens uploaden naar Azure, en om te communiceren met Azure Migrate voor replicatie en migratie.
vCenter-server | Binnenkomende verbindingen op TCP-poort 443 zodat het apparaat kan de replicatie - momentopnamen, kopiëren van gegevens, maken momentopnamen vrijgeven
vSphere/EXSI-host | Verkeer op TCP-poort 902 voor het apparaat om gegevens te repliceren van momentopnamen. 


## <a name="agent-based-migration-vmware-server-requirements"></a>Op basis van een agent, migratie-VMware-serververeisten

Deze tabel bevat een overzicht van ondersteuning voor evaluatie en beperkingen voor VMware-virtualisatieservers.

**Ondersteuning** | **Details**
--- | ---
**vCenter server/ESXI** | VMware-VM's die u migreert, moet worden beheerd door een of meer vCenter Server 5.5, 6.0, 6.5 of 6.7 actief of heeft die worden uitgevoerd op een ESXI-host met vSphere-versie 5.5, 6.0, 6.5 of 6.7.

### <a name="agent-based-migration-vcenter-server-permissions"></a>Op basis van een agent, migratie-vCenter Server-machtigingen

**Machtigingen** | **Details**
--- | --- 
Datastore.AllocateSpace | Toegewezen ruimte op een gegevensarchief toestaan voor een virtuele machine, momentopname of kloon van virtuele schijf. 
Datastore.Browse | Toestaan dat bladeren van VM-logboekbestanden om op te lossen momentopname maken en verwijderen.
Datastore.LowLevelFileOperations | Lezen, schrijven, verwijderen en wijzig de naam van bewerkingen in de browser gegevensarchief om op te lossen momentopname maken/verwijderen.
Datastore.UpdateVirtualMachineFiles | Paden naar VM-bestanden op een gegevensarchief bijgewerkt nadat het gegevensarchief resignatured is toestaan
Network.AssignNetwork | Toestaan dat een netwerk toewijzen aan een VM-Resource.
AssignVirtualMachineToResourcePool | Toewijzing van een virtuele machine aan een resourcegroep toestaan.
Resource.MigratePoweredOffVirtualMachine | Migratie van een uitgeschakelde VM aan een andere resourcegroep of de host staan.
Resource.MigratePoweredOnVirtualMachine | Migratie met behulp van vMotion van een VM ingeschakeld op een andere resourcegroep of de host staan.
Tasks.CreateTask | Toestaan dat een uitbreiding van een gebruiker gedefinieerde-taak maken.
Tasks.UpdateTask | Toestaan dat een uitbreiding van een gebruiker gedefinieerde taak bijwerken.
VirtualMachine.Configuration. | Toestaan dat de VM-opties en apparaten configureren.
Virtuele Machine.Interaction.AnswerQuestion | Toestaan dat de resolutie van problemen met statusovergangen van de virtuele machine of de runtime-fouten.
Virtuele Machine.Interaction.DeviceConnection | Het wijzigen van de verbindingsstatus van de ontkoppelde virtuele apparaten van een virtuele machine toegestaan.
Virtuele Machine.Interaction.ConfigureCDMedia | Configuratie van een virtueel CD-ROM of DVD-apparaat toestaan.
Virtuele Machine.Interaction.ConfigureFloppyMedia | Configuratie van een virtueel diskettestation apparaat toestaan.
Virtuele Machine.Interaction.PowerOff | Kan de virtuele machine wordt uitgeschakeld tijdens de migratie naar Azure.
Virtuele Machine.Interaction.PowerOn | Toestaan dat een VM ingeschakeld uitschakelen wordt ingeschakeld en hervat een onderbroken virtuele machine.
Virtual Machine.Interaction.VMwareToolsInstall | Toestaan dat koppelen en ontkoppelen van het installatieprogramma van de CD voor VMware-hulpprogramma's als een cd-rom-station voor het gastbesturingssysteem.
VirtualMachine.Inventory.CreateNew | Toestaan dat het maken van een virtuele machine en de toewijzing van de vereiste resources.
VirtualMachine.Inventory.Register | Toevoegen van een bestaande virtuele machine met een vCenter-Server of de host inventarisatie toestaan.
VirtualMachine.Inventory.Unregister | Toestaan dat de registratie van een VMe vanuit een vCenter-Server of de host-inventarisatie.
VirtualMachine.Provisioning.AllowVirtualMachineFilesUpload | Schrijfbewerkingen op bestanden die zijn gekoppeld aan een virtuele machine, met inbegrip van vmx, schijven, logboeken en nvram toestaan.
VirtualMachine.Provisioning.AllowVirtualMachineDownload | Toestaan dat leesbewerkingen op bestanden die zijn gekoppeld aan een virtuele machine, voor het downloaden van de logboeken voor het oplossen van problemen.
VirtualMachine.SnapshotManagement.RemoveSnapshot | Toestaan dat het verwijderen van een momentopname van de geschiedenis van de momentopname.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Vereisten voor migratie-replicatie-agent op basis van apparaat

De vereisten voor de [replicatie toestel](migrate-replication-appliance.md) gebruikt voor het migreren op basis van een agent van virtuele VMware-machines en fysieke servers met Azure Migrate-servermigratie worden samengevat in de tabel.

> [!NOTE]
> Bij het instellen van de replicatie-apparaat met behulp van het OVA-sjabloon die u in de Azure Migrate-hub, wordt het apparaat wordt uitgevoerd Windows Server 2016 en voldoet aan de vereisten van het ondersteuningsteam. Als u de replicatie-apparaat handmatig op een fysieke server instellen en zorg ervoor dat voldoet het aan de vereisten.



**Onderdeel** | **Vereiste** 
--- | ---
 | **Instellingen voor VMware** (VMware-VM-apparaat)
**PowerCLI** | [PowerCLI versie 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) moet worden geïnstalleerd als het apparaat van de replicatie wordt uitgevoerd op een VMware-VM.
**Type NIC** | VMXNET3 (als het apparaat een VMware-VM is)
 | **Hardware-instellingen** 
CPU-kernen | 8 
RAM | 16 GB
Aantal schijven | Drie: De OS schijf, cacheschijf proces en bewaarstation.
Vrije schijfruimte (cache) | 600 GB
Vrije schijfruimte (bewaarschijf) | 600 GB
**Instellingen voor software** | 
Besturingssysteem | WindowsServer 2016 of Windows Server 2012 R2
Landinstelling van het besturingssysteem | Engels (en-us)
TLS | TLS 1.2 moet worden ingeschakeld.
.NET Framework | .NET framework 4.6 of hoger moet worden geïnstalleerd op de computer (met sterke cryptografie ingeschakeld.
MySQL | MySQL moet worden geïnstalleerd op het apparaat.<br/> MySQL moet worden geïnstalleerd. U kunt ook handmatig installeren of Site Recovery kan worden geïnstalleerd tijdens de implementatie van het apparaat. 
Andere apps | U mag niet andere apps uitvoeren op het apparaat van de replicatie.
Windows Server-functies | Deze rollen niet worden ingeschakeld: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Groepsbeleid | Geen groepsbeleid voor deze inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot registerbewerkingsprogramma's voorkomen. <br> -Logica vertrouwen voor bestandsbijlagen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Er zijn geen bestaande standaardwebsite <br> -Er zijn geen bestaande website/toepassing luistert op poort 443 <br>-Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling 
**Netwerkinstellingen** | 
Type IP-adres | Statisch 
Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport) 
Type NIC | VMXNET3 

### <a name="replication-appliance-url-access"></a>Toegang tot de URL's van de replicatie-apparaat

De replicatie-apparaat moet toegang hebben tot deze URL's.

**URL** | **Details**
--- | ---
\*.backup.windowsazure.com | Gebruikt voor gerepliceerde gegevensoverdracht en coördinatie
\*.store.core.windows.net | Gebruikt voor gerepliceerde gegevensoverdracht en coördinatie
\*.blob.core.windows.net | Gebruikt voor toegang tot opslagaccount waarin de gerepliceerde gegevens
\*.hypervrecoverymanager.windowsazure.com | Gebruikt voor bewerkingen en coördinatie
https:\//management.azure.com | Gebruikt voor bewerkingen en coördinatie 
*.services.visualstudio.com | Gebruikt voor andere doeleinden telemetrie (dit is optioneel)
time.nist.gov | Wordt gebruikt om de tijdsynchronisatie tussen de systeemtijd en de algemene tijd te controleren.
time.windows.com | Wordt gebruikt om de tijdsynchronisatie tussen de systeemtijd en de algemene tijd te controleren.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | OVF setup moet toegang hebben tot deze URL's. Ze worden gebruikt voor toegangsbeheer en identiteitsbeheer door Azure Active Directory
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | Om MySQL downloaden te voltooien


#### <a name="mysql-installation-options"></a>MySQL-installatieopties

MySQL kan worden geïnstalleerd op de replicatie-apparaat met een van deze methoden.

**Installeren** | **Details**
--- | ---
Handmatig downloaden en installeren | MySQL-toepassing downloaden en plaats deze in de map C:\Temp\ASRSetup vervolgens handmatig installeren.<br/> Bij het instellen van het toestel MySQL wordt weergegeven als reeds geïnstalleerd. 
Online niet downloaden | Het installatieprogramma van MySQL in de map C:\Temp\ASRSetup plaatsen. Wanneer u het apparaat installeren en klik om te downloaden en installeren van MySQL, gebruikt setup het installatieprogramma dat u hebt toegevoegd. 
Het downloaden vanuit Azure migreren | Wanneer u het apparaat installeert en MySQL wordt gevraagd, selecteert u **Download en installeer**.



## <a name="agent-based-migration-vmware-vm-requirements"></a>Vereisten voor migratie op basis van een agent-VMware-VM 's

**Ondersteuning** | **Details**
--- | ---
**Machine-werkbelasting** | Azure Migrate ondersteunt de migratie van elke workload (bijvoorbeeld Active Directory, SQL server, enzovoort) die worden uitgevoerd op een ondersteunde machine.
**Besturingssystemen** | Voor de meest recente informatie, raadpleegt u de [-besturingssysteemondersteuning](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) voor Site Recovery. Azure Migrate biedt ondersteuning voor besturingssysteem van identieke virtuele machine.
**Linux-systeem/Gast bestandsopslag** | Voor de meest recente informatie, raadpleegt u de [support voor Linux-bestand system](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) voor Site Recovery. Azure Migrate heeft identieke systeemondersteuning van de Linux-bestand.
**Netwerkopslag /** | Voor de meest recente informatie, raadpleegt u de [netwerk](../site-recovery/vmware-physical-azure-support-matrix.md#network) en [opslag](../site-recovery/vmware-physical-azure-support-matrix.md#storage) vereisten voor Site Recovery. Azure Migrate biedt identieke netwerkopslag vereisten.
**Azure-vereisten** | Voor de meest recente informatie, raadpleegt u de [Azure-netwerk](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [opslag](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage), en [compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) vereisten voor Site Recovery. Azure Migrate heeft dezelfde vereisten voor VMware-migratie.
**Mobility-service** | De Mobility-service-agent moet worden geïnstalleerd op elke virtuele machine die u wilt migreren.
**Doelschijf** | VM's kunnen alleen worden gemigreerd naar managed disks (standard HHD, premium SSD) in Azure.

   

## <a name="agent-based-migration-url-access-requirements"></a>Vereisten voor migratie-URL op basis van een agent

De Mobility-service die wordt uitgevoerd op virtuele VMware-machines moet internetverbinding met internet.

- Wanneer u de Mobility-service implementeert, wordt een controle van de netwerkverbinding naar de URL's in de onderstaande tabel wordt samengevat.
- Als u een URL-gebaseerde firewall.proxy, kunt u toegang tot deze URL's, ervoor te zorgen dat de proxy wordt omgezet een CNAME-records hebt ontvangen tijdens het opzoeken van de URL's.

**URL** | **Details**  
--- | --- 
*.portal.azure.com | Navigeer naar de Azure Migrate in Azure portal.
*.windows.net | Meld u aan bij uw Azure-abonnement.
*.microsoftonline.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate. 
management.azure.com | Active Directory-apps maken voor het apparaat om te communiceren met de service Azure Migrate.
dc.services.visualstudio.com | App-logboeken die worden gebruikt voor het controleren van interne te uploaden.
*.vault.azure.net | Geheimen in de Azure Key Vault beheren.
*.servicebus.windows.net | Communicatie tussen het apparaat en de service Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Verbinding maken met Azure Migrate-service-URL's.
*.blob.core.windows.net | Gegevens uploaden naar storage-accounts.

## <a name="agent-based-migration-port-requirements"></a>Migratie op basis van een agent-Poortvereisten

**apparaat** | **verbinding**
--- | --- 
VM's | De Mobility-service die wordt uitgevoerd op virtuele machines communiceert met de on-premises configuratieserver op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.<br/><br/> Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op de server configuration) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
Replicatie-apparaat | De replicatie-apparaat stuurt replicatie met Azure via HTTPS-poort 443 uitgaande.
Processerver | De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.<br/> De processerver wordt standaard uitgevoerd op het apparaat van de replicatie.

## <a name="azure-vm-requirements"></a>Vereisten voor Azure VM 's

Alle on-premises virtuele machines worden gerepliceerd naar Azure moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel wordt samengevat. Wanneer Site Recovery een controle van vereisten voor replicatie voert, mislukt de controle als aan bepaalde vereisten zijn niet voldaan.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gast-besturingssysteem | Controleer of de ondersteunde besturingssystemen voor [virtuele VMware-machines met behulp van zonder agents replicatie](#agentless-migration-vmware-vm-requirements), en voor [virtuele VMware-machines met behulp van replicatie op basis van een agent](#agent-based-migration-vmware-vm-requirements).<br/> U kunt elke werkbelasting die wordt uitgevoerd op een ondersteund besturingssysteem migreren. | Controle mislukt als niet-ondersteund.
Architectuur van de Gast-besturingssysteem | 64-bit. | Controle mislukt als niet-ondersteund.
Grootte van de besturingssysteemschijf | Maximaal 2048 GB. | Controle mislukt als niet-ondersteund.
Aantal besturingssysteemschijven | 1 | Controle mislukt als niet-ondersteund.
Aantal gegevensschijven | 64 of minder. | Controle mislukt als niet-ondersteund.
Grootte van de gegevensschijf | Maximaal 4095 GB | Controle mislukt als niet-ondersteund.
Netwerkadapters | Meerdere netwerkadapters worden ondersteund. | 
Gedeelde VHD | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
FC-schijf | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
BitLocker | Wordt niet ondersteund. | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt. 
VM-naam | Van 1 tot 63 tekens bevatten.<br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. |  Werk de waarde in de eigenschappen van de machine in Site Recovery.
Verbinding maken na de migratie van Windows | Verbinding maken met virtuele Azure-machines waarop Windows wordt uitgevoerd na de migratie:<br/> -Schakel RDP in op de on-premises VM vóór de migratie. Zorg dat TCP- en UDP-regels zijn toegevoegd voor het profiel **Openbaar** en dat RDP is toegestaan in **Windows Firewall** > **Toegestane apps** voor alle profielen.<br/> Voor site-naar-site VPN-toegang, schakelt u RDP en toestaan dat RDP in **Windows Firewall** -> **toegestane apps en functies** voor **domein en privé** netwerken. Controleer daarnaast of SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). | 
Verbinding maken na de migratie-Linux | Verbinding maken met Azure-VM's na de migratie met behulp van SSH:<br/> Controleer dat de Secure Shell-service is ingesteld op Start, en dat firewall-regels van een SSH-verbinding toestaan vóór de migratie, op de on-premises computer.<br/> Na een failover op de Azure-VM, binnenkomende verbindingen toestaan met de SSH-poort voor de regels voor netwerkbeveiligingsgroepen op de failover VM, en voor het Azure-subnet waarmee deze verbonden. Bovendien een openbaar IP-adres toevoegen voor de virtuele machine. |  


## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor VMware](tutorial-prepare-vmware.md) evaluatie en migratie.








