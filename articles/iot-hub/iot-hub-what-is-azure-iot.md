---
title: Azure-oplossingen voor het Internet of Things | Microsoft Docs
description: Een overzicht van IoT op Azure met onder andere een voorbeeldarchitectuur voor een oplossing en hoe deze zich verhoudt tot Azure IoT Hub, apparaat-SDK’s en vooraf geconfigureerde oplossingen
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: dobett

---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## Volgende stappen
Azure IoT-Hub is een Azure-service voor beveiligde en betrouwbare bidirectionele communicatie tussen de back-end van uw toepassing en miljoenen apparaten. Het stelt de back-end van de toepassing in staat om:

* telemetrie op schaal van uw apparaten te ontvangen;
* gegevens van uw apparaten naar een processor voor streaming-gebeurtenissen te routeren;
* uploads van bestanden vanaf apparaten te ontvangen;
* cloud-naar-apparaat-opdrachten naar specifieke apparaten te verzenden.

Met IoT-Hub kunt u zelf de back-end van uw oplossing implementeren. IoT-Hub bevat bovendien een register met apparaat-id’s dat wordt gebruikt voor het inrichten van apparaten, hun beveiligingsreferenties en hun rechten om verbinding maken met de hub. Zie [Wat is IoT-Hub?][lnk-iot-hub] voor meer informatie over IoT-Hub.

Zie [Overview of Azure IoT Hub device management (overzicht van apparaatbeheer via Azure IoT Hub)][lnk-device-management] voor meer informatie over hoe Azure IoT-Hub op standaarden gebaseerd IoT-apparaatbeheer voor u mogelijk maakt om uw apparaten op afstand te beheren, te configureren en bij te werken.

U kunt de SDK's van het IoT-apparaat gebruiken voor het implementeren van clienttoepassingen op een groot aantal hardwareplatforms en besturingssystemen. De SDK's van het IoT-apparaat bevatten bibliotheken die het eenvoudiger maken om telemetrie te verzenden naar een IoT-hub en cloud-naar-apparaatopdrachten te ontvangen. Wanneer u de SDK's gebruikt, kunt u kiezen uit verschillende netwerkprotocollen om te communiceren met IoT Hub. Raadpleeg ook de [informatie over apparaat-SDK's][lnk-device-sdks].

Raadpleeg de zelfstudie [Aan de slag met IoT Hub][lnk-getstarted] als u code wilt leren schrijven en een aantal voorbeelden wilt uitvoeren.

Wellict bent u ook geïnteresseerd in de [Azure IoT Suite][lnk-iot-suite]. Dit is een verzameling van vooraf geconfigureerde oplossingen. Met IoT-Suite kunt u snel aan de slag en IoT-projecten schalen om veelvoorkomende problemen met algemene IoT-scenario's op te lossen. Denk bijvoorbeeld aan externe controle, beheer van assets en voorspellend onderhoud.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md


<!--HONumber=Oct16_HO3-->


