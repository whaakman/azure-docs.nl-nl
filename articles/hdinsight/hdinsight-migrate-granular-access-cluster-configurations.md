---
title: Wijzigingen in de configuratie van access - Azure HDInsight-cluster
description: Meer informatie over de wijzigingen aan gevoelige cluster configuratievelden openen.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 85a6737ee111975d4c7ecf078673280127bfd0fd
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59610212"
---
# <a name="changes-to-cluster-configuration-access"></a>Wijzigingen in de configuratie van toegang tot het cluster

De nieuwste versie van de SDK van Azure HDInsight biedt enkele belangrijke wijzigingen voor de ondersteuning van meer fijnmazig rollen gebaseerde toegang om gevoelige informatie te verkrijgen. Als onderdeel van deze, sommige wijzigingen **actie is mogelijk vereist** als u een van de betrokken methoden.

## <a name="sdk-for-net-500"></a>SDK voor .NET 5.0.0

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) wordt **retourneren niet langer gevoelige parameters** , zoals opslagsleutels (core-site) of HTTP-referenties (gateway).
    - Gebruiken om op te halen van alle configuraties, met inbegrip van gevoelige parameters `ConfigurationOperationsExtensions.List` voortaan.  Houd er rekening mee dat gebruikers met de rol 'Lezer' is niet mogelijk om deze methode te gebruiken. Hierdoor nauwkeurige controle over welke gebruikers toegang gevoelige gegevens voor een cluster tot. 
    - Als u wilt alleen HTTP-gateway-referenties ophalen, gebruikt u `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) nu is afgeschaft en vervangen door `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) en [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) zijn afgeschaft. HTTP is nu altijd ingeschakeld, zodat deze methoden zijn niet meer nodig hebt.