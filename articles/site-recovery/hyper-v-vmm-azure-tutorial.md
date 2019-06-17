---
title: Herstel na noodgevallen instellen voor on-premises Hyper-V-machines in VMM-clouds naar Azure met Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen van on-premises Hyper-V-machines in System Center VMM-clouds naar Azure met behulp van Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 653db1497fcce5981bba7416f073b0330ca2861f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66398157"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Instellen van herstel na noodgevallen van on-premises Hyper-V-machines in VMM-clouds naar Azure

In dit artikel wordt beschreven hoe u replicatie inschakelen voor on-premises Hyper-V-machines die worden beheerd door System Center Virtual Machine Manager (VMM), voor herstel na noodgevallen naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service. Als u geen VMM, [Volg deze zelfstudie](hyper-v-azure-tutorial.md) in plaats daarvan.

Dit is de derde zelfstudie in een serie die laat zien u hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM's. In de vorige zelfstudie we [de on-premises Hyper-V-omgeving voorbereid](hyper-v-prepare-on-premises-tutorial.md) voor herstel na noodgevallen naar Azure.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel selecteren.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen en de replicatiedoelomgeving instellen.
> * Stel netwerktoewijzing in om een koppeling tussen VMM VM-netwerken en virtuele netwerken van Azure.
> * Een replicatiebeleid maken.
> * Replicatie voor een VM inschakelen.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Voor gedetailleerde instructies, Raadpleeg de artikelen in de **handleidingen** sectie van de [documentatie over Site Recovery](https://docs.microsoft.com/azure/site-recovery).

## <a name="before-you-begin"></a>Voordat u begint

Dit is de derde zelfstudie in een reeks. Hierbij wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)

## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. In de Azure-portal, gaat u naar **Recovery Services-kluizen** en selecteer de kluis. We de kluis voorbereid **ContosoVMVault** in de vorige zelfstudie.
2. In **aan de slag**, selecteer **siteherstel**, en selecteer vervolgens **infrastructuur voorbereiden**.
3. In **beveiligingsdoel** > **waar bevinden de machines zich?** , selecteer **On-premises**.
4. In **waar wilt u de machines repliceren?** , selecteer **naar Azure**.
5. In **zijn de machines gevirtualiseerd?** , selecteer **Ja, met Hyper-V**.
6. In **gebruikt u System Center VMM voor het beheren van uw Hyper-V-hosts?** , selecteer **Ja**.
7.  Selecteer **OK**.

    ![Replicatiedoel](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

1. In **implementatieplanning**, als u van plan bent een grote implementatie, de Deployment Planner voor Hyper-V downloaden via de koppeling op de pagina. [Meer informatie](hyper-v-deployment-planner-overview.md) over het plannen van Hyper-V-implementatie.
2. We hebben nodig niet de Deployment Planner voor deze zelfstudie. In **hebt u de implementatieplanning uitgevoerd?** , selecteer **doe ik later**, en selecteer vervolgens **OK**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Bij het instellen van de bronomgeving, kunt u de Azure Site Recovery Provider installeren op de VMM-server en de server in de kluis registreren. U kunt de Azure Recovery Services-agent installeren op elke Hyper-V-host.

1. In **infrastructuur voorbereiden**, selecteer **bron**.
2. In **bron voorbereiden**, selecteer **+ VMM** om toe te voegen een VMM-server. Controleer in **Server toevoegen**of **System Center VMM-server** wordt weergegeven bij **Servertype**.
3. Download het installatieprogramma voor de Microsoft Azure Site Recovery Provider.
4. Download de kluisregistratiesleutel. U moet deze sleutel wanneer u de Provider setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.
5. Download het installatieprogramma voor de Microsoft Azure Recovery Services-agent.

    ![Provider, registratie en -agent downloaden](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>De provider installeren op de VMM-server

1. Ga in de installatiewizard van Azure Site Recovery Provider naar **Microsoft Update**, en kies ervoor om Microsoft Update te gebruiken om te controleren op Provider-updates.
2. In **installatie**, accepteer de standaardlocatie voor installatie van de Provider en selecteert u **installeren**.
3. Na de installatie, in de Wizard registratie van Microsoft Azure Site Recovery > **Kluisinstellingen**, selecteer **Bladeren**, en in **sleutelbestand**, selecteer de kluissleutel die het bestand u hebt gedownload.
4. Geef de Azure Site Recovery-abonnement en de kluisnaam (**ContosoVMVault**). Geef een beschrijvende naam voor de VMM-server bij het identificeren ervan in de kluis.
5. Selecteer in **Proxy-instellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
6. Accepteer de standaardlocatie voor het certificaat dat wordt gebruikt om gegevens te versleutelen. Versleutelde gegevens worden ontsleuteld wanneer u een failover uitvoert.
7. In **cloudmetagegevens synchroniseren**, selecteer **cloudmetagegevens synchroniseren naar Site Recovery-portal**. Deze actie moet worden slechts één keer op elke server. Selecteer **registreren**.
8. Nadat de server is geregistreerd in de kluis, selecteert u **voltooien**.

Nadat de registratie is voltooid, metagegevens van de server worden opgehaald door Azure Site Recovery en de VMM-server wordt weergegeven in **Site Recovery-infrastructuur**.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Installeer de Recovery Services-agent op Hyper-V-hosts

Installeer de agent op elke Hyper-V-host met VM's die u wilt repliceren.

1. In de Wizard Setup van Microsoft Azure Recovery Services Agent > **controle van vereisten**, selecteer **volgende**. Ontbrekende vereiste onderdelen worden automatisch geïnstalleerd.
2. In **installatie-instellingen**, accepteert de installatielocatie en de cachelocatie. De cachestation moet ten minste 5 GB aan opslag. U wordt aangeraden een station met 600 GB of meer van de vrije ruimte. Selecteer vervolgens **Install**.
3. In **installatie**, wanneer de installatie is voltooid, selecteert u **sluiten** om de wizard te voltooien.

    ![Agent installeren](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

1. Selecteer **Infrastructuur voorbereiden** > **Doel**.
2. Selecteer het abonnement en de resourcegroep (**ContosoRG**) waarin de Azure VM's zullen worden gemaakt na een failover.
3. Selecteer de **Resource Manager** implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

1. In **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **netwerktoewijzing**, selecteer de **+ netwerktoewijzing** pictogram.
2. In **netwerktoewijzing toevoegen**, selecteert u de bron-VMM-server. Selecteer **Azure** als het doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. In **Bronnetwerk**, selecteert u de bron on-premises VM-netwerk.
5. In **doelnetwerk**, selecteer het Azure-netwerk waarin Azure-VM van replica's te staan komen wanneer ze na een failover worden gemaakt. Selecteer vervolgens **OK**.

    ![Netwerktoewijzing](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

1. Selecteer **infrastructuur voorbereiden** > **replicatie-instellingen** >  **+ maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. We maken gebruik van **ContosoReplicationPolicy**.
3. Behoud de standaardinstellingen en selecteer **OK**.
    - **Kopieerfrequentie** geeft aan dat er, na de initiële replicatie delta-gegevens worden gerepliceerd om de vijf minuten.
    - **Bewaarperiode voor herstelpunten** geeft aan dat elk herstelpunt twee uur zal worden bewaard.
    - **Frequentie van de app-consistente momentopname** geeft aan dat er elk uur herstelpunten met app-consistente momentopnamen worden gemaakt.
    - **Begintijd initiële replicatie** geeft aan dat de initiële replicatie direct wordt gestart.
    - **Versleutelen van gegevens die zijn opgeslagen in Azure** is ingesteld op de standaardwaarde (**uit**) en geeft aan dat de gegevens in rust in Azure wordt niet versleuteld.
4. Nadat het beleid is gemaakt, selecteert u **OK**. Wanneer u een nieuw beleid maakt, is het automatisch gekoppeld aan de VMM-cloud.

## <a name="enable-replication"></a>Replicatie inschakelen

1. In **toepassing repliceren**, selecteer **bron**.
2. In **bron**, selecteert u de VMM-cloud. Selecteer vervolgens **OK**.
3. In **doel**, Controleer of het doel (Azure), het juiste kluisabonnement wordt en selecteert u de **Resource Manager** model.
4. Selecteer de **contosovmsacct1910171607** storage-account en de **ContosoASRnet** Azure-netwerk.
5. In **virtuele machines** > **Selecteer**, selecteer de virtuele machine die u wilt repliceren. Selecteer vervolgens **OK**.

   U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
