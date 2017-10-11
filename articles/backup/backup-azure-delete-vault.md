---
title: " Een Recovery Services-kluis in Azure verwijderen | Microsoft Docs "
description: Het verwijderen van een Azure Backup en Recovery Services-kluis. Een back-upkluis kan worden aangeroepen, een Azure-cloud-kluis, of de Azure recovery-kluis. Het oplossen van problemen wanneer u een back-upkluis in de klassieke portal of Azure-portal niet verwijderen.
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: ae4a73d12898c62fe2c5cf3683bc7c1c8c845fdf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen
De Azure Backup-service heeft twee soorten kluizen: de Backup-kluis en de Recovery Services-kluis. De Backup-kluis was er het eerst. Vervolgens kwam de Recovery Services-kluis ter ondersteuning van de uitgebreide Resource Manager-implementaties. Vanwege de uitgebreide mogelijkheden en de afhankelijkheden van de informatie die moeten worden opgeslagen in de kluis kan verwijderen van een back-up of Recovery Services-kluis verwarrend zijn. Dit artikel wordt uitgelegd hoe u verwijdert de kluizen in de klassieke portal en de Azure-portal.  

| **Implementatietype** | **Portal** | **De kluisnaam van de** |
| --- | --- | --- |
| Klassiek |Klassiek |Back-upkluis |
| Resource Manager |Azure |Recovery Services-kluis |

> [!NOTE]
> Oplossingen die met Resource Manager zijn geïmplementeerd, kunnen niet met Backup-kluizen worden beveiligd. U kunt echter een Recovery Services-kluis classically geïmplementeerde servers en virtuele machines te beschermen.  
>

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Vanaf **15 oktober 2017** kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. <br/> **Vanaf 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

In dit artikel gebruiken we de term kluis om te verwijzen naar het generieke formulier van de Backup-kluis of een Recovery Services-kluis. We gebruiken de formele naam, de Backup-kluis of de Recovery Services-kluis, wanneer het onderscheid maken tussen de kluizen nodig is.

## <a name="deleting-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen
Verwijderen van een Recovery Services-kluis is een proces één stap - *opgegeven in de kluis bevat niet alle resources*. Voordat u een Recovery Services-kluis verwijderen kunt, moet u verwijdert of alle bronnen in de kluis. Als u probeert te verwijderen van een kluis die bronnen bevat, krijgt u een fout opgetreden, zoals de volgende afbeelding:

