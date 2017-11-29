---
title: Beveiligt van Azure naar een on-premises site | Microsoft Docs
description: U kunt een failback naar on-premises om virtuele machines starten na een failover van virtuele machines naar Azure. Informatie over het opnieuw te beveiligen voordat u een failback.
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: 17a43de3faaa3a146fa9d8f43d36545d6d82b274
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="reprotect-from-azure-to-an-on-premises-site"></a>Beveiligt van Azure naar een on-premises site



## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe beveiligt Azure virtuele machines van Azure naar een on-premises site. Volg de instructies in dit artikel wanneer u klaar bent, mislukken back-uw virtuele VMware-machines of Windows of Linux fysieke servers nadat ze hebt failover van de on-premises site naar Azure (zoals beschreven in [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-failover.md)).

> [!WARNING]
> U kunt geen failback nadat u een hebt [migratie voltooid](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), een virtuele machine naar een andere resourcegroep verplaatst of verwijderd van virtuele machine van Azure. Als u de beveiliging van de virtuele machine uitschakelt, maar u kunt geen failback. Als de virtuele machine is gemaakt in Azure (geboren in de cloud) vervolgens u kan niet opnieuw beveiligen naar on-premises. De machine moet zijn in eerste instantie beveiligde lokale en een failover naar Azure voordat opnieuw beveiligen.


Nadat de beveiligingspoging is voltooid en de beveiligde virtuele machines worden gerepliceerd, kunt u een failback op de virtuele machines zodat deze naar de lokale site starten.

> [!NOTE]
> U kunt alleen beveiligt en failback naar een ESXi-host. U kunt geen failback van de virtuele machine in Hyper-v-hosts of VMware-werkstations of andere virtualisatieplatform.

Opmerkingen of vragen plaatsen aan het einde van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Bekijk de volgende video over het failover van Azure naar een on-premises site voor een snel overzicht.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Tijdens de failover naar Azure, de lokale site mag niet toegankelijk is en kan daarom de configuratieserver zijn niet beschikbaar of afsluiten. Tijdens het beveiligt en failback moet de lokale configuratie-server actief en een verbonden status OK hebben.


Wanneer u beveiligt virtuele machines voorbereidt, uitvoeren of kunt u de volgende vereiste acties overwegen:

* Als een vCenter-server de virtuele machines die u failback wilt naar beheert, zorgt u ervoor dat u hebt de [machtigingen vereist](site-recovery-vmware-to-azure-classic.md) voor de detectie van virtuele machines op de vCenter-servers.

  > [!WARNING]
  > Als u momentopnamen aanwezig zijn op het hoofddoel lokale of de virtuele machine, mislukt de beveiligingspoging. Voordat u doorgaat naar beveiligt, kunt u de momentopnamen op het hoofddoel verwijderen. De momentopnamen op de virtuele machine worden automatisch samengevoegd tijdens een taak opnieuw beveiligen.

