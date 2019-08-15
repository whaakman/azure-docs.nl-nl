---
title: Een Recovery Services kluis verwijderen in azure
description: Hierin wordt beschreven hoe u een Recovery Services kluis verwijdert.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: dacurwin
ms.openlocfilehash: 9c63170b60a871182042acab8a35e505c603f260
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69018876"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

In dit artikel wordt beschreven hoe u een [Azure Backup](backup-overview.md) Recovery Services kluis verwijdert. Het bevat instructies voor het verwijderen van afhankelijkheden en het verwijderen van een kluis.


## <a name="before-you-start"></a>Voordat u begint

U kunt een Recovery Services kluis met afhankelijkheden zoals beveiligde servers of back-upbeheer servers die zijn gekoppeld aan de kluis, niet verwijderen.

- Een kluis met back-upgegevens kan niet worden verwijderd (dat wil zeggen, zelfs als u de beveiliging hebt gestopt, maar de back-upgegevens hebt behouden).

- Als u een kluis verwijdert die afhankelijkheden bevat, wordt de volgende fout weer gegeven:

  ![Fout bij verwijderen van kluis](./media/backup-azure-delete-vault/error.png)

- Als u een on-premises beveiligd item (MARS, MABS of DPM naar Azure) verwijdert vanuit een portal met afhankelijkheden, wordt er een waarschuwings bericht weer gegeven:

  ![Fout bij verwijderen van beveiligde server](./media/backup-azure-delete-vault/error-message.jpg)

  
Als u de kluis zonder problemen wilt verwijderen, kiest u het scenario dat overeenkomt met uw installatie en voert u de aanbevolen stappen uit:

