---
title: Referenties doorgeven naar Azure met Desired State Configuration | Microsoft Docs
description: Informatie over het veilig referenties worden doorgegeven aan virtuele machines in Azure met behulp van PowerShell Desired State Configuration (DSC).
services: virtual-machines-windows
documentationcenter: ''
author: zjalexander
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/17/2018
ms.author: zachal,migreene
ms.openlocfilehash: f372685692c2f02984bf0e0b8deeae27ce94422b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30914819"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Referenties doorgeven aan de handler Azure DSCExtension
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

In dit artikel bevat informatie over de uitbreiding Desired State Configuration (DSC) voor Azure. Zie voor een overzicht van de DSC-uitbreiding handler [Inleiding tot de extensie Azure Desired State Configuration handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="pass-in-credentials"></a>Referenties doorgeven

Als onderdeel van het configuratieproces u mogelijk nodig hebt voor het instellen van gebruikersaccounts, toegang tot services of een programma installeert in een gebruikerscontext. Als u wilt doen, moet u referenties opgeven.

DSC kunt u configuraties met parameters instellen. Referenties zijn doorgegeven aan de configuratie en veilig opgeslagen in het MOF-bestanden in een configuratie met parameters. De extensie Azure-handler vereenvoudigt Referentiebeheer dankzij automatisch beheer van certificaten.

De volgende DSC-configuratiescript wordt een lokale gebruikersaccount gemaakt met het opgegeven wachtwoord:

```powershell
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

Het is belangrijk om op te nemen **knooppunt localhost** als onderdeel van de configuratie. De extensie-handler specifiek zoekt de **knooppunt localhost** instructie. Als deze instructie ontbreekt, werken de volgende stappen niet. Het is ook belangrijk om op te nemen de typecast **[PsCredential]**. Dit specifieke type activeert de uitbreiding voor het versleutelen van de referentie.

Dit script publiceren naar Azure Blob-opslag:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

De extensie Azure DSC instellen en geef de referenties:

```powershell
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```

## <a name="how-a-credential-is-secured"></a>Hoe een referentie op die wordt beveiligd

Vraagt u deze code uitvoert om een referentie. Nadat de referentie is opgegeven, wordt deze kort opgeslagen in het geheugen. Wanneer de referentie is gepubliceerd met behulp van de **Set AzureVmDscExtension** de referentie-cmdlet wordt verzonden via HTTPS naar de virtuele machine. In de virtuele machine opgeslagen Azure de referentie op schijf wordt versleuteld met het lokale certificaatarchief van de virtuele machine. De referentie kort in het geheugen wordt ontsleuteld en vervolgens opnieuw versleuteld deze doorgeven aan DSC.

Dit proces is anders dan [met behulp van veilige configuraties zonder de extensie-handler](https://msdn.microsoft.com/powershell/dsc/securemof). De Azure-omgeving biedt een manier voor het verzenden van configuratiegegevens veilig via certificaten. Wanneer u de handler van DSC-uitbreiding gebruikt, u hoeft niet te bieden **$CertificatePath** of een **$CertificateID**/ **$Thumbprint** vermelding in **ConfigurationData**.

## <a name="next-steps"></a>Volgende stappen

* Ophalen van een [Inleiding tot Azure DSC-uitbreiding handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bekijk de [Azure Resource Manager-sjabloon voor de uitbreiding van DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Voor meer informatie over PowerShell DSC, gaat u naar de [PowerShell-documentatiecentrum](https://msdn.microsoft.com/powershell/dsc/overview).
* Ga voor meer functionaliteit die u beheren kunt met behulp van PowerShell DSC, en voor meer DSC-resources, het [PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
