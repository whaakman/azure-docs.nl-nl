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
ms.openlocfilehash: a8160e677fa99d8cb691db39d7f29ba6eddbd261
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004676"
---
## <a name="enabling-extended-offline-operation-preview"></a>Inschakelen van uitgebreide offline bewerking (Preview)
Beginnen met de [v1.0.2 release](https://aka.ms/edge102) van de Edge-Runtime, de Edge-apparaat en downstream-verbinding te maken met deze apparaten kunnen worden geconfigureerd voor uitgebreide offline bewerking. 

Met deze mogelijkheid kunnen lokale modules of downstream-apparaten opnieuw worden geverifieerd met het Edge-apparaat, indien nodig en communiceren met elkaar met behulp van berichten en -methoden, zelfs wanneer de IoT-Hub is verbroken. Raadpleeg deze [blogbericht](https://aka.ms/iot-edge-offline) en [concept artikel](../articles/iot-edge/offline-capabilities.md) voor meer details en het bereik van deze mogelijkheid.

Voor het inschakelen van uitgebreide offline in een gatewayscenario voor tot stand brengen van een bovenliggende / onderliggende relatie tussen de edge-apparaat- en downstream-apparaten die worden verbonden.

1. Details van blade van de Edge-apparaat in de portal voor IoT Hub, klik op de **onderliggende apparaten beheren (preview)** knop in de bovenste opdrachtbalk.

1. Klik op de **+ toevoegen** knop.

1. Selecteer de apparaten die onderliggende en gebruikt u de pijl-rechts om te kiezen die om toe te voegen als onderliggende items uit de lijst met apparaten.

1. Klik op **OK** om te bevestigen.

1. In de **Modules instellen** scherm van de details van het Edge-apparaat, klikt u op **geavanceerde instellingen voor Edge-Runtime configureren**, en klikt u onder **Edge Hub** omgevingsvariabelen een vermelding toevoegen **UpstreamProtocol** met waarde **MQTT**. De dezelfde omgevingsvariabele toevoegen en de waarde voor de **Edge Agent** ook. 

1. Klik op **opslaan** en zorg ervoor dat u **indienen** de wijzigingen na het klikken op **volgende** twee keer.

Het Edge-apparaat en de onderliggende apparaten zijn nu beschikbaar voor uitgebreide offline bewerking.  