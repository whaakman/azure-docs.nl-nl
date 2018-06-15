---
title: Hoe u een failback van Azure naar VMware | Microsoft Docs
description: U kunt een failback naar on-premises om virtuele machines starten na een failover van virtuele machines naar Azure. Meer informatie over de stappen voor het failback.
services: site-recovery
documentationcenter: ''
author: nsoneji
manager: gauravd
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: nisoneji
ms.openlocfilehash: 50dccf6196b7affd3d21087f851b929d0e850f6d
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768471"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Failback van Azure naar een on-premises site

In dit artikel wordt beschreven hoe mislukken back virtuele machines van Azure Virtual Machines on-premises VMware-omgeving. Volg de instructies in dit artikel mislukken back-uw virtuele VMware-machines of Windows of Linux fysieke servers nadat ze hebt failover van de on-premises site naar Azure met behulp van de [-Failover in Site Recovery](site-recovery-failover.md) zelfstudie.

## <a name="prerequisites"></a>Vereisten
- Zorg ervoor dat u de details hebt gelezen over de [verschillende soorten failback](concepts-types-of-failback.md) en de bijbehorende beperkingen.

> [!WARNING]
> U kunt geen failback nadat u een hebt [migratie voltooid](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), een virtuele machine naar een andere resourcegroep verplaatst of verwijderd van virtuele machine van Azure. Als u de beveiliging van de virtuele machine uitschakelt, maar u kunt geen failback.

> [!WARNING]
> Een fysieke server van Windows Server 2008 R2 SP1, kan niet als beveiligd en een failover naar Azure, worden uitgevoerd voor terug.

> [!NOTE]
> Als u virtuele VMware-machines failover vervolgens kunt u niet failback naar een Hyper-v-host.


- Voordat u doorgaat, voert u opnieuw beveiligen zodat de virtuele machines in een gerepliceerde status zijn en u kunt een failover naar een on-premises site starten. Zie voor meer informatie [hoe beveiligt van Azure met on-premises](site-recovery-how-to-reprotect.md).

- Zorg ervoor dat de vCenter een verbonden status heeft in voordat u een failback. Anders mislukt schijven verbreken en deze terug naar de virtuele machine te koppelen.

- Tijdens de failover naar Azure, de lokale site mag niet toegankelijk is en kan daarom de configuratieserver zijn niet beschikbaar of afsluiten. Tijdens het beveiligt en failback moet de lokale configuratie-server actief en een verbonden status OK hebben. 

- De virtuele machine moet aanwezig zijn in de configuratiedatabase van de server tijdens de failback, of failback won't slagen. Daarom zorgen ervoor dat u regelmatig geplande back-ups van uw server. Als er een ramp optreedt, moet u de server met het oorspronkelijke IP-adres voor failback werken herstellen.

- De hoofddoelserver mag geen eventuele momentopnamen voordat beveiligt/failback.

## <a name="overview-of-failback"></a>Overzicht van failback
Nadat u hebt een failover uitvoeren naar Azure, kunt u schakelt terug naar uw on-premises site door het uitvoeren van de volgende stappen uit:

