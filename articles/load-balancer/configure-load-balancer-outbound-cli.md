---
title: Load balancing en uitgaande regels configureren in de standaardversie van Load Balancer met behulp van Azure CLI | Microsoft Docs
description: In dit artikel laat zien hoe het configureren van load balancing en uitgaande regels in een Standard Load Balancer met de Azure CLI.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jpconnock
tags: azure-resource-manager
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: kumud
ms.openlocfilehash: dd549005776656bb755b78fc0ca73d24edcc77bd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973742"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Configureren van taakverdeling en regels voor uitgaand verkeer in de standaardversie van Load Balancer met behulp van Azure CLI

In deze quickstart ziet u configureren hoe regels voor uitgaand verkeer in Standard Load Balancer met behulp van Azure CLI.  

Wanneer u klaar bent, de Load Balancer-resource bevat twee front-ends en regels die zijn gekoppeld aan deze: één voor binnenkomende en uitgaande.  Elke front-end bevat een verwijzing naar een openbaar IP-adres en deze scenario maakt gebruik van een ander openbaar IP-adres voor inkomend en uitgaand.   De load balancer-regel biedt alleen binnenkomende load balancing en de uitgaande regel bepaalt de uitgaande NAT opgegeven voor de virtuele machine.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep maken met [az group create](https://docs.microsoft.com/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Het volgende voorbeeld wordt een resourcegroep met de naam *myresourcegroupoutbound* in de *eastus2* locatie:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Virtueel netwerk maken
Maak een virtueel netwerk met de naam *myvnetoutbound* met een subnet met de naam *mysubnetoutbound* in de *myresourcegroupoutbound* met behulp van [az network vnet Maak](https://docs.microsoft.com/cli/azure/network/vnet#create).

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Inkomende openbare IP-adres maken 

Om toegang te krijgen tot uw web-app op internet, hebt u een openbaar IP-adres nodig voor de load balancer. Een Standard Load Balancer biedt alleen ondersteuning voor standaard, openbare IP-adressen. Gebruik [az network public-ip maken](https://docs.microsoft.com/cli/azure/network/public-ip#create) te maken van een Standard openbaar IP-adres met de naam *mypublicipinbound* in *myresourcegroupoutbound*.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Uitgaande openbare IP-adres maken 

Maak een standaard IP-adres voor front-end-uitgaande van de Load Balancer-configuratie [az network public-ip maken](https://docs.microsoft.com/cli/azure/network/public-ip#create) met de naam *mypublicipoutbound* in *myresourcegroupoutbound*.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```


## <a name="create-azure-load-balancer"></a>Azure Load Balancer maken

In deze sectie wordt beschreven hoe u de volgende onderdelen van de load balancer kunt maken en configureren:
  - Een front-end IP-adres dat het binnenkomende netwerkverkeer op de load balancer ontvangt.
  - Een back endpool waar de frontend-IP de belasting verzendt met gelijke taakverdeling van netwerkverkeer.
  - een statustest die bepaalt de status van de back-end-VM-exemplaren.
  - Een load balancer inkomende regel waarmee wordt gedefinieerd hoe verkeer wordt gedistribueerd naar de virtuele machines.
  - Een load balancer uitgaande regel waarmee wordt gedefinieerd hoe verkeer wordt gedistribueerd naar de VM's.

### <a name="create-load-balancer"></a>Load Balancer maken

Een Load Balancer maken met de inkomende IP-adres met [az network lb maken](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#create) met de naam *lb* die een binnenkomende frontend-IP-configuratie met de naam bevat *myfrontendinbound*, een back endadresgroep met de naam *bepool* dat is gekoppeld aan het openbare IP-adres *mypublicipinbound* die u in de vorige stap hebt gemaakt.

```azurecli-interactive
  az network lb create \
    --resource-group myresourcegroupoutbound \
    --name lb \
    --sku standard \
    --backend-pool-name bepool \
    --frontend-ip-name myfrontendinbound \
    --location eastus2 \
    --public-ip-address mypublicipinbound   
  ```

### <a name="create-outbound-frontend-ip"></a>Uitgaande frontend-IP-adres maken
Maken van de uitgaande frontend-IP-configuratie voor de Load Balancer met [az network lb frontend-ip maken](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#create) met en uitgaande front-end-IP-configuratie met de naam *myfrontendoutbound* dat wil zeggen dat is gekoppeld aan het openbare IP-adres *mypublicipoutbound*

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Statustest maken

Een statuscontrole controleert alle exemplaren van de virtuele machines om ervoor te zorgen dat deze netwerkverkeer kunnen verzenden. Het exemplaar van een virtuele machine met mislukte testcontroles wordt uit de load balancer verwijderd totdat deze weer online komt en een testcontrole bepaalt of deze in orde is. Maak met [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#create) een statustest om de status van de virtuele machines te bewaken. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name http \
    --protocol http \
    --port 80 \
    --path /  
```

### <a name="create-load-balancing-rule"></a>Regel voor taakverdeling maken

Een load balancer-regel definieert de front-end-IP-configuratie voor het binnenkomende verkeer en de back endadresgroep voor het ontvangen van het verkeer, samen met de gewenste bron- en doelserver. Maak een load balancer-regel *myinboundlbrule* met [az network lb-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#create) voor het luisteren naar poort 80 in de front-endpool *myfrontendinbound* en verzenden netwerkverkeer met Load Balancing naar de back-endadresgroep *bepool* ook van poort 80. 

>[!NOTE]
>Deze load-balancingregel wordt automatische uitgaande (S) NAT uitgeschakeld als gevolg van deze regel met de--uitschakelen uitgaande snat-parameter. Uitgaande NAT wordt alleen geleverd door de regel voor uitgaande.

```azurecli-interactive
az network lb rule create \
--resource-group myresourcegroupoutbound \
--lb-name lb \
--name inboundlbrule \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--probe http \
--frontend-ip-name myfrontendinbound \
--backend-pool-name bepool \
--disable-outbound-snat
```

### <a name="create-outbound-rule"></a>Uitgaande regel maken

Een uitgaande regel definieert het front-end openbare IP-adres, vertegenwoordigd door de front-end *myfrontendoutbound*, die wordt gebruikt voor alle uitgaande NAT-verkeer, evenals de back-endpool waarop deze regel van toepassing is.  Maakt een uitgaande regel *myoutboundrule* voor uitgaand netwerkverkeer vertaling van alle virtuele machines (NIC IP-configuraties) in *bepool* back-endpool.  De onderstaande opdracht ook de uitgaande time-out voor inactiviteit wordt gewijzigd van 4 tot 15 minuten en 10000 SNAT poorten in plaats van 1024 toegewezen.  Beoordeling [regels voor uitgaand verkeer](https://aka.ms/lboutboundrules) voor meer informatie.

```azurecli-interactive
az network lb outbound-rule create \
 --resource-group myresourcegroupoutbound \
 --lb-name lb \
 --name outboundrule \
 --frontend-ip-configs myfrontendoutbound \
 --protocol All \
 --idle-timeout 15 \
 --outbound-ports 10000 \
 --address-pool bepool
```
## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep, de load balancer en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Volgende stappen
In dit artikel standaardversie van Load Balancer gemaakt, zowel binnenkomend verkeersregels voor taakverdeling, geconfigureerd en statustest geconfigureerd voor de virtuele machines in de back-endpool. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)

