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
ms.openlocfilehash: 484567dd9d9c3d050e7be25bd685a5b8d3de0687
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825504"
---
## <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes
We gaan onze code uitvoeren! In het terminalvenster voert u deze opdracht uit vanuit de **hoofdcodemap**, webfrontend:

```cmd
azds up
```

Houd de uitvoer van de opdracht in de gaten. Er zijn meerdere zaken die u zullen opvallen tijdens de uitvoering:
- De broncode is gesynchroniseerd met de dev-ruimte in Azure.
- Er wordt een containerinstallatiekopie gemaakt in Azure, zoals is aangegeven door de Docker-assets in uw codemap.
- Er worden Kubernetes-objecten gemaakt die de containerinstallatiekopie gebruiken zoals wordt aangegeven in de Helm-grafiek in uw codemap.
- Er wordt informatie over het eindpunt of de eindpunten van de container weergegeven. In ons geval verwachten we een openbare HTTP-URL.
- Ervan uitgaande dat de fases hierboven goed worden afgerond, moet u nu `stdout`-uitvoer (en `stderr`) gaan zien terwijl de container wordt opgestart.

> [!Note]
> Deze stappen nemen de eerste keer dat de opdracht `up` wordt uitgevoerd, meer tijd in beslag, maar latere uitvoeringen zullen sneller verlopen.

### <a name="test-the-web-app"></a>De web-app testen
Scan de console-uitvoer voor informatie over de openbare URL die door de opdracht `up` is gemaakt. Het zal in deze vorm te zien zijn: 

`Running at public URL: http://<servicename>-<cluster-name>.<guid>.<region>.aksapp.io` 

Open deze URL in een browservenster. Dan ziet u dat de web-app wordt geladen. Terwijl de container wordt uitgevoerd, wordt `stdout`- en `stderr`-uitvoer naar het terminalvenster gestreamd.
