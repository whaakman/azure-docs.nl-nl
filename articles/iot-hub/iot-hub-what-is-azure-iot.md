---
title: Azure-oplossingen voor het Internet of Things | Microsoft Docs
description: "Een overzicht van IoT op Azure met onder andere een voorbeeldarchitectuur voor een oplossing en hoe deze zich verhoudt tot Azure IoT Hub, apparaat-SDK’s en vooraf geconfigureerde oplossingen"
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
ms.date: 10/05/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: 00b2e90901d763d218d1118f1d7ef2bf617a2d2f


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Volgende stappen
Azure IoT-Hub is een Azure-service voor beveiligde en betrouwbare bidirectionele communicatie tussen de back-end van uw toepassing en miljoenen apparaten. Het stelt de back-end van de toepassing in staat om:

* telemetrie op schaal van uw apparaten te ontvangen;
* gegevens van uw apparaten naar een processor voor streaming-gebeurtenissen te routeren;
* uploads van bestanden vanaf apparaten te ontvangen;
* cloud-naar-apparaat-opdrachten naar specifieke apparaten te verzenden.

Met IoT-Hub kunt u zelf de back-end van uw oplossing implementeren. IoT Hub bevat bovendien een id-register dat wordt gebruikt voor het inrichten van apparaten, hun beveiligingsreferenties en hun rechten om verbinding maken met de IoT-hub. Zie [Wat is IoT Hub?][lnk-iot-hub] voor meer informatie over IoT Hub.

Zie [Overzicht van apparaatbeheer met IoT Hub][lnk-device-management] voor meer informatie over hoe Azure IoT Hub op standaarden gebaseerd IoT-apparaatbeheer voor u mogelijk maakt, om zo uw apparaten op afstand te beheren, te configureren en bij te werken.

U kunt de Azure IoT Device SDK's gebruiken voor het implementeren van clienttoepassingen op een groot aantal hardwareplatforms en besturingssystemen. De apparaat-SDK's bevatten bibliotheken die het eenvoudiger maken om telemetrie te verzenden naar een IoT-hub en cloud-naar-apparaatopdrachten te ontvangen. Wanneer u de apparaat-SDK's gebruikt, kunt u kiezen uit verschillende netwerkprotocollen om te communiceren met IoT Hub. Raadpleeg ook de [informatie over apparaat-SDK's][lnk-device-sdks].

Raadpleeg de zelfstudie [Aan de slag met IoT Hub][lnk-getstarted] als u code wilt leren schrijven en een aantal voorbeelden wilt uitvoeren.

Wellicht bent u ook geïnteresseerd in [Azure IoT Suite][lnk-iot-suite]. Dit is een verzameling van vooraf geconfigureerde oplossingen. Met IoT-Suite kunt u snel aan de slag en IoT-projecten schalen om veelvoorkomende problemen met algemene IoT-scenario's op te lossen. Denk bijvoorbeeld aan externe controle, beheer van assets en voorspellend onderhoud.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Nov16_HO5-->


