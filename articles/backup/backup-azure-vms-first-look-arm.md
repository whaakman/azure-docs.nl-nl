---
title: 'Eerste blik: virtuele machines van Azure beveiligen met een Recovery Services-kluis | Microsoft Docs'
description: "Beveilig virtuele machines van Azure met een Recovery Services-kluis. Gebruik back-ups van virtuele machines die met Resource Manager of het klassieke model zijn geïmplementeerd, Premium Storage-VM's, versleutelde virtuele machines en virtuele machines op beheerde schijven om uw gegevens te beveiligen. Maak en registreer een Recovery Services-kluis. Registreer VM's, maak beleid en beveilig VM's in Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/04/2017
ms.author: markgal;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 639f008eea61b973b9d32dc734d42d5c4e93e924
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-to-recovery-services-vaults"></a>Back-ups maken van virtuele Azure-machines naar Recovery Services-kluizen
> [!div class="op_single_selector"]
> * [Virtuele machines beveiligen met een Recovery Services-kluis](backup-azure-vms-first-look-arm.md)
> * [Virtuele machines beveiligen met een back-upkluis](backup-azure-vms-first-look.md)
>
>

In deze zelfstudie leert u hoe u een Recovery Services-kluis maakt en back-ups maakt van een virtuele machine (VM) van Azure. Recovery Services-kluizen beveiligen:

* VM's die zijn geïmplementeerd met Azure Resource Manager
* Klassieke VM's
* Standaardopslag-VM's
* Premium Storage-VM's
* Virtuele machines die op beheerde schijven worden uitgevoerd
* Virtuele machines die zijn versleuteld met Azure Disk Encryption
* Toepassingsconsistente back-up van Windows-VM's met behulp van VSS- en Linux-VM's en aangepaste scripts met momentopnamen vooraf en achteraf

