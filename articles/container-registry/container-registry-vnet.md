---
title: Een Azure container registry in een virtueel netwerk implementeren
description: Toegang tot een Azure container registry toestaan alleen van resources in een Azure-netwerk of van openbare IP-adresbereiken.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/14/2019
ms.author: danlep
ms.openlocfilehash: 0a4d9f355a5cdc92bab4491c08677042c42986cb
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517926"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Toegang beperken tot een Azure container registry met behulp van een Azure-netwerk of firewall-regels

[Azure-netwerk](../virtual-network/virtual-networks-overview.md) biedt beveiligde, particuliere netwerken voor uw Azure- en on-premises resources. Uw persoonlijke Azure container registry in een Azure-netwerk implementeert, kunt u ervoor zorgen dat alleen de resources in het virtuele netwerk toegang het register tot. Voor cross-premises scenario's, kunt u ook firewallregels om toe te staan van specifieke IP-adressen alleen toegang tot het register configureren.

In dit artikel ziet u twee scenario's voor het netwerk toegangsregels om te beperken van toegang tot een Azure container registry maken: van een virtuele machine die wordt geïmplementeerd in hetzelfde netwerk of van het openbare IP-adres van een virtuele machine.

> [!IMPORTANT]
> Deze functie is momenteel in preview en sommige [gelden beperkingen](#preview-limitations). Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.
>

## <a name="preview-limitations"></a>Preview-beperkingen

* Alleen een **Premium** containerregister kan worden geconfigureerd met regels voor network access. Zie voor meer informatie over Servicelagen register [Azure Container Registry-SKU's](container-registry-skus.md). 

* Alleen een [Azure Kubernetes Service](../aks/intro-kubernetes.md) cluster of Azure [virtuele machine](../virtual-machines/linux/overview.md) kan worden gebruikt als een host voor toegang tot een containerregister in een virtueel netwerk. *Andere Azure-services zoals Azure Container Instances worden momenteel niet ondersteund.*

* Elk register biedt ondersteuning voor maximaal 100 regels voor virtueel netwerk.

## <a name="prerequisites"></a>Vereisten

* Het gebruik van de Azure CLI de stappen in dit artikel, Azure CLI versie 2.0.58 of hoger is vereist. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

* Als u nog een containerregister hebt, maakt u er een (Premium-SKU vereist) en push de installatiekopie van een voorbeeld zoals `hello-world` uit Docker Hub. Gebruik bijvoorbeeld de [Azure-portal] [ quickstart-portal] of de [Azure CLI] [ quickstart-cli] naar een register maken. 

## <a name="about-network-rules-for-a-container-registry"></a>Over netwerkregels voor een container registry

Een Azure container registry standaard accepteert verbindingen via internet vanaf hosts op een netwerk. Met een virtueel netwerk, kunt u alleen Azure-resources, zoals een AKS-cluster of virtuele Azure-machine veilig toegang krijgen tot het register, zonder een netwerkgrens kruisende toestaan. U kunt ook configureren met network-firewallregels aan lijst met geaccepteerde internet voor specifieke openbare IP-adresbereiken. 

Als u wilt beperken tot een register, eerst de standaardactie wijzigen van het register zodat deze alle netwerkverbindingen weigert. Vervolgens voegt u netwerktoegangsregels. Clients verleende toegang via het netwerkregels moet blijven [verifiëren naar het containerregister](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) en worden geautoriseerd voor toegang tot de gegevens.

### <a name="service-endpoint-for-subnets"></a>Service-eindpunt voor subnetten

Als u wilt toestaan toegang vanuit een subnet in een virtueel netwerk, die u wilt toevoegen een [service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) voor de Azure Container Registry-service. 

Multitenant-services, zoals Azure Container Registry, gebruikt één set met IP-adressen voor alle klanten. Een service-eindpunt wijst een eindpunt voor toegang tot een register toe. Dit eindpunt biedt verkeer een optimale route naar de resource via de Azure-backbone-netwerk. De identiteit van het virtuele netwerk en het subnet worden ook verzonden bij elke aanvraag.

### <a name="firewall-rules"></a>Firewall-regels

Bieden voor IP-netwerkregels voor toegestane internet-adresbereiken met behulp van de CIDR-notatie zoals *16.17.18.0/24* of een afzonderlijke IP-adressen, zoals *16.17.18.19*. IP-netwerkregels zijn alleen toegestaan voor *openbare* internet-IP-adressen. IP-adresbereiken is gereserveerd voor particuliere netwerken (zoals gedefinieerd in RFC 1918) zijn niet toegestaan in de IP-regels.

## <a name="create-a-docker-enabled-virtual-machine"></a>Een Docker-functionaliteit virtuele machine maken

In dit artikel gebruikt u een Docker-functionaliteit Ubuntu-VM voor toegang tot een Azure container registry. Voor het gebruik van Azure Active Directory-verificatie in het register, installeert u ook de [Azure CLI] [ azure-cli] op de virtuele machine. Als u al een virtuele machine van Azure hebt, moet u deze stap het maken van overslaan.

U kunt dezelfde resourcegroep gebruiken voor uw virtuele machine en uw containerregister. Deze instellingen vereenvoudigt opschonen aan het einde, maar is niet vereist. Als u ervoor kiest om een afzonderlijke resourcegroep voor de virtuele machine en het virtuele netwerk te maken, voert u [az-groep maken][az-group-create]. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *westcentralus* locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

Implementeer nu een standaard virtuele-machine met Ubuntu Azure [az vm maken][az-vm-create]. Het volgende voorbeeld wordt een virtuele machine met de naam *myDockerVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Het duurt enkele minuten voordat de virtuele machine wordt gemaakt. Wanneer de opdracht is voltooid, noteer de `publicIpAddress` weergegeven door de Azure CLI. Dit adres gebruiken om te maken van SSH-verbindingen met de virtuele machine en eventueel voor later instellen van de firewall-regels.

### <a name="install-docker-on-the-vm"></a>Docker installeren op de virtuele machine

Nadat de virtuele machine wordt uitgevoerd, maakt u een SSH-verbinding met de virtuele machine. Vervang *publicIpAddress* met het openbare IP-adres van uw virtuele machine.

```bash
ssh azureuser@publicIpAddress
```

Voer de volgende opdracht uit om Docker te installeren op de Ubuntu-VM:

```bash
sudo apt install docker.io -y
```

Na de installatie voert u de volgende opdracht om te controleren of Docker correct wordt uitgevoerd op de virtuele machine:

```bash
sudo docker run -it hello-world
```

Uitvoer:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Azure-CLI installeren

Volg de stappen in [Azure CLI installeren met apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) de Azure CLI installeren op uw virtuele Ubuntu-machine. Zorg ervoor dat u versie 2.0.58 installeren voor dit artikel of hoger.

Sluit de SSH-verbinding.

## <a name="allow-access-from-a-virtual-network"></a>Toegang vanuit een virtueel netwerk toestaan

In deze sectie configureert u het containerregister zodat toegang vanaf een subnet in een Azure-netwerk. Equivalent met behulp van de Azure CLI en Azure portal vindt u stappen.

### <a name="allow-access-from-a-virtual-network---cli"></a>Toegang vanuit een virtueel netwerk - CLI

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Een service-eindpunt toevoegen aan een subnet

Wanneer u een virtuele machine maakt, maakt Azure standaard een virtueel netwerk in dezelfde resourcegroep bevinden. De naam van het virtuele netwerk is gebaseerd op de naam van de virtuele machine. Bijvoorbeeld, als u uw virtuele machine de naam *myDockerVM*, de standaardnaam van het virtuele netwerk is *myDockerVMVNET*, met een subnet met de naam *myDockerVMSubnet*. Dit controleren in Azure portal of met behulp van de [az network vnet list] [ az-network-vnet-list] opdracht:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Uitvoer:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Gebruik de [az network vnet subnet update] [ az-network-vnet-subnet-update] toe te voegen opdracht een **Microsoft.ContainerRegistry** service-eindpunt voor uw subnet. Vervang de namen van uw virtuele netwerk en subnet in de volgende opdracht:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Gebruik de [az network vnet subnet show] [ az-network-vnet-subnet-show] opdracht voor het ophalen van de resource-ID van het subnet. U moet dit in een latere stap voor het configureren van een regel voor toegang.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Uitvoer:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Standaard toegang tot het netwerk naar het register wijzigen

Standaard staat een Azure container registry verbindingen van hosts op een netwerk. Om te beperken van toegang tot het geselecteerde netwerk, wijzigt u de standaardactie voor het weigeren van toegang. Vervang de naam van het register in de volgende [az acr update] [ az-acr-update] opdracht:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Netwerk-regel toevoegen aan het register

Gebruik de [az acr netwerk-regel toevoegen] [ az-acr-network-rule-add] opdracht voor het toevoegen van een regel van het netwerk naar het register waarmee toegang vanaf de VM subnet. Vervang de naam van het containerregister en de resource-ID van het subnet in de volgende opdracht: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

Blijven [toegang tot het register controleren](#verify-access-to-the-registry).

### <a name="allow-access-from-a-virtual-network---portal"></a>Toegang vanuit een virtueel netwerk - portal

#### <a name="add-service-endpoint-to-subnet"></a>Service-eindpunt toevoegen aan subnet

Wanneer u een virtuele machine maakt, maakt Azure standaard een virtueel netwerk in dezelfde resourcegroep bevinden. De naam van het virtuele netwerk is gebaseerd op de naam van de virtuele machine. Bijvoorbeeld, als u uw virtuele machine de naam *myDockerVM*, de standaardnaam van het virtuele netwerk is *myDockerVMVNET*, met een subnet met de naam *myDockerVMSubnet*.

Een service-eindpunt voor Azure Container Registry toevoegen aan een subnet:

1. In het zoekvak boven aan de [Azure-portal][azure-portal], voer *virtuele netwerken*. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waar uw virtuele machine is geïmplementeerd, zoals *myDockerVMVNET*.
1. Onder **instellingen**, selecteer **subnetten**.
1. Selecteer het subnet waarop uw virtuele machine wordt geïmplementeerd, zoals *myDockerVMSubnet*.
1. Onder **Service-eindpunten**, selecteer **Microsoft.ContainerRegistry**.
1. Selecteer **Opslaan**.

![Service-eindpunt toevoegen aan subnet][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Toegang tot het netwerk voor register configureren

Standaard staat een Azure container registry verbindingen van hosts op een netwerk. Om te beperken van toegang tot het virtuele netwerk:

1. Navigeer in de portal naar uw containerregister.
1. Onder **instellingen**, selecteer **Firewall en virtuele netwerken**.
1. Kies voor het weigeren van toegang standaard, zodat toegang vanaf **geselecteerde netwerken**. 
1. Selecteer **bestaand virtueel netwerk toevoegen**, en selecteer het virtuele netwerk en subnet dat u hebt geconfigureerd met een service-eindpunt. Selecteer **Toevoegen**.
1. Selecteer **Opslaan**.

![Virtueel netwerk voor container registry configureren][acr-vnet-portal]

Blijven [toegang tot het register controleren](#verify-access-to-the-registry).

## <a name="allow-access-from-an-ip-address"></a>Zodat toegang vanaf een IP-adres

In deze sectie configureert u het containerregister zodat toegang vanaf een subnet in een Azure-netwerk. Equivalent met behulp van de Azure CLI en Azure portal vindt u stappen.

### <a name="allow-access-from-an-ip-address---cli"></a>Zodat toegang vanaf een IP-adres - CLI

#### <a name="change-default-network-access-to-registry"></a>Standaard toegang tot het netwerk naar het register wijzigen

Als u dit nog niet hebt gedaan, moet u de registerconfiguratie voor het weigeren van toegang standaard bijgewerkt. Vervang de naam van het register in de volgende [az acr update] [ az-acr-update] opdracht:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Netwerk-regel verwijderen uit het register

Als u een regel voor toegang vanuit de VM subnet eerder hebt toegevoegd, verwijdert u het service-eindpunt van het subnet en een regel voor het netwerk. Vervang de naam van het containerregister en de resource-ID van het subnet dat u hebt opgehaald in een eerdere stap in de [az acr netwerk-regel verwijderen] [ az-acr-network-rule-remove] opdracht: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Netwerk-regel toevoegen aan het register

Gebruik de [az acr netwerk-regel toevoegen] [ az-acr-network-rule-add] opdracht voor het toevoegen van een regel van het netwerk naar het register waarmee toegang vanaf het IP-adres van de virtuele machine. Vervang de naam van het containerregister en het openbare IP-adres van de virtuele machine in de volgende opdracht.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

Blijven [toegang tot het register controleren](#verify-access-to-the-registry).

### <a name="allow-access-from-an-ip-address---portal"></a>Toegang vanaf een IP-adres - portal

#### <a name="remove-existing-network-rule-from-registry"></a>Bestaande netwerk regel verwijderen uit het register

Als u een regel voor toegang vanuit de VM subnet eerder hebt toegevoegd, verwijdert u de bestaande regel. Deze sectie overslaan als u wilt toegang tot het register van een andere virtuele machine.

* Werk de subnetinstellingen voor het verwijderen van het service-eindpunt van het subnet voor Azure Container Registry. 

  1. In de [Azure-portal][azure-portal], gaat u naar het virtuele netwerk waar uw virtuele machine is geïmplementeerd.
  1. Onder **instellingen**, selecteer **subnetten**.
  1. Selecteer het subnet waarop uw virtuele machine is geïmplementeerd.
  1. Onder **Service-eindpunten**, verwijdert u het selectievakje voor **Microsoft.ContainerRegistry**. 
  1. Selecteer **Opslaan**.

* Verwijder de netwerk-regel waarmee het subnet toegang tot het register.

  1. Navigeer in de portal naar uw containerregister.
  1. Onder **instellingen**, selecteer **Firewall en virtuele netwerken**.
  1. Onder **virtuele netwerken**, selecteert u de naam van het virtuele netwerk en selecteer vervolgens **verwijderen**.
  1. Selecteer **Opslaan**.

#### <a name="add-network-rule-to-registry"></a>Netwerk-regel toevoegen aan het register

1. Navigeer in de portal naar uw containerregister.
1. Onder **instellingen**, selecteer **Firewall en virtuele netwerken**.
1. Als u dit nog niet hebt gedaan, kiest u zodat toegang vanaf **geselecteerde netwerken**. 
1. Onder **virtuele netwerken**, Controleer of er is geen netwerk is geselecteerd.
1. Onder **Firewall**, geef het openbare IP-adres van een virtuele machine. Of voer een adresbereik in CIDR-notatie waarop de IP-adres van de virtuele machine bevat.
1. Selecteer **Opslaan**.

![Firewallregel voor container registry configureren][acr-vnet-firewall-portal]

Blijven [toegang tot het register controleren](#verify-access-to-the-registry).

## <a name="verify-access-to-the-registry"></a>De toegang tot het register controleren

Controleer na een wachttijd van een paar minuten voor de configuratie bij te werken, of dat de virtuele machine toegang heeft tot de container registry. Een SSH-verbinding maken met uw virtuele machine en voer de [az acr login] [ az-acr-login] opdracht Meld u aan bij uw register. 

```bash
az acr login --name mycontainerregistry
```

U kunt registerbewerkingen uitvoeren zoals uitvoeren `docker pull` voor het ophalen van een voorbeeldafbeelding uit het register. Vervangen door een installatiekopie en tag waarde die geschikt is voor het register, voorafgegaan door het registernaam van de aanmeldingsserver (zonder hoofdletters):

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

De installatiekopie van wordt docker is naar de virtuele machine.

In dit voorbeeld laat zien waartoe u toegang hebt de privécontainerregister via het netwerk toegangsregel. Echter kan niet het register worden geopend vanaf een andere aanmelding-host waarop een regel van het netwerk toegang geconfigureerd niet. Als u probeert aan te melden vanaf een andere host via de `az acr login` opdracht of `docker login` uitvoer is vergelijkbaar met de volgende opdracht:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Toegang tot het register van de standaardinstelling herstellen

Verwijderen voor het herstellen van het register zodat toegang standaard geen netwerkregels die zijn geconfigureerd. Vervolgens stelt u de standaardactie om toegang te verlenen. Equivalent met behulp van de Azure CLI en Azure portal vindt u stappen.

### <a name="restore-default-registry-access---cli"></a>Herstellen van de standaard toegang tot het register - CLI

#### <a name="remove-network-rules"></a>Netwerkregels verwijderen

Een lijst van netwerkregels die zijn geconfigureerd voor uw register wilt bekijken, voert u de volgende [az acr netwerk-rule list] [ az-acr-network-rule-list] opdracht:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

Voor elke regel die is geconfigureerd, voert u de [az acr netwerk-regel verwijderen] [ az-acr-network-rule-remove] opdracht om deze te verwijderen. Bijvoorbeeld:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Toegang toestaan

Vervang de naam van het register in de volgende [az acr update] [ az-acr-update] opdracht:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Terugzetten standaard toegang tot het register - portal


1. In de portal, gaat u naar het containerregister en selecteer **Firewall en virtuele netwerken**.
1. Onder **virtuele netwerken**, selecteert u elk virtueel netwerk en selecteer vervolgens **verwijderen**.
1. Onder **Firewall**, selecteert u elk-adresbereik, en selecteer vervolgens het pictogram verwijderen.
1. Onder **zodat toegang vanaf**, selecteer **alle netwerken**. 
1. Selecteer **Opslaan**.

## <a name="clean-up-resources"></a>Resources opschonen

Als u alle Azure-resources in dezelfde resource groeperen en deze niet meer nodig hebt gemaakt, kunt u eventueel de resources verwijderen met behulp van een enkel [az group delete](/cli/azure/group) opdracht:

```azurecli
az group delete --name myResourceGroup
```

Als u wilt opschonen van uw resources in de portal, gaat u naar de resourcegroep myResourceGroup. Als de resourcegroep is geladen, klikt u op **resourcegroep verwijderen** te verwijderen van de resourcegroep en de resources die zijn opgeslagen.

## <a name="next-steps"></a>Volgende stappen

Meerdere virtuele-netwerkbronnen en functies zijn besproken in dit artikel al kort. De Azure Virtual Network-documentatie wordt uitgebreid beschreven hoe deze onderwerpen:

* [Virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Service-eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
