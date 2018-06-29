---
title: Configureren en controleren van de IoT-apparaten op grote schaal met Azure IoT Hub (CLI) | Microsoft Docs
description: Azure IoT Hub automatische inrichting configuraties gebruiken voor het toewijzen van een configuratie aan meerdere apparaten
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: c12a07aabdecb070cfa99f8851f907499599a1fc
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036227"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Configureren en controleren van de IoT-apparaten op grote schaal met de Azure CLI

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel van de herhalende en complexe taken van het beheer van grote apparaat vloten via het geheel van hun levenscycli. Met automatische device management, kunt u gericht op een reeks apparaten op basis van hun eigenschappen, een gewenste configuratie definiëren en IoT-Hub apparaten bijwerken wanneer ze scope binnenkomen te laten.  Dit wordt uitgevoerd met behulp van de configuratie van een automatische inrichting, die ook u samenvatten voltooiing en naleving kunt, het samenvoegen van de greep en conflicten en implementeer configuraties in een gefaseerde benadering.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Automatische inrichting configuraties werk door een reeks apparaat horende bijwerken met de gewenste eigenschappen en rapportage een overzicht op basis van het apparaat twin gerapporteerd eigenschappen.  Dit introduceert een nieuwe klasse en een JSON-document aangeroepen een _configuratie_ die bestaat uit drie delen:

* De **doel voorwaarde** definieert het bereik van apparaat horende worden bijgewerkt. De doel-voorwaarde is opgegeven als een query op het apparaat twin tags en/of eigenschappen gerapporteerd.

* De **inhoud** bepaalt de gewenste eigenschappen moeten worden toegevoegd of bijgewerkt in het betreffende apparaat horende. De inhoud bevat een pad naar het gedeelte van de gewenste eigenschappen die moeten worden gewijzigd.

* De **metrische gegevens** definiëren van de samenvatting tellingen van verschillende configuratiestatussen zoals **geslaagd**, **In uitvoering**, en **fout**. Aangepaste metrische gegevens zijn opgegeven als de query's op apparaat twin eigenschappen gerapporteerd.  Systeem metrische gegevens zijn standaard metrische gegevens die het meten van de updatestatus twin, zoals het aantal horende apparaten die zijn gericht en het aantal horende die zijn bijgewerkt. 

## <a name="cli-prerequisites"></a>CLI-vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Implementeer apparaat horende apparaten configureren

Automatische inrichting configuraties vereisen het gebruik van horende apparaten synchroniseren tussen de cloud en apparaten.  Raadpleeg [Understand and gebruik apparaat horende in IoT-Hub] [ lnk-device-twin] voor instructies over het gebruik van horende apparaten.

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met tags

