---
title: Een Recovery Services kluis verwijderen in azure
description: Hierin wordt beschreven hoe u een Recovery Services kluis verwijdert.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: f98b9a02d12cc53ba23857b203ee3eaed9dd7cfa
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466645"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

In dit artikel wordt beschreven hoe u een [Azure Backup](backup-overview.md) Recovery Services kluis verwijdert. Het bevat instructies voor het verwijderen van afhankelijkheden en het verwijderen van een kluis.


## <a name="before-you-start"></a>Voordat u begint

U kunt een Recovery Services kluis met afhankelijkheden zoals beveiligde servers of back-upbeheer servers die zijn gekoppeld aan de kluis, niet verwijderen.<br/>
Een kluis met back-upgegevens kan niet worden verwijderd (dat wil zeggen, zelfs als u de beveiliging hebt gestopt, maar de back-upgegevens hebt behouden).

Als u een kluis verwijdert die afhankelijkheden bevat, wordt de volgende fout weer gegeven:

![fout bij verwijderen van kluis](./media/backup-azure-delete-vault/error.png)

Als u de kluis zonder problemen wilt verwijderen, kiest u het scenario dat overeenkomt met uw installatie en voert u de aanbevolen stappen uit:

Scenario | Stappen voor het verwijderen van afhankelijkheden om de kluis te verwijderen |
-- | --
Ik heb on-premises bestanden en mappen die zijn beveiligd met Azure Backup-Agent (MARS) back-up naar Azure | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor Mars-agent](#for-mars-agent)
Ik heb on-premises machines die zijn beveiligd met MABS (Microsoft Azure Backup Server) of DPM naar Azure (System Center Data Protection Manager) | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor MABS-agent](#for-mabs-agent)
Ik heb beveiligde items in de Cloud (bijvoorbeeld laaS VM, Azure-bestands share, enzovoort)  | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor beveiligde items in de Cloud](#for-protected-items-in-cloud)
Ik heb beveiligde items in zowel on-premises als in de Cloud | Voer de stappen in back-upgegevens en back-upitems verwijderen in de onderstaande volg orde uit: <br> - [Voor beveiligde items in de Cloud](#for-protected-items-in-cloud)<br> - [Voor MABS-agent](#for-mars-agent) <br> - [Voor MABS-agent](#for-mabs-agent)
Ik heb geen beveiligde items on-premises of in de Cloud. Ik krijg echter nog steeds de fout melding voor het verwijderen van de kluis | Voer de stappen in [de Recovery Services kluis verwijderen met behulp van Azure Resource Manager-client](#delete-the-recovery-services-vault-using-azure-resource-manager-client)
Ik heb die lokale on-premises server niet meer (verloren/buiten gebruik gesteld) en ik wil de Recovery Services kluis verwijderen | Neem contact op met micro soft ondersteuning.

## <a name="delete-backup-data-and-backup-items"></a>Back-upgegevens en back-upitems verwijderen

Voordat u verder gaat, leest u **[deze](#before-you-start)** sectie om inzicht te krijgen in de afhankelijkheden en het verwijderings proces van de kluis.

### <a name="for-protected-items-in-cloud"></a>Voor beveiligde items in de Cloud

Voer de onderstaande stappen uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

1. Vanuit de portal > Recovery Services kluis > Back-upitems de beveiligde items in de Cloud kiezen.

    ![het back-uptype selecteren](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Voor elk item moet u met de rechter muisknop klikken en **back-up stoppen**kiezen.

    ![het back-uptype selecteren](./media/backup-azure-delete-vault/stop-backup-item.png)

3. Kies in **back-up** > stoppen**een optie**selecteren **back-upgegevens verwijderen**.
4. Typ de naam van het item en klik op **back-up stoppen**.
   - Hiermee wordt gecontroleerd of u het item wilt verwijderen.
   - De knop **back-up stoppen** wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u de gegevens behoudt en niet verwijdert, kunt u de kluis niet verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Controleer de **melding** ![back-upgegevens](./media/backup-azure-delete-vault/messages.png)verwijderen. Na het volt ooien van de service wordt het volgende bericht weer gegeven: **Back-up stoppen en back-upgegevens verwijderen voor '*back-upitem*'** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** in het menu **back-** upitems om te controleren of de back-up is verwijderd.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>Voor MARS-agent

Voer de stappen in de onderstaande volg orde uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

- [Stap 1: Back-upitems verwijderen uit de MARS-beheer console](#step-1-delete-backup-items-from-mars-management-console)
- [Stap 2: Azure Backup Agent verwijderen uit Portal](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>Stap 1: Back-upitems verwijderen uit de MARS-beheer console

Neem contact op met micro soft ondersteuning als u deze stap niet kunt uitvoeren vanwege een niet-beschik baarheid van de server.

- Start de MARS Management-console, ga naar het deel venster **acties** en kies **back-up plannen**.
- Als u **een geplande back-up wizard wilt wijzigen of stoppen** , kiest u de optie **stoppen met dit back-upschema en verwijdert u alle opgeslagen back-ups** en klikt u op **volgende**.

    ![Een geplande back-up wijzigen of stoppen](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Klik op **volt ooien**om **een geplande back-up** wizard te stoppen.

    ![Een geplande back-up stoppen](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- U wordt gevraagd om een beveiligings pincode in te voeren. Voer de onderstaande stappen uit om de pincode te genereren:
  - Meld u aan bij Azure Portal.
  - Blader naar **Recovery Services** > **Eigenschappen**van de kluis**instellingen** > .
  - Klik onder **BEVEILIGINGS pincode**op **genereren**. Deze pincode kopiëren. (Deze pincode is slechts vijf minuten geldig)
- Plak de pincode in de beheer console (client-app) en klik op **OK**.

  ![Beveiligings pincode](./media/backup-azure-delete-vault/security-pin.png)

- In de wizard **voortgang van back-up wijzigen** ziet *u dat verwijderde back-upgegevens 14 dagen bewaard blijven. Na die tijd worden de back-upgegevens permanent verwijderd.*  

    ![Back-upinfrastructuur verwijderen](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nu u de back-upitems van on-premises hebt verwijderd, voert u de volgende stappen uit vanuit de portal.

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>Stap 2: Azure Backup Agent verwijderen uit Portal

Zorg ervoor dat [stap 1](#step-1-delete-backup-items-from-mars-management-console) is voltooid voordat u verder gaat:

1. Klik in het menu van het kluis dashboard op **back-upinfrastructuur**.
2. Klik op **beveiligde servers** om de infrastructuur servers weer te geven.

    ![Selecteer uw kluis om het dash board ervan te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. Klik in de lijst met **beveiligde servers** op Azure backup agent.

    ![het back-uptype selecteren](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Klik op de server in de lijst met servers die zijn beveiligd met Azure Backup Agent.

    ![de specifieke beveiligde server selecteren](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Klik op het dash board van de geselecteerde server op **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Typ in het menu **verwijderen** de naam van de server en klik op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Als u de onderstaande fout ziet, voert u eerst de stappen uit die worden beschreven in [Back-upitems verwijderen uit de beheer console](#step-1-delete-backup-items-from-mars-management-console).
>
>![kan niet verwijderen](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Neem contact op met micro soft ondersteuning als u de stappen voor het verwijderen van back-ups uit de beheer console niet kunt uitvoeren, bijvoorbeeld vanwege een niet-beschik baarheid van de server met de beheer console.

7. Controleer de **melding** ![back-upgegevens](./media/backup-azure-delete-vault/messages.png)verwijderen. Na het volt ooien van de service wordt het volgende bericht weer gegeven: **Back-up stoppen en back-upgegevens verwijderen voor '*back-upitem*'** . **De bewerking is voltooid**.
8. Klik op **vernieuwen** in het menu **back-** upitems om te controleren of de back-up is verwijderd.


### <a name="for-mabs-agent"></a>Voor MABS-agent

Voer de stappen in de onderstaande volg orde uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

- [Stap 1: Back-upitems verwijderen uit de MABS-beheer console](#step-1-delete-backup-items-from-mabs-management-console)
- [Stap 2: Azure Backup beheerser vers verwijderen uit Portal](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>Stap 1: Back-upitems verwijderen uit de MABS-beheer console

Neem contact op met micro soft ondersteuning als u deze stap niet kunt uitvoeren vanwege een niet-beschik baarheid van de server.

**Methode 1** Voer de onderstaande stappen uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

1.  Klik in DPM Administrator-console op **beveiliging** op de navigatie balk.
2.  Selecteer in het weergave paneel het lid van de beveiligings groep dat u wilt verwijderen. Klik met de rechter muisknop om de optie **beveiliging van groeps leden stoppen** te kiezen.
3.  Schakel in het dialoog venster **beveiliging stoppen** het selectie vakje **beveiligde gegevens** > verwijderen**opslag online verwijderen** in en klik vervolgens op **beveiliging stoppen**.

    ![Opslag online verwijderen](./media/backup-azure-delete-vault/delete-storage-online.png)

De status van de beveiligde leden is nu gewijzigd in **niet-actieve replica beschikbaar**.

5. Klik met de rechter muisknop op inactieve beveiligings groep en selecteer **inactieve beveiliging verwijderen**.

    ![Inactieve beveiliging verwijderen](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Selecteer in het venster **inactieve beveiliging verwijderen** de optie **online opslag verwijderen** en klik op **OK**.

    ![Replica's op schijf en online verwijderen](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Methode 2** Start de **MABS-beheer** console. Selecteer **Ik wil online beveiliging**in het gedeelte **methode voor gegevens beveiliging selecteren** .

  ![methode voor gegevens beveiliging selecteren](./media/backup-azure-delete-vault/data-protection-method.png)

Nu u de back-upitems van on-premises hebt verwijderd, voert u de volgende stappen uit vanuit de portal.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>Stap 2: Azure Backup beheerser vers verwijderen uit Portal

Zorg ervoor dat [stap 1](#step-1-delete-backup-items-from-mabs-management-console) is voltooid voordat u verder gaat:

1. Klik in het menu van het kluis dashboard op **back-upinfrastructuur**.
2. Klik op **back-upbeheer servers** om servers te bekijken.

    ![kluis selecteren om het dash board ervan te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Klik met de rechter muisknop op het item > **verwijderen**.
4. Typ in het menu **verwijderen** de naam van de server en klik op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Als u de onderstaande fout ziet, voert u eerst de stappen uit die worden beschreven in [Back-upitems verwijderen uit de beheer console](#step-2-from-portal-remove-azure-backup-management-servers).
>
>![kan niet verwijderen](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Neem contact op met micro soft ondersteuning als u de stappen voor het verwijderen van back-ups uit de beheer console niet kunt uitvoeren, bijvoorbeeld vanwege een niet-beschik baarheid van de server met de beheer console.

5. Controleer de **melding** ![back-upgegevens](./media/backup-azure-delete-vault/messages.png)verwijderen. Na het volt ooien van de service wordt het volgende bericht weer gegeven: **Back-up stoppen en back-upgegevens verwijderen voor '*back-upitem*'** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** in het menu **back-** upitems om te controleren of de back-up is verwijderd.


## <a name="delete-the-recovery-services-vault"></a>Recovery Services-kluis verwijderen

1. Wanneer alle afhankelijkheden zijn verwijderd, bladert u naar het deel venster **essentiële** elementen in het menu kluis.
2. Controleer of er geen **back-** upitems, **back-upbeheer servers**of **gerepliceerde items** worden weer gegeven. Als er nog items in de kluis worden weer gegeven, [verwijdert u deze](#delete-backup-data-and-backup-items).

3. Als er geen items meer zijn in de kluis, klikt u op het kluis dashboard op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Klik op **Ja**om te controleren of u de kluis wilt verwijderen. De kluis wordt verwijderd en de portal keert terug naar het menu **nieuwe** service.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>De Recovery Services kluis verwijderen met behulp van Azure Resource Manager-client

Deze optie voor het verwijderen van de Recovery Services kluis wordt alleen aanbevolen wanneer alle afhankelijkheden worden verwijderd en er nog steeds een *fout optreedt*bij het verwijderen van de kluis.



- Controleer in  het deel venster Essentials in het menu kluis of er geen **back-** upitems, **back-upbeheer servers**of **gerepliceerde items** worden weer gegeven. Als er back-upitems zijn, voert u de stappen in [back-upgegevens verwijderen en back-](#delete-backup-data-and-backup-items)upitems uit.
- Probeer [de kluis opnieuw te verwijderen uit de portal](#delete-the-recovery-services-vault).
- Als alle afhankelijkheden worden verwijderd en u nog steeds de *kluis verwijdert* , gebruikt u het hulp programma ARMClient om de volgende stappen uit te voeren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installeer de Choco lade vanaf [hier](https://chocolatey.org/) en om ARMClient te installeren, voert u de onderstaande opdracht uit:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Meld u aan bij uw Azure-account en voer deze opdracht uit:

    `ARMClient.exe login [environment name]`

3. Verzamel in het Azure Portal de abonnements-ID en de naam van de resource groep voor de kluis die u wilt verwijderen.

Raadpleeg dit [document](https://github.com/projectkudu/ARMClient/blob/master/README.md)voor meer informatie over de opdracht ARMClient.

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Azure Resource Manager-client gebruiken om Recovery Services kluis te verwijderen

1. Voer de volgende opdracht uit met de abonnements-ID, de naam van de resource groep en de naam van de kluis. Wanneer u de opdracht uitvoert, wordt de kluis verwijderd als er geen afhankelijkheden zijn.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Als de kluis niet leeg is, ontvangt u de fout "kluis kan niet worden verwijderd omdat er bestaande resources zijn in deze kluis". Ga als volgt te werk om een beveiligde items/container in een kluis te verwijderen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Controleer in de Azure Portal of de kluis is verwijderd.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services kluizen.<br/>
[Meer informatie over](backup-azure-manage-windows-server.md) Recovery Services kluizen bewaken en beheren.
