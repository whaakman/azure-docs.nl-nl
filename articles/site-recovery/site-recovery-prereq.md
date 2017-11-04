---
title: Vereisten voor replicatie naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Meer informatie over de vereisten voor virtuele machines en fysieke machines repliceren naar Azure met behulp van de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: cb7c3892dc0acbffdec636e5e7b3ce063660153c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-to-azure-by-using-site-recovery"></a>Vereisten voor replicatie van on-premises naar Azure met behulp van Site Recovery

> [!div class="op_single_selector"]
> * [Van Azure repliceren naar Azure](site-recovery-azure-to-azure-prereq.md)
> * [On-premises repliceren naar Azure](site-recovery-prereq.md)

Azure Site Recovery kunt u ondersteuning voor uw zakelijke continuïteit en noodherstelplan (BCDR) door replicatie van Azure een virtuele machine (VM) te organiseren naar een andere Azure-regio. Site Recovery repliceert ook fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter. Als een storing op uw primaire locatie optreedt, kunt u een failover naar een secundaire locatie om apps en workloads beschikbaar te houden. U kunt niet terug naar uw primaire locatie wanneer de primaire locatie weer normaal functioneert. Zie voor meer informatie over Site Recovery [wat is Site Recovery?](site-recovery-overview.md).

In dit artikel overzicht we van de vereisten voor het begin van de Site Recovery de replicatie van een lokale machine naar Azure.

U kunt na de eventuele opmerkingen aan de onderkant van het artikel. Ook kunt u vragen technische stellen in de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Azure-vereisten

