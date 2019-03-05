---
title: Een failover- en failback-overschakeling uitvoeren van virtuele Azure IaaS-machines die zijn gerepliceerd naar een secundaire Azure-regio als onderdeel van herstel na een noodgeval met de Azure Site Recovery-service.
description: Leer hoe u een failover- en failback-overschakeling kunt uitvoeren van virtuele Azure-machines die zijn gerepliceerd naar een secundaire Azure-regio als onderdeel van herstel na een noodgeval met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 46dae28fd6c9eaa3d5e03f5f06c5e92449653679
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737719"
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions"></a>Failover en failback tussen Azure-regio's voor Azure-VM's

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgevallen door de replicatie, failover en failback van on-premises machines en virtuele Azure-machines te beheren en in te delen.

In deze zelfstudie wordt beschreven hoe u een failover van één virtuele Azure-machine naar een secundaire Azure-regio uitvoert. Nadat u een failover hebt uitgevoerd, kunt een failback naar de primaire regio uitvoeren wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een failover uitvoeren van de virtuele Azure-machine
> * De secundaire virtuele Azure-machine opnieuw beveiligen, zodat deze wordt gerepliceerd naar de primaire regio
> * Een failback uitvoeren van de secundaire virtuele machine
> * De primaire virtuele machine opnieuw beveiligen naar de secundaire regio

> [!NOTE]
> Deze zelfstudie is bedoeld om de gebruiker stapsgewijs te begeleiden bij het uitvoeren van een failover naar een doelregio en terug met een minimale hoeveelheid aanpassing. Raadpleeg de documenten bij Instructies voor Azure-VM's als u meer informatie wilt over de verschillende aspecten van het uitvoeren van een failover, inclusief netwerkoverwegingen, automatisering en het oplossen van problemen.

## <a name="prerequisites"></a>Vereisten

- Zorg dat u een [herstelanalyse](azure-to-azure-tutorial-dr-drill.md) hebt uitgevoerd om te controleren of alles werkt zoals verwacht.
- Controleer de eigenschappen van de virtuele machine voordat u de testfailover uitvoert. De virtuele machine moet voldoen aan [Azure-vereisten](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).

## <a name="run-a-failover-to-the-secondary-region"></a>Een failover uitvoeren naar de secundaire regio

1. Selecteer in **Gerepliceerde items** de virtuele machine waarnaar u een failover wilt uitvoeren en selecteer **Failover**

   ![Failover](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:

   * **Meest recent** (standaardwaarde): Met deze optie worden alle gegevens in de Site Recovery-service verwerkt en het laagste beoogde herstelpunt geboden.
   * **Laatst verwerkt**: Met deze optie wordt de virtuele machine teruggezet naar het meest recente herstelpunt dat door de Site Recovery-service is verwerkt.
   * **Aangepast**: Gebruik deze optie om de failover naar een bepaald herstelpunt uit te voeren. Deze optie is handig voor het uitvoeren van een testfailover.

3. Selecteer **Sluit de computer af voordat de failover wordt gestart** als u wilt dat Site Recovery probeert virtuele bronmachines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. Let op: Site Recovery schoont de bron niet op na een failover.

4. Volg de voortgang van de failover op de pagina **Taken**.

5. Valideer de virtuele machine na de failover door u aan te melden bij de virtuele machine. Als u naar een ander herstelpunt voor de virtuele machine wilt gaan, kunt u de optie **Herstelpunt wijzigen** gebruiken.

6. Wanneer u tevreden bent met de virtuele machine waarvoor u de failover hebt uitgevoerd, kunt u de failover **Doorvoeren**.
   Als u de failover doorvoert, worden alle herstelpunten die beschikbaar zijn voor de service verwijderd. De optie **Herstelpunt wijzigen** is niet meer beschikbaar.

## <a name="reprotect-the-secondary-vm"></a>De secundaire virtuele machine opnieuw beveiligen

Na een failover van de virtuele machine moet u deze opnieuw beveiligen zodat deze terug naar de primaire regio wordt gerepliceerd.

1. Zorg ervoor dat de virtuele machine de status **Failover is doorgevoerd** heeft en controleer of de primaire regio beschikbaar is, en of u er nieuwe resources op kunt maken en deze kunt openen.
2. Klik in **Kluis** > **Gerepliceerde items** met de rechtermuisknop op de virtuele machine waarvoor de failover is uitgevoerd is en selecteer vervolgens **Opnieuw beveiligen**.

   ![Klik met de rechtermuisknop om opnieuw te beveiligen](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. U ziet dat de richting van beveiliging, secundaire naar primaire regio, al is geselecteerd.
3. Controleer de informatie over **resourcegroep, netwerk, opslag en beschikbaarheidssets**. Alle resources die zijn gemarkeerd als (nieuw) worden gemaakt als onderdeel van het opnieuw beveiligen.
4. Klik op **OK** om het opnieuw beveiligen te activeren. Met deze taak wordt de doelsite van de meest recente gegevens voorzien. Vervolgens worden de verschillen naar de primaire regio gerepliceerd. De virtuele machine heeft nu een beveiligde status.

> [!NOTE]
> Raadpleeg het [uitleggedeelte](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) voor meer informatie over de reproductiewerkstroom en wat er tijden reproductie gebeurt.


## <a name="fail-back-to-the-primary-region"></a>Een failback naar de primaire regio uitvoeren

Nadat de virtuele machines opnieuw zijn beveiligd, kunt u als het nodig is een failback uitvoeren naar de primaire regio. Als u dit wilt doen, stelt u een failover van de secundaire in naar de primaire regio, zoals beschreven in dit artikel.

![Klik met de rechtermuisknop om opnieuw te beveiligen](./media/azure-to-azure-tutorial-failover-failback/failback.png)

Als u de voorgaande schermafbeelding ziet, heeft de VM 'ContosoWin2016' een failover-overschakeling uitgevoerd van US - centraal naar US - oost en een failback uitgevoerd van US - oost naar US - centraal.

Door de failover wordt de VM in de secundaire regio, de regio voor noodherstel, uitgeschakeld en wordt de VM in de primaire regio gemaakt en opgestart. **Onthoud** dat de VM's voor noodherstel de status ongedaan gemaakt blijven behouden zoals hierboven wordt weergegeven. Dit gedrag is inherent aan het ontwerp, omdat Azure Site Recovery de informatie van de virtuele machine bewaart, wat later eventueel handig is voor failover van de primaire naar de secundaire regio. Er worden geen kosten in rekening gebracht voor virtuele machines waarvan de toewijzing ongedaan is gemaakt; de status moet dus worden bewaard.
