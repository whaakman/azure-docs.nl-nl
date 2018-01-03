---
title: Herstel na noodgevallen voor lokale Hyper-V-machines (zonder VMM) naar Azure met Azure Site Recovery instellen | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen van lokale Hyper-V-machines (zonder VMM) naar Azure met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Herstel na noodgevallen van lokale Hyper-V-machines naar Azure instellen

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u voor het instellen van herstel na noodgevallen van lokale Hyper-V-machines naar Azure. De zelfstudie is relevant voor Hyper-V-machines die niet worden beheerd door System Center Virtual Machine Manager (VMM). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Replicatiebron en doel van uw selecteren.
> * De bron-replicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatie doelomgeving instellen.
> * Maak een replicatiebeleid.
> * Replicatie inschakelen voor een virtuele machine.

Dit is de derde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)

Voordat u begint, is het handig om [bekijken van de architectuur](concepts-hyper-v-to-azure-architecture.md) voor deze noodherstelscenario.

## <a name="select-a-replication-goal"></a>Selecteer een doel replicatie


1. In **alle Services** > **Recovery Services-kluizen**, klikt u op de naam van de kluis die we in de vorige zelfstudie voorbereid **ContosoVMVault**.
2. In **aan de slag**, klikt u op **siteherstel**. Klik vervolgens op **infrastructuur voorbereiden**
3. In **beveiligingsdoel** > **waar worden de machines die zich**, selecteer **On-premises**.
4. In **waar wilt u uw machines repliceren**, selecteer **naar Azure**.
5. In **zijn uw gevirtualiseerde machines**, selecteer **Ja, met Hyper-V**.
6. In **gebruikt u de System Center VMM**, selecteer **Nee**. Klik vervolgens op **OK**.

    ![Replicatie-doel](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

U toevoegen als u de bronomgeving instelt, Hyper-V-hosts met een Hyper-V-site, downloaden en installeren van de Azure Site Recovery Provider en de Azure Recovery Services-agent en de Hyper-V-site in de kluis registreren. 

1. In **infrastructuur voorbereiden**, klikt u op **bron**.
2. Klik op **+ Hyper-V-Site**, en geef de naam van de site wordt gemaakt in de vorige zelfstudie **ContosoHyperVSite**.
3. Klik op **+ Hyper-V-Server**.
4. Download het installatiebestand van de Provider.
5. Download de kluisregistratiesleutel. U moet dit als u Provider setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Download de Provider](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>De Provider installeren

Het Provider-installatiebestand (AzureSiteRecoveryProvider.exe) uitvoeren op elke Hyper-V-host u toegevoegd aan de **ContosoHyperVSite** site. Setup installeert de Azure Site Recovery Provider en de Recovery Services-agent op elke Hyper-V-host.

1. In de Azure Site Recovery Provider Setup-wizard > **Microsoft Update**, meldt zich aan voor Microsoft Update gebruiken om te controleren op updates van de Provider.
2. In **installatie**, accepteer de standaardlocatie voor installatie voor de Provider en de agent en klikt u op **installeren**.
3. Na de installatie in de Wizard registratie van Microsoft Azure Site Recovery > **Kluisinstellingen**, klikt u op **Bladeren**, en in **sleutelbestand**, selecteer de sleutel van de kluis bestand dat u gedownload. 
4. Geef het Azure Site Recovery-abonnement, de kluisnaam (**ContosoVMVault**), en de Hyper-V-site (**ContosoHyperVSite**) waarmee de Hyper-V server behoort.
5. In **Proxy-instellingen**, selecteer **rechtstreeks verbinding maken met Azure Site Recovery zonder een proxy**.
6. In **registratie**nadat de server is geregistreerd in de kluis, klikt u op **voltooien**.

Metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Site Recovery-infrastructuur** > **Hyper-V-Hosts**. Dit kan tot 30 minuten duren.


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen. 

1. Klik op **infrastructuur voorbereiden** > **doel**.
2. Selecteer het abonnement en de resourcegroep **ContosoRG**, in die de Azure VM's na een failover worden gemaakt.
3. Selecteer de **Resource Manager '** implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="set-up-a-replication-policy"></a>Instellen van een beleid voor wachtwoordreplicatie

1. Klik op **infrastructuur voorbereiden** > **replicatie-instellingen** > **+ maken en koppelen**.
2. In **beleid maken en koppelen**, een beleidsnaam opgeven **ContosoReplicationPolicy**.
3. Laat de standaardinstellingen en klik op **OK**.
    - **Kopieerfrequentie** geeft aan dat delta gegevens (na de initiële replicatie) om de vijf minuten wordt gerepliceerd.
    - **Herstel bewaarperiode** geeft aan dat de windows bewaren voor elk herstelpunt twee twee uur.
    - **App-consistente momentopname frequentie** geeft aan dat met toepassingsconsistente momentopnamen herstelpunten elk uur wordt gemaakt.
    - **Begintijd van de initiële replicatie**, geeft aan dat de initiële replicatie wordt onmiddellijk starten.
4. Nadat het beleid is gemaakt, klikt u op **OK**. Wanneer u een nieuw beleid voor het automatisch gekoppeld aan de opgegeven Hyper-V-site maakt (**ContosoHyperVSite**)

    ![Beleid voor replicatie](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Replicatie inschakelen


1. In **toepassing repliceren**, klikt u op **bron**. 
2. In **bron**, selecteer de **ContosoHyperVSite** site. Klik vervolgens op **OK**.
3. In **doel**, Azure als doel, het abonnement kluis controleren en de **Resource Manager** implementatiemodel.
4. Selecteer de **contosovmsacct1910171607** storage-account in de vorige zelfstudie voor gerepliceerde gegevens is gemaakt en de **ContosoASRnet** netwerk, in welke Azure VM's worden geplaatst na een failover.
5. In **virtuele machines** > **Selecteer**, selecteert u de virtuele machine die u wilt repliceren. Klik vervolgens op **OK**.

 U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen
[Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
