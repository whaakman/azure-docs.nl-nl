---
title: Azure PowerShell-Script steekproef - certificaat van de toepassing toevoegen aan een cluster | Microsoft Docs
description: 'Azure PowerShell-Script voorbeeld: een toepassingscertificaat toevoegen aan een Service Fabric-cluster.'
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/29/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 8a000d797c3bd10606d297ed8da67229fe0c8a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Het toepassingscertificaat van een toevoegen aan een Service Fabric-cluster

Dit voorbeeldscript wordt een zelfondertekend certificaat gemaakt in de opgegeven Azure sleutelkluis en installeert deze met alle knooppunten van het Service Fabric-cluster. Het certificaat kan ook worden gedownload naar een lokale map. De naam van het gedownloade certificaat is dezelfde als de naam van het certificaat in de sleutelkluis. Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure. 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten: elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Voeg AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Een nieuw toepassingscertificaat toevoegen aan de virtuele-machineschaalset waaruit het cluster.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
