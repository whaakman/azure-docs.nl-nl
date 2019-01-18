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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: f6fd560d6ecb9feaa3df14b288cee5d98ce55aee
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391086"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>API-versieprofielen gebruiken voor PowerShell in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

API-versieprofielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure Stack. Een API-versie-profiel is een set van AzureRM PowerShell-modules met specifieke API-versies. Elk cloudplatform bevat een set ondersteunde API-versieprofielen. Bijvoorbeeld, Azure Stack biedt ondersteuning voor een specifieke datum Profielversie zoals **2018-03-01-hybride**, en biedt ondersteuning voor Azure de **nieuwste** API-versie-profiel. Wanneer u een profiel installeert, worden de AzureRM PowerShell-modules die met het opgegeven profiel overeenkomen geïnstalleerd.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installeer de PowerShell-module vereist voor het gebruik van API-versieprofielen

De **AzureRM.Bootstrapper** -module die is beschikbaar via de PowerShell Gallery biedt PowerShell-cmdlets die nodig zijn voor het werken met API-versieprofielen. Gebruik de volgende cmdlet voor het installeren van de **AzureRM.Bootstrapper** module:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="azure-stack-version-and-profile-versions"></a>Azure Stack-versie en profielversies

De volgende tabel bevat de vereiste versie van de API-profiel en de PowerShell-beheerder module moniker gebruikt voor recente versies van Azure Stack. Als u van dit artikel met een versie voordat u 1808 gebruikmaakt, update uw versie profiel en moniker met de juiste waarde.

| Versie Nee. | Profiel voor API-versie | PS moniker van beheerder-module |
| --- | --- | --- |
| 1811 of hoger | 2018-03-01-hybride | 1.6.0 |
| 1808 of hoger | 2018-03-01-hybride | 1.5.0 |
| 1804 of hoger | 2017-03-09-profiel | 1.4.0 |
| Versies vóór 1804 | 2017-03-09-profiel | 1.2.11 |

> [!NOTE]  
> Een upgrade van de 1.2.11 versie, Zie de [Migratiehandleiding](https://aka.ms/azpsh130migration).

## <a name="install-a-profile"></a>Een profiel installeren

Gebruik de **Install-AzureRmProfile** cmdlet met de **2018-03-01-hybride** profiel van de API-version voor het installeren van de AzureRM-modules vereist voor Azure Stack. De Azure Stack-operator-modules zijn niet geïnstalleerd met dit profiel API-versie. Ze moeten worden geïnstalleerd afzonderlijk zoals opgegeven in de stap 3 van de [PowerShell installeren voor Azure Stack](../azure-stack-powershell-install.md) artikel.

```PowerShell
Install-AzureRMProfile -Profile 2018-03-01-hybrid
```

## <a name="install-and-import-modules-in-a-profile"></a>Installeren en importeren van modules in een profiel

Gebruik de **gebruik-AzureRmProfile** cmdlet om te installeren en importeren van modules die gekoppeld aan een API-versie-profiel zijn. U kunt slechts één API-versie-profiel importeren in een PowerShell-sessie. Als u wilt importeren in een ander profiel van de API-versie, moet u een nieuwe PowerShell-sessie openen. De **gebruik-AzureRMProfile** cmdlet voert de volgende taken uit:

1. Controleert of de PowerShell-modules die zijn gekoppeld aan het opgegeven profiel van de API-versie zijn geïnstalleerd in het huidige bereik.  
2. Downloadt en installeert de modules als ze nog niet zijn geïnstalleerd.
3. Hiermee importeert de modules in de huidige PowerShell-sessie.

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser -Force
```

Uitvoeren als u wilt installeren en geselecteerde AzureRM-modules importeren uit een profiel van API-versie, de **gebruik-AzureRMProfile** cmdlet met de **Module** parameter:

```PowerShell
# Installs and imports the compute, storage and network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
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

Gebruik de **Update-AzureRmProfile** cmdlet voor het bijwerken van de modules in het profiel van een API-versie naar de meest recente versie van de modules die beschikbaar in de PSGallery zijn. Het verdient aanbeveling dat u altijd uitgevoerd de **Update-AzureRmProfile** cmdlet in een nieuwe PowerShell-sessie het voorkomen van conflicten bij het importeren van modules. De **Update-AzureRmProfile** cmdlet worden de volgende taken uitgevoerd:

1. Controleert of als de meest recente versies van de modules zijn geïnstalleerd in de opgegeven API-versie profiel voor het huidige bereik.  
2. Vraagt u om te installeren als ze nog niet zijn geïnstalleerd.  
3. Installeert en de bijgewerkte modules importeert in de huidige PowerShell-sessie.  

```PowerShell
Update-AzureRmProfile -Profile 2018-03-01-hybrid
```

<!-- To remove the previously installed versions of the modules before updating to the latest available version, use the Update-AzureRmProfile cmdlet along with the **-RemovePreviousVersions** parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2018-03-01-hybrid -RemovePreviousVersions
``` -->

Deze cmdlet wordt de volgende taken uitgevoerd:  

1. Controleert of als de meest recente versies van de modules zijn geïnstalleerd in de opgegeven API-versie profiel voor het huidige bereik.  
2. Hiermee verwijdert u de oudere versies van modules in het huidige profiel van de API-versie en in de huidige PowerShell-sessie.  
3. vraagt u de nieuwste versie te installeren.  
4. Installeert en de bijgewerkte modules importeert in de huidige PowerShell-sessie.  

## <a name="uninstall-profiles"></a>Profielen verwijderen

Gebruik de **Uninstall-AzureRmProfile** cmdlet voor het verwijderen van het opgegeven profiel van de API-versie.

```PowerShell
Uninstall-AzureRmProfile -Profile  2018-03-01-hybrid
```

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  
