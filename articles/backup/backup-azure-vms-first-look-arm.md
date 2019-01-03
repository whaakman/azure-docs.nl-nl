---
title: Back-up van virtuele Azure-machines met de Azure Backup-service
description: Meer informatie over het back-up van virtuele Azure-machines met de Azure Backup-service
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: 0c394a92bff3ace210ee0db156f47bb8912bf45d
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631580"
---
# <a name="back-up-azure-vms-with-the-azure-backup-service"></a>Back-up van virtuele Azure-machines met de Azure Backup-service

In dit artikel wordt uitgelegd hoe u de beveiliging van een virtuele machine van de virtuele machines operations menu of de Recovery Services-kluis configureren. Recovery Services-kluizen beveiligen:

* VM's die zijn geïmplementeerd met Azure Resource Manager
* Klassieke VM's
* Standaardopslag-VM's
* Premium Storage-VM's
* Virtuele machines die op beheerde schijven worden uitgevoerd
* Virtuele machines die zijn versleuteld met Azure Disk Encryption
* Toepassingsconsistente back-up van Windows-VM's met behulp van VSS- en Linux-VM's en aangepaste scripts met momentopnamen vooraf en achteraf

Zie het artikel [Back-ups van virtuele machines voor Premium Storage maken en terugzetten](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) voor meer informatie over het beschermen van Premium Storage-VM's. Zie [Back-ups van virtuele machines op beheerde schijven maken en terugzetten](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup) voor meer informatie over ondersteuning voor virtuele machines op beheerde schijven. Voor meer informatie over pre- en post-script framework voor Linux-VM back-ups raadpleegt u [Application consistent Linux VM backup using pre-script and post-script](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) (Toepassingsconsistente back-up van Linux-VM's met pre-script en post-script).

