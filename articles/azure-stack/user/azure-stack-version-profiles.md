---
title: Beheren van profielen voor API-versie in Azure-Stack | Microsoft Docs
description: Meer informatie over API-versie-profielen in Azure-Stack
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6B749785-DCF5-4AD8-B808-982E7C6BBA0E
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: mabrigg
ms.openlocfilehash: d86a54ea9e165269131eb961df7f74703f0ec6ff
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Profielen voor API-versie in Azure-Stack beheren

Profielen van Azure App Service-versie van de API-functie biedt een manier voor het beheren van versieverschillen tussen Azure en Azure-Stack. Een API-versie-profiel is een set van AzureRM PowerShell-modules met specifieke API-versies. Elk cloudplatform heeft een set van ondersteunde API-versie-profielen. Azure-Stack ondersteunt bijvoorbeeld een specifieke, datum Profielversie zoals **2017-03-09-profiel**, en biedt ondersteuning voor Azure de *nieuwste* profiel API-version. Wanneer u een profiel hebt geïnstalleerd, worden de AzureRM PowerShell-modules die met het opgegeven profiel overeenkomen geïnstalleerd.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Installeer de PowerShell-module voor het gebruik van profielen voor API-versie vereist

De **AzureRM.Bootstrapper** module die is beschikbaar via de PowerShell-galerie biedt PowerShell-cmdlets die nodig zijn voor het werken met profielen voor API-versie. Gebruik de volgende cmdlet voor het installeren van de **AzureRM.Bootstrapper** module:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
De **AzureRM.Bootstrapper** -module is een voorbeeld van, zodat de details en functionaliteit nog worden gewijzigd. Download en installeer de nieuwste versie van deze module van de PowerShell Gallery, voer de volgende cmdlet:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Een profiel installeren

Gebruik de **installeren AzureRmProfile** cmdlet uit met de **2017-03-09-profiel** profiel van de API-version voor het installeren van de vereiste door Azure Stack AzureRM-modules. 

>[!NOTE]
>De Azure-Stack cloud beheerder modules zijn met dit profiel API-versie niet geïnstalleerd. De beheerder modules moeten worden geïnstalleerd, afzonderlijk zoals opgegeven in stap 3 van de [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md) artikel.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Installeren en modules in een profiel importeren

Gebruik de **gebruik AzureRmProfile** cmdlet voor het installeren en importeren van modules die gekoppeld aan een profiel voor API-versie zijn. U kunt slechts één API-versie-profiel importeren in een PowerShell-sessie. Als u wilt importeren in een ander profiel van de API-versie, moet u een nieuwe PowerShell-sessie openen. De **gebruik AzureRMProfile** cmdlet wordt uitgevoerd de volgende taken:  
1. Controleert of de PowerShell-modules die zijn gekoppeld aan het opgegeven profiel van de API-versie zijn geïnstalleerd in de huidige scope.  
2. Downloadt en installeert de modules als ze nog niet zijn geïnstalleerd.   
3. Hiermee importeert de modules in de huidige PowerShell-sessie. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Uitvoeren als u wilt installeren en geselecteerde AzureRM modules importeren uit een profiel voor API-versie, de **gebruik AzureRMProfile** cmdlet uit met de *Module* parameter:

```PowerShell
# Installs and imports the Compute, Storage, and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>De geïnstalleerde profielen ophalen

Gebruik de **Get-AzureRmProfile** cmdlet voor de lijst met beschikbare profielen voor API-versie: 

```PowerShell
# Lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# Lists the API version profiles that are installed on your machine.
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Profielen bijwerken

Gebruik de **Update AzureRmProfile** cmdlet voor het bijwerken van de modules in een profiel van de API-versie naar de nieuwste versie van de modules die beschikbaar in de PowerShell-galerie zijn. Het is raadzaam dat u uitvoert het **Update AzureRmProfile** cmdlet in een nieuwe PowerShell-sessie om conflicten te voorkomen bij het importeren van modules. De **Update AzureRmProfile** cmdlet wordt uitgevoerd de volgende taken:

1. Controleert of de nieuwste versies van de modules zijn geïnstalleerd in het opgegeven profiel voor API-versie voor de huidige scope.  
2. Vraagt u de modules installeren als ze nog niet zijn geïnstalleerd.  
3. Installeert en importeert de bijgewerkte modules in de huidige PowerShell-sessie.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

U kunt de eerder geïnstalleerde versies van de modules verwijderen voordat u naar de meest recente versie bijwerken met de **Update AzureRmProfile** cmdlet samen met de *- RemovePreviousVersions* parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Deze cmdlet wordt uitgevoerd de volgende taken:  

1. Controleert of de nieuwste versies van de modules zijn geïnstalleerd in het opgegeven profiel voor API-versie voor de huidige scope.  
2. Verwijdert de oudere versies van de modules uit het huidige profiel voor de API-versie en in de huidige PowerShell-sessie.  
3. Vraagt u de meest recente versie van de modules installeren.  
4. Installeert en importeert de bijgewerkte modules in de huidige PowerShell-sessie.  
 
## <a name="uninstall-profiles"></a>Profielen verwijderen

Gebruik de **verwijderen AzureRmProfile** cmdlet voor het verwijderen van het opgegeven profiel van de API-versie:

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Volgende stappen
* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)  
