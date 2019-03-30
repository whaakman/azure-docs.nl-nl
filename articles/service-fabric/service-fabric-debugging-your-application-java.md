---
title: Fouten opsporen in uw Azure Service Fabric-toepassing in Eclipse | Microsoft Docs
description: De betrouwbaarheid en prestaties van uw services verbeteren door de ontwikkeling en foutopsporing van hen in Eclipse op een lokaal ontwikkelcluster.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 8f0470b10589ecbbc9e2c98e8d3445435e7f8ed4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668820"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Fouten opsporen in uw Service Fabric Java-toepassing met behulp van Eclipse
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Een lokaal ontwikkelcluster starten met de volgende stappen in [instellen van uw Service Fabric-ontwikkelomgeving](service-fabric-get-started-linux.md).

2. Werk entryPoint.sh van de service die u fouten opsporen, wilt zodat deze het java-proces wordt gestart met de externe foutopsporing voor parameters. Dit bestand kan worden gevonden op de volgende locatie: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Poort 8001 is in dit voorbeeld voor foutopsporing ingesteld.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Manifest van de toepassing bij te werken als u het aantal instanties of het aantal replica's voor de service die foutopsporing wordt uitgevoerd op 1. Deze instelling voorkomt conflicten voor de poort die wordt gebruikt voor het opsporen van fouten. Stel bijvoorbeeld voor stateless services ``InstanceCount="1"`` in en stel voor stateful services de doel- en min-replicasetgrootten als volgt in op 1: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. De toepassing implementeren.

5. Selecteer in de Eclipse IDE **Run -> Debug Configurations -> Remote Java Application en voer de verbindingseigenschappen** en stel de eigenschappen als volgt:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Onderbrekingspunten instellen op de gewenste plaatsen en fouten opsporen in de toepassing.

Als de toepassing is gecrasht, wilt u mogelijk ook coredumps inschakelen. Voer ``ulimit -c`` in een shell en als het resultaat 0, wordt de coredumps zijn niet ingeschakeld. Om in te schakelen onbeperkte coredumps, dan de volgende opdracht: ``ulimit -c unlimited``. U kunt ook controleren of de status van de opdracht ``ulimit -a``.  Als u bijwerken van het pad van de generatie coredump wilt, voeren ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Volgende stappen

* [Logboeken verzamelen met Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).
* [Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
