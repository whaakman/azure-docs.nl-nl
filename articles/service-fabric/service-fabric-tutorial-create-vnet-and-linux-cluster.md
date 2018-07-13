---
title: Een Linux Service Fabric-cluster maken in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een Linux Service Fabric-cluster implementeert in een bestaand virtueel Azure-netwerk met behulp van Azure CLI.
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
ms.openlocfilehash: 161687ec2275558adb235dc63b5244a0a8ff7e47
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110790"
---
# <a name="tutorial-deploy-a-linux-service-fabric-cluster-into-an-azure-virtual-network"></a>Zelfstudie: Een Linux Service Fabric-cluster implementeren in een virtueel Azure-netwerk

Deze zelfstudie is deel één van een serie. U leert hoe u een Linux Service Fabric-cluster in een [virtueel Azure-netwerk (VNET)](../virtual-network/virtual-networks-overview.md) en een [netwerkbeveiligingsgroep (NSG)](../virtual-network/virtual-networks-nsg.md) implementeert met behulp van Azure CLI en een sjabloon. Wanneer u klaar bent, wordt er in de cloud een cluster uitgevoerd waarin u toepassingen kunt implementeren. Als u met behulp van PowerShell een Windows-cluster wilt maken, raadpleegt u [Een Service Fabric Windows-cluster in een Azure-netwerk implementeren](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VNET maken in Azure met Azure CLI
> * Een beveiligd Service Fabric-cluster maken in Azure met behulp van Azure CLI
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van Service Fabric CLI
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
* Installeer de [Service Fabric CLI](service-fabric-cli.md)
* Installeer de [Azure CLI 2.0](/cli/azure/install-azure-cli)

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

In deze zelfstudie wordt een cluster met vijf knooppunten geïmplementeerd op één knooppunttype. Voor een implementatie van een productiecluster is [capaciteitsplanning](service-fabric-cluster-capacity.md) echter van groot belang. Hier volgen enkele aandachtspunten voor dat proces.

* Het aantal knooppunten en knooppunttypen dat uw cluster nodig heeft
* De eigenschappen van elk knooppunttype (bijvoorbeeld grootte, primair knooppunt, internetverbinding en het aantal VM's)
* De betrouwbaarheid en duurzaamheid van de clusterkenmerken

## <a name="download-and-explore-the-template"></a>De sjabloon downloaden en verkennen

Download de volgende Resource Manager-sjabloonbestanden:

* [vnet-linuxcluster.json][template]
* [vnet-linuxcluster.parameters.json][parameters]

De sjabloon [vnet-linuxcluster.json][template] implementeert een aantal resources, waaronder de volgende.

### <a name="service-fabric-cluster"></a>Service Fabric-cluster

Een Linux-cluster wordt geïmplementeerd met de volgende kenmerken:

* één knooppunttype
* vijf knooppunten in het primaire knooppunttype (configureerbaar in de sjabloonparameters)
* een besturingssysteem: Ubuntu 16.04 LTS (configureerbaar in de sjabloonparameters)
* een beveiligd certificaat (configureerbaar in de sjabloonparameters)
* een ingeschakelde [DNS-service](service-fabric-dnsservice.md)
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

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtueel netwerk, subnet en netwerkbeveiligingsgroep

De namen van het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep zijn gedefinieerd in de sjabloonparameters.  Ook de adresruimten van het virtuele netwerk en het subnet zijn gedefinieerd in de sjabloonparameters:

* virtuele netwerkadresruimte: 10.0.0.0/16
* Service Fabric-subnetadresruimte: 10.0.2.0/24

De volgende regels voor binnenkomend verkeer zijn ingeschakeld in de netwerkbeveiligingsgroep. U kunt de poortwaarden wijzigen door de sjabloonvariabelen te wijzigen.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Bereik kortstondige poorten: 49152 tot 65534 (minimaal 256 poorten vereist)
* Poorten voor toepassingsgebruik: 80 en 443
* Bereik toepassingspoorten: 49152 tot 65534 (deze worden gebruikt voor communicatie tussen services en niet geopend in de load balancer)
* Blokkeer alle andere poorten

Als er andere toepassingspoorten nodig zijn, moet u de resource Microsoft.Network/loadBalancers en de resource Microsoft.Network/networkSecurityGroups zo wijzigen dat inkomend verkeer is toegestaan.

## <a name="set-template-parameters"></a>De sjabloonparameters instellen

Het parameterbestand [vnet-cluster.parameters.json][parameters] bepaalt veel waarden die worden gebruikt om het cluster en de bijbehorende resources te implementeren. Enkele van de parameters die u mogelijk moet wijzigen voor uw implementatie:

|Parameter|Voorbeeldwaarde|Opmerkingen|
|---|---||
|adminUserName|vmadmin| De gebruikersnaam van de beheerder van de cluster-VM's. |
|adminPassword|Password#1234| Het wachtwoord van de beheerder van de cluster-VM's.|
|clusterName|mysfcluster123| De naam van het cluster. |
|location|southcentralus| De locatie van het cluster. |
|certificateThumbprint|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de certificaatvingerafdruk in. Bijvoorbeeld 6190390162C988701DB5676EB81083EA608DCCF3. </p>|
|certificateUrlValue|| <p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de URL van het certificaat in. Bijvoorbeeld https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346.</p>|
|sourceVaultValue||<p>De waarde moet leeg zijn als u een zelfondertekend certificaat maakt of als u een certificaatbestand opgeeft.</p><p>Als u een bestaand certificaat wilt gebruiken dat u eerder hebt geüpload naar een sleutelkluis, vult u de waarde van de bronkluis in. Bijvoorbeeld /subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT.</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Het virtuele netwerk en het cluster implementeren

Stel vervolgens de netwerktopologie in en implementeer het Service Fabric-cluster. De Resource Manager-sjabloon [vnet-linuxcluster.json][template] maakt een virtueel netwerk (VNET), evenals een subnet en een netwerkbeveiligingsgroep (NSG) voor Service Fabric. De sjabloon implementeert ook een cluster met certificaatbeveiliging ingeschakeld.  Gebruik voor productieclusters een certificaat van een certificeringsinstantie (CA) als clustercertificaat. Een zelfondertekend certificaat kan worden gebruikt om testclusters te beveiligen.

Het volgende script maakt gebruik van de opdracht en de sjabloon [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) om een nieuw cluster te implementeren dat met een bestaand certificaat is beveiligd. De opdracht maakt ook een nieuwe sleutelkluis in Azure en uploadt uw certificaat.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file vnet-linuxcluster.json --parameter-file vnet-linuxcluster.parameters.json
```

## <a name="connect-to-the-secure-cluster"></a>Verbinding maken met het beveiligde cluster

Maak verbinding met het cluster met behulp van de Service Fabric CLI-opdracht `sfctl cluster select` en uw sleutel.  Opmerking: gebruik voor een zelfondertekend certificaat alleen de optie **--no-verify**.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Controleer of u bent verbonden en of het cluster goed werkt door de opdracht `sfctl cluster health` uit te voeren.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Resources opschonen

In de andere artikelen van deze zelfstudiereeks wordt gebruikgemaakt van het cluster dat u zojuist hebt gemaakt. Als u niet meteen verdergaat met het volgende artikel, is het wellicht een goed idee om het cluster te verwijderen. U bespaart dan kosten. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen.

Meld u aan bij Azure en selecteer de abonnements-id waarmee u het cluster wilt verwijderen.  U kunt uw abonnements-id vinden door u aan te melden bij [Azure Portal](http://portal.azure.com). Verwijder de resourcegroep en alle clusterresources met behulp van de opdracht [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete).

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een VNET maken in Azure met Azure CLI
> * Een beveiligd Service Fabric-cluster maken in Azure met behulp van Azure CLI
> * Het cluster beveiligen met een X.509-certificaat
> * Verbinding maken met het cluster met behulp van Service Fabric CLI
> * Een cluster verwijderen

Ga nu verder met de volgende zelfstudie om te leren hoe u uw cluster kunt schalen.
> [!div class="nextstepaction"]
> [Een cluster schalen](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.parameters.json