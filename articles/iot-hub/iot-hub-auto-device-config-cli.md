---
title: Configureren en controleren van IoT-apparaten op schaal met Azure IoT Hub (CLI) | Microsoft Docs
description: Azure IoT Hub automatische inrichting configuraties gebruiken voor het toewijzen van een configuratie op meerdere apparaten
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: f81ef3c231874f314d6fe023ba247a0bcff61e90
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42057510"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Configureren en controleren van IoT-apparaten op schaal met behulp van de Azure CLI

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van grote apparaat vloten via het geheel van de levenscycli hiervan te beheren. Met automatische device management, kunt u richten op een set met apparaten op basis van hun eigenschappen, definieert een gewenste configuratie en IoT Hub-apparaten bijgewerkt wanneer ze scope binnenkomen te laten.  Dit wordt uitgevoerd met behulp van de configuratie van een automatische inrichting, die ook u samenvatten is voltooid en naleving kunt, ingang samenvoegen en conflicten en configuraties in een gefaseerde benadering worden uitgerold.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Automatische inrichting configuraties werk door een set dubbele apparaten bijwerken met de gewenste eigenschappen en rapportage een overzicht op basis van de apparaatdubbel gerapporteerde eigenschappen.  Dit introduceert een nieuwe klasse en JSON-document met de naam een *configuratie* die bestaat uit drie delen:

* De **voorwaarde als doel** definieert het bereik van apparaatdubbels om te worden bijgewerkt. De doelvoorwaarde is opgegeven als een query op apparaat apparaatdubbel-tags en/of gerapporteerde eigenschappen.

* De **inhoud richten** bepaalt de gewenste eigenschappen om te worden toegevoegd of bijgewerkt in de betreffende apparaatdubbels. De inhoud bevat een pad naar de sectie van de gewenste eigenschappen die moeten worden gewijzigd.

* De **metrische gegevens** definiëren de samenvatting tellingen van verschillende configuratiestatussen zoals **succes**, **In voortgang**, en **fout**. Aangepaste metrische gegevens zijn opgegeven als query's op apparaat gerapporteerde eigenschappen.  Metrische systeemmeetgegevens zijn standaard metrische gegevens die het meten van de status van het dubbele bijwerken, zoals het aantal dubbele apparaten die zijn gericht en het aantal dubbele die zijn bijgewerkt. 

## <a name="cli-prerequisites"></a>CLI-vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Apparaatdubbels implementeren om apparaten te configureren

Automatische inrichting configuraties vereisen het gebruik van apparaatdubbels om te synchroniseren tussen de cloud en apparaten.  Raadpleeg [apparaatdubbels begrijpen en gebruiken in IoT Hub](iot-hub-devguide-device-twins.md) voor informatie over het gebruik van apparaatdubbels.

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met behulp van tags

Voordat u een configuratie maken kunt, moet u opgeven welke apparaten die u wilt toepassen. Azure IoT Hub identificeert-apparaten met behulp van tags in de apparaatdubbel. Elk apparaat kan meerdere labels, en u ze een manier die zinvol is voor uw oplossing kunt definiëren. Als u apparaten in verschillende locaties beheert, kan u bijvoorbeeld de volgende codes toevoegen aan een apparaatdubbel:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="define-the-target-content-and-metrics"></a>De inhoud van de doel- en metrische gegevens definiëren

De doel-inhoud en metrische query's zijn opgegeven als JSON-documenten die worden beschreven van het apparaat twin gewenste eigenschappen die moeten worden ingesteld en gerapporteerde eigenschappen die moeten worden gemeten.  Sla de inhoud van de doel- en metrische gegevens lokaal als txt-bestanden voor het maken van een automatische apparaatconfiguratie met behulp van Azure CLI 2.0. U gebruikt de bestandspaden in een volgende sectie van de volgende wanneer u de opdracht uit om de configuratie van toepassing op het apparaat uitvoert. 

Hier volgt een voorbeeld van basic target-inhoud:

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
   az iot hub configuration create --config-id [configuration id] \
     --labels [labels] --content [file path] --hub-name [hub name] \
     --target-condition [target query] --priority [int] \
     --metrics [metric queries]
```

* --**configuratie-id** -de naam van de configuratie die in de IoT hub wordt gemaakt. Geef uw configuratie van een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.

* --**labels** -labels voor het bijhouden van uw configuratie toevoegen. Labels zijn naam, waarde-paren die uw implementatie te beschrijven. Bijvoorbeeld, `HostPlatform, Linux` of `Version, 3.0.1`

* --**inhoud** -Inline JSON of bestand pad naar de doel-inhoud moet worden ingesteld als dubbel gewenste eigenschappen. 

* --**naam van de hub** -naam van de IoT-hub waarin de configuratie wordt gemaakt. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

* --**doelvoorwaarde** -Geef een doelvoorwaarde om te bepalen welke apparaten wordt met deze configuratie worden toegepast. De voorwaarde is gebaseerd op het apparaat apparaatdubbel-tags of apparaatdubbel gewenste eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld, `tags.environment='test'` of `properties.desired.devicemodel='4000x'`. 

* --**prioriteit** -een positief geheel getal zijn. In het geval dat twee of meer configuraties zijn gericht op hetzelfde apparaat, geldt de configuratie met de hoogste numerieke waarde voor de prioriteit.

* --**metrische gegevens** -bestandspad naar de metrische query's. Metrische gegevens bieden samenvatting tellingen van de verschillende statussen aan die een apparaat rapporteren kan als gevolg van het toepassen van configuratie-inhoud. U kunt bijvoorbeeld een metrische waarde voor in behandeling zijnde wijzigingen, een metrische waarde voor de fouten en metrische gegevens voor wijzigingen in de geslaagde maken. 

## <a name="monitor-a-configuration"></a>Een configuratie controleren

Gebruik de volgende opdracht om de inhoud van een configuratie weer te geven:

```cli
az iot hub configuration show --config-id [configuration id] \
  --hub-name [hub name]
