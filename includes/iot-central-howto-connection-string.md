---
title: bestand opnemen
description: bestand opnemen
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 04/09/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: eae4b1e919270413d4ca6627964fad9c7f5bd9bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176161"
---
1. Gebruik de `dps-keygen` opdrachtregel-hulpprogramma voor het genereren van een verbindingsreeks:

    Voor het installeren van de [sleutel generator-hulpprogramma](https://github.com/Azure/dps-keygen), voer de volgende opdracht uit:

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Voer de volgende opdracht uit met behulp van de gegevens van de verbinding die u eerder hebt genoteerd voor het genereren van een verbindingsreeks:

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Kopieer de verbindingsreeks van de `dps-keygen` uitvoer gebruiken in de apparaatcode van uw.