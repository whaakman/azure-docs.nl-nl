---
title: Azure Automation-runbooks met activiteit Logs and alerts bewaken
description: Dit artikel begeleidt u bij Azure Automation-runbooks met het activiteitenlogboek bewaking
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: a0bd291af98477308cda898580fd52e33b1e6bbd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360378"
---
# <a name="monitoring-runbooks-with-azure-activity-log-alerts"></a>Bewaking van runbooks met Azure-activiteit logboek waarschuwingen

In dit artikel leert u hoe u waarschuwingen op basis van de voltooiingsstatus van runbooks.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure op https://portal.azure.com

## <a name="create-alert"></a>Waarschuwing maken

Waarschuwingen kunnen u voor het definiëren van een voorwaarde om te controleren en een actie moet worden uitgevoerd wanneer deze voorwaarde wordt voldaan.

Navigeer in de Azure-portal naar **alle services** en selecteer **Monitor**. Selecteer op de pagina Monitor **waarschuwingen** en klik op **+ nieuwe waarschuwingsregel**.

### <a name="define-the-alert-condition"></a>De meldingsvoorwaarde definiëren

1. Onder **1. Definieer meldingsvoorwaarde**, klikt u op **+ Selecteer doel**. Kies uw abonnement en klikt u onder **filteren op resourcetype**, selecteer **Automation-Accounts**. Kies uw Automation-Account en klik op **gedaan**.

   ![Selecteer een resource voor de waarschuwing](./media/automation-alert-activity-log/select-resource.png)

### <a name="configure-alert-criteria"></a>Waarschuwingscriteria configureren

1. Klik op **+ criteria toevoegen**. Selecteer **metrische gegevens** voor de **type signaal**, en kies **totaal aantal taken** uit de tabel.

1. De **configureren signaal logica** pagina is waarin het definiëren van de logica waarmee de waarschuwing wordt geactiveerd. Onder de historische grafiek u twee dimensies krijgt **Runbooknaam** en **Status**. Dimensies zijn verschillende eigenschappen voor een metriek die kunnen worden gebruikt om resultaten te filteren. Voor **Runbooknaam**, selecteer het runbook moet op een waarschuwing of niets op waarschuwing op alle runbooks. Voor **Status**, selecteer een status in de vervolgkeuzelijst die u wilt controleren. De runbook-naam en de status waarden die worden weergegeven in de vervolgkeuzelijst zijn alleen voor taken die zijn uitgevoerd in de afgelopen week.

   Als u wilt de waarschuwing bij een status- of runbook die niet in de vervolgkeuzelijst wordt weergegeven, klikt u op de **\+** naast de dimensie. Hiermee wordt een dialoogvenster waarin u kunt invoeren in een aangepaste waarde niet voor deze dimensie onlangs verzonden is geopend. Als u een waarde opgeeft die niet voor een eigenschap bestaat wordt de waarschuwing niet geactiveerd.

1. Onder **waarschuwing logica**, de voorwaarde en de drempelwaarde voor de waarschuwing te definiëren. Een voorbeeld van de voorwaarde gedefinieerd wordt onder weergegeven.

1. Onder **Evaluated op basis van** selecteert u het interval voor de query en hoe vaak u wilt dat deze query is uitgevoerd. Als u bijvoorbeeld **gedurende de laatste vijf minuten** voor **periode** en **elke 1 minuut** voor **frequentie**, de waarschuwing wordt gezocht naar het nummer van runbooks die aan uw criteria wordt voldaan gedurende de afgelopen vijf minuten. Deze query is uitgevoerd elke minuut en zodra de waarschuwingscriteria die u hebt gedefinieerd, is niet meer gevonden in een venster 5 minuten, de waarschuwing wordt opgelost zelf. Wanneer u klaar bent, klikt u op **gedaan**.

   ![Selecteer een resource voor de waarschuwing](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Definieer de details van waarschuwing

1. Selecteer bij **2. Definieer de details van waarschuwing**, geven de waarschuwing in een beschrijvende naam en beschrijving. Stel de **ernst** overeenkomen met uw meldingsvoorwaarde. Er zijn vijf ernstcategorieën gaat van 0 tot 5. De waarschuwingen worden behandeld het dezelfde onafhankelijk van de ernst, kunt u de ernst zodat deze overeenkomen met uw bedrijfslogica koppelen.

1. Aan de onderkant van de sectie wordt een knop waarmee u de regel na voltooiing inschakelen. Regels zijn standaard ingeschakeld bij het maken van. Als u Nee selecteert, kunt u de waarschuwing en deze is gemaakt in een **uitgeschakelde** status. Van de **regels** pagina in de Azure-Monitor, kunt u selecteren en op **inschakelen** voor het inschakelen van de waarschuwing als u klaar bent.

### <a name="define-the-action-to-take"></a>Definieer de actie te ondernemen

1. Onder **3. Actiegroep definiëren**, klikt u op **+ nieuwe actiegroep**. Een actiegroep is een groep van acties die u voor meerdere waarschuwingen gebruiken kunt. Deze kunnen bevatten, maar zijn niet beperkt tot e-mailmeldingen, runbooks, webhooks en nog veel meer. Zie voor meer informatie over actiegroepen, [maken en beheren van actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md)

1. In de **actie groepsnaam** vak, wijs hieraan een beschrijvende naam en een korte naam. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze groep.

1. In de **acties** onder sectie **ACTIETYPE**, selecteer **e-mailadres/SMS/Push/stem**.

1. Op de **e-mailadres/SMS/Push/stem** pagina, een naam geven. Controleer de **e** selectievakje in en voer een geldig e-mailadres moet worden gebruikt.

   ![E-mailbericht actie beheergroep configureren](./media/automation-alert-activity-log/add-action-group.png)

1. Klik op **OK** op de **e-mailadres/SMS/Push/stem** pagina om te sluiten en klik op **OK** sluiten de **actie-groep toevoegen** pagina. De naam die is opgegeven in deze pagina wordt opgeslagen als de **ACTIENAAM**.

1. Na het voltooien klikt u op **Opslaan**. Hiermee maakt u de regel die waarschuwt u wanneer een runbook is voltooid met een bepaalde status.

> [!NOTE]
> Wanneer u een e-mailadres toevoegt aan een groep in te grijpen, wordt een e-mailmelding verzonden waarin dat het adres is toegevoegd aan een groep in te grijpen.

## <a name="notification"></a>Melding

Als de waarschuwing criteria wordt voldaan, wordt de actie gedefinieerd door de actiegroep uitgevoerd. In dit artikel worden bijvoorbeeld wordt een e-mailbericht verzonden. De volgende afbeelding is een voorbeeld van een e-mailbericht wordt weergegeven nadat de waarschuwing wordt geactiveerd:

![E-mailmelding](./media/automation-alert-activity-log/alert-email.png)

Zodra de metriek niet langer buiten de drempelwaarde die is gedefinieerd is, wordt de waarschuwing is uitgeschakeld en de actie gedefinieerd door de actiegroep wordt uitgevoerd. Als een e-actietype is ingeschakeld, wordt een resolutie e-mailadres verzonden met de mededeling is opgelost.

## <a name="next-steps"></a>Volgende stappen

Doorgaan met het volgende artikel voor meer informatie over andere manieren dat u alertings in uw Automation-Account integreren kunt.

> [!div class="nextstepaction"]
> [Een waarschuwing voor het activeren van een Azure Automation-runbook gebruiken](automation-create-alert-triggered-runbook.md)