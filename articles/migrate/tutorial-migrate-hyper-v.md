---
title: On-premises Hyper-V virtuele machines migreren naar Azure met Azure migratie-Server migreren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises Hyper-V-machines migreren naar Azure met Azure Migrate-servermigratie
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 4a88e9dddd492d5c24698bcde8c3a1fd942eaf66
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854200"
---
# <a name="migrate-hyper-v-vms-to-azure"></a>Hyper-V VM's migreren naar Azure 

Dit artikel ziet u hoe u on-premises Hyper-V-machines migreren naar Azure, met behulp van de migratie van de zonder Agent met het hulpprogramma voor migratie van Azure-Server migreren.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen, workloads en persoonlijke/openbare cloud, virtuele machines naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

In deze zelfstudie is de derde in een serie die laat zien hoe u om te beoordelen en migreren van Hyper-V naar Azure met behulp van Azure Migrate-Server-evaluatie en migratie. In deze zelfstudie leert u het volgende:


> [!div class="checklist"]
> * Azure en uw on-premises Hyper-V-omgeving voorbereiden
> * De bronomgeving instellen en implementeren van een replicatie-apparaat.
> * De doel-bank instellen...
> * Replicatie inschakelen.
> * Een testmigratie om te controleren of dat alles werkt zoals verwacht worden uitgevoerd.
> * Voer een volledige migratie naar Azure.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

Voordat u aan deze zelfstudie begint, dient u eerst:

