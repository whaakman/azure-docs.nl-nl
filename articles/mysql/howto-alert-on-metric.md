---
title: Waarschuwingen van de metrische gegevens voor Azure-Database configureren voor MySQL in Azure-portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u configureert en toegang tot metrische waarschuwingen voor Azure-Database voor MySQL vanuit de Azure-portal.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: 5b4263e76f89ae27b9c9f317db3038def01e3e6b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>De Azure portal gebruiken om in te stellen waarschuwingen op metrische gegevens voor Azure-Database voor MySQL 

In dit artikel laat zien hoe Azure-Database instellen voor MySQL waarschuwingen met de Azure portal. U kunt een waarschuwing op basis van de bewaking van metrische gegevens voor uw Azure-services kunt ontvangen.

De waarschuwing activeert wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u toewijst. De waarschuwing triggers zowel wanneer het eerst de voorwaarde is voldaan en vervolgens later wanneer die voorwaarde wordt niet langer wordt voldaan. 

U kunt de volgende acties uitvoeren als er wordt een waarschuwing configureren:
* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar andere e-mailberichten die u opgeeft.
* een webhook aanroepen

U kunt configureren en informatie krijgen over de regels voor waarschuwingen met behulp van:
* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Opdrachtregelinterface (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Monitor voor Azure REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Een waarschuwingsregel maken op een waarde van de Azure-portal
1. In de [Azure-portal](https://portal.azure.com/), selecteert u de Azure-Database voor de MySQL-server die u wilt bewaken.

2. Onder de **bewaking** sectie van de zijbalk, selecteer **waarschuwing regels** zoals wordt weergegeven:

   ![Selecteer een waarschuwing regels](./media/howto-alert-on-metric/1-alert-rules.png)

3. Selecteer **metrische waarschuwing toevoegen** (+ pictogram). 

4. De **regel toevoegen** pagina wordt geopend, zoals hieronder wordt weergegeven.  Vul de vereiste gegevens in:

   ![Metrische waarschuwing formulier toevoegen](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Instelling | Beschrijving  |
   |---------|---------|
   | Naam | Geef een naam voor de waarschuwingsregel. Deze waarde wordt verzonden in de e-mailmelding voor waarschuwingen. |
   | Beschrijving | Geef een korte beschrijving van de waarschuwingsregel. Deze waarde wordt verzonden in de e-mailmelding voor waarschuwingen. |
   | Waarschuwing bij | Kies **metrische gegevens** voor dit type waarschuwing. |
   | Abonnement | Dit veld is vooraf ingevuld met het abonnement dat als host fungeert voor uw Azure-Database voor MySQL. |
   | Resourcegroep | Dit veld is vooraf ingevuld met de resourcegroep van uw Azure-Database voor MySQL. |
   | Resource | Dit veld is vooraf ingevuld met de naam van uw Azure-Database voor MySQL. |
   | Gegevens | Selecteer de metriek die u wilt een waarschuwing voor het uitgeven. Bijvoorbeeld: **opslagpercentage**. |
   | Voorwaarde | Kies de voorwaarde voor de meetwaarde moet worden vergeleken met. Bijvoorbeeld: **groter is dan**. |
   | Drempelwaarde | Drempelwaarde voor de metriek, bijvoorbeeld 85 (procent). |
   | Periode | De periode die de metrische regel moet worden voldaan voordat de waarschuwing triggers. Bijvoorbeeld: **in de afgelopen 30 minuten**. |

   Op basis van het voorbeeld, de waarschuwing wordt gezocht naar opslagpercentage dan 85% gedurende een periode van 30 minuten. Deze waarschuwing wordt geactiveerd wanneer het gemiddelde percentage van de opslag meer dan 85% gedurende 30 minuten is. Zodra de eerste trigger is plaatsvindt, activeert het opnieuw wanneer het gemiddelde percentage van de opslag is lager dan 85% meer dan 30 minuten.

5. Kies de meldingsmethode die u voor de waarschuwingsregel wilt instellen. 

   Controleer **e-eigenaren, bijdragers en lezers** optie als u de abonnement-administrators en medebeheerders wilt ontvangen wanneer de waarschuwing wordt geactiveerd.

   Als u aanvullende e-mailberichten een melding ontvangen wilt wanneer de waarschuwing wordt geactiveerd, deze toevoegen in de **aanvullende beheerder email(s)** veld. Scheid meerdere e-mailberichten met puntkomma's -  *email@contoso.com;email2@contoso.com*

   Geef desgewenst een geldige URI in de **Webhook** als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.

6. Selecteer **OK** de waarschuwing wilt maken.

   Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="manage-your-alerts"></a>Waarschuwingen beheren
Nadat u een waarschuwing hebt gemaakt, kunt u deze selecteren en de volgende acties uitvoeren:

* Een grafiek weer met de metrische drempel en de werkelijke waarden van de vorige dag relevant zijn voor deze waarschuwing weergeven.
* **Bewerken** of **verwijderen** de waarschuwingsregel.
* **Schakel** of **inschakelen** de waarschuwing, als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [configureren van webhooks in waarschuwingen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Ophalen van een [overzicht van metrische gegevens verzameling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
