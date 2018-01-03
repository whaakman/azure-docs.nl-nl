---
title: "Monitor voor Resource Manager geïmplementeerde virtuele machine back-ups | Microsoft Docs"
description: "Controleer gebeurtenissen en waarschuwingen van de back-ups van Resource Manager geïmplementeerde virtuele machine. Verzenden van e-mail op basis van waarschuwingen."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: ebd7a886f5853ec3fa9b6e816083e9edd868ef76
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Waarschuwingen voor back-ups van een virtuele Azure-machine controleren
Waarschuwingen zijn reacties van de service een drempelwaarde bereikt of overschreden. Weten wanneer start problemen kan essentieel zijn voor de bedrijven kosten worden beperkt. Waarschuwingen doorgaans niet plaats op een planning en dus is het handig om te weten zo snel mogelijk na het optreden van waarschuwingen. Bijvoorbeeld, als een back-up of herstel de taak is mislukt, een waarschuwing wordt gegenereerd binnen vijf minuten van de fout. Op het kluisdashboard bevat de tegel waarschuwingen voor back-up kritiek en waarschuwingsniveau gebeurtenissen. U kunt alle gebeurtenissen weergeven in de instellingen voor waarschuwingen voor back-up. Maar wat moet u doen als een waarschuwing treedt op wanneer u aan een afzonderlijke probleem werkt? Als u niet weet wanneer de waarschuwing gebeurt, kan dat een kleine ongemak zijn of deze gegevens in gevaar kan brengen. Om te zorgen dat de juiste personen zich bewust bent van een waarschuwing - wanneer deze zich voordoet, de service voor het verzenden van meldingen van waarschuwingen via e-mail te configureren. Zie voor meer informatie over het instellen van e-mailmeldingen [meldingen configureren](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Hoe vind ik informatie over de waarschuwingen
U kunt informatie over de gebeurtenis die een waarschuwing heeft veroorzaakt, moet u de blade back-up waarschuwingen openen. Er zijn twee manieren om de blade back-up waarschuwingen te openen: vanuit de back-up waarschuwingen tegel op het kluisdashboard of vanuit de blade waarschuwingen en gebeurtenissen.

De blade back-up waarschuwingen openen vanaf de tegel waarschuwingen voor back-up:

* Op de **waarschuwingen voor back-up** tegel op het kluisdashboard, klikt u op **Kritiek** of **waarschuwing** om de operationele gebeurtenissen voor die ernstniveau weer te geven.

    ![Tegel back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

De blade back-up waarschuwingen openen vanuit de blade waarschuwingen en gebeurtenissen:

1. Klik in het kluisdashboard op **alle instellingen**. ![Knop voor alle instellingen](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Op de **instellingen** blade, klikt u op **waarschuwingen en gebeurtenissen**. ![Knop voor waarschuwingen en gebeurtenissen](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Op de **waarschuwingen en gebeurtenissen** blade, klikt u op **waarschuwingen voor back-up**. ![Knop voor back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts.png)

    De **waarschuwingen voor back-up** blade wordt geopend en de gefilterde waarschuwingen weergeeft.

    ![Tegel back-waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. U kunt gedetailleerde informatie over een bepaalde waarschuwing, in de lijst van gebeurtenissen, klikt u op de waarschuwing te openen de **Details** blade.

    ![Gebeurtenisdetails](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Zie voor het aanpassen van de kenmerken die wordt weergegeven in de lijst [aanvullende gebeurtenis kenmerken weergeven](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Meldingen configureren
 U kunt de service voor het verzenden van e-mailmeldingen voor waarschuwingen die is opgetreden na verloop van het afgelopen uur of wanneer bepaalde typen gebeurtenissen optreden.

E-mailmeldingen voor waarschuwingen instellen

1. Klik op het menu waarschuwingen voor back-up **meldingen configureren**

    ![Back-menu van waarschuwingen](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    De blade van de meldingen configureren wordt geopend.

    ![Blade meldingen configureren](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Klik op de blade configureren meldingen voor e-mailmeldingen **op**.

    De ontvangers en de ernst dialoogvensters een ster naast ze hebben omdat deze informatie vereist is. Geef ten minste één e-mailadres en selecteer ten minste één ernst.
3. In de **ontvangers (E-mail)** dialoogvenster, typ de e-mailadressen voor wie de meldingen ontvangen. Gebruik de indeling: username@domainname.com. Scheid meerdere e-mailadressen met een puntkomma (;).
4. In de **hoogte** gebied kiezen **Per waarschuwing** melding verzenden wanneer de opgegeven waarschuwing optreedt, of **per uur Digest** voor het verzenden van een samenvatting voor het afgelopen uur.
5. In de **ernst** dialoogvenster, kiest u een of meer niveaus die u wilt activeren, e-mailmeldingen.
6. Klik op **Opslaan**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Welke typen waarschuwingen zijn beschikbaar voor back-up van Azure IaaS VM?
   | Waarschuwingsniveau | Waarschuwingen verzonden |
   | --- | --- |
   | Kritiek |Back-upfouten, herstelfout |
   | Waarschuwing |Geen |
   | Informatief |None |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Zijn er situaties waarin het e-mailbericht zelfs niet is verzonden nadat er meldingen zijn geconfigureerd?
Zijn er situaties waarin een waarschuwing niet is verzonden, zelfs als de meldingen juist is geconfigureerd. In de volgende situaties e-mail worden geen meldingen verzonden om te voorkomen dat de waarschuwing ruis:

* Als meldingen zijn geconfigureerd voor de per uur Digest, en er een waarschuwing gegenereerd en binnen het uur is opgelost.
* De taak is geannuleerd.
* Een back-uptaak wordt geactiveerd en mislukt en een andere back-uptaak wordt uitgevoerd.
* Een geplande back-uptaak voor een VM Resource Manager-functionaliteit wordt gestart, maar de virtuele machine niet meer bestaat.

## <a name="customize-your-view-of-events"></a>De weergave van gebeurtenissen aanpassen
De **controlelogboeken** instelling wordt geleverd met een vooraf gedefinieerde set filters en kolommen met gegevens over operationele gebeurtenissen. U kunt de weergave zodanig aanpassen dat wanneer de **gebeurtenissen** blade wordt geopend, ziet u de gewenste informatie.

1. In de [kluisdashboard](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), blader naar en klikt u op **controlelogboeken** openen de **gebeurtenissen** blade.

    ![Controlelogboeken](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    De **gebeurtenissen** er wordt een blade geopend met de operationele gebeurtenissen gefilterd voor de huidige kluis.

    ![Controlefilter Logboeken](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    De blade ziet u de lijst met kritieke, fout, waarschuwing en informatieve gebeurtenissen die zijn opgetreden in de afgelopen week. De tijdsduur wordt een standaardwaarde ingesteld in de **Filter**. De **gebeurtenissen** blade ziet u ook een staafdiagram bijhouden wanneer de gebeurtenissen is opgetreden. Als u niet zien van het staafdiagram wilt in de **gebeurtenissen** menu, klikt u op **verbergen grafiek** buiten de grafiek in-of uitschakelen. De standaardweergave van gebeurtenissen bevat informatie voor bewerking, niveau, Status, bronnen en tijd. Zie de sectie voor informatie over het blootstellen van extra kenmerken van de gebeurtenis, [uitbreiden gebeurtenisgegevens](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Voor meer informatie over een operationele gebeurtenissen in de **bewerking** kolom, klikt u op een operationele gebeurtenissen om de blade te openen. De blade bevat gedetailleerde informatie over de gebeurtenissen. Gebeurtenissen zijn gegroepeerd op hun correlatie-ID en een lijst van de gebeurtenissen die is opgetreden in de tijdsduur.

    ![Details van bewerking](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. U kunt gedetailleerde informatie over een bepaalde gebeurtenis, in de lijst van gebeurtenissen, klikt u op de gebeurtenis te openen de **Details** blade.

    ![Gebeurtenisdetails](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Niveau van gebeurtenissen bevat gedetailleerde informatie opgehaald. Als u liever dat veel informatie over elke gebeurtenis te zien en wilt dit veel details die moeten worden toevoegen de **gebeurtenissen** blade, Zie de sectie [uitbreiden gebeurtenisgegevens](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-the-event-filter"></a>Het gebeurtenisfilter aanpassen
Gebruik de **Filter** aan te passen of kies de informatie die wordt weergegeven in een bepaalde blade. Voor het filteren van gegevens van de gebeurtenis:

1. In de [kluisdashboard](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), blader naar en klikt u op **controlelogboeken** openen de **gebeurtenissen** blade.

    ![Controlelogboeken](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    De **gebeurtenissen** er wordt een blade geopend met de operationele gebeurtenissen gefilterd voor de huidige kluis.

    ![Controlefilter Logboeken](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. Op de **gebeurtenissen** menu, klikt u op **Filter** om deze blade te openen.

    ![Open de blade filteren](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. Op de **Filter** blade aanpassen de **niveau**, **tijdsspanne**, en **aanroeper** filters. De andere filters zijn niet beschikbaar omdat ze zijn ingesteld op de huidige informatie voor de Recovery Services-kluis.

    ![Details van de audit-logboeken-query](./media/backup-azure-monitor-vms/filter-blade.png)

    U kunt opgeven de **niveau** van gebeurtenis: kritiek, fout, waarschuwing of ter informatie. U kunt een combinatie van gebeurtenisniveaus, maar u moet ten minste één niveau geselecteerd hebben. Het uitschakelen van het niveau van de in- of uitschakelen. De **tijdsspanne** filter kunt u de tijdsduur voor het vastleggen van gebeurtenissen opgeven. Als u een aangepaste tijdsspanne gebruikt, kunt u de begin- en eindtijden instellen.
4. Zodra u klaar bent voor het opvragen van de operations-logboeken met uw filter, klikt u op **Update**. De resultaten weergegeven in de **gebeurtenissen** blade.

    ![Details van bewerking](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Aanvullende gebeurtenis weergavekenmerken
Met behulp van de **kolommen** knop, kunt u aanvullende gebeurtenis kenmerken worden weergegeven in de lijst op de **gebeurtenissen** blade. De lijst met gebeurtenissen geeft informatie weer voor bewerking, niveau, Status, bronnen en tijd. Extra kenmerken inschakelen:

1. Op de **gebeurtenissen** blade, klikt u op **kolommen**.

    ![Open kolommen](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    De **kolommen kiezen** blade wordt geopend.

    ![Blade kolommen](./media/backup-azure-monitor-vms/columns-blade.png)
2. Klik op het selectievakje in het kenmerk. Het kenmerk selectievakje in-of uitschakelen in- of uitschakelen.
3. Klik op **opnieuw** opnieuw instellen van de lijst met kenmerken in de **gebeurtenissen** blade. Gebruik na het toevoegen of verwijderen van kenmerken in de lijst, **opnieuw** om de nieuwe lijst met kenmerken van de gebeurtenis weer te geven.
4. Klik op **bijwerken** kenmerken van de gegevens in de gebeurtenisstroom bijwerken. De volgende tabel bevat informatie over elk kenmerk.

| Kolomnaam | Beschrijving |
| --- | --- |
| Bewerking |De naam van de bewerking |
| Niveau |Het niveau van de bewerking, waarden die kunnen worden: informatief, waarschuwing, fout of kritiek |
| Status |Beschrijvende status van de bewerking |
| Resource |URL van de bron. ook wel bekend als de resource-ID |
| Time |Tijd, gemeten vanaf de huidige tijd, wanneer de gebeurtenis heeft plaatsgevonden |
| Caller |Wie of wat aangeroepen of de gebeurtenis; het systeem of een gebruiker |
| Timestamp |De tijd waarop de gebeurtenis is geactiveerd |
| Resourcegroep |De gekoppelde resourcegroep |
| Resourcetype |Het interne resourcetype dat wordt gebruikt door Resource Manager |
| Abonnements-id |De gekoppelde abonnements-ID |
| Category |Categorie van de gebeurtenis |
| Correlatie-id |Gemeenschappelijke op verwante gebeurtenissen |

## <a name="use-powershell-to-customize-alerts"></a>PowerShell gebruiken om aan te passen van waarschuwingen
U kunt aangepaste meldingen van waarschuwingen voor de taken in de portal. Als u deze taken, regels voor waarschuwingen op basis van PowerShell te definiëren op de operationele Logboeken gebeurtenissen. Gebruik *PowerShell versie 1.3.0 of hoger*.

Gebruik een opdracht zoals het volgende script definiëren van een aangepaste melding om te waarschuwen voor mislukte back-ups:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : U kunt ResourceId ophalen uit de controlelogboeken. De ResourceId is een URL die is opgegeven in de kolom bron van de logboeken van de bewerking.

**OperationName** : OperationName is in de notatie ' Microsoft.RecoveryServices/recoveryServicesVault/*EventName*"waar *EventName* kan:<br/>

* Registreren <br/>
* Registratie ongedaan maken <br/>
* ConfigureProtection <br/>
* Back-up maken <br/>
* Herstellen <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Status** : ondersteunde waarden zijn gestart, is voltooid of mislukt.

**ResourceGroup** : dit is de resourcegroep waartoe de resource behoort. U kunt de resourcegroep-kolom toevoegen aan de gegenereerde logboeken. Resourcegroep is een van de beschikbare typen van informatie over de gebeurtenis.

**Naam** : naam van de waarschuwingsregel.

**CustomEmail** : Geef de aangepaste e-mailadres waarnaar u wilt verzenden van een melding van waarschuwingen

**SendToServiceOwners** : deze optie waarschuwingsmeldingen worden verzonden naar alle beheerders en medebeheerders van het abonnement. Kan worden gebruikt in **nieuw AzureRmAlertRuleEmail** cmdlet

### <a name="limitations-on-alerts"></a>Beperkingen voor waarschuwingen
Waarschuwingen op basis van gebeurtenissen zijn onderworpen aan de volgende beperkingen:

1. Waarschuwingen worden verzonden op alle virtuele machines in de Recovery Services-kluis. U kunt de waarschuwing voor een subset van virtuele machines in een Recovery Services-kluis niet aanpassen.
2. Deze functie is in Preview. [Meer informatie](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Waarschuwingen worden verzonden vanuit 'alerts-noreply@mail.windowsazure.com'. U kunt de afzender voor e-mailadres momenteel niet wijzigen.

## <a name="next-steps"></a>Volgende stappen
Gebeurtenislogboeken geweldige postmortemkeuring inschakelen en ondersteuning voor de back-upbewerkingen controleren. De volgende bewerkingen worden geregistreerd:

* Registreren
* Registratie ongedaan maken
* Beveiliging configureren
* Back-up (zowel gepland en de back-up op aanvraag)
* Herstellen
* Stop de beveiliging
* Back-upgegevens verwijderen
* Beleid toevoegen
* Beleid verwijderen
* Beleid bijwerken
* Taak annuleren

Voor een brede uitleg van gebeurtenissen, bewerkingen en controlelogboeken alle Azure-services, Zie het artikel [gebeurtenissen bekijken en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Bekijk voor meer informatie over het opnieuw maken van een virtuele machine vanaf een herstelpunt [Azure Virtual machines herstellen](backup-azure-arm-restore-vms.md). Als u informatie over het beveiligen van uw virtuele machines, Zie [eerste kennismaking: Maak een Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md). Meer informatie over de beheertaken voor VM-back-ups in het artikel [beheren Azure virtuele machine back-ups](backup-azure-manage-vms.md).
