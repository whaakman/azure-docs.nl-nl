---
title: Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het Hyper-V-machines in Azure een failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72065d01-ed0f-430e-b117-a5885cecd1db
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/15/2017
ms.author: raynew
ms.openlocfilehash: e1cc21661450a983c25b24fe2a6228e26ceecec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar Azure

De [Azure Site Recovery](site-recovery-overview.md) service beheert en regelt de replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie wordt beschreven hoe u een Hyper-V virtuele machine in Azure een failover. Nadat u hebt failover, schakelt u terug naar uw on-premises site wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Failover van Hyper-V-machines on-premises naar Azure
> * Failback van Azure met on-premises en repliceren naar Azure opnieuw starten

## <a name="overview"></a>Overzicht
Failover en failback bestaat uit twee fasen:

1. **Failover naar Azure**: failover machines van de lokale site naar Azure.
2. **Failback van Azure met on-premises**: een geplande failover uitvoeren van Azure met on-premises. Na de geplande failover moet u omgekeerde replicatie van on-premises repliceren naar Azure opnieuw inschakelen. 


## <a name="fail-over-to-azure"></a>Failover naar Azure

### <a name="failover-prerequisites"></a>Vereisten voor failover

Failover voltooien:

- Zorg ervoor dat u hebt voltooid, een [disaster recovery inzoomen](tutorial-dr-drill-azure.md) om te controleren of alles werkt zoals verwacht.
- Bereid desgewenst verbinding maken met virtuele Azure-machines voordat u een failover uitvoert.
- Controleer de eigenschappen van de virtuele machine voordat u de failover uitvoert.

#### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Als u verbinding maken met virtuele Azure-machines met RDP na een failover wilt, het volgende doen:

##### <a name="azure-vms-running-windows"></a>Virtuele machines in Azure waarop Windows wordt uitgevoerd

1. Voor toegang tot Azure VM's via het internet, schakelt u RDP op de lokale virtuele machine voordat failover wordt uitgevoerd. Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
2. Als u wilt openen via site-naar-site VPN, schakelt u RDP in op de on-premises machine. RDP moet worden toegestaan de **Windows Firewall** -> **toegestane apps en functies** voor **domein- en persoonlijke** netwerken. Controleer of de SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er moet geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeren. Als er, kunt u niet aanmelden met de virtuele machine totdat de update is voltooid. 
3. Controleer op de Windows Azure virtuele machine na een failover **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven. Als u geen verbinding maken, Controleer of de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


##### <a name="azure-vms-running-linux"></a>Virtuele machines in Azure waarop Linux wordt uitgevoerd

1. Controleer op de lokale machine vóór de failover, dat de Secure Shell-service is ingesteld op automatisch starten op opstarten van het systeem. Controleer dat de firewallregels voor een SSH-verbinding toestaan.
2. Op de Azure virtuele machine na een failover, kunt u binnenkomende verbindingen voor de SSH-poort voor de netwerkbeveiligingsgroepen op failover VM en voor het Azure-subnet waarmee deze verbonden.  [Een openbaar IP-adres toevoegen](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) voor de virtuele machine. U kunt controleren **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven.


#### <a name="verify-vm-properties"></a>Controleer de eigenschappen van de virtuele machine

