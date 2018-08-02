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
ms.openlocfilehash: eb186f4b6e1d742c9cae51e68b3d3dbda1bb751c
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400418"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Detecteren en oplossen van de verbinding verbreekt met Azure IoT Hub

Problemen met de netwerkverbinding voor IoT-apparaten kunnen lastig zijn om op te lossen omdat er veel mogelijke Point of failure. Apparaat-side-toepassingslogica, fysieke netwerken, protocollen, hardware en Azure IoT Hub kunnen alle problemen veroorzaken. Dit document bevat aanbevelingen voor het detecteren en oplossen van problemen met het apparaat verbinding van de cloud aan (in plaats van apparaat-side).

## <a name="get-alerts-and-error-logs"></a>Get-waarschuwingen en foutenlogboeken

Azure Monitor gebruiken voor het ophalen van waarschuwingen en Logboeken geschreven wanneer apparaatverbindingen verwijderen.

### <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen 

Als u wilt aanmelden apparaat connection-gebeurtenissen en fouten, diagnostische gegevens inschakelen voor IoT-Hub. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Ga naar uw IoT-hub.
1. Selecteer **diagnostische instellingen**.
1. Selecteer vervolgens **diagnostische gegevens inschakelen**.
1. Zorg ervoor dat u inschakelt **verbindingen** logboeken te verzamelen. 
1. Analyse om eenvoudiger te maken, schakelt u **verzenden naar Log Analytics** ([Zie prijzen](https://azure.microsoft.com/pricing/details/log-analytics/)). Een voorbeeld later in dit artikel maakt gebruik van Log Analytics.

   ![Aanbevolen instellingen][2]

Zie voor meer informatie, [de status van Azure IoT Hub bewaken en problemen vast te stellen](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-connected-devices-count-metric"></a>Waarschuwingen instellen voor de verbonden apparaten aantal metrische gegevens

Voor waarschuwingen bij het verbreken van de apparaten, kunt u waarschuwingen configureren op de *verbonden apparaten* metrische gegevens. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw IoT-Hub.
1. Selecteer **waarschuwingen (klassiek)**.
1. Klik op **metrische waarschuwing toevoegen (klassiek)**.
1. Vul het formulier en selecteer **OK**. 

   ![Aanbevolen waarschuwing voor metrische gegevens][3]

Zie voor meer informatie, [wat zijn klassieke waarschuwingen in Microsoft Azure?](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="resolve-common-connectivity-errors"></a>Algemene connectiviteit oplossen

Als diagnostische logboeken en waarschuwingen voor verbonden apparaten zijn ingeschakeld, krijgt u waarschuwingen wanneer er iets fout gaat. In deze sectie wordt beschreven hoe u veelvoorkomende problemen oplossen wanneer u een waarschuwing ontvangt. De onderstaande stappen wordt ervan uitgegaan dat u Log Analytics hebt ingesteld voor uw logboeken met diagnostische gegevens. 

1. Ga aan de werkruimte voor **Log Analytics** in Azure portal.
1. Klik op **zoeken in logboeken**.
1. Voer deze query om te isoleren foutenlogboeken connectiviteit voor IoT-Hub, druk in het vak **uitvoeren**.

    ```
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Als er resultaten zijn, zoek de `OperationName`, `ResultType` (foutcode), en `ResultDescription` (foutmelding) voor meer details over de fout.

   ![Voorbeeld van foutenlogboek][4]

1. Deze tabel gebruiken om te begrijpen en oplossen van veelvoorkomende fouten.

    | Fout | Hoofdoorzaak | Oplossing |
    |---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 404104 DeviceConnectionClosedRemotely | De verbinding is gesloten door het apparaat, maar IoT-Hub weet niet waarom. Veelvoorkomende oorzaken zijn MQTT/AMQP verlies van connectiviteit voor time-outs en internet. | Zorg ervoor dat het apparaat verbinding kan maken met IoT Hub door [de verbinding testen](tutorial-connectivity.md). Als de verbinding prima is, maar het apparaat herhaaldelijk verbroken wordt, zorg ervoor dat u het juiste keep alive apparaat logica voor uw eigen keuze aan protocol (MQTT/AMPQ) implementeren. |
    | 401003 IoTHubUnauthorized | IoT Hub kan niet verifiëren van de verbinding. | Zorg ervoor dat de SAS- of andere security token dat u gebruikt niet is verlopen. [Azure IoT SDK's](iot-hub-devguide-sdks.md) automatisch genereren van tokens zonder speciale configuratie. |
    | 409002 LinkCreationConflict | Er zijn meer dan één verbindingen voor hetzelfde apparaat. Wanneer een nieuwe verbindingsaanvraag voor een apparaat gaat, wordt het vorige voorbeeld vanwege de volgende fout gesloten door IoT Hub. | Zorg ervoor dat u een nieuwe verbindingsaanvraag uitgeven, alleen als de verbinding komt. |
    | 500001 ServerError | IoT Hub is een probleem met de serverzijde. Het probleem is waarschijnlijk tijdelijk. IoT Hub-team werkt moeilijk te onderhouden terwijl [de SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/), kleine subsets van knooppunten van de IoT Hub kunnen af en toe optreden van tijdelijke fouten. Wanneer uw apparaat probeert te verbinden met een knooppunt dat problemen ondervindt, kunt u ontvangt deze foutmelding. | Om te beperken de tijdelijke fouten, geven u een nieuwe poging van het apparaat. Naar [automatisch kunt beheren voor nieuwe pogingen](iot-hub-reliability-features-in-sdks.md), zorg ervoor dat u de nieuwste versie van de [Azure IoT SDK's](iot-hub-devguide-sdks.md).<br><br>Zie voor het wordt aanbevolen voor afhandeling van tijdelijke fouten en nieuwe pogingen [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults.md).  <br><br>Als het probleem zich blijft na nieuwe pogingen voordoen, controleert u [Resource Health](iot-hub-monitor-resource-health.md#use-azure-resource-health) en [Azure Status](https://azure.microsoft.com/status/history/) om te zien of een bekend probleem met een IoT-Hub. Als er geen bekende problemen en het probleem blijft bestaan, [contact op met ondersteuning](https://azure.microsoft.com/support/options/) voor verder onderzoek. |
    | 500008 GenericTimeout | IoT Hub de verbindingsaanvraag voordat de time-out kan niet worden voltooid. Zoals 500001 ServerError is dit een fout waarschijnlijk tijdelijk. | Volg de stappen voor probleemoplossing voor 500001 ServerError hoofdoorzaak en los deze fout.|

## <a name="other-steps-to-try"></a>Andere stappen om te proberen

Als de bovenstaande stappen hebt gehad, vindt hier u meer volgende proberen:

* Als u toegang hebt tot de problematische apparaten fysiek of op afstand (zoals SSH), volgt u de [gids voor probleemoplossing van apparaat-side](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) om door te gaan met het oplossen van problemen.
* Controleer of uw apparaten zijn **ingeschakeld** in Azure portal > uw IoT-Hub > IoT-apparaten.
* Hulp van [forum van Azure IoT Hub](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub), of [ondersteuning van Azure](https://azure.microsoft.com/support/options/).

Ter verbetering van de documentatie voor iedereen, laat u hieronder een opmerking als deze handleiding, u kunt niet hebt.

<!-- Images -->
[1]: ../../includes/media/iot-hub-diagnostics-settings/turnondiagnostics.png
[2]: ./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png
[3]: ./media/iot-hub-troubleshoot-connectivity/metric-alert.png
[4]: ./media/iot-hub-troubleshoot-connectivity/diag-logs.png
