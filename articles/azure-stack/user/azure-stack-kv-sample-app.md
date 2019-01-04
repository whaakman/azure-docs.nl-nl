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
ms.topic: get-started-article
ms.date: 12/27/2018
ms.author: sethm
ms.openlocfilehash: b17f6301a41dbb1f64edf9d027dff0f57c09282c
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53808771"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Een voorbeeldtoepassing die gebruikmaakt van sleutels en geheimen die zijn opgeslagen in een key vault

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Volg de stappen in dit artikel voor het uitvoeren van een voorbeeldtoepassing met de naam **HelloKeyVault** die worden opgehaald van sleutels en geheimen van een sleutel in Azure Stack-kluis.

## <a name="prerequisites"></a>Vereisten

U kunt de volgende vereisten installeren via de Azure Stack [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden zijn via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).
* In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Maken en de sleutelkluis en toepassingsinstellingen

Om voor te bereiden voor de voorbeeldtoepassing:

* Een key vault maakt in Azure Stack.
* Een toepassing registreren in Azure Active Directory (Azure AD).

U kunt de Azure portal of PowerShell gebruiken om voor te bereiden voor de voorbeeldtoepassing. In dit artikel wordt beschreven hoe u een sleutelkluis maken en registreren van een toepassing met behulp van PowerShell.

>[!NOTE]
>Het PowerShell-script maakt standaard een nieuwe toepassing in Active Directory. U kunt echter een van uw bestaande toepassingen registreren.

Voordat u het volgende script uitvoert, zorg ervoor dat u waarden opgeven voor de `aadTenantName` en `applicationPassword` variabelen. Als u een waarde op voor geen opgeeft `applicationPassword`, met dit script wordt een willekeurig wachtwoord gegenereerd.

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

![Voor Key vault met toegangssleutels](media/azure-stack-kv-sample-app/settingsoutput.png)

Noteer de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** waarden die zijn geretourneerd door het vorige script. U gebruikt deze waarden in de toepassing HelloKeyVault uit te voeren.

## <a name="download-and-configure-the-sample-application"></a>Downloaden en configureren van de voorbeeldtoepassing

De sleutelkluis-voorbeeld downloaden van de Azure [Key Vault-client voorbeelden](https://www.microsoft.com/download/details.aspx?id=45343) pagina. Pak de inhoud van het ZIP-bestand op uw ontwikkelwerkstation. Er zijn twee toepassingen in de map met voorbeelden. In dit artikel wordt gebruikgemaakt van **HelloKeyVault**.

Laden van de **HelloKeyVault** voorbeeld:

* Blader naar de **Microsoft.Azure.KeyVault.Samples** > **voorbeelden** > **HelloKeyVault** map.
* Open de **HelloKeyVault** toepassing in Visual Studio.

### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

In Visual Studio:

* Open het bestand HelloKeyVault\App.config en zoek de &lt; **appSettings** &gt; element.
* Update de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** sleutels met de waarden die zijn geretourneerd door die zijn gebruikt bij het maken van de key vault. Het bestand App.config heeft standaard een tijdelijke aanduiding voor `AuthCertThumbprint`. Vervang deze tijdelijke aanduiding met `AuthClientSecret`.

  ![App-instellingen](media/azure-stack-kv-sample-app/appconfig.png)

* De oplossing opnieuw maakt.

## <a name="run-the-application"></a>De toepassing uitvoeren

Wanneer u HelloKeyVault uitvoert, wordt de toepassing zich aanmeldt bij Azure AD en vervolgens het token AuthClientSecret gebruikt om de key vault in Azure Stack te verifiëren.

U kunt het voorbeeld HelloKeyVault te gebruiken:

* Eenvoudige bewerkingen zoals het maken, coderen, verpakken, en verwijderen uitvoeren op de sleutels en geheimen.
* Parameters doorgeven zoals `encrypt` en `decrypt` naar HelloKeyVault, en de opgegeven wijzigingen toepassen op een key vault.

## <a name="next-steps"></a>Volgende stappen

- [Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-kv-deploy-vm-with-secret.md)
- [Een VM implementeren met een Key Vault-certificaat](azure-stack-kv-push-secret-into-vm.md)
