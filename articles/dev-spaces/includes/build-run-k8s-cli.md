---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 1cf301ab3b4430c7bb79f6fdd72243f9f2ad1d9b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
## <a name="build-and-run-code-in-kubernetes"></a>Ontwikkel en voer de code in Kubernetes
Laten we onze code uitgevoerd! Voer deze opdracht uit in het terminalvenster de **code hoofdmap**, webfrontend:

```cmd
azds up
```

Let op de uitvoer van de opdracht, ziet u verschillende dingen zoals die:
- Broncode is gesynchroniseerd met de ontwikkelomgeving in Azure.
- Een installatiekopie van een container is ingebouwd in Azure, zoals opgegeven door de Docker-elementen in de map van uw code.
- Kubernetes objecten worden gemaakt die gebruikmaken van de installatiekopie van het container zoals opgegeven door de grafiek Helm in de map van uw code.
- Informatie over de eindpunten van de container wordt weergegeven. In ons geval verwacht we een openbare URL voor HTTPS.
- Ervan uitgaande dat de bovenstaande stadia worden uitgevoerd, moet u beginnen om te zien `stdout` (en `stderr`) uitvoeren als de container opgestart wordt.

> [!Note]
> Deze stappen langer duurt de eerste keer dat de `up` opdracht wordt uitgevoerd, maar latere uitvoeringen zijn sneller.

## <a name="test-the-web-app"></a>De web-app testen
Scannen van de uitvoer van de console voor meer informatie over de openbare URL die is gemaakt door de `up` opdracht. Het formulier zal zijn: 

`Running at public URL: https://<servicename>-<environmentname>.<guid>.<region>.aksapp.io` 

Deze URL in een browservenster geopend en ziet u het laden van de web-app. Als de container wordt uitgevoerd, `stdout` en `stderr` uitvoer naar het terminalvenster gestreamd.
