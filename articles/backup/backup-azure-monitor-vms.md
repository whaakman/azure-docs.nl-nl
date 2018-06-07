---
title: Back-waarschuwingen voor Azure virtual machines controleren
description: Controleer gebeurtenissen en waarschuwingen van de virtuele machine van Azure back-uptaken. Verzenden van e-mail op basis van waarschuwingen.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: markgal
ms.openlocfilehash: 3783014738ec4e8f185531773b1259dc63e7f49f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606304"
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Waarschuwingen voor back-ups van een virtuele Azure-machine controleren
Waarschuwingen zijn reacties van de service een drempelwaarde bereikt of overschreden. Weten wanneer start problemen kan essentieel zijn voor de bedrijven kosten worden beperkt. Waarschuwingen doorgaans niet plaats op een planning en dus is het handig om te weten zo snel mogelijk na het optreden van waarschuwingen. Bijvoorbeeld, als een back-up of herstel de taak is mislukt, een waarschuwing wordt gegenereerd binnen vijf minuten van de fout. Op het kluisdashboard bevat de tegel waarschuwingen voor back-up kritiek en waarschuwingsniveau gebeurtenissen. U kunt alle gebeurtenissen weergeven in de instellingen voor waarschuwingen voor back-up. Maar wat moet u doen als een waarschuwing treedt op wanneer u aan een afzonderlijke probleem werkt? Als u niet weet wanneer de waarschuwing gebeurt, kan dat een kleine ongemak zijn of deze gegevens in gevaar kan brengen. Om te zorgen dat de juiste personen zich bewust bent van een waarschuwing - wanneer deze zich voordoet, de service voor het verzenden van meldingen van waarschuwingen via e-mail te configureren. Zie voor meer informatie over het instellen van e-mailmeldingen [meldingen configureren](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Hoe vind ik informatie over de waarschuwingen
U kunt informatie over de gebeurtenis die een waarschuwing heeft veroorzaakt, moet u de sectie waarschuwingen van de back-up openen. Er zijn twee manieren om te openen in de sectie waarschuwingen van de back-up: vanaf de back-up waarschuwingen tegel in het kluisdashboard of in de sectie waarschuwingen en gebeurtenissen.

De blade back-up waarschuwingen openen vanaf de tegel waarschuwingen voor back-up:

* Op de **waarschuwingen voor back-up** tegel op het kluisdashboard, klikt u op **Kritiek** of **waarschuwing** om de operationele gebeurtenissen voor die ernstniveau weer te geven.

    ![Tegel back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

De blade back-up waarschuwingen openen vanaf de sectie waarschuwingen en gebeurtenissen:

1. Klik in het kluisdashboard op **alle instellingen**. ![Knop voor alle instellingen](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Op de **instellingen** blade, klikt u op **waarschuwingen en gebeurtenissen**. ![Knop voor waarschuwingen en gebeurtenissen](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Op de **waarschuwingen en gebeurtenissen** blade, klikt u op **waarschuwingen voor back-up**. ![Knop voor back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts.png)

    De **waarschuwingen voor back-up** sectie wordt geopend en de gefilterde waarschuwingen weergeeft.

    ![Tegel back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. U kunt gedetailleerde informatie over een bepaalde waarschuwing, in de lijst van gebeurtenissen, klikt u op de waarschuwing te openen de **Details** sectie.

    ![Gebeurtenisdetail](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Zie voor het aanpassen van de kenmerken die wordt weergegeven in de lijst [aanvullende gebeurtenis kenmerken weergeven](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Meldingen configureren
 U kunt de service voor het verzenden van e-mailmeldingen voor waarschuwingen die is opgetreden na verloop van het afgelopen uur of wanneer bepaalde typen gebeurtenissen optreden.

E-mailmeldingen voor waarschuwingen instellen

1. Klik op het menu waarschuwingen voor back-up **meldingen configureren**

    ![Back-menu van waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    De sectie van de meldingen configureren wordt geopend.

    ![Blade meldingen configureren](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Klik in de sectie configureren meldingen voor e-mailmeldingen op **op**.

    De ontvangers en de ernst dialoogvensters een ster naast ze hebben omdat deze informatie vereist is. Geef ten minste één e-mailadres en selecteer ten minste één ernst.
3. In de **ontvangers (E-mail)** dialoogvenster, typ de e-mailadressen voor wie de meldingen ontvangen. Gebruik de indeling: username@domainname.com. Scheid meerdere e-mailadressen met een puntkomma (;).
4. In de **hoogte** gebied kiezen **Per waarschuwing** melding verzenden wanneer de opgegeven waarschuwing optreedt, of **per uur Digest** voor het verzenden van een samenvatting voor het afgelopen uur.
5. In de **ernst** dialoogvenster, kiest u een of meer niveaus die u wilt activeren, e-mailmeldingen.
6. Klik op **Opslaan**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Welke typen waarschuwingen zijn beschikbaar voor back-up van Azure IaaS VM?
   | Waarschuwingsniveau | Waarschuwingen verzonden |
   | --- | --- |
   | Kritiek | voor back-up mislukt, herstelfout |
   | Waarschuwing | voor back-uptaken is geslaagd met waarschuwingen (bijvoorbeeld: Sommige schrijvers is mislukt tijdens het maken van een momentopname) |
   | Informatief | Er is geen informatieve waarschuwingen zijn momenteel beschikbaar voor virtuele machine van Azure back-up |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Zijn er situaties waarin het e-mailbericht zelfs niet is verzonden nadat er meldingen zijn geconfigureerd?
Zijn er situaties waarin een waarschuwing niet is verzonden, zelfs als de meldingen juist is geconfigureerd. In de volgende situaties e-mail worden geen meldingen verzonden om te voorkomen dat de waarschuwing ruis:

* Als meldingen zijn geconfigureerd voor de per uur Digest, en er een waarschuwing gegenereerd en binnen het uur is opgelost.
* De taak is geannuleerd.
* Een back-uptaak wordt geactiveerd en mislukt en een andere back-uptaak wordt uitgevoerd.
* Een geplande back-uptaak voor een VM Resource Manager-functionaliteit wordt gestart, maar de virtuele machine niet meer bestaat.

## <a name="using-activity-logs-to-get-notifications-for-successful-backups"></a>Met behulp van activiteitenlogboeken meldingen voor geslaagde back-ups wilt ontvangen

Als u worden gewaarschuwd wilt nadat de back-ups zijn voltooid, kunt u waarschuwingen die zijn gebaseerd op de [activiteitenlogboeken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) van de kluis.

### <a name="login-into-azure-portal"></a>Aanmelding bij Azure-portal
Aanmelden bij de Azure-portal en gaat u verder met de relevante Azure Recovery Services-kluis en klik op de sectie 'Activiteitenlogboek' in de eigenschappen.

### <a name="identify-appropriate-log"></a>Juiste logboek identificeren

De weergegeven in de volgende afbeelding om te controleren of u activiteitenlogboeken voor geslaagde back-ups zijn ontvangen filters toepassen. De timespan dienovereenkomstig wijzigen om records weer te geven.

![Activiteitenlogboeken](./media/backup-azure-monitor-vms/activity-logs-identify.png)

U kunt klikken op het segment 'JSON' voor meer informatie en bekijken door te kopiëren-plakken deze naar een teksteditor. De details van de kluis en het item wat het activiteitenlogboek dat wil zeggen, het back-item moet worden weergegeven.

Klik vervolgens op 'Activiteit logboek waarschuwing toevoegen' voor het genereren van waarschuwingen voor alle dergelijke Logboeken.

### <a name="add-activity-log-alert"></a>Waarschuwing voor het logboek van activiteit toevoegen

Te klikken op 'Melding toevoegen activiteit log' ziet u een scherm zoals hieronder wordt weergegeven

![Waarschuwing voor een activiteitenlogboek](./media/backup-azure-monitor-vms/activity-logs-alerts-successful.png)
    
Het abonnement en resourcegroep worden gebruikt voor het opslaan van de waarschuwing. De criteria worden vooraf ingevuld. Zorg ervoor dat alle waarden relevant zijn voor uw behoeften.

Voor geslaagde back-ups is 'Niveau' gemarkeerd als 'Ter informatie' en Status als 'Voltooid'.

Als u een bovenstaande 'resource' selecteert, wordt de waarschuwing gegenereerd wanneer de activiteitenlogboeken zijn vastgelegd voor die bron of de kluis. Als u wilt dat de regel van toepassing zijn op alle kluizen, laat u de 'resource' als u wilt niet leeg zijn.

### <a name="define-action-on-alert-firing"></a>Actie definiëren op waarschuwing starten

Gebruik de actiegroep' ' om de actie bij het genereren van een waarschuwing te definiëren. U kunt klikken op 'Action-type' voor meer informatie over de beschikbare acties zoals e-mail, de SMS/het integratie met ITSM enzovoort.

![Activiteitengroep logboek actie](./media/backup-azure-monitor-vms/activity-logs-alerts-action-group.png)


Wanneer u op OK klikt, een activiteit logboek waarschuwing wordt gegenereerd en latere activiteitenlogboeken voor geslaagde back-ups vastgelegd de actie wordt geactiveerd, zoals gedefinieerd in de groep in te grijpen.

### <a name="limitations-on-alerts"></a>Beperkingen voor waarschuwingen
Waarschuwingen op basis van gebeurtenissen zijn onderworpen aan de volgende beperkingen:

1. Waarschuwingen worden verzonden op alle virtuele machines in de Recovery Services-kluis. U kunt de waarschuwing voor een subset van virtuele machines in een Recovery Services-kluis niet aanpassen.
2. Waarschuwingen worden verzonden vanuit 'alerts-noreply@mail.windowsazure.com'. U kunt de afzender voor e-mailadres momenteel niet wijzigen.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over het opnieuw maken van een virtuele machine vanaf een herstelpunt [Azure Virtual machines herstellen](backup-azure-arm-restore-vms.md).

Als u informatie over het beveiligen van uw virtuele machines, Zie [eerste kennismaking: Maak een Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md). 

Meer informatie over de beheertaken voor VM-back-ups in het artikel [beheren Azure virtuele machine back-ups](backup-azure-manage-vms.md).
