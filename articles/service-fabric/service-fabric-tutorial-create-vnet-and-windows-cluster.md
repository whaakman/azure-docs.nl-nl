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
ms.date: 02/19/2019
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 64ba17053179d428f5ef7e5ce9685240bde6665f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822988"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Zelfstudie: Een Windows Service Fabric-cluster implementeren in een virtueel Azure-netwerk

Deze zelfstudie is deel één van een serie. U leert hoe u een Service Fabric-cluster met Windows in een [virtueel Azure-netwerk (VNET)](../virtual-network/virtual-networks-overview.md) en een [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) implementeert met behulp van PowerShell en een sjabloon. Wanneer u klaar bent, wordt er in de cloud een cluster uitgevoerd waarin u toepassingen kunt implementeren.  Zie [Een beveiligd Linux-cluster maken in Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md) als u een Linux-cluster wilt maken met behulp van Azure CLI.

In deze zelfstudie wordt een productiescenario beschreven.  Zie [Een testcluster maken](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) als u voor testdoeleinden snel een kleiner cluster wilt maken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Azure Active Directory-verificatie instellen
> * Een beveiligd Service Fabric-cluster maken in Azure PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd cluster maken in Azure
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer de [Service Fabric SDK en PowerShell-module](service-fabric-get-started.md)
* Installeer de [Azure Powershell-module, versie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)
* Bekijk de belangrijkste concepten van [Azure-clusters](service-fabric-azure-clusters-overview.md)

