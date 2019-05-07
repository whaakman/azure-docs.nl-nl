---
title: Wat is er nieuw in Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van nieuwe functies in Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: raynew
ms.openlocfilehash: e2145fbbb5fa09aa3321742ca8a786822f6f0641
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148649"
---
# <a name="whats-new-in-site-recovery"></a>Nieuw in Site Recovery

De [Azure Site Recovery](site-recovery-overview.md) service is bijgewerkt en verbeterd regelmatig. Als u wilt, kunt u op de hoogte blijven vindt in dit artikel u informatie over de meest recente versies, nieuwe functies en nieuwe inhoud. Deze pagina wordt regelmatig bijgewerkt.

Als u suggesties voor Site Recovery-onderdelen hebt, willen we graag [graag uw feedback](https://feedback.azure.com/forums/256299-site-recovery).


## <a name="updates-march-2019"></a>Updates (maart 2019)

### <a name="update-rollup-35"></a>Updatepakket 35

[Updatepakket 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)

#### <a name="vmwarephysical-server-disaster-recovery"></a>Herstel na noodgevallen van VMware/fysieke machine
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Beheerde schijven** | Replicatie van on-premises VMware-machines en fysieke servers is nu rechtstreeks naar beheerde schijven in Azure. On-premises gegevens worden verzonden naar een cache-opslagaccount in Azure, en herstelpunten worden gemaakt op beheerde schijven in de doellocatie. Dit zorgt ervoor dat u hoeft te beheren van meerdere doelopslagaccounts.
**Configuratieserver** | Site Recovery biedt nu ondersteuning voor de configuratieservers van een met meerdere NIC's. Voordat u de configuratieserver in de kluis registreert, moet u de extra adapters toevoegen aan de virtuele machine van de configuratieserver. Als u daarna toevoegt, moet u de server in de kluis opnieuw te registreren.


## <a name="updates-february-2019"></a>Updates (februari 2019)

### <a name="update-rollup-34"></a>Updatepakket 34 

[Updatepakket 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).


### <a name="update-rollup-33"></a>Updatepakket 33 

[Updatepakket 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).


#### <a name="azure-vm-disaster-recovery"></a>Azure VM-noodherstel 
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Netwerktoewijzing** | U kunt nu alle beschikbare doelnetwerk gebruiken voor herstel na noodgevallen virtuele Azure-machine, wanneer u replicatie inschakelt. 
**Standard-SSD** | U kunt nu herstel na noodgevallen voor het gebruik van virtuele Azure-machines instellen [Standard-SSD-schijven](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Storage Spaces Direct** | U kunt instellen van herstel na noodgeval voor apps die worden uitgevoerd op Azure VM-apps met behulp van [Storage Spaces Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) voor hoge beschikbaarheid.  Met behulp van Storage Spaces Direct (S2D) samen met Site Recovery biedt uitgebreide beveiliging van Azure VM-workloads. S2D kunt u een gastcluster in Azure hosten. Dit is vooral nuttig wanneer u een virtuele machine als host fungeert voor een essentiële toepassing, zoals SAP ASCS-laag, SQL Server of scale-out bestandsserver.


#### <a name="vmwarephysical-server-disaster-recovery"></a>Herstel na noodgevallen van VMware/fysieke machine
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux BRTFS bestandssysteem** | Site Recovery biedt nu ondersteuning voor replicatie van VMware-VM's met het bestandssysteem BRTFS. Replicatie wordt niet ondersteund als:<br/><br/>-De onderliggende volume voor BTRFS file system is gewijzigd na het inschakelen van replicatie.<br/><br/>-Het bestandssysteem is verspreid over meerdere schijven.<br/><br/>-De bestandssysteem BTRFS biedt ondersteuning voor RAID.
**Windows Server 2019** | Er is ondersteuning toegevoegd voor computers met Windows Server 2019.


## <a name="updates-january-2019"></a>Updates (januari 2019)

### <a name="accelerated-networking-azure-vms"></a>Versneld netwerken (virtuele Azure-machines)

Versneld netwerken inschakelen één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, verbetering van netwerkprestaties. Wanneer u replicatie voor een Azure-VM inschakelt, wordt met Site Recovery detecteert of versneld netwerken is ingeschakeld. Als deze is, na failover automatisch geconfigureerd door Site Recovery netwerken op de doelreplica virtuele machine van Azure, voor zowel versnelde [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) en [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

[Meer informatie](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="update-rollup-32"></a>Updatepakket 32 

[Updatepakket 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).

#### <a name="azure-vm-disaster-recovery"></a>Azure VM-noodherstel

Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Ondersteuning is toegevoegd voor Red Hat-werkstation 6, 7 en nieuwe versies van de kernel voor Ubuntu, Debian en SUSE.
**Storage Spaces Direct** | Site Recovery biedt ondersteuning voor virtuele Azure-machines met behulp van Storage Spaces Direct (S2D).

#### <a name="vmware-vmsphysical-servers-replication"></a>Replicatie van VMware-machines/fysieke servers 
**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Ondersteuning is toegevoegd voor Red Hat Enterprise Linux 7.6, RedHat werkstation 6, 7, Oracle Linux 6.10/7.6 en nieuwe versies van de kernel voor Ubuntu, Debian en SUSE.


### <a name="update-rollup-31"></a>Updatepakket 31 

[Updatepakket 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).

#### <a name="vmware-vmsphysical-servers-replication"></a>Replicatie van VMware-machines/fysieke servers 
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Er is ondersteuning toegevoegd voor Oracle Linux 6,8 en 6,9/7.0, en voor de kernel UEK5.
**LVM** | Er is ondersteuning toegevoegd voor LVM en LVM2-volumes.<br/><br/> De map bevinden op een partitie op schijf en LVM volumes wordt nu ondersteund.
**Adreslijsten** | Er is ondersteuning toegevoegd voor deze mappen instellen als afzonderlijke partities of bestandssystemen die niet op dezelfde systeemschijf als:<br/><br/> / (hoofd), bevinden, / usr, / usr/local, /var, / etc.
**Windows Server 2008** | Er is ondersteuning toegevoegd voor dynamische schijven.
**Failover** | Failover-tijd voor virtuele VMware-machines waar storvsc en vsbus zijn niet opstartstuurprogramma's verbeterd.
**Ondersteuning voor UEFI** | Azure-VM's bieden geen ondersteuning voor opstarttype UEFI. U kunt nu on-premises fysieke servers met UEFI migreren naar Azure met Site Recovery. Site Recovery worden gemigreerd van de server door het opstarttype converteren naar BIOS vóór de migratie. Site Recovery eerder ondersteund deze conversie een hoeveelheid alleen voor virtuele machines. Ondersteuning is beschikbaar voor fysieke servers met Windows Server 2012 of hoger.

#### <a name="azure-vm-disaster-recovery"></a>Azure VM-noodherstel
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Ondersteund voor Oracle Linux 6,8 en 6,9/7.0; is toegevoegd en voor de kernel UEK5.
**Linux BRTFS bestandssysteem** | Ondersteund voor virtuele machines in Azure.
**Azure VM's in beschikbaarheidszones** | U kunt replicatie naar een andere regio voor Azure VM's die zijn geïmplementeerd in beschikbaarheidszones inschakelen. U kunt nu schakelt replicatie voor een Azure-VM en het doel van failover ingesteld op één VM-instantie, een virtuele machine in een beschikbaarheidsset of een virtuele machine in een beschikbaarheidszone. De instelling niet van invloed zijn op replicatie. [Lezen](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) de aankondiging.
**Firewall ingeschakeld opslag (portal/PowerShell)** | Er is ondersteuning toegevoegd voor [firewall ingeschakeld opslagaccounts](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> U kunt virtuele Azure-machines repliceren met niet-beheerde schijven op de firewall ingeschakeld storage-accounts naar een andere Azure-regio voor herstel na noodgevallen.<br/><br/> U kunt firewall ingeschakeld storage-accounts gebruiken als doelopslagaccounts voor niet-beheerde schijven.<br/><br/> Ondersteunde in portal en met behulp van PowerShell.

## <a name="updates-december-2018"></a>Updates (December 2018)

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>Automatische updates voor de Mobility-service (Azure VM's)

Site Recovery wordt een optie voor automatische updates toegevoegd aan de Mobility-service-extensie. De Mobility-service-extensie is geïnstalleerd op elke Azure-VM gerepliceerd door Site Recovery. Wanneer u replicatie inschakelt, selecteert u of Site Recovery voor het beheren van updates voor de extensie is toegestaan.

Updates geen virtuele machine opnieuw opstarten is vereist, en hebben geen invloed op de replicatie. [Meer informatie](azure-to-azure-autoupdate.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Prijscalculator voor noodherstel van Azure-VM

Disaster Recovery van de Azure VM's worden licentiekosten van de virtuele machine en de netwerk- en kosten in rekening gebracht. Azure biedt een [prijscalculator](https://aka.ms/a2a-cost-estimator) kunt u deze kosten te achterhalen. Site Recovery biedt nu een [voorbeeldprijzen schatting](https://aka.ms/a2a-cost-estimator) dat prijzen voor de implementatie van een steekproef op basis van een app met drie-lagen met zes VM's met 12 standaard harde schijven en 6 Premium-SSD-schijven.

- Het voorbeeld wordt ervan uitgegaan dat een gegevenswijziging van 10 GB per dag voor standard en 20 GB voor premium.
- U kunt de variabelen om een schatting van de kosten te wijzigen voor uw specifieke implementatie.
- Kunt u het aantal virtuele machines, het aantal en type van beheerde schijven en de verwachte gegevens van de totale snelheid van verwacht voor de virtuele machines wijzigen.
- U kunt bovendien een factor compressie voor een schatting van de bandbreedtekosten toepassen.

[Lezen](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) de aankondiging.


## <a name="updates-october-2018"></a>Updates (oktober 2018)

### <a name="update-rollup-30"></a>Updatepakket 30 

[Updatepakket 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).

#### <a name="azure-vm-disaster-recovery"></a>Azure VM-noodherstel
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Ondersteuning voor regio** | Site Recovery is ondersteuning toegevoegd voor Australië centraal 1 en Australië centraal 2.
**Ondersteuning voor schijfversleuteling** | Er is ondersteuning toegevoegd voor herstel na noodgevallen van virtuele Azure-machines met Azure Disk Encryption (ADE) versleuteld met de Azure AD-app. [Meer informatie](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Schijf uitsluiten** | Niet-geïnitialiseerde schijven worden nu automatisch uitgesloten tijdens de replicatie van virtuele Azure-machine.
**Firewall ingeschakeld opslag (PowerShell)** | Er is ondersteuning toegevoegd voor [firewall ingeschakeld opslagaccounts](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> U kunt virtuele Azure-machines repliceren met niet-beheerde schijven op de firewall ingeschakeld storage-accounts naar een andere Azure-regio voor herstel na noodgevallen.<br/><br/> U kunt firewall ingeschakeld storage-accounts gebruiken als doelopslagaccounts voor niet-beheerde schijven.<br/><br/> Met behulp van PowerShell alleen wordt ondersteund.


### <a name="update-rollup-29"></a>Updatepakket 29 

[Updatepakket 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).


## <a name="updates-august-2018"></a>Updates (augustus 2018)

### <a name="update-rollup-28"></a>Updatepakket 28 

[Updatepakket 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).

#### <a name="azure-vms-disaster-recovery"></a>Noodherstel voor Azure VM 's 
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Added supported for RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/>
**Cloudondersteuning voor** | Herstel na noodgevallen voor Azure-VM's in de cloud voor Duitsland ondersteund.
**Herstel na noodgevallen voor abonnementen** | Ondersteuning voor het repliceren van virtuele Azure-machines in één regio naar een andere regio in een ander abonnement, binnen dezelfde Azure Active Directory-tenant. [Meer informatie](https://aka.ms/cross-sub-blog).

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>Herstel na noodgevallen van virtuele VMware-machine/fysieke machine 
Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Er is ondersteuning toegevoegd voor Red Hat Enterprise Linux 6.10, CentOS 6.10.<br/><br/> Op basis van Linux-VM's gebruiken de partitiestijl GUID partitietabel (GPT) in de verouderde BIOS-compatibiliteitsmodus worden nu ondersteund. Controleer de [Veelgestelde vragen over Azure-VM](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) voor meer informatie. 
**Herstel na noodgevallen voor virtuele machines na migratie** | Ondersteuning voor herstel na noodgevallen naar een secundaire regio inschakelen voor een on-premises virtuele VMware-machine gemigreerd naar Azure, zonder te verwijderen van de Mobility-service op de virtuele machine voordat u replicatie inschakelt.
**Windows Server 2008** | Ondersteuning voor het migreren van machines actieve Windows Server 2008 R2-2008 64-bits en 32-bits.<br/><br/> De migratie alleen (replicatie en failover). Failback wordt niet ondersteund.

## <a name="updates-july-2018"></a>Updates (juli 2018)

### <a name="update-rollup-27-july-2018"></a>Updatepakket 27 (juli 2018)

[Updatepakket 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het updatepakket).
**Probleem opgelost/verbeteringen** | Een aantal problemen en verbeterde (zoals beschreven in het updatepakket).

#### <a name="azure-vms-disaster-recovery"></a>Noodherstel voor Azure VM 's 

Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Er is ondersteuning toegevoegd voor Red Hat Enterprise Linux 7.5.

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>Herstel na noodgevallen van virtuele VMware-machine/fysieke machine 

Nieuwe functies toegevoegd in de update.

**Functie** | **Details**
--- | ---
**Linux-ondersteuning** | Er is ondersteuning toegevoegd voor Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12.



## <a name="next-steps"></a>Volgende stappen

Up-to-date houden met onze updates op de [Azure Updates](https://azure.microsoft.com/updates/?product=site-recovery) pagina.
