---
title: Status van de opslagopties in Azure Service Fabric NET | Microsoft Docs
description: Meer informatie over het opslaan van betrouwbare status in Service Fabric-NET-toepassingen die worden uitgevoerd op Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 11/27/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 81d9fc7babef7c1d14a0acc420d0c16c05b83215
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354326"
---
# <a name="state-management-with-service-fabric"></a>Statusbeheer met Service Fabric

Service Fabric ondersteunt veel verschillende opties voor status-opslag. Zie voor een conceptueel overzicht van de status van beheer van patronen en Service Fabric, [Service Fabric-concepten: Status](/azure/service-fabric/service-fabric-concepts-state). Deze dezelfde concepten gelden voor uw services worden uitgevoerd binnen of buiten het net van Service Fabric. 

Met Service Fabric NET, kunt u eenvoudig een nieuwe toepassing implementeren en deze verbinden met een bestaand gegevensarchief die wordt gehost in Azure. Naast het gebruik van een externe database, zijn er verschillende opties voor het opslaan van gegevens, afhankelijk van of de service wenst voor lokale of externe opslag. 

## <a name="volumes"></a>Volumes

Containers maken vaak gebruik van tijdelijke schijven. Tijdelijke schijven zijn echter kortstondige, zodat u een nieuwe tijdelijke schijf ophalen en de gegevens gaan verloren wanneer een container vastloopt. Het is ook moeilijk om gegevens op schijven met tijdelijke delen met andere containers. Volumes zijn mappen die in uw containerinstanties die u gebruiken kunt om vast te leggen de status worden gekoppeld. Volumes bieden u de opslag voor algemeen gebruik en kunnen u bestanden met behulp van API's van een normale schijf i/o-bestand voor lezen/schrijven. De volumeresource beschrijft het koppelen van een map en welke back-ups opslag te gebruiken. U kunt Azure File storage of Service Fabric-Volume-schijf voor het opslaan van gegevens.

![Volumes][image3]

### <a name="service-fabric-reliable-volume"></a>Service Fabric betrouwbare Volume

Service Fabric betrouwbare Volume is een Docker-volume-stuurprogramma gebruikt voor het koppelen van een lokaal volume naar een container. Lees- en schrijfbewerkingen zijn lokale bewerkingen en fast. Gegevens worden gerepliceerd uit voor secundaire knooppunten, maximaal beschikbaar maken. Failover is ook snel. Wanneer een container vastloopt, failover het-schakeling naar een knooppunt dat al een kopie van uw gegevens. Zie voor een voorbeeld [over het implementeren van een app met Service Fabric betrouwbare Volume](service-fabric-mesh-howto-deploy-app-sfreliable-disk-volume.md).

### <a name="azure-files-volume"></a>Azure Files-Volume

Azure-bestanden Volume is een Docker-volume-stuurprogramma gebruikt voor het koppelen van een Azure-bestandsshare naar een container. Azure Files storage maakt gebruik van netwerkopslag, dus lees- en schrijfbewerkingen plaatsvinden via het netwerk. Vergeleken met de betrouwbare Service Fabric-Volume, Azure File storage is minder goed presterende, maar biedt ook de gegevensoptie goedkoper en volledig betrouwbare. Zie voor een voorbeeld [over het implementeren van een app met Azure-bestanden Volume](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toepassingsmodel [Service Fabric-resources](service-fabric-mesh-service-fabric-resources.md)

[image3]: ./media/service-fabric-mesh-storing-state/volumes.png
