---
title: Een Windows Service Fabric-cluster maken in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een Windows Service Fabric-cluster implementeert in een virtueel Azure-netwerk en -netwerkbeveiligingsgroep met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/22/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f795333e8af2f09800dedc0b65030c42165d6bbb
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068900"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Zelfstudie: Een Windows Service Fabric-cluster implementeren in een virtueel Azure-netwerk

Deze zelfstudie is deel één van een serie. U leert hoe u een Service Fabric-cluster met Windows in een [virtueel Azure-netwerk (VNET)](../virtual-network/virtual-networks-overview.md) en een [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) implementeert met behulp van PowerShell en een sjabloon. Wanneer u klaar bent, wordt er in de cloud een cluster uitgevoerd waarin u toepassingen kunt implementeren.  Zie [Een beveiligd Linux-cluster maken in Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md) als u een Linux-cluster wilt maken met behulp van Azure CLI.

In deze zelfstudie wordt een productiescenario beschreven.  Zie [een testcluster met drie knooppunten maken](./scripts/service-fabric-powershell-create-test-cluster.md) als u snel een klein cluster wilt maken voor testdoeleinden.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Een beveiligd Service Fabric-cluster maken in Azure PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd cluster maken in Azure
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [API Management implementeren met Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer de [Service Fabric SDK en PowerShell-module](service-fabric-get-started.md)
* Installeer de [Azure Powershell-module, versie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Met de volgende procedures wordt er een Service Fabric-cluster met vijf knooppunten gemaakt. Gebruik de [Azure-prijscalculator](https://azure.microsoft.com/pricing/calculator/) om de kosten te berekenen voor het uitvoeren van een Service Fabric-cluster in Azure.

## <a name="key-concepts"></a>Belangrijkste concepten

Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen naar duizenden machines worden geschaald. Een machine of VM die onderdeel uitmaakt van een cluster, heet een knooppunt. Aan elk knooppunt wordt een knooppuntnaam toegewezen (een tekenreeks). Knooppunten hebben kenmerken zoals plaatsingseigenschappen.

Het knooppunttype bepaalt de grootte, het aantal en de eigenschappen van een set virtuele machines in het cluster. Elk gedefinieerd knooppunttype is ingesteld als een [virtuele-machineschaalset](/azure/virtual-machine-scale-sets/), een Azure-rekenresource die u gebruikt om een verzameling virtuele machines als set te implementeren en te beheren. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. Knooppunttypen worden gebruikt voor het definiëren van rollen voor een set clusterknooppunten, zoals 'front-end' of 'back-end'.  Uw cluster kan meer dan één knooppunttype hebben, maar voor productieclusters moet het primaire knooppunttype ten minste vijf VM's bevatten (of ten minste drie VM's voor testclusters).  [Service Fabric-systeemservices](service-fabric-technical-overview.md#system-services) worden op de knooppunten van het primaire knooppunttype geplaatst.

Het cluster wordt beveiligd met een clustercertificaat. Een clustercertificaat is een X.509-certificaat dat wordt gebruikt om de communicatie tussen knooppunten te beveiligen en om eindpunten voor clusterbeheer aan een beheerclient toe te wijzen.  Het clustercertificaat biedt ook een SSL voor de API voor HTTPS-beheer en voor Service Fabric Explorer via HTTPS. Zelfondertekende certificaten zijn handig voor testclusters.  Gebruik voor productieclusters een certificaat van een certificeringsinstantie (CA) als clustercertificaat.

Het clustercertificaat moet:

* een persoonlijke sleutel bevatten,
* gemaakt worden voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange-bestand (.pfx),
* een onderwerpnaam hebben die overeenkomt met het domein dat u gebruikt voor toegang tot het Service Fabric-cluster. Deze overeenkomst is vereist om de eindpunten van het HTTPS-beheer van het cluster en Service Fabric Explorer te voorzien van SSL. U kunt geen SSL-certificaat van een certificeringsinstantie (CA) krijgen voor het domein .cloudapp.azure.com. U hebt voor uw cluster een aangepaste domeinnaam nodig. Wanneer u een certificaat van een CA aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Azure Key Vault wordt gebruikt om certificaten voor Service Fabric-clusters in Azure te beheren.  Wanneer een cluster in Azure is geïmplementeerd, is de Azure-resourceprovider verantwoordelijk voor het maken van Service Fabric-clusters die certificaten ophalen uit Key Vault en installeren op de cluster-VM's.

In deze zelfstudie wordt een cluster met vijf knooppunten van één knooppunttype geïmplementeerd. Voor een implementatie van een productiecluster is [capaciteitsplanning](service-fabric-cluster-capacity.md) echter van groot belang. Hier volgen enkele aandachtspunten voor dat proces.

* Het aantal knooppunten en knooppunttypen dat uw cluster nodig heeft
* De eigenschappen van elk knooppunttype (bijvoorbeeld grootte, primair knooppunt, internetverbinding en het aantal VM's)
* De betrouwbaarheid en duurzaamheid van de clusterkenmerken

## <a name="download-and-explore-the-template"></a>De sjabloon downloaden en verkennen

Download de volgende Resource Manager-sjabloonbestanden:

* [vnet-cluster.json][template]
* [vnet-cluster.parameters.json][parameters]

De [vnet-cluster.json][template] implementeert een aantal bronnen, waaronder de volgende.

### <a name="service-fabric-cluster"></a>Service Fabric-cluster

Er wordt een Windows-cluster geïmplementeerd met de volgende kenmerken:

* één knooppunttype
* vijf knooppunten van het primaire knooppunttype (te configureren in de sjabloonparameters)
* Besturingssysteem: Windows Server 2016 Datacenter met Containers (te configureren in de sjabloonparameters)
* beveiligd met een certificaat (configureerbaar in de sjabloonparameters)
* [omgekeerde proxy](service-fabric-reverseproxy.md) is ingeschakeld
* [DNS-service](service-fabric-dnsservice.md) is ingeschakeld
* een bronzen [duurzaamheidsniveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (configureerbaar in de sjabloonparameters)
 * een zilveren [betrouwbaarheidsniveau](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (configureerbaar in de sjabloonparameters)
* eindpunt van de clientverbinding: 19000 (configureerbaar in de sjabloonparameters)
* eindpunt van de HTTP-gateway: 19080 (configureerbaar in de sjabloonparameters)

### <a name="azure-load-balancer"></a>Azure Load Balancer

Er wordt een load balancer geïmplementeerd en er worden tests en regels ingesteld voor de volgende poorten:

* het eindpunt van de clientverbinding: 19000
* het eindpunt van de HTTP-gateway: 19080
* toepassingspoort: 80
* toepassingspoort: 443
* omgekeerde proxy van Service Fabric: 19081

Als er andere toepassingspoorten nodig zijn, moet u de bron Microsoft.Network/loadBalancers en de bron Microsoft.Network/networkSecurityGroups zo wijzigen dat verkeer kan binnenkomen.

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtueel netwerk, subnet en netwerkbeveiligingsgroep

De namen van het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep zijn gedefinieerd in de sjabloonparameters.  De adresruimten van het virtuele netwerk en het subnet worden ook gedefinieerd in de sjabloonparameters:

* adresruimte van virtueel netwerk: 172.16.0.0/20
* Service Fabric-subnetadresruimte: 172.16.2.0/23

De volgende regels voor binnenkomend verkeer worden ingeschakeld in de netwerkbeveiligingsgroep. U kunt de poortwaarden wijzigen door de sjabloonvariabelen te wijzigen.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Bereik kortstondige poorten: 49152 tot 65534 (minimaal 256 poorten vereist)
* Poorten voor toepassingsgebruik: 80 en 443
* Bereik toepassingspoorten: 49152 tot 65534 (deze worden gebruikt voor communicatie tussen services en niet geopend in de load balancer)
* Blokkeer alle andere poorten

Als er andere toepassingspoorten nodig zijn, moet u de bron Microsoft.Network/loadBalancers en de bron Microsoft.Network/networkSecurityGroups zo wijzigen dat verkeer kan binnenkomen.

## <a name="set-template-parameters"></a>De sjabloonparameters instellen

Het parameterbestand [vnet-cluster.parameters.json][parameters] bepaalt veel waarden die worden gebruikt om het cluster en de bijbehorende resources te implementeren. Enkele van de parameters die u mogelijk moet wijzigen voor uw implementatie:

|Parameter|Voorbeeldwaarde|Opmerkingen|
|---|---||
|adminUserName|vmadmin| De gebruikersnaam van de beheerder van de cluster-VM's.[Gebruikersnaamvereisten voor VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Het wachtwoord van de beheerder van de cluster-VM's. [Vereisten voor wachtwoorden voor VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| De naam van het cluster. Mag alleen letters en cijfers bevatten. De lengte kan minimaal 3 en maximaal 23 tekens zijn.|
|location|southcentralus| De locatie van het cluster. |
|certificateThumbprint|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de certificaatvingerafdruk in. Bijvoorbeeld "6190390162C988701DB5676EB81083EA608DCCF3"</p>. |
|certificateUrlValue|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft. </p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de URL van het certificaat in. Bijvoorbeeld https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346.</p>|
|sourceVaultValue||<p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de bronkluis in. Bijvoorbeeld /subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT.</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Het virtuele netwerk en het cluster implementeren

Stel vervolgens de netwerktopologie in en implementeer het Service Fabric-cluster. De Resource Manager-sjabloon [vnet-cluster.json][template] maakt een virtueel netwerk (VNET), evenals een subnet en een netwerkbeveiligingsgroep (NSG) voor Service Fabric. De sjabloon implementeert ook een cluster met certificaatbeveiliging ingeschakeld.  Gebruik voor productieclusters een certificaat van een certificeringsinstantie (CA) als clustercertificaat. Een zelfondertekend certificaat kan worden gebruikt om testclusters te beveiligen.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Een cluster maken met behulp van een bestaand certificaat

Het volgende script maakt gebruik van de cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) en een sjabloon om een nieuw cluster in Azure te implementeren. De cmdlet maakt ook een nieuwe sleutelkluis in Azure en uploadt uw certificaat.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\vnet-cluster.json" `
-ParameterFile "$templatepath\vnet-cluster.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Een cluster met een nieuw, zelfondertekend certificaat maken

Het volgende script maakt gebruik van de cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) en een sjabloon om een nieuw cluster in Azure te implementeren. De cmdlet maakt ook een nieuwe sleutelkluis in Azure, voegt een nieuw zelfondertekend certificaat toe aan de sleutelkluis en downloadt het certificaatbestand lokaal.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\vnet-cluster.json" `
-ParameterFile "$templatepath\vnet-cluster.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Verbinding maken met het beveiligde cluster

Maak verbinding met het cluster met behulp van de Service Fabric PowerShell-module die bij de Service Fabric-SDK wordt geïnstalleerd.  Installeer eerst het certificaat in de persoonlijke opslag (Mijn opslag) van de huidige gebruiker op uw computer.  Voer de volgende PowerShell-opdracht uit:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

U bent nu klaar om verbinding te maken met het beveiligde cluster.

De **Service Fabric** PowerShell-module biedt veel cmdlets voor het beheren van Service Fabric-clusters, toepassingen en services.  Gebruik de cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) om verbinding te maken met het beveiligde cluster. De vingerafdruk van het certificaat en de eindpuntdetails van de verbinding vindt u in de uitvoer van de vorige stap.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Controleer of u bent verbonden en of het cluster goed werkt door de cmdlet [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) uit te voeren.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Resources opschonen

De andere artikelen in deze zelfstudiereeks maken gebruik van het cluster dat u hebt gemaakt. Als u niet onmiddellijk verdergaat met het volgende artikel, kunt u overwegen om het cluster en de sleutelkluis te verwijderen om kosten te besparen. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen.

Verwijder de resourcegroep en alle clusterbronnen met behulp van de cmdlet [Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  Verwijder ook de resourcegroep die de sleutelkluis bevat.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Een beveiligde Service Fabric-cluster maken in Azure met behulp van PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

Ga nu verder met de volgende zelfstudie om te leren hoe u uw cluster kunt schalen.
> [!div class="nextstepaction"]
> [Een cluster schalen](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-cluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-cluster.parameters.json
