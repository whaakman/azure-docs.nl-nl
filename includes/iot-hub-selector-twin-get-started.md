---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: eef073f5f4d1eb39fd5ccd8dafacd7074158fa37
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667858"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub persistent voor elk apparaat dat verbinding maakt met het apparaat.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Apparaat apparaatdubbels gebruiken voor het volgende:

* Meta gegevens van het apparaat opslaan vanuit de back-end van uw oplossing.

* Rapporteert de huidige status informatie, zoals de beschik bare mogelijkheden en voor waarden (bijvoorbeeld de gebruikte verbindings methode) vanuit uw apparaat-app.

* Synchroniseer de status van langlopende werk stromen (zoals firmware en configuratie-updates) tussen een apparaat-app en een back-end-app.

* Query's uitvoeren op de meta gegevens, configuratie of status van uw apparaat.

Apparaatdubbels zijn ontworpen voor synchronisatie en voor het uitvoeren van query's op de configuraties en voor waarden van apparaten. Meer informatie over het gebruik van apparaatdubbels vindt u in inzicht in de apparaatdubbels van het [apparaat](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Apparaatdubbels van apparaten worden opgeslagen in een IoT-hub en bevatten:

* *Tags*, meta gegevens van apparaten die alleen toegankelijk zijn voor de back-end van de oplossing;

* *gewenste eigenschappen*, JSON-objecten die worden gewijzigd door de back-end van de oplossing en kunnen worden weer gegeven door de apparaat-app. maar

* *gerapporteerde eigenschappen*, JSON-objecten die worden gewijzigd door de apparaat-app en die kunnen worden gelezen door de back-end van de oplossing. Tags en eigenschappen kunnen geen matrices bevatten, maar objecten kunnen worden genest.

![Dubbele afbeelding van apparaat met functionaliteit](./media/iot-hub-selector-twin-get-started/twin.png)

Daarnaast kan de back-end van de oplossing het apparaat apparaatdubbels opvragen op basis van alle bovenstaande gegevens.
Raadpleeg [device apparaatdubbels](../articles/iot-hub/iot-hub-devguide-device-twins.md) voor meer informatie over de apparaatdubbels van het apparaat en de referentie voor de [IOT hub-query taal](../articles/iot-hub/iot-hub-devguide-query-language.md) voor het uitvoeren van query's.


In deze zelfstudie ontdekt u hoe u:

* Maak een back-end-app waarmee *Tags* worden toegevoegd aan een apparaat, en een gesimuleerde apparaat-app die het verbindings kanaal rapporteert als een gerapporteerde *eigenschap* op het apparaat dubbele.

* Zoek apparaten uit uw back-end-app met behulp van filters voor de tags en eigenschappen die eerder zijn gemaakt.
