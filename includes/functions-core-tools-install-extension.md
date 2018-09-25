---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 09/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1b53c53b1e5fb089eb9b8a9b816b11a1eea126d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044506"
---
Als u functies lokaal ontwikkelt, kunt u de uitbreidingen die u nodig hebt met behulp van Azure Functions Core Tools vanaf de Terminal of vanaf een opdrachtprompt installeren.

Nadat u hebt bijgewerkt uw *function.json* bestand om op te nemen van de bindingen die uw functie nodig heeft, voer de volgende opdracht uit in de projectmap.

```bash
func extensions install
```

De opdracht leest de *function.json* bestand om te zien welke pakketten die u nodig hebt, worden ze geïnstalleerd en wordt het project extensies. Voegt een nieuwe bindingen toe op de huidige versie, maar wordt de bestaande bindingen niet bijgewerkt. Gebruik de `--force` optie bestaande bindingen bijwerken naar de meest recente versie bij het installeren van nieuwe labels.

Als u wilt dat een bepaalde versie van een pakket te installeren of u wilt om pakketten te installeren voordat u bewerkt de *function.json* bestand, gebruikt u de `func extensions install` opdracht met de naam van het pakket, zoals wordt weergegeven in het volgende voorbeeld:

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Vervang `<target_version>` met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org).
