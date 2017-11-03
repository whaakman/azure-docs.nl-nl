---
title: Aan de slag met Azure IoT rand (Windows) | Microsoft Docs
description: Het maken van een Azure-IoT-Edge-gateway op een Windows-machine en meer informatie over belangrijke concepten in Azure IoT rand zoals modules en JSON-configuratiebestanden.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 262963250921ceb6f1a2fed0f68c36a9f052b47b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Verken rand van Azure IoT-architectuur in Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

De **build.cmd** script genereert de uitvoer ervan weergegeven in de **bouwen** map in de lokale kopie van de **iot-edge** opslagplaats. Deze uitvoer bevat veel bestanden, maar dit voorbeeld is gericht op drie:
- Twee IoT Edge-modules: **bouwen\\modules\\berichtenlogboek\\Debug\\logger.dll** en **bouwen\\modules\\hello_world\\ Fouten opsporen in\\hello\_world.dll**. 
- Een uitvoerbaar bestand: **bouwen\\voorbeelden\\hello\_world\\Debug\\hello\_world\_sample.exe**. Dit proces duurt een JSON-configuratiebestand als een opdrachtregelargument.

De vierde bestand dat u in dit voorbeeld gebruikt bevindt zich niet in de build-map, maar is opgenomen wanneer u deze iot-edge opslagplaats gekloond:
- Een JSON-configuratiebestand: **voorbeelden\\hello\_world\\src\\hello\_world\_win.json**. Dit bestand bevat de paden naar de twee modules. Ook wordt aangegeven waar logger.dll schrijft naar de uitvoer ervan weergegeven. De standaardwaarde is **log.txt** in de huidige werkmap. 

   >[!NOTE]
   >Als u de voorbeeld-modules verplaatsen of uw eigen voor tests wilt toevoegen, werkt de **module.path** waarden in het configuratiebestand moet worden gezocht. De module-paden zijn ten opzichte van de map waar **hello\_world\_sample.exe** zich bevindt. 

Als u wilt uitvoeren in het voorbeeld, de volgende stappen uit:

1. Navigeer naar de **bouwen** map in de hoofdmap van uw lokale exemplaar van de **iot-edge** opslagplaats.

1. Voer de volgende opdracht uit:

   ```cmd
   samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
   ```

1. De volgende uitvoer betekent dat het voorbeeld correct wordt uitgevoerd:

   ```cmd
   gateway successfully created from JSON
   gateway shall run until ENTER is pressed
   ```
  
1. Druk op de **Enter** sleutel om het proces te stoppen.


[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
