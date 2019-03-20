---
title: Back-up van Windows-machines met de Azure Backup MARS-agent
description: Gebruik de Azure Backup Microsoft Recovery Services agent (MARS) naar de back-up van Windows-machines.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: 7a1bd6da68b49481429709c7e4fd37dd5c07ae2c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200783"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Back-up van Windows-machines met de Azure Backup MARS-agent

In dit artikel wordt uitgelegd hoe u back-up van Windows-machines met behulp van de [Azure Backup](backup-overview.md) -service en de agent van de Microsoft Azure Recovery Services (MARS), ook wel bekend als de Azure Backup-agent.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Controleer de vereisten en maak een Recovery Services-kluis.
> * De MARS-agent downloaden en instellen
> * Maak een back-upbeleid en planning.
> * Een ad-hoc back-up uitvoeren.

## <a name="about-the-mars-agent"></a>Over de MARS-agent

De MARS-agent wordt gebruikt door Azure Backup back-up van bestanden, mappen en de systeemstatus van on-premises machines en virtuele Azure-machines naar een back-Recovery Services-kluis in Azure. U kunt de agent als volgt uitvoeren:

- De agent rechtstreeks op de on-premises Windows-computers uitvoeren, zodat ze kunnen back-up rechtstreeks naar een back-Recovery Services-kluis in Azure.
- De agent Azure VM's met Windows (side-by-side met de virtuele machine van Azure Backup-extensie) naar het back-up van bepaalde bestanden en mappen op de virtuele machine worden uitgevoerd.
- Voer de agent op een Microsoft Azure Backup-Server (MABS) of een System Center Data Protection - server Manager (DPM). In dit scenario, computers en werkbelastingen back-up naar MABS/DPM en vervolgens MABS/DPM een back-up van een kluis in Azure met behulp van de MARS-agent.
Wat u kunt back-up, is afhankelijk van de waarop de agent is geïnstalleerd.

