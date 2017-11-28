---
title: Hoe u een failback van Azure naar VMware | Microsoft Docs
description: U kunt een failback naar on-premises om virtuele machines starten na een failover van virtuele machines naar Azure. Meer informatie over de stappen voor het failback.
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: ruturajd
ms.openlocfilehash: ad424818f41e6b48e754dd0d39771248a1cd04fb
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Failback van Azure naar een on-premises site

In dit artikel wordt beschreven hoe back virtuele machines van Azure Virtual Machines naar de lokale site mislukt. Volg de instructies in dit artikel mislukken back-uw virtuele VMware-machines of Windows of Linux fysieke servers nadat ze hebt failover van de on-premises site naar Azure met behulp van de [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md) zelfstudie.

> [!WARNING]
> U kunt geen failback nadat u een hebt [migratie voltooid](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), een virtuele machine naar een andere resourcegroep verplaatst of verwijderd van virtuele machine van Azure. Als u de beveiliging van de virtuele machine uitschakelt, maar u kunt geen failback.

> [!NOTE]
> Als u virtuele VMware-machines failover vervolgens kunt u niet failback naar een Hyper-v-host.

## <a name="overview-of-failback"></a>Overzicht van failback
Hier volgt de werking van failback. Nadat u hebt een failover uitvoeren naar Azure, schakelt u terug naar uw on-premises site in een aantal fasen:

