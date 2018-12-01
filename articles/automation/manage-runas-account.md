---
title: Azure Automation uitvoeren als-accounts beheren
description: In dit artikel wordt beschreven hoe u uw uitvoeren als-accounts beheren met PowerShell of vanuit de portal.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 09/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7bd84b42cfa61d199d70e02345f9229a45fd7704
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726165"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Azure Automation uitvoeren als-accounts beheren

Run As-accounts in Azure Automation worden gebruikt voor verificatie voor het beheren van resources in Azure met de Azure-cmdlets.

Wanneer u een uitvoeren als-account maakt, maakt u een nieuwe service-principalgebruiker in Azure Active Directory en de rol Inzender toegewezen aan deze gebruiker op het abonnementsniveau. Voor runbooks die gebruikmaken van Hybrid Runbook Workers op Azure virtual machines, kunt u [beheerde identiteiten voor een Azure-resources](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) in plaats van uitvoeren als-accounts om uw Azure-resources te verifiëren.

Er zijn twee typen uitvoeren als-Accounts:

* **Azure uitvoeren als-Account** -dit account wordt gebruikt om Resource Manager deployment model resources te beheren.
  * Er wordt een Azure AD-toepassing met een zelf-ondertekend certificaat gemaakt. Daarnaast wordt er voor de toepassing in Azure AD een service-principalaccount gemaakt en wordt aan dit account de rol Inzender toegewezen in uw huidige abonnement. U kunt deze instelling wijzigen in Eigenaar of een andere rol. Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie.
  * Er wordt een Automation-certificaatasset gemaakt met de naam *AzureRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door de Azure AD-toepassing.
  * Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de toepassing-id, tenant-id, abonnement-id en certificaatvingerafdruk.

* **Azure klassieke uitvoeren als-Account** -dit account wordt gebruikt om klassieke implementatie model resources te beheren.
  * Er wordt een Automation-certificaatasset gemaakt met de naam *AzureClassicRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door het beheercertificaat.
  * Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureClassicRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de naam van het abonnement, de abonnements-id en de certificaatassetnaam.
  
  > [!NOTE]
  > Abonnementen voor Azure Cloud Solution Provider (Azure CSP) ondersteunen alleen de Azure Resource Manager-model, niet - Azure Resource Manager-services zijn niet beschikbaar in het programma. Wanneer u een CSP-abonnement heeft niet de klassiek uitvoeren als-Account gemaakt. Het Azure uitvoeren als-Account wordt nog steeds gemaakt. Zie voor meer informatie over het CSP-abonnementen, [beschikbare services in CSP-abonnementen](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments).

## <a name="permissions"></a>Machtigingen voor het uitvoeren als-accounts configureren

Als u wilt maken of bijwerken van een uitvoeren als-account, moet u de specifieke rechten en machtigingen hebben. Een globale beheerder/Medebeheerder kunt voltooien van de taken. In een situatie waarin u werkt met scheiding van functies, ziet de volgende tabel u een overzicht van de taken, de cmdlet equivalent en de machtigingen die nodig zijn:

