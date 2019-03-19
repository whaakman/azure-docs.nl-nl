---
title: Wat is er nieuw in Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van nieuwe functies in Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: raynew
ms.openlocfilehash: fece1adc9ad2f5844a7d6cc1e0e9cc92d44de6d8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892928"
---
# <a name="whats-new-in-site-recovery"></a>Nieuw in Site Recovery

De [Azure Site Recovery](site-recovery-overview.md) service is bijgewerkt en verbeterd regelmatig. Als u wilt, kunt u op de hoogte blijven vindt in dit artikel u informatie over de meest recente versies, nieuwe functies en nieuwe inhoud. Deze pagina wordt regelmatig bijgewerkt.

Als u suggesties voor Site Recovery-onderdelen hebt, willen we graag [graag uw feedback](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>Q1 2019 

### <a name="update-rollup-34-february-2019"></a>Updatepakket 34 (februari 2019)

[Updatepakket 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)



### <a name="update-rollup-33-february-2019"></a>Updatepakket 33 (februari 2019)

[Updatepakket 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)
**Netwerktoewijzing** | U kunt nu alle beschikbare doelnetwerk gebruiken voor herstel na noodgevallen virtuele Azure-machine, wanneer u replicatie inschakelt. 
**Standard-SSD** | U kunt nu herstel na noodgevallen voor het gebruik van virtuele Azure-machines instellen [Standard-SSD-schijven](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Storage Spaces Direct** | U kunt instellen van herstel na noodgeval voor apps die worden uitgevoerd op Azure VM-apps met behulp van [Storage Spaces Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) voor hoge beschikbaarheid.
**BRTFS bestandssysteem** | Ondersteund voor VMware-VM's, naast de Azure-VM's.<br/><br/> Niet ondersteund als: De onderliggende volume voor BTRFS file system is gewijzigd na het inschakelen van replicatie, het bestandssysteem is verspreid over meerdere schijven, of als de BTRFS bestandssysteem ondersteunt RAID.



### <a name="update-rollup-32-january-2019"></a>Updatepakket 32 (januari 2019)

[Updatepakket 31](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)
**Herstel na noodgevallen voor Linux** | **Azure-VM's**: Red Hat werkstation 6/7; ondersteuning voor nieuwe versies van de kernel voor Ubuntu, Debian en SUSE.<br/><br/> **VMware-machines/fysieke servers**: Red Hat Enterprise Linux 7.6; Red Hat werkstation 6/7; Oracle Linux 6.10/7.6; worden ondersteund voor nieuwe versies van de kernel voor Ubuntu, Debian en SUSE.


### <a name="update-rollup-31-january-2019"></a>Updatepakket 31 (januari 2019)

[Updatepakket 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)
**Herstel na noodgevallen voor Linux** | **Azure-VM's**: Oracle Linux 6,8 en 6,9/7.0; ondersteuning voor UEK5 kernels.<br/><br/> **VMware-machines/fysieke servers**: Oracle Linux 6,8 en 6,9/7.0; ondersteuning voor UEK5 kernel.
**BRTFS bestandssysteem** | Ondersteund voor virtuele machines in Azure.
**LVM** | Er is ondersteuning toegevoegd voor LVM en LVM2-volumes.<br/><br/> De map bevinden op een partitie op schijf en LVM volumes wordt ondersteund.
**Adreslijsten** | Er is ondersteuning toegevoegd voor deze mappen seet als afzonderlijke partities of bestandssystemen die niet op dezelfde systeemschijf als: / (hoofd), bevinden, / usr, / usr/local, /var, / etc.
**Windows Server 2008** | Er is ondersteuning toegevoegd voor dynamische schijven.
**VMware-VM-failover** | Failover-tijd voor virtuele VMware-machines waar storvsc en vsbus zijn niet opstartstuurprogramma's verbeterd.
**Ondersteuning voor UEFI** | Azure-VM's bieden geen ondersteuning voor opstarttype UEFI. U kunt nu on-premises fysieke servers met UEFI migreren naar Azure met Site Recovery. Site Recovery worden gemigreerd van de server door het opstarttype converteren naar BIOS vóór de migratie. Site Recovery eerder ondersteund deze conversie een hoeveelheid alleen voor virtuele machines. Ondersteuning is beschikbaar voor fysieke servers met Windows Server 2012 of hoger.
**Azure VM's in beschikbaarheidszones** | U kunt replicatie naar een andere regio voor Azure VM's die zijn geïmplementeerd in beschikbaarheidszones inschakelen. organisatie-eenheid kan nu replicatie inschakelen voor een Azure-VM en het doel van failover ingesteld op één VM-instantie, een virtuele machine in een beschikbaarheidsset of een virtuele machine in een beschikbaarheidszone. De instelling niet van invloed zijn op replicatie. [Lezen](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) de aankondiging.


## <a name="q4-2018"></a>K4 2018

### <a name="update-rollup-30-october-2018"></a>Updatepakket 30 (oktober 2018)

[Updatepakket 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)
**Ondersteuning voor regio** | Site Recovery is ondersteuning toegevoegd voor Australië centraal 1 en Australië centraal 2.
**Ondersteuning voor schijfversleuteling** | Er is ondersteuning toegevoegd voor herstel na noodgevallen van virtuele Azure-machines met Azure Disk Encryption (ADE) versleuteld met de Azure AD-app. [Meer informatie](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Schijf uitsluiten** | Er niet-geïnitialiseerd schijven worden nu automatisch uitgesloten tijdens de replicatie van virtuele Azure-machine.
**Opslag firewall is ingeschakeld** | Er is ondersteuning toegevoegd voor [firewall ingeschakeld opslagaccounts](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> U kunt virtuele Azure-machines repliceren met niet-beheerde schijven op de firewall ingeschakeld storage-accounts naar een andere Azure-regio voor herstel na noodgevallen.<br/><br/> U kunt firewall ingeschakeld storage-accounts gebruiken als doelopslagaccounts voor niet-beheerde schijven.<br/><br/> Met behulp van PowerShell alleen wordt ondersteund.


### <a name="update-rollup-29-october-2018"></a>Updatepakket 29 (oktober 2018)

[Updatepakket 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Probleem opgelost** | Een aantal problemen en verbeterde (zoals beschreven in het pakket)

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Automatische updates voor de Mobility-service-extensie

Site Recovery wordt een optie voor automatische updates toegevoegd aan de Mobility-service-extensie. De Mobility-service-extensie is geïnstalleerd op elke Azure-VM gerepliceerd door Site Recovery. Wanneer u replicatie inschakelt, selecteert u of Site Recovery voor het beheren van updates voor de extensie is toegestaan. Updates geen virtuele machine opnieuw opstarten is vereist, en hebben geen invloed op de replicatie. [Meer informatie](azure-to-azure-autoupdate.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Herstel na noodgeval voor virtuele machines met versnelde netwerken

Versneld netwerken inschakelen één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, verbetering van netwerkprestaties. Wanneer u replicatie voor een Azure-VM inschakelt, wordt met Site Recovery detecteert of versneld netwerken is ingeschakeld. Als deze is, na failover automatisch geconfigureerd door Site Recovery netwerken op de doelreplica virtuele machine van Azure, voor zowel versnelde [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) en [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Meer informatie](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Prijscalculator voor noodherstel van Azure-VM

Herstel na noodgevallen voor Azure-VM's worden licentiekosten voor virtuele machine en de netwerk- en kosten in rekening gebracht. Azure biedt een [prijscalculator](https://aka.ms/a2a-cost-estimator) kunt u deze kosten te achterhalen. Site Recovery biedt nu een [voorbeeldprijzen schatting](https://aka.ms/a2a-cost-estimator) dat prijzen voor de implementatie van een steekproef op basis van een app met drie-lagen met zes VM's met 12 standaard harde schijven en 6 Premium-SSD-schijven. Het voorbeeld wordt ervan uitgegaan dat een gegevenswijziging van 10 GB per dag voor standard en 20 GB voor premium. U kunt de variabelen om een schatting van de kosten te wijzigen voor uw specifieke implementatie. Kunt u het aantal virtuele machines, het aantal en type van beheerde schijven en de verwachte gegevens van de totale snelheid van verwacht voor de virtuele machines wijzigen. U kunt bovendien een factor compressie voor een schatting van de bandbreedtekosten toepassen. [Lezen](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) de aankondiging.



## <a name="q3-2018"></a>Q3 2018 


### <a name="update-rollup-28-august-2018"></a>Updatepakket 28 (augustus 2018)

[Updatepakket 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Herstel na noodgevallen voor Linux** | **Azure-VM's**: Added supported for RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> **Virtuele VMware-machines**: RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> Op basis van Linux-VM's gebruiken de partitiestijl GUID partitietabel (GPT) in de verouderde BIOS-compatibiliteitsmodus worden nu ondersteund.
**Cloudondersteuning voor** | Herstel na noodgevallen voor Azure-VM's in de cloud voor Duitsland ondersteund.
**Herstel na noodgevallen voor abonnementen** | Ondersteuning voor het repliceren van virtuele Azure-machines in één regio naar een andere regio in een ander abonnement, binnen dezelfde Azure Active Directory-tenant. [Meer informatie](https://aka.ms/cross-sub-blog).
**Windows Server 2008** | Ondersteuning voor het migreren van machines actieve Windows Server 2008 R2-2008 64-bits en 32-bits.<br/><br/> De migratie alleen (replicatie en failover). Failback wordt niet ondersteund.

### <a name="update-rollup-27-july-2018"></a>Updatepakket 27 (juli 2018)

[Updatepakket 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) biedt de volgende updates.

**Update** | **Details**
--- | ---
**Providers en agents** | Een update voor de Site-herstelagenten en providers (zoals beschreven in het pakket)
**Herstel na noodgevallen voor Linux** | **Azure-VM's**: Red Hat Enterprise Linux 7.5<br/><br/> **VMware-machines/fysieke servers**: Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12




## <a name="next-steps"></a>Volgende stappen

Up-to-date houden met onze updates op de [Azure Updates](https://azure.microsoft.com/updates/?product=site-recovery) pagina.




 









