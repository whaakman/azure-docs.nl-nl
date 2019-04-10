---
title: Herstel na noodgevallen instellen voor on-premises Hyper-V-VM's (zonder VMM) naar Azure met Azure Site Recovery | Microsoft Docs
description: Leer hoe u herstel na noodgevallen kunt instellen voor on-premises Hyper-V-VM's (zonder VMM) naar Azure met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: da643a4d7a1dc74385b3854c1952af5ba93bd241
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358096"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Herstel na noodgevallen instellen voor on-premises Hyper-V-VM's naar Azure

c

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel selecteren.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatiedoelomgeving instellen.
> * Een replicatiebeleid maken.
> * Replicatie voor een VM inschakelen.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees het artikel in de sectie How To van de Site Recovery inhoudsopgave voor gedetailleerde instructies.

## <a name="before-you-begin"></a>Voordat u begint
Dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)



## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. Selecteer de kluis in **Recovery Services-kluizen**. We de kluis voorbereid **ContosoVMVault** in de vorige zelfstudie.
2. Klik in **Aan de slag** op **Site Recovery**. Klik vervolgens op **Infrastructuur voorbereiden**.
3. In **beveiligingsdoel** > **waar bevinden de machines zich?** Selecteer **On-premises**.
4. In **waar wilt u de machines repliceren?**, selecteer **naar Azure**.
5. In **zijn de machines gevirtualiseerd?** Selecteer **Ja, met Hyper-V**.
6. Bij **Gebruikt u System Center VMM om uw Hyper-V-hosts te beheren** selecteert u **Nee**. Klik vervolgens op **OK**.

    ![Replicatiedoel](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

1. In **implementatieplanning**, als u van plan bent een grote implementatie, de Deployment Planner voor Hyper-V downloaden via de koppeling op de pagina. [Meer informatie](hyper-v-deployment-planner-overview.md) over het plannen van Hyper-V-implementatie.
2. We hebben nodig niet de deployment planner voor de doeleinden van deze zelfstudie. In **hebt u de implementatieplanning uitgevoerd?** Selecteer **doe ik later**. Klik vervolgens op **OK**.

    ![Implementatieplanning](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Als u de bronomgeving instelt, kunt u een Hyper-V-site maken en Hyper-V-hosts met virtuele machines die u wilt repliceren naar de site toevoegen. Download en installeer vervolgens op elke host de Azure Site Recovery Provider en de Azure Recovery Services-agent. Registreer daarna de Hyper-V-site in de kluis. 

1. Onder **infrastructuur voorbereiden**, klikt u op **bron**.
2. In **bron voorbereiden**, klikt u op **+ Hyper-V-Site**.
3. In **maken van Hyper-V-site**, en geef de naam van site. We maken gebruik van **ContosoHyperVSite**.

    ![Hyper-V-site](./media/hyper-v-azure-tutorial/hyperv-site.png)

3. Nadat de site is gemaakt, in **bron voorbereiden** > **stap 1: Selecteer Hyper-V-site**, selecteer de site die u hebt gemaakt.
4. Klik op **+Hyper-V-server**.

    ![Hyper-V-server](./media/hyper-v-azure-tutorial/hyperv-server.png)

4. Download het installatieprogramma voor de Microsoft Azure Site Recovery Provider.
6. Download de registratiesleutel voor de kluis. U moet deze sleutel om de Provider te installeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Provider downloaden](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Provider installeren

Het gedownloade installatiebestand (AzureSiteRecoveryProvider.exe) installeren op elke Hyper-V-host die u wilt toevoegen aan de Hyper-V-site. De Azure Site Recovery Provider en de Recovery Services-agent worden op elke Hyper-V-host geïnstalleerd.

1. Voer de Setup-bestand.
2. Ga in de installatiewizard van Azure Site Recovery Provider naar **Microsoft Update**, en kies ervoor om Microsoft Update te gebruiken om te controleren op Provider-updates.
3. Accepteer in **Installatie** de standaardinstallatielocatie van de Provider en agent en klik op **Installeren**.
4. Ga na de installatie naar de wizard Registratie voor Microsoft Azure Site Recovery en naar **Kluisinstellingen**, klik op **Bladeren**. Selecteer in **Sleutelbestand** het gedownloade sleutelbestand van de kluis. 
5. Geef het Azure Site Recovery-abonnement, de kluisnaam (**ContosoVMVault**) en de Hyper-V-site (**ContosoHyperVSite**) op waartoe de Hyper-V-server behoort.
6. Selecteer in **Proxy-instellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
7. Klik nadat de server is geregistreerd in de kluis in **Registratie** op **Voltooien**.

De metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Infrastructuur voor Site Recovery** > **Hyper-V-hosts**. Dit proces duurt maximaal 30 minuten.        

#### <a name="install-on-hyper-v-core-server"></a>Op Hyper-V-core-server installeren

Als u een Hyper-V-core-server uitvoert, wordt het setup-bestand downloaden en doe het volgende:

1. Pak de bestanden van AzureSiteRecoveryProvider.exe naar een lokale map door te voeren

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2.  Run `.\setupdr.exe /i. Resultaten worden geregistreerd in % Programdata%\ASRLogs\DRASetupWizard.log.

3.  Registreer de server met de volgende opdracht:

    ```
    cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```
 

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen. 

1. Klik op **Infrastructuur voorbereiden** > **Doel**.
2. Selecteer het abonnement en de resourcegroep **ContosoRG**, waarin de Azure-VM's na failover worden gemaakt.
3. Selecteer het **Resource Manager**-implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

1. Klik op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. We maken gebruik van **ContosoReplicationPolicy**.
3. Voor deze zelfstudie laten we de standaardinstellingen.
    - **Kopieerfrequentie** geeft aan hoe vaak de deltagegevens (na de initiële replicatie) worden gerepliceerd, om de vijf minuten standaard.
    - **Bewaarperiode voor herstelpunten** geeft aan dat herstelpunten worden bewaard gedurende twee uur.
    - **Frequentie van de app-consistente momentopname** geeft aan dat er elk uur herstelpunten met app-consistente momentopnamen worden gemaakt.
    - **Starttijd van de initiële replicatie** geeft aan dat de initiële replicatie direct wordt gestart.
4. Nadat het beleid is gemaakt, klikt u op **OK**. Wanneer u een nieuw beleid maakt, dit automatisch gekoppeld aan de opgegeven Hyper-V-site (in onze zelfstudie dat **ContosoHyperVSite**)

    ![Beleid voor replicatie](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Replicatie inschakelen


1. Klik in **Toepassing repliceren** op **Bron**. 
2. Selecteer in **Bron** de site **ContosoHyperVSite**. Klik vervolgens op **OK**.
3. Verifieer in **Doel** dat Azure het doel is, dat het juiste kluisabonnement wordt gebruikt en dat het **Resource Manager**-implementatiemodel wordt gebruikt.
4. Als u de instellingen van de zelfstudie gebruikt, selecteert u de **contosovmsacct1910171607** storage-account gemaakt in de vorige zelfstudie voor gerepliceerde gegevens, en de **ContosoASRnet** netwerk, in welke Azure-VM's wordt na failover worden geplaatst.
5. Selecteer in **Virtuele machines** > **Selecteren** de VM's die u wilt repliceren. Klik vervolgens op **OK**.

   U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Wanneer de taak **De beveiliging voltooien** is voltooid, is de initiële replicatie voltooid en is de virtuele machine klaar voor failover.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
