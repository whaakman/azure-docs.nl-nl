---
title: Fouten opsporen in uw Azure Service Fabric-toepassing in Eclipse | Microsoft Docs
description: De betrouwbaarheid en prestaties van uw services verbeteren door de ontwikkeling en foutopsporing ze in Eclipse op een lokaal ontwikkelcluster.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: f3bcee3794de35005bd387ecfae7e6707f3cb5ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Fouten opsporen in uw Eclipse met Java Service Fabric-toepassing
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Een lokaal ontwikkelcluster starten door de stappen in [instellen van uw ontwikkelomgeving Service Fabric](service-fabric-get-started-linux.md).

2. Werk entryPoint.sh van de service die u opsporen in, wilt zodat deze de java-proces met de parameters voor foutopsporing op afstand begint. Dit bestand kan worden gevonden op de volgende locatie: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Poort 8001 is voor foutopsporing in dit voorbeeld ingesteld.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Werk het Manifest van de toepassing door het instellen van het aantal exemplaren of het aantal replica's voor de service die foutopsporing wordt uitgevoerd op 1. Deze instelling voorkomt de poort die wordt gebruikt voor het opsporen van conflicten. Bijvoorbeeld voor stateless services instellen ``InstanceCount="1"`` en voor stateful services het doel heeft en min replica grootten ingesteld op 1 als volgt: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

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
