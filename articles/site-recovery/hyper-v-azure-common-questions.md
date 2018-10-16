---
title: Algemene vragen - Hyper-V naar Azure-replicatie met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van de veelgestelde vragen over on-premises Hyper-V-machines repliceren naar Azure met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 10/10/2018
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 7c5316aea2bfdb9a96b9aeb2dd5387b2643a92b2
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319724"
---
# <a name="common-questions---hyper-v-to-azure-replication"></a>Veelgestelde vragen - Hyper-V naar Azure-replicatie

In dit artikel vindt u antwoorden op veelgestelde vragen, we zien bij het repliceren van on-premises Hyper-V-machines naar Azure. 


## <a name="general"></a>Algemeen

### <a name="how-is-site-recovery-priced"></a>Hoe wordt de Site Recovery geprijsd?
Beoordeling [prijzen voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) details.

### <a name="how-do-i-pay-for-azure-vms"></a>Hoe moet ik betalen voor Azure VM's?
Tijdens de replicatie, gegevens worden gerepliceerd naar Azure storage en betaalt u geen wijzigingen in de virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele machines van Azure IaaS. Hierna wordt u gefactureerd voor de rekenresources die u in Azure gebruikt.

## <a name="azure"></a>Azure

### <a name="what-do-i-need-in-azure"></a>Wat moet ik in Azure?
U moet een Azure-abonnement, een Recovery Services-kluis, een storage-account en een virtueel netwerk. De kluis, de storage-account en het netwerk moeten zich in dezelfde regio bevinden.

