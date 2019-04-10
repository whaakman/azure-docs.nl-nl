---
title: Toestaan dat toepassingen om op te halen van Azure Stack Key Vault-geheimen | Microsoft Docs
description: Een voorbeeld-app gebruiken om te werken met Azure Stack Key Vault
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sethm
ms.lastreviewed: 04/08/2019
ms.openlocfilehash: 7ca02b35cd8f302b856b2d7fcbfb5bac304f1a00
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358627"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Een voorbeeldtoepassing die gebruikmaakt van sleutels en geheimen die zijn opgeslagen in een key vault

*Van toepassing op Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Volg de stappen in dit artikel om uit te voeren van de voorbeeldtoepassing **HelloKeyVault** die worden opgehaald van sleutels en geheimen van een sleutel in Azure Stack-kluis.

## <a name="prerequisites"></a>Vereisten

U kunt de volgende vereisten uit installeren de [Azure Stack Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden zijn via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).
* In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Maken en de sleutelkluis en toepassingsinstellingen

Om voor te bereiden voor de voorbeeldtoepassing:

* Een key vault maakt in Azure Stack.
* Een toepassing registreren in Azure Active Directory (Azure AD).

U kunt de Azure portal of PowerShell gebruiken om voor te bereiden voor de voorbeeldtoepassing. In dit artikel wordt beschreven hoe u een sleutelkluis maken en registreren van een toepassing met behulp van PowerShell.

> [!NOTE]
> Het PowerShell-script maakt standaard een nieuwe toepassing in Active Directory. U kunt echter een van uw bestaande toepassingen registreren.

Voordat u het volgende script uitvoert, zorg ervoor dat u waarden opgeven voor de `aadTenantName` en `applicationPassword` variabelen. Als u een waarde op voor geen opgeeft `applicationPassword`, met dit script wordt een willekeurig wachtwoord gegenereerd.

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script generates a random password during app creation.
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
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

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
Add-AzureRmAccount `
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

# Create a new resource group and a key vault in that resource group.
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

De volgende afbeelding ziet de uitvoer van het script gebruikt voor het maken van de key vault:

![Voor Key vault met toegangssleutels](media/azure-stack-key-vault-sample-app/settingsoutput.png)

Noteer de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** waarden die zijn geretourneerd door het vorige script. U gebruikt deze waarde om uit te voeren de **HelloKeyVault** toepassing.

## <a name="download-and-configure-the-sample-application"></a>Downloaden en configureren van de voorbeeldtoepassing

De sleutelkluis-voorbeeld downloaden van de Azure [Key Vault-client voorbeelden](https://www.microsoft.com/download/details.aspx?id=45343) pagina. Pak de inhoud van het ZIP-bestand op uw ontwikkelwerkstation. Er zijn twee toepassingen in de map met voorbeelden. In dit artikel wordt gebruikgemaakt van **HelloKeyVault**.

Laden van de **HelloKeyVault** voorbeeld:

1. Blader naar de **Microsoft.Azure.KeyVault.Samples**, **voorbeelden**, **HelloKeyVault** map.
2. Open de **HelloKeyVault** toepassing in Visual Studio.

### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

In Visual Studio:

1. Open het bestand HelloKeyVault\App.config en zoek de `<appSettings>` element.
2. Update de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** sleutels met de waarden die zijn geretourneerd door die zijn gebruikt bij het maken van de key vault. Het bestand App.config heeft standaard een tijdelijke aanduiding voor `AuthCertThumbprint`. Vervang deze tijdelijke aanduiding met `AuthClientSecret`.

   ![App-instellingen](media/azure-stack-key-vault-sample-app/appconfig.png)

3. De oplossing opnieuw maakt.

## <a name="run-the-application"></a>De toepassing uitvoeren

Bij het uitvoeren van **HelloKeyVault**, de toepassing zich aanmeldt bij Azure AD en gebruikt vervolgens de `AuthClientSecret` token om te verifiëren aan de sleutelkluis in Azure Stack.

U kunt de **HelloKeyVault** voorbeeld op:

* Eenvoudige bewerkingen zoals het maken, coderen, verpakken, en verwijderen uitvoeren op de sleutels en geheimen.
* Parameters doorgeven zoals `encrypt` en `decrypt` naar **HelloKeyVault**, en de opgegeven wijzigingen toepassen op een key vault.

## <a name="next-steps"></a>Volgende stappen

* [Een VM implementeren met een Key Vault-wachtwoord](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Een VM implementeren met een Key Vault-certificaat](azure-stack-key-vault-push-secret-into-vm.md)
