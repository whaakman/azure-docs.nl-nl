---
title: Gebruik Azure IoT Edge-apparaten als gateways | Microsoft Docs
description: Gebruik Azure IoT Edge maakt een transparante ondoorzichtig of proxy-gatewayapparaat dat gegevens uit meerdere downstream apparaten verzendt naar de cloud of lokaal verwerkt.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e1825bcdd8dbb06ef027919416b2d0532a7df9c2
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47422827"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Hoe een IoT Edge-apparaat kan worden gebruikt als een gateway

Het doel van gateways in IoT-oplossingen is specifiek voor de oplossing en verbindingsmogelijkheden voor apparaten met edge-analyses te combineren. Azure IoT Edge kan worden gebruikt om te voldoen aan alle behoeften voor een IoT-gateway, ongeacht of ze met betrekking tot connectiviteit, identiteits- of edge-analyses. Gateway-patronen in dit artikel alleen verwijzen naar eigenschappen van downstream apparaatconnectiviteit en apparaat-id niet hoe apparaatgegevens worden verwerkt op de gateway.

## <a name="patterns"></a>Patronen
Er zijn drie patronen voor het gebruik van een IoT Edge-apparaat als gateway: transparant protocol vertaling en identiteit vertaling:
* **Transparante** -apparaten die in theorie verbinding met IoT Hub maken kunnen kunnen verbinding maken met een gateway-apparaat in plaats daarvan. Dit betekent dat de downstream-apparaten hun eigen IoT Hub-identiteiten hebben en van de protocollen MQTT-, AMQP- of HTTP gebruikmaakt. De gateway wordt gewoon doorgegeven voor communicatie tussen de apparaten en IoT-Hub. De apparaten zijn niet op de hoogte dat ze met de cloud via een gateway communiceren en een gebruiker interactie met de apparaten in IoT Hub geen informatie over het tussenliggende gateway-apparaat is. De gateway is dus transparant. Raadpleeg de [een transparante gateway maken] [ lnk-iot-edge-as-transparent-gateway] uitleg voor meer informatie over het gebruik van een IoT Edge-apparaat als een transparante gateway.
* **NAT-protocol** – ook wel aangeduid als ondoorzichtige gateway patroon, apparaten die geen ondersteuning voor MQTT-, AMQP- of HTTP gebruiken een gateway-apparaat om gegevens te verzenden naar IoT Hub. De gateway is slimme genoeg is om te begrijpen dat protocol dat wordt gebruikt door de downstream-apparaten; het is echter dat het apparaat met identiteit in IoT Hub. Alle informatie lijkt erop dat het afkomstig is van een apparaat, de gateway. Dit betekent dat downstream apparaten aanvullende identificatiegegevens in hun berichten insluiten moeten als cloud-toepassingen reden over de gegevens op basis van per apparaat wilt. Bovendien IoT-Hub primitieven zoals dubbele en methoden zijn alleen beschikbaar voor de gateway-apparaat, niet downstream-apparaten.
* **Identiteit vertaling** -apparaten die geen verbinding met IoT-Hub maken verbinding maken met een gateway-apparaat waarmee u IoT-Hub identiteits- en protocol vertaling namens de downstream-apparaten. De gateway is slimme begrijpt het protocol dat wordt gebruikt door de downstream-apparaten en vertalen van IoT-Hub primitieven identiteit bieden. Downstream-apparaten worden weergegeven in IoT Hub als eersteklas-apparaten met dubbels en methoden. Een gebruiker kan communiceren met de apparaten in IoT Hub en is geen informatie over het tussenliggende gateway-apparaat.

![Diagrammen van gateway-patronen][1]

