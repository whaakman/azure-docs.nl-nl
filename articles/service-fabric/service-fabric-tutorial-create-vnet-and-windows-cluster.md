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
ms.date: 11/02/2017
ms.author: ryanwi
ms.openlocfilehash: edf9f646207ec31730b557e90a6d19a4b69985bc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Een Service Fabric Windows-cluster in een Azure-netwerk implementeren
Deze zelfstudie maakt deel uit een reeks. U leert het implementeren van een Service Fabric-cluster waarop Windows wordt uitgevoerd in een bestaande Azure-netwerk (VNET) en subplan net met behulp van PowerShell. Wanneer u klaar bent, hebt u een cluster uitvoert in de cloud die u kunt toepassingen implementeren op.  Zie het maken van een Linux-cluster met behulp van Azure CLI [beveiligde Linux-cluster maken op Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

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
> * [Een in- of -cluster schalen](/service-fabric-tutorial-scale-cluster.md)
> * [Upgrade de runtime van een cluster](service-fabric-tutorial-upgrade-cluster.md)
> * [API Management met Service Fabric implementeren](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Service Fabric SDK en PowerShell-module](service-fabric-get-started.md)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

De volgende procedures maken een vijf knooppunten Service Fabric-cluster. Voor het berekenen van de kosten die zijn gemaakt door het uitvoeren van een Service Fabric-cluster in Azure gebruik de [Azure Prijscalculator](https://azure.microsoft.com/pricing/calculator/).

## <a name="introduction"></a>Inleiding
Deze zelfstudie implementeert u een cluster met vijf knooppunten in een type met één knooppunt in een virtueel netwerk in Azure.

Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen worden geschaald naar duizenden computers. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een knooppunt genoemd. Elk knooppunt wordt de knooppuntnaam van een (een tekenreeks) toegewezen. Knooppunten hebben kenmerken zoals plaatsingseigenschappen.

Het type van een knooppunt bepaalt de grootte, het aantal en de eigenschappen voor een set van virtuele machines in het cluster. Elke gedefinieerde knooppunttype is ingesteld als een [virtuele-machineschaalset](/azure/virtual-machine-scale-sets/), een Azure compute resource die u kunt implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype kan vervolgens worden uitgebreid of omlaag onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken kan hebben. Knooppunttypen worden gebruikt voor het definiëren van rollen voor een set van clusterknooppunten, zoals 'front-end' of 'back-end'.  Het cluster kan meer dan één knooppunttype hebben moet, maar het primaire knooppunttype ten minste vijf VM's voor productie-clusters (of ten minste drie virtuele machines voor testclusters).  [Service Fabric-systeemservices](service-fabric-technical-overview.md#system-services) worden geplaatst op de knooppunten van het type van het primaire knooppunt.

## <a name="cluster-capacity-planning"></a>Planning clustercapaciteit
Deze zelfstudie implementeert u een cluster met vijf knooppunten in een type met één knooppunt.  Voor elk cluster productie-implementatie is capaciteitsplanning een belangrijke stap. Hier volgen enkele overwegingen als onderdeel van dit proces.

- Het aantal knooppunten van het type nodig voor het cluster 
- De eigenschappen van elk knooppunttype (bijvoorbeeld grootte, primaire, verbonden met internet en het aantal VM's)
- De betrouwbaarheid en duurzaamheid kenmerken van het cluster

Zie voor meer informatie [overwegingen bij capaciteitsplanning Cluster](service-fabric-cluster-capacity.md).

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
Stel de netwerktopologie die API Management en de Service Fabric-cluster wordt geïmplementeerd. De [network.json] [ network-arm] Resource Manager-sjabloon is geconfigureerd voor het maken van een virtueel netwerk (VNET) en ook een beveiligingsgroep subnet en het netwerk (NSG) voor Service Fabric en een subnet en het NSG voor API Management . API Management wordt verderop in deze zelfstudie geïmplementeerd. Meer informatie over VNETs, subnetten en nsg's [hier](../virtual-network/virtual-networks-overview.md).

De [network.parameters.json] [ network-parameters-arm] parameterbestand bevat de namen van de subnetten en nsg's die Service Fabric en API Management op implementeert.  API Management wordt geïmplementeerd in de [volgende zelfstudie](service-fabric-tutorial-deploy-api-management.md). Voor deze handleiding hoeft de parameterwaarden die niet worden gewijzigd. De Service Fabric-Resource Manager-sjablonen gebruiken deze waarden.  Als de waarden hier zijn gewijzigd, moet u ze wijzigen in de andere Resource Manager-sjablonen in deze zelfstudie gebruikt en de [implementeren API Management-zelfstudie](service-fabric-tutorial-deploy-api-management.md). 

De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
- [Network.JSON][network-arm]
- [Network.parameters.JSON][network-parameters-arm]

Gebruik de volgende PowerShell-opdracht de Resource Manager-sjabloon en de parameterbestanden bestanden voor de netwerkinstallatie van de te implementeren:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile C:\winclustertutorial\network.json -TemplateParameterFile C:\winclustertutorial\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>De Service Fabric-cluster implementeren
Zodra de netwerkbronnen hebt implementeren, wordt de volgende stap is een Service Fabric-cluster implementeren voor het VNET in het subnet en NSG aangewezen voor de Service Fabric-cluster. Een cluster implementeren op een bestaande VNET en subnet (geïmplementeerd eerder in dit artikel), moet een Resource Manager-sjabloon.  De sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u in de vorige stap hebt ingesteld voor deze zelfstudie reeks.  

De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
- [cluster.JSON][cluster-arm]
- [cluster.parameters.JSON][cluster-parameters-arm]

Deze sjabloon gebruiken om een beveiligde cluster te maken.  Een certificaat van de cluster is een X.509-certificaat gebruikt voor het beveiligen van communicatie van knooppunt naar en de eindpunten voor het beheer van cluster naar een management-client te verifiëren.  Het certificaat van het cluster biedt ook een met SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS. Azure Sleutelkluis wordt gebruikt voor het beheren van certificaten voor Service Fabric-clusters in Azure.  Wanneer een cluster is geïmplementeerd in Azure, wordt de Azure-resourceprovider verantwoordelijk voor het maken van Service Fabric-clusters certificaten ophaalt uit Sleutelkluis en installeert ze op het cluster virtuele machines. 

U kunt een certificaat van een certificeringsinstantie (CA) gebruiken als het certificaat van het cluster of voor testdoeleinden, een zelfondertekend certificaat maken. De cluster-certificaat moet:

- een persoonlijke sleutel bevatten.
- voor sleuteluitwisseling, kan worden geëxporteerd naar een bestand Personal Information Exchange (.pfx) worden gemaakt.
- een onderwerpnaam hebben die overeenkomt met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster. Deze overeenkomst is vereist voor SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de. cloudapp.azure.com domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Vul in deze leeg parameters in de *cluster.parameters.json* -bestand voor uw implementatie:

|Parameter|Waarde|
|---|---|
|adminPassword|Wachtwoord #1234|
|adminUserName|vmadmin|
|Clusternaam|mysfcluster|
|location|southcentralus|

Laat de *certificateThumbprint*, *certificateUrlValue*, en *sourceVaultValue* parameters leeg te maken van een zelfondertekend certificaat.  Als u een bestaand certificaat hebt geüpload om een sleutelkluis te gebruiken wilt, vult u deze parameterwaarden.

Het volgende script maakt gebruik van de [nieuw AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet en sjabloon voor het implementeren van een nieuw cluster in Azure. De cmdlet ook een nieuwe sleutelkluis maakt in Azure, wordt een nieuw zelfondertekend certificaat toegevoegd aan de sleutelkluis en downloadt het certificaatbestand lokaal. U kunt een bestaand certificaat en/of de sleutelkluis opgeven met behulp van de andere parameters van de [nieuw AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet.

```powershell
# Variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster"
$vaultname = "clusterkeyvault111"
$vaultgroupname="clusterkeyvaultgroup111"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile 'C:\winclustertutorial\cluster.json' `
-ParameterFile 'C:\winclustertutorial\cluster.parameters.json' -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname
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

Ga vervolgens naar de volgende zelfstudie voor informatie over het schalen van uw cluster.
> [!div class="nextstepaction"]
> [Schalen van een Cluster](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
