---
title: Een Recovery Services-kluis in Azure verwijderen
description: Beschrijft hoe u een Recovery Services-kluis verwijderen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: 01c20ce84f5c97b3a0ac437fe602861085b5052c
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827877"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Dit artikel wordt beschreven hoe u verwijdert een [Azure Backup](backup-overview.md) Recovery Services-kluis. Deze bevat instructies voor het verwijderen van de afhankelijkheden en vervolgens een kluis verwijderen.


## <a name="before-you-start"></a>Voordat u begint

U kunt een Recovery Services-kluis met afhankelijkheden, zoals beveiligde servers of servers voor het beheer van back-up die is gekoppeld aan de kluis niet verwijderen.<br/>
Kluis back-upgegevens die kan niet worden verwijderd (dat wil zeggen, zelfs als u hebt gestopt met de beveiliging, maar de back-upgegevens behouden).

Als u een kluis die afhankelijkheden bevat verwijdert, worden deze de volgende fout weergegeven:

![Fout bij de kluis verwijderen](./media/backup-azure-delete-vault/error.png)

Als u wilt verwijderen zonder problemen een kluis de stappen die worden vermeld in de onderstaande volgorde:
- Stop de beveiliging en back-upgegevens verwijderen
- Verwijderen van de beveiligde servers of servers voor back-upbeheer
- De kluis verwijderen


## <a name="delete-backup-data-and-backup-items"></a>Back-upgegevens en back-upitems verwijderen

Voordat u verder verder lezen gaat **[dit ](#before-you-start)** sectie om te begrijpen van de afhankelijkheden en verwijdering-kluis.

### <a name="for-protected-items-in-cloud"></a>Voor beveiligde items in de Cloud

Beveiliging stoppen en verwijderen van de back-upgegevens, uitvoeren van de hieronder:

1. Vanuit de portal > Recovery Services-kluis > back-Upitems kiest u de beveiligde items in de cloud.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Voor elk item, moet u met de rechtermuisknop op en kies **back-up stoppen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/stop-backup-item.png)

3. In **back-up stoppen** > **Kies een optie**, selecteer **back-upgegevens verwijderen**.
4. Typ de naam van het item en klikt u op **back-up stoppen**.
   - Hiermee wordt gecontroleerd dat u wilt verwijderen van het item.
   - De **back-up stoppen** knop wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u behouden en de gegevens niet verwijderen, kunt u zich niet aan de kluis verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Controleer de **melding** ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). Na de voltooiing van de service wordt het bericht weergegeven: **Back-up stoppen en verwijderen van back-up van gegevens voor '*back-upitem*"** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** op de **back-Upitems** menu, om te controleren als het back-upitem is verwijderd.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>Voor MARS-agent

Stop de beveiliging en back-upgegevens verwijderen, voert u stappen in de onderstaande volgorde:

