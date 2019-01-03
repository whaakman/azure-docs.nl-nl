---
title: Containerexemplaren in een Azure-netwerk implementeren
description: Leer hoe u groepen met containers implementeren met een nieuwe of bestaande Azure-netwerk.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 11/28/2018
ms.author: danlep
ms.openlocfilehash: 172ddd11cb956ab6d74e1ce870e2378205dd1613
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993283"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Containerexemplaren in een Azure-netwerk implementeren

[Azure-netwerk](../virtual-network/virtual-networks-overview.md) biedt beveiligde, particuliere netwerken, inclusief filters, Routering en voor uw Azure-peering en on-premises resources. Uw containers kunnen door groepen met containers implementeren in een Azure-netwerk, veilig communiceren met andere resources in het virtuele netwerk.

Containergroepen geïmplementeerd in een Azure-netwerk inschakelen scenario's zoals:

* Rechtstreekse communicatie tussen containergroepen in hetzelfde subnet
* Verzenden [taakgebaseerde](container-instances-restart-policy.md) uitvoer van de werkbelasting van containerinstanties met een database in het virtuele netwerk
* Ophalen van inhoud voor container instances vanuit een [service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) in het virtuele netwerk
* Container-communicatie met virtuele machines in het virtuele netwerk
* Container-communicatie met on-premises bronnen via een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of [ExpressRoute](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Deze functie is momenteel in preview en sommige [gelden beperkingen](#preview-limitations). Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="virtual-network-deployment-limitations"></a>Virtueel netwerk Implementatiebeperkingen

Er gelden bepaalde beperkingen wanneer u groepen met containers in een virtueel netwerk implementeert.

* Windows-containers worden niet ondersteund
* Voor het implementeren van containergroepen aan een subnet, mag niet het subnet een andere resourcetypen bevatten. Verwijder alle bestaande resources uit een bestaand subnet vóór de implementatie van containergroepen toe of maak een nieuw subnet.
* Containergroepen geïmplementeerd met een virtueel netwerk ondersteunen momenteel geen openbare IP-adressen of DNS-naam van labels.
* Vanwege de extra netwerkresources die betrokken zijn is een containergroep implementeren met een virtueel netwerk meestal iets langzamer dan het implementeren van een standard containerexemplaar.

## <a name="preview-limitations"></a>Beperkingen voor Preview

Hoewel deze functie nog in preview, gelden de volgende beperkingen bij het implementeren van containerinstanties met een virtueel netwerk.

**Ondersteund** regio's:

* Noord-Europa (northeurope)
* West-Europa (Europa West)
* VS-West (VS West)
* VS-Oost (VS-Oost)

**Niet-ondersteunde** netwerkbronnen:

* Netwerkbeveiligingsgroep
* Azure Load Balancer

**Verwijderen van de resource-netwerk** vereist [extra stappen](#delete-network-resources) nadat u groepen met containers hebt geïmplementeerd in het virtuele netwerk.

## <a name="required-network-resources"></a>Vereiste netwerkbronnen

Er zijn drie Azure Virtual Network-resources die vereist voor het implementeren van containergroepen met een virtueel netwerk: de [virtueel netwerk](#virtual-network) zelf, een [overgedragen subnet](#subnet-delegated) binnen het virtuele netwerk en een [netwerk profiel](#network-profile). 

### <a name="virtual-network"></a>Virtueel netwerk

Een virtueel netwerk definieert de adresruimte waarin u een of meer subnetten maakt. Vervolgens implementeert u Azure-resources (zoals containergroepen) in de subnetten in uw virtuele netwerk.

### <a name="subnet-delegated"></a>Subnet (gedelegeerd)

Subnetten voor het segmenteren van het virtuele netwerk in afzonderlijke adresruimten die worden gebruikt door de Azure-resources die u erin plaatst. U maken een of meerdere subnetten binnen een virtueel netwerk.

Het subnet dat u voor containergroepen gebruikt mag alleen containergroepen bevatten. Wanneer u eerst een containergroep naar een subnet implementeert, delegeert Azure dat subnet in Azure Container Instances. Zodra gedelegeerd, kan het subnet alleen worden gebruikt voor groepen met containers. Als u resources dan containergroepen te implementeren naar een gedelegeerde subnet, mislukt de bewerking.

### <a name="network-profile"></a>Netwerkprofiel

Een netwerkprofiel is een sjabloon voor het configureren van netwerk voor Azure-resources. Het geeft bepaalde netwerkeigenschappen voor de resource, bijvoorbeeld, het subnet waarin deze moet worden geïmplementeerd. Wanneer u het eerst gebruiken de [az container maken] [ az-container-create] opdracht een containergroep implementeren op een subnet (en dus in een virtueel netwerk), maakt Azure een netwerkprofiel voor u. U kunt vervolgens dat netwerkprofiel gebruiken voor toekomstige implementaties naar het subnet. 

Voor het gebruik van Resource Manager-sjabloon, YAML-bestand of een programmatische methode een containergroep implementeren op een subnet, moet u de volledige Resource Manager resource-ID van een netwerkprofiel opgeven. Kunt u een profiel eerder hebt gemaakt met behulp van [az container maken][az-container-create], of een profiel maken met Resource Manager-sjabloon (Zie [voorbeeldsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet) en [verwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkprofiles)). Als u de ID van een eerder gemaakt profiel, gebruikt de [az netwerk profieloverzicht] [ az-network-profile-list] opdracht. 

In het volgende diagram zijn groepen met meerdere containers geïmplementeerd op een subnet gedelegeerd naar Azure Container Instances. Nadat u hebt één containergroep van de geïmplementeerd naar een subnet, kunt u extra containergroepen te implementeren door de dezelfde netwerkprofiel op te geven.

![Containergroepen binnen een virtueel netwerk][aci-vnet-01]

## <a name="deployment-scenarios"></a>Implementatiescenario's

U kunt [az container maken] [ az-container-create] groepen met containers implementeren naar een nieuw virtueel netwerk en dat Azure aan de vereiste netwerkresources voor u maken of implementeren op een bestaand virtueel netwerk. 

### <a name="new-virtual-network"></a>Nieuw virtueel netwerk

Als u wilt implementeren naar een nieuw virtueel netwerk en Azure automatisch de netwerkbronnen voor u te maken, geeft u de volgende wanneer u [az container maken][az-container-create]:

* Naam van virtueel netwerk
* Adresvoorvoegsel van virtueel netwerk in CIDR-indeling
* Subnetnaam
* Adresvoorvoegsel subnet in CIDR-indeling

Het virtuele netwerk en subnet-adresvoorvoegsels geven de adresruimten voor het virtuele netwerk en subnet, respectievelijk. Deze waarden worden weergegeven in de notatie (Classless Inter-Domain Routing), bijvoorbeeld `10.0.0.0/16`. Zie voor meer informatie over het werken met subnetten [toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet](../virtual-network/virtual-network-manage-subnet.md).

Nadat u hebt uw eerste containergroep met deze methode geïmplementeerd, kunt u door het virtuele netwerk en subnetnamen of het netwerkprofiel die Azure automatisch voor u maakt op te geven aan hetzelfde subnet implementeren. Omdat Azure het subnet in Azure Container Instances delegeert, kunt u implementeren *alleen* containergroepen met het subnet.

### <a name="existing-virtual-network"></a>Bestaand virtueel netwerk

Een containergroep om een bestaand virtueel netwerk te implementeren:

1. Maak een subnet binnen uw bestaande virtuele netwerk, of een bestaand subnet van leeg *alle* andere bronnen
1. Implementeren van een containergroep met [az container maken] [ az-container-create] en geeft u een van de volgende:
   * Virtuele-netwerknaam en de naam van subnet
   * VM-resource-ID en subnet resource-ID, waardoor met behulp van een virtueel netwerk van een andere resourcegroep
   * Profielnaam van netwerk- of -ID, die u kunt verkrijgen met behulp van [lijst met az-profiel][az-network-profile-list]

Nadat u uw eerste containergroep aan een bestaand subnet implementeert, delegeert Azure dat subnet in Azure Container Instances. U kunt niet meer bronnen dan containergroepen implementeren op dat subnet.

## <a name="deployment-examples"></a>Voorbeelden van implementatie

De volgende secties wordt beschreven hoe u groepen met containers implementeren in een virtueel netwerk met de Azure CLI. De opdrachtvoorbeelden worden opgemaakt voor de **Bash** shell. Als u liever een andere shell zoals PowerShell of Command Prompt, overeenkomstig de voortzetting van regeltekens aanpassen.

### <a name="deploy-to-a-new-virtual-network"></a>Implementeren naar een nieuw virtueel netwerk

Eerst een containergroep implementeren en geef de parameters op voor een nieuw virtueel netwerk en subnet. Wanneer u deze parameters opgeeft, Azure maakt het virtuele netwerk en subnet, het subnet in Azure Container instances voor netwerkapparaten delegeert, en maakt ook een netwerkprofiel. Zodra deze resources zijn gemaakt, wordt uw containergroep wordt geïmplementeerd op het subnet.

Voer de volgende [az container maken] [ az-container-create] opdracht die Hiermee worden instellingen voor een nieuw virtueel netwerk en subnet. U moet de naam van een resourcegroep die is gemaakt in een regio opgeven die [ondersteunt](#preview-limitations) containergroepen in een virtueel netwerk. Deze opdracht wordt geïmplementeerd de [microsoft/aci-helloworld] [ aci-helloworld] container die wordt uitgevoerd een kleine Node.js-webserver met een statische website-pagina. In de volgende sectie gaat u een tweede containergroep aan hetzelfde subnet implementeren en testen van communicatie tussen de twee containerinstanties.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image microsoft/aci-helloworld \
    --vnet aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Wanneer u naar een nieuw virtueel netwerk implementeert met behulp van deze methode, kan de implementatie enkele minuten duren terwijl de netwerkresources worden gemaakt. Na de initiële implementatie voltooid extra container groep implementaties sneller.

### <a name="deploy-to-existing-virtual-network"></a>Implementeren naar een bestaand virtueel netwerk

Nu dat u hebt een containergroep geïmplementeerd naar een nieuw virtueel netwerk, een tweede containergroep aan hetzelfde subnet implementeren en controleren of de communicatie tussen de twee containerinstanties.

Haal eerst het IP-adres van de eerste containergroep die u hebt geïmplementeerd, de *appcontainer*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

De uitvoer moet het IP-adres van de containergroep worden weergegeven in het privé-subnet:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Stel nu `CONTAINER_GROUP_IP` aan de IP-adres dat u hebt opgehaald met de `az container show` opdracht in en voer de volgende `az container create` opdracht. Deze tweede container *commchecker*, een installatiekopie op basis van Alpine Linux wordt uitgevoerd en wordt uitgevoerd `wget` op basis van de eerste containergroep privé-subnet IP-adres.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet aci-vnet \
    --subnet aci-subnet
```

Nadat deze tweede containerimplementatie is voltooid, de logboeken ophalen zodat u de uitvoer van ziet de `wget` opdracht wordt uitgevoerd:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

Als de tweede container met de eerste is gecommuniceerd, zijn uitvoer vergelijkbaar met:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

Die moet worden weergegeven door de logboekuitvoer `wget` kan verbinding maken en de indexbestand downloaden van de eerste container met behulp van de privé IP-adres op het lokale subnet is. Netwerkverkeer tussen de twee containergroepen bleef binnen het virtuele netwerk.

### <a name="deploy-to-existing-virtual-network---yaml"></a>Implementeren naar een bestaand virtueel netwerk - YAML

U kunt ook een containergroep implementeren op een bestaand virtueel netwerk met behulp van een YAML-bestand. Als u wilt implementeren op een subnet in een virtueel netwerk, kunt u verschillende extra eigenschappen opgeven in de YAML:

* `ipAddress`: De IP-adresinstellingen voor de containergroep.
  * `ports`: De poorten te openen, indien van toepassing.
  * `protocol`: Het protocol (TCP of UDP) voor de poort geopend.
* `networkProfile`: Hiermee geeft u netwerkinstellingen, zoals het virtuele netwerk en subnet voor een Azure-resource.
  * `id`: De volledige Resource Manager resource-ID van de `networkProfile`.

Voor het implementeren van een containergroep met een virtueel netwerk met een YAML-bestand, moet u eerst de ID van het netwerkprofiel niet ophalen. Uitvoeren van de [az netwerk profielenlijst] [ az-network-profile-list] opdracht, de naam van de resourcegroep waarin uw virtuele netwerk en gedelegeerde subnet op te geven.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

De uitvoer van de opdracht geeft de volledige resource-ID voor het netwerkprofiel:

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Beschikt u over het netwerk profiel-ID, de volgende YAML kopiëren naar een nieuw bestand met de naam *vnet-implementeren-aci.yaml*. Onder `networkProfile`, vervangen de `id` waarde met de ID die u zojuist hebt opgehaald, sla het bestand. Deze YAML wordt gemaakt van de containergroep van een met de naam *appcontaineryaml* in uw virtuele netwerk.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: microsoft/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/container/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Implementeren van de containergroep met de [az container maken] [ az-container-create] opdracht op te geven de naam van de YAML-bestand voor de `--file` parameter:

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Nadat de implementatie is voltooid, voert de [az container show] [ az-container-show] opdracht om de status ervan weer te geven:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                     IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   microsoft/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Resources opschonen

### <a name="delete-container-instances"></a>Containerexemplaren verwijderen

Wanneer u klaar bent met de containerinstanties werkt u hebt gemaakt, verwijdert deze met de volgende opdrachten:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Verwijderen van netwerkbronnen

De eerste preview van deze functie is vereist voor verschillende aanvullende opdrachten om de netwerkresources te verwijderen dat u eerder hebt gemaakt. Als u de voorbeeldopdrachten in de vorige secties van dit artikel te maken van uw virtuele netwerk en subnet gebruikt, kunt u het volgende script gebruiken om de netwerkresources te verwijderen.

Voordat het script wordt uitgevoerd, stelt u de `RES_GROUP` variabele de naam van de resourcegroep met het virtuele netwerk en subnet die moet worden verwijderd. Het script is geformatteerd voor de Bash-shell. Als u liever een andere shell zoals PowerShell of Command Prompt, moet u variabele toewijzing en accessors overeenkomstig aanpassen.

> [!WARNING]
> Met dit script worden resources verwijderd. Het virtuele netwerk en alle subnetten die erin worden verwijderd. Zorg dat u niet meer nodig *eventuele* van de resources in het virtuele netwerk, met inbegrip van eventuele subnetten die deze bevat, voordat u dit script uitvoert. Eenmaal is verwijderd, **deze resources zijn niet kan worden hersteld**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --remove delegations 0

# Delete the subnet
az network vnet subnet delete --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Volgende stappen

Zie voor het implementeren van een nieuw virtueel netwerk, subnet, netwerkprofiel en containergroep met een Resource Manager-sjabloon, [maakt een Azure-containergroep met een VNet](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
).

Meerdere virtuele-netwerkbronnen en functies zijn besproken in dit artikel al kort. De Azure Virtual Network-documentatie wordt uitgebreid beschreven hoe deze onderwerpen:

* [Virtueel netwerk](../virtual-network/manage-virtual-network.md)
* [Subnet](../virtual-network/virtual-network-manage-subnet.md)
* [Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list
