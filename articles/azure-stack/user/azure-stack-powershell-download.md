---
title: Azure-Stack-hulpprogramma's downloaden vanuit GitHub | Microsoft Docs
description: Informatie over het downloaden van de hulpprogramma's voor het werken met Azure-Stack.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: 69ca030d1b7601df424fa9446e1d194a3f6bd50a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Azure-Stack-hulpprogramma's vanuit GitHub downloaden

AzureStack-hulpprogramma's is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules die u gebruiken kunt om te beheren en implementeren van resources voor Azure-Stack. U kunt downloaden en deze PowerShell-modules op de Azure-Stack Development Kit, of op een externe windows-client gebruiken als u van plan bent om VPN-verbinding te maken. U kunt deze hulpprogramma's, de GitHub-opslagplaats klonen of downloaden van de map AzureStack-hulpprogramma's. 

Download de opslagplaats klonen, [Git](https://git-scm.com/download/win) voor Windows, open een opdrachtpromptvenster en voer het volgende script:

```PowerShell
# Change directory to the root directory 
cd \

# clone the repository
git clone https://github.com/Azure/AzureStack-Tools.git --recursive

# Change to the tools directory
cd AzureStack-Tools
```

Voer het volgende script voor het downloaden van de map hulpprogramma's:

```PowerShell
# Change directory to the root directory 
cd \

# Download the tools archive
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
