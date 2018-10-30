---
title: Failback van Azure tijdens het herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery | Microsoft Docs
description: Leer hoe u een failback uitvoeren naar de on-premises site na een failover naar Azure, tijdens het herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure.
author: nsoneji
ms.service: site-recovery
ms.date: 10/11/2018
ms.topic: conceptual
ms.author: nisoneji
ms.openlocfilehash: becf0b8c9f14d81b0a25fb1727b69452c3ff690c
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215171"
---
# <a name="fail-back-vmware-vms-and-physical-servers-from-azure-to-an-on-premises-site"></a>Failback virtuele VMware-machines en fysieke servers van Azure naar een on-premises site

Dit artikel wordt beschreven hoe u een failover back virtuele machines van Azure Virtual Machines naar een on-premises VMware-omgeving. Volg de instructies in dit artikel mislukken back-uw VMware-machines of Windows/Linux fysieke servers nadat ze hebben een failover van de on-premises site naar Azure met behulp van de [Failover in Azure Site Recovery](site-recovery-failover.md) zelfstudie.

## <a name="prerequisites"></a>Vereisten
- Zorg ervoor dat u hebt gelezen, de details over de [verschillende typen failback](concepts-types-of-failback.md) en de bijbehorende beperkingen.

> [!WARNING]
> U kunt geen failback uitvoeren nadat u een hebt [migratie voltooid](migrate-overview.md#what-do-we-mean-by-migration), een virtuele machine naar een andere resourcegroep verplaatst of verwijderd van virtuele machine van Azure. Als u de beveiliging van de virtuele machine uitschakelen, kunt geen u failback.

> [!WARNING]
> Een fysieke server Windows Server 2008 R2 SP1 kan geen als beveiligd en failover naar Azure, failback uitgevoerd.

> [!NOTE]
> Als de virtuele VMware-machines failover kunt u kunt geen failback uitvoeren naar een Hyper-V-host.


- Voordat u doorgaat, de stappen opnieuw beveiligen zodat de virtuele machines zich in een gerepliceerde status en kunt u een failover naar een on-premises site starten. Zie voor meer informatie, [over het opnieuw beveiligen van Azure naar on-premises](vmware-azure-reprotect.md).

- Zorg ervoor dat de vCenter een verbonden status heeft is voordat u een failback uitvoeren. Anders mislukt loskoppelen van schijven en deze koppelen terug naar de virtuele machine.

- Tijdens de failover naar Azure de on-premises site mogelijk niet toegankelijk is, en de configuratieserver is mogelijk niet beschikbaar of afgesloten omlaag. Tijdens het opnieuw beveiligen en failback moet de on-premises configuratieserver actief is en in een verbonden status OK hebben. 

- Tijdens een failback wordt de virtuele machine moet aanwezig zijn in de database van de configuratieserver of failback wordt niet voltooid. Zorg ervoor dat u regelmatig geplande back-ups van uw server uitvoeren. Als er zich een noodgeval voordoet, die u wilt herstellen van de server met het oorspronkelijke IP-adres voor failback om te werken.

- De hoofddoelserver hoeft niet alle momentopnamen voordat opnieuw beveiligen/failback wordt geactiveerd.

## <a name="overview-of-failback"></a>Overzicht van failback
Nadat u hebt een failover naar Azure, kunt u terug naar uw on-premises site niet door het uitvoeren van de volgende stappen uit:

1. [Opnieuw beveiligen](vmware-azure-reprotect.md) de virtuele machines in Azure zodat zij beginnen te repliceren naar virtuele VMware-machines in uw on-premises site. Als onderdeel van dit proces moet u ook:

    * Instellen van het hoofddoel van een on-premises. Gebruik van een Windows-hoofddoel voor Windows virtuele machines en een [Linux-hoofddoel](vmware-azure-install-linux-master-target.md) voor virtuele Linux-machines.
    * Instellen van een [processerver](vmware-azure-set-up-process-server-azure.md).
    * Start [opnieuw beveiligen](vmware-azure-reprotect.md) de on-premises virtuele machine uitschakelen en synchroniseren van gegevens van de Azure virtuele machine met de on-premises-schijven.

2. Nadat u uw virtuele machines in Azure repliceren naar uw on-premises site, start u een failover van Azure naar de on-premises site.

3. Nadat uw gegevens terug is mislukt, u opnieuw beveiligen de on-premises virtuele machines opnieuw zodat ze gaan repliceren naar Azure.

Bekijk de volgende video over hoe u een failback uitvoeren naar een on-premises site voor een snel overzicht:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Stappen voor het uitvoeren van een failback

> [!IMPORTANT]
> Voordat u een failback, zorg ervoor dat u opnieuw beveiligen van de virtuele machines hebt voltooid. De virtuele machines moeten zich in een beveiligde status en hun status moet **OK**. Als u wilt opnieuw met het beveiligen van de virtuele machines, lezen [over het opnieuw beveiligen](vmware-azure-reprotect.md).

1. Selecteer de virtuele machine op de pagina gerepliceerde items. Met de rechtermuisknop op het selecteren **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting (van Azure). Selecteer het herstelpunt (meest recente of de meest recente app-consistente) die u wilt gebruiken voor de failover. De app toepassingsconsistente punt is achter het laatste punt in tijd en zorgt ervoor dat gegevens verloren gaan.
3. Tijdens de failover schakelt Site Recovery de virtuele machines op Azure. Nadat u hebt gecontroleerd dat failback voltooid zoals verwacht, kunt u controleren dat de virtuele machines in Azure worden afgesloten.
4. **Doorvoeren** is vereist voor het verwijderen van de virtuele failover-machine van Azure. Met de rechtermuisknop op het beveiligde item en selecteer vervolgens **doorvoeren**. Een taak verwijdert de failover-virtuele machines in Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Welke herstelpunt kan ik failback de virtuele machines?

Tijdens de failback hebt u twee opties voor failback van de virtuele machine/herstelplan te gaan.

- Als u het laatst verwerkte punt in tijd selecteert, failover alle virtuele machines naar hun laatst beschikbare punt in tijd. Als er een replicatiegroep in het herstelplan te gaan, failover elke virtuele machine van de replicatiegroep-schakeling naar de onafhankelijke laatste punt in tijd.

  U kunt geen failback een virtuele machine wanneer dat nodig is ten minste één herstelpunt. U kunt geen failback een herstelplan totdat alle virtuele machines ten minste één herstelpunt.

  > [!NOTE]
  > Meest recente herstelpunt is een crash-consistente herstelpunt.

- Als u het App-consistente herstelpunt selecteert, wordt een failback van één virtuele machine herstelt naar de meest recente toepassingsconsistente herstelpunt. In het geval van een herstelplan met een replicatiegroep, wordt elke replicatiegroep aan de algemene beschikbare herstelpunt herstelt.
Toepassingsconsistente herstelpunten kunnen achter in de tijd, en er mogelijk verlies van gegevens.

## <a name="what-happens-to-vmware-tools-post-failback"></a>Wat gebeurt er met VMware-hulpprogramma's post failback?

In het geval van een Windows virtuele machine schakelt Site Recovery de VMware-hulpprogramma's tijdens de failover. Tijdens een failback van de virtuele machine van Windows, zijn de VMware-hulpprogramma's ingeschakeld. 


## <a name="reprotect-from-on-premises-to-azure"></a>Opnieuw beveiligen van on-premises naar Azure
Nadat de failback is voltooid en u doorvoeren hebt gestart, wordt de herstelde virtuele machines in Azure worden verwijderd. Nu de virtuele machine is terug op de on-premises site, maar wordt niet worden beveiligd. Als u wilt repliceren naar Azure opnieuw start, het volgende doen:

1. Selecteer **kluis** > **instelling** > **gerepliceerde items**, selecteert u de virtuele machines die een failback uitgevoerd en selecteer vervolgens  **Opnieuw beveiligen**.
2. Voer de waarde van de processerver die worden gebruikt moet voor het verzenden van gegevens terug naar Azure.
3. Selecteer **OK** om te beginnen met de taak opnieuw beveiligen.

> [!NOTE]
> Wanneer een on-premises virtuele machine wordt opgestart, duurt het even voordat de agent te registreren naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd kunt opnieuw beveiligen mislukt dat en wordt een foutbericht weergegeven waarin staat dat de agent is niet geïnstalleerd. Wacht een paar minuten en probeer het vervolgens opnieuw opnieuw beveiligen.

## <a name="next-steps"></a>Volgende stappen

Nadat de taak opnieuw beveiligen is voltooid, de virtuele machine repliceren naar Azure, en u kunnen doen een [failover](site-recovery-failover.md) uw virtuele machines wilt verplaatsen naar Azure opnieuw.