![Fout bij het verwijderen van de kluis](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Totdat u de bronnen uit de kluis hebt uitgeschakeld, te klikken op **probeer** produceert dezelfde fout. Als u op dit foutbericht wordt weergegeven achtergebleven bent, klikt u op **annuleren** en gebruik de volgende stappen uit om te verwijderen van de resources in de kluis.

### <a name="removing-the-items-from-a-vault-protecting-a-vm"></a>Verwijderen van de items in een kluis beveiligen van een virtuele machine
Als u de Recovery Services-kluis openen al hebt, gaat u naar de tweede stap.

1. De Azure portal openen en open de kluis die u wilt verwijderen uit het Dashboard.

   Als u niet de Recovery Services-kluis vastgemaakt aan het Dashboard in het menu Hub klikt **meer Services** en typt u in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluizen**.

   ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer de kluis die u wilt verwijderen uit de lijst.

   ![Kies kluis in lijst](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. Bekijk in de weergave van de kluis, de **Essentials** deelvenster. Als u wilt verwijderen een kluis, kan niet alle beveiligde items. Als er een andere waarde dan nul, onder **back-Upitems** of **back-up van beheerservers**, moet u deze items verwijderen voordat u de kluis kunt verwijderen.

    ![Bekijk Essentials-deelvenster voor beveiligde items](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VM's en bestanden/mappen worden beschouwd als back-Upitems en staan in de **back-Upitems** van het deelvenster Essentials. Een DPM-server wordt weergegeven in de **back-up-beheerserver** van het deelvenster Essentials. **Gerepliceerde Items** hebben betrekking op de Azure Site Recovery-service.
3. Om te beginnen met het verwijderen van de beveiligde items uit de kluis, de items te zoeken in de kluis. Klik in het kluisdashboard op **instellingen**, en klik vervolgens op **back-up items** om deze blade te openen.

    ![Kies kluis in lijst](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    De **back-Upitems** blade bevat afzonderlijke lijsten, op basis van het itemtype: Azure Virtual Machines of bestandsmappen (Zie afbeelding). De weergegeven lijst standaard itemtype is Azure Virtual Machines. De lijst wilt weergeven van bestandsmappen items in de kluis, selecteer **bestandsmappen** uit de vervolgkeuzelijst.
4. Voordat u een item uit de kluis van een virtuele machine te beveiligen verwijderen kunt, moet de back-uptaak van het item stoppen en verwijderen van de gegevens voor herstelpunt. Voer de volgende stappen uit voor elk item in de kluis:

    a. Op de **back-ups** blade met de rechtermuisknop op het item en selecteer in het contextmenu **back-up stoppen**.

    ![de back-uptaak stoppen](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    De blade back-up stoppen wordt geopend.

    b. Op de **back-up stoppen** blade van de **kiest u een optie** selecteert u **back-upgegevens verwijderen** > Typ de naam van het item > en klik op **back-up stoppen**.

    Typ de naam van het item, om te controleren of dat u wilt verwijderen. De **back-up stoppen** knop wordt geactiveerd zodra u controleren of het item. Als u het dialoogvenster Typ de naam van het back-item niet ziet, kiest u de **back-upgegevens behouden** optie.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    U kunt desgewenst een reden waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe opgeven. Nadat u op **back-up stoppen**, toestaan dat de taak verwijderen te voltooien voordat u probeert te verwijderen van de kluis. Om te controleren of de taak is voltooid, controleert u de Azure-berichten ![verwijderen van de back-upgegevens](./media/backup-azure-delete-vault/messages.png). <br/>
    Zodra de taak voltooid is, verschijnt een bericht waarin staat het back-upproces is gestopt en de back-upgegevens, voor het item, is verwijderd.

    c. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** de resterende items in de kluis.

      ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wanneer er geen items in de lijst zijn, schuif naar de **Essentials** deelvenster in de blade back-up-kluis. Er mag niet een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items worden nog steeds in de kluis weergegeven, terug naar stap 3 en kies een ander item type lijst.  
5. Wanneer er geen items meer op de werkbalk van de kluis zijn, klikt u op **verwijderen**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-vault.png)
6. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Wat gebeurt er als ik het back-upproces is gestopt, maar de gegevens behouden?
Als u het back-upproces maar per ongeluk gestopt *bewaard* de gegevens, moet u de back-upgegevens verwijderen voordat u de kluis kunt verwijderen. De back-upgegevens verwijderen:

1. Op de **back-ups** blade met de rechtermuisknop op het item en klik in het contextmenu op **verwijderen van de back-upgegevens**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    De **back-up-gegevens verwijderen** blade wordt geopend.
2. Op de **back-up-gegevens verwijderen** blade, typ de naam van het item en klik op **verwijderen**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Zodra u de gegevens hebt verwijderd, terug naar stap 4c en doorgaan met het proces.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Een kluis die wordt gebruikt voor het beveiligen van een DPM-server verwijderen
Voordat u een kluis die wordt gebruikt voor het beveiligen van een DPM-server verwijderen kunt, moet u wissen herstelpunten die zijn gemaakt en vervolgens Hef de registratie van de server uit de kluis.

De gegevens die zijn gekoppeld aan een beveiligingsgroep verwijderen:

1. Klik in de DPM Administrator-Console op **beveiliging** > Selecteer een beveiligingsgroep > Selecteer lid van de beveiligingsgroep > en klik in het lint op **verwijderen**.

  Selecteer lid van de beveiligingsgroep voor het activeren van de **verwijderen** knop in het lint. In het voorbeeld wordt het lid is **dummyvm9**. Als u wilt meerdere leden in de beveiligingsgroep selecteert, de Ctrl-toets ingedrukt en klikt u op de leden.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    De **beveiliging stoppen** dialoogvenster wordt geopend.
2. In de **beveiliging stoppen** dialoogvenster Selecteer **beveiligde gegevens verwijderen**, en klik op **beveiliging stoppen**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Als u wilt verwijderen een kluis, moet u wissen of verwijderen, de kluis van beveiligde gegevens. Afhankelijk van het aantal herstelpunten en gegevens in de beveiligingsgroep duurt een willekeurige plaats van een paar seconden tot enkele minuten om de gegevens te verwijderen. De **beveiliging stoppen** dialoogvenster wordt de status weergegeven wanneer de taak is voltooid.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Herhaal deze procedure voor alle leden in alle beveiligingsgroepen.

    Verwijder alle beveiligde gegevens en bescherming groepen.
4. Nadat alle leden verwijderen uit de beveiligingsgroep, overschakelen naar de Azure-portal. Open het kluisdashboard en zorg ervoor dat er geen **back-Upitems**, **back-up van beheerservers**, of **gerepliceerde items**. Klik op de werkbalk kluis **verwijderen**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-vault.png)

    Als er back-up-beheerservers die is geregistreerd in de kluis, kunt u de kluis niet verwijderen, zelfs als er geen gegevens in de kluis zijn. Als u de back-up-beheerservers die zijn gekoppeld aan de kluis verwijderd, maar er servers die worden vermeld zijn in de **Essentials** deelvenster Zie [vinden van de back-up-beheerservers die is geregistreerd in de kluis](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault).
5. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Een kluis die wordt gebruikt voor het beveiligen van een productieserver verwijderen
Voordat u een kluis die wordt gebruikt voor het beveiligen van een productieserver verwijderen kunt, moet u verwijderen of de registratie van de server uit de kluis.

De productieserver die zijn gekoppeld aan de kluis verwijderen:

1. Open in de Azure portal het kluisdashboard en klik op **instellingen** > **back-upinfrastructuur** > **productieservers**.

    ![Open de blade productieservers](./media/backup-azure-delete-vault/delete-production-server.png)

    De **productieservers** blade wordt geopend en een lijst met alle productieservers in de kluis.

    ![lijst met productieservers](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Op de **productieservers** blade met de rechtermuisknop op de server en klik op **verwijderen**.

    ![productieserver verwijderen ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    De **verwijderen** blade wordt geopend.

    ![productieserver verwijderen ](./media/backup-azure-delete-vault/delete-blade.png)
3. Op de **verwijderen** blade Bevestig de servernaam van de en klik op **verwijderen**. U moet de server activeren correct naam de **verwijderen** knop.

    Zodra de kluis zijn verwijderd, verschijnt een bericht weergegeven met dat de kluis is verwijderd. Ga terug naar het deelvenster Essentials in het kluisdashboard na het verwijderen van alle servers in de kluis.
4. Zorg ervoor dat er op het kluisdashboard geen **back-Upitems**, **back-up van beheerservers**, of **gerepliceerde items**. Klik op de werkbalk kluis **verwijderen**.
5. Om te bevestigen dat u wilt verwijderen van de kluis, klikt u op **Ja**.

    De kluis wordt verwijderd en de portal worden de **nieuw** Servicemenu.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Verwijderen van een back-upkluis in de klassieke portal
De volgende instructies zijn voor het verwijderen van een back-upkluis in de klassieke portal. Voordat u de Backup-kluis verwijderen kunt, u moet de herstelpunten, verwijderen of een back-up items en verwijdert u de geregistreerde servers. De geregistreerde servers zijn de Windows Server, werkstation of virtuele machines die zijn geregistreerd in de kluis.

1. Open de [klassieke portal](https://manage.windowsazure.com).

2. Selecteer de kluis die u wilt verwijderen uit de lijst met back-upkluizen.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Het kluisdashboard wordt geopend. Bekijk het nummer van de Windows-Servers en/of Azure virtuele machines die zijn gekoppeld aan de kluis. Bekijk ook de totale opslag in Azure verbruikt. Stop alle back-uptaken en alle gegevens verwijderen voordat u verwijdert de kluis.

3. Klik op de **beveiligde Items** tabblad en klik vervolgens op **beveiliging stoppen**

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    De **Stop de beveiliging van uw kluis'** dialoogvenster wordt weergegeven.
4. In de **Stop de beveiliging van uw kluis'** dialoogvenster selectievakje **verwijderen die zijn gekoppeld back-upgegevens** en klik op ![vinkje](./media/backup-azure-delete-vault/checkmark.png). <br/>
    U kunt eventueel Kies een reden voor het stoppen van de beveiliging en een opmerking opgeven.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Na het verwijderen van de items in de kluis, is de kluis leeg.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. Klik in de lijst van tabbladen op **geregistreerde Items**. De **Type** vervolgkeuzelijst menu, kunt u kiezen welk type server geregistreerd in de kluis. Het type is Windows Server of Azure virtuele Machine. Selecteer de virtuele machine naar de kluis is geregistreerd in het volgende voorbeeld en klikt u op **Unregister**.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Als u verwijderen van de registratie voor een Windows-Server wilt uit de **Type** vervolgkeuzelijst, selecteer **Windows Server**, klikt u op ![vinkje](./media/backup-azure-delete-vault/checkmark.png) om het scherm te vernieuwen en vervolgens Klik op **verwijderen**. <br/>

  ![Windows Server selecteren](./media/backup-azure-delete-vault/select-windows-server.png)

6. Klik in de lijst van tabbladen op **Dashboard** te openen dat tabblad. Controleer of er zijn geen geregistreerde servers of virtuele machines in Azure wordt beveiligd in de cloud. Controleer ook of dat er zijn geen gegevens in de opslag. Klik op **verwijderen** verwijderen van de kluis.

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Het bevestigingsscherm voor back-up verwijderen kluis wordt geopend. Selecteer een optie waarom u de kluis wilt verwijderen en klik op ![vinkje](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Back-upgegevens verwijderen](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    De kluis wordt verwijderd en u gaat terug naar het dashboard van de klassieke portal.

### <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Zoeken naar de back-up beheerservers geregistreerd in de kluis
Als u meerdere servers die zijn geregistreerd in een kluis hebt, kan het lastig zijn om te onthouden zijn. Zie de servers die zijn geregistreerd bij de kluis en deze verwijderen:

1. Open het kluisdashboard.
2. In de **Essentials** deelvenster, klikt u op **instellingen** om deze blade te openen.

    ![Open de instellingenblade](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Op de **blade instellingen**, klikt u op **back-upinfrastructuur**.
4. Op de **back-upinfrastructuur** blade, klikt u op **back-up-beheerservers**. De blade back-up-beheerservers wordt geopend.

    ![lijst met back-beheerservers](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Als u wilt een server verwijderen uit de lijst, klik met de rechtermuisknop op de naam van de server en klik vervolgens op **verwijderen**.
    De **verwijderen** blade wordt geopend.
6. Op de **verwijderen** blade geeft u de naam van de server. Als het een lange naam, kunt u kopieert en plakt deze uit de lijst met Servers voor het beheer van back-up. Klik vervolgens op **verwijderen**.  
