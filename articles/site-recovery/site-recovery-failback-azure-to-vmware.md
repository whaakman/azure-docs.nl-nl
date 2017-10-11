---
title: Failback virtuele VMware-machines van Azure met on-premises | Microsoft Docs
description: Meer informatie over mislukte terug naar de lokale site na een failover van virtuele VMware-machines en fysieke servers naar Azure.
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: dde0bb6b4f6bc10afdd7d40adc6689d42b37de81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Mislukken back virtuele VMware-machines en fysieke servers naar de lokale site


Dit artikel wordt beschreven hoe u failback Azure virtuele machines van Azure naar de lokale site. Volg de instructies hier wanneer u klaar bent om een failback uit op uw virtuele VMware-machines of fysieke Windows of Linux-servers nadat u hebt uw gebruik van deze machines opnieuw beveiligd [verwijzing](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Als u de klassieke Azure portal gebruikt, raadpleegt u instructies vermeld [hier](site-recovery-failback-azure-to-vmware-classic.md) voor verbeterde VMware aan Azure-architectuur en [hier](site-recovery-failback-azure-to-vmware-classic-legacy.md) voor de verouderde architectuur

## <a name="overview"></a>Overzicht
De diagrammen in deze sectie geven de failback-architectuur voor dit scenario.

Wanneer de processerver lokale is en u een Azure ExpressRoute-verbinding gebruikt, gebruikt u deze architectuur:

![Architectuurdiagram voor ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Wanneer de processerver in Azure is en u een VPN of een ExpressRoute-verbinding hebt, gebruikt u deze architectuur:

![Diagram voor VPN-architectuur](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Raadpleeg de volgende afbeelding voor een volledige lijst van poorten en het Architectuurdiagram van de failback:

![Lijst van alle poorten failover failback](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Nadat u hebt een failover uitvoeren naar Azure, schakelt u terug naar uw on-premises site in drie fasen:

* **Fase 1**: U opnieuw de Azure VM's beveiligen zodat deze beginnen te repliceren naar de VMware-virtuele machines die worden uitgevoerd op uw on-premises site.
* **Fase 2**: nadat u uw Azure VM's worden gerepliceerd naar uw on-premises site, voert u een failover naar een failback van Azure.
* **Fase 3**: nadat u uw gegevens terug is mislukt, beveiligt de lokale virtuele machines die u niet terug naar, zodat deze beginnen te repliceren naar Azure.

### <a name="fail-back-to-the-original-location-or-an-alternate-location"></a>Failback naar de oorspronkelijke locatie of een alternatieve locatie
Nadat u een VMware VM failover, kunt u failback naar dezelfde bron voor VM als deze nog steeds lokale bestaat. In dit scenario wordt alleen de delta's terug mislukt.

Als de failover van fysieke servers is failback altijd een nieuwe VMware VM. Voordat u niet opnieuw een fysieke computer, houd er rekening mee dat:
* Een beveiligde fysieke machine wordt weer als een virtuele machine geleverd wanneer deze wordt een failover van Azure naar VMware. Een fysieke computer Windows Server 2008 R2 SP1 kan niet als deze wordt beveiligd en een failover naar Azure, worden uitgevoerd voor terug. Een Windows Server 2008 R2 SP1-machine die gestart als een virtuele machine op lokale zich failback.
* Zorg ervoor dat u ten minste één hoofddoelserver samen met de benodigde ESX/ESXi-hosts die u moet een failback naar detecteren.

Als u niet terug naar de oorspronkelijke VM, het volgende is vereist:

* Als de virtuele machine wordt beheerd door een vCenter-server, moet het hoofddoel ESX-host toegang hebben tot het gegevensarchief van virtuele machines.
* Als de virtuele machine zich op een ESX-host, maar wordt niet beheerd door de vCenter, moet de harde schijf van de virtuele machine zich in een gegevensopslag die toegankelijk is voor de MT host.
* Als uw virtuele machine zich op een ESX-host en maakt geen gebruik van vCenter, moet u detectie van de ESX-host van de MT uitvoeren voordat u opnieuw beveiligen. Dit geldt als u bent terug fysieke servers te mislukken.
* Een andere optie is (als de lokale virtuele machine bestaat) te verwijderen voordat u een failback. Failback maakt vervolgens een nieuwe virtuele machine op dezelfde host als de hoofddoelserver ESX-host.

Wanneer u een failover naar een alternatieve locatie, wordt de gegevens naar het hetzelfde gegevensarchief en de dezelfde ESX-host die de on-premises hoofddoelserver hersteld.

## <a name="prerequisites"></a>Vereisten
* Mislukken back-VMware-machines en fysieke servers, moet u een VMware-omgeving. Mislukte terug naar een fysieke server wordt niet ondersteund.
* Als u wilt een failback uit, moet u hebt gemaakt een Azure-netwerk wanneer u in eerste instantie beveiliging instelt. Failback moet een VPN- of ExpressRoute-verbinding van het Azure-netwerk waarin de Azure VM's zich bevinden op de lokale site.
* Als de virtuele machines die u wilt failback naar worden beheerd door een vCenter-server, zorg ervoor dat hebt u de vereiste machtigingen voor de detectie van virtuele machines op de vCenter-servers. Zie voor meer informatie [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
* Als u momentopnamen aanwezig zijn op een virtuele machine, mislukt de beveiligingspoging. U kunt de momentopnamen of de schijven verwijderen.
* Voordat u een failback uit op, maakt u deze onderdelen:
  * **Maken van een processerver in Azure**. Dit onderdeel is een Azure VM die u maakt en blijven uitvoeren tijdens de failback. Nadat de failback is voltooid, kunt u de virtuele machine verwijderen.
  * **Maken van een hoofddoelserver**: de hoofddoelserver gegevens verzendt en ontvangt failback. De beheerserver die u hebt lokale gemaakt heeft een hoofddoelserver die standaard wordt geïnstalleerd. Afhankelijk van het volume is mislukt-back-verkeer moet u mogelijk echter maken van een afzonderlijke hoofddoelserver voor failback.
  * Voor het maken van een extra hoofddoelserver die op Linux wordt uitgevoerd, de Linux-VM voordat u de hoofddoelserver installeert indien ingesteld verderop.
* Een configuratieserver is vereist on-premises als u een failback doet. Tijdens de failback, moet de virtuele machine zich in de configuratiedatabase van de server. Failback kan niet worden voltooid als de configuratie van server-database geen VM bevat. Controleer dus regelmatig geplande back-ups van uw server te maken. U wilt herstellen met hetzelfde IP-adres, zodat failback werkt in een noodgeval.
* De disk.enableUUID=true instellen **configuratieparameters** doel van de master virtuele machine in VMware. Als deze rij niet bestaat, moet u deze toevoegen. Deze instelling is vereist voor het bieden van een consistente universally unique identifier (UUID) naar de virtuele machine-schijfbestand (VMDK) zodat deze correct is gekoppeld.
* Houd rekening met een ' Master doelserver opslag vMotioned mag niet ' voorwaarde, waardoor de failback mislukken. De virtuele machine kan niet actief omdat de schijven zijn niet beschikbaar gesteld aan deze.
* Een station, een station bewaren op de hoofddoelserver aangeroepen toevoegen. Voeg een schijf en formatteren van de schijf.

## <a name="failback-policy"></a>Beleid voor failback
Als u wilt repliceren naar on-premises, moet u een beleid voor failback. Het beleid wordt automatisch gemaakt wanneer u een vooruit-beleid maken en automatisch gekoppeld aan de configuratieserver is. Deze kan niet worden bewerkt. Het beleid heeft de volgende replicatie-instellingen:

* RPO drempelwaarde = 15 minuten
* Herstel bewaarperiode = 24 uur
* App-consistente momentopname frequentie = 60 minuten

 ![Replicatie-instellingen van het failbackbeleid voor](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Instellen van een processerver in Azure
Processerver in Azure installeren zodat de Azure VM's kunt de gegevens terug naar de on-premises hoofddoelserver verzenden.

Als u uw virtuele machines als klassieke resources hebt beveiligd (dat wil zeggen, de virtuele machine in Azure hersteld is voor een virtuele machine die is gemaakt met het klassieke implementatiemodel), moet u een processerver in Azure. Als u de virtuele machines met Azure Resource Manager als het implementatietype hebt hersteld, moet de processerver Resource Manager hebben als het implementatietype. Het implementatietype wordt geselecteerd door de Azure-netwerk dat u de processerver te implementeren.

1. In **kluis** > **instellingen** > **Site Recovery-infrastructuur** (onder **beheren**) > **configuratieservers** (onder **voor VMware en fysieke Machines**), selecteer de configuratieserver.
2. Klik op **verwerken Server**.

  ![Knop Server verwerken](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Kiezen voor het implementeren van de processerver als **een failback processerver in Azure implementeren**.
4. Selecteer het abonnement dat u de virtuele machines naar hebt hersteld.
5. Selecteer het Azure-netwerk dat u de virtuele machines naar hebt hersteld. De proces-Server moet zich in hetzelfde netwerk, zodat de herstelde virtuele machines en de processerver kunnen communiceren.
6. Als u hebt geselecteerd een *klassieke implementatiemodel* netwerk- en installeer vervolgens de processerver in het maken van een virtuele machine via Azure Marketplace.

 ![Het venster 'processerver toevoegen'](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 Als u de processerver maakt, moet u aandacht schenken aan het volgende:
 * De naam van de afbeelding is *Microsoft Azure Site Recovery proces Server V2*. Selecteer **klassieke** als het implementatiemodel.

       ![Select "Classic" as the Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Installeren van de processerver volgens de instructies in [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
7. Als u selecteert de *Resource Manager* Azure netwerk, het implementeren van de processerver door te geven van de volgende informatie:

  * De naam van de resourcegroep die u wilt de-server implementeren
  * De naam van de server
  * Een gebruikersnaam en wachtwoord voor aanmelding bij de server
  * Het opslagaccount dat u wilt de-server implementeren
  * Het subnet en de netwerkinterface die u wilt er verbinding mee maken
   >[!NOTE]
   >Moet u uw eigen [netwerkinterface](../virtual-network/virtual-networks-multiple-nics.md) (NIC) en selecteer het terwijl u de processerver implementeert.

    ![Geef informatie op in het dialoogvenster 'processerver toevoegen'](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Klik op **OK**. Deze actie wordt een taak die u een Resource Manager deployment type virtuele machine tijdens de installatie van de processerver maakt geactiveerd. Voer voor het registreren van de server en de configuratieserver setup in de virtuele machine door de instructies in [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md). Een taak voor het implementeren van de processerver wordt ook geactiveerd.

  De processerver wordt weergegeven op de **configuratieservers** > **servers die zijn gekoppeld** > **processervers** tabblad.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > De processerver is niet zichtbaar onder **VM-eigenschappen**. Het is alleen op de **Servers** tabblad in de beheerserver waarmee deze geregistreerd bij. Het kan 10 tot 15 minuten duren voordat de processerver worden weergegeven.


## <a name="set-up-the-master-target-server-on-premises"></a>Instellen van de hoofddoelserver server on-premises
De hoofddoelserver ontvangt de failback-gegevens. De server wordt automatisch geïnstalleerd op de lokale management-server, maar als u bent niet terug te veel gegevens, moet u mogelijk een extra hoofddoelserver instellen. Instellen van een hoofddoelserver lokaal, doet u het volgende:

> [!NOTE]
> Als u een hoofddoelserver op Linux instelt, gaat u naar de volgende procedure. Gebruik alleen CentOS 6.6 minimaal besturingssysteem als het hoofddoel OS.

1. Als u de hoofddoelserver op Windows instelt, opent u de pagina snel starten van de virtuele machine die u op de hoofddoelserver installeert.
2. Download het installatiebestand voor de Azure Site Recovery Unified Setup-wizard.
3. Voer setup en in **voordat u begint met**, selecteer **toevoegen van extra processervers moet worden uitgebreid implementatie**.
4. Voltooi de wizard op dezelfde manier als u dit hebt gedaan wanneer u [instellen van de beheerserver](site-recovery-vmware-to-azure-classic.md). Op de **Server configuratiedetails** pagina, geeft u het IP-adres van de hoofddoelserver en voer een wachtwoordzin voor toegang tot de virtuele machine.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Linux-VM instellen als de hoofddoelserver
Installeer de CentOS 6.6 minimaal besturingssysteem voordat u de beheerserver met de hoofddoelserver als een Linux-VM kunt instellen. Vervolgens ophalen van de SCSI-id's voor elke harde SCSI-schijf, een aantal extra pakketten installeren en een aantal aangepaste wijzigingen toepassen.

#### <a name="install-centos-66"></a>CentOS 6.6 installeren

1. De CentOS 6.6 minimaal besturingssysteem installeren op de beheerserver VM. ISO op een DVD-station behouden en start het systeem. Slaat het testen van media. Selecteer **Amerikaans Engels** als de taal selecteren **Basic opslagapparaten**, controleert u of de harde schijf niet alle belangrijke gegevens, klikt u op **Ja**, en alle gegevens te wissen. Voer de hostnaam van de beheerserver en selecteert u de netwerkadapter van de server.  In de **System bewerken** dialoogvenster, **automatisch verbinding maken**, en voegt u een statisch IP-adres, het netwerk en DNS-instellingen. Geef een tijdzone. Voor toegang tot de beheerserver, voer het root-wachtwoord.
2. Wanneer u wordt gevraagd welk type installatie dat u wilt, selecteert u **maken aangepaste lay-out** als de partitie. Klik op **Volgende**. Selecteer **vrije**, en klik vervolgens op **maken**. Maak  **/** , **/var/crash**, en **/home partities** met **FS Type:** **ext4**. Maken van de wisseling partitie **FS Type: wisselen**.
3. Als de bestaande apparaten zijn gevonden, wordt er een waarschuwing weergegeven. Klik op **indeling** om de schijf met de partitie-instellingen te formatteren. Klik op **wijziging schrijven naar schijf** de partitie wijzigingen toe te passen.
4. Selecteer **installeren opstartlaadprogramma** > **volgende** het opstartlaadprogramma installeren op de basispartitie.
5. Wanneer de installatie voltooid is, klikt u op **opnieuw opstarten**.

#### <a name="retrieve-the-scsi-ids"></a>De SCSI-id's ophalen

1. Na de installatie de SCSI-id's voor elke harde SCSI-schijf in de virtuele machine worden opgehaald. Schakel de VM-beheerserver om dit te doen. In de eigenschappen van de virtuele machine in VMware, met de rechtermuisknop op de VM-vermelding > **instellingen bewerken** > **opties**.
2. Selecteer **Geavanceerd** > **algemene**, en klik vervolgens op **configuratieparameters**. Deze optie is niet beschikbaar wanneer de computer wordt uitgevoerd. Voor de optie beschikbaar is, moet u de machine afgesloten.
3. Voer een van de volgende bewerkingen uit:
 * Als de rij **schijf. EnableUUID** bestaat, zorg ervoor dat de waarde is ingesteld op **True** (hoofdlettergevoelig). Als de waarde is al ingesteld op True, kunt u annuleren en de SCSI-opdracht in een gastbesturingssysteem test nadat deze opgestart.
 * Als de rij **schijf. EnableUUID** niet bestaat, klikt u op **rij toevoegen**, en vervolgens toe te voegen met de **True** waarde. Gebruik geen dubbele aanhalingstekens.

#### <a name="install-additional-packages"></a>Extra pakketten installeren
Downloaden en installeren van extra pakketten.

1. Zorg ervoor dat de hoofddoelserver is verbonden met Internet.
2. Als u wilt downloaden en installeren van 15 pakketten uit de opslagplaats voor CentOS, deze opdracht uitvoeren: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Als de bronmachines die u wilt beveiligen Linux wordt uitgevoerd met een Reiser of XFS bestandssysteem voor het basis- of -apparaat, downloaden en installeren van extra pakketten als volgt:

   * \#cd /usr/local
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#reiserfs in de rpm – ivh-kmod reiserfs-0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#– ivh xfsprogs-3.1.1 rpm-16.el6.x86_64.rpm
   * \#YUM apparaat-mapper-multipath (vereist om in te schakelen op de hoofddoelserver multipath-pakketten) installeren

#### <a name="apply-custom-changes"></a>Aangepaste wijzigingen toepassen
Nadat u de stappen na de installatie voltooid en de pakketten geïnstalleerd, kunt u aangepaste wijzigingen toepassen door het volgende te doen:

1. Kopieer de RHEL 6 64 Unified Agent binaire naar de virtuele machine. Als u wilt het binaire bestand untar, deze opdracht uitvoeren: `tar –zxvf <file name>`.
2. Als u machtigingen geven, deze opdracht uitvoeren: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Voer het volgende script: `# ./ApplyCustomChanges.sh`. Slechts één keer wordt uitgevoerd. Nadat deze is uitgevoerd, moet u de server opnieuw opgestart.

## <a name="run-the-failback"></a>De failback worden uitgevoerd
### <a name="reprotect-the-azure-vms"></a>De virtuele machines in Azure beveiligt
1. In **kluis**in **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die is failover en selecteer vervolgens **opnieuw beveiligen**.
2. Op de blade kunt u zien dat de richting van beveiliging **Azure met On-premises** is al geselecteerd.
3. In **Hoofddoelserver** en **processerver**, selecteer de on-premises hoofddoelserver en de processerver van Azure VM.
4. Selecteer het gegevensarchief die u de schijven wilt on-premises te herstellen. Gebruik deze optie als de lokale virtuele machine is verwijderd en u moet nieuwe schijven wilt maken. De optie negeren als de schijven is al aanwezig, maar u moet nog steeds een waarde opgeven.
5. Gebruik een bewaarstation stoppen van de punten in de tijd waarop de virtuele machine naar on-premises wordt gerepliceerd. Sommige criteria van een bewaarstation worden hier weergegeven. Zonder deze criteria het station niet wordt weergegeven voor de hoofddoelserver.

  * Volume mag niet worden gebruikt voor andere doeleinden (doel van replicatie, enzovoort).
  * Volume mag niet zijn vergrendeld.
  * Volume mag niet het cachevolume zijn. (De installatie van de hoofddoelserver mag niet bestaan op dat volume. De processerver plus master aangepaste installatie als doel het volume is niet in aanmerking komen voor bewaarvolume. Hier, de geïnstalleerde processerver plus master volume doel is het cachevolume van het hoofddoel.)
  * Het type bestandssysteem volume mag niet FAT en FAT32.
  * De volumecapaciteit mag niet nul zijn.
  * Het bewaarvolume standaard voor Windows is R-volume.
  * Het standaard bewaarvolume voor Linux is /mnt/retention.

6. Het failbackbeleid voor wordt automatisch geselecteerd.
7. Nadat u op **OK** om te beginnen met de beveiligingspoging, een taak begint met het repliceren van de virtuele machine van Azure naar de lokale site. U kunt de voortgang volgen op de **taken** tabblad.

Als u herstellen naar een alternatieve locatie wilt, selecteert u de bewaarstation en gegevensopslag die zijn geconfigureerd voor de hoofddoelserver. Wanneer u een failover naar de lokale site, de virtuele VMware-machines in de failback beveiligingsplan het hetzelfde gegevensarchief gebruiken als de hoofddoelserver. Als u herstellen van de replica virtuele machine van Azure naar dezelfde lokale virtuele machine wilt, moet de lokale virtuele machine al in het gegevensarchief dezelfde als de hoofddoelserver. Als er geen VM on-premises, wordt een nieuwe tijdens beveiligingspoging gemaakt.

![Selecteer "Azure met on-premises' in de vervolgkeuzelijst](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

U kunt ook op een niveau van de planning recovery beveiligt. Als u een replicatiegroep hebt, kunt u alleen met behulp van een herstelplan beveiligt. Wanneer u met behulp van een herstelplan beveiligt, moet u de vorige waarden gebruiken voor elke beveiligde computer.

> [!NOTE]
> Replicatiegroepen moeten worden beveiligd met de dezelfde hoofddoel terug. Als ze worden beschermd met de andere hoofddoelserver servers, kan niet voor deze algemene punt in tijd worden bepaald.

### <a name="run-a-failover-to-the-on-premises-site"></a>Een failover uitvoeren naar de lokale site
Nadat u de virtuele machine beveiligt, kunt u een failover van Azure met on-premises starten.

1. Op de pagina gerepliceerde items met de rechtermuisknop op de virtuele machine en selecteer vervolgens **niet-geplande Failover**.
2. In **bevestigen Failover**, Controleer of de failoverrichting (van Azure) en selecteer het herstelpunt dat u wilt gebruiken voor de failover (de meest recente of de meest recente app-consistente herstelpunt). Een app-consistente herstelpunt vindt plaats voordat het meest recente punt in tijd en hierdoor kunnen gegevens verloren gaan.
3. Tijdens de failover afgesloten Site Recovery de Azure VM's. Nadat u hebt gecontroleerd dat failback is voltooid zoals verwacht, kunt u controleren om ervoor te zorgen dat de Azure VM's hebt afgesloten zoals verwacht.

### <a name="reprotect-the-on-premises-site"></a>Beveiligt de lokale site
De virtuele machine om ervoor te zorgen dat de virtuele machines in Azure hersteld worden verwijderd nadat de failback is voltooid, worden doorgevoerd. Om dit te doen met de rechtermuisknop op het beveiligde item en klik vervolgens op **doorvoeren**. Deze actie wordt een taak die u de voormalige herstelde virtuele machines in Azure verwijdert geactiveerd.

Nadat het doorvoeren is voltooid, wordt uw gegevens moet terug op de lokale site, maar deze won't worden beveiligd. Als u wilt repliceren naar Azure opnieuw, het volgende doen:

1. In **kluis**in **instelling** > **gerepliceerde items**, selecteert u de virtuele machines die zijn mislukt terug en klik vervolgens op **opnieuw beveiligen**.
2. Geeft de waarde van de processerver die moet worden gebruikt om gegevens te verzenden naar Azure.
3. Klik op **OK**.

Nadat de beveiligingspoging voltooid is, wordt de virtuele machine repliceert naar Azure en kunt u een failover uitvoeren.

### <a name="resolve-common-failback-issues"></a>Veelvoorkomende failback problemen oplossen
* Als u een alleen-lezen vCenter-gebruikersdetectie uitvoert en beveiligen van virtuele machines, het is gelukt en failover werkt. Tijdens de beveiligingspoging mislukt failover, omdat de datastores kan niet worden gedetecteerd. Als een symptoom ziet u de datastores tijdens beveiligingspoging vermeld. U lost dit probleem, kunt u het bijwerken van de vCenter-referentie met een juiste account dat machtigingen heeft en probeer het opnieuw. Zie voor meer informatie [repliceren VMware virtuele machines en fysieke servers naar Azure met Azure Site Recovery](site-recovery-vmware-to-azure-classic.md)
* Wanneer u een failback uit op een Linux-VM en on-premises uitgevoerd, kunt u zien dat het pakket Network Manager is verwijderd van de machine. Deze verwijdering gebeurt omdat het pakket Network Manager wordt verwijderd wanneer de virtuele machine in Azure is hersteld.
* Wanneer een virtuele machine is geconfigureerd met een statisch IP-adres en wordt een failover naar Azure, wordt het IP-adres verkregen via DHCP. Wanneer u een failover uitvoert back met on-premises, blijft de virtuele machine het IP-adres verkrijgen via DHCP. Handmatig aanmelden bij de computer en het IP-adres weer instellen op een statisch adres indien nodig.
* Als u van ESXi 5.5 gratis versie of editie free 6 vSphere-Hypervisor gebruikmaakt, failover zou slagen, maar failback niet zou slagen. Upgraden naar de evaluatielicentie beide programma zodat failback.
* Als u de configuratieserver niet kan van de processerver bereiken, Controleer de verbinding met de configuratieserver door Telnet op de servercomputer van de configuratie op poort 443. U kunt ook de configuratieserver van de processerver-machine te pingen. Een proces-Server moet ook een heartbeat hebben wanneer deze is verbonden met de configuratieserver.
* Als u een failback naar een alternatieve vCenter wilt, zorg ervoor dat uw nieuwe vCenter wordt gedetecteerd en of de hoofddoelserver ook wordt gedetecteerd. Een typische symptoom is dat de datastores zijn niet toegankelijk of zichtbaar in de **beveiligt** in het dialoogvenster.
* Een WS2008R2SP1-machine die wordt beveiligd als een fysieke on-premises machine van Azure kan niet worden mislukt met on-premises.

## <a name="fail-back-with-expressroute"></a>Mislukt met de ExpressRoute
U kunt niet terug via een VPN-verbinding of met behulp van een ExpressRoute-verbinding. Als u een ExpressRoute-verbinding gebruiken wilt, moet u rekening houden met het volgende:

* De ExpressRoute-verbinding moet worden ingesteld op de virtuele Azure-netwerk die de bronmachines failover en waar de Azure VM's zich bevinden, na de failover.
* Gegevens worden gerepliceerd naar Azure storage-account op een openbaar eindpunt. Voor het gebruik van een ExpressRoute-verbinding instellen openbare peering in ExpressRoute met de doel-Datacenter voor replicatie van Site Recovery.
