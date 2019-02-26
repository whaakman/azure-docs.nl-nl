---
title: GMSA voor services van Azure Service Fabric-container instellen | Microsoft Docs
description: Ontdek hoe u gMSA voor een container die wordt uitgevoerd in Azure Service Fabric instellen.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: 4ad697e01ef9e023232e2a2a16e4584a2779f84a
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806297"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>GMSA instellen voor Windows-containers die worden uitgevoerd in Service Fabric

Voor het instellen van gMSA (groep beheerde serviceaccounts), een referentie-specificatiebestand (`credspec`) op alle knooppunten in het cluster is geplaatst. Het bestand kan worden gekopieerd op alle knooppunten met behulp van een VM-extensie.  De `credspec` bestand moet bevatten de gegevens van de gMSA-account. Voor meer informatie over de `credspec` bestand, Zie [serviceaccounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). De referentie-specificatie en de `Hostname` tag zijn opgegeven in het toepassingsmanifest. De `Hostname` tag moet overeenkomen met de naam van het gMSA-account dat de container wordt uitgevoerd onder.  De `Hostname` tag kunt u de container om zichzelf te verifiëren bij andere services in het domein met behulp van Kerberos-verificatie.  Een voorbeeld voor het opgeven van de `Hostname` en de `credspec` in de toepassing het manifest wordt weergegeven in het volgende codefragment:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Lees de volgende stap, de volgende artikelen:

* [Een Windows-container implementeren in Service Fabric in Windows Server 2016](service-fabric-get-started-containers.md)
* [Een Docker-container implementeren in Service Fabric in Linux](service-fabric-get-started-containers-linux.md)
