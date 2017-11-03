---
title: Maken van een Windows-Service Fabric-cluster in Azure | Microsoft Docs
description: Informatie over het implementeren van een Windows-Service Fabric-cluster in een bestaand Azure virtueel netwerk met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: b3bab57f5ca6627b4532284376a9809d5ab543f2
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Een Service Fabric Windows-cluster in een Azure-netwerk implementeren
Deze zelfstudie maakt deel uit een reeks. U leert het implementeren van een Windows-Service Fabric-cluster naar een bestaande Azure-netwerk (VNET) en subplan net met behulp van PowerShell. Wanneer u klaar bent, hebt u een cluster uitvoert in de cloud die u kunt toepassingen implementeren op.  Zie het maken van een Linux-cluster met behulp van Azure CLI [beveiligde Linux-cluster maken op Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Een beveiligde Service Fabric-cluster maken in Azure PowerShell
> * Beveiligen van het cluster met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Verwijderen van een cluster

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * Een beveiligde cluster in Azure maken
> * [API Management met Service Fabric implementeren](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Service Fabric SDK en PowerShell-module](service-fabric-get-started.md)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

De volgende procedures maken een vijf knooppunten Service Fabric-cluster. Voor het berekenen van de kosten die zijn gemaakt door het uitvoeren van een Service Fabric-cluster in Azure gebruik de [Azure Prijscalculator](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Aanmelden bij Azure en uw abonnement te selecteren
Deze handleiding gebruikt Azure PowerShell. Wanneer u een nieuwe PowerShell-sessie start, zich aanmelden bij uw Azure-account en uw abonnement te selecteren voordat u Azure-opdrachten uitvoeren.
 
Voer het volgende script aan te melden bij uw Azure-account selecteren voor uw abonnement:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een nieuwe resourcegroep voor uw implementatie en wijs hieraan een naam en een locatie.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc
```

## <a name="deploy-the-network-topology"></a>De netwerktopologie implementeren
Stel de netwerktopologie die API Management en de Service Fabric-cluster wordt geïmplementeerd. De [network.json] [ network-arm] Resource Manager-sjabloon is geconfigureerd voor het maken van een virtueel netwerk (VNET) en ook een beveiligingsgroep subnet en het netwerk (NSG) voor Service Fabric en een subnet en het NSG voor API Management . Meer informatie over VNETs, subnetten en nsg's [hier](../virtual-network/virtual-networks-overview.md).

De [network.parameters.json] [ network-parameters-arm] parameterbestand bevat de namen van de subnetten en nsg's die Service Fabric en API Management op implementeert.  API Management wordt geïmplementeerd in de [volgende zelfstudie](service-fabric-tutorial-deploy-api-management.md). Voor deze handleiding hoeft de parameterwaarden die niet worden gewijzigd. De Service Fabric-Resource Manager-sjablonen gebruiken deze waarden.  Als de waarden hier zijn gewijzigd, moet u ze wijzigen in de andere Resource Manager-sjablonen in deze zelfstudie gebruikt en de [implementeren API Management-zelfstudie](service-fabric-tutorial-deploy-api-management.md). 

De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
- [Network.JSON][network-arm]
- [Network.parameters.JSON][network-parameters-arm]

Gebruik de volgende PowerShell-opdracht de Resource Manager-sjabloon en de parameterbestanden bestanden voor de netwerkinstallatie van de te implementeren:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>De Service Fabric-cluster implementeren
Zodra de netwerkbronnen hebt implementeren, wordt de volgende stap is een Service Fabric-cluster implementeren voor het VNET in het subnet en NSG aangewezen voor de Service Fabric-cluster. Een cluster implementeren op een bestaande VNET en subnet (geïmplementeerd eerder in dit artikel), moet een Resource Manager-sjabloon.  Zie voor meer informatie [maken van een cluster met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). De sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u in de vorige stap hebt ingesteld voor deze zelfstudie reeks.  De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
- [cluster.JSON][cluster-arm]
- [cluster.parameters.JSON][cluster-parameters-arm]

Een certificaat wordt gebruikt voor het beveiligen van communicatie van knooppunt naar voor uw cluster en voor het beheren van de gebruikerstoegang tot uw Service Fabric-cluster. API Management gebruikt dit certificaat ook toegang tot de Service Fabric Naming Service voor de servicedetectie van de. 

Het volgende script maakt gebruik van de [nieuw AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet voor het implementeren van een nieuw cluster in Azure. De cmdlet ook een sleutelkluis maakt in Azure, maakt een zelfondertekend certificaat en de sleutelkluis en downloadt het certificaatbestand lokaal.   

```powershell
# Certificate variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"

# Variables for VM admin.
$adminuser="vmadmin"
$adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

# Variables for common values
$clustername = "mysfcluster"
$vmsku = "Standard_D2_v2"
$vaultname = "clusterkeyvault"
$vaultgroupname="clusterkeyvaultgroup"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. Possible values: 1, 3-99
$clustersize=5 

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname `
-TemplateFile .\cluster.json -ParameterFile .\cluster.parameters.json
```

## <a name="connect-to-the-secure-cluster"></a>Verbinding maken met de beveiligde cluster
Verbinding maken met het cluster met behulp van de Service Fabric PowerShell-module geïnstalleerd met de Service Fabric SDK.  Installeer eerst het certificaat in het archief Persoonlijk (Mijn) van de huidige gebruiker op uw computer.  Voer de volgende PowerShell-opdracht:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

U bent nu klaar om verbinding te maken met het beveiligde cluster.

De **Service Fabric** PowerShell-module biedt veel cmdlets voor het beheren van Service Fabric-clusters, toepassingen en services.  Gebruik de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet verbinding maken met de beveiligde cluster. De vingerafdruk en verbinding eindpunt certificaatdetails zijn gevonden in de uitvoer van de vorige stap.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Controleer of u verbonden bent en het cluster in orde is met behulp van de [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Resources opschonen
De andere artikelen in deze zelfstudie reeks gebruik van het cluster dat u zojuist hebt gemaakt. Als u niet onmiddellijk verplaatsen naar het volgende artikel, is het raadzaam om het cluster en sleutelkluis te vermijden kosten te verwijderen. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen.

Verwijderen van de resourcegroep en alle clusterbronnen met behulp van de [cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  De resourcegroep met de sleutelkluis ook verwijderd.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Maken van een beveiligde Service Fabric-cluster in Azure met behulp van PowerShell
> * Beveiligen van het cluster met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Verwijderen van een cluster

Ga vervolgens naar de volgende zelfstudie voor meer informatie over het implementeren van API Management met Service Fabric.
> [!div class="nextstepaction"]
> [Implementeren van API Management](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
