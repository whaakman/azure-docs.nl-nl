---
title: Gebruik dynamische telemetrie | Microsoft Docs
description: Volg deze zelfstudie voor meer informatie over het gebruik van dynamische telemetrie met de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 60e9ee00fabf15a62e782c70bca251b1a8e617c3
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynamische telemetrie gebruiken met de vooraf geconfigureerde oplossing voor externe controle

Dynamische telemetrie kunt u telemetrie verzonden naar de vooraf geconfigureerde oplossing voor externe controle te visualiseren. De gesimuleerde apparaten die met de vooraf geconfigureerde oplossing implementeert verzenden temperatuur en vochtigheid telemetrie die u op het dashboard kunt visualiseren. Als u bestaande gesimuleerde apparaten, nieuwe gesimuleerde apparaten maken of aanpassen fysieke apparaten met de vooraf geconfigureerde oplossing verbinden kunt u andere waarden telemetrie zoals de externe temperatuur, RPM of windsnelheid verzenden. U kunt vervolgens visualiseren van deze extra telemetrie op het dashboard.

Deze zelfstudie wordt een eenvoudige Node.js gesimuleerd apparaat u eenvoudig aanpassen kunt als u wilt experimenteren met dynamische telemetrie.

Deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-abonnement. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
* [Node.js] [ lnk-node] versie 0.12.x of hoger.

U kunt voltooien van deze zelfstudie op elk besturingssysteem, zoals Windows of Linux, waar u Node.js kunt installeren.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Een type telemetrie toevoegen

De volgende stap is het vervangen van de telemetrie die is gegenereerd door het gesimuleerde apparaat Node.js met een nieuwe set met waarden:

1. Stop het gesimuleerde apparaat Node.js door te typen **Ctrl + C** in uw opdrachtprompt of de shell.
2. In het bestand remote_monitoring.js ziet u de basisgegevens waarden voor de bestaande temperatuur, vochtigheid en externe temperatuur telemetrie. Voeg een waarde basisgegevens voor **rpm** als volgt:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. De Node.js gesimuleerd apparaat maakt gebruik van de **generateRandomIncrement** functie in het bestand remote_monitoring.js een willekeurige verhoging toevoegen aan de basisgegevens waarden. Een willekeurige de **rpm** waarde door een regel code na de bestaande randomizations toe te voegen als volgt:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. De nieuwe rpm-waarde toevoegen aan de JSON-nettolading in die het apparaat naar IoT Hub verzendt:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Voer het Node.js gesimuleerde apparaat met de volgende opdracht:

    `node remote_monitoring.js`

6. Houd rekening met het nieuwe RPM telemetrie type die wordt weergegeven in de grafiek in het dashboard:

![RPM toevoegen aan het dashboard][image3]

> [!NOTE]
> U wilt uitschakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk te zien.

## <a name="customize-the-dashboard-display"></a>De dashboardweergave aanpassen

De **apparaatgegevens** bericht metagegevens over de telemetrie die het apparaat met IoT Hub verzenden kunt kan bevatten. Deze metagegevens kunt opgeven welke telemetrie die het apparaat verzendt. Wijzig de **deviceMetaData** waarde in het bestand remote_monitoring.js wilt opnemen een **telemetrie** definitie volgende de **opdrachten** definitie. De volgende code codefragment bevat de **opdrachten** definition (voegt toe een `,` nadat de **opdrachten** definitie):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> De oplossing voor externe controle gebruikt een niet-hoofdlettergevoelige overeenkomst voor de definitie van metagegevens met gegevens in de telemetriestroom vergelijken.


Toevoegen van een **telemetrie** definitie zoals weergegeven in het bovenstaande codefragment verandert het gedrag van het dashboard niet. De metagegevens kan echter ook bevatten een **DisplayName** kenmerk voor het aanpassen van de weergave in het dashboard. Update de **telemetrie** definitie van metagegevens zoals weergegeven in het volgende fragment:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

De volgende schermafbeelding ziet hoe de grafieklegenda op het dashboard Hiermee wijzigt u deze wijziging:

![De grafieklegenda aanpassen][image4]

> [!NOTE]
> U wilt uitschakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk te zien.

## <a name="filter-the-telemetry-types"></a>De typen telemetrie filteren

De grafiek in het dashboard wordt standaard elke gegevensreeks in de telemetriestroom. U kunt de **apparaatgegevens** metagegevens van de weergave van specifieke telemetrie typen op de grafiek onderdrukken. 

Als u de grafiek alleen temperatuur en vochtigheid telemetrie weergeven, weglaten **ExternalTemperature** van de **apparaatgegevens** **telemetrie** metagegevens als volgt:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

De **buiten temperatuur** niet meer weergegeven in de grafiek:

![De telemetrie op het dashboard filteren][image5]

Deze wijziging is alleen van invloed op de grafiek weergeven. De **ExternalTemperature** gegevenswaarden nog steeds worden opgeslagen en beschikbaar gesteld voor de verwerking van de back-end.

> [!NOTE]
> U wilt uitschakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk te zien.

## <a name="handle-errors"></a>Afhandelen van fouten

Voor een gegevensstroom op de grafiek moet worden weergegeven de **Type** in de **apparaatgegevens** metagegevens moet overeenkomen met het gegevenstype van de telemetrie-waarden. Bijvoorbeeld, als de metagegevens geeft aan dat de **Type** van vochtigheid gegevens is **int** en een **dubbele** is gevonden in de telemetriestroom en vervolgens de telemetrie vochtigheid niet wordt weergegeven in de grafiek. Echter, de **vochtigheid** waarden zijn nog steeds opgeslagen en beschikbaar gesteld voor de verwerking van de back-end.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gezien hoe u dynamische telemetrie gebruikt, kunt u meer informatie over hoe de gegevens van een apparaat voor het gebruiken van de vooraf geconfigureerde oplossingen: [metagegevens van apparaten informatie in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[image3]: media/iot-suite-v1-dynamic-telemetry/image3.png
[image4]: media/iot-suite-v1-dynamic-telemetry/image4.png
[image5]: media/iot-suite-v1-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
