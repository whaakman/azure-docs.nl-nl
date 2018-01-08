---
title: Een virtuele-machineschaalset maken met de Azure CLI 2.0 | Microsoft Docs
description: Leer hoe u snel een virtuele-machineschaalset kunt maken met Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 222bfa1c3070fa4634cf5c48d934a6387c48a4b0
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Een virtuele-machineschaalset maken met de Azure CLI 2.0
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. U kunt het aantal VM’s in de schaalset handmatig schalen of regels voor automatisch schalen definiëren op basis van resourcegebruik zoals CPU, vraag naar geheugen, of netwerkverkeer. In dit Aan de slag-artikel maakt u een virtuele-machineschaalset met behulp van de Azure CLI 2.0. U kunt ook een schaalset maken met [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) of [Azure Portal](virtual-machine-scale-sets-create-portal.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.20 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Een schaalset maken
Voordat u een schaalset kunt maken, moet u eerst een resourcegroep maken met [az group create](/cli/azure/group#create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele-machineschaalset met [az vmss create](/cli/azure/vmss#create). Het volgende voorbeeld wordt een schaalset gemaakt met de naam *myScaleSet*, en worden SSH-sleutels gegenereerd als deze nog niet bestaan:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.


## <a name="install-nginx-webserver"></a>NGINX-webserver installeren
U kunt uw schaalset testen met de aangepaste scriptextensie waarmee een script wordt gedownload en uitgevoerd, en NGINX op de VM-instanties wordt geïnstalleerd. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Zie voor meer informatie het [overzicht van de aangepaste scriptextensie](../virtual-machines/linux/extensions-customscript.md).

Pas de aangepaste scriptextensie waarmee NGINX wordt geïnstalleerd als volgt toe:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-web-traffic"></a>Webverkeer toestaan
Als u wilt dat verkeer de webserver kan bereiken, maakt u een load balancer-regel met behulp van [az network lb rule create](/cli/azure/network/lb/rule#create). In het volgende voorbeeld wordt een regel met de naam *myLoadBalancerRuleWeb* gemaakt:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-web-server"></a>De webserver testen
Als u de webserver in actie wilt zien, achterhaalt u het openbare IP-adres van de load balancer met [az network public-ip show](/cli/azure/network/public-ip#show). In het volgende voorbeeld wordt het IP-adres voor *myScaleSetLBPublicIP* opgehaald, dat is gemaakt als onderdeel van de schaalset:

```azurecli-interactive 
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

Voer in een webbrowser het openbare IP-adres van de load balancer in. Via de load balancer wordt verkeer naar een van uw VM-instanties gedistribueerd, zoals wordt weergegeven in het volgende voorbeeld:

![Standaardwebpagina in NGINX](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Resources opschonen
U kunt [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, schaalset en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt. Dit doet u als volgt:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Volgende stappen
In dit Aan de slag-artikel hebt u een standaard schaalset gemaakt en de aangepaste scriptextensie gebruikt om een standaard NGINX-webserver te installeren op de VM-instanties. Breid voor betere schaalbaarheid en automatisering uw schaalset uit met de volgende praktische artikelen:

- [Deploy your application on virtual machine scale sets](virtual-machine-scale-sets-deploy-app.md) (Uw toepassing implementeren op virtuele-machineschaalsets)
- Automatisch schalen met [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) of [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Use automatic OS upgrades for your scale set VM instances](virtual-machine-scale-sets-automatic-upgrade.md) (Automatische upgrades voor het besturingssysteem gebruiken voor de VM-instanties in uw schaalset)