---
title: Een Event Hub-gebeurtenisbron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een gebeurtenisbron die is verbonden met een Event Hub voor uw Time Series Insights-omgeving toevoegen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: ce4bf1ab74e4203f0deb7b2984ffa6a66d5efd4a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627107"
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Het toevoegen van een Event Hub-gebeurtenisbron voor Time Series Insights-omgeving

In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het toevoegen van een gebeurtenisbron die gegevens vanaf een Event Hub in uw Time Series Insights-omgeving leest.

## <a name="prerequisites"></a>Vereisten
- Maak een Time Series Insights-omgeving. Zie voor meer informatie, [een Azure Time Series Insights-omgeving maken](time-series-insights-get-started.md) 
- Een Event Hub maken. Zie voor meer informatie over Event Hubs [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure portal](../event-hubs/event-hubs-create.md)
- De Event Hub moet actieve berichten gebeurtenissen worden verzonden in. Zie voor meer informatie, [gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Maak een speciale klantengroep in Event Hub voor de Time Series Insight-omgeving te gebruiken vanuit. Elke Time Series Insights-gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebruiken van gebeurtenissen uit de dezelfde consumergroep, worden alle lezers waarschijnlijk mislukte tests ziet. Houd er rekening mee dat er ook een limiet van 20 consumergroepen per Event Hub is. Zie voor meer informatie, de [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Een consumentengroep toevoegen aan uw Event Hub
Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure Event Hubs. Geef een speciale klantengroep voor gebruik door deze Time Series Insights-omgeving alleen op een betrouwbare manier om gegevens te lezen uit uw Event Hub.

Om toe te voegen een nieuwe consumentengroep in uw Event Hub, de volgende stappen uit:
1. Zoek in de Azure-portal en open uw Event Hub.

2. Onder de **entiteiten** kop, selecteer **consumentengroepen**.

   ![Event Hub - toevoegen van een consumentengroep](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Selecteer **+ Consumergroep** om toe te voegen een nieuwe consumergroep. 

4. Op de **consumentengroepen** pagina, geeft u een nieuwe unieke **naam**.  Deze dezelfde naam gebruiken bij het maken van een nieuwe bron van gebeurtenis in de Time Series Insights-omgeving.

5. Selecteer **maken** te maken van de nieuwe consumergroep.

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **omgeving topologie** kop, klikt u op **gebeurtenisbronnen**.

   ![Gebeurtenis gegevensbronnen + toevoegen](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. Klik op **+ Toevoegen**.

5. Geef een **naam van bron** uniek is voor deze Time Series Insights-omgeving, zoals **gebeurtenisstroom**.

   ![De eerste drie parameters in het formulier invullen.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. Selecteer de **bron** als **Event Hub**.

7. Selecteer de juiste **importoptie**. 
   - Kies **gebruik Event Hub uit de beschikbare abonnementen** wanneer u al een bestaande Event Hub hebt op een van uw abonnementen. Dit is de eenvoudigste methode.
   - Kies **bieden Event Hub-instellingen handmatig** wanneer de Event Hub bevindt zich buiten uw abonnementen, of u wilt om geavanceerde opties te kiezen. 

8. Als u hebt geselecteerd de **gebruik Event Hub uit de beschikbare abonnementen** optie, de volgende tabel wordt uitgelegd dat de vereiste eigenschap:

   ![Details van abonnement en de Event hub](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Selecteer het abonnement waarin deze event hub is gemaakt.
   | Service Bus-naamruimte | Selecteer de Service Bus-naamruimte met de Event Hub.
   | Event hub-naam | Selecteer de naam van de Event Hub.
   | De naam van een Event hub-beleid | Selecteer het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad configureren van Event Hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Beleidssleutel voor Event hub | Waarde van de sleutel mogelijk vooraf worden ingevuld.
   | Event hub-consumentengroep | De consumentengroep beperkt tot het lezen van gebeurtenissen uit de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor uw gebeurtenisbron. |
   | Serialisatie-indeling voor gebeurtenissen | JSON is de enige beschikbare serialisatie op dit moment. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens verzonden naar Event Hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |


9. Als u hebt geselecteerd de **bieden Event Hub-instellingen handmatig** optie, de volgende tabel wordt uitgelegd dat de vereiste eigenschap:

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Het abonnement waarin deze event hub is gemaakt.
   | Resourcegroep | De resourcegroep waarin deze event hub is gemaakt.
   | Service Bus-naamruimte | Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, is ook een Service Bus-naamruimte gemaakt.
   | Event hub-naam | De naam van uw Event Hub. Wanneer u uw event hub hebt gemaakt, u deze ook een specifieke naam gegeven.
   | De naam van een Event hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad configureren van Event Hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Beleidssleutel voor Event hub | De gedeelde toegangssleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
   | Event hub-consumentengroep | De Consumentengroep beperkt tot het lezen van gebeurtenissen uit de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor uw gebeurtenisbron.
   | Serialisatie-indeling voor gebeurtenissen | JSON is de enige beschikbare serialisatie op dit moment. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens verzonden naar Event Hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

10. De toegewezen TSI consument groepsnaam die u hebt toegevoegd aan uw Event hub toevoegen.

11. Selecteer **maken** om toe te voegen van de bron van de nieuwe gebeurtenis.
   
   ![Klik op Maken](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Wanneer de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch met het streamen van gegevens naar uw omgeving.


## <a name="next-steps"></a>Volgende stappen
- [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) om de gegevens te beveiligen.
- [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
- Toegang tot uw omgeving in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
