---
title: Aan de slag met Azure IoT rand (Linux) | Microsoft Docs
description: Kom meer te weten over het bouwen van een gateway op een Linux-computer en over belangrijke concepten in de Azure IoT Edge, zoals modules en JSON-configuratiebestanden.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb65e3c34d2b2a14370792d8506c13c8c5fb522e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Azure IoT Edge-architectuur in Linux verkennen

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Het voorbeeld uitvoeren

De uitvoer van het script **build.sh** wordt gegenereerd in de map **build** in de lokale kopie van de **iot-edge**-opslagplaats. Deze uitvoer bevat de twee IoT Edge-modules die in dit voorbeeld worden gebruikt.

Het bouwscript plaatst **liblogger.so** in de map **build/modules/logger/** en **libhello\_world.so** in de map **build/modules/hello_world/**. Deze paden voor de **pad module** waarden zoals weergegeven in het voorbeeld JSON-instellingenbestand.

De Hallo\_world\_voorbeeld wordt het pad naar een JSON-configuratiebestand als een opdrachtregelargument. Het volgende voorbeeld van een JSON-bestand maakt deel uit van de SDK-opslagplaats op **samples/hello\_world/src/hello\_world\_lin.json**. Dit configuratiebestand werkt zoals tenzij u het build script wijzigen voor de rand van de IoT-modules of voorbeeld uitvoerbare bestanden in niet-standaardlocaties plaatsen.

> [!NOTE]
> De modulepaden zijn relatief aan de huidige werkmap vanwaaruit het uitvoerbare bestand hello\_world\_sample wordt gestart, niet aan de map waarin het uitvoerbare bestand zich bevindt. Het voorbeeld-JSON-configuratiebestand schrijft standaard 'log.txt' in uw huidige werkmap.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Navigeer naar de **bouwen** map in de hoofdmap van uw lokale exemplaar van de **iot-edge** opslagplaats.

1. Voer de volgende opdracht uit:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

