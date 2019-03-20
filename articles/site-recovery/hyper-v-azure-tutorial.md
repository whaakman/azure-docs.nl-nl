---
title: Herstel na noodgevallen instellen voor on-premises Hyper-V-VM's (zonder VMM) naar Azure met Azure Site Recovery | Microsoft Docs
description: Leer hoe u herstel na noodgevallen kunt instellen voor on-premises Hyper-V-VM's (zonder VMM) naar Azure met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/28/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ac214e9d29bade37bcb00f804859db730131752b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58087326"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Herstel na noodgevallen instellen voor on-premises Hyper-V-VM's naar Azure

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgevallen door de replicatie, failover en failback van on-premises machines en virtuele Azure-machines te beheren en in te delen.

In deze zelfstudie leert u hoe u herstel na noodgevallen instelt voor on-premises Hyper-V-VM's naar Azure. De zelfstudie is van toepassing op Hyper-V-VM's die niet worden beheerd door System Center Virtual Machine Manager (VMM). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel selecteren.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatiedoelomgeving instellen.
> * Een replicatiebeleid maken.
> * Replicatie voor een VM inschakelen.

Dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)

Voordat u begint, is het handig om [de architectuur te bekijken](concepts-hyper-v-to-azure-architecture.md) voor dit noodherstelscenario.

## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren


