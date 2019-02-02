---
title: Implementeren van Kubernetes op Azure Stack met behulp van Active Directory Federated Services (AD FS) | Microsoft Docs
description: Informatie over het implementeren van Kubernetes op Azure Stack met behulp van Active Directory Federated Services (AD FS).
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
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: fa95915615efdd8832a983d03a600beec23d1247
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658943"
---
# <a name="deploy-kubernetes-to-azure-stack-using-active-directory-federated-services"></a>Kubernetes op Azure Stack met behulp van Active Directory Federated Services implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie.

U kunt de stappen in dit artikel om te implementeren en instellen van de resources voor Kubernetes. Volg deze stappen als Active Directory Federated Services (AD FS) de identity management-service is.

## <a name="prerequisites"></a>Vereisten 

Als u wilt beginnen, zorg ervoor dat u de juiste machtigingen hebt en dat uw Azure Stack gereed is.

1. Een SSH openbare en persoonlijke sleutelpaar genereren om aan te melden bij de Linux-VM in Azure Stack. U moet de openbare sleutel bij het maken van het cluster.

    Zie voor instructies over het genereren van een sleutel [SSH-sleutel genereren](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Controleer of u een geldig abonnement in de portal van uw Azure Stack-tenant hebben en dat u onvoldoende openbare IP hebt-adressen beschikbaar om toe te voegen nieuwe toepassingen.

    Het cluster kan niet worden geïmplementeerd naar een Azure Stack **beheerder** abonnement. U moet gebruiken een **gebruiker** abonnement. 

1. Als u geen Kubernetes-Cluster in de marketplace, neemt u contact op met uw Azure Stack-beheerder.

## <a name="create-a-service-principal"></a>Een service-principal maken

U moet samenwerken met uw Azure Stack-beheerder om het instellen van uw service-principal bij gebruik van AD FS als uw oplossing voor identiteit. De service-principal geeft uw toepassing toegang heeft tot de Azure Stack-resources.

1. De beheerder van uw Azure Stack biedt u een certificaat en de gegevens voor de service-principal. Deze informatie moet er als volgt uitzien:

    ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
    ```

2. Uw nieuwe service-principal een rol als een bijdrager aan uw abonnement toewijzen. Zie voor instructies [een rol toewijzen](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal#assign-role-to-service-principal).

3. Maak een key vault voor het opslaan van uw certificaat voor de implementatie.

    - U hebt de volgende soorten informatie nodig:

        | Value | Description |
        | ---   | ---         |
        | Azure Resource Manager Endpoint | De Microsoft Azure Resource Manager is een raamwerk waarmee beheerders te implementeren, beheren en bewaken van Azure-resources. Deze taken kunnen worden verwerkt in Azure Resource Manager als een groep, in plaats van afzonderlijk, in één bewerking.<br>Het eindpunt in de Azure Stack Development Kit (ASDK) is: `https://management.local.azurestack.external/`<br>Het eindpunt in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
        | Uw abonnements-ID | De [abonnements-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack. |
        | Naam van de gebruiker | De naam van de gebruiker. |
        | Naam van de resourcegroep  | De naam van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep. De resourcenaam moet alleen alfanumerieke tekens en kleine letters. |
        | De naam van de Key Vault | De naam van de kluis.<br> Regex-patroon: `^[a-zA-Z0-9-]{3,24}$` |
        | Resourcegroeplocatie | De locatie van de resourcegroep. Dit is de regio die u voor uw Azure Stack-installatie kiest. |

    - Open PowerShell met een opdrachtprompt. Voer het volgende script met de parameters die zijn bijgewerkt naar uw waarden:

    ```PowerShell  
        $armEndpoint="<Azure Resource Manager Endpoint>"
        $subscriptionId="<Your Subscription ID>"
        $username="<your user name >"
        $resource_group_name = "<the resource group name >"
        $key_vault_name = "<keyvault name>"
        $resource_group_location="<resource group location>"
        
        # Login Azure Stack Environment
        Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
        $mycreds = Get-Credential
        Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
        
        # Create new Resource group and key vault
        New-AzureRmResourceGroup -Name $resource_group_name -Location $resource_group_location -Force
        
        # Note, Do not omit -EnabledForTemplateDeployment flag
        New-AzureRmKeyVault -VaultName $key_vault_name -ResourceGroupName $resource_group_name -Location local -EnabledForTemplateDeployment
        
        # Obtain the security identifier(SID) of the active directory user
        $adUser = Get-ADUser -Filter "Name -eq '$username'" -Credential $mycreds
        $objectSID = $adUser.SID.Value
        # Set the key vault access policy
        Set-AzureRmKeyVaultAccessPolicy -VaultName $key_vault_name -ResourceGroupName $resource_group_name -ObjectId $objectSID -BypassObjectIdValidation -PermissionsToKeys all -PermissionsToSecrets all
    ```

4. Upload uw certificaat naar de Key Vault.

    - U hebt de volgende soorten informatie nodig:

        | Value | Description |
        | ---   | ---         |
        | Pad naar het certificaat | De FQDN-naam of het bestand pad naar het certificaat. |
        | Certificaatwachtwoord | Wachtwoord voor het certificaat. |
        | Geheime naam | De geheime sleutel die in de vorige stap zijn geproduceerd. |
        | De naam van de Key Vault | De naam van de keyvault account in de vorige stap hebt gemaakt. |
        | Azure Resource Manager Endpoint | Het eindpunt in de Azure Stack Development Kit (ASDK) is: `https://management.local.azurestack.external/`<br>Het eindpunt in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
        | Uw abonnements-ID | De [abonnements-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack. |

    - Open PowerShell met een opdrachtprompt. Voer het volgende script met de parameters die zijn bijgewerkt naar uw waarden:

    ```PowerShell  
        
    # upload the pfx to key vault
    $tempPFXFilePath = "<certificate path>"
    $password = "<certificate password>"
    $keyVaultSecretName = "<secret name>"
    $keyVaultName = "<keyvault name>"
    $armEndpoint="<Azure Resource Manager Endpoint>"
    $subscriptionId="<Your Subscription ID>"
    # Login Azure Stack Environment
    Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
    $mycreds = Get-Credential
    Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
    
    $certContentInBytes = [io.file]::ReadAllBytes($tempPFXFilePath)
    $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
    $jsonObject = @"
    {
    "data": "$pfxAsBase64EncodedString",
    "dataType" :"pfx",
    "password": "$password"
    }
    "@
    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret 
    ```

## <a name="deploy-kubernetes"></a>Implementeren van Kubernetes

1. Open de [Azure Stack-portal](https://portal.local.azurestack.external).

1. Selecteer **+ een resource maken** > **Compute** > **Kubernetes-Cluster**. Klik op **Create**.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Basisbeginselen

1. Selecteer **basisbeginselen** in Kubernetes-Cluster maken.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Selecteer uw **abonnement** -id.

1. Voer de naam van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep. De resourcenaam moet alleen alfanumerieke tekens en kleine letters.

1. Selecteer de **locatie** van de resourcegroep. Dit is de regio die u voor uw Azure Stack-installatie kiest.

### <a name="2-kubernetes-cluster-settings"></a>2. Instellingen voor Kubernetes-Cluster

1. Selecteer **Kubernetes-Cluster instellingen** in Kubernetes-Cluster maken.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-adfs.png)

1. Voer de **Linux VM-Beheerdersgebruikersnaam**. De naam van de gebruiker voor de virtuele Linux-Machines die deel van het Kubernetes-cluster uitmaken en DVM.

1. Voer de **openbare SSH-sleutel** gebruikt voor verificatie op alle Linux-machines gemaakt als onderdeel van de Kubernetes-cluster en DVM.

1. Voer de **Master profiel DNS-voorvoegsel** die uniek is voor de regio. Dit moet een regio-unieke naam, zoals `k8s-12345`. Probeer te hebt gekozen deze gelijk zijn aan de resourcegroep een naam als best practice.

    > [!Note]  
    > Gebruik een master profiel van nieuwe en unieke DNS-voorvoegsel voor elk cluster.

1. Selecteer de **Kubernetes Master Pool profiel aantal**. Het aantal bevat het aantal knooppunten in de master-pool. Er mag uit 1 tot en met 7. Deze waarde moet een oneven getal.

1. Selecteer **de VMSize van de master-VM's van Kubernetes**.

1. Selecteer de **groep Kubernetes-profiel knooppunten**. Het aantal bevat het aantal agents in het cluster. 

1. Selecteer de **Opslagprofiel**. U kunt ervoor kiezen **Blob schijf** of **beheerde schijf**. Hiermee geeft u het VM-grootte van de Kubernetes-knooppunt VM's. 

1. Selecteer **ADFS** voor de **Azure Stack-identiteitssysteem** voor uw Azure Stack-installatie.

1. Voer de **Service-Principal ClientId** dit wordt gebruikt door de Kubernetes Azure-cloud-provider. De Client-ID die wordt geïdentificeerd als de toepassings-ID wanneer uw Azure Stack-beheerder heeft de service-principal gemaakt.

1. Voer de **Key Vault-resourcegroep**. 

1. Voer de **sleutelkluisnaam**.

1. Voer de **Key Vault-geheim**.

1. Voer de **Kubernetes Azure Cloud Provider-versie**. Dit is de versie van de Kubernetes Azure-provider. Azure Stack brengt een aangepaste Kubernetes-build voor elke Azure Stack-versie.

### <a name="3-summary"></a>3. Samenvatting

1. Selecteer Samenvatting. De blade wordt een validatiebericht voor de configuratie-instellingen van uw Kubernetes-Cluster.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Controleer uw instellingen.

3. Selecteer **OK** om uw cluster te implementeren.

> [!TIP]  
>  Als u vragen over uw implementatie hebt, kunt u uw vragen of zien als iemand de vraag in al is beantwoord de [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met uw cluster](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)