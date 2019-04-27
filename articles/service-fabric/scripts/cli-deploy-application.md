---
title: Voorbeeld van de implementatie van Azure Service Fabric CLI (sfctl) met behulp van een script
description: Een toepassing implementeren naar een Azure Service Fabric-cluster met Azure Service Fabric CLI
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
origin.date: 04/16/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: f3ab32101a46c1044954f2efe88ac05ab81af24a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621995"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Een toepassing implementeren naar een Service Fabric-cluster

Met dit voorbeeldscript wordt een toepassingspakket naar een installatieopslag van een cluster gekopieerd, wordt het toepassingstype in het cluster geregistreerd en wordt een instantie van het toepassingstype gemaakt. Dit is het moment waarop ook alle eventuele standaardservices worden gemaakt.

Installeer indien nodig ook [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Voorbeeldscript

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Upload the application files to the image store
# (note the last folder name, Debug in this example)
sfctl application upload \
    --path  C:\Code\svcfab-vs\svcfab-vs\pkg\Debug \
    --show-progress

# Register the application (manifest files) from the image store
# (Note the last folder from the previous command is used: Debug)
sfctl application provision \
    --application-type-build-path Debug \
    --timeout 500

# Create an instance of the registered application and 
# auto deploy any defined services
sfctl application create \
    --app-name fabric:/MyApp \
    --app-type MyAppType \
    --app-version 1.0.0

```

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Wanneer dit is gebeurd, kan het [verwijderscript](cli-remove-application.md) worden gebruikt om de toepassing te verwijderen. Met het verwijderscript wordt de instantie van de toepassing verwijderd, de registratie van het toepassingstype ongedaan gemaakt en het toepassingspakket verwijderd uit de installatiekopieopslag.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).

<!--Update_Description: update meta properties, update link -->