Zie het artikel [Back-ups van virtuele machines voor Premium Storage maken en terugzetten](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) voor meer informatie over het beschermen van Premium Storage-VM's. Zie [Back-ups van virtuele machines op beheerde schijven maken en terugzetten](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup) voor meer informatie over ondersteuning voor virtuele machines op beheerde schijven. Voor meer informatie over pre- en post-script framework voor Linux-VM back-ups raadpleegt u [Application consistent Linux VM backup using pre-script and post-script (Toepassingsconsistente back-up van Linux-VM's met pre-script en post-script)] (https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Kijk [hier](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) voor meer informatie over waarvan u wel en niet een back-up kunt maken.

> [!NOTE]
> In deze zelfstudie wordt ervan uitgegaan dat u een VM in uw Azure-abonnement hebt en dat u maatregelen hebt genomen om toe te staan dat de back-upservice toegang tot de VM kan krijgen.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Afhankelijk van het aantal virtuele machines dat u wilt beveiligen, kunt u vanaf verschillende beginpunten starten. Als u in één bewerking een back-up wilt maken van meerdere virtuele machines, gaat u naar de Recovery Services-kluis en [start u de back-uptaak vanuit het dashboard van de kluis](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Als u een back-up van één virtuele machine wilt maken, kunt u de back-uptaak starten vanaf de VM-beheerblade.

## <a name="configure-the-backup-job-from-the-vm-management-blade"></a>De back-uptaak configureren vanaf de VM-beheerblade

Voer de volgende stappen uit als u de back-uptaak wilt configureren vanaf de VM-beheerblade in Azure Portal. Deze stappen zijn niet van toepassing op virtuele machines in de klassieke portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Meer services** en typ **Virtuele machines** in het dialoogvenster Filter. Terwijl u typt, wordt de lijst met resources gefilterd. Wanneer u Virtuele machines ziet, selecteert u deze optie.

  ![Klik in het menu Hub op Meer services om een tekstvenster te openen. Typ hierin 'Virtuele machines'.](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  De lijst met virtuele machines in het abonnement wordt weergegeven.

  ![De lijst met virtuele machines in het abonnement wordt weergegeven.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken.

  ![De lijst met virtuele machines in het abonnement wordt weergegeven.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  Wanneer u de virtuele machine selecteert, schuift de lijst met virtuele machines naar links en worden de VM-beheerblade en het dashboard Virtuele machine geopend. </br>
 ![VM-beheerblade](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

4. Klik in de sectie **Instellingen** van de VM-beheerblade op **Back-up**. </br>

  ![Back-upoptie op VM-beheerblade](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

  De blade Back-up inschakelen wordt geopend.

  ![Back-upoptie op VM-beheerblade](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Klik voor de Recovery Services-kluis op **Bestaande selecteren** en kies de kluis uit de vervolgkeuzelijst.

  ![Wizard Back-up inschakelen](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

  Als er geen Recovery Services-kluizen zijn of als u een nieuw kluis wilt gebruiken, klikt u op **Nieuwe maken** en geeft u de naam op voor de nieuwe kluis. Een nieuwe kluis wordt in dezelfde resourcegroep en op dezelfde locatie als de virtuele machine gemaakt. Als u een Recovery Services-kluis met verschillende waarden wilt maken, raadpleeg dan de sectie over [het maken van een Recovery Services-kluis](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm).

6. Klik op **Back-up beleid** als u de details van het back-upbeleid wilt bekijken.

  De blade **Back-upbeleid** met de details van het geselecteerde beleid wordt geopend. Als er nog andere beleidsregels zijn, gebruikt u de vervolgkeuzelijst om een ander back-upbeleid te kiezen. Als u een beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid. Als u de wijzigingen in het back-upbeleid wilt opslaan en naar de blade Back-up inschakelen wilt terugkeren, klikt u op **OK**.

  ![Back-upbeleid selecteren](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new-2.png)

7. Klik op de blade Back-op inschakelen op **Back-up inschakelen** om het beleid te implementeren. Als u het beleid implementeert, koppelt u het aan de kluis en de virtuele machines.

  ![Knop Backup inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. U kunt de voortgang van de configuratie volgen via de meldingen die in de portal worden weergegeven. In het volgende voorbeeld ziet u dat de implementatie is gestart.

  ![Back-upmeldingen inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Klik na voltooiing van de configuratie op de VM-beheerblade op **Back-up** om de blade Back-upitem te openen en de details te bekijken.

  ![Weergave Back-upitem VM](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

  Zolang de eerste back-up nog niet is voltooid, wordt voor **Status van de laatste back-up** de tekst **Waarschuwing (eerste back-up in behandeling)** weergegeven. Als u wilt nagaan wanneer de volgende geplande back-uptaak wordt uitgevoerd, klikt u onder **Back-upbeleid** op de naam van het beleid. De blade Back-upbeleid met het tijdstip van de geplande back-up wordt geopend.

10. Als u een back-uptaak wilt uitvoeren en het eerste herstelpunt wilt maken, klikt u op de blade Back-upkluis op **Nu back-up maken**.

  ![Op Nu back-up maken klikken om de eerste back-up uit te voeren](./media/backup-azure-vms-first-look-arm/backup-now.png)

  De blade Nu back-up maken wordt geopend.

  ![toont de blade Nu back-up maken](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Klik op de blade Nu back-up maken op het kalenderpictogram en selecteer in de kalender de laatste dag dat dit herstelpunt wordt bewaard. Klik vervolgens op **Back-up**.

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
> Het maken van back-ups van VM's is een lokaal proces. Het is niet mogelijk van VM's op de ene locatie een back-up in een Recovery Services-kluis op een andere locatie te maken. Daarom moet er voor elke Azure-locatie met VM's waarvoor u een back-up wilt maken, ten minste één Recovery Services-kluis op die locatie bestaan.
>
>

Een Recovery Services-kluis maken:

1. Meld u met uw Azure-abonnement aan bij [Azure Portal](https://portal.azure.com/) als u dit nog niet hebt gedaan.
2. Klik in het menu Hub op **Meer services** en typ **Recovery Services** in het dialoogvenster Filter. Terwijl u typt, wordt de lijst met resources gefilterd. Wanneer Recovery Services-kluizen in de lijst wordt weergegeven, klikt u erop.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Als er Recovery Services-kluizen in het abonnement aanwezig zijn, worden deze weergegeven.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    De blade Recovery Services-kluis wordt geopend en u wordt gevraagd een **naam**, **abonnement**, **resourcegroep** en **locatie** in te voeren.

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
  > Als u niet zeker weet op welke locatie uw VM zich bevindt, sluit u het dialoogvenster voor het maken van de kluis en gaat u naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u in elke regio een Recovery Services-kluis maken. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. U hoeft geen opslagaccounts voor het opslaan van de back-upgegevens op te geven. Dit wordt automatisch door de Recovery Services-kluis en de Azure Backup-service afgehandeld.
  >

8. Klik onder aan de blade Recovery Services-kluis op **Maken**.

    Het kan enkele minuten duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal. Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis na enkele minuten niet ziet, klik dan op **Vernieuwen**.

    ![Op de knop Vernieuwen klikken](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Wanneer u uw kluis in de lijst met Recovery Services-kluizen ziet, kunt u de opslagredundantie instellen.

Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Als de Recovery Services-kluis uw primaire back-up is, laat u de opslagoptie voor replicatie ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie wilt die minder duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante](../storage/common/storage-redundancy.md#locally-redundant-storage) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer op de blade **Recovery Services-kluizen** de nieuwe kluis.

  ![De nieuwe kluis in de lijst met Recovery Services-kluizen selecteren](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Wanneer u de kluis selecteert, worden de blade Instellingen (*met bovenaan de kluisnaam*) en de blade met kluisdetails geopend.

  ![De opslagconfiguratie voor nieuwe kluis bekijken](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. Schuif op de blade Instellingen van de nieuwe kluis omlaag naar het gedeelte Beheren en klik op **Back-upinfrastructuur**.
    De blade Back-up maken van infrastructuur wordt geopend.
3. Klik op de blade Back-up maken van infrastructuur op **Back-up maken van configuratie** om de blade **Back-up maken van configuratie** te openen.

    ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Kies het type opslagreplicatie dat geschikt is voor uw kluis.

    ![keuzes bij opslagconfiguratie](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Uw kluis heeft standaard geografisch redundante opslag. Als Azure uw primaire eindpunt is voor back-upopslag, blijf dan **Geografisch redundant** gebruiken. Als Azure niet uw primaire eindpunt is voor back-upopslag, kiest u **Lokaal redundant**, zodat u de kosten voor Azure-opslag verlaagt. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante ](../storage/common/storage-redundancy.md#locally-redundant-storage) opslag in dit [overzicht van opslagredundantie](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren welke items moeten worden beveiligd
Voordat u een VM met een kluis registreert, voert u het detectieproces uit om ervoor te zorgen dat nieuwe virtuele machines die zijn toegevoegd aan het abonnement, worden geïdentificeerd. Er wordt een query uitgevoerd om de virtuele Azure-machines in het abonnement weer te geven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio. In Azure Portal wordt met scenario verwezen naar wat u in de Recovery Services-kluis wilt opnemen. Het beleid is de planning voor hoe vaak en wanneer er herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Anders klikt u in het menu Hub op **Meer services**, typt u **Recovery Services** in de lijst met resources en klikt u op **Recovery Services-kluizen**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.

    ![Weergave van de lijst met Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Selecteer een kluis in de lijst met Recovery Services-kluizen om het dashboard ervan te openen.

     ![Blade Kluis openen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Klik in het menu van het kluisdashboard op **Back-up** om de blade Back-up te openen.

    ![Blade Back-up openen](./media/backup-azure-arm-vms-prepare/backup-button.png)

    De blades Back-up en Doel van de back-up worden geopend.

    ![Blade Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. Kies Azure in de vervolgkeuzelijst **Waar wordt uw werkbelasting uitgevoerd?** van de blade Doel van de back-up. Kies Virtuele machine in de vervolgkeuzelijst **Waarvan wilt u een back-up maken** en klik op **OK**.

    Hiermee wordt de VM-extensie bij de kluis geregistreerd. De blade Doel van de back-up wordt gesloten en de blade **Back-upbeleid** wordt geopend.

    ![Blade Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Selecteer op de blade Back-upbeleid het back-upbeleid dat u op de kluis wilt toepassen.

    ![Back-upbeleid selecteren](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven. Als u een beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid.
    Klik op **OK** om het back-upbeleid aan de kluis te koppelen.

    De blade Back-upbeleid wordt gesloten en de blade **Virtuele machines selecteren** wordt geopend.
5. Kies op de blade **Virtuele machines selecteren** de virtuele machines die u aan het opgegeven beleid wilt koppelen en klik op **OK**.

    ![Workload selecteren](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    De geselecteerde virtuele machine wordt gevalideerd. Als u de verwachte virtuele machines niet ziet, controleert u of ze bestaan op dezelfde Azure-locatie als de Recovery Services-kluis. De locatie van de Recovery Services-kluis wordt weergegeven op het kluisdashboard.

6. Nu alle instellingen voor de kluis zijn gedefinieerd, klikt u op de blade Back-up op **Back-up inschakelen** om het beleid te implementeren op de kluizen en virtuele machines. Met het implementeren van het beleid wordt niet het eerste herstelpunt voor de virtuele machine gemaakt.

    ![Back-up inschakelen](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nadat de back-up is ingeschakeld, wordt het back-upbeleid volgens het schema uitgevoerd. Ga echter verder om de eerste back-uptaak te starten.

## <a name="initial-backup"></a>Eerste back-up
Als u een back-upbeleid op de virtuele machine hebt geïmplementeerd, betekent dit niet dat er een back-up van de gegevens is gemaakt. De eerste geplande back-up (zoals gedefinieerd in het back-upbeleid) is standaard de eerste back-up. Totdat de eerste back-up plaatsvindt, wordt voor de status van de laatste back-up op de blade **Back-uptaken** de tekst **Waarschuwing (eerste back-up in behandeling)** weergegeven.

![Back-up in behandeling](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Tenzij de eerste back-up binnenkort wordt gemaakt, wordt aanbevolen dat u **Nu een back-up maken** uitvoert.

De eerste back-uptaak uitvoeren:

1. Klik op het kluisdashboard op het getal onder **Back-upitems** of klik op de tegel **Back-upitems**. <br/>
  ![Pictogram Instellingen](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  De blade **Back-upitems** wordt geopend.

  ![Back-upitems](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Selecteer het item op de blade **Back-upitems**.

  ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  De lijst **Back-upitems** wordt geopend. <br/>

  ![Geactiveerde back-uptaak](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Klik in de lijst **Back-upitems** op het weglatingsteken **...**  om het contextmenu te openen.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Het contextmenu wordt weergegeven.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klik in het contextmenu op **Nu back-up maken**.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  De blade Nu back-up maken wordt geopend.

  ![toont de blade Nu back-up maken](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Klik op de blade Nu back-up maken op het kalenderpictogram en selecteer in de kalender de laatste dag dat dit herstelpunt wordt bewaard. Klik vervolgens op **Back-up**.

  ![de laatste dag instellen dat het herstelpunt van Nu back-up maken wordt bewaard](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Implementatiemeldingen laten u weten dat de back-uptaak is geactiveerd en dat u de voortgang van de taak op de pagina Back-uptaken kunt controleren. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.

6. Als u de status van de eerste back-up wilt bekijken of volgen, klikt u op de tegel **Back-uptaken** van het kluisdashboard op **Bezig**.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  De blade Back-uptaken wordt geopend.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  Op de blade **Back-uptaken** kunt u de status van alle taken bekijken. Controleer of de back-uptaak voor de virtuele machine nog steeds bezig is of is voltooid. Wanneer de back-uptaak is voltooid, verandert de status in *Voltooid*.

  > [!NOTE]
  > Als onderdeel van de back-upbewerking wordt met de Azure Backup-service aan de back-upextensie in elke VM opdracht gegeven om alle schrijfbewerkingen leeg te maken en een consistente momentopname te maken.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent op de virtuele machine installeren
Deze informatie wordt verstrekt voor het geval u deze nodig hebt. De Azure VM-agent moet worden geïnstalleerd op de virtuele Azure-machine om de Backup-extensie te kunnen gebruiken. Als uw VM echter is gemaakt vanuit de Azure-galerie, is de VM-agent al aanwezig op de virtuele machine. Op VM's die zijn gemigreerd vanuit on-premises datacenters, is geen VM-agent geïnstalleerd. In dat geval moet de VM-agent worden geïnstalleerd. Als u problemen ondervindt bij het maken van een back-up van de virtuele Azure-machine, controleert u of de Azure VM-agent correct is geïnstalleerd op de virtuele machine (zie de volgende tabel). Als u een aangepaste VM maakt, [zorgt u ervoor dat het selectievakje **De VM-agent installeren** is ingeschakeld](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) voordat de virtuele machine wordt ingericht.

Meer informatie over de [VM-agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) en [hoe u deze installeert](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

De volgende tabel bevat aanvullende informatie over de VM-agent voor Windows- en Linux-VM's.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |<li>Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li>[Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |<li> Installeer de meest recente [Linux-agent](https://github.com/Azure/WALinuxAgent) vanuit GitHub. U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li> [Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N.v.t. |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent op de virtuele machine is geïnstalleerd, installeert de Azure Backup-service de Backup-extensie in de VM-agent. De Azure Backup-service wordt probleemloos bijgewerkt en er wordt zonder tussenkomst van de gebruiker een patch voor de Backup-extensie uitgevoerd.

De Backup-service installeert de back-upextensie, zelfs als de virtuele machine niet wordt uitgevoerd. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Azure Backup-service blijft echter back-ups van de VM maken, zelfs als deze is uitgeschakeld en de extensie niet kan worden geïnstalleerd. Dit type back-up wordt aangeduid als Offline VM en het herstelpunt is *consistent vastlopen*.

## <a name="troubleshooting-information"></a>Informatie over probleemoplossing
Als u problemen hebt bij het uitvoeren van de taken in dit artikel, raadpleegt u de [richtlijnen voor probleemoplossing](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Prijzen
De kosten voor het maken van back-ups van virtuele Azure-machines is gebaseerd op het aantal beveiligde exemplaren. Zie voor een definitie van een beveiligd exemplaar [Wat is een beveiligd exemplaar?](backup-introduction-to-azure-backup.md#what-is-a-protected-instance) Zie voor een voorbeeld van het berekenen van de kosten voor het maken van back-ups van een virtuele machine [Hoe worden beveiligde exemplaren berekend?](backup-azure-vms-introduction.md#calculating-the-cost-of-protected-instances) Zie de pagina met prijzen van Azure Backup voor meer informatie over [de prijzen van Backup](https://azure.microsoft.com/pricing/details/backup/).

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen zag worden, [stuurt u ons feedback](http://aka.ms/azurebackup_feedback).
