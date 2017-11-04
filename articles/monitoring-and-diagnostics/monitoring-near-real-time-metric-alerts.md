---
title: Near-realtime metrische waarschuwingen in Azure Monitor | Microsoft Docs
description: Bijna realtime metriek kunnen waarschuwingen u Azure-resource metrische gegevens zo vaak als 1 min. controleren.
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: aeeb6c2fb87e6c19991ef243ee7230f4e8f4e251
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Bijna realtime metrische waarschuwingen (Preview)
Monitor voor Azure ondersteunt nu een nieuw type metrische waarschuwingen bijna Real-Time metriek waarschuwingen (Preview) genoemd. Deze functie is momenteel in de openbare preview.
Deze waarschuwingen verschillen van reguliere metrische waarschuwingen in enkele manieren

- **Verbeterde latentie** -bijna realtime metriek waarschuwingen kunnen bewaken wijzigingen in metrische waarden zo snel 1 min.
- **Meer controle over metrische voorwaarden** -bijna realtime metriek waarschuwingen toestaan dat gebruikers uitgebreidere waarschuwingsregels definiëren. De waarschuwingen bieden nu ondersteuning voor het bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens.
- **Bewaking van meerdere metrische gegevens gecombineerd** -bijna realtime metriek waarschuwingen kunnen meerdere metrische gegevens controleren (momenteel twee) met een enkele regel. Waarschuwing wordt geactiveerd als zowel de metrische gegevens die in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
- **Modulaire waarschuwingssysteem** - bijna realtime metrische waarschuwingen gebruik [actiegroepen](monitoring-action-groups.md). Deze functionaliteit kan gebruikers acties in een modulaire manier maken en deze voor veel waarschuwingsregels opnieuw te gebruiken.

> [!NOTE]
> Bijna realtime metrische waarschuwingen is functie momenteel in de openbare preview. De functionaliteit en de gebruikerservaring kan worden gewijzigd.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Welke bronnen kan ik bijna realtime metrische waarschuwingen voor maken?
Volledige lijst met brontypen die worden ondersteund door bijna realtime metrische waarschuwingen:

* Microsoft.ApiManagement/service
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Sql/servers/elasticpools
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.Timeseriesinsights
* Microsoft.CognitiveServices/accounts


## <a name="create-a-near-real-time-metric-alert"></a>Een waarschuwing voor Near realtime metrische maken
Op dit moment is bijna realtime metriek kunnen waarschuwingen alleen worden gemaakt via de Azure portal. Ondersteuning voor het configureren van bijna realtime metrische waarschuwingen via PowerShell-opdrachtregelinterface (CLI) en Azure Monitor REST-API is binnenkort beschikbaar.

1. In de [portal](https://portal.azure.com/), zoekt de resource die u geïnteresseerd bent in de bewaking en selecteert u deze. Deze bron moet zijn van een van de volgende resourcetypen die worden vermeld in de [vorige sectie](#what-resources-can-i-create-near-real-time-metric-alerts-for). U kunt ook Doe hetzelfde voor alle ondersteunde bronnen typen centraal van de Monitor > waarschuwingen.

2. Selecteer **waarschuwingen** of **waarschuwing regels** onder de sectie bewaking. De tekst en het pictogram mogelijk enigszins afwijken voor verschillende bronnen.
   ![Bewaking](./media/insights-alerts-portal/AlertRulesButton.png)

3. Klik op de **toevoegen bijna realtime metrische gegevens waarschuwing (preview)** opdracht. Als de opdracht lichter gekleurd weergegeven, controleert u dat de bron wordt geselecteerd in het filter.

    ![In de buurt van de waarschuwing knop realtime metrische gegevens toevoegen](./media/monitoring-near-real-time-metric-alerts/AddNRTAlertButton.png)

4. **Naam** uw Waarschuwing regel en kies een **beschrijving**, die ook weergegeven in e-mailmeldingen.
5. Selecteer de **metriek** u wilt bewaken, en kies vervolgens een **voorwaarde**, **Tijdaggregatie**, en **drempelwaarde** waarde voor de metriek. Selecteer desgewenst een ander **metriek** u wilt bewaken, en kies vervolgens een **voorwaarde**, **Tijdaggregatie**, en **drempelwaarde** waarde voor de tweede metriek. 

    ![Voeg bijna realtime metrische gegevens Alert1](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert1.png) ![bijna realtime metrische gegevens Alert2 toevoegen](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert2.png)
6. Kies de **periode** tijdsperiode waarin de meetwaarde regels moeten worden voldaan voordat de waarschuwing triggers. Dus bijvoorbeeld, als u de periode 'in de afgelopen 5 minuten' en de waarschuwing ziet er voor CPU hierboven 80% (en NetworkIn meer dan 500 MB), de waarschuwing wordt geactiveerd wanneer de CPU is al consistent bovenstaande 80% 5 minuten. Zodra de eerste trigger is plaatsvindt, deze opnieuw wordt geactiveerd wanneer de CPU onder 80% gedurende vijf minuten blijft. De waarschuwing wordt beoordeeld volgens de **evaluatie frequentie**


6. Kies een passende **ernst** in de vervolgkeuzelijst.

7. Geef als u wilt gebruiken een nieuw of bestaand **actie groep**.

8. Als u wilt maken **nieuw** actie groep, de actiegroep een naam en een korte naam geven, geeft u acties (Webhook SMS, e-mailadres) en vult u de details van de respectieve.


8. Selecteer **OK** wanneer u klaar bent voor het maken van de waarschuwing.   

Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="managing-near-real-time-metric-alerts"></a>Bijna realtime metrische waarschuwingen beheren
Wanneer u een waarschuwing hebt gemaakt, kunt u deze selecteren en:

* Een grafiek weer met de metrische drempel en de werkelijke waarden van de vorige dag weergeven.
* Bewerk of verwijder deze.
* **Schakel** of **inschakelen** deze als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor deze waarschuwing.



