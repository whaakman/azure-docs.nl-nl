---
title: Azure Service Fabric CLI Script voorbeeld implementeren
description: Een toepassing implementeert op een Azure Service Fabric-cluster met de Azure Service Fabric-CLI
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: a9b7b2e3a8355aa0da0069bd27d2f61d8b5b8028
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Een toepassing naar een Service Fabric-cluster implementeren

Dit voorbeeldscript toepassingspakketten kopieert naar een cluster image store, registreert het toepassingstype in het cluster en maakt een toepassingsexemplaar van het toepassingstype. Alle services die standaard worden ook op dit moment gemaakt.

Installeer zo nodig de [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Voorbeeld van een script

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Wanneer u klaar bent, de [verwijderen](cli-remove-application.md) script kan worden gebruikt om de toepassing te verwijderen. Het script verwijderen worden verwijderd van het toepassingsexemplaar, heft de registratie van het toepassingstype en het toepassingspakket verwijdert uit de image store.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie de [Service Fabric CLI documentatie](../service-fabric-cli.md).

Extra Service Fabric CLI steekproeven voor de Azure Service Fabric vindt u in de [Service Fabric CLI voorbeelden](../samples-cli.md).
