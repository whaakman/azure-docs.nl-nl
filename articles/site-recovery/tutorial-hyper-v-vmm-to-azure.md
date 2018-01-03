---
title: Instellen van herstel na noodgevallen van lokale Hyper-V-machines in VMM-clouds naar Azure met Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen van lokale Hyper-V-machines in System Center VMM-clouds naar Azure, met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5f364c6f8a88ad53c12909f0e9f10afcce5ef8af
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Herstel na noodgevallen van lokale Hyper-V-machines in VMM-clouds naar Azure instellen

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u voor het instellen van herstel na noodgevallen van lokale Hyper-V-machines naar Azure. De zelfstudie is relevant voor Hyper-V-machines die worden beheerd door System Center Virtual Machine Manager (VMM). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Replicatiebron en doel van uw selecteren.
> * De bron-replicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatie doelomgeving instellen.
> * De netwerktoewijzing instellen om een koppeling tussen VMM VM-netwerken en virtuele netwerken in Azure.
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een virtuele machine

Dit is de derde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)

Voordat u begint, is het handig om [bekijken van de architectuur](concepts-hyper-v-to-azure-architecture.md) voor deze noodherstelscenario.



## <a name="select-a-replication-goal"></a>Selecteer een doel replicatie

1. In **alle Services** > **Recovery Services-kluizen**, klikt u op de kluisnaam we in deze zelfstudies gebruiken **ContosoVMVault**.
2. In **aan de slag**, klikt u op **siteherstel**. Klik vervolgens op **infrastructuur voorbereiden**
3. In **beveiligingsdoel** > **waar worden de machines die zich**, selecteer **On-premises**.
4. In **waar wilt u uw machines repliceren**, selecteer **naar Azure**.
5. In **zijn uw gevirtualiseerde machines**, selecteer **Ja, met Hyper-V**.
6. In **gebruikt u de System Center VMM**, selecteer **Ja**. Klik vervolgens op **OK**.

    ![Replicatie-doel](./media/tutorial-hyper-v-vmm-to-azure/replication-goal.png)



## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Bij het instellen van de bronomgeving, moet u de Azure Site Recovery Provider en de Azure Recovery Services-agent installeren en on-premises-servers in de kluis registreren. 

