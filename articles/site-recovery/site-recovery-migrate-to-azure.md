---
title: Migreren naar Azure met Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de migratie van VM&quot;s en fysieke servers naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: f82634af931a1e9a9646c5631ebd0e5923a0adcc
ms.openlocfilehash: cbb6de4587871c40c9d4e97c9fb2a88eab4945a6


---
# <a name="migrate-to-azure-with-site-recovery"></a>Migreren naar Azure met Site Recovery?

Lees dit artikel voor een overzicht van hoe u de service Azure Site Recovery gebruikt om virtuele machines en fysieke servers te migreren.

Organisaties hebben een BCDR-strategie nodig die bepaalt op welke wijze toepassingen, workloads en gegevens beschikbaar blijven tijdens geplande en ongeplande uitval, en ervoor zorgt dat deze zo snel mogelijk worden hersteld naar een normale manier van functioneren. In uw BCDR-strategie moeten bedrijfsgegevens veilig en herstelbaar blijven. Ook moet ervoor worden gezorgd dat workloads continu beschikbaar blijven wanneer zich een noodsituatie voordoet.

Site Recovery is een Azure-service die bijdraagt aan uw BCDR-strategie door replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert. Meer informatie vindt u in [Wat is Site Recovery?](site-recovery-overview.md)

Dit artikel beschrijft de implementatie in [Azure Portal](https://portal.azure.com). De [klassieke Azure-portal](https://manage.windowsazure.com/) kan worden gebruikt voor het onderhoud van de bestaande Site Recovery-kluizen. U kunt geen nieuwe kluizen maken.

U kunt onder aan dit artikel eventuele opmerkingen plaatsen. Technische vragen kunt u stellen op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Wat wordt precies bedoeld met 'migreren’?

U kunt Site Recovery implementeren voor volledige replicatie van on-premises VM's en fysieke servers, naar Azure of een secundaire site. U repliceert machines, voert een failover uit vanaf de primaire site wanneer er een storing is en voert weer een failback naar de primaire site uit wanneer de storing is hersteld. Behalve voor een volledige replicatie kunt u Site Recovery ook gebruiken om VM's en fysieke servers te migreren naar Azure, zodat gebruikers vanaf Azure VM's toegang hebben tot de workload. Migratie brengt replicatie evenals failover van de primaire site naar Azure met zich mee. In tegenstelling tot volledige replicatie omvat migratie echter geen mechanisme voor failback.

## <a name="what-can-site-recovery-migrate"></a>Wat kan er met Site Recovery worden gemigreerd?

U kunt:

- Migreer workloads die worden uitgevoerd op on-premises virtuele Hyper-V-machines, virtuele VMware-machines en fysieke servers, zodat ze worden uitgevoerd op virtuele Azure-machines. U kunt in dit scenario ook een volledige replicatie en failback uitvoeren.
- [Azure IaaS-VM's](site-recovery-migrate-azure-to-azure.md) migreren tussen Azure-regio's. Momenteel wordt in dit scenario alleen migratie ondersteund, dus geen failback.
- Migreer [AWS Windows-exemplaren](site-recovery-migrate-aws-to-azure.md) naar Azure IaaS-VM's. Momenteel wordt in dit scenario alleen migratie ondersteund, dus geen failback.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Virtuele machines en fysieke servers on-premises migreren

Als u on-premises virtuele Hyper-V-machines, virtuele VMware-machines en fysieke servers wilt migreren, volgt u bijna dezelfde stappen als bij normale replicatie. U stelt een Recovery Services-kluis in, configureert de vereiste beheerservers (afhankelijk van wat u wilt migreren), voegt deze toe aan de kluis en geeft replicatie-instellingen op. U schakelt replicatie in voor de machines die u wilt migreren en voert een snelle testfailover uit om te controleren of alles naar behoren werkt.

Nadat u hebt gecontroleerd of de replicatieomgeving werkt, gebruikt u een geplande of niet-geplande failover, afhankelijk van [wat er wordt ondersteund](site-recovery-failover.md#failover-and-failback) voor uw scenario. Voor migratie hoeft u geen failover door te voeren of iets te verwijderen. In plaats daarvan selecteert u de optie **Volledige migratie** voor elke machine die u wilt migreren. Met de actie **Volledige migratie** voltooit u het migratieproces, verwijdert u de replicatie voor de machine en zet u de facturering van Site Recovery voor de machine stop.

## <a name="migrate-between-azure-regions"></a>Migreren tussen Azure-regio's

Met Site Recovery kunt u virtuele Azure-machines migreren tussen gebieden. In dit scenario wordt alleen migratie ondersteund. Met andere woorden, u kunt de Azure-VM's repliceren en een failover naar een andere regio uitvoeren, maar u kunt geen failback uitvoeren. In dit scenario stelt u een Recovery Services-kluis in en implementeert u een on-premises configuratieserver om de replicatie te beheren. Daarna voegt u deze toe aan de kluis en geeft u replicatie-instellingen op. U schakelt replicatie in voor de machines die u wilt migreren en voert een snelle testfailover uit. Daarna voert u een niet-geplande failover uit met de optie **Volledige migratie**.

## <a name="migrate-aws-to-azure"></a>AWS migreren naar Azure

U kunt AWS-exemplaren naar Azure-VM's migreren. In dit scenario wordt alleen migratie ondersteund. Met andere woorden, u kunt de Azure-exemplaren repliceren en een failover naar Azure uitvoeren, maar u kunt geen failback uitvoeren. Voor migratiedoeleinden worden AWS-exemplaren op dezelfde manier afgehandeld als fysieke servers. U stelt een Recovery Services-kluis in en implementeert een on-premises configuratieserver om de replicatie te beheren. Daarna voegt u deze toe aan de kluis en geeft u replicatie-instellingen op. U schakelt replicatie in voor de machines die u wilt migreren en voert een snelle testfailover uit. Daarna voert u een niet-geplande failover uit met de optie **Volledige migratie**.




## <a name="next-steps"></a>Volgende stappen

- [Virtuele VMware-machines migreren naar Azure](site-recovery-vmware-to-azure.md)
- [Fysieke servers migreren naar Azure](site-recovery-vmware-to-azure.md)
- [Virtuele Hyper-V-machines in VMM-clouds migreren naar Azure](site-recovery-vmm-to-azure.md)
- [Virtuele Hyper-V-machines zonder VMM migreren naar Azure](site-recovery-hyper-v-site-to-azure.md)
- [Virtuele Azure-machines migreren tussen Azure-regio's](site-recovery-migrate-azure-to-azure.md)
- [AWS-exemplaren migreren naar Azure](site-recovery-migrate-aws-to-azure.md)



<!--HONumber=Jan17_HO4-->


