---
title: Voorbeelden van Azure PowerShell - Service Fabric | Microsoft Docs
description: Voorbeelden van Azure PowerShell - Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: 
tags: 
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 17b10011af915a06e7c65f8d321a1e452956c28a
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="azure-powershell-samples"></a>Azure PowerShell-voorbeelden

De volgende tabel bevat koppelingen naar voorbeelden van de PowerShell-scripts die maken en beheren van Service Fabric-clusters, toepassingen en services.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Cluster maken** ||
| [Maak een cluster (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Een Azure Service Fabric-cluster maakt. |
| **Cluster, knooppunten en infrastructuur beheren** ||
| [Het toepassingscertificaat van een toevoegen](./scripts/service-fabric-powershell-add-application-certificate.md)| Voegt een X.509-certificaat van toepassing op alle knooppunten in een cluster. |
| [Het RDP-poortbereik op cluster virtuele machines bijwerken](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Hiermee wijzigt u het RDP-poortbereik op clusterknooppunt virtuele machines in een geïmplementeerde cluster.|
| [Bijwerken van de gebruiker met beheerdersrechten en het wachtwoord voor het knooppunt VM 's](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | De gebruikersnaam van de beheerder en het wachtwoord voor clusterknooppunt VMs-updates. |
| [Een poort in de load balancer openen](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Open de poort van een toepassing in de Azure load balancer waarmee binnenkomend verkeer op een specifieke poort. |
| [Maken van een binnenkomende groep van de netwerkbeveiligingsregel](./scripts/service-fabric-powershell-add-nsg-rule.md) | Maak een binnenkomende groep netwerkbeveiligingsregel dat binnenkomend verkeer naar het cluster op een specifieke poort toegestaan. |
| **Toepassingen beheren** ||
| [Een toepassing implementeren](./scripts/service-fabric-powershell-deploy-application.md)| Een toepassing implementeert op een cluster.|
| [Upgrade van een toepassing](./scripts/service-fabric-powershell-upgrade-application.md)| Bijwerken van een toepassing.|
| [Een toepassing verwijderen](./scripts/service-fabric-powershell-remove-application.md)| Een toepassing verwijderen uit een cluster.|
