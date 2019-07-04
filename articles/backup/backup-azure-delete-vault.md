---
title: Een Recovery Services-kluis in Azure verwijderen
description: Beschrijft hoe u een Recovery Services-kluis verwijderen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: raynew
ms.openlocfilehash: e195d9a4b9d2bbe21848e083dbccf864188e0790
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508409"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Dit artikel wordt beschreven hoe u verwijdert een [Azure Backup](backup-overview.md) Recovery Services-kluis. Deze bevat instructies voor afhankelijkheden verwijderen en vervolgens een kluis verwijderen en verwijderen van een kluis geforceerd beëindigd.


## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, is het belangrijk om te begrijpen dat u een Recovery Services-kluis met de servers niet verwijderen die is geregistreerd in het of die back-upgegevens bevat.

- Als u wilt verwijderen zonder problemen een kluis, servers die deze bevat de registratie ongedaan maken, gegevens van de kluis verwijderen en verwijder vervolgens de kluis.
- Als u probeert te verwijderen van een kluis waarvoor nog steeds afhankelijkheden, een foutbericht dat wordt uitgegeven en moet u handmatig verwijderen van de kluis afhankelijkheden, met inbegrip van:
    - Back-ups van items
    - Beveiligde servers
    - Back-up van beheerservers (Azure Backup Server, DPM) ![selecteert u de kluis om het dashboard te openen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Als u niet wilt behouden van gegevens in de Recovery Services-kluis en de kluis wilt verwijderen, kunt u de kluis geforceerd beëindigd verwijderen.
- Als u probeert te verwijderen van een kluis, maar niet, is nog steeds de kluis geconfigureerd voor het ontvangen van back-upgegevens.


## <a name="delete-a-vault-from-the-azure-portal"></a>Een kluis verwijderen uit de Azure portal

1. Open het dashboard van de kluis.  
2. Klik in het dashboard op **verwijderen**. Controleer of dat u wilt verwijderen.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Als u een foutbericht krijgt, verwijdert u [back-up items](#remove-backup-items), [infrastructuurservers](#remove-azure-backup-management-servers), en [herstelpunten](#remove-azure-backup-agent-recovery-points), en verwijder vervolgens de kluis.

![Fout bij de kluis verwijderen](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>De Recovery Services-kluis met behulp van Azure Resource Manager-client verwijderen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installeer chocolatey van [hier](https://chocolatey.org/) en ARMClient uitvoeren te installeren de onderstaande opdracht:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Meld u aan bij uw Azure-account en voer deze opdracht uit:

    `ARMClient.exe login [environment name]`

3. Verzamel de abonnement-ID en resource groepsnaam voor de kluis die u wilt verwijderen in de Azure-portal.

Raadpleeg voor meer informatie over ARMClient opdracht dit [document](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Azure Resource Manager-client gebruiken om te verwijderen van de Recovery Services-kluis

1. Voer de volgende opdracht uit met behulp van uw abonnements-ID, de Resourcegroepnaam en de naam van de kluis. Wanneer u de opdracht uitvoert worden de kluis verwijderd als u geen eventuele afhankelijkheden.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Als de kluis niet leeg zijn, ontvangt u de fout 'Kluis kan niet worden verwijderd omdat er bestaande resources binnen deze kluis'. Een beveiligde items verwijderen / container in een kluis, het volgende doen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Controleer of de kluis is verwijderd in de Azure-portal.


## <a name="remove-vault-items-and-delete-the-vault"></a>Items van de kluis verwijderen en de kluis verwijderen

Verwijder alle afhankelijkheden voordat de Recovery Services-kluis wordt verwijderd.

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

## <a name="deleting-backup-items-from-management-console"></a>Als u back-items uit de console Groepsbeleidsbeheer

Als de back-up items verwijderen uit een back-upinfrastructuur, gaat u naar uw on-premises server Management Console (MARS, Azure Backup-Server of SC DPM, afhankelijk van waar uw back-items zijn beveiligd).

### <a name="for-mars-agent"></a>Voor MARS-agent

- Start de MARS-beheerconsole, gaat u naar de **acties** deelvenster en kies **back-up plannen**.
- Van **wijzigen of een geplande back-up stoppen** wizard, kiest u de optie **stoppen met het gebruik van deze back-upschema en verwijderen van alle opgeslagen back-ups** en klikt u op **volgende**.

    ![Wijzigen of een geplande back-up stoppen](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Van **een geplande back-up stoppen** wizard, klikt u op **voltooien**.

    ![Een geplande back-up stoppen](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- U wordt gevraagd een pincode voor beveiliging op te geven. Voor het genereren van de PINCODE, voer de onderstaande stappen te volgen:
  - Meld u aan bij Azure Portal.
  - Blader naar **Recovery Services-kluis** > **instellingen** > **eigenschappen**.
  - Onder **BEVEILIGINGSPINCODE**, klikt u op **genereren**. Kopieer deze PINCODE. (Deze PINCODE is geldig voor slechts vijf minuten)
- Plak de PINCODE in de beheerconsole voor (client-app) en klikt u op **Ok**.

  ![Beveiligingspincode](./media/backup-azure-delete-vault/security-pin.png)

- In de **back-up uitgevoerd wijzigen** wizard u ziet *verwijderde back-upgegevens worden 14 dagen worden bewaard. Na deze periode, back-gegevens worden definitief verwijderd.*  

    ![Een back-upinfrastructuur verwijderen](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nu dat u hebt de back-upitems verwijderd van on-premises, voer de volgende stappen uit vanuit de portal:
- Voor MARS-Volg de stappen in [herstelpunten verwijderen Azure Backup-agent](#remove-azure-backup-agent-recovery-points)

### <a name="for-mabs-agent"></a>Voor de MAB-agent

Er zijn verschillende methoden voor online beveiliging stoppen en verwijderen, voert u een van de onderstaande methoden:

**Methode 1**

Start de **MABS Management** console. In de **methode voor gegevensbeveiliging selecteren** sectie selecteert **ik Kies voor online beveiliging**.

  ![Methode voor gegevensbeveiliging selecteren](./media/backup-azure-delete-vault/data-protection-method.png)

**Methode 2**

Als u wilt verwijderen van een beveiligingsgroep, moet u eerst beveiliging van de groep stoppen. Gebruik de volgende procedure voor beveiliging stoppen en verwijderen van een beveiligingsgroep inschakelen.

1.  Klik in de DPM Administrator-Console op **Protection** op de navigatiebalk.
2.  Selecteer in het weergavepaneel lid van de beveiligingsgroep die u wilt verwijderen. Klik met de rechtermuisknop om te kiezen **stoppen van de leden van de beveiligingsgroep** optie.
3.  Uit de **beveiliging stoppen** in het dialoogvenster, selecteer **beveiligde gegevens verwijderen** > **opslag online verwijderen** selectievakje en klik vervolgens op **stoppen Beveiliging**.

    ![Opslag online verwijderen](./media/backup-azure-delete-vault/delete-storage-online.png)

De status beveiligd lid is nu gewijzigd naar **inactieve replica is beschikbaar**.

5. Met de rechtermuisknop op de inactieve beveiligingsgroep en selecteer **inactieve beveiliging verwijderen**.

    ![Inactieve beveiliging verwijderen](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Uit de **inactieve beveiliging verwijderen** venster **opslag online verwijderen** en klikt u op **Ok**.

    ![Replica's op schijf en online verwijderen](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

Nu dat u hebt de back-upitems verwijderd van on-premises, voer de volgende stappen uit vanuit de portal:
- Voor MABS en DPM Volg de stappen in [verwijderen Azure Backup-beheerservers](#remove-azure-backup-management-servers).


### <a name="remove-azure-backup-management-servers"></a>Azure Backup-beheerservers verwijderen

Voordat u de Azure-back-beheerserver verwijdert, zorg ervoor dat u het uitvoeren van de stappen in [als u back-items uit de console Groepsbeleidsbeheer](#deleting-backup-items-from-management-console).

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om servers weer te geven.

    ![Selecteer de kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Met de rechtermuisknop op het item > **verwijderen**.
4. Op de **verwijderen** menu, typ de naam van de server en klikt u op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)
5.  (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.

> [!NOTE]
> Als u ziet de onderstaande fout en vervolgens voert u de stappen uit die worden vermeld in eerste [als u back-items uit de console Groepsbeleidsbeheer](#deleting-backup-items-from-management-console).
>
>![verwijderen is mislukt](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Als u niet de stappen uitvoeren om back-ups verwijderen via de beheerconsole, bijvoorbeeld vanwege onbeschikbaarheid van de server met de beheerconsole van contact op met Microsoft ondersteuning.

6. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
7. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
8. Na het verwijderen van een item in de lijst op de **back-upinfrastructuur** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.


### <a name="remove-azure-backup-agent-recovery-points"></a>Herstelpunten voor Azure backup-agent verwijderen

Voordat u de Azure back-upherstelpunt verwijdert, zorg ervoor dat u het uitvoeren van de stappen in [als u back-items uit de console Groepsbeleidsbeheer](#deleting-backup-items-from-management-console).

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **beschermde Servers** om de infrastructuurservers weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. In de **beschermde Servers** lijst, klikt u op Azure Backup-Agent.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Klik op de server in de lijst met servers die worden beveiligd met behulp van Azure backup-agent.

    ![Selecteer de beveiligde server](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Klik op het dashboard van de geselecteerde server, **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Op de **verwijderen** menu, typ de naam van de server en klikt u op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.

> [!NOTE]
> Als u ziet de onderstaande fout en vervolgens voert u de stappen uit die worden vermeld in eerste [als u back-items uit de console Groepsbeleidsbeheer](#deleting-backup-items-from-management-console).
>
>![verwijderen is mislukt](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Als u niet de stappen uitvoeren om back-ups verwijderen via de beheerconsole, bijvoorbeeld vanwege onbeschikbaarheid van de server met de beheerconsole van contact op met Microsoft ondersteuning. 

8. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
9. Na het verwijderen van een item in de lijst op de **back-upinfrastructuur** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.


### <a name="delete-the-vault-after-removing-dependencies"></a>De kluis verwijderen na het verwijderen van afhankelijkheden

1. Wanneer alle afhankelijkheden zijn verwijderd, schuif naar de **Essentials** deelvenster in het kluismenu.
2. Controleren of dat er een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items is nog steeds worden weergegeven in de kluis, verwijdert u deze.

3. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**. De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Wat gebeurt er als ik het back-upproces te stoppen, maar blijven de gegevens bewaard?

Als u het back-upproces te stoppen, maar blijven de gegevens per ongeluk bewaard, moet u de back-upgegevens verwijderen, zoals beschreven in de vorige secties.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services-kluizen.
