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
ms.openlocfilehash: c8ce05e644ad556542314b17151b808586734824
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315314"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Azure-VM's tussen Azure-regio's mislukken

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en organiseren van replicatie, failover en mislukken terug van on-premises machines en virtuele Azure-machines (VM's).

In deze zelfstudie wordt beschreven hoe u een enkele virtuele machine van Azure voor failback. Nadat u een failover hebt uitgevoerd, kunt een failback naar de primaire regio uitvoeren wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> 
> * De Azure-VM in de secundaire regio van failback.
> * Opnieuw beveiligen van de primaire Azure-VM naar de secundaire regio.
> 
> [!NOTE]
> 
> In deze zelfstudie helpt u bij de failover van een paar virtuele machines naar een doelregio en weer terug naar de regio van de gegevensbron met minimale aanpassingen. Lees de artikelen in 'How To' voor virtuele machines van Azure voor meer gedetailleerde instructies.

## <a name="before-you-start"></a>Voordat u begint

> * Zorg ervoor dat de virtuele machine de **Failover is doorgevoerd** staat.
> * Controleer of de primaire regio beschikbaar is, en maakt en nieuwe resources kunt u.
> * Zorg ervoor dat nieuwe beveiliging is ingeschakeld.

## <a name="fail-back-to-the-primary-region"></a>Een failback naar de primaire regio uitvoeren

Nadat de virtuele machines opnieuw zijn beveiligd, kunt u een failback naar de primaire regio indien nodig.

1. Klik in de kluis op **gerepliceerde Items** en selecteer de virtuele machine die opnieuw beveiligd is.

    ![Failback naar de primaire](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Klik op **Testfailover** om uit te voeren van een test back-failover naar de primaire regio.
4. Selecteer het herstelpunt en het virtuele netwerk voor de test-failover, en klik op **OK**. U kunt de virtuele machine in de primaire regio gemaakt test bekijken.
5. Nadat de test-failover is voltooid, klikt u op **failovertest** voor het opschonen van resources die in de regio van de gegevensbron voor de testfailover zijn gemaakt.
6. In **gerepliceerde items**, selecteer de virtuele machine en klikt u op **Failover**.
7. In **Failover**, selecteer een herstelpunt voor de failover.
    - **Meest recente (standaard)**: Verwerkt de gegevens in de Site Recovery-service en biedt de laagste Recovery Point Objective (RPO).
    - **Laatst verwerkt**: Hiermee herstelt u de virtuele machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery.
    - **Aangepast**: Failover-schakeling naar een bepaald herstelpunt. Deze optie is handig voor het uitvoeren van een testfailover.

8. Selecteer **sluit de computer voordat u begint met failover** als u wilt dat Site Recovery probeert een afsluiten van de bron-VM's voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. Houd er rekening mee dat Site Recovery niet, verwijdert u de bron na een failover.
9. Volg de voortgang van de failover op de pagina **Taken**.
10. Valideer de virtuele machine door aan te melden bij na de failover. U kunt het herstelpunt dat desgewenst wijzigen.
11. Nadat u de failover hebt gecontroleerd, klikt u op **geeft de failover**. Hiermee verwijdert u doorvoeren van de beschikbare herstelpunten. De wijziging van het punt hersteloptie is niet meer beschikbaar.
12. De virtuele machine moet worden weergegeven als een failover en failback uitgevoerd.

    ![Virtuele machine op de primaire en secundaire regio 's](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Het herstel na noodgevallen virtuele machines blijven in de afsluiting van de status toewijzing ongedaan gemaakt. Dit is standaard omdat de VM-informatie die nuttig voor failover van de primaire naar de secundaire regio later zijn kan door Site Recovery wordt opgeslagen. Niet in rekening gebracht voor de toewijzing ongedaan gemaakt virtuele machines, zodat ze moeten worden opgeslagen omdat ze zijn.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) over de stroom opnieuw beveiligen.
