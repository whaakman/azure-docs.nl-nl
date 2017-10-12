---
title: Een Azure Automation Uitvoeren als-account maken met PowerShell | Microsoft Docs
description: In dit artikel wordt beschreven hoe u met PowerShell een upgrade van uw Automation-account uitvoert om Uitvoeren als-accounts te maken als u deze stap in het begin niet hebt uitgevoerd vanuit de portal.
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
ms.openlocfilehash: fb23b3ea41910687fd586f80e5dd327344991e0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a>Automation Uitvoeren als-account bijwerken met PowerShell
In de volgende gevallen kunt u PowerShell gebruiken om uw bestaande Automation-account bij te werken:

* U maakt wel een Automation-account, maar geen Uitvoeren als-account.
* U gebruikt al een Automation-account voor het beheer van Resource Manager-resources en u wilt het account bijwerken met het Uitvoeren als-account voor runbookverificatie.
* U gebruikt al een Automation-account voor het beheer van klassieke resources en u wilt dit bijwerken, zodat u het klassieke Uitvoeren als-account kunt gebruiken in plaats van een nieuw account te maken, en uw runbooks en activa daarnaartoe te migreren.   
* U wilt een Uitvoeren als-account maken en een klassiek Uitvoeren als-account via een certificaat dat is uitgegeven door de certificeringsinstantie van uw bedrijf.

## <a name="prerequisites"></a>Vereisten

* Het script kan alleen worden uitgevoerd op Windows 10 en Windows Server 2016 met Azure Resource Manager-modules 2.01 en hoger. Uitvoeren wordt niet ondersteund in eerdere versies van Windows.
* Azure PowerShell 1.0 en hoger. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over de PowerShell 1.0-release.
* Een Automation-account waarnaar wordt verwezen als de waarde voor de parameter *– AutomationAccountName* en *- ApplicationDisplayName* in het volgende PowerShell-script.

Ga als volgt te werk om de waarden op te halen voor *SubscriptionID*, *ResourceGroup* en *AutomationAccountName*, die vereiste parameters zijn voor de scripts:
1. Selecteer in Azure Portal uw Automation-account op de blade **Automation-account** en selecteer **Alle instellingen**. 
2. Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**. 
3. Let op de waarden op de blade **Eigenschappen**.

![De blade Eigenschappen voor het Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a>PowerShell-script voor Uitvoeren als-account maken
Dit PowerShell-script biedt ondersteuning voor de volgende configuraties:

* Een Uitvoeren als-account maken met een zelfondertekend certificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een bedrijfscertificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government.

Afhankelijk van de configuratieoptie die u selecteert, maakt het script de volgende items.

**Voor Uitvoeren als-accounts:**

* Er wordt een Azure AD-toepassing gemaakt die wordt geëxporteerd met het zelfondertekende certificaat of de openbare sleutel van het bedrijfscertificaat en een service-principalaccount voor deze toepassing in Azure AD. Daarnaast wordt voor dit account de rol Inzender toegewezen in uw huidige abonnement. U kunt deze instelling wijzigen in Eigenaar of een andere rol. Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie.
* Er wordt een Automation-certificaatasset gemaakt met de naam *AzureRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door de Azure AD-toepassing.
* Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de toepassing-id, tenant-id, abonnement-id en certificaatvingerafdruk.

**Voor klassieke uitvoeren als-accounts:**

* Er wordt een Automation-certificaatasset gemaakt met de naam *AzureClassicRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door het beheercertificaat.
* Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureClassicRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de naam van het abonnement, de abonnements-id en de certificaatassetnaam.

>[!NOTE]
> Als u ervoor kiest om een klassiek Uitvoeren als-account te maken, moet u na het uitvoeren van het script het openbare certificaat (bestandsnaamextensie .cer) uploaden naar het beheerarchief voor het abonnement waarin het Automation-account is gemaakt.
> 

1. Sla het volgende script op uw computer op. Sla het bestand in dit voorbeeld op met de bestandsnaam *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Start op uw computer **Windows PowerShell** op vanaf het **Start**scherm met verhoogde gebruikersrechten.
3. Ga vanuit de opdrachtregel-shell met verhoogde bevoegdheden naar de map die het script bevat dat u in stap 1 hebt gemaakt.  
4. Voer het script uit met de parameterwaarden voor de configuratie die u nodig hebt.

    **Een Uitvoeren als-account maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een bedrijfscertificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Nadat het script is uitgevoerd, wordt u gevraagd zich te verifiëren met Azure. Meld u aan met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
    >
    >

Let op het volgende nadat het script is uitgevoerd:
* Als u een klassiek Uitvoeren als-account hebt gemaakt met een zelfondertekend openbaar certificaat (.cer-bestand), wordt het door het script gemaakt en opgeslagen in de map met tijdelijke bestanden op uw computer, onder het gebruikersprofiel *%USERPROFILE%\AppData\Local\Temp*, dat u hebt gebruikt voor het uitvoeren van de PowerShell-sessie.
* Gebruik dit certificaat als u een klassiek Uitvoeren als-account hebt gemaakt met een openbaar certificaat (.cer-bestand). Volg de instructies voor het [uploaden van een API-beheercertificaat naar de klassieke Azure-portal](../azure-api-management-certs.md) en valideer de configuratie van de referenties met klassieke implementatieresources met behulp van de [Voorbeeldcode voor verificatie bij Resource Manager-resources](automation-verify-runas-authentication.md#classic-run-as-authentication). 
* Als u *geen* klassiek Uitvoeren als-account hebt gemaakt, verifieert u met Resource Manager-resources en valideert u de configuratie van de referenties met behulp van de [voorbeeldcode om verificatie met Service Management-resources uit te voeren](automation-verify-runas-authentication.md#automation-run-as-authentication).

## <a name="next-steps"></a>Volgende stappen
* Zie [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md) (Toepassingsobjecten en service-principalobjecten) voor meer informatie over service-principals.
* Voor meer informatie over certificaten en Azure-services raadpleegt u [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md) (Overzicht van certificaten voor Azure Cloud Services).