- [Stap 1: Back-up items verwijderen uit de MARS-beheerconsole](#step-1-delete-backup-items-from-mars-management-console)
- [Stap 2: Verwijderen uit de portal de Azure backup-agent](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>Stap 1: Back-up items verwijderen uit de MARS-beheerconsole

Neem contact op met Microsoft ondersteuning als u niet om uit te voeren van deze stap vanwege onbeschikbaarheid van de server.

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

Nu dat u hebt de back-upitems verwijderd van on-premises, voltooi de volgende stappen uit vanuit de portal.

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>Stap 2: Verwijderen uit de portal Azure Backup-Agent

Zorg ervoor dat [stap 1](#step-1-delete-backup-items-from-mars-management-console) is voltooid voordat u doorgaat:

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

> [!NOTE]
> Als u ziet de onderstaande fout en vervolgens voert u de stappen uit die worden vermeld in eerste [als u back-items uit de console Groepsbeleidsbeheer](#step-1-delete-backup-items-from-mars-management-console).
>
>![verwijderen is mislukt](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Als u niet de stappen uitvoeren om back-ups verwijderen via de beheerconsole, bijvoorbeeld vanwege onbeschikbaarheid van de server met de beheerconsole van contact op met Microsoft ondersteuning.

7. Controleer de **melding** ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). Na de voltooiing van de service wordt het bericht weergegeven: **Back-up stoppen en verwijderen van back-up van gegevens voor '*back-upitem*"** . **De bewerking is voltooid**.
8. Klik op **vernieuwen** op de **back-Upitems** menu, om te controleren als het back-upitem is verwijderd.


### <a name="for-mabs-agent"></a>Voor de MAB-agent

Stop de beveiliging en back-upgegevens verwijderen, voert u stappen in de onderstaande volgorde:

- [Stap 1: Verwijderen van back-upitems van MABS-beheerconsole](#step-1-delete-backup-items-from-mabs-management-console)
- [Stap 2: Verwijderen uit de portal Azure Backup-beheerservers](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>Stap 1: Verwijderen van back-upitems van MABS-beheerconsole

Neem contact op met Microsoft ondersteuning als u niet om uit te voeren van deze stap vanwege onbeschikbaarheid van de server.

**Methode 1** voor beveiliging stoppen en verwijderen van back-upgegevens, voer de onderstaande stappen te volgen:

1.  Klik in de DPM Administrator-Console op **Protection** op de navigatiebalk.
2.  Selecteer in het weergavepaneel lid van de beveiligingsgroep die u wilt verwijderen. Klik met de rechtermuisknop om te kiezen **stoppen van de leden van de beveiligingsgroep** optie.
3.  Uit de **beveiliging stoppen** in het dialoogvenster, selecteer **beveiligde gegevens verwijderen** > **opslag online verwijderen** selectievakje en klik vervolgens op **stoppen Beveiliging**.

    ![Opslag online verwijderen](./media/backup-azure-delete-vault/delete-storage-online.png)

De status beveiligd lid is nu gewijzigd naar **inactieve replica is beschikbaar**.

5. Met de rechtermuisknop op de inactieve beveiligingsgroep en selecteer **inactieve beveiliging verwijderen**.

    ![Inactieve beveiliging verwijderen](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Uit de **inactieve beveiliging verwijderen** venster **opslag online verwijderen** en klikt u op **Ok**.

    ![Replica's op schijf en online verwijderen](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Methode 2** Start de **MABS Management** console. In de **methode voor gegevensbeveiliging selecteren** sectie selecteert **ik Kies voor online beveiliging**.

  ![Methode voor gegevensbeveiliging selecteren](./media/backup-azure-delete-vault/data-protection-method.png)

Nu dat u hebt de back-upitems verwijderd van on-premises, voltooi de volgende stappen uit vanuit de portal.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>Stap 2: Verwijderen uit de portal Azure Backup-beheerservers

Zorg ervoor dat [stap 1](#step-1-delete-backup-items-from-mabs-management-console) is voltooid voordat u doorgaat:

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om servers weer te geven.

    ![Selecteer de kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Met de rechtermuisknop op het item > **verwijderen**.
4. Op de **verwijderen** menu, typ de naam van de server en klikt u op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Als u ziet de onderstaande fout en vervolgens voert u de stappen uit die worden vermeld in eerste [als u back-items uit de console Groepsbeleidsbeheer](#step-2-from-portal-remove-azure-backup-management-servers).
>
>![verwijderen is mislukt](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Als u niet de stappen uitvoeren om back-ups verwijderen via de beheerconsole, bijvoorbeeld vanwege onbeschikbaarheid van de server met de beheerconsole van contact op met Microsoft ondersteuning.

5. Controleer de **melding** ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png). Na de voltooiing van de service wordt het bericht weergegeven: **Back-up stoppen en verwijderen van back-up van gegevens voor '*back-upitem*"** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** op de **back-Upitems** menu, om te controleren als het back-upitem is verwijderd.


## <a name="delete-the-recovery-services-vault"></a>Recovery Services-kluis verwijderen

1. Wanneer alle afhankelijkheden zijn verwijderd, schuif naar de **Essentials** deelvenster in het kluismenu.
2. Controleren of dat er een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items nog steeds in de kluis weergegeven [verwijderen](#delete-backup-data-and-backup-items).

3. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**. De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>De Recovery Services-kluis met behulp van Azure Resource Manager-client verwijderen

Deze optie om te verwijderen van de Recovery Services-kluis wordt alleen aanbevolen als alle afhankelijkheden zijn verwijderd en er nog steeds de *fout bij verwijderen van kluis*.



- Uit de **Essentials** deelvenster in het kluismenu controleren of dat er een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als er back-upitems, voert u de stappen in [verwijderen van back-upgegevens en back-upitems](#delete-backup-data-and-backup-items).
- Probeer [de kluis verwijderen uit de portal](#delete-the-recovery-services-vault).
- Als alle afhankelijkheden zijn verwijderd en er nog steeds de *fout bij verwijderen van kluis* vervolgens ARMClient hulpprogramma gebruiken voor het uitvoeren van de stappen hieronder;

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installeer chocolatey van [hier](https://chocolatey.org/) en ARMClient uitvoeren te installeren de onderstaande opdracht:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Meld u aan bij uw Azure-account en voer deze opdracht uit:

    `ARMClient.exe login [environment name]`

3. Verzamel de abonnement-ID en resource groepsnaam voor de kluis die u wilt verwijderen in de Azure-portal.

Raadpleeg voor meer informatie over ARMClient opdracht dit [document](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Azure Resource Manager-client gebruiken om te verwijderen van de Recovery Services-kluis

1. Voer de volgende opdracht uit met behulp van uw abonnements-ID, de Resourcegroepnaam en de naam van de kluis. Wanneer u de opdracht uitvoert, worden de kluis verwijderd als u geen eventuele afhankelijkheden.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Als de kluis niet leeg is, ontvangt u de fout 'Kluis kan niet worden verwijderd omdat er bestaande resources binnen deze kluis'. Een beveiligde items verwijderen / container in een kluis, het volgende doen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Controleer of de kluis is verwijderd in de Azure-portal.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services-kluizen.<br/>
[Meer informatie over](backup-azure-manage-windows-server.md) Recovery Services-kluizen beheren en controleren.
