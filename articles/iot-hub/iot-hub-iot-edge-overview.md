---
title: Overzicht van Azure IoT-rand | Microsoft Docs
description: Beschrijft de architectuur belangrijkste concepten in Azure IoT rand zoals gateways, modules en beleggingsmakelaars.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2017
ms.author: dobett
ms.openlocfilehash: 11f6375c319c8945b0278003ee08215715f7ac42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Azure IoT rand architectuur concepten

Voordat u alle voorbeeldcode bekijken of uw eigen IoT rand met veldgateway maken, moet u de belangrijkste concepten die onderbouwing van de architectuur van IoT rand begrijpen.

## <a name="iot-edge-modules"></a>Rand van de IoT-modules

U een gateway bouwen met Azure IoT rand door te maken en montage *IoT rand modules*. Modules gebruiken *berichten* om gegevens met elkaar uit te wisselen. Een module ontvangt een bericht, voert daarmee een bepaalde actie uit, zet het eventueel om in een nieuw bericht en publiceert dit bericht vervolgens zodat het door andere modules kan worden verwerkt. Sommige modules kunnen alleen nieuwe berichten produceren en verwerken nooit binnenkomende berichten. Een keten van modules vormt een gegevensverwerkingspijplijn waarbij elke module een transformatie uitvoert van de gegevens op één punt in die pijplijn.

![Een keten van modules in de gateway gebouwd met de Azure IoT Edge][1]

IoT-rand bevat de volgende onderdelen:

* Vooraf geschreven modules die algemene functies van de gateway uitvoeren.
* De interfaces die een ontwikkelaar kan gebruiken om aangepaste modules te schrijven.
* De infrastructuur die nodig is om een reeks modules te implementeren en uit te voeren.

De SDK biedt een abstractielaag waarmee u kunt bouwen gateways worden uitgevoerd op verschillende besturingssystemen en platforms.

![Azure IoT Edge-abstractielaag][2]

## <a name="messages"></a>Berichten

Hoewel de idee van modules die berichten aan elkaar doorgeven een handige manier is om te visualiseren hoe een gateway werkt, wordt hiermee niet nauwkeurig weergegeven wat er precies gebeurt. Een broker IoT Edge-modules gebruiken om te communiceren met elkaar. Modules voor het publiceren van berichten naar de broker (messaging patronen, zoals bus of publiceren/abonneren met) en laat de broker routeren van het bericht met de modules die zijn verbonden.

Een module gebruikt de functie **Broker_Publish** om een bericht naar de broker te publiceren. De broker levert berichten aan een module door een callbackfunctie te activeren. Een bericht bestaat uit een reeks sleutel/waarde-eigenschappen en inhoud die als een blok geheugen worden doorgegeven.

![De rol van de broker in Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a>Berichtroutering en filteren

Er zijn twee manieren om te leiden van berichten op de juiste IoT Edge-modules:

* U kunt een set koppelingen doorgeven aan de broker, zodat de broker de bron- en sink voor elke module weet.
* Een module kunt filteren op de eigenschappen van het bericht.

Een module mag alleen reageren op een bericht als het bericht is bedoeld voor die module. Koppelingen en effectief filteren bericht maakt een pijplijn bericht.

## <a name="next-steps"></a>Volgende stappen

Zie voor deze concepten die worden toegepast in een voorbeeld kunt u uitvoeren [verkennen Azure IoT rand architectuur][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
