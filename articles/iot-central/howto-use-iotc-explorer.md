---
title: Apparaat-connectiviteit controleren met behulp van Azure IoT Central Explorer
description: Bewaken van apparaat-berichten en bekijk device twin wijzigingen via de IoT Central Explorer CLI.
author: viv-liu
ms.author: viviali
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 39359054818b81c0f5f2cdc9413ae2a0c1dc8c65
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57310264"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Apparaat-connectiviteit controleren met behulp van Azure IoT Central Explorer

*In dit onderwerp is van toepassing op builders en beheerders.*

Zie uw apparaten bij IoT Central verzenden en houd rekening met wijzigingen op het dubbele IoT Hub-berichten met behulp van IoT Central Explorer CLI. Deze open-source-hulpprogramma kunt u beter inzicht krijgen in de status van de connectiviteit van apparaten en problemen van apparaat-berichten niet bereiken van de cloud of niet reageert op wijzigingen van de dubbele apparaten.

## <a name="visit-the-iotc-explorer-repo-in-githubhttpsakamsiotciotcexplorercligithub"></a>[Ga naar de iotc-explorer-opslagplaats in GitHub](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Vereisten
+ Node.js versie 8.x of hoger: https://nodejs.org
+ U moet een beheerder van uw app voor het genereren van een toegangstoken voor gebruik in iotc-explorer ophalen

## <a name="installing-iotc-explorer"></a>Iotc explorer installeren

Voer de volgende opdracht uit vanaf de opdrachtregel om te installeren:

```
npm install -g iotc-explorer
```

> [!NOTE]
> Moet u meestal om uit te voeren van de installatieopdracht met `sudo` in Unix-achtige omgevingen.

## <a name="running-iotc-explorer"></a>Actieve iotc-explorer

Hieronder volgen enkele opdrachten en de algemene opties die u uitvoeren kunt wanneer u `iotc-explorer`. Als u de volledige set opdrachten en opties, u kunt doorgeven `--help` naar `iotc-explorer` of een van de subopdrachten.

### <a name="login"></a>Aanmelden

Voordat u aan de slag gaat, moet u een beheerder van uw IoT Central-toepassing in een toegangstoken voor gebruik. De beheerder heeft de volgende stappen uit:
1. Ga naar **beheer/toegangstokens**. 
1. Selecteer **genereren**.
![Schermafbeelding van pagina-token toegang](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Voer de Tokennaam van een, selecteert u **volgende**, en **Kopieer de waarde van het Token**.
    > [!NOTE]
    > De waarde voor het token wordt alleen slechts één keer weergegeven, zodat deze moet worden gekopieerd voordat u het dialoogvenster te sluiten. Nadat u het dialoogvenster te sluiten, wordt er nooit opnieuw wordt weergegeven.

    ![Schermafbeelding van dialoogvenster-token toegang kopiëren](media/howto-use-iotc-explorer/copyaccesstoken.png)

U kunt dit token vervolgens gebruiken voor aanmelding bij de CLI door uit te voeren:

```sh
iotc-explorer login "<Token value>"
```

Als u niet hebt liever het token wordt bewaard in de geschiedenis van uw shell, kunt u het token out en deze in plaats daarvan opgeven wanneer u hierom wordt gevraagd:

```
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Monitor voor apparaat-berichten

U kunt controleren of de berichten die afkomstig zijn van een specifiek apparaat of alle apparaten in uw toepassing met de `monitor-messages` opdracht. Hiermee wordt een watcher wordt voortdurend nieuwe berichten uitvoer wanneer deze worden gestart.

Als u wilt bekijken van alle apparaten in uw toepassing, moet u de volgende opdracht uitvoeren:

```
iotc-explorer monitor-messages
```
De uitvoer: ![opdrachtuitvoer monitor-berichten](media/howto-use-iotc-explorer/monitormessages.PNG)

Als u wilt een specifiek apparaat bekijken, moet u alleen de apparaat-ID toevoegen aan het einde van de opdracht:

```
iotc-explorer monitor-messages <your-device-id>
```

U kunt ook de uitvoer van een machine geschikte indeling door toe te voegen van de opdracht hebben de `--raw` optie met de opdracht:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Ophalen van apparaatdubbel

U kunt de `get-twin` opdracht voor het ophalen van de inhoud van het dubbele voor een apparaat met IoT Central. Voer de volgende opdracht uit om dit te doen:

```
iotc-explorer get-twin <your-device-id>
```

De uitvoer: ![uitvoer van de opdracht get-twin](media/howto-use-iotc-explorer/getdevicetwin.PNG)

Net als bij `monitor-messages`, krijgt u een meer machine-vriendelijk uitvoer door door te geven de `--raw` optie:

```
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u met de IoT Central Explorer, de voorgestelde volgende stap is om te verkennen [beheer van apparaten IoT Central](howto-manage-devices.md).
