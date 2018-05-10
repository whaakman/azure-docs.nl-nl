---
title: Een beveiligde Service Fabric-cluster in Azure-Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van een beveiligde Service Fabric-cluster in Azure-Stack
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
ms.date: 05/08/2018
ms.author: mattbriggs
ms.reviewer: shnatara
ms.openlocfilehash: a88d8dd2af94ac796a3b2e3c667fd40a308f02a1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Een Service Fabric-cluster in Azure-Stack implementeren

Gebruik de **Service Fabric-Cluster** item vanuit Azure Marketplace voor het implementeren van een beveiligde Service Fabric-cluster in Azure-Stack. 

Zie voor meer informatie over het werken met Service Fabric [overzicht van Azure Service Frabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) en [scenario's voor beveiliging van Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security), in de documentatie van Azure.

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het implementeren van de Service Fabric-cluster:
1. **Cluster-certificaat**  
   Dit is het X.509-servercertificaat dat u aan de KeyVault toevoegen bij het implementeren van Service Fabric. 
   - De **CN** op dit certificaat als moet overeenkomen met de Fully Qualified Domain Name (FQDN) van het Service Fabric-cluster dat u maakt. 
   - Indeling van het certificaat moet PFX, als de openbare en persoonlijke sleutels vereist zijn. 
   Zie [vereisten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het maken van dit certificaat serverzijde.

    > [!NOTE]  
    > U kunt een zelfondertekend certificaat inplace van het x.509-certificaat voor server gebruiken voor testdoeleinden. Zelfondertekende certificaten hoeven niet overeenkomen met de FQDN-naam van het cluster.

2.  **Beheerder clientcertificaat** dit is het certificaat dat de client gebruiken moet om de Service Fabric-cluster kunt zelfondertekend te verifiëren. Zie [vereisten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het maken van dit certificaat van de client.

3.  **De volgende items moeten beschikbaar zijn in de Stack Azure Marketplace:**
     - **Windows Server 2016** – de sjabloon wordt de installatiekopie van het Windows Server 2016 gebruikt om de cluster te maken.  
     - **Klant-Scriptextensie** -extensie van de virtuele Machine van Microsoft.  
     - **PowerShell Desired Configuration fase** -extensie van de virtuele Machine van Microsoft.


## <a name="add-a-secret-to-key-vault"></a>Een geheim toevoegen aan Key Vault
Als u wilt een Service Fabric-cluster implementeert, moet u de juiste KeyVault *geheim id* of -URL voor het Service Fabric-cluster. De Azure Resource Manager-sjabloon duurt een KeyVault als invoer en vervolgens haalt het certificaat Cluster tijdens de installatie van de Service Fabric-cluster. 

> [!IMPORTANT]  
> U moet PowerShell gebruiken voor het toevoegen van een geheim aan KeyVault voor gebruik met Service Fabric. Gebruik niet de portal.  

Het volgende script gebruiken voor het maken van de KeyVault en voeg de *cluster certificaat* aan. (Zie de [vereisten](#prerequisites).) Voordat u het script uitvoert, het voorbeeldscript controleren en bijwerken van de opgegeven parameters voor uw omgeving. Dit script wordt ook uitvoer van de waarden die u moet opgeven voor de Azure Resource Manager-sjabloon. 

> [!TIP]  
> Voordat het script kan worden voltooid, moet er een openbare aanbieding met de services voor Compute, netwerk, opslag en KeyVault bevat. 

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


Zie voor meer informatie [KeyVault beheren op Azure-Stack met PowerShell](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-kv-manage-powershell).

## <a name="deploy-the-marketplace-item"></a>Implementeer het Marketplace-item

1. Ga in de gebruikersportal naar **nieuw** > **Compute** > **Service Fabric-Cluster**. 

   ![Selecteer de Service Fabric-Cluster](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

2. Voor elke pagina, zoals *basisbeginselen*, de implementatie-formulier invullen. Standaardinstellingen gebruiken als u niet zeker van een waarde bent. Selecteer **OK** om door te gaan naar de volgende pagina:

   ![Basisbeginselen](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

3. Op de *netwerkinstellingen* pagina kunt u bepaalde poorten openen voor uw toepassingen:

   ![Netwerkinstellingen](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

4. Op de *beveiliging* pagina, het toevoegen van de waarden die u hebt gekregen [maken van de KeyVault Azure](#add-a-secret-to-key-vault) en uploadt het geheim.

   Voor de *Admin Client certificaatvingerafdruk*, voer de vingerafdruk van het *Admin clientcertificaat*. (Zie de [vereisten](#prerequisites).)
   
   - Bron Sleutelkluis: Geef het volledige *keyVault-id* tekenreeks in de script-resultaten. 
   - Cluster-certificaat-URL: Geef de volledige URL van de *geheim Id* uit de resultaten van het script. 
   - Cluster certificaatvingerafdruk: Geef de *Cluster certificaatvingerafdruk* uit de resultaten van het script.
   - Certificaatvingerafdrukken Beheerclient: Geef de *Admin Client certificaatvingerafdruk* u hebt gemaakt in de vereisten. 

   ![Uitvoer van het script](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Beveiliging](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

5. Voltooi de wizard en selecteer vervolgens **maken** voor het implementeren van de Service Fabric-Cluster.



## <a name="access-the-service-fabric-cluster"></a>Toegang tot de Service Fabric-Cluster
U kunt toegang tot de Service Fabric-cluster met behulp van de Service Fabric Explorer of een Service Fabric PowerShell.


### <a name="use-service-fabric-explorer"></a>Gebruik Service Fabric Explorer
1.  Controleer of de webbrowser toegang tot uw clientcertificaat Admin heeft en kan worden geverifieerd voor uw Service Fabric-cluster.  

    a. Open Internet Explorer en Ga naar **Internetopties** > **inhoud** > **certificaten**.
  
    b. Selecteer op de certificaten, **importeren** starten de *Wizard Certificaat importeren*, en klik vervolgens op **volgende**. Op de *te importeren bestand* pagina op **Bladeren**, en selecteer de **Admin clientcertificaat** u hebt opgegeven in de Azure Resource Manager-sjabloon.
        
       > [!NOTE]  
       > Dit certificaat is niet het Cluster-certificaat dat eerder is toegevoegd aan de KeyVault.  

    c. Zorg ervoor dat u 'Personal Information Exchange' geselecteerd in de vervolgkeuzelijst voor uitbreiding van de Verkenner-venster.  

       ![Personal information exchange](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. Op de *certificaatarchief* pagina **persoonlijke**, en voltooi de wizard.  
       ![Certificaatarchief](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
2. Zoek de FQDN-naam van uw Service Fabric-cluster:  

    a. Ga naar de resourcegroep die is gekoppeld aan uw Service Fabric-cluster en zoek de *openbaar IP-adres* resource. Selecteer het object dat is gekoppeld aan het openbare IP-adres te openen de *openbaar IP-adres* blade.  

      ![Openbaar IP-adres](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. Op de blade openbare IP-adres, de FQDN-naam wordt weergegeven als *DNS-naam*.  

      ![DNS-naam](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

3. Bekijk de resultaten van de implementatie van de sjabloon voor de URL voor de Service Fabric Explorer en het eindpunt voor de Client verbinding vindt.

4. Ga in uw browser naar https://*FQDN*: 19080. Vervang *FQDN* met de FQDN van uw Service Fabric-cluster uit stap 2.   
   Als u een zelfondertekend certificaat gebruikt hebt, krijgt u een waarschuwing dat de verbinding is niet beveiligd. Selecteer het volgende om door te gaan naar de website **meer informatie**, en vervolgens **gaat u naar de webpagina**. 

5. U moet een certificaat selecteren om te verifiëren naar de site. Selecteer **meer opties**, kies het juiste certificaat en klik vervolgens op **OK** verbinding maken met de Service Fabric Explorer. 

   ![Verifiëren](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



## <a name="use-service-fabric-powershell"></a>Service Fabric PowerShell gebruiken

1. Installeer de *Microsoft Azure Service Fabric SDK* van [voorbereiden van uw ontwikkelomgeving in Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) in de documentatie van Azure Service Fabric.  

2. Nadat de installatie voltooid is, configureert u de systeemomgevingsvariabelen om ervoor te zorgen dat de Service Fabric-cmdlets toegankelijk vanaf PowerShell zijn.  
    
    a. Ga naar **Configuratiescherm** > **systeem en beveiliging** > **System**, en selecteer vervolgens **Geavanceerde systeeminstellingen**.  
    
      ![Het Configuratiescherm](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. Op de **Geavanceerd** tabblad van *Systeemeigenschappen*, selecteer **omgevingsvariabelen**.  

    c. Voor *systeemvariabelen*, bewerk **pad** en zorg ervoor dat **C:\\Program Files\\Microsoft Service Fabric\\bin\\Fabric \\Fabric.Code** wordt boven aan de lijst met omgevingsvariabelen.  

      ![Omgeving variabelenlijst](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

3. Na het wijzigen van de volgorde van de omgevingsvariabelen PowerShell opnieuw en voer vervolgens de volgende PowerShell-script om toegang krijgen tot de Service Fabric-cluster:

   ````PowerShell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ````
   
   > [!NOTE]  
   > Er is geen *https://* vóór de naam van het cluster in het script. 19000-poort is vereist.
 
