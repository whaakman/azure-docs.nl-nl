---
title: Instellen van herstel na noodgevallen voor Hyper-V-machines tussen on-premises sites met Azure Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen voor Hyper-V-machines tussen uw on-premises sites met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 4cc4da130d9253bf40e5d02806088a95b2195e7c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38531718"
---
# <a name="set-up-disaster-recovery-for-hyper-v-vms-to-a-secondary-on-premises-site"></a>Instellen van herstel na noodgevallen voor Hyper-V-machines naar een secundaire on-premises site

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgevallen door de replicatie, failover en failback van on-premises machines en virtuele Azure-machines te beheren en in te delen.

Dit artikel ziet u hoe u herstel na noodgevallen naar een secundaire site instelt voor on-premises Hyper-V-VM's die worden beheerd in System Center Virtual Machine Manager (VMM)-clouds. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * On-premises VMM-servers en Hyper-V-hosts voorbereiden
> * Maakt een Recovery Services-kluis voor Site Recovery 
> * Instellen van de bron en doel van de replicatie-omgevingen. 
> * Netwerktoewijzing instellen 
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een VM

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van dit scenario:

- Controleer de [scenario-architectuur en onderdelen](hyper-v-vmm-architecture.md).
- Zorg ervoor dat de VMM-servers en Hyper-V-hosts te voldoen aan [ondersteuningsvereisten](hyper-v-vmm-secondary-support-matrix.md).
- Controleer of de virtuele machines die u wilt repliceren voldoen aan [gerepliceerde machine ondersteuning](hyper-v-vmm-secondary-support-matrix.md#replicated-vm-support).
- VMM-servers voorbereiden op netwerktoewijzing.

### <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing

[Netwerktoewijzing](hyper-v-vmm-network-mapping.md) toewijzingen tussen on-premises VMM VM-netwerken in de bron- en clouds. Toewijzing doet het volgende:

- Verbonden virtuele machines met juiste doel-VM-netwerken na een failover. 
- Optimaal replica-VM's op Hyper-V-host doelservers geplaatst. 
- Als u geen netwerktoewijzing configureert, replica-VM's niet verbonden met een VM-netwerk na een failover.

VMM als volgt voorbereiden:

1. Zorg ervoor dat u hebt [VMM logische netwerken](https://docs.microsoft.com/system-center/vmm/network-logical) op de bron- en VMM-servers.
    - Het logische netwerk op de bronserver moet worden gekoppeld aan de broncloud waarin de Hyper-V-hosts zich bevinden.
    - Het logische netwerk op de doelserver moet worden gekoppeld aan de doel-cloud.
1. Zorg ervoor dat u hebt [VM-netwerken](https://docs.microsoft.com/system-center/vmm/network-virtual) op de bron- en VMM-servers. VM-netwerken moeten zijn gekoppeld aan het logische netwerk op elke locatie.
2. Verbinding maken met virtuele machines op de bron-Hyper-V-hosts met de bron-VM-netwerk. 


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Kies een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u naar wilt repliceren.

1. Klik op **siteherstel** > **stap 1: infrastructuur voorbereiden** > **beveiligingsdoel**.
2. Selecteer **naar herstelsite**, en selecteer **Ja, met Hyper-V**.
3. Selecteer **Ja** om aan te geven u VMM gebruikt voor het beheren van de Hyper-V-hosts.
4. Selecteer **Ja** hebt u een secundaire VMM-server. Als u replicatie tussen clouds op een enkele VMM-server implementeert, klikt u op **Nee**. Klik vervolgens op **OK**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

De Azure Site Recovery Provider installeren op de VMM-servers, detecteren en servers in de kluis registreren.

1. Klik op **Infrastructuur voorbereiden** > **Bron**.
2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen.
3. In **-Server toevoegen**, controleert u of **System Center VMM-server** wordt weergegeven in **servertype**.
4. Download het installatieprogramma voor de Azure Site Recovery-provider.
5. Download de registratiesleutel. U hebt deze nodig wanneer u de Provider installeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/hyper-v-vmm-disaster-recovery/source-settings.png)

6. Installeer de Provider op elke VMM-server. U hoeft niet te expliciet te installeren op de Hyper-V-hosts.

### <a name="install-the-azure-site-recovery-provider"></a>De Azure Site Recovery Provider installeren

1. Voer het Providerinstallatiebestand uit setup-bestand op elke VMM-server. Als VMM is geïmplementeerd in een cluster, voor het eerst als volgt installeren:
    -  De Provider installeren op een actief knooppunt en voltooi de installatie voor het registreren van de VMM-server in de kluis.
    - Vervolgens installeert u de Provider op de andere knooppunten. Clusterknooppunten moeten alle dezelfde versie van de Provider uitvoeren.
2. Setup wordt uitgevoerd een aantal controles en vraagt toestemming om de VMM-service te stoppen. De VMM-service wordt opnieuw opgestart nadat setup is voltooid. Als u op een VMM-cluster installeert, wordt u gevraagd de clusterrol te stoppen.
3. In **Microsoft Update**, u kunt ervoor kiezen om op te geven dat de provider-updates worden geïnstalleerd in overeenstemming met uw Microsoft Update-beleid.
4. In **installatie**, accepteren of wijzigen van de standaardlocatie voor installatie en klikt u op **installeren**.
5. Nadat de installatie is voltooid, klikt u op **registreren** de server registreren in de kluis.

    ![Installatielocatie](./media/hyper-v-vmm-disaster-recovery/provider-register.png)
6. Controleer in **Kluisnaam** de naam van de kluis waarin de server wordt geregistreerd. Klik op **Volgende**.
7. In **proxyverbinding**, opgeven hoe de Provider die wordt uitgevoerd op de VMM-server verbinding maakt met Azure.
   - U kunt opgeven dat de provider moet verbinding rechtstreeks met internet of via een proxyserver. Proxy-instellingen opgeven indien nodig.
   - Als u een proxy gebruikt, wordt een VMM RunAs-account (DRAProxyAccount) automatisch gemaakt, met de opgegeven proxyreferenties. Configureer de proxyserver zodanig dat dit account kan worden geverifieerd. De RunAs-Accountinstellingen kunnen worden gewijzigd in de VMM-console > **instellingen** > **Security** > **Run As-Accounts**.
   - Start de VMM-service voor het bijwerken van wijzigingen.
8. In **registratiesleutel**, selecteer de sleutel die u hebt gedownload en gekopieerd naar de VMM-server.
9. De versleutelingsinstelling is niet relevant in dit scenario. 
10. Geef in **Servernaam** een beschrijvende naam op om de VMM-server in de kluis te identificeren. Geef in een cluster, de VMM-clusterrolnaam op.
11. In **cloudmetagegevens synchroniseren**, selecteren of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet dat alle clouds synchroniseren wilt, laat u deze instelling uitgeschakeld. U kunt elke cloud afzonderlijk synchroniseren in de cloudeigenschappen in de VMM-console.
12. Klik op **Volgende** om het proces te voltooien. Na de registratie haalt Site Recovery metagegevens van de VMM-server. De server wordt weergegeven in **Servers** > **VMM-Servers** in de kluis.
13. Nadat de server wordt weergegeven in de kluis **bron** > **bron voorbereiden** selecteert u de VMM-server en selecteer de cloud waarin de Hyper-V-host zich bevindt. Klik vervolgens op **OK**.


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer de doel-VMM-server en de cloud:

1. Klik op **infrastructuur voorbereiden** > **doel**, en selecteer de doel-VMM-server.
2. VMM-clouds die zijn gesynchroniseerd met Site Recovery worden weergegeven. Selecteer de doelcloud.

   ![Doel](./media/hyper-v-vmm-disaster-recovery/target-vmm.png)


## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

Voordat u begint, zorg er dan voor dat alle hosts met behulp van het beleid hebben hetzelfde besturingssysteem. Als hosts verschillende versies van Windows Server worden uitgevoerd, moet u meerdere beleidsregels.

1. Als u nieuw replicatiebeleid wilt maken, klikt u op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. Het type bron en het doel moet **Hyper-V**.
3. In **Hyper-V-hostversie**, selecteer welk besturingssysteem wordt uitgevoerd op de host.
4. In **verificatietype** en **verificatiepoort**, opgeven hoe verkeer tussen de primaire en Hyper-V-hostservers is geverifieerd.
    - Selecteer **certificaat** , tenzij u een werkende Kerberos-omgeving hebt. Azure Site Recovery configureert automatisch certificaten voor HTTPS-verificatie. U hoeft te ondernemen handmatig.
    - Standaard wordt poort 8083 en 8084 (voor certificaten) in de Windows Firewall op de Hyper-V-hostservers worden geopend.
    - Als u selecteert **Kerberos**, een Kerberos-ticket wordt gebruikt voor wederzijdse verificatie van de host-servers. Kerberos is alleen relevant voor Hyper-V-hostservers wordt uitgevoerd op Windows Server 2012 R2 of hoger.
1. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).
2. In **bewaarperiode voor herstelpunten**, geef \how lang (in uren) is de bewaarperiode voor elk herstelpunt. Gerepliceerde machines kunnen worden hersteld naar een willekeurig punt in een venster.
3. In **frequentie App-consistente momentopname**, opgeven hoe vaak (1-12 uur) herstelpunten met toepassingsconsistente momentopnamen worden gemaakt. Hyper-V maakt gebruik van twee soorten momentopnamen:
    - **Standard momentopname**: steeds incrementele momentopnamen van de volledige virtuele machine.
    - **App-consistente momentopname**: een point-in-time-momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat de apps een consistente status hebben zijn wanneer de momentopname wordt gemaakt. App-consistente momentopnamen is ingeschakeld, is van invloed op prestaties van de app op de bron-VM's. Stel een waarde die kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
4. In **gegevenscompressie tijdens overdracht**, opgeven of overgedragen replicatiegegevens moeten worden gecomprimeerd.
5. Selecteer **verwijderen replica-VM**om op te geven dat de replica virtuele machine moeten worden verwijderd als u de beveiliging voor de bron-VM uitschakelen. Als u deze instelling inschakelt als u de beveiliging voor de bron-VM wordt deze verwijderd uit de Site Recovery-console uitschakelen, Site Recovery-instellingen voor VMM worden verwijderd uit de VMM-console en de replica is verwijderd.
6. In **voor de eerste replicatiemethode**, als u via het netwerk repliceert, Geef op of de initiële replicatie start of plannen. Om de netwerkbandbreedte hebt opgeslagen, kunt u plannen dat het buiten de drukste tijden. Klik vervolgens op **OK**.

     ![Beleid voor replicatie](./media/hyper-v-vmm-disaster-recovery/replication-policy.png)
     
7. Het nieuwe beleid wordt automatisch gekoppeld aan de VMM-cloud. In **replicatiebeleid**, klikt u op **OK**. 


## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik op **Toepassing repliceren** > **Bron**. 
2. In **bron**, selecteert u de VMM-server en de cloud waarin de Hyper-V-hosts die u wilt repliceren zich bevinden. Klik vervolgens op **OK**.
3. In **doel**, controleert u of de secundaire VMM-server en de cloud.
4. In **virtuele machines**, selecteert u de virtuele machines die u wilt beveiligen in de lijst.


U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen

[Noodherstelanalyse uitvoeren](hyper-v-vmm-test-failover.md)