1. Selecteer in **Alle services** > **Recovery Services-kluizen** de kluis die in de vorige zelfstudie is voorbereid: **ContosoVMVault**.
2. Klik in **Aan de slag** op **Site Recovery**. Klik vervolgens op **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. Bij **Gebruikt u System Center VMM om uw Hyper-V-hosts te beheren** selecteert u **Nee**. Klik vervolgens op **OK**.

    ![Replicatiedoel](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Wanneer u van plan bent een grote implementatie uit te voeren, moet u ervoor zorgen dat u eerst de [implementatieplanning voor Hyper-V-replicatie hebt voltooid](hyper-v-deployment-planner-overview.md). In het kader van deze zelfstudie selecteert u bij **Hebt u de implementatieplanning uitgevoerd?**, **Dat doe ik later** in de vervolgkeuzelijst.

![Implementatieplanning](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Voor het instellen van de bronomgeving, maakt u eerst een Hyper-V-site en voegt u Hyper-V-hosts toe aan die site. Download en installeer vervolgens op elke host de Azure Site Recovery Provider en de Azure Recovery Services-agent. Registreer daarna de Hyper-V-site in de kluis. 

1. Klik in **Infrastructuur voorbereiden** op **Bron**.
2. Klik op **+Hyper-V-site** en geef de naam op van de site die in de vorige zelfstudie is gemaakt: **ContosoHyperVSite**.

    ![Hyper-V-site](./media/hyper-v-azure-tutorial/hyperv-site.png)

3. Nadat de site is gemaakt, klikt u op **+Hyper-V Server**.

    ![Hyper-V-server](./media/hyper-v-azure-tutorial/hyperv-server.png)

4. Download het Provider-installatiebestand.
6. Download de registratiesleutel voor de kluis. U hebt deze sleutel nodig om de Provider-installatie uit te voeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Provider downloaden](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Provider installeren

Voer het Provider-installatiebestand (AzureSiteRecoveryProvider.exe) uit op elke Hyper-V-host die u hebt toegevoegd aan de site **ContosoHyperVSite**. De Azure Site Recovery Provider en de Recovery Services-agent worden op elke Hyper-V-host geïnstalleerd.

1. Ga in de installatiewizard van Azure Site Recovery Provider naar **Microsoft Update**, en kies ervoor om Microsoft Update te gebruiken om te controleren op Provider-updates.
2. Accepteer in **Installatie** de standaardinstallatielocatie van de Provider en agent en klik op **Installeren**.
3. Ga na de installatie naar de wizard Registratie voor Microsoft Azure Site Recovery en naar **Kluisinstellingen**, klik op **Bladeren**. Selecteer in **Sleutelbestand** het gedownloade sleutelbestand van de kluis. 
4. Geef het Azure Site Recovery-abonnement, de kluisnaam (**ContosoVMVault**) en de Hyper-V-site (**ContosoHyperVSite**) op waartoe de Hyper-V-server behoort.
5. Selecteer in **Proxy-instellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
6. Klik nadat de server is geregistreerd in de kluis in **Registratie** op **Voltooien**.

De metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Infrastructuur voor Site Recovery** > **Hyper-V-hosts**. Dit proces duurt maximaal 30 minuten.        

Indien u een Hyper-V core-server gebruikt, volgt u de onderstaande stappen zodra u de aanmeldingsgegevens van de provider en van de kluis hebt gedownload, zoals [hier](#set-up-the-source-environment) wordt vermeld

1. Pak de bestanden van AzureSiteRecoveryProvider.exe uit door het uitvoeren van

    ``AzureSiteRecoveryProvider.exe /x:. /q``
 
    Hiermee worden de bestanden uitgepakt naar de lokale map.
 
2.  Voer ``.\setupdr.exe /i `` uit.

    De resultaten worden vastgelegd in %Programdata%\ASRLogs\DRASetupWizard.log

3.  Gebruik de volgende opdracht om de server te registreren:

``cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved" ``
 

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen. 

1. Klik op **Infrastructuur voorbereiden** > **Doel**.
2. Selecteer het abonnement en de resourcegroep **ContosoRG**, waarin de Azure-VM's na failover worden gemaakt.
3. Selecteer het **Resource Manager**-implementatiemodel.

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

> [!NOTE]
> Bij replicatiebeleid voor Hyper-V naar Azure wordt de kopieerfrequentie van 15 minuten buiten gebruik gesteld. In plaats daarvan worden de instellingen voor een kopieerfrequentie van 5 minuten en van 30 seconden gebruikt. Replicatiebeleid waarbij gebruik wordt gemaakt van een kopieerfrequentie van 15 minuten wordt automatisch bijgewerkt om gebruik te maken van een instelling voor een kopieerfrequentie van 5 minuten. De opties voor een kopieerfrequentie van 5 minuten en 30 seconden bieden verbeterde replicatieprestaties en betere beoogde herstelpunten in vergelijking met een kopieerfrequentie van 15 minuten, met een minimale impact op bandbreedtegebruik en gegevensoverdrachtvolume.

1. Klik op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.
2. Geef in **Beleid maken en koppelen** de beleidsnaam **ContosoReplicationPolicy** op.
3. Behoud de standaardinstellingen en klik op **OK**.
    - **Kopieerfrequentie** geeft aan dat deltagegevens (na de initiële replicatie) om de vijf minuten worden gerepliceerd.
    - **Bewaarperiode van het herstelpunt** geeft aan dat het retentietijdvenster voor elk herstelpunt twee uur is.
    - **Frequentie van de app-consistente momentopname** geeft aan dat er elk uur herstelpunten met app-consistente momentopnamen worden gemaakt.
    - **Starttijd van de initiële replicatie** geeft aan dat de initiële replicatie direct wordt gestart.
4. Nadat het beleid is gemaakt, klikt u op **OK**. Wanneer u een nieuw beleid maakt, wordt dit automatisch gekoppeld aan de opgegeven Hyper-V-site (**ContosoHyperVSite**)

    ![Beleid voor replicatie](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Replicatie inschakelen


1. Klik in **Toepassing repliceren** op **Bron**. 
2. Selecteer in **Bron** de site **ContosoHyperVSite**. Klik vervolgens op **OK**.
3. Verifieer in **Doel** dat Azure het doel is, dat het juiste kluisabonnement wordt gebruikt en dat het **Resource Manager**-implementatiemodel wordt gebruikt.
4. Selecteer het opslagaccount **contosovmsacct1910171607**, dat in de vorige zelfstudie voor gerepliceerde gegevens is gemaakt, en het netwerk **ContosoASRnet**, waarin Azure-VM's na failover worden geplaatst.
5. Selecteer in **Virtuele machines** > **Selecteren** de VM's die u wilt repliceren. Klik vervolgens op **OK**.

   U kunt de voortgang van de actie **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Wanneer de taak **De beveiliging voltooien** is voltooid, is de initiële replicatie voltooid en is de virtuele machine klaar voor failover.

## <a name="next-steps"></a>Volgende stappen
[Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
