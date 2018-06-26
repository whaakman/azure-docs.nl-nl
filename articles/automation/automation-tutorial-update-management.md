---
title: Updates en patches voor uw Windows Azure-VM's beheren
description: Dit artikel biedt een overzicht van het gebruik van Azure Automation - Updatebeheer om updates en patches te beheren voor Windows Azure-VM's.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 92258ce7ea39a06f2af85efd9174b1b200710566
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36216963"
---
# <a name="manage-windows-updates-by-using-azure-automation"></a>Windows-updates beheren met behulp van Azure Automation

Met de oplossing voor updatebeheer kunt u updates en patches beheren voor virtuele machines. In deze zelfstudie leert u hoe u snel de status van beschikbare updates beoordeelt, de installatie van vereiste updates plant, de implementatieresultaten bekijkt en een waarschuwing maakt om te controleren of de updates juist zijn toegepast.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Onboarding voor Updatebeheer uitvoeren voor een VM 
> * Een update-evaluatie bekijken
> * Waarschuwingen configureren
> * Een update-implementatie plannen
> * De resultaten van een implementatie bekijken

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw Azure-tegoed voor Visual Studio-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u registreren voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een [Azure Automation-account](automation-offering-get-started.md) voor het opslaan van de watcher- en actie-runbooks en de Watcher-taak.
* Een [virtuele machine](../virtual-machines/windows/quick-create-portal.md) voor de onboarding.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="enable-update-management"></a>Updatebeheer inschakelen

Schakel voor deze zelfstudie eerst updatebeheer in op de VM:

1. Selecteer in Azure Portal in het linkermenu de optie **Virtuele machines**. Selecteer een VM in de lijst.
2. Selecteer op de VM-pagina, onder **BEWERKINGEN** de optie **Updatebeheer**. Het deelvenster **Updatebeheer inschakelen** wordt geopend.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze VM. Deze validatie bevat controles voor een Azure Log Analytics-werkruimte en het gekoppelde Automation-account, en controleert of de oplossing voor updatebeheer zich in de werkruimte bevindt.

