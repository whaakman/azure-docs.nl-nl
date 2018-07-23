---
title: Hulpprogramma's voor Azure Stack downloaden vanuit GitHub | Microsoft Docs
description: Informatie over het downloaden van de hulpprogramma's die zijn vereist voor het werken met Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.author: mabrigg
ms.openlocfilehash: 9c1e4abe50b06db58a4ca05a99e1ae4a531b2294
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187448"
---
# <a name="download-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

AzureStack-hulpprogramma's is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules die u gebruiken kunt om te beheren en resources implementeren op Azure Stack.

## <a name="download-targets"></a>Doelen downloaden

U kunt downloaden en gebruiken van deze PowerShell-modules op Azure Stack Development Kit, of naar een externe Windows-client die gebruikmaakt van een VPN-verbinding.

## <a name="how-to-get-the-tools"></a>Hoe u de hulpprogramma's downloaden

Als u deze hulpprogramma's, de AzureStack-hulpprogramma's-GitHub-opslagplaats klonen of downloaden van de map AzureStack-hulpprogramma's voor het volgende script uit te voeren:

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

De opslagplaats AzureStack-hulpprogramma's bevat PowerShell-modules die ondersteuning bieden voor de volgende functies voor Azure Stack:

| Functionaliteit | Beschrijving | Wie kan deze module gebruiken? |
| --- | --- | --- |
| [Mogelijkheden van de cloud](https://github.com/Azure/AzureStack-Tools/tree/master/CloudCapabilities) | Gebruik deze module om op te halen van de mogelijkheden van de cloud van een cloud. Bijvoorbeeld, krijgt u de mogelijkheden van de cloud, zoals API-versie, Azure Resource Manager-resources, enz. VM-extensies voor Azure Stack en Azure-clouds, met behulp van deze module. | Cloudbeheerders en gebruikers. |
| [Resource Manager-beleid voor Azure Stack](azure-stack-policy-module.md) | Gebruik deze module voor het configureren van een Azure-abonnement of een Azure-resourcegroep met de dezelfde versiebeheer en service-beschikbaarheid als Azure Stack. | Cloudbeheerders en gebruikers |
| [Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md) | Gebruik deze module verbinding maken met een Azure Stack-exemplaar via PowerShell en het configureren van VPN-verbinding met Azure Stack. | Cloudbeheerders en gebruikers |
| [Validatie van sjabloon](azure-stack-validate-templates.md) | Gebruik deze module om te controleren of als een bestaande of een nieuwe sjabloon kan worden geïmplementeerd met Azure Stack. | Cloudbeheerders en gebruikers |

## <a name="next-steps"></a>Volgende stappen

- [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)
- [Via een VPN verbinding maken met Azure Stack Development Kit](azure-stack-connect-azure-stack.md)
