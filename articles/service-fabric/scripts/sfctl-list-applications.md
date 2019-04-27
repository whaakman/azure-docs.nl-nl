---
title: Voorbeeld van Service Fabric CLI-script - toepassingen in een cluster opvragen
description: Voorbeeld van Service Fabric CLI-script - toepassingen opvragen die zijn ingericht in een Service Fabric-cluster.
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
origin.date: 04/13/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: 8fd83190f3cf92ef0f88ff0fb2a807e03199a5c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621927"
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>Toepassingen opvragen die worden uitgevoerd in een Service Fabric-cluster

Dit voorbeeldscript maakt verbinding met een Service Fabric-cluster en geeft vervolgens een lijst weer met alle ingerichte toepassingen.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Voorbeeldscript

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Retrieve all applications from the cluster
sfctl application list
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).
<!--Update_Description: update meta properties -->