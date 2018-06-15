---
title: Azure-Stack-hulpprogramma's downloaden vanuit GitHub | Microsoft Docs
description: Informatie over het downloaden van de hulpprogramma's die vereist zijn voor het werken met Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: a116b7a048ff95ca601a65633cdc63f98fefee9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075286"
---
# <a name="download-azure-stack-tools-from-github"></a>Azure-Stack-hulpprogramma's vanuit GitHub downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

**AzureStack-Tools** is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules voor het beheren en implementeren van resources op Azure-Stack. Als u van plan bent om VPN-verbinding te maken, kunt u deze PowerShell-modules kunt downloaden op de Azure-Stack Development Kit, of op een externe Windows-client. U kunt deze hulpprogramma's, de GitHub-opslagplaats klonen of downloaden de **AzureStack-Tools** map het volgende script uit te voeren:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Functionaliteit van de modules

De **AzureStack-Tools** opslagplaats bevat PowerShell-modules die ondersteuning bieden voor Azure-Stack voor de volgende functies:  

| Functionaliteit | Beschrijving | wie deze module kunt gebruiken? |
| --- | --- | --- |
| [Cloud-mogelijkheden](user/azure-stack-validate-templates.md) | Gebruik deze module om op te halen van de cloud-mogelijkheden van een cloud. U kunt bijvoorbeeld de cloud-mogelijkheden, zoals API-versie en Azure Resource Manager-resources ophalen met behulp van deze module. U kunt ook de VM-extensies voor Azure-Stack en Azure-clouds ophalen met behulp van deze module. | Cloudoperators en gebruikers |
| [Resource Manager-beleid voor Azure-Stack](user/azure-stack-policy-module.md) | Gebruik deze module voor het configureren van een Azure-abonnement of een Azure-resourcegroep met de dezelfde versiebeheer en service-beschikbaarheid als Azure-Stack. | Cloudoperators en gebruikers |
| [Registreren bij Azure](azure-stack-register.md) | Gebruik deze module uw development kit exemplaar registreren bij Azure. U kunt na de registratie, downloaden de marketplace-items van Azure en ze in Azure-Stack gebruiken. | Cloudoperators |
| [Azure Stack-implementatie](azure-stack-run-powershell-script.md) | Gebruik deze module voor het voorbereiden van de Azure-Stack-hostcomputer implementeren en te implementeren met behulp van de installatiekopie van de virtuele harde schijf (VHD) Azure-Stack. | Cloudoperators|
| [Verbinding maken met Azure Stack](azure-stack-connect-powershell.md) | Gebruik deze module voor het configureren van VPN-verbinding met Azure-Stack. | Cloudoperators en gebruikers |
| [Validatie van de sjabloon](user/azure-stack-validate-templates.md) | Gebruik deze module om te controleren of als een bestaande of een nieuwe sjabloon kan worden toegepast op Azure-Stack. | Cloudoperators en gebruikers|


## <a name="next-steps"></a>Volgende stappen
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](user/azure-stack-powershell-configure-user.md)   
* [Via een VPN verbinding maken met Azure Stack Development Kit](azure-stack-connect-azure-stack.md)  
