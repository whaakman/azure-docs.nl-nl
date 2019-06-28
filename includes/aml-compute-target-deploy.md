---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: 05082544830bad543d6743d016a91ef8565767f4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331648"
---
| COMPUTE-doel | Gebruik | GPU-ondersteuning | FPGA-ondersteuning | Description |
| ----- | ----- | ----- | ----- | ----- |
| [Local&nbsp;web&nbsp;service](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Testing/debug | Misschien | &nbsp; | Goed voor beperkte testen en problemen oplossen. Hardwareversnelling, is afhankelijk van de met behulp van bibliotheken in het lokale systeem.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Realtime Deductie |  [yes](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [yes](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Geschikt voor grootschalige productie-implementaties. Biedt snelle reactietijden en automatisch schalen van de geïmplementeerde service. Cluster automatisch schalen wordt niet ondersteund via de SDK van Azure Machine Learning. Als u wilt wijzigen van de knooppunten in het AKS-cluster, de gebruikersinterface voor het AKS-cluster in Azure portal te gebruiken. AKS is de enige optie die beschikbaar zijn voor de visuele interface. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test- of dev | &nbsp;  | &nbsp; | Goed voor de laag schaal, op basis van CPU-werkbelastingen waarvoor < 48 GB RAM-geheugen |
| [Azure Machine Learning-Computing](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Preview) Batch&nbsp;Deductie | ja | &nbsp;  | Batch scoren op serverless Computing uitvoeren. Biedt ondersteuning voor normale en met lage prioriteit VM's. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Preview) IoT&nbsp;module |  &nbsp; | &nbsp; | Implementeren en bedienen ML-modellen op IoT-apparaten. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | via IoT Edge |  &nbsp; | ja | Implementeren en bedienen ML-modellen op IoT-apparaten. |
