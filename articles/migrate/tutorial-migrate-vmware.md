---
title: Migreren on-premises virtuele VMware-machines naar Azure met uitzonderingsbewaking zonder agents Azure migreren servermigratie | Microsoft Docs
description: Beschrijft hoe om uit te voeren en zonder agents migratie van on-premises VMware-machines naar Azure met Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 3510c0505a5a3c1353642baf5060a83d13fdd43a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809125"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>Virtuele VMware-machines migreren naar Azure (zonder Agent)

Dit artikel ziet u hoe u on-premises VMware-machines migreren naar Azure, met behulp van de migratie van de zonder Agent met het hulpprogramma voor migratie van Azure-Server migreren.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen en workloads en AWS/GCP VM-instanties naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

In deze zelfstudie is de derde in een serie die laat zien hoe u om te beoordelen en migreren van virtuele VMware-machines naar Azure met behulp van Azure Migrate-Server-evaluatie en migratie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Virtuele machines voorbereiden voor migratie.
> * Het hulpprogramma voor migratie van Azure migratie-Server toevoegen.
> * Virtuele machines die u wilt migreren detecteren.
> * Beginnen met het repliceren van virtuele machines.
> * Een testmigratie om te controleren of dat alles werkt zoals verwacht worden uitgevoerd.
> * Voer een volledige VM-migratie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="migration-methods"></a>Migratiemethoden

U kunt virtuele VMware-machines migreren naar Azure met behulp van het hulpprogramma voor migratie van Azure-Server migreren. Dit hulpprogramma biedt een aantal opties voor VMware-VM-migratie:

- Migratie met behulp van zonder agents replicatie. Virtuele machines migreren zonder dat u hoeft te installeren op deze.
- Migratie met een agent voor replicatie. Een agent installeren op de virtuele machine voor replicatie.

Om te beslissen of u wilt gebruiken van de migratie zonder agent of op basis van een agent, lees deze artikelen:

- [Informatie over hoe](server-migrate-overview.md) migratie zonder Agent werkt, en [de beperkingen bekijken](server-migrate-overview.md#agentless-migration-limitations).
- [Lees dit artikel](tutorial-migrate-vmware-agent.md) als u wilt gebruiken van de methode op basis van een agent.

## <a name="prerequisites"></a>Vereisten

Voordat u aan deze zelfstudie begint, dient u eerst:

1. [Inzicht in](migrate-architecture.md) de architectuur van de migratie van VMware.
2. [Voltooi de eerste zelfstudie](tutorial-prepare-vmware.md) in deze reeks het instellen van Azure en VMware voor migratie. In deze zelfstudie moet u bijzonder naar:
    - [Azure voorbereiden](tutorial-prepare-vmware.md#prepare-azure) voor migratie.
    - [Voorbereiden van de on-premises omgeving](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) voor migratie.
    
3. Het is raadzaam dat u het beoordelen van virtuele VMware-machines met Azure Migrate-evaluatie Server probeert voordat u migreert ze naar Azure. Voor het instellen van de evaluatie, [voltooien van de tweede zelfstudie](tutorial-assess-vmware.md) in deze reeks. Als u niet wilt beoordelen van virtuele machines kunt u deze zelfstudie overslaan. Hoewel u wordt aangeraden dat u een evaluatie uitproberen, maar u hebt geen tot een evaluatie uitvoeren voordat u een migratie probeert.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Het hulpprogramma voor migratie van Azure Migrate-Server toevoegen

Als u niet de tweede zelfstudie voor het evalueren van virtuele VMware-machines uitvoert, moet u [Volg deze instructies](how-to-add-tool-first-time.md) instellen van een Azure Migrate-project en selecteert u het hulpprogramma voor migratie van Azure-Server migreren. 

Als u de tweede zelfstudie gevolgd en hebt al een Azure Migrate-project instellen, als volgt het hulpprogramma voor migratie van Azure Migrate-Server toevoegen:

1. Klik in de Azure Migrate-project op **overzicht**. 
2. In **ontdekken, beoordelen, en migratie van servers**, klikt u op **evalueren en migreren van servers**.

     ![Evalueren en migreren van servers](./media/tutorial-migrate-vmware/assess-migrate.png)

3. In **hulpprogramma voor migratie van**, selecteer **Klik hier als u wilt toevoegen van een hulpprogramma voor migratie wanneer u klaar bent om**.

    ![Hulpprogramma selecteren](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Selecteer in de lijst met hulpprogramma's, **Azure Migrate: Servermigratie** > **hulpprogramma toevoegen**

    ![Hulpprogramma voor migratie van server](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Instellen van het apparaat Azure Migrate

Azure Migrate-Server-migratie wordt uitgevoerd een lichtgewicht VMware-VM-apparaat. Het toestel VM-detectie wordt uitgevoerd en verzendt de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure migreren servermigratie. Het hetzelfde apparaat wordt ook gebruikt door het hulpprogramma Azure Migrate-Server-evaluatie.

Als u de tweede zelfstudie voor het evalueren van virtuele VMware-machines hebt gevolgd, instellen u al van het apparaat tijdens deze zelfstudie. Als u niet hebt die zelfstudie volgt, moet u het apparaat is nu ingesteld. Hiervoor u: 

- Een sjabloon voor OVA-bestanden downloaden en importeren naar de vCenter-Server.
- Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan. 
- Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.

Volg de instructies in [in dit artikel](how-to-set-up-appliance-vmware.md) voor het instellen van het apparaat.


## <a name="prepare-vms-for-migration"></a>Virtuele machines voorbereiden voor migratie

Azure Migrate is vereist voor sommige wijzigingen van de virtuele machine om ervoor te zorgen dat virtuele machines kunnen worden gemigreerd naar Azure.

- Voor sommige [besturingssystemen](server-migrate-overview.md#agentless-migration-limitations), Azure Migrate kunt u deze wijzigingen automatisch.
- Als een virtuele machine waarop een van deze besturingssystemen niet u migreert waarnaar, volgt u de instructies voor het voorbereiden van de virtuele machine.
- Het is belangrijk dat u deze wijzigingen voordat u begint met de migratie. Als u de virtuele machine migreert voordat u de wijziging hebt aangebracht, de virtuele machine mogelijk niet opgestart in Azure.
- Configuratiewijzigingen die u op de on-premises VM's worden gerepliceerd naar Azure, nadat replicatie voor de virtuele machine is ingeschakeld. Om ervoor te zorgen dat wijzigingen worden gerepliceerd, zorg ervoor dat het herstelpunt dat u naar migreert hoger dan de tijd waarbinnen de configuratiewijzigingen is on-premises zijn aangebracht.


### <a name="prepare-windows-server-vms"></a>Windows Server Virtual machines voorbereiden

**Actie** | **Details** | **Instructies**
--- | --- | ---
Zorg ervoor dat Windows-volumes in de Azure-VM dezelfde stationsletters als de on-premises VM gebruiken. | SAN-beleid configureren als Online alle. | 1. Aanmelden bij de virtuele machine met een beheerdersaccount en open een opdrachtvenster.<br/> 2. Type **diskpart** de Diskpart-hulpprogramma uit te voeren.<br/> 3. Type **SAN-beleid OnlineAll =**<br/> 4. Typ Exit Diskpart verlaten, en sluit de opdrachtprompt.
Seriële toegang tot Azure-console inschakelt voor de Azure-VM | Dit helpt bij het oplossen van problemen. U moet niet opnieuw opstarten van de virtuele machine. De Azure-VM wordt opgestart met behulp van de schijf en dit is gelijk aan voor de nieuwe virtuele machine opnieuw worden opgestart. | Ga als volgt [deze instructies](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console#enable-serial-console-in-custom-or-older-images) om in te schakelen.
Hyper-V-Gast-integratie installeren | Als computers met Windows Server 2003, u migreert waarnaar, moet u Hyper-V Gast-integratieservices installeren op de VM-besturingssysteem. | [Meer informatie](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Extern bureaublad | Extern bureaublad inschakelen op de virtuele machine en controleer dat de Windows Firewall niet is blokkeert de toegang van extern bureaublad op alle profielen. | [Meer informatie](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Virtuele Linux-machines voorbereiden

**Actie** | **Details** 
--- | --- | ---
Hyper-V Linux integratieservices installeren | De meeste nieuwe versies van Linux-distributies zijn dit standaard opgenomen.
Opnieuw opbouwen van de Linux-init-installatiekopie bevat de benodigde stuurprogramma's van Hyper-V | Dit zorgt ervoor dat de virtuele machine wordt opgestart in Azure en alleen op bepaalde distributies vereist is.
Seriële console van het Azure-logboekregistratie inschakelen | Dit helpt bij het oplossen van problemen. U moet niet opnieuw opstarten van de virtuele machine. De Azure-VM wordt opgestart met behulp van de schijf en dit is gelijk aan voor de nieuwe virtuele machine opnieuw worden opgestart.<br/> Ga als volgt [deze instructies](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) om in te schakelen.
Bijwerken van toewijzingsbestand van apparaat | Bijwerken van het apparaat kaart-bestand met de naam van het apparaat naar volume koppelingen, permanente apparaat-id's gebruiken
Vermeldingen in fstab bijwerken | Vermeldingen voor het gebruik van permanent volume-id's worden bijgewerkt.
Udev-regel verwijderen | Verwijder de udev-regels die namen van de gebruikersinterface op basis van mac-adres enzovoort gereserveerd.
Update-netwerkinterfaces | Netwerkinterfaces voor het ontvangen van IP-adres op basis van DHCP bijwerken.
Ssh inschakelen | Zorg ervoor dat ssh is ingeschakeld en de sshd-service is ingesteld op automatisch starten bij opnieuw opstarten.<br/> Zorg ervoor dat binnenkomende ssh verbindingsaanvragen worden niet geblokkeerd door de firewall besturingssysteem of aanpasbare regels.

[In dit artikel volgen](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic) die worden beschreven stappen voor het uitvoeren van een Linux-VM op Azure, en bevat instructies voor een aantal populaire Linux-distributies.  


## <a name="replicate-vms"></a>Virtuele machines repliceren

Met de detectie is voltooid, kunt u beginnen met replicatie van VMware-machines naar Azure.

1. In de Azure Migrate-project > **Servers**, **Azure Migrate: Servermigratie**, klikt u op **repliceren**.

    ![Virtuele machines repliceren](./media/tutorial-migrate-vmware/select-replicate.png)

2. In **repliceren**, > **instellingen van de gegevensbron** > **zijn de machines gevirtualiseerd?** , selecteer **Ja, met VMware vSphere**.
3. In **On-premises apparaat**, selecteert u de naam van het apparaat Azure Migrate, die u hebt ingesteld > **OK**. 

    ![Instellingen voor gegevensbron](./media/tutorial-migrate-vmware/source-settings.png)

    - Deze stap wordt ervan uitgegaan dat u al hebt ingesteld een apparaat wanneer u de zelfstudie hebt voltooid.
    - Als u een apparaat hebt ingesteld, volgt u de instructies in [in dit artikel](how-to-set-up-appliance-vmware.md).

4. In **virtuele machines**, selecteert u de machines die u wilt repliceren.
    - Als u hebt een evaluatie uitgevoerd voor de virtuele machines, kunt u de grootte van de virtuele machine en de schijf type (premium of standaard) aanbevelingen van de resultaten van de evaluatie kunt toepassen. Om dit te doen, in **migratie-instellingen importeren uit een Azure Migrate-evaluatie?** , selecteer de **Ja** optie.
    - Als u een evaluatie niet hebt uitgevoerd, of als u niet wilt gebruiken van de evaluatie-instellingen, selecteert u de **Nee** opties.
    - Als u gebruik van de evaluatie hebt geselecteerd, selecteert u de VM-groep en de naam van de evaluatie.

    ![Beoordeling selecteren](./media/tutorial-migrate-vmware/select-assessment.png)

5. In **virtuele machines**, virtuele machines zoeken naar behoefte en controleren van elke virtuele machine die u wilt migreren. Klik vervolgens op **volgende: Instellingen voor doel**.

    ![VM's selecteren](./media/tutorial-migrate-vmware/select-vms.png)

6. In **doel instellingen**, selecteert u het abonnement en doelregio waarnaar u migreert, en geef de resourcegroep waarin de Azure VM's worden geplaatst die na de migratie. In **Virtueelnetwerk**, selecteert u het Azure VNet/subnet waarop de Azure VM's na de migratie wordt gekoppeld.
7. In **Azure Hybrid Benefit**:

    - Selecteer **Nee** als u niet wilt dat Azure Hybrid Benefit van toepassing. Klik op **Volgende**.
    - Selecteer **Ja** als u Windows Server-machines die zijn gedekt door een actieve Software Assurance- of Windows Server-abonnementen hebt en u wilt het voordeel toepassen op de computers waarnaar u migreert. Klik op **Volgende**.

    ![Doelinstellingen](./media/tutorial-migrate-vmware/target-settings.png)

8. In **Compute**, Controleer de VM-naam, de grootte, de OS-schijftype en de beschikbaarheidsset. Virtuele machines moeten voldoen aan [Azure-vereisten](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM-grootte**: Als u aanbevelingen voor de evaluatie, bevat de VM-grootte vervolgkeuzelijst de aanbevolen grootte. Azure Migrate kiest anders een grootte op basis van het meest overeenkomt met in de Azure-abonnement. U kunt ook kiezen een handmatige grootte in **Azure VM-grootte**. 
    - **Besturingssysteemschijf**: Geef de besturingssysteemschijf (Start) voor de virtuele machine. De besturingssysteemschijf is de schijf met de bootloader van besturingssysteem en het installatieprogramma. 
    - **Beschikbaarheidsset**: Als de virtuele machine moet zich in een Azure-beschikbaarheidsset instellen na de migratie, geeft u de set. De set moet zich in de doelresourcegroep die u voor de migratie opgeeft.

    ![Compute-VM-instellingen](./media/tutorial-migrate-vmware/compute-settings.png)

9. In **schijven**, opgeven of de VM-schijven moeten worden gerepliceerd naar Azure en selecteer het schijftype (standaard SSD harde schijf of premium-beheerde schijven) in Azure. Klik op **Volgende**.
    - U kunt schijven uitsluiten van replicatie.
    - Als u schijven uitsluit, zich niet aanwezig zijn op de virtuele Azure-machine na de migratie. 

    ![Disks](./media/tutorial-migrate-vmware/disks.png)

10. In **bekijken en replicatie starten**, Controleer de instellingen en klik op **repliceren** starten van de initiële replicatie voor de servers.

> [!NOTE]
> U kunt replicatie-instellingen bijwerken voordat u begint de replicatie, in **beheren** > **machines repliceren**. Instellingen kunnen niet worden gewijzigd nadat replicatie is gestart.

### <a name="provisioning-for-the-first-time"></a>Voor de eerste keer wordt ingericht

Als dit de eerste virtuele machine die u in de Azure Migrate-project repliceert, richt Azure Migrate-servermigratie automatisch deze resources in dezelfde resourcegroep als het project.

- **Service bus**: Azure Migrate-Server-migratie maakt gebruik van de servicebus replicatie orchestration om berichten te verzenden naar het apparaat.
- **Storage-account van gateway**: Migratie-server maakt gebruik van de storage-account van de gateway voor het opslaan van statusinformatie over de virtuele machines die worden gerepliceerd.
- **Logboekopslagaccount**: Het apparaat Azure Migrate replicatielogboeken wordt voor virtuele machines worden geüpload naar een opslagaccount. Azure Migrate geldt de replicatie-informatie voor de beheerde replicaschijven.
- **Sleutelkluis**: Het apparaat Azure Migrate maakt gebruik van de key vault voor het beheren van verbindingsreeksen voor de servicebus en toegangssleutels voor de storage-accounts die worden gebruikt in replicatie. U moet u de machtigingen die de key vault nodig heeft voor toegang tot het opslagaccount bij het voorbereiden hebt ingesteld. [Bekijk deze machtigingen](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>Bijhouden en controleren


- Wanneer u klikt op **repliceren** wordt een taak Start replicatie gestart. 
- Wanneer de taak Start replicatie voltooid is, begint de machines in de initiële replicatie naar Azure.
- Tijdens de initiële replicatie wordt een momentopname van een virtuele machine gemaakt. Schijfgegevens van de momentopname wordt gerepliceerd naar beheerde replicaschijven in Azure.
- Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Incrementele wijzigingen naar on-premises schijven worden regelmatig gerepliceerd naar de replicaschijven in Azure.

U kunt de status van de taak in de portalmeldingen bijhouden.

U kunt de replicatiestatus controleren door te klikken op **servers repliceren** in **Azure Migrate: Servermigratie**.
![Monitor voor replicatie](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren


Wanneer wordt begonnen met replicatie van verschillen, kunt u een testmigratie uitvoeren voor de virtuele machines, voordat u een volledige migratie naar Azure uitvoert. Het is raadzaam dat te ten minste één keer voor elke machine doen voordat u deze migreren.

- Controles zijn uitgevoerd een testmigratie die migratie werkt zoals verwacht, zonder gevolgen voor de on-premises computers die blijven werken, en doorgaan met repliceren. 
- Testmigratie simuleert de migratie door het maken van een Azure-VM met behulp van gerepliceerde gegevens (meestal migreren naar een VNet niet-productie in uw Azure-abonnement).
- U kunt de gerepliceerde test virtuele machine van Azure gebruiken om de migratie valideren, testen van de app uit te voeren en los eventuele problemen op voordat de volledige migratie.

Een testmigratie als volgt:


1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **Test gemigreerd servers**.

     ![Gemigreerde servers testen](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Met de rechtermuisknop op de virtuele machine om te testen, en klikt u op **Test migreren**.

    ![Migratie testen](./media/tutorial-migrate-vmware/test-migrate.png)

3. In **testmigratie**, selecteer het Azure VNet waarin de virtuele machine van Azure geplaatst na de migratie worden. Het is raadzaam om dat een niet-productie-VNet te gebruiken.
4. De **de testmigratie** taak wordt gestart. De taak in de portalmeldingen bewaken.
5. Nadat de migratie is voltooid, geven de gemigreerde Azure-VM in **virtuele Machines** in Azure portal. De machinenaam van de heeft een achtervoegsel **-Test**.
6. Nadat de test is voltooid, met de rechtermuisknop op de Azure-VM in **machines repliceren**, en klikt u op **opschonen van de testmigratie**.

    ![Opschonen van de migratie](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>Virtuele machines migreren

Nadat u hebt gecontroleerd dat de testmigratie werkt zoals verwacht, kunt u de on-premises machines migreren.

1. In de Azure Migrate-project > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **servers repliceren**.

    ![Servers repliceren](./media/tutorial-migrate-vmware/replicate-servers.png)

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
-  Overweeg de implementatie van [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) om uw resourcegebruik en uitgaven te bewaken.


## <a name="next-steps"></a>Volgende stappen

Onderzoek de [migratie naar de cloud](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) in Azure Cloud Adoption Framework.
