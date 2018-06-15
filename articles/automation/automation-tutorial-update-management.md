---
title: Updates en patches voor uw Windows Azure-VM's beheren
description: Dit artikel bevat een overzicht van het gebruik van Azure Automation - Updatebeheer om updates en patches voor uw Windows Azure-VM's te beheren.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 84ec2a5852e6aaeb4b9fe6ef11924209d03fb54b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34054756"
---
# <a name="manage-windows-updates-with-azure-automation"></a>Windows-updates beheren met Azure Automation

Met Updatebeheer kunt u updates en patches voor uw virtuele machines beheren.
In deze zelfstudie leert u hoe snel de status van de beschikbare updates beoordeelt, de installatie van vereiste updates plant, implementatieresultaat kunt bekijken en een waarschuwing kunt maken om te controleren of updates correct zijn toegepast.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VM onboarden voor updatebeheer
> * Een update-evaluatie bekijken
> * Waarschuwingen configureren
> * Een update-implementatie plannen
> * De resultaten van een implementatie bekijken

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw Azure-tegoed activeren voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een [Automation-account](automation-offering-get-started.md) voor opslag van de Watcher- en actie-runbooks en de Watcher-taak.
* Een [virtuele machine](../virtual-machines/windows/quick-create-portal.md) voor de onboarding.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="enable-update-management"></a>Updatebeheer inschakelen

Voor deze zelfstudie moet u eerst updatebeheer inschakelen op uw virtuele machine.

1. Selecteer in het menu links in de Azure Portal **Virtuele machines** en selecteer een virtuele machine in de lijst
2. Klik op de pagina met virtuele machines onder **Updatebeheer** op het kopje **Bewerkingen**. De pagina **Updatebeheer inschakelen** wordt geopend.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze virtuele machine. Deze validatie bevat controles voor een Log Analytics-werkruimte en het gekoppelde Automation-account en controleert of de oplossing Updatebeheer zich in de werkruimte bevindt.

