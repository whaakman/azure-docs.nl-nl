---
title: Toepassingen kunnen ophalen van geheimen Azure Stack Sleutelkluis | Microsoft Docs
description: Een voorbeeld-app gebruiken om te werken met Azure Sleutelkluis voor Stack
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2017
ms.author: mabrigg
ms.openlocfilehash: 50103dca21d047c5cee211b2250e750739131bc1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Voorbeeldtoepassing die gebruikmaakt van sleutels en geheimen die zijn opgeslagen in een sleutelkluis

In dit artikel wordt beschreven hoe u een voorbeeldtoepassing (HelloKeyVault) die sleutels en geheimen opgehaald uit een sleutelkluis in Azure-Stack uitvoeren.

## <a name="prerequisites"></a>Vereisten 

Voer de volgende vereisten een van de [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md). 

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Maken en de sleutelkluis en toepassingsinstellingen

U moet eerst een sleutelkluis maken in Azure-Stack en een toepassing registreren in Azure Active Directory (Azure AD). U kunt maken en de sleutelkluizen registreren via de Azure-portal of PowerShell. Dit artikel ziet u de PowerShell-manier om de taken uitvoeren. Deze PowerShell-script wordt standaard een nieuwe toepassing gemaakt in Active Directory. Echter, u kunt ook gebruiken een van uw bestaande toepassingen. Zorg ervoor dat u een waarde opgeven voor de `aadTenantName` en `applicationPassword` variabelen. Als u niet een waarde opgeven voor de `applicationPassword` variabele, dit script genereert een willekeurig wachtwoord. 

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local' 

# Password for the application. If not specified, this script will generate a random password during app creation.
$applicationPassword = '' 
                         
# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "PLEASE FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Login-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `
  
$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey
    
# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault within that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location   

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host

``` 

De volgende schermafbeelding ziet de uitvoer van het vorige script:

![App-configuratie](media/azure-stack-kv-sample-app/settingsoutput.png)

Noteer de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** door het vorige script geretourneerde waarden. Deze waarden kunt u de toepassing HelloKeyVault uitvoeren.

## <a name="download-and-run-the-sample-application"></a>Downloaden en uitvoeren van de voorbeeldtoepassing

De sleutelkluis-voorbeeld downloaden van de Azure [Sleutelkluis client voorbeelden](https://www.microsoft.com/en-us/download/details.aspx?id=45343) pagina. Pak de inhoud van het ZIP-bestand op uw ontwikkelwerkstation. Er zijn twee voorbeelden in de map met voorbeelden. We het voorbeeld HellpKeyVault in dit artikel gebruiken. Blader naar de **Microsoft.Azure.KeyVault.Samples** > **voorbeelden** > **HelloKeyVault** map en open de toepassing HelloKeyVault in Visual Studio. 

Open het bestand HelloKeyVault\App.config en vervang de waarden van de <appSettings> element met de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** waarden door het vorige script geretourneerd. Houd er rekening mee dat de App.config bevat standaard een tijdelijke aanduiding voor *AuthCertThumbprint*, maar gebruiken *AuthClientSecret* in plaats daarvan. Nadat u de instellingen vervangt, opnieuw opbouwen van de oplossing en de toepassing niet starten.

![App-instellingen](media/azure-stack-kv-sample-app/appconfig.png)
 
De toepassing zich aanmeldt bij Azure AD en vervolgens dat token gebruikt voor verificatie bij de sleutelkluis in Azure-Stack. Bewerkingen zoals maakt voor de toepassing uitvoert, coderen en verpakken van de sleutels en geheimen van de sleutelkluis verwijderen. U kunt ook specifieke parameters zoals doorgeven *versleutelen* en *ontsleutelen* tot de toepassing die ervoor zorgt dat de toepassing wordt uitgevoerd alleen deze bewerkingen op basis van de kluis. 


## <a name="next-steps"></a>Volgende stappen
[Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-kv-deploy-vm-with-secret.md)

[Een virtuele machine met een certificaat Sleutelkluis implementeren](azure-stack-kv-push-secret-into-vm.md)



