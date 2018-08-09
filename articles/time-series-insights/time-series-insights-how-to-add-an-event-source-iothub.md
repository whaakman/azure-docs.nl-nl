---
title: Een IoT Hub-gebeurtenisbron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een gebeurtenisbron die is verbonden met een IoT-Hub aan uw Time Series Insights-omgeving toevoegen
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: b6beecbf64cee925f62ac4c82919926fcb79940a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627369"
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Het toevoegen van een IoT Hub-gebeurtenisbron voor Time Series Insights-omgeving
In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het toevoegen van een gebeurtenisbron die gegevens uit een IoT-Hub in uw Time Series Insights-omgeving leest.

## <a name="prerequisites"></a>Vereisten
- Maak een Time Series Insights-omgeving. Zie voor meer informatie, [een Azure Time Series Insights-omgeving maken](time-series-insights-get-started.md) 
- Een IoT Hub maken. Zie voor meer informatie over IoT-Hubs [een IoT-Hub met behulp van de Azure portal maken](../iot-hub/iot-hub-create-through-portal.md)
- De IoT-Hub moet actieve berichten gebeurtenissen worden verzonden in.
- Maak een speciale klantengroep in IoT Hub voor de Time Series Insight-omgeving te gebruiken vanuit. Elke Time Series Insights-gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebruiken van gebeurtenissen uit de dezelfde consumergroep, worden alle lezers waarschijnlijk mislukte tests ziet. Zie voor meer informatie, de [Ontwikkelaarshandleiding voor IoT Hub](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Een consumentengroep toevoegen aan uw IoT-Hub
Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure IoT Hubs. Geef een speciale klantengroep voor gebruik door deze Time Series Insights-omgeving alleen op een betrouwbare manier om gegevens te lezen uit uw IoT-Hub.

Een nieuwe consumergroep toevoegen aan uw IoT-Hub, de volgende stappen uit:
1. Zoek in de Azure-portal en open uw IoT-Hub.

2. Onder de **Messaging** kop, selecteer **eindpunten**. 

   ![Toevoegen van een consumentengroep](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Selecteer de **gebeurtenissen** eindpunt, en de **eigenschappen** pagina wordt geopend.

4. Onder de **consumentengroepen** kop, Geef een nieuwe unieke naam voor de consumergroep. Deze dezelfde naam in Time Series Insights-omgeving gebruiken bij het maken van een nieuwe gebeurtenisbron.

5. Selecteer **opslaan** om op te slaan van de nieuwe consumergroep.

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **omgeving topologie** kop, klikt u op **gebeurtenisbronnen**.
   ![Gebeurtenis gegevensbronnen + toevoegen](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. Klik op **+ Toevoegen**.

5. Geef een **naam van bron** uniek is voor deze Time Series Insights-omgeving, zoals **gebeurtenisstroom**.

   ![De eerste drie parameters in het formulier invullen.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Selecteer de **bron** als **IoT-Hub**.

7. Selecteer de juiste **importoptie**. 
   - Kies **gebruikt IoT-Hub in de beschikbare abonnementen** wanneer u al een IoT Hub hebt op een van uw abonnementen. Dit is de eenvoudigste methode.
   - Kies **bieden IoT Hub-instellingen handmatig** wanneer de IoT-Hub bevindt zich buiten uw abonnementen, of u wilt om geavanceerde opties te kiezen. 

8. Als u hebt geselecteerd de **gebruikt IoT-Hub in de beschikbare abonnementen** optie, de volgende tabel wordt uitgelegd dat de vereiste eigenschap:

   ![Details van abonnement en de Event hub](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Selecteer het abonnement waarin deze IoT Hub is gemaakt.
   | De naam van IoT Hub | Selecteer de naam van de IoT-Hub.
   | IoT Hub-beleidsnaam | Selecteer het beleid voor gedeelde toegang, die kan worden gevonden op het tabblad voor IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **service verbinding maken met** machtigingen.
   | Sleutel voor het IoT-Hub | De sleutel wordt vooraf ingevuld.
   | IoT Hub-consumentengroep | De consumentengroep beperkt tot het lezen van gebeurtenissen van de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor uw gebeurtenisbron.
   | Serialisatie-indeling voor gebeurtenissen | JSON is de enige beschikbare serialisatie op dit moment. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar IoT Hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

9. Als u hebt geselecteerd de **bieden IoT Hub-instellingen handmatig** optie, de volgende tabel wordt uitgelegd dat de vereiste eigenschap:

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Abonnements-id | Het abonnement waarin deze IoT Hub is gemaakt.
   | Resourcegroep | Naam van de resourcegroep waarin deze IoT Hub is gemaakt.
   | De naam van IoT Hub | De naam van uw IoT-Hub. Wanneer u uw IoT-Hub gemaakt, u deze ook een specifieke naam gegeven.
   | IoT Hub-beleidsnaam | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad voor IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **service verbinding maken met** machtigingen.
   | Sleutel voor het IoT-Hub | De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
   | IoT Hub-consumentengroep | De consumentengroep beperkt tot het lezen van gebeurtenissen van de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor uw gebeurtenisbron.
   | Serialisatie-indeling voor gebeurtenissen | JSON is de enige beschikbare serialisatie op dit moment. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
   | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar IoT Hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** binnen de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

10. Voeg de toegewezen TSI consument groepsnaam die u hebt toegevoegd aan uw IoT-Hub.

11. Selecteer **maken** om toe te voegen van de bron van de nieuwe gebeurtenis.

   ![Klik op Maken](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Wanneer de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch met het streamen van gegevens naar uw omgeving.

## <a name="next-steps"></a>Volgende stappen
- [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) om de gegevens te beveiligen.
- [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
- Toegang tot uw omgeving in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
