---
title: Referenties worden doorgegeven aan Azure met behulp van DSC | Microsoft Docs
description: Overzicht van de referenties veilig worden doorgegeven aan virtuele machines in Azure met behulp van PowerShell Desired State Configuration
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Referenties worden doorgegeven aan de handler Azure DSC-uitbreiding
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

In dit artikel bevat informatie over de Desired State Configuration-extensie voor Azure. Een overzicht van de DSC-uitbreiding handler kan worden gevonden op [Inleiding tot de extensie Azure Desired State Configuration handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Doorgeven van referenties
Als onderdeel van het configuratieproces, u mogelijk nodig hebt voor het instellen van gebruikersaccounts, toegang tot services of een programma installeert in een gebruikerscontext. Als u wilt doen, moet u referenties opgeven. 

DSC kunt u met parameters configuraties waarin de referenties zijn doorgegeven aan de configuratie en veilig opgeslagen in het MOF-bestanden. De extensie Azure Handler vereenvoudigt Referentiebeheer dankzij automatisch beheer van certificaten. 

Houd rekening met de volgende DSC-configuratiescript dat een lokale gebruikersaccount met het opgegeven wachtwoord maakt:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Het is belangrijk om op te nemen *knooppunt localhost* als onderdeel van de configuratie. Als deze instructie ontbreekt, werken de volgende stappen niet als de extensie-handler specifiek zoekt naar het knooppunt localhost-instructie. Het is ook belangrijk om op te nemen de typecast *[PsCredential]*, zoals dit specifieke type de uitbreiding activeert voor het versleutelen van de referentie. 

Dit script publiceren naar blob storage:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Stelt u de Azure DSC-uitbreiding en geef de referenties:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hoe referenties worden beveiligd
Vraagt u deze code uitvoert om een referentie. Zodra deze is opgegeven, wordt opgeslagen in het geheugen kort. Wanneer deze wordt gepubliceerd met `Set-AzureVmDscExtension` cmdlet, deze wordt verzonden via HTTPS naar de virtuele machine, een waar Azure slaat deze versleuteld op de schijf met het lokale certificaatarchief van de virtuele machine. Vervolgens wordt kort ontsleuteld in het geheugen en opnieuw versleuteld deze doorgeven aan DSC.

Dit gedrag is anders dan [met behulp van veilige configuraties zonder de extensie-handler](https://msdn.microsoft.com/powershell/dsc/securemof). De Azure-omgeving biedt een manier voor het verzenden van configuratiegegevens veilig via certificaten. Wanneer u de handler van DSC-uitbreiding, is niet nodig voor een $CertificatePath of een $CertificateID / $Thumbprint vermelding in ConfigurationData.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure DSC-uitbreiding handler [Inleiding tot de extensie Azure Desired State Configuration handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Bekijk de [Azure Resource Manager-sjabloon voor de uitbreiding van DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Voor meer informatie over PowerShell DSC [gaat u naar de PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview). 

Aanvullende functionaliteit die u met PowerShell DSC beheren kunt vinden [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) voor meer DSC-resources.

