---
title: Overzicht van Azure industriële IoT | Microsoft Docs
description: Overzicht van industriële IoT
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 1dee51354ba9210e52459dcf7a96695bc3736648
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759263"
---
# <a name="what-is-industrial-iot-iiot"></a>Wat is industriële IoT (IIoT)

IIoT is het industriële Internet of Things. IIoT verbetert industriële efficiëntie door de IoT-toepassing in de productie-industrie. 

## <a name="improve-industrial-efficiencies"></a>Industriële efficiëntie verbeteren

Verbeter uw operationele productiviteit en winstgevendheid met een oplossingsverbetering voor verbonden factory's. Laat uw industriële uitrusting en apparaten verbinding maken in de cloud en bewaak deze—inclusief de machines die al op de werkvloer werkzaam zijn. Analyseer uw IoT-gegevens voor inzichten waarmee u de prestaties van de gehele werkvloer kunt verbeteren.

Te verminderen de tijdrovend proces van toegang tot factory floor machines met Azure IoT OPC UA device management (OPC dubbele) en richt u uw tijd op het bouwen van IIoT oplossingen. Het beheren van certificaten en industriële activa-integratie met Azure IoT OPC UA-Certificaatbeheer (OPC-kluis) te stroomlijnen en erop vertrouwen dat asset connectiviteit is beveiligd. Deze microservices bieden een REST-achtige API boven [industriële IoT Azure-onderdelen](https://github.com/Azure/azure-iiot-opc-ua). De service-API biedt u beheer van edge-module-functionaliteit. 

![Industriële IoT-beveiligingsoverzicht](media/overview-iot-industrial/overview.png)

> [!NOTE]
> Zie voor meer informatie over Azure industriële IoT-services, GitHub [opslagplaats ](https://github.com/Azure/azure-iiot-services).
Als u niet bekend bent met de werking van Azure IoT Edge-modules, begint u met de volgende artikelen:
- [Informatie over Azure IoT Edge](../iot-edge/about-iot-edge.md)
- [Azure IoT Edge-modules](../iot-edge/iot-edge-modules.md)

## <a name="connected-factory"></a>Verbonden factory

[Verbonden Factory](../iot-accelerators/iot-accelerators-connected-factory-features.md) is een implementatie van de Microsoft Azure industriële IoT-referentiearchitectuur die kan worden aangepast om te voldoen aan specifieke zakelijke vereisten. De volledige oplossing-code is open-source en beschikbaar op GitHub-opslagplaats voor verbonden Factory-oplossing accelerator. U kunt de implementatie gebruiken als uitgangspunt voor een commercieel product en om binnen enkele minuten een vooraf gebouwde oplossing te implementeren in uw Azure-abonnement. 

## <a name="factory-floor-connectivity"></a>Factory floor connectiviteit

Azure IoT OPC UA-Apparaatbeheer, ook wel bekend als OPC dubbele, is een IIoT-onderdeel dat automatiseert de detectie van en de registratie en beheer op afstand van industriële apparaten via REST API's biedt. OPC-Twin maakt gebruik van Azure IoT Edge en IoT Hub om de cloud en het netwerk factory verbinding te maken. OPC-Twin kunnen ontwikkelaars IIoT richten op het IIoT toepassingen bouwen zonder u zorgen te maken over hoe u veilig toegang krijgen tot de on-premises computers.

## <a name="security"></a>Beveiliging

Azure IoT OPC UA-Certificaatbeheer of OPC-kluis is een implementatie van OPC UA globale detectie Server (GDS) die u kunt configureren, te registreren en beheren van de levenscyclus van het certificaat voor OPC UA-server en clienttoepassingen in de cloud. OPC-Vault vereenvoudigt de implementatie en het onderhoud van de beveiligde activa-verbinding in de industrie ruimte. Door het automatiseren van Certificaatbeheer kunnen OPC kluis factory operators van de handmatige en complexe processen die zijn gekoppeld aan de connectiviteit en Certificaatbeheer.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een inleiding tot industriële IoT en de bijbehorende onderdelen hebt, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Wat is de OPC-Twin](overview-opc-twin.md)
