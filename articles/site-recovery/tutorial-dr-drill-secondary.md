---
title: Uitvoeren van een herstel na noodgevallen detailanalyse naar uw secundaire on-premises site met Azure Site Recovery | Microsoft Docs
description: Informatie over het uitvoeren van een herstel na noodgevallen detailanalyse naar uw secundaire on-premises site met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 445878e2-6682-49ba-914d-4c6824ab08a6
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 8737c2a22bd729cfc15d5448e1ec0becef643fd5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-a-disaster-recovery-drill-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Uitvoeren van een herstel na noodgevallen detailanalyse voor Hyper-V-machines naar uw site secundaire on-premises

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u een herstel na noodgevallen detailanalyse voor Hyper-V virtuele machines uitvoeren met uw secundaire on-premises site. Hyper-V-machines moeten worden beheerd in een privécloud van System Center Virtual Machine Manager (VMM). Een detailanalyse valideert van uw replicatiestrategie voor zonder verlies van gegevens of uitvaltijd en niet van invloed op die betrekking hebben op uw productieomgeving. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten voor testfailover voorbereiden
> * Een testfailover voor één computer uitvoeren


## <a name="prerequisites"></a>Vereisten

- U kunt een testfailover uitvoeren met een aantal opties voor netwerken op de secundaire site. U kunt de failover zonder een netwerk worden uitgevoerd met een bestaand netwerk of kunt Site Recovery automatisch maken van een testnetwerk. 
**Als u wilt een bestaand productienetwerk gebruiken voor de testfailover:**:
    - De primaire virtuele machine moet worden afgesloten wanneer u de testfailover uitvoert. Anders worden twee virtuele machines met dezelfde identiteit in hetzelfde netwerk, worden uitgevoerd op hetzelfde moment. 
    - Als u de wijzigingen voor het testen van virtuele machines, gaan de wijzigingen verloren wanneer u de testfailover opschonen. Wijzigingen worden niet gerepliceerd naar de primaire virtuele machine.
    - Testen van een productienetwerk zorgt ervoor dat de uitvaltijd voor productieworkloads. Vraag uw gebruikers niet te gebruiken van gerelateerde apps wanneer het herstel na noodgevallen inzoomen uitgevoerd wordt. 
- Het testnetwerk replica hoeft niet overeen met het VMM logisch netwerktype gebruikt voor de testfailover. Maar bepaalde combinaties werken niet. Bijvoorbeeld als de replica maakt gebruik van DHCP- en isolatie op basis van een VLAN, u Windows-Netwerkvirtualisatie niet gebruiken voor het testfailovernetwerk omdat deze IP-adresgroepen moet. 
- Het is raadzaam dat u het netwerk die u hebt geselecteerd voor netwerktoewijzing voor testfailover niet gebruikt.


## <a name="run-a-test-failover-for-a-vm"></a>Een testfailover uitvoeren voor een virtuele machine

1. In **gerepliceerde Items**, klikt u op de virtuele machine > **Testfailover**.
2. In **Testfailover**, opgeven hoe de test-virtuele machines worden verbonden met netwerken na de testfailover. We raden aan dat voor deze zelfstudie u Site Recovery automatisch maken van een testnetwerk laten. [Meer informatie](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery).
3. Klik op **OK** om te beginnen met de failover. Voortgang volgen op de **taken** tabblad.
4. Nadat de failover is voltooid, moet u controleren of de test VM's worden gestart.
5. Wanneer u bent klaar, klikt u op **opschonen testfailover**. Hiermee verwijdert u de test-virtuele machines en eventuele netwerken die zijn gemaakt tijdens de testfailover.
6. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. 


## <a name="next-steps"></a>Volgende stappen

[Een productie-failover uitvoert](tutorial-vmm-to-vmm-failover-failback.md)






