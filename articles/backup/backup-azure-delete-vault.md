---
title: Een Recovery Services-kluis in Azure verwijderen
description: Beschrijft hoe u een Recovery Services-kluis verwijderen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: raynew
ms.openlocfilehash: e83698af6bb1caab1568375b726753d34a8c8467
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57861346"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Dit artikel wordt beschreven hoe u verwijdert een [Azure Backup](backup-overview.md) Recovery Services-kluis. Deze bevat instructies voor afhankelijkheden verwijderen en vervolgens een kluis verwijderen en verwijderen van een kluis geforceerd beëindigd.




## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, is het belangrijk om te begrijpen dat u een Recovery Services-kluis met de servers niet verwijderen die is geregistreerd in het of die back-upgegevens bevat.


- Als u wilt verwijderen zonder problemen een kluis, unregister-servers in deze en de gegevens van de kluis verwijderen.
- Als u niet wilt behouden van gegevens in de Recovery Services-kluis en de kluis wilt verwijderen, kunt u de kluis geforceerd beëindigd verwijderen.
- Als u probeert te verwijderen van een kluis, maar niet, is nog steeds de kluis geconfigureerd voor het ontvangen van back-upgegevens.

Voor informatie over het verwijderen van een kluis, Zie de sectie [een kluis verwijderen uit Azure portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Als sectie [verwijderen van de kluis geforceerd beëindigd](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Als u niet zeker weet wat er in de kluis en u ervoor zorgen moet dat kunt u de kluis verwijderen, Zie de sectie [verwijderen kluis afhankelijkheden en de kluis verwijderen](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-the-azure-portal"></a>Een kluis verwijderen uit de Azure portal

1. Open de lijst met Recovery Services-kluizen in de portal.
2. Selecteer de kluis die u wilt verwijderen uit de lijst. Het kluisdashboard wordt geopend.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Klik in het kluisdashboard op **verwijderen**. Controleer of dat u wilt verwijderen.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

2. Als er afhankelijkheden van de kluis, de **fout bij verwijderen van kluis** wordt weergegeven: 

    ![Fout tijdens verwijderen van kluis](./media/backup-azure-delete-vault/vault-delete-error.png)

    - Volg deze instructies om afhankelijkheden te verwijderen voordat u de kluis verwijdert, controleert u
    - [Volg deze instructies](#delete-the-recovery-services-vault-by-force) PowerShell gebruiken voor het verwijderen van de kluis geforceerd beëindigd. 

## <a name="delete-the-recovery-services-vault-by-force"></a>Verwijder de Recovery Services-kluis geforceerd beëindigd

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt PowerShell gebruiken om te verwijderen van een Recovery Services-kluis geforceerd beëindigd. Dit betekent dat de kluis en alle gekoppelde back-upgegevens permanent worden verwijderd. 

> [!Warning]
> Wanneer met behulp van PowerShell een Recovery Services-kluis verwijderen, moet u controleren dat u wilt alle back-upgegevens in de kluis permanent te verwijderen.
>

Een Recovery Services-kluis verwijderen:

1. Aanmelden bij uw Azure-abonnement met de `Connect-AzAccount` opdracht in en volg de aanwijzingen aanwijzingen.

   ```powershell
    Connect-AzAccount
   ```
2. De eerste keer dat u Azure Backup gebruikt, moet u de provider Azure Recovery Service registreren in uw abonnement met [registreren AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider).

   ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

3. Open een PowerShell-venster met beheerdersbevoegdheden.
4. Gebruik `Set-ExecutionPolicy Unrestricted` beperkingen verwijderen.
5. Voer de volgende opdracht om te downloaden van het Azure Resource Manager-clientpakket van chocolately.org.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

6. Gebruik de volgende opdracht om de Azure Resource Manager API-Client te installeren.

   `choco.exe install armclient`

7. Verzamel de abonnement-ID en resource groepsnaam voor de kluis die u wilt verwijderen in de Azure-portal.
8. Voer de volgende opdracht uit met behulp van uw abonnements-ID, de Resourcegroepnaam en de naam van de kluis in PowerShell. Wanneer u de opdracht uitvoert, worden de kluis en alle afhankelijkheden verwijderd.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
9. Als de kluis niet leeg zijn, ontvangt u de fout 'Kluis kan niet worden verwijderd omdat er bestaande resources binnen deze kluis'. Als u wilt verwijderen een ingesloten in een kluis, het volgende doen:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

10. Aanmelden bij uw abonnement in Azure portal en controleer of de kluis is verwijderd.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Afhankelijkheden van de kluis verwijderen en de kluis verwijderen

U kunt handmatig de afhankelijkheden van de kluis, als volgt verwijderen:

- In de **back-Upitems** menu, remove-afhankelijkheden:
    - Back-ups van Azure Storage (Azure-bestanden)
    - SQL Server in virtuele Azure-machines
    - Back-ups van virtuele machines van Azure
- In de **back-upinfrastructuur** menu, remove-afhankelijkheden:
    - Back-ups van Microsoft Azure Backup-Server (MABS)
    - Back-ups van System Center DPM

![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

### <a name="remove-backup-items"></a>Back-up items verwijderen

Hier vindt u een voorbeeld waarin wordt uitgelegd hoe u back-upgegevens verwijderen uit Azure Files.

1. Klik op **back-up Items** > **Azure Storage (Azure-bestanden)**

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Met de rechtermuisknop op elk item Azure Files verwijderen en klik op **back-up stoppen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/stop-backup-item.png)


3. In **back-up stoppen** > **Kies een optie**, selecteer **back-upgegevens verwijderen**.
4. Typ de naam van het item en klikt u op **back-up stoppen**. 
   - Hiermee wordt gecontroleerd dat u wilt verwijderen van het item.
   - De **back-up stoppen** knop wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u behouden en de gegevens niet verwijderen, kunt u zich niet aan de kluis verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.
6. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
7. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
8. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>Een back-upinfrastructuur servers verwijderen

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om servers weer te geven. 

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

2. Met de rechtermuisknop op het item > **verwijderen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. . In **back-up stoppen** > **Kies een optie**, selecteer **back-upgegevens verwijderen**.
4. Typ de naam van het item en klikt u op **back-up stoppen**. 
   - Hiermee wordt gecontroleerd dat u wilt verwijderen van het item.
   - De **back-up stoppen** knop wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u behouden en de gegevens niet verwijderen, kunt u zich niet aan de kluis verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.
6. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
7. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
8. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.


### <a name="remove-azure-backup-agent-recovery-points"></a>Herstelpunten voor Azure backup-agent verwijderen

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om de infrastructuurservers weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. In de **beschermde Servers** lijst, klikt u op Azure Backup-Agent.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Klik op de server in de lijst met servers die worden beveiligd met behulp van Azure backup-agent.

    ![Selecteer de beveiligde server](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Klik op het dashboard van de geselecteerde server, **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Op de **verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.
   - Hiermee wordt gecontroleerd dat u wilt verwijderen van het item.
   - De **back-up stoppen** knop wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u behouden en de gegevens niet verwijderen, kunt u zich niet aan de kluis verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.
8. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
7. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
8. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.



### <a name="delete-the-vault-after-removing-dependencies"></a>De kluis verwijderen na het verwijderen van afhankelijkheden

1. Wanneer alle afhankelijkheden zijn verwijderd, schuif naar de **Essentials** deelvenster in het kluismenu.

    - Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld.
    - Als items is nog steeds worden weergegeven in de kluis, verwijdert u deze.

2. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**. De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Wat gebeurt er als ik het back-upproces te stoppen, maar blijven de gegevens bewaard?

Als u het back-upproces te stoppen, maar blijven de gegevens per ongeluk bewaard, moet u de back-upgegevens verwijderen, zoals beschreven in de vorige secties.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services-kluizen.
