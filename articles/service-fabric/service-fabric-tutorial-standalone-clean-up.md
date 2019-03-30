---
title: 'Zelfstudie: Zelfstandig Service Fabric-cluster opschonen - Azure Service Fabric | Microsoft Docs'
description: In deze zelfstudie leert u hoe u het zelfstandige cluster kunt opschonen
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 28cc2bf4794620641fb6af46bd4017d74f87e955
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666988"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Zelfstudie: Opschonen van uw zelfstandige cluster

Zelfstandige Service Fabric-clusters bieden u de mogelijkheid om uw eigen omgeving te kiezen en een cluster te maken als onderdeel van de benadering "Elk besturingssysteem, elke cloud" die we in Service Fabric hanteren. In deze reeks zelfstudies maakt u een zelfstandig cluster dat in AWS wordt gehost en installeert u vervolgens een toepassing in het cluster.

Deze zelfstudie is deel vier een serie. In dit deel van de reeks wordt uitgelegd hoe u de AWS-resources kunt opschonen die u hebt gemaakt voor het hosten van uw Service Fabric-cluster.

In deel vier van de serie leert u het volgende:

> [!div class="checklist"]
> * Service Fabric-cluster opschonen
> * AWS-resources opschonen

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric-cluster opschonen

* Ga met behulp van RDP naar het EC2-exemplaar dat u hebt gebruikt voor de installatie van Service Fabric.
* Open PowerShell.
* Ga naar de uitgepakte map uit de tweede zelfstudie.
* Voer de volgende opdracht uit om het Service Fabric-cluster te verwijderen:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

* Druk op `Y` als dat wordt gevraagd. Als de opdracht is gelukt, ziet de uitvoer er als volgt uit, maar dan met uw eigen IP-adressen:

```powershell
Best Practices Analyzer completed successfully.
Removing configuration from machine 172.31.21.141
Removing configuration from machine 172.31.27.1
Removing configuration from machine 172.31.20.163
Configuration removed from machine 172.31.21.141
Configuration removed from machine 172.31.27.1
Configuration removed from machine 172.31.20.163
The cluster is successfully removed.
```

## <a name="clean-up-aws-resources"></a>AWS-resources opschonen

* Meld u aan bij uw AWS-account.
* Ga naar de EC2-console.
* Selecteer de drie knooppunten die u hebt gemaakt in deel een van de zelfstudie.
* Klik op **Actions** > **Instance State** > **Terminate**

## <a name="next-steps"></a>Volgende stappen

In deel vier van de reeks hebt u geleerd hoe u de resources opschoont die zijn gemaakt in eerdere stappen.

> [!div class="checklist"]
> * Uw resources opschonen

> [!div class="nextstepaction"]
> [Terug naar het begin](service-fabric-tutorial-standalone-create-infrastructure.md)