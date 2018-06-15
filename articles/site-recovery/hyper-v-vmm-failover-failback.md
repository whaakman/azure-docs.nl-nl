---
title: Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar een secundair datacenter met Site Recovery | Microsoft Docs
description: Informatie over het Hyper-V-machines met uw secundaire on-premises site failover en failback naar primaire site met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: ecb0b9395ce7071442ddf0dd976e1ca57b8be906
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161138"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar de locatie van uw secundaire on-premises

De [Azure Site Recovery](site-recovery-overview.md) service beheert en regelt de replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

In dit artikel wordt beschreven hoe failover van een Hyper-V-virtuele machine in een cloud System Center Virtual Machine Manager (VMM) naar een secundaire site voor VMM worden beheerd. Nadat u een failover hebt uitgevoerd, valt u terug op uw on-premises site wanneer deze beschikbaar is. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Failover van een Hyper-V virtuele machine uit een primaire VMM-cloud naar een secundaire VMM-cloud
> * Beveilig opnieuw vanaf de secundaire site naar de primaire en failback
> * Optioneel beginnen van primaire repliceren naar de secundaire opnieuw

## <a name="failover-and-failback"></a>Failover en failback

Failover en failback bestaat uit drie fasen:

1. **Failover naar de secundaire site**: failover machines van de primaire site naar de secundaire.
2. **Failback van de secundaire site**: virtuele machines repliceren van secundaire op primaire en voer een geplande failover failback.
3. Na de geplande failover moet u eventueel starten vanaf de primaire site repliceren naar de secundaire opnieuw.


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u hebt voltooid, een [disaster recovery inzoomen](hyper-v-vmm-test-failover.md) om te controleren of alles werkt zoals verwacht.
- Zorg ervoor dat de primaire en secundaire VMM-servers zijn verbonden met Site Recovery voor het voltooien van failback.



## <a name="run-a-failover-from-primary-to-secondary"></a>Een failover van primaire naar secundaire uitvoert

U kunt een reguliere of geplande failover uitvoeren voor Hyper-V-machines.

- Gebruik een reguliere failover voor onverwachte storingen. Wanneer u deze failover uitvoert, Site Recovery maakt een virtuele machine in de secundaire site en drijft de. Verlies van gegevens kan optreden, afhankelijk van in afwachting van gegevens die zijn niet gesynchroniseerd.
- Een geplande failover kan worden gebruikt voor onderhoud, hetzij tijdens de verwachte uitval. Deze optie biedt geen gegevens verloren gaan. Wanneer een geplande failover wordt geactiveerd, worden de Bronmachines afgesloten. Niet-gesynchroniseerde gegevens worden gesynchroniseerd en de failover wordt geactiveerd. 
- 
Deze procedure wordt beschreven hoe een reguliere failover uit te voeren.


1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.
1. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Site Recovery wordt ook proberen te synchroniseren van on-premises gegevens die nog niet nog naar de secundaire site verzonden voordat de failover. Houd er rekening mee dat de failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
2. U moet nu mogelijk om te zien van de virtuele machine in de secundaire VMM-cloud.
3. Nadat u hebt gecontroleerd dat de virtuele machine **doorvoeren** de failover. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.  


## <a name="reverse-replicate-and-failover"></a>Omgekeerde replicatie en failover

Starten van de secundaire site repliceren naar de primaire en failback naar de primaire site. Nadat de virtuele machines worden opnieuw uitgevoerd in de primaire site, kunt u ze naar de secundaire site repliceren.  

 
1. Klik op de virtuele machine > Klik op **omgekeerde replicatie**.
2. Zodra de taak voltooid is, klikt u op de virtuele machine > In **Failover**, Controleer of de failoverrichting (van de secundaire VMM-cloud) en selecteert u de bron- en doellocaties. 
4. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
5. Controleer of de virtuele machine beschikbaar is in de primaire VMM-cloud.
6. Als u repliceren van de primaire virtuele machine naar de secundaire site opnieuw starten wilt, klikt u op **omgekeerde replicatie**.

## <a name="next-steps"></a>Volgende stappen
[Bekijk de stap](hyper-v-vmm-disaster-recovery.md) voor Hyper-V-machines repliceren naar een secundaire site.
