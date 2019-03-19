---
title: Kubernetes toevoegen aan de Azure Stack Marketplace | Microsoft Docs
description: Informatie over het toevoegen van Kubernetes op Azure Stack Marketplace.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: ca58059716ebebfaf663412b37014ae4f534d0e3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58081505"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Kubernetes op Azure Stack Marketplace toevoegen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!note]  
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie.

Als een Marketplace-item kunt u Kubernetes aanbieden aan uw gebruikers. Uw gebruikers kunnen vervolgens Kubernetes implementeert in een enkele, gecoördineerde bewerking.

Het volgende artikel bekijken met behulp van een Azure Resource Manager-sjabloon te implementeren en de resources voor een zelfstandige Kubernetes-cluster inrichten. Voordat u begint, controleert u uw Azure Stack en de instellingen van de globale Azure-tenant. De vereiste gegevens verzamelen over uw Azure Stack. Benodigde resources toevoegen aan uw tenant en de Azure Stack Marketplace. Het cluster is afhankelijk van een Ubuntu-server, aangepaste scripts en het Kubernetes-Cluster Marketplace-item in de marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Een plan, een aanbieding en een abonnement maken

Maak een plan, een aanbieding en een abonnement voor het Kubernetes-Marketplace-item. U kunt ook gebruik van een bestaand plan en aanbieding.

1. Aanmelden bij de [-beheerportal.](https://adminportal.local.azurestack.external)

1. Maak een plan als het basisplan. Zie voor instructies [een plan maken in Azure Stack](azure-stack-create-plan.md).

1. Maak een aanbieding. Zie voor instructies [een aanbieding maken in Azure Stack](azure-stack-create-offer.md).

1. Selecteer **biedt**, en zoek de aanbieding die u hebt gemaakt.

1. Selecteer **overzicht** in de blade van de aanbieding.

1. Selecteer **in een statuswijziging**. Selecteer **Openbaar**.

1. Selecteer **+ een resource maken** > **aanbiedingen en abonnementen** > **abonnement** om een abonnement te maken.

    a. Voer een **weergavenaam**.

    b. Voer een **gebruiker**. Gebruik Azure AD-account dat is gekoppeld aan uw tenant.

    c. **Beschrijving van de provider**

    d. Stel de **Directory-tenant** bij Azure AD-tenant voor uw Azure Stack. 

    e. Selecteer **bieden**. Selecteer de naam van de aanbieding die u hebt gemaakt. Noteer de abonnement-ID.

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>Maken van een service-principal en de referenties in AD FS

Als u Active Directory Federated Services (AD FS) voor uw identity management-service gebruikt, moet u een service-principal voor gebruikers implementeert een Kubernetes-cluster maken.

1. Maken en exporteren van een zelfondertekend certificaat gebruikt voor de service-principal maken. 

    - U hebt de volgende soorten informatie nodig:

       | Value | Description |
       | ---   | ---         |
       | Wachtwoord | Een nieuw wachtwoord invoeren voor het certificaat. |
       | Pad naar het lokale certificaat | Voer het pad en de naam van het certificaat. Bijvoorbeeld: `c:\certfilename.pfx` |
       | Certificaatnaam | Voer de naam van het certificaat. |
       | Certificaatarchieflocatie |  Bijvoorbeeld: `Cert:\LocalMachine\My` |

    - Open PowerShell met een opdrachtprompt. Voer het volgende script met de parameters die zijn bijgewerkt naar uw waarden:

        ```PowerShell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "CN=<certificate name>"
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2.  Noteer het nieuwe certificaat-ID weergegeven in uw PowerShell-sessie `1C2ED76081405F14747DC3B5F76BB1D83227D824`. De ID wordt gebruikt bij het maken van de service-principal.

    ```PowerShell  
    VERBOSE: Generated new certificate 'CN=<certificate name>' (1C2ED76081405F14747DC3B5F76BB1D83227D824).
    ```

3. Service-principal met behulp van het certificaat maken.

    - U hebt de volgende soorten informatie nodig:

       | Value | Description                     |
       | ---   | ---                             |
       | ERCS IP | In de ASDK, is het eindpunt van de bevoegde normaal gesproken `AzS-ERCS01`. |
       | De naam van de toepassing | Een eenvoudige naam voor de toepassingsservice-principal invoeren. |
       | Certificaatarchieflocatie | Het pad op de computer waarop u het certificaat hebt opgeslagen. Dit wordt aangegeven door de locatie van de store en de certificaat-ID gegenereerd in de eerste stap. Bijvoorbeeld: `Cert:\LocalMachine\My\1C2ED76081405F14747DC3B5F76BB1D83227D824` |

       Wanneer u hierom wordt gevraagd, moet u de volgende referenties gebruiken om te verbinden met het eindpunt van de bevoegdheden. 
        - Gebruikersnaam: Geef het account CloudAdmin in de notatie <Azure Stack domain>\cloudadmin. (Voor ASDK is de naam van de gebruiker azurestack\cloudadmin.)
        - Wachtwoord: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domein administrator-account.

    - Voer het volgende script met de parameters die zijn bijgewerkt naar uw waarden:

        ```PowerShell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        $certStoreLocation="<certificate location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - De details van Service-principal zoeken zoals het onderstaande codefragment

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>De installatiekopie van een Ubuntu-server toevoegen

De volgende Ubuntu-Server-installatiekopie toevoegen aan de Marketplace:

1. Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services**, en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Ubuntu Server` in.

1. Selecteer de nieuwste versie van de server. De volledige versie controleren en ervoor te zorgen dat u de nieuwste versie hebt:
    - **Publisher**: Canonical
    - **Bieden**: UbuntuServer
    - **Version**: 16.04.201806120 (of meest recente versie)
    - **SKU**: 16.04-LTS

1. Selecteer **downloaden.**

## <a name="add-a-custom-script-for-linux"></a>Toevoegen van een aangepast script voor Linux

De Kubernetes uit de Marketplace toevoegen:

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services** en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Custom Script for Linux` in.

1. Het script met het volgende profiel te selecteren:
   - **Bieden**: Aangepast Script voor Linux 2.0
   - **Version**: 2.0.6 (of de meest recente versie)
   - **Publisher**: Microsoft Corp

     > [!Note]  
     > Meer dan één versie van het aangepaste Script voor Linux kan worden vermeld. U moet de laatste versie van het item toevoegen.

1. Selecteer **downloaden.**


## <a name="add-kubernetes-to-the-marketplace"></a>Kubernetes toevoegen aan de marketplace

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services** en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Kubernetes` in.

1. Selecteer `Kubernetes Cluster`.

1. Selecteer **downloaden.**

    > [!note]  
    > Het duurt vijf minuten voor de marketplace-item wordt weergegeven in de Marketplace.

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Bijwerken of verwijderen van de Kubernetes 

Tijdens het bijwerken van het Kubernetes-item, verwijdert u het vorige item in de Marketplace. Volg de instructies in dit artikel voor het toevoegen van dat de Kubernetes bijwerken naar de marketplace.

De Kubernetes-item verwijderen:

1. Verbinding maken met Azure Stack met PowerShell als een operator. Zie voor instructies, [verbinding maken met Azure Stack met PowerShell als operator](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Het huidige item van de Kubernetes-Cluster niet vinden in de galerie.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Noteer de naam van het huidige item, zoals `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Gebruik de volgende PowerShell-cmdlet om het item te verwijderen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Volgende stappen

[Een Kubernetes met Azure Stack implementeren](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Overzicht van services in Azure Stack-aanbieding](azure-stack-offer-services-overview.md)
