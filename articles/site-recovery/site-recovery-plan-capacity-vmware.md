---
title: Plannen van capaciteit en schaalbaarheid voor noodherstel van VMware naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: In dit artikel kunt u plannen van capaciteit en schaalbaarheid bij het instellen van herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van Azure Site Recovery.
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 29e01177d4b096449cd906a22b47223078c6493e
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107817"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Plannen van capaciteit en schaalbaarheid voor noodherstel van VMware naar Azure

In dit artikel gebruiken voor het plannen van capaciteit en schalen wanneer u on-premises VMware-machines en fysieke servers naar Azure met behulp van repliceren [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Hoe start ik plannen van capaciteit?

Voor meer informatie over vereisten voor Azure Site Recovery-infrastructuur, verzamelt u informatie over uw replicatieomgeving door [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner-doc) voor VMware-replicatie. Zie voor meer informatie, [over Site Recovery Deployment Planner voor VMware naar Azure](site-recovery-deployment-planner.md). 

Site Recovery Deployment Planner biedt een rapport met volledige informatie over compatibele en niet-compatibele virtuele machines, schijven per VM en gegevensverloop per schijf. Het hulpprogramma bevat ook een overzicht van netwerkbandbreedtevereisten om te voldoen aan de RPO-doel en de Azure-infrastructuur die zijn voor succesvolle replicatie en failover vereist.

## <a name="capacity-considerations"></a>Overwegingen voor capaciteit

Onderdeel | Details
--- | ---
**Replicatie** | **Maximale dagelijkse veranderingssnelheid**: Een beveiligde virtuele machine kan slechts één processerver gebruiken. Een dagelijkse kan worden verwerkt door een enkele processerver wijzigen beoordelen tot 2 TB. 2 TB is dus dat de maximale dagelijkse veranderingssnelheid van gegevens die wordt ondersteund voor een beveiligde machine.<br /><br /> **Maximale doorvoer**: Een gerepliceerde machine kan deel uitmaken van een opslagaccount in Azure. Een standaard Azure Storage-account kan een maximum van 20.000 aanvragen per seconde verwerken. Het is raadzaam dat u het aantal i/o-bewerkingen per seconde (IOP's) via een bronmachine 20.000 beperken. Bijvoorbeeld, als u een bronmachine met vijf schijven en elke schijf 120 IOPS (8 kB groot) op de broncomputer genereert, is de bron-VM in de Azure disk IOPS-limiet van 500. (Het aantal vereiste opslagaccounts voor is gelijk aan de totale bronmachine gedeeld door 20.000 IOP's.)
**Configuratieserver** | De configuratieserver moet mogelijk voor het afhandelen van het dagelijkse tarief capaciteit voor wijzigen voor alle workloads die worden uitgevoerd op beveiligde machines. De configuratie-computer moet voldoende bandbreedte continu om gegevens te repliceren naar Azure Storage.<br /><br /> Er is een best practice om de configuratieserver op hetzelfde netwerk- en LAN-segment als de machines die u wilt beveiligen. U kunt de configuratieserver plaatsen op een ander netwerk, maar machines die u wilt beveiligen Laagzichtbaarheid-3-netwerk moeten hebben.<br /><br /> Aanbevelingen voor de grootte voor de configuratieserver worden samengevat in de tabel in de volgende sectie.
**Processerver** | De eerste processerver is standaard geïnstalleerd op de configuratieserver. U kunt extra processervers om te schalen van uw omgeving kunt implementeren. <br /><br /> De processerver ontvangt gegevens van replicatie van beveiligde machines. De processerver optimaliseert de gegevens met behulp van caching, compressie en codering. De processerver verzendt vervolgens de gegevens naar Azure. De proces-server-machine moet voldoende bronnen zijn voor deze taken uitvoeren.<br /><br /> De processerver maakt gebruik van een cache op basis van een schijf. Gebruik een afzonderlijke cacheschijf 600 GB of meer voor het afhandelen van gegevenswijzigingen die zijn opgeslagen als een knelpunt netwerk of een storing optreedt.

## <a name="size-recommendations-for-the-configuration-server-and-inbuilt-process-server"></a>Aanbevelingen voor de grootte voor de configuratieserver en ingebouwde processerver

Een configuratieserver die gebruikmaakt van een ingebouwde processerver ter bescherming van de werkbelasting kan verwerken van maximaal 200 virtuele machines op basis van de volgende configuraties:

CPU | Geheugen | Cachegrootte van de schijf | Veranderingssnelheid van gegevens | Beveiligde machines
--- | --- | --- | --- | ---
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB of minder | Gebruik voor het repliceren van minder dan 100 machines.
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) | 18 GB | 600 GB | 501 GB tot 1 TB | Gebruik voor het repliceren van 100-150-machines.
16 vcpu's (2-sockets * 8 kernen \@ 2,5 GHz) | 32 GB | 1 TB | > 1 TB tot 2 TB | Gebruiken om te repliceren 151 tot 200 computers.
Een andere configuratieserver implementeren met behulp van een [OVF-sjabloon](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). | | | | Een nieuwe configuratieserver implementeren als u meer dan 200 machines repliceert.
Implementatie van een andere [processerver](vmware-azure-set-up-process-server-scale.md#download-installation-file). | | | &GT; 2 TB| Een nieuwe uitbreidbare processerver implementeren als de totale dagelijkse veranderingssnelheid van gegevens groter dan 2 TB is.

In deze configuraties:

* Elke bron-VM heeft drie schijven van 100 GB elk.
* We benchmarking opslag van acht shared access signature-stations van 10 K RPM gebruikt met RAID 10 voor de cache schijf metingen.

## <a name="size-recommendations-for-the-process-server"></a>Aanbevelingen voor de grootte voor de processerver

De processerver is het onderdeel die verantwoordelijk is voor replicatie van gegevens in Azure Site Recovery. Als de dagelijkse veranderingssnelheid groter dan 2 TB is, moet u scale-out processervers voor het afhandelen van de replicatie-belasting toevoegen. Als u wilt schalen, kunt u het volgende doen:

* Het aantal configuratieservers verhogen door te implementeren met behulp van een [OVF-sjabloon](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). Bijvoorbeeld, kunt u maximaal 400 machines beveiligen met behulp van twee configuratieservers.
* Voeg [scale-out processervers](vmware-azure-set-up-process-server-scale.md#download-installation-file). De scale-out processervers gebruiken voor het afhandelen van replicatieverkeer in plaats van (of naast) de configuratieserver.

De volgende tabel worden beschreven in dit scenario:

* Instellen van een uitbreidbare processerver.
* U beveiligde virtuele machines voor het gebruik van de uitbreidbare processerver hebt geconfigureerd.
* Elke beveiligde bron-VM heeft drie schijven van 100 GB elk.

Aanvullende processerver | Cachegrootte van de schijf | Veranderingssnelheid van gegevens | Beveiligde machines
--- | --- | --- | ---
4 vcpu's (2-sockets * 2 kernen \@ 2,5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | Gebruik voor het repliceren van 85 of minder machines.
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz), 12 GB geheugen | 600 GB | 251 GB tot 1 TB | Gebruiken om te repliceren 86-150-machines.
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) 24 GB geheugen | 1 TB | > 1 TB tot 2 TB | Gebruiken om te repliceren 151-225 machines.

