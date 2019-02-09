---
title: Doorgeven van referenties naar Azure met behulp van Desired State Configuration
description: Informatie over het doorgeven van referenties veilig op virtuele Azure-machines met behulp van PowerShell Desired State Configuration (DSC).
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: 6618906f7b1b063de18a4f8a418c1c2744ca1533
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55975781"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Doorgeven van referenties aan de handler Azure DSCExtension

In dit artikel bevat informatie over de extensie Desired State Configuration (DSC) voor Azure. Zie voor een overzicht van de DSC-extensie-handler [Inleiding tot de Azure Desired State Configuration-extensie-handler](dsc-overview.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="pass-in-credentials"></a>Referenties worden doorgegeven

Als onderdeel van het configuratieproces, u mogelijk nodig hebt voor het instellen van gebruikersaccounts, toegang tot services, of installeren van een programma in een gebruikerscontext. Als u wilt doen, moet u referenties op te geven.

U kunt DSC gebruiken voor het instellen van configuraties met parameters. Referenties zijn in een configuratie met parameters doorgegeven aan de configuratie en veilig opgeslagen in het MOF-bestanden. De handler voor Azure-extensie vereenvoudigt het Referentiebeheer door te geven automatisch beheer van certificaten.

Het volgende script voor DSC-configuratie wordt een lokaal gebruikersaccount gemaakt met het opgegeven wachtwoord:

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

Het is belangrijk om op te nemen **knooppunt localhost** als onderdeel van de configuratie. De extensie-handler specifiek gekeken naar de **knooppunt localhost** instructie. Als deze verklaring ontbreekt, wordt werken de volgende stappen niet. Het is ook belangrijk om de typecast **[PsCredential]**. Dit specifieke type activeert de extensie voor het versleutelen van de referentie.

Met dit script publiceren naar Azure Blob-opslag:

`Publish-AzVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Instellen van de Azure-DSC-extensie en geef de referentie op:

```powershell
$configurationName = 'Main'
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = 'user_configuration.ps1.zip'
$vm = Get-AzVM -Name 'example-1'

$vm = Set-AzVMDscExtension -VMName $vm -ConfigurationArchive $configurationArchive -ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzVM
```

## <a name="how-a-credential-is-secured"></a>Hoe een referentie is beveiligd

Uitvoering van deze code wordt gevraagd om een referentie. Nadat de referentie op die is opgegeven, wordt deze korte tijd opgeslagen in het geheugen. Wanneer de referentie op die is gepubliceerd met behulp van de **Set AzVMDscExtension** cmdlet, de referentie op die wordt verzonden via HTTPS naar de virtuele machine. In de virtuele machine slaat Azure de referentie op schijf versleuteld met behulp van het lokale certificaatarchief van de virtuele machine. De referentie is kort ontsleuteld in het geheugen en vervolgens opnieuw versleuteld als u wilt deze doorgeven aan DSC.

Dit proces is anders dan [met behulp van veilige configuraties zonder de extensie-handler](/powershell/dsc/securemof). De Azure-omgeving biedt een manier voor het verzenden van configuratiegegevens ontvangen via certificaten veilig. Wanneer u de handler voor DSC-extensie gebruikt, moet u niet opgeven **$CertificatePath** of een **$CertificateID**/ **$Thumbprint** vermelding in **ConfigurationData**.

## <a name="next-steps"></a>Volgende stappen

- Krijgen een [Inleiding tot Azure DSC-extensie handler](dsc-overview.md).
- Bekijk de [Azure Resource Manager-sjabloon voor de DSC-extensie](dsc-template.md).
- Voor meer informatie over PowerShell DSC, gaat u naar de [PowerShell-documentatiecentrum](/powershell/dsc/overview).
- Ga voor meer functionaliteit die u beheren kunt met behulp van PowerShell DSC, evenals meer DSC-resources, het [PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).