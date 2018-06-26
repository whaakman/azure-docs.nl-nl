---
title: Een Recovery Services-kluis in Azure verwijderen '
description: In dit artikel wordt uitgelegd hoe een Recovery Services-kluis verwijderen. Dit artikel bevat stappen voor probleemoplossing als u probeert te verwijderen van een kluis, maar niet.
services: service-name
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 6/21/2018
ms.author: markgal
ms.openlocfilehash: d8169eba6790e49a85d69434663faabe7430942e
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937597"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Dit artikel wordt uitgelegd hoe u alle items verwijderen uit een Recovery Services-kluis en vervolgens te verwijderen. U kunt een Recovery Services-kluis niet verwijderen als deze is geregistreerd met een server en gegevens back-up bevat. Als u probeert te verwijderen van een kluis, maar kan niet, wordt de kluis nog steeds geconfigureerd voor het ontvangen van de back-upgegevens.

Voor informatie over het verwijderen van een kluis, Zie de sectie [een kluis verwijderen vanuit Azure-portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Zie de sectie als u niet wilt bewaren van gegevens in de Recovery Services-kluis en wilt verwijderen van de kluis, [verwijderen van de kluis door force](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Als u niet zeker weet wat is er in de kluis en u ervoor zorgen moet dat u de kluis kunt verwijderen, Zie de sectie [verwijderen kluis afhankelijkheden en delete kluis](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Een kluis verwijderen vanuit Azure-portal

Als u de Recovery Services-kluis openen al hebt, gaat u naar de tweede stap.

1. De Azure portal openen en open de kluis die u wilt verwijderen uit het Dashboard.

   Als u niet de Recovery Services-kluis vastgemaakt aan het Dashboard in het menu Hub klikt **alle Services** en typt u in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. U kunt de lijst met kluizen in uw abonnement op **Recovery Services-kluizen**.

   ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   De lijst met Recovery Services-kluizen wordt weergegeven. 

   ![Kies kluis in lijst](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

2. Selecteer de kluis die u wilt verwijderen uit de lijst. Wanneer u de kluis selecteert, wordt het kluisdashboard wordt geopend.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

3. U kunt een kluis, in het kluisdashboard verwijderen op **verwijderen**. U gevraagd om te controleren dat u wilt verwijderen van de kluis.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Als de **fout bij het verwijderen Vault** wordt weergegeven, kunt u de afhankelijkheden van de kluis verwijderen of u kunt PowerShell gebruiken voor het verwijderen van de kluis geforceerd. De volgende secties wordt uitgelegd hoe u deze taken.

    ![Fout bij het verwijderen van de kluis](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>De Recovery Services-kluis geforceerd verwijderen

PowerShell kunt u een Recovery Services-kluis geforceerd verwijderen. Op een geforceerde manier wordt de Recovery Services-kluis en alle gekoppelde back-upgegevens, definitief verwijderd. 

> [!Warning]
> Wanneer een Recovery Services-kluis verwijderen met behulp van PowerShell, moet u zeker dat u wilt alle back-gegevens in de kluis permanent te verwijderen.
>

Een Recovery Services-kluis verwijderen:

1. Aanmelden bij uw Azure-account.

   Aanmelden bij uw Azure-abonnement met de `Connect-AzureRmAccount` opdracht in en volg de op het scherm richtingen.

   ```powershell
    Connect-AzureRmAccount
   ```
   De eerste keer dat u Azure Backup gebruikt, moet u de Azure Recovery Service-provider registreren in uw abonnement met behulp van [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider).

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

2. Open een PowerShell-venster met Administrator-bevoegdheden.

3. Gebruik `Set-ExecutionPolicy Unrestricted` beperkingen verwijderen.

4. Voer de volgende opdracht het Azure Resource Manager-clientpakket downloaden van chocolately.org.

    `iex ((New-Object System.Net.WebClient) DownloadString('https://chocolatey.org/install.ps1))`

5. Gebruik de volgende opdracht in de Azure Resource Manager-API-Client te installeren.

   `choco.exe install armclient`

6. Verzamel de abonnements-ID en de bijbehorende Resourcegroepnaam voor de Recovery Services-kluis die u wilt verwijderen in de Azure portal.

7. Voer in PowerShell de volgende opdracht met uw abonnements-ID, de naam van resourcegroep en de naam van de Recovery Services-kluis. Wanneer u de opdracht uitvoert, worden de kluis en alle afhankelijkheden verwijderd.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
8. Aanmelden bij uw abonnement in de Azure portal en controleer of dat de kluis is verwijderd.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Kluis en de afhankelijkheden van de kluis verwijderen

Verwijder handmatig de kluis om afhankelijkheden te verwijderen, de configuratie tussen elk item of de server en de Recovery Services-kluis. Als u de volgende procedure doorloopt, gebruikt u de **back-Upitems** menu (Zie afbeelding) voor:

* Back-ups van Azure Storage (Azure-bestanden)
* SQL Server in Azure VM-back-ups
* Back-ups van virtuele machines in Azure
* Microsoft Azure Recovery Services agent back-ups

Gebruik de **back-upinfrastructuur** menu (Zie afbeelding) voor:

* Azure Backup-Server back-ups
* System Center DPM-back-ups

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. Schuif omlaag naar de sectie beveiligde Items in het dashboard kluismenu en klikt u op **back-Upitems**. U kunt in dit menu stoppen en verwijderen van Azure-bestandsservers, SQL-Servers in de virtuele machine van Azure en virtuele machines in Azure. In dit voorbeeld hebt we back-upgegevens van een Azure-bestandsserver verwijderen.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/selected-backup-items.png)

2. Selecteer een back-type dat alle items van dat type.

    ![Selecteer het type back-up](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. Voor alle items in de lijst met de rechtermuisknop op het item en selecteer in het contextmenu **back-up stoppen**.

    ![Selecteer het type back-up](./media/backup-azure-delete-vault/stop-backup-item.png) 

    De back-up stoppen menu wordt geopend.

4. Op de **back-up stoppen** menu van de **kiest u een optie** selecteert u **back-upgegevens verwijderen**, typ de naam van het item en klik op **back-up stoppen**.

    Typ de naam van het item, om te controleren of dat u wilt verwijderen. De **back-up stoppen** knop wordt geactiveerd zodra u controleren of het item. Als u de gegevens worden bewaard, kunt u niet kunt verwijderen van de kluis.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Als u wilt, Geef een reden waarom u de gegevens wilt verwijderen en opmerkingen toevoegen. Om te controleren of de taak is voltooid, controleert u de Azure-berichten ![verwijderen van de back-upgegevens](./media/backup-azure-delete-vault/messages.png). <br/>
    Zodra de taak is voltooid, de service verzendt een bericht: *het back-upproces is gestopt en de back-upgegevens is verwijderd*.

5. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** de items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items worden nog steeds in de kluis weergegeven, terug naar stap 3 en kies een ander item type lijst.  

6. Wanneer er geen items meer op de werkbalk van de kluis zijn, klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.

## <a name="removing-azure-backup-server-or-dpm"></a>Azure Backup-Server of DPM verwijderen

1. Schuif omlaag naar de sectie beheren in het dashboard kluismenu en klikt u op **back-upinfrastructuur**. 

2. Klik in het submenu **back-up-beheerservers** om weer te geven van de Azure Backup-Servers en System Center DPM-server. u kunt stoppen en verwijderen van Azure bestandsservers, SQL-Servers in de virtuele machine van Azure en virtuele machines in Azure. 

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Met de rechtermuisknop op het item dat u wilt verwijderen en selecteer in het submenu **verwijderen**.

    ![Selecteer het type back-up](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    De back-up stoppen menu wordt geopend.

4. Op de **back-up stoppen** menu van de **kiest u een optie** selecteert u **back-upgegevens verwijderen**, typ de naam van het item en klik op **back-up stoppen**.

    Om te controleren of dat u wilt verwijderen, typt u de naam. De **back-up stoppen** knop wordt geactiveerd zodra u controleren of het item. Als u de gegevens worden bewaard, kunt u de kluis niet verwijderen.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    U kunt desgewenst een reden waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe opgeven. Om te controleren of de taak is voltooid, controleert u de Azure-berichten ![verwijderen van de back-upgegevens](./media/backup-azure-delete-vault/messages.png). <br/>
    Zodra de taak voltooid is, de service verzendt een bericht: het back-upproces is gestopt en de back-upgegevens is verwijderd.

5. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** de resterende items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items worden nog steeds in de kluis weergegeven, terug naar stap 3 en kies een ander item type lijst.  
6. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.


## <a name="removing-azure-backup-agent-recovery-points"></a>Herstelpunten voor Azure Backup-agent verwijderen

1. Schuif omlaag naar de sectie beheren in het dashboard kluismenu en klikt u op **back-upinfrastructuur**.

2. Klik in het submenu op **beveiligde Servers** beveiligd servertypen, met inbegrip van Azure backup-agent om de lijst met weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. In de **beveiligde Servers** lijst, klikt u op Azure Backup Agent.

    ![Selecteer het type back-up](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    De lijst met servers die zijn beveiligd met Azure backup-agent wordt geopend.

    ![Selecteer de specifieke beveiligde server](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

4. Klik in de lijst met servers op een om de menu te openen.

    ![de geselecteerde server dashboard weergeven](./media/backup-azure-delete-vault/selected-protected-server.png)

5. Klik op de geselecteerde server dashboard menu **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Op de **verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.

    Typ de naam van het item, om te controleren of dat u wilt verwijderen. De **verwijderen** knop wordt geactiveerd zodra u controleren of het item.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    U kunt desgewenst een reden waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe opgeven. Om te controleren of de taak is voltooid, controleert u de Azure-berichten ![verwijderen van de back-upgegevens](./media/backup-azure-delete-vault/messages.png). <br/>
    Zodra de taak voltooid is, de service verzendt een bericht: het back-upproces is gestopt en de back-upgegevens is verwijderd.

7. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** de resterende items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in het menu Recovery Services-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items worden nog steeds in de kluis weergegeven, terug naar stap 3 en kies een ander item type lijst.  
8. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

9. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Wat gebeurt er als ik het back-upproces stoppen, maar de gegevens behouden?

Als u het back-upproces maar per ongeluk stopt *behouden* de gegevens, moet u de back-upgegevens verwijderen voordat u de kluis kunt verwijderen. De back-upgegevens verwijderen:

1. Op de **back-ups** menu met de rechtermuisknop op het item en klik in het contextmenu op **verwijderen van de back-upgegevens**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    De **back-up-gegevens verwijderen** menu wordt geopend.
2. Op de **back-up-gegevens verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Zodra u de gegevens hebt verwijderd, terug naar stap 4c en doorgaan met het proces.
