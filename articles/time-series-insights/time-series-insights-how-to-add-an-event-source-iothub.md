---
title: Het toevoegen van een gebeurtenisbron IoT-Hub aan Azure Time Series Insights | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een gebeurtenisbron die is verbonden met een IoT-Hub aan uw omgeving Time Series Insights toevoegt
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
ms.openlocfilehash: 0469c35056d1d02457c162b8540af472b84f1e92
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Het toevoegen van een gebeurtenisbron IoT-Hub aan Time Series Insights-omgeving
Dit artikel wordt beschreven hoe u met de Azure portal een gebeurtenisbron die gegevens uit een IoT-Hub in uw omgeving Time Series Insights leest toevoegen.

## <a name="prerequisites"></a>Vereisten
- Maakt een tijd reeks Insights-omgeving. Zie voor meer informatie [een Azure Time Series Insights-omgeving maken](time-series-insights-get-started.md) 
- Een IoT Hub maken. Zie voor meer informatie over IoT Hubs [een IoT Hub met de Azure portal maken](../iot-hub/iot-hub-create-through-portal.md)
- De IoT-Hub moet actieve bericht gebeurtenissen worden verzonden in hebben.
- Maak een speciale klantengroep in IoT Hub voor de omgeving Time Series inzicht gebruiken uit. Elke keer reeks Insights gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebeurtenissen van de dezelfde consumergroep gebruiken, zijn alle lezers waarschijnlijk fouten. Zie voor meer informatie de [Ontwikkelaarshandleiding voor IoT Hub](../iot-hub/iot-hub-devguide.md).

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **omgeving topologie** kop, klikt u op **gebeurtenisbronnen**.
   ![Gebeurtenis gegevensbronnen + toevoegen](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. Klik op **+ toevoegen**.

5. Geef een **gebeurtenisnaam bron** uniek is voor deze Time Series Insights-omgeving, zoals **stroom gebeurtenissen**.

   ![Vul in de eerste drie parameters in de vorm.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Selecteer de **bron** als **IoT-Hub**.

7. Selecteer de relevante **optie importeren**. 
   - Kies **gebruik IoT Hub uit de beschikbare abonnementen** wanneer u al een IoT Hub hebt op een van uw abonnementen. Dit is de beste aanpak.
   - Kies **bieden IoT Hub-instellingen handmatig** wanneer de IoT-Hub is extern voor uw abonnementen, of u wilt geavanceerde opties te kiezen. 

8. Als u hebt geselecteerd de **gebruik IoT Hub uit de beschikbare abonnementen** optie, de volgende tabel wordt uitgelegd dat alle vereiste eigenschappen:

   ![Details van abonnement en Event hub](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Selecteer het abonnement waarin deze IoT Hub is gemaakt.
   | Naam van de IoT Hub | Selecteer de naam van de IoT-Hub.
   | Naam van de IoT Hub-beleid | Selecteer het beleid voor gedeelde toegang, die kan worden gevonden op het tabblad IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **service verbinding** machtigingen.
   | IoT Hub beleidssleutel | De sleutel is vooraf ingevuld.
   | IoT Hub klantengroep | De consumergroep gebeurtenissen moeten worden gelezen uit de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.
   | Gebeurtenis serialisatie-indeling | JSON is de enige beschikbare serialisatie op dit moment. De event-berichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de timestamp-eigenschap | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens verzonden naar IoT Hub. Deze waarde is de **naam** van de specifieke gebeurtenis-eigenschap in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als er niets wordt opgegeven, de **gebeurtenistijd in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

9. Als u hebt geselecteerd de **bieden IoT Hub-instellingen handmatig** optie, de volgende tabel wordt uitgelegd dat alle vereiste eigenschappen:

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Het abonnement waarin deze IoT Hub is gemaakt.
   | Resourcegroep | Naam van de resourcegroep waarin deze IoT Hub is gemaakt.
   | Naam van de IoT Hub | De naam van uw IoT-Hub. Wanneer u uw IoT-Hub gemaakt, u dit ook een specifieke naam gegeven.
   | Naam van de IoT Hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **service verbinding** machtigingen.
   | IoT Hub beleidssleutel | De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
   | IoT Hub klantengroep | De consumergroep gebeurtenissen moeten worden gelezen uit de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.
   | Gebeurtenis serialisatie-indeling | JSON is de enige beschikbare serialisatie op dit moment. De event-berichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de timestamp-eigenschap | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens verzonden naar IoT Hub. Deze waarde is de **naam** van de specifieke gebeurtenis-eigenschap in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als er niets wordt opgegeven, de **gebeurtenistijd in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

10. Selecteer **maken** om toe te voegen van de nieuwe gebeurtenisbron.

   ![Klik op Maken](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Wanneer de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch met het streamen van gegevens naar uw omgeving.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Een consumergroep toevoegen aan uw IoT-Hub
Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure IoT Hubs. Geef een speciale klantengroep voor gebruik door deze Time Series Insights omgeving alleen betrouwbaar gegevens lezen uit uw IoT-Hub.

Als u wilt een nieuwe consumergroep toevoegen aan uw IoT-Hub, de volgende stappen uit:
1. Zoek in de Azure-portal en open uw IoT-Hub.

2. Onder de **Messaging** kop, selecteer **eindpunten**. 

   ![Een consumergroep toevoegen](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Selecteer de **gebeurtenissen** eindpunt, en de **eigenschappen** pagina wordt geopend.

4. Onder de **consumergroepen** kop Geef een nieuwe unieke naam voor de consumergroep. Deze dezelfde naam in tijd reeks Insights omgeving gebruiken bij het maken van een nieuwe gebeurtenisbron.

5. Selecteer **opslaan** om op te slaan van de nieuwe consumergroep.

## <a name="next-steps"></a>Volgende stappen
- [Definieer gegevenstoegangsbeleid](time-series-insights-data-access.md) om de gegevens te beveiligen.
- [Verzenden van gebeurtenissen](time-series-insights-send-events.md) aan de gebeurtenisbron-.
- Toegang tot uw omgeving in de [Time Series Insights explorer](https://insights.timeseries.azure.com).
