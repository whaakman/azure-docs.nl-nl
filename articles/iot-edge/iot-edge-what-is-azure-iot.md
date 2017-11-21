---
title: Azure-oplossingen voor het Internet of Things (IoT Edge) | Microsoft Docs
description: "Overzicht van een voorbeeldoplossing met IoT-architectuur en hoe deze is gekoppeld aan apparaten, de Azure IoT Hub-service, Azure IoT-apparaat-SDK's, Azure IoT-service-SDK’s en andere Azure-services."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: 587b733106d511ec63d71f67a06e520324a3e594
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Volgende stappen

Azure IoT Edge is een Azure-service die analyse en gegevensverwerking aan de rand van het netwerk mogelijk maakt. Met IoT Edge kunt uw apparaten voorzien van op containers gebaseerde code die logica bevat die rechtstreeks wordt opgehaald vanuit de Azure-services die u al gebruikt of u kunt uw eigen oplossingsspecifieke code gebruiken. Hiermee kunt u uw apparaten de volgende mogelijkheden geven:

* Fungeren als gatewayapparaten die gegevens van meerdere leaf-apparaten samenvoegen en verwerken.
* Anomaliedetectie uitvoeren en reageren op wijzigingen in de omgeving zonder te moeten wachten op instructies uit de cloud.
* Bandbreedte en opslagkosten tot het minimum beperken door gegevens vooraf te verwerken en de resultaten te verzenden. 

IoT Edge omvat ook een cloudinterface die extern beheer van apparaten mogelijk maakt. U kunt code implementeren, de status controleren en deze bijwerken zonder dat u fysiek toegang hoeft te hebben tot uw apparaten. U kunt individuele apparaten op afstand beheren of implementaties maken die worden toegepast op grote sets met apparaten die u definieert. Zie [IoT Edge-implementaties voor individuele apparaten of op grote schaal][lnk-deployment] voor meer informatie.

Zie [Hoe Azure IoT Edge werkt][lnk-overview] voor meer informatie over de onderdelen die IoT Edge mogelijk maken.

Als u Azure IoT Hub nog niet hebt gebruikt, kunt u beginnen met een [Overzicht van de service Azure IoT Hub][lnk-iot-hub].

[lnk-deployment]: module-deployment-monitoring.md
[lnk-overview]: how-iot-edge-works.md
[lnk-iot-hub]: ../iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
