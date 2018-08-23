---
title: Met behulp van API-versieprofielen met PowerShell in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met PowerShell in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 8da02641cc54f9308f8e0bbb8d2b28da9a930aa2
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060782"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>API-versieprofielen gebruiken voor PowerShell in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

API-versieprofielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure Stack. Een API-versie-profiel is een set van AzureRM PowerShell-modules met specifieke API-versies. Elk cloudplatform bevat een set ondersteunde API-versieprofielen. Bijvoorbeeld, Azure Stack biedt ondersteuning voor een specifieke datum Profielversie zoals **2017-03-09-profiel**, en biedt ondersteuning voor Azure de **nieuwste** API-versie-profiel. Wanneer u een profiel installeert, worden de AzureRM PowerShell-modules die met het opgegeven profiel overeenkomen geïnstalleerd.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installeer de PowerShell-module vereist voor het gebruik van API-versieprofielen

De **AzureRM.Bootstrapper** -module die is beschikbaar via de PowerShell Gallery biedt PowerShell-cmdlets die nodig zijn voor het werken met API-versieprofielen. Gebruik de volgende cmdlet om de AzureRM.Bootstrapper-module te installeren:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="install-a-profile"></a>Een profiel installeren

Gebruik de **Install-AzureRmProfile** cmdlet met de **2017-03-09-profiel** profiel van de API-version voor het installeren van de AzureRM-modules vereist voor Azure Stack. De Azure Stack-operator-modules zijn niet geïnstalleerd met dit profiel API-versie. Ze moeten worden geïnstalleerd afzonderlijk zoals opgegeven in de stap 3 van de [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md) artikel.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Installeren en importeren van modules in een profiel

Gebruik de **gebruik-AzureRmProfile** cmdlet om te installeren en importeren van modules die gekoppeld aan een API-versie-profiel zijn. U kunt slechts één API-versie-profiel importeren in een PowerShell-sessie. Als u wilt importeren in een ander profiel van de API-versie, moet u een nieuwe PowerShell-sessie openen. De cmdlet gebruiken-AzureRMProfile wordt uitgevoerd de volgende taken:  
1. Controleert of de PowerShell-modules die zijn gekoppeld aan het opgegeven profiel van de API-versie zijn geïnstalleerd in het huidige bereik.  
2. Downloadt en installeert de modules als ze nog niet zijn geïnstalleerd.   
3. Hiermee importeert de modules in de huidige PowerShell-sessie. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Als u wilt installeren en geselecteerde AzureRM-modules importeren uit een profiel van API-versie, voert u de cmdlet gebruiken-AzureRMProfile met de **Module** parameter:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>De geïnstalleerde profielen ophalen

Gebruik de **Get-AzureRmProfile** cmdlet om op te halen van de lijst met beschikbare API-versieprofielen: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Profielen bijwerken

Gebruik de **Update-AzureRmProfile** cmdlet voor het bijwerken van de modules in het profiel van een API-versie naar de meest recente versie van de modules die beschikbaar in de PSGallery zijn. Het is raadzaam om uit te voeren altijd de **Update-AzureRmProfile** cmdlet in een nieuwe PowerShell-sessie het voorkomen van conflicten bij het importeren van modules. De cmdlet Update-AzureRmProfile wordt uitgevoerd de volgende taken:

1. Controleert of als de meest recente versies van de modules zijn geïnstalleerd in de opgegeven API-versie profiel voor het huidige bereik.  
2. Vraagt u om te installeren als ze nog niet zijn geïnstalleerd.  
3. Installeert en de bijgewerkte modules importeert in de huidige PowerShell-sessie.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Als u wilt de eerder geïnstalleerde versies van de modules verwijderen voordat u bijwerkt naar de meest recente beschikbare versie, gebruikt u de cmdlet Update-AzureRmProfile samen met de **- RemovePreviousVersions** parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Deze cmdlet wordt de volgende taken uitgevoerd:  

1. Controleert of als de meest recente versies van de modules zijn geïnstalleerd in de opgegeven API-versie profiel voor het huidige bereik.  
2. Hiermee verwijdert u de oudere versies van modules in het huidige profiel van de API-versie en in de huidige PowerShell-sessie.  
4. vraagt u de nieuwste versie te installeren.  
5. Installeert en de bijgewerkte modules importeert in de huidige PowerShell-sessie.  
 
## <a name="uninstall-profiles"></a>Profielen verwijderen

Gebruik de **Uninstall-AzureRmProfile** cmdlet voor het verwijderen van het opgegeven profiel van de API-versie.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Volgende stappen
* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  
