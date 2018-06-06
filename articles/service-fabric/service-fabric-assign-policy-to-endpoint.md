---
title: Toegangsbeleid toewijzen aan Azure Service Fabric-service-eindpunten | Microsoft Docs
description: Informatie over het toewijzen van beveiliging beleid toegang aan HTTP of HTTPS-eindpunten in uw Service Fabric-service.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 33d6d83d4dcd0ec9bebf8d4197684e0e52c48ac5
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701315"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten
Als u een run as-beleid toepassen en de servicemanifest HTTP-eindpunt bronnen declareert, moet u een **SecurityAccessPolicy**.  **SecurityAccessPolicy** zorgt ervoor dat de poorten die zijn toegewezen aan deze eindpunten correct zijn beperkt tot het gebruikersaccount dat de service wordt uitgevoerd als. Anders **http.sys** geen toegang tot de service en ophalen van fouten met aanroepen van de client. Het volgende voorbeeld wordt het account Customer1 naar een eindpunt aangeroepen **EndpointName**, waardoor het volledige toegangsrechten.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Voor een HTTPS-eindpunt ook duiden op de naam van het certificaat om terug te keren naar de client. U verwijst naar het certificaat met behulp van **EndpointBindingPolicy**.  Het certificaat is gedefinieerd in de **certificaten** sectie van het toepassingsmanifest.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```

> [!WARNING] 
> Wanneer u HTTPS gebruikt, gebruik niet dezelfde poort en certificaat voor andere service-exemplaren (onafhankelijk van de toepassing) geïmplementeerd op hetzelfde knooppunt. Upgraden van twee verschillende services met behulp van dezelfde poort in verschillende toepassingsexemplaren leidt tot een upgrade mislukt. Zie voor meer informatie [bijwerken van meerdere toepassingen met HTTPS-eindpunten ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende artikelen voor de volgende stappen:
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
