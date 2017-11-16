---
title: Herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines met Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen in Azure voor lokale virtuele VMware-machines met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 461feb952f7e2eddba9c7218b3463868e8cb7965
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines

Deze zelfstudie laat zien hoe u voor het instellen van herstel na noodgevallen in Azure voor lokale VMware virtuele machine met Windows. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken voor Site Recovery
> * Instellen van de bron en doel van de replicatie-omgevingen
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een virtuele machine

Dit is de derde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)

Voordat u begint, is het handig om [bekijken van de architectuur](concepts-vmware-to-azure-architecture.md) voor noodherstelscenario.

## <a name="configure-vmware-account-permissions"></a>Machtigingen van de VMware-account configureren

1. Maak een rol op het niveau van vCenter. Geef de naam op voor de rol **Azure_Site_Recovery**.
2. De volgende machtigingen toewijzen aan de **Azure_Site_Recovery** rol.

   **Taak** | **Rolmachtigingen /** | **Details**
   --- | --- | ---
   **VM-detectie** | Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Ten minste een alleen-lezen-gebruiker.<br/><br/> Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).
   **Een volledige replicatie, failover en failback** |  Data Center-object doorgeven aan het onderliggende Object –> rol Azure_Site_Recovery =<br/><br/> Datastore -> ruimte toewijzen, gegevensopslag, op laag niveau bestandsbewerkingen bladeren, bestand verwijderen, bestanden van virtuele machine bijwerken<br/><br/> Netwerk -> netwerk toewijzen<br/><br/> Resource -> VM toewijzen aan de resourcegroep, migreren van virtuele machine is uitgeschakeld, stroomvoorziening op virtuele machine migreren<br/><br/> Taken maken taak, updatetaak -><br/><br/> Virtuele machine-configuratie ><br/><br/> Virtuele machine -> Interact -> vraag beantwoorden, apparaatverbinding, CD's configureren, configureren van diskettes, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> inventaris -> maken, registreren, registratie<br/><br/> Virtuele machine inrichten -> -> downloaden van de virtuele machine toestaan, toestaan uploaden van bestanden van virtuele machine<br/><br/> Virtuele machine-momentopnamen >-Remove momentopnamen > | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).

3. Maak een gebruiker op de vCenter-server of vSphere-host. De rol wordt toegewezen aan de gebruiker.

## <a name="specify-what-you-want-to-replicate"></a>Geef op wat u wilt repliceren

De Mobility-service moet worden geïnstalleerd op elke virtuele machine die u wilt repliceren. Site Recovery wordt deze service automatisch geïnstalleerd wanneer u replicatie voor de virtuele machine inschakelt. Automatische installatie moet u een account die door Site Recovery wordt gebruikt voor toegang tot de virtuele machine voorbereiden.

U kunt een domein of lokale account gebruiken. Voor virtuele Linux-machines, moet het account hoofdmap op de bronserver Linux zijn. Voor Windows virtuele machines, als u niet een domeinaccount, Schakel externe gebruiker toegangsbeheer op de lokale computer:

  - In de registery onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, de DWORD-vermelding toevoegen **LocalAccountTokenFilterPolicy** en de waarde ingesteld op 1.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Instellen van de bronomgeving bestaat uit het downloaden van de Site Recovery Unified Setup, instellen van de configuratieserver en registreren van deze in de kluis en detectie van virtuele machines.

De configuratieserver is één lokale VMware VM voor het hosten van alle onderdelen van de Site Recovery. Deze virtuele machine wordt uitgevoerd voor de configuratieserver, de processerver en de hoofddoelserver.

- De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- De processerver fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. De Mobility-service de processerver ook geïnstalleerd op virtuele machines die u repliceren wilt, en voert u automatische detectie van virtuele machines op de on-premises VMware-servers.
- De hoofddoelserver verwerkt replicatiegegevens tijdens de failback vanuit Azure.

De configuratieserver VM moet een maximaal beschikbare VMware VM die voldoet aan de volgende vereisten:

| **Vereiste** | **Details** |
|-----------------|-------------|
| Aantal CPU-kernen| 8 |
| RAM | 12 GB |
| Aantal schijven | 3 - besturingssysteemschijf, proces-serverschijf cache, bewaarstation (voor failback) |
| Vrije schijfruimte (cache van de processerver) | 600 GB |
| Vrije schijfruimte (bewaarschijf) | 600 GB |
| Besturingssysteem | Windows Server 2012 R2 |
| Landinstelling van het besturingssysteem | Engels (en-us) |
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Windows Server-functies | Deze rollen niet inschakelt: Active Directory Domain Services, Internet Information Services, Hyper-V |
| NIC-type | VMXNET3 |
| Type IP-adres | Statisch |
| Poorten | 443 (Orchestration-besturingselement)<br/>9443 (Gegevenstransport)|

Zorg ervoor dat de systeemklok is gesynchroniseerd met een Time-Server op de configuratieserver VM.
Tijd moet worden gesynchroniseerd binnen 15 minuten. Als het tijdsverschil groter dan 15 minuten is, mislukt de installatie.

Zorg ervoor dat de configuratieserver toegang hebt tot deze URL's:

   [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Alle IP-adressen gebaseerde firewallregels moeten communicatie met Azure toestaan.

- Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.
    - IP-adresbereiken voor de Azure-regio van uw abonnement en voor VS-West (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.

Alle IP-adressen gebaseerde firewallregels moeten kunnen communiceren met toestaan [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en de poorten 443 (HTTPS) en 9443 (gegevensreplicatie). Zorg ervoor dat IP-adresbereiken voor de Azure-regio van uw abonnement en voor VS-West (gebruikt voor toegangsbeheer en identiteitsbeheer) toestaan.

### <a name="download-the-site-recovery-unified-setup"></a>Downloaden van de Site Recovery Unified Setup

1. Open de [Azure-portal](https://portal.azure.com) en klik op **alle resources**.
2. Klik op de Recovery Services-kluis met de naam **ContosoVMVault**.
3. Klik op **siteherstel** > **infrastructuur voorbereiden** > **beveiligingsdoel**.
4. Selecteer **On-premises** voor waar uw machines zich bevinden, **naar Azure** voor waar u uw machines repliceren en **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.
5. Klik in het deelvenster van de bron voorbereiden op **+ configuratieserver**.
6. In **Server toevoegen**, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
7. Download het installatiebestand van de Site Recovery Unified Setup.
8. Download de kluisregistratiesleutel. U moet dit wanneer u Unified Setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

   ![Bron instellen](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>De configuratieserver instellen

1. Voer het installatiebestand voor de geïntegreerde Setup uit.
2. In **voordat u begint**, selecteer **installeren van de configuratieserver en de processerver** klikt u vervolgens op **volgende**.

3. In **van derden softwarelicentie**, klikt u op **ik ga akkoord** wilt downloaden en installeren van MySQL, klikt u vervolgens op **volgende**.

4. Selecteer bij **Registratie** de registratiesleutel die u hebt gedownload uit de kluis.

5. Geef bij **Internetinstellingen** op hoe de provider die op de configuratieserver wordt uitgevoerd, via internet verbinding moet maken met Azure Site Recovery.

   - Als u verbinding wilt maken met de proxy die momenteel is ingesteld op de computer, selecteer **verbinding maken met Azure Site Recovery via een proxyserver**.
   - Als u wilt dat de Provider rechtstreeks verbinding maakt, selecteert u **rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
   - Als de bestaande proxy verificatie vereist is of als u wilt een aangepaste proxy gebruikt voor de providerverbinding, selecteer **verbinding maken met aangepaste proxyinstellingen**, en het adres, poort en referenties opgeven.

   ![Firewall](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. Tijdens Setup wordt in **Controle op vereisten** gecontroleerd of de installatie kan worden uitgevoerd. Als er een waarschuwing wordt weergegeven over **Synchronisatiecontrole voor algemene tijd**, moet u controleren of de tijd op de systeemklok (instellingen voor **datum en tijd**) overeenkomt met de tijdzone.

   ![Vereisten](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. Maak bij **MySQL-configuratie** referenties voor aanmelden bij de MySQL-serverinstantie die is geïnstalleerd.

8. In **omgeving Details**, selecteer **Ja** beveiligen van virtuele VMware-machines. Setup controleert of PowerCLI 6.0 is geïnstalleerd.

9. Selecteer bij **Installatielocatie** waar u de binaire bestanden wilt installeren en de cache wilt opslaan. Het station dat u selecteert, moet ten minste 5 GB vrije schijfruimte bevatten, maar wij raden u aan een cachestation te gebruiken met minstens 600 GB vrije ruimte.

10. Geef bij **Netwerk selecteren** de listener op (netwerkadapter en SSL-poort) met behulp waarvan de configuratieserver replicatiegegevens verzendt en ontvangt. Poort 9443 is de standaardpoort voor het verzenden en ontvangen van replicatieverkeer, maar u kunt dit poortnummer aanpassen aan de vereisten van de omgeving. We ook openen poort 443, die wordt gebruikt voor het indelen van replicatiebewerkingen. Gebruik geen poort 443 voor het verzenden of ontvangen van replicatieverkeer.

11. Lees de informatie bij **Samenvatting** en klik op **Installeren**. Setup installeert de configuratieserver en registreert aan de Azure Site Recovery-service.

    ![Samenvatting](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Wanneer de installatie is voltooid, wordt er een wachtwoordzin gegenereerd. U hebt deze nodig bij het inschakelen van de replicatie. Kopieer de wachtwoordzin daarom en bewaar deze op een veilige locatie. De server wordt weergegeven op de **instellingen** > **Servers** deelvenster in de kluis.

### <a name="configure-automatic-discovery"></a>Configureer Automatische detectie

Voor het detecteren van virtuele machines, moet de configuratieserver verbinding maken met on-premises VMware-servers. Voeg de vCenter-server, of vSphere-hosts met een account dat beheerdersrechten op de server heeft voor de doeleinden van deze zelfstudie.

1. Start op uw configuratieserver **CSPSConfigtool.exe**. Dit bestand is beschikbaar als snelkoppeling op het bureaublad en in de map *installatielocatie*\home\svsystems\bin.

2. Klik op **Accounts beheren** > **Account toevoegen**.

   ![Account toevoegen](./media/tutorial-vmware-to-azure/credentials1.png)

3. Voeg in **Accountdetails** het account toe dat moet worden gebruikt voor automatische detectie.

   ![Details](./media/tutorial-vmware-to-azure/credentials2.png)

Een server toevoegen:

1. Open de [Azure-portal](https://portal.azure.com) en klik op **alle resources**.
2. Klik op de Recovery Services-kluis met de naam **ContosoVMVault**.
3. Klik op **Site Recovery** > **infrastructuur voorbereiden** > **bron**
4. Selecteer **+ vCenter** verbinding maken met een vCenter-server of vSphere ESXi-host.
5. In **vCenter toevoegen**, Geef een beschrijvende naam voor de server. Geef vervolgens de IP-adres of FQDN-naam.
6. Laat de poort 443, tenzij uw VMware-Server naar aanvragen op een andere poort luistert.
7. Selecteer het account moet worden gebruikt voor het verbinden met de server. Klik op **OK**.

Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen en virtuele machines worden gedetecteerd.

> [!NOTE]
> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Klik op **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Opgeven of de doel-implementatiemodel is Resource Manager gebaseerde of klassieke.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

   ![Doel](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Open de [Azure-portal](https://portal.azure.com) en klik op **alle resources**.
2. Klik op de Recovery Services-kluis met de naam **ContosoVMVault**.
3. Klik op om een beleid voor wachtwoordreplicatie **Site Recovery-infrastructuur** > **replicatiebeleid** > **+ replicatiebeleid**.
4. In **maken van beleid voor wachtwoordreplicatie**, een beleidsnaam opgeven **VMwareRepPolicy**.
5. In **RPO drempelwaarde**, gebruikt u de standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Een waarschuwing wordt gegenereerd als continue replicatie deze limiet overschrijdt.
6. In **herstel bewaarperiode**, gebruik de standaardwaarde van 24 uur voor hoe lang de bewaarperiode voor elk herstelpunt is. Voor deze zelfstudie selecteert u 72 uur. Gerepliceerde virtuele machines kunnen worden hersteld naar een willekeurig punt in een venster.
7. In **App-consistente momentopname frequentie**, de standaardwaarde van 60 minuten gebruiken voor de frequentie dat toepassingsconsistente momentopnamen worden gemaakt. Klik op **OK** om het beleid te maken.

   ![Beleid](./media/tutorial-vmware-to-azure/replication-policy.png)

Het beleid is automatisch gekoppeld aan de configuratieserver. Standaard wordt automatisch een overeenkomende beleid gemaakt voor failback. Bijvoorbeeld, als het replicatiebeleid **rep beleid** wordt het failbackbeleid voor **rep-beleid-failback**. Dit beleid wordt niet gebruikt, totdat u een failback vanuit Azure initiëren.

## <a name="enable-replication"></a>Replicatie inschakelen

Site Recovery installeert de Mobility-service wanneer replicatie is ingeschakeld voor een virtuele machine. Het kan 15 minuten of langer om wijzigingen te laten uitvoeren en in de portal weergegeven.

Replicatie als volgt inschakelen:

1. Klik op **toepassing repliceren** > **bron**.
2. In **bron**, selecteer de configuratieserver.
3. In **type Machine**, selecteer **virtuele Machines**.
4. In **vCenter/vSphere-Hypervisor**, selecteert u de vCenter-server die de host vSphere beheert of Selecteer de host.
5. Selecteer de processerver (configuratieserver). Klik op IThen **OK**.
6. In **doel**, selecteer het abonnement en de resourcegroep waarin u wilt de mislukte over virtuele machines maken. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of de resource management), voor failover virtuele machines.
7. Selecteer de Azure storage-account dat u gebruiken wilt voor het repliceren van gegevens.
8. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
9. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren.
10. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.
11. In **eigenschappen** > **eigenschappen configureren**, selecteer het account dat wordt gebruikt door de processerver voor het installeren van de Mobility-service automatisch op de machine.
12. In **replicatie-instellingen** > **replicatie-instellingen configureren**, Controleer of de juiste replicatiebeleid is geselecteerd.
13. Klik op **replicatie inschakelen**.

U kunt de voortgang van volgen de **beveiliging inschakelen** taak **instellingen** > **taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.

Voor het bewaken van virtuele machines die u toevoegt, kunt u de laatste keer dat gedetecteerde controleren voor virtuele machines in **Configuration Servers**
> **Laatste Contact op**. Als u wilt toevoegen virtuele machines zonder te wachten op de geplande detectie, markeer de configuratieserver (Klik niet op deze), en klik op **vernieuwen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](site-recovery-test-failover-to-azure.md)
