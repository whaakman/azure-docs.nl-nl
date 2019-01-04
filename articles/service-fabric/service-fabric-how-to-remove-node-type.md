---
title: Hoe op afstand een knooppunt typt in Azure Service Fabric | Microsoft Docs
description: Informatie over het verwijderen van een knooppunttype in Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: v-steg
manager: JeanPaul.Connick
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/26/2018
ms.author: v-steg
ms.openlocfilehash: 3704a356763b16a30285baee1aabffdd3aa3f8aa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53980638"
---
# <a name="remove-a-service-fabric-node-type"></a>Verwijderen van de soort van een Service Fabric-knooppunt

Gebruik [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) te verwijderen van de soort van een Service Fabric-knooppunt.

De twee bewerkingen die zich voordoen wanneer Remove-AzureRmServiceFabricNodeType heet zijn:
1.  De virtuele Machine schaal ingesteld (VMSS) achter het knooppunttype is verwijderd.
2.  Voor alle knooppunten binnen dat knooppunttype, wordt de status van de gehele voor dat knooppunt uit het systeem verwijderd. Als er services op dat knooppunt zijn, klikt u vervolgens de services eerst verplaatst uit naar een ander knooppunt. Als de clustermanager een knooppunt niet voor de replica/service vinden kan, klikt u vervolgens is de bewerking vertraagd/geblokkeerd.

> [!NOTE]
> Remove-AzureRmServiceFabricNodeType gebruiken om te verwijderen van een knooppunttype uit een productiecluster wordt niet aanbevolen moet regelmatig worden gebruikt. In dit geval is dit een zeer gevaarlijke opdracht als de virtuele-Machineschaalset resource achter het knooppunttype in feite worden verwijderd. Wanneer u Remove-AzureRmServiceFabricNodeType aanroept, er geen manier om te weten als u geïnteresseerd bent over het verwijderen wordt veilig is. 

## <a name="durability-characteristics"></a>Kenmerken van duurzaamheid
Bij het gebruik van de Remove-AzureRmServiceFabricNodeType veiligheid prioriteit boven snelheid. Silver- of Gold [duurzaamheid kenmerken](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) worden aanbevolen, omdat:
- Brons geeft u geen garanties over het opslaan van informatie over de status.
- Silver en Gold duurzaamheid eventuele wijzigingen in de VMSS-trap.
- Goud biedt u ook controle over de Azure-updates onder VMSS.

Service Fabric 'regelt' onderliggende wijzigingen en updates zodat de gegevens zijn niet verloren gaan. Wanneer u een knooppunt met de duurzaamheid Brons verwijdert, kunt u informatie over de status verliezen. Als u een primaire knooppunttype verwijderen wilt en uw toepassing staatloos is, is Brons acceptabel. Wanneer u stateful werkbelastingen in de productieomgeving uitvoert, moet de minimale configuratie Silver. Op dezelfde manier voor productiescenario's moet het primaire knooppunttype altijd worden Silver- of Gold.

### <a name="more-about-bronze-durability"></a>Meer informatie over duurzaamheid Brons

Wanneer u een knooppunttype dat is Brons verwijdert, gaan alle knooppunten in het knooppunttype onmiddellijk omlaag. Service Fabric Brons knooppunten VMSS updates niet overlappen, alle virtuele machines dus u kunt onmiddellijk. Als u iets stateful op deze knooppunten hebt, gaat de gegevens verloren. Nu, zelfs als u stateless was, alle knooppunten in de Service Fabric deel uitmaken van de ring, zodat u een volledige groep mogelijk verloren gaan, die invloed kunnen hebben op het cluster zelf.

In tegenstelling tot het verwijderen van één knooppunt, omdat in theorie, verwijdert u één knooppunt tegelijk, wacht totdat de replica's en services voor het overzetten, wachten op het systeem te laten stabiliseren, voordat u verwijdert een ander knooppunt, enzovoort.  Echter, als u meerdere knooppunten gelijktijdig in één keer verwijderen, uw cluster mogelijk uitvallen (omdat de Service Fabric niet alle updates VMSS overlappen met de duurzaamheid brons).

## <a name="recommended-node-type-removal-process"></a>Aanbevolen procedure voor het verwijderen van type knooppunt

Het knooppunttype in het meest veilige en snelle manier verwijderen:
1.  Als u de duurzaamheid brons of niet dat het systeem toepassingen die informatie over de status die wilt bevatten, gaan verloren als onderdeel van de eerste lege stateful gegevens uit de knooppunten die worden beïnvloed door het type knooppunt verwijderen van verwijdering type knooppunt verplaatsen.
2.  Voer [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) op elk van de knooppunten die moeten worden verwijderd.
3.  Voer [Remove-AzureRmVmss](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#remove-vms-from-a-scale-set) voor virtuele machines die worden beïnvloed door het type knooppunt verwijderen.
4. Voer [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) te verwijderen van het knooppunttype.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over cluster [duurzaamheid kenmerken](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Meer informatie over [knooppunttypen en Virtual Machine Scale Sets](service-fabric-cluster-nodetypes.md).
- Meer informatie over [Service Fabric-cluster schalen](service-fabric-cluster-scaling.md).