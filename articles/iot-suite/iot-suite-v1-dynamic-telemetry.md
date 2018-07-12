---
title: Dynamische telemetrie gebruiken | Microsoft Docs
description: Volg deze zelfstudie voor informatie over het dynamische telemetrie gebruiken bij de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 60e9ee00fabf15a62e782c70bca251b1a8e617c3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723999"
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynamische telemetrie gebruiken bij de vooraf geconfigureerde oplossing voor externe controle

Dynamische telemetrie kunt u voor het visualiseren van alle telemetrie die verzonden naar de vooraf geconfigureerde oplossing voor externe controle. De gesimuleerde apparaten die met de vooraf geconfigureerde oplossing implementeert verzenden temperatuur en vochtigheid telemetrie die u op het dashboard kunt visualiseren. Als u bestaande gesimuleerde apparaten aanpassen, nieuwe gesimuleerde apparaten maken of fysieke apparaten verbinden met de vooraf geconfigureerde oplossing kunt u andere telemetriewaarden, zoals de externe temperatuur, RPM of windsnelheid verzenden. Vervolgens kunt u deze extra telemetrie op het dashboard visualiseren.

In deze zelfstudie wordt een eenvoudige gesimuleerd apparaat voor Node.js, die u eenvoudig aanpassen kunt om te experimenteren met dynamische telemetrie.

Voor deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-abonnement. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
* [Node.js] [ lnk-node] versie 0.12.x of hoger.

U kunt voltooien van deze zelfstudie op elk besturingssysteem, zoals Windows of Linux, waar u Node.js kunt installeren.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Een telemetrietype toevoegen

De volgende stap wordt vervangen door de telemetrie die is gegenereerd door de Node.js gesimuleerd apparaat met een nieuwe set waarden:

1. Het gesimuleerde apparaat met Node.js stoppen door te typen **Ctrl + C** in het opdrachtprompt of een shell.
2. In het bestand remote_monitoring.js ziet u de waarden van de basis voor de bestaande temperatuur, vochtigheid en externe temperatuurtelemetrie. Voeg een waarde basisgegevens voor **rpm** als volgt:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. De Node.js gesimuleerd apparaat maakt gebruik van de **generateRandomIncrement** functie in het bestand remote_monitoring.js een willekeurige verhoging toevoegen aan de basis-waarden. Een willekeurige kleur geven de **rpm** waarde door het toevoegen van een regel code na de bestaande randomizations als volgt:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. De nieuwe rpm-waarde toevoegen aan de JSON-nettolading die het apparaat naar IoT Hub verzendt:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Het Node.js gesimuleerde apparaat met de volgende opdracht uitvoeren:

    `node remote_monitoring.js`

6. Houd rekening met de nieuwe RPM-telemetrietype die op de grafiek in het dashboard wordt weergegeven:

![RPM aan het dashboard toevoegen][image3]

> [!NOTE]
> U moet mogelijk om te schakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk weergegeven.

## <a name="customize-the-dashboard-display"></a>De dashboardweergave aanpassen

De **apparaatgegevens** berichten kan metagegevens bevatten over de telemetrie die het apparaat naar IoT Hub verzenden kunt. Deze metagegevens kunt opgeven welke telemetrietypen is die het apparaat verzendt. Wijzig de **deviceMetaData** waarde in het bestand remote_monitoring.js om op te nemen een **telemetrie** volgende definitie van de **opdrachten** definitie. De volgende code codefragment bevat de **opdrachten** definitie (Zorg ervoor dat u toevoegt een `,` nadat de **opdrachten** definitie):

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
> De oplossing voor externe bewaking maakt gebruik van een niet-hoofdlettergevoelige overeenkomst voor het vergelijken van de definitie van metagegevens met gegevens in de telemetriestroom.


Toevoegen van een **telemetrie** definitie zoals wordt weergegeven in het bovenstaande codefragment wordt het gedrag van het dashboard niet gewijzigd. De metagegevens kan echter ook bevatten een **DisplayName** kenmerk voor het aanpassen van de weergave in het dashboard. Update de **telemetrie** definitie van metagegevens zoals wordt weergegeven in het volgende codefragment:

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

De volgende schermafbeelding ziet u hoe deze wijziging de grafieklegenda op het dashboard wijzigt:

![De grafieklegenda aanpassen][image4]

> [!NOTE]
> U moet mogelijk om te schakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk weergegeven.

## <a name="filter-the-telemetry-types"></a>De telemetrietypen filteren

De grafiek op het dashboard wordt standaard elke gegevensreeks in de telemetriestroom. U kunt de **apparaatgegevens** metagegevens voor het onderdrukken van de weergave van specifieke telemetrietypen op de grafiek. 

Als u de grafiek ziet u de enige temperatuur en vochtigheid telemetrie, weglaten **ExternalTemperature** uit de **apparaatgegevens** **telemetrie** metagegevens als volgt te werk:

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

De **Outdoor temperatuur** niet meer weergegeven in de grafiek:

![Filter de telemetrie op het dashboard][image5]

Deze wijziging is alleen van invloed op de grafiek weergeven. De **ExternalTemperature** gegevenswaarden nog steeds worden opgeslagen en beschikbaar gesteld voor de endverwerking van een back-.

> [!NOTE]
> U moet mogelijk om te schakelen en schakel vervolgens het Node.js-apparaat op de **apparaten** pagina in het dashboard om de wijziging onmiddellijk weergegeven.

## <a name="handle-errors"></a>Afhandeling van fouten

Voor een gegevensstroom om weer te geven in het diagram, de **Type** in de **apparaatgegevens** metagegevens moet overeenkomen met het gegevenstype van de telemetriewaarden. Bijvoorbeeld, als de metagegevens van de geeft aan dat de **Type** vochtigheid gegevens is **int** en een **dubbele** is gevonden in de telemetriestroom en vervolgens de vochtigheidstelemetrie niet wordt weergegeven op de grafiek. Echter, de **vochtigheid** waarden nog steeds worden opgeslagen en beschikbaar gesteld voor de verwerking van een back-end.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gezien hoe u dynamische telemetrie gebruiken, kunt u meer informatie over hoe de gegevens van een apparaat voor het gebruik van de vooraf geconfigureerde oplossingen: [Apparaatmetagegevens de informatie in de externe controle vooraf geconfigureerde oplossing] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[image3]: media/iot-suite-v1-dynamic-telemetry/image3.png
[image4]: media/iot-suite-v1-dynamic-telemetry/image4.png
[image5]: media/iot-suite-v1-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
