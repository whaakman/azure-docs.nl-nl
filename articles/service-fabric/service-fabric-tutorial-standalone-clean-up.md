---
title: 'Zelfstudie: Zelfstandig Service Fabric-cluster opschonen - Azure Service Fabric | Microsoft Docs'
description: In deze zelfstudie leert u hoe u voor het opschonen van uw zelfstandige cluster
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
ms.openlocfilehash: 9127ad1fe148f85779913454adf6578a9a8a1055
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274087"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Zelfstudie: Opschonen van uw zelfstandige cluster

Zelfstandige Service Fabric-clusters bieden u de mogelijkheid om uw eigen omgeving te kiezen en een cluster te maken als onderdeel van de benadering "Elk besturingssysteem, elke cloud" die we in Service Fabric hanteren. In deze zelfstudie, een zelfstandige cluster die worden gehost op AWS- of Azure maken en installeren van een toepassing erin.

Deze zelfstudie is deel vier een serie. Dit deel van de zelfstudie laat zien hoe u voor het opschonen van de AWS- of Azure-resources die u hebt gemaakt voor het hosten van uw Service Fabric-cluster.

In deel vier van de serie leert u het volgende:

> [!div class="checklist"]
> * Service Fabric-cluster opschonen
> * Opschonen van uw AWS- of Azure-resources

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric-cluster opschonen

1. Verbinding met de virtuele machine die u voor het geïnstalleerde Service Fabric gebruikt.
2. Open PowerShell.
3. Ga naar de uitgepakte map uit de tweede zelfstudie.
4. Voer de volgende opdracht uit om het Service Fabric-cluster te verwijderen:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Voer `Y` wanneer hierom wordt gevraagd, als deze voltooid is de uitvoer ziet er als volgt met uw eigen IP-adressen vervangen in:

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

1. Meld u aan bij uw AWS-Account.
2. Ga naar de EC2-console.
3. Selecteer de drie knooppunten die u hebt gemaakt in deel een van de zelfstudie.
4. Klik op **acties** > **status exemplaar** > **beëindigen**.

## <a name="clean-up-azure-resources"></a>Opschonen van de Azure-resources

1. Meld u aan bij Azure Portal.
2. Ga naar de **virtuele Machines** sectie.
3. Schakel de selectievakjes voor de drie knooppunten die u hebt gemaakt in deel één van de zelfstudie.
4. Klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deel vier van de reeks hebt u geleerd hoe u de resources opschoont die zijn gemaakt in eerdere stappen.

> [!div class="checklist"]
> * Uw resources opschonen

> [!div class="nextstepaction"]
> [Terug naar het begin](service-fabric-tutorial-standalone-create-infrastructure.md)
