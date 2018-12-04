---
title: Opnieuw beveiligen van virtuele machines van Azure naar een on-premises site tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers | Microsoft Docs
description: Leer hoe u failback van Azure naar de on-premises site na een failover naar Azure tijdens het herstel na noodgevallen van virtuele VMware-machines en fysieke servers.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: e965848b0c3c009444762dafdf42acc080b6915e
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834946"
---
# <a name="reprotect-and-fail-back-machines-to-an-on-premises-site-after-failover-to-azure"></a>Opnieuw beveiligen en mislukt de back-machines naar een on-premises site na een failover naar Azure

Na [failover](site-recovery-failover.md) van on-premises VMware-machines of fysieke servers naar Azure, de eerste stap in mislukte terug naar uw on-premises site is de Azure-VM's die zijn gemaakt tijdens de failover opnieuw beveiligen. Dit artikel wordt beschreven hoe u dit doet. 

Bekijk de volgende video over het failover van Azure naar een on-premises site voor een snel overzicht.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="before-you-begin"></a>Voordat u begint

Als u een sjabloon voor het maken van uw virtuele machines, zorg ervoor dat elke virtuele machine heeft een eigen UUID voor de schijven gebruikt. Als de UUID van de on-premises virtuele machine veroorzaakt een met de UUID van het hoofddoel conflict omdat beide zijn gemaakt op basis van dezelfde sjabloon, mislukt de opnieuw beveiligen. Een andere hoofddoelserver is niet op basis van dezelfde sjabloon gemaakt implementeren. Let op de volgende informatie:
- Als u wilt een failback uitvoeren naar een alternatieve vCenter, zorg ervoor dat worden uw nieuwe vCenter en de hoofddoelserver gedetecteerd. Een typische symptoom is dat de gegevensopslag niet toegankelijk zijn of die niet zichtbaar in de **opnieuw beveiligen** in het dialoogvenster.
- Als u wilt repliceren naar on-premises, moet u een beleid voor failback. Dit beleid wordt automatisch gemaakt wanneer u een beleid vooruit maakt. Let op de volgende informatie:
    - Dit beleid is automatisch gekoppeld aan de configuratieserver tijdens het maken van.
    - Dit beleid niet meer worden bewerkt.
    - De setwaarden van het beleid zijn (RPO-drempelwaarde = 15 minuten, bewaarperiode van het herstelpunt = 24 uur, App consistentie van de frequentie van momentopname = 60 minuten).
- Tijdens het opnieuw beveiligen en failback moet de on-premises configuratieserver actief en verbonden.
- Als de virtuele machines waarop u moet een failback wordt beheerd door een vCenter-server, zorg ervoor dat u hebt de [vereiste machtigingen](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) voor detectie van virtuele machines op de vCenter-servers.
- Verwijder de momentopnamen op de hoofddoelserver voordat u opnieuw beveiligen. Als u momentopnamen aanwezig op het hoofddoel van on-premises of op de virtuele machine zijn, wordt opnieuw beveiligen mislukt. De momentopnamen op de virtuele machine worden automatisch samengevoegd tijdens een taak opnieuw beveiligen.
- Alle virtuele machines van een replicatiegroep moet van het hetzelfde besturingssysteemtype (alle Windows- of alle Linux). Een replicatiegroep met gemengde besturingssystemen op dit moment wordt niet ondersteund voor opnieuw beveiligen en failback naar on-premises. Dit is omdat het hoofddoel van hetzelfde besturingssysteem als de virtuele machine moet zijn. Alle virtuele machines van een replicatiegroep moet de dezelfde hoofddoelserver hebben. 
- Een configuratieserver is vereiste on-premises als u failback. Tijdens een failback wordt bestaan de virtuele machine op de configuratie van server-database. Anders wordt de failback is mislukt. Zorg ervoor dat u regelmatig geplande back-ups van uw configuratieserver. Als er een noodgeval, de server met hetzelfde IP-adres herstellen zodat failback werkt.
- Opnieuw beveiligen en failback moet een site-naar-site (S2S) VPN om gegevens te repliceren. Geef het netwerk zodat de failover-virtuele machines in Azure (ping) de configuratie van de on-premises server kunnen bereiken. Ook kunt u een processerver in het Azure-netwerk van de virtuele failover-machine implementeren. Ook moet deze processerver communiceren met de on-premises configuratieserver.
- Zorg ervoor dat u de volgende poorten voor failover en failback openen:

    ![Poorten voor failover en failback](./media/vmware-azure-reprotect/failover-failback.png)

