---
title: Status van de opslagopties in Azure Service Fabric NET | Microsoft Docs
description: Meer informatie over het opslaan van betrouwbare status in Service Fabric-NET-toepassingen die worden uitgevoerd op Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 6aa268cf56bfb8be9c27a9e0d9e5c9f4464b0c9d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076212"
---
# <a name="state-management-with-service-fabric"></a>Statusbeheer met Service Fabric
Service Fabric ondersteunt veel verschillende opties voor status-opslag. Zie voor een conceptueel overzicht van de status van beheer van patronen en Service Fabric, [concepten van Service Fabric: status](/azure/service-fabric/service-fabric-concepts-state). Deze dezelfde concepten gelden voor uw services worden uitgevoerd binnen of buiten het net van Service Fabric. 

## <a name="state-storage-options-in-azure-service-fabric-mesh"></a>Status van de opslagopties in Azure Service Fabric NET
Met Service Fabric NET, kunt u eenvoudig een nieuwe toepassing implementeren en deze verbinden met een bestaand gegevensarchief die wordt gehost in Azure. Naast het gebruik van een externe database, zijn er verschillende opties voor het opslaan van gegevens, afhankelijk van of de service wenst voor lokale of externe opslag. 

* Gerepliceerde gegevens lokaal opgeslagen
  * Betrouwbare verzamelingen (niet beschikbaar in preview)
    * Een bibliotheek waarmee gegevensstructuren zoals wachtrijen en -sleutel / waarde-paren te gebruiken in de service wordt geïmplementeerd
    * Dit biedt de eenvoudigste en snelste manier om te communiceren met gegevens, terwijl er eenvoudig partitie routering in combinatie met intelligente routering in Service Fabric NET
  * Service Fabric volume stuurprogramma (niet beschikbaar in preview)
    * Een docker-volume-stuurprogramma om te koppelen van een lokaal volume naar een container
    * Dit biedt u de ultieme flexibiliteit bij het opslaan van gegevens lokaal, via een API die ondersteuning biedt voor bestandsopslag.

* Externe opslag
  * Azure bestanden volume-stuurprogramma
    * Een Azure Files-share koppelen voor een container van een docker volume stuurprogramma
    * Biedt u een minder prestaties, maar ook volledige goedkoper flexibele en betrouwbare optie, via een API die ondersteuning biedt voor bestandsopslag.
    * [Gebruiksaanwijzing: een app implementeert met Azure Files-volume](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)
    
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toepassingsmodel [Service Fabric-resources](service-fabric-mesh-service-fabric-resources.md)