## <a name="use-cases"></a>Gebruiksvoorbeelden
Alle gateway patronen bieden de volgende voordelen:
* **Analyse van de rand** – gebruik AI-services lokaal om gegevens te verwerken die afkomstig zijn van downstream apparaten zonder te verzenden volledige kwaliteit telemetrie naar de cloud. Zoek en reageren op inzichten lokaal en alleen een subset van gegevens verzenden naar IoT Hub. 
* **Downstream apparaat isolatie** – het gateway-apparaat kunt beschermen alle downstream-apparaten van blootstelling aan internet. Deze kan blijven tussen een OT-netwerk dat niet is verbonden en een IT-netwerk waarmee u toegang hebt tot het web. 
* **Verbinding multiplexing** - alle apparaten die verbinding maakt met IoT Hub via een IoT-Edge apparaat dezelfde onderliggende verbinding gebruikt.
* **Aanpassing van verkeer** -exponentieel uitstel in het geval van IoT-Hub beperking, tijdens het opslaan van de berichten die lokaal wordt automatisch implementeren door de IoT Edge-apparaat. Dit maken wilt voor uw oplossing tegen pieken in verkeer.
* **Beperkte ondersteuning voor offline** : het gateway-apparaat worden lokaal opgeslagen berichten en dubbele updates die naar IoT Hub kan niet worden bezorgd.

Een gateway doet protocollen vertalen kan ook uitvoeren randanalyses, apparaat isolatie, verkeer vloeiend maken en offline ondersteuning aan bestaande apparaten en nieuwe apparaten die beperkte resource zijn. Veel bestaande apparaten produceren gegevens die bedrijfsinzichten aansturen kunnen; ze zijn echter niet ontworpen met cloud-connectiviteit in gedachten. Ondoorzichtige gateways zodat deze gegevens kunnen worden ontgrendeld en gebruikt in een end-to-end-IoT-oplossing.

Een gateway die identiteit vertaling bieden de voordelen van protocollen vertalen en bovendien staan voor de volledige beheermogelijkheden van downstream apparaten vanuit de cloud. Alle apparaten in uw IoT-oplossing te zien in IoT Hub, ongeacht het protocol met ze spreekt.

## <a name="cheat-sheet"></a>Cheatsheet
Hier volgt een snel referentieoverzicht waarmee IoT-Hub primitieven vergeleken bij het gebruik van transparant ondoorzichtig (protocol) en proxy-gateways.

| &nbsp; | Transparante gateway | Protocollen vertalen | Identiteit vertaling |
|--------|-------------|--------|--------|
| Identiteiten die zijn opgeslagen in het id-register van IoT Hub | Identiteiten voor alle verbonden apparaten | Alleen de identiteit van de gateway-apparaat | Identiteiten voor alle verbonden apparaten |
| Dubbel apparaat | Elk verbonden apparaat heeft een eigen apparaatdubbel | Alleen de gateway is een dubbele apparaat- en -module | Elk verbonden apparaat heeft een eigen apparaatdubbel |
| Directe methoden en cloud-naar-apparaat-berichten | De cloud kan voorzien in elk verbonden apparaat afzonderlijk | Het gateway-apparaat kan alleen voorzien in de cloud | De cloud kan voorzien in elk verbonden apparaat afzonderlijk |
| [Vertragingen in IoT Hub en quota][lnk-iothub-throttles-quotas] | Van toepassing op elk apparaat | Van toepassing op de gateway-apparaat | Van toepassing op elk apparaat |

Wanneer u een patroon ondoorzichtige gateway (protocollen vertalen) gebruikt, delen alle apparaten die verbinding te maken via de gateway die de dezelfde wachtrij van het cloud-naar-apparaat, die maximaal 50 berichten kan bevatten. Betekent dit dat het patroon ondoorzichtige gateway moet worden gebruikt wanneer heel weinig apparaten zijn verbonden via de veldgateway voor elk en hun cloud-naar-apparaat verkeer laag is.

## <a name="next-steps"></a>Volgende stappen
Gebruik een IoT Edge-apparaat als een [transparante gateway][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway-linux.md
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md

[1]: ./media/iot-edge-as-gateway/edge-as-gateway.png
