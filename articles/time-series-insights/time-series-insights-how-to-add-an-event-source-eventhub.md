---
title: Een Event Hub gebeurtenisbron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een gebeurtenisbron die is verbonden met een Event Hub aan uw omgeving Time Series Insights toevoegt.
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/21/2017
ms.openlocfilehash: c07c847784eb13c62e350e9c655e027e7df696a3
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Het toevoegen van een Event Hub gebeurtenisbron aan Time Series Insights-omgeving

Dit artikel wordt beschreven hoe u met de Azure portal een gebeurtenisbron die gegevens uit een Event Hub in uw omgeving Time Series Insights leest toevoegen.

## <a name="prerequisites"></a>Vereisten
- Maakt een tijd reeks Insights-omgeving. Zie voor meer informatie [een Azure Time Series Insights-omgeving maken](time-series-insights-get-started.md) 
- Maak een Event Hub. Zie voor meer informatie over Event Hubs [een Event Hubs-naamruimte en een event hub met de Azure portal maken](../event-hubs/event-hubs-create.md)
- De Event Hub moet actieve bericht gebeurtenissen worden verzonden in hebben. Zie voor meer informatie [gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Een speciale klantengroep maken in de Event Hub voor de omgeving Time Series inzicht gebruiken uit. Elke keer reeks Insights gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebeurtenissen van de dezelfde consumergroep gebruiken, zijn alle lezers waarschijnlijk fouten. Houd er rekening mee dat er ook een limiet van 20 consumergroepen per Event Hub is. Zie voor meer informatie de [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md).

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **omgeving topologie** kop, klikt u op **gebeurtenisbronnen**.

   ![Gebeurtenis gegevensbronnen + toevoegen](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. Klik op **+ toevoegen**.

5. Geef een **gebeurtenisnaam bron** uniek is voor deze Time Series Insights-omgeving, zoals **stroom gebeurtenissen**.

   ![Vul in de eerste drie parameters in de vorm.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. Selecteer de **bron** als **Event Hub**.

7. Selecteer de relevante **optie importeren**. 
   - Kies **gebruik Event Hub uit de beschikbare abonnementen** wanneer u al een bestaande Event Hub hebt op een van uw abonnementen. Dit is de beste aanpak.
   - Kies **bieden Event Hub-instellingen handmatig** wanneer de Event Hub buiten uw abonnementen, of u wilt kiezen geavanceerde opties. 

8. Als u hebt geselecteerd de **gebruik Event Hub uit de beschikbare abonnementen** optie, de volgende tabel wordt uitgelegd dat alle vereiste eigenschappen:

   ![Details van abonnement en Event hub](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Selecteer het abonnement waarin deze event hub is gemaakt.
   | Service bus-naamruimte | Selecteer de Service Bus-naamruimte die de Event Hub bevat.
   | Naam Event hub | Selecteer de naam van de Event Hub.
   | Naam Event hub-beleid | Selecteer het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Event hub beleidssleutel | Waarde van de sleutel mogelijk vooraf worden ingevuld.
   | Event hub klantengroep | De consumergroep gebeurtenissen moeten worden gelezen van de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron. |
   | Gebeurtenis serialisatie-indeling | JSON is de enige beschikbare serialisatie op dit moment. De event-berichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de timestamp-eigenschap | Om te bepalen deze waarde, moet u inzicht in de berichtindeling van de berichtgegevens in Event Hub worden verzonden. Deze waarde is de **naam** van de specifieke gebeurtenis-eigenschap in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als er niets wordt opgegeven, de **gebeurtenistijd in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |


9. Als u hebt geselecteerd de **bieden Event Hub-instellingen handmatig** optie, de volgende tabel wordt uitgelegd dat alle vereiste eigenschappen:

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Het abonnement waarin deze event hub is gemaakt.
   | Resourcegroep | De resourcegroep waarin deze event hub is gemaakt.
   | Service bus-naamruimte | Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Service Bus-naamruimte gemaakt.
   | Naam Event hub | De naam van uw Event Hub. Wanneer u uw event hub hebt gemaakt, u deze ook een specifieke naam gegeven.
   | Naam Event hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Event hub beleidssleutel | De toegang tot de gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
   | Event hub klantengroep | De Consumergroep gebeurtenissen moeten worden gelezen van de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.
   | Gebeurtenis serialisatie-indeling | JSON is de enige beschikbare serialisatie op dit moment. De event-berichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de timestamp-eigenschap | Om te bepalen deze waarde, moet u inzicht in de berichtindeling van de berichtgegevens in Event Hub worden verzonden. Deze waarde is de **naam** van de specifieke gebeurtenis-eigenschap in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als er niets wordt opgegeven, de **gebeurtenistijd in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |


10. Selecteer **maken** om toe te voegen van de nieuwe gebeurtenisbron.
   
   ![Klik op Maken](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Wanneer de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch met het streamen van gegevens naar uw omgeving.


### <a name="add-a-consumer-group-to-your-event-hub"></a>Een consumergroep toevoegen aan uw Event Hub
Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure Event Hubs. Geef een speciale klantengroep voor gebruik door deze Time Series Insights omgeving alleen betrouwbare manier om gegevens te lezen van uw Event Hub.

Als u wilt een nieuwe consumergroep toevoegen aan uw Event Hub, de volgende stappen uit:
1. Zoek in de Azure-portal en open uw Event Hub.

2. Onder de **entiteiten** kop, selecteer **consumergroepen**.

   ![Event Hub - een consumergroep toevoegen](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Selecteer **+ Consumergroep** toevoegen van een nieuwe consumergroep. 

4. Op de **consumergroepen** pagina, geeft u een nieuwe unieke **naam**.  Deze dezelfde naam gebruiken bij het maken van een nieuwe gebeurtenisbron in de Time Series Insights-omgeving.

5. Selecteer **maken** voor het maken van de nieuwe consumergroep.

## <a name="next-steps"></a>Volgende stappen
- [Definieer gegevenstoegangsbeleid](time-series-insights-data-access.md) om de gegevens te beveiligen.
- [Verzenden van gebeurtenissen](time-series-insights-send-events.md) aan de gebeurtenisbron-.
- Toegang tot uw omgeving in de [Time Series Insights explorer](https://insights.timeseries.azure.com).
