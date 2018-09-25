---
title: Een virtuele machine netwerk routeringsprobleem vaststellen - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u een VM-netwerk routering probleem met behulp van de volgende hop-mogelijkheden van Azure Network Watcher op te sporen.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 8b881e51bfac25b83a828ad1f44fcd6d7da1c791
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948042"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Een virtuele machine netwerk routeringsprobleem vaststellen - Azure CLI

In dit artikel, kunt u een virtuele machine (VM) implementeren, en controleert op communicatie naar een IP-adres en de URL. U stelt de oorzaak van mislukte communicatie vast en leert hoe u dit probleem kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel moet u de Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). Nadat u de CLI-versie hebt gecontroleerd, voert u `az login` uit om een verbinding met Azure te maken. De CLI-opdrachten in dit artikel worden opgemaakt als u wilt uitvoeren in een Bash-shell.

## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een VM kunt maken, maakt u eerst een resourcegroep die de VM bevat. Maak een resourcegroep maken met [az group create](/cli/azure/group#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Maak een VM met [az vm create](/cli/azure/vm#az-vm-create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. In het volgende voorbeeld wordt een VM gemaakt met de naam *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Ga niet door met de resterende stappen totdat de VM is gemaakt en uitvoer wordt geretourneerd met de CLI.

## <a name="test-network-communication"></a>Netwerkcommunicatie testen

Als u wilt testen netwerkcommunicatie met Network Watcher, moet u eerst een netwerk-watcher in de regio voor de virtuele machine die u wilt testen inschakelen en vervolgens de volgende hop-mogelijkheden van Network Watcher gebruiken om communicatie te testen.

### <a name="enable-network-watcher"></a>Netwerk-watcher inschakelen

Als u al een netwerk-watcher ingeschakeld in de regio VS-Oost, gaat u naar [gebruik volgende hop](#use-next-hop). Gebruik de [az network watcher configureren](/cli/azure/network/watcher#az-network-watcher-configure) opdracht voor het maken van een network watcher in de regio VS-Oost:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Volgende hop gebruiken

Azure maakt automatisch routes naar standaardbestemmingen. U kunt uw eigen, aangepaste routes maken om die standaardroutes te overschrijven. Soms hebben aangepaste routes tot gevolg dat de communicatie mislukt. Als u wilt testen van de routering van een virtuele machine, gebruikt u [az network watcher show volgende hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) om te bepalen van de volgende hop routering wanneer verkeer bestemd voor een specifiek adres is.

Uitgaande communicatie van de VM naar een van de IP-adressen testen voor www.bing.com:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Na enkele seconden, de uitvoer wordt gemeld dat de **nextHopType** is **Internet**, en dat de **routeTableId** is **Systeemroute**. Dit resultaat laat u weten dat er een geldige route naar de bestemming is.

Uitgaande communicatie van de VM naar 172.31.0.100 testen:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

De uitvoer die wordt geretourneerd, informeert u die **geen** is de **nextHopType**, en dat de **routeTableId** is ook **Systeemroute**. Hieruit kunt u afleiden dat er wel een geldige systeemroute is naar de bestemming, maar dat er geen volgende hop is voor het routeren van het verkeer naar de bestemming.

## <a name="view-details-of-a-route"></a>Details van een route weergeven

Voor het analyseren van verdere routering, Controleer de effectieve routes voor de netwerkinterface met de [az network nic show-effectief-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table) opdracht:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

De volgende tekst is opgenomen in de resulterende uitvoer:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Wanneer u gebruikt de `az network watcher show-next-hop` opdracht voor het testen van uitgaande communicatie naar 13.107.21.200 in [gebruik volgende hop](#use-next-hop), de route met de **addressPrefix** 0.0.0.0/0** is gebruikt voor het routeren van verkeer naar het adres, aangezien Er zijn geen andere route in de uitvoer bevat het adres. De standaardinstelling is dat alle adressen die niet zijn opgegeven in het adresvoorvoegsel van een andere route, worden doorgestuurd naar internet.

Wanneer u gebruikt de `az network watcher show-next-hop` opdracht voor het testen van uitgaande communicatie naar 172.31.0.100 echter, het resultaat u geïnformeerd dat er is geen type volgende hop. In de resulterende uitvoer ziet u ook de volgende tekst:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

Zoals u in de uitvoer van ziet de `az network watcher nic show-effective-route-table` opdracht, maar er is een standaardroute voor het voorvoegsel 172.16.0.0/12, waaronder de 172.31.0.100-adres, de **nextHopType** is **geen**. Azure maakt een standaardroute naar 172.16.0.0/12, maar stelt geen volgend hoptype in, tenzij daar een reden voor is. Als u bijvoorbeeld het adresbereik 172.16.0.0/12 toegevoegd aan de adresruimte van het virtuele netwerk, Azure verandert de **nextHopType** naar **virtueel netwerk** voor de route. Vervolgens ziet u een selectievakje **virtueel netwerk** als de **nextHopType**.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, als deze niet meer nodig zijn:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een virtuele machine gemaakt en gediagnosticeerd de routering van de virtuele machine. U hebt geleerd dat Azure verschillende standaardroutes maakt en u hebt de routering naar twee verschillende bestemmingen getest. Lees hier meer over [routering in Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en hoe u [aangepaste routes maakt](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Voor uitgaande verbindingen van de virtuele machine, u kunt ook bepalen de latentie en toegestane en geweigerde netwerkverkeer tussen de virtuele machine en een eindpunt met behulp van Network Watcher [probleemoplossing voor verbindingen](network-watcher-connectivity-cli.md) mogelijkheid. U kunt de communicatie tussen een virtuele machine en een eindpunt, zoals een IP-adres of de URL, na verloop van tijd met behulp van de mogelijkheden van Network Watcher connection monitor bewaken. Voor meer informatie Zie [bewaken van een netwerkverbinding](connection-monitor.md).