### <a name="what-azure-storage-account-do-i-need"></a>Welke Azure-opslagaccount heb ik nodig?
U moet een LRS of GRS-opslagaccount. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Premium storage wordt ondersteund.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Mijn Azure-account moet machtigingen voor het maken van virtuele machines?
Als u een abonnementsbeheerder bent, hebt u de replicatiemachtigingen die u nodig hebt. Als u niet bent, moet u machtigingen voor het maken van een Azure-VM in de resourcegroep en het virtuele netwerk dat u opgeeft bij het configureren van Site Recovery en machtigingen voor het schrijven naar het geselecteerde opslagaccount. [Meer informatie](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="is-replication-data-sent-to-site-recovery"></a>Worden er replicatiegegevens verzonden met Site Recovery?
Nee, Site Recovery gerepliceerde gegevens niet worden onderschept, en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Replicatiegegevens worden uitgewisseld tussen Hyper-V-hosts en Azure storage. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Kunnen we de metagegevens van on-premises bewaren binnen een geografische regio's?
Ja. Wanneer u een kluis in een regio maken, zorgen we dat alle metagegevens die wordt gebruikt door Site Recovery blijft binnen een geografische grens van die regio.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Ja, zowel versleuteling-in-transit en [versleuteling in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.


## <a name="deployment"></a>Implementatie

### <a name="what-can-i-do-with-hyper-v-to-azure-replication"></a>Wat kan ik doen met Hyper-V naar Azure-replicatie?

- **Herstel na noodgevallen**: U kunt volledige noodherstel instellen. In dit scenario kunt u on-premises Hyper-V-machines repliceren naar Azure-opslag:
    - U kunt virtuele machines repliceren naar Azure. Als uw on-premises infrastructuur niet beschikbaar is, schakelt u over naar Azure.
    - Wanneer u een failover uitvoert, kan Azure-VM's worden gemaakt met behulp van de gerepliceerde gegevens. U kunt toegang tot apps en workloads in de Azure VM's.
    - Als uw on-premises datacenter weer beschikbaar is, kunt u failback van Azure naar uw on-premises site.
- **Migratie**: U kunt Site Recovery gebruiken voor het migreren van on-premises Hyper-V-machines naar Azure storage. Vervolgens kunt u een failover van on-premises naar Azure. Na een failover zijn uw toepassingen en workloads beschikbaar en worden uitgevoerd op Azure Virtual machines.


### <a name="what-do-i-need-on-premises"></a>Wat kan ik on-premises nodig?

U moet een of meer virtuele machines wordt uitgevoerd op een of meer zelfstandige of geclusterde Hyper-V-hosts. U kunt ook virtuele machines die worden uitgevoerd op hosts die worden beheerd door System Center Virtual Machine Manager (VMM) repliceren.
    - Als u bent VMM, niet wordt uitgevoerd tijdens de implementatie van Site Recovery, deelt u Hyper-V-hosts en clusters in Hyper-V-sites. U installeert de Site Recovery-agents (Azure Site Recovery Provider en Recovery Services agent) op elke Hyper-V-host.
    - Als Hyper-V-hosts zich in een VMM-cloud bevinden, kunt u replicatie in VMM indelen. U kunt de Site Recovery Provider installeren op de VMM-server en de Recovery Services-agent op elke Hyper-V-host. U toewijzen tussen VMM logische/VM-netwerken en Azure VNets.
    - 
[Meer informatie](hyper-v-azure-architecture.md) over Hyper-V naar Azure-architectuur.

### <a name="can-i-replicate-vms-located-on-a-hyper-v-cluster"></a>Kan ik repliceren van virtuele machines die zich op een Hyper-V-cluster?

Ja, Site Recovery biedt ondersteuning voor geclusterde Hyper-V-hosts. Houd rekening met het volgende:

- Alle knooppunten van het cluster moeten worden geregistreerd bij dezelfde kluis.
- Als u VMM niet gebruikt, moeten alle Hyper-V-hosts in het cluster worden toegevoegd aan de dezelfde Hyper-V-site.
- U installeert de Azure Site Recovery Provider en Recovery Services-agent op elke Hyper-V-host in het cluster, en elke host toevoegt aan een Hyper-V-site.
- Er zijn geen specifieke stappen moet worden uitgevoerd op het cluster.
- Als u het hulpprogramma Deployment Planner voor Hyper-V uitvoert, verzamelt het hulpprogramma de profielgegevens in het knooppunt dat wordt uitgevoerd en waarop de virtuele machine wordt uitgevoerd. Het hulpprogramma kan niet alle gegevens verzamelen uit een knooppunt dat uitgeschakeld, maar deze wordt bijgehouden dat knooppunt. Nadat het knooppunt actief is, start het hulpprogramma voor verzamelen van de profielgegevens van de virtuele machine van het (als de virtuele machine deel uit van de lijst van de virtuele machine profiel maakt en wordt uitgevoerd op het knooppunt).
- Als een virtuele machine op een Hyper-V-host in een Site Recovery-kluis gemigreerd naar een andere Hyper-V-host in hetzelfde cluster of naar een zelfstandige host, wordt niet replicatie voor de virtuele machine beïnvloed. De Hyper-V-host moet voldoen aan [vereisten](hyper-v-azure-support-matrix.md#on-premises-servers), en worden geconfigureerd in een Site Recovery-kluis. 


### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan ik virtuele machines beveiligen als Hyper-V wordt uitgevoerd op een clientbesturingssysteem?
Nee, virtuele machines moeten zich bevinden op een Hyper-V-hostserver die wordt uitgevoerd op een ondersteunde Windows-servercomputer. Als u wilt beveiligen van een client-computer kan [deze als een fysieke machine repliceert](physical-azure-disaster-recovery.md) naar Azure.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kan ik virtuele machines van de tweede generatie Hyper-V repliceren naar Azure?
Ja. Site Recovery converteert van generatie 2 naar generatie 1 tijdens de failover. Bij de failback wordt de machine geconverteerd naar generatie 2.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan ik Site Recovery-scenario's met een SDK automatiseren?
Ja. U kunt Site Recovery-werkstromen automatiseren met de Rest API-, PowerShell- of Azure-SDK. Momenteel ondersteunde scenario's voor het repliceren van Hyper-V naar Azure met behulp van PowerShell:

- [Het repliceren van Hyper-V zonder VMM met behulp van PowerShell](hyper-v-azure-powershell-resource-manager.md)
- [Replicatie van Hyper-V met VMM met behulp van Powershell](hyper-v-vmm-powershell-resource-manager.md)

## <a name="replication"></a>Replicatie

### <a name="where-do-on-premises-vms-replicate-to"></a>Waar repliceer on-premises machines naar?
Gegevens worden gerepliceerd naar Azure storage. Wanneer u een failover uitvoert, maakt Site Recovery automatisch virtuele Azure-machines uit de storage-account.

### <a name="what-apps-can-i-replicate"></a>Welke apps kan ik repliceren?
U kunt elke app of het uitvoeren van een Hyper-V-VM die aan de voldoet workload repliceren [replicatievereisten](hyper-v-azure-support-matrix.md#replicated-vms). Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden failover is uitgevoerd en kan niet naar een intelligente status. Site Recovery kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory, en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="whats-the-replication-process"></a>Wat is het replicatieproces?

1. Wanneer de eerste replicatie wordt geactiveerd, wordt een Hyper-V-VM-momentopname wordt gemaakt.
2. Virtuele harde schijven op de virtuele machine worden één voor één gerepliceerd, tot ze allemaal zijn gekopieerd naar Azure. Dit kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. Informatie over het verhogen van de netwerkbandbreedte.
3. Als er tijdens de initiële replicatie wordt uitgevoerd, worden de wijzigingen in de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze logboekbestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een bijbehorend .hrl-bestand dat wordt verzonden naar een secundaire opslag. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
4. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd.
5. Schijfwijzigingen in het logboek gesynchroniseerd en samengevoegd op de bovenliggende schijf.
6. Nadat de initiële replicatie is voltooid, wordt de beveiliging voltooien op de virtuele machine-taak wordt uitgevoerd. Het netwerk en de overige instellingen na de replicatie zo geconfigureerd dat de virtuele machine is beveiligd.
7. In deze fase kunt u de instellingen van de virtuele machine om ervoor te zorgen dat deze gereed voor failover is controleren. U kunt een Dr-herstelanalyse (testfailover) voor de virtuele machine, om te controleren of deze niet meer dan zoals verwacht kan uitvoeren.
8. Na de initiële replicatie begint de replicatie van verschillen, in overeenstemming met het replicatiebeleid.
9. Wijzigingen die zijn geregistreerd .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand.
10. Het logboek wordt verzonden naar het storage-account van de klant. Wanneer een logboek onderweg naar Azure is, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
11. Tijdens de begin- en delta-replicatie, kunt u de virtuele machine in Azure portal controleren.

[Meer informatie](hyper-v-azure-architecture.md#replication-process) over het replicatieproces.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Kan ik repliceren naar Azure met een site-naar-site-VPN?

Site Recovery repliceert gegevens van on-premises naar Azure storage via een openbaar eindpunt of met behulp van openbare ExpressRoute-peering. Replicatie via een site-naar-site VPN-netwerk wordt niet ondersteund.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Kan ik repliceren naar Azure met ExpressRoute?

Ja, ExpressRoute kan worden gebruikt voor het repliceren van virtuele machines naar Azure. Site Recovery worden gegevens gerepliceerd naar een Azure Storage-Account via een openbaar eindpunt en moet u voor het instellen van [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) voor Site Recovery-replicatie. Nadat de virtuele machines een failover uitvoeren naar een Azure-netwerk, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>Waarom kan ik niet repliceren via VPN?

Wanneer u naar Azure repliceren, replicatieverkeer bereikt de openbare eindpunten van een Azure Storage-account, dus u kunt alleen repliceren via het openbare internet met ExpressRoute (openbare peering) en VPN werkt niet. 

### <a name="what-are-the-replicated-vm-requirements"></a>Wat zijn de vereisten van de gerepliceerde VM's?

Voor replicatie, moet een ondersteund besturingssysteem op een Hyper-V-virtuele machine worden uitgevoerd. Bovendien hebben de virtuele machine moet voldoen aan de vereisten voor Azure-VM's. [Meer informatie](hyper-v-azure-support-matrix.md#replicated-vms) in de ondersteuningsmatrix.

### <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?

Hyper-V-machines kunnen elke 30 seconden (met uitzondering van premium storage), 5 minuten of 15 minuten worden gerepliceerd.

###<a name="can-i-extend-replication"></a>Kan ik replicatie verlengen?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-initial-replication"></a>Kan ik een offline initiële replicatie?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Kan ik schijven uitsluiten?
Ja, kunt u schijven uitsluiten van replicatie. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven kunnen worden gerepliceerd. De besturingssysteemschijf moet een standaardschijf.



## <a name="security"></a>Beveiliging

### <a name="what-access-does-site-recovery-need-to-hyper-v-hosts"></a>Welke toegang Site Recovery moet Hyper-V-hosts

Site Recovery moet toegang hebben tot Hyper-V-hosts voor het repliceren van de virtuele machines die u selecteert. Site Recovery installeert het volgende op de Hyper-V-hosts:

- Als u niet met zijn VMM, de Azure Site Recovery Provider en Recovery Services-agent geïnstalleerd op elke host.
- Als u VMM uitvoert, wordt de Recovery Services-agent is geïnstalleerd op elke host. De Provider wordt uitgevoerd op de VMM-server.


### <a name="what-does-site-recovery-install-on-hyper-v-vms"></a>Wat Site Recovery op Hyper-V-machines installeren?

Site Recovery wordt expliciet niets mee geïnstalleerd op de Hyper-V-machines die zijn ingeschakeld voor replicatie.




## <a name="failover-and-failback"></a>Failover en failback


### <a name="how-do-i-fail-over-to-azure"></a>Hoe ik een failover naar Azure?

U kunt een geplande of niet-geplande failover van on-premises Hyper-V-machines naar Azure uitvoert.
    - Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
    - U kunt een niet-geplande failover uitvoeren als uw primaire site kan niet worden geopend.
    - U kunt een enkele machine failover of plannen voor herstel, voor het indelen van failover van meerdere virtuele machines maken.
    - U moet een failover uitvoeren. Nadat de eerste fase van de failover is voltooid, zou het mogelijk om te zien van de gemaakte replica-VM's in Azure. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig. Vervolgens geeft u de failover, toegang tot de workload via de replica virtuele machine van Azure.
   

### <a name="how-do-i-access-azure-vms-after-failover"></a>Hoe krijg ik toegang tot Azure-VM's na een failover?
Na een failover, kunt u Azure-VM's openen via een beveiligde internetverbinding, via een site-naar-site-VPN of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Failover wordt uitgevoerd gegevens tegen?
Azure is ontworpen voor herstelbaarheid. Site Recovery is ontworpen voor failover naar een secundaire Azure-datacenter, in overeenstemming met de Azure SLA. Wanneer een failover optreedt, zorgen wij ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
[Failover](site-recovery-failover.md) wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u [ PowerShell](/powershell/module/azurerm.siterecovery) dat een failover wordt geactiveerd.

### <a name="how-do-i-fail-back"></a>Hoe ik failback?

Nadat uw on-premises infrastructuur opnieuw actief is, kunt u failover terug. Failback vindt plaats in drie fasen:

1. U een vliegende start een geplande failover van Azure naar de on-premises site met behulp van een aantal verschillende opties:

    - Downtime minimaliseren: als u deze optie gebruikt de Site Recovery worden gegevens voordat de failover wordt gesynchroniseerd. Het gewijzigde gegevensblokken controleert en downloadt deze naar de on-premises site, terwijl de virtuele machine van Azure houdt actief is, leiden tot minimale downtime. Wanneer u handmatig opgeven dat de failover moet worden voltooid, de Azure-VM wordt afgesloten, een definitieve wijzigingen worden gekopieerd en de failover wordt gestart.
    - Volledige download: met deze optie worden de gegevens gesynchroniseerd tijdens de failover. Deze optie wordt de hele schijf gedownload. Het is sneller omdat er geen controlesommen worden berekend, maar er meer downtime is. Gebruik deze optie als u hebt zijn met de replica virtuele Azure-machines gedurende een bepaalde periode, of als de on-premises VM is verwijderd.

2. U kunt selecteren voor failover naar dezelfde virtuele machine of een andere virtuele machine. U kunt opgeven dat Site Recovery de virtuele machine maken te als deze nog niet bestaat.
3. Nadat de initiële synchronisatie is voltooid, selecteert u om de failover te voltooien. Nadat deze is voltooid, kunt u zich aanmelden op de on-premises-VM om te controleren of dat alles werkt zoals verwacht. In de Azure-portal, kunt u zien dat de Azure VM's zijn gestopt.
4. U geeft de failover voor het voltooien van en toegang tot de workload via de on-premises VM opnieuw.
5. Nadat de werkbelastingen terug zijn mislukt, schakelt u omgekeerde replicatie, zodat de on-premises VM's naar Azure opnieuw repliceren.

### <a name="can-i-fail-back-to-a-different-location"></a>Kan ik een failback uitvoeren naar een andere locatie?
Ja, als u een failover naar Azure, kunt u failover naar een andere locatie als de oorspronkelijke map is niet beschikbaar. [Meer informatie](hyper-v-azure-failback.md#failback-to-an-alternate-location-in-hyper-v-environment).
