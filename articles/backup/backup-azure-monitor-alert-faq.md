---
title: Azure Monitoring waarschuwing Veelgestelde vragen over back-up
description: 'Antwoorden op veelgestelde vragen over: De waarschuwing Azure back-up controleren'
services: backup
author: srinathvasireddy
manager: sivan
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: srinathv
ms.openlocfilehash: bb684f65539b4429862b2dce0e378d8f659d2975
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705031"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Monitoring waarschuwing - Veelgestelde vragen over back-up
In dit artikel vindt u antwoorden op veelgestelde vragen over de Azure monitoring waarschuwing.

## <a name="configure-azure-backup-reports"></a>Azure Backup-rapporten configureren

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>Hoe kan ik als rapportagegegevens is gestart doorgestuurd naar een opslagaccount controleren?
Ga naar het opslagaccount dat u hebt geconfigureerd, en selecteer containers. Als de container een vermelding voor insights-logs-azurebackupreport heeft, betekent dit dat rapportagegegevens is gestart stromen.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Wat is de frequentie van de gegevens-push naar een opslagaccount en de Azure Backup-inhoudspakket in Power BI?
  Het duurt ongeveer 24 uur om gegevens te pushen naar een opslagaccount voor dag 0-gebruikers. Nadat deze initiële push is voltooid, worden gegevens worden vernieuwd met de frequentie wordt weergegeven in de volgende afbeelding.

  * Gegevens met betrekking tot **taken**, **waarschuwingen**, **back-Upitems**, **kluizen**, **beschermde Servers**, en  **Beleid** naar een opslagaccount van de klant wordt gepusht, en wanneer ze wordt vastgelegd.

  * Gegevens met betrekking tot **opslag** wordt gepusht naar een opslagaccount van de klant om de 24 uur.

       ![Azure Backup-rapporten gegevens pushen frequentie](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power BI beschikt over een [geplande vernieuwing eenmaal per dag](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). U kunt handmatig vernieuwen van de gegevens in Power BI uitvoeren voor het inhoudspakket.

### <a name="how-long-can-i-retain-reports"></a>Hoe lang kan ik rapporten behouden?
Wanneer u een storage-account configureert, kunt u een bewaarperiode voor gegevens in de storage-account. Voer stap 6 in de [Opslagaccount voor rapporten](backup-azure-configure-reports.md#configure-storage-account-for-reports) sectie. U kunt ook [rapporten in Excel analyseren](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) en deze wilt opslaan voor een langere bewaarperiode, op basis van uw behoeften.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>Zie ik mijn gegevens in rapporten nadat ik de storage-account configureren?
 Alle gegevens die zijn gegenereerd na het configureren van een storage-account wordt doorgestuurd naar het opslagaccount en is beschikbaar in rapporten. Taken in uitvoering worden niet gepusht voor rapportage. Nadat de taak is voltooid of mislukt, wordt deze verzonden naar rapporten.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Als ik de storage-account om rapporten weer te geven al hebt geconfigureerd, kan ik de configuratie voor het gebruik van een ander opslagaccount wijzigen?
Ja, kunt u de configuratie te verwijzen naar een ander opslagaccount. Gebruik de meest recent geconfigureerde storage-account terwijl u verbinding met de Azure Backup-inhoudspakket maken. Nadat een ander opslagaccount is geconfigureerd, loopt nieuwe gegevens ook in dit opslagaccount. Oudere gegevens (voordat u de configuratie wijzigt) wordt nog steeds in de oudere storage-account.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Kan ik rapporten bekijken in kluizen en abonnementen?
Ja, kunt u hetzelfde opslagaccount in verschillende kluizen om cross-kluis rapporten weer te geven. U kunt ook hetzelfde opslagaccount voor kluizen configureren voor abonnementen. Vervolgens kunt u dit storage-account terwijl u verbinding maken met de Azure Backup-inhoudspakket in Power BI om de rapporten weer te geven. Het geselecteerde opslagaccount moet zich in dezelfde regio als de Recovery Services-kluis.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Hoe lang duurt het voor de status van de Azure backup-agent om weer te geven in de portal?
De Azure-portal kan maximaal 15 minuten duren in overeenstemming met de status van de Azure backup-agent-taak.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wanneer een back-uptaak is mislukt, hoe lang duurt het voordat een waarschuwing te activeren?
Een waarschuwing wordt gegenereerd binnen 20 minuten van de Azure back-upfouten.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Is er een aanvraag waarin een e-mailbericht niet verzonden als meldingen zijn geconfigureerd?
Ja. In de volgende situaties, worden geen meldingen verzonden.

* Als meldingen per uur worden geconfigureerd en een waarschuwing wordt gegenereerd en opgelost binnen het uur
* Wanneer een taak is geannuleerd
* Als een tweede back-uptaak is mislukt omdat de oorspronkelijke back-uptaak uitgevoerd wordt

## <a name="recovery-services-vault"></a>Recovery Services-kluis

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Hoe lang duurt het voor de status van de Azure backup-agent om weer te geven in de portal?
De Azure-portal kan maximaal 15 minuten duren in overeenstemming met de status van de Azure backup-agent-taak.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wanneer een back-uptaak is mislukt, hoe lang duurt het voordat een waarschuwing te activeren?
Een waarschuwing wordt gegenereerd binnen 20 minuten van de Azure back-upfouten.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Is er een aanvraag waarin een e-mailbericht niet verzonden als meldingen zijn geconfigureerd?
Ja. In de volgende situaties, worden geen meldingen verzonden.

* Als meldingen per uur worden geconfigureerd en een waarschuwing wordt gegenereerd en opgelost binnen het uur
* Wanneer een taak is geannuleerd
* Als een tweede back-uptaak is mislukt omdat de oorspronkelijke back-uptaak uitgevoerd wordt

## <a name="next-steps"></a>Volgende stappen

Lees de andere veelgestelde vragen over:

- [Veelgestelde vragen over](backup-azure-vm-backup-faq.md) over back-ups van virtuele Azure-machine.
- [Veelgestelde vragen over](backup-azure-file-folder-backup-faq.md) over de Azure backup-agent
