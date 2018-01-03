---
title: Identiteit voor Azure-app maken met PowerShell | Microsoft Docs
description: "Hierin wordt beschreven hoe u met Azure PowerShell een Azure Active Directory-toepassing en service-principal maken en toegang tot resources via toegangsbeheer op basis van rollen. Er wordt weergegeven hoe toepassing met een wachtwoord of certificaat te verifiëren."
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
ms.date: 12/28/2017
ms.author: tomfitz
ms.openlocfilehash: 9431483293bcc252b79d02ba2d655a3aa86aaa4a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources

Wanneer u een app of een script dat nodig heeft voor toegang tot resources hebt, kunt u een identiteit voor de app instellen en de app met een eigen referenties verifiëren. Deze identiteit staat bekend als een service-principal. Deze aanpak kunt u:

* Machtigingen toekennen aan de identiteit van de app die anders zijn dan uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* Een certificaat voor verificatie gebruiken bij het uitvoeren van een onbewaakt script.

Dit artikel laat zien hoe u [Azure PowerShell](/powershell/azure/overview) voor het instellen van alles wat u nodig hebt voor een toepassing worden uitgevoerd onder een eigen referenties en identiteit.

## <a name="required-permissions"></a>Vereiste machtigingen
Als u dit artikel, moet u voldoende machtigingen in uw Azure Active Directory en uw Azure-abonnement hebben. In het bijzonder moet u een app maken in de Azure Active Directory en de service-principal toewijzen aan een rol. 