1. [Beveiligt](site-recovery-how-to-reprotect.md) de virtuele machines in Azure zodat deze beginnen te repliceren naar VMware-virtuele machines in uw on-premises site. Als onderdeel van dit proces moet u ook:
    1. Het hoofddoel van een lokale instellen: Windows hoofddoel voor virtuele machines van Windows en [Linux-hoofddoel](site-recovery-how-to-install-linux-master-target.md) voor virtuele Linux-machines.
    2. Instellen van een [processerver](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Initiëren [beveiligt](site-recovery-how-to-reprotect.md). Dit wordt de on-premises virtuele machine uitschakelen en de Azure VM-gegevens synchroniseren met de lokale schijven.
5. Nadat u uw virtuele machines in Azure worden gerepliceerd naar uw on-premises site, start u een mislukken via van Azure naar de lokale site.

Nadat uw gegevens terug is mislukt, beveiligt u de lokale virtuele machines die u niet terug naar, zodat deze beginnen te repliceren naar Azure.

Bekijk de volgende video over het failover van Azure naar een on-premises site voor een snel overzicht.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-to-the-original-or-alternate-location"></a>Failback naar de oorspronkelijke of alternatieve locatie

Als de failover van een virtuele VMware-machine kunt u failback naar dezelfde bron lokale virtuele machine als deze nog bestaat. In dit scenario worden alleen de wijzigingen terug gerepliceerd. Dit scenario wordt ook wel herstel oorspronkelijke locatie. Als de on-premises virtuele machine niet bestaat, is het scenario voor het herstel van een alternatieve locatie.

> [!NOTE]
> U kunt alleen failback naar de oorspronkelijke server met vCenter en configuratie. U kunt een nieuwe configuratieserver en gebruik van failback niet implementeren. U kunt ook een nieuwe vCenter toevoegen aan afgesloten configuratieserver en failback naar de nieuwe vCenter.

#### <a name="original-location-recovery"></a>Herstel oorspronkelijke locatie

Als u niet terug naar de oorspronkelijke virtuele machine, zijn de volgende voorwaarden zijn vereist:
* Als de virtuele machine wordt beheerd door een vCenter-server, moet toegang tot het gegevensarchief van de virtuele machine hebben op het hoofddoel ESX-host.
* Als de virtuele machine zich op een ESX-host, maar wordt niet beheerd door de vCenter, moet zich de harde schijf van de virtuele machine in een gegevensopslag die toegang het hoofddoel host tot.
* Als uw virtuele machine zich op een ESX-host en maakt geen gebruik van vCenter, moet u detectie van de ESX-host van het hoofddoel uitvoeren voordat u opnieuw beveiligen. Dit geldt als u bent terug fysieke servers te mislukken.
* U kunt niet naar een virtuele storage area network (vSAN) of een schijf die op basis van niet-gecodeerd apparaat (RDM) toewijzen als de schijven bestaan al en zijn verbonden met de on-premises virtuele machine.

#### <a name="alternate-location-recovery"></a>Herstel van alternatieve locatie
Als u de on-premises virtuele machine bestaat niet voor de virtuele machine opnieuw te beveiligen, wordt het scenario voor het herstel van een alternatieve locatie genoemd. De werkstroom opnieuw beveiligen maakt de on-premises virtuele machine opnieuw. Ook wordt hierdoor het downloaden van een volledige gegevens.

* Wanneer u een failover naar een alternatieve locatie, wordt de virtuele machine worden hersteld naar dezelfde ESX-host waarop de hoofddoelserver is geïmplementeerd. Het gegevensarchief die wordt gebruikt voor het maken van de schijf worden het hetzelfde gegevensarchief die is geselecteerd bij de virtuele machine opnieuw te beveiligen.
* U kunt niet weer alleen aan een virtuele machine file system (VMFS) of een virtueel SAN gegevensarchief. Als u een RDM hebt, werkt beveiligt en failback niet.
* Beveiligt, moet u een grote eerste gegevensoverdracht die wordt gevolgd door de wijzigingen. Dit proces bestaat omdat de virtuele machine niet on-premises bestaat. De volledige gegevens moet opnieuw worden gerepliceerd. Deze opnieuw beveiligen duurt ook langer dan een herstel van de oorspronkelijke locatie.
* U afkeuren terug naar schijven op basis van RDM niet. Nieuwe virtuele machine-schijven (VMDKs) kunnen alleen worden gemaakt op een gegevensopslag VMFS/virtueel SAN.

Een fysieke computer als een failover naar Azure, failover back alleen als een virtuele VMware-machine (ook wel P2A2V genoemd). Deze stroom valt onder het hersteldoel alternatieve locatie.

* Een fysieke server van Windows Server 2008 R2 SP1, kan niet als beveiligd en een failover naar Azure, worden uitgevoerd voor terug.
* Zorg ervoor dat u ten minste één hoofddoelserver en de benodigde ESX/ESXi-hosts waaraan u moet een failback uit detecteren.

## <a name="have-you-completed-reprotection"></a>Hebt u beveiligingspoging voltooid?
Voordat u doorgaat, voert u opnieuw beveiligen zodat de virtuele machines in een gerepliceerde status zijn en u kunt een failover naar een on-premises site starten. Zie voor meer informatie [hoe beveiligt van Azure met on-premises](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Tijdens de failover naar Azure, de lokale site mag niet toegankelijk is en kan daarom de configuratieserver zijn niet beschikbaar of afsluiten. Tijdens het beveiligt en failback moet de lokale configuratie-server actief en een verbonden status OK hebben.

* Een configuratieserver is on-premises vereist als u een failback doet. De server moet de status actief en verbonden met de service zodanig zijn dat de status OK. De virtuele machine moet aanwezig zijn in de configuratiedatabase van de server tijdens de failback, of failback won't slagen. Dus zorgen ervoor dat u regelmatig geplande back-ups van uw server. Als er een ramp optreedt, moet u de server met hetzelfde IP-adres voor failback werken herstellen.
* De hoofddoelserver mag geen eventuele momentopnamen voordat beveiligt/failback.

## <a name="steps-to-fail-back"></a>Stappen voor failback

> [!IMPORTANT]
> Voordat u een failback-bewerking starten, zorg ervoor dat u de virtuele machines waarvoor hebt voltooid. De virtuele machines moet in een beveiligde staat en hun status moet **OK**. Lezen voor de virtuele machines beveiligt, [hoe beveiligt](site-recovery-how-to-reprotect.md).

1. Selecteer de virtuele machine op de pagina gerepliceerde items en met de rechtermuisknop op het selecteren **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting (van Azure) en selecteer het herstelpunt (laatste of de meest recente app consistent) die u wilt gebruiken voor de failover. Het app-consistente punt achter het laatste herstelpunt is in de tijd en zorgt ervoor dat er gegevens verloren gaan.
3. Tijdens de failover afgesloten Site Recovery de virtuele machines in Azure. Nadat u hebt gecontroleerd dat failback is voltooid zoals verwacht, kunt u controleren dat de virtuele machines in Azure is afgesloten.

### <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Welk herstelpunt kan ik een failback uit op de virtuele machines?

Tijdens de failback hebt u twee opties failback van de virtuele machine/herstelplan.

Als u het meest recente verwerkte punt in tijd selecteert, wordt alle virtuele machines failover worden uitgevoerd naar het laatste punt in tijd. Er is een replicatiegroep in het herstelplan, is vervolgens elke virtuele machine van de replicatiegroep failover de onafhankelijke laatste herstelpunt in de tijd.

U kan geen failback uit op een virtuele machine totdat er ten minste één herstelpunt. U kan niet een failback uit op een herstelplan totdat alle virtuele machines ten minste één herstelpunt.

> [!NOTE]
> Meest recente herstelpunt is een crashconsistent herstelpunt.

Als u de toepassing consistente herstelpunt selecteert, wordt een failback van één virtuele machine herstellen naar de meest recente beschikbare toepassingsconsistente herstelpunt. Elke replicatiegroep herstelt naar de algemene beschikbare herstelpunt in het geval van een herstelplan aan een replicatiegroep.
Houd er rekening mee dat toepassingsconsistente herstelpunten kunnen zich niet achter in de tijd en er wordt mogelijk verlies van gegevens.

### <a name="what-happens-to-vmware-tools-post-failback"></a>Wat gebeurt er met VMware tools post failback?

Tijdens de failover naar Azure, kunnen niet de VMware-hulpprogramma's worden uitgevoerd op virtuele machine van Azure. In geval van een virtuele Windows-computer wordt de VMware-hulpprogramma's in ASR uitgeschakeld tijdens de failover. In geval van een virtuele Linux-machine, wordt de VMware-hulpprogramma's in ASR verwijderd tijdens de failover.

De VMware-hulpprogramma's zijn tijdens de failback van de Windows virtuele machine opnieuw is ingeschakeld bij de failback. Op deze manier voor een virtuele linux-machine, worden de VMware-hulpprogramma's geïnstalleerd op de computer tijdens de failback.

## <a name="next-steps"></a>Volgende stappen

Nadat de failback is voltooid, moet u het doorvoeren van de virtuele machine om ervoor te zorgen dat de herstelde virtuele machines in Azure worden verwijderd.

### <a name="commit"></a>Doorvoeren
Doorvoeren is vereist voor de mislukte verwijderen via de virtuele machine van Azure.
Met de rechtermuisknop op het beveiligde item en klik vervolgens op **doorvoeren**. Een taak verwijdert de mislukte via virtuele machines in Azure.

### <a name="reprotect-from-on-premises-to-azure"></a>Beveiligt on-premises naar Azure

Nadat het vastleggen is voltooid, wordt de virtuele machine is terug op de lokale site, maar won't worden beveiligd. Als u wilt repliceren naar Azure opnieuw start, het volgende doen:

1. In **kluis** > **instelling** > **gerepliceerde items**, selecteert u de virtuele machines die zijn mislukt terug en klik vervolgens op **opnieuw beveiligen**.
2. Geeft de waarde van de processerver die moet worden gebruikt om gegevens te verzenden naar Azure.
3. Klik op **OK** om te beginnen met de taak opnieuw beveiligen.

> [!NOTE]
> Nadat een on-premises virtuele machine wordt opgestart, duurt het enige tijd voor de agent registreren terug naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd beveiligt mislukt en retourneert een foutmelding dat de agent niet is geïnstalleerd. Wacht een paar minuten en probeer het vervolgens opnieuw beveiligt.

Nadat de taak opnieuw beveiligen is voltooid, kunt de virtuele machine wordt gerepliceerd naar Azure, en u een failover.

## <a name="common-issues"></a>Algemene problemen
Zorg ervoor dat de vCenter een verbonden status heeft in voordat u een failback. Anders mislukt schijven verbreken en bijvoegen terug naar de virtuele machine.

### <a name="common-error-codes"></a>Algemene foutcodes

#### <a name="error-code-8038"></a>Foutcode 8038

*Kan niet online zetten van de on-premises virtuele machine vanwege de fout*

Dit gebeurt wanneer 
1. De de on-premises virtuele machine op een host die beschikt niet over voldoende geheugen ingericht wordt opgeroepen.

Dit probleem op te lossen
1. U kunt meer geheugen op de host ESXi inrichten.
2. de virtuele machine naar een andere ESXi-host die voldoende geheugen heeft om de virtuele machine opstarten vMotion.

