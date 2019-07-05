---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d62da82b4a4dd35532dd8776a9111689db469201
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448366"
---
## <a name="register-extensions"></a>Extensies registreren

Functies met uitzondering van de HTTP- en timer wordt geactiveerd, bindingen in runtimeversie 2.x worden geïmplementeerd als extensiepakketten. In versie 2.x van de Azure Functions-runtime die u moet expliciet de extensies voor de bindingstypen gebruikt in uw functies te registreren. De uitzonderingen op deze zijn HTTP-bindingen en timer wordt geactiveerd, waarvoor geen extensies vereist.

U kunt bindinguitbreidingen afzonderlijk installeren of u een extensie-bundel-verwijzing naar het projectbestand host.json kunt toevoegen. Extensie-bundels Hiermee verwijdert u de kans op pakket compatibiliteitsproblemen bij het gebruik van meerdere bindingstypen. Het is de aanbevolen aanpak voor het registreren van de bindinguitbreidingen. Extensie bundels verwijdert ook de vereiste van het installeren van de .NET Core SDK 2.x. 

### <a name="extension-bundles"></a>extensie-bundels

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Zie voor meer informatie, [registreren Azure Functions-bindinguitbreidingen](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Voordat u de bindingen aan het bestand functions.json toevoegen, moet u de extensie bundels toevoegen aan de host.json.

### <a name="register-individual-extensions"></a>Afzonderlijke uitbreidingen registreren

Als u nodig hebt voor het installeren van extensies die zich niet in een bundel, kunt u afzonderlijke extensiepakketten voor specifieke bindingen handmatig registreren. 

> [!NOTE]
> Extensies handmatig registreren met behulp van `func extensions install`, hebt u de .NET Core 2.x SDK is geïnstalleerd.

Nadat u hebt bijgewerkt uw *function.json* bestand om op te nemen van de bindingen die uw functie nodig heeft, voer de volgende opdracht uit in de projectmap.

```bash
func extensions install
```

De opdracht leest de *function.json* bestand om te zien welke pakketten die u nodig hebt, worden ze geïnstalleerd en wordt het project extensies. Voegt een nieuwe bindingen toe op de huidige versie, maar wordt de bestaande bindingen niet bijgewerkt. Gebruik de `--force` optie bestaande bindingen bijwerken naar de meest recente versie bij het installeren van nieuwe labels.