---
title: Voorbeelden van Azure PowerShell - Service Fabric | Microsoft Docs
description: Voorbeelden van Azure PowerShell - Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: ''
tags: ''
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/29/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5d8137b2ca6c47713b7a681090963dd2749dd1f2
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634044"
---
# <a name="azure-powershell-samples"></a>Azure PowerShell-voorbeelden

De volgende tabel bevat koppelingen naar voorbeelden van PowerShell-scripts voor het maken en beheren van Service Fabric-clusters, -toepassingen en -services.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Cluster maken** ||
| [Een cluster maken (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Hiermee wordt een Azure Service Fabric-cluster gemaakt. |
| **Cluster, knooppunten en infrastructuur beheren** ||
| [Een toepassingscertificaat toevoegen](./scripts/service-fabric-powershell-add-application-certificate.md)| Hiermee wordt een X.509-certificaat toegevoegd aan alle knooppunten in een cluster. |
| [Het RDP-poortbereik bijwerken op cluster-VM's](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Hiermee wijzigt u het RDP-poortbereik op clusterknooppunt-VM's in een geïmplementeerd cluster.|
| [De gebruiker met beheerdersrechten en het wachtwoord bijwerken voor clusterknooppunt-VM's](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Hiermee worden de gebruiker met beheerdersrechten en het wachtwoord voor clusterknooppunt-VM's bijgewerkt. |
| [Een poort in de load balancer openen](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Open een toepassingspoort in de Azure-load balancer om inkomend verkeer via een specifieke poort toe te staan. |
| [Een regel voor inkomend verkeer voor een netwerkbeveiligingsgroep maken](./scripts/service-fabric-powershell-add-nsg-rule.md) | Maak een regel voor inkomend verkeer voor een netwerkbeveiligingsgroep om inkomend verkeer naar het cluster op een specifieke poort toe te staan. |
| **Toepassingen beheren** ||
| [Een app implementeren](./scripts/service-fabric-powershell-deploy-application.md)| Hiermee wordt een toepassing geïmplementeerd in een cluster.|
| [Een upgrade van een app uitvoeren](./scripts/service-fabric-powershell-upgrade-application.md)| Hiermee voert u een upgrade uit van een app.|
| [Een app verwijderen](./scripts/service-fabric-powershell-remove-application.md)| Hiermee verwijdert u een app uit een cluster.|
