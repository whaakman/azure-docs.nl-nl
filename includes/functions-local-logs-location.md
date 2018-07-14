---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/01/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88c01e8e57d4a92478b8b1ca0689ff0f8e499b39
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38739370"
---
Wanneer de host functies lokaal wordt uitgevoerd, worden logboeken geschreven naar het volgende pad:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Op Windows, `<DefaultTempDirectory>` is de eerste gevonden waarde van de USERPROFILE TMP, TEMP omgevingsvariabelen of de Windows-map.
Op Mac OS of Linux, `<DefaultTempDirectory>` is de omgevingsvariabele TMPDIR.

> [!NOTE]
> Wanneer de Functions-host wordt gestart, overschrijft de structuur van het bestaande bestand in de map.