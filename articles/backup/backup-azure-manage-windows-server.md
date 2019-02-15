---
title: Azure Recovery Services-kluizen en servers beheren
description: Beheren van taken en waarschuwingen in een Azure Recovery Services-kluis.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/21/2018
ms.author: raynew
ms.openlocfilehash: 6a29aeda52fe599ec7e2ee3b3ea1846e05b73d7d
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268869"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Recovery Services-kluizen beheren en controleren

In dit artikel wordt uitgelegd hoe u de Recovery Services-kluis **overzicht** dashboard om te controleren en beheren van uw Recovery Services-kluizen. Wanneer u een Recovery Services-kluis in de lijst opent de **overzicht** dashboard voor de geselecteerde kluis wordt geopend. Het dashboard biedt verschillende details over de kluis. Er zijn *tegels* waarin: de status van kritieke en waarschuwingen, in uitvoering en mislukte back-uptaken en de hoeveelheid lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS) gebruikt. Als u back-up van virtuele Azure-machines naar de kluis, de [ **back-up de Voorcontrolestatus** tegel toont alle items die kritieke of waarschuwingsstatus](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/). De volgende afbeelding is de **overzicht** dashboard voor **Contoso-kluis**. De **back-Upitems** tegel ziet er negen items zijn geregistreerd bij de kluis.

![Recovery services-kluis-dashboard](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

De vereisten voor dit artikel zijn: een Azure-abonnement, een Recovery Services-kluis en dat er is ten minste één back-upitem geconfigureerd voor de kluis.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Een Recovery Services-kluis openen

Te bewaken, waarschuwingen, of gegevens over een Recovery Services-kluis weergeven, opent u de kluis.

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/) met behulp van uw Azure-abonnement.

