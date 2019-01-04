---
title: Algemene vragen - herstel na noodgevallen van Azure naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat een overzicht van veelgestelde vragen bij het instellen van herstel na noodgeval voor virtuele Azure-machines naar aonther Azure-regio met Azure Site Recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 6fe7152e43640a809ab9f4de39b1c6b599975a20
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969944"
---
# <a name="common-questions---azure-to-azure-replication"></a>Veelgestelde vragen - replicatie van Azure naar Azure

In dit artikel vindt u antwoorden op veelgestelde vragen, we zien bij het implementeren van herstel na noodgeval voor virtuele Azure-machines naar een andere Azure-regio. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Algemeen
### <a name="how-is-site-recovery-priced"></a>Hoe wordt de Site Recovery geprijsd?
Beoordeling [prijzen voor Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) details.

### <a name="how-to-configure-site-recovery-on-azure-vms-what-are-the-best-practices"></a>Klik hier voor meer informatie over het configureren van Site Recovery op Azure Virtual machines. Wat zijn de best practices?
1. [Azure naar Azure-architectuur begrijpen](azure-to-azure-architecture.md)
2. [Controleer de ondersteunde en niet-ondersteunde configuraties](azure-to-azure-support-matrix.md)
3. [Herstel na noodgevallen instellen voor virtuele Azure-machines](azure-to-azure-how-to-enable-replication.md)
4. [Een testfailover uitvoeren](azure-to-azure-tutorial-dr-drill.md)
5. [Failover en failback naar de primaire regio](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-azure-disk-encryption-enabled-vms"></a>Kan ik repliceren Azure disk encryption ingeschakeld virtuele machines?
Ja, kunt u ze repliceren. Raadpleeg [artikel](azure-to-azure-how-to-enable-replication-ade-vms.md) om in te schakelen van virtuele machines voor replicatie van Azure disk encryption (ADE) is ingeschakeld. Houd er rekening mee dat alleen Azure-VM's met Windows OS en ingeschakeld voor versleuteling met Azure AD-app die momenteel worden ondersteund door Azure Site Recovery.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Kan ik virtuele machines repliceren naar een ander abonnement?
Ja, kunt u virtuele Azure-machines repliceren naar een ander abonnement met in dezelfde Azure Active Directory-tenant.
Configureren van herstel na Noodgevallen [voor abonnementen](https://azure.microsoft.com/blog/cross-subscription-dr) is eenvoudig. U kunt een ander abonnement op het moment van replicatie.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Ik kan aan zone vastgemaakte virtuele Azure-machines repliceren naar een andere regio.
Ja, u kunt [aan zone vastgemaakte VM's repliceren](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) naar een andere regio.

### <a name="can-i-exclude-disks"></a>Kan ik schijven uitsluiten?

Ja, kunt u schijven uitsluiten op het moment van beveiliging met behulp van PowerShell. Raadpleeg [powershell-richtlijnen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine) schijf uitsluiten

###<a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?
Replicatie is continue bij het repliceren van virtuele Azure-machines naar een andere Azure-regio. Controleer de [Azure naar Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process) replicatiearchitectuur om de details te begrijpen.

### <a name="can-i-replicate-virtual-machines-within-a-same-region-i-need-this-to-migrate-vms"></a>Kan ik virtuele machines in dezelfde regio repliceren? Moet ik dit voor het migreren van virtuele machines?
U kunt Azure naar Azure DR-oplossing niet gebruiken voor het repliceren van virtuele machines in dezelfde regio.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Kan ik virtuele machines repliceren naar een Azure-regio?
Met Site Recovery kunt u repliceren en herstellen van virtuele machines tussen elke twee regio's binnen hetzelfde geografische cluster. Geografische clusters worden gedefinieerd gegevenslatentie en -soevereiniteit waarmee u rekening moet houden. Zie voor meer informatie, Site Recovery [regio ondersteuningsmatrix](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Is Site Recovery voor verbinding met internet nodig?

Nee, Site Recovery vereist geen verbinding met internet, maar toegang tot de Site Recovery-URL's en IP-adressen zoals vermeld in dit [artikel](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)

## <a name="replication-policy"></a>Replicatiebeleid

### <a name="what-is-a-replication-policy"></a>Wat is beleid voor replicatie?
Hiermee worden de instellingen voor recovery point bewaren geschiedenis en de app de momentopnamefrequentie gedefinieerd. Azure Site Recovery maakt standaard een nieuw replicatiebeleid met de standaardinstellingen van 24 uur voor de bewaarperiode voor herstelpunten en ' 60 minuten voor de frequentie voor app-consistente momentopname te maken.

### <a name="what-is-crash-consistent-recovery-point"></a>Wat is een crash-consistent herstelpunt?
Crash-consistent herstelpunt vertegenwoordigt de gegevens op de schijf als de virtuele machine is vastgelopen of het netsnoer is opgehaald uit de server gelijktijdig met de momentopname werd gemaakt. Deze bevat geen iets dat in het geheugen was toen de momentopname werd gemaakt. Vandaag de dag kunnen de meeste toepassingen herstellen van crash-consistente momentopnamen. Een crash-consistente herstelpunt is meestal genoeg voor geen database-besturingssystemen en toepassingen, zoals bestandsservers, DHCP-servers en afdrukservers.

### <a name="what-is-application-consistent-recovery-point"></a>Wat is toepassingsconsistente herstelpunt uitgevoerd? 
Toepassingsconsistente herstelpunten zijn gemaakt op basis van app-consistente momentopnamen die dezelfde gegevens als crash-consistente momentopnamen, met de toevoeging van alle gegevens in het geheugen en alle lopende transacties vastleggen. Vanwege de extra inhoud toepassingsconsistente momentopnamen zijn het meest betrokken en nemen de langste om uit te voeren. Toepassingsconsistente herstelpunten worden aanbevolen voor database-besturingssystemen en toepassingen, zoals SQL.

### <a name="how-are-recovery-points-generated-and-saved"></a>Hoe worden herstelpunten die zijn gegenereerd en opgeslagen?
Als u wilt weten kunt hoe herstelpunten worden gegenereerd door Site Recovery nemen een voorbeeld van het replicatiebeleid waaraan de bewaarperiode van 24 uur en momentopname van de App-consistente frequentie van 1 uur voor herstelpunten.
Site Recovery maakt crash toepassingsconsistente punt elke 5 minuten en de gebruiker beschikt niet over een besturingselement te wijzigen van deze frequentie. Één uur durende gebruiker zal daarom voor de afgelopen 12 crash-consistente punten en 1 app toepassingsconsistente punt om de verkeersbelasting hebben. Als de tijd worden uitgevoerd, Site Recovery prunes alle herstelpunten buiten het afgelopen uur en alleen opslaan één recovery per uur wijst.
Ter illustratie wordt in de onderstaande schermafbeelding:


1. Voor minder dan afgelopen uur tijd zijn er herstelpunten met de frequentie van vijf minuten.
2. Voor meer dan het afgelopen uur zien we dat slechts één herstelpunt per uur worden bewaard.

  ![herstel punten generatie](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kan ik gegevens herstellen?
Het oudste herstelpunt dat u kunt gebruiken is dit 72 uur.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-there-is-no-recovery-points-generation-due-to-some-issue-for-more-than-24-hours-does-my-previous-recovery-points-will-be-pruned"></a>Wat gebeurt er als ik heb een replicatiebeleid van 24 uur en er geen punten recovery genereren vanwege een probleem opgetreden tijdens gedurende meer dan 24 uur is. Wordt mijn vorige recovery punten worden verwijderd?
Nee, Site Recovery blijft uw alle vorige herstelpunten in dit geval. 

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Nadat de replicatie is ingeschakeld op een virtuele machine, hoe kan ik wijzigen het replicatiebeleid? 
Ga naar de Site Recovery-kluis > Site Recovery-infrastructuur > replicatiebeleid. Selecteer beleid dat u wilt bewerken en de wijzigingen opslaan. Een wijziging wordt toegepast op alle bestaande replicaties te. 

### <a name="are-all-the-recovery-points-complete-copy-of-the-vm-or-differential"></a>Zijn alle herstelpunten volledige kopie van de virtuele machine of het verschil?
In het geval van initiële replicatie het eerste herstelpunt opgehaald die zijn gegenereerd hebben de volledige kopie en opeenvolgende herstelpunten deltawijzigingen hebben.

### <a name="does-increasing-recovery-points-retention-windows-increases-the-storage-cost"></a>Herstelpunten retentietijdvenster verhoogt de kosten voor opslag?
Ja, als u de bewaarperiode van 24 uur tot 72 uur verhoogt en Site Recovery wordt de herstelpunten voor toevoeging Sla 48 uur wordt in rekening gebracht u kosten voor opslag. Bijvoorbeeld als een enkel herstelpunt wijzigingen van 10 GB en kosten van GB per heeft is $0.16 per maand, en vervolgens het normaal zou extra kosten $1.6 * 48 per maand zijn.

## <a name="failover"></a>Failover

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?

Failover wordt niet automatisch uitgevoerd. Initiëren van failover met één klik in de portal of kunt u Site Recovery [PowerShell](azure-to-azure-powershell.md) dat een failover wordt geactiveerd. 

### <a name="can-i-retain-public-ip-address-after-failover"></a>Kan ik openbare IP-adres behouden na een failover?

Openbare IP-adres van de productietoepassing **kunnen niet worden behouden bij failover**. Workloads naar gebracht als onderdeel van failover-proces moet een openbaar IP-Azure-resource beschikbaar in de doelregio worden toegewezen. Deze stap kan worden gedaan handmatig of met herstelplannen is geautomatiseerd. Raadpleeg [artikel](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan) om toe te wijzen openbaar IP-adres met behulp van het herstelplan.  

### <a name="can-i-retain-private-ip-address-during-failover"></a>Kan ik persoonlijke IP-adres behouden tijdens de failover?
Ja, kunt u het privé IP-adres behouden. Wanneer u herstel na noodgevallen voor virtuele machines van Azure inschakelt maakt Site Recovery standaard doelresources op basis van resource-instellingen voor gegevensbron. Voor Azure VM's geconfigureerd met statische IP-adressen, Site Recovery wordt geprobeerd om in te richten van hetzelfde IP-adres voor de doel-VM, als deze zich niet in gebruik. Raadpleeg [artikel](site-recovery-retain-ip-azure-vm-failover.md) privé-IP-adres onder verschillende omstandigheden behouden.

### <a name="after-failover-the-server-does-not-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-with-a-new-ip-address"></a>Na een failover, heeft de server geen hetzelfde IP-adres als de bron-VM. Waarom is deze met een nieuw IP-adres toegewezen?

Site Recovery probeert voor het IP-adres op de beste vermogen op het moment van failover. Als deze is die worden uitgevoerd door een andere virtuele machine, wordt het volgende beschikbare IP-adres is ingesteld als het doel. Voor een volledige uitleg over hoe Site Recovery omgaat met adressering, [raadpleegt u dit artikel.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms)

### <a name="what-does-latestlowest-rpo-recovery-points-means"></a>Wat doet herstel van de meest recente (laagste RPO) verwijst betekent?
Deze optie worden eerst alle gegevens die is verzonden naar Site Recovery-service te maken van een herstelpunt voor elke virtuele machine voordat de failover wordt uitgevoerd naar het verwerkt. Deze optie biedt het laagste RPO (Recovery Point Objective), omdat de virtuele machine gemaakt nadat de failover heeft de gegevens die zijn gerepliceerd naar de Site Recovery wanneer de failover werd geactiveerd.

### <a name="does-latest-lowest-rpo-recovery-points-have-impact-on-failover-rto"></a>Is laatste (laagste RPO) herstelpunten invloed hebben op Failover RTO
Ja, als de Site Recovery verwerkt alle in behandeling gegevens voordat de failover wordt uitgevoerd, heeft deze optie hogere RTO in vergelijking met anderen.

### <a name="what-does-latest-processed-option-in-recovery-points-mean"></a>Wat doet laatste optie in recovery points gemiddelde verwerkt?
Deze optie wordt over alle virtuele machines in het plan naar het meest recente herstelpunt dat is verwerkt door Site Recovery. Als u wilt zien het meest recente herstelpunt voor een specifieke virtuele machine, Controleer laatste herstelpunten in de instellingen van de virtuele machine. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Als ik repliceer tussen twee Azure-regio's wat gebeurt er als mijn primaire regio een stroomstoring optreedt?
U kunt een failover wordt geactiveerd nadat de onderbreking. Site Recovery hoeft niet de connectiviteit van de primaire regio om uit te voeren van de failover.

## <a name="recovery-plan"></a>Plan voor herstel

### <a name="what-is-a-recovery-plan-"></a>Wat is een herstelplan?
Plannen voor herstel in Site Recovery gecoördineerd failoverherstel van virtuele machines. Dit maakt het herstel consistente wijze nauwkeurige, herhaalbare en geautomatiseerde. Een plan voor herstel worden de volgende behoeften voor de gebruiker:

- Het definiëren van een groep van virtuele machines die failover samen.
- Het definiëren van de afhankelijkheden tussen de virtuele machines zodat de toepassing correct weergegeven wordt.
- Het herstel, samen met aangepaste handmatige acties automatiseren zodat taken dan de failover van de virtuele machines kunnen ook worden bereikt.

[Meer informatie](site-recovery-create-recovery-plans.md) over plannen voor herstel.

### <a name="how-does-sequencing-is-achieved-in-a-recovery-plan"></a>Hoe kiest, wordt er sequentiëren is bereikt in een herstelplan?

In het herstelplan, kunt u meerdere groepen voor het bereiken van sequentiëren. Elke failover voor de groep op een tijdstip, wat betekent dat virtuele machines die deel van dezelfde groep uitmaken wordt er een failover samen gevolgd door een andere groep. Controleer het [model van de aanvraag met behulp van het herstelplan te gaan.](https://review.docs.microsoft.com/azure/site-recovery/recovery-plan-overview?branch=pr-en-us-61681#model-apps) 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Hoe vind ik de RTO bepaalt van een herstelplan?
Om te controleren of de RTO bepaalt van een herstelplan, failovertests uit het herstelplan te gaan en gaat u naar de Site Recovery-taken.
Zoals hieronder wordt weergegeven, SAP Test het herstelplan bijvoorbeeld 8 minuten 59 seconden nodig was voor failover met alle virtuele machines en voer eventuele acties die zijn opgegeven.

  ![COM-fout](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kan ik automation-runbooks aan het herstelplan toevoegen?
Ja, kunt u Azure automation-runbooks integreren in uw plan voor herstel. [Meer informatie](site-recovery-runbook-automation.md)

## <a name="reprotect-and-failback"></a>Opnieuw beveiligen en Failback 

### <a name="after-doing-a-failover-from-primary-region-to-disaster-recovery-region-does-vms-in-a-dr-region-gets-protected-automatically"></a>Nadat u een failover van primaire regio uitvoert naar de Dr-regio virtuele machines in een DR-regio heeft wordt automatisch beveiligd?
Nee, wanneer u [failover](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) Azure virtuele machines van de ene regio naar een andere, de virtuele machines opstarten in de DR-regio's in een niet-beveiligde status. Als u wilt uitvoeren van een failback de VM's naar de primaire regio, moet u [opnieuw beveiligen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) de virtuele machines in de secundaire regio.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-secondary-region-to-primary-region"></a>Op het moment van opnieuw beveiligen Site Recovery repliceert volledige gegevens van secundaire regio naar primaire regio?
Dat hangt ervan af op de situatie, bijvoorbeeld als de virtuele machine van de bronregio aanwezig wordt alleen de wijzigingen tussen de bronschijf en de doelschijf worden gesynchroniseerd. De verschillen zijn berekend door het vergelijken van beide schijven en vervolgens overgedragen. Dit meestal een paar uur duren. Raadpleeg [artikel]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) voor meer informatie over wat er tijdens het opnieuw beveiligen gebeurt.

### <a name="how-much-time-does-it-take-to-failback"></a>Hoeveel tijd doet het failback-proces uitvoeren?
Als opnieuw beveiligen is voltooid vervolgens is dit meestal de hoeveelheid tijd die vergelijkbaar zijn met failover van de primaire regio naar de secundaire regio. 

## <a name="security"></a>Beveiliging
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery gerepliceerde gegevens niet worden onderschept, en heeft geen informatie over wat wordt uitgevoerd op uw virtuele machines. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  
Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Ja, zowel versleuteling-in-transit en [versleuteling in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.

## <a name="next-steps"></a>Volgende stappen
* [Beoordeling](azure-to-azure-support-matrix.md) vereisten ondersteunen.
* [Instellen van](azure-to-azure-tutorial-enable-replication.md) replicatie van Azure naar Azure.