```

* --**configuratie-id** -de naam van de configuratie die deel uitmaakt van de IoT-hub.

* --**naam van de hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

Inspecteer de configuratie in het opdrachtvenster. De **metrische gegevens** eigenschap bevat een aantal voor elke metrische gegevens die wordt geëvalueerd door elke hub:

* **targetedCount** -een systeem-metric die Hiermee geeft u het aantal dubbele apparaten in IoT-Hub die overeenkomen met de doelitems voorwaarde.

* **appliedCount** -een systeem metrische waarde geeft het aantal apparaten waarvoor het doel inhoud toegepast.

* **Uw aangepaste metrische gegevens** -alle metrische gegevens die u hebt gedefinieerd wordt beschouwd als metrische gegevens over gebruikers.

U kunt een lijst van apparaat-id's of objecten voor elk van de metrische gegevens weergeven met behulp van de volgende opdracht uit:

```cli
az iot hub configuration show-metric --config-id [configuration id] \
   --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
```

* --**configuratie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.

* --**metrische gegevens-id** : de naam van de metrische gegevens waarvoor u wilt bijvoorbeeld de lijst met apparaat-id's, Zie `appliedCount`.

* --**naam van de hub** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`.

* --**metrische gegevens van het type** -type van metrische gegevens kan worden `system` of `user`.  Metrische systeemmeetgegevens zijn `targetedCount` en `appliedCount`. Alle andere metrische gegevens zijn metrische gegevens over gebruikers.

## <a name="modify-a-configuration"></a>Een configuratie wijzigen

Wanneer u een configuratie wijzigt, worden de wijzigingen onmiddellijk gerepliceerd naar alle apparaten uit de doelgroep. 

Als u de doelvoorwaarde bijwerkt, gebeuren de volgende updates:

* Als een apparaatdubbel is niet voldoen aan de oude doelvoorwaarde, maar voldoet aan de nieuwe doelvoorwaarde en deze configuratie de hoogste prioriteit voor die apparaatdubbel is, wordt deze configuratie toegepast op het dubbele apparaat. 

* Als een apparaatdubbel is niet meer voldoet aan de doelvoorwaarde, de instellingen van de configuratie wordt verwijderd en het dubbele apparaat wordt gewijzigd door de configuratie van de volgende hoogste prioriteit. 

* Als een apparaatdubbel momenteel met deze configuratie niet meer voldoet aan de doelvoorwaarde en niet voldoet aan de doelvoorwaarde van alle andere configuraties, klikt u vervolgens de instellingen van de configuratie wordt verwijderd en geen andere wijzigingen worden aangebracht op het dubbele. 

Gebruik de volgende opdracht in een configuratie bij te werken:

```cli
az iot hub configuration update --config-id [configuration id] \
   --hub-name [hub name] --set [property1.property2='value']
```

* --**configuratie-id** -de naam van de configuratie die deel uitmaakt van de IoT-hub.

* --**naam van de hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`.

* --**Stel** -een eigenschap in de configuratie bijwerken. U kunt de volgende eigenschappen bijwerken:

    * targetCondition - voorbeeld `targetCondition=tags.location.state='Oregon'`

    * labels 

    * prioriteit

## <a name="delete-a-configuration"></a>Een configuratie verwijderen

Wanneer u een configuratie verwijdert, worden alle dubbele apparaten op de volgende configuratie met hoogste prioriteit. Als u dubbele apparaten niet voldoen aan de doelvoorwaarde van een andere configuratie, wordt er geen andere instellingen toegepast. 

Gebruik de volgende opdracht uit om te verwijderen van een configuratie:

```cli
az iot hub configuration delete --config-id [configuration id] \
   --hub-name [hub name] 
```
* --**configuratie-id** -de naam van de configuratie die deel uitmaakt van de IoT-hub.

* --**naam van de hub** -naam van de IoT-hub waarin de configuratie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe configureren en controleren van IoT-apparaten op schaal. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Uw IoT-Hub apparaatidentiteiten bulksgewijs beheren](iot-hub-bulk-identity-mgmt.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

Om te verkennen met behulp van de IoT Hub Device Provisioning Service inschakelen zero-touch, just-in-time inrichting, Zie: 

* [Azure IoT Hub Device Provisioning Service](/azure/iot-dps)