2. Klik in de portal op **alle services**.

   ![Lijst met Recovery Services-kluizen stap 1 openen](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. In de **alle services** in het dialoogvenster, type **herstelservices**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer de **Recovery Services-kluizen** optie wordt weergegeven, klikt u hierop om de lijst met Recovery Services-kluizen openen in uw abonnement.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Klik op een kluis om te openen in de lijst met kluizen, de **overzicht** dashboard.

    ![Recovery services-kluis-dashboard](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Het dashboard overzicht gebruikt tegels voor waarschuwingen en gegevens back-uptaak.

## <a name="monitor-backup-jobs-and-alerts"></a>Back-taken bewaken en waarschuwingen

De Recovery Services-kluis **overzicht** dashboard bevat tegels voor informatie over controle en gebruik. De tegels in de weergave van de sectie bewaking kritiek en waarschuwing waarschuwingen, en In voortgang en mislukte taken. Klik op een bepaalde waarschuwing of taak om het menu waarschuwingen voor back-up- of back-uptaken, gefilterd voor die taak of een waarschuwing te openen.

![Taken van de back-dashboard](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

De sectie bewaking toont de resultaten van vooraf gedefinieerde **waarschuwingen voor back-up** en **back-uptaken** query's. De bewaking-tegels bevatten actuele informatie over:

* Kritiek en waarschuwing waarschuwingen voor back-uptaken (in de afgelopen 24 uur)
* De voorcontrolestatus voor virtuele machines van Azure - voor meer informatie over de controle vooraf voor de status, Zie de [back-up-blog over de status van de controle vooraf voor de back-up van](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/).
* De back-uptaken in voortgang, en taken die zijn mislukt (in de afgelopen 24 uur).

De tegels gebruik geven:

* Het aantal back-up-items die zijn geconfigureerd voor de kluis.
* De Azure-opslag (gescheiden door LRS en GRS) die worden gebruikt door de kluis.

Klik op de tegels (met uitzondering van opslag, back-up) om het bijbehorende menu te openen. In de bovenstaande afbeelding ziet de waarschuwingen voor back-up-tegel u drie kritieke waarschuwingen. Op de rij kritieke waarschuwingen in de tegel waarschuwingen voor back-up klikt, wordt de back-up-waarschuwingen gefilterd voor kritieke waarschuwingen geopend.

![Back-up waarschuwingenmenu gefilterd voor kritieke waarschuwingen](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Het menu back-up waarschuwingen in de afbeelding hierboven, wordt gefilterd op: Status actief is, ernst is kritiek en -tijd is de afgelopen 24 uur.

## <a name="manage-backup-alerts"></a>Back-up waarschuwingen beheren

Klik op toegang tot het menu back-up waarschuwingen in het menu Recovery Services-kluis **waarschuwingen voor back-up**.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Waarschuwingen voor back-up in dit rapport worden de waarschuwingen voor de kluis.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Waarschuwingen

De lijst met waarschuwingen voor back-up weergegeven de geselecteerde gegevens voor de gefilterde waarschuwingen. In het menu back-up-waarschuwingen kunt u filteren op waarschuwing of kritieke waarschuwingen.

| Waarschuwingsniveau | Gebeurtenissen die waarschuwingen genereren |
| ----------- | ----------- |
| Kritiek | U ontvangt kritieke waarschuwingen wanneer: Back-uptaken mislukken, recovery-taken mislukken, en wanneer u beveiliging stoppen op een server, maar blijven de gegevens bewaard.|
| Waarschuwing | U waarschuwing ontvangt wanneer een waarschuwing: Back-uptaken voltooid met waarschuwingen, bijvoorbeeld wanneer minder dan 100 bestanden worden niet opgeslagen vanwege problemen met de beschadiging, of wanneer meer dan 1.000.000 bestanden kunnen worden opgeslagen). |
| Informatief | Er is geen informatieve waarschuwingen zijn momenteel in gebruik. |

### <a name="viewing-alert-details"></a>Waarschuwingsdetails weergeven

Het rapport waarschuwingen voor back-up worden bijgehouden acht details over elke waarschuwing. Gebruik de **kolommen kiezen** om de details in het rapport bewerken.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts.png)

Standaard worden alle gegevens, met uitzondering van **tijd laatste keer voorgekomen**, worden weergegeven in het rapport.

* Waarschuwing
* Back-upitem
* Beveiligde Server
* Severity
* Duur
* Aanmaaktijd
* Status
* Tijd laatste keer voorgekomen

### <a name="change-the-details-in-alerts-report"></a>De details in rapport waarschuwingen wijzigen

1. Voor het wijzigen van de rapportgegevens in de **waarschuwingen voor back-up** menu, klikt u op **kolommen kiezen**.

   ![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   De **kolommen kiezen** menu wordt geopend.

2. In de **kolommen kiezen** menu, kiest u de details die u wilt weergeven in het rapport.

    ![Kies in het menu van de kolommen](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Klik op **gedaan** uw wijzigingen opslaan en sluiten van het menu van de kolommen kiezen.

   Als u wijzigingen aanbrengen, maar niet wilt dat om de wijzigingen te behouden, klikt u op **opnieuw** opgeslagen om terug te keren naar de laatste de geselecteerde configuratie.

### <a name="change-the-filter-in-alerts-report"></a>Wijzig het filter in rapport waarschuwingen

Gebruik de **Filter** menu om te wijzigen van de ernst, Status, begintijd en eindtijd voor de waarschuwingen.

> [!NOTE]
> Bewerken van de waarschuwingen voor back-up wijzigen niet filter de waarschuwing of kritieke waarschuwingen in de kluis een dashboard.
>  

1. Klik om te wijzigen van het filter waarschuwingen voor back-up, in het menu back-up waarschuwingen **Filter**.

   ![Kies in het filtermenu](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Het Filter-menu wordt weergegeven.

   ![Kies in het filtermenu](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. De ernst, Status, begintijd en eindtijd, bewerken en klik op **gedaan** uw wijzigingen op te slaan.

## <a name="configuring-notifications-for-alerts"></a>Meldingen voor waarschuwingen configureren

Meldingen voor het genereren van e-mailberichten wanneer er een waarschuwing of kritieke waarschuwing optreedt configureren. U kunt e-mailwaarschuwingen te verzenden elk uur of wanneer een bepaalde waarschuwing optreedt.

   ![Waarschuwingen filteren](./media/backup-azure-manage-windows-server/configure-notification.png)

E-mailmeldingen zijn standaard **op**. Klik op **uit** stoppen van de e-mailmeldingen.

Op de **waarschuwen** toe, kies **Per waarschuwing** als niet wilt groeperen of zijn veel items die waarschuwingen kunnen genereren. Elke waarschuwing resulteert in één melding (de standaardinstelling), en een e-mailadres van het probleem zou moeten onmiddellijk worden verzonden.

Als u selecteert **samenvatting per uur**, een e-mailbericht is verzonden naar de ontvangers het niet-opgeloste waarschuwingen die in het afgelopen uur gegenereerd waarin wordt uitgelegd. Een resolutie van e-mailbericht wordt verzonden aan het einde van het uur.

Kies de ernst van waarschuwing (Kritiek of waarschuwing) gebruikt voor het genereren van e-mailbericht. Er zijn momenteel geen waarschuwingen informatie.

## <a name="manage-backup-items"></a>Back-up items beheren

Een Recovery Services-kluis bevat veel soorten back-upgegevens. Zie voor een volledige lijst van de back-uptypen [die toepassingen en werkbelastingen kunnen een back-up](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use). Voor het beheren van de verschillende servers, computers, databases en werkbelastingen, klikt u op de **back-Upitems** tegel om de inhoud van de kluis weer te geven.

![Back-upitems van tegel](./media/backup-azure-manage-windows-server/backup-items.png)

De lijst met back-Upitems, gerangschikt op Type back-upbeheer, wordt geopend.

![lijst met back-upitems](./media/backup-azure-manage-windows-server/list-backup-items.png)

Als u wilt een specifiek type beveiligd exemplaar verkennen, klikt u op het item in de kolom Type back-upbeheer. Bijvoorbeeld, in de bovenstaande afbeelding zijn er twee Azure beveiligde virtuele machines in deze kluis. Te klikken op **virtuele Azure-Machine**, opent u de lijst van beveiligde virtuele machines in deze kluis.

![lijst van het type back-up](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

De lijst met virtuele machines heeft nuttige gegevens: de bijbehorende resourcegroep, vorige [controle vooraf voor de back-up](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/), Status van de laatste back-up en datum van de meest recente herstelpunt. Het beletselteken opent in de laatste kolom, u het menu voor het activeren van algemene taken. De nuttige gegevens die zijn opgegeven in de kolommen, verschilt voor elk type back-up.

![lijst van het type back-up](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Back-uptaken beheren

De **back-uptaken** tegel op het kluisdashboard ziet u het aantal taken die in de afgelopen 24 uur wordt uitgevoerd of is mislukt. De tegel bevat een kort overzicht in het menu back-uptaken.

![Back-up items van instellingen](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

Voor meer informatie over de taken, klikt u op **In voortgang** of **mislukt** om het menu back-uptaken is gefilterd op die staat.

### <a name="backup-jobs-menu"></a>Back-uptaken menu

De **back-uptaken** menu geeft informatie weer over het Item type bewerking, Status, begintijd en duur.  

Open het menu back-uptaken in het hoofdmenu van de kluis, te klikken op **back-uptaken**.

![Back-up items van instellingen](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

De lijst met back-uptaken wordt geopend.

![Back-up items van instellingen](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Het menu back-uptaken toont de status voor alle bewerkingen, op alle back-uptypen, voor de afgelopen 24 uur. Gebruik **Filter** de filters wijzigen. De filters worden in de volgende secties beschreven.

De filters wijzigen:

1. In de kluis back-uptaken menu, klikt u op **Filter**.

   ![Back-up items van instellingen](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Het menu Filter wordt geopend.

   ![Back-up items van instellingen](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Kies de instellingen voor het filter en klik op **gedaan**. De gefilterde lijst wordt vernieuwd op basis van de nieuwe instellingen.

#### <a name="item-type"></a>Itemtype

Het itemtype is het type back-upbeheer van het beveiligde exemplaar. Er zijn vier typen; Zie de volgende lijst. U kunt alle itemtypen, of een itemtype weergeven. U kunt twee of drie itemtypen niet selecteren. De beschikbare typen zijn:

* Alle itemtypen
* virtuele Azure-machine
* Bestanden en mappen
* Azure Storage
* Azure-workload

#### <a name="operation"></a>Bewerking

U kunt één bewerking, of alle bewerkingen bekijken. U kunt twee of drie bewerkingen niet selecteren. De beschikbare bewerkingen zijn:

* Alle bewerkingen
* Registreren
* Back-up configureren
* Backup
* Herstellen
* Back-up uitschakelen
* back-upgegevens verwijderen

#### <a name="status"></a>Status

U kunt de Status van alle of een weergeven. U kunt twee of drie statussen niet selecteren. De beschikbare statussen zijn:

* Status van alle
* Voltooid
* Wordt uitgevoerd
* Mislukt
* Geannuleerd
* Voltooid met waarschuwingen

#### <a name="start-time"></a>Begintijd

De dag en tijd waarop de query wordt gestart. De standaardwaarde is een periode van 24 uur.

#### <a name="end-time"></a>Eindtijd

De dag en tijdstip waarop de query eindigt.

### <a name="export-jobs"></a>Taken exporteren

Gebruik **taken exporteren** een werkblad met alle taken menugegevens maken. Het werkblad heeft een blad dat bevat een overzicht van alle taken en afzonderlijke werkbladen voor elke taak.

Als u wilt de gegevens van taken naar een spreadsheet exporteren, klikt u op **taken exporteren**. De service maakt een werkblad met de naam van de kluis en de datum, maar u kunt de naam wijzigen.

## <a name="monitor-backup-usage"></a>Bewaak het gebruik van de back-up

De back-upopslag-tegel in het dashboard ziet u de opslag verbruikt in Azure. Opslaggebruik is van toepassing op:

* LRS-opslaggebruik voor de cloud die is gekoppeld aan de kluis
* GRS-opslaggebruik voor de cloud die is gekoppeld aan de kluis


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Hoe lang duurt het voor de status van de Azure backup-agent om weer te geven in de portal?
De Azure-portal kan maximaal 15 minuten duren in overeenstemming met de status van de Azure backup-agent-taak.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wanneer een back-uptaak is mislukt, hoe lang duurt het voordat een waarschuwing te activeren?
Een waarschuwing wordt gegenereerd binnen 20 minuten van de Azure back-upfouten.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Is er een aanvraag waarin een e-mailbericht niet verzonden als meldingen zijn geconfigureerd?
Ja. In de volgende situaties, worden geen meldingen verzonden.

* Als meldingen per uur worden geconfigureerd en een waarschuwing wordt gegenereerd en opgelost binnen het uur
* Wanneer een taak is geannuleerd
* Als een tweede back-uptaak is mislukt omdat de oorspronkelijke back-uptaak uitgevoerd wordt

## <a name="troubleshooting-monitoring-issues"></a>Het oplossen van problemen met de bewaking

**Probleem:** Taken en/of waarschuwingen van de Azure backup-agent ook niet weergegeven in de portal.

**Stappen voor probleemoplossing:** Het proces ```OBRecoveryServicesManagementAgent```, verzendt de taak en de waarschuwing naar de Azure Backup-service. Van tijd tot tijd dit proces kan zijn vastgelopen of afsluiten.

1. Als u wilt controleren of het proces wordt niet uitgevoerd, opent u **Taakbeheer**, en Controleer ```OBRecoveryServicesManagementAgent``` wordt uitgevoerd.

2. Als het proces niet wordt uitgevoerd, opent u **Configuratiescherm**, en blader door de lijst met services. Starten of opnieuw starten **Microsoft Azure Recovery Services Management Agent**.

    Ga voor meer informatie de logboeken op:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` Bijvoorbeeld:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Volgende stappen
* [Herstellen van WindowsServer of Windows-Client van Azure](backup-azure-restore-windows-server.md)
* Zie voor meer informatie over Azure Backup, [overzicht van Azure-back-up](backup-introduction-to-azure-backup.md)
* Ga naar de [Azure back-Forum](https://go.microsoft.com/fwlink/p/?LinkId=290933)
