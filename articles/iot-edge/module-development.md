---
title: Modules ontwikkelen voor Azure IoT rand | Microsoft Docs
description: Informatie over het maken van aangepaste modules voor Azure IoT rand
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 95b1d5d4e5e11f96b6abb17f0aeba935cc65512d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules---preview"></a>Overzicht van de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules - voorbeeld

In dit artikel wordt uitgelegd welke functies beschikbaar zijn bij het schrijven van toepassingen die worden uitgevoerd als de rand van de IoT-module en hoe om te profiteren van deze.

## <a name="iot-edge-runtime-environment"></a>Runtime-omgeving zijde IoT
De rand van de IoT-runtime levert de infrastructuur voor het integreren van de functionaliteit van meerdere IoT Edge-modules en deze te implementeren op de IoT Edge-apparaten. Op hoog niveau, kunt een programma als een Edge van de IoT-module worden verpakt. Echter om te kunnen profiteren van de rand van de IoT communicatie en functies op management, een programma wordt uitgevoerd in een module kan verbinding maken met de lokale rand van de IoT hub, geïntegreerd in de rand van de IoT-runtime.

## <a name="using-the-iot-edge-hub"></a>Met behulp van de rand van de IoT-hub
De rand van de IoT-hub biedt twee belangrijke functies: proxy met IoT Hub en lokale communicatie.

### <a name="iot-hub-primitives"></a>IoT Hub primitieven
IoT Hub ziet een module-exemplaar analogously op een apparaat, in de zin dat deze:

* Er is een module-twin, die is uniek en geïsoleerd van de [apparaat twin] [ lnk-devicetwin] en de andere module horende van het apparaat;
* het verzenden van [apparaat-naar-cloudberichten][lnk-iothub-messaging];
* het kan ontvangen [methoden directe] [ lnk-methods] gericht specifiek op de identiteit.

Een module kan niet op dit moment cloud-naar-apparaat-berichten ontvangen of de functie voor het uploaden van bestand gebruiken.

Bij het schrijven van een module kunt u gewoon gebruiken de [Azure IoT-Device SDK] [ lnk-devicesdk] verbinding maken met de rand van de IoT-hub en de bovenstaande functionaliteit gebruiken, net als bij het gebruik van IoT Hub met de apparaattoepassing van een, de enige het verschil is dat, van uw toepassing back-end, u moet verwijzen naar de module-id in plaats van de identiteit van het apparaat wordt.

Zie [ontwikkelen en implementeren van een module IoT Edge op een gesimuleerd apparaat] [ lnk-tutorial2] voor een voorbeeld van een module-toepassing die apparaat-naar-cloud-berichten verzendt en maakt gebruik van de module-twin.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten
Om in te schakelen complexe verwerking van apparaat-naar-cloud-berichten rand van de IoT hub biedt declaratieve routering van berichten tussen modules en tussen modules en IoT Hub.
Hierdoor kunnen modules worden onderschept en verwerken van berichten die door andere modules verzonden en deze kan doorgeven in complexe pijplijnen.
Het artikel [modulesamenstelling] [ lnk-module-comp] wordt uitgelegd hoe u voor het opstellen van modules in complexe pijplijnen met behulp van routes.

Een IoT-Edge-module kan anders dan een normale IoT Hub apparaat-toepassing, apparaat-naar-cloud-berichten die via een proxyserver doorgestuurd door de lokale rand van de IoT-hub worden voordat ze ontvangen.

Rand van de IoT-hub geeft de berichten naar de module op basis van declaratieve routes die worden beschreven in de [modulesamenstelling] [ lnk-module-comp] artikel. Bij het ontwikkelen van een IoT-Edge-module, u kunt deze berichten ontvangen door in te stellen bericht handlers, zoals wordt weergegeven in de zelfstudie [ontwikkelen en implementeren van een module IoT Edge op een gesimuleerd apparaat][lnk-tutorial2].

Om het maken van routes vereenvoudigen, IoT rand voegt het concept van module *invoer* en *uitvoer* eindpunten. Een module kan ontvangen alle apparaat-naar-cloud-berichten doorgestuurd naar het zonder op te geven van de invoer en apparaat-naar-cloud-berichten kan verzenden zonder op te geven geen uitvoer.
Gebruik expliciete invoer en uitvoer, maar maakt routeringsregels eenvoudiger te begrijpen. Zie [modulesamenstelling] [ lnk-module-comp] voor meer informatie over de regels voor het doorsturen en invoer- en eindpunten voor modules.

Apparaat-naar-cloud-berichten die zijn verwerkt door de hub rand datumstempel ten slotte zijn met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| $connectionDeviceId | De apparaat-ID van de client die het bericht heeft verzonden |
| $connectionModuleId | De module-ID van de module die het bericht heeft verzonden |
| $inputName | De ingang die dit bericht heeft ontvangen. Kan niet leeg zijn. |
| $outputName | De uitvoer die wordt gebruikt om het bericht te verzenden. Kan niet leeg zijn. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Verbinding maken met de rand van de IoT hub vanuit een module
Verbinden met de lokale rand van de IoT-hub vanuit een module bestaat uit twee stappen: de verbindingsreeks gebruiken opgegeven door de runtime IoT rand wanneer de module wordt gestart, en zorg ervoor dat uw toepassing de door de rand van de IoT-hub op het apparaat aangeboden certificaat accepteert.

Het maken van verbinding tekenreeks moet worden gebruikt door de runtime IoT rand in de omgevingsvariabele wordt geïnjecteerd `EdgeHubConnectionString`. Dit maakt het beschikbaar is voor alle programma's die wil worden gebruikt.

Analogously, het certificaat te gebruiken voor het valideren van de rand van de IoT hub-verbinding wordt geïnjecteerd door de runtime IoT Edge in een bestand waarvan het pad beschikbaar in de omgevingsvariabele is `EdgeModuleCACertificateFile`.

De zelfstudie [ontwikkelen en implementeren van een module IoT Edge op een gesimuleerd apparaat] [ lnk-tutorial2] laat zien hoe om ervoor te zorgen dat het certificaat in het archief van de computer in uw toepassing module is. Duidelijk, een andere methode verbindingen met het werk van dat certificaat vertrouwen.

## <a name="packaging-as-an-image"></a>Verpakking als afbeelding
Rand van de IoT-modules worden geleverd als Docker-installatiekopieën.
U kunt Docker toolchain rechtstreeks of Visual Studio Code gebruiken zoals wordt weergegeven in de zelfstudie [ontwikkelen en implementeren van een module IoT Edge op een gesimuleerd apparaat][lnk-tutorial2].

## <a name="next-steps"></a>Volgende stappen

Nadat u een module ontwikkelt, informatie over hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal][lnk-howto-deploy].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-devicetwin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-messaging]: ../iot-hub/iot-hub-devguide-messaging.md
[lnk-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-tutorial2]: tutorial-csharp-module.md
[lnk-module-comp]: module-composition.md
[lnk-howto-deploy]: how-to-deploy-monitor.md
