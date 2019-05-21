---
title: Azure-VM gerepliceerd naar een secundaire Azure-regio voor herstel na noodgeval met de Azure Site Recovery-service mislukt.
description: Leer hoe u een failover voor Azure-VM met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 339cbf18d79053dfb0704d928b8c9251c73b3a6e
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951782"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Failback van virtuele Azure-machine tussen Azure-regio 's

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgeval, door de replicatie, failover en failback van on-premises machines en Azure-VM’s (virtuele machines) te beheren en in te delen.

In deze zelfstudie wordt beschreven hoe u een enkele virtuele machine van Azure voor failback. Nadat u een failover hebt, moet u een failback naar de primaire regio als deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> 
> * De virtuele machine in de secundaire regio van failback.
> * De primaire virtuele machine terug naar de secundaire regio opnieuw beveiligen.
> 
> [!NOTE]
> 
> In deze zelfstudie helpt u bij de failover van een paar virtuele machines naar een doelregio en weer terug naar de regio van de gegevensbron met minimale aanpassingen. Raadpleeg voor meer gedetailleerde instructies, de [instructies begeleidt op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/windows/).

## <a name="before-you-start"></a>Voordat u begint

* Zorg ervoor dat de status van de virtuele machine is **Failover is doorgevoerd**.
* Controleer dat de primaire regio beschikbaar is en dat u kunt maken en toegang tot nieuwe resources.
* Zorg ervoor dat deze opnieuw beveiligen is ingeschakeld.

## <a name="fail-back-to-the-primary-region"></a>Een failback naar de primaire regio uitvoeren

Nadat de virtuele machines opnieuw zijn beveiligd, kunt u een failback naar de primaire regio indien nodig.

1. Selecteer in de kluis **gerepliceerde items**, en selecteer vervolgens de virtuele machine die is beveiligd.

    ![Failback naar de primaire](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Selecteer **Testfailover** om uit te voeren van een test back-failover naar de primaire regio.
4. Selecteer het herstelpunt en het virtuele netwerk voor de test-failover, en selecteer vervolgens **OK**. U kunt de virtuele machine in de primaire regio gemaakt test bekijken.
5. Nadat de failovertest voltooid is, selecteert u **failovertest** voor het opschonen van resources die in de regio van de gegevensbron voor de testfailover zijn gemaakt.
6. In **gerepliceerde items**, selecteer de virtuele machine en selecteer vervolgens **Failover**.
7. In **Failover**, selecteer een herstelpunt om te fungeren als failover voor:
    - **Meest recente (standaard)**: Verwerkt de gegevens in de Site Recovery-service en biedt het laagste beoogde herstelpunt (RPO).
    - **Laatst verwerkt**: Hiermee herstelt u de virtuele machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery.
    - **Aangepast**: Failover-schakeling naar een bepaald herstelpunt. Deze optie is handig voor het uitvoeren van een testfailover.

8. Selecteer **sluit de computer voordat u begint met failover** als u wilt dat Site Recovery probeert een afsluiten van de bron-VM's voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als afsluiten is mislukt. Houd er rekening mee dat Site Recovery niet, verwijdert u de bron na een failover.
9. Volg de voortgang van de failover op de pagina **Taken**.
10. Nadat de failover voltooid is, meld u aan bij het valideren de virtuele machine. U kunt het herstelpunt dat desgewenst wijzigen.
11. Nadat u de failover hebt gecontroleerd, schakelt u **geeft de failover**. Hiermee verwijdert u doorvoeren van de beschikbare herstelpunten. De wijziging van het punt hersteloptie is niet meer beschikbaar.
12. De virtuele machine moet worden weergegeven als een failover en failback uitgevoerd.

    ![Virtuele machine op de primaire en secundaire regio 's](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Het herstel na noodgevallen virtuele machines blijven in de status afsluiten/toewijzing ongedaan gemaakt. Dit is standaard omdat de VM-informatie die nuttig voor failover van de primaire naar de secundaire regio later zijn kan door Site Recovery wordt opgeslagen. Niet in rekening gebracht voor de toewijzing ongedaan gemaakt virtuele machines, zodat ze moeten worden opgeslagen omdat ze zijn.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) over de stroom opnieuw beveiligen.
