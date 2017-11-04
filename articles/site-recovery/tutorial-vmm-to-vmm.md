---
title: Instellen van herstel na noodgevallen voor Hyper-V virtuele machines tussen uw on-premises sites met Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen voor Hyper-V virtuele machines tussen uw on-premises sites met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 65eda71c-3ca3-41bc-b02d-00fecc1557d7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: raynew
ms.openlocfilehash: 1647e9d69da3e991bec4e00b3a1083a254fa9550
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-disaster-recovery-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Instellen van herstel na noodgevallen voor Hyper-V-machines met uw secundaire on-premises site

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie ziet u hoe u herstel na noodgevallen naar een secundaire site instelt voor lokale Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds worden beheerd. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Voorbereiden van de on-premises VMM-servers en Hyper-V-hosts
> * Een Recovery Services-kluis maken voor Site Recovery 
> * Instellen van de bron en doel van de replicatie-omgevingen. 
> * Stel netwerktoewijzing (Als Hyper-V wordt beheerd door System Center VMM)
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een virtuele machine

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Controleer de [scenario-architectuur en onderdelen](concepts-hyper-v-to-secondary-architecture.md).
- Controleer de [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md) voor alle onderdelen.
- Zorg ervoor dat VMM-servers en Hyper-V-hosts voldoen [ondersteuningsvereisten](site-recovery-support-matrix-to-sec-site.md).
- Controleer of de virtuele machines die u wilt repliceren voldoen [gerepliceerde machine ondersteuning](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- VMM-servers voorbereiden op netwerktoewijzing.

### <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing

[Netwerktoewijzing](site-recovery-network-mapping.md) toewijzingen tussen lokale VMM VM-netwerken in de bron en doel-clouds. Toewijzing doet het volgende:

- Maakt verbinding met virtuele machines juiste doel VM-netwerken na een failover. 
- Replica-VM wordt optimaal wordt geplaatst op doelservers Hyper-V-host. 
- Als u geen netwerktoewijzing configureert, replica VMs niet verbonden met een VM-netwerk na een failover.

VMM als volgt voorbereiden:

1. Zorg ervoor dat u hebt [logische netwerken VMM](https://docs.microsoft.com/system-center/vmm/network-logical) op de bron en doel-VMM-servers.
    - Het logische netwerk op de bronserver moet worden gekoppeld aan de broncloud waarin Hyper-V-hosts zich bevinden.
    - Het logische netwerk op de doelserver moet worden gekoppeld aan de doelcloud.
1. Zorg ervoor dat u hebt [VM-netwerken](https://docs.microsoft.com/system-center/vmm/network-virtual) op de bron en doel-VMM-servers. VM-netwerken moeten worden gekoppeld aan het logische netwerk in elke vestiging.
2. Verbinding maken met virtuele machines op de bron-Hyper-V-hosts met de bron-VM-netwerk. 


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Kies een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u naar wilt repliceren.

1. Klik op **siteherstel** > **stap 1: infrastructuur voorbereiden** > **beveiligingsdoel**.
2. Selecteer **met site recovery**, en selecteer **Ja, met Hyper-V**.
3. Selecteer **Ja** om aan te geven u VMM voor het beheren van Hyper-V-hosts.
4. Selecteer **Ja** hebt u een secundaire VMM-server. Als u replicatie tussen clouds op één VMM-server implementeert, klikt u op **Nee**. Klik vervolgens op **OK**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

De Azure Site Recovery Provider installeren op VMM-servers en detecteren en servers in de kluis registreren.

1. Klik op **Infrastructuur voorbereiden** > **Bron**.
2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen.
3. In **Server toevoegen**, controleert u of **System Center VMM-server** wordt weergegeven in **servertype**.
4. Download het installatieprogramma voor de Azure Site Recovery-provider.
5. Download de registratiesleutel. U moet dit als u de Provider installeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/tutorial-vmm-to-vmm/source-settings.png)

6. De Provider installeren op elke VMM-server. U hoeft niet expliciet niets te installeren op Hyper-V-hosts.

### <a name="install-the-azure-site-recovery-provider"></a>De Azure Site Recovery Provider installeren

1. Voer de Provider setup-bestand op elke VMM-server. Als VMM is geïmplementeerd in een cluster, voor het eerst als volgt installeren:
    -  De Provider installeren op een actief knooppunt en voltooi de installatie om de VMM-server in de kluis registreren.
    - Vervolgens installeert u de Provider op de andere knooppunten. Clusterknooppunten moeten allemaal dezelfde versie van de Provider uitvoeren.
2. Setup enkele vereiste controles uitgevoerd en vraagt toestemming om de VMM-service te stoppen. De VMM-service wordt opnieuw opgestart nadat setup is voltooid. Als u op een VMM-cluster installeert, wordt u gevraagd om te stoppen van de Cluster-rol.
3. In **Microsoft Update**, u kunt ervoor kiezen om op te geven dat de provider-updates zijn geïnstalleerd in overeenstemming met uw Microsoft Update-beleid.
4. In **installatie**, accepteer of wijzig de standaardlocatie voor installatie, en klikt u op **installeren**.
5. Nadat de installatie is voltooid, klikt u op **registreren** de server in de kluis registreren.

    ![Installatielocatie](./media/tutorial-vmm-to-vmm/provider-register.png)
6. Controleer in **Kluisnaam** de naam van de kluis waarin de server wordt geregistreerd. Klik op **Volgende**.
7. In **proxyverbinding**, opgeven hoe de Provider die wordt uitgevoerd op de VMM-server verbinding maakt met Azure.
   - U kunt opgeven dat de provider verbinding met het internet rechtstreeks of via een proxyserver maken moet. Proxy-instellingen opgeven, indien nodig.
   - Als u een proxy gebruikt, wordt een VMM RunAs-account (DRAProxyAccount) automatisch gemaakt, met de opgegeven proxyreferenties. Configureer de proxyserver zodanig dat dit account kan worden geverifieerd. De RunAs-Accountinstellingen kunnen worden gewijzigd in de VMM-console > **instellingen** > **beveiliging** > **Run As-Accounts**.
   - Start de VMM-service voor het bijwerken van wijzigingen.
8. In **registratiesleutel**, selecteer de sleutel die u hebt gedownload en gekopieerd naar de VMM-server.
9. De instelling voor wachtwoordversleuteling is niet relevant is in dit scenario. 
10. Geef in **Servernaam** een beschrijvende naam op om de VMM-server in de kluis te identificeren. Geef de naam van de VMM-cluster-rol in een cluster.
11. In **cloudmetagegevens synchroniseren**Selecteer of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet dat alle clouds synchroniseren wilt, laat u deze instelling uitgeschakeld. U kunt elke cloud afzonderlijk in de cloudeigenschappen in de VMM-console synchroniseren.
12. Klik op **Volgende** om het proces te voltooien. Na registratie haalt Site Recovery metagegevens van de VMM-server. De server wordt weergegeven in **Servers** > **VMM-Servers** in de kluis.
13. Nadat de server wordt weergegeven in de kluis in **bron** > **bron voorbereiden** selecteert u de VMM-server en selecteer de cloud waarin de Hyper-V-host bevindt. Klik vervolgens op **OK**.


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer de doel-VMM-server en de cloud:

1. Klik op **infrastructuur voorbereiden** > **doel**, en selecteer de doel-VMM-server.
2. VMM-clouds die zijn gesynchroniseerd met Site Recovery worden weergegeven. Selecteer de doelcloud.

   ![doel](./media/tutorial-vmm-to-vmm/target-vmm.png)


## <a name="set-up-a-replication-policy"></a>Instellen van een beleid voor wachtwoordreplicatie

Voordat u begint, zorg er dan voor dat alle hosts met het beleid hebben hetzelfde besturingssysteem. Als hosts verschillende versies van Windows Server worden uitgevoerd, moet u meerdere beleidsregels voor replicatie.

1. Als u nieuw replicatiebeleid wilt maken, klikt u op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. Het type bron en doel moet **Hyper-V**.
3. In **versie van Hyper-V-host**, selecteer welk besturingssysteem wordt uitgevoerd op de host.
4. In **verificatietype** en **verificatiepoort**, opgeven hoe verkeer tussen de primaire en herstelsites Hyper-V-hostservers is geverifieerd.
    - Selecteer **certificaat** tenzij u beschikken over een werkende Kerberos-omgeving. Azure Site Recovery configureert automatisch certificaten voor HTTPS-verificatie. U hoeft niet verder niets te doen handmatig.
    - Standaard wordt poort 8083 en 8084 (voor certificaten) in de Windows Firewall op de Hyper-V-hostservers worden geopend.
    - Als u selecteert **Kerberos**, een Kerberos-ticket wordt gebruikt voor wederzijdse verificatie van de host-servers. Kerberos is alleen relevant voor Hyper-V-hostservers met Windows Server 2012 R2 of hoger.
1. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).
2. In **herstel bewaarperiode**, geef \how lang (in uren) het moet worden bewaard voor elk herstelpunt. Gerepliceerde machines kunnen worden hersteld naar een willekeurig punt in een venster.
3. In **App-consistente momentopname frequentie**, opgeven hoe vaak (1-12 uur) herstelpunten met toepassingsconsistente momentopnamen worden gemaakt. Hyper-V gebruikt twee soorten momentopnamen:
    - **Standaardmomentopname**: biedt een incrementele momentopname van de volledige virtuele machine.
    - **App-consistente momentopname**: een momentopname van de punt in tijd van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat apps in een consistente status wanneer de momentopname wordt gemaakt. Toepassingsconsistente momentopnamen inschakelt, beïnvloedt de appprestaties op de bron-VM's. Stel een waarde die kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
4. In **gegevensoverdracht compressie**, opgeven of de verzonden replicatiegegevens moeten worden gecomprimeerd.
5. Selecteer **replica verwijderen VM**, om op te geven dat de replica virtuele machine moet worden verwijderd als u beveiliging voor de bron-VM uitschakelt. Als u deze instelling inschakelt wanneer u beveiliging voor de bron-VM wordt deze verwijderd uit de Site Recovery-console uitschakelt, Site Recovery-instellingen voor VMM worden verwijderd uit de VMM-console en de replica is verwijderd.
6. In **initiële replicatiemethode**, als u via het netwerk repliceert, geef aan of de initiële replicatie start of plannen. Mogelijk wilt u plannen buiten de drukste tijden voor het opslaan van de netwerkbandbreedte. Klik vervolgens op **OK**.

     ![Beleid voor replicatie](./media/tutorial-vmm-to-vmm/replication-policy.png)
     
7. Het nieuwe beleid is automatisch gekoppeld aan de VMM-cloud. In **replicatiebeleid**, klikt u op **OK**. 


## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik op **toepassing repliceren** > **bron**. 
2. In **bron**, selecteert u de VMM-server en de cloud waarin u wilt repliceren Hyper-V-hosts zich bevinden. Klik vervolgens op **OK**.
3. In **doel**, controleert u de secundaire VMM-server en de cloud.
4. In **virtuele machines**, selecteert u de virtuele machines die u wilt beveiligen in de lijst.


U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen

[Noodherstelanalyse uitvoeren](tutorial-dr-drill-secondary.md)