Zie voor meer informatie over wat u wel en niet kan back-up maken, [uw omgeving voorbereiden op back-up van virtuele Azure-machines](backup-azure-arm-vms-prepare.md#before-you-start).

> [!NOTE]
> Backup-service maakt een afzonderlijke resourcegroep dan de resourcegroep van de virtuele machine voor het opslaan van de verzameling van herstelpunt. Klanten wordt aangeraden niet aan het vergrendelen van de resourcegroep gemaakt voor gebruik door de Backup-service.
De naamgeving indeling van de resourcegroep hebt gemaakt met Backup-service is: AzureBackupRG_`<Geo>`_`<number>`
<br>Bijvoorbeeld: AzureBackupRG_northeurope_1
>
>

Afhankelijk van het aantal virtuele machines dat u wilt beveiligen, kunt u vanaf verschillende beginpunten starten. Als u in één bewerking een back-up wilt maken van meerdere virtuele machines, gaat u naar de Recovery Services-kluis en [start u de back-uptaak vanuit het dashboard van de kluis](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Als u wilt back-up van een enkele virtuele machine, [starten van de back-uptaak in VM operations menu](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu).

## <a name="configure-the-backup-job-from-the-vm-operations-menu"></a>De back-uptaak in het menu van de bewerkingen VM configureren

Gebruik de volgende stappen uit om te configureren van de back-uptaak in het menu van de bewerkingen virtuele Machine. De stappen gelden alleen voor virtuele machines in Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Alle services** en typ **Virtuele machines** in het dialoogvenster Filter. Terwijl u typt, wordt de lijst met resources gefilterd. Wanneer u Virtuele machines ziet, selecteert u deze optie.

  ![Schermopname die laat zien hoe u vanuit Alle services naar virtuele machines navigeert](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  De lijst met virtuele machines (VM) in het abonnement wordt weergegeven.

  ![De lijst met virtuele machines in het abonnement wordt weergegeven.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken.

  ![De lijst met virtuele machines in het abonnement wordt weergegeven.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  Wanneer u de virtuele machine selecteert, wordt de lijst met verschuivingen voor virtuele machines aan de linkerkant en het menu van de virtuele machine beheer en het dashboard virtuele machine geopend.

4. In het menu van het beheer van virtuele machine in de **Operations** sectie, klikt u op **back-up**. </br>

  ![Back-upoptie op VM-management-menu](./media/backup-azure-vms-first-look-arm/vm-management-menu.png)

  Het menu voor de back-up inschakelen wordt geopend.

  ![Back-upoptie op VM-management-menu](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup.png)

5. Klik in het gebied van Recovery Services-kluis, **Selecteer een bestaande** en kiest u een kluis in de vervolgkeuzelijst.

  ![Wizard Back-up inschakelen](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

  Als er geen Recovery Services-kluizen zijn of als u een nieuw kluis wilt gebruiken, klikt u op **Nieuwe maken** en geeft u de naam op voor de nieuwe kluis. Er wordt een nieuwe kluis in dezelfde resourcegroep en in dezelfde regio als de virtuele machine gemaakt. Als u een Recovery Services-kluis met verschillende waarden wilt maken, raadpleeg dan de sectie over [het maken van een Recovery Services-kluis](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm).

6. Selecteer een beleid in het menu van de back-upbeleid kiezen. De details voor het geselecteerde beleid wordt weergegeven onder de vervolgkeuzelijst.

  Als u wilt een nieuw beleid maken of bewerken van het bestaande beleid, klikt u op **maken (of bewerken) een nieuw beleid** om de back-up beleid-editor te openen. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid. Sla de wijzigingen in de back-upbeleid en terugkeren naar de back-up inschakelen-menu, klikt u op **OK**.

  ![Back-upbeleid selecteren](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Als u wilt de Recovery Services-kluis en back-up beleid toepassen op de virtuele machine, klikt u op **back-up inschakelen** het beleid te implementeren. Als u het beleid implementeert, koppelt u het aan de kluis en de virtuele machines.

  ![Knop Backup inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. U kunt de voortgang van de configuratie volgen via de meldingen die in de portal worden weergegeven. In het volgende voorbeeld ziet u dat de implementatie is gestart.

  ![Back-upmeldingen inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Nadat de voortgang van de configuratie is voltooid, klikt u op de virtuele machine management-menu, klikt u op **back-up** opent u het menu back-up en de beschikbare details weergeven.

  ![Weergave Back-upitem VM](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

  Zolang de eerste back-up nog niet is voltooid, wordt voor **Status van de laatste back-up** de tekst **Waarschuwing (eerste back-up in behandeling)** weergegeven. Om te zien wanneer de volgende geplande back-uptaak zich voordoet, onder **samenvatting** klikt u op de naam van het beleid. Het menu back-upbeleid wordt geopend en toont de tijd van de geplande back-up.

10. Ter bescherming van de virtuele machine, klikt u op **back-up nu**.

  ![Op Nu back-up maken klikken om de eerste back-up uit te voeren](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

  Het menu Nu back-up maken wordt geopend.

  ![toont de blade Nu back-up maken](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Klik op het pictogram van de kalender in het menu nu back-up, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag dit herstelpunt wordt bewaard, en klikt u op **OK**.

  ![de laatste dag instellen dat het herstelpunt van Nu back-up maken wordt bewaard](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Implementatiemeldingen laten u weten dat de back-uptaak is geactiveerd en dat u de voortgang van de taak op de pagina Back-uptaken kunt controleren.

## <a name="configure-the-backup-job-from-the-recovery-services-vault"></a>De back-uptaak configureren vanuit de Recovery Services-kluis
Als u de back-uptaak wilt configureren, moet u de volgende stappen voltooien.

1. Maak een Recovery Services-kluis voor een virtuele machine.
2. Gebruik Azure Portal om een scenario te selecteren, een beleid voor back-ups in te stellen en te bepalen welke items er moeten worden beveiligd.
3. De eerste back-up uitvoeren.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Een Recovery Services-kluis voor een VM maken
Een Recovery Services-kluis is een entiteit waarmee alle back-ups en herstelpunten worden opgeslagen die in de loop van de tijd zijn gemaakt. De Recovery Services-kluis bevat ook het back-upbeleid dat wordt toegepast op de beveiligde VM's.

> [!NOTE]
> Het maken van back-ups van VM's is een lokaal proces. Het is niet mogelijk van VM's in de ene regio een back-up in een Recovery Services-kluis in een andere regio te maken. Daarom moet er voor elke Azure-regio met VM's waarvoor u een back-up wilt maken ten minste één Recovery Services-kluis in die regio bestaan.
>
>

Een Recovery Services-kluis maken:

1. Meld u met uw Azure-abonnement aan bij [Azure Portal](https://portal.azure.com/) als u dit nog niet hebt gedaan.
2. Klik in het menu Hub op **Alle services** en typ **Recovery Services** in het dialoogvenster Filter. Terwijl u typt, wordt de lijst met resources gefilterd. Wanneer Recovery Services-kluizen in de lijst wordt weergegeven, klikt u erop.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Als er Recovery Services-kluizen in het abonnement aanwezig zijn, worden deze weergegeven.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    De Recovery Services vault menu geopend, waarin wordt gevraagd u bieden een **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![Een Recovery Services-kluis maken, stap 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die tussen 2 en 50 tekens bevat. De naam moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.

5. Kies het Azure-abonnement in de vervolgkeuzelijst in het gedeelte **Abonnement**. Als u slechts één abonnement hebt, wordt dit weergegeven en kunt u doorgaan met de volgende stap. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaard- (of voorgestelde) abonnement. Er zijn alleen meerdere mogelijkheden als uw organisatieaccount is gekoppeld aan meerdere Azure-abonnementen.

6. In het gedeelte **Resourcegroep**:

    * selecteert u **Nieuw** als u een resourcegroep wilt maken,
    of
    * selecteert u **Bestaande gebruiken** en klikt u op de vervolgkeuzelijst om de lijst met beschikbare resourcegroepen te zien.

  Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.

7. Klik op **Locatie** om de geografische regio voor de kluis te selecteren. Deze keuze bepaalt de geografische regio waar uw back-upgegevens naartoe worden verzonden.

  > [!IMPORTANT]
  > Als u niet zeker weet in welke regio uw VM zich bevindt, sluit u het dialoogvenster voor het maken van de kluis en gaat u naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u in elke regio een Recovery Services-kluis maken. Maak de kluis in de eerste regio voordat u verdergaat met de volgende regio. U hoeft geen opslagaccounts voor het opslaan van de back-upgegevens op te geven. Dit wordt automatisch door de Recovery Services-kluis en de Azure Backup-service afgehandeld.
  >

8. Aan de onderkant van het menu Recovery Services-kluis, klikt u op **maken**.

    Het kan enkele minuten duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal. Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis na enkele minuten niet ziet, klik dan op **Vernieuwen**.

    ![Op de knop Vernieuwen klikken](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Wanneer u uw kluis in de lijst met Recovery Services-kluizen ziet, kunt u de opslagredundantie instellen.

Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Als de Recovery Services-kluis uw primaire back-up is, laat u de opslagoptie voor replicatie ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie wilt die minder duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Uit de **Recovery Services-kluizen** in het menu selecteert u de nieuwe kluis.

  ![De nieuwe kluis in de lijst met Recovery Services-kluizen selecteren](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Wanneer u de kluis, het menu instellingen selecteert (*waarvan de naam van de kluis is aan de bovenkant*) en open het kluisdashboard.

  ![De opslagconfiguratie voor nieuwe kluis bekijken](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-update.png)

2. In het menu voor het beheer van de nieuwe kluis gebruikt u de verticale dia bladert u omlaag naar het gedeelte beheer en klikt u op **back-upinfrastructuur** om het menu back-upinfrastructuur te openen.

   ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/set-storage-config-bkup-infra.png)

3. Klik in het menu back-upinfrastructuur **back-upconfiguratie** openen de **back-upconfiguratie** menu.

    ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/set-storage-open-infra.png)
4. Kies het type opslagreplicatie dat geschikt is voor uw kluis.

    ![keuzes bij opslagconfiguratie](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Uw kluis heeft standaard geografisch redundante opslag. Als Azure uw primaire eindpunt is voor back-upopslag, blijf dan **Geografisch redundant** gebruiken. Als Azure niet uw primaire eindpunt is voor back-upopslag, kiest u **Lokaal redundant**, zodat u de kosten voor Azure-opslag verlaagt. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante ](../storage/common/storage-redundancy-lrs.md) opslag in dit [overzicht van opslagredundantie](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren welke items moeten worden beveiligd
Voordat u een VM met een kluis registreert, voert u het detectieproces uit om ervoor te zorgen dat nieuwe virtuele machines die zijn toegevoegd aan het abonnement, worden geïdentificeerd. Er wordt een query uitgevoerd om de virtuele Azure-machines in het abonnement weer te geven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio. In Azure Portal wordt met scenario verwezen naar wat u in de Recovery Services-kluis wilt opnemen. Het beleid is de planning voor hoe vaak en wanneer er herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Klik anders op **Alle services**. Typ **Recovery Services** en klik op **Recovery Services-kluizen**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.

    ![Weergave van de lijst met Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Selecteer een kluis in de lijst met Recovery Services-kluizen om het dashboard ervan te openen.

     ![Het menu Kluis openen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Klik in het menu van het kluisdashboard op **Back-up** om het menu Back-up te openen.

    ![Menu in de back-up](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Het menu back-ups en back-updoel openen.

    ![Het menu Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. In het menu doel van de back-up van de **waar wordt uw werkbelasting uitgevoerd** vervolgkeuzelijst, kiezen voor Azure. Kies Virtuele machine in de vervolgkeuzelijst **Waarvan wilt u een back-up maken** en klik op **OK**.

    Hiermee wordt de VM-extensie bij de kluis geregistreerd. Het menu back-updoel wordt gesloten en de **back-upbeleid** menu wordt geopend.

    ![Het menu Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Selecteer op het menu back-upbeleid het back-upbeleid dat u wilt toepassen op de kluis.

    ![Back-upbeleid selecteren](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven. Als u een beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid.
    Klik op **OK** om het back-upbeleid aan de kluis te koppelen.

    Het menu back-upbeleid wordt gesloten en de **virtuele machines selecteren** menu wordt geopend.
5. In de **virtuele machines selecteren** menu, kiest u de virtuele machines om te koppelen aan het opgegeven beleid en klikt u op **OK**.

    ![Workload selecteren](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    De geselecteerde virtuele machine wordt gevalideerd. Als u de verwachte virtuele machines niet ziet, controleert u of ze bestaan op dezelfde Azure-locatie als de Recovery Services-kluis en controleert u of ze al worden beveiligd. De locatie van de Recovery Services-kluis wordt weergegeven op het kluisdashboard.

6. Nu dat u alle instellingen voor de kluis zijn gedefinieerd in het menu back-up, klikt u op **back-up inschakelen** het beleid voor de kluis en de VM's implementeren. Met het implementeren van het beleid wordt niet het eerste herstelpunt voor de virtuele machine gemaakt.

    ![Back-up inschakelen](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nadat de back-up is ingeschakeld, wordt het back-upbeleid volgens het schema uitgevoerd. Ga echter verder om de eerste back-uptaak te starten.

## <a name="initial-backup"></a>Eerste back-up
Als u een back-upbeleid op de virtuele machine hebt geïmplementeerd, betekent dit niet dat er een back-up van de gegevens is gemaakt. De eerste geplande back-up (zoals gedefinieerd in het back-upbeleid) is standaard de eerste back-up. Totdat de initiële back-up plaatsvindt, de Status van de laatste back-up op de **back-uptaken** menu wordt weergegeven als **waarschuwing (eerste back-up in behandeling)**.

![Back-up in behandeling](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Tenzij de eerste back-up binnenkort wordt gemaakt, wordt aanbevolen dat u **Nu een back-up maken** uitvoert.

De eerste back-uptaak uitvoeren:

1. Klik op het kluisdashboard op het getal onder **Back-upitems** of klik op de tegel **Back-upitems**. <br/>
  ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Het menu **Back-upitems** wordt geopend.

  ![Back-upitems](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Op de **back-Upitems** in het menu selecteert u het item.

  ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  De lijst **Back-upitems** wordt geopend. <br/>

  ![Geactiveerde back-uptaak](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Klik in de lijst **Back-upitems** op het weglatingsteken **...**  om het contextmenu te openen.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Het contextmenu wordt weergegeven.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klik in het contextmenu op **Nu back-up maken**.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Het menu Nu back-up maken wordt geopend.

  ![ziet u het menu nu back-up](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Klik op het pictogram van de kalender in het menu nu back-up, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag dit herstelpunt wordt bewaard, en klikt u op **back-up**.

  ![de laatste dag instellen dat het herstelpunt van Nu back-up maken wordt bewaard](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Implementatiemeldingen laten u weten dat de back-uptaak is geactiveerd en dat u de voortgang van de taak op de pagina Back-uptaken kunt controleren. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.

  > [!NOTE]
  > Alle de gegevens die worden ondersteund door Azure Backup is versleuteld in rust via [Storage Service Encryption (SSE)](../storage/common/storage-service-encryption.md).
  >
  >

6. Als u de status van de eerste back-up wilt bekijken of volgen, klikt u op de tegel **Back-uptaken** van het kluisdashboard op **Bezig**.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Het menu back-uptaken wordt geopend.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  In de **back-uptaken** menu, kunt u de status van alle taken bekijken. Controleer of de back-uptaak voor de virtuele machine nog steeds bezig is of is voltooid. Wanneer de back-uptaak is voltooid, verandert de status in *Voltooid*.

  > [!NOTE]
  > Als onderdeel van de back-upbewerking wordt met de Azure Backup-service aan de back-upextensie in elke VM opdracht gegeven om alle schrijfbewerkingen leeg te maken en een consistente momentopname te maken.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent op de virtuele machine installeren
Deze informatie wordt verstrekt voor het geval u deze nodig hebt. De Azure VM-agent moet worden geïnstalleerd op de virtuele Azure-machine om de Backup-extensie te kunnen gebruiken. Als uw VM echter is gemaakt vanuit de Azure-galerie, is de VM-agent al aanwezig op de virtuele machine. Op VM's die zijn gemigreerd vanuit on-premises datacenters, is geen VM-agent geïnstalleerd. In dat geval moet de VM-agent worden geïnstalleerd. Als u problemen ondervindt bij het maken van een back-up van de virtuele Azure-machine, controleert u of de Azure VM-agent correct is geïnstalleerd op de virtuele machine (zie de volgende tabel). Als u een aangepaste VM maakt, installeert u de VM-agent voordat de virtuele machine wordt ingericht.

Meer informatie over de [VM-agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) en [hoe u deze installeert](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

De volgende tabel bevat aanvullende informatie over de VM-agent voor Windows- en Linux-VM's.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |<li>Download en installeer de [agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li>[Werk de VM-eigenschap bij](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |<li> Installeer de meest recente [Linux-agent](https://github.com/Azure/WALinuxAgent) vanuit GitHub. U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li> [Werk de VM-eigenschap bij](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N/A |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent op de virtuele machine is geïnstalleerd, installeert de Azure Backup-service de Backup-extensie in de VM-agent. De Azure Backup-service wordt probleemloos bijgewerkt en er wordt zonder tussenkomst van de gebruiker een patch voor de Backup-extensie uitgevoerd.

De Backup-service installeert de back-upextensie, zelfs als de virtuele machine niet wordt uitgevoerd. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Azure Backup-service blijft echter back-ups van de VM maken, zelfs als deze is uitgeschakeld en de extensie niet kan worden geïnstalleerd. Dit type back-up wordt aangeduid als Offline VM en het herstelpunt is *consistent vastlopen*.

## <a name="troubleshooting-information"></a>Informatie over probleemoplossing
Als u problemen hebt bij het uitvoeren van de taken in dit artikel, raadpleegt u de [richtlijnen voor probleemoplossing](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Prijzen
De kosten voor het maken van back-ups van virtuele Azure-machines is gebaseerd op het aantal beveiligde exemplaren. Zie voor een definitie van een beveiligd exemplaar [Wat is een beveiligd exemplaar?](backup-introduction-to-azure-backup.md#what-is-a-protected-instance) Zie de pagina met prijzen van Azure Backup voor meer informatie over [de prijzen van Backup](https://azure.microsoft.com/pricing/details/backup/).

## <a name="next-steps"></a>Volgende stappen

[Beheren](backup-azure-manage-vms.md) uw back-ups.