Een [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd met functies en services zoals Updatebeheer. De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren.

Tijdens het validatieproces wordt ook gecontroleerd of de VM is ingericht met MMA (Microsoft Monitoring Agent) en Automation Hybrid Runbook Worker. Deze agent wordt gebruikt om te communiceren met Azure Automation en om informatie op te vragen over de status van de update. De agent vereist dat poort 443 is geopend, zodat met de Azure Automation-service kan worden gecommuniceerd en updates kunnen worden gedownload.

Als een van de volgende vereiste onderdelen ontbreekt na de onboarding, wordt dit automatisch toegevoegd:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-werkruimte
* Een [Automation-account](./automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) (ingeschakeld op de VM)

Stel onder **Updatebeheer** het volgende in: de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt. Selecteer vervolgens **Inschakelen**. Als deze opties niet beschikbaar zijn, betekent dit dat er een andere automatiseringsoplossing is ingeschakeld voor de VM. In dit geval moeten dezelfde werkruimte en hetzelfde Automation-account worden gebruikt.

![Schakel het venster voor de oplossing voor updatebeheer in](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Het inschakelen van de oplossing kan enkele minuten duren. Sluit gedurende deze tijd het browservenster niet. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates op de VM naar Log Analytics verzonden. Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

## <a name="view-update-assessment"></a>Update-evaluatie bekijken

Nadat Updatebeheer is ingeschakeld, wordt het deelvenster **Updatebeheer** geopend. Als er updates ontbreken, wordt een lijst met ontbrekende updates weergegeven op het tabblad **Ontbrekende updates**.

Selecteer onder **KOPPELING NAAR INFORMATIE** de koppeling naar de update om het ondersteuningsartikel voor de update te openen in een nieuw venster. In dit venster kunt u belangrijke informatie lezen over de update.

![Updatestatus bekijken](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Klik ergens anders in de update om het deelvenster **Zoeken in logboeken** te openen voor de geselecteerde update. De query voor zoeken in logboeken is vooraf gedefinieerd voor deze specifieke update. U kunt deze query wijzigen of uw eigen query maken om gedetailleerde informatie weer te geven over de updates die zijn geïmplementeerd of die ontbreken in uw omgeving.

![Updatestatus bekijken](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Waarschuwingen configureren

In deze stap stelt u een waarschuwing in die u laat weten wanneer de implementatie van de updates is voltooid. De waarschuwing die u maakt, is gebaseerd op een Log Analytics-query. U kunt een aangepaste query schrijven voor extra waarschuwingen als u rekening wilt houden met veel verschillende scenario’s. Ga in Azure Portal naar **Monitor** en selecteer vervolgens **Waarschuwing maken**. 

Selecteer onder **Regel maken** bij **1. Waarschuwingsvoorwaarde definiëren** de optie **Doel selecteren**. Selecteer onder **Filteren op resourcetype** de optie **Log Analytics**. Selecteer de Log Analytics-werkruimte en selecteer vervolgens **Gereed**.

![Waarschuwing maken](./media/automation-tutorial-update-management/create-alert.png)

Selecteer **Criteria toevoegen**.

Selecteer onder **Signaallogica configureren** in de tabel de optie **Aangepast zoeken in logboeken**. Voer de volgende query in het tekstvak **Zoekquery** in:

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```
Met deze query worden de computers en de naam van de update-uitvoering geretourneerd die binnen het opgegeven tijdsbestek zijn voltooid.

Voer onder **Waarschuwingslogica** voor **Drempelwaarde** in: **1**. Wanneer u klaar bent, selecteert u **Gereed**.

![Signaallogica configureren](./media/automation-tutorial-update-management/signal-logic.png)

Selecteer bij **2. Waarschuwingsdetails definiëren** een naam en beschrijving voor de waarschuwing. Stel **Ernst** in op **Informational(Sev 2)** omdat de waarschuwing is bedoeld voor een geslaagde uitvoering.

![Signaallogica configureren](./media/automation-tutorial-update-management/define-alert-details.png)

Selecteer onder **3. Actiegroep definiëren** de optie **Nieuwe actiegroep**. Een actiegroep is een groep acties die u op meerdere waarschuwingen kunt toepassen. Deze acties kunnen bestaan uit, maar zijn niet beperkt tot, e-mailmeldingen, runbooks, webhooks en nog veel meer. Raadpleeg [Actiegroepen maken en beheren](../monitoring-and-diagnostics/monitoring-action-groups.md) voor meer informatie over actiegroepen.

Voer in het vak **Naam van actiegroep** een naam in voor de waarschuwing en een korte naam. De korte naam wordt gebruikt in plaats van een volledige naam voor de actiegroep als er meldingen worden verzonden door deze groep te gebruiken.

Voer onder **Acties** een naam in voor de actie, bijvoorbeeld **E-mailmeldingen**. Selecteer onder **ACTIETYPE** de optie **E-mail/sms/push/spraak**. Selecteer onder **DETAILS** de optie **Details bewerken**.

Voer in het deelvenster **E-mail/sms/push/spraak** een naam in. Selecteer het selectievakje **E-mail** en voer vervolgens een geldig e-mailadres in.

![Een e-mailactiegroep configureren](./media/automation-tutorial-update-management/configure-email-action-group.png)

Selecteer **OK** in het deelvenster **E-mail/sms/push/spraak**. Selecteer **OK** in het deelvenster **Actiegroep toevoegen**.

Selecteer onder **Regel maken** bij **Acties aanpassen** de optie **E-mailonderwerp** als u het onderwerp van het waarschuwingsbericht wilt aanpassen. Wanneer u klaar bent, selecteert u **Waarschuwingsregel maken**. De waarschuwing laat u weten wanneer een update-implementatie is voltooid en op welke machines deze update-implementatie is uitgevoerd.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Vervolgens plant u een implementatie volgens de releaseplanning en het servicevenster om updates te installeren. U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

> [!WARNING]
> Wanneer opnieuw opstarten is vereist voor een update, wordt de VM automatisch opnieuw opgestart.

Als u een nieuwe update-implementatie voor de VM wilt plannen, gaat u naar **Updatebeheer** en selecteert u vervolgens **Update-implementatie plannen**.

Geef onder **Nieuwe update-implementatie** de volgende informatie op:

* **Naam**: voer een unieke naam in voor de update-implementatie.

* **Besturingssysteem**: selecteer het besturingssysteem dat het doel is voor de update-implementatie.

* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. Voor deze zelfstudie laat u alle typen geselecteerd.

  De classificatietypen zijn:

   |OS  |Type  |
   |---------|---------|
   |Windows     | Essentiële updates</br>Beveiligingsupdates</br>Updatepakketten</br>Functiepakketten</br>Servicepacks</br>Definitie-updates</br>Hulpprogramma's</br>Updates        |
   |Linux     | Essentiële en beveiligingsupdates</br>Andere Updates       |

   Raadpleeg [Updateclassificaties](automation-update-management.md#update-classifications) voor een beschrijving van de classificatietypen.

* **Planningsinstellingen**: het deelvenster **Planningsinstellingen** wordt geopend. De standaard begintijd is 30 minuten na de huidige tijd. U kunt de starttijd op elke gewenste tijd instellen, maar minstens 10 minuten na de huidige tijd.

   U kunt ook opgeven dat de implementatie eenmaal moet worden uitgevoerd, of u kunt een planning met meerdere implementaties instellen. Selecteer onder **Terugkeerpatroon** de optie **Eenmaal**. Laat de standaardwaarde staan op 1 dag en selecteer **OK**. Hiermee stelt u een terugkerend schema in.

* **Onderhoudsvenster (minuten)**: laat hier de standaardwaarde staan. U kunt het tijdvenster instellen waarin de update-implementatie moet plaatsvinden. Met deze instelling zorgt u ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsvensters.

![Deelvenster Planningsinstellingen bijwerken](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Als u klaar bent met het configureren van de planning, selecteert u **Maken**. U keert nu terug naar het statusdashboard. Selecteer **Geplande update-implementaties** om de gemaakte implementatieplanning weer te geven.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** onder **Updatebeheer**. Als de implementatie momenteel wordt uitgevoerd, is de status **Wordt uitgevoerd**. Als de implementatie correct is voltooid, wordt de status gewijzigd in **Geslaagd**. Als er fouten optreden met één of meer updates in de implementatie, verandert de status in **Gedeeltelijk mislukt**.

Selecteer de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor een specifieke implementatie](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Onder **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de VM. In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie.

De volgende lijst toont de beschikbare waarden:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd**: de update is voltooid.
* **Mislukt**: de update is mislukt.

Selecteer **Alle logboeken** als u alle logboekvermeldingen wilt zien die tijdens de implementatie zijn gemaakt.

Selecteer **Uitvoer** om de taakstroom te zien van het runbook dat verantwoordelijk is voor het beheer van de update-implementatie op de doel-VM.

Selecteer **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

Zodra de update-implementatie is voltooid, wordt er een e-mail verzonden die vergelijkbaar is met het volgende voorbeeld, om aan te geven dat de implementatie is geslaagd:

![E-mailactiegroep configureren](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Onboarding voor Updatebeheer uitvoeren voor een VM 
> * Een update-evaluatie bekijken
> * Waarschuwingen configureren
> * Een update-implementatie plannen
> * De resultaten van een implementatie bekijken

Ga door naar het overzicht van de oplossing voor updatebeheer.

> [!div class="nextstepaction"]
> [Oplossing voor updatebeheer](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
