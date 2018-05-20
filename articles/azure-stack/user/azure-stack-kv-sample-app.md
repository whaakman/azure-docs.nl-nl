---
title: Toepassingen kunnen ophalen van geheimen Azure Stack Sleutelkluis | Microsoft Docs
description: Een voorbeeld-app gebruiken om te werken met Azure Sleutelkluis voor Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/11/2018
ms.author: mabrigg
ms.openlocfilehash: 39bce286c756660cd8755358cf98f2c8d35ce351
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Een voorbeeldtoepassing die gebruikmaakt van sleutels en geheimen die zijn opgeslagen in een sleutelkluis

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Volg de stappen in dit artikel voor het uitvoeren van een voorbeeldtoepassing (HelloKeyVault) die sleutels en geheimen opgehaald uit een sleutelkluis in Azure-Stack.

## <a name="prerequisites"></a>Vereisten

U kunt de volgende vereisten installeren vanaf de Azure-Stack [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Maken en de sleutelkluis en toepassingsinstellingen

Voorbereiden voor de voorbeeldtoepassing:

* Een sleutelkluis maken in Azure-Stack.
* Registreert een toepassing in Azure Active Directory (Azure AD).

U kunt de Azure-portal of PowerShell gebruiken om voor te bereiden voor de voorbeeldtoepassing. In dit artikel laat zien hoe een sleutelkluis maken en een toepassing registreren met behulp van PowerShell.

>[!NOTE]
>Het PowerShell-script wordt standaard een nieuwe toepassing gemaakt in Active Directory. U kunt echter een van uw bestaande toepassingen registreren.

 Voordat u het volgende script uitvoert, zorg ervoor dat u waarden opgeven voor de `aadTenantName` en `applicationPassword` variabelen. Als u niet een waarde opgeven voor `applicationPassword`, dit script genereert een willekeurig wachtwoord.

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

De volgende schermafbeelding ziet u de uitvoer van het script waarmee u de sleutelkluis hebt gemaakt:

![Sleutel kluis met toegangstoetsen](media/azure-stack-kv-sample-app/settingsoutput.png)

Noteer de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** door het vorige script geretourneerde waarden. Deze waarden kunt u de toepassing HelloKeyVault uitvoeren.

## <a name="download-and-configure-the-sample-application"></a>Downloaden en configureren van de voorbeeldtoepassing

De sleutelkluis-voorbeeld downloaden van de Azure [Sleutelkluis client voorbeelden](https://www.microsoft.com/en-us/download/details.aspx?id=45343) pagina. Pak de inhoud van het ZIP-bestand op uw ontwikkelwerkstation. Er zijn twee toepassingen in de map samples, HelloKeyVault maakt gebruik van dit artikel.

Het voorbeeld HelloKeyVault laden:

* Blader naar de **Microsoft.Azure.KeyVault.Samples** > **voorbeelden** > **HelloKeyVault** map.
* Open de HelloKeyVault-toepassing in Visual Studio.

### <a name="configure-the-sample-application"></a>De voorbeeldtoepassing configureren

In Visual Studio:

* Open het bestand HelloKeyVault\App.config en vinden, bladert u naar de &lt; **appSettings** &gt; element.
* Update de **VaultUrl**, **AuthClientId**, en **AuthClientSecret** sleutels met de waarden die door de gebruikte om de sleutelkluis te maken. (Het bestand App.config heeft standaard een tijdelijke aanduiding voor *AuthCertThumbprint*. Vervang deze tijdelijke aanduiding met *AuthClientSecret*.)

  ![App-instellingen](media/azure-stack-kv-sample-app/appconfig.png)

* De oplossing opnieuw worden opgebouwd.

## <a name="run-the-application"></a>De toepassing uitvoeren

Wanneer u HelloKeyVault uitvoert, wordt de toepassing zich aanmeldt bij Azure AD en vervolgens het token AuthClientSecret gebruikt om de sleutelkluis in Azure-Stack te verifiëren.

U kunt de steekproef die HelloKeyVault gebruiken:

* Basisbewerkingen zoals maakt, versleutelen, verpakken, en verwijderen uit te voeren op de sleutels en geheimen.
* Parameters zoals doorgeven *versleutelen* en *ontsleutelen* naar HelloKeyVault, en de opgegeven wijzigingen toepassen op een sleutelkluis.

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-kv-deploy-vm-with-secret.md)

[Een virtuele machine met een certificaat Sleutelkluis implementeren](azure-stack-kv-push-secret-into-vm.md)
