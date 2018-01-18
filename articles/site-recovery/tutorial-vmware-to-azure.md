---
title: Herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines met Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen in Azure voor lokale virtuele VMware-machines met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8acc8deff8b635c97e8722d65a728aebf0e49bb3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines

Deze zelfstudie laat zien hoe u herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines waarop Windows wordt uitgevoerd. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Geef de replicatiebron en de doelserver.
> * De bron-replicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatie doelomgeving instellen.
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een virtuele machine

Dit is de derde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)

Voordat u begint, is het handig om [bekijken van de architectuur](concepts-vmware-to-azure-architecture.md) voor noodherstelscenario.


## <a name="select-a-replication-goal"></a>Selecteer een doel replicatie

1. In **Recovery Services-kluizen**, klikt u op de kluisnaam **ContosoVMVault**.
2. In **aan de slag**, klikt u op de Site Recovery. Klik vervolgens op **infrastructuur voorbereiden**.
3. In **beveiligingsdoel** > **waar worden de machines die zich**, selecteer **On-premises**.
4. In ** waar wilt u uw machines repliceren Selecteer **naar Azure**.
5. In **zijn uw gevirtualiseerde machines**, selecteer **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Als u de bronomgeving instelt, moet u een enkele, maximaal beschikbare, host on-premises Site Recovery-onderdelen op lokale machine. Onderdelen zijn de configuratieserver, de processerver en de hoofddoelserver.

- De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- De processerver fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. De Mobility-service de processerver ook geïnstalleerd op virtuele machines die u repliceren wilt, en de werking van automatische detectie van de lokale virtuele VMware-machines.
- De hoofddoelserver verwerkt replicatiegegevens tijdens de failback vanuit Azure.

Als u de configuratieserver als een maximaal beschikbare VMware VM instelt, kunt u een voorbereid OVF-sjabloon downloaden en importeren van de sjabloon in VMware voor de virtuele machine maken. Na het instellen van de configuratieserver, registreert u deze in de kluis. Na de registratie detecteert de Site Recovery lokale virtuele VMware-machines.

### <a name="download-the-vm-template"></a>De VM-sjabloon downloaden

1. In de kluis, gaat u naar **infrastructuur voorbereiden** > **bron**.
2. In **bron voorbereiden**, klikt u op **+ configuratieserver**.
3. In **Server toevoegen**, controleert u of **configuratieserver voor VMware** wordt weergegeven in **servertype**.
4. Download de sjabloon Open Virtualization Format (OVF) voor de configuratieserver.

  > [!TIP]
  De nieuwste versie van de configuratie van server-sjabloon rechtstreeks vanuit kan worden gedownload [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>Importeer de VMware-sjabloon

1. Meld u bij de VMware vCenter-server of vSphere ESXi-host, met behulp van de VMWare vSphere Client.
2. Op de **bestand** selecteert u **OVF-sjabloon implementeren**, om de wizard OVF-sjabloon implementeren te starten.  

     ![OVF-sjabloon](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. In **Select bron**, geef de locatie van de gedownloade OVF.
4. In **Bekijk de details**, klikt u op **volgende**.
5. In **naam en een map selecteren**, en **Select configuration**, accepteer de standaardinstellingen.
6. In **opslag selecteren**, voor het selecteren van de beste prestaties **Thick inrichten enthousiaste nul** in **Selecteer virtuele schijf-indeling**.
4. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
5. In **klaar om te voltooien**:
  - Als u de virtuele machine met de standaardinstellingen instelt, selecteert u **na de implementatie inschakelen** > **voltooien**.
  - Als u een extra netwerkinterface toevoegen wilt, schakelt u **na de implementatie inschakelen**, en selecteer vervolgens **voltooien**. De sjabloon van de server configuratie met één NIC wordt geïmplementeerd, maar u kunt extra netwerkinterfacekaarten toevoegen na de implementatie.

  
## <a name="add-an-additional-adapter"></a>Voeg een extra adapter

Als u een extra NIC en de configuratieserver toevoegen wilt, dat doen voordat u de server in de kluis registreren. Na de registratie wordt niet voor het toevoegen van extra adapters ondersteund.

1. In de vSphere Client voorraad, met de rechtermuisknop op de virtuele machine en selecteer **instellingen bewerken**.
2. In **Hardware**, klikt u op **toevoegen** > **Ethernet-Adapter**. Klik op **Volgende**.
3. Selecteer en adaptertype en een netwerk. 
4. Selecteer voor de virtuele NIC verbinding als de virtuele machine is ingeschakeld, **verbinding maken met power op**. Klik op **volgende** > **voltooien**, en klik vervolgens op **OK**.


## <a name="register-the-configuration-server"></a>Registreer de configuratieserver 

1. Schakel op de virtuele machine uit de VMWare vSphere Client-console.
2. De virtuele machine opnieuw opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en geef een administrator-wachtwoord.
3. Nadat de installatie is voltooid, meld u aan bij de virtuele machine als de beheerder.
4. De eerste keer dat u zich aanmeldt, de Azure Site Recovery Configuration Tool wordt gestart.
5. Geef een naam die wordt gebruikt voor het registreren van de configuratieserver met Site Recovery. Klik op **Volgende**.
6. Het hulpprogramma controleert of de virtuele machine verbinding met Azure maken kan. Nadat de verbinding is gemaakt, klikt u op **aanmelden**, aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u wilt registreren van de configuratieserver.
7. Het hulpprogramma voert enkele configuratietaken en vervolgens opnieuw opgestart.
8. Meld u bij de computer opnieuw. De configuratiewizard voor server management wordt automatisch gestart.

### <a name="configure-settings-and-connect-to-vmware"></a>Instellingen configureren en verbinding maken met VMware

1. In de configuratiewizard voor server management > **Setup-connectiviteit**, selecteer de NIC die replicatieverkeer ontvangt. Klik vervolgens op **Opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. In **Selecteer Recovery Services-kluis**, selecteert u uw Azure-abonnement en de betreffende resourcegroep en -kluis.
3. In **software van derden installeren**, accepteert u de licentie agreeemtn en klikt u op **downloaden en installeren**, MySQL-Server te installeren.
4. Klik op **installeren VMware PowerLCI**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doen. Klik vervolgens op **doorgaan**
5. In **configuratie valideren**, vereisten kunnen worden gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren**, geef de FQDN of IP-adres van de vCenter-server of vSphere-host, op welke virtuele machines die u wilt repliceren zich bevinden. Geef de poort waarop de server luistert en een beschrijvende naam moet worden gebruikt voor de VMware-server in de kluis.
7. Geef referenties op die door de configuratieserver worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren VMware-machines die beschikbaar voor replicatie zijn. Klik op **toevoegen**, en klik vervolgens op **doorgaan**.
8. In **referenties voor de virtuele machine configureren**, de gebruikersnaam en wachtwoord invoeren die wordt gebruikt voor de Mobility-service automatisch wordt geïnstalleerd op computers, wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account dat lokale administrator-bevoegdheden op de machines die u wilt repliceren. Geef details voor het hoofdaccount voor Linux.
9. Klik op **Finalize configuratie** om inschrijving te voltooien. 
10. Nadat de registratie is voltooid, wordt in de Azure portal controleren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Klik vervolgens op **OK** doelinstellingen configureren.


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
