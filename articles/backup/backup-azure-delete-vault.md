---
title: Een Recovery Services-kluis in Azure verwijderen '
description: In dit artikel wordt uitgelegd hoe u een Recovery Services-kluis verwijderen. Dit artikel bevat stappen voor probleemoplossing als u probeert te verwijderen van een kluis, maar niet.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: d514074e56ff37cc7af6a97ea86aa9e02e3763e0
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492280"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

In dit artikel wordt beschreven hoe u alle items uit een Recovery Services-kluis verwijderen en vervolgens te verwijderen. U kunt een Recovery Services-kluis niet verwijderen als deze is geregistreerd bij een server en gegevens back-up bevat. Als u probeert te verwijderen van een kluis, maar niet, is nog steeds de kluis geconfigureerd voor het ontvangen van back-upgegevens.

Voor informatie over het verwijderen van een kluis, Zie de sectie [een kluis verwijderen uit Azure portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Zie de sectie als u niet wilt behouden van gegevens in de Recovery Services-kluis en de kluis wilt verwijderen, [verwijderen van de kluis geforceerd beëindigd](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Als u niet zeker weet wat er in de kluis en u ervoor zorgen moet dat kunt u de kluis verwijderen, Zie de sectie [verwijderen kluis afhankelijkheden en de kluis verwijderen](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Een kluis verwijderen uit Azure portal

Als u de Recovery Services-kluis openen al hebt, gaat u naar de tweede stap.

1. Open de Azure-portal en open de kluis die u wilt verwijderen vanuit het Dashboard.

   Als u geen Recovery Services-kluis vastgemaakt aan het Dashboard, in het Hub-menu, klikt u op **alle Services** en typ in de lijst met resources **herstelservices**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Als u wilt de lijst met kluizen in uw abonnement weergeven, klikt u op **Recovery Services-kluizen**.

   ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   De lijst met Recovery Services-kluizen wordt weergegeven. 

   ![Kies de kluis uit lijst](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

1. Selecteer de kluis die u wilt verwijderen uit de lijst. Wanneer u de kluis selecteert, wordt het kluisdashboard wordt geopend.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Als u wilt verwijderen van een kluis, in het dashboard van de kluis, klikt u op **verwijderen**. U wordt gevraagd om te controleren of dat u wilt verwijderen van de kluis.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Als de **fout bij verwijderen van kluis** wordt weergegeven, kunt u de afhankelijkheden van de kluis verwijderen, of u kunt PowerShell gebruiken voor het verwijderen van de kluis geforceerd beëindigd. De volgende secties wordt uitgelegd hoe u deze taken.

    ![Fout tijdens verwijderen van kluis](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Verwijder de Recovery Services-kluis geforceerd beëindigd

U kunt PowerShell gebruiken om te verwijderen van een Recovery Services-kluis geforceerd beëindigd. Door force betekent dat is de Recovery Services-kluis en alle gekoppelde back-upgegevens, definitief verwijderd. 

> [!Warning]
> Wanneer met behulp van PowerShell een Recovery Services-kluis verwijderen, moet u zeker dat u wilt alle back-upgegevens in de kluis permanent te verwijderen.
>

Een Recovery Services-kluis verwijderen:

1. Meld u aan bij uw Azure-account.

   Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

   ```powershell
    Connect-AzureRmAccount
   ```
   De eerste keer dat u Azure Backup gebruikt, moet u de Azure Recovery Service-provider registreren in uw abonnement met behulp van [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider).

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

1. Open een PowerShell-venster met beheerdersbevoegdheden.

1. Gebruik `Set-ExecutionPolicy Unrestricted` beperkingen verwijderen.

1. Voer de volgende opdracht om te downloaden van het Azure Resource Manager-clientpakket van chocolately.org.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

1. Gebruik de volgende opdracht om de Azure Resource Manager API-Client te installeren.

   `choco.exe install armclient`

1. Verzamel de abonnements-ID en de naam van gekoppelde resource voor de Recovery Services-kluis die u wilt verwijderen in de Azure-portal.

1. Voer de volgende opdracht uit met behulp van uw abonnements-ID, de naam van de resourcegroep en de naam van de Recovery Services-kluis in PowerShell. Wanneer u de opdracht uitvoert, worden de kluis en alle afhankelijkheden verwijderd.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
   De kluis moet leeg zijn voordat u het kunt verwijderen. Anders krijgt u een vermelding van "Kluis kan niet worden verwijderd omdat er bestaande resources binnen deze kluis" fout. De volgende opdracht laat zien hoe u een container in een kluis verwijderen:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```
   
1. Meld u aan uw abonnement in Azure portal en controleer of dat de kluis wordt verwijderd.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Afhankelijkheden van de kluis verwijderen en de kluis verwijderen

Als u wilt de kluis afhankelijkheden handmatig verwijderen, de configuratie tussen elk item of de server en de Recovery Services-kluis te verwijderen. Als u de volgende procedure doorloopt, gebruikt u de **back-Upitems** menu (Zie afbeelding) voor:

* Back-ups van Azure Storage (Azure-bestanden)
* SQL Server in virtuele Azure-machines
* Back-ups van virtuele machines van Azure
* Microsoft Azure Recovery Services agent back-ups

Gebruik de **back-upinfrastructuur** menu (Zie afbeelding) voor:

* Back-ups van Azure Backup Server
* Back-ups van System Center DPM

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. Schuif omlaag naar het gedeelte van de beveiligde Items in de kluis in het menu en klik op **back-Upitems**. U kunt in dit menu stoppen en verwijderen van Azure-bestandsservers, SQL-Servers in de virtuele machine van Azure en Azure virtual machines. In dit voorbeeld hebt we back-upgegevens verwijderen van een Azure-bestandsserver.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/selected-backup-items.png)

1. Selecteer een type back-up om alle objecten van dat type weer te geven.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

1. Voor alle items in de lijst met de rechtermuisknop op het item en selecteer in het contextmenu **back-up stoppen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/stop-backup-item.png) 

    Het menu back-up stoppen wordt geopend.

1. Op de **back-up stoppen** in het menu van de **Kies een optie** in het menu **back-upgegevens verwijderen**, typ de naam van het item en klikt u op **back-up stoppen**.

    Typ de naam van het item, om te controleren of dat u wilt verwijderen. De **back-up stoppen** knop wordt geactiveerd zodra u het item controleren. Als u de gegevens worden bewaard, kunt u zich niet aan de kluis verwijderen.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Als u wilt, Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe. Als u wilt controleren of de taak is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). <br/>
    Nadat de taak is voltooid, de service verzendt een bericht: *het back-upproces is gestopt en de back-upgegevens is verwijderd*.

1. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items is nog steeds worden weergegeven in de kluis, gaat u terug naar stap 3 en kies een ander item type lijst.  

1. Wanneer er geen items meer in de werkbalk van de kluis zijn, klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**.

    De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="removing-azure-backup-server-or-dpm"></a>Azure Backup-Server of DPM verwijderen

1. Schuif omlaag naar het gedeelte beheren in de kluis in het menu en klik op **back-upinfrastructuur**. 

1. Klik in het submenu **back-up-beheerservers** om de Azure Backup-Servers en System Center DPM-server weer te geven. u kunt stoppen en verwijderen van Azure-bestandsservers, SQL-Servers in de virtuele machine van Azure en Azure virtual machines. 

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

1. Met de rechtermuisknop op het item dat u wilt verwijderen en selecteer in het submenu **verwijderen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Het menu back-up stoppen wordt geopend.

1. Op de **back-up stoppen** in het menu van de **Kies een optie** in het menu **back-upgegevens verwijderen**, typ de naam van het item en klikt u op **back-up stoppen**.

    Als u wilt controleren of dat u wilt verwijderen, typt u de naam. De **back-up stoppen** knop wordt geactiveerd zodra u het item controleren. Als u de gegevens worden bewaard, kunt u de kluis niet verwijderen.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Geef desgewenst een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe. Om te controleren dat de taak is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). <br/>
    Nadat de taak voltooid is, de service verzendt een bericht: het back-upproces is gestopt en de back-upgegevens is verwijderd.

1. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de resterende items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items is nog steeds worden weergegeven in de kluis, gaat u terug naar stap 3 en kies een ander item type lijst.  
1. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**.

    De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.


## <a name="removing-azure-backup-agent-recovery-points"></a>Herstelpunten voor Azure backup-agent verwijderen

1. Schuif omlaag naar het gedeelte beheren in de kluis in het menu en klik op **back-upinfrastructuur**.

1. Klik in het submenu **beschermde Servers** beveiligd servertypen, met inbegrip van Azure backup-agent om de lijst weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

1. In de **beschermde Servers** lijst, klikt u op Azure Backup-Agent.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    De lijst met servers die worden beveiligd met Azure backup-agent, wordt geopend.

    ![Selecteer de beveiligde server](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

1. In de lijst met servers, klikt u op een om het menu te openen.

    ![de geselecteerde server dashboard weergeven](./media/backup-azure-delete-vault/selected-protected-server.png)

1. Klik op de geselecteerde server in het menu **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

1. Op de **verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.

    Typ de naam van het item, om te controleren of dat u wilt verwijderen. De **verwijderen** knop wordt geactiveerd zodra u het item controleren.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    Geef desgewenst een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe. Om te controleren dat de taak is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). <br/>
    Nadat de taak voltooid is, de service verzendt een bericht: het back-upproces is gestopt en de back-upgegevens is verwijderd.

1. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de resterende items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items is nog steeds worden weergegeven in de kluis, gaat u terug naar stap 3 en kies een ander item type lijst.  
1. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**.

    De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Wat gebeurt er als ik het back-upproces te stoppen, maar blijven de gegevens bewaard?

Als u stopt met het back-upproces maar per ongeluk *behouden* de gegevens, moet u de back-upgegevens verwijderen voordat u de kluis kunt verwijderen. De back-upgegevens verwijderen:

1. Op de **back-Upitems** in het menu met de rechtermuisknop op het item en klik in het contextmenu op **back-upgegevens verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    De **back-upgegevens verwijderen** menu wordt geopend.
1. Op de **back-upgegevens verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Nadat u de gegevens hebt verwijderd, gaat u terug naar stap 4c en doorgaan met het proces.
