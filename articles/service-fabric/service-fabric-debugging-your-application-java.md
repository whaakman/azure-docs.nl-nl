---
title: Fouten opsporen in uw Azure Service Fabric-toepassing in Eclipse | Microsoft Docs
description: De betrouwbaarheid en prestaties van uw services verbeteren door de ontwikkeling en foutopsporing ze in Eclipse op een lokaal ontwikkelcluster.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: timlt
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 0e9e816fa84816b1b5d12f066dc65aee7b4930f7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205450"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Fouten opsporen in uw Eclipse met Java Service Fabric-toepassing
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Een lokaal ontwikkelcluster starten door de stappen in [instellen van uw ontwikkelomgeving Service Fabric](service-fabric-get-started-linux.md).

2. Werk entryPoint.sh van de service die u opsporen in, wilt zodat deze de java-proces met de parameters voor foutopsporing op afstand begint. Dit bestand kan worden gevonden op de volgende locatie: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Poort 8001 is in dit voorbeeld voor foutopsporing ingesteld.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Werk het Manifest van de toepassing door het instellen van het aantal exemplaren of het aantal replica's voor de service die foutopsporing wordt uitgevoerd op 1. Deze instelling voorkomt conflicten voor de poort die wordt gebruikt voor het opsporen van fouten. Stel bijvoorbeeld voor stateless services ``InstanceCount="1"`` in en stel voor stateful services de doel- en min-replicasetgrootten als volgt in op 1: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Implementeer de toepassing.

5. Selecteer in de IDE Eclipse **Run -> fouten opsporen in configuraties -> externe Java-toepassing en voer de verbindingseigenschappen** en stel de eigenschappen als volgt:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Onderbrekingspunten instellen op de gewenste plaatsen en fouten opsporen in de toepassing.

Als de toepassing is gecrasht, kunt u ook coredumps inschakelen. Uitvoeren van ``ulimit -c`` in een shell en als het resultaat 0, wordt de coredumps zijn niet ingeschakeld. Voer de volgende opdracht zodat onbeperkte coredumps: ``ulimit -c unlimited``. U kunt ook de status van de opdracht controleren ``ulimit -a``.  Als u wilt dat het pad van de generatie coredump bijwerken, uitvoermachtigingen ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Volgende stappen

* [Verzamelen van logboeken met diagnostische gegevens van Linux Azure](service-fabric-diagnostics-how-to-setup-lad.md).
* [Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
