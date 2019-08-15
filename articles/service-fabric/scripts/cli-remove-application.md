---
title: Voorbeeldscript van Azure Service Fabric CLI (sfctl) voor verwijderen van toepassing
description: Hier leest u hoe u een toepassing verwijdert uit een Azure Service Fabric-cluster met behulp van de Azure Service Fabric-CLI
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 12/06/2017
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: faec3a519333eab1774989804002e339e509ebba
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035300"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Een toepassing verwijderen uit een Service Fabric-cluster

Met dit voorbeeldscript wordt een actief exemplaar van de Service Fabric-toepassing verwijderd waarna de registratie van een type en versie van de toepassing bij het cluster ongedaan wordt gemaakt.  Het verwijderen van het exemplaar van de toepassing betekent dat ook alle actieve service-exemplaren worden verwijderd die aan die toepassing zijn gekoppeld. Vervolgens worden de toepassingsbestanden verwijderd uit de installatiekopieopslag. 

Installeer indien nodig ook [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Voorbeeldscript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).
