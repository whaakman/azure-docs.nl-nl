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
ms.openlocfilehash: 6fb497a5b6da00dece43c7f41ea3c411f385a2ba
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726884"
---
Wanneer u lokaal functies ontwikkelt, kunt u de uitbreidingen die u nodig hebt met het gebruik van Azure Functions Core's vanaf de Terminal of vanaf een opdrachtprompt installeren. 

Nadat u hebt bijgewerkt uw *function.json* dat moet worden opgenomen alle bindingen met de functie moet, de `func extensions install` opdracht in de projectmap. De opdracht leest de *function.json* bestand om te zien welke pakketten u nodig hebt en worden ze geïnstalleerd.

Als u wilt installeren van een bepaalde versie van een pakket of het gewenste om pakketten te installeren voordat u bewerkt de *function.json* gebruikt u de `func extensions install` opdracht met de naam van het pakket, zoals wordt weergegeven in het volgende voorbeeld:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Vervang `<target_version>` met een specifieke versie van het pakket. Geldige versies worden vermeld op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org).
