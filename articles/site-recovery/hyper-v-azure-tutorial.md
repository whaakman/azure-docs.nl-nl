---
title: Herstel na noodgevallen instellen voor on-premises Hyper-V-machines (zonder VMM) naar Azure met Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen van on-premises Hyper-V-machines (zonder VMM) naar Azure met behulp van Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: f4c9ddf608033f6b85268fe74b12861eb8c30082
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295162"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Herstel na noodgevallen instellen voor on-premises Hyper-V-VM's naar Azure

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor herstel na noodgevallen door te beheren en organiseren van replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Dit is de derde zelfstudie in een reeks. Hier ziet u hoe u herstel na noodgevallen van on-premises Hyper-V-machines naar Azure instelt. In deze zelfstudie is van toepassing Hyper-V-machines die niet worden beheerd door Microsoft System Center Virtual Machine Manager (VMM).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel selecteren.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen en de replicatiedoelomgeving instellen.
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
6. In **gebruikt u System Center VMM voor het beheren van uw Hyper-V-hosts?** , selecteer **Nee**.
7. Selecteer **OK**.

    ![Replicatiedoel](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

1. In **implementatieplanning**, als u van plan bent een grote implementatie, de Deployment Planner voor Hyper-V downloaden via de koppeling op de pagina. [Meer informatie](hyper-v-deployment-planner-overview.md) over het plannen van Hyper-V-implementatie.
2. We hebben nodig niet de Deployment Planner voor deze zelfstudie. In **hebt u de implementatieplanning uitgevoerd?** , selecteer **doe ik later**, en selecteer vervolgens **OK**.

    ![Implementatieplanning](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Als u de bronomgeving instelt, moet u een Hyper-V-site maken en toevoegen aan die site de Hyper-V-hosts met virtuele machines die u wilt repliceren. Vervolgens u downloaden en installeren van de Azure Site Recovery Provider en de Azure Recovery Services-agent op elke host en registreert u de Hyper-V-site in de kluis.

1. Onder **infrastructuur voorbereiden**, selecteer **bron**.
2. In **bron voorbereiden**, selecteer **+ Hyper-V-Site**.
3. In **maken van Hyper-V-site**, geef de naam van de site. We maken gebruik van **ContosoHyperVSite**.

    ![Hyper-V-site](./media/hyper-v-azure-tutorial/hyperv-site.png)

4. Nadat de site is gemaakt, in **bron voorbereiden** > **stap 1: Selecteer Hyper-V-site**, selecteer de site die u hebt gemaakt.
5. Selecteer **+ Hyper-V-Server**.

    ![Hyper-V-server](./media/hyper-v-azure-tutorial/hyperv-server.png)

6. Download het installatieprogramma voor de Microsoft Azure Site Recovery Provider.
7. Download de kluisregistratiesleutel. U moet deze sleutel om de Provider te installeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Provider en de registratiesleutel downloaden](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Provider installeren

Het gedownloade installatiebestand (AzureSiteRecoveryProvider.exe) installeren op elke Hyper-V-host die u wilt toevoegen aan de Hyper-V-site. Setup installeert de Azure Site Recovery Provider en Recovery Services-agent op elke Hyper-V-host.

1. Voer de setup-bestand.
2. Ga in de installatiewizard van Azure Site Recovery Provider naar **Microsoft Update**, en kies ervoor om Microsoft Update te gebruiken om te controleren op Provider-updates.
3. In **installatie**, accepteer de standaardlocatie voor installatie van de Provider en agent en selecteert u **installeren**.
4. Na de installatie, in de Wizard registratie van Microsoft Azure Site Recovery > **Kluisinstellingen**, selecteer **Bladeren**, en in **sleutelbestand**, selecteer de kluissleutel die het bestand u hebt gedownload.
5. Geef het Azure Site Recovery-abonnement, de kluisnaam (**ContosoVMVault**) en de Hyper-V-site (**ContosoHyperVSite**) op waartoe de Hyper-V-server behoort.
6. Selecteer in **Proxy-instellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
7. In **registratie**, nadat de server is geregistreerd in de kluis, selecteer **voltooien**.

De metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Infrastructuur voor Site Recovery** > **Hyper-V-hosts**. Dit proces duurt maximaal 30 minuten.

#### <a name="install-the-provider-on-a-hyper-v-core-server"></a>De Provider installeren op een Hyper-V-core-server

Als u een Hyper-V-core-server uitvoert, wordt het setup-bestand downloaden en als volgt te werk:

1. Pak de bestanden van AzureSiteRecoveryProvider.exe naar een lokale map met deze opdracht:

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2. Voer `.\setupdr.exe /i` uit. Resultaten worden geregistreerd in % Programdata%\ASRLogs\DRASetupWizard.log.

3. Registreer de server door het uitvoeren van deze opdracht:

    ```
    cd  "C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen:

1. Selecteer **Infrastructuur voorbereiden** > **Doel**.
2. Selecteer het abonnement en de resourcegroep **ContosoRG** waarin de Azure VM's zullen worden gemaakt na een failover.
3. Selecteer het **Resource Manager**-implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

1. Selecteer **infrastructuur voorbereiden** > **replicatie-instellingen** >  **+ maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. We maken gebruik van **ContosoReplicationPolicy**.
3. Voor deze zelfstudie laten we de standaard-instellingen:
    - **Kopieerfrequentie** geeft aan hoe vaak de deltagegevens (na de initiële replicatie) worden gerepliceerd. De standaardfrequentie is om de vijf minuten.
    - **Bewaarperiode voor herstelpunten** geeft aan dat herstelpunten worden bewaard gedurende twee uur.
    - **Frequentie van de app-consistente momentopname** geeft aan dat er elk uur herstelpunten met app-consistente momentopnamen worden gemaakt.
    - **Begintijd initiële replicatie** geeft aan dat de initiële replicatie direct wordt gestart.
4. Nadat het beleid is gemaakt, selecteert u **OK**. Wanneer u een nieuw beleid maakt, heeft dit automatisch gekoppeld aan de opgegeven Hyper-V-site. In onze zelfstudie dat **ContosoHyperVSite**.

    ![Beleid voor replicatie](./media/hyper-v-azure-tutorial/replication-policy.png)

## <a name="enable-replication"></a>Replicatie inschakelen

1. In **toepassing repliceren**, selecteer **bron**.
2. Selecteer in **Bron** de site **ContosoHyperVSite**. Selecteer vervolgens **OK**.
3. In **doel**, Controleer of het doel (Azure), het juiste kluisabonnement wordt en de **Resource Manager** implementatiemodel.
4. Als u de instellingen van de zelfstudie gebruikt, selecteert u de **contosovmsacct1910171607** storage-account gemaakt in de vorige zelfstudie voor gerepliceerde gegevens. Selecteert u ook de **ContosoASRnet** netwerk, in welke Azure-VM's moeten worden geplaatst na een failover.
5. In **virtuele machines** > **Selecteer**, selecteer de virtuele machine die u wilt repliceren. Selecteer vervolgens **OK**.

   U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
