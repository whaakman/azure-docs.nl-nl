---
title: Azure Service Fabric-omgevingsvariabelen | Microsoft Docs
description: Referentiedocumentatie voor Service Fabric-omgevingsvariabelen
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
ms.openlocfilehash: 1c8400898dba59f312ba9d994ee711a5e241973a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60946719"
---
# <a name="service-fabric-environment-variables"></a>Service Fabric-omgevingsvariabelen

Service Fabric heeft ingebouwde omgevingsvariabelen instellen voor elke service-exemplaar. De volledige lijst van omgevingsvariabelen lager is dan:

| Omgevingsvariabele                         | Description                                                            | Voorbeeld                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | De naam van de fabric-uri van de toepassing                                 | fabric: / Mijntoepassing                                                |
| Fabric_CodePackageName                       | De naam van het codepakket met die het proces behoort              | Code                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | De ip-adres of FQDN-naam van het eindpunt                                 | 10.0.0.1                                                     |
| Fabric\_Endpoint\_*ServiceEndpointName*              | Poortnummer voor het eindpunt                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Logboekmap                                                             | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\log      |
| Fabric_Folder_App_Temp                       | Tijdelijke map                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Werkmap                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\work     |
| Fabric_Folder_Application                    | De basismap van toepassingen                                           | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | Een Boole-waarde op te geven of het proces een container is                   | false                                                                |
| Fabric_NodeId                                | Het knooppunt-ID van het knooppunt waarop het proces wordt uitgevoerd                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | De IP- of FQDN-naam van het knooppunt, zoals opgegeven in het cluster-manifestbestand. | localhost of 10.0.0.1                                                |
| Fabric_NodeName                              | De naam van het knooppunt van het knooppunt waarop het proces wordt uitgevoerd                          | _Node_0                                                              |
| Fabric_ServiceName                           | De fabric-uri-naam van de service, als service wordt gehost in de modus ExclusiveProcess. De waarde van deze variabele is alleen beschikbaar als u de service ServicePackageActivationMode ExclusiveProcess maakt.  | fabric: / Mijntoepassing/MijnService                                               |
| Fabric_ServicePackageActivationId            | The ServicePackageActivationId                                         | A GUID                                                               |
| Fabric_ServicePackageName                    | Naam van het pakket met de service het proces maakt deel uit van                     | Web1Pkg                                                              |

Interne omgevingsvariabelen die worden gebruikt door de Service Fabric-Runtime:

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
