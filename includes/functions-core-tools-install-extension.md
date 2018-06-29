---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063745"
---
Wanneer u lokaal functies ontwikkelt, kunt u de uitbreidingen die u nodig hebt met het gebruik van Azure Functions Core's vanaf de Terminal of vanaf een opdrachtprompt installeren. 

Nadat u hebt bijgewerkt uw *function.json* dat moet worden opgenomen alle bindingen met de functie moet, de `func extensions install` opdracht in de projectmap. De opdracht leest de *function.json* bestand om te zien welke pakketten u nodig hebt en worden ze geïnstalleerd.

Als u wilt installeren van een bepaalde versie van een pakket of het gewenste om pakketten te installeren voordat u bewerkt de *function.json* gebruikt u de `func extensions install` opdracht met de naam van het pakket, zoals wordt weergegeven in het volgende voorbeeld:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Vervang `<target_version>` met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden vermeld op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org).