1. [Beveiligt](site-recovery-how-to-reprotect.md) de virtuele machines in Azure zodat deze beginnen te repliceren naar VMware-virtuele machines in uw on-premises site. Als onderdeel van dit proces moet u ook:
    1. Het hoofddoel van een lokale instellen: Windows hoofddoel voor virtuele machines van Windows en [Linux-hoofddoel](site-recovery-how-to-install-linux-master-target.md) voor virtuele Linux-machines.
    2. Instellen van een [processerver](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Initiëren [beveiligt](site-recovery-how-to-reprotect.md). Dit wordt de on-premises virtuele machine uitschakelen en de Azure VM-gegevens synchroniseren met de lokale schijven.

1. Als uw virtuele machines in Azure worden gerepliceerd naar uw on-premises site, start u een mislukken via van Azure naar de lokale site.

1. Nadat uw gegevens terug is mislukt, beveiligt u de on-premises virtuele machines opnieuw, zodat deze beginnen te repliceren naar Azure.

Bekijk de volgende video over het failback naar een on-premises site voor een snel overzicht.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Stappen voor failback

> [!IMPORTANT]
> Voordat u een failback-bewerking starten, zorg ervoor dat u de virtuele machines waarvoor hebt voltooid. De virtuele machines moet in een beveiligde staat en hun status moet **OK**. Lezen voor de virtuele machines beveiligt, [hoe beveiligt](site-recovery-how-to-reprotect.md).

1. Selecteer de virtuele machine op de pagina gerepliceerde items en met de rechtermuisknop op het selecteren **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting (van Azure) en selecteer het herstelpunt (laatste of de meest recente app consistent) die u wilt gebruiken voor de failover. Het app-consistente punt achter het laatste herstelpunt is in de tijd en zorgt ervoor dat er gegevens verloren gaan.
3. Tijdens de failover afgesloten Site Recovery de virtuele machines in Azure. Nadat u hebt gecontroleerd dat failback is voltooid zoals verwacht, kunt u controleren dat de virtuele machines in Azure is afgesloten.
4. **Doorvoeren** is vereist voor de mislukte verwijderen via de virtuele machine uit Azure.Right en klik op het beveiligde item en klik vervolgens op **doorvoeren**. Een taak verwijdert de mislukte via virtuele machines in Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Welk herstelpunt kan ik een failback uit op de virtuele machines?

Tijdens de failback hebt u twee opties failback van de virtuele machine/herstelplan.

- Als u het meest recente verwerkte punt in tijd selecteert, wordt alle virtuele machines failover worden uitgevoerd naar het laatste punt in tijd. Het geval, er is een replicatiegroep in het herstelplan, is klikt u vervolgens elke virtuele machine van de replicatiegroep failover de onafhankelijke laatste herstelpunt in de tijd.

    U kan geen failback uit op een virtuele machine totdat er ten minste één herstelpunt. U kan niet een failback uit op een herstelplan totdat alle virtuele machines ten minste één herstelpunt.

> [!NOTE]
> Meest recente herstelpunt is een crashconsistent herstelpunt.

- Als u de toepassing consistente herstelpunt selecteert, wordt er een failback van één virtuele machine herstelt naar de meest recente beschikbare toepassingsconsistente herstelpunt. In het geval van een herstelplan met een replicatiegroep, wordt elke replicatiegroep herstelt naar de algemene beschikbare herstelpunt.
Toepassingsconsistente herstelpunten kunnen zich niet achter in de tijd en er wordt mogelijk verlies van gegevens.

## <a name="what-happens-to-vmware-tools-post-failback"></a>Wat gebeurt er met VMware tools post failback?

Azure Site Recovery wordt de VMware-hulpprogramma's in het geval van een virtuele Windows-computer uitgeschakeld tijdens de failover. De VMware-hulpprogramma's zijn tijdens de failback van de Windows virtuele machine opnieuw is ingeschakeld. 


## <a name="reprotect-from-on-premises-to-azure"></a>Beveiligt on-premises naar Azure
Nadat failback is voltooid en u doorvoeren hebt geïnitieerd worden de herstelde virtuele machines in Azure verwijderd. Nu de virtuele machine terug op de lokale site is, maar deze won't worden beveiligd. Als u wilt repliceren naar Azure opnieuw start, het volgende doen:

1. In **kluis** > **instelling** > **gerepliceerde items**, selecteert u de virtuele machines die zijn mislukt terug en klik vervolgens op **opnieuw beveiligen**.
2. Geeft de waarde van de processerver die moet worden gebruikt om gegevens te verzenden naar Azure.
3. Klik op **OK** om te beginnen met de taak opnieuw beveiligen.

> [!NOTE]
> Nadat een on-premises virtuele machine wordt opgestart, duurt het enige tijd voor de agent registreren terug naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd beveiligt mislukt en retourneert een foutmelding dat de agent niet is geïnstalleerd. Wacht een paar minuten en probeer het vervolgens opnieuw beveiligt.

## <a name="next-steps"></a>Volgende stappen

Nadat de taak opnieuw beveiligen is voltooid, de virtuele machine wordt gerepliceerd naar Azure en u kunt doen een [failover](site-recovery-failover.md) uw virtuele machines wilt verplaatsen naar Azure opnieuw.


