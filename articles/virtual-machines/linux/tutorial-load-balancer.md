---
title: Het laden van Linux virtuele machines in Azure een balans vinden tussen | Microsoft Docs
description: Informatie over het gebruik van de Azure load balancer maken van een maximaal beschikbare en beveiligde toepassing over drie Linux VM 's
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dc25d6106ad67710660b1a5c48270a7082688d51
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Het laden van Linux virtuele machines in Azure maken van een maximaal beschikbare toepassing saldo
Taakverdeling biedt een hoger niveau van de beschikbaarheid van binnenkomende aanvragen verspreid over meerdere virtuele machines. In deze zelfstudie leert u over de verschillende onderdelen van de Azure load balancer die verkeer distribueren en bieden hoge beschikbaarheid. Procedures voor:

> [!div class="checklist"]
> * Een Azure load balancer maken
> * Een load balancer health test maken
> * Regels voor netwerkverkeer voor de load balancer maken
> * Cloud-init gebruiken voor het maken van een eenvoudige Node.js-app
> * Virtuele machines maken en koppelen aan een load balancer
> * Een load balancer in actie weergeven
> * Toevoegen of virtuele machines van een load balancer verwijderen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Overzicht van Azure load balancer
Een Azure load balancer is een Layer-4 (TCP, UDP) load balancer die zorgt voor hoge beschikbaarheid door het distribueren van inkomend verkeer tussen orde virtuele machines. Een load balancer health test controleert een bepaalde poort op elke virtuele machine en wordt alleen verkeer naar een operationele virtuele machine.

U definieert een front-end-IP-configuratie met een of meer openbare IP-adressen. Deze front-end-IP-configuratie kunt de load balancer en toepassingen via Internet toegankelijk zijn. 

