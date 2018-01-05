---
title: Begrijpen hoe een Azure-IoT-randapparaat als een gateway kan worden gebruikt voor andere apparaten | Microsoft Docs
description: Gebruik Azure IoT rand maken een transparante ondoorzichtige of proxy-gatewayapparaat dat gegevens uit meerdere downstream apparaten verzendt naar de cloud of lokaal verwerkt.
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/27/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 3f2f9258b97d4886f41a2b991ff4de7e16379245
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway---preview"></a>Hoe een Edge van de IoT-apparaat kan worden gebruikt als een gateway - preview

Het doel van de gateways in IoT-oplossingen is specifiek voor de oplossing en connectiviteit van apparaten combineren met rand analytics. Azure IoT-rand kan worden gebruikt om te voldoen aan alle behoeften voor een IoT-gateway, ongeacht of ze zijn gerelateerd aan connectiviteit-, identiteits- of edge analytics. Gateway-patronen in dit artikel alleen verwijzen naar eigenschappen van connectiviteit downstream apparaten en apparaat-id niet hoe apparaatgegevens wordt verwerkt op de gateway.

## <a name="patterns"></a>Patronen
Er zijn drie patronen voor het gebruik van een IoT-randapparaat als een gateway: transparant, protocol vertaling en identiteit vertaling:
* **Transparante** : apparaten die theoretisch verbinding met IoT Hub maken kunnen verbinding kunnen maken met een gateway-apparaat in plaats daarvan. Dit betekent dat de downstream-apparaten hun eigen identiteiten IoT Hub hebben en van de protocollen MQTT, AMQP of HTTP gebruikmaakt. De gateway stuurt gewoon communicatie tussen de apparaten en IoT Hub. De apparaten zich niet bewust zijn dat ze met de cloud via een gateway communiceren en een gebruiker interactie met de apparaten in IoT Hub niet op de hoogte van de tussenliggende gateway-apparaat is. De gateway is dus transparant. Raadpleeg de [maken van een transparante gateway] [ lnk-iot-edge-as-transparent-gateway] -procedure voor specifieke informatie over het gebruik van een IoT-randapparaat als een transparante gateway.
* **De vertaling protocol** : apparaten die geen ondersteuning voor MQTT, AMQP of HTTP bieden een gateway-apparaat gebruiken om gegevens te verzenden naar IoT Hub. De gateway is smart om te begrijpen dat protocol dat wordt gebruikt door de downstream-apparaten; het is echter het enige apparaat dat de identiteit van IoT-Hub heeft. Alle informatie lijkt erop dat het afkomstig is van een apparaat, de gateway. Dit betekent dat downstream apparaten aanvullende identificatiegegevens in hun berichten insluiten moeten als cloud-toepassingen reden over de gegevens op basis van per apparaat wilt. Bovendien, zoals IoT Hub primitieven twin en methoden zijn alleen beschikbaar voor het gateway-apparaat niet downstream-apparaten.
* **De vertaling van identiteit** -apparaten die geen verbinding met IoT Hub maken verbinding maken met een gatewayapparaat dat zorgt voor een IoT Hub identiteits- en protocol omzetting namens de downstream-apparaten. De gateway is slim genoeg om het protocol dat wordt gebruikt door de downstream-apparaten begrijpen, identiteit bieden en IoT Hub primitieven vertalen. Downstream apparaten weergegeven in IoT Hub als klas apparaten met horende en methoden. Een gebruiker kan communiceren met de apparaten in IoT Hub en is niet op de hoogte van de tussenliggende gateway-apparaat.

![Diagrammen van gateway patronen][1]

