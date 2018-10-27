---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 304637422c0b8fd4dfa99e2e434e13d12f1fb342
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164187"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub is een apparaatdubbel voor elk apparaat dat is verbonden met het.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Apparaatdubbels om te gebruiken:

* Metagegevens van apparaten uit de back-end van uw oplossing Store.

* Rapport informatie over de huidige status, zoals de beschikbare mogelijkheden en voorwaarden (bijvoorbeeld: de connectiviteit-methode gebruikt) van uw apparaat-app.

* De status van langlopende werkstromen (zoals firmware en configuratie-updates) tussen een apparaat-app en een back-end-app worden gesynchroniseerd.

* Query uitvoeren op de metagegevens van apparaten, de configuratie of de status.

Apparaatdubbels zijn ontworpen voor synchronisatie en voor het uitvoeren van query's apparaatconfiguraties en voorwaarden. Meer informatie over het gebruik van apparaatdubbels vindt u [meer informatie over apparaatdubbels](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Apparaatdubbels zijn opgeslagen in een IoT-hub en bevatten:

* *tags*, de metagegevens van de apparaten is alleen toegankelijk is voor de back-end oplossing;

* *gewenste eigenschappen*, JSON-objecten die kunnen worden gewijzigd door de oplossing back-end en zichtbaar zijn door de apparaat-app; en

* *gerapporteerde eigenschappen*, JSON-objecten kunnen worden aangepast door de apparaat-app en kan worden gelezen door de back-end van de oplossing. Labels en eigenschappen mag geen matrices bevatten, maar objecten kunnen worden genest.

![Apparaat dubbele-functionaliteit voor weergeven van afbeeldingen](./media/iot-hub-selector-twin-get-started/twin.png)

De back-end van de oplossing kan bovendien apparaatdubbels op basis van de bovenstaande gegevens opvragen.
Raadpleeg [meer informatie over apparaatdubbels](../articles/iot-hub/iot-hub-devguide-device-twins.md) voor meer informatie over apparaatdubbels, zodat de [IoT Hub-querytaal](../articles/iot-hub/iot-hub-devguide-query-language.md) verwijzing voor het uitvoeren van query's.


In deze zelfstudie ontdekt u hoe u:

* Maakt een back-end-app die wordt toegevoegd *tags* een apparaatdubbel en een gesimuleerde apparaat-app die rapporten van het kanaal verbinding als een *gerapporteerde eigenschap* op het dubbele apparaat.

* Query uitvoeren op apparaten van uw back-end-app met behulp van filters op de labels en eigenschappen van eerder hebt gemaakt.
