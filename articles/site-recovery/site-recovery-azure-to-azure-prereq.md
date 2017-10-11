---
title: Vereisten voor replicatie naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van vereisten voor virtuele machines en fysieke machines repliceren naar Azure met behulp van de Azure Site Recovery-service.
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
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: fb5b8c9ac96ac44d0112919664a177f33ef392da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-to-another-region-by-using-azure-site-recovery"></a>Vereisten voor het virtuele Azure-machines repliceren naar een andere regio met behulp van Azure Site Recovery

> [!div class="op_single_selector"]
> * [Van Azure repliceren naar Azure](site-recovery-azure-to-azure-prereq.md)
> * [On-premises repliceren naar Azure](site-recovery-prereq.md)

De Azure Site Recovery-service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door te organiseren:
* Replicatie van Azure virtuele machines naar een andere Azure-regio.
* Replicatie van fysieke on-premises servers en virtuele machines in Azure of naar een secundair datacenter. 

Wanneer er storingen optreden op uw primaire locatie, kunt u failover naar een secundaire locatie om apps en workloads beschikbaar te houden. U kunt niet terug naar uw primaire locatie wanneer deze weer normaal functioneert. Zie voor meer informatie over Site Recovery [wat is Site Recovery?](site-recovery-overview.md).

In dit artikel bevat een overzicht van de vereiste onderdelen om te beginnen met Site Recovery de replicatie van on-premises naar Azure.

Na de eventuele opmerkingen aan de onderkant van het artikel of technische vragen over de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Azure-vereisten

**Vereiste** | **Details**
--- | ---
**Azure-account** | Een [Microsoft Azure](http://azure.microsoft.com/) account.<br/><br/> U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
**Site Recovery-service** | Zie voor meer informatie over prijzen voor Site Recovery [prijzen voor Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). U wordt aangeraden dat u een Recovery Services-kluis maken in de doel-Azure-regio die u wilt gebruiken als herstellocatie na noodgevallen. Bijvoorbeeld, als uw virtuele bronmachines worden uitgevoerd in VS-Oost, en u wilt repliceren naar VS-midden, raden wij u aan de kluis te maken in VS-midden.|
**Azure capaciteit** | Voor het doel-Azure-regio die u wilt gebruiken als uw locatie van het herstel na noodgevallen moet u een abonnement met voldoende capaciteit voor virtuele machines, opslagaccounts en netwerkonderdelen hebben. Neem contact op met ondersteuning van meer capaciteit toe te voegen.
**Richtlijnen voor opslag** | Zorg ervoor dat u volgt de [richtlijnen voor opslag](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) voor uw bron Azure virtuele machines om eventuele prestatieproblemen te voorkomen. Als u de standaardinstellingen volgt, maakt Site Recovery de vereiste storage-accounts op basis van de configuratie van de bron. Als u aanpassen en uw eigen instellingen selecteert, zorg ervoor dat u volgt de [schaalbaarheidsdoelen voor virtuele-machineschijven](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Richtlijnen voor netwerken** | U moet geaccepteerde uitgaande verbinding van uw Azure-VM voor specifieke URL's of het IP-adresbereiken. Raadpleeg voor meer informatie de [richtlijnen voor het repliceren van virtuele machines van Azure toegang](site-recovery-azure-to-azure-networking-guidance.md) artikel.
**Azure VM** | Zorg ervoor dat de meest recente basiscertificaten aanwezig op de Windows- of Linux-VM zijn. Als de meest recente basiscertificaten niet aanwezig zijn, kan niet de virtuele machine vanwege veiligheidsbeperkingen Site Recovery worden geregistreerd.

>[!NOTE]
>Voor meer informatie over ondersteuning voor specifieke configuraties leest de [ondersteuningsmatrix](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [richtlijnen voor het repliceren van virtuele machines van Azure toegang](site-recovery-azure-to-azure-networking-guidance.md).
- Beginnen met het beveiligen van uw werkbelastingen door [virtuele Azure-machines repliceren](site-recovery-azure-to-azure.md).
