---
title: Implementeren van een beveiligde Service Fabric-cluster in Azure Stack | Microsoft Docs
description: Informatie over het implementeren van een beveiligde Service Fabric-cluster in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: shnatara
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: d0d725a57c27fe30215d77a596f6fb3b8c8720d6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098005"
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Een Service Fabric-cluster in Azure Stack implementeren

Gebruik de **Service Fabric-Cluster** item uit de Azure Marketplace om een beveiligd Service Fabric-cluster in Azure Stack implementeren. 

Zie voor meer informatie over het werken met Service Fabric [overzicht van Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) en [Service Fabric-clusterbeveiligingsscenario's](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security), in de documentatie van Azure.

De Service Fabric-cluster in Azure Stack is niet met behulp van de resourceprovider Microsoft.ServiceFabric. In plaats daarvan wordt de Service Fabric-cluster in Azure Stack, een virtuele-machineschaalset met vooraf geïnstalleerde software instellen met behulp van Desired State Configuration (DSC).

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist om de Service Fabric-cluster te implementeren:
1. **Clustercertificaat**  
   Dit is het X.509-servercertificaat dat u aan Key Vault toevoegen bij het implementeren van Service Fabric. 
   - De **CN** op dit certificaat als moet overeenkomen met de volledig domein naam (Fully Qualified Domain Name) van de Service Fabric-cluster dat u hebt gemaakt. 
   - Opmaak van het certificaat moet PFX, als de openbare en persoonlijke sleutels vereist zijn. 
     Zie [vereisten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het maken van deze server-side-certificaat.

     > [!NOTE]  
     > Voor testdoeleinden kunt u een directe zelf-ondertekend certificaat van het x.509-servercertificaat. Zelfondertekende certificaten hoeven niet overeenkomt met de FQDN-naam van het cluster.

1. **Beheerder clientcertificaat** dit is het certificaat dat de client wordt gebruikt voor verificatie bij de Service Fabric-cluster zelf kan worden ondertekend. Zie [vereisten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het maken van dit clientcertificaat.

1. **De volgende items moeten beschikbaar zijn in de Azure Stack Marketplace:**
    - **Windows Server 2016** – de sjabloon maakt gebruik van de installatiekopie van het Windows Server 2016 om het cluster te maken.  
    - **Voor Klantenscripts** -extensie van de virtuele Machine van Microsoft.  
    - **PowerShell Desired Configuration fase** -extensie van de virtuele Machine van Microsoft.


## <a name="add-a-secret-to-key-vault"></a>Een geheim toevoegen aan Key Vault
Voor het implementeren van een Service Fabric-cluster, moet u de juiste KeyVault *geheim id* of de URL voor de Service Fabric-cluster. De Azure Resource Manager-sjabloon wordt een Key Vault gebruikt als invoer. De sjabloon worden vervolgens het Clustercertificaat dat is opgehaald bij de installatie van de Service Fabric-cluster.

> [!IMPORTANT]  
> U moet PowerShell gebruiken voor een geheim toevoegen aan Key Vault voor gebruik met Service Fabric. Gebruik niet de portal.  

Gebruik het volgende script in de Key Vault maken en toevoegen de *clustercertificaat* toe. (Zie de [vereisten](#prerequisites).) Voordat u het script uitvoert, het voorbeeldscript controleren en bijwerken van de opgegeven parameters zodat deze overeenkomt met uw omgeving. Met dit script wordt ook uitvoer van de waarden die u moet opgeven voor de Azure Resource Manager-sjabloon. 

> [!TIP]  
> Voordat het script slagen kan, is er moeten een openbare aanbieding met de services voor berekening, netwerk, opslag en Key Vault. 

  ```PowerShell
    function Get-ThumbprintFromPfx($PfxFilePath, $Password) 
        {
            return New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($PfxFilePath, $Password)
        }
    
    function Publish-SecretToKeyVault ($PfxFilePath, $Password, $KeyVaultName)
       {
            $keyVaultSecretName = "ClusterCertificate"
            $certContentInBytes = [io.file]::ReadAllBytes($PfxFilePath)
            $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
    
            $jsonObject = ConvertTo-Json -Depth 10 ([pscustomobject]@{
                data     = $pfxAsBase64EncodedString
                dataType = 'pfx'
                password = $Password
            })
    
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName -SecretValue $secret
            
            $pfxCertObject = Get-ThumbprintFromPfx -PfxFilePath $PfxFilePath -Password $Password
    
            Write-Host "KeyVault id: " -ForegroundColor Green
            (Get-AzureRmKeyVault -VaultName $KeyVaultName).ResourceId
            
            Write-Host "Secret Id: " -ForegroundColor Green
            (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName).id
    
            Write-Host "Cluster Certificate Thumbprint: " -ForegroundColor Green
            $pfxCertObject.Thumbprint
       }
    
    #========================== CHANGE THESE VALUES ===============================
    $armEndpoint = "https://management.local.azurestack.external"
    $tenantId = "your_tenant_ID"
    $location = "local"
    $clusterCertPfxPath = "Your_path_to_ClusterCert.pfx"
    $clusterCertPfxPassword = "Your_password_for_ClusterCert.pfx"
    #==============================================================================
    
    Add-AzureRmEnvironment -Name AzureStack -ARMEndpoint $armEndpoint
    Login-AzureRmAccount -Environment AzureStack -TenantId $tenantId
    
    $rgName = "sfvaultrg"
    Write-Host "Creating Resource Group..." -ForegroundColor Yellow
    New-AzureRmResourceGroup -Name $rgName -Location $location
    
    Write-Host "Creating Key Vault..." -ForegroundColor Yellow
    $Vault = New-AzureRmKeyVault -VaultName sfvault -ResourceGroupName $rgName -Location $location -EnabledForTemplateDeployment -EnabledForDeployment -EnabledForDiskEncryption
    
    Write-Host "Publishing certificate to Vault..." -ForegroundColor Yellow
    Publish-SecretToKeyVault -PfxFilePath $clusterCertPfxPath -Password $clusterCertPfxPassword -KeyVaultName $vault.VaultName
   ``` 


Zie voor meer informatie, [KeyVault beheren in Azure Stack met PowerShell](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-key-vault-manage-powershell).

## <a name="deploy-the-marketplace-item"></a>De Marketplace-item implementeren

1. Ga in de gebruikersportal naar **+ een resource maken** > **Compute** > **Service Fabric-Cluster**. 

   ![Selecteer Service Fabric-Cluster](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

1. Voor elke pagina, zoals *basisbeginselen*, vul het formulier in de implementatie. Standaardinstellingen gebruiken als u niet zeker van een waarde bent. Selecteer **OK** om door te gaan naar de volgende pagina:

   ![Basisbeginselen](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

1. Op de *netwerkinstellingen* pagina, kunt u specifieke poorten te openen voor uw toepassingen:

   ![Netwerkinstellingen](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

1. Op de *Security* pagina, voegt u de waarden die u hebt verkregen via [het maken van de Azure Key Vault](#add-a-secret-to-key-vault) en het geheim te uploaden.

   Voor de *Admin-Client de vingerafdruk van certificaat*, voer de vingerafdruk van het *Admin clientcertificaat*. (Zie de [vereisten](#prerequisites).)
   
   - De Bronsleutelkluis:  Geef het volledige *Key Vault-id* tekenreeks in de scriptresultaten. 
   - Cluster-URL voor certificaat: Geef de volledige URL van de *geheim Id* in de scriptresultaten. 
   - Cluster de vingerafdruk van certificaat: Geef de *Cluster certificaatvingerafdruk* uit de scriptresultaten.
   - Beheerclient Certificaatvingerafdrukken: Geef de *Admin-Client de vingerafdruk van certificaat* hebt gemaakt in de vereisten. 

   ![Scriptuitvoer](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Beveiliging](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

1. Voltooi de wizard en selecteer vervolgens **maken** naar de Service Fabric-Cluster implementeren.



## <a name="access-the-service-fabric-cluster"></a>Toegang tot de Service Fabric-Cluster
U kunt toegang tot de Service Fabric-cluster met behulp van de Service Fabric Explorer of de Service Fabric PowerShell.


### <a name="use-service-fabric-explorer"></a>Use Service Fabric Explorer
1.  Valideren dat de webbrowser toegang tot uw certificaat voor de Beheerclient heeft en kan worden geverifieerd met uw Service Fabric-cluster.  

    a. Open Internet Explorer en Ga naar **Internetopties** > **inhoud** > **certificaten**.
  
    b. Met certificaten, selecteer **importeren** om te beginnen de *Wizard Certificaat importeren*, en klik vervolgens op **volgende**. Op de *te importeren bestand* pagina op **Bladeren**, en selecteer de **Admin clientcertificaat** u hebt opgegeven voor de Azure Resource Manager-sjabloon.
        
       > [!NOTE]  
       > Dit certificaat is niet het clustercertificaat dat eerder is toegevoegd aan Key Vault.  

    c. Zorg ervoor dat u 'Personal Information Exchange hebt"geselecteerd in de vervolgkeuzelijst voor uitbreiding van het bestand Explorer-venster.  

       ![Personal information exchange](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. Op de *certificaat Store* weergeeft, schakelt **persoonlijke**, en voltooi de wizard.  
       ![Certificaatarchief](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
1. Zoek de FQDN-naam van uw Service Fabric-cluster:  

    a. Ga naar de resourcegroep die is gekoppeld aan uw Service Fabric-cluster en zoek de *openbaar IP-adres* resource. Selecteer het object dat is gekoppeld aan het openbare IP-adres te openen de *openbaar IP-adres* blade.  

      ![Openbaar IP-adres](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. Op de blade openbaar IP-adres, de FQDN-naam wordt weergegeven als *DNS-naam*.  

      ![DNS-naam](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

1. Als u de URL voor de Service Fabric Explorer, en het eindpunt van de clientverbinding zoekt, bekijk de resultaten van de implementatie van de sjabloon.

1. Ga in uw browser naar https://*FQDN*: 19080. Vervang *FQDN* met de FQDN van uw Service Fabric-cluster uit stap 2.   
   Als u een zelfondertekend certificaat hebt gebruikt, krijgt u een waarschuwing dat de verbinding is niet beveiligd. Selecteer om door te gaan naar de website, **meer informatie**, en vervolgens **gaat u naar de webpagina**. 

1. Als u wilt verifiëren aan de site, moet u een certificaat wilt gebruiken. Selecteer **meer opties**, kies het juiste certificaat en klik vervolgens op **OK** verbinding maken met de Service Fabric Explorer. 

   ![Verifiëren](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



## <a name="use-service-fabric-powershell"></a>Service Fabric PowerShell gebruiken

1. Installeer de *Microsoft Azure Service Fabric SDK* van [uw ontwikkelomgeving voorbereiden in Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) in de documentatie van Azure Service Fabric.  

1. Nadat de installatie voltooid is, configureert u de omgevingsvariabelen om ervoor te zorgen dat de Service Fabric-cmdlets toegankelijk is vanuit PowerShell zijn.  
    
    a. Ga naar **Configuratiescherm** > **systeem en beveiliging** > **System**, en selecteer vervolgens **Geavanceerde systeeminstellingen**.  
    
      ![Het Configuratiescherm](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. Op de **Geavanceerd** tabblad van *Systeemeigenschappen*, selecteer **omgevingsvariabelen**.  

    c. Voor *systeemvariabelen*, bewerken **pad** en zorg ervoor dat **C:\\Program Files\\Microsoft Service Fabric\\bin\\Fabric \\Fabric.Code** is aan de bovenkant van de lijst met omgevingsvariabelen.  

      ![Lijst met variabelen van omgeving](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

1. Na het wijzigen van de volgorde van de omgevingsvariabelen, start PowerShell en voer de volgende PowerShell-script om toegang krijgen tot de Service Fabric-cluster:

   ```PowerShell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ```
   
   > [!NOTE]  
   > Er is geen *https://* voordat u de naam van het cluster in het script. 19000-poort is vereist.

## <a name="next-steps"></a>Volgende stappen

[Kubernetes op Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md)
