---
title: GMSA voor services van Azure Service Fabric-container instellen | Microsoft Docs
description: Ontdek hoe u gMSA voor een container die wordt uitgevoerd in Azure Service Fabric instellen.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: subramar
ms.openlocfilehash: b4aa08c10cef91b3bf2eef4c87967a74524f6500
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621797"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>GMSA instellen voor Windows-containers die worden uitgevoerd in Service Fabric

Voor het instellen van gMSA (groep beheerde serviceaccounts), een referentie-specificatiebestand (`credspec`) op alle knooppunten in het cluster is geplaatst. Het bestand kan worden gekopieerd op alle knooppunten met behulp van een VM-extensie.  De `credspec` bestand moet bevatten de gegevens van de gMSA-account. Voor meer informatie over de `credspec` bestand, Zie [maken van een referentie-specificatie](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). De referentie-specificatie en de `Hostname` tag zijn opgegeven in het toepassingsmanifest. De `Hostname` tag moet overeenkomen met de naam van het gMSA-account dat de container wordt uitgevoerd onder.  De `Hostname` tag kunt u de container om zichzelf te verifiëren bij andere services in het domein met behulp van Kerberos-verificatie.  Een voorbeeld voor het opgeven van de `Hostname` en de `credspec` in de toepassing het manifest wordt weergegeven in het volgende codefragment:

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
