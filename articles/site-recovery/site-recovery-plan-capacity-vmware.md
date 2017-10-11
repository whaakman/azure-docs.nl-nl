---
title: Plannen van capaciteit en de schaalbaarheid van VMware-replicatie naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel voor plan capaciteit en schaal gebruiken wanneer virtuele VMware-machines repliceren naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: rayne
ms.openlocfilehash: 8b580ac239bfb6d7b633fb03d4cfb91b168b0610
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Plannen van capaciteit en de schaalbaarheid van VMware-replicatie met Azure Site Recovery

Gebruik dit artikel bij het plannen van capaciteit en de schaling bij het lokale virtuele VMware-machines en fysieke servers repliceren naar Azure met achterhalen [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Hoe start capaciteitsplanning?

Informatie verzamelen over uw replicatieomgeving door het uitvoeren van de [Azure Site Recovery implementatie Planner](https://aka.ms/asr-deployment-planner-doc) voor VMware-replicatie. [Meer informatie](site-recovery-deployment-planner.md) over dit hulpprogramma. U kunt informatie verzamelen over compatibele en niet-compatibele virtuele machines, schijven per virtuele machine, en gegevens verloop per schijf. Het hulpprogramma vallen ook netwerkbandbreedte en de Azure-infrastructuur die nodig zijn voor een geslaagde failover voor replicatie en test.

## <a name="capacity-considerations"></a>Overwegingen voor capaciteitsplanning

**Onderdeel** | **Details** |
--- | --- | ---
**Replicatie** | **Maximale dagelijkse wijzigingssnelheid:** beveiligde machine kan alleen een van de processerver gebruiken, en een enkel proces-server kan een dagelijks verwerken wijziging beoordelen maximaal 2 TB. 2 TB is dus dat de maximale dagelijkse gegevens wijzigen die wordt ondersteund voor een beveiligde machine.<br/><br/> **Maximale doorvoer:** een gerepliceerde machine kan deel uitmaken van een opslagaccount in Azure. Standard-opslagaccount maximaal 20.000 aanvragen per seconde kan verwerken en het is raadzaam het aantal i/o-bewerkingen per seconde (IOPS) te houden voor een bronmachine tot 20.000. Bijvoorbeeld, als er een bronmachine met 5 schijven en elke schijf 120 IOPS (grootte van 8 kB) op de bronmachine wordt gegenereerd, worden in de Azure per schijf-IOPS limiet van 500. (Het aantal vereiste opslagaccounts voor is gelijk aan de totale bronmachine IOP's, gedeeld door 20.000.)
**Configuratieserver** | De configuratieserver moet overweg kan de dagelijkse snelheid capaciteit voor wijzigen voor alle werkbelastingen op beveiligde machines uitgevoerd en moet voldoende bandbreedte continu om gegevens te repliceren naar Azure Storage.<br/><br/> Als een best practice vinden de configuratieserver in het hetzelfde netwerk en de LAN-segment als de machines die u wilt beveiligen. Dit kan zich bevinden op een ander netwerk, maar machines die u wilt beveiligen layer 3-netwerk zichtbaarheid aan moeten hebben.<br/><br/> Aanbevelingen voor de grootte voor de configuratieserver worden samengevat in de tabel in de volgende sectie.
**Processerver** | De eerste processerver wordt standaard geïnstalleerd op de configuratieserver. U kunt aanvullende processen servers om te schalen van uw omgeving kunt implementeren. <br/><br/> De processerver ontvangt replicatiegegevens van beveiligde machines en optimaliseert met caching, compressie en codering. Vervolgens verzendt de gegevens naar Azure. De proces-servercomputer hebt voldoende bronnen zijn voor deze taken uitvoeren.<br/><br/> De processerver maakt gebruik van een cache op basis van schijven. Gebruik een afzonderlijke cache schijf 600 GB of meer voor de verwerking van gewijzigde gegevens opgeslagen in het geval van een knelpunt netwerk of een storing.

## <a name="size-recommendations-for-the-configuration-server"></a>Aanbevelingen voor de grootte voor de configuratieserver

**CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vcpu's (2-sockets * 4 kernen @ 2,5 gigahertz [GHz]) | 16 GB | 300 GB | 500 GB of minder | Minder dan 100 machines repliceren.
12 vcpu's (2-sockets * @ 2,5 GHz-6 kernen) | 18 GB | 600 GB | 500 GB tot 1 TB | 100 tot 150-machines repliceren.
16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen) | 32 GB | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150 tot 200-machines.
Een andere processerver implementeren | | | > 2 TB | Aanvullende processen servers implementeren als u meer dan 200 machines repliceert, of als de dagelijkse hoeveelheden wijzigen frequentie hoger is dan 2 TB.

Waar:

* Elke bronmachine is geconfigureerd met 3 schijven van 100 GB.
* We gebruiken benchmarking opslag van 8 SAS-schijven van 10 K RPM, RAID 10 voor cache schijf metingen.

## <a name="size-recommendations-for-the-process-server"></a>Aanbevelingen voor de grootte voor de processerver

Als u wilt meer dan 200 machines beveiligt, of de dagelijkse wijzigingssnelheid groter dan 2 TB is, kunt u processervers voor het afhandelen van de belasting van de replicatie kunt toevoegen. Als u wilt uitbreiden, kunt u het volgende doen:

* Verhoog het aantal configuratieservers. U kunt bijvoorbeeld maximaal 400 machines met twee configuratieservers beveiligen.
* Meer processervers toevoegen en deze gebruiken voor het afhandelen van verkeer in plaats van (of als aanvulling op) de configuratieserver.

De volgende tabel beschrijft een scenario waarin:

* U niet van plan bent te gebruiken van de configuratieserver als een processerver.
* U hebt een extra processerver ingesteld.
* U kunt beveiligde virtuele machines voor het gebruik van de aanvullende processerver hebt geconfigureerd.
* Elke machine beveiligde bron is geconfigureerd met drie schijven van 100 GB.

**Configuratieserver** | **Aanvullende processerver** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vcpu's (2-sockets * 4 kernen @ 2,5 GHz), 16 GB geheugen | 4 vcpu's (2-sockets * 2 kernen @ 2,5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | 85 of minder machines repliceren.
8 vcpu's (2-sockets * 4 kernen @ 2,5 GHz), 16 GB geheugen | 8 vcpu's (2-sockets * 4 kernen @ 2,5 GHz), 12 GB geheugen | 600 GB | 250 GB tot 1 TB | Repliceren tussen 85 150-machines.
12 vcpu's (2-sockets * 6 kernen @ 2,5 GHz), 18 GB geheugen | 12 vcpu's (2-sockets * 6 kernen @ 2,5 GHz) 24 GB geheugen | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150 225 machines.

De manier waarop u uw servers schalen, is afhankelijk van uw voorkeur voor een model omhoog schalen of scale-out.  Als u door het implementeren van een paar geavanceerde configuratie en processervers opschalen of uitbreiden door het implementeren van meerdere servers met minder bronnen. Bijvoorbeeld, als u beveiligen 220 machines wilt, kan u doen het volgende:

* Stel de configuratieserver met 12 vCPU, 18 GB geheugen, en een extra processerver met 12 vCPU, 24 GB geheugen. Beveiligde machines voor het gebruik van alleen de aanvullende processerver configureren.
* Twee configuratieservers (2 x 8 vCPU, 16 GB RAM-geheugen) en twee extra processervers (1 x 8 vCPU en 4 vCPU x 1 te verwerken 135 + 85 [220] machines) instellen. Beveiligde machines voor het gebruik van alleen de aanvullende processen servers configureren.


## <a name="control-network-bandwidth"></a>Netwerkbandbreedte

Nadat u hebt gebruikt de [het hulpprogramma implementatie Planner](site-recovery-deployment-planner.md) voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (initiële replicatie en klikt u vervolgens delta), kunt u bepalen van de hoeveelheid bandbreedte die wordt gebruikt voor replicatie met een aantal opties:

* **Bandbreedte beperken**: VMware-verkeer dat wordt gerepliceerd naar Azure verloopt via een specifiek proces-server. U kunt de bandbreedte op de machines die worden uitgevoerd als processervers beperken.
* **Netwerkbandbreedte beïnvloeden**: U hebt invloed op de bandbreedte die wordt gebruikt voor de replicatiegroep met behulp van een aantal registersleutels:
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** registerwaarde geeft u het aantal threads die worden gebruikt voor gegevensoverdracht (initiële replicatie of deltareplicatie) van een schijf. Hoe hoger de waarde, hoe meer netwerkbandbreedte er voor replicatie wordt gebruikt.
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** geeft het aantal threads die worden gebruikt voor gegevensoverdracht tijdens failback.

### <a name="throttle-bandwidth"></a>Bandbreedte beperken

1. Open de Azure Backup MMC-module op de computer die fungeert als de processerver. Een snelkoppeling voor back-up is standaard beschikbaar zijn op het bureaublad of in de volgende map: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik in de module op **Eigenschappen wijzigen**.

    ![Schermopname van Azure Backup MMC-module optie Eigenschappen wijzigen](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Op de **bandbreedtebeperking** tabblad **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**. Stel de limieten voor het werk en niet-werk uren. Het geldige bereik ligt tussen 512 Kbps en 102 Mbps.

    ![Schermopname van Azure back-up eigenschappenvenster](./media/site-recovery-vmware-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om een beperking in te stellen. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** geeft aan dat er geen beperking is vereist.

### <a name="influence-network-bandwidth-for-a-vm"></a>Netwerkbandbreedte beïnvloeden voor een virtuele machine

1. Navigeer in register van de VM naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Wijzig de waarde van het verkeer van de bandbreedte op de replicerende schijf beïnvloeden, **UploadThreadsPerVM**, of maak de sleutel als deze niet bestaat.
   * Wijzig de waarde van de bandbreedte voor failbackverkeer vanuit Azure beïnvloeden, **DownloadThreadsPerVM**.
2. De standaardwaarde is 4. In netwerken met overprovisioning moeten deze registersleutels zodanig worden gewijzigd dat niet de standaardwaarden worden gebruikt. Het maximum is 32. Houd het verkeer in de gaten om de waarde te optimaliseren.


## <a name="deploy-additional-process-servers"></a>Aanvullende processen servers implementeren

Als u Schalen buiten uw implementatie dan 200 bronmachines, of u hebt een totaal dagelijks verloop in verhouding van meer dan 2 TB, moet u extra processervers voor de verwerking van het netwerkverkeer. Volg deze instructies voor het instellen van de processerver. Na het instellen van de server, moet u bronmachines voor het gebruik van deze migreren.

1. In **siteherstel servers**, klikt u op de configuratieserver en klik vervolgens op **processerver**.

    ![Schermopname van Site Recovery servers optie een processerver toevoegen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. In **servertype**, klikt u op **processerver (lokale)**.

    ![Schermafbeelding van de processerver dialoogvenster](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Download het bestand van Site Recovery Unified Setup en voer dit voor het installeren van de processerver. Dit ook geregistreerd in de kluis.
4. In **Voordat u begint** selecteert u **Add additional process servers to scale out deployment** (Extra processervers toevoegen om implementatie uit te schalen).
5. Voltooi de wizard op dezelfde manier als u dit hebt gedaan wanneer u [instellen](#step-2-set-up-the-source-environment) de configuratieserver.

    ![Schermopname van Azure Site Recovery Unified Setup-wizard](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. In **Server configuratiedetails**, het IP-adres van de configuratieserver en de wachtwoordzin opgeven. Uitvoeren als u de wachtwoordzin, **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** op de configuratieserver.

    ![Schermopname van Server configuratiedetails pagina](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Machines voor het gebruik van de nieuwe processerver migreren
1. In **instellingen** > **siteherstel servers**, klikt u op de configuratieserver uit en vouw vervolgens **servers verwerken**.

    ![Schermafbeelding van de processerver dialoogvenster](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Met de rechtermuisknop op de processerver die momenteel in gebruik en klik op **Switch**.

    ![Schermopname van configuratie in het dialoogvenster van server](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. In **proces van Select doelserver**, selecteer de nieuwe processerver die u wilt gebruiken en selecteer vervolgens de virtuele machines die de server wordt afgehandeld. Klik op het informatiepictogram voor informatie over de server. Om te kunnen maken met het laden van de beslissingen, wordt de gemiddelde ruimte die nodig is om elke geselecteerde virtuele machine worden gerepliceerd naar de nieuwe processerver weergegeven. Klik op het vinkje om te repliceren naar de nieuwe processerver.


## <a name="next-steps"></a>Volgende stappen

Downloaden en uitvoeren van de [Azure Site Recovery-implementatie plannen](https://aka.ms/asr-deployment-planner)
