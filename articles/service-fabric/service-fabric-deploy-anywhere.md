---
title: Azure Service Fabric-clusters maken in Windows Server- en Linux | Microsoft Docs
description: Service Fabric-clusters worden uitgevoerd op Windows Server- en Linux-, wat betekent dat u zult implementeren- en overal host Service Fabric-toepassingen kunt u Windows Server of Linux uitvoeren.
services: service-fabric
documentationcenter: .net
author: Chackdan
manager: timlt
editor: 
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2017
ms.author: chackdan
ms.openlocfilehash: e3cfad19e42af24edd68befd7b1eac8cef41a1d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Service Fabric-clusters op Windows Server of Linux maken
Een Azure Service Fabric-cluster is een set netwerk verbonden virtuele of fysieke machines waarin uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Clusters kunnen worden geschaald naar duizenden knooppunten. Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de service partitie replica's en exemplaren op tussen het toegenomen aantal knooppunten. De algemene verbetert de prestaties van toepassingen en verkleint u conflicten over toegang tot het geheugen. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de replica's en exemplaren op tussen het kleinere aantal knooppunten beter te benutten van de hardware op elk knooppunt.

Service Fabric kunt voor het maken van Service Fabric-clusters op elke virtuele machines of computers waarop Windows Server of Linux wordt uitgevoerd. Dit betekent dat u zich kunt implementeren en Service Fabric-toepassingen uitvoeren in een omgeving waar het hebben van een set van Windows Server- of Linux-computers onderling, on-premises, Microsoft Azure worden of met elke cloudprovider.

## <a name="create-service-fabric-clusters-on-azure"></a>Service Fabric-clusters maken in Azure
Maken van een cluster in Azure wordt uitgevoerd via een sjabloon Resource Model of de [Azure-portal](https://portal.azure.com). Lees [Service Fabric-cluster maken met behulp van een Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) of [een Service Fabric-cluster maken van de Azure-portal](service-fabric-cluster-creation-via-portal.md) voor meer informatie.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Ondersteunde besturingssystemen voor clusters op Azure
U staat op clusters maken op virtuele machines die deze besturingssystemen wordt uitgevoerd:

* Windows Server 2012 R2
* Windows Server 2016 
* Linux Ubuntu 16.04  

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Zelfstandige Service Fabric-clusters lokale maken of bij een cloudprovider
Service Fabric bevat een installatiepakket voor u zelfstandige Service Fabric-clusters lokale maken of op elke cloudprovider.

Lees voor meer informatie over het instellen van zelfstandige Service Fabric-clusters op Windows Server, [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

  > [!NOTE]
  > Zelfstandige clusters worden niet die momenteel ondersteund voor Linux. Linux wordt ondersteund op één verpakking voor ontwikkeling en meerdere machines Azure Linux-clusters.
  >

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Alle cloudimplementaties tegenover on-premises implementaties
Het proces voor het maken van een Service Fabric-cluster lokale is vergelijkbaar met het proces van het maken van een cluster in een cloud van uw keuze met een set van virtuele machines. De eerste stappen voor het inrichten van de virtuele machines worden bepaald door de cloudprovider of on-premises omgeving die u gebruikt. Zodra u een set van virtuele machines met de netwerkverbinding tussen deze twee is ingeschakeld hebt, klikt u vervolgens de stappen voor het instellen van het Service Fabric-pakket, de clusterinstellingen bewerken en maken van het cluster worden uitgevoerd en scripts voor zijn identiek. Dit zorgt ervoor dat uw kennis en ervaring van het besturingssysteem en Service Fabric-clusters beheren overgedragen wanneer u nieuwe hostingomgevingen als doel.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Voordelen van zelfstandige Service Fabric-clusters maken
* U bent gratis kiezen elke cloudprovider voor het hosten van uw cluster.
* Service Fabric-toepassingen, één keer worden geschreven, kunnen worden uitgevoerd in omgevingen met meerdere hosting met minimale geen wijzigingen aangebracht.
* Kennis van het bouwen van Service Fabric-toepassingen zijn voor de via van de ene hosting-omgeving naar een andere.
* Praktijkervaring van uitvoert en beheert de Service Fabric-uitvoert clusters via van de ene omgeving naar een andere.
* Brede klant reach is unbounded door het hosten van de beperkingen van de omgeving.
* Een extra beveiligingslaag betrouwbaarheid en bescherming tegen storingen wijdverbreid bestaat omdat u de services verplaatsen kunt naar een andere implementatieomgeving als een gegevensprovider center of de cloud een onleesbaar heeft.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Ondersteunde besturingssystemen voor zelfstandige clusters
U staat op het maken van clusters op virtuele machines of computers met deze besturingssystemen (Linux wordt nog niet ondersteund):

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Voordelen van het Service Fabric-clusters op Azure via zelfstandige die service Fabric-clusters lokale gemaakt
Service Fabric-clusters op Azure biedt voordelen ten opzichte van de on-premises optie, dus als er geen specifieke behoeften voor waarin u uw clusters, uitvoeren en vervolgens het is raadzaam dat u deze op Azure uitvoeren. In Azure bieden we een integratie met andere Azure-functies en services die bewerkingen en beheer van het cluster kunt gemakkelijker en betrouwbaarder.

* **Azure-portal:** Azure-portal kunt u gemakkelijk maken en beheren van clusters.
* **Azure Resource Manager:** gebruik van Azure Resource Manager kunt u eenvoudig beheer van alle resources die door het cluster wordt gebruikt als een eenheid en vereenvoudigt u kosten bijhouden en facturering.
* **Service Fabric-Cluster als een Azure-Resource** A Service Fabric-cluster is een Azure-resource, waarmee u kunt het model zoals andere resources in Azure.
* **Integratie met Azure-infrastructuur** Service Fabric-coördinaten met de onderliggende Azure-infrastructuur voor het besturingssysteem, netwerk- en andere updates van beschikbaarheid en betrouwbaarheid van uw toepassingen te verbeteren.  
* **Diagnostische gegevens:** in Azure, bieden we integratie met Azure diagnoses en Log Analytics.
* **Automatische schaling:** voor clusters op Azure, bieden we ingebouwde automatische schaling functionaliteit als gevolg van de virtuele-machineschaalsets. In on-premises en andere cloudomgevingen hebt u uw eigen automatische schaling functie of schaal handmatig met behulp van de API's die Service Fabric beschikbaar maakt voor het schalen van clusters bouwen.

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op virtuele machines of computers waarop Linux wordt uitgevoerd: [een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