Een [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd door functies en services zoals Updatebeheer. De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren.

Tijdens het validatieproces wordt ook gecontroleerd of de virtuele machine is ingericht met Microsoft Monitoring Agent (MMA) en Automation Hybrid Runbook Worker.
Deze agent wordt gebruikt om met Azure Automation te communiceren en om informatie op te vragen over de status van de update. De agent vereist dat poort 443 is geopend, zodat met de Azure Automation-service kan worden gecommuniceerd en updates kunnen worden gedownload.

Als een van de volgende vereiste onderdelen ontbreekt na de onboarding, wordt dit automatisch toegevoegd:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-werkruimte
* [Automation-account](./automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) wordt ingeschakeld op de virtuele machine.

Het scherm **Updatebeheer** wordt geopend. Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt en klik op **Inschakelen**. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de VM en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt.

![Venster om oplossing voor updatebeheer in te schakelen](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Het inschakelen van de oplossing kan enkele minuten duren. Gedurende deze tijd mag u het browservenster niet sluiten.
Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates op de VM naar Log Analytics verzonden.
Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

## <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven.
Als er updates ontbreken, ziet u een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

Selecteer **KOPPELING NAAR INFORMATIE** voor een update om het ondersteuningsartikel voor de update in een nieuw venster te openen. Hier vindt u belangrijke informatie over de update.

![Updatestatus bekijken](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Als u ergens anders op de update klikt, wordt het venster **Zoeken in logboeken** geopend voor de geselecteerde update. De query voor zoeken in logboeken is vooraf gedefinieerd voor die specifieke update. U kunt deze query wijzigen of uw eigen query maken om gedetailleerde informatie over de geïmplementeerde of ontbrekende updates in uw omgeving weer te geven.

![Updatestatus bekijken](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerting"></a>Waarschuwingen configureren

In deze stap configureert u een waarschuwing die u laat weten wanneer de updates met succes zijn geïmplementeerd. De waarschuwing die u maakt, is gebaseerd op een Log Analytics-query. Er kan een willekeurige aangepaste query worden geschreven voor extra waarschuwingen voor tal van uiteenlopende scenario’s. Ga in de Azure Portal naar **Controleren** en klik op **Waarschuwing maken**. hiermee wordt de pagina **Regel maken** geopend.

Klik onder **1. Meldingsvoorwaarde maken** op **+ Doel selecteren**. Selecteer onder **Filteren op resourcetype** de optie **Log Analytics**. Kies uw Log Analytics-werkruimte en klik op **Gereed**.

![waarschuwing maken](./media/automation-tutorial-update-management/create-alert.png)

Klik op de knop **+ Criteria toevoegen** om de pagina **Signaallogica configureren** te openen. Kies **Aangepast zoeken in logboeken** in de tabel. Voer in het tekstvak **Zoekquery** te volgende query in. Deze query retourneert de computers en de uitvoeringsnaam van de update die binnen het opgegeven tijdsbestek werd uitgevoerd.

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```

Voer **1** in als **Drempelwaarde** voor de waarschuwingslogica. Klik op **Gereed** als u klaar bent.

![Signaallogica configureren](./media/automation-tutorial-update-management/signal-logic.png)

Selecteer bij **2. Geef details op voor de waarschuwing** en voer een beschrijvende naam en beschrijving in voor de waarschuwing. Stel de **Ernst** in op **Informatief (ernst 2)**: de waarschuwing is voor succesvolle uitvoering.

![Signaallogica configureren](./media/automation-tutorial-update-management/define-alert-details.png)

Klik onder **3. Actiegroep definiëren** op **+ Nieuwe actiegroep**. Een actiegroep is een groep acties die u op meerdere waarschuwingen kunt toepassen. Deze kunnen bestaan uit, maar zijn niet beperkt tot, e-mailmeldingen, runbooks, webhooks en nog veel meer. Raadpleeg [Actiegroepen maken en beheren](../monitoring-and-diagnostics/monitoring-action-groups.md) voor meer informatie over actiegroepen

Voer in het vak **Naam van de actiegroep** een beschrijvende naam en een korte naam in. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

Geef de actie onder **Acties** een beschrijvende naam, zoals **E-mailmeldingen**, en selecteer onder **ACTIETYPE** **E-mail/sms/push/spraak**. Selecteer onder **DETAILS** de optie **Details bewerken**.

Geef op de pagina **E-mail/sms/push/spraak** een naam op. Schakel het selectievakje **E-mail** in en voer een geldig e-mailadres in dat moet worden gebruikt.

![E-mailactiegroep configureren](./media/automation-tutorial-update-management/configure-email-action-group.png)

Klik op **OK** op de pagina **E-mail/sms/push/spraak** om de pagina te sluiten en klik op **OK** om de pagina **Actiegroep toevoegen** te sluiten.

U kunt het onderwerp van de e-mail die zal worden verzonden aanpassen door op **E-mailonderwerp** onder **Acties aanpassen** op de pagina **Regel maken** te klikken. Klik op **Waarschuwingsregel maken** als u klaar bent. Er wordt nu een regel gemaakt die u waarschuwt als een update met succes is geïmplementeerd. Ook ontvangt u bericht over de machines waarop de update-implementatie werd uitgevoerd.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Nu de waarschuwingen zijn geconfigureerd, kunt u een implementatie na uw release-planning en servicevenster plannen om updates te installeren.
U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie.
Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

> [!WARNING]
> Wanneer updates vereisen dat de VM opnieuw moet worden opgestart, wordt deze automatisch opnieuw opgestart.

Plan een nieuwe update-implementatie voor de VM door terug te gaan naar **Updatebeheer** en **Implementatie van de update plannen**, bovenaan op het scherm, te selecteren.

Geef de volgende gegevens op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor de update-implementatie.

* **Besturingssysteem**: kies het besturingssysteem dat het doel is voor de update-implementatie.

* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. Voor deze zelfstudie laat u alle typen geselecteerd.

  De classificatietypen zijn:

   |OS  |Type  |
   |---------|---------|
   |Windows     | Essentiële updates</br>Beveiligingsupdates</br>Updatepakketten</br>Functiepakketten</br>Servicepacks</br>Definitie-updates</br>Hulpprogramma's</br>Updates        |
   |Linux     | Essentiële en beveiligingsupdates</br>Andere Updates       |

   Raadpleeg [Updateclassificaties](automation-update-management.md#update-classifications) voor een beschrijving van de classificatietypen.

* **Schema-instellingen** - Hiermee wordt de pagina Schema-instellingen geopend. De standaard begintijd is 30 minuten na de huidige tijd. De tijd kan worden ingesteld op een willekeurig moment vanaf 10 minuten in de toekomst.

   U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen.
   Selecteer **Eenmaal** onder **Terugkeerpatroon**. Laat de standaardwaarde op 1 dag staan en klik op **OK**. Hiermee stelt u een terugkerend schema in.

* **Onderhoudsvenster (minuten)**: laat deze waarde op de standaardwaarde staan. U kunt opgeven binnen welke periode de update-implementatie moet plaatsvinden. Met deze instelling zorgt u ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsperioden (vensters).

![Scherm met instellingen voor de updateplanning](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Nadat u de planning hebt geconfigureerd, klikt u op de knop **Maken**. U keert terug naar het statusdashboard. Selecteer **Geplande update-implementaties** om de gemaakte implementatieplanning weer te geven.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
De status wordt weergegeven als **Wordt uitgevoerd** terwijl de implementatie wordt uitgevoerd.
Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er fouten optreden met één of meer updates in de implementatie, verandert de status in **Gedeeltelijk mislukt**.
Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor specifieke implementatie](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Op de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de VM.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie.
De volgende lijst toont de beschikbare waarden:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd**: de update is voltooid.
* **Mislukt**: de update is mislukt.

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheer van de implementatie van de updates op de doel-VM.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

Nadat uw update met succes is geïmplementeerd, wordt er een e-mail die vergelijkbaar is met de volgende afbeelding verzonden om te vertellen dat de implementatie is geslaagd.

![E-mailactiegroep configureren](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een VM onboarden voor updatebeheer
> * Een update-evaluatie bekijken
> * Waarschuwingen configureren
> * Een update-implementatie plannen
> * De resultaten van een implementatie bekijken

Ga door naar het overzicht van de oplossing voor updatebeheer.

> [!div class="nextstepaction"]
> [Oplossing voor updatebeheer](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