## <a name="use-cases"></a>Gebruiksvoorbeelden
Alle gateway patronen bieden de volgende voordelen:
* **Rand analytics** – gebruik AI services lokaal om gegevens te verwerken die afkomstig zijn van de downstream-apparaten zonder verzenden volledige fidelity telemetrie naar de cloud. Zoek en reageren op lokaal insights en alleen een subset van gegevens verzenden naar IoT Hub. 
* **Downstream apparaat isolatie** – het gateway-apparaat kunt afschermen alle downstream-apparaten van blootgesteld aan internet. Deze kan bevinden zich tussen een iet-netwerk dat geen verbinding met en een IT-netwerk dat toegang tot het web biedt. 
* **Verbinding multiplex** : alle apparaten die verbinding maakt met IoT Hub gebruiken de dezelfde onderliggende verbinding via een IoT-Edge apparaat.
* **Verkeer vloeiend** -Edge van de IoT-apparaat wordt automatisch implementeren exponentieel uitstel in geval van een IoT Hub beperking, terwijl de berichten lokaal persistent maken. Hierdoor werkt uw oplossing robuuste naar pieken in het verkeer.
* **Beperkte ondersteuning voor offline** : het gateway-apparaat lokaal berichten worden opgeslagen en twin updates die naar IoT Hub kan niet worden bezorgd.

Een gateway biedt protocolvertaling kan ook uitvoeren rand analytics, apparaat isolatie verkeer-aanpassing en Offlineondersteuning aan bestaande apparaten en nieuwe apparaten die beperkt resource zijn. Veel bestaande apparaten zijn het opstellen van gegevens die ten grondslag liggen kunnen zakelijke inzichten; ze zijn echter niet ontworpen met cloud-connectiviteit in gedachten. Ondoorzichtige gateways kunnen deze gegevens kunnen worden ontgrendeld en gebruikt in een end-to-end-IoT-oplossing.

Een gateway die heeft de vertaling van identiteit bieden de voordelen van protocolvertaling en bovendien toestaan voor volledig beheer van downstream apparaten vanuit de cloud. Alle apparaten in uw IoT-oplossing worden weergegeven in IoT Hub ongeacht het protocol met ze te laten spreken.

## <a name="cheat-sheet"></a>Overzichtskaart
Hier volgt een snelle bedriegen blad waarmee IoT Hub primitieven, met behulp van transparante, ondoorzichtige, wordt vergeleken en proxy-gateways.

| &nbsp; | Transparante gateway | Protocolvertaling | De vertaling van identiteit |
|--------|-------------|--------|--------|
| Identiteiten die zijn opgeslagen in de id-register van IoT Hub | Identiteit van alle verbonden apparaten | Alleen de identiteit van het gateway-apparaat | Identiteit van alle verbonden apparaten |
| Dubbel apparaat | Elk aangesloten apparaat heeft een eigen apparaat-twin | Alleen de gateway heeft een horende apparaten en -module | Elk aangesloten apparaat heeft een eigen apparaat-twin |
| Directe methoden en cloud-naar-apparaat-berichten | De cloud kunt elk aangesloten apparaat afzonderlijk oplossen | De cloud kan zich alleen richten op het gateway-apparaat | De cloud kunt elk aangesloten apparaat afzonderlijk oplossen |
| [IoT Hub, vertragingen en quota 's][lnk-iothub-throttles-quotas] | Van toepassing op elk apparaat | Van toepassing op het gateway-apparaat | Van toepassing op elk apparaat |

Wanneer u een patroon ondoorzichtige gateway (protocolvertaling) gebruikt, delen alle apparaten die verbinding maken via de gateway die dezelfde cloud-naar-apparaat wachtrijexemplaar, dit kan maximaal 50 berichten bevatten. Betekent dit dat het patroon ondoorzichtige gateway moet worden gebruikt wanneer heel weinig apparaten verbinding via de veldgateway voor elk maken en hun cloud-naar-apparaat-verkeer laag is.

## <a name="next-steps"></a>Volgende stappen
Gebruik een IoT-randapparaat als een [transparante gateway][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway.md
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md

[1]: ./media/iot-edge-as-gateway/edge-as-gateway.png
