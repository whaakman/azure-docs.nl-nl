---
title: Instellen van herstel na noodgevallen van on-premises Hyper-V-machines in VMM-clouds naar Azure met Azure Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen van on-premises Hyper-V-machines in System Center VMM-clouds naar Azure, met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 64559f653ba8a466de7bec10db34383b508e3e4b
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361290"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Instellen van herstel na noodgevallen van on-premises Hyper-V-machines in VMM-clouds naar Azure

In dit artikel wordt beschreven hoe u replicatie inschakelen voor on-premises Hyper-V-machines die worden beheerd door System Center Virtual Machine Manager (VMM), voor herstel na noodgevallen naar Azure met de [Azure Site Recovery](site-recovery-overview.md) service. Als u geen VMM, klikt u vervolgens [Volg deze zelfstudie](hyper-v-azure-tutorial.md).

Dit is de derde zelfstudie in een serie die laat zien u hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM's. In de vorige zelfstudie we [de on-premises Hyper-V-omgeving voorbereid](hyper-v-prepare-on-premises-tutorial.md) voor herstel na noodgevallen naar Azure. 

In deze zelfstudie leert u het volgende:


> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel selecteren.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatiedoelomgeving instellen.
> * Instellen van netwerktoewijzing, om een koppeling tussen VMM VM-netwerken en virtuele netwerken van Azure.
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een VM


> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees het artikel in de sectie How To van de Site Recovery inhoudsopgave voor gedetailleerde instructies.

## <a name="before-you-begin"></a>Voordat u begint

Dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md) dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:


## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. Selecteer de kluis in **Recovery Services-kluizen**. We de kluis voorbereid **ContosoVMVault** in de vorige zelfstudie.
2. Klik in **Aan de slag** op **Site Recovery**. Klik vervolgens op **Infrastructuur voorbereiden**.
3. In **beveiligingsdoel** > **waar bevinden de machines zich?**, selecteer **On-premises**.
4. In **waar wilt u de machines repliceren?**, selecteer **naar Azure**.
5. In **zijn de machines gevirtualiseerd?** Selecteer **Ja, met Hyper-V**.
6. In **gebruikt u System Center VMM**, selecteer **Ja**. Klik vervolgens op **OK**.

    ![Replicatiedoel](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)


## <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

1. In **implementatieplanning**, als u van plan bent een grote implementatie, de Deployment Planner voor Hyper-V downloaden via de koppeling op de pagina. [Meer informatie](hyper-v-deployment-planner-overview.md) over het plannen van Hyper-V-implementatie.
2. We hebben nodig niet de deployment planner voor de doeleinden van deze zelfstudie. In **hebt u de implementatieplanning uitgevoerd?**, selecteer **doe ik later**. Klik vervolgens op **OK**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Bij het instellen van de bronomgeving, moet u de Azure Site Recovery Provider installeren op de VMM-server en de server in de kluis registreren. U kunt de Azure Recovery Services-agent installeren op elke Hyper-V-host. 

