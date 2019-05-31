---
title: Ondersteunde resources voor Azure Red Hat OpenShift | Microsoft Docs
description: Krijg inzicht in welke Azure-regio's en grootten van virtuele machines worden ondersteund door Microsoft Azure Red Hat OpenShift.
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/15/2019
ms.openlocfilehash: c226227797802ab58d1bcbaadb7e97e780b30560
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306220"
---
# <a name="azure-red-hat-openshift-resources"></a>Azure Red Hat OpenShift-resources

In dit onderwerp bevat de Azure-regio's en grootten van virtuele machines wordt ondersteund door de service Microsoft Azure Red Hat OpenShift.

## <a name="azure-regions"></a>Azure-regio's

Zie [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) clusters voor een huidige lijst met regio's waarin u Azure Red Hat OpenShift kunt implementeren.

## <a name="virtual-machine-sizes"></a>Grootten van virtuele machines

Hier volgen de ondersteunde VM-groottes die u voor de rekenknooppunten in uw cluster Azure Red Hat OpenShift opgeven kunt.

> [!Important]
> Elke virtuele machine heeft een ander aantal stations dat kan worden gekoppeld. Dit mogelijk niet als onmiddellijk wissen als het geheugen of CPU-grootte.
> Niet alle VM-grootten zijn beschikbaar in alle regio's. Zelfs als de API biedt ondersteuning voor de grootte die u opgeeft, kan er een foutbericht als de grootte is niet beschikbaar in de regio die u opgeeft.
> Zie [huidige lijst van ondersteunde VM-grootten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) voor meer informatie.

## <a name="compute-node-sizes"></a>Knooppuntgrootten COMPUTE

De volgende grootte van rekenknooppunten worden ondersteund door de Azure Red Hat OpenShift REST-API:

|Grootte|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|
|-|-|-|
|Standard E4s v3|4|32 GB|
|Standard E8s v3|8|64 GB|
|Standard E16s v3|16|128 GB|
|Standard E32s v3|32|256 GB|
|-|-|-|
|Standard F8s versie 2|8|16 GB|
|Standard F16s v2|16|32 GB|
|Standard F32s versie 2|32|64 GB|

## <a name="master-node-sizes"></a>Grootte van master

De volgende master / knooppuntgrootten infrastructuur worden ondersteund door de Azure Red Hat OpenShift REST-API:

|Grootte|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|

## <a name="next-steps"></a>Volgende stappen

Probeer de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.