Met de volgende procedures wordt er een Service Fabric-cluster met zeven knooppunten gemaakt. Gebruik de [Azure-prijscalculator](https://azure.microsoft.com/pricing/calculator/) om de kosten te berekenen voor het uitvoeren van een Service Fabric-cluster in Azure.

## <a name="download-and-explore-the-template"></a>De sjabloon downloaden en verkennen

Download de volgende Resource Manager-sjabloonbestanden:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Met deze sjabloon wordt een veilig cluster van zeven virtuele machines en drie knooppunttypen in een virtueel netwerk en een netwerkbeveiligingsgroep geïmplementeerd.  Andere voorbeeldsjablonen zijn te vinden op [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates).  Met [azuredeploy.json][template] wordt een aantal resources geïmplementeerd, waaronder de volgende.

### <a name="service-fabric-cluster"></a>Service Fabric-cluster

In de resource **Microsoft.ServiceFabric/clusters** wordt een Windows-cluster geconfigureerd met de volgende kenmerken:

* drie knooppunttypen
* vijf knooppunten van het primaire knooppunttype (te configureren in de sjabloonparameters), een knooppunt van elk van de andere twee knooppunttypen
* Besturingssysteem: Windows Server 2016 Datacenter met Containers (te configureren in de sjabloonparameters)
* beveiligd met een certificaat (configureerbaar in de sjabloonparameters)
* [omgekeerde proxy](service-fabric-reverseproxy.md) is ingeschakeld
* [DNS-service](service-fabric-dnsservice.md) is ingeschakeld
* een bronzen [duurzaamheidsniveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (configureerbaar in de sjabloonparameters)
* een zilveren [betrouwbaarheidsniveau](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (configureerbaar in de sjabloonparameters)
* het eindpunt van de clientverbinding: 19000 (configureerbaar in de sjabloonparameters)
* het eindpunt van de HTTP-gateway: 19080 (configureerbaar in de sjabloonparameters)

### <a name="azure-load-balancer"></a>Azure Load Balancer

In de resource **Microsoft.Network/loadBalancers** wordt een load balancer geconfigureerd en worden tests en regels ingesteld voor de volgende poorten:

* het eindpunt van de clientverbinding: 19000
* het eindpunt van de HTTP-gateway: 19080
* toepassingspoort: 80
* toepassingspoort: 443
* omgekeerde proxy van Service Fabric: 19081

Als er andere toepassingspoorten nodig zijn, moet u de resource **Microsoft.Network/loadBalancers** en de resource **Microsoft.Network/networkSecurityGroups** zo wijzigen dat verkeer kan binnenkomen.

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtueel netwerk, subnet en netwerkbeveiligingsgroep

De namen van het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep zijn gedefinieerd in de sjabloonparameters.  Adresruimten van het virtuele netwerk en subnet worden ook gedeclareerd in de sjabloonparameters en geconfigureerd in de resource **Microsoft.Network/virtualNetworks**:

* adresruimte van virtueel netwerk: 172.16.0.0/20
* Service Fabric-subnetadresruimte: 172.16.2.0/23

De volgende regels voor binnenkomend verkeer worden ingeschakeld in de resource **Microsoft.Network/networkSecurityGroups**. U kunt de poortwaarden wijzigen door de sjabloonvariabelen te wijzigen.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Bereik kortstondige poorten: 49152 tot 65534 (minimaal 256 poorten vereist)
* Poorten voor toepassingsgebruik: 80 en 443
* Bereik toepassingspoorten: 49152 tot 65534 (deze worden gebruikt voor communicatie tussen services en niet geopend in de load balancer)
* Blokkeer alle andere poorten

Als er andere toepassingspoorten nodig zijn, moet u de resource **Microsoft.Network/loadBalancers** en de resource **Microsoft.Network/networkSecurityGroups** zo wijzigen dat verkeer kan binnenkomen.

### <a name="windows-defender"></a>Windows Defender
[Windows Defender Antivirus](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) is standaard geïnstalleerd en functioneert op Windows Server 2016. De gebruikersinterface wordt standaard geïnstalleerd op een aantal SKU's, maar is niet vereist.  Voor elk knooppunttype dat/elke VM-schaalset die is aangegeven in de sjabloon, wordt de [Antimalware voor Azure-VM-extensie](/azure/virtual-machines/extensions/iaas-antimalware-windows) gebruikt voor het uitsluiten van de Service Fabric-mappen en processen:

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>De sjabloonparameters instellen

Het parameterbestand [azuredeploy.parameters.json][parameters] bepaalt veel waarden die worden gebruikt om het cluster en de bijbehorende resources te implementeren. Enkele van de parameters die u mogelijk moet wijzigen voor uw implementatie:

|Parameter|Voorbeeldwaarde|Opmerkingen|
|---|---||
|adminUserName|vmadmin| De gebruikersnaam van de beheerder van de cluster-VM's.[Gebruikersnaamvereisten voor VM](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Het wachtwoord van de beheerder van de cluster-VM's. [Vereisten voor wachtwoorden voor VM](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| De naam van het cluster. Mag alleen letters en cijfers bevatten. De lengte kan minimaal 3 en maximaal 23 tekens zijn.|
|location|southcentralus| De locatie van het cluster. |
|certificateThumbprint|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de SHA1-waarde van de certificaatvingerafdruk in. Bijvoorbeeld "6190390162C988701DB5676EB81083EA608DCCF3"</p>. |
|certificateUrlValue|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft. </p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de URL van het certificaat in. Bijvoorbeeld https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346.</p>|
|sourceVaultValue||<p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de bronkluis in. Bijvoorbeeld /subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT.</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Clientverificatie via Azure Active Directory instellen
Voor Service Fabric-clusters die zijn geïmplementeerd in een openbaar netwerk dat wordt gehost op Azure, is de aanbeveling voor wederzijdse verificatie van client-naar-knooppunt:
* Azure Active Directory gebruiken voor de identiteit van de client
* Een certificaat voor de identiteit van de server en SSL-codering van HTTP-communicatie

Het instellen van Azure Active Directory om clients te verifiëren voor een Service Fabric-cluster moet worden uitgevoerd voordat u [het cluster maakt](#createvaultandcert).  Azure Active Directory maakt het beheren van toegang tot toepassingen door organisaties (bekend als tenants) mogelijk. 

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsoles [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) en [Visual Studio](service-fabric-manage-application-in-visual-studio.md). Als gevolg hiervan, maakt u twee Azure Active Directory-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.  Nadat de toepassingen zijn gemaakt, wijst u gebruikers toe aan de rollen alleen-lezen en beheerder.

> [!NOTE]
> U moet de volgende stappen uitvoeren voordat u het cluster maakt. Omdat de scripts clusternamen en eindpunten verwachten, moeten de waarden worden gepland en geen waarden zijn die u al hebt gemaakt.

In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u dit nog niet hebt gedaan, begint u door [Een Azure Active Directory-tenant verkrijgen](../active-directory/develop/quickstart-create-new-tenant.md) te lezen.

Ter vereenvoudiging van enkele van de stappen voor het configureren van Azure Active Directory met een Service Fabric-cluster, hebben we een set Windows PowerShell-scripts gemaakt. [Download de scripts](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) op uw computer.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure Active Directory-toepassingen maken en gebruikers toewijzen aan rollen
Maak twee Azure Active Directory-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing. Nadat u de toepassingen voor uw cluster hebt gemaakt, wijst u uw gebruikers toe aan de [rollen die worden ondersteund door Service Fabric](service-fabric-cluster-security-roles.md): alleen-lezen en beheerder.

Voer `SetupApplications.ps1` uit, en geef de tenant-ID, de naam van het cluster en de antwoord-URL van de webtoepassing op als parameters.  Geef ook gebruikersnamen en wachtwoorden voor de gebruikers op.  Bijvoorbeeld:

```PowerShell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Voor nationale clouds (bijvoorbeeld Azure Government, Azure China, Azure Duitsland), moet u ook de parameter `-Location` opgeven.

U vindt uw *TenantId* ofwel de map-id in de [Azure-portal](https://portal.azure.com). Selecteer **Azure Active Directory -> Eigenschappen** en kopieer de waarde **Map-id**.

De *Clusternaam* wordt gebruikt voor als prefix aan de Azure Active Directory-toepassingen die zijn gemaakt door het script. Deze hoeft niet precies overeen te komen met de naam van het daadwerkelijke cluster. De naam is uitsluitend bedoeld om het eenvoudiger te maken dat Azure Active Directory-artefacten worden toegewezen aan het Service Fabric-cluster waarmee ze worden gebruikt.

*WebApplicationReplyUrl* wordt het standaardeindpunt dat Azure Active Directory retourneert naar uw gebruikers nadat ze klaar zijn aanmelden. Stel dit eindpunt in als het Service Fabric Explorer-eindpunt voor uw cluster, dit is standaard:

https://&lt;cluster_domain&gt;:19080/Explorer

U wordt gevraagd of u zich aanmeldt bij een account dat beheerdersrechten voor de Azure Active Directory-tenant heeft. Nadat u zich hebt aangemeld, maakt het script de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de toepassingen van de tenant in de [Azure-portal](https://portal.azure.com), ziet u twee nieuwe vermeldingen:

   * *ClusterName*\_Cluster
   * *ClusterName*\_Client

Het script drukt de JSON af die is vereist voor de Azure Resource Manager-sjabloon bij het maken van het cluster, dus is het een goed idee om het PowerShell-venster geopend te houden.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure Active Directory-configuratie toevoegen voor het gebruik van Azure Active Directory voor clienttoegang
Configureer in [azuredeploy.json][template] Azure Active Directory in de sectie **Microsoft.ServiceFabric/clusters**.  Voeg parameters toe voor de tenant-id, de toepassings-id van het cluster en de clienttoepassings-id.  

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

Voeg de parameterwaarden toe in het parameterbestand [azuredeploy.parameters.json][parameters].  Bijvoorbeeld:

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Het virtuele netwerk en het cluster implementeren

Stel vervolgens de netwerktopologie in en implementeer het Service Fabric-cluster. De Resource Manager-sjabloon [azuredeploy.json][template] maakt een virtueel netwerk (VNET), evenals een subnet en een netwerkbeveiligingsgroep (NSG) voor Service Fabric. De sjabloon implementeert ook een cluster met certificaatbeveiliging ingeschakeld.  Gebruik voor productieclusters een certificaat van een certificeringsinstantie (CA) als clustercertificaat. Een zelfondertekend certificaat kan worden gebruikt om testclusters te beveiligen.

De sjabloon in dit artikel implementeert een cluster dat gebruik maakt van de certificaatvingerafdruk voor het identificeren van het clustercertificaat.  Geen twee certificaten kunnen dezelfde vingerafdruk hebben, waardoor certificaatbeheer moeilijker wordt. Schakelen tussen een geïmplementeerd cluster vanuit vingerafdrukken voor certificaten naar het gebruik van gewone namen voor certificaten maakt het beheer van certificaten veel eenvoudiger.  Lees [Cluster wijzigen naar certificaatbeheer met gewone namen](service-fabric-cluster-change-cert-thumbprint-to-cn.md) voor informatie over het bijwerken van het cluster voor het gebruik van gewone namen voor certificaten voor het beheren van certificaten.

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
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
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
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

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

De **Service Fabric** PowerShell-module biedt veel cmdlets voor het beheren van Service Fabric-clusters, toepassingen en services.  Gebruik de cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) om verbinding te maken met het beveiligde cluster. De SHA1-vingerafdruk van het certificaat en de eindpuntdetails van de verbinding vindt u in de uitvoer van de vorige stap.

Als u eerder een AAD-clientverificatie heeft ingesteld, voert u het volgende uit: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

Als u geen AAD-clientverificatie heeft ingesteld, voert u het volgende uit:
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

In de andere artikelen van deze zelfstudiereeks wordt gebruikgemaakt van het cluster dat u zojuist hebt gemaakt. Als u niet meteen verdergaat met het volgende artikel, is het wellicht een goed idee om [het cluster te verwijderen](service-fabric-cluster-delete.md). U bespaart dan kosten.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een VNET maken in Azure met behulp van PowerShell
> * Een sleutelkluis maken en een certificaat uploaden
> * Azure Active Directory-verificatie instellen
> * Een beveiligde Service Fabric-cluster maken in Azure met behulp van PowerShell
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van PowerShell
> * Een cluster verwijderen

Ga nu verder met de volgende zelfstudie om te leren hoe u uw cluster kunt schalen.
> [!div class="nextstepaction"]
> [Een cluster schalen](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
