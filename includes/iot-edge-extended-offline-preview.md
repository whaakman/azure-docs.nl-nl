---
title: bestand opnemen
description: IOT edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: be4d82577584e83e29f2511d51256fda0970e917
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263937"
---
## <a name="enabling-extended-offline-operation-preview"></a>Inschakelen van uitgebreide offline bewerking (Preview)
Met de [gebruikgemaakt van v1.0.4 release](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) van de Edge-Runtime, de Edge-apparaat en downstream-verbinding te maken met deze apparaten kunnen worden geconfigureerd voor uitgebreide offline bewerking. 

Met deze mogelijkheid kunnen lokale modules of downstream-apparaten opnieuw worden geverifieerd met het Edge-apparaat, indien nodig en communiceren met elkaar met behulp van berichten en -methoden, zelfs wanneer de IoT-Hub is verbroken. Raadpleeg deze [blogbericht](https://aka.ms/iot-edge-offline) en [concept artikel](../articles/iot-edge/offline-capabilities.md) voor meer details en het bereik van deze mogelijkheid.

Voor het inschakelen van uitgebreide offline in een gatewayscenario voor tot stand brengen van een bovenliggende / onderliggende relatie tussen de edge-apparaat- en downstream-apparaten die worden verbonden.

1. Details van blade van de Edge-apparaat in de portal voor IoT Hub, klik op de **onderliggende apparaten beheren (preview)** knop in de bovenste opdrachtbalk.

1. Klik op de **+ toevoegen** knop.

1. Selecteer de apparaten die onderliggende en gebruikt u de pijl-rechts om te kiezen die om toe te voegen als onderliggende items uit de lijst met apparaten.

1. Klik op **OK** om te bevestigen.

Het Edge-apparaat en de onderliggende apparaten zijn nu beschikbaar voor uitgebreide offline bewerking.  