1. [Beoordeling](migrate-architecture.md) de architectuur van de migratie van Hyper-V.
2. [Voltooi de eerste zelfstudie](tutorial-prepare-hyper-v.md) in deze reeks het instellen van Azure en Hyper-V voor migratie. In de eerste zelfstudie hebt u:
    - [Azure voorbereiden](tutorial-prepare-hyper-v.md#prepare-azure) voor migratie.
    - [Voorbereiden van de on-premises omgeving](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-migration) voor migratie.
3. Het is raadzaam dat u het beoordelen van Hyper-V-machines met behulp van Azure Migrate-Server-evaluatie probeert, voordat u migreert ze naar Azure. Hiervoor [voltooien van de tweede zelfstudie](tutorial-assess-hyper-v.md) in deze reeks. Hoewel u wordt aangeraden dat u een beoordeling probeert, hebt u geen een evaluatie uitvoeren voordat u virtuele machines migreren.
4. Zorg ervoor dat uw Azure-account de rol Inzender voor virtuele machines is toegewezen zodat u machtigingen hebben voor:

    - Het maken van een VM in de geselecteerde resourcegroep.
    - Het maken van een VM in het geselecteerde virtuele netwerk.
    - Schrijven naar een door Azure beheerde schijf.   
5. [Instellen van een Azure-netwerk](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Wanneer u naar Azure migreert, worden de gemaakte Azure-VM's zijn gekoppeld aan een Azure-netwerk die u opgeeft bij het instellen van de migratie.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Het hulpprogramma voor migratie van Azure Migrate-Server toevoegen

Als u niet de tweede zelfstudie voor het evalueren van Hyper-V-machines uitvoert, moet u [Volg deze instructies](how-to-add-tool-first-time.md) instellen van een Azure Migrate-project en het hulpprogramma voor migratie van Azure Migrate-Server toevoegen aan het project.

Als u de tweede zelfstudie gevolgd en hebt al een Azure Migrate-project instellen, voegt u het hulpprogramma voor migratie van Azure-Server migreren als volgt toe:

1. Klik in de Azure Migrate-project op **overzicht**. 
2. In **ontdekken, beoordelen, en migratie van servers**, klikt u op **evalueren en migreren van servers**.
3. In **hulpprogramma voor migratie van**, selecteer **Klik hier als u wilt toevoegen van een hulpprogramma voor migratie wanneer u klaar bent om**.

    ![Hulpprogramma selecteren](./media/tutorial-migrate-hyper-v/select-migration-tool.png)

4. Selecteer in de lijst met hulpprogramma's, **Azure Migrate: Servermigratie** > **hulpprogramma toevoegen**

    ![Hulpprogramma voor migratie van server](./media/tutorial-migrate-hyper-v/server-migration-tool.png)


## <a name="set-up-the-azure-migrate-appliance"></a>Instellen van het apparaat Azure Migrate

Azure Migrate-Server-migratie wordt uitgevoerd een lichtgewicht Hyper-V-VM-apparaat.

- Het toestel VM-detectie wordt uitgevoerd en verzendt de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure migreren servermigratie.
- Het hetzelfde apparaat wordt ook gebruikt door het hulpprogramma Azure Migrate-Server-evaluatie.

Het apparaat instellen:
- Als u de tweede zelfstudie voor het evalueren van Hyper-V-machines hebt gevolgd, instellen u al van het apparaat tijdens deze zelfstudie.
- Als u niet hebt die zelfstudie volgt, moet u het apparaat is nu ingesteld. Hiervoor u: 

    - Download een gecomprimeerde Hyper-V-VHD vanuit Azure portal.
    - Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan. 
    - Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.

    Volg de instructies in [in dit artikel](how-to-set-up-appliance-hyper-v.md) voor het instellen van het apparaat.

## <a name="prepare-hyper-v-hosts"></a>Hyper-V-hosts voorbereiden

1. In de Azure Migrate-project > **Servers**in **Azure Migrate: Servermigratie**, klikt u op **ontdekken**.
2. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , selecteer **Ja, met Hyper-V**.
3. In **doelregio**, selecteert u de Azure-regio waarnaar u wilt migreren van de machines.
6. Selecteer **bevestigen dat de doelregio voor migratie-regionaam**.
7. Klik op **resources maken**. Hiermee maakt u een Azure Site Recovery-kluis op de achtergrond.
    - Als u al migratie met Azure Migrate-servermigratie hebt ingesteld, wordt deze optie niet weergegeven omdat resources eerder zijn ingesteld.
    - U kunt de doelregio voor dit project niet wijzigen nadat u op deze knop te klikken.
    - Alle toekomstige migraties worden naar deze regio.
    
8. In **voorbereiden Hyper-V-hostservers**, de provider voor Hyper-V-replicatie en het registratiesleutelbestand downloaden.
    - De registratiesleutel is nodig voor het registreren van de Hyper-V-host met de migratie van Azure-Server migreren.
    - De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Provider en de sleutel downloaden](./media/tutorial-migrate-hyper-v/download-provider-hyper-v.png)

4. Kopieer de provider-installatiebestand en registratiesleutelbestand op elke Hyper-V-host (of het clusterknooppunt) uitvoeren van virtuele machines die u wilt repliceren.
5. Voer de provider setup-bestand op elke host, zoals beschreven in de volgende procedure.
6. Na de installatie van de provider op de hosts in **machines detecteren**, klikt u op **registratie voltooien**.

    ![Registratie voltooien](./media/tutorial-migrate-hyper-v/finalize-registration.png)

Duurt tot vijftien minuten na de registratie voltooien totdat gedetecteerde virtuele machines worden weergegeven in Azure Migrate-servermigratie. Als u virtuele machines zijn gedetecteerd, de **servers gedetecteerd** tellen oploopt.

![Gedetecteerde servers](./media/tutorial-migrate-hyper-v/discovered-servers.png)

### <a name="register-hyper-v-hosts"></a>Hyper-V-hosts registreren

Installeer het gedownloade installatiebestand (AzureSiteRecoveryProvider.exe) op elke relevante Hyper-V-host.

1. Uitgevoerd op de provider-installatiebestand op elke host of cluster-knooppunt.
2. In de wizard Setup van Provider > **Microsoft Update**, meldt zich aan voor Microsoft Update gebruiken om te controleren op updates van de Provider.
3. In **installatie**, accepteer de standaardlocatie voor installatie van de Provider en agent en selecteert u **installeren**.
4. Na de installatie, in de Wizard registratie > **Kluisinstellingen**, selecteer **Bladeren**, en in **sleutelbestand**, selecteert u het key vault-bestand dat u hebt gedownload.
5. In **Proxy-instellingen**, opgeven hoe de provider die wordt uitgevoerd op de host verbinding maakt met internet.
    - Als het apparaat zich achter een proxyserver bevindt, moet u de proxy-instellingen opgeven.
    - Geef de naam van de proxyserver als **http://ip-address** , of **http://FQDN** . HTTPS-proxy-servers worden niet ondersteund.
   

6. Zorg ervoor dat de provider kan bereiken het [vereiste URL's](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access).
7. In **registratie**, nadat de host is geregistreerd, klikt u op **voltooien**.

## <a name="replicate-hyper-v-vms"></a>Virtuele Hyper-V-machines repliceren

Met de detectie is voltooid, kunt u beginnen met replicatie van Hyper-V-machines naar Azure.

1. In de Azure Migrate-project > **Servers**, **Azure Migrate: Servermigratie**, klikt u op **repliceren**.
2. In **repliceren**, > **instellingen van de gegevensbron** > **zijn de machines gevirtualiseerd?** , selecteer **Ja, met Hyper-V**. Klik vervolgens op **volgende: Virtuele machines**.
3. In **virtuele machines**, selecteert u de machines die u wilt repliceren.
    - Als u hebt een evaluatie uitgevoerd voor de virtuele machines, kunt u de grootte van de virtuele machine en de schijf type (premium of standaard) aanbevelingen van de resultaten van de evaluatie kunt toepassen. Om dit te doen, in **migratie-instellingen importeren uit een Azure Migrate-evaluatie?** , selecteer de **Ja** optie.
    - Als u een evaluatie niet hebt uitgevoerd, of als u niet wilt gebruiken van de evaluatie-instellingen, selecteert u de **Nee** opties.
    - Als u gebruik van de evaluatie hebt geselecteerd, selecteert u de VM-groep en de naam van de evaluatie.

        ![Beoordeling selecteren](./media/tutorial-migrate-hyper-v/select-assessment.png)

4. In **virtuele machines**, virtuele machines zoeken naar behoefte en controleren van elke virtuele machine die u wilt migreren. Klik vervolgens op **volgende: Instellingen voor doel**.

    ![VM's selecteren](./media/tutorial-migrate-hyper-v/select-vms.png)

5. In **doel instellingen**, selecteert u de doelregio waarnaar u gaat migreren, het abonnement en de resourcegroep waarin de Azure VM's worden geplaatst die na de migratie.
7. In **Replicatieopslagaccount**, selecteer het Azure storage-account waarin de gerepliceerde gegevens worden opgeslagen in Azure.
8. **Virtueel netwerk**, selecteert u het Azure VNet/subnet waarop de Azure VM's na de migratie wordt gekoppeld.
9. In **Azure Hybrid Benefit**:

    - Selecteer **Nee** als u niet wilt dat Azure Hybrid Benefit van toepassing. Klik op **Volgende**.
    - Selecteer **Ja** als u Windows Server-machines die zijn gedekt door een actieve Software Assurance- of Windows Server-abonnementen hebt en u wilt het voordeel toepassen op de computers waarnaar u migreert. Klik op **Volgende**.

    ![Doelinstellingen](./media/tutorial-migrate-hyper-v/target-settings.png)

10. In **Compute**, Controleer de VM-naam, de grootte, de OS-schijftype en de beschikbaarheidsset. Virtuele machines moeten voldoen aan [Azure-vereisten](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM-grootte**: Als u aanbevelingen voor de evaluatie, bevat de VM-grootte vervolgkeuzelijst de aanbevolen grootte. Azure Migrate kiest anders een grootte op basis van het meest overeenkomt met in de Azure-abonnement. U kunt ook kiezen een handmatige grootte in **Azure VM-grootte**. 
    - **Besturingssysteemschijf**: Geef de besturingssysteemschijf (Start) voor de virtuele machine. De besturingssysteemschijf is de schijf met de bootloader van besturingssysteem en het installatieprogramma. 
    - **Beschikbaarheidsset**: Als de virtuele machine moet zich in een Azure-beschikbaarheidsset instellen na de migratie, geeft u de set. De set moet zich in de doelresourcegroep die u voor de migratie opgeeft.

    ![Compute-VM-instellingen](./media/tutorial-migrate-hyper-v/compute-settings.png)

11. In **schijven**, opgeven of de VM-schijven moeten worden gerepliceerd naar Azure en selecteer het schijftype (standaard SSD harde schijf of premium-beheerde schijven) in Azure. Klik op **Volgende**.
    - U kunt schijven uitsluiten van replicatie.
    - Als u schijven uitsluit, zich niet aanwezig zijn op de virtuele Azure-machine na de migratie. 

    ![Disks](./media/tutorial-migrate-hyper-v/disks.png)

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
- Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Incrementele wijzigingen naar on-premises schijven worden regelmatig gerepliceerd naar Azure.

U kunt de status van de taak in de portalmeldingen bijhouden.

U kunt de replicatiestatus controleren door te klikken op **servers repliceren** in **Azure Migrate: Servermigratie**.
![Monitor voor replicatie](./media/tutorial-migrate-hyper-v/replicating-servers.png)



## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren


Wanneer wordt begonnen met replicatie van verschillen, kunt u een testmigratie uitvoeren voor de virtuele machines, voordat u een volledige migratie naar Azure uitvoert. Het is raadzaam dat te ten minste één keer voor elke machine doen voordat u deze migreren.

- Controles zijn uitgevoerd een testmigratie die migratie werkt zoals verwacht, zonder gevolgen voor de on-premises computers die blijven werken, en doorgaan met repliceren. 
- Testmigratie simuleert de migratie door het maken van een Azure-VM met behulp van gerepliceerde gegevens (meestal migreren naar een VNet niet-productie in uw Azure-abonnement).
- U kunt de gerepliceerde test virtuele machine van Azure gebruiken om de migratie valideren, testen van de app uit te voeren en los eventuele problemen op voordat de volledige migratie.

Een testmigratie als volgt:


1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **Test gemigreerd servers**.

     ![Gemigreerde servers testen](./media/tutorial-migrate-hyper-v/test-migrated-servers.png)

2. Met de rechtermuisknop op de virtuele machine om te testen, en klikt u op **Test migreren**.

    ![Migratie testen](./media/tutorial-migrate-hyper-v/test-migrate.png)

3. In **testmigratie**, selecteer het Azure VNet waarin de virtuele machine van Azure geplaatst na de migratie worden. Het is raadzaam om dat een niet-productie-VNet te gebruiken.
4. De **de testmigratie** taak wordt gestart. De taak in de portalmeldingen bewaken.
5. Nadat de migratie is voltooid, geven de gemigreerde Azure-VM in **virtuele Machines** in Azure portal. De machinenaam van de heeft een achtervoegsel **-Test**.
6. Nadat de test is voltooid, met de rechtermuisknop op de Azure-VM in **machines repliceren**, en klikt u op **opschonen van de testmigratie**.

    ![Opschonen van de migratie](./media/tutorial-migrate-hyper-v/clean-up.png)


## <a name="migrate-vms"></a>Virtuele machines migreren

Nadat u hebt gecontroleerd dat de testmigratie werkt zoals verwacht, kunt u de on-premises machines migreren.

1. In de Azure Migrate-project > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **servers repliceren**.

    ![Servers repliceren](./media/tutorial-migrate-hyper-v/replicate-servers.png)

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
