---
title: Identiteit voor Azure-app maken met PowerShell | Microsoft Docs
description: Hierin wordt beschreven hoe u met Azure PowerShell een Azure Active Directory-toepassing en service-principal maken en toegang tot resources via toegangsbeheer op basis van rollen. Er wordt weergegeven hoe toepassing met een certificaat te verifiëren.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 6ab1b2357e88525f4730b5ad550cfcf3acbb906e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Azure PowerShell gebruiken voor het maken van een service-principal met een certificaat

Wanneer u een app of een script dat nodig heeft voor toegang tot resources hebt, kunt u een identiteit voor de app instellen en de app met een eigen referenties verifiëren. Deze identiteit staat bekend als een service-principal. Deze aanpak kunt u:

* Machtigingen toekennen aan de identiteit van de app die anders zijn dan uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* Een certificaat voor verificatie gebruiken bij het uitvoeren van een onbewaakt script.

> [!IMPORTANT]
> In plaats van een service-principal maken, kunt u overwegen Azure AD beheerde Service-identiteit voor de identiteit van uw toepassingen. Azure AD-MSI is een openbare preview-functie van Azure Active Directory die vereenvoudigt het maken van een identiteit voor de code. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor Azure AD MSI en toegang tot bronnen die ondersteuning bieden voor Azure Active Directory-verificatie, is Azure AD MSI een betere optie voor u. Zie voor meer informatie over Azure AD MSI, met inbegrip van welke services momenteel ondersteuning [Service-identiteit voor Azure-resources beheerd](../active-directory/managed-service-identity/overview.md).

In dit artikel leest u hoe een service-principal die verifieert met een certificaat maken. Als u een service-principal met een wachtwoord instelt, Zie [een Azure-service-principal maken met Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).

U moet hebben de [meest recente versie](/powershell/azure/get-started-azureps) van PowerShell voor dit artikel.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u dit artikel, moet u voldoende machtigingen in uw Azure Active Directory en de Azure-abonnement hebben. In het bijzonder moet u een app maken in de Azure Active Directory en de service-principal toewijzen aan een rol.

De eenvoudigste manier om te controleren of uw account over de juiste machtigingen beschikt, verloopt via de portal. Zie [controleert u de vereiste machtiging](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Service-principal maken met een zelf-ondertekend certificaat

Het volgende voorbeeld wordt een eenvoudig scenario behandeld. Hierbij [nieuw AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) een service-principal maken met een zelfondertekend certificaat en maakt gebruik van [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) om toe te wijzen de [Inzender](../role-based-access-control/built-in-roles.md#contributor)role in de service-principal. De roltoewijzing is afgestemd op uw geselecteerde Azure-abonnement. Gebruik om te selecteren in een ander abonnement, [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

In het voorbeeld sluimert voor 20 seconden wacht u even voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wachten, ziet u een foutbericht waarin wordt gemeld: "Principal {ID} bestaat niet in de map {DIR-ID}." U lost deze fout, wacht u even Voer vervolgens de **New AzureRmRoleAssignment** opdracht opnieuw.

U kunt het bereik van de roltoewijzing aan een specifieke resourcegroep met behulp van de **ResourceGroupName** parameter. U kunt het bereik op een specifieke bron via ook de **ResourceType** en **ResourceName** parameters. 

Als u **hebben geen Windows 10 of Windows Server 2016**, moet u voor het downloaden van de [zelf-ondertekend certificaat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) van Microsoft Script Center. Pak de inhoud en importeren van de cmdlet die u nodig hebt.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Vervangen door de volgende twee regels voor het genereren van het certificaat in het script.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -StoreName My `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Geef het certificaat via automatische PowerShell-script

Wanneer u zich aanmeldt als een service-principal, moet u voor de tenant-ID van de map voor uw AD-app. Een tenant is een exemplaar van Azure Active Directory.

```powershell
$TenantId = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzureRmADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Service-principal maken met het certificaat van certificeringsinstantie

Het volgende voorbeeld wordt een certificaat dat is uitgegeven door een certificeringsinstantie voor service-principal maken. De toewijzing is afgestemd op de opgegeven Azure-abonnement. Hiermee worden toegevoegd aan de service-principal de [Inzender](../role-based-access-control/built-in-roles.md#contributor) rol. Als er een fout optreedt tijdens de roltoewijzing, het opnieuw probeert de toewijzing.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Geef het certificaat via automatische PowerShell-script
Wanneer u zich aanmeldt als een service-principal, moet u voor de tenant-ID van de map voor uw AD-app. Een tenant is een exemplaar van Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

De toepassings-ID en het tenant-ID zijn niet gevoelige, zodat u ze rechtstreeks in uw script insluiten kunt. Als u ophalen van de tenant-ID wilt, gebruiken:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Als u de toepassings-ID ophalen wilt, gebruikt:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Referenties wijzigen

U kunt de referenties voor een AD-app, hetzij vanwege een beveiligingsinbreuk of een vervaldatum referentie wijzigen met de [verwijderen AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) en [nieuw AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.

Voor het verwijderen van de referenties voor een toepassing gebruiken:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

Als u wilt toevoegen de waarde van een certificaat, een zelfondertekend certificaat te maken zoals in dit artikel. Vervolgens gebruikt:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Fouten opsporen

Mogelijk dat u de volgende fouten bij het maken van een service-principal:

* **'Authentication_Unauthorized'** of **'geen abonnement gevonden in de context.'** -U hebt deze fout te zien wanneer uw account beschikt niet over de [vereist machtigingen](#required-permissions) op de Azure Active Directory om een app te registreren. Normaal gesproken u deze fout te zien wanneer alleen admin gebruikers in uw Azure Active Directory apps registreren kunnen en uw account niet een beheerder is. Vraag uw beheerder u ofwel toewijzen aan een beheerdersrol of waarmee gebruikers om apps te registreren.

* Uw account **"heeft geen autorisatie om uit te voeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/subscriptions / {guid}."**  -Deze fout te zien wanneer uw account beschikt niet over voldoende machtigingen aan een rol toewijzen aan een identiteit. Vraag de abonnementsbeheerder u toe te voegen aan de rol beheerder voor gebruikerstoegang.

## <a name="next-steps"></a>Volgende stappen
* Als u een service-principal met een wachtwoord instelt, Zie [een Azure-service-principal maken met Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).
* Zie voor gedetailleerde stappen voor het integreren van een toepassing in Azure voor het beheren van resources [Ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](resource-manager-api-authentication.md).
* Zie voor een meer gedetailleerde uitleg van toepassingen en service-principals [toepassingsobjecten en Service-Principal objecten](../active-directory/active-directory-application-objects.md). 
* Zie voor meer informatie over Azure Active Directory-verificatie, [verificatie scenario's voor Azure AD](../active-directory/active-directory-authentication-scenarios.md).
