---
title: Azure IoT device management met IoT-extensie voor Azure CLI 2.0 | Microsoft Docs
description: Gebruik de IoT-extensie voor Azure CLI 2.0-hulpprogramma voor Azure IoT Hub Apparaatbeheer, met daarin de rechtstreekse methoden en de Twin eigenschappen van de gewenste opties.
services: iot-hub
documentationcenter: ''
author: chrissie926
manager: timlt
tags: ''
keywords: Azure iot-Apparaatbeheer, Apparaatbeheer via azure iot hub, apparaat management iot, Apparaatbeheer via iot hub
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 27a8dca68000e3f4b7f33508fc6dbc3450fc80b4
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>De IoT-uitbreiding voor Azure CLI 2.0 wordt gebruikt voor het beheer van Azure IoT Hub-apparaten

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[De IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) is een nieuwe open-source IoT-extensie die wordt toegevoegd aan de mogelijkheden van [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Azure CLI 2.0 bevat opdrachten voor interactie met Azure resource manager en beheer-eindpunten. U kunt bijvoorbeeld Azure CLI 2.0 gebruiken om een virtuele machine in Azure of een IoT-hub te maken. Extensie van de CLI kunt een Azure-service naar de Azure CLI zodat die u toegang tot extra servicespecifieke mogelijkheden verbeteren. De extensie IoT biedt IoT ontwikkelaars opdrachtregel toegang tot alle IoT Hub, IoT-rand en IoT Hub apparaat-inrichtingsservice mogelijkheden.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Beheeroptie          | Taak                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Directe methoden             | Een apparaat fungeren zoals starten of stoppen van verzenden van berichten of het apparaat opnieuw maken.                                        |
| Eigenschappen van de gewenste Twin    | Een apparaat in een bepaalde status, zoals een LED in groen instellen of het instellen van het interval voor het verzenden van telemetrie tot 30 minuten geplaatst.         |
| Dubbele eigenschappen gerapporteerd   | Ophalen van de gerapporteerde status van een apparaat. Bijvoorbeeld, meldt het apparaat dat de LED nu knippert.                                    |
| Labels Twin                  | Apparaatspecifieke metagegevens niet opslaan in de cloud. Bijvoorbeeld, de implementatielocatie van een snoep-machine.                         |
| Apparaat twin query 's        | Query uitvoeren op alle apparaten horende om op te halen die met een willekeurige voorwaarden, zoals de apparaten die gebruikt worden om te identificeren. |

Zie voor meer gedetailleerde uitleg over de verschillen en richtlijnen over het gebruik van deze opties, [apparaat-naar-cloud communicatie richtlijnen](iot-hub-devguide-d2c-guidance.md) en [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md).

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub persistente een apparaat twin voor elk apparaat dat verbinding met het maakt. Zie voor meer informatie over apparaat horende [aan de slag met apparaat horende](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Wat u leert

U meer informatie over het gebruik van de IoT-extensie voor Azure CLI 2.0 met verschillende beheeropties op uw ontwikkelcomputer.

## <a name="what-you-do"></a>Wat u doet

Azure CLI 2.0 en de IoT-extensie voor Azure CLI 2.0 uitvoeren met verschillende opties.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure-IoT-hub in uw abonnement.
  - Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.

- Zorg ervoor dat uw apparaat wordt uitgevoerd met de clienttoepassing tijdens deze zelfstudie.

- [Python 2.7 x of Python 3.x](https://www.python.org/downloads/)

- Installeer Azure CLI 2.0. Een eenvoudige manier om Azure CLI 2.0 te installeren in Windows is de [MSI](https://aka.ms/InstallAzureCliWindows) te downloaden en installeren. U kunt ook de installatie-instructies volgen op [Microsoft Docs](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor het instellen van Azure CLI 2.0 in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –version` om de versie te valideren. 

- De IoT-uitbreiding te installeren. De eenvoudigste manier is `az extension add --name azure-cli-iot-ext` uit te voeren. [In het Leesmij-bestand bij de IoT-extensie](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) worden verschillende manieren voor het installeren van de extensie beschreven.


## <a name="log-in-to-your-azure-account"></a>Meld u aan bij uw Azure-account

Meld u aan bij uw Azure-account met de volgende opdracht:

```bash
az login
```

## <a name="direct-methods"></a>Directe methoden

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Apparaateigenschappen gewenst twin

Instellen van een interval van de gewenste eigenschap 3000 = met de volgende opdracht:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Deze eigenschap kan worden gelezen vanaf uw apparaat.

## <a name="device-twin-reported-properties"></a>Apparaat-twin gerapporteerd eigenschappen

Haal de gerapporteerde eigenschappen van het apparaat met de volgende opdracht:

```bash
az iot hub device-twin show -n <your hub name> -d <your device id>
```

Een van de dubbele eigenschappen $metadata is gerapporteerd. $lastUpdated waarin de laatste keer dat de app apparaat bijgewerkt de gerapporteerde eigenschap is ingesteld.

## <a name="device-twin-tags"></a>Apparaat twin labels

De labels en eigenschappen van het apparaat weergegeven door met de volgende opdracht:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Voeg een veld rol = temperatuur en vochtigheid aan het apparaat met de volgende opdracht:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Apparaat twin query 's

Query uitvoeren op apparaten met een label van de rol = 'temperatuur en vochtigheid' door de volgende opdracht uit te voeren:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Query uitvoeren op alle apparaten, behalve die met een label van de rol = 'temperatuur en vochtigheid' door de volgende opdracht uit te voeren:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u apparaat-naar-cloud-berichten worden gecontroleerd en cloud-naar-apparaat-berichten verzenden tussen uw IoT-apparaat en de Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