1. Klik in **Infrastructuur voorbereiden** op **Bron**.
2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen. Controleer in **Server toevoegen**of **System Center VMM-server** wordt weergegeven bij **Servertype**.
3. Download het installatieprogramma voor de Microsoft Azure Site Recovery Provider.
4. Download de registratiesleutel voor de kluis. U hebt deze nodig wanneer u de Provider-installatie uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.
5. Download het installatieprogramma voor de Microsoft Azure Recovery Services-agent.

    ![Downloaden](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>De provider installeren op de VMM-server

1. Ga in de installatiewizard van Azure Site Recovery Provider naar **Microsoft Update**, en kies ervoor om Microsoft Update te gebruiken om te controleren op Provider-updates.
2. In **installatie**, accepteer de standaardlocatie voor installatie van de Provider en klikt u op **installeren**. 
3. Na de installatie, in de Wizard registratie van Microsoft Azure Site Recovery > **Kluisinstellingen**, klikt u op **Bladeren**, en in **sleutelbestand**, selecteer de kluissleutel bestand dat u gedownload.
4. Geef de Azure Site Recovery-abonnement en de kluisnaam (**ContosoVMVault**). Geef een beschrijvende naam voor de VMM-server bij het identificeren ervan in de kluis.
5. Selecteer in **Proxy-instellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
6. Accepteer de standaardlocatie voor het certificaat dat wordt gebruikt om gegevens te versleutelen. Versleutelde gegevens worden ontsleuteld wanneer u een failover uitvoert.
7. In **cloudmetagegevens synchroniseren**, selecteer **cloudmetagegevens synchroniseren naar Site Recovery-portal**. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Klik vervolgens op **registreren**.
8. Nadat de server in de kluis is geregistreerd, klikt u op **voltooien**.

Nadat de registratie is voltooid, metagegevens van de server worden opgehaald door Azure Site Recovery en de VMM-server wordt weergegeven in **Site Recovery-infrastructuur**.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Installeer de Recovery Services-agent op Hyper-V-hosts

Installeer de agent op elke Hyper-V-host met VM's die u wilt repliceren.

1. In de Wizard Setup van Microsoft Azure Recovery Services Agent > **controle van vereisten**, klikt u op **volgende**. Ontbrekende vereiste onderdelen worden automatisch geïnstalleerd.
2. In **installatie-instellingen**, accepteer de installatielocatie en de cachelocatie. De cachestation moet ten minste 5 GB aan opslag. U wordt aangeraden een station met 600 GB of meer van de vrije ruimte. Klik vervolgens op **Installeren**.
3. In **installatie**, wanneer de installatie is voltooid, klikt u op **sluiten** om de wizard te voltooien.

    ![Agent installeren](./media/hyper-v-vmm-azure-tutorial/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

1. Klik op **Infrastructuur voorbereiden** > **Doel**.
2. Selecteer het abonnement en de resourcegroep (**ContosoRG**) waarin de Azure VM's zullen worden gemaakt na een failover.
3. Selecteer de **Resource Manager** implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

1. Klik in **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **Netwerktoewijzing** op het pictogram **+Netwerktoewijzing**.
2. In **netwerktoewijzing toevoegen**, selecteert u de bron-VMM-server. Selecteer **Azure** als het doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. In **Bronnetwerk**, selecteert u de bron on-premises VM-netwerk.
5. In **doelnetwerk**, selecteer het Azure-netwerk waarin Azure-VM van replica's te staan komen wanneer ze na een failover worden gemaakt. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

1. Klik op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. We maken gebruik van **ContosoReplicationPolicy**.
3. Behoud de standaardinstellingen en klik op **OK**.
    - **Kopieerfrequentie** geeft aan dat deltagegevens (na de initiële replicatie) om de vijf minuten worden gerepliceerd.
    - **Bewaarperiode voor herstelpunten** geeft aan dat voor elk herstelpunt gedurende twee uur behouden blijft.
    - **Frequentie van de app-consistente momentopname** geeft aan dat er elk uur herstelpunten met app-consistente momentopnamen worden gemaakt.
    - **Starttijd van de initiële replicatie** geeft aan dat de initiële replicatie direct wordt gestart.
    - **Versleutelen van gegevens die zijn opgeslagen in Azure** -standaard **uit** instelling geeft aan dat at-rest-gegevens in Azure wordt niet versleuteld.
4. Nadat het beleid is gemaakt, klikt u op **OK**. Wanneer u nieuw beleid maakt, wordt dit automatisch gekoppeld aan de VMM-cloud.

## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik in **Toepassing repliceren** op **Bron**. 
2. In **bron**, selecteert u de VMM-cloud. Klik vervolgens op **OK**.
3. In **doel**, controleren of Azure als doel, het juiste kluisabonnement wordt en selecteert u de **Resource Manager** model.
4. Selecteer de **contosovmsacct1910171607** storage-account en de **ContosoASRnet** Azure-netwerk.
5. Selecteer in **Virtuele machines** > **Selecteren** de VM's die u wilt repliceren. Klik vervolgens op **OK**.

   U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Wanneer de taak **De beveiliging voltooien** is voltooid, is de initiële replicatie voltooid en is de virtuele machine klaar voor failover.



## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
