---
title: Azure-Stack-hulpprogramma's downloaden vanuit GitHub | Microsoft Docs
description: Informatie over het downloaden van de hulpprogramma's voor het werken met Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 28F360AD-789A-488D-965F-FC6E6CCF3329
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: f4c4d907fe76c5e6ee5893b5b4dd1b2d766877a7
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287250"
---
# <a name="download-azure-stack-tools-from-github"></a>Azure-Stack-hulpprogramma's vanuit GitHub downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

AzureStack-hulpprogramma's is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules die u gebruiken kunt om te beheren en implementeren van resources voor Azure-Stack.

## <a name="download-targets"></a>Doelen downloaden

U kunt downloaden en gebruiken van deze PowerShell-modules Azure Stack Development Kit of naar een externe Windows-client die gebruikmaakt van een VPN-verbinding.

## <a name="how-to-get-the-tools"></a>Het ophalen van de hulpprogramma 's

Als u deze hulpprogramma's, de AzureStack-hulpprogramma's voor GitHub-opslagplaats klonen of downloaden van de map AzureStack-hulpprogramma's voor het volgende script uit te voeren:

```PowerShell
# Change directory to the root directory
cd \

# Download the tools archive
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>Functies die worden geleverd door de modules

De opslagplaats AzureStack-hulpprogramma's bevat PowerShell-modules die ondersteuning bieden voor Azure-Stack voor de volgende functies:

| Functionaliteit | Beschrijving | wie deze module kunt gebruiken? |
| --- | --- | --- |
| [Cloud-mogelijkheden](azure-stack-validate-templates.md) | Gebruik deze module om op te halen van de cloud-mogelijkheden van een cloud. Bijvoorbeeld, krijgt u de mogelijkheden van de cloud zoals API-versie, Azure Resource Manager-resources, enz. VM-extensies voor Azure-Stack en Azure cloud met behulp van deze module. | Cloudbeheerders en gebruikers. |
| [Resource Manager-beleid voor Azure-Stack](azure-stack-policy-module.md) | Gebruik deze module voor het configureren van een Azure-abonnement of een Azure-resourcegroep met de dezelfde versiebeheer en service-beschikbaarheid als Azure-Stack. | Cloudbeheerders en gebruikers |
| [Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md) | Gebruik deze module verbinding maken met een exemplaar netwerkstack Azure via PowerShell en het configureren van VPN-verbinding met Azure-Stack. | Cloudbeheerders en gebruikers |
| [Validatie van de sjabloon](azure-stack-validate-templates.md) | Gebruik deze module om te controleren of als een bestaande of een nieuwe sjabloon kan worden toegepast op Azure-Stack. | Cloudbeheerders en gebruikers |

## <a name="next-steps"></a>Volgende stappen

* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)
* [Via een VPN verbinding maken met Azure Stack Development Kit](azure-stack-connect-azure-stack.md)
