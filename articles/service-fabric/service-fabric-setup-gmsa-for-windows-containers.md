---
title: GMSA instellen voor Azure Service Fabric container Services | Microsoft Docs
description: Meer informatie over het instellen van gMSA voor een container die wordt uitgevoerd in azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: athinanthny
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
ms.openlocfilehash: 09994c7676de8470efff1707598ddf32a48e41a0
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599181"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>GMSA instellen voor Windows-containers die worden uitgevoerd op Service Fabric

Voor het instellen van gMSA (door een groep beheerde service accounts), wordt een`credspec`referentie-specificatie bestand () op alle knoop punten in het cluster geplaatst. Het bestand kan worden gekopieerd op alle knoop punten met behulp van een VM-extensie.  Het `credspec` bestand moet de gMSA-account gegevens bevatten. Zie `credspec` [een referentie specificatie maken](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec)voor meer informatie over het bestand. De referentie specificatie en de `Hostname` -tag worden opgegeven in het manifest van de toepassing. Het `Hostname` label moet overeenkomen met de naam van het gMSA-account dat de container wordt uitgevoerd.  Met `Hostname` de tag kan de container zichzelf verifiëren bij andere services in het domein met behulp van Kerberos-verificatie.  In het volgende code fragment `Hostname` wordt een `credspec` voor beeld gegeven voor het opgeven en de in het manifest van de toepassing:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Lees de volgende artikelen als volgende stap:

* [Een Windows-container implementeren voor het Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Een docker-container implementeren voor Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
