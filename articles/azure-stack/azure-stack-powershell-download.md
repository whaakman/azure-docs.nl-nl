---
title: Hulpprogramma's voor Azure Stack downloaden vanuit GitHub | Microsoft Docs
description: Informatie over het downloaden van de hulpprogramma's die vereist zijn voor het werken met Azure Stack.
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 9eb8b3aefedb3e3330715408595a21596f33a413
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480936"
---
# <a name="download-azure-stack-tools-from-github"></a>Hulpprogramma's voor Azure Stack downloaden vanuit GitHub

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

**AzureStack-hulpprogramma's** is een [GitHub-opslagplaats](https://github.com/Azure/AzureStack-Tools) die als host fungeert voor PowerShell-modules voor het beheren en implementeren van resources met Azure Stack. Als u van plan bent om VPN-verbinding te maken, kunt u deze PowerShell-modules downloaden op de Azure Stack Development Kit, of op een externe Windows-client. U kunt deze hulpprogramma's, de GitHub-opslagplaats klonen of downloaden de **AzureStack-hulpprogramma's** map het volgende script uit te voeren:

```powershell
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

De **AzureStack-hulpprogramma's** opslagplaats bevat een PowerShell-modules die ondersteuning bieden voor de volgende functies voor Azure Stack:  

| Functionaliteit | Description | Wie kan deze module gebruiken? |
| --- | --- | --- |
| [Mogelijkheden van de cloud](user/azure-stack-validate-templates.md) | Gebruik deze module om op te halen van de mogelijkheden van de cloud van een cloud. Bijvoorbeeld, met behulp van deze module, krijgt u de mogelijkheden van de cloud, zoals API-versie en Azure Resource Manager-resources. U kunt ook de VM-extensies voor Azure Stack en Azure-clouds ophalen met behulp van deze module. | Cloudoperators en gebruikers |
| [Resource Manager-beleid voor Azure Stack](user/azure-stack-policy-module.md) | Gebruik deze module voor het configureren van een Azure-abonnement of een Azure-resourcegroep met de dezelfde versiebeheer en service-beschikbaarheid als Azure Stack. | Cloudoperators en gebruikers |
| [Registreren bij Azure](azure-stack-register.md) | Gebruik deze module voor het registreren van uw exemplaar van de kit ontwikkeling met Azure. Nadat u hebt geregistreerd, kunt u de marketplace-items van Azure downloaden en te gebruiken in Azure Stack. | Cloudoperators |
| [Azure Stack-implementatie](azure-stack-run-powershell-script.md) | Deze module gebruiken voor het voorbereiden van de Azure Stack-hostcomputer te implementeren en te implementeren met behulp van de installatiekopie van de virtuele harde schijf (VHD) Azure Stack. | Cloudoperators|
| [Verbinding maken met Azure Stack](azure-stack-connect-powershell.md) | Deze module gebruiken voor het configureren van VPN-verbinding met Azure Stack. | Cloudoperators en gebruikers |
| [Validatie van sjabloon](user/azure-stack-validate-templates.md) | Gebruik deze module om te controleren of als een bestaande of een nieuwe sjabloon kan worden geïmplementeerd met Azure Stack. | Cloudoperators en gebruikers|


## <a name="next-steps"></a>Volgende stappen
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](user/azure-stack-powershell-configure-user.md)   
* [Via een VPN verbinding maken met Azure Stack Development Kit](azure-stack-connect-azure-stack.md)  
