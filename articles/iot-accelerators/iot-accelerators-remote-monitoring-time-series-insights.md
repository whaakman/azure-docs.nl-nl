---
title: Externe bewaking telemetrie met Azure Time Series Insights visualiseren | Microsoft Docs
description: Informatie over het configureren van uw Time Series Insights-omgeving om te verkennen en analyseren van telemetrie die is gegenereerd door de oplossingsverbetering voor externe controle.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 02b3afbc62be7a7a9c396de888378d762405248a
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383322"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-remote-monitoring-solution-accelerator"></a>Time Series Insights gebruiken om te visualiseren van telemetrie verzonden vanaf de oplossingsverbetering voor externe controle

Als operator kunt u de mogelijkheden van de visualisatie out-of-the-box-gegevens van de oplossingsverbetering voor externe controle uitbreiden. In deze gebruiksaanwijzing wordt beschreven hoe u met een Time Series Insights-omgeving kunt visualiseren en analyseren van telemetrie verzonden naar de oplossingsverbetering voor externe controle.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in deze handleiding, moet u een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

De stappen in deze handleiding wordt ervan uitgegaan dat u hebt de oplossingsverbetering voor externe controle geïmplementeerd in uw Azure-abonnement. Als u de oplossingsversneller nog niet hebt geïmplementeerd, volgt u de stappen in de [implementeren en uitvoeren van een cloud-gebaseerde oplossing voor externe controle](quickstart-remote-monitoring-deploy.md) Quick Start.

In dit artikel wordt ervan uitgegaan dat de naam van uw oplossingenaccelerator is **contoso-simulatie**. Vervang **contoso-simulatie** met de naam van uw oplossingenaccelerator als u de volgende stappen uitvoeren.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="time-series-insights-explorer"></a>Verkenner van Time Series Insights

De Verkenner van Time Series Insights is een web-app die u gebruiken kunt voor het visualiseren van uw telemetrie.

1. Selecteer in de Azure-portal, de Time Series Insights **overzicht** tabblad.

1. Als u wilt de Time Series Insights explorer web-app openen, klikt u op **gaat u naar de omgeving**:

    ![Verkenner van Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-environment.png)

1. Selecteer in het deelvenster tijd selecteren **afgelopen 30 minuten** van de snelle tijden en klik op **zoeken**:

    ![Time Series Insights explorer zoeken](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-search-time.png)

1. Selecteer in het deelvenster voorwaarden aan de linkerkant **temperatuur** als de **meting** en **iothub-verbinding-apparaat-id** als de **splitsen door** waarde:

    ![Time Series Insights explorer query](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query1.png)

1. Met de rechtermuisknop op de grafiek en selecteer **gebeurtenissen onderzoeken**:

    ![Gebeurtenissen van Time Series Insights explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explore-events.png)

1. In een raster ziet u de gebeurtenisgegevens:

    ![Time Series Insights explorer tabel](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-table.png)

1. Klik op de knop perspectief weergeven:

    ![Time Series Insights explorer perspectief](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klik op **+** een nieuwe query toevoegen aan het perspectief:

    ![Verkenner van Time Series Insights Query toevoegen](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-new-query.png)

1. Selecteer **afgelopen 30 minuten** als de tijdsspanne **vochtigheid** als de **meting**, en **iothub-verbinding-apparaat-id** als de **Splitsen door** waarde:

    ![Time Series Insights explorer query](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query2.png)

1. Klik op de knop perspectief weergave om uw apparaat telemetrie-dashboard weer te geven:

    ![Time Series Insights explorer-dashboard](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verkennen en het opvragen van gegevens in de Verkenner van Time Series Insights, [Azure Time Series Insights explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
