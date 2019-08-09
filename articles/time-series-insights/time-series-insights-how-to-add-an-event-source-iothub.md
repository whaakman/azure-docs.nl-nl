---
title: Een IoT hub-gebeurtenis bron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een gebeurtenis bron die is verbonden met een IoT-hub kunt toevoegen aan uw Time Series Insights-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 234fe28cb8bd9dcb97e307836961e2f587e15181
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846629"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>Een IoT hub-gebeurtenis bron toevoegen aan uw Time Series Insights omgeving

In dit artikel wordt beschreven hoe u de Azure Portal kunt gebruiken om een gebeurtenis bron toe te voegen waarmee gegevens worden gelezen van Azure IoT Hub naar uw Azure Time Series Insights omgeving.

> [!NOTE]
> De instructies in dit artikel zijn van toepassing op Azure Time Series Insights GA en aan Time Series Insights-voorbeeld omgevingen.

## <a name="prerequisites"></a>Vereisten

* Een [Azure time series Insights omgeving](time-series-insights-update-create-environment.md)maken.
* Maak een [IOT-hub met behulp van de Azure Portal](../iot-hub/iot-hub-create-through-portal.md).
* Voor de IoT-hub moeten actieve bericht gebeurtenissen worden verzonden in.
* Maak een speciale Consumer groep in de IoT-hub voor de Time Series Insights omgeving van waaruit kan worden verbruikt. Elke Time Series Insights-gebeurtenisbron beschikken over een eigen speciale klantengroep die niet worden gedeeld met alle andere consumenten. Als meerdere lezers gebruiken van gebeurtenissen uit de dezelfde consumergroep, worden alle lezers waarschijnlijk mislukte tests ziet. Zie de [ontwikkelaars handleiding voor Azure IOT hub](../iot-hub/iot-hub-devguide.md)voor meer informatie.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Een Consumer groep toevoegen aan uw IoT-hub

Toepassingen gebruiken consumenten groepen voor het ophalen van gegevens uit Azure IoT Hub. Als u gegevens op een betrouw bare manier van uw IoT-hub wilt lezen, geeft u een speciale consumenten groep op die alleen door deze Time Series Insights omgeving wordt gebruikt.

Een nieuwe consumenten groep toevoegen aan uw IoT-hub:

1. Zoek en open uw IoT-hub in de Azure Portal.

1. Selecteer onder **instellingen**de optie **ingebouwde eind punten**en selecteer vervolgens het eind punt **gebeurtenissen** .

   [![Selecteer op de pagina eind punten samen stellen de knop gebeurtenissen](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-one.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-one.png#lightbox)

1. Voer onder **consumenten groepen**een unieke naam in voor de consumenten groep. Gebruik dezelfde naam in uw Time Series Insights omgeving wanneer u een nieuwe gebeurtenis bron maakt.

1. Selecteer **Opslaan**.

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **Alle resources** in het menu aan de linkerkant. Selecteer uw Time Series Insights-omgeving.

1. Onder **omgeving topologie**, selecteer **gebeurtenisbronnen**, en selecteer vervolgens **toevoegen**.

   [![Selecteer gebeurtenis bronnen en selecteer vervolgens de knop toevoegen](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-two.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-two.png#lightbox)

1. Voer in het deel venster **nieuwe bron van gebeurtenis** de naam van de **gebeurtenis bron**een unieke naam in voor deze time series Insights omgeving. Voer bijvoorbeeld **gebeurtenis-stream**in.

1. Selecteer **IOT hub**voor **bron**.

1. Selecteer een waarde voor de **optie importeren**:

   * Als u al een IoT-hub in een van uw abonnementen hebt, selecteert u **IOT hub gebruiken uit beschik bare abonnementen**. Deze optie is de eenvoudigste oplossing.
   
     [![Selecteer opties in het deel venster nieuwe gebeurtenis bron](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-three.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-three.png#lightbox)

    * In de volgende tabel worden de eigenschappen beschreven die vereist zijn voor de optie **IOT hub van beschik bare abonnementen gebruiken** :

       [![Nieuw deel venster gebeurtenis Bron: eigenschappen die moeten worden ingesteld in de optie IoT Hub van beschik bare abonnementen gebruiken](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-four.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-four.png#lightbox)

       | Eigenschap | Description |
       | --- | --- |
       | Abonnements-id | Selecteer het abonnement waarin de IoT-hub is gemaakt.
       | Naam van IoT Hub | Selecteer de naam van de IoT-hub.
       | Beleids naam van IoT-hub | Selecteer het beleid voor gedeelde toegang. U kunt het beleid voor gedeelde toegang vinden op het tabblad instellingen van IoT hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Voor het beleid voor gedeelde toegang voor uw gebeurtenis bron *moeten* **service Connect** -machtigingen zijn toegewezen.
       | Beleids sleutel voor IoT hub | De sleutel is vooraf ingevuld.
       | Klanten groep voor IoT hub | De Consumer groep die gebeurtenissen van de IoT-hub leest. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken.
       | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. De gebeurtenis berichten moeten de volgende indeling hebben of er kunnen geen gegevens worden gelezen. |
       | Naam van de eigenschap timestamp | Als u deze waarde wilt bepalen, moet u weten wat de bericht indeling is van de bericht gegevens die worden verzonden naar de IoT-hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

    * Als de IoT-hub zich buiten uw abonnementen bevindt, of als u geavanceerde opties wilt kiezen, selecteert u **IOT hub-instellingen hand matig opgeven**.

      In de volgende tabel worden de vereiste eigenschappen voor de **IOT hub instellingen hand matig opgeven**:

       | Eigenschap | Description |
       | --- | --- |
       | Abonnements-id | Het abonnement waarin de IoT-hub is gemaakt.
       | Resource group | De naam van de resource groep waarin de IoT-hub is gemaakt.
       | Naam van IoT Hub | De naam van uw IoT-hub. Wanneer u uw IoT-hub hebt gemaakt, hebt u een naam opgegeven voor de IoT-hub.
       | Beleids naam van IoT-hub | Het beleid voor gedeelde toegang. U kunt het beleid voor gedeelde toegang maken op het tabblad instellingen van IoT hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Voor het beleid voor gedeelde toegang voor uw gebeurtenis bron *moeten* **service Connect** -machtigingen zijn toegewezen.
       | Beleids sleutel voor IoT hub | De gedeelde toegangs sleutel die wordt gebruikt voor het verifiëren van toegang tot de naam ruimte Azure Service Bus. Voer de primaire of secundaire sleutel hier in.
       | Klanten groep voor IoT hub | De Consumer groep die gebeurtenissen van de IoT-hub leest. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken.
       | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. De gebeurtenis berichten moeten de volgende indeling hebben of er kunnen geen gegevens worden gelezen. |
       | Naam van de eigenschap timestamp | Als u deze waarde wilt bepalen, moet u weten wat de bericht indeling is van de bericht gegevens die worden verzonden naar de IoT-hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

1. Voeg de toegewezen Time Series Insights naam van de Consumer groep toe die u hebt toegevoegd aan uw IoT-hub.

1. Selecteer **Maken**.

   [![De knop maken](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-five.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub-five.png#lightbox)

1. Nadat u de gebeurtenis bron hebt gemaakt, wordt door Time Series Insights automatisch streamen van gegevens naar uw omgeving gestart.

## <a name="next-steps"></a>Volgende stappen

* [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) om de gegevens te beveiligen.

* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.

* Toegang tot uw omgeving in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
