---
title: Back-waarschuwingen voor virtuele Azure-machines controleren
description: Controleer gebeurtenissen en waarschuwingen van de virtuele machine van Azure back-uptaken. Verzenden van e-mail op basis van waarschuwingen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: c5c3f498f62d6399534333f8166a514a10f044c5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310153"
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Waarschuwingen voor back-ups van een virtuele Azure-machine controleren

Waarschuwingen zijn antwoorden van de service een gebeurtenis drempelwaarde bereikt of overschreden. Weten wanneer problemen start kan zijn essentieel voor zakelijke kosten. Waarschuwingen doorgaans niet plaats op basis van een schema, en dus is het handig om te weten zo snel mogelijk na waarschuwingen gegenereerd. Bijvoorbeeld, wanneer een back-up of herstel de taak mislukt, een waarschuwing wordt gegenereerd binnen vijf minuten van de fout. In het dashboard van de kluis bevat de tegel waarschuwingen voor back-up waarschuwing op het niveau en kritieke gebeurtenissen. U kunt alle gebeurtenissen weergeven in de instellingen voor waarschuwingen voor back-up. Maar wat kunt u doen als er een waarschuwing optreedt wanneer u aan een afzonderlijke probleem werkt? Als u niet weet wanneer de waarschuwing gebeurt, wordt een kleine ongemak of deze gegevens in gevaar kan brengen. Als u wilt controleren of dat de juiste personen zich bewust bent van een waarschuwing - wanneer deze zich voordoet, de service voor het verzenden van meldingen van waarschuwingen via e-mail te configureren. Zie voor meer informatie over het instellen van e-mailmeldingen [meldingen configureren](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Hoe vind ik informatie over de waarschuwingen

Als u informatie over de gebeurtenis die een waarschuwing heeft veroorzaakt, moet u de sectie waarschuwingen van de back-up openen. Er zijn twee manieren om te openen van de sectie waarschuwingen van de back-up: een van de waarschuwingen voor back-up tegel in het dashboard van de kluis of in de sectie waarschuwingen en gebeurtenissen.

De back-up waarschuwingen-blade geopend vanaf de tegel waarschuwingen voor back-up:

* Op de **waarschuwingen voor back-up** tegel op het kluisdashboard, klikt u op **kritieke** of **waarschuwing** om de operationele gebeurtenissen voor dat ernstniveau weer te geven.

    ![Back-up van de tegel waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

De back-up waarschuwingen-blade geopend in de sectie waarschuwingen en gebeurtenissen:

1. Klik vanuit het kluisdashboard op **alle instellingen**. ![Knop voor alle instellingen](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Op de **instellingen** blade, klikt u op **waarschuwingen en gebeurtenissen**. ![Knop voor waarschuwingen en gebeurtenissen](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Op de **waarschuwingen en gebeurtenissen** blade, klikt u op **waarschuwingen voor back-up**. ![Knop voor back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts.png)

    De **waarschuwingen voor back-up** sectie wordt geopend en toont de gefilterde waarschuwingen.

    ![Back-up van de tegel waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Klik op de waarschuwing te openen om gedetailleerde informatie over een bepaalde waarschuwing, uit de lijst met gebeurtenissen weer te geven de **Details** sectie.

    ![Gebeurtenisdetail](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Zie voor het aanpassen van de kenmerken die wordt weergegeven in de lijst, [nog een gebeurtenis kenmerken weergeven](backup-azure-monitor-vms.md)

## <a name="configure-notifications"></a>Meldingen configureren

 U kunt de service voor het verzenden van e-mailmeldingen voor waarschuwingen die is opgetreden in het afgelopen uur, of wanneer bepaalde typen gebeurtenissen plaatsvinden.

Voor het instellen van e-mailmeldingen voor waarschuwingen

1. Klik op het menu back-up waarschuwingen **meldingen configureren**

    ![Menu voor back-up waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    De sectie van de meldingen configureren wordt geopend.

    ![Blade meldingen configureren](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Klik in de sectie configureren meldingen voor e-mailmeldingen op **op**.

    De ontvangers en de ernst dialoogvensters hebben een ster naast omdat deze informatie vereist is. Geef ten minste één e-mailadres, en Selecteer minimaal één Ernstniveau.
3. In de **ontvangers (e)** dialoogvenster, typ de e-mailadressen voor wie de meldingen ontvangen. Gebruik de indeling: username@domainname.com. Scheid meerdere e-mailadressen met een puntkomma (;).
4. In de **waarschuwen** gebied, kiest u **Per waarschuwing** te sturen wanneer de opgegeven waarschuwing wordt gegenereerd of **samenvatting per uur** voor het verzenden van een samenvatting voor het afgelopen uur.
5. In de **ernst** dialoogvenster, kiest u een of meer niveaus die u wilt activeren, e-mailmelding.
6. Klik op **Opslaan**.

### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Welke typen waarschuwingen zijn beschikbaar voor Azure IaaS VM backup

   | Waarschuwingsniveau | Waarschuwingen die zijn verzonden |
   | --- | --- |
   | Kritiek | voor back-up mislukt, herstel is mislukt |
   | Waarschuwing | voor back-uptaken is geslaagd met waarschuwingen (bijvoorbeeld: Sommige schrijvers is mislukt tijdens het maken van een momentopname) |
   | Informatief | op dit moment zijn geen informatieve waarschuwingen beschikbaar voor Azure VM backup |

### <a name="situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Situaties waarin de e-mailbericht is niet verzonden, zelfs als er meldingen zijn geconfigureerd

Er zijn situaties waarbij een waarschuwing niet verzonden wordt, zelfs als de meldingen juist zijn geconfigureerd. Meldingen worden in de volgende situaties e-mail niet verzonden om te voorkomen dat waarschuwingsruis:

* Als meldingen zijn geconfigureerd voor samenvatting per uur en een waarschuwing wordt gegenereerd en opgelost binnen het uur.
* De taak is geannuleerd.
* Een back-uptaak wordt geactiveerd en vervolgens mislukt en een andere back-uptaak wordt uitgevoerd.
* Een geplande back-uptaak voor een VM Resource Manager-functionaliteit wordt gestart, maar de virtuele machine niet meer bestaat.

## <a name="using-activity-logs-to-get-notifications-for-successful-backups"></a>Gebruik activiteitenlogboeken voor meldingen voor geslaagde back-ups

> [!NOTE]
> We hebben verplaatst naar een nieuw model moet activiteitenlogboeken van Azure Backup op de Recovery Services-kluizen. Helaas heeft hier last generatie van activiteitenlogboeken in Azure onafhankelijke Clouds. Als gebruikers van Azure onafhankelijke Cloud gemaakt/geconfigureerd geen waarschuwingen van activiteitenlogboeken via Azure Monitor, zoals die hier worden vermeld, zouden ze niet worden geactiveerd. In dat geval zou u wordt geadviseerd dergelijke gebruikers kunnen instellingen voor diagnostische en LA werkruimte of [Power BI rapportageoplossing](backup-azure-configure-reports.md) om de relevante informatie te verkrijgen. Ook wordt in alle Azure openbare regio's, als een gebruiker Recovery Services-activiteitenlogboeken in een Log Analytics-werkruimte verzamelen is zoals vermeld [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity), deze logboeken ook niet weergegeven.

Als u een melding wilt nadat de back-ups zijn gelukt, kunt u waarschuwingen die zijn gebouwd op de [activiteitenlogboeken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) van de kluis.

### <a name="login-into-azure-portal"></a>Meld u aan bij Azure portal

Meld u aan bij de Azure-portal en gaat u verder met de relevante Azure Recovery Services-kluis en klik op de sectie 'Activiteitenlogboek' in de eigenschappen.

### <a name="identify-appropriate-log"></a>Identificeren van de juiste logboek

De filters weergegeven in de volgende afbeelding om te controleren of u activiteitenlogboeken voor geslaagde back-ups ontvangt van toepassing. De timespan dienovereenkomstig wijzigen om records weer te geven.

![Activiteitenlogboeken](./media/backup-azure-monitor-vms/activity-logs-identify.png)

U kunt klikken op het segment 'JSON' voor meer informatie en bekijken door te kopiëren-plakken deze naar een teksteditor. De details van de kluis en een item dat het activiteitenlogboek dat wil zeggen, de back-upitem geactiveerd moet worden weergegeven.

Klik vervolgens op 'Een waarschuwing voor activiteitenlogboek toevoegen' waarschuwingen voor alle deze logboeken worden gegenereerd.

### <a name="add-activity-log-alert"></a>Waarschuwing voor activiteitenlogboek toevoegen

Te klikken op 'Waarschuwing voor activiteitenlogboek toevoegen' ziet u een scherm zoals hieronder wordt weergegeven

![Waarschuwing voor activiteitenlogboek](./media/backup-azure-monitor-vms/activity-logs-alerts-successful.png) het abonnement en resourcegroep worden gebruikt voor het opslaan van de waarschuwing. De criteria worden vooraf ingevuld. Zorg ervoor dat alle waarden zijn relevant zijn voor uw behoeften.

Voor geslaagde back-ups, is het 'niveau' gemarkeerd als 'Ter informatie' en de Status 'Voltooid'.

Als u een bovenstaande 'resource' selecteert, wordt de waarschuwing gegenereerd wanneer de activiteitenlogboeken zijn vastgelegd voor die resource of de kluis. Als u wilt dat de regel van toepassing zijn op alle kluizen, laat u de 'resource' als u wilt niet leeg zijn.

### <a name="define-action-on-alert-firing"></a>Actie definiëren die op de waarschuwing starten

De groep' acties' gebruiken om de actie bij het genereren van een waarschuwing te definiëren. U kunt klikken op 'Actietype' voor meer informatie over de beschikbare acties zoals e-mailadres/SMS/integratie met ITSM-enzovoort.

![Activiteit log actiegroep](./media/backup-azure-monitor-vms/activity-logs-alerts-action-group.png)

Zodra u op OK klikt, wordt een waarschuwing voor activiteitenlogboek wordt gegenereerd en latere activiteitenlogboeken vastgelegd voor geslaagde back-ups worden de actie wordt geactiveerd, zoals gedefinieerd in de actiegroep.

### <a name="limitations-on-alerts"></a>Beperkingen met betrekking tot waarschuwingen

Waarschuwingen op basis van gebeurtenissen zijn afhankelijk van de volgende beperkingen:

1. Waarschuwingen worden geactiveerd op alle virtuele machines in de Recovery Services-kluis. U kunt de waarschuwing voor een subset van virtuele machines in een Recovery Services-kluis niet aanpassen.
2. Waarschuwingen worden verzonden vanuit 'alerts-noreply@mail.windowsazure.com'. U kunt de afzender voor e-mailadres momenteel niet wijzigen.

## <a name="next-steps"></a>Volgende stappen

Bekijk voor meer informatie over het opnieuw maken van een virtuele machine vanaf een herstelpunt [Azure-VM's herstellen](backup-azure-arm-restore-vms.md).

Als u informatie over het beveiligen van uw virtuele machines nodig hebt, raadpleegt u [eerste blik: Maak een back-up van VM's naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md).

Meer informatie over de beheertaken voor back-ups van virtuele machine in het artikel [beheren Azure virtuele machine back-ups](backup-azure-manage-vms.md).
