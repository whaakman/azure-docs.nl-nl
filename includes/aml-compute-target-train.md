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
ms.openlocfilehash: 1b62d4b2ac1bb69e2270c3202d29eb595df7aac4
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806028"
---
**COMPUTE-doelen kunnen worden hergebruikt van één trainingstaak naar de volgende**. Bijvoorbeeld, wanneer u een externe VM aan uw werkruimte koppelen, u deze opnieuw kunt gebruiken voor meerdere taken.

|Training &nbsp;doelen| GPU-ondersteuning |[Geautomatiseerde ML](../articles/machine-learning/service/concept-automated-ml.md) | [ML-pijplijnen](../articles/machine-learning/service/concept-ml-pipelines.md) | [Visuele interface](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Lokale computer](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Misschien | ja | &nbsp; | &nbsp; |
|[Azure Machine Learning-Computing](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| ja | Ja & <br/>hyperparameter&nbsp;tuning | ja | ja |
|[Externe virtuele machine](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |ja | Ja & <br/>hyperparameter afstemmen | ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | ja | ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | ja | &nbsp; |