De eenvoudigste manier om te controleren of uw account over de juiste machtigingen beschikt, verloopt via de portal. Zie [controleert u de vereiste machtiging](resource-group-create-service-principal-portal.md#required-permissions).

Nu gaat u verder met een sectie voor verificatie met:

* [wachtwoord](#create-service-principal-with-password)
* [zelf-ondertekend certificaat](#create-service-principal-with-self-signed-certificate)
* [certificaat van certificeringsinstantie](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>PowerShell-opdrachten

Als u een service-principal instelt, u het volgende gebruiken:

| Opdracht | Beschrijving |
| ------- | ----------- | 
| [Nieuwe AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Hiermee maakt u een Azure Active Directory service-principal |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | De opgegeven RBAC-rol toegewezen aan de opgegeven principal, bij het opgegeven bereik. |


## <a name="create-service-principal-with-password"></a>Service-principal maken met wachtwoord

Als een service-principal maken met de rol Inzender voor uw abonnement, gebruiken: 

```powershell
Login-AzureRmAccount
$password = convertto-securestring {provide-password} -asplaintext -force
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password $password
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

In het voorbeeld sluimert voor 20 seconden wacht u even voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wacht, ziet u een foutbericht waarin wordt gemeld: ' PrincipalNotFound: Principal {ID} bestaat niet in de map. "

Het volgende script kunt u opgeven van een scope dan het standaardabonnement en de roltoewijzing pogingen als een fout optreedt:

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $SecurePassword = convertto-securestring $Password -asplaintext -force
 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $SecurePassword
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Enkele items te weten over het script:

* De identiteit om toegang te verlenen aan de standaardabonnement, hoeft u geen ResourceGroup of SubscriptionId parameters opgeven.
* Geef de parameter ResourceGroup alleen als u wilt beperken welke van de roltoewijzing aan een resourcegroep.
*  In dit voorbeeld kunt u de service-principal toevoegen aan de rol Inzender. Zie voor andere rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md).
* Het script modus ingeschakeld gedurende 15 seconden om toe te staan enige tijd voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wacht, ziet u een foutbericht waarin wordt gemeld: ' PrincipalNotFound: Principal {ID} bestaat niet in de map. "
* Als u de service principal toegang verlenen tot meer abonnementen of resourcegroepen moet, voert u de `New-AzureRMRoleAssignment` cmdlet opnieuw met een verschillend bereik.


### <a name="provide-credentials-through-powershell"></a>Geef referenties op via PowerShell
Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren. Gebruik voor de gebruikersnaam, het `ApplicationId` die u hebt gemaakt voor de toepassing. Gebruik voor het wachtwoord die u hebt opgegeven bij het maken van het account. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-ID}
```

De tenant-ID is niet gevoelige, dus u deze rechtstreeks in uw script insluiten kunt. Als u ophalen van de tenant-ID wilt, gebruiken:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Service-principal maken met een zelf-ondertekend certificaat

Als een service-principal maken met een zelfondertekend certificaat en de rol Inzender voor uw abonnement, gebruiken: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

In het voorbeeld sluimert voor 20 seconden wacht u even voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wacht, ziet u een foutbericht waarin wordt gemeld: ' PrincipalNotFound: Principal {ID} bestaat niet in de map. "

Het volgende script kunt u opgeven van een scope dan het standaardabonnement en de roltoewijzing pogingen als een fout optreedt. U moet Azure PowerShell 2.0 op Windows 10 of Windows Server 2016 hebben.

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Enkele items te weten over het script:

* De identiteit om toegang te verlenen aan de standaardabonnement, hoeft u geen ResourceGroup of SubscriptionId parameters opgeven.
* Geef de parameter ResourceGroup alleen als u wilt beperken welke van de roltoewijzing aan een resourcegroep.
* In dit voorbeeld kunt u de service-principal toevoegen aan de rol Inzender. Zie voor andere rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md).
* Het script modus ingeschakeld gedurende 15 seconden om toe te staan enige tijd voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wacht, ziet u een foutbericht waarin wordt gemeld: ' PrincipalNotFound: Principal {ID} bestaat niet in de map. "
* Als u de service principal toegang verlenen tot meer abonnementen of resourcegroepen moet, voert u de `New-AzureRMRoleAssignment` cmdlet opnieuw met een verschillend bereik.

Als u **hebben geen Windows 10 of Windows Server 2016 Technical Preview**, moet u voor het downloaden van de [zelf-ondertekend certificaat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) van Microsoft Script Center. Pak de inhoud en importeren van de cmdlet die u nodig hebt.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Vervangen door de volgende twee regels voor het genereren van het certificaat in het script.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Geef het certificaat via automatische PowerShell-script
Wanneer u zich aanmeldt als een service-principal, moet u voor de tenant-ID van de map voor uw AD-app. Een tenant is een exemplaar van Azure Active Directory. Als u slechts één abonnement hebt, kunt u het volgende gebruiken:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

De toepassings-ID en het tenant-ID zijn niet gevoelige, zodat u ze rechtstreeks in uw script insluiten kunt. Als u ophalen van de tenant-ID wilt, gebruiken:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Als u de toepassings-ID ophalen wilt, gebruikt:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Service-principal maken met het certificaat van certificeringsinstantie
Als u een certificaat dat is uitgegeven door een certificeringsinstantie wilt service-principal maken, gebruikt u het volgende script:

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

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId
 
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

Enkele items te weten over het script:

* Toegang is afgestemd op het abonnement.
* In dit voorbeeld kunt u de service-principal toevoegen aan de rol Inzender. Zie voor andere rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md).
* Het script modus ingeschakeld gedurende 15 seconden om toe te staan enige tijd voor de nieuwe service principal doorgeven in Azure Active Directory. Als uw script niet lang genoeg wacht, ziet u een foutbericht waarin wordt gemeld: ' PrincipalNotFound: Principal {ID} bestaat niet in de map. "
* Als u de service principal toegang verlenen tot meer abonnementen of resourcegroepen moet, voert u de `New-AzureRMRoleAssignment` cmdlet opnieuw met een verschillend bereik.

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
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
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
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

U kunt een wachtwoord, gebruiken:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

Als u wilt toevoegen de waarde van een certificaat, een zelfondertekend certificaat te maken zoals in dit artikel. Vervolgens gebruikt:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a>Toegangstoken om te vereenvoudigen aanmelden opslaan
Om te voorkomen dat de referenties voor de service-principal geven telkens wanneer deze zich aanmeldt, kunt u het toegangstoken opslaan.

Als u het huidige toegangstoken in een latere sessie, kunt u het profiel opslaan.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Open het profiel en bekijk de inhoud ervan. U ziet dat deze een toegangstoken bevat. In plaats van handmatig opnieuw in te loggen, het profiel niet laden.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Het toegangstoken is verlopen, zodat u met behulp van een opgeslagen profiel werkt alleen voor zolang het token ongeldig is.
>  

U kunt ook aanroepen REST-bewerkingen van PowerShell om aan te melden. U kunt het toegangstoken voor gebruik met andere bewerkingen ophalen uit het verificatieantwoord. Zie voor een voorbeeld van het ophalen van het toegangstoken door aan te roepen REST-bewerkingen [genereren van een Token toegang](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Fouten opsporen

De volgende fouten kunnen optreden bij het maken van een service-principal:

* **'Authentication_Unauthorized'** of **'geen abonnement gevonden in de context.'** -U hebt deze fout te zien wanneer uw account beschikt niet over de [vereist machtigingen](#required-permissions) op de Azure Active Directory om een app te registreren. Normaal gesproken u deze fout te zien wanneer alleen admin gebruikers in uw Azure Active Directory apps registreren kunnen en uw account niet een beheerder is. Vraag uw beheerder u ofwel toewijzen aan een beheerdersrol of waarmee gebruikers om apps te registreren.

* Uw account **"heeft geen autorisatie om uit te voeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/subscriptions / {guid}."**  -Deze fout te zien wanneer uw account beschikt niet over voldoende machtigingen aan een rol toewijzen aan een identiteit. Vraag de abonnementsbeheerder u toe te voegen aan de rol beheerder voor gebruikerstoegang.

## <a name="sample-applications"></a>Voorbeeldtoepassingen
Zie voor informatie over logboekregistratie in als de toepassing via verschillende platforms:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Volgende stappen
* Zie voor gedetailleerde stappen voor het integreren van een toepassing in Azure voor het beheren van resources [Ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](resource-manager-api-authentication.md).
* Zie voor een meer gedetailleerde uitleg van toepassingen en service-principals [toepassingsobjecten en Service-Principal objecten](../active-directory/active-directory-application-objects.md). 
* Zie voor meer informatie over Azure Active Directory-verificatie, [verificatie scenario's voor Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Zie voor een lijst met beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).

