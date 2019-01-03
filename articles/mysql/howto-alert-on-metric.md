---
title: Configureren van waarschuwingen voor metrische gegevens voor Azure Database voor MySQL in Azure portal
description: Dit artikel wordt beschreven hoe u configureert en toegang tot metrische waarschuwingen voor Azure Database voor MySQL in Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 999b1d03ad8cb0b27de10ff6457c0e6cc9112ee7
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548728"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>De Azure portal gebruiken voor het instellen van waarschuwingen over metrische gegevens voor Azure Database for MySQL 

Dit artikel leest u hoe het instellen van Azure Database voor MySQL-waarschuwingen via Azure portal. U kunt een waarschuwing op basis van metrische gegevens voor uw Azure-services controleren ontvangen.

De waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metrische gegevens van een drempel die u toewijst overschrijden. De waarschuwing triggers beide wanneer de voorwaarde wordt voldaan en klik daarna als die voorwaarde is niet meer wordt voldaan aan. 

Een waarschuwing als u wilt de volgende acties uitvoeren wanneer deze wordt geactiveerd, kunt u configureren:
* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar extra e-mailadressen die u opgeeft.
* Een webhook aanroepen

U kunt configureren en informatie over met behulp van regels voor waarschuwingen:
* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [Opdrachtregelinterface (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Een waarschuwingsregel maken op een metrische waarde van de Azure-portal
1. In de [Azure-portal](https://portal.azure.com/), selecteert u de Azure Database for MySQL-server die u wilt bewaken.

2. Onder de **bewaking** sectie van de zijbalk Selecteer **waarschuwingsregels** zoals wordt weergegeven:

   ![Selecteer waarschuwingsregels](./media/howto-alert-on-metric/1-alert-rules.png)

3. Selecteer **metrische waarschuwing toevoegen** (+ pictogram). 

4. De **regel toevoegen** pagina wordt geopend, zoals hieronder weergegeven.  Vul de vereiste gegevens:

   ![Metrische waarschuwing toevoegen](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Instelling | Description  |
   |---------|---------|
   | Name | Geef een naam voor de waarschuwingsregel. Deze waarde wordt verzonden in de e-mailmelding voor waarschuwingen. |
   | Description | Geef een korte beschrijving van de waarschuwingsregel. Deze waarde wordt verzonden in de e-mailmelding voor waarschuwingen. |
   | Waarschuwen bij | Kies **metrische gegevens** voor dit type waarschuwing. |
   | Abonnement | Dit veld wordt vooraf ingevuld met het abonnement dat als host fungeert voor uw Azure Database voor MySQL. |
   | Resourcegroep | Dit veld wordt vooraf ingevuld met de resourcegroep van uw Azure Database voor MySQL. |
   | Resource | Dit veld wordt vooraf ingevuld met de naam van uw Azure Database voor MySQL. |
   | Gegevens | Selecteer de metrische gegevens die u wilt een waarschuwing voor uitgeven. Bijvoorbeeld, **opslagpercentage**. |
   | Voorwaarde | Kies de voorwaarde voor de metrische gegevens moet worden vergeleken met. Bijvoorbeeld, **groter is dan**. |
   | Drempelwaarde | Drempelwaarde voor de meetwaarde, bijvoorbeeld 85 (procent). |
   | Periode | De periode dat de regel voor metrische gegevens moet worden voldaan voordat de waarschuwing triggers. Bijvoorbeeld, **in de afgelopen 30 minuten**. |

   Op basis van het voorbeeld, de waarschuwing zoekt naar opslagpercentage dan 85% gedurende een periode van 30 minuten. Deze waarschuwing wordt geactiveerd wanneer het gemiddelde percentage van de opslag gedurende 30 minuten meer dan 85% is geweest. Nadat de eerste trigger is die zich voordoet, activeert het opnieuw wanneer het gemiddelde percentage van de opslag is lager dan 85% meer dan 30 minuten.

5. Kies de meldingsmethode die u wilt gebruiken voor de waarschuwingsregel. 

   Controleer **e-eigenaren, bijdragers en lezers** optie als u wilt de abonnementsbeheerders en CO-beheerder moet worden verzonden wanneer de waarschuwing wordt geactiveerd.

   Als u aanvullende e-mailberichten wilt die een melding ontvangen wanneer de waarschuwing wordt geactiveerd, voegt u ze op in de **beheerder email(s)** veld. Meerdere e-mailberichten gescheiden door puntkomma's -  *email@contoso.com;email2@contoso.com*

   (Optioneel) Geef een geldige URI in de **Webhook** veld als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.

6. Selecteer **OK** om de waarschuwing te maken.

   Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="manage-your-alerts"></a>De waarschuwingen beheren
Als u een waarschuwing hebt gemaakt, kunt u deze selecteren en de volgende acties uitvoeren:

* Een grafiek weer met de drempelwaarde voor metrische gegevens en de werkelijke waarden van de vorige dag relevant zijn voor deze waarschuwing weergeven.
* **Bewerken** of **verwijderen** de waarschuwingsregel.
* **Uitschakelen** of **inschakelen** de waarschuwing, als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [configureren van webhooks in waarschuwingen](../azure-monitor/platform/alerts-webhooks.md).
* Krijgen een [overzicht van metrische gegevens verzameling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
