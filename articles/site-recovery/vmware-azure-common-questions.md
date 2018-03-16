---
title: Veelgestelde vragen - VMware naar Azure replicatie met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van veelgestelde vragen wanneer u een lokale virtuele VMware-machines repliceren naar Azure met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.openlocfilehash: 7e556bff2e9ebdd1efc969660cc8b4a33f3adcdb
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="common-questions---vmware-to-azure-replication"></a>Veelgestelde vragen - VMware naar Azure replicatie

In dit artikel vindt u antwoorden op veelgestelde vragen die we zien wanneer de lokale virtuele VMware-machines repliceren naar Azure. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Algemeen
### <a name="how-is-site-recovery-priced"></a>Hoe prijs van Site Recovery?
Bekijk [prijzen voor Azure Site Recovery](https://azure.microsoft.com/en-in/pricing/details/site-recovery/) details.

### <a name="how-do-i-pay-for-azure-vms"></a>Hoe betaal ik voor virtuele Azure-machines?
Gegevens worden gerepliceerd naar Azure storage tijdens de replicatie en betaalt u geen wijzigingen in de VM. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch Azure IaaS virtuele machines. Hierna wordt u gefactureerd voor de rekenresources die u in Azure verbruikt.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>Wat kan ik doen met VMware naar Azure replicatie?
- **Herstel na noodgevallen**: U kunt een volledige noodherstel instellen. In dit scenario kunt u lokale virtuele VMware-machines repliceren naar Azure-opslag. Klik, als uw on-premises infrastructuur niet beschikbaar is, kunt u een failover naar Azure. Wanneer u een failover uitvoert, kan Azure VM's worden gemaakt met behulp van de gerepliceerde gegevens. U kunt toegang tot apps en werkbelastingen op de Azure VM's totdat uw on-premises datacentrum weer beschikbaar is. U kunt vervolgens failback van Azure naar uw on-premises site.
- **Migratie**: U kunt Site Recovery lokale virtuele VMware-machines migreren naar Azure. In dit scenario kunt u lokale virtuele VMware-machines repliceren naar Azure-opslag. Vervolgens schakelt u over on-premises naar Azure. Na een failover de je apps en werkbelastingen zijn beschikbaar en worden uitgevoerd op Azure Virtual machines.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Wat moet ik in Azure?
U moet een Azure-abonnement, een Recovery Services-kluis, een opslagaccount en een virtueel netwerk. De kluis, de storage-account en het netwerk moet in dezelfde regio.

### <a name="what-azure-storage-account-do-i-need"></a>Welke Azure storage-account moet ik gebruiken?
U moet een LRS of GRS-opslagaccount. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Premium-opslag wordt ondersteund.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Mijn Azure-account is nodig voor machtigingen voor het maken van virtuele machines?
Als u een abonnement beheerder bent, kunt u de replicatie-machtigingen die u nodig hebt. Als dat niet het geval is, moet u machtigingen voor het maken van een Azure VM in de resourcegroep en het virtuele netwerk die u opgeeft wanneer u Site Reocvery configureert en schrijfmachtigingen voor de record van de geselecteerde opslagaccount. [Meer informatie](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>On-premises 

### <a name="what-do-i-need-on-premises"></a>Wat kan ik lokale nodig?
Op on-premises moet u Site Recovery-onderdelen op één VMware VM geïnstalleerd. U moet ook een VMware-infrastructuur, met ten minste één ESXi-host en wordt aangeraden een vCenter-server. Bovendien moet u een of meer virtuele VMware-machines repliceren. [Meer informatie](vmware-azure-architecture.md) over VMware aan Azure-architectuur.

### <a name="where-do-on-premises-vms-replicate-to"></a>Waar lokale virtuele machines repliceren naar?
Repliceren naar Azure storage. Wanneer u een failover uitvoert, maakt Site Recovery automatisch virtuele Azure-machines van het storage-account.

### <a name="what-apps-can-i-replicate"></a>Welke apps kan ik repliceren?
U kunt een app of voor de werkbelasting die wordt uitgevoerd op een VMware-VM die voldoet aan repliceren [vereisten voor replicatie](vmware-physical-azure-support-matrix.md##replicated-machines). Site Recovery biedt ondersteuning voor toepassingsgevoelige replicatie, zodat apps kunnen worden failover en kan niet naar een intelligente status. Site Recovery kan worden geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en werkt uitstekend samen met toonaangevende leveranciers, waaronder Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Kan ik repliceren naar Azure met een site-naar-site-VPN?
Site Recovery repliceert gegevens van on-premises naar Azure-opslag via een openbaar eindpunt of met behulp van openbare ExpressRoute-peering. Replicatie via een site-naar-site VPN-netwerk wordt niet ondersteund.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Kan ik repliceren naar Azure met ExpressRoute?
Ja, ExpressRoute kan worden gebruikt voor virtuele machines repliceren naar Azure. Site Recovery repliceert gegevens naar een Azure Storage-Account via een openbaar eindpunt en u moet instellen [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) voor replicatie van Site Recovery. Nadat de virtuele machines via failover naar een Azure-netwerk, kunt u deze kunt openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>Waarom kan ik niet repliceren via VPN?

Als u naar Azure repliceert, replicatieverkeer bereikt de openbare eindpunten van een Azure Storage-account, dus u kunt alleen repliceren via het openbare internet met met ExpressRoute (openbare peering) en VPN niet werkt. 



## <a name="what-are-the-replicated-vm-requirements"></a>Wat zijn de vereisten van de gerepliceerde VM?

Voor replicatie, moet een ondersteund besturingssysteem op een VM VMware worden uitgevoerd. Bovendien hebben de virtuele machine moet voldoen aan de vereisten voor Azure Virtual machines. [Meer informatie](vmware-physical-azure-support-matrix.md##replicated-machines) in de ondersteuningsmatrix.

## <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?
Replicatie is continue wanneer virtuele VMware-machines repliceren naar Azure.

## <a name="can-i-extend-replication"></a>Kan ik Breid replicatie?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in aanvragen [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Kan ik een offline initiële replicatie doen?
Nee, dit wordt niet ondersteund. Aanvragen van deze functie in de [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Kan ik schijven uitsluiten
Ja, kunt u schijven uitsluiten van replicatie. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven kunnen worden gerepliceerd. De besturingssysteemschijf moet een standaardschijf zijn.

### <a name="can-i-add-a-new-vm-to-an-existing-replication-group"></a>Kan ik een nieuwe virtuele machine toevoegen aan een bestaande replicatiegroep
Ja.

## <a name="configuration-server"></a>Configuratieserver

### <a name="what-does-the-configuration-server-do"></a>Wat is de configuratieserver?
De configuratieserver wordt uitgevoerd de on-premises Site Recovery-onderdelen, waaronder: 
- De configuratieserver die de communicatie tussen de on-premises en Azure coördineert en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Het ontvangt replicatiegegevens; Deze optimaliseert met caching, compressie en codering; en zendt die deze naar Azure storage-., de processerver ook Mobility-Service geïnstalleerd op virtuele machines u wilt repliceren en wordt automatische detectie van de lokale virtuele VMware-machines uitgevoerd.
- De hoofddoelserver die replicatiegegevens tijdens de failback vanuit Azure afgehandeld.

### <a name="where-do-i-set-up-the-configuration-server"></a>Waar kan ik de configuratieserver instellen?
In dat geval moet u een virtuele machine van één maximaal beschikbare on-premises VMware in voor de configuratieserver.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Wat zijn de vereisten voor de configuratieserver?

Controleer de [vereisten](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kan ik handmatig instellen de configuratieserver in plaats van met een sjabloon?
Het is raadzaam dat u de meest recente versie van de sjabloon OVF [maken van de configuratieserver VM](vmware-azure-deploy-configuration-server.md). Als voor een bepaalde reden niet mogelijk, bijvoorbeeld u geen toegang tot de VMware-server, kunt u [het installatiebestand Unified](physical-azure-set-up-source.md) vanuit de portal en voer deze uit op een virtuele machine. 

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kan een configuratieserver repliceren naar meer dan één regio?
Nee. Om dit te doen, moet u een configuratieserver in elke regio instellen.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kan ik een configuratieserver in Azure hosten?
Tijdens het mogelijk is moet de Azure-VM met de configuratieserver communiceren met uw on-premises VMware-infrastructuur en virtuele machines. De overhead waarschijnlijk niet mogelijk of gewenst is.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Waar vind ik de meest recente versie van de configuratie van server-sjabloon
Download de nieuwste versie van de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="how-do-i-update-the-configuration-server"></a>Hoe kan ik de configuratieserver bijwerken?
U installeren updatepakketten. U vindt de nieuwste update-informatie in de [wiki updates pagina](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

## <a name="mobility-service"></a>Mobility-service

### <a name="where-can-i-find-the-mobility-service-installers"></a>Waar vind ik de Mobility-service-installatieprogramma
Het installatieprogramma's worden bewaard op de **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** map op de configuratieserver.

## <a name="how-do-i-install-the-mobility-service"></a>Hoe kan ik de Mobility-service installeren?
U installeert op elke virtuele machine die u repliceren wilt, met behulp van een [push-installatie](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), of het handmatig installeren van [de gebruikersinterface](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui), of [met behulp van PowerShell](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). U kunt ook implementeren met een implementatieprogramma zoals [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md), of [Azure Automation en DSC](vmware-azure-mobility-deploy-automation-dsc.md).



## <a name="security"></a>Beveiliging

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Welke toegang Site Recovery moet VMware servers?
Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Instellen van een VMware VM met de configuratieserver en andere on-premises Site Recovery-onderdelen. [Meer informatie](vmware-azure-deploy-configuration-server.md)
- Virtuele machines automatisch detecteren voor replicatie. Meer informatie over het voorbereiden van een account voor automatische detectie. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Welke toegang Site Recovery moet VMware-machines?

- Als u wilt repliceren, moet een VMware VM hebben de Site Recovery Mobility-service geïnstalleerd en uitgevoerd. U kunt het hulpprogramma handmatig implementeren of u kunt opgeven dat Site Recovery een push-installatie van de service doen moet wanneer u replicatie voor een virtuele machine inschakelen. Site Recovery moet voor de push-installatie een account die kan worden gebruikt om de serviceonderdeel te installeren. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). De processerver (standaard uitgevoerd op de configuratieserver) voert deze installatie. [Meer informatie](vmware-azure-install-mobility-service.md) over de installatie van de Mobility-service.
- Tijdens de replicatie communiceren de virtuele machines als volgt met Site Recovery:
    - Virtuele machines communiceren met de configuratieserver op poort 443 voor HTTPS voor het replicatiebeheer van.
    - Virtuele machines verzenden gegevens van replicatie naar de processerver op poort HTTPS 9443 (kan worden aangepast).
    - Als u de consistentie tussen meerdere VM's inschakelt, wordt virtuele machines met elkaar communiceren via poort 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Replicatiegegevens verzonden naar Site Recovery?
Nee, Site Recovery gerepliceerde gegevens niet onderscheppen en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Replicatiegegevens uitgewisseld tussen de VMware-hypervisors en Azure storage. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001: 2013, 27018, HIPAA-, DPA gecertificeerd en is bezig te SOC2 en FedRAMP JAB-beoordelingen.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Kunnen we lokale metagegevens behouden in een geografische regio 's
Ja. Wanneer u een kluis in een regio maakt, zorgt u ervoor dat alle metagegevens wordt gebruikt door Site Recovery blijft in deze regio geografische grens.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Ja, zowel versleuteling in de doorvoer en [-versleuteling in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.


## <a name="failover-and-failback"></a>Failover en failback
### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kunt ik herstellen?
Voor VMware naar Azure is het oudste herstelpunt, dat kunt u 72 uur.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Hoe krijg ik toegang tot virtuele Azure-machines na een failover?
U kunt virtuele Azure-machines na een failover openen via een beveiligde internetverbinding, via een site-naar-site VPN of via Azure ExpressRoute. U moet een aantal zaken om verbinding te kunnen voorbereiden. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Failover wordt uitgevoerd gegevens robuuste?
Azure is ontworpen voor herstelbaarheid. Site Recovery is ontworpen voor een failover naar een secundair Azure datacenter, in overeenstemming met de Azure SLA. Wanneer failover plaatsvindt, we zorgen ervoor dat uw metagegevens en kluizen binnen dezelfde geografische regio die u hebt gekozen voor uw kluis blijven.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
[Failover](site-recovery-failover.md) wordt niet automatisch uitgevoerd. U start een failover met één klik in de portal of u kunt [ PowerShell](/powershell/module/azurerm.siterecovery) voor het activeren van een failover.

### <a name="can-i-fail-back-to-a-different-location"></a>Kan ik failback naar een andere locatie?
Ja, als u een failover naar Azure, u kunt failback naar een andere locatie als de oorspronkelijke map is niet beschikbaar. [Meer informatie](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Waarom moet ik een VPN of ExpressRoute failback?

Wanneer u een failback van Azure, gegevens van Azure terug naar uw lokale virtuele machine wordt gekopieerd en persoonlijke toegang is vereist. 



## <a name="automation-and-scripting"></a>Automatisering en scripts

### <a name="can-i-set-up-replication-with-scripting"></a>Kan ik replicatie met het uitvoeren van scripts instellen?
Ja. U kunt met behulp van de Rest-API, PowerShell of de SDK van Azure Site Recovery-werkstromen automatiseren. [Meer](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Prestaties en capaciteit
### <a name="can-i-throttle-replication-bandwidth"></a>Kan ik de replicatie bandbreedte beperken?
Ja. [Meer informatie](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Volgende stappen
* [Bekijk](vmware-physical-azure-support-matrix.md) vereisten ondersteunen.
* [Instellen van](vmware-azure-tutorial.md) VMware naar Azure replicatie.
