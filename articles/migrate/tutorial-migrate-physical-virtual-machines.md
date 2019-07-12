---
title: On-premises fysieke computers of virtuele machines migreren naar Azure met Azure Migrate-servermigratie | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises fysieke computers of virtuele machines migreren naar Azure met Azure Migrate-servermigratie.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8dc3b485d83f7bc55ca913e2c706164d4953ee61
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809601"
---
# <a name="migrate-physical-or-virtualized-servers-to-azure"></a>Fysieke of gevirtualiseerde servers migreren naar Azure 

In dit artikel wordt beschreven hoe u fysieke of gevirtualiseerde servers migreren naar Azure. Het hulpprogramma voor migratie van Azure-Server migreren biedt migratie van fysieke en gevirtualiseerde servers, met behulp van replicatie op basis van een agent. Met dit hulpprogramma, kunt u een breed scala aan machines migreren naar Azure:

- On-premises fysieke servers migreren.
- Virtuele machines gevirtualiseerde door platformen, zoals Xen, KVM migreren.
- Hyper-V of VMware-VM's migreren. Dit is handig als om een bepaalde reden u geen gebruik van de standaard migratiestroom die Azure Migrate-servermigratie biedt voor [Hyper-V](tutorial-migrate-hyper-v.md), [VMware zonder agent](tutorial-migrate-vmware.md) migratie of [VMware op basis van een agent](tutorial-migrate-vmware-agent.md) migratie.
- Migreren van virtuele machines die worden uitgevoerd in privéclouds.
- Virtuele machines in openbare clouds zoals Amazon Web Services (AWS) en Google Cloud Platform (GCP) migreren.


[Azure Migrate](migrate-services-overview.md) biedt een centrale hub volgen detectie, beoordeling en migratie van uw on-premises toepassingen en werkbelastingen en cloud-VM-exemplaren, naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.


In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Azure voorbereiden voor migratie van met het hulpprogramma voor migratie van Azure-Server migreren.
> * Controleer de vereisten voor machines die u wilt migreren en voorbereiden van een virtuele machine voor het Azure Migrate replicatie apparaat dat wordt gebruikt om te detecteren en machines migreren naar Azure.
> * Het hulpprogramma voor migratie van Azure Migrate-Server toevoegen in de Azure Migrate-hub.
> * Instellen van het apparaat van de replicatie.
> * De Mobility-service installeren op computers die u wilt migreren.
> * Replicatie inschakelen.
> * Een testmigratie om te controleren of dat alles werkt zoals verwacht worden uitgevoerd.
> * Voer een een volledige migratie naar Azure.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Bekijk de uitleg voor Azure Migrate voor gedetailleerde instructies.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

Voordat u aan deze zelfstudie begint, dient u eerst:

1. [Beoordeling](migrate-architecture.md) de migratiearchitectuur.
2. Zorg ervoor dat uw Azure-account de rol Inzender voor virtuele machines is toegewezen zodat u machtigingen hebben voor:

    - Het maken van een VM in de geselecteerde resourcegroep.
    - Het maken van een VM in het geselecteerde virtuele netwerk.
    - Schrijven naar een door Azure beheerde schijf. 

