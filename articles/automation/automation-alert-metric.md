---
title: Azure Automation-runbooks met metrische waarschuwingen controleren
description: Dit artikel begeleidt u bij Azure Automation-runbooks op basis van metrische gegevens controleren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/01/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 7932d057a348957d369ba325044055ac8dfe3428
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58170187"
---
# <a name="monitoring-runbooks-with-metric-alerts"></a>Runbooks met metrische waarschuwingen controleren

In dit artikel leert u hoe u waarschuwingen op basis van de voltooiingsstatus van runbooks te maken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij Azure op https://portal.azure.com

## <a name="create-alert"></a>Waarschuwing maken

Waarschuwingen kunnen u voor het definiëren van een voorwaarde om te controleren en een actie moet worden uitgevoerd wanneer deze voorwaarde wordt voldaan.

Navigeer in de Azure-portal naar uw Automation-Account. Onder **bewaking**, selecteer **waarschuwingen** en klikt u op **+ nieuwe waarschuwingsregel**. Het bereik voor het doel is al gedefinieerd in uw Automation-Account.

### <a name="configure-alert-criteria"></a>Waarschuwingscriteria configureren

1. Klik op **+ criteria toevoegen**. Selecteer **metrische gegevens** voor de **type signaal**, en kies **totaal aan taken dat** uit de tabel.

2. De **signaallogica configureren** pagina is waar u de logica definiëren die de waarschuwing wordt geactiveerd. Onder de historische grafiek, u twee dimensies krijgt **Runbooknaam** en **Status**. Dimensies zijn verschillende eigenschappen voor metrische gegevens die kunnen worden gebruikt om resultaten te filteren. Voor **Runbooknaam**, selecteer het runbook dat u wilt waarschuwen op of laat leeg voor de waarschuwing op alle runbooks. Voor **Status**, selecteert u een status in de vervolgkeuzelijst die u wilt controleren. De runbook-naam en -status waarden die worden weergegeven in de vervolgkeuzelijst worden alleen voor taken die zijn uitgevoerd in de afgelopen week.

   Als u wilt een waarschuwing op basis van een status- of -runbook dat wordt niet in de vervolgkeuzelijst weergegeven, klikt u op de **\+** naast de dimensie. Deze actie wordt een dialoogvenster waarin u kunt invoeren in een aangepaste waarde, die nog niet is verzonden voor deze dimensie onlangs geopend. Als u een waarde die niet bestaat voor een eigenschap die de waarschuwing wordt niet worden geactiveerd.

   > [!NOTE]
   > Als u niet van toepassing is een naam op voor de **RunbookName** dimensie, als er eventuele runbooks die voldoen aan de criteria status en die verborgen systeemrunbooks bevat, ontvangt u een waarschuwing.

3. Onder **waarschuwen logische**, definiëren de voorwaarde en drempelwaarde voor de waarschuwing. Een Preview-versie van de voorwaarde die is gedefinieerd wordt weergegeven onder.

4. Onder **Evaluated op basis van**, selecteert u het interval voor de query en hoe vaak u wilt dat deze query is uitgevoerd. Bijvoorbeeld, als u ervoor kiest **in de afgelopen 5 minuten** voor **periode** en **elke 1 minuut** voor **frequentie**, de waarschuwing wordt gekeken naar het nummer van runbooks die voldoen aan uw criteria in de afgelopen 5 minuten. Deze query wordt elke minuut wordt uitgevoerd en wanneer de waarschuwingscriteria die u hebt gedefinieerd, wordt niet meer gevonden in een venster 5 minuten, de waarschuwing wordt opgelost zelf. Klik op **Gereed** als u klaar bent.

   ![Selecteer een resource voor de waarschuwing](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Waarschuwingsdetails definiëren

1. Selecteer bij **2. Geef details op voor de waarschuwing** en voer een beschrijvende naam en beschrijving in voor de waarschuwing. Stel de **ernst** zodat deze overeenkomen met de voorwaarde voor de waarschuwing. Er zijn vijf variërend van 0 tot 5 dagen per week. De waarschuwingen worden behandeld het dezelfde onafhankelijk van de ernst, kunt u de ernst zodat deze overeenkomt met uw bedrijfslogica koppelen.

1. Aan de onderkant van de sectie wordt een knop waarmee u de regel na voltooiing in te schakelen. Regels zijn standaard ingeschakeld tijdens het maken van. Als u Nee selecteert, u de waarschuwing maken kunt en deze gemaakt een **uitgeschakelde** staat. Uit de **regels** pagina in Azure Monitor kunt u deze selecteren en op **inschakelen** om in te schakelen van de waarschuwing wanneer u klaar bent.

### <a name="define-the-action-to-take"></a>De actie te ondernemen definiëren

1. Selecteer onder **3. Actiegroep definiëren**, klikt u op **+ nieuwe actiegroep**. Een actiegroep is een groep van de acties die u in meer dan één waarschuwing gebruiken kunt. Deze kunnen bevatten, maar niet beperkt tot, e-mailmeldingen, runbooks, webhooks en nog veel meer. Raadpleeg [Actiegroepen maken en beheren](../azure-monitor/platform/action-groups.md) voor meer informatie over actiegroepen

1. Voer in het vak **Naam van de actiegroep** een beschrijvende naam en een korte naam in. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

1. In de **acties** sectie onder **ACTIETYPE**, selecteer **e-mailadres/SMS/Push/stem**.

1. Geef op de pagina **E-mail/sms/push/spraak** een naam op. Schakel het selectievakje **E-mail** in en voer een geldig e-mailadres in dat moet worden gebruikt.

   ![E-mailactiegroep configureren](./media/automation-alert-activity-log/add-action-group.png)

1. Klik op **OK** op de pagina **E-mail/sms/push/spraak** om de pagina te sluiten en klik op **OK** om de pagina **Actiegroep toevoegen** te sluiten. De naam die is opgegeven in deze pagina wordt opgeslagen als de **ACTIENAAM**.

1. Na het voltooien klikt u op **Opslaan**. Deze actie wordt de regel die waarschuwt u wanneer een runbook is voltooid met een bepaalde status gemaakt.

> [!NOTE]
> Als u een e-mailadres toevoegt aan een groep actie, wordt een e-mailmelding verzonden met vermelding van dat het adres is toegevoegd aan een actiegroep.

## <a name="notification"></a>Melding

Als de waarschuwingscriteria wordt voldaan, wordt de actie gedefinieerd door de actiegroep uitgevoerd. In dit artikel worden bijvoorbeeld wordt een e-mailbericht verzonden. De volgende afbeelding is een voorbeeld van een e-mailbericht wordt weergegeven nadat de waarschuwing wordt geactiveerd:

![E-mailmelding](./media/automation-alert-activity-log/alert-email.png)

Zodra de metriek niet langer buiten de drempelwaarde die is gedefinieerd is, wordt de waarschuwing is gedeactiveerd en de actie die is gedefinieerd in de actiegroep wordt uitgevoerd. Als een e-mailbericht actietype is ingeschakeld, wordt een resolutie van e-mailadres verzonden met de mededeling is opgelost.

## <a name="next-steps"></a>Volgende stappen

Doorgaan met het volgende artikel voor meer informatie over andere manieren dat u alertings in uw Automation-Account integreren kunt.

> [!div class="nextstepaction"]
> [Gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook](automation-create-alert-triggered-runbook.md)
