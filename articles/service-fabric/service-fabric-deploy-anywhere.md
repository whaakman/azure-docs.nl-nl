---
title: Service Fabric-clusters maken in Windows Server en Linux | Microsoft Docs
description: Service Fabric-clusters worden uitgevoerd op Windows Server en Linux, wat betekent dat u zult kunnen implementeren en host-Service Fabric-toepassingen overal kunt u Windows Server of Linux uitvoeren.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2018
ms.author: dekapur
ms.openlocfilehash: e4540076b29cf3cd51f03239a1868e18a41781d9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726522"
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Service Fabric-clusters maken in Windows Server of Linux
Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Clusters kunnen worden geschaald naar duizenden knooppunten. Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de partitiereplica's service en de exemplaren in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitiereplica's en exemplaren voor het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt.

Service Fabric kunt u het maken van Service Fabric-clusters op alle virtuele machines of computers waarop Windows Server of Linux wordt uitgevoerd. Dit betekent dat u kunt implementeren en uitvoeren van Service Fabric-toepassingen in een omgeving waar u hebt een set van Windows Server of Linux-computers die zijn verbonden, on-premises, Microsoft Azure worden, of met elke andere cloudprovider.

## <a name="create-service-fabric-clusters-on-azure"></a>Service Fabric-clusters maken in Azure
Het maken van een cluster in Azure wordt gedaan via een sjabloon voor de Resource-Model of de [Azure-portal](https://portal.azure.com). Lezen [maken van een Service Fabric-cluster met behulp van Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) of [een Service Fabric-cluster maken vanuit Azure portal](service-fabric-cluster-creation-via-portal.md) voor meer informatie.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Ondersteunde besturingssystemen voor clusters op Azure
Kunt u clusters maken in virtuele machines die deze besturingssystemen wordt uitgevoerd:

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 1709
* WindowsServer 1803
* Linux Ubuntu 16.04
* Red Hat Enterprise Linux 7.4 (ondersteuning voor Preview-versie)

> [!NOTE]
> Als u besluit het implementeren van Service Fabric in Windows Server 1709, houd er rekening mee dat (1) het is niet een van long term servicing branch, zodat u wellicht versies in de toekomst te verplaatsen, en (2) als u containers implementeert, containers die zijn gebouwd op Windows Server 2016 niet geschikt voor Windows Server  1709, en vice versa (u moet ze voor het implementeren van deze opnieuw maken).
>

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Zelfstandige Service Fabric clusters on-premises maken of met elke andere cloudprovider
Service Fabric biedt een installatiepakket voor u maken van zelfstandige Service Fabric-clusters on-premises of op elke andere cloudprovider.

Lees voor meer informatie over het instellen van zelfstandige Service Fabric-clusters in Windows Server, [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

  > [!NOTE]
  > Zelfstandige clusters momenteel niet ondersteund voor Linux. Linux wordt ondersteund op one-box voor ontwikkelings- en Azure Linux-clusters voor meerdere machines.
  >

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Alle cloudimplementaties versus de on-premises implementaties
Het proces voor het maken van een Service Fabric-cluster on-premises is vergelijkbaar met het proces voor het maken van een cluster in elke cloud van uw keuze met een set van virtuele machines. De eerste stappen voor het inrichten van de virtuele machines worden beheerst door de cloudprovider of on-premises-omgeving die u gebruikt. Zodra u een set van virtuele machines met de netwerkverbinding tussen beide zijn ingeschakeld hebt, klikt u vervolgens de stappen voor het instellen van het Service Fabric-pakket, de clusterinstellingen bewerken en maken van het cluster worden uitgevoerd en management scripts zijn identiek. Dit zorgt ervoor dat uw kennis en ervaring van het besturingssysteem en het beheren van Service Fabric-clusters worden overgedragen wanneer u nieuwe hostingomgevingen als doel.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Voordelen van het maken van zelfstandige Service Fabric-clusters
* U staat op vrije keuze van alle cloudproviders voor het hosten van uw cluster.
* Service Fabric-toepassingen, één keer worden geschreven, kunnen worden uitgevoerd in omgevingen met meerdere hosting met minimale geen wijzigingen.
* Kennis van het bouwen van Service Fabric-toepassingen die zijn doorgevoerd in van de ene host-omgeving naar een andere.
* Operationele ervaring uitvoeren en beheren van Service Fabric clusters uitvoert via van de ene omgeving naar een andere.
* Brede klant bereik is het niet-gebonden hostingbeperkingen omgeving.
* Er bestaat een extra laag voor betrouwbaarheid en de bescherming tegen storingen wijdverbreid omdat u de services verplaatsen kunt via naar een andere implementatieomgeving als een gegevensprovider center of in de cloud een onleesbaar heeft.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Ondersteunde besturingssystemen voor zelfstandige clusters
U zijn kunt maken van clusters op virtuele machines of op computers met deze besturingssystemen (Linux is nog niet ondersteund):

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Voordelen van het Service Fabric-clusters op Azure via zelfstandige die service Fabric-clusters on-premises gemaakt
Service Fabric-clusters worden uitgevoerd op Azure biedt voordelen ten opzichte van de on-premises optie, dus als u geen specifieke behoeften voor waarin u uw clusters, uitvoeren en het is raadzaam dat u ze hebt uitgevoerd op Azure. In Azure bieden we integratie met andere Azure-functies en services, waarmee bewerkingen en het beheer van het cluster eenvoudiger en betrouwbaarder.

* **Azure-portal:** Azure-portal kunt u eenvoudig maken en beheren van clusters.
* **Azure Resource Manager:** gebruik van Azure Resource Manager kunt u eenvoudig beheer van alle resources die door het cluster worden gebruikt als een eenheid en kosten bijhouden en facturering vereenvoudigt.
* **Service Fabric-Cluster als een Azure-Resource** A Service Fabric-cluster is een Azure-resource, dus u kunt het model, zoals u ook andere resources in Azure.
* **Integratie met Azure-infrastructuur** coördineert de Service Fabric met de onderliggende Azure-infrastructuur voor OS-, netwerk en andere upgrades worden uitgevoerd voor betere beschikbaarheid en betrouwbaarheid van uw toepassingen.  
* **Diagnostische gegevens:** op Azure, bieden we integratie met Azure diagnostics en Log Analytics.
* **Automatisch schalen:** voor clusters op Azure, bieden we ingebouwde functionaliteit voor automatisch schalen vanwege een virtuele-machineschaalsets. In on-premises en andere cloudomgevingen hebt u uw eigen automatisch schalen functie of schaal handmatig met behulp van de API's die Service Fabric beschikbaar maakt voor het schalen van clusters.

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

