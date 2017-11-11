---
title: Azure Backup-kluizen en servers met het klassieke implementatiemodel Azure beheren | Microsoft Docs
description: Gebruik deze handleiding voor informatie over het beheren van Azure Backup-kluizen en servers.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: markgal;
ms.openlocfilehash: ecc2eb996546810dad169dc25175ab10ebb164aa
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Azure Backup-kluizen en -servers beheren met het klassieke implementatiemodel
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klassiek](backup-azure-manage-windows-server-classic.md)
>
>

In dit artikel vindt u een overzicht van de Backup-beheertaken die beschikbaar is via de klassieke Azure portal en de Microsoft Azure Backup agent.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 30 November 2017 zich u niet PowerShell gebruiken voor het maken van de Backup-kluizen. **Per 30 November 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

## <a name="management-portal-tasks"></a>Beheertaken voor de portal
1. Aanmelden bij de [beheerportal](https://manage.windowsazure.com).
2. Klik op **Recovery Services**, klikt u op de naam van back-upkluis naar de pagina snel starten.

    ![Recovery services](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Als u de opties aan de bovenkant van de pagina snel starten selecteert, ziet u de beschikbare beheertaken.

![Tabbladen beheren](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Dashboard
Selecteer **Dashboard** overzicht gebruik voor de server. De **overzicht gebruik** omvat:

* Het aantal Windows-Servers geregistreerd naar cloud
* Het aantal virtuele machines in Azure wordt beveiligd in cloud
* De totale opslagruimte is verbruikt in Azure
* De status van recente taken

Aan de onderkant van het Dashboard kunt u de volgende taken uitvoeren:

* **Certificaten beheren** - als een certificaat is gebruikt voor het registreren van de server en gebruik deze bijwerken van het certificaat. Als u referenties voor de kluis, gebruik geen **certificaat beheren**.
* **Verwijder** -Hiermee verwijdert u de huidige back-upkluis. Als een back-upkluis niet meer wordt gebruikt, kunt u het vrijmaken van opslagruimte verwijderen. **Verwijder** is alleen ingeschakeld nadat alle geregistreerde servers zijn verwijderd uit de kluis.

![Taken van de back-dashboard](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Geregistreerde items
Selecteer **geregistreerde Items** om weer te geven van de namen van de servers die zijn geregistreerd in deze kluis.

![Geregistreerde items](./media/backup-azure-manage-windows-server-classic/registered-items.png)

De **Type** filteren standaardinstellingen voor virtuele Machine van Azure. Als u wilt weergeven van de namen van de servers die zijn geregistreerd in deze kluis, selecteer **WindowsServer** in de vervolgkeuzelijst.

Hier kunt u de volgende taken uitvoeren:

* **Opnieuw registreren toestaan** : wanneer deze optie is geselecteerd voor een server die u kunt de **registratiewizard** vault een tweede keer in de Microsoft Azure Backup agent lokaal op de server geregistreerd met de back-up. Mogelijk moet u opnieuw registreren vanwege een fout in het certificaat of als een server moest opnieuw worden opgebouwd.
* **Verwijder** -Hiermee verwijdert u een server uit de back-upkluis. Alle opgeslagen gegevens die zijn gekoppeld aan de server wordt onmiddellijk verwijderd.

    ![Geregistreerde items taken](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Beveiligde items
Selecteer **beveiligde Items** om de items die back-ups zijn van de servers weer te geven.

![Beveiligde items](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Configureren
Van de **configureren** tabblad kunt u de juiste optie voor redundantie. Na het maken van een kluis en voordat de computers zijn geregistreerd bij deze is het beste moment om de optie opslag redundantie geschikt.

> [!WARNING]
> Wanneer een item naar de kluis is geregistreerd, wordt de opslagoptie voor redundantie is vergrendeld en kan niet worden gewijzigd.
>
>

![Configureren](./media/backup-azure-manage-windows-server-classic/configure.png)

Raadpleeg dit artikel voor meer informatie over [redundantie van gegevensopslag](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Backup agent-taken
### <a name="console"></a>Console
Open de **Microsoft Azure backup-agent** (u vindt deze door te zoeken naar uw computer *Microsoft Azure Backup*).

![Back-upagent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Van de **acties** beschikbaar op het recht van de back-upagent-console kunt u de volgende beheertaken uitvoeren:

* Server registreren
* Back-up plannen
* Nu een back-maken
* Eigenschappen wijzigen

![Console agentacties](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> Naar **gegevens herstellen**, Zie [bestanden terugzetten op een WindowsServer of Windows-clientcomputer](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Wijzigen van een bestaande back-up
1. Klik in de Microsoft Azure Backup agent op **back-up plannen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. In de **Wizard Back-upschema** laat de **wijzigingen aanbrengen in de back-items of tijden** optie is geselecteerd en klik op **volgende**.

    ![Een geplande back-up wijzigen](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Als u wilt toevoegen of wijzigen van items, op de **Items selecteren voor back-up** scherm op **Items toevoegen**.

    U kunt ook instellen **Uitsluitingsinstellingen** via deze pagina in de wizard. Als u wilt uitsluiten van bestanden of bestandstypen leest u de procedure voor het toevoegen van [uitsluitingsinstellingen](#exclusion-settings).
4. Selecteer de bestanden en mappen die u wilt back-up en klikt u op **OK**.

    ![Items toevoegen](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Geef de **back-upschema** en klik op **volgende**.

    U kunt de dag (maximaal 3 keer per dag) of een wekelijkse back-ups plannen.

    ![Uw back-upschema opgeven](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Het back-upschema opgeven wordt gedetailleerd uitgelegd in dit [artikel](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Selecteer de **bewaarbeleid** voor de back-up en klik op **volgende**.

    ![Selecteer uw bewaarbeleid](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Op de **bevestiging** scherm Lees de informatie en klik op **voltooien**.
8. Zodra de wizard is voltooid met het maken van de **back-upschema**, klikt u op **sluiten**.

    Na het wijzigen van beveiliging, kunt u bevestigen dat back-ups correct worden geactiveerd door te gaan naar de **taken** tabblad en te bevestigen dat de wijzigingen worden doorgevoerd in de back-uptaken.

### <a name="enable-network-throttling"></a>Netwerkbeperking inschakelen
De Azure Backup agent biedt een tabblad bandbreedtebeperking zodat u kunt bepalen hoe netwerkbandbreedte wordt gebruikt tijdens de gegevensoverdracht. Dit besturingselement is handig als u back wilt-up van gegevens tijdens werkuren, maar niet wilt dat de back-upproces andere internetverkeer verstoort zijn. Beperken van de gegevens overbrengen van toepassing op back-up en herstelbewerkingen.  

Naar beperking inschakelen:

1. In de **backup-agent**, klikt u op **eigenschappen wijzigen**.
2. Selecteer de **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen** selectievakje.

    ![Netwerkbeperking](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Wanneer u de beperking hebt ingeschakeld, geef de toegestane bandbreedte voor tijdens de back gegevensoverdracht- **werkuren** en **niet-werkuren**.

    De waarden van de bandbreedte begint in 512 kB per seconde (Kbps) en maximaal 1023 megabytes per seconde (Mbps) kunnen gaan. U kunt ook aanwijzen is gestart en voltooien voor **werkuren**, en welke dagen van de week worden beschouwd als werk dagen. De tijd buiten de werkuren aangewezen wordt beschouwd als niet-werkuren.
4. Klik op **OK**.

## <a name="exclusion-settings"></a>Uitsluitingsinstellingen
1. Open de **Microsoft Azure backup-agent** (u vindt deze door te zoeken naar uw computer *Microsoft Azure Backup*).

    ![Open back-upagent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. Klik in de Microsoft Azure Backup agent op **back-up plannen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. In de Wizard Back-upschema laat de **wijzigingen aanbrengen in de back-items of tijden** optie is geselecteerd en klik op **volgende**.

    ![Een schema wijzigen](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Klik op **uitsluitingen instellingen**.

    ![Selecteer items die u wilt uitsluiten](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Klik op **uitsluiting toevoegen**.

    ![Uitsluitingen toevoegen](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Selecteer de locatie en klik vervolgens op **OK**.

    ![Selecteer een locatie voor uitsluiting](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Toevoegen van extensie in de **bestandstype** veld.

    ![Door het bestandstype uitsluiten](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Een uitbreiding .mp3 toevoegen

    ![Voorbeeld van een type](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Klik op als u een andere extensie **uitsluiting toevoegen** en voert u een ander bestandstype (extensie .jpeg toevoegen).

    ![Voorbeeld van een ander type](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Wanneer u alle extensies hebt toegevoegd, klikt u op **OK**.
9. Ga verder met de Wizard planning back-up door te klikken op **volgende** totdat de **bevestigingspagina**, klikt u vervolgens op **voltooien**.

    ![Uitsluiting bevestigen](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Volgende stappen
* [WindowsServer of Windows-Client uit Azure herstellen](backup-azure-restore-windows-server.md)
* Zie voor meer informatie over Azure Backup, [overzicht van Azure-back-up](backup-introduction-to-azure-backup.md)
* Ga naar de [Azure back-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)
