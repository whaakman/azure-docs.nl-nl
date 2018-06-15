---
title: Azure Service Fabric-omgevingsvariabelen | Microsoft Docs
description: -Naslagdocumentatie voor Service Fabric-omgevingsvariabelen
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: f7c36fec7ff58c225e41899e8264ca1dde95ce7c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213245"
---
# <a name="service-fabric-environment-variables"></a>Service Fabric-omgevingsvariabelen

Service Fabric heeft ingebouwde omgevingsvariabelen ingesteld voor elk service-exemplaar. De volledige lijst met omgevingsvariabelen lager is dan:

| Omgevingsvariabele                         | Beschrijving                                                            | Voorbeeld                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | De fabric-uri-naam van de toepassing                                 | fabric: / Mijntoepassing                                                |
| Fabric_CodePackageName                       | De naam van het codepakket waartoe het proces behoort              | Code                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | De IP-adres of FQDN-naam van het eindpunt                                 | 10.0.0.1                                                     |
| Fabric\_Endpoint\_*ServiceEndpointName*              | Poortnummer op voor het eindpunt                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Map voor het logboek                                                             | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\log      |
| Fabric_Folder_App_Temp                       | Tijdelijke map                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Werkmappen                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\work     |
| Fabric_Folder_Application                    | De basismap van toepassingen                                           | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | Een Boole-waarde die aangeeft of het proces een container is                   | false                                                                |
| Fabric_NodeId                                | De knooppunt-ID van het knooppunt waarop het proces wordt uitgevoerd                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | Het IP of FQDN van het knooppunt, zoals opgegeven in het cluster manifestbestand. | localhost of 10.0.0.1                                                |
| Fabric_NodeName                              | De naam van het knooppunt van het knooppunt waarop het proces wordt uitgevoerd                          | _Node_0                                                              |
| Fabric_ServiceName                           | De naam van de service, als deze service wordt gehost in de modus ExclusiveProcess. De waarde van deze variabele is alleen beschikbaar als u de service met ServicePackageActivationMode ExclusiveProcess maakt.  | MyService                                               |
| Fabric_ServicePackageActivationId            | De ServicePackageActivationId                                         | EEN GUID                                                               |
| Fabric_ServicePackageName                    | Naam van het pakket met de service het proces maakt deel uit van                     | Web1Pkg                                                              |

Interne omgevingsvariabelen die door de Service Fabric-Runtime:

- Fabric_ApplicationHostId
- Fabric_ApplicationHostType
- Fabric_ApplicationId
- Fabric_CodePackageInstanceId
- Fabric_CodePackageInstanceSeqNum
- Fabric_RuntimeConnectionAddress
- Fabric_ServicePackageActivationGuid
- Fabric_ServicePackageInstanceId
- Fabric_ServicePackageInstanceSeqNum
- Fabric_ServicePackageVersionInstance
- FabricActivatorAddress
- FabricPackageFileName
- HostedServiceName