Hoe u de schaal van uw servers, is afhankelijk van uw voorkeur voor een scale-up- of scale-out-model. Een paar geavanceerde configuratieservers implementeren om omhoog te schalen, en verwerken van servers. Als u wilt schalen, door meer servers waarvoor minder bronnen te implementeren. Bijvoorbeeld, als u wilt 200 machines beveiligt met een algemene dagelijkse veranderingssnelheid van gegevens van 1,5 TB, kan u een van de volgende acties uitvoeren:

* Instellen van een enkel proces-server (16 vCPU, 24 GB aan RAM-geheugen).
* Instellen van twee processervers (2 x 8 vCPU, 2 * 12 GB aan RAM-geheugen).

## <a name="control-network-bandwidth"></a>Netwerkbandbreedte

Nadat u hebt gebruikt [Site Recovery Deployment Planner](site-recovery-deployment-planner.md) voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (initiële replicatie en klikt u vervolgens de delta-), die u hebt een aantal opties voor het beheren van de hoeveelheid bandbreedte die wordt gebruikt voor replicatie:

* **Bandbreedte beperken**: VMware-verkeer dat wordt gerepliceerd naar Azure wordt gerouteerd via een specifiek proces. U kunt de bandbreedte op de machines die worden uitgevoerd als processervers beperken.
* **Bandbreedte van invloed zijn op**: U kunt de bandbreedte die wordt gebruikt voor replicatie via diverse registersleutels beïnvloeden:
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** registerwaarde Hiermee geeft u het aantal threads die worden gebruikt voor gegevensoverdracht (initiële replicatie of deltareplicatie) van een schijf. Een hogere waarde verhoogt de netwerkbandbreedte die wordt gebruikt voor replicatie.
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** registerwaarde Hiermee geeft u het aantal threads die worden gebruikt voor gegevensoverdracht tijdens een failback.

