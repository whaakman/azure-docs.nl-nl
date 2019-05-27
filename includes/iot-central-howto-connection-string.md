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
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151968"
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