Virtuele machines verbinding maken met een load balancer met behulp van de virtuele netwerkinterfacekaart (NIC). Als u wilt distribueren verkeer naar de virtuele machines, een back-end-adresgroep bevat het IP-adressen van de virtuele (NIC's) die is verbonden met de load balancer.

Voor het beheren van de stroom van verkeer, kunt u regels van de load balancer voor bepaalde poorten en protocollen die zijn toegewezen aan uw virtuele machines definiëren.

Als u de vorige zelfstudie voor het gevolgd [maken van een virtuele-machineschaalset](tutorial-create-vmss.md), een load balancer voor u is gemaakt. Al deze onderdelen zijn geconfigureerd voor u als onderdeel van de schaal is ingesteld.


## <a name="create-azure-load-balancer"></a>Azure load balancer maken
Deze sectie beschrijft hoe u kunt maken en configureren van elk onderdeel van de load balancer. Voordat u de load balancer maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupLoadBalancer* in de *eastus* locatie:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken
Voor toegang tot uw app op het Internet, moet u een openbaar IP-adres voor de load balancer. Maken van een openbaar IP-adres met [az netwerk openbare ip-maken](/cli/azure/network/public-ip#create). Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myPublicIP* in de *myResourceGroupLoadBalancer* resourcegroep:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Een load balancer maken
Maken van een load balancer met [az network Load Balancer maken](/cli/azure/network/lb#create). Het volgende voorbeeld wordt een load balancer met de naam *myLoadBalancer* en wijst de *myPublicIP* adres aan de front-end-IP-configuratie:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Een health test maken
Als u wilt toestaan dat de load balancer om de status van uw app te controleren, moet u een health test gebruiken. De health-test wordt dynamisch toegevoegd of verwijderd van virtuele machines van de load balancer draaiing op basis van hun reactie op statuscontroles. Standaard wordt een virtuele machine verwijderd uit de load balancer-distributie na twee opeenvolgende fouten met intervallen van 15 seconden. U maakt een health test op basis van een protocol of de pagina voor de controle van een specifieke status voor uw app. 

Het volgende voorbeeld wordt een TCP-test. U kunt ook aangepaste HTTP-tests voor meer fijnmazige statuscontroles maken. Wanneer u een aangepaste HTTP-test, moet u de pagina controle van status, zoals *healthcheck.js*. De test moet retourneren een **HTTP 200 OK** antwoord voor de load balancer moet worden gedraaid houden de host.

U gebruikt voor het maken van een TCP health test [az network Load Balancer-test maken](/cli/azure/network/lb/probe#create). Het volgende voorbeeld wordt een health test met de naam *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken
Een regel voor load balancer wordt gebruikt om te definiëren hoe verkeer wordt gedistribueerd naar de virtuele machines. Definieert u de front-end-IP-configuratie voor het binnenkomende verkeer en de back-end-IP-adresgroep voor het ontvangen van het verkeer, samen met de vereiste poort van de bron- en doelserver. Om er zeker van te zijn dat alleen orde virtuele machines verkeer ontvangen, moet u ook de health test gebruiken definiëren.

Maken van een regel voor load balancer met [az network Load Balancer-regel maken](/cli/azure/network/lb/rule#create). Het volgende voorbeeld wordt een regel met naam *myLoadBalancerRule*, gebruikt de *myHealthProbe* health test en saldo's verkeer op poort *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Virtueel netwerk configureren
Voordat u een aantal virtuele machines implementeren en uw balancer kunt testen, moet u de resources van de ondersteunende virtueel netwerk maken. Zie voor meer informatie over virtuele netwerken van de [Azure Virtual Networks beheren](tutorial-virtual-network.md) zelfstudie.

### <a name="create-network-resources"></a>Maken van netwerkbronnen
Maak een virtueel netwerk met [az network vnet maken](/cli/azure/network/vnet#create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* met een subnet met de naam *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

Als u wilt een netwerkbeveiligingsgroep toevoegen, gebruikt u [az netwerk nsg maken](/cli/azure/network/nsg#create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Maken van een groep van de netwerkbeveiligingsregel met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create). Het volgende voorbeeld wordt een groep netwerkbeveiligingsregel met de naam *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuele NIC's zijn gemaakt met [az netwerk nic maken](/cli/azure/network/nic#create). Het volgende voorbeeld maakt drie virtuele NIC's. (Een virtuele NIC voor elke VM die u maakt voor uw app in de volgende stappen uit). U kunt extra virtuele NIC's en virtuele machines maken op elk gewenst moment en toe te voegen aan de load balancer:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

Wanneer alle drie virtuele NIC's worden gemaakt, door te gaan naar de volgende stap


## <a name="create-virtual-machines"></a>Virtuele machines maken

### <a name="create-cloud-init-config"></a>Cloud-init-configuratie maken
In een vorige zelfstudie over [het aanpassen van een virtuele Linux-machine op de eerste keer opstarten](tutorial-automate-vm-deployment.md), hebt u geleerd hoe u de aanpassing van de virtuele machine met cloud-init automatiseren. U kunt het bestand met dezelfde configuratie cloud init NGINX installeren en uitvoeren van een eenvoudige 'Hallo wereld' Node.js-app in de volgende stap. Toegang tot deze eenvoudige app in een webbrowser om te zien van de load balancer in actie aan het einde van de zelfstudie.

Maak een bestand met de naam in uw huidige shell *cloud init.txt* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. Voer `sensible-editor cloud-init.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a>Virtuele machines maken
Ter verbetering van de hoge beschikbaarheid van uw app in uw virtuele machines in een beschikbaarheidsset te plaatsen. Zie voor meer informatie over beschikbaarheidssets, de vorige [het maken van maximaal beschikbare virtuele machines](tutorial-availability-sets.md) zelfstudie.

Maken van een beschikbaarheidsset met [az vm beschikbaarheidsset maken](/cli/azure/vm/availability-set#create). Het volgende voorbeeld wordt een benoemde beschikbaarheidsset *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Nu kunt u de virtuele machines met [az vm maken](/cli/azure/vm#create). Het volgende voorbeeld maakt drie virtuele machines en SSH-sleutels genereert als deze niet al bestaan:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Er zijn achtergrondtaken die uitvoeren blijven nadat de Azure CLI keert u terug naar de prompt. De `--no-wait` parameter wacht niet totdat de taken te voltooien. Het is mogelijk een andere paar minuten voordat u toegang hebt tot de app. De load balancer-test status wordt automatisch gedetecteerd wanneer de app wordt uitgevoerd op elke virtuele machine. Zodra de app wordt uitgevoerd, begint de load balancer-regel voor het distribueren van verkeer.


## <a name="test-load-balancer"></a>Test load balancer
Het openbare IP-adres van de load balancer met [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#show). Het volgende voorbeeld verkrijgt het IP-adres voor *myPublicIP* eerder hebt gemaakt:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Vervolgens kunt u het openbare IP-adres in invoeren aan een webbrowser. Houd er rekening mee - duurt het enkele minuten duren voordat de virtuele machines moet gereed voor de load balancer-verkeer naar ze te distribueren. De app wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar het volgende voorbeeld gedistribueerde:

![Actieve Node.js-app](./media/tutorial-load-balancer/running-nodejs-app.png)

Overzicht van de load balancer verkeer verdelen over alle drie virtuele machines waarop uw app wordt uitgevoerd, u kunt force vernieuwen van uw webbrowser.


## <a name="add-and-remove-vms"></a>Toevoegen en verwijderen van virtuele machines
U wilt uitvoeren van onderhoud op de virtuele machines waarop uw app, zoals het installeren van updates voor het besturingssysteem wordt uitgevoerd. Om verkeer naar uw app, moet u wellicht extra virtuele machines toevoegen. Deze sectie wordt beschreven hoe u verwijderen of toevoegen van een virtuele machine van de load balancer.

### <a name="remove-a-vm-from-the-load-balancer"></a>Een virtuele machine van de load balancer verwijderen
U kunt een virtuele machine verwijderen uit de back-end-adresgroep met [az nic ip-config-netwerkadresgroep verwijderen](/cli/azure/network/nic/ip-config/address-pool#remove). Het volgende voorbeeld verwijdert u de virtuele NIC voor **myVM2** van *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Als u wilt zien van de load balancer verkeer verdelen over de resterende twee virtuele machines waarop uw app wordt uitgevoerd. u kunt force vernieuwen uw webbrowser. U kunt nu onderhoud uitvoeren op de virtuele machine, zoals het installeren van updates voor het besturingssysteem of het uitvoeren van een VM opnieuw wordt opgestart.

Een overzicht van virtuele machines met virtuele NIC's die zijn verbonden met de load balancer gebruiken [az netwerk lb-adresgroep weergeven](/cli/azure/network/lb/address-pool#show). Zoeken en filteren van de ID van de virtuele NIC als volgt:

```azurecli-interactive
az network lb address-pool show \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myBackEndPool \
    --query backendIpConfigurations \
    --output tsv | cut -f4
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waaruit blijkt dat de virtuele NIC voor VM 2 niet langer deel van de back-end-adresgroep uitmaakt:

```bash
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/ipconfig1
/subscriptions/<guid>/resourceGroups/myResourceGroupLoadBalancer/providers/Microsoft.Network/networkInterfaces/myNic3/ipConfigurations/ipconfig1
```

### <a name="add-a-vm-to-the-load-balancer"></a>Een virtuele machine toevoegen aan de load balancer
Na het uitvoeren van onderhoud van de virtuele machine, of als u over meer capaciteit nodig hebt, kunt u een virtuele machine toevoegen aan de back-end-adresgroep met [az nic ip-config-netwerkadresgroep toevoegen](/cli/azure/network/nic/ip-config/address-pool#add). Het volgende voorbeeld wordt de virtuele NIC voor **myVM2** naar *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```

Gebruik om te controleren dat de virtuele NIC is verbonden met de back-end-adresgroep, [az netwerk lb-adresgroep weergeven](/cli/azure/network/lb/address-pool#show) opnieuw uit de vorige stap.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt gemaakt van een load balancer en virtuele machines is gekoppeld. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure load balancer maken
> * Een load balancer health test maken
> * Regels voor netwerkverkeer voor de load balancer maken
> * Cloud-init gebruiken voor het maken van een eenvoudige Node.js-app
> * Virtuele machines maken en koppelen aan een load balancer
> * Een load balancer in actie weergeven
> * Toevoegen of virtuele machines van een load balancer verwijderen

Ga naar de volgende zelfstudie voor meer informatie over de onderdelen van de virtuele Azure-netwerk.

> [!div class="nextstepaction"]
> [Virtuele machines en virtuele netwerken beheren](tutorial-virtual-network.md)