**Vereiste** | **Details**
--- | ---
**Azure-account** | Een [Microsoft Azure-account](http://azure.microsoft.com/). U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
**Site Recovery-service** | Zie voor meer informatie over prijzen voor de Azure Site Recovery-service [prijzen voor Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | U moet een Azure Storage-account voor het opslaan van gerepliceerde gegevens. Het opslagaccount moet zich in dezelfde regio bevinden als de Azure Recovery Services-kluis. Virtuele machines in Azure worden gemaakt wanneer failover plaatsvindt.<br/><br/> Afhankelijk van het resourcemodel dat u wilt gebruiken voor Azure VM-failover, kunt u een account instellen met behulp van de [Azure Resource Manager-implementatiemodel](../storage/common/storage-create-storage-account.md) of de [klassieke implementatiemodel](../storage/common/storage-create-storage-account.md).<br/><br/>U kunt [geografisch redundante opslag](../storage/common/storage-redundancy.md#geo-redundant-storage) of lokaal redundante opslag gebruiken. Wij raden geografisch redundante opslag aan. Met geografisch redundante opslag zijn gegevens flexibel te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld.<br/><br/> U kunt een standaard Azure-opslagaccount gebruiken of kunt u Azure Premium-opslag. [Premium-opslag](https://docs.microsoft.com/azure/storage/storage-premium-storage) wordt meestal gebruikt voor virtuele machines die u een consistent hoge i/o-prestaties en lage latentie moeten. Een virtuele machine kunt I/O-intensieve werkbelastingen hosten met premium-opslag. Als u Premium Storage gebruikt voor gerepliceerde gegevens, hebt u ook een standaardopslagaccount nodig. In een standaardopslagaccount worden replicatielogboeken opgeslagen die de doorlopende wijzigingen in de on-premises gegevens vastleggen.<br/><br/>
**Beperkingen voor opslag** | U kunt de storage-accounts die u gebruikt in Site Recovery voor een andere resourcegroep verplaatsen naar of met een ander abonnement gebruiken niet verplaatsen.<br/><br/> Op dit moment kunnen repliceren naar premium storage-accounts in Centraal, India en India Zuid is niet beschikbaar.
**Azure-netwerk** | U moet een Azure-netwerk waarmee virtuele Azure-machines na een failover worden verbonden. Het Azure-netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.<br/><br/> In de Azure-portal kunt u een Azure-netwerk maken met behulp van de [Resource Manager-implementatiemodel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of de [klassieke implementatiemodel](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Als u van System Center Virtual Machine Manager (VMM) naar Azure repliceren, moet u netwerktoewijzing tussen VMM VM-netwerken en Azure-netwerken instellen. Dit zorgt ervoor dat virtuele Azure-machines verbinding met de juiste netwerken na een failover maken.
**Beperkingen op het netwerk** | U kunt de netwerkaccounts die u gebruikt in Site Recovery voor een andere resourcegroep verplaatsen naar of met een ander abonnement gebruiken niet verplaatsen.
**Netwerktoewijzing** | Als u Microsoft Hyper-V-machines in VMM-clouds repliceren, moet u netwerktoewijzing instellen. Dit zorgt ervoor dat virtuele Azure-machines verbinding met de juiste netwerken maken wanneer ze zijn gemaakt na een failover.

> [!NOTE]
> De volgende secties beschrijven de vereisten voor de verschillende onderdelen van de klantomgeving. Zie voor meer informatie over ondersteuning voor specifieke configuraties, de [ondersteuningsmatrix](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-to-azure"></a>Herstel na noodgevallen van virtuele VMware-machines of fysieke Windows of Linux-servers naar Azure
De volgende onderdelen zijn vereist voor herstel na noodgevallen van virtuele VMware-machines of fysieke Windows of Linux-servers. Dit zijn naast die hierboven worden beschreven [Azure-vereisten](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Configuratieserver of extra processerver

Instellen van een lokale machine als de configuratieserver u communicatie tussen de lokale site en Azure indeelt. De onderstaande tabel vertelt over het systeem- en softwarevereisten van een virtuele machine die kunnen worden geconfigureerd als een configuratieserver.

> [!IMPORTANT]
> Bij het implementeren van een configuratie-Server voor het beveiligen van virtuele VMware-machines, raden wij aan dat u als implementeren een **maximaal beschikbare (HA)** virtuele machine.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

### <a name="vmware-vcenter-or-vsphere-host-prerequisites"></a>VMware vCenter of vSphere host-vereisten

| **Onderdeel** | **Vereisten** |
| --- | --- |
| **vSphere** | U moet een of meer VMware vSphere hypervisors hebben.<br/><br/>Hypervisors moet vSphere versie 6.0, 5.5 of 5.1, met de nieuwste updates worden uitgevoerd.<br/><br/>We raden aan dat vSphere-hosts en vCenter-servers in hetzelfde netwerk bevindt als de processerver zijn. Tenzij u een speciale processerver hebt ingesteld, is dit het netwerk waarin de configuratieserver zich bevindt. |
| **vCenter** | Het is raadzaam dat u implementeert een VMware vCenter-server als uw vSphere-hosts wilt beheren. Moet dat het vCenter versie 6.0 of 5.5, worden uitgevoerd met de nieuwste updates.<br/><br/>**Beperking**: Site Recovery biedt geen ondersteuning voor replicatie tussen de exemplaren van vCenter vMotion. DRS-opslag- en Storage vMotion worden ook niet ondersteund op een hoofddoel VM nadat een beveiligt.|

### <a name="replicated-machine-prerequisites"></a>Vereisten voor gerepliceerde machines

| **Onderdeel** | **Vereisten** |
| --- | --- |
| **Lokale machines** (VMware-machines) | Gerepliceerde virtuele machines moet VMware tools geïnstalleerd en uitgevoerd.<br/><br/> Virtuele machines moeten voldoen aan [vereisten voor Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) voor het maken van een virtuele machine in Azure.<br/><br/>Schijfcapaciteit op elke beveiligde computer niet meer dan 1023 GB. <br/><br/>Een minimaal 2 GB aan beschikbare ruimte voor de installatie is vereist voor de installatie van onderdelen.<br/><br/>Als u wilt de consistentie tussen meerdere VM's inschakelen, moet u poort 20004 geopend op de lokale firewall VM.<br/><br/>Namen voor machines moeten liggen tussen 1 en maximaal 63 tekens bevatten (u kunt gebruiken letters, cijfers en afbreekstreepjes). De naam moet beginnen met een letter of cijfer en eindigen met een letter of cijfer. <br/><br/>Nadat u replicatie voor een machine hebt ingeschakeld, kunt u de naam van Azure kunt wijzigen.<br/><br/> |
| **Windows-machines** (fysieke of VMware) | De machine moet een van de volgende ondersteunde 64-bits besturingssystemen worden uitgevoerd: <br/>-Windows Server 2016 (Server Core, Server met Bureaubladbelevenis)<br/>-Windows Server 2012 R2<br/>-Windows Server 2012<br/>-Windows Server 2008 R2 met SP1 of hoger<br/><br/> Het besturingssysteem moet worden geïnstalleerd op station C. De besturingssysteemschijf moet wel een standaardschijf zijn Windows, en niet als dynamische. De gegevensschijf kan niet dynamisch zijn.<br/><br/>|
| **Linux-machines** (fysieke of VMware) | De machine moet een van de volgende ondersteunde 64-bits besturingssystemen worden uitgevoerd: <br/>-Red Hat Enterprise Linux 5.2-5,11 6.1 naar 6,9, 7.0-7.3<br/>-CentOS 5.2-5,11 6.1 naar 6,9, 7.0-7.3<br/>-Server Ubuntu 14.04 TNS (Zie voor een lijst van ondersteunde kernel-versies voor Ubuntu [ondersteunde besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Ubuntu 16.04 TNS server (Zie voor een lijst van ondersteunde kernel-versies voor Ubuntu [ondersteunde besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Debian 7- of Debian 8<br/>-Oracle Enterprise Linux 6.5 of 6.4, met Red Hat-compatibele kernel of Unbreakable Enterprise Kernel versie 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 SP4 of SUSE Linux Enterprise Server 11 SP3<br/><br/>Uw bestanden/etc/hosts op beveiligde machines moeten vermeldingen die de naam van de lokale host aan IP-adressen die zijn gekoppeld aan alle netwerkadapters toewijzen hebben.<br/><br/>Na een failover als u verbinding wilt maken naar een Azure-virtuele machine waarop Linux wordt uitgevoerd met behulp van een client Secure Shell (SSH), zorg ervoor dat de SSH-service op de beveiligde computer is ingesteld op automatisch starten op opstarten van het systeem. Zorg ook dat de firewallregels voor een SSH-verbinding met de beveiligde computer toestaan.<br/><br/>De hostnaam, koppelpunten, apparaatnamen, en Linux-systeempaden en bestandsnamen (bijvoorbeeld /etc/ bevinden en /usr) moeten alleen in het Engels zijn.<br/><br/>De volgende mappen (indien ingesteld als afzonderlijke partities of -bestandssystemen) moet worden op dezelfde schijf (schijf met het besturingssysteem) op de bronserver:<br/>- / (root)<br/>- / opstarten<br/>-/usr<br/>-/ usr/lokaal<br/>-/var<br/>- / enzovoort<br/><br/>XFS v5 functies, zoals metagegevens controlesom, worden momenteel niet ondersteund door Site Recovery op XFS-bestandssystemen. Zorg ervoor dat uw bestandssystemen XFS eventuele v5-functies niet gebruiken. U kunt het hulpprogramma xfs_info gebruiken om te controleren van de superblock XFS voor de partitie. Als **ftype** is ingesteld op **1**, XFS v5 functies worden gebruikt.<br/><br/>Het hulpprogramma lsof moet op Red Hat Enterprise Linux 7 en CentOS 7-servers zijn geïnstalleerd en beschikbaar.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-to-azure-no-vmm"></a>Herstel na noodgevallen van Hyper-V-machines naar Azure (niets VMM)

De volgende onderdelen zijn vereist voor herstel na noodgevallen van Hyper-V-machines in VMM-clouds. Dit zijn naast die hierboven worden beschreven [Azure-vereisten](#azure-requirements).

| **Vereiste** | **Details** |
| --- | --- |
| **Hyper-V-host** |Een of meer lokale servers moeten Windows Server 2012 R2 met de meest recente updates en de Hyper-V-rol is ingeschakeld, of Microsoft Hyper-V Server 2012 R2 worden uitgevoerd.<br/><br/>Hyper-V-servers moeten een of meer virtuele machines hebben.<br/><br/>Hyper-V-servers moeten worden verbonden met Internet, rechtstreeks of via een proxyserver.<br/><br/>Hyper-V-servers moeten beschikken over de oplossingen die zijn beschreven in het Knowledge Base-artikel [2961977](https://support.microsoft.com/kb/2961977) geïnstalleerd.
|**Provider en agent**| Tijdens de implementatie van Site Recovery kunt u de Azure Site Recovery-provider installeren. De installatie van de provider wordt ook de Azure Recovery Services-agent geïnstalleerd op elke Hyper-V-server waarop de virtuele machines die u wilt beveiligen. <br/><br/>Alle Hyper-V-servers in een Site Recovery-kluis moeten overeenkomen met de versies van de provider en de agent hebben.<br/><br/>De provider moet via Internet verbinding maken met Site Recovery. Verkeer kan worden verzonden, rechtstreeks of via een proxy. HTTPS gebaseerde proxy wordt niet ondersteund. De proxyserver moet toegang geven tot de volgende URL's:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Als u IP-adressen gebaseerde firewallregels op de server hebt, zorg ervoor dat de regels communicatie met Azure toestaan.<br/><br/> Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653),- en HTTPS (poort 443).<br/><br/> IP-adresbereiken voor de Azure-regio van uw abonnement en voor het westen van de VS (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure"></a>Herstel na noodgevallen van Hyper-V-machines in VMM-clouds naar Azure

De volgende onderdelen zijn vereist voor herstel na noodgevallen van Hyper-V-machines in VMM-clouds. Dit zijn naast die hierboven worden beschreven [Azure-vereisten](#azure-requirements).

| **Vereiste** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |U moet een of meer VMM-servers met System Center 2012 R2 of hoger hebben. Elke VMM-server moet een of meer clouds zijn geconfigureerd. <br/><br/>Een cloud moet hebben:<br/>-Een of meer VMM-hostgroepen.<br/>-Een of meer Hyper-V-hostservers of -clusters in elke hostgroep.<br/><br/>Zie voor meer informatie over het instellen van VMM-clouds [maken van een cloud in Virtual Machine Manager 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Op Hyper-V-servers moet ten minste Windows Server 2012 R2 met de Hyper-V-rol of Microsoft Hyper-V Server 2012 R2 worden uitgevoerd. De meest recente updates moeten zijn geïnstalleerd.<br/><br/> Een Hyper-V-server moet een of meer virtuele machines hebben.<br/><br/> Een Hyper-V-hostserver of het cluster met virtuele machines die u wilt repliceren, moet worden beheerd in een VMM-cloud.<br/><br/>Hyper-V-servers moeten worden verbonden met Internet, rechtstreeks of via een proxyserver.<br/><br/>Hyper-V-servers moeten beschikken over de oplossingen die zijn beschreven in het Knowledge Base-artikel [2961977](https://support.microsoft.com/kb/2961977) geïnstalleerd.<br/><br/>Hyper-V-hostservers moeten toegang tot Internet voor gegevensreplicatie in Azure. |
| **Provider en agent** |Installeer de Azure Site Recovery Provider op de VMM-server tijdens de implementatie van Azure Site Recovery. Recovery Services-Agent installeren op Hyper-V-hosts. De provider en de agent moeten verbinding maken met Azure rechtstreeks via Internet of via een proxy. Op HTTPS gebaseerde proxy wordt niet ondersteund. De proxyserver op de VMM-server en Hyper-V-hosts moet toegang tot toestaan: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Als u IP-adressen gebaseerde firewallregels op de VMM-server, zorg ervoor dat de regels communicatie met Azure toestaan.<br/><br/> Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) - en HTTPS (poort 443).<br/><br/>IP-adresbereiken voor de Azure-regio voor uw abonnement en voor het westen van de VS (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Vereisten voor gerepliceerde machines

| **Onderdeel** | **Details** |
| --- | --- |
| **Beveiligde virtuele machines** | Ondersteunt alle besturingssystemen die worden ondersteund door site Recovery [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Virtuele machines moeten voldoen aan de [vereisten voor Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) voor het maken van een virtuele machine in Azure. Namen voor machines moeten liggen tussen 1 en maximaal 63 tekens bevatten (u kunt gebruiken letters, cijfers en afbreekstreepjes). De naam moet beginnen met een letter of cijfer en eindigen met een letter of cijfer. <br/><br/>U kunt de naam van de virtuele machine wijzigen nadat u replicatie voor de virtuele machine hebt ingeschakeld. <br/><br/> Schijfcapaciteit voor elke beveiligde computer niet meer dan 1023 GB. Een virtuele machine kan maximaal 16 schijven hebben (maximaal 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-a-customer-owned-site"></a>Herstel na noodgevallen van Hyper-V-machines in VMM-clouds met een site die eigendom zijn van de klant

De volgende onderdelen zijn vereist voor herstel na noodgevallen van Hyper-V-machines in VMM-clouds met een site die eigendom zijn van de klant. Dit zijn naast die hierboven worden beschreven [Azure-vereisten](#azure-requirements).

| **Onderdeel** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |  Het is raadzaam dat u een VMM-server op zowel de primaire site en de secundaire site implementeert.<br/><br/> Als u wilt repliceren tussen clouds op één VMM-server, moet u ten minste twee clouds die zijn geconfigureerd op de VMM-server.<br/><br/> VMM-servers moeten ten minste draaien op System Center 2012 SP1 met de nieuwste updates.<br/><br/> Elke VMM-server moet een of meer clouds hebben. Alle clouds moeten de Hyper-V-Capaciteitsprofiel set hebben. <br/><br/>Clouds moeten een of meer VMM-hostgroepen hebben. Zie voor meer informatie over het instellen van VMM-clouds [voorbereiden voor Azure Site Recovery-implementatie](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Hyper-V-servers moeten ten minste draaien op Windows Server 2012 met de Hyper-V-rol is ingeschakeld en de meest recente updates hebt geïnstalleerd.<br/><br/> Een Hyper-V-server moet een of meer virtuele machines hebben.<br/><br/>  Hyper-V-hostservers moeten zich in hostgroepen in de primaire en secundaire VMM-clouds.<br/><br/> Als u op Windows Server 2012 R2 Hyper-V in een cluster uitvoert, raden wij aan dat u de updates beschreven in Knowledge Base-artikel installeert [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Als u Hyper-V wordt uitgevoerd in een cluster op Windows Server 2012 en een statische IP-adressen gebaseerde cluster hebt, wordt niet automatisch een clusterbroker gemaakt. U moet de clusterbroker handmatig configureren. Zie voor meer informatie over de clusterbroker [configureert de replica broker-functie voor replicatie van cluster naar cluster](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Provider** | Tijdens de implementatie van Site Recovery de Azure Site Recovery-provider te installeren op VMM-servers. De provider communiceert met Site Recovery via HTTPS (poort 443) naar de organisatie van replicatie. Gegevens replicatie tussen de primaire en secundaire Hyper-V-servers via het LAN of via een VPN-verbinding.<br/><br/> De provider die wordt uitgevoerd op de VMM-server heeft toegang tot de volgende URL's:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>De Site Recovery-provider moet toestaan firewall communicatie van de VMM-servers voor de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en het protocol HTTPS (poort 443) toestaan. |


## <a name="url-access"></a>URL-toegang
Deze URL's moeten beschikbaar zijn vanuit VMware, VMM en Hyper-V-hostservers zijn:

|**URL** | **VMM naar VMM** | **VMM naar Azure** | **Hyper-V naar Azure** | **VMware naar Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Toestaan | Toestaan | Toestaan | Toestaan |
|``*.backup.windowsazure.com`` | Niet vereist | Toestaan | Toestaan | Toestaan |
|``*.hypervrecoverymanager.windowsazure.com`` | Toestaan | Toestaan | Toestaan | Toestaan |
|``*.store.core.windows.net`` | Toestaan | Toestaan | Toestaan | Toestaan |
|``*.blob.core.windows.net`` | Niet vereist | Toestaan | Toestaan | Toestaan |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Niet vereist | Niet vereist | Niet vereist | Toestaan dat SQL downloaden |
|``time.windows.com`` | Toestaan | Toestaan | Toestaan | Toestaan|
|``time.nist.gov`` | Toestaan | Toestaan | Toestaan | Toestaan |
