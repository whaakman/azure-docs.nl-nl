---
title: Wat is er nieuw in Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van nieuwe functies in Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: raynew
ms.openlocfilehash: 61e66a19b625141c69a9473373d3d5d808e18fde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211114"
---
# <a name="whats-new-in-site-recovery"></a>Nieuw in Site Recovery

De [Azure Site Recovery](site-recovery-overview.md) service is bijgewerkt en verbeterd regelmatig. Als u wilt, kunt u op de hoogte blijven vindt in dit artikel u informatie over de meest recente versies, nieuwe functies en nieuwe inhoud. Deze pagina wordt regelmatig bijgewerkt.

Als u suggesties voor Site Recovery-onderdelen hebt, willen we graag [graag uw feedback](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>Q1 2019

### <a name="linux-support"></a>Linux Support

[Updatepakket 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) biedt u een update voor Site-herstelagenten en leveranciers. De updates voegt ondersteuning toe voor Linux als volgt:

- **Herstel na noodgeval voor virtuele Azure-machines**: Red Hat werkstation 6/7; nieuwe kernelversies voor Ubuntu, Debian en SUSE.
- **Herstel na noodgeval voor VMware-machines/fysieke servers naar Azure**: Red Hat Enterprise Linux 7.6; Red Hat werkstation 6/7; Oracle Linux 6.10/7.6 nieuwe kernelversies Ubuntu, Debian en SUSE.



## <a name="q4-2018"></a>K4 2018

## <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Prijscalculator voor noodherstel van Azure-VM

Herstel na noodgevallen voor Azure-VM's worden licentiekosten voor virtuele machine en de netwerk- en kosten in rekening gebracht. Azure biedt een [prijscalculator](https://aka.ms/a2a-cost-estimator) kunt u deze kosten te achterhalen. Site Recovery biedt nu een [voorbeeldprijzen schatting](https://aka.ms/a2a-cost-estimator) dat prijzen voor de implementatie van een steekproef op basis van een app met drie-lagen met zes VM's met 12 standaard harde schijven en 6 Premium-SSD-schijven. Het voorbeeld wordt ervan uitgegaan dat een gegevenswijziging van 10 GB per dag voor standard en 20 GB voor premium. U kunt de variabelen om een schatting van de kosten te wijzigen voor uw specifieke implementatie. Kunt u het aantal virtuele machines, het aantal en type van beheerde schijven en de verwachte gegevens van de totale snelheid van verwacht voor de virtuele machines wijzigen. U kunt bovendien een factor compressie voor een schatting van de bandbreedtekosten toepassen. [Lezen](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) de aankondiging.

### <a name="support-for-azure-vms-in-zones"></a>Ondersteuning voor Azure VM's in zones

Azure-beschikbaarheidszones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. U kunt nu schakelt replicatie voor een Azure-VM en het doel van failover ingesteld op één VM-instantie, een virtuele machine in een beschikbaarheidsset of een virtuele machine in een beschikbaarheidszone. De instelling niet van invloed zijn op replicatie. [Lezen](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) de aankondiging.
 
### <a name="disaster-recovery-for-encrypted-vms"></a>Herstel na noodgevallen voor versleutelde virtuele machines

Site Recovery biedt ondersteuning voor virtuele Azure-machines met Azure Disk Encryption (ADE) versleuteld met de Azure AD-app. [Meer informatie](azure-to-azure-how-to-enable-replication-ade-vms.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Herstel na noodgeval voor virtuele machines met versnelde netwerken

Versneld netwerken inschakelen één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, verbetering van netwerkprestaties. Wanneer u replicatie voor een Azure-VM inschakelt, wordt met Site Recovery detecteert of versneld netwerken is ingeschakeld. Als deze is, na failover automatisch geconfigureerd door Site Recovery netwerken op de doelreplica virtuele machine van Azure, voor zowel versnelde [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) en [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Meer informatie](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Automatische updates voor de Mobility-service-extensie

Site Recovery wordt een optie voor automatische updates toegevoegd aan de Mobility-service-extensie. De Mobility-service-extensie is geïnstalleerd op elke Azure-VM gerepliceerd door Site Recovery. Wanneer u replicatie inschakelt, selecteert u of Site Recovery voor het beheren van updates voor de extensie is toegestaan. Updates geen virtuele machine opnieuw opstarten is vereist, en hebben geen invloed op de replicatie. [Meer informatie](azure-to-azure-autoupdate.md).

### <a name="support-for-standard-ssd-disks"></a>Ondersteuning voor standard-SSD-schijven

Azure geïntroduceerd [standaard stations SSD (Solid State)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd) schijven bieden een kosteneffectieve oplossing voor apps, zoals webservers die consistente prestaties nodig, maar geen hoge schijf-IOPS. Deze combineren elementen van premium SSD en HDD standaardschijven. Site Recovery biedt herstel na noodgevallen voor Azure-VM's met behulp van de Standard-SSD-schijf. Standaard wordt het schijftype behouden na een failover naar de doelregio.

### <a name="support-for-azure-storage-firewall"></a>Ondersteuning voor de firewall van Azure storage

U kunt Azure storage-accounts in een specifieke set netwerken beveiligen door het inschakelen van firewall-regels voor het account. U opslagaccounts voor het weigeren van verkeer van interne netwerken en het internet standaard configureren en vervolgens toegang verleend aan verkeer vanuit specifieke VNets. Site Recovery biedt ondersteuning voor replicatie voor virtuele machines met niet-beheerde schijven op de firewall ingeschakeld storage-accounts naar een secundaire regio. In de doelregio voor niet-beheerde schijven, kunt u storage-accounts met een firewall is ingeschakeld. U kunt ook toegang tot het cacheopslagaccount beperken door het beperken van toegang tot het netwerk met alleen netwerk waarin de bron-VM's zich bevinden. Houd er rekening mee dat u moet [toegang toestaan](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) voor vertrouwde Microsoft-services.

## <a name="q3-2018"></a>Q3 2018 

### <a name="linux-support"></a>Linux Support

#### <a name="update-rollup-28"></a>Updatepakket 28

[Updatepakket 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) biedt u een update voor Site-herstelagenten en leveranciers. De updates voegt ondersteuning toe voor Linux als volgt:

- **Herstel na noodgeval voor virtuele Azure-machines**: Red Hat Enterprise Linux 6.10; CentOS 6.10; Op basis van Linux-VM's gebruiken de partitiestijl GUID partitietabel (GPT) in de verouderde BIOS-compatibiliteitsmodus worden nu ondersteund.
- **Herstel na noodgeval voor VMware-machines/fysieke servers naar Azure**: Red Hat Enterprise Linux 6.10; CentOS 6.10; Op basis van Linux-VM's gebruiken de partitiestijl GUID partitietabel (GPT) in de verouderde BIOS-compatibiliteitsmodus worden nu ondersteund.

#### <a name="update-rollup-27"></a>Updatepakket 27

[Updatepakket 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) biedt u een update voor Site-herstelagenten en leveranciers. De updates voegt ondersteuning toe voor Linux als volgt:

- **Herstel na noodgeval voor virtuele Azure-machines**: Red Hat Enterprise Linux 7.5
- **Herstel na noodgeval voor VMware-machines/fysieke servers naar Azure**: SUSE Linux Enterprise Server 12, Red Hat Enterprise Linux 7.5

### <a name="support-for-azure-vms-running-on-windows-server-2016"></a>Ondersteuning voor Azure VM's die worden uitgevoerd op Windows Server 2016

Azure VM's die worden uitgevoerd op Windows Server 2016 kunnen Azure-regio's met Azure Site Recovery worden gerepliceerd.

### <a name="support-for-azure-vms-running-storage-spaces-direct"></a>Ondersteuning voor Azure VM's met Storage Spaces Direct

[Storage Spaces Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) (beschikbaar via Windows Server 2016 en hoger) stations gegroepeerd in een opslaggroep en gebruikt u vervolgens capaciteit van de groep opslagruimten te maken. Opslagruimten kunnen worden gebruikt op een zelfstandige virtuele machine op een [gastcluster van virtuele Azure-machines](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) met lokale opslag op elk clusterknooppunt of gedeelde opslag in het cluster.

## <a name="next-steps"></a>Volgende stappen

Up-to-date houden met onze updates op de [Azure Updates](https://azure.microsoft.com/updates/?product=site-recovery) pagina.


