---
title: WinRM-toegang instellen voor een Azure-VM | Microsoft Docs
description: Setup WinRM-toegang voor gebruik met een Azure-VM gemaakt in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: kasing
ms.openlocfilehash: 22a522fcde2b79d89e6084cdcfcbf64e4e5bd5ce
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977963"
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>WinRM-toegang instellen voor virtuele Machines in Azure Resource Manager

Hier volgen de stappen die u moet uitvoeren om in te stellen van een virtuele machine met de WinRM-connectiviteit

1. Een sleutelkluis maken
2. Een zelfondertekend certificaat maken
3. De zelf-ondertekend certificaat uploaden naar Key Vault
4. De URL voor uw zelf-ondertekend certificaat in de Key Vault ophalen
5. Verwijzen naar de URL van uw zelf-ondertekende certificaten tijdens het maken van een virtuele machine

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="step-1-create-a-key-vault"></a>Stap 1: Een sleutelkluis maken
U kunt de onderstaande opdracht om te maken van de Key Vault

```
New-AzKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Stap 2: Een zelfondertekend certificaat maken
U kunt een zelfondertekend certificaat met deze PowerShell-script maken

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Stap 3: De zelf-ondertekend certificaat uploaden naar de Key Vault
Voordat u het certificaat uploadt naar de Key Vault in stap 1 hebt gemaakt, moet deze geconverteerd naar een indeling die de Microsoft.Compute-resourceprovider begrijpt. De onderstaande PowerShell script kunt u dat doen

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Stap 4: De URL voor uw zelf-ondertekend certificaat in de Key Vault ophalen
De Microsoft.Compute-resourceprovider moet een URL naar het geheim in de Key Vault tijdens het inrichten van de virtuele machine. Hierdoor kunnen de Microsoft.Compute-resourceprovider voor het downloaden van het geheim en de equivalente certificaat maken op de virtuele machine.

> [!NOTE]
> De URL van de geheime sleutel nodig heeft om de versie ook. Een voorbeeld-URL ziet eruit als hieronder https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve
> 
> 

#### <a name="templates"></a>Sjablonen
Krijgt u de koppeling naar de URL in de sjabloon met de onderstaande code

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
U krijgt deze URL met behulp van de onderstaande PowerShell-opdracht

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Stap 5: Verwijzen naar de URL van uw zelf-ondertekende certificaten tijdens het maken van een virtuele machine
#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
Tijdens het maken van een virtuele machine via sjablonen, haalt het certificaat naar verwezen in de geheimen en de winRM-sectie zoals hieronder:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Een voorbeeldsjabloon voor het bovenstaande hier kan worden gevonden op [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

De broncode voor deze sjabloon kunt u vinden op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Stap 6: Verbinding maken met de virtuele machine
Voordat u verbinding met de virtuele machine maken kunt, moet u controleren of moet uw computer is geconfigureerd voor extern beheer met WinRM. Start PowerShell als beheerder en voer de onderstaande opdracht om te controleren of u bent ingesteld.

    Enable-PSRemoting -Force

> [!NOTE]
> Mogelijk moet u controleren of dat de WinRM-service wordt uitgevoerd als de bovenstaande niet werkt. U kunt doen dat met `Get-Service WinRM`
> 
> 

Zodra de installatie is voltooid, kunt u verbinding maken met de virtuele machine met behulp van de onderstaande opdracht

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