- Lees alle de [vereisten voor poorten en URL in de whitelist](vmware-azure-deploy-configuration-server.md#prerequisites).

## <a name="deploy-a-process-server-in-azure"></a>Een processerver in Azure implementeren

Mogelijk moet u een processerver in Azure voordat u een failback naar uw on-premises site:
- De processerver ontvangt gegevens van de beveiligde virtuele machine in Azure, en verzendt die gegevens vervolgens naar de on-premises site.
- Een netwerk met lage latentie is vereist tussen de processerver en de beveiligde virtuele machine. In het algemeen moet u rekening houden met latentie bij het bepalen of u een processerver in Azure moet:
    - Als u een Azure ExpressRoute-verbinding instellen hebt, kunt u een on-premises processerver om gegevens te verzenden omdat de latentie tussen de virtuele machine en de processerver laag is.
    - Echter, als u alleen een S2S-VPN hebt, raden wij de processerver in Azure implementeert.
    - Het is raadzaam om met behulp van een Azure-processerver tijdens failback. De prestaties van de replicatie is hoger als de processerver dichter bij de replicerende virtuele machine (de failover-machine in Azure is). Voor een concepttest kunt u de processerver on-premises en ExpressRoute om persoonlijke peering.

Een processerver in Azure implementeren:

1. Als u een processerver in Azure implementeren wilt, Zie [instellen van een processerver in Azure voor failback](vmware-azure-set-up-process-server-azure.md).
2. De virtuele Azure-machines verzenden replicatiegegevens naar de processerver. Netwerken zodanig configureren dat de processerver door de Azure VM's bereiken kan.
3. Houd er rekening mee dat replicatie van Azure naar on-premises kan gebeuren alleen via de S2S VPN-verbinding of de persoonlijke peering van het ExpressRoute-netwerk. Zorg ervoor dat er voldoende bandbreedte beschikbaar via dat netwerkkanaal is.

## <a name="deploy-a-separate-master-target-server"></a>Implementeren van een afzonderlijke hoofddoelserver

De hoofddoelserver ontvangt failbackgegevens. De master target-server wordt standaard uitgevoegd op de on-premises configuratieserver. Echter, afhankelijk van de hoeveelheid verkeer is mislukt-back, u mogelijk wilt maken van een afzonderlijke hoofddoelserver voor failback. Dit is het maken van een:

* [Maken van een Linux-hoofddoelserver](vmware-azure-install-linux-master-target.md) voor failback van virtuele Linux-machines. Dit is vereist.
* (Optioneel) Maak een afzonderlijke hoofddoelserver voor failback van de Windows-VM. U doet dit door geïntegreerde Setup opnieuw uitvoeren en selecteren om te maken van een hoofddoelserver. [Meer informatie](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers).

Nadat u een hoofddoelserver maakt, moet u de volgende taken uitvoeren:

- Als de virtuele machine aanwezig on-premises op de vCenter-server wordt, moet de hoofddoelserver toegang tot de virtuele Machine-schijf (VMDK)-bestand van de on-premises virtuele machine. Toegang is vereist voor de gerepliceerde gegevens schrijven naar de schijven van de virtuele machine. Zorg ervoor dat de gegevensopslag van de on-premises virtuele machine is gekoppeld op het hoofddoel host met toegang voor lezen/schrijven.
- Als de virtuele machine niet aanwezig zijn on-premises op de vCenter-server, moet de Site Recovery-service te maken van een nieuwe virtuele machine tijdens het opnieuw beveiligen. Deze virtuele machine wordt gemaakt op de ESX-host waarop u het hoofddoel maakt. Kies de ESX-host zorgvuldig, zodat de virtuele failback-machine wordt gemaakt op de host die u wilt.
- U kunt Storage vMotion niet gebruiken voor de hoofddoelserver. Met Storage vMotion voor de hoofddoelserver kan leiden tot het failback-bewerking mislukt. De virtuele machine starten niet omdat de schijven zijn niet beschikbaar voor deze. Om dit te voorkomen, sluit u de hoofddoelservers uit van uw vMotion-lijst.
- Als een hoofddoelserver een opslag-vMotion-taak na het opnieuw beveiligen ondergaat, wordt de beveiligde virtuele machine-schijven die zijn gekoppeld aan het hoofddoel migreren naar het doel van de taak vMotion. Als u na deze mislukken, wordt loskoppelen van de schijf mislukt omdat de schijven zijn niet gevonden. Er wordt vervolgens moeilijk te vinden van de schijven in uw storage-accounts. U moet deze handmatig zoeken en deze koppelt aan de virtuele machine. Daarna kan de on-premises virtuele machine worden opgestart.
- Bewaarstation toevoegen aan uw bestaande Windows-hoofddoelserver. Voeg een nieuwe schijf toe en formatteren van de schijf. De retentieschijf wordt gebruikt voor het stoppen van de punten in de tijd wanneer de virtuele machine terug naar de on-premises site worden gerepliceerd. Hieronder vindt u enkele criteria bewaarstation. Als aan deze criteria worden niet voldaan, wordt het station wordt niet weergegeven voor de hoofddoelserver:
    - Het volume wordt niet gebruikt voor andere doeleinden, zoals een doel van replicatie.
    - Het volume is niet in de vergrendelingsmodus bevinden.
    - Het volume is niet een cachevolume. De installatie van de hoofddoelserver kan niet bestaan op het desbetreffende volume. Het aangepaste installatievolume voor de doel-server en -master proces niet in aanmerking voor een volume bewaren. Wanneer de processerver en hoofddoelserver worden geïnstalleerd op een volume, is het volume een cachevolume van het hoofddoel.
    - Het type bestandssysteem van het volume is niet FAT of FAT32.
    - De capaciteit van het volume gelijk is aan nul.
    - Het standaardbewaarvolume voor Windows is het R-volume.
    - Het standaardbewaarvolume voor Linux is/mnt/Retention.
- Als u een bestaande proces serverconfiguratie/server-machine of een schaal of proces server/master target servermachine, moet u een nieuw station toevoegen. Het nieuwe station moet voldoen aan deze vereisten. Als de retentieschijf niet aanwezig is, wordt het niet weergegeven in de vervolgkeuzelijst selecteren in de portal. Wanneer u een station aan de on-premises hoofddoelserver toevoegen, duurt het maximaal 15 minuten voor het station moet worden weergegeven in de selectie in de portal. U kunt ook de configuratieserver vernieuwen als het station niet wordt weergegeven na 15 minuten.
- VMware-hulpprogramma's of open-vm-hulpprogramma's installeren op de hoofddoelserver. Zonder de hulpprogramma's, kan niet de gegevensopslag op van het hoofddoel ESXi-host worden gedetecteerd.
- Stel de `disk.EnableUUID=true` instellen in de configuratieparameters van de hoofddoelserver virtuele machine in VMware. Als deze rij niet bestaat, deze toevoegen. Deze instelling is vereist voor een consistente UUID voor de VMDK zodat deze correct koppelt.
- De ESX-host waarop de hoofddoelserver wordt gemaakt, moet ten minste één virtuele machine file system (VMFS) gegevensopslag die is gekoppeld aan deze hebben. Als er geen gegevensopslag VMFS zijn gekoppeld, de **gegevensopslag** invoer op de pagina opnieuw beveiligen is leeg en kan niet worden voortgezet.
- De hoofddoelserver geen momentopnamen op de schijven. Als er momentopnamen, mislukt opnieuw beveiligen en failback.
- De hoofddoelserver kan niet een Paravirtual SCSI-controller hebben. De controller mag alleen bestaan uit een LSI Logic-controller. Zonder een domeincontroller LSI Logic opnieuw beveiligen is mislukt.
- Voor elk exemplaar, kan het hoofddoel maximaal 60 schijven die zijn gekoppeld aan hebben. Als het aantal virtuele machines opnieuw aan de on-premises hoofddoelserver wordt beveiligd met meer dan een totaal van 60 schijven aan de hoofddoelserver reprotects beginnen mislukken. Zorg ervoor dat u hebt onvoldoende master zich schijf sleuven of extra hoofddoelservers implementeren.
    

## <a name="enable-reprotection"></a>Opnieuw beveiligen inschakelen

Wanneer een virtuele machine wordt opgestart in Azure, duurt het even voordat de agent te registreren naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd kunt u het niet mogelijk om opnieuw te beveiligen en een foutmelding die aangeeft dat de agent is niet geïnstalleerd. Als dit het geval is, wacht een paar minuten en probeer het vervolgens opnieuw opnieuw beveiligen:


1. Selecteer **kluis** > **gerepliceerde items**. Met de rechtermuisknop op de virtuele machine die een failover, en selecteer vervolgens **opnieuw beveiligen**. Of, vanaf de knoppen, selecteert u de machine en selecteer vervolgens **opnieuw beveiligen**.
2. Controleer de **Azure naar On-premises** richting van beveiliging is ingeschakeld.
3. In **Masterdoelserver** en **processerver**, selecteert u de on-premises hoofddoelserver en de processerver.  
4. Voor **gegevensopslag**, selecteert u het waarnaar u herstellen van de schijven wilt on-premises gegevensarchief. Deze optie wordt gebruikt wanneer de on-premises virtuele machine wordt verwijderd en moet u nieuwe schijven te maken. Deze optie wordt genegeerd als de schijven al bestaan. U moet wel een waarde op te geven.
5. Selecteer het bewaarstation.
6. Het failback-beleid wordt automatisch geselecteerd.
7. Selecteer **OK** om te beginnen met opnieuw beveiligen. Er wordt een taak gestart voor het repliceren van de VM van Azure naar de on-premises site. U kunt de voortgang volgen op het tabblad **Taken**. Wanneer het opnieuw beveiligen is gelukt, krijgt de virtuele machine een beveiligde status.

Let op de volgende informatie:
- Als u wilt herstellen naar een alternatieve locatie (wanneer de on-premises virtuele machine wordt verwijderd), selecteer het bewaarstation en gegevensopslag die zijn geconfigureerd voor de hoofddoelserver. Wanneer u een failback naar de on-premises site, de virtuele VMware-machines in de failback-beschermingsplan dezelfde gegevensopslag gebruiken als de hoofddoelserver. Een nieuwe virtuele machine wordt vervolgens gemaakt in vCenter.
- Als u herstellen van de virtuele machine op Azure aan een bestaande on-premises virtuele machine wilt, koppelt u de gegevensopslag van de on-premises virtuele machine met lees-/ schrijftoegang op de hoofddoelserver van ESXi-host.

    ![In het dialoogvenster opnieuw beveiligen](./media/vmware-azure-reprotect/reprotectinputs.png)

- U kunt ook opnieuw beveiligen op het niveau van een plan voor herstel. Een replicatiegroep kan opnieuw worden beveiligd alleen via een plan voor herstel. Wanneer u opnieuw beveiligen met behulp van een herstelplan, moet u de waarden opgeven voor elke beveiligde computer.
- Gebruik de dezelfde hoofddoelserver om een replicatiegroep opnieuw te beveiligen. Als u een andere hoofddoelserver gebruikt om opnieuw te beveiligen van een replicatiegroep, kan de server kan geen gemeenschappelijk tijdstip bieden in-time.
- De on-premises virtuele machine is altijd uitgeschakeld tijdens het opnieuw beveiligen. Dit helpt ervoor te zorgen dat gegevens tijdens de replicatie consistent blijven. Op de virtuele machine niet inschakelt nadat het opnieuw beveiligen is voltooid.



## <a name="common-issues"></a>Algemene problemen

- Site Recovery ondersteunt momenteel alleen met een gegevensarchief VMFS of virtueel SAN failback. Een NFS-gegevensopslag wordt niet ondersteund. Vanwege deze beperking, de invoer van de selectie van gegevensopslag op het scherm opnieuw beveiligen is leeg voor NFS-gegevensopslag, of het gegevensarchief virtueel SAN bevat, maar tijdens de taak is mislukt. Als u een failback uitvoeren wilt, kunt u een gegevensarchief VMFS on-premises maken en een failback naar deze. Deze failback zorgt ervoor dat een volledige download van de VMDK.
- Als u een alleen-lezengebruiker vCenter-detectie uitvoeren en virtuele machines beveiligt, beveiliging is geslaagd en uitvoeren van failovers werkt. Tijdens het opnieuw beveiligen mislukt de failover omdat de gegevensopslag kan niet worden gedetecteerd. Een symptoom is dat de gegevensopslag worden niet tijdens het opnieuw beveiligen weergegeven. U lost dit probleem, kunt u de vCenter-referenties bijwerken met een juiste account dat machtigingen heeft en en probeer het opnieuw. 
- Wanneer u een failback van virtuele Linux-machine en on-premises uitvoert, kunt u zien dat het pakket Network Manager is verwijderd van de machine. Deze verwijdering doet zich voor omdat het pakket Network Manager wordt verwijderd wanneer de virtuele machine is hersteld in Azure.
- Wanneer een virtuele Linux-machine is geconfigureerd met een statisch IP-adres en een failover naar Azure, wordt het IP-adres verkregen via DHCP. Als u een failover naar on-premises, blijft de virtuele machine het IP-adres verkrijgen via DHCP. Handmatig aanmelden bij de machine en stel het IP-adres terug naar een statisch adres indien nodig. Een Windows virtuele machine kunt opnieuw het statische IP-adres verkrijgen.
- Als u de gratis versie van de ESXi 5.5 of de gratis versie van vSphere 6 Hypervisor gebruikt, de failover wordt voltooid, maar failback niet lukt. Upgraden naar de evaluatielicentie van beide programma zodat failback.
- Als de configuratieserver van de processerver kan niet worden bereikt, kunt u Telnet gebruiken om te controleren op connectiviteit met de configuratieserver op poort 443. U kunt ook de configuratieserver van de processerver te pingen. Een processerver hebt ook een heartbeat wanneer deze verbonden met de configuratieserver.
- Een Windows Server 2008 R2 SP1-server die is beveiligd als een fysieke on-premises server kan niet worden mislukt van Azure naar een on-premises site.
- U kunt geen failback uitvoeren terug in de volgende omstandigheden:
    - U machines hebt gemigreerd naar Azure. [Meer informatie](migrate-overview.md#what-do-we-mean-by-migration).
    - U hebt verplaatst een virtuele machine naar een andere resourcegroep.
    - U hebt de Azure-VM hebt verwijderd.
    - U de beveiliging van de virtuele machine uitgeschakeld.
    - Maken van de VM handmatig in Azure. De machine moet zijn in eerste instantie beveiligde on-premises en failover naar Azure voordat opnieuw beveiligen.
    - U kunt niet alleen op een ESXi-host. U kunt geen failback VMware-machines of fysieke servers naar Hyper-V-hosts, fysieke machines of VMware-werkstations.


## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine heeft een beveiligde status ingevoerd, kunt u [een failback initieert](vmware-azure-failback.md). De failback wordt afgesloten met de virtuele machine in Azure en de on-premises virtuele machine wordt opgestart. Verwacht dat de enige uitvaltijd voor de toepassing. Kies een tijd voor failback wanneer de toepassing downtime tolereren kan.