### <a name="throttle-bandwidth"></a>Bandbreedte beperken

1. Open de Azure Backup MMC-module op de computer die u als de processerver gebruiken. Een snelkoppeling voor back-up is standaard beschikbaar zijn op het bureaublad of in de volgende map: C:\Program Files\Microsoft Azure Recovery Services Agent\bin.
2. Selecteer in de module **eigenschappen wijzigen**.

    ![Schermafbeelding van de Azure Backup MMC-module de optie om eigenschappen te wijzigen](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Op de **beperking** tabblad **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**. Stel de limieten in voor werktijden en niet-wordt gewerkt. Geldige bereiken liggen tussen 512 Kbps en 1023 Mbps.

    ![Schermopname van het dialoogvenster Eigenschappen van de Azure-back-up](./media/site-recovery-vmware-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om een beperking in te stellen. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** geeft aan dat er geen beperking is vereist.

### <a name="alter-the-network-bandwidth-for-a-vm"></a>De netwerkbandbreedte voor een virtuele machine wijzigen

1. In het register van de virtuele machine, gaat u naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Als u wilt wijzigen van de bandbreedteverkeer op een replicerende schijf, wijzig de waarde van **UploadThreadsPerVM**. Maak de sleutel aan als deze nog niet bestaat.
   * Als u wilt wijzigen van de bandbreedte voor failbackverkeer vanuit Azure, wijzig de waarde van **DownloadThreadsPerVM**.
2. De standaardwaarde voor elke sleutel is **4**. In netwerken met overprovisioning moeten deze registersleutels zodanig worden gewijzigd dat niet de standaardwaarden worden gebruikt. De maximale waarde die u kunt gebruiken is **32**. Houd het verkeer in de gaten om de waarde te optimaliseren.

## <a name="set-up-the-site-recovery-infrastructure-to-protect-more-than-500-vms"></a>Instellen van de Site Recovery-infrastructuur meer dan 500 VM's beveiligen

Voordat u de Site Recovery-infrastructuur hebt ingesteld, toegang krijgen tot de omgeving om te meten van de volgende factoren: compatibele virtuele machines, de dagelijkse veranderingssnelheid van gegevens, de vereiste netwerkbandbreedte voor de RPO die u bereiken wilt, het nummer van Site Recovery onderdelen die vereist zijn, en de tijd die nodig zijn om de initiële replicatie te voltooien. De volgende stappen voor het verzamelen van de vereiste gegevens:

1. Als u wilt deze parameters meten, Site Recovery Deployment Planner in uw omgeving worden uitgevoerd. Zie voor nuttige richtlijnen [over Site Recovery Deployment Planner voor VMware naar Azure](site-recovery-deployment-planner.md).
2. Implementeren van een configuratieserver die voldoet aan de [aanbevolen waarden voor de configuratieserver](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server). Als uw productie-werkbelasting groter is dan 650 virtuele machines, implementeert u een andere configuratieserver.
3. Implementeren op basis van de gemeten dagelijkse veranderingssnelheid van gegevens, [scale-out processervers](vmware-azure-set-up-process-server-scale.md#download-installation-file) met behulp van [richtlijnen grootte](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).
4. Als u verwacht dat de gegevenswijzigingssnelheid voor een virtuele machine van schijf 2 MBps, overschrijden [instellen van een Premium storage-account](tutorial-prepare-azure.md#create-a-storage-account). Site Recovery Deployment Planner wordt uitgevoerd voor een bepaalde periode. Pieken in de veranderingssnelheid van gegevens op andere momenten kunnen niet worden vastgelegd in het rapport.
5. [Instellen van de netwerkbandbreedte](site-recovery-plan-capacity-vmware.md#control-network-bandwidth) op basis van de RPO die u wilt bereiken.
6. Wanneer de infrastructuur is ingesteld, schakelt u herstel na noodgevallen voor uw workload. Voor meer informatie Zie [de bronomgeving instellen voor VMware naar Azure-replicatie](vmware-azure-set-up-source.md).

## <a name="deploy-additional-process-servers"></a>Extra processervers implementeren

Als u de schaal van uw implementatie meer dan 200 bronmachines of als u een totaal dagelijks verloop snelheid van meer dan 2 TB hebt, moet u processervers voor het afhandelen van het verkeersvolume toevoegen. Zie voor meer informatie over het instellen van de processerver, [schaal voor failback met behulp van extra processervers](vmware-azure-set-up-process-server-scale.md). Na het instellen van de processerver, kunt u migreren bronmachines om deze te gebruiken.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migreren van machines voor het gebruik van de nieuwe processerver

1. Selecteer **instellingen** > **Site Recovery-servers**. Selecteer de configuratieserver uit en vouw vervolgens **servers verwerken**.

    ![Schermafbeelding van het dialoogvenster van de processerver](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Met de rechtermuisknop op de processerver die momenteel in gebruik en selecteer vervolgens **Switch**.

    ![Schermopname van het dialoogvenster van de configuratie-server](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. In **doelprocesserver selecteren**, selecteert u de nieuwe processerver die u wilt gebruiken. Selecteer vervolgens de virtuele machines op de server af te handelen. Als u informatie over de server, selecteer het pictogram voor informatie. Voor hulp bij het laden van beslissingen maken, wordt de gemiddelde ruimte die is vereist voor het repliceren van elke geselecteerde virtuele machine naar de nieuwe processerver weergegeven. Klik op het vinkje om te beginnen met repliceren naar de nieuwe processerver.

## <a name="deploy-additional-master-target-servers"></a>Aanvullende hoofddoelservers implementeren

Meer dan één hoofddoelserver is in de volgende scenario's vereist:

*   Wilt u een op basis van Linux virtuele machine te beschermen.
*   De hoofddoelserver beschikbaar is op de configuratieserver heeft geen toegang tot de gegevensopslag van de virtuele machine.
*   Het totale aantal schijven op de hoofddoelserver (het nummer van lokale schijven op server) plus het aantal schijven moeten worden beveiligd, is groter dan 60 schijven.

Zie voor informatie over het toevoegen van een hoofddoelserver voor een op basis van Linux virtuele machine, [installeren van een Linux-hoofddoelserver voor failback](vmware-azure-install-linux-master-target.md).

Om toe te voegen een hoofddoelserver voor een Windows-virtuele machine:

1. Ga naar **Recovery Services-kluis** > **Site Recovery-infrastructuur** > **configuratieservers**.
2. Selecteer de vereiste configuratie-server en selecteer vervolgens **Masterdoelserver**.

    ![Schermafbeelding van de knop toevoegen Masterdoelserver](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Download het geïntegreerde setup-bestand en voer vervolgens het bestand op de virtuele machine voor het instellen van de hoofddoelserver.
4. Selecteer **installeren hoofddoel** > **volgende**.

    ![Schermafbeelding van de installatie van het hoofddoel selecteren](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Selecteer de standaardlocatie voor installatie, en selecteer vervolgens **installeren**.

     ![Schermafbeelding van de standaardlocatie voor installatie](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Selecteer voor het registreren van het hoofddoel met de configuratieserver, **doorgaan naar configuratie**.

    ![Schermafbeelding van de doorgaan naar configuratie-knop](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Geef het IP-adres van de configuratieserver en voer vervolgens de wachtwoordzin. Zie voor informatie over het genereren van een wachtwoordzin, [genereren een wachtwoordzin voor de configuratieserver](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase). 

    ![Schermopname die laat waar zien u de IP-adres en een wachtwoordzin invoert voor de configuratieserver](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Selecteer **Registreren**. Wanneer de registratie is voltooid, selecteert u **voltooien**.

Wanneer de registratie is voltooid, de server wordt weergegeven in de Azure portal op **Recovery Services-kluis** > **Site Recovery-infrastructuur**  >   **Configuratieservers**, in de hoofddoelservers van de configuratieserver.

 > [!NOTE]
 > Download de nieuwste versie van de [hoofddoel server geïntegreerde setup-bestand voor Windows](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Volgende stappen

Downloaden en uitvoeren van [Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner).