|Taak|Cmdlet  |Minimale machtigingen  |
|---|---------|---------|
|Azure AD-toepassing maken|[New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication)     | Developer toepassingsrol        |
|Een referentie toevoegen aan de toepassing.|[New-AzureRmADAppCredential](/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | Beheerder van de toepassing of globale beheerder         |
|Maken en een Azure AD-service-principal ophalen|[Nieuwe AzureRMADServicePrincipal](/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | Beheerder van de toepassing of globale beheerder        |
|Toewijzen of de RBAC-rol voor de opgegeven principal ophalen|[Nieuwe-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | Beheerder van gebruikerstoegang of eigenaar        |
|Maken of verwijderen van een Automation-certificaat|[New-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | Inzender voor resourcegroep         |
|Maken of verwijderen van een Automation-verbinding|[New-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|Inzender voor resourcegroep |

* Een AD-gebruikersaccount met machtigingen heeft die equivalent zijn aan de rol Inzender voor Microsoft.Automation-resources, zoals wordt beschreven in artikel [rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md#contributor).  
* Gebruikers zonder beheerdersrechten in uw Azure AD-tenant kunnen [AD-toepassingen registreren](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions) als de optie **Gebruikers kunnen toepassingen registreren** van de Azure AD-tenant op de pagina **Gebruikersinstellingen** is ingesteld op **Ja**. Als de app-registratie-instelling is ingesteld op **Nee**, moet de gebruiker die deze actie uitvoert een globale beheerder zijn in Azure AD.

Als u niet lid zijn van Active Directory-exemplaar van het abonnement voordat u bent toegevoegd aan de globale beheerder/medebeheerder rol van het abonnement, bent u als Gast toegevoegd. In dit geval ontvangt u een `You do not have permissions to create…` waarschuwing op de **Automation-Account toevoegen** pagina. Gebruikers die zijn toegevoegd aan de rol van globale beheerder/medebeheerder, kunnen worden verwijderd uit het Active Directory-exemplaar van het abonnement en opnieuw worden toegevoegd, zodat ze een volledige gebruiker worden in Active Directory. U kunt deze situatie controleren door in het deelvenster **Azure Active Directory** van Azure Portal **Gebruikers en groepen** te selecteren. Selecteer vervolgens **Alle gebruikers**, de specifieke gebruiker en **Profiel**. De waarde van het kenmerk **Gebruikerstype** onder het gebruikersprofiel mag niet gelijk zijn aan **Gast**.

## <a name="create-a-run-as-account-in-the-portal"></a>Een uitvoeren als-account maken in de Portal

In deze sectie voert u de volgende stappen uit om uw Azure Automation-account bij te werken in Azure Portal. U maakt afzonderlijke Uitvoeren als- en Klassiek Uitvoeren als-accounts. Als u geen klassieke resources hoeft te beheren, kunt u alleen de Uitvoeren als-account van Azure maken.  

1. Meld u aan bij Azure Portal met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Automation**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Automation-accounts**.
3. Selecteer op de pagina **Automation-accounts** uw Automation-account uit de lijst met Automation-accounts.
4. Selecteer in het linkerdeelvenster de optie **Uitvoeren als-accounts** in de sectie **Accountinstellingen**.  
5. Afhankelijk van welk account u nodig hebt, selecteert u **Uitvoeren als-account van Azure**  of **Klassiek Uitvoeren als-account van Azure**. Na deze selectie wordt het deelvenster **Azure uitvoeren als-account toevoegen** of **Klassiek Uitvoeren als-account toevoegen** weergegeven. Controleer de overzichtsgegevens en klik op **Maken** om verder te gaan met het maken van een Uitvoeren als-account.  
6. Terwijl in Azure het Uitvoeren als-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen. Er wordt ook een banner weergegeven waarin staat dat het account wordt gemaakt. Dit proces kan enkele minuten duren.  

## <a name="create-run-as-account-using-powershell"></a>Uitvoeren als-account maken met behulp van PowerShell

## <a name="prerequisites"></a>Vereisten

De volgende lijst bevat de vereisten voor het uitvoeren als-account maken in PowerShell:

* Windows 10 of Windows Server 2016 met Azure Resource Manager-modules 3.4.1 en hoger. Het PowerShell-script biedt geen ondersteuning voor eerdere versies van Windows.
* Azure PowerShell 1.0 en hoger. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor meer informatie over de PowerShell 1.0-release.
* Een Automation-account waarnaar wordt verwezen als de waarde voor de parameter *– AutomationAccountName* en *- ApplicationDisplayName*.
* Machtigingen heeft die equivalent zijn aan wat wordt weergegeven in [vereist machtigingen voor het uitvoeren als-accounts configureren](#permissions)

Om op te halen van de waarden voor *SubscriptionID*, *ResourceGroup*, en *AutomationAccountName*, dit zijn vereiste parameters voor het script, voer de volgende stappen uit:

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Automation**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Automation-accounts**.
1. Selecteer op de Automation-accountpagina uw Automation-account en selecteer vervolgens onder **Accountinstellingen** de optie **Eigenschappen**.  
1. Houd er rekening mee de **abonnements-ID**, **naam**, en **resourcegroep** waarden op de **eigenschappen** pagina.

   ![De pagina Automation-account 'Eigenschappen'](media/manage-runas-account/automation-account-properties.png)

Dit PowerShell-script biedt ondersteuning voor de volgende configuraties:

* Een Uitvoeren als-account maken met een zelfondertekend certificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat.
* Maak een Uitvoeren als-account en een klassiek Uitvoeren als-account via een certificaat dat is uitgegeven door de certificeringsinstantie van uw bedrijf.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government.

>[!NOTE]
> Als u ervoor kiest om een klassiek Uitvoeren als-account te maken, moet u na het uitvoeren van het script het openbare certificaat (bestandsnaamextensie .cer) uploaden naar het beheerarchief voor het abonnement waarin het Automation-account is gemaakt.

1. Sla het volgende script op uw computer op. Sla het bestand in dit voorbeeld op met de bestandsnaam *New-RunAsAccount.ps1*.

   Het script maakt gebruik van meerdere Azure Resource Manager-cmdlets om resources te maken. De volgende tabel ziet u de cmdlets en hun machtigingen die nodig zijn.

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId) 
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
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

    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    Connect-AzureRmAccount -Environment $EnvironmentName 
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

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
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

    > [!IMPORTANT]
    > **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt [uw-modules bijwerken](automation-update-azure-modules.md) in uw Automation -Account.

1. Start op uw computer **Windows PowerShell** op vanaf het **Start**scherm met verhoogde gebruikersrechten.
1. Ga vanuit de opdrachtregel-shell met verhoogde bevoegdheden naar de map die het script bevat dat u in stap 1 hebt gemaakt.  
1. Voer het script uit met de parameterwaarden voor de configuratie die u nodig hebt.

    **Een Uitvoeren als-account maken met een zelfondertekend certificaat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een bedrijfscertificaat**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government**
  
    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
    ```

    > [!NOTE]
    > Nadat het script is uitgevoerd, wordt u gevraagd zich te verifiëren met Azure. Meld u aan met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.

Let op het volgende nadat het script is uitgevoerd:

* Als u een klassiek Uitvoeren als-account hebt gemaakt met een zelfondertekend openbaar certificaat (.cer-bestand), wordt het door het script gemaakt en opgeslagen in de map met tijdelijke bestanden op uw computer, onder het gebruikersprofiel *%USERPROFILE%\AppData\Local\Temp*, dat u hebt gebruikt voor het uitvoeren van de PowerShell-sessie.

* Gebruik dit certificaat als u een klassiek Uitvoeren als-account hebt gemaakt met een openbaar certificaat (.cer-bestand). Volg de instructies voor [een API-beheercertificaat uploaden naar de Azure portal](../azure-api-management-certs.md).

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Een Uitvoeren als- of klassiek Uitvoeren als-account verwijderen

In dit gedeelte wordt beschreven hoe u uw Uitvoeren als- of klassieke Uitvoeren als-account kunt verwijderen en opnieuw kunt maken. Als u deze actie uitvoert, blijft het Automation-account behouden. Nadat u het Uitvoeren als- of klassieke Uitvoeren als-account hebt verwijderd, kunt u het opnieuw maken in Azure Portal.

1. Open in Azure Portal het Automation-account.

2. Selecteer op de pagina **Automation-account** de optie **Uitvoeren als-accounts**.

3. Selecteer op de pagina **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account dat u wilt verwijderen. Klik vervolgens in het deelvenster **Eigenschappen** voor het geselecteerde account op **Verwijderen**.

 ![Uitvoeren als-account verwijderen](media/manage-runas-account/automation-account-delete-runas.png)

1. Terwijl het account wordt verwijderd, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

1. Nadat het account is verwijderd, kunt u het opnieuw maken door op de eigenschappenpagina **Uitvoeren als-accounts** de optie **Azure Uitvoeren als-account** te selecteren.

 ![Het Automation uitvoeren als-account opnieuw maken](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>Zelfondertekend certificaat vernieuwen

Op een bepaald moment voordat uw uitvoeren als-account is verlopen, moet u het certificaat te vernieuwen. Als u denkt dat het Uitvoeren als-account is aangetast, kunt u het verwijderen en opnieuw maken. In deze sectie wordt besproken hoe u deze bewerkingen uitvoert.

Het zelfondertekende certificaat dat u voor het Uitvoeren als-account hebt gemaakt, verloopt één jaar na de aanmaakdatum. U kunt het certificaat op elk gewenst moment vernieuwen voordat het verloopt. Wanneer u deze verlengt, wordt het huidige geldige certificaat behouden om ervoor te zorgen dat eventuele runbooks die in de wachtrij staan of nog actief en die worden geverifieerd met het account uitvoeren als dit een negatieve worden niet beïnvloed. Het certificaat blijft geldig tot de vervaldatum.

> [!NOTE]
> Als u het Uitvoeren als-account voor Automation hebt geconfigureerd om een certificaat te gebruiken dat is uitgegeven door de certificeringsinstantie van uw bedrijf, en u deze optie gebruikt, wordt het bedrijfscertificaat vervangen door een zelfondertekend certificaat.

Ga als volgt te werk om het certificaat te vernieuwen:

1. Open in Azure Portal het Automation-account.

1. Selecteer **Run As-Accounts** onder **Accountinstellingen**.

    ![Eigenschappendeelvenster voor Automation-account](media/manage-runas-account/automation-account-properties-pane.png)

1. Selecteer op de pagina **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account waarvoor u het certificaat wilt vernieuwen.

1. Klik in het deelvenster **Eigenschappen** voor het geselecteerde account op **Certificaat vernieuwen**.

    ![Certificaat vernieuwen voor Uitvoeren als-account](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Terwijl het certificaat wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen. 

## <a name="limiting-run-as-account-permissions"></a>Uitvoeren als-accountmachtigingen beperken

Als u wilt beheren die zijn gericht op van automation voor resources in Azure Automation, krijgt de uitvoeren als-account standaard Inzender-rechten in het abonnement. Als u nodig hebt om te beperken wat de RunAs-service-principal kunt doen, kunt u het account verwijderen uit de inzendersrol aan het abonnement en dit toevoegen als een bijdrager aan de resourcegroepen die u wilt opgeven.

Selecteer in de Azure portal, **abonnementen** en kies het abonnement van uw Automation-Account. Selecteer **toegangsbeheer (IAM)** en selecteer vervolgens de **roltoewijzingen** tabblad. Zoek naar de service-principal voor uw Automation-Account (het ziet eruit als \<AutomationAccountName\>_unique-id). Selecteer het account en klikt u op **verwijderen** te verwijderen uit het abonnement.

![Abonnement inzenders](media/manage-runas-account/automation-account-remove-subscription.png)

Selecteer de service-principal toevoegen aan een resourcegroep, de resourcegroep in Azure portal en selecteer **toegangsbeheer (IAM)**. Selecteer **roltoewijzing toevoegen**, Hiermee opent u de **roltoewijzing toevoegen** pagina. Voor **rol**, selecteer **Inzender**. In de **Selecteer** tekstvak Typ de naam van de service-principal voor uw uitvoeren als-account in en selecteert u deze in de lijst. Klik op **Opslaan** om de wijzigingen op te slaan. Deze stappen voor de resourcegroepen die u wilt uw Azure Automation uitvoeren als-service-principal toegang te geven.

## <a name="misconfiguration"></a>Onjuiste configuratie

Bepaalde configuratie-items die nodig zijn voor het juist functioneren van het Uitvoeren als- of klassieke Uitvoeren als-account, zijn mogelijk verwijderd of niet juist gemaakt tijdens de initiële configuratie. Dit zijn onder meer de volgende items:

* Certificaatasset
* Verbindingsasset
* Uitvoeren als-account is verwijderd uit de rol Inzender
* Service-principal of toepassing in Azure AD

In het voorgaande en andere gevallen van onjuiste configuratie detecteert het Automation-account de wijzigingen en geeft het de status *Onvolledig* weer op de eigenschappenpagina **Uitvoeren als-accounts** voor het account.

![Onvolledige Uitvoeren als-configuratiestatus](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Als u het Uitvoeren als-account selecteert, wordt het volgende foutbericht weergegeven in het deelvenster **Eigenschappen** van het account:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

U kunt deze problemen met het Uitvoeren als-account snel oplossen door het account te verwijderen en opnieuw te maken.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Service-Principals [toepassing en Service-Principal-objecten](../active-directory/develop/app-objects-and-service-principals.md).
* Zie voor meer informatie over certificaten en Azure-services, [overzicht van certificaten voor Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
