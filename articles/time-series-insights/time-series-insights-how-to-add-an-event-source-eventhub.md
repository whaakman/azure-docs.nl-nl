---
title: Een Event Hubs-gebeurtenisbron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een gebeurtenisbron die verbonden met Azure Event Hubs op uw Time Series Insights-omgeving toevoegen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: ce76a9dcd8b717b6ab0aef4c3c12c2116ec4f237
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104693"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Een event hub-gebeurtenisbron toevoegen aan uw Time Series Insights-omgeving

In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het toevoegen van een gebeurtenisbron die gegevens uit Azure Event Hubs op uw Azure Time Series Insights-omgeving leest.

> [!NOTE]
> De stappen in dit artikel beschreven gelden zowel de Time Series Insights algemeen beschikbaar en de Time Series Insights Preview omgevingen.

## <a name="prerequisites"></a>Vereisten

- Maak een Time Series Insights-omgeving. Zie voor meer informatie, [maken van een Azure Time Series Insights-omgeving](./time-series-insights-update-create-environment.md).
- Maak een Event Hub. Zie voor meer informatie over Event Hubs [een Event Hubs-naamruimte en een event hub maken met behulp van de Azure-portal](../event-hubs/event-hubs-create.md).
- De event hub moet actieve berichtgebeurtenissen verzonden naar deze hebben. Zie voor meer informatie, [gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Maak een speciale klantengroep in de event hub die de Time Series Insights-omgeving kan verbruiken. Elke Time Series Insights-gebeurtenisbron beschikken over een eigen speciale klantengroep die niet worden gedeeld met alle andere consumenten. Als meerdere lezers gebruiken van gebeurtenissen uit de dezelfde consumergroep, worden alle lezers waarschijnlijk mislukte tests ziet. Er is een limiet van 20 consumergroepen per event hub. Zie voor meer informatie, de [Programmeerhandleiding voor Event Hubs](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Een consumentengroep toevoegen aan uw event hub

Toepassingen gebruiken consumentengroepen gegevens wilt halen uit Azure Event Hubs. Geef een speciale klantengroep voor gebruik door deze Time Series Insights-omgeving, op een betrouwbare manier om gegevens te lezen uit uw event hub.

Om toe te voegen een nieuwe consumentengroep in uw event hub:

1. Zoek in de Azure-portal en open uw event hub.

1. Onder **entiteiten**, selecteer **consumentengroepen**, en selecteer vervolgens **Consumergroep**.

   ![Event hub - toevoegen van een consumentengroep](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

1. Op de **consumentengroepen** pagina, voert u een nieuwe unieke waarde voor **naam**.  Gebruik deze dezelfde naam als u een nieuwe gebeurtenisbron in de Time Series Insights-omgeving maken.

1. Selecteer **Maken**.

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Ga naar uw bestaande Time Series Insights-omgeving. Selecteer in het menu links **alle resources**, en selecteer vervolgens uw Time Series Insights-omgeving.

1. Onder **omgeving topologie**, selecteer **gebeurtenisbronnen**, en selecteer vervolgens **toevoegen**.

   ![Bronnen van gebeurtenissen, selecteer de knop toevoegen](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

1. Voer een waarde voor **naam van bron** die uniek is voor deze Time Series Insights-omgeving, zoals **gebeurtenisstroom**.

1. Voor **bron**, selecteer **Event Hub**.

1. Selecteer de juiste waarden voor **importoptie**:
   - Als u een bestaande event hub in een van uw abonnementen hebt, selecteert u **gebruik Event Hub uit de beschikbare abonnementen**. Deze optie is de eenvoudigste oplossing.
   - Als de event hub bevindt zich buiten uw abonnementen of als u wilt geavanceerde opties selecteert, selecteert u **bieden Event Hub-instellingen handmatig**.

   ![Voer in het deelvenster nieuwe gebeurtenis bron waarden voor de eerste drie parameters](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

1. De volgende tabel beschrijft de vereiste eigenschappen voor de **gebruik Event Hub uit de beschikbare abonnementen** optie:

   ![Details van abonnement en de event hub](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Eigenschap | Description |
   | --- | --- |
   | Abonnements-id | Selecteer het abonnement waarin deze event hub is gemaakt.
   | Service Bus-naamruimte | Selecteer de Azure Service Bus-naamruimte met de event hub.
   | Naam van Event Hub | Selecteer de naam van de event hub.
   | De naam van een Event hub-beleid | Selecteer het beleid voor gedeelde toegang. U kunt het beleid voor gedeelde toegang maken in de event hub **configureren** tabblad. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Beleidssleutel voor Event hub | Waarde van de sleutel mogelijk vooraf worden ingevuld.
   | Event hub-consumentengroep | De consumentengroep die gebeurtenissen uit de event hub kan lezen. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken. |
   | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. Gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar de event hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

1. De volgende tabel beschrijft de vereiste eigenschappen voor de **bieden Event Hub-instellingen handmatig** optie:

   | Eigenschap | Description |
   | --- | --- |
   | Abonnements-id | Het abonnement waarin deze event hub is gemaakt.
   | Resourcegroep | De resourcegroep waarin deze event hub is gemaakt.
   | Service Bus-naamruimte | Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe event hub hebt gemaakt, is ook een Service Bus-naamruimte gemaakt.
   | Naam van Event Hub | De naam van uw event hub. Wanneer u uw event hub hebt gemaakt, u deze ook een specifieke naam gegeven.
   | De naam van een Event hub-beleid | Het beleid voor gedeelde toegang. U kunt een gedeeld toegangsbeleid maken in de event hub **configureren** tabblad. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **lezen** machtigingen.
   | Beleidssleutel voor Event hub | De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
   | Event hub-consumentengroep | De consumentengroep die gebeurtenissen uit de event hub kan lezen. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken.
   | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. Gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar de event hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

1. De toegewezen Time Series Insights consument groepsnaam die u hebt toegevoegd aan uw event hub toevoegen.

1. Selecteer **Maken**.

   ![Selecteer Maken](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Nadat de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch streaming-gegevens aan uw omgeving.

## <a name="next-steps"></a>Volgende stappen

* [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) om de gegevens te beveiligen.
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
* Toegang tot uw omgeving in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
