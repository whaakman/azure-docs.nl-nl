---
title: 'Veelgestelde vragen: Herstel na noodgevallen van Azure naar Azure met Azure Site Recovery | Microsoft Docs'
description: In dit artikel bevat een overzicht van veelgestelde vragen bij het instellen van herstel na noodgevallen van virtuele Azure-machines naar een andere Azure-regio met behulp van Azure Site Recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: a277e392acb8587e05bb78d1d8dacce40bf91f56
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449551"
---
# <a name="common-questions-azure-to-azure-replication"></a>Veelgestelde vragen: Replicatie van Azure naar Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over het implementeren van herstel na noodgeval (DR) van Azure-VM's naar een andere Azure-regio met behulp van Azure Site Recovery. Als u vragen hebt na het lezen van dit artikel, plaatst u deze op de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Algemeen
### <a name="how-is-site-recovery-priced"></a>Hoe wordt de Site Recovery geprijsd?
Beoordeling [prijzen voor Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) details.

### <a name="what-are-the-best-practices-for-configuring-site-recovery-on-azure-vms"></a>Wat zijn de aanbevolen procedures voor het configureren van Site Recovery op Azure Virtual machines?
1. [Azure-naar-Azure-architectuur begrijpen](azure-to-azure-architecture.md)
2. [Controleer de ondersteunde en niet-ondersteunde configuraties](azure-to-azure-support-matrix.md)
3. [Herstel na noodgevallen instellen voor virtuele Azure-machines](azure-to-azure-how-to-enable-replication.md)
4. [Een testfailover uitvoeren](azure-to-azure-tutorial-dr-drill.md)
5. [Failover en failback uitvoeren naar de primaire regio](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>Kan ik repliceren van virtuele machines via Azure disk encryption ingeschakeld?
Ja, kunt u ze repliceren. Zie het artikel [repliceren Azure disk encryption ingeschakeld virtuele machines naar een andere Azure-regio](azure-to-azure-how-to-enable-replication-ade-vms.md). Azure Site Recovery ondersteunt momenteel alleen Azure-VM's die een Windows-besturingssysteem worden uitgevoerd en ingeschakeld voor de versleuteling met Azure Active Directory (Azure AD)-apps.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Kan ik virtuele machines repliceren naar een ander abonnement?
Ja, kunt u virtuele Azure-machines repliceren naar een ander abonnement binnen dezelfde Azure AD-tenant.
Configureren van herstel na Noodgevallen [voor abonnementen](https://azure.microsoft.com/blog/cross-subscription-dr) is eenvoudig. U kunt een ander abonnement op het moment van replicatie.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Kan ik zone vastgemaakt virtuele Azure-machines repliceren naar een andere regio?
Ja, u kunt [zone vastgemaakt VM's repliceren](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) naar een andere regio.

### <a name="can-i-exclude-disks"></a>Kan ik schijven uitsluiten?

Ja, kunt u schijven op het moment van beveiliging uitsluiten met behulp van PowerShell. Zie voor meer informatie de [PowerShell-richtlijnen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine).

### <a name="how-often-can-i-replicate-to-azure"></a>Hoe vaak kan ik repliceren naar Azure?
Replicatie is continue wanneer u virtuele Azure-machines naar een andere Azure-regio repliceert. Zie voor meer informatie de [Azure-naar-Azure-replicatiearchitectuur](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-to-migrate-vms"></a>Kan ik virtuele machines binnen een regio repliceren? Ik heb nodig dit voor het migreren van virtuele machines.
U kunt een Azure-naar-Azure-DR-oplossing niet gebruiken voor het repliceren van virtuele machines binnen een regio.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Kan ik virtuele machines repliceren naar een Azure-regio?
Met Site Recovery kunt u repliceren en herstellen van virtuele machines tussen elke twee regio's binnen hetzelfde geografische cluster. Geografische clusters worden met een gegevenslatentie en soevereiniteit waarmee u rekening moet gedefinieerd. Zie voor meer informatie, de Site Recovery [regio ondersteuningsmatrix](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Is Site Recovery voor verbinding met internet nodig?

Nee, Site Recovery is geen verbinding met internet vereist. Maar dit vereist toegang tot de Site Recovery-URL's en IP-adressen, zoals vermeld in [in dit artikel](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges).

## <a name="replication-policy"></a>Beleid voor replicatie

### <a name="what-is-a-replication-policy"></a>Wat is beleid voor replicatie?
Hiermee worden de instellingen voor de bewaargeschiedenis van herstelpunten en de frequentie van de app-consistente momentopnamen gedefinieerd. Azure Site Recovery maakt standaard een nieuw replicatiebeleid met de standaardinstellingen van:

* 24 uur voor de bewaargeschiedenis van herstelpunten.
* 60 minuten voor de frequentie van toepassingsconsistente momentopnamen.

[Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>Wat is een crash-consistente herstelpunt?
Een crash-consistente herstelpunt vertegenwoordigt de gegevens op de schijf als de virtuele machine is vastgelopen of het netsnoer is opgehaald uit de server gelijktijdig met de momentopname werd gemaakt. Deze bevat geen iets dat in het geheugen was toen de momentopname werd gemaakt. 

Vandaag de dag kunnen de meeste toepassingen herstellen van crash-consistente momentopnamen. Een crash-consistente herstelpunt is meestal genoeg voor niet-database-besturingssystemen en toepassingen, zoals bestandsservers, DHCP-servers en afdrukservers.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Wat is de frequentie van de crash-consistent herstelpunt punt generatie?
Site Recovery maakt een crash-consistente herstelpunt om de 5 minuten.

### <a name="what-is-an-application-consistent-recovery-point"></a>Wat is een toepassingsconsistente herstelpunt? 
Toepassingsconsistente herstelpunten zijn gemaakt op basis van toepassingsconsistente momentopnamen. Toepassingsconsistente momentopnamen kunt u dezelfde gegevens als crash-consistente momentopnamen, met de toevoeging van alle gegevens in het geheugen en alle lopende transacties vastleggen. 

Vanwege de extra inhoud toepassingsconsistente momentopnamen zijn het meest betrokken en nemen de langste om uit te voeren. Het wordt aangeraden de toepassingsconsistente herstelpunten voor database-besturingssystemen en toepassingen, zoals SQL Server.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Wat is de minimale frequentie van toepassingsconsistente recovery point generatie?
Site Recovery maakt een toepassingsconsistente herstelpunt met een minimale frequentie van binnen 1 uur.

### <a name="how-are-recovery-points-generated-and-saved"></a>Hoe worden herstelpunten die zijn gegenereerd en opgeslagen?
Om te begrijpen hoe herstelpunten in Site Recovery wordt gegenereerd, we nemen een voorbeeld van een replicatiebeleid waarvoor een herstelpunt bewaarperiode van 24 uur en een momentopname van een app-consistente frequentie van 1 uur.

Site Recovery maakt een crash-consistente herstelpunt om de 5 minuten. De gebruiker kan deze frequentie niet wijzigen. Zodat de gebruiker voor het afgelopen uur, 12 crash-consistente heeft wijzen punten en 1 app-consistente om uit te kiezen. Als de tijd worden uitgevoerd, Site Recovery prunes alle verwijst buiten het afgelopen uur van het herstelplan en slechts 1 recovery opgeslagen per uur punt.

De volgende schermafbeelding ziet u het voorbeeld. In de schermafbeelding:

1. Tijd van minder dan het afgelopen uur, zijn er herstelpunten met een frequentie van vijf minuten.
2. Site Recovery houdt gedurende de periode voorbij het afgelopen uur, slechts 1 herstelpunt.

  ![Lijst met gegenereerde herstelpunten](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Hoe ver terug kan ik gegevens herstellen?
Het oudste herstelpunt dat u kunt gebruiken is dit 72 uur.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>Wat gebeurt er als ik een beleid voor replicatie van 24 uur en een probleem wordt voorkomen dat Site Recovery genereren herstelpunten gedurende meer dan 24 uur? Mijn vorige herstelpunten verloren?
Nee, Site Recovery houdt uw vorige herstelpunten. Afhankelijk van de bewaarperiode voor herstelpunten van herstel, vervangt 24 uur in dit geval, Site Recovery oudste herstelpunt alleen als er een generatie van nieuwe punten. In dit geval, omdat er geen nieuw herstelpunt gegenereerd vanwege een probleem opgetreden tijdens, blijven de oude punten intact wanneer we het venster retentie bereiken.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Nadat de replicatie is ingeschakeld op een virtuele machine, hoe kan ik wijzigen het replicatiebeleid? 
Ga naar **Site Recovery-kluis** > **Site Recovery-infrastructuur** > **replicatiebeleid**. Selecteer het beleid dat u wilt bewerken en de wijzigingen niet opslaan. Een wijziging wordt toegepast op alle bestaande replicaties te. 

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Zijn alle herstelpunten voor een volledige kopie van de virtuele machine of een differentiële?
Het eerste herstelpunt dat gegenereerd, is de volledige kopie. Opeenvolgende herstelpunten hebben deltawijzigingen.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Is de bewaarperiode van herstelpunten vergroot de opslagkosten?
Ja. Als u de bewaarperiode van 24 uur tot 72 uur verhoogt, kan Site Recovery wordt de herstelpunten opslaan voor een aanvullende 48 uur. Storage-kosten in rekening worden gebracht door de tijd toegevoegd. Bijvoorbeeld, als een enkel herstelpunt deltawijzigingen van 10 GB heeft en de kosten per GB per maand $0.16 is, zou de extra kosten $1.6 * 48 per maand.

## <a name="multi-vm-consistency"></a>Multi-VM-consistentie 

### <a name="what-is-multi-vm-consistency"></a>Wat is een Multi-VM-consistentie?
Dit betekent dat ervoor te zorgen dat het herstelpunt dat consistent voor alle gerepliceerde virtuele machines is.
Site Recovery biedt een optie van ' Multi-VM-consistentie,' die, wanneer u selecteert, maakt een replicatiegroep om alle machines samen repliceren die deel uitmaken van de groep.
Alle virtuele machines hebben gedeelde crash-consistente en toepassingsconsistente herstelpunten wanneer ze bent failover.
Lees de zelfstudie voor [Multi-VM-consistentie inschakelen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication).

### <a name="can-i-failover-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Kan ik één virtuele machine in de replicatiegroep van een Multi-VM-consistentie failover?
Als u de optie "Multi-VM-consistentie" selecteert, worden u aangeeft dat de toepassing is afhankelijk van de virtuele machines binnen een groep. Daarom kan de failover van één virtuele machine is niet toegestaan. 

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Het aantal virtuele machines kan ik repliceren als onderdeel van de replicatiegroep van een Multi-VM-consistentie
U kunt 16 virtuele machines repliceren samen in een replicatiegroep.

### <a name="when-should-i-enable-multi-vm-consistency-"></a>Wanneer moet ik Multi-VM-consistentie inschakelen?
Omdat het CPU-intensieve, kan Multi-VM-consistentie inschakelen werkbelasting prestaties beïnvloeden. Het moet worden gebruikt als machines dezelfde werkbelasting worden uitgevoerd en u consistentie tussen meerdere computers. Als u twee SQL Server-exemplaren en twee webservers in een toepassing hebt, moet u bijvoorbeeld Multi-VM-consistentie voor de SQL Server-exemplaren alleen hebben.


## <a name="failover"></a>Failover

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?

Failover wordt niet automatisch uitgevoerd. U start failover met één klik in de portal of kunt u [PowerShell](azure-to-azure-powershell.md) dat een failover wordt geactiveerd. 

### <a name="can-i-retain-a-public-ip-address-after-failover"></a>Kan ik een openbaar IP-adres behouden na een failover?

Het openbare IP-adres van de productietoepassing *kunnen niet worden behouden bij failover*. Workloads naar gebracht als onderdeel van het failoverproces moet een Azure openbare IP-resource die beschikbaar is in de doelregio worden toegewezen. U kunt deze stap handmatig doen of automatiseren via een plan voor herstel. Als u wilt een openbaar IP-adres toewijzen met behulp van een herstelplan, Zie [openbare IP-adressen instellen na een failover](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan).  

### <a name="can-i-retain-a-private-ip-address-during-failover"></a>Kan ik een privé IP-adres behouden tijdens de failover?
Ja, kunt u een privé IP-adres behouden. Bij het inschakelen van herstel na Noodgeval voor virtuele machines van Azure maakt Site Recovery standaard doelresources op basis van resource-instellingen voor gegevensbron. Voor Azure VM's geconfigureerd met statische IP-adressen, Site Recovery wordt geprobeerd om in te richten van hetzelfde IP-adres voor de doel-VM, als deze zich niet in gebruik. Als u wilt behouden privé-IP-adres onder verschillende omstandigheden, Zie [behouden IP-adressen tijdens de failover](site-recovery-retain-ip-azure-vm-failover.md).

### <a name="after-failover-the-server-doesnt-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-a-new-ip-address"></a>Na een failover hebben niet de server hetzelfde IP-adres als de bron-VM. Waarom wordt er een nieuw IP-adres toegewezen?

Site Recovery probeert te bieden van het IP-adres op het moment van failover. Als een andere virtuele machine dit adres duurt, wordt de eerstvolgende beschikbare IP-adres in Site Recovery ingesteld als het doel. Zie voor een volledige uitleg over hoe Site Recovery omgaat met adressering [instellen van netwerktoewijzing en IP-adressen voor virtuele netwerken](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms).

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>Wat zijn **nieuwste (laagste RPO)** herstelpunten?
De **nieuwste (laagste RPO)** optie verwerkt eerst alle gegevens die is verzonden naar de Site Recovery-service te maken van een herstelpunt voor elke virtuele machine voordat de failover wordt uitgevoerd naar deze. Deze optie biedt het laagste beoogde herstelpunt (RPO), omdat de virtuele machine gemaakt nadat failover de gegevens die zijn gerepliceerd naar de Site Recovery is wanneer de failover werd geactiveerd.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>Voer **nieuwste (laagste RPO)** herstelpunten die invloed hebben op de failover RTO?
Ja. Site Recovery verwerkt alle in behandeling gegevens voordat de failover-overschakeling uitvoeren, zodat deze optie een hogere beoogde hersteltijd (RTO) in vergelijking met andere opties heeft.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Wat doet de **laatst verwerkte** optie bij het herstellen van gemiddelde verwijst?
De **laatst verwerkt** optie mislukt op alle virtuele machines in het plan naar het laatste herstelpunt dat Site Recovery verwerkte verwijzen. Raadpleeg het meest recente herstelpunt voor een specifieke virtuele machine, **laatste herstelpunten** in de instellingen van de virtuele machine. Deze optie biedt een lage RTO, omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Als ik repliceer tussen twee Azure-regio's, wat gebeurt er als mijn primaire regio een stroomstoring optreedt?
U kunt een failover wordt geactiveerd nadat de onderbreking. Site Recovery hoeft niet de connectiviteit van de primaire regio om uit te voeren van de failover.

### <a name="what-is-a-rto-of-a-virtual-machine-failover-"></a>Wat is een RTO van een virtuele machine failover?
Site Recovery heeft een [RTO SLA van twee uur](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/). De meeste van de tijd, mislukt Site Recovery echter failover van virtuele machines binnen enkele minuten. U kunt de RTO berekenen door te gaan naar de failover taken waarin de tijd die nodig was voor de virtuele machine openen. Plan voor herstel RTO, raadpleegt u onderstaande sectie. 

## <a name="recovery-plan"></a>Herstelplan

### <a name="what-is-a-recovery-plan"></a>Wat is een herstelplan?
Een plan voor herstel in Site Recovery coördineert het failoverherstel van virtuele machines. Dit maakt het herstel consistente wijze nauwkeurige, herhaalbare en geautomatiseerde. Een plan voor herstel worden de volgende behoeften voor de gebruiker:

- Een groep van virtuele machines die samen een failover definiëren
- De afhankelijkheden tussen virtuele machines definiëren zodat de toepassing correct weergegeven wordt
- Het herstel, samen met aangepaste handmatige acties voor taken anders dan de failover van virtuele machines te automatiseren

[Meer informatie](site-recovery-create-recovery-plans.md) over plannen voor herstel.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Hoe wordt de sequentiëren van een herstelplan te gaan bereikt?

U kunt meerdere groepen voor het bereiken van sequentiëren maken in een plan voor herstel. Elke groep failover in één keer wordt uitgevoerd. Virtuele machines die deel van de dezelfde groep mislukt uitmaken via samen, gevolgd door een andere groep. Zie voor informatie over het model van een toepassing met behulp van een herstelplan, [over plannen voor herstel](recovery-plan-overview.md#model-apps). 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Hoe vind ik de RTO bepaalt van een herstelplan?
Om te controleren of de RTO bepaalt van een herstelplan, voer een failovertest uit voor het herstelplan te gaan en gaat u naar **Site Recovery-taken**.
In het volgende voorbeeld heeft de taak met de naam SAPTestRecoveryPlan 8 minuten en 59 seconden failover van alle virtuele machines en specifieke acties uitvoeren.

![Lijst met Site Recovery-taken](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kan ik automation-runbooks toevoegen aan het herstelplan te gaan?
Ja, kunt u Azure Automation-runbooks integreren in uw plan voor herstel. [Meer informatie](site-recovery-runbook-automation.md).

## <a name="reprotection-and-failback"></a>Opnieuw beveiligen en failback 

### <a name="after-a-failover-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Na een failover van de primaire regio naar een Dr-regio, zijn virtuele machines in een DR-regio automatisch beveiligd?
Nee. Wanneer u [failover](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) Azure-VM's van de ene regio naar een andere, de virtuele machines starten in de DR-regio's in een niet-beveiligde status. Als u wilt uitvoeren van een failback de VM's naar de primaire regio, moet u [opnieuw beveiligen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) de virtuele machines in de secundaire regio.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Op het moment van opnieuw beveiligen, Site Recovery repliceert volledige gegevens van de secundaire regio naar de primaire regio?
Dat hangt ervan af op de situatie. Bijvoorbeeld, als de bronregio VM bestaat, zijn alleen de wijzigingen tussen de bronschijf en de doelschijf gesynchroniseerd. Site Recovery de verschillen worden berekend door het vergelijken van de schijven en vervolgens worden de gegevens worden overgebracht. Dit proces duurt normaal gesproken een paar uur. Zie voor meer informatie over wat er tijdens het opnieuw beveiligen gebeurt [Azure-VM's opnieuw beveiligen een failover naar de primaire regio]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-fail-back"></a>Hoeveel tijd doet het allemaal voor failback van toets maken?
Na het opnieuw beveiligen is de hoeveelheid tijd voor failback meestal vergelijkbaar met de tijd voor de failover van de primaire regio naar een secundaire regio. 

## <a name="security"></a>Beveiliging
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery biedt geen gerepliceerde gegevens worden onderschept en er geen informatie over wat wordt uitgevoerd op uw virtuele machines. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  
Site Recovery is ISO 27001: 2013, 27018, HIPAA, DPA gecertificeerd en wordt momenteel SOC2 en FedRAMP JAB-beoordelingen.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Ja, zowel versleuteling-in-transit en [versleuteling in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) worden ondersteund.

## <a name="next-steps"></a>Volgende stappen
* [Beoordeling](azure-to-azure-support-matrix.md) vereisten ondersteunen.
* [Instellen van](azure-to-azure-tutorial-enable-replication.md) replicatie van Azure naar Azure.
