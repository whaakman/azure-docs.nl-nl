---
title: Problemen vaststellen en oplossen met Azure IoT Hub de verbinding verbreekt
description: Meer informatie over het opsporen en oplossen van veelvoorkomende problemen met connectiviteit van apparaten voor Azure IoT Hub
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: jlian
ms.openlocfilehash: 6cc5e45ab28a1c83125a37cefb289b1662096eb0
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648816"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Detecteren en oplossen van de verbinding verbreekt met Azure IoT Hub

Problemen met de netwerkverbinding voor IoT-apparaten kunnen lastig zijn om op te lossen omdat er veel mogelijke Point of failure. Apparaat-side-toepassingslogica, fysieke netwerken, protocollen, hardware en Azure IoT Hub kunnen alle problemen veroorzaken. In dit artikel bevat aanbevelingen voor het detecteren en oplossen van problemen met het apparaat verbinding vanaf de cloud (in plaats van apparaat-kant).

## <a name="get-alerts-and-error-logs"></a>Get-waarschuwingen en foutenlogboeken

Azure Monitor gebruiken voor het ophalen van waarschuwingen en Logboeken geschreven wanneer apparaatverbindingen verwijderen.

### <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen

Als u wilt aanmelden apparaat connection-gebeurtenissen en fouten, diagnostische gegevens inschakelen voor IoT-Hub.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar uw IoT-hub.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen**.
1. Schakel **verbindingen** logboeken te verzamelen.
1. Inschakelen voor eenvoudigere analyse **verzenden naar Log Analytics** ([Zie prijzen](https://azure.microsoft.com/pricing/details/log-analytics/)). Zie het voorbeeld onder [oplossen van fouten in de basisnetwerkverbinding](#resolve-connectivity-errors).

   ![Aanbevolen instellingen][2]

Zie voor meer informatie, [de status van Azure IoT Hub bewaken en problemen vast te stellen](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-connected-devices-count-metric"></a>Waarschuwingen instellen voor de _verbonden apparaten_ aantal metrische gegevens

Voor waarschuwingen bij het verbreken van de apparaten, kunt u waarschuwingen configureren op de **verbonden apparaten (preview)** metrische gegevens.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar uw IoT-hub.
1. Selecteer **waarschuwingen**.
1. Selecteer **nieuwe waarschuwingsregel**.
1. Selecteer **voorwaarde toevoegen**, selecteer vervolgens "Verbonden apparaten (preview)".
1. Instellen van de gewenste drempelwaarden en opties voor waarschuwingen door de volgende stappen voltooien.

Zie voor meer informatie, [wat zijn klassieke waarschuwingen in Microsoft Azure?](../azure-monitor/platform/alerts-overview.md).

## <a name="resolve-connectivity-errors"></a>Connectiviteit oplossen

Als u logboeken met diagnostische gegevens en waarschuwingen voor verbonden apparaten inschakelt, krijgt u waarschuwingen wanneer er fouten optreden. In deze sectie wordt beschreven hoe u veelvoorkomende problemen oplossen wanneer u een waarschuwing ontvangt. De onderstaande stappen wordt ervan uitgegaan dat u logboeken van Azure Monitor hebt ingesteld voor uw logboeken met diagnostische gegevens.

1. Ga aan de werkruimte voor **Log Analytics** in Azure portal.
1. Selecteer **zoeken in logboeken**.
1. Voer de volgende query om te isoleren foutenlogboeken connectiviteit voor IoT-Hub, en selecteer vervolgens **uitvoeren**:

    ```
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Als er resultaten zijn, zoek naar `OperationName`, `ResultType` (foutcode), en `ResultDescription` (foutmelding) voor meer details over de fout.

   ![Voorbeeld van foutenlogboek][4]

1. Deze tabel gebruiken om te begrijpen en oplossen van veelvoorkomende fouten.

    | Fout | Hoofdoorzaak | Oplossing |
    |---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 404104 DeviceConnectionClosedRemotely | De verbinding is gesloten door het apparaat, maar de IoT Hub weet niet waarom. Veelvoorkomende oorzaken zijn MQTT/AMQP verlies van connectiviteit voor time-outs en internet. | Zorg ervoor dat het apparaat verbinding kan maken met IoT Hub door [de verbinding testen](tutorial-connectivity.md). Als de verbinding prima is, maar het apparaat herhaaldelijk verbroken wordt, zorg ervoor dat u het juiste keep alive apparaat logica voor uw eigen keuze aan protocol (MQTT/AMPQ) implementeren. |
    | 401003 IoTHubUnauthorized | IoT Hub kan niet verifiëren van de verbinding. | Zorg ervoor dat de SAS- of andere security token dat u gebruikt niet is verlopen. [Azure IoT SDK's](iot-hub-devguide-sdks.md) automatisch genereren van tokens zonder speciale configuratie. |
    | 409002 LinkCreationConflict | Een apparaat heeft meer dan één verbinding. Wanneer een nieuwe verbindingsaanvraag voor een apparaat gaat, wordt het vorige voorbeeld vanwege de volgende fout gesloten door IoT Hub. | In de meest voorkomende geval een apparaat een verbinding verbreken detecteert en probeert de verbinding te herstellen, maar nog steeds beschouwt het verbonden apparaat IoT-Hub. IoT Hub de vorige verbinding wordt gesloten en deze fout wordt vastgelegd. Deze fout treedt meestal op als een neveneffect van een probleem met andere, tijdelijke, dus zoeken naar andere fouten in de logboeken probleem verder oplossen. Anders, zorg ervoor dat u het uitgeven van een nieuwe verbindingsaanvraag alleen als de verbinding komt. |
    | 500001 ServerError | IoT Hub is een probleem met de serverzijde. Het probleem is waarschijnlijk tijdelijk. De IoT Hub-team werkt die moeilijk te onderhouden terwijl [de SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/), kleine subsets van knooppunten van de IoT Hub kunnen af en toe optreden van tijdelijke fouten. Wanneer uw apparaat probeert te verbinden met een knooppunt dat problemen ondervindt, kunt u ontvangt deze foutmelding. | Om te beperken de tijdelijke fouten, geven u een nieuwe poging van het apparaat. Naar [automatisch kunt beheren voor nieuwe pogingen](iot-hub-reliability-features-in-sdks.md#connection-and-retry), zorg ervoor dat u de nieuwste versie van de [Azure IoT SDK's](iot-hub-devguide-sdks.md).<br><br>Zie voor het wordt aanbevolen voor afhandeling van tijdelijke fouten en nieuwe pogingen [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).  <br><br>Als het probleem zich blijft na nieuwe pogingen voordoen, controleert u [Resource Health](iot-hub-monitor-resource-health.md#use-azure-resource-health) en [Azure Status](https://azure.microsoft.com/status/history/) om te zien of een bekend probleem met een IoT-Hub. Als er geen bekende problemen en het probleem blijft bestaan, [contact op met ondersteuning](https://azure.microsoft.com/support/options/) voor verder onderzoek. |
    | 500008 GenericTimeout | IoT Hub de verbindingsaanvraag voordat de time-out kan niet worden voltooid. Als een 500001 ServerError is dit een fout waarschijnlijk tijdelijk. | Volg de stappen voor probleemoplossing voor een 500001 ServerError op de hoofdoorzaak te achterhalen en los deze fout.|

## <a name="other-steps-to-try"></a>Andere stappen om te proberen

Als de vorige stappen niet hebt, kunt u het volgende proberen:

* Als u toegang hebt tot de problematische apparaten fysiek of op afstand (zoals SSH), volgt u de [gids voor probleemoplossing van apparaat-side](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) om door te gaan met het oplossen van problemen.
* Controleer of uw apparaten zijn **ingeschakeld** in Azure portal > uw IoT-hub > IoT-apparaten.
* Hulp van [forum van Azure IoT Hub](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub), of [ondersteuning van Azure](https://azure.microsoft.com/support/options/).

Ter verbetering van de documentatie voor iedereen, laat u een opmerking in het gedeelte hieronder met feedback als deze handleiding, u kunt niet hebt.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het oplossen van problemen van voorbijgaande aard, [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).
* Voor meer informatie over Azure IoT SDK en het beheer van nieuwe pogingen, Zie [over het beheren van verbindingen en betrouwbare uitwisseling van berichten met behulp van Azure IoT Hub apparaat-SDK's](iot-hub-reliability-features-in-sdks.md#connection-and-retry).

<!-- Images -->
[1]: ../../includes/media/iot-hub-diagnostics-settings/turnondiagnostics.png
[2]: ./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png
[3]: ./media/iot-hub-troubleshoot-connectivity/metric-alert.png
[4]: ./media/iot-hub-troubleshoot-connectivity/diag-logs.png