3. [Instellen van een Azure-netwerk](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Als u naar Azure repliceert, worden virtuele Azure-machines gemaakt en gekoppeld aan een Azure-netwerk die u opgeeft bij het instellen van de migratie.


## <a name="prepare-azure"></a>Azure voorbereiden

Azure-machtigingen instellen voordat u met Azure Migrate-servermigratie kunt migreren.

- **Maak een project**: Uw Azure-account moet machtigingen voor het maken van een Azure Migrate-project. 
- **Registreren van het apparaat Azure Migrate replicatie**: De replicatie-toepassing maakt en registreert u een Azure Active Directory-app in uw Azure-account. Overdragen van machtigingen voor deze.
- **Key Vault maken**: Voor het migreren van machines, maakt Azure Migrate een Key Vault in de resourcegroep, om toegang tot sleutels naar de replicatie-storage-account in uw abonnement te beheren. Voor het maken van de kluis, moet u machtigingen voor capaciteitstoewijzingen rol maken voor de resourcegroep waarin de Azure Migrate-project zich bevindt. 


### <a name="assign-permissions-to-create-project"></a>Machtigingen toewijzen aan het project maken

1. In de Azure-portal, opent u het abonnement en selecteer **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven.
3. U moet beschikken over **Inzender** of **eigenaar** machtigingen.
    - Als u een gratis Azure-account hebt gemaakt, u kunt de eigenaar van uw abonnement.
    - Als u niet de eigenaar bent, werkt u met de eigenaar van de rol toewijzen.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Machtigingen toewijzen aan de replicatie-apparaat registreren

Voor de migratie op basis van een agent overdragen van machtigingen voor Azure Migrate servermigratie maken en registreren van een Azure AD-app in uw account. U kunt machtigingen met behulp van een van de volgende methoden:

- Een tenant/globale beheerder kan machtigingen verlenen aan gebruikers in de tenant, maken en registreren van Azure AD-apps.
- Een tenant/globale beheerder kan de rol van ontwikkelaar van toepassing (met de machtigingen) toewijzen aan het account.

Is het vermelden waard dat:

- De apps hebt geen andere toegangsmachtigingen voor het abonnement dan de hierboven beschreven.
- Deze machtigingen hoeft u alleen een nieuwe replicatie-apparaat te registreren. Nadat de replicatie-apparaat is ingesteld, kunt u de machtigingen verwijderen. 


#### <a name="grant-account-permissions"></a>Accountmachtigingen verlenen

De tenant/globale beheerder kan als volgt machtigingen verlenen

1. De tenant/globale beheerder moet in Azure AD, navigeer naar **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
2. De beheerder moet ingesteld **App-registraties** naar **Ja**.

    ![Azure AD-machtigingen](./media/tutorial-migrate-physical-virtual-machines/aad.png)

> [!NOTE]
> Dit is een standaardinstelling die gevoelige niet. [Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Toepassingsontwikkelaar rol toewijzen 

De tenant/globale beheerder kan de rol van ontwikkelaar toewijzen aan een account. [Meer informatie](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-permissions-to-create-key-vault"></a>Toewijzen van machtigingen voor het maken van Key Vault

Rolmachtigingen-toewijzing voor de resourcegroep waarin de Azure Migrate-project zich, als volgt bevindt toewijzen:

1. Selecteer in de resourcegroep in Azure portal, **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven. U moet **eigenaar** (of **Inzender** en **Administrator voor gebruikerstoegang**) machtigingen.
3. Als u niet de vereiste machtigingen hebt, kunt u deze van de eigenaar van de resource-groep aanvragen. 

## <a name="prepare-for-migration"></a>Voorbereiden op migratie

### <a name="check-machine-requirements-for-migration"></a>Machine-vereisten voor de migratie controleren

Zorg ervoor dat computers voldoen aan de vereisten voor migratie naar Azure. 

> [!NOTE]
> Migratie op basis van een agent met Azure Migrate-servermigratie is gebaseerd op de functies van de Azure Site Recovery-service. Als aan bepaalde vereisten koppelen aan de documentatie voor Site Recovery.

1. [Verifieer](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) de vereisten voor VMware-servers.
2. [Controleer of](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) VM vereisten voor migratie ondersteunen.
3. Controleer de instellingen van de virtuele machine. On-premises machines u naar Azure repliceren moeten voldoen aan [vereisten voor Azure VM's](migrate-support-matrix-vmware.md#azure-vm-requirements).


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Een virtuele machine voor het apparaat replicatie voorbereiden

Azure Migrate-Server-migratie maakt gebruik van een toestel replicatie voor virtuele machines repliceren naar Azure. Het apparaat van de replicatie wordt uitgevoerd de volgende onderdelen.

- **Configuratieserver**: De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- **Processerver**: De processerver fungeert als replicatiegateway. Deze ontvangt replicatiegegevens; optimaliseert met caching, compressie en codering, en verzendt ze naar een cache-opslagaccount in Azure. 

Voordat u begint, moet u een Windows Server 2016-machine voor het hosten van de replicatie-apparaat voorbereiden. De machine moet voldoen aan de [deze vereisten](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements).


## <a name="add-the-azure-migrate-server-migration-tool"></a>Het hulpprogramma voor migratie van Azure Migrate-Server toevoegen

Instellen van een Azure Migrate-project en vervolgens het hulpprogramma voor migratie van Azure-Server migreren aan toevoegen.

1. In Azure portal > **alle services**, zoeken naar **Azure Migrate**.
2. Onder **Services**, selecteer **Azure Migrate**.
3. In **overzicht**, klikt u op **evalueren en migreren van servers**.
4. Onder **ontdekken, beoordelen en servers migreren**, klikt u op **evalueren en migreren van servers**.

    ![Detecteren en evalueren van servers](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. In **ontdekken, beoordelen en servers migreren**, klikt u op **toevoegen van extra**.
6. In **Migrate-project**, selecteer uw Azure-abonnement en maak een resourcegroep als u dit niet hebt.
7. In **projectdetails**, geef de naam van het project en het geografische gebied waarin u wilt maken van het project en klik op **volgende**

    ![Een Azure Migrate-project maken](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    U kunt een Azure Migrate-project maken in een van deze locaties.

    **Geografie** | **Regio**
    --- | ---
    Azië | Azië - zuidoost
    Europa | Europa - noord of Europa - west
    Verenigde Staten | US - oost of US - west-centraal

    De opgegeven geografie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines. U kunt een doelregio voor de daadwerkelijk migratie selecteren.
8. In **Selecteer assessment tool**, selecteer **toevoegen van een hulpprogramma voor upgradebeoordeling voorlopig overslaan** > **volgende**.
9. In **hulpprogramma voor migratie selecteren**, selecteer **Azure Migrate: Servermigratie** > **volgende**.
10. In **bekijken en toevoegen van extra**, Controleer de instellingen en klik op **hulpprogramma's toevoegen**
11. Na het toevoegen van het hulpprogramma, wordt deze weergegeven in de Azure Migrate-project > **Servers** > **hulpprogramma voor migratie van**.

## <a name="set-up-the-replication-appliance"></a>De replicatie-apparaat instellen

De eerste stap van de migratie is het instellen van het apparaat van de replicatie. U het installer-bestand voor het apparaat downloaden en uitvoeren op de [machine die u hebt voorbereid](#prepare-a-machine-for-the-replication-appliance). Na de installatie van het apparaat, registreren u deze bij de migratie van Azure-Server migreren.


### <a name="download-the-replication-appliance-installer"></a>Download het installatieprogramma van de replicatie-apparaat

1. In de Azure Migrate-project > **Servers**in ***Azure Migrate: Servermigratie**, klikt u op **ontdekken**.

    ![VM's detecteren](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **niet gevirtualiseerd/Overig**.
4. In **doelregio**, selecteert u de Azure-regio waarnaar u wilt migreren van de machines.
5. Selecteer **bevestigen dat de doelregio voor migratie-regionaam**.
6. Klik op **resources maken**. Hiermee maakt u een Azure Site Recovery-kluis op de achtergrond.
    - Worden als u al migratie met Azure Migrate-servermigratie hebt ingesteld, de doeloptie kan niet geconfigureerd, omdat resources eerder zijn ingesteld.
    - U kunt de doelregio voor dit project niet wijzigen nadat u op deze knop te klikken.
    - Alle toekomstige migraties worden naar deze regio.

7. In **wilt u een nieuwe replicatie-apparaat installeren?** , selecteer **installeren van een apparaat replicatie**.
9. In **downloadt en installeert u de replicatie toestelsoftware**, download het installatieprogramma van het apparaat en de registratiesleutel. Hebt u nodig voor de sleutel om het apparaat te registreren. De sleutel is geldig gedurende vijf dagen nadat deze gedownload.

    ![Provider downloaden](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Het apparaat het setup-bestand en sleutelbestand kopiëren naar de Windows Server 2016-machine die u hebt gemaakt voor het apparaat.
11. Voer het installatiebestand van replicatie toestel zoals beschreven in de volgende procedure.
12. Nadat het apparaat opnieuw is opgestart na de installatie, **machines detecteren**, selecteert u het nieuwe apparaat in **Selecteer configuratieserver**, en klikt u op **registratie voltooien**. Voltooien registratie voert een aantal laatste taken voor het voorbereiden van de replicatie-apparaat.

    ![Registratie voltooien](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Duurt tot vijftien minuten na de registratie voltooien totdat gedetecteerde computers worden weergegeven in Azure Migrate-servermigratie. Als u virtuele machines zijn gedetecteerd, de **servers gedetecteerd** tellen oploopt.

![Gedetecteerde servers](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>De Mobility-service installeren

Op computers die u wilt migreren, moet u de Mobility-service-agent installeren. De agent-installatieprogramma's zijn beschikbaar op het apparaat van de replicatie. U vindt het juiste installatieprogramma en installeer de agent op elke machine die u wilt migreren. Doe dit als volgt:

1. Aanmelden bij de replicatie-apparaat.
2. Navigeer naar **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Het installatieprogramma voor het besturingssysteem van de machine en de versie niet vinden. Beoordeling [ondersteunde besturingssystemen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Kopieer het installatiebestand naar de computer die u wilt migreren.
5. Zorg ervoor dat u de wachtwoordzin die is gegenereerd tijdens de implementatie van het apparaat.
    - Het bestand in een tijdelijk bestand op de machine Store.
    - U kunt de wachtwoordzin op het apparaat replicatie verkrijgen. Uitvoeren vanaf de opdrachtregel **C:\ProgramData\ASR\svsystems\bin\genpassphrase.exe - v** om de huidige wachtwoordzin weer te geven.
    - De wachtwoordzin niet opnieuw te genereren. Dit verbreekt de connectiviteit en u moet de replicatie-apparaat registreren.


### <a name="install-on-windows"></a>Installeren in Windows

1. Pak de inhoud van de installer-bestand naar een lokale map (bijvoorbeeld C:\Temp) op de machine als volgt:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Voer het installatieprogramma van de Mobility-Service:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Registreer de agent met de replicatie-apparaat:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Installeren in Linux

1. Pak de inhoud van het installatieprogramma tarball naar een lokale map (bijvoorbeeld /tmp/MobSvcInstaller) op de machine als volgt:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Voer het installatiescript:
    ```
    sudo ./install -r MS -q
    ```
3. Registreer de agent met de replicatie-apparaat:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Virtuele machines repliceren

1. In de Azure Migrate-project > **Servers**, **Azure Migrate: Servermigratie**, klikt u op **repliceren**.

    ![Virtuele machines repliceren](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. In **repliceren**, > **instellingen van de gegevensbron** > **zijn de machines gevirtualiseerd?** , selecteer **Ja, met VMware vSphere**.
3. In **On-premises apparaat**, selecteert u de naam van het apparaat Azure Migrate, die u hebt ingesteld.
4. In **vCenter-server**, geef de naam van de vCenter-server beheren van de VM's of de vSphere-server waarop de virtuele machines worden gehost.
5. In **processerver**, selecteert u de naam van het apparaat van de replicatie.
6. In **Gast referenties**, u een beheerdersaccount voor virtuele machine die wordt gebruikt voor push-installatie van de Mobility-service opgeven. In deze zelfstudie installeert we de Mobility-service handmatig, zodat u een dummy-account kunt toevoegen. Klik vervolgens op **volgende: Virtuele machines**.

    ![Virtuele machines repliceren](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. In **virtuele Machines**in **migratie-instellingen importeren uit een evaluatie?** , laat de standaardinstelling **Nee, ik de migratie-instellingen moet opgeven handmatig**.
8. Controleer elke virtuele machine die u wilt migreren. Klik vervolgens op **volgende: Instellingen voor doel**.

    ![VM's selecteren](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. In **doel instellingen**, selecteert u het abonnement en doelregio waarnaar u migreert, en geef de resourcegroep waarin de Azure VM's worden geplaatst die na de migratie.
10. In **Virtueelnetwerk**, selecteert u het Azure VNet/subnet waarop de Azure VM's na de migratie wordt gekoppeld.
11. In **Azure Hybrid Benefit**:

    - Selecteer **Nee** als u niet wilt dat Azure Hybrid Benefit van toepassing. Klik op **Volgende**.
    - Selecteer **Ja** als u Windows Server-machines die zijn gedekt door een actieve Software Assurance- of Windows Server-abonnementen hebt en u wilt het voordeel toepassen op de computers waarnaar u migreert. Klik op **Volgende**.

    ![Doelinstellingen](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. In **Compute**, Controleer de VM-naam, de grootte, de OS-schijftype en de beschikbaarheidsset. Virtuele machines moeten voldoen aan [Azure-vereisten](migrate-support-matrix-vmware.md#azure-vm-requirements).

    - **VM-grootte**: Standaard kiest Azure Migrate-servermigratie een grootte op basis van het meest overeenkomt met in de Azure-abonnement. U kunt ook kiezen een handmatige grootte in **Azure VM-grootte**. 
    - **Besturingssysteemschijf**: Geef de besturingssysteemschijf (Start) voor de virtuele machine. De besturingssysteemschijf is de schijf met de bootloader van besturingssysteem en het installatieprogramma. 
    - **Beschikbaarheidsset**: Als de virtuele machine moet zich in een Azure-beschikbaarheidsset instellen na de migratie, geeft u de set. De set moet zich in de doelresourcegroep die u voor de migratie opgeeft.

    ![Instellingen voor COMPUTE](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. In **schijven**, opgeven of de VM-schijven moeten worden gerepliceerd naar Azure en selecteer het schijftype (standaard SSD/HDD- of premium beheerde schijven) in Azure. Klik op **Volgende**.
    - U kunt schijven uitsluiten van replicatie.
    - Als u schijven uitsluit, zich niet aanwezig zijn op de virtuele Azure-machine na de migratie. 

    ![Schijfinstellingen voor](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. In **bekijken en replicatie starten**, Controleer de instellingen en klik op **repliceren** starten van de initiële replicatie voor de servers.

> [!NOTE]
> U kunt replicatie-instellingen bijwerken voordat u begint de replicatie, **beheren** > **machines repliceren**. Instellingen kunnen niet worden gewijzigd nadat replicatie is gestart.



## <a name="track-and-monitor"></a>Bijhouden en controleren

- Wanneer u klikt op **repliceren** wordt een taak Start replicatie gestart. 
- Wanneer de taak Start replicatie voltooid is, begint de machines in de initiële replicatie naar Azure.
- Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Incrementele wijzigingen naar on-premises schijven worden regelmatig gerepliceerd naar de replicaschijven in Azure.


U kunt de status van de taak in de portalmeldingen bijhouden.

U kunt de replicatiestatus controleren door te klikken op **servers repliceren** in **Azure Migrate: Servermigratie**.
![Monitor voor replicatie](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren


Wanneer wordt begonnen met replicatie van verschillen, kunt u een testmigratie uitvoeren voor de virtuele machines, voordat u een volledige migratie naar Azure uitvoert. Het is raadzaam dat te ten minste één keer voor elke machine doen voordat u deze migreren.

- Controles zijn uitgevoerd een testmigratie die migratie werkt zoals verwacht, zonder gevolgen voor de on-premises computers die blijven werken, en doorgaan met repliceren. 
- Testmigratie simuleert de migratie door het maken van een Azure-VM met behulp van gerepliceerde gegevens (meestal migreren naar een VNet niet-productie in uw Azure-abonnement).
- U kunt de gerepliceerde test virtuele machine van Azure gebruiken om de migratie valideren, testen van de app uit te voeren en los eventuele problemen op voordat de volledige migratie.

Een testmigratie als volgt:


1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **Test gemigreerd servers**.

     ![Gemigreerde servers testen](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Met de rechtermuisknop op de virtuele machine om te testen, en klikt u op **Test migreren**.

    ![Migratie testen](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. In **testmigratie**, selecteer het Azure VNet waarin de virtuele machine van Azure geplaatst na de migratie worden. Het is raadzaam om dat een niet-productie-VNet te gebruiken.
4. De **de testmigratie** taak wordt gestart. De taak in de portalmeldingen bewaken.
5. Nadat de migratie is voltooid, geven de gemigreerde Azure-VM in **virtuele Machines** in Azure portal. De machinenaam van de heeft een achtervoegsel **-Test**.
6. Nadat de test is voltooid, met de rechtermuisknop op de Azure-VM in **machines repliceren**, en klikt u op **opschonen van de testmigratie**.

    ![Opschonen van de migratie](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>Virtuele machines migreren

Nadat u hebt gecontroleerd dat de testmigratie werkt zoals verwacht, kunt u de on-premises machines migreren.

1. In de Azure Migrate-project > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **servers repliceren**.

    ![Servers repliceren](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. In **machines repliceren**, met de rechtermuisknop op de virtuele machine > **migreren**.
3. In **migreren** > **virtuele machines afsluiten en het uitvoeren van een geplande migratie zonder gegevensverlies**, selecteer **Ja** > **OK** .
    - Standaard Azure Migrate de on-premises VM wordt afgesloten en wordt uitgevoerd een replicatie op-verzoek om te synchroniseren van alle VM-wijzigingen die sinds de laatste replicatie is opgetreden. Dit zorgt ervoor zonder verlies van gegevens.
    - Als u niet dat de virtuele machine afsluiten wilt, selecteert u **Nee**
4. Een migratietaak start voor de virtuele machine. De taak in Azure meldingen volgen.
5. Nadat de taak is voltooid, kunt u weergeven en beheren van de virtuele machine van de **virtuele Machines** pagina.

## <a name="complete-the-migration"></a>Migratie voltooien

1. Nadat de migratie is voltooid, met de rechtermuisknop op de virtuele machine > **migratie stoppen**. Deze replicatie voor de on-premises machine stopt en opschonen van gegevens over de replicatiestatus voor de virtuele machine.
2. Installeer de Azure-VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) of [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) agent op de gemigreerde virtuele machines.
3. Voer correcties van de app uit na de migratie, zoals updates van de databaseverbindingsreeksen en webserverconfiguraties.
4. Voer acceptatietesten van de toepassing en de migratie uit op de gemigreerde toepassing die nu wordt uitgevoerd in Azure.
5. Overzetten van verkeer naar de gemigreerde virtuele machine van Azure-instantie.
6. Verwijder de on-premises VM's uit uw lokale VM-inventaris.
7. Verwijder de on-premises VM's uit de lokale back-ups.
8. Werk eventuele interne documentatie bij met de nieuwe locatie en het nieuwe IP-adres van de Azure VM's. 

## <a name="post-migration-best-practices"></a>Aanbevolen procedures voor na de migratie

- Voor grotere flexibiliteit:
    - Houd uw gegevens veilig door back-ups van virtuele Azure VM‘s te maken met behulp van de Azure Backup-service. [Meer informatie](../backup/quick-backup-vm-portal.md).
    - Houd workloads continu beschikbaar door Azure VM‘s naar een secundaire regio te repliceren met Site Recovery. [Meer informatie](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Voor betere beveiliging:
    - Vergrendelen en beperken van toegang tot het binnenkomende verkeer met [Azure Security Center - Just-in-time-beheer](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Beperk het netwerkverkeer naar beheereindpunten met [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implementeer [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) om schijven te beveiligen en gegevens te beschermen tegen diefstal en onbevoegde toegang.
    - Lees meer informatie over [IaaS-resources beveiligen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) en bezoek het [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Voor controle en beheer:
    - Overweeg de implementatie van [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) om uw resourcegebruik en uitgaven te bewaken.


## <a name="next-steps"></a>Volgende stappen

Onderzoek de [migratie naar de cloud](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) in Azure Cloud Adoption Framework.
