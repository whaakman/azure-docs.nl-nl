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
ms.openlocfilehash: a463ac9f9584cb13cadbcf79674d56b2f8e47c2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753098"
---
| COMPUTE-doel | Gebruik | GPU-ondersteuning | FPGA-ondersteuning | Description |
| ----- | ----- | ----- | ----- | ----- |
| [Lokale webservice](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Testing/debug | Misschien | &nbsp; | Goed voor beperkte testen en problemen oplossen. Hardwareversnelling, is afhankelijk van de met behulp van bibliotheken in het lokale systeem.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Realtime Deductie |  [yes](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [yes](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Geschikt voor grootschalige productie-implementaties. Biedt automatisch schalen en snelle responstijden.  AKS is de enige optie die beschikbaar zijn voor de visuele interface. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test- of dev | &nbsp;  | &nbsp; | Goed voor de laag schaal, op basis van CPU-werkbelastingen waarvoor < 48 GB RAM-geheugen |
| [Azure Machine Learning-Computing](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Preview) Batch Deductie | ja | &nbsp;  | Batch scoren op serverless Computing uitvoeren. Biedt ondersteuning voor normale en met lage prioriteit VM's. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Preview) IoT-module |  &nbsp; | &nbsp; | Implementeren en bedienen ML-modellen op IoT-apparaten. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | via IoT Edge |  &nbsp; | ja | Implementeren en bedienen ML-modellen op IoT-apparaten. |