* Voordat u een failback uit op, maakt u twee extra onderdelen:

  * **Processerver**: de [processerver](site-recovery-vmware-setup-azure-ps-resource-manager.md) ontvangt gegevens van de beveiligde virtuele machine in Azure en verzendt gegevens naar de lokale site. Een traag netwerk is vereist tussen de processerver en de beveiligde virtuele machine. U kunt dus een on-premises processerver hebben als u een Azure ExpressRoute-verbinding of een op basis van een Azure-processerver en een VPN.
  
  * **Hoofddoelserver**: de hoofddoelserver ontvangt gegevens van failback. De lokale management-server die u hebt gemaakt, heeft een hoofddoelserver standaard geïnstalleerd. Afhankelijk van het volume is mislukt-back-verkeer moet u mogelijk echter maken van een afzonderlijke hoofddoelserver voor failback.
    * [Een virtuele Linux-machine moet een Linux-hoofddoelserver](site-recovery-how-to-install-linux-master-target.md).
    * Een virtuele Windows-computer moet een Windows-hoofddoelserver. U kunt de lokale proces server en master doelmachines opnieuw gebruiken.
    * Het hoofddoel heeft andere vereisten die worden vermeld in [algemene wat u moet controleren op een hoofddoel voordat beveiligt](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

> [!NOTE]
> Alle virtuele machines van een replicatiegroep moeten van hetzelfde besturingssysteem type (alle Windows of Linux alle). Een replicatiegroep met gemengde besturingssystemen wordt momenteel niet ondersteund voor beveiligt en failback naar on-premises. Dit is omdat het hoofddoel van hetzelfde besturingssysteem als de virtuele machine moet en de virtuele machines van een replicatiegroep moet het dezelfde hoofddoel hebben. 

    

* Een configuratieserver is vereist on-premises wanneer u een failback uit. Tijdens de failback, moet de virtuele machine zich in de configuratiedatabase van de server. Anders wordt de failback is mislukt. 

> [!IMPORTANT]
> Zorg ervoor dat u rekening houden met regelmatig geplande back-ups van uw configuratieserver. Als er een ramp, de server met hetzelfde IP-adres herstellen zodat failback werkt.

> [!WARNING]
> Een replicatiegroep mag alleen hebben VM's van Windows of Linux VM's en niet een combinatie van beide omdat alle virtuele machines in een groep replictaion maakt gebruik van dezelfde hoofddoelserver en Linux-VM is een server voor Linux-hoofddoel vereist en verstandig, zoals voor de virtuele machine van Windows.

* Stel de `disk.EnableUUID=true` instellen in de configuratieparameters van het hoofddoel virtuele machine in VMware. Als deze rij niet bestaat, moet u deze toevoegen. Deze instelling is vereist voor een consistente UUID naar de schijf van de virtuele machine (VMDK) bieden, zodat deze correct koppelt.

* *U kunt Storage vMotion niet gebruiken op de hoofddoelserver*. Hierdoor kan de failback mislukken. De virtuele machine starten niet omdat de schijven niet beschikbaar voor deze zijn. U voorkomt dit probleem, uitsluiten van de master doelservers uw lijst met vMotion.

* Voeg een nieuwe schijf toe aan de hoofddoelserver: een station bewaren. Voeg een nieuwe schijf en formatteren van de schijf.


### <a name="frequently-asked-questions"></a>Veelgestelde vragen

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-to-replicate-data-back-to-the-on-premises-site"></a>Waarom moet ik een S2S-VPN- of een ExpressRoute-verbinding om gegevens te repliceren naar de lokale site
Terwijl de replicatie van on-premises naar Azure kan gebeuren via Internet of een ExpressRoute-verbinding met openbare peering, beveiligingspoging en failback vereisen van een site-naar-site (S2S) VPN om gegevens te repliceren. Geef het netwerk zodat de failover virtuele machines in Azure (ping) de configuratie van de lokale server kunnen bereiken. U kunt ook een processerver in de Azure-netwerk van de virtuele machine van failover implementeren. Dit processerver moet ook communiceren met de lokale configuratie-server.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Wanneer moet ik een processerver in Azure installeren?


De virtuele machines in Azure die u wilt opnieuw beveiligen verzenden gegevens van replicatie naar de processerver. Instellen van uw netwerk zodat de virtuele machines in Azure de processerver bereiken kunnen.

U kunt een processerver in Azure implementeren of de bestaande processerver die u hebt gebruikt tijdens de failover. Het belangrijkste in overweging moet nemen is de latentie van de gegevens van de virtuele machines in Azure te verzenden naar de processerver.

Als u een ExpressRoute-verbinding instellen hebt, kunt u een on-premises processerver om gegevens te verzenden omdat de latentie tussen de virtuele machine en de processerver laag is.

 ![Architectuurdiagram voor ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Als u alleen een S2S-VPN hebt, is het echter raadzaam te implementeren van de processerver in Azure.

 ![Diagram voor VPN-architectuur](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Houd er rekening mee dat replicatie van Azure met on-premises kan alleen via de S2S VPN-verbinding of de persoonlijke peering van uw netwerk ExpressRoute gebeuren. Zorg ervoor dat er voldoende bandbreedte beschikbaar via dat netwerkkanaal is.

Zie voor meer informatie over het installeren van een op basis van een Azure-processerver [een processerver worden uitgevoerd in Azure beheren](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> U wordt aangeraden met behulp van een op basis van een Azure-processerver tijdens failback. De prestaties van replicaties is hoger als de processerver dichter bij de replicerende virtuele machine (de failover-machine in Azure is). Echter tijdens uw bewijs van het concept (POC)- of demonstratiedoeleinden, kunt u de lokale processerver samen met ExpressRoute met privépeering de POC-fase sneller voltooid.

> [!NOTE]
> Replicatie van on-premises naar Azure kan alleen via internet of ExpressRoute gebeuren met openbare peering. Replicatie van Azure met on-premises kan alleen via een S2S VPN- of ExpressRoute gebeuren met persoonlijke peering


#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Welke poorten is het openen van op verschillende onderdelen zodat beveiligingspoging kan werken?

![Poorten voor failover en failback](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Welke hoofddoelserver moet ik gebruiken voor beveiligingspoging?
Een on-premises hoofddoelserver-server is vereist voor gegevens ontvangen van de processerver en vervolgens naar de lokale virtuele machine VMDK te schrijven. Als u Windows virtuele machines beveiligt, moet u een Windows-hoofddoelserver. U kunt het proces lokale de doel-server en master hergebruiken<!-- !todo component -->. Voor virtuele Linux-machines moet u een extra on-premises Linux-hoofddoel instellen.


Zie voor informatie over het installeren van een hoofddoelserver:

* [Het installeren van de hoofddoelserver Windows](site-recovery-vmware-to-azure.md)
* [Server voor Linux-hoofddoel installeren](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback"></a>Welke typen datastore tijdens failback op de lokale ESXi-host worden ondersteund?

Op dit moment ondersteunt Azure Site Recovery mislukt weer alleen aan een virtuele machine file system (VMFS) of een virtueel SAN gegevensarchief. Een NFS-gegevensopslag wordt niet ondersteund. Vanwege deze beperking Voer de selectie van het gegevensarchief op de beveiligt scherm is leeg in het geval van NFS datastores of het gegevensarchief virtueel SAN bevat, maar tijdens de taak is mislukt. Als u van plan bent een failback, kunt u lokale voor VMFS gegevensopslag maken en weer terug naar het mislukken. Deze failback wordt een volledige download van de VMDK.

### <a name="common-things-to-check-after-completing-installation-of-the-master-target-server"></a>Algemene wat u moet controleren na het voltooien van de installatie van de hoofddoelserver

* Als de virtuele machine aanwezig op lokaal op de vCenter-server is, moet de hoofddoelserver toegang tot de lokale virtuele-machine-VMDK. Toegang nodig is voor de gerepliceerde gegevens schrijven naar de schijven van de virtuele machine. Zorg ervoor dat de lokale virtuele machine gegevensopslag op het hoofddoel host met lees-/ schrijftoegang is gekoppeld.

* Als de virtuele machine niet aanwezig op lokaal op de vCenter-server is, moet de Site Recovery-service een nieuwe virtuele machine maken tijdens beveiligingspoging. Deze virtuele machine wordt gemaakt op de ESX-host waarop u het hoofddoel maakt. Kies de ESX-host zorgvuldig, zodat de failback van virtuele machine wordt gemaakt op de host die u wilt.

* *U kunt Storage vMotion niet gebruiken voor de hoofddoelserver*. Hierdoor kan de failback mislukken. De virtuele machine starten niet omdat de schijven niet beschikbaar voor deze zijn.

  > [!WARNING]
  > Als een hoofddoel Storage vMotion taak na beveiligingspoging ondergaat, worden de schijven van de beveiligde virtuele machines die zijn gekoppeld aan het hoofddoel migreren naar het doel van de taak vMotion. Als u na deze mislukken, wordt loskoppelen van de schijf mislukt, omdat de schijven zijn niet gevonden. Vervolgens wordt het moeilijk te vinden van de schijven in uw storage-accounts. U moet deze handmatig zoeken en deze koppelt aan de virtuele machine. Hierna is kan de on-premises virtuele machine worden opgestart.

* Een bewaarstation toevoegen aan uw bestaande Windows-hoofddoelserver. Voeg een nieuwe schijf en formatteren van de schijf. Het bewaarstation wordt gebruikt voor het stoppen van de punten in de tijd wanneer de virtuele machine terug naar de lokale site gerepliceerd. Hier volgen enkele criteria van een bewaarstation. Zonder deze criteria voldoen, wordt het station niet weergegeven voor de hoofddoelserver.

   * Het volume wordt niet gebruikt voor andere doeleinden, zoals een doel van de replicatie.

   * Het volume is niet in de modus voor een toepassingsvergrendeling.

   * Het volume is niet een cachevolume. De installatie van de hoofddoelserver mag niet bestaan op dat volume. Het volume van de aangepaste installatie voor de doel-proces model en de server is niet in aanmerking komen voor een bewaarvolume. Wanneer de processerver en de hoofddoelserver worden geïnstalleerd op een volume, is het volume een cachevolume van het hoofddoel.

   * Het type van het bestandssysteem van het volume is niet FAT of FAT32.

   * De capaciteit van het volume gelijk is aan nul.

   * Het bewaarvolume standaard voor Windows is het R-volume.

   * Het standaard bewaarvolume voor Linux is /mnt/retention.

  > [!IMPORTANT]
  > U moet een nieuwe schijf toevoegen als u een bestaande proces server of configuratieserver server-machine of een schaal of een proces op het server-master server doelmachine. Het nieuwe station moet voldoen aan deze vereisten. Als het bewaarstation niet aanwezig is, wordt het niet weergegeven in de selectielijst van de vervolgkeuzelijst op de portal. Nadat u een station hebt toegevoegd aan het hoofddoel lokale, duurt het 15 minuten duren voordat het station moet worden weergegeven in de selectie op de portal. U kunt ook de configuratieserver vernieuwen als het station niet wordt weergegeven na 15 minuten.

* Een Linux failover virtuele machine vereist een Linux-hoofddoelserver. Een Windows failover virtuele machine vereist een Windows-hoofddoelserver.

* Installeer VMware tools op de hoofddoelserver. Zonder de VMware-hulpprogramma's kan niet de datastores op het hoofddoel ESXi-host worden gedetecteerd.

* Schakel de `disk.EnableUUID=true` parameter op de hoofddoelserver virtuele machine met behulp van de vCenter-eigenschappen. <!-- !todo Needs link. -->

* Het hoofddoel moet ten minste één VMFS datastore gekoppeld zijn. Als er geen, de **Datastore** invoer op de pagina opnieuw beveiligen wordt niet leeg zijn en kan niet worden voortgezet.

* De hoofddoelserver kan geen momentopnamen op de schijven hebben. Als er momentopnamen, beveiligingspoging en failback is mislukt.

* Het hoofddoel kan niet een Paravirtuele SCSI-controller hebben. De controller kan alleen worden een LSI Logic-controller. Zonder een domeincontroller LSI Logic mislukt beveiligingspoging.

* Op een bepaald geval kan het hoofddoel atmst 60 schijven die zijn gekoppeld aan deze hebben. Als het aantal virtuele machines opnieuw aan het hoofddoel lokale wordt beveiligd som totale aantal meer dan 60 schijven hebt, gaat u aan het hoofddoel reprotects zal mislukken. Zorg ervoor dat er voldoende master schijf sleuven doel of extra hoofddoelservers implementeren.

<!--
### Failback policy
To replicate back to on-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with the configuration server during creation.
2. This policy is not editable.
3. The set values of the policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-to-reprotect"></a>Stappen voor het opnieuw beveiligen

> [!NOTE]
> Nadat een virtuele machine wordt opgestart in Azure, duurt het enige tijd voor de agent registreren terug naar de configuratieserver (maximaal 15 minuten). Gedurende deze tijd beveiligingspoging mislukt en wordt een foutbericht geeft aan dat de agent niet is geïnstalleerd. Wacht een paar minuten en probeer het vervolgens opnieuw beveiligingspoging.



1. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die failover uitgevoerd is is en selecteer vervolgens **opnieuw beveiligen**. U kunt ook klikt u op de machine en selecteer **opnieuw beveiligen** van de opdrachtknoppen.
2. U ziet dat de blade de richting van beveiliging, **Azure met On-premises**, is al geselecteerd.
3. In **Hoofddoelserver** en **processerver**, selecteer de on-premises hoofddoelserver en de processerver.
4. Voor **Datastore**, selecteert u het gegevensarchief waarnaar u herstellen van de schijven wilt on-premises. Deze optie wordt gebruikt wanneer de on-premises virtuele machine is verwijderd en moet u nieuwe schijven te maken. Deze optie wordt genegeerd als de schijven is al aanwezig, maar u moet nog steeds een waarde opgeven.
5. Kies het station bewaren.
6. Het failbackbeleid voor wordt automatisch geselecteerd.
7. Klik op **OK** om te beginnen met de beveiligingspoging. Een taak begint met het repliceren van de virtuele machine van Azure naar de lokale site. U kunt de voortgang volgen op de **taken** tabblad.

Als u wilt herstellen naar een alternatieve locatie (wanneer de on-premises virtuele machine wordt verwijderd), selecteert u de bewaarstation en gegevensopslag die zijn geconfigureerd voor de hoofddoelserver. Wanneer u een failover naar de lokale site, de virtuele VMware-machines in de failback beveiligingsplan het hetzelfde gegevensarchief gebruiken als de hoofddoelserver. Een nieuwe virtuele machine wordt vervolgens gemaakt in vCenter.

Als u herstellen van de virtuele machine in Azure een bestaande on-premises virtuele machine wilt, koppel datastores van de lokale virtuele machine met lees-/ schrijftoegang op de hoofddoelserver van ESXi-host.
    ![In het dialoogvenster opnieuw te beveiligen](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

U kunt ook op het niveau van een herstelplan beveiligt. Een replicatiegroep kan opnieuw worden beveiligd via een herstelplan alleen. Wanneer u met behulp van een herstelplan beveiligt, moet u de waarden opgeven voor elke beveiligde computer.

> [!NOTE]
> Gebruik de dezelfde hoofddoelserver aan een replicatiegroep beveiligt. Als u een andere hoofddoelserver aan een replicatiegroep beveiligt, opgeven niet de server een gemeenschappelijk punt in tijd.

> [!NOTE]
> De on-premises virtuele machine is uitgeschakeld tijdens de beveiligingspoging. Dit helpt ervoor te zorgen dat gegevens tijdens de replicatie. Zet niet op de virtuele machine nadat beveiligingspoging is voltooid.

Nadat de beveiligingspoging is geslaagd, wordt de virtuele machine een beveiligde status invoeren.

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine heeft een beschermd zijn ingevoerd, kunt u [initiëren van een failback](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

De failback wordt de virtuele machine in Azure afsluiten en opstarten van de on-premises virtuele machine. Verwachten enige uitvaltijd voor de toepassing. Kies een tijd voor failback wanneer de toepassing uitvaltijd tolereren kan.

## <a name="common-problems"></a>Algemene problemen

* Als u een sjabloon voor het maken van uw virtuele machines, zorg ervoor dat elke virtuele machine heeft een eigen UUID voor de schijven gebruikt. Als de UUID van de lokale virtuele machine veroorzaakt een met die van het hoofddoel conflict omdat beide zijn gemaakt met dezelfde sjabloon, mislukt de beveiligingspoging. Een andere hoofddoelserver niet met dezelfde sjabloon gemaakt is implementeren.

* Als u een alleen-lezen vCenter-gebruikersdetectie uitvoeren en virtuele machines beveiligen, beveiliging is geslaagd en failover werkt. Tijdens de beveiligingspoging mislukt failover, omdat de datastores kan niet worden gedetecteerd. Een symptoom is dat de datastores tijdens beveiligingspoging worden niet vermeld. U lost dit probleem, kunt u het bijwerken van de vCenter-referentie met een juiste account dat machtigingen heeft en probeer het opnieuw. Zie voor meer informatie [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Wanneer u een failback uit op een virtuele Linux-machine en on-premises uitgevoerd, kunt u zien dat het pakket Network Manager is verwijderd van de machine. Deze verwijdering gebeurt omdat het pakket Network Manager wordt verwijderd wanneer de virtuele machine in Azure is hersteld.

* Wanneer een virtuele Linux-machine is geconfigureerd met een statisch IP-adres en wordt een failover naar Azure, wordt het IP-adres verkregen van DHCP. Wanneer u een failover naar on-premises, blijft de virtuele machine het IP-adres verkrijgen via DHCP. Handmatig aanmelden bij de computer en het IP-adres weer instellen op een statisch adres indien nodig. De vaste IP-adres kunt opnieuw verkrijgen van een virtuele Windows-computer.

* Als u ESXi 5.5 gratis versie of editie free 6 vSphere-Hypervisor gebruikt, failover is uitgevoerd, maar failback niet is gelukt. Upgraden naar de evaluatielicentie beide programma zodat failback.

* Als u de configuratieserver van de processerver niet kan bereiken, moet u Telnet gebruiken om te controleren van de verbinding met de configuratieserver op poort 443. U kunt ook de configuratieserver van de processerver te pingen. Een processerver moet ook een heartbeat hebben wanneer deze is verbonden met de configuratieserver.

* Als u een failback naar een alternatieve vCenter wilt, ervoor zorgen dat uw nieuwe vCenter en de hoofddoelserver worden gedetecteerd. Een typische symptoom is dat de datastores zijn niet toegankelijk of zichtbaar in de **beveiligt** in het dialoogvenster.

* Een Windows Server 2008 R2 SP1-server die is beveiligd, omdat het een fysieke on-premises-server kan niet worden kan failback vanuit Azure een on-premises site.

### <a name="common-error-codes"></a>Algemene foutcodes

#### <a name="error-code-95226"></a>Foutcode 95226

*Beveiligt, is mislukt omdat de virtuele machine van Azure niet kunnen bereiken van de lokale configuratie-server.*

Dit gebeurt wanneer 
1. Virtuele machine van Azure kan de lokale configuratie-server kan niet worden bereikt en daarom niet worden gedetecteerd en geregistreerd voor de configuratieserver. 
2. De InMage Scout Application service op de virtuele machine van Azure die moet worden uitgevoerd om te communiceren met de lokale configuratie-server mogelijk niet na failover worden uitgevoerd.

Dit probleem op te lossen
1. U moet ervoor zorgen dat het netwerk van virtuele machine van Azure is zodanig geconfigureerd dat de virtuele machine met de lokale configuratie-server communiceren kan. U doet dit door een Site naar Site VPN terug naar uw on-premises datacentrum instellen of configureren van een ExpressRoute-verbinding met de persoonlijke peering in het virtuele netwerk van virtuele machine van Azure. 
2. Als u al een netwerk zodanig geconfigureerd dat de virtuele machine van Azure met de lokale configuratie-server communiceren kan hebt, meldt u zich vervolgens naar de virtuele machine en 'InMage Scout Application Service' controleren. Als u merkt dat de InMage Scout Application Service niet wordt uitgevoerd handmatig starten van de service en controleer of het starttype van de service is ingesteld op automatisch.

### <a name="error-code-78052"></a>Foutcode 78052
Beveiligt mislukt met het volgende foutbericht: *beveiliging voor de virtuele machine kan niet worden voltooid.*

Dit kan gebeuren als gevolg van twee redenen
1. De virtuele machine die u opnieuw uit te beveiligen is een Windows Server 2016. Momenteel dit besturingssysteem wordt niet ondersteund voor failback echter zo snel mogelijk worden ondersteund.
2. Er bestaat al een virtuele machine met dezelfde naam op de Master-doelserver u terug naar mislukken.

U kunt dit probleem oplossen door een andere hoofddoelserver op een andere host selecteren zodat de beveiligt de machine op een andere host maakt, waarbij de namen geen conflict veroorzaken. U kunt ook het hoofddoel naar een andere host waar het conflict naam niet gebeurt vMotion. Als de bestaande virtuele machine een losse machine is, kunt u deze alleen wijzigen zodat de nieuwe virtuele machine ophalen op dezelfde ESXi-host gemaakt kan.

### <a name="error-code-78093"></a>Foutcode 78093

*De virtuele machine wordt niet uitgevoerd in een toepassing is vastgelopen of niet toegankelijk.*

Als u wilt een mislukte via de virtuele machine naar on-premises beveiligt, moet u de virtuele machine van Azure uitgevoerd. Dit is zodat de mobility-service met de configuratieserver registreert on-premises en kunt beginnen met het repliceren van door de communicatie met de processerver. Als de machine zich op een netwerk onjuist of wordt niet uitgevoerd (is vastgelopen of afsluiten), bereiken niet de mobility-service in de virtuele machine om te beginnen met het beveiligt door de configuratieserver. U kunt de virtuele machine opnieuw zodat deze communicatie back lokale kunt starten. De taak opnieuw beveiligen opnieuw starten na het starten van de virtuele machine van Azure

### <a name="error-code-8061"></a>Foutcode 8061

*Het gegevensarchief is niet toegankelijk is vanaf ESXi-host.*

Raadpleeg de [de hoofdsleutel van de doel-vereisten](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) en de [datastores ondersteunen](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) voor failback

