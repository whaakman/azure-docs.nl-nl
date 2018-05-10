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
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
Wanneer de host functies lokaal wordt uitgevoerd, worden logboeken geschreven naar het volgende pad:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

In Windows, `<DefaultTempDirectory>` is de eerste gevonden waarde van de USERPROFILE TMP, TEMP omgevingsvariabelen of de Windows-map.
Op Mac OS- of Linux, `<DefaultTempDirectory>` is de omgevingsvariabele TMPDIR.

> [!NOTE]
> Wanneer de host functies wordt gestart, overschrijft de bestaande structuur in de map.