1. In **infrastructuur voorbereiden**, klikt u op **bron**.
2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen. In **Server toevoegen**, controleert u of **System Center VMM-server** wordt weergegeven in **servertype**.
3. Download het installatieprogramma voor de Microsoft Azure Site Recovery Provider.
4. Download de kluisregistratiesleutel. U moet dit als u Provider setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.
5. De Recovery Services-agent downloaden.

    ![Downloaden](./media/tutorial-hyper-v-vmm-to-azure/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>De provider installeren op de VMM-server

1. In de Azure Site Recovery Provider Setup-wizard > **Microsoft Update**, meldt zich aan voor Microsoft Update gebruiken om te controleren op updates van de Provider.
2. In **installatie**, accepteer de standaardlocatie voor installatie van de Provider en klikt u op **installeren**. 
3. Na de installatie in de Wizard registratie van Microsoft Azure Site Recovery > **Kluisinstellingen**, klikt u op **Bladeren**, en in **sleutelbestand**, selecteer de sleutel van de kluis bestand dat u gedownload.
4. Geef de Azure Site Recovery-abonnement en de kluisnaam (**ContosoVMVault**). Geef een beschrijvende naam voor de VMM-beheerserver bij het identificeren ervan in de kluis.
5. In **Proxy-instellingen**, selecteer **rechtstreeks verbinding maken met Azure Site Recovery zonder een proxy**.
6. Accepteer de standaardlocatie voor het certificaat dat wordt gebruikt om gegevens te versleutelen. Versleutelde gegevens ontsleuteld wanneer u een failover uitvoert.
7. In **cloudmetagegevens synchroniseren**, selecteer **cloudmetagegevens synchroniseren naar de Site Recovery-portal**. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Klik vervolgens op **registreren**.
8. Nadat de server is geregistreerd in de kluis, klikt u op **voltooien**.

Nadat de registratie is voltooid, metagegevens van de server worden opgehaald door Azure Site Recovery en de VMM-server wordt weergegeven in **Site Recovery-infrastructuur**.

### <a name="install-the-recovery-services-agent"></a>De Recovery Services-agent installeren

Installeer de agent op elke Hyper-V-host met VM's die u wilt repliceren.

1. In de Wizard Setup van Microsoft Azure Recovery Services Agent > **controle van vereisten**, klikt u op **volgende**. Ontbrekende vereiste onderdelen worden automatisch geïnstalleerd.
2. In **installatie-instellingen**, accepteer de installatielocatie en de cachelocatie. De cachestation moet ten minste 5 GB aan opslagruimte. U wordt aangeraden een station met 600 GB of meer aan vrije ruimte. Klik vervolgens op **Installeren**.
3. In **installatie**, wanneer de installatie is voltooid, klikt u op **sluiten** om de wizard te voltooien.

    ![Agent installeren](./media/tutorial-hyper-v-vmm-to-azure/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

1. Klik op **infrastructuur voorbereiden** > **doel**.
2. Selecteer het abonnement en de resourcegroep (**ContosoRG**) in die de Azure VM's na een failover worden gemaakt.
3. Selecteer de **Resource Manager '** implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

1. Klik in **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **Netwerktoewijzing** op het pictogram **+Netwerktoewijzing**.
2. In **netwerktoewijzing toevoegen**, selecteert u de bron-VMM-server. Selecteer **Azure** als doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. In **Bronnetwerk**, selecteert u de bron on-premises VM-netwerk.
5. In **doelnetwerk**, selecteer het Azure-netwerk in welke replica Azure VM's geplaatst worden wanneer ze zijn gemaakt na een failover. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/tutorial-hyper-v-vmm-to-azure/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Instellen van een beleid voor wachtwoordreplicatie

1. Klik op **infrastructuur voorbereiden** > **replicatie-instellingen** > **+ maken en koppelen**.
2. In **beleid maken en koppelen**, een beleidsnaam opgeven **ContosoReplicationPolicy**.
3. Laat de standaardinstellingen en klik op **OK**.
    - **Kopieerfrequentie** geeft aan dat delta gegevens (na de initiële replicatie) om de vijf minuten wordt gerepliceerd.
    - **Herstel bewaarperiode** geeft aan dat de windows bewaren voor elk herstelpunt twee twee uur.
    - **App-consistente momentopname frequentie** geeft aan dat met toepassingsconsistente momentopnamen herstelpunten elk uur wordt gemaakt.
    - **Begintijd van de initiële replicatie**, geeft aan dat de initiële replicatie wordt onmiddellijk starten.
    - **Versleutelen van gegevens die zijn opgeslagen in Azure** -standaard **uit** instelling geeft aan dat in rust gegevens in Azure is niet versleuteld.
4. Nadat het beleid is gemaakt, klikt u op **OK**. Wanneer u nieuw beleid maakt, wordt dit automatisch gekoppeld aan de VMM-cloud.

## <a name="enable-replication"></a>Replicatie inschakelen

1. In **toepassing repliceren**, klikt u op **bron**. 
2. In **bron**, selecteert u de VMM-cloud. Klik vervolgens op **OK**.
3. In **doel**, controleert u Azure als doel, het abonnement kluis en selecteer de **Resource Manager** model.
4. Selecteer de **contosovmsacct1910171607** storage-account en de **ContosoASRnet** Azure-netwerk.
5. In **virtuele machines** > **Selecteer**, selecteert u de virtuele machine die u wilt repliceren. Klik vervolgens op **OK**.

 U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.


## <a name="next-steps"></a>Volgende stappen
[Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
