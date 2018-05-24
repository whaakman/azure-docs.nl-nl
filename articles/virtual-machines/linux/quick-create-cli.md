---
title: 'Snelstartgids: Een virtuele Linux-machine maken met de Azure CLI 2.0 | Microsoft Docs'
description: In deze snelstartgids leert u hoe u de Azure CLI 2.0 gebruikt om een virtuele Linux-machine te maken
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c45f8f010d69337d21fce327933990a573988a4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-virtual-machine-with-the-azure-cli-20"></a>Snelstartgids: Een virtuele Linux-machine maken met de Azure CLI 2.0

De Azure CLI 2.0 wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze snelstartgids wordt beschreven hoe u de Azure CLI 2.0 gebruikt voor het implementeren van een virtuele Linux-machine (VM) in Azure waarop Ubuntu wordt uitgevoerd. Om uw virtuele machine in actie te zien, voert u SSH voor de virtuele machine uit en installeert u de NGINX-webserver.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de Azure CLI versie 2.0.30 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm#az_vm_create).

In het volgende voorbeeld wordt de VM *myVM* gemaakt, wordt het gebruikersaccount *azureuser* toegevoegd en worden SSH-sleutels gemaakt als deze nog niet aanwezig zijn in de standaardsleutellocatie (*~/.ssh*). Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. In het volgende voorbeeld van uitvoer ziet u dat het maken van de virtuele machine is geslaagd.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Noteer uw eigen `publicIpAddress` in de uitvoer van uw virtuele machine. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Standaard worden alleen SSH-verbindingen geopend wanneer u een virtuele Linux-machine in Azure maakt. Gebruik [az vm open-port](/cli/azure/vm#az_vm_open_port) om TCP-poort 80 te openen voor gebruik met de NGINX-webserver:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

SSH met uw virtuele machine zoals normaal. Vervang **publicIPAddress** door het openbare IP-adres van uw VM, zoals is aangegeven in de vorige uitvoer van de VM:

```bash
ssh azureuser@publicIpAddress
```

## <a name="install-web-server"></a>Webserver installeren

Als u uw virtuele machine in actie wilt zien, installeert u de NGINX-webserver. Gebruik de volgende opdrachten vanuit uw SSH-sessie om pakketbronnen bij te werken en het meest recente NGINX-pakket te installeren:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

`exit` de SSH-sessie wanneer u klaar bent.

## <a name="view-the-web-server-in-action"></a>De webserver in actie zien

Nu NGINX is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van NGINX weer te geven. Gebruik het openbare IP-adres van uw virtuele machine dat is verkregen in een vorige stap. In het volgende voorbeeld ziet u de NGINX-standaardwebsite:

![Standaardsite van NGINX](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az_group_delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt. Zorg ervoor dat u de SSH-sessie met uw virtuele machine hebt afgesloten en verwijder de resources als volgt:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkpoort geopend voor internetverkeer en een eenvoudige webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Linux-machines.


> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Linux-machines](./tutorial-manage-vm.md)
