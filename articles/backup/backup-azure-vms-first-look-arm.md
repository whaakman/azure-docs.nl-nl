---
title: 'Eerste blik: virtuele machines van Azure beveiligen met een Recovery Services-kluis | Microsoft Docs'
description: "Beveilig virtuele machines van Azure met een Recovery Services-kluis. Gebruik back-ups van virtuele machines die met Resource Manager of het klassieke model zijn geïmplementeerd en virtuele machines voor Premium-opslag om uw gegevens te beveiligen. Maak en registreer een Recovery Services-kluis. Registreer VM&quot;s, maak beleid en beveilig VM&quot;s in Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/10/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: 85b291e3916d1274fefc71bc0c1f12cac2920bb4
ms.openlocfilehash: 77b4f6e5ee18cb3772487820bc72d7794f82162f


---
# <a name="first-look-protect-azure-vms-with-a-recovery-services-vault"></a>Eerste blik: virtuele machines van Azure beveiligen met een Recovery Services-kluis
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
* Virtuele machines die met Azure Disk Encryption zijn versleuteld, met BitLocker-versleutelingssleutels (BEK) en Key Encryption Keys (KEK)

Zie [Back-ups van VM's voor Premium Storage maken en herstellen](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) voor meer informatie

> [!NOTE]
> In deze zelfstudie wordt ervan uitgegaan dat u een VM in uw Azure-abonnement hebt en dat u maatregelen hebt genomen om toe te staan dat de back-upservice toegang tot de VM kan krijgen.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

U kunt op verschillende punten beginnen, op basis van het aantal VM's dat u wilt beschermen. Als u van meerdere virtuele machines een back-up wilt maken in één bewerking, gaat u naar de Recovery Services-kluis en start u het back-up maken vanuit het kluisdashboard. Als u van één VM een back-up wilt maken, kunt u rechtstreeks vanaf de beheerblade van de VM een back-up maken.

## <a name="configure-backup-from-vm-management-blade"></a>Back-up configureren vanaf VM-beheerblade
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Meer services** en typ **Virtuele machines** in de lijst met resources.  De lijst met virtuele machines wordt weergegeven. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken. Hiermee wordt de beheerblade van de virtuele machine geopend. 
 ![VM-beheerblade](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)
 
3. Klik op de beheerblade van de VM op de optie Back-up (aan de linkerkant, onder Instellingen).
![Back-upoptie op VM-beheerblade](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

4. Hiermee wordt de blade Back-up inschakelen geopend. Deze blade heeft twee verwachte invoeren: Recovery Services-kluis (een back-upmiddel van Azure dat wordt gebruikt om back-ups van de VM's op te slaan) en Back-upbeleid. Het ingestelde back-upbeleid specificeert het back-upschema en bepaalt hoelang back-upkopieën moeten worden bewaard. Deze blade heeft standaardopties. U kunt deze aanpassen op basis van uw back-upvereisten. 
![Wizard Back-up inschakelen](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Voor Recovery Services-kluis kunt u een bestaande kluis selecteren of een nieuwe maken. Als u een nieuwe kluis maakt, wordt deze in dezelfde resourcegroep gemaakt als de virtuele machine en is de locatie hetzelfde als die van de virtuele machine. Als u een Recovery Services-kluis met verschillende waarden wilt maken, kunt u [een Recovery Services-kluis maken](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm) voordat u in stap 3 op de optie Back-up klikt en deze kluis op deze blade selecteren. 

6. Selecteer op de blade Back-upbeleid het back-upbeleid dat u wilt toepassen op de kluis en klik op **OK**.
    ![Back-upbeleid selecteren](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden in de details weergegeven. Als u een beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. De vervolgkeuzelijst biedt ook een optie waarmee u de tijd kunt wijzigen waarop de momentopname wordt gemaakt. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid. Nadat u op **OK** hebt geklikt, is het back-upbeleid gekoppeld aan de virtuele machine.
    
7. Klik op Back-up inschakelen om het maken van een back-up te configureren op de virtuele machine. Hiermee wordt een implementatie geactiveerd. 
![Knop Back-up inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. U kunt de voortgang van de configuratie via meldingen volgen. 
![Back-upmeldingen inschakelen](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Wanneer de implementatie van Back-up configureren voltooid is, kunt u op de optie Back-up op de VM-beheerblade klikken om naar de blade Back-upitem te gaan die hoort bij de VM waarvan een back-up is gemaakt.
![Weergave Back-upitem VM](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

## <a name="configure-backup-from-recovery-services-vault-view"></a>Back-up configureren vanuit de Recovery Services-kluisweergave
U voert hierbij de volgende basisstappen uit.  

1. Een Recovery Services-kluis voor een VM maken.
2. Azure Portal gebruiken om een scenario te selecteren, een beleid in te stellen en te bepalen welke items moeten worden beveiligd.
3. De eerste back-up uitvoeren.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Een Recovery Services-kluis voor een VM maken
Een Recovery Services-kluis is een entiteit waarmee alle back-ups en herstelpunten worden opgeslagen die in de loop van de tijd zijn gemaakt. De Recovery Services-kluis bevat ook het back-upbeleid dat wordt toegepast op de beveiligde VM's.

> [!NOTE]
> Het maken van back-ups van VM's is een lokaal proces. Het is niet mogelijk om van VM's op de ene locatie een back-up in een Recovery Services-kluis op een andere locatie te maken. Daarom moet er voor elke Azure-locatie met VM's waarvoor u een back-up wilt maken, ten minste één Recovery Services-kluis op die locatie bestaan.
>
>

Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    De blade Recovery Services-kluis wordt geopend en u wordt gevraagd een **naam**, **abonnement**, **resourcegroep** en **locatie** in te voeren.

    ![Een Recovery Services-kluis maken, stap 5](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)
4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die tussen 2 en 50 tekens bevat. De naam moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
5. Klik op **Abonnement** om de beschikbare lijst met abonnementen te bekijken. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaard- (of voorgestelde) abonnement. Er zijn alleen meerdere mogelijkheden als uw organisatieaccount is gekoppeld aan meerdere Azure-abonnementen.
6. Klik op **Resourcegroep** om de lijst met beschikbare resourcegroepen te bekijken of klik op **Nieuw** om een resourcegroep te maken. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
7. Klik op **Locatie** om de geografische regio voor de kluis te selecteren. De kluis **moet** zich in dezelfde regio bevinden als de virtuele machines die u wilt beveiligen.

   > [!IMPORTANT]
   > Als u niet zeker weet op welke locatie uw VM zich bevindt, sluit u het dialoogvenster voor het maken van de kluis en gaat u naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u in elke regio een Recovery Services-kluis maken. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. U hoeft geen opslagaccounts voor het opslaan van de back-upgegevens op te geven: dit wordt automatisch door de Recovery Services-kluis en de Azure Backup-service afgehandeld.
   >
   >
8. Klik op **Create**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal. Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/rs-list-of-vaults.png)

Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Laat de optie ingesteld op geografisch redundante opslag als dit uw primaire back-up is. Kies lokaal redundante opslag als u een goedkopere optie wilt die minder duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante](../storage/storage-redundancy.md#locally-redundant-storage) opslag in het [overzicht van Azure Storage-replicatie](../storage/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer de kluis om het kluisdashboard en de blade Instellingen te openen. Als de blade **Instellingen** niet wordt geopend, klikt u op **Alle instellingen** op het kluisdashboard.
2. Klik op de blade **Instellingen** op **Back-upinfrastructuur** > **Back-upconfiguratie** om de blade **Back-upconfiguratie** te openen. Kies op de blade **Back-upconfiguratie** de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren welke items moeten worden beveiligd
Voordat u een VM met een kluis registreert, voert u het detectieproces uit om ervoor te zorgen dat nieuwe virtuele machines die zijn toegevoegd aan het abonnement, worden geïdentificeerd. Er wordt een query uitgevoerd om de virtuele Azure-machines in het abonnement weer te geven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio. In Azure Portal wordt met scenario verwezen naar wat u in de Recovery Services-kluis wilt opnemen. Het beleid is de planning voor hoe vaak en wanneer er herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Als er nog geen Recovery Services-kluis is geopend, maar Azure Portal wordt weergeven, klikt u in het menu Hub op **Bladeren**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

     ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

     De lijst met Recovery Services-kluizen wordt weergegeven.
   * Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.

     ![Blade Kluis openen](./media/backup-azure-vms-first-look-arm/vault-settings.png)
2. Klik in het menu van het kluisdashboard op **Back-up** om de blade Back-up te openen.

    ![Blade Back-up openen](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Wanneer de blade wordt geopend, wordt met de Backup-service gezocht naar nieuwe VM's in het abonnement.

    ![VM's detecteren](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)
3. Klik op de blade Back-up op **Back-updoel** om de blade Back-updoel openen.

    ![Blade Scenario openen](./media/backup-azure-vms-first-look-arm/select-backup-goal-one.png)
4. Stel op de blade Back-updoel **Waar wordt de workload uitgevoerd** in op Azure en **Waarvan wilt u een back-up maken** op Virtuele machine klik vervolgens op **OK**.

    De blade Back-updoel wordt gesloten en de blade Back-upbeleid wordt geopend.

    ![Blade Scenario openen](./media/backup-azure-vms-first-look-arm/select-backup-goal-two.png)
5. Selecteer op de blade Back-upbeleid het back-upbeleid dat u wilt toepassen op de kluis en klik op **OK**.

    ![Back-upbeleid selecteren](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden in de details weergegeven. Als u een beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. De vervolgkeuzelijst biedt ook een optie waarmee u de tijd kunt wijzigen in 19.00 uur, wanneer de momentopname wordt gemaakt. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid. Nadat u op **OK** hebt geklikt, is het back-upbeleid gekoppeld aan de kluis.

    Kies vervolgens de VM's die u aan de kluis wilt koppelen.
6. Kies de virtuele machines die u aan het opgegeven beleid wilt koppelen, en klik op **Selecteren**.

    ![Workload selecteren](./media/backup-azure-vms-first-look-arm/select-vms-to-backup-new.png)

    Als de gewenste VM niet wordt weergegeven, controleert u of deze bestaat op dezelfde Azure-locatie als de Recovery Services-kluis.
7. Nu alle instellingen voor de kluis zijn gedefinieerd, klikt u op de blade Back-up op **Back-up inschakelen** onder aan de pagina. Hierop wordt het beleid voor de kluis en de VM's geïmplementeerd.

    ![Back-up inschakelen](./media/backup-azure-vms-first-look-arm/enable-backup-settings-new.png)

## <a name="initial-backup"></a>Eerste back-up
Als u een back-upbeleid op de virtuele machine hebt geïmplementeerd, betekent dit niet dat er een back-up van de gegevens is gemaakt. De eerste geplande back-up (zoals gedefinieerd in het back-upbeleid) is standaard de eerste back-up. Totdat de eerste back-up plaatsvindt, wordt voor de status van de laatste back-up op de blade **Back-uptaken** de tekst **Waarschuwing (eerste back-up in behandeling)** weergegeven.

![Back-up in behandeling](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Tenzij de eerste back-up binnenkort wordt gemaakt, wordt aanbevolen dat u **Nu een back-up maken** uitvoert.

Voer **Nu een back-up maken** als volgt uit:

1. Klik op het kluisdashboard op de tegel **Back-up** op **Azure Virtual Machines** <br/>
    ![Pictogram Instellingen](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    De blade **Back-upitems** wordt geopend.
2. Klik met de rechtermuisknop op de blade **Back-upitems** op de kluis waarvan u een back-up wilt maken, en klik op **Nu een back-up maken**.

    ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    De back-uptaak wordt geactiveerd. <br/>

    ![Geactiveerde back-uptaak](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. Geef weer dat de eerste back-up is voltooid door op het kluisdashboard op de tegel **Back-uptaken** op **Azure Virtual Machines** te klikken.

    ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    De blade Back-uptaken wordt geopend.
4. Op de blade Back-uptaken ziet u de status van alle taken.

    ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Als onderdeel van de back-upbewerking wordt met de Azure Backup-service aan de back-upextensie in elke VM opdracht gegeven om alle schrijfbewerkingen leeg te maken en een consistente momentopname te maken.
   >
   >

    Wanneer de back-uptaak is voltooid, wordt de status gewijzigd in *Voltooid*.

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent op de virtuele machine installeren
Deze informatie wordt verstrekt voor het geval u deze nodig hebt. De Azure VM-agent moet worden geïnstalleerd op de virtuele Azure-machine om de Backup-extensie te kunnen gebruiken. Als uw VM echter is gemaakt vanuit de Azure-galerie, is de VM-agent al aanwezig op de virtuele machine. Op VM's die zijn gemigreerd vanuit on-premises datacenters, is geen VM-agent geïnstalleerd. In dat geval moet de VM-agent worden geïnstalleerd. Als u problemen ondervindt bij het maken van een back-up van de Azure VM, controleert u of de Azure VM-agent correct is geïnstalleerd op de virtuele machine (zie de onderstaande tabel). Als u een aangepaste VM maakt, [zorgt u ervoor dat het selectievakje **De VM-agent installeren** is ingeschakeld](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) voordat de virtuele machine wordt ingericht.

Meer informatie over de [VM-agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) en [hoe u deze installeert](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

De volgende tabel bevat aanvullende informatie over de VM-agent voor Windows- en Linux-VM's.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |<li>Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li>[Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |<li> Installeer de meest recente [Linux-agent](https://github.com/Azure/WALinuxAgent) vanuit GitHub. U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren. <li> [Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N.v.t. |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent op de virtuele machine is geïnstalleerd, installeert de Azure Backup-service de Backup-extensie in de VM-agent. De Azure Backup-service wordt probleemloos bijgewerkt en er wordt zonder tussenkomst van de gebruiker een patch voor de Backup-extensie uitgevoerd.

De Backup-extensie wordt geïnstalleerd door de Backup-service, ongeacht of de VM wordt uitgevoerd of niet. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Azure Backup-service blijft echter back-ups van de VM maken, zelfs als deze is uitgeschakeld en de extensie niet kan worden geïnstalleerd. Dit wordt Offline-VM genoemd. In dit geval is het herstelpunt *crashconsistent*.

## <a name="troubleshooting-information"></a>Informatie over probleemoplossing
Als u problemen hebt bij het uitvoeren van de taken in dit artikel, raadpleegt u de [richtlijnen voor probleemoplossing](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Prijzen
Azure VM-back-up wordt in rekening gebracht op basis van het model Beveiligde instanties. Meer informatie over [Prijzen van Backup](https://azure.microsoft.com/pricing/details/backup/)

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen zag worden, [stuurt u ons feedback](http://aka.ms/azurebackup_feedback).



<!--HONumber=Nov16_HO2-->