> [!NOTE]
> Er is de primaire methode voor het back-ups van virtuele Azure-machines met behulp van een Azure Backup-extensie op de virtuele machine. Dit back-ups van de hele virtuele machine. Het is raadzaam om te installeren en gebruiken van de MARS-agent samen met de extensie als u wilt back-up van bepaalde bestanden en mappen op de virtuele machine. [Meer informatie](backup-architecture.md#architecture-direct-backup-of-azure-vms).

![Back-upproces stappen](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Voordat u begint

- [Informatie over hoe](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders) Azure Backup back-ups van Windows-machines met de MARS-agent.
- [Meer informatie over](backup-architecture.md#architecture-back-up-to-dpmmabs) de back-architectuur van de MARS-agent die wordt uitgevoerd op een secundaire MABS of DPM-server.
- [Beoordeling](backup-support-matrix-mars-agent.md) wat wordt ondersteund en wat kan een back-up met de MARS-agent.
- Controleer of u toegang tot internet op de machines die u back wilt-up.
- Als u wilt back-up van een server of client naar Azure, moet u een Azure-account. Als u niet hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten.

### <a name="verify-internet-access"></a>Controleer of u toegang tot internet

Als uw computer toegang tot internet beperkt heeft, zorgt u ervoor dat firewall-instellingen op de machine of de proxy toestaan deze URL's en IP-adres:

**URL 's**

- www\.msftncsi.com
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

**IP-adres**

- 20.190.128.0/18
- 40.126.0.0/18


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een Recovery Services-kluis alle back-ups en herstelpunten die u na verloop van tijd maakt worden opgeslagen en bevat de back-upbeleid dat wordt toegepast op back-ups van virtuele machines. Maak een kluis als volgt:

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/) met behulp van uw Azure-abonnement.
2. In het zoekvak, typ **herstelservices** en klikt u op **Recovery Services-kluizen**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png)

3. Op de **Recovery Services-kluizen** menu, klikt u op **+ toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden.

   - De naam moet uniek zijn voor het Azure-abonnement.
   - 2 en 50 tekens kan bevatten.
   - Deze moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.

5. Selecteer de Azure-abonnement, resourcegroep en geografische regio waarin de kluis moet worden gemaakt. Back-upgegevens wordt verzonden naar de kluis. Klik vervolgens op **Maken**.

    ![Een Recovery Services-kluis maken, stap 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   - Het kan even duren voor de kluis is gemaakt.
   - Houd de statusmeldingen in de rechterbovenhoek van de portal. Als u na enkele minuten kunt u de kluis niet ziet, klikt u op **vernieuwen**.

     ![Op de knop Vernieuwen klikken](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Opslagredundantie instellen

Azure Backup verwerkt automatisch de opslag voor de kluis. U moet opgeven hoe die storage wordt gerepliceerd.

1. Klik op de blade **Recovery Services-kluizen** op de nieuwe kluis. Onder de **instellingen** sectie, klikt u op **eigenschappen**.
2. In **eigenschappen**onder **back-upconfiguratie**, klikt u op **Update**.

3. Selecteer het opslagtype voor replicatie en klikt u op **opslaan**.

      ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

- Het is raadzaam dat als u Azure als een primaire back-upopslag-eindpunt, echter ook doorgaan met de standaard **geografisch redundante** instelling.
- Als Azure niet uw primaire eindpunt is voor back-upopslag, kiest u **Lokaal redundant**, zodat u de kosten voor Azure-opslag verlaagt.
- Meer informatie over [geo](../storage/common/storage-redundancy-grs.md) en [lokale](../storage/common/storage-redundancy-lrs.md) redundantie.

## <a name="download-the-mars-agent"></a>De MARS-agent downloaden

Download de MARS-agent voor installatie op computers die u back wilt-up.

- Als u de agent al hebt geïnstalleerd op computers, zorg er dan voor dat u de meest recente versie uitvoert.
- De meest recente versie is beschikbaar in de portal of via een [directe download](https://aka.ms/azurebackup_agent)

1. In de kluis onder **aan de slag**, klikt u op **back-up**.

    ![Open de blade back-updoelstelling](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. In **waar wordt uw werkbelasting uitgevoerd?**, selecteer **On-premises**. U moet deze optie selecteert, zelfs als u wilt de MARS-agent installeren op een Azure-VM.
3. In **waarvan wilt u back-up maken?**, selecteer **bestanden en mappen** en/of **systeemstatus**. Er zijn een aantal andere opties beschikbaar, maar ze worden alleen ondersteund als u een secundaire server back-up uitvoert. Klik op **infrastructuur voorbereiden**.

      ![Bestanden en mappen configureren](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. Op de **infrastructuur voorbereiden**onder **installeren Recovery Services-agent**, de MARS-agent downloaden.

    ![infrastructuur voorbereiden](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. Klik in het downloadpop-upvenster op **Opslaan**. Standaard wordt het bestand **MARSagentinstaller.exe** opgeslagen in de map Downloads.

6. Controleer nu **al gedownload of met behulp van de meest recente Recovery Services-Agent**, en download de kluisreferenties.

    ![kluisreferenties downloaden](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. Klik op **Opslaan**. Het bestand wordt gedownload naar uw downloadmap. U kunt het bestand met kluisreferenties niet openen.

## <a name="install-and-register-the-agent"></a>De agent installeren en registreren

1. Voer de **MARSagentinstaller.exe** -bestand op computers die u back wilt-up.
2. In de Wizard Setup van MARS Agent > **installatie-instellingen**, Geef op waar u wilt installeren van de agent en een locatie moet worden gebruikt voor de cache. Klik op **Volgende**.
   - Azure Backup maakt gebruik van de cache voor het opslaan van gegevens momentopnamen voordat ze worden verzonden naar Azure.
   - Locatie van de cache moet ruimte ten minste 5% van de grootte van de gegevens die u moet back-up hebben.

     ![MARS wizard Installatie-instellingen](./media/backup-configure-vault/mars1.png)

2. In **proxyconfiguratie**, opgeven hoe de agent wordt uitgevoerd op de Windows-machine maakt verbinding met het internet. Klik op **Volgende**.

   - Als u een aangepaste proxy opgeven de proxy-instellingen en de referenties indien nodig.
   - Houd er rekening mee dat de agent toegang hebben tot moet [deze URL's](#verify-internet-access).

     ![Toegang tot internet MARS-wizard](./media/backup-configure-vault/mars2.png)

3. In **installatie** controleert u de controle van vereisten en op **installeren**.
4. Nadat de agent is geïnstalleerd, klikt u op **doorgaan naar registratie**.
5. In de **Wizard Server registreren** > **kluis-id**, bladeren en selecteert u de referenties voor dat u hebt gedownload. Klik op **Volgende**.

    ![Register - kluisreferenties](./media/backup-configure-vault/register1.png)

6. In **Versleutelingsinstelling**, Geef een wachtwoordzin die wordt gebruikt voor het versleutelen en ontsleutelen van back-ups voor de machine.

    - De wachtwoordzin voor de versleuteling op een veilige locatie opslaan.
    - Als u verloren gaan of de wachtwoordzin vergeet, kan geen Microsoft helpen de back-upgegevens te herstellen. Sla het bestand daarom op een veilige locatie op. U moet een back-up te herstellen.

7. Klik op **voltooien**. De agent wordt nu geïnstalleerd en uw computer wordt geregistreerd bij de kluis. U kunt nu uw back-up configureren en plannen.

## <a name="create-a-backup-policy"></a>Maak een back-upbeleid

Wanneer u momentopnamen maken van de gegevens voor het maken van herstelpunten, Hiermee geeft u het back-upbeleid en hoe lang herstelpunten.

- U configureren een back-upbeleid met behulp van de MARS-agent.
- Azure Backup wordt niet automatisch rekening zomer-en wintertijd (Zomertijd). Hierdoor kunnen sommige verschil tussen de werkelijke tijd en de geplande back-uptijd.

Maak een beleid als volgt:

1. Open de MARS-agent op elke machine. U vindt deze door te zoeken naar **Microsoft Azure Backup** op uw machine.
2. In **acties**, klikt u op **back-up plannen**.

    ![Een back-up van de Windows Server plannen](./media/backup-configure-vault/schedule-first-backup.png)

3. In de Wizard van de back-up plannen > **aan de slag**, klikt u op **volgende**.
4. In **Items selecteren voor back-up**, klikt u op **Items toevoegen**.
5. In **Items selecteren**, selecteert u wat u wilt back-up. Klik vervolgens op **OK**.
6. In **Items selecteren voor back-up** pagina, klikt u op **volgende**.
7. In **back-upschema opgeven** pagina, opgeven als u wilt uitvoeren dagelijks of wekelijks back-ups. Klik op **Volgende**.

    - Een herstelpunt wordt gemaakt wanneer er een back-up wordt gemaakt.
    - Het aantal herstelpunten die zijn gemaakt in uw omgeving is afhankelijk van uw back-upschema.

1. U kunt dagelijkse back-ups, maximaal drie keer per dag plannen. De schermafbeelding ziet u bijvoorbeeld twee dagelijkse back-ups, één om middernacht en één op 18: 00 uur.

    ![Dagelijks schema](./media/backup-configure-vault/day-schedule.png)

9. U kunt de wekelijkse back-ups te starten. De schermafbeelding ziet u bijvoorbeeld back-ups elke alternatieve zondag & woensdag genomen om 9:30 uur en 1:00 uur.

    ![Wekelijks schema](./media/backup-configure-vault/week-schedule.png)

8. Op de **retentiebeleid selecteren** pagina, opgeven hoe het opslaan van historische kopieën van uw gegevens. Klik op **Volgende**.

   - Instellingen voor het bewaren opgeven welke herstelpunten moeten worden opgeslagen en hoe lang ze moeten worden opgeslagen voor.
   - Bijvoorbeeld, als u een dagelijkse instelling voor de bewaarperiode instelt, geeft u aan dat op het moment dat is opgegeven voor de bewaarperiode voor dagelijkse, het meest recente herstelpunt wordt bewaard voor het opgegeven aantal dagen. Of, als een ander voorbeeld kan geeft u een maandelijks bewaarbeleid om aan te geven dat het herstelpunt dat is gemaakt op de 30 van elke maand gedurende 12 maanden moet worden opgeslagen.
   - Dagelijkse en wekelijkse bewaarperiode van het herstelpunt is meestal valt samen met de back-upschema. Wat betekent dat wanneer de back-up wordt geactiveerd op basis van planning, het herstelpunt dat door de back-up gemaakt voor de duur die zijn aangegeven in de dagelijks of wekelijks bewaarbeleid is opgeslagen.
   - Een voorbeeld: op de volgende schermafbeelding:
     - Dagelijkse back-ups op middernacht en 18: 00 uur worden bewaard gedurende zeven dagen.
     - Back-ups die op een zaterdag om middernacht en 18: 00 uur worden bewaard gedurende 4 weken.
     - Back-ups die op zaterdag op de laatste week van de maand vanaf middernacht en 18: 00 uur worden bewaard gedurende 12 maanden. -Back-ups die op zaterdag in de laatste week van maart worden bewaard gedurende tien jaar.

   ![Retentie-voorbeeld](./media/backup-configure-vault/retention-example.png)

11. In **eerste back-uptype kiezen** opgeven hoe de eerste back-up uitvoeren via het netwerk of offline. Klik op **Volgende**.

10. In **bevestiging**, lees de informatie en klik vervolgens op **voltooien**.
11. Nadat u de wizard voor het maken van een back-upschema hebt doorlopen, klikt u op **Sluiten**.

### <a name="perform-the-initial-backup-offline"></a>De eerste back-up offline uitvoeren

U kunt een eerste back-up automatisch uitgevoerd via het netwerk of offline. Offline-seeding voor een eerste back-up is handig als u grote hoeveelheden gegevens waarvoor veel van de netwerkbandbreedte om over te dragen. U doen een offline-overdracht als volgt:

1. U kunt de back-upgegevens schrijven naar een tijdelijke locatie.
2. Het hulpprogramma AzureOfflineBackupDiskPrep kunt u de gegevens van de faseringslocatie kopiëren naar een of meer SATA-schijven.
3. Het hulpprogramma maakt een Azure Import-taak. [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) over Azure importeren en exporteren.
4. U verzendt de SATA-schijven naar een Azure-datacenter.
5. In het datacenter, wordt de schijfgegevens gekopieerd naar een Azure storage-account.
6. Azure Backup kunt u de gegevens van het opslagaccount gekopieerd naar de kluis en incrementele back-ups zijn gepland.

[Meer informatie](backup-azure-backup-import-export.md) over offline-seeding.

### <a name="enable-network-throttling"></a>Netwerkbeperking inschakelen

U kunt bepalen hoe netwerkbandbreedte wordt gebruikt door de MARS-agent door in te schakelen netwerkbeperking. Beperking is handig als u wilt back-up van gegevens tijdens werkuren, maar wilt bepalen hoeveel bandbreedte wordt gebruikt voor back-up en herstellen van de activiteit.

- Azure back-up netwerk gebruik bandbreedtebeperking [Quality of Service (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) op het lokale besturingssysteem.
- Netwerkbeperking voor back-up is beschikbaar op Windows Server 2008 R2 en hoger en Windows 7 en hoger. Besturingssystemen, moet de nieuwste servicepacks worden uitgevoerd.

Netwerkbeperking als volgt inschakelen:

1. Klik in de MARS-agent op **eigenschappen wijzigen**.
2. Op de **beperking** tabblad controle **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**.

    ![Netwerkbeperking](./media/backup-configure-vault/throttling-dialog.png)
3. Geef de bandbreedte die is toegestaan tijdens en buiten werkuren. Bandbreedtewaarden begint in 512 Kbps en gaat u tot 1023 MBps. Klik vervolgens op **OK**.

## <a name="run-an-ad-hoc-backup"></a>Een ad-hoc back-up uitvoeren

1. Klik in de MARS-agent op **nu een Back-Up maken**. Dit begint de eerste replicatie via het netwerk.

    ![Nu back-up maken van Windows Server](./media/backup-configure-vault/backup-now.png)

2. In **bevestiging**, Controleer de instellingen en klik op **Back-Up**.
3. Klik op **Sluiten** om de wizard te sluiten. Als u dit doet voordat de back-up is voltooid, blijft de wizard om uit te voeren op de achtergrond.

Nadat de eerste back-up is voltooid, wordt de status **Taak voltooid** weergegeven in de back-upconsole.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het](backup-azure-restore-windows-server.md) bestanden herstellen.
