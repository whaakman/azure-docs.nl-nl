---
title: Azure IoT-Apparaatbeheer met IoT-extensie voor Azure CLI | Microsoft Docs
description: Gebruik de IoT-extensie voor Azure CLI-hulpprogramma voor Azure IoT Hub-Apparaatbeheer, met daarin de directe methoden en beheeropties van de Apparaatdubbel-gewenste eigenschappen.
author: chrissie926
manager: ''
keywords: Azure iot-Apparaatbeheer, Apparaatbeheer via azure iot hub, apparaat management iot, iot hub-Apparaatbeheer
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: c5d4299d2d391a25bfab579f5f78da3718afc24c
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042996"
---
# <a name="use-the-iot-extension-for-azure-cli-for-azure-iot-hub-device-management"></a>Gebruik de IoT-extensie voor Azure CLI voor Azure IoT Hub-Apparaatbeheer

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[De IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) is een nieuw open source IoT-extensie die wordt toegevoegd aan de mogelijkheden van de [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). De Azure CLI bevat opdrachten voor interactie met Azure resourcemanager- en eindpunten voor beheer. Bijvoorbeeld, kunt u Azure CLI om een Azure-VM of een IoT-hub te maken. Een CLI-extensie kunt een Azure-service om te verbeteren van de Azure CLI geven die u toegang tot aanvullende service-specifieke mogelijkheden. De IoT-extensie biedt ontwikkelaars IoT vanaf de opdrachtregel toegang tot alle IoT Hub, IoT Edge en IoT Hub Device Provisioning Service-mogelijkheden.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Management-optie          | Taak  |
|----------------------------|-----------|
| Directe methoden             | Een apparaat fungeren, zoals starten of stoppen van berichten verzenden of opnieuw opstarten van het apparaat maken.                                        |
| Apparaatdubbel-gewenste eigenschappen    | Een apparaat in een bepaalde status, zoals een LED groen instellen of het instellen van het interval voor het verzenden van telemetrie tot 30 minuten plaatsen.         |
| Gerapporteerde eigenschappen   | Ophalen van de gerapporteerde status van een apparaat. Bijvoorbeeld, meldt het apparaat dat de LED nu knippert.                                    |
| Apparaatdubbel-tags                  | Store apparaatspecifieke metagegevens in de cloud. Bijvoorbeeld, de implementatielocatie van een Verkoopautomaat.                         |
| Apparaatdubbel-query 's        | Query uitvoeren op alle apparaatdubbels om op te halen die met willekeurige voorwaarden, zoals het identificeren van de apparaten die beschikbaar voor gebruik zijn. |

Zie voor meer gedetailleerde uitleg over de verschillen en richtlijnen over het gebruik van deze opties, [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) en [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md).

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub is een apparaatdubbel voor elk apparaat dat is verbonden met het. Zie voor meer informatie over apparaatdubbels [aan de slag met apparaatdubbels](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Wat u leert

Leert u de IoT-extensie voor Azure CLI gebruikt met verschillende beheeropties voor op uw ontwikkelcomputer.

## <a name="what-you-do"></a>Wat u allemaal doen

Azure CLI en de IoT-extensie voor Azure CLI uitvoeren met verschillende opties.

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) die voorziet in de volgende vereisten:

  - Een actief Azure-abonnement.
  - Een Azure IoT-hub in uw abonnement.
  - Een clienttoepassing die berichten naar uw Azure-IoT-hub verzendt.

* Zorg ervoor dat uw apparaat wordt uitgevoerd met de clienttoepassing tijdens deze zelfstudie.

* [Python 2.7 x of Python 3.x](https://www.python.org/downloads/)

* De Azure CLI. Als u nodig hebt om deze te installeren, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Uw Azure CLI-versie moet ten minste 2.0.24 of hoger. Gebruik `az –version` om de versie te valideren. 

* De IoT-extensie installeren. De eenvoudigste manier is `az extension add --name azure-cli-iot-ext` uit te voeren. [In het Leesmij-bestand bij de IoT-extensie](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) worden verschillende manieren voor het installeren van de extensie beschreven.

## <a name="log-in-to-your-azure-account"></a>Meld u aan bij uw Azure-account

Meld u aan uw Azure-account met de volgende opdracht:

```bash
az login
```

## <a name="direct-methods"></a>Directe methoden

```bash
az iot hub invoke-device-method --device-id <your device id> \
  --hub-name <your hub name> \
  --method-name <the method name> \
  --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Gewenste apparaatdubbeleigenschappen

Stel een gewenste eigenschap interval = 3000 door het uitvoeren van de volgende opdracht uit:

```bash
az iot hub device-twin update -n <your hub name> \
  -d <your device id> --set properties.desired.interval = 3000
```

Deze eigenschap kan worden gelezen vanaf uw apparaat.

## <a name="device-twin-reported-properties"></a>Apparaatdubbel-gerapporteerde eigenschappen

Ophalen van de gerapporteerde eigenschappen van het apparaat met de volgende opdracht:

```bash
az iot hub device-twin show -n <your hub name> -d <your device id>
```

Een van de dubbele eigenschappen $metadata is gerapporteerd. $lastUpdated waarin de laatste keer dat de apparaat-app bijgewerkt de gerapporteerde eigenschap is ingesteld.

## <a name="device-twin-tags"></a>Apparaat apparaatdubbel-tags

De labels en eigenschappen van het apparaat weergegeven door de volgende opdracht uit:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

De rol van een veld toevoegen = temperatuur en vochtigheid aan het apparaat met de volgende opdracht:

```bash
az iot hub device-twin update \
  --hub-name <your hub name> \
  --device-id <your device id> \
  --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Apparaatdubbel-query 's

Query uitvoeren op apparaten met een code van de rol = 'temperatuur en vochtigheid' door het uitvoeren van de volgende opdracht uit:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Query uitvoeren op alle apparaten, behalve die met een code van de rol = 'temperatuur en vochtigheid' door het uitvoeren van de volgende opdracht uit:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u bewaken van apparaat-naar-cloud-berichten en cloud-naar-apparaat-berichten tussen uw IoT-apparaat en Azure IoT Hub verzendt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
