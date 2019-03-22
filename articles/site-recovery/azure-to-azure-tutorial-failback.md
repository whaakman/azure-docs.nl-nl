---
title: Azure IaaS VM's die worden gerepliceerd naar een secundaire Azure-regio voor herstel na noodgeval met de Azure Site Recovery-service mislukt.
description: Leer hoe u een failover voor Azure-VM met de Azure Site Recovery-service.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: sideeksh
ms.custom: mvc
ms.openlocfilehash: d8721f313907f0e0519dca52f5565853f1c44110
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089694"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Azure-VM's tussen Azure-regio's mislukken

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en organiseren van replicatie, failover en mislukken terug van on-premises machines en virtuele Azure-machines (VM's).

In deze zelfstudie wordt beschreven hoe u een enkele virtuele machine van Azure voor failback. Nadat u een failover hebt uitgevoerd, kunt een failback naar de primaire regio uitvoeren wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> 
> * Een failback uitvoeren van de secundaire virtuele machine
> * Opnieuw beveiligen van de primaire virtuele machine terug naar de secundaire regio
> 
> [!NOTE]
> 
> Deze zelfstudie is bedoeld als richtlijn voor de gebruiker door de stappen voor de failover naar een doelregio en terug met minimale aanpassingen; Als u wilt meer informatie over de verschillende aspecten die zijn gekoppeld aan de failover, met inbegrip van overwegingen voor netwerken, verwijzen automatisering of het oplossen van problemen naar de documenten in 'How To' voor virtuele Azure-machines.

## <a name="prerequisites"></a>Vereisten

> * Zorg ervoor dat de virtuele machine heeft de status van de Failover is doorgevoerd en controleer of de primaire regio beschikbaar is, en maakt en nieuwe resources kunt u.
> * Zorg ervoor dat nieuwe beveiliging is ingeschakeld.

## <a name="fail-back-to-the-primary-region"></a>Een failback naar de primaire regio uitvoeren

Nadat de virtuele machines opnieuw zijn beveiligd, kunt u failover naar de primaire regio als en als u wilt.

1. Ga naar uw Recovery Services-kluis. Klik op de gerepliceerde Items en selecteer de virtuele machine die opnieuw beveiligd.

2. U ziet het volgende. Houd er rekening mee dat het is vergelijkbaar met de blade voor test-failover en failover van de primaire regio.
![Failback naar de primaire](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Klik op Test Failover naar een testfailover naar de primaire regio uitvoeren. Kies het herstelpunt en het Virtueelnetwerk voor de testfailover en klik op OK. Hier ziet u de test die virtuele machine gemaakt in de primaire regio die u kunt toegang tot en inspecteren.

4. Wanneer Failover testen voldoende is, kunt u klikken op failovertest opschonen voor het opschonen van resources die in de regio van de gegevensbron voor de testfailover zijn gemaakt.

5. Selecteer de virtuele machine die u failover wilt in gerepliceerde items > Failover.

6. Selecteer een herstelpunt voor de failover tijdens Failover. U kunt een van de volgende opties gebruiken:
    1. Meest recente (standaard): Deze optie worden alle gegevens in de Site Recovery-service verwerkt en biedt de laagste Recovery Point Objective (RPO)
    2. Laatst verwerkte: Deze optie wordt de virtuele machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery-service
    3. Aangepast: Gebruik deze optie om failover naar een bepaald herstelpunt. Deze optie is handig voor het uitvoeren van een test-failover

7. Selecteer afsluiten machine voordat de failover wordt gestart als u wilt dat Site Recovery probeert virtuele bronmachines afgesloten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. Let op: Site Recovery schoont de bron niet op na een failover.

8. Volg de voortgang van de failover op de pagina met taken

9. Valideer de virtuele machine na de failover door u aan te melden bij de virtuele machine. Als u gaan van een ander herstelpunt voor de virtuele machine wilt, kunt u de hersteloptie punt wijzigen.

10. Wanneer u tevreden met de mislukte bent via virtuele machine, geeft u de failover. Als u de failover doorvoert, worden alle herstelpunten die beschikbaar zijn voor de service verwijderd. De wijziging van het punt hersteloptie is niet meer beschikbaar.

![Virtuele machine op de primaire en secundaire regio 's](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

Als u de voorgaande schermafbeelding ziet, heeft de VM 'ContosoWin2016' een failover-overschakeling uitgevoerd van US - centraal naar US - oost en een failback uitgevoerd van US - oost naar US - centraal.

Houd er rekening mee dat de DR-VMs in de toewijzing ongedaan gemaakt afsluitingsstaat blijft. Dit gedrag is inherent aan het ontwerp, omdat Azure Site Recovery de informatie van de virtuele machine bewaart, wat later eventueel handig is voor failover van de primaire naar de secundaire regio. Niet in rekening gebracht voor de toewijzing ongedaan gemaakt voor de virtuele machines, zodat deze moet worden opgeslagen omdat het.

> [!NOTE]
> Zie de ['procedures' sectie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) voor meer informatie over de werkstroom opnieuw beveiliging en wat er gebeurt tijdens het opnieuw beveiliging.
