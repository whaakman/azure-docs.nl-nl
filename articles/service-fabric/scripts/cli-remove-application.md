---
title: Voorbeeldscript van Azure Service Fabric CLI (sfctl) voor verwijderen van toepassing
description: Hier leest u hoe u een toepassing verwijdert uit een Azure Service Fabric-cluster met behulp van de Azure Service Fabric-CLI
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
origin.date: 12/06/2017
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 4e0f4f7f4b8bfc643f0680b7fa1d1fc5b063eb5a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621961"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Een toepassing verwijderen uit een Service Fabric-cluster

Met dit voorbeeldscript wordt een actief exemplaar van de Service Fabric-toepassing verwijderd waarna de registratie van een type en versie van de toepassing bij het cluster ongedaan wordt gemaakt.  Het verwijderen van het exemplaar van de toepassing betekent dat ook alle actieve service-exemplaren worden verwijderd die aan die toepassing zijn gekoppeld. Vervolgens worden de toepassingsbestanden verwijderd uit de installatiekopieopslag. 

Installeer indien nodig ook [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Voorbeeldscript

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Delete the application
sfctl application delete \
    --application-id svcfab_app \
    --timeout 500

# Unprovision the application type
sfctl application unprovision \
    --application-type-name svcfab_appType \
    --application-type-version 1.0.0 \
    --timeout 500

# Delete the application files from the image store
sfctl store delete \
    --content-path myappfolder
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).

<!--Update_Description: update meta properties, update link -->