Controleer of de VM-eigenschappen en zorg ervoor dat de virtuele machine voldoet aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. In **beveiligde Items**, klikt u op **gerepliceerde Items** > VM.
2. In de **gerepliceerde item** deelvenster er is een overzicht van VM-informatie, gezondheidsstatus, en de meest recente beschikbare herstelpunten. Klik op **eigenschappen** om meer details te bekijken.
3. In **berekening en netwerk**, kunt u de naam van Azure, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en [beheerde instellingen voor de schijf](#managed-disk-considerations)
4. U kunt weergeven en wijzigen van de netwerkinstellingen, met inbegrip van de netwerk-en het subnet waarin de virtuele machine in Azure worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.
5. In **schijven**, vindt u informatie over het besturingssysteem en gegevensschijven op de virtuele machine.


### <a name="run-a-failover-from-on-premises-to-azure"></a>Een failover uitvoeren van on-premises naar Azure

U kunt een reguliere of geplande failover uitvoeren voor Hyper-V-machines

- Gebruik een reguliere failover voor onverwachte storingen. Wanneer u deze failover uitvoert, Site Recovery een virtuele machine in Azure maakt en drijft de. U kunt de failover uitvoeren op een specifiek herstelpunt. Verlies van gegevens kan optreden, afhankelijk van het herstelpunt dat u gebruikt.
- Een geplande failover kan worden gebruikt voor onderhoud, hetzij tijdens de verwachte uitval. Deze optie biedt geen gegevens verloren gaan. Wanneer een geplande failover wordt geactiveerd, worden de Bronmachines afgesloten. Niet-gesynchroniseerde gegevens worden gesynchroniseerd en de failover wordt geactiveerd.

Deze procedure wordt beschreven hoe een reguliere failover uit te voeren.


1. In **instellingen** > **gerepliceerde items** klikt u op de virtuele machine > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover naar. U kunt een van de volgende opties gebruiken:
    - **Meest recente** (standaard): deze optie verwerkt eerst alle gegevens die worden verzonden naar Site Recovery. Het biedt de laagste RPO (beoogd herstelpunt) omdat de virtuele machine in Azure gemaakt nadat de failover is de gegevens die is gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
    - **Meest recente verwerkte**: deze optie is overgenomen van de virtuele machine naar het recentste herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
    - **Meest recente app-consistente**: deze optie is overgenomen van de virtuele machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery. 
    - **Aangepaste**: Geef een herstelpunt.
3. Als u bent Hyper-V-machines in System Center VMM-clouds naar Azure, failover en gegevensversleuteling is ingeschakeld voor de cloud in **versleutelingssleutel**, selecteert u het certificaat dat was uitgegeven wanneer u de versleuteling van gegevens tijdens de installatie van de Provider ingeschakeld op de VMM-server...
4. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Site Recovery wordt ook proberen te synchroniseren van on-premises gegevens die nog niet nog naar Azure, verzonden voordat de failover. Houd er rekening mee dat de failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
5. Als u verbinding maken met de Azure VM voorbereid, verbinding maken met na de failover te valideren.
6. Nadat u hebt gecontroleerd, **doorvoeren** de failover. Hiermee verwijdert u de beschikbare herstelpunten.

> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: voordat failover wordt gestart, VM-replicatie is gestopt. Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.  


## <a name="fail-back-from-azure-to-on-premises"></a>Failback van Azure met on-premises

### <a name="prerequisites-for-failback"></a>Vereisten voor failback

Zorg dat de primaire VMM-server/Hyper-V-server is verbonden met Site Recovery voor het voltooien van failback.


### <a name="run-a-failback-and-reprotect-on-premises-vms"></a>Een failback uitvoeren en beveiligt on-premises virtuele machines

Failover van Azure naar de lokale site en start de virtuele machines van de lokale site repliceren naar Azure.

1. In **instellingen** > **gerepliceerde items** klikt u op de virtuele machine > **geplande Failover**.
2. In **geplande Failover bevestigen**, Controleer of de failoverrichting (van Azure) en selecteert u de bron- en doellocaties. 
3. In **gegevenssynchronisatie**, opgeven hoe u wilt synchroniseren:
    - **Synchroniseren van gegevens voordat failover wordt uitgevoerd (alleen nog deltawijzigingen gesynchroniseerd)**— deze optie wordt de virtuele machine uitvaltijd geminimaliseerd omdat het synchroniseert zonder dat de virtuele machine afgesloten. Dit is wat het doet:
        1. Maakt een momentopname van de virtuele machine van Azure en kopieert deze naar de lokale Hyper-V-host. De virtuele machine wordt uitgevoerd in Azure.
        2. De virtuele machine van Azure afgesloten zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set wijzigingen worden overgebracht naar de on-premises server en de lokale virtuele machine wordt gestart.
    - **Synchroniseren van gegevens tijdens de failover alleen (volledige download)**: Gebruik deze optie als u gedurende een lange periode hebt zijn uitgevoerd in Azure. Deze optie is sneller, omdat er meerdere schijfwijzigingen verwacht, en niet besteed tijd aan controlesom voldoende. Deze optie voert het downloaden van een schijf. Het is ook handig wanneer de lokale virtuele machine is verwijderd.
4. Als u bent Hyper-V-machines in System Center VMM-clouds naar Azure, failover en gegevensversleuteling is ingeschakeld voor de cloud in **versleutelingssleutel**, selecteert u het certificaat dat was uitgegeven wanneer u de versleuteling van gegevens tijdens de installatie van de Provider ingeschakeld op de VMM-server.
5. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
6. Als u hebt geselecteerd voor het synchroniseren van gegevens voordat de failover wordt uitgevoerd, nadat de eerste gegevenssynchronisatie wordt uitgevoerd en u klaar bent om het virtuele Azure-machines klikt u op afsluiten **taken** > geplande failover taaknaam > **volledige Failover**. De Azure VM is afgesloten, de overgedragen van de meest recente wijzigingen on-premises en de lokale virtuele machine wordt gestart.
7. Meld u bij de lokale virtuele machine om te controleren is beschikbaar, zoals verwacht.
8. De lokale virtuele machine is nu in behandeling doorvoeren. Klik op **doorvoeren**, de failover doorvoeren. Dit wordt verwijderd van de Azure VM's en de schijven en wordt de lokale virtuele machine voorbereid voor omgekeerde replicatie.
9. Inschakelen voor het starten van de lokale virtuele machine repliceren naar Azure, **omgekeerde replicatie**.


> [!NOTE]
> Omgekeerde replicatie repliceert alleen wijzigingen die hebben plaatsgevonden sinds de Azure VM is uitgeschakeld en alleen nog deltawijzigingen worden verzonden.

