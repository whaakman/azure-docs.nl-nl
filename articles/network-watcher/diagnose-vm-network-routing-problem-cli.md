---
title: Diagnose van een virtuele machine routering netwerkprobleem - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe een virtuele machine-netwerk routering probleem met de volgende hop-mogelijkheden van Azure-netwerk-Watcher op te sporen.
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
ms.openlocfilehash: fcb7ec2e40b5c0e8794d2f4d70395dcbecca019c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182498"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Diagnose van een virtuele machine routering netwerkprobleem - Azure CLI

In dit artikel wordt een virtuele machine (VM) implementeren, en worden vervolgens controleren communicatie naar een IP-adres en een URL. U oorzaak de van een communicatiefout is opgetreden en hoe u het kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Nadat u de versie van de CLI controleren, voert `az login` geen verbinding maken met Azure. De CLI-opdrachten in dit artikel worden opgemaakt als u wilt uitvoeren in een Bash-shell.

## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een virtuele machine maken kunt, moet u een resourcegroep te bevatten van de virtuele machine maken. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. Het volgende voorbeeld wordt een virtuele machine met de naam *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Niet doorgaan met de resterende stappen totdat de virtuele machine wordt gemaakt en de CLI wordt de uitvoer geretourneerd.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Als u wilt testen netwerkcommunicatie met de netwerk-Watcher, moet u eerst inschakelen in de regio die de virtuele machine die u wilt testen, is in een netwerk-watcher en gebruik vervolgens de netwerk-Watcher volgende hop mogelijkheid om communicatie te testen.

### <a name="enable-network-watcher"></a>Inschakelen van netwerk-watcher

Als u al een netwerk-watcher ingeschakeld in de regio VS-Oost, gaat u naar [gebruik volgende hop](#use-next-hop). Gebruik de [az netwerk-watcher configureren](/cli/azure/network/watcher#az-network-watcher-configure) opdracht voor het maken van een netwerk-watcher in de regio VS-Oost:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Gebruik de volgende hop

Azure maakt automatisch routes naar standaard bestemmingen. U kunt aangepaste routes die de standaardroutes overschrijven maken. Soms aangepaste routes kunnen leiden tot communicatie mislukken. U kunt testen routering van een virtuele machine met [az netwerk-watcher weergeven volgende hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) om te bepalen van de volgende hop routering wanneer verkeer is dat is bestemd voor een specifiek adres.

Uitgaande communicatie van de virtuele machine op een van de IP-adressen voor www.bing.com testen:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Na enkele seconden, informeert u met de uitvoer die de **nextHopType** is **Internet**, en dat de **routeTableId** is **Systeemroute**. Dit laat u weten of er een geldige route naar de bestemming is.

Uitgaande communicatie van de virtuele machine 172.31.0.100 testen:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

De uitvoer die wordt geretourneerd, informeert u die **geen** is de **nextHopType**, en dat de **routeTableId** is ook **Systeemroute**. Dit laat u weten dat er is een geldige route naar de bestemming, maar er geen volgende hop is voor het routeren van het verkeer naar de bestemming.

## <a name="view-details-of-a-route"></a>Details weergeven van een route

Voor het analyseren van verdere routering, Controleer de effectieve routes voor de netwerkinterface met de [az netwerk nic-ingang-route-tabel weergeven](/cli/azure/network/nic#az-network-nic-show-effective-route-table) opdracht:

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

Wanneer u gebruikt de `az network watcher show-next-hop` opdracht voor het testen van uitgaande communicatie 13.107.21.200 in [gebruik volgende hop](#use-next-hop), de route met de **addressPrefix** 0.0.0.0/0** is gebruikt om gegevensverkeer te routeren naar het adres sindsdien Er zijn geen andere route in de uitvoer bevat de adres. Standaard worden alle adressen niet worden opgegeven in het adresvoorvoegsel van een andere route doorgestuurd naar internet.

Wanneer u gebruikt de `az network watcher show-next-hop` opdracht voor het testen van uitgaande communicatie 172.31.0.100 echter, het resultaat u laten weten dat er geen volgend hoptype is. In de resulterende uitvoer ziet u ook de volgende tekst:

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

Zoals u in de uitvoer van ziet de `az network watcher nic show-effective-route-table` opdracht, maar er is een standaardroute aan het voorvoegsel 172.16.0.0/12, waaronder de 172.31.0.100 adres, de **nextHopType** is **geen**. Azure maakt een standaardroute voor 172.16.0.0/12, maar pas er is een reden voor een volgend hoptype niet opgeven. Als u bijvoorbeeld het adresbereik 172.16.0.0/12 toegevoegd aan de adresruimte van het virtuele netwerk, Azure verandert de **nextHopType** naar **virtueel netwerk** voor de route. Vervolgens wordt een controle wilt weergeven **virtueel netwerk** als de **nextHopType**.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az_group_delete) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, als deze niet meer nodig zijn:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een virtuele machine gemaakt en gediagnosticeerd netwerkroutering van de virtuele machine. Hebt u geleerd dat Azure verschillende standaardroutes maakt en getest routering naar twee verschillende bestemmingen. Meer informatie over [routering in Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en hoe [maken van aangepaste routes](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Voor uitgaande verbindingen van de VM, u kunt ook bepalen de latentie en toegestane en geweigerde netwerkverkeer tussen de virtuele machine en een eindpunt met behulp van netwerk-Watcher [verbinding oplossen](network-watcher-connectivity-cli.md) mogelijkheid. U kunt de communicatie tussen een virtuele machine en een eindpunt, zoals een IP-adres of -URL, gedurende een periode met de netwerk-Watcher verbinding monitor mogelijkheid bewaken. Voor meer informatie Zie [bewaken van een netwerkverbinding](connection-monitor.md).