Scenario | Stappen voor het verwijderen van afhankelijkheden om de kluis te verwijderen |
-- | --
Ik heb on-premises bestanden en mappen die zijn beveiligd met Azure Backup-Agent (MARS) back-up naar Azure | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor Mars-agent](#delete-backup-items-from-mars-management-console)
Ik heb on-premises machines die zijn beveiligd met MABS (Microsoft Azure Backup Server) of DPM naar Azure (System Center Data Protection Manager) | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor MABS-agent](#delete-backup-items-from-mabs-management-console)
Ik heb beveiligde items in de Cloud (bijvoorbeeld laaS VM, Azure-bestands share, enzovoort)  | Voer de stappen in back-upgegevens verwijderen en back-upitems uit- [voor beveiligde items in de Cloud](#delete-protected-items-in-cloud)
Ik heb beveiligde items in zowel on-premises als in de Cloud | Voer de stappen in back-upgegevens en back-upitems verwijderen in de onderstaande volg orde uit: <br> - [Voor beveiligde items in de Cloud](#delete-protected-items-in-cloud)<br> - [Voor MARS-agent](#delete-backup-items-from-mars-management-console) <br> - [Voor MABS-agent](#delete-backup-items-from-mabs-management-console)
Ik heb geen beveiligde items on-premises of in de Cloud. Ik krijg echter nog steeds de fout melding voor het verwijderen van de kluis | Voer de stappen in [de Recovery Services kluis verwijderen met behulp van Azure Resource Manager-client](#delete-the-recovery-services-vault-using-azure-resource-manager-client)


## <a name="delete-protected-items-in-cloud"></a>Beveiligde items in de Cloud verwijderen

Voordat u doorgaat, leest u **[deze](#before-you-start)** sectie om inzicht te krijgen in de afhankelijkheden en het verwijderings proces van de kluis.

Voer de onderstaande stappen uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

1. Kies in Portal > **Recovery Services kluis** > **back-** upitems de beveiligde items in de Cloud (bijvoorbeeld virtuele machine, Azure Storage (Azure files), SQL op Azure VM, enzovoort).

    ![het back-uptype selecteren](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. Klik met de rechter muisknop op het back-upitem. Afhankelijk van of het back-upitem is beveiligd of niet, wordt de back **-up stoppen** of **back-upgegevens verwijderen**weer gegeven in het menu.

    - Selecteer back-upgegevens **verwijderen** in de vervolg keuzelijst voor het stoppen van de **back-up**. Voer de **naam** van het back-upitem in (hoofdletter gevoelig), selecteer een **reden**, Voer **opmerkingen**in en klik op **back-up stoppen**.

        ![het back-uptype selecteren](./media/backup-azure-delete-vault/stop-backup-item.png)

    - Voor het **verwijderen van back-upgegevens**voert u de naam van het back-upitem in (hoofdletter gevoelig), selecteert u een **reden**, voert u **opmerkingen**in en klikt u op **verwijderen**. 

         ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Controleer de **melding** ![back-upgegevens](./media/backup-azure-delete-vault/messages.png)verwijderen. Na het volt ooien van de service wordt het volgende bericht weer gegeven: **Back-up stoppen en back-upgegevens verwijderen voor '*back-upitem*'** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** in het menu **back-** upitems om te controleren of de back-up is verwijderd.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>Beveiligde items on-premises verwijderen

Voordat u verder gaat, leest u **[deze](#before-you-start)** sectie om inzicht te krijgen in de afhankelijkheden en het verwijderings proces van de kluis.

1. Klik in het menu van het kluis dashboard op **back-upinfrastructuur**.
2. Afhankelijk van uw on-premises scenario kiest u de onderstaande optie:

      - Voor **Azure backup-agent**kiest u **beveiligde servers** > **Azure backup agent** en selecteert u de server die u wilt verwijderen. 

        ![Selecteer uw kluis om het dash board ervan te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - Kies voor **Azure backup server**/**DPM** **back-upbeheer servers**. Selecteer de server die u wilt verwijderen. 


          ![kluis selecteren om het dash board ervan te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. De Blade **verwijderen** wordt weer gegeven met het waarschuwings bericht.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server.png)

     Controleer het waarschuwings bericht en de instructies in het dialoog venster toestemming.
    
    > [!NOTE]
    >- Als de beveiligde server is gesynchroniseerd met de Azure-service en er back-upitems bestaan, worden in het selectie vakje toestemming het aantal afhankelijke back-upitems en de koppeling voor het weer geven van de back-upitems weer gegeven.
    >- Als de beveiligde server niet is gesynchroniseerd met de Azure-service en er back-upitems bestaan, wordt in het selectie vakje toestemming het aantal back-upitems weer gegeven.
    >- Als er geen back-upitems zijn, wordt in het selectie vakje toestemming gevraagd om te verwijderen.

4. Schakel het selectie vakje toestemming in en klik op **verwijderen**.




5. Controleer de **melding** ![back-upgegevens](./media/backup-azure-delete-vault/messages.png)verwijderen. Na het volt ooien van de service wordt het volgende bericht weer gegeven: **Back-up stoppen en back-upgegevens verwijderen voor '*back-upitem*'** . **De bewerking is voltooid**.
6. Klik op **vernieuwen** in het menu **back-** upitems om te controleren of de back-up is verwijderd.

U kunt nu door gaan met het verwijderen van de back-upitems uit de beheer console:
    
   - [Items die zijn beveiligd met MARS](#delete-backup-items-from-mars-management-console)
    - [Items die zijn beveiligd met MABS](#delete-backup-items-from-mabs-management-console)


### <a name="delete-backup-items-from-mars-management-console"></a>Back-upitems verwijderen uit de MARS-beheer console

Back-upitems verwijderen uit de MARS-beheer console

- Start de MARS Management-console, ga naar het deel venster **acties** en kies **back-up plannen**.
- Als u **een geplande back-up wizard wilt wijzigen of stoppen** , kiest u de optie **stoppen met dit back-upschema en verwijdert u alle opgeslagen back-ups** en klikt u op **volgende**.

    ![Een geplande back-up wijzigen of stoppen](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Klik op **volt ooien**om **een geplande back-up** wizard te stoppen.

    ![Een geplande back-up stoppen](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- U wordt gevraagd om een beveiligings pincode in te voeren. Voer de onderstaande stappen uit om de pincode te genereren:
  - Meld u aan bij Azure Portal.
  - Blader naar **Recovery Services** > **Eigenschappen**van de kluis**instellingen** > .
  - Klik onder **BEVEILIGINGS pincode**op **genereren**. Deze pincode kopiëren. (Deze pincode is slechts vijf minuten geldig.)
- Plak de pincode in de beheer console (client-app) en klik op **OK**.

  ![Beveiligings pincode](./media/backup-azure-delete-vault/security-pin.png)

- In de wizard **voortgang van back-up wijzigen** ziet *u dat verwijderde back-upgegevens 14 dagen bewaard blijven. Na die tijd worden de back-upgegevens permanent verwijderd.*  

    ![Back-upinfrastructuur verwijderen](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nu u de back-upitems van on-premises hebt verwijderd, voert u de volgende stappen uit vanuit de portal.

### <a name="delete-backup-items-from-mabs-management-console"></a>Back-upitems verwijderen uit de MABS-beheer console

Back-upitems verwijderen uit de MABS-beheer console

**Methode 1** Voer de onderstaande stappen uit om de beveiliging te stoppen en de back-upgegevens te verwijderen:

1.  Klik in DPM Administrator-console op **beveiliging** op de navigatie balk.
2.  Selecteer in het weergave paneel het lid van de beveiligings groep dat u wilt verwijderen. Klik met de rechter muisknop om de optie **beveiliging van groeps leden stoppen** te kiezen.
3.  Schakel in het dialoog venster **beveiliging stoppen** het selectie vakje **beveiligde gegevens** > verwijderen**opslag online verwijderen** in en klik vervolgens op **beveiliging stoppen**.

    ![Opslag online verwijderen](./media/backup-azure-delete-vault/delete-storage-online.png)

De status van de beveiligde leden is nu gewijzigd in **niet-actieve replica beschikbaar**.

4. Klik met de rechter muisknop op inactieve beveiligings groep en selecteer **inactieve beveiliging verwijderen**.

    ![Inactieve beveiliging verwijderen](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. Selecteer in het venster **inactieve beveiliging verwijderen** de optie **online opslag verwijderen** en klik op **OK**.

    ![Replica's op schijf en online verwijderen](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Methode 2** Start de **MABS-beheer** console. Schakel in de sectie **methode voor gegevens beveiliging selecteren** de optie **Ik wil online beveiliging**uit.

  ![methode voor gegevens beveiliging selecteren](./media/backup-azure-delete-vault/data-protection-method.png)

Nu u de back-upitems van on-premises hebt verwijderd, voert u de volgende stappen uit vanuit de portal.


## <a name="delete-the-recovery-services-vault"></a>Recovery Services-kluis verwijderen

1. Wanneer alle afhankelijkheden zijn verwijderd, bladert u naar het deel venster **essentiële** elementen in het menu kluis.
2. Controleer of er geen **back-** upitems, **back-upbeheer servers**of **gerepliceerde items** worden weer gegeven. Als items nog steeds in de kluis worden weer gegeven, raadpleegt u de sectie [voordat u begint](#before-you-start) .

3. Als er geen items meer zijn in de kluis, klikt u op het kluis dashboard op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Klik op **Ja**om te controleren of u de kluis wilt verwijderen. De kluis wordt verwijderd en de portal keert terug naar het menu **nieuwe** service.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>De Recovery Services kluis verwijderen met behulp van Azure Resource Manager-client

Deze optie voor het verwijderen van de Recovery Services kluis wordt alleen aanbevolen wanneer alle afhankelijkheden worden verwijderd en er nog steeds een *fout optreedt*bij het verwijderen van de kluis.

- Controleer in het deel venster Essentials in het menu kluis of er geen **back-** upitems, **back-upbeheer servers**of **gerepliceerde items** worden weer gegeven. Als er back-upitems zijn, raadpleegt u de sectie [voordat u begint](#before-you-start) .
- Probeer [de kluis opnieuw te verwijderen uit de portal](#delete-the-recovery-services-vault).
- Als alle afhankelijkheden worden verwijderd en u nog steeds de *kluis verwijdert*, gebruikt u het hulp programma ARMClient om de volgende stappen uit te voeren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installeer de Choco lade vanaf [hier](https://chocolatey.org/) en om ARMClient te installeren, voert u de onderstaande opdracht uit:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Meld u aan bij uw Azure-account en voer deze opdracht uit:

    `ARMClient.exe login [environment name]`

3. Verzamel in het Azure Portal de abonnements-ID en de naam van de resource groep voor de kluis die u wilt verwijderen.

Raadpleeg dit [document](https://github.com/projectkudu/ARMClient/blob/master/README.md)voor meer informatie over de opdracht ARMClient.

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Azure Resource Manager-client gebruiken om Recovery Services kluis te verwijderen

1. Voer de volgende opdracht uit met de abonnements-ID, de naam van de resource groep en de naam van de kluis. Wanneer u de opdracht uitvoert, wordt de kluis verwijderd als er geen afhankelijkheden zijn.

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Als de kluis niet leeg is, ontvangt u de fout "kluis kan niet worden verwijderd omdat er bestaande resources zijn in deze kluis". Ga als volgt te werk om een beveiligde items/container in een kluis te verwijderen:

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Controleer in de Azure Portal of de kluis is verwijderd.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services kluizen.<br/>
[Meer informatie over](backup-azure-manage-windows-server.md) Recovery Services kluizen bewaken en beheren.
