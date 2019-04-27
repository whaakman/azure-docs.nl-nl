---
title: Service Fabric CLI-voorbeeldscript - bijwerken van een toepassing op een cluster
description: Service Fabric CLI-voorbeeldscript - bijwerken van een toepassing met een nieuwe versie. Dit voorbeeld voert ook een upgrade van een geïmplementeerde toepassing uit met de nieuwe bits.
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
origin.date: 12/06/2017
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: ffc60279ae414055c893c024d0ffd98267e6655f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621703"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Een toepassingscertificaat toevoegen aan een Service Fabric-cluster

Met dit voorbeeldscript wordt een nieuwe versie van een bestaande toepassing geüpload, en wordt vervolgens een geïmplementeerde toepassing bijgewerkt met de nieuwe bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Voorbeeldscript

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Upload the latest bits of an application
sfctl application upload --path ~/app_package_dir_2

# Provision the new application
sfctl application provision --application-type-build-path app_package_dir_2

# Upgrade an existing up with the new version
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).

<!--Update_Description: update meta properties -->