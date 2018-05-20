---
title: GMSA voor services van Azure Service Fabric-container instellen | Microsoft Docs
description: Ontdek hoe u setup gMSA voor een container in Azure Service Fabric uitgevoerd.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: subramar
ms.openlocfilehash: e4cd0b42e21609f88edc28c8fd7b5c433d56b3c1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Instellen van gMSA voor Windows-containers die zijn uitgevoerd op de Service Fabric

Voor het instellen van gMSA (groep beheerde serviceaccounts), een referentie specification-bestand (`credspec`) op alle knooppunten in het cluster is geplaatst. Het bestand kan worden gekopieerd op alle knooppunten met behulp van een VM-extensie.  De `credspec` -bestand moet bevatten de gegevens van de gMSA-account. Voor meer informatie over de `credspec` bestand, Zie [serviceaccounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). De referentie-specificatie en de `Hostname` code zijn opgegeven in het toepassingsmanifest. De `Hostname` label moet overeenkomen met de naam van de gMSA-account die compatibel is met de container.  De `Hostname` code ervoor dat de container zichzelf te verifiëren bij andere services in het domein Kerberos-verificatie gebruiken.  Een voorbeeld voor het opgeven van de `Hostname` en de `credspec` in de toepassing manifest wordt weergegeven in het volgende fragment:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Lees de volgende artikelen een volgende stap:

* [Een Windows-container implementeren op Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementeert een Docker-container op Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
