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
ms.openlocfilehash: f9de8d213d11a8ccb3ffff484a67560d9e2abe77
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
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

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende artikelen voor de volgende stappen:
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
