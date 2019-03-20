---
title: Waarschuwingen voor metrische gegevens voor Azure Database voor MariaDB configureren in Azure portal
description: Dit artikel wordt beschreven hoe u configureert en toegang tot metrische waarschuwingen voor Azure Database voor MariaDB vanuit Azure portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/07/2019
ms.openlocfilehash: ce9804a4ecad4cd0f2e797038dc10c71b50de9bf
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122709"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mariadb"></a>De Azure portal gebruiken voor het instellen van waarschuwingen over metrische gegevens voor Azure Database voor MariaDB

Dit artikel leest u hoe het instellen van Azure Database voor MariaDB waarschuwingen via Azure portal. U kunt een waarschuwing op basis van metrische gegevens voor uw Azure-services controleren ontvangen.

De waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metrische gegevens van een drempel die u toewijst overschrijden. De waarschuwing triggers beide wanneer de voorwaarde wordt voldaan en klik daarna als die voorwaarde is niet meer wordt voldaan aan.

Een waarschuwing als u wilt de volgende acties uitvoeren wanneer deze wordt geactiveerd, kunt u configureren:
* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar extra e-mailadressen die u opgeeft.
* Een webhook aanroepen

U kunt configureren en informatie over met behulp van regels voor waarschuwingen:
* [Azure-portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure-CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric"></a>Een waarschuwingsregel maken op een metrische waarde
1. In de [Azure-portal](https://portal.azure.com/), selecteert u de Azure Database voor MariaDB-server die u wilt bewaken.

2. Onder de **bewaking** sectie van de zijbalk Selecteer **waarschuwingen** zoals wordt weergegeven:

   ![Selecteer waarschuwingsregels](./media/howto-alert-metric/2-alert-rules.png)

3. Selecteer **metrische waarschuwing toevoegen** (+ pictogram).

4. De **maken regel** pagina wordt geopend, zoals hieronder weergegeven. Vul de vereiste gegevens:

   ![Metrische waarschuwing toevoegen](./media/howto-alert-metric/4-add-rule-form.png)

5. Binnen de **voorwaarde** sectie, selecteer **voorwaarde toevoegen**.

6. Selecteer een metrische waarde uit de lijst met signalen om te worden gewaarschuwd op. Selecteer "Opslag procent" in dit voorbeeld.
   
   ![Metrische waarde selecteren](./media/howto-alert-metric/6-configure-signal-logic.png)

7. Configureer de waarschuwingslogica, waaronder de **voorwaarde** (ex.) 'Meer dan'), **drempelwaarde** (ex.) 85 procent), **tijdverzameling**, **periode** van tijd de regel voor metrische gegevens moet worden voldaan voordat de waarschuwing triggers (ex.) "In de afgelopen 30 minuten"), en **frequentie**.
   
   Selecteer **gedaan** wanneer u klaar bent.

   ![Metrische waarde selecteren](./media/howto-alert-metric/7-set-threshold-time.png)

8. Binnen de **actiegroepen** sectie, selecteer **nieuw** om een nieuwe groep voor het ontvangen van meldingen op de waarschuwing te maken.

9. Vul het formulier 'Actiegroep toevoegen' met een naam, de korte naam, het abonnement en de resourcegroep.

10. Configureer een **e-mailadres/SMS/Push/stem** actietype.
    
    Kies 'E-mailbericht Azure Resource Manager rol' te selecteren van Abonnementseigenaren, bijdragers en lezers om meldingen te ontvangen.
   
    (Optioneel) Geef een geldige URI in de **Webhook** veld als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.

    Selecteer **OK** wanneer dit is voltooid.

    ![Actiegroep](./media/howto-alert-metric/10-action-group-type.png)

11. Geef een naam, beschrijving en ernst.

    ![Actiegroep](./media/howto-alert-metric/11-name-description-severity.png) 

12. Selecteer **waarschuwingsregel maken** om de waarschuwing te maken.

    Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="manage-your-alerts"></a>De waarschuwingen beheren
Als u een waarschuwing hebt gemaakt, kunt u deze selecteren en de volgende acties uitvoeren:

* Een grafiek weer met de drempelwaarde voor metrische gegevens en de werkelijke waarden van de vorige dag relevant zijn voor deze waarschuwing weergeven.
* **Bewerken** of **verwijderen** de waarschuwingsregel.
* **Uitschakelen** of **inschakelen** de waarschuwing, als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [configureren van webhooks in waarschuwingen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Krijgen een [overzicht van metrische gegevens verzameling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
