---
title: Maak een Azure Key Vault-certificaat | Microsoft Docs
description: Wordt uitgelegd over het registreren van een virtuele machine van een VHD voor Azure zijn geïmplementeerd.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 4223f9ec3bfaeacf7843508b13b5b5d81474311f
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196824"
---
# <a name="create-certificates-for-azure-key-vault"></a>Certificaten voor Azure Key Vault maken

In dit artikel wordt uitgelegd hoe u de zelfondertekende certificaten vereist om een verbinding van de Windows Remote Management (WinRM) en een wordt gehost op Azure-machine (VM) te maken. Dit proces bestaat uit drie stappen:

1.  Maak het beveiligingscertificaat. 
2.  De Azure Key Vault voor het opslaan van dit certificaat maken. 
3.  De certificaten op deze sleutelkluis Store. 

U kunt een nieuwe of een bestaande resourcegroep gebruiken voor dit werk.  De eerste benadering wordt gebruikt in de volgende uitleg.


## <a name="create-the-certificate"></a>Maken van het certificaat

Bewerken en voer de volgende Azure Powershell-script voor het maken van het certificaatbestand (.pfx) in een lokale map.  U moet vervangen door de waarden voor de volgende parameters:

|  **Parameter**        |   **Beschrijving**                                                               |
|  -------------        |   ---------------                                                               |
| `$certroopath` | Lokale map op te slaan van het pfx-bestand op  |
| `$location`    | Een van de Azure standaard geografische locaties  |
| `$vmName`      | Naam van de geplande Azure-machine   |
| `$certname`    | Naam van het certificaat; moet overeenkomen met de volledig gekwalificeerde domeinnaam van de geplande virtuele machine  |
| `$certpassword` | Wachtwoord voor de certificaten moet overeenkomen met het wachtwoord voor de geplande virtuele machine  |
|  |  |

```powershell
    # Certification creation script 
    
    # pfx certification stored path
    $certroopath = "C:\certLocation"
    
    #location of the resource group
    $location = "westus"
    
    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"
    
    # Certification name - should match with FQDN of Windows Azure creating VM 
    $certname = "$vmName.$location.cloudapp.azure.com"
    
    # Certification password - should be match with password of Windows Azure creating VM 
    $certpassword = "SecretPassword@123"
    
    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb 

```
> [!TIP]
> Dezelfde PowerShell-console-sessie actief houden tijdens deze stappen zodat de waarden van de verschillende parameters, blijven behouden.

> [!WARNING]
> Als u dit script opslaat, bewaar alleen op een veilige locatie omdat deze informatie over de beveiliging (wachtwoord bevat).


## <a name="create-the-key-vault"></a>De key vault maken

Kopieer de inhoud van de [sleutelkluis implementatiesjabloon](./cpp-key-vault-deploy-template.md) naar een bestand op uw lokale computer. (in het onderstaande voorbeeldscript van deze resource is `C:\certLocation\keyvault.json`.)  Bewerken en voer de volgende Azure Powershell-script om een Azure Key Vault-instantie en de bijbehorende resourcegroep te maken.  U moet vervangen door de waarden voor de volgende parameters:

|  **Parameter**        |   **Beschrijving**                                                               |
|  -------------        |   ---------------                                                               |
| `$postfix`            | Willekeurige numerieke tekenreeks toegevoegd aan de implementatie-id 's                     |
| `$rgName`             | Azure (RG) de naam van resourcegroep maken                                        |
|  `$location`          | Een van de Azure standaard geografische locaties                                  |
| `$kvTemplateJson`     | Pad van het bestand (keyvault.json) met Resource Manager-sjabloon voor key vault |
| `$kvname`             | Naam van de nieuwe key vault                                                       |
|  |  |

```powershell
    # Creating Key vault in resource group
    
    # "Random" number for deployment identifiers
    $postfix = "0101048"
    
    # Resource group name
    $rgName = "TestRG$postfix"
    
    # Location of Resource Group
    $location = "westus"
    
    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"
    
    # Key vault name
    $kvname = "isvkv$postfix"
    
    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzureAccount
        $accountNum = 0
        $accounts.Id | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host 
        
        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }
        
        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Id
                              
        Write-Host "User $id Selected"
        $myobjectId=(Get-AzureRmADUser -Mail $id)[0].Id
      }
      catch
      {
      Write-Host $_.Exception.Message    
      Break
      } 

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzureSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host
    
        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].SubscriptionId
        $mysubName=$subslist[$selectedsub-1].SubscriptionName
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message    
        Break
        }
    
    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     Create-ResourceGroup -rgName $rgName -location $location
     Write-Host "-----------------------------------" 
    
    # Create key vault and configure access
    New-AzureRmResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId
    
    Set-AzureRmKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys all -PermissionsToSecrets all 
        
```

## <a name="store-the-certificate"></a>Het certificaat Store

U kunt nu de certificaten die zijn opgenomen in het pfx-bestand naar de nieuwe sleutelkluis door uit te voeren in het volgende script opslaan. 

```powershell
    #push certificate to key vault secret

     $fileName =$certroopath+"\$certname"+".pfx" 
      
     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    
            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
    "@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
            $objAzureKeyVaultSecret=Set-AzureKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id 
    
```


## <a name="next-steps"></a>Volgende stappen

Vervolgens wordt u [een VM implementeren vanaf uw gebruiker VM-installatiekopie](./cpp-deploy-vm-user-image.md).
