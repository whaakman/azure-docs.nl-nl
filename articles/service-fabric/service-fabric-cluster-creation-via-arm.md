---
title: Een Azure Service Fabric-cluster maken | Microsoft Docs
description: Meer informatie over het instellen van een beveiligd Service Fabric-cluster in Azure met behulp van Azure Resource Manager.  U kunt een cluster met behulp van een standaardsjabloon of uw eigen clustersjabloon maken.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/16/2018
ms.author: aljo
ms.openlocfilehash: 4ebd53db9622c5a40f67cba04aa35cbfbaa78c8d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446110"
---
# <a name="create-a-service-fabric-cluster-using-azure-resource-manager"></a>Maken van een Service Fabric-cluster met behulp van Azure Resource Manager 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure-portal](service-fabric-cluster-creation-via-portal.md)
>
>

Een [Azure Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een netwerk verbonden reeks virtuele machines waarop uw microservices worden geïmplementeerd en beheerd.  Een Service Fabric-cluster worden uitgevoerd in Azure is een Azure-resource en geïmplementeerd met behulp van Azure Resource Manager. Dit artikel wordt beschreven hoe u een beveiligd Service Fabric-cluster in Azure met behulp van de Resource Manager implementeert. U kunt een standaardsjabloon voor het cluster of een aangepaste sjabloon gebruiken.  Als u nog een aangepaste sjabloon hebt, kunt u [informatie over het maken van een](service-fabric-cluster-creation-create-template.md).

Clusterbeveiliging is geconfigureerd als een cluster is eerste ingesteld en kan later worden gewijzigd. Lees voordat u een cluster instelt, [Service Fabric-clusterbeveiligingsscenario's][service-fabric-cluster-security]. In Azure, Service Fabric gebruikt x509-certificaat voor het beveiligen van uw cluster en de eindpunten, verifiëren van clients en -gegevens versleutelen. Azure Active Directory wordt ook aanbevolen voor beveiligde toegang tot eindpunten voor beheer. Azure AD-tenants en gebruikers moeten worden gemaakt voordat het cluster te maken.  Lees voor meer informatie, [instellen van Azure AD om clients te verifiëren](service-fabric-cluster-creation-setup-aad.md).

Als u het maken van een productiecluster u productiewerkbelastingen worden uitgevoerd, raden we u eerst lezen via de [productie gereedheid controlelijst](service-fabric-production-readiness-checklist.md).

## <a name="prerequisites"></a>Vereisten 
In dit artikel gebruikt u de Service Fabric-RM powershell of Azure CLI-modules om een cluster te implementeren:

* [Azure PowerShell 4.1 en hoger][azure-powershell]
* [Azure CLI versie 2.0 en hoger][azure-CLI]

U vindt de referentiedocumentatie voor de Service Fabric-modules:
* [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric)
* [AZ SF CLI-module](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Voordat u een van de opdrachten in dit artikel uitvoert, eerst aanmelden bij Azure.

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subscriptionId>
```

```azurecli
az login
az account set --subscription $subscriptionId
```

## <a name="create-a-new-cluster-using-a-system-generated-self-signed-certificate"></a>Maak een nieuw cluster met behulp van een zelfondertekend certificaat van het systeem gegenereerde

Gebruik de volgende opdrachten om een cluster dat is beveiligd met een zelf-ondertekend certificaat door het systeem gegenereerde te maken. Met deze opdracht stelt u een primaire cluster-certificaat dat wordt gebruikt voor clusterbeveiliging en het instellen van beheerderstoegang management-bewerkingen met behulp van dat certificaat uit te voeren.  Zelfondertekende certificaten zijn handig voor testclusters te beveiligen.  Productieclusters moeten zijn beveiligd met een certificaat van een certificeringsinstantie (CA).

### <a name="use-the-default-cluster-template-that-ships-in-the-module"></a>Gebruik de standaardsjabloon voor het cluster dat wordt meegeleverd met de module

Gebruik de volgende opdracht voor het maken van een cluster snel, door de minimale parameters, met behulp van de standaardsjabloon op te geven.

De sjabloon die wordt gebruikt, is beschikbaar op de [voorbeeldsjablonen van Azure Service Fabric: windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

De volgende opdracht kunt maken beide Windows of Linux-clusters, moet u het besturingssysteem dienovereenkomstig opgeven. De PowerShell/CLI-opdrachten wordt ook het certificaat in de opgegeven *CertificateOutputFolder*, maar zorg ervoor dat certificatenmap al hebt gemaakt. De opdracht wordt ook andere parameters, zoals VM-SKU.

> [!NOTE]
> De volgende PowerShell-opdracht werkt alleen met Azure Resource Manager PowerShell versie 6.1 >. Om te controleren of de huidige versie van Azure Resource Manager PowerShell-versie, voer de volgende PowerShell-opdracht 'Get-Module AzureRM'. Ga als volgt [deze koppeling](/powershell/azure/azurerm/install-azurerm-ps) uw Azure Resource Manager PowerShell-versie bijwerken. 
>
>

Implementatie van het cluster met behulp van PowerShell:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password123!@#" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password4321!@#" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser
```

Het cluster met behulp van Azure CLI implementeren:

```azurecli
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!4321"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser
```

### <a name="use-your-own-custom-template"></a>Uw eigen aangepaste sjabloon gebruiken

Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Meer informatie over het [aanpassen van de clustersjabloon][customize-your-cluster-template].

Als u al een aangepaste sjabloon hebt, moet u controleren of alle drie certificaat betrekking heeft parameters in de sjabloon en de parameter-bestand als volgt een naam en waarden zijn null zijn als volgt:

```json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

Implementatie van het cluster met behulp van PowerShell:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Het cluster met behulp van Azure CLI implementeren:

```azurecli
declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath
```

## <a name="create-a-new-cluster-using-your-own-x509-certificate"></a>Maak een nieuw cluster met behulp van uw eigen X.509-certificaat

Gebruik de volgende opdracht om te maken van de cluster, hebt u een certificaat dat u gebruiken wilt voor het beveiligen van uw cluster met.

Als dit een CA ondertekend certificaat dat u uiteindelijk wordt gebruik voor andere doeleinden dan ook, klikt u vervolgens het verdient aanbeveling dat u een afzonderlijke resourcegroep specifiek voor uw key vault bieden. Het is raadzaam dat u de key vault in een eigen resourcegroep geplaatst. Deze actie kunt u de reken- en -resourcegroepen op, met inbegrip van de resourcegroep waarin uw Service Fabric-cluster, zonder verlies van uw sleutels en geheimen verwijderen. **De resourcegroep waarin uw key vault *moet zich in dezelfde regio* als het cluster dat wordt gebruikt.**

### <a name="use-the-default-five-node-one-node-type-template-that-ships-in-the-module"></a>Gebruik de standaard vijf knooppunten, één knooppunt type sjabloon die wordt geleverd in de module
De sjabloon die wordt gebruikt, is beschikbaar op de [Azure-voorbeelden: Windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Implementatie van het cluster met behulp van PowerShell:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 
```

Het cluster met behulp van Azure CLI implementeren:

```azurecli
declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser
```

### <a name="use-your-own-custom-cluster-template"></a>Gebruik uw eigen aangepaste cluster-sjabloon
Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Meer informatie over het [aanpassen van de clustersjabloon][customize-your-cluster-template].

Als u al een aangepaste sjabloon hebt, zorg ervoor dat zijn dubbele-Controleer of alle drie certificaat gerelateerde parameters in de sjabloon en de parameter-bestand zijn met de naam als volgt en waarden null als volgt.

```json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

Implementatie van het cluster met behulp van PowerShell:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword
```

Het cluster met behulp van Azure CLI implementeren:

```azurecli
declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

### <a name="use-a-pointer-to-a-secret-uploaded-into-a-key-vault"></a>Een verwijzing naar een geheim dat is geüpload naar een key vault gebruiken

Voor het gebruik van een bestaande sleutelkluis, de key vault moet [ingeschakeld voor implementatie](../key-vault/key-vault-manage-with-cli2.md#bkmk_KVperCLI) om toe te staan van de compute resourceprovider certificaten van het verkrijgen en installeren op clusterknooppunten.

Implementatie van het cluster met behulp van PowerShell:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -SecretIdentifier $secretId -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Het cluster met behulp van Azure CLI implementeren:

```azurecli
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier az $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligd cluster worden uitgevoerd in Azure. Volgende [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en leer hoe u [toepassingsgeheimen beheren](service-fabric-application-secret-management.md).

Zie voor de JSON-syntaxis en de eigenschappen van een sjabloon wilt gebruiken, [Microsoft.ServiceFabric/clusters sjabloonverwijzing](/azure/templates/microsoft.servicefabric/clusters).

<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[customize-your-cluster-template]: service-fabric-cluster-creation-create-template.md
