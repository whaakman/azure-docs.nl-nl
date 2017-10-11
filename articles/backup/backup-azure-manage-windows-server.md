---
title: Azure recovery services-kluizen en servers beheren | Microsoft Docs
description: Gebruik deze handleiding voor informatie over het beheren van Azure recovery services-kluizen en servers.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: 5922e308f5c205a07bd329c28322ae82cea0e1fa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a>Azure Recovery Services-kluizen en -servers controleren en beheren voor Windows-machines
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klassiek](backup-azure-manage-windows-server-classic.md)
>
>

In dit artikel vindt u een overzicht van de back-monitor en beheertaken via de Azure portal en de Microsoft Azure Backup agent beschikbaar. In dit artikel wordt ervan uitgegaan dat u al een Azure-abonnement en ten minste één Recovery Services-kluis hebt gemaakt.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Een Recovery Services-kluis openen

De Recovery Services-kluisdashboard ziet u de gegevens of de kenmerken van een Recovery Services-kluis.

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/) met behulp van uw Azure-abonnement.
2. Klik in het menu Hub op **meer Services**.

    ![Lijst met Recovery Services-kluizen stap 1 openen](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. U wilt openen een Recovery Services-kluis. Begint te typen in het dialoogvenster **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluizen** om de lijst met Recovery Services-kluizen in uw abonnement weer te geven.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    De lijst met Recovery Services-kluizen wordt geopend.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Selecteer de naam van de Recovery Services-kluis die u wilt openen in de lijst met kluizen. De Recovery Services-kluis dashboard blade wordt geopend.

    ![Recovery services-kluisdashboard](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Nu u de Recovery Services-kluis hebt geopend, proberen een van de taken bewaken of beheren.

## <a name="monitor-backup-jobs-and-alerts"></a>Monitor voor back-uptaken en waarschuwingen

U controleren taken en waarschuwingen vanuit het dashboard Recovery Services-kluis, waarin u zien hoe:

* Details van de back-waarschuwingen
* Bestanden en mappen, evenals Azure virtuele machines die beveiligd in de cloud
* Totale opslagruimte is verbruikt in Azure
* Status van de back-uptaak

![Taken van de back-dashboard](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

Op de gegevens in elk van deze tegels te klikken, wordt de bijbehorende blade waar u gerelateerde taken beheren geopend.

Vanaf de bovenkant van het Dashboard:

* Instellingen geeft toegang tot beschikbare back-uptaken.
* Back-up - kunt u back-up van nieuwe bestanden en mappen (of virtuele Azure-machines) naar de Recovery Services-kluis.
* DELETE - als een recovery services-kluis niet meer wordt gebruikt, kunt u verwijderen het vrijmaken van opslagruimte. Verwijderen is alleen ingeschakeld nadat alle beveiligde servers zijn verwijderd uit de kluis.

![Taken van de back-dashboard](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a>Waarschuwingen voor back-ups met Azure backup-agent:
| Waarschuwingsniveau | Waarschuwingen verzonden |
| --- | --- |
| Kritieke |Back-upfouten, herstelfout |
| Waarschuwing |Back-up is voltooid met waarschuwingen (indien minder dan honderd bestanden worden niet gekopieerd vanwege problemen met de beschadiging en meer dan 1 miljoen bestanden zijn met succes back-up) |
| Informatief |Geen |

## <a name="manage-backup-alerts"></a>Back-up waarschuwingen beheren
Klik op de **waarschuwingen voor back-up** tegel openen de **waarschuwingen voor back-up** blade waarschuwingen en beheren.

![Back-waarschuwingen](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

De tegel waarschuwingen voor back-up toont het aantal:

* kritieke waarschuwingen niet omgezet in de afgelopen 24 uur
* waarschuwingen niet omgezet in de afgelopen 24 uur

Op elk van deze koppelingen te klikken, gaat u naar de **waarschuwingen voor back-up** blade met een gefilterde weergave van waarschuwingen (kritieke of waarschuwingsstatus).

Op de blade back-up waarschuwingen u:

* Kies de juiste informatie om op te nemen met uw waarschuwingen.

    ![Kolommen kiezen](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* Waarschuwingen filteren op ernst, status en beginnen of eindigen tijden.

    ![Waarschuwingen filteren](./media/backup-azure-manage-windows-server/filter-alerts.png)
* Meldingen configureren voor de ernst, frequentie en ontvangers, evenals waarschuwingen inschakelen of uitschakelen.

    ![Waarschuwingen filteren](./media/backup-azure-manage-windows-server/configure-notifications.png)

Als **Per waarschuwing** is geselecteerd als de **hoogte** frequentie geen groepering of vermindering van e-mailberichten optreedt. Elke waarschuwing resulteert in 1 melding. Dit is de standaardinstelling en het e-mailadres voor de oplossing ook onmiddellijk wordt verstuurd.

Als **per uur Digest** is geselecteerd als de **hoogte** frequentie één e-mailbericht verzonden naar de gebruiker melden dat er nieuwe waarschuwingen gegenereerd in het afgelopen uur niet opgelost. Een resolutie e-mailbericht is verzonden aan het einde van het uur.

Waarschuwingen kunnen worden verzonden voor de volgende niveaus:

* Kritieke
* Waarschuwing
* Informatie

Deactiveren van de waarschuwing met de **deactiveren** knop in de blade taak details. Wanneer u klikt op deactiveert, kunt u de opmerkingen bij de resolutie opgeven.

Kies van de kolommen die u wilt weergeven als onderdeel van de waarschuwing met de **kolommen kiezen** knop.

> [!NOTE]
> Van de **instellingen** blade u back-waarschuwingen beheren door te selecteren **bewaking en rapporten > waarschuwingen en gebeurtenissen > back-waarschuwingen** en vervolgens te klikken op **Filter** of  **Meldingen configureren**.
>
>

## <a name="manage-backup-items"></a>Back-up-items beheren
Het beheren van lokale back-ups is nu beschikbaar in de beheerportal. In de sectie back-up van het dashboard, het **back-ups** tegel ziet u het aantal back-items die zijn beveiligd door de kluis.

Klik op **bestandsmappen** tegel in de back-up-Items.

![Tegel back-items](./media/backup-azure-manage-windows-server/backup-items-tile.png)

De blade back-Items wordt geopend met het filter ingesteld op bestand met de map waar u elke specifieke back-up vermeld item zien.

![Back-items](./media/backup-azure-manage-windows-server/backup-item-list.png)

Als u een specifieke back-item uit de lijst selecteert, ziet u de essentiële gegevens voor het item.

> [!NOTE]
> Van de **instellingen** blade u bestanden en mappen beheren door te selecteren **beveiligde Items > back-Upitems** en selecteren **bestandsmappen** in de vervolgkeuzelijst.
>
>

![Back-items uit de instellingen voor](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a>Back-uptaken beheren
Back-up taken voor zowel on-premises (wanneer de on-premises server is een back-up naar Azure) en Azure back-ups zijn zichtbaar in het dashboard.

In de sectie back-up van het dashboard ziet u de tegel van de taak back-up van het aantal taken:

* wordt uitgevoerd
* mislukt in de afgelopen 24 uur.

Voor het beheren van uw back-uptaken, klikt u op de **back-uptaken** tegel, waarmee de blade back-uptaken wordt geopend.

![Back-items uit de instellingen voor](./media/backup-azure-manage-windows-server/backup-jobs.png)

Wijzigen van de informatie die beschikbaar zijn in de blade back-uptaken met de **kolommen kiezen** knop aan de bovenkant van de pagina.

Gebruik de **Filter** knop van bestanden en mappen en back-up van virtuele machine van Azure kunt selecteren.

Als u uw back-ups van bestanden en mappen niet ziet, klikt u op **Filter** knop aan de bovenkant van de pagina en selecteer **bestanden en mappen** in het menu-Item.

> [!NOTE]
> Van de **instellingen** blade u back-uptaken beheren door te selecteren **bewaking en rapporten > taken > back-uptaken** en selecteren **bestandsmappen** in de vervolgkeuzelijst het menu.
>
>

## <a name="monitor-backup-usage"></a>Gebruik back-up controleren
In de sectie back-up van het dashboard toont de tegel back-up-gebruik de opslag in Azure verbruikt. Opslaggebruik is voorzien:

* LRS-opslaggebruik voor de cloud die is gekoppeld aan de kluis
* Cloud GRS gebruik van opslag die is gekoppeld aan de kluis

## <a name="manage-your-production-servers"></a>Uw productieservers beheren
Voor het beheren van uw productieservers, klikt u op **instellingen**.

Klik onder beheren op **back-upinfrastructuur > productieservers**.

De productieservers blade een lijst met alle beschikbare productieservers. Klik op een server in de lijst met gegevens van de server openen.

![Beveiligde items](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-the-azure-backup-agent"></a>Open de Azure Backup agent
Open de **Microsoft Azure backup-agent** (vindt u het door te zoeken naar uw computer *Microsoft Azure Backup*).

![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/snap-in-search.png)

Van de **acties** beschikbaar op het recht van de back-upagent console u beheertaken het volgende:

* Server registreren
* Back-up plannen
* Nu een back-maken
* Eigenschappen wijzigen

![Microsoft Azure Backup agent console acties](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> Naar **gegevens herstellen**, Zie [bestanden terugzetten op een WindowsServer of Windows-clientcomputer](backup-azure-restore-windows-server.md).
>
>

## <a name="modify-the-backup-schedule"></a>Het back-upschema wijzigen
1. Klik in de Microsoft Azure Backup agent op **back-up plannen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. In de **Wizard Back-upschema** laat de **wijzigingen aanbrengen in de back-items of tijden** optie is geselecteerd en klik op **volgende**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. Als u wilt toevoegen of wijzigen van items, op de **Items selecteren voor back-up** scherm op **Items toevoegen**.

    U kunt ook instellen **Uitsluitingsinstellingen** via deze pagina in de wizard. Als u wilt uitsluiten van bestanden of bestandstypen leest u de procedure voor het toevoegen van [uitsluitingsinstellingen](#manage-exclusion-settings).
4. Selecteer de bestanden en mappen die u wilt back-up en klikt u op **OK**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. Geef de **back-upschema** en klik op **volgende**.

    U kunt de dag (maximaal 3 keer per dag) of een wekelijkse back-ups plannen.

    ![Items voor back-up van Windows Server](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Het back-upschema opgeven wordt gedetailleerd uitgelegd in dit [artikel](backup-azure-backup-cloud-as-tape.md).
   >

6. Selecteer de **bewaarbeleid** voor de back-up en klik op **volgende**.

    ![Items voor back-up van Windows Server](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. Op de **bevestiging** scherm Lees de informatie en klik op **voltooien**.
8. Zodra de wizard is voltooid met het maken van de **back-upschema**, klikt u op **sluiten**.

    Na het wijzigen van beveiliging, kunt u bevestigen dat back-ups correct worden geactiveerd door te gaan naar de **taken** tabblad en te bevestigen dat de wijzigingen worden doorgevoerd in de back-uptaken.

## <a name="enable-network-throttling"></a>Netwerkbeperking inschakelen

De Azure Backup agent biedt een tabblad bandbreedtebeperking zodat u kunt bepalen hoe netwerkbandbreedte wordt gebruikt tijdens de gegevensoverdracht. Dit besturingselement is handig als u back wilt-up van gegevens tijdens werkuren, maar niet wilt dat de back-upproces andere internetverkeer verstoort zijn. Beperken van de gegevens overbrengen van toepassing op back-up en herstelbewerkingen.  

Naar beperking inschakelen:

1. In de **backup-agent**, klikt u op **eigenschappen wijzigen**.
2. Op de ** tabblad beperking, selecteer **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**.

    ![Netwerkbeperking](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    Wanneer u de beperking hebt ingeschakeld, geef de toegestane bandbreedte voor tijdens de back gegevensoverdracht- **werkuren** en **niet-werkuren**.

    De waarden van de bandbreedte begint in 512 kB per seconde (Kbps) en maximaal 1023 megabytes per seconde (Mbps) kunnen gaan. U kunt ook aanwijzen is gestart en voltooien voor **werkuren**, en welke dagen van de week worden beschouwd als werk dagen. De tijd buiten de werkuren aangewezen wordt beschouwd als niet-werkuren.
3. Klik op **OK**.

## <a name="manage-exclusion-settings"></a>Uitsluitingsinstellingen voor beheren
1. Open de **Microsoft Azure backup-agent** (u vindt deze door te zoeken naar uw computer *Microsoft Azure Backup*).

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. Klik in de Microsoft Azure Backup agent op **back-up plannen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. In de Wizard Back-upschema laat de **wijzigingen aanbrengen in de back-items of tijden** optie is geselecteerd en klik op **volgende**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. Klik op **uitsluitingen instellingen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. Klik op **uitsluiting toevoegen**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. Selecteer de locatie en klik vervolgens op **OK**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. Toevoegen van extensie in de **bestandstype** veld.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    Een uitbreiding .mp3 toevoegen

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    Klik op als u een andere extensie **uitsluiting toevoegen** en voert u een ander bestandstype (extensie .jpeg toevoegen).

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. Wanneer u alle extensies hebt toegevoegd, klikt u op **OK**.
9. Ga verder met de Wizard planning back-up door te klikken op **volgende** totdat de **bevestigingspagina**, klikt u vervolgens op **voltooien**.

    ![Een Windows Server back-up plannen](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**W1. De status van de back-uptaak wordt als voltooid in de Azure backup-agent, waarom niet het ophalen van onmiddellijk weergegeven in de portal?**

A1. Er wordt op maximale vertraging van 15 minuten tussen de status van de back-uptaak doorgevoerd in de Azure backup agent en de Azure portal.

**Q.2 als een back-uptaak is mislukt, hoe lang duurt een melding kan genereren?**

A.2 een waarschuwing wordt gegenereerd binnen de 20 minuten van de Azure Backup-fout.

**Q3. Is er een aanvraag waarin een e-mailbericht niet verzonden als meldingen zijn geconfigureerd?**

A3. Hieronder volgen de gevallen wanneer de melding niet verzonden wordt om te reduceren van de waarschuwing ruis:

* Als meldingen per uur zijn geconfigureerd en er een waarschuwing gegenereerd en opgelost binnen het uur
* Taak is geannuleerd.
* Tweede back-uptaak is mislukt, omdat de oorspronkelijke back-uptaak wordt uitgevoerd.

## <a name="troubleshooting-monitoring-issues"></a>Het oplossen van problemen met bewaking
**Probleem:** taken en/of waarschuwingen van de Azure Backup agent worden niet weergegeven in de portal.

**Stappen voor probleemoplossing:** het proces ```OBRecoveryServicesManagementAgent```, verzendt de taak en waarschuwing gegevens naar de Azure Backup-service. Tijd tot tijd dit proces zijn vastgelopen of afsluiten.

1. Om te controleren of het proces wordt niet uitgevoerd, opent u **Taakbeheer** en controleer of de ```OBRecoveryServicesManagementAgent``` proces wordt uitgevoerd.
2. Ervan uitgaande dat het proces niet wordt uitgevoerd, opent u **Configuratiescherm** en de lijst met services bladeren. Starten of opnieuw starten **Microsoft Azure Recovery Services Management Agent**.

    Ga voor meer informatie de logbestanden op:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Bijvoorbeeld:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Volgende stappen
* [WindowsServer of Windows-Client uit Azure herstellen](backup-azure-restore-windows-server.md)
* Zie voor meer informatie over Azure Backup, [overzicht van Azure-back-up](backup-introduction-to-azure-backup.md)
* Ga naar de [Azure back-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)