Voordat u een configuratie maken kunt, moet u opgeven welke apparaten die u wilt beïnvloeden. Azure IoT Hub identificeert-apparaten met labels in de apparaat-twin. Elk apparaat kan meerdere labels hebben, en kunt u ze een manier die zinvol is voor uw oplossing. Als u apparaten in verschillende locaties beheren, mag u de volgende codes toevoegen aan een apparaat-twin:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```
## <a name="define-the-target-content-and-metrics"></a>Definieer de doel-inhoud en metrische gegevens

De doel-inhoud en meetwaarde op query's zijn opgegeven als JSON-documenten met een beschrijving van het apparaat twin gewenste eigenschappen die moeten worden ingesteld en gerapporteerde eigenschappen die moeten worden gemeten.  Sla de doel-inhoud en metrische gegevens lokaal als txt-bestanden voor het maken van een met automatische apparaatconfiguratie met Azure CLI 2.0. U gebruikt de bestandspaden volgende verderop in wanneer u de opdracht uit om de configuratie van toepassing op uw apparaat worden uitgevoerd. 

Hier volgt een voorbeeld van een eenvoudig doel inhoud:

```json
{
  "content": {
    "deviceContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Hier volgen enkele voorbeelden van metrische query's:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

## <a name="create-a-configuration"></a>Een configuratie maken

U configureren doelapparaten door het maken van een configuratie die uit de doel-inhoud en metrische gegevens bestaat. 

Gebruik de volgende opdracht om een configuratie te maken:

   ```cli
   az iot hub configuration create --config-id [configuration id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int] --metrics [metric queries]
   ```

* **--config-id** -de naam van de configuratie die wordt gemaakt in de IoT-hub. Geef de configuratie van een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
* **--labels** -labels voor het bijhouden van uw configuratie toevoegen. Labels zijn naam, waarde-paren die uw implementatie te beschrijven. Bijvoorbeeld, `HostPlatform, Linux` of `Version, 3.0.1`
* **--inhoud** -Inline JSON of pad naar de doel-inhoud moet worden ingesteld als twin gewenst eigenschappen. 
* **--naam hub** -naam van de IoT-hub waarin de configuratie wordt gemaakt. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--doelvoorwaarden** -Geef een doel-voorwaarde om te bepalen welke apparaten met deze configuratie moeten worden toegepast. De voorwaarde is gebaseerd op het apparaat twin tags of apparaat twin gewenst eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld: `tags.environment='test'` of `properties.desired.devicemodel='4000x'`. 
* **--prioriteit** -een positief geheel getal. In het geval dat twee of meer configuraties zijn gericht op hetzelfde apparaat, geldt de configuratie met de hoogste numerieke waarde voor de prioriteit.
* **--metrische gegevens** -bestandspad naar de metrische query's. Metrische gegevens bieden een samenvatting van de aantallen voor de verschillende statussen die een apparaat rapporteren kan als gevolg van het toepassen van configuratie-inhoud. U kunt bijvoorbeeld een metriek voor in behandeling zijnde instellingen wijzigingen, een metriek op fouten en gegevens voor wijzigingen in de geslaagde maken. 

## <a name="monitor-a-configuration"></a>Een configuratie controleren

Gebruik de volgende opdracht om de inhoud van een configuratie weer te geven:

   ```cli
az iot hub configuration show --config-id [configuration id] --hub-name [hub name]
   ```
* **--config-id** -de naam van de configuratie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

Inspecteer de configuratie in het opdrachtvenster. De **metrische gegevens** eigenschappenlijsten een aantal voor elke metriek die wordt geëvalueerd door elke hub:
* **targetedCount** -systeem metric die het aantal apparaten horende in IoT-Hub die overeenkomen met de doel-voorwaarde aangeeft.
* **appliedCount** -systeem metric geeft het aantal apparaten waarvan het doel inhoud toegepast.
* **Uw aangepaste metric** -metrische gegevens die u hebt gedefinieerd wordt beschouwd als metrische gegevens over gebruikers.

U kunt een lijst met apparaat-id's of objecten voor elk van de metrische gegevens weergeven met behulp van de volgende opdracht:

   ```cli
az iot hub configuration show-metric --config-id [configuration id] --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
   ```

* **--config-id** -de naam van de implementatie die in de IoT-hub voorkomt.
* **--metriek-id** : de naam van de metrische gegevens waarvoor u een overzicht van de apparaat-id's, bijvoorbeeld wilt maken `appliedCount`
* **--naam hub** -naam van de IoT-hub die de implementatie bevat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--metriek type** -type metrische gegevens kan worden `system` of `user`.  Systeem metrische gegevens zijn `targetedCount` en `appliedCount`. Alle andere metrische gegevens zijn metrische gegevens over gebruikers.

## <a name="modify-a-configuration"></a>Een configuratie wijzigen

Wanneer u een configuratie wijzigt, worden de wijzigingen direct repliceren naar alle apparaten. 

Als u de doelvoorwaarden bijwerkt, gebeuren de volgende updates:
* Als een apparaat-twin niet voldoen aan de oude doelvoorwaarden, maar voldoet aan de nieuwe doel-voorwaarde en deze configuratie de hoogste prioriteit voor dat apparaat is, wordt deze configuratie toegepast op de apparaat-twin. 
* Als een apparaat-twin voldoet niet aan de doelvoorwaarden, de instellingen van de configuratie wordt verwijderd en de apparaat-twin wordt gewijzigd door de configuratie van de volgende hoogste prioriteit. 
* Als een apparaat twin momenteel met deze configuratie niet meer voldoet aan de voorwaarde doel en voldoet niet aan de voorwaarde van het doel van alle andere configuraties, de instellingen van de configuratie wordt verwijderd en geen andere wijzigingen worden aangebracht op de twin. 

Gebruik de volgende opdracht in een configuratie bij te werken:

   ```cli
az iot hub configuration update --config-id [configuration id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--config-id** -de naam van de configuratie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--ingesteld** -eigenschap in de configuratie bijwerken. U kunt de volgende eigenschappen bijwerken:
    * targetCondition - voorbeeld `targetCondition=tags.location.state='Oregon'`
    * labels 
    * prioriteit

## <a name="delete-a-configuration"></a>Een configuratie verwijderen

Wanneer u een configuratie verwijdert, nemen alle horende apparaten op hun volgende hoogste prioriteit-configuratie. Als horende apparaten niet voldoen aan de voorwaarde van het doel van een andere configuratie, wordt er geen andere instellingen toegepast. 

Gebruik de volgende opdracht om te verwijderen van een configuratie:

   ```cli
az iot hub configuration delete --config-id [configuration id] --hub-name [hub name] 
   ```
* **--config-id** -de naam van de configuratie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe configureren en controleren van de IoT-apparaten op schaal. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Beheren van uw IoT Hub apparaat-id's in bulk][lnk-bulkIDs]
* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

Als u wilt verkennen met behulp van de IoT Hub apparaat inrichtingsservice om in te schakelen zonder tussenkomst, Zie just-in-time-inrichting: 

* [Azure IoT Hub apparaat-Service inricht][lnk-dps]

[lnk-device-twin]: iot-hub-devguide-device-twins.md
[lnk-bulkIDs]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
[lnk-portal]: https://portal.azure.com
