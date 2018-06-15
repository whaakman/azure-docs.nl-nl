---
title: Failback van Azure naar VMware met Azure Site Recovery | Microsoft Docs
description: U kunt een failback naar on-premises om virtuele machines starten na een failover van virtuele machines naar Azure. Meer informatie over de stappen voor het failback.
services: site-recovery
author: nsoneji
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: nisoneji
ms.openlocfilehash: 8f580fa40bade2bf586dd116729881b249bbba88
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
ms.locfileid: "29944004"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Failback van Azure naar een on-premises site

In dit artikel wordt beschreven hoe mislukken back virtuele machines van Azure Virtual Machines naar een on-premises VMware-omgeving. Volg de instructies in dit artikel mislukken back-uw virtuele VMware-machines of Windows of Linux fysieke servers nadat ze hebt failover van de on-premises site naar Azure met behulp van de [-Failover in Azure Site Recovery](site-recovery-failover.md) zelfstudie.

## <a name="prerequisites"></a>Vereisten
- Zorg ervoor dat u de details hebt gelezen over de [verschillende soorten failback](concepts-types-of-failback.md) en de bijbehorende beperkingen.

> [!WARNING]
> U terug nadat u hebt niet afkeuren [migratie voltooid](migrate-overview.md#what-do-we-mean-by-migration), een virtuele machine naar een andere resourcegroep verplaatst of verwijderd van virtuele machine van Azure. Als u de beveiliging van de virtuele machine uitschakelt, kan niet u failback uit.

> [!WARNING]
> Een fysieke server van Windows Server 2008 R2 SP1, kan niet als beveiligd en een failover naar Azure, worden uitgevoerd voor terug.

> [!NOTE]
> Als de virtuele VMware-machines failover kan niet u failback naar een Hyper-V-host.


- Voordat u doorgaat, voert u opnieuw beveiligen zodat de virtuele machines in een gerepliceerde status worden en u met een failover naar een on-premises site beginnen kunt. Zie voor meer informatie [hoe beveiligt van Azure met on-premises](vmware-azure-reprotect.md).

- Zorg ervoor dat de vCenter een verbonden status heeft in voordat u een failback. Anders mislukt schijven verbreken en deze terug naar de virtuele machine te koppelen.

- Tijdens de failover naar Azure, de lokale site niet meer toegankelijk zijn en de configuratieserver is mogelijk niet beschikbaar of afgesloten omlaag. Tijdens het beveiligt en failback moet de lokale configuratie-server actief en een verbonden status OK hebben. 

- De virtuele machine moet aanwezig zijn in de configuratiedatabase van de server tijdens failback of failback won't slagen. Zorg ervoor dat u regelmatig geplande back-ups van uw server uitvoeren. Als er een ramp optreedt, moet u de server met het oorspronkelijke IP-adres voor failback werken herstellen.

- De hoofddoelserver mag geen eventuele momentopnamen voordat beveiligt/failback.

## <a name="overview-of-failback"></a>Overzicht van failback
Nadat u hebt een failover uitvoeren naar Azure, kunt u schakelt terug naar uw on-premises site door het uitvoeren van de volgende stappen uit:

1. [Beveiligt](vmware-azure-reprotect.md) de virtuele machines in Azure zodat deze beginnen te repliceren naar VMware-virtuele machines in uw on-premises site. Als onderdeel van dit proces moet u ook:

    * Het hoofddoel van een lokale instellen. Een Windows-hoofddoel gebruiken voor virtuele machines van Windows en een [Linux-hoofddoel](vmware-azure-install-linux-master-target.md) voor virtuele Linux-machines.
    * Instellen van een [processerver](vmware-azure-set-up-process-server-azure.md).
    * Start [beveiligt](vmware-azure-reprotect.md) de on-premises virtuele machine uitschakelen en de Azure VM-gegevens synchroniseren met de lokale schijven.

2. Nadat u uw virtuele machines in Azure repliceren naar uw on-premises site, start u een failover van Azure naar de lokale site.

3. Nadat uw gegevens terug is mislukt, beveiligt u de on-premises virtuele machines opnieuw zodat deze beginnen te repliceren naar Azure.

Bekijk de volgende video over het failback naar een on-premises site voor een snel overzicht:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Stappen voor failback

> [!IMPORTANT]
> Voordat u een failback, zorg er dan voor dat u klaar bent met de beveiligingspoging van de virtuele machines. De virtuele machines moet in een beveiligde staat en hun status moet **OK**. Lezen voor de virtuele machines beveiligt, [hoe beveiligt](vmware-azure-reprotect.md).

1. Selecteer de virtuele machine op de pagina gerepliceerde items. Met de rechtermuisknop op het Selecteer **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting (van Azure). Selecteer het herstelpunt (laatste of de meest recente app consistent) die u wilt gebruiken voor de failover. Het app-consistente punt achter het laatste herstelpunt is in de tijd en zorgt ervoor dat er gegevens verloren gaan.
3. Tijdens de failover afgesloten Site Recovery de virtuele machines in Azure. Nadat u hebt gecontroleerd dat failback voltooid zoals verwacht, kunt u controleren dat de virtuele machines in Azure worden afgesloten.
4. **Doorvoeren** is vereist voor het verwijderen van de failover-virtuele machine van Azure. Met de rechtermuisknop op het beveiligde item en selecteer vervolgens **doorvoeren**. Een taak verwijdert de failover virtuele machines in Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Welk herstelpunt kan ik een failback uit op de virtuele machines?

Tijdens de failback hebt u twee opties failback van de virtuele machine/herstelplan.

- Als u het meest recente verwerkte punt in tijd selecteert, failover alle virtuele machines naar het laatste punt in tijd. Als er een replicatiegroep in het herstelplan, wordt elke virtuele machine van de replicatiegroep overgenomen door de onafhankelijke laatste herstelpunt in de tijd.

  U kan geen failback uit op een virtuele machine totdat er ten minste één herstelpunt. U kan niet een failback uit op een herstelplan totdat alle virtuele machines ten minste één herstelpunt.

  > [!NOTE]
  > Meest recente herstelpunt is een crashconsistent herstelpunt.

- Als u het toepassingsconsistente herstelpunt selecteert, wordt er een failback van één virtuele machine herstelt naar de meest recente beschikbare toepassingsconsistente herstelpunt. In het geval van een herstelplan met een replicatiegroep, wordt elke replicatiegroep herstelt naar de algemene beschikbare herstelpunt.
Toepassingsconsistente herstelpunten kunnen zich niet achter in de tijd en er wordt mogelijk verlies van gegevens.

## <a name="what-happens-to-vmware-tools-post-failback"></a>Wat gebeurt er met VMware tools post failback?

Site Recovery wordt de VMware-hulpprogramma's in het geval van een virtuele Windows-computer uitgeschakeld tijdens de failover. De VMware-hulpprogramma's worden ingeschakeld tijdens de failback van de virtuele machine van Windows. 


## <a name="reprotect-from-on-premises-to-azure"></a>Beveiligt on-premises naar Azure
Nadat de failback is voltooid en u doorvoeren hebt gestart, wordt de herstelde virtuele machines in Azure worden verwijderd. Nu de virtuele machine terug op de lokale site is, maar deze won't worden beveiligd. Als u wilt repliceren naar Azure opnieuw start, het volgende doen:

1. Selecteer **kluis** > **instelling** > **gerepliceerde items**, selecteer de virtuele machines die niet zijn geslaagd terug en selecteer vervolgens  **Beveilig de farm opnieuw**.
2. Voer de waarde van de processerver die moet worden gebruikt om gegevens te verzenden naar Azure.
3. Selecteer **OK** om te beginnen met de taak opnieuw beveiligen.

> [!NOTE]
> Nadat een on-premises virtuele machine wordt opgestart, duurt het enige tijd voor de agent registreren terug naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd beveiligt mislukt en retourneert een foutmelding dat de agent is niet geïnstalleerd. Wacht een paar minuten en probeer het vervolgens opnieuw beveiligt.

## <a name="next-steps"></a>Volgende stappen

Nadat de taak opnieuw beveiligen is voltooid, de virtuele machine repliceren naar Azure, en u kunnen doen een [failover](site-recovery-failover.md) uw virtuele machines wilt verplaatsen naar Azure opnieuw.


