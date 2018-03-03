---
title: "Selecteer de installatiekopieën van het Linux-VM met de Azure CLI | Microsoft Docs"
description: "Informatie over het gebruik van de Azure CLI om te bepalen van de uitgever, aanbieding, SKU en versie voor installatiekopieën van Marketplace VM."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c65ebbc8a61c13b96364dadde45bd4bca828e337
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Linux-VM-installatiekopieën zoeken in Azure Marketplace met de Azure CLI
Dit onderwerp wordt beschreven hoe u met de Azure CLI 2.0 VM-installatiekopieën vinden in Azure Marketplace. Gebruik deze informatie om een Marketplace-installatiekopie opgeven wanneer u een virtuele machine via een programma met de CLI maken Resource Manager-sjablonen of andere hulpprogramma's.

Zorg ervoor dat u de meest recente geïnstalleerd [Azure CLI 2.0](/cli/azure/install-az-cli2) en u bent aangemeld bij een Azure-account (`az login`).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="list-popular-images"></a>Lijst met populaire installatiekopieën

Voer de [az vm afbeeldingenlijst](/cli/azure/vm/image#az_vm_image_list) opdracht, zonder de `--all` optie voor een overzicht van populaire VM-installatiekopieën in Azure Marketplace. Voer bijvoorbeeld de volgende opdracht een lijst wilt weergeven in de cache van populaire afbeeldingen in tabelindeling:

```azurecli
az vm image list --output table
```

De uitvoer bevat de installatiekopie URN (de waarde in de *Urn* kolom). Wanneer u een virtuele machine maakt met een van deze populaire Marketplace-installatiekopieën, kunt u ook opgeven de *UrnAlias*, een verkorte vorm zoals *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Specifieke installatiekopieën zoeken

Ga voor een specifieke VM-installatiekopie in de Marketplace, gebruiken de `az vm image list` opdracht met de `--all` optie. Deze versie van de opdracht neemt enige tijd voltooid en kan langdurige uitvoer geretourneerd zodat u doorgaans de lijst filteren op `--publisher` of een andere parameter. 

De volgende opdracht geeft bijvoorbeeld alle Debian-aanbiedingen (Vergeet niet dat zonder de `--all` overschakelen, zoekt alleen de lokale cache van algemene installatiekopieën):

```azurecli
az vm image list --offer Debian --all --output table 

```

Gedeeltelijke uitvoer: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

Toepassen van filters voor soortgelijke de `--location`, `--publisher`, en `--sku` opties. U kunt zelfs gedeeltelijke overeenkomsten uitvoeren op een filter, zoals het zoeken naar `--offer Deb` alle Debian installatiekopieën vinden.

Als u geen dat een bepaalde locatie met opgeeft de `--location` optie, de waarden voor de standaardlocatie worden geretourneerd. (Een andere standaardlocatie ingesteld door `az configure --defaults location=<location>`.)

De volgende opdracht worden bijvoorbeeld alle Debian 8 SKU's op de locatie West-Europa:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Gedeeltelijke uitvoer:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-the-images"></a>Installatiekopieën van het navigeren 
Een installatiekopie niet vinden in een locatie op een andere manier is om uit te voeren de [az vm image lijst-uitgevers](/cli/azure/vm/image#az_vm_image_list_publishers), [az vm image lijst-aanbiedingen](/cli/azure/vm/image#az_vm_image_list_offers), en [az vm image lijst-SKU's](/cli/azure/vm/image#az_vm_image_list_skus) opdrachten in de reeks. Met deze opdrachten moet u deze waarden bepalen:

1. Geef de uitgevers van installatiekopieën weer.
2. Geef de aanbiedingen voor een bepaalde uitgever weer.
3. Geef de SKU's voor een bepaalde aanbieding weer.

U kunt vervolgens een versie om te implementeren kiezen voor een geselecteerde SKU.

De volgende opdracht worden bijvoorbeeld de uitgevers van de installatiekopie op de locatie VS-West:

```azurecli
az vm image list-publishers --location westus --output table
```

Gedeeltelijke uitvoer:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Gebruik deze informatie om te zoeken aanbiedingen van een bepaalde uitgever. Bijvoorbeeld, als *Canonical* installatiekopie-uitgever is gevonden op de locatie VS-West, hun aanbiedingen door het uitvoeren van `azure vm image list-offers`. Geef de locatie en de uitgever zoals in het volgende voorbeeld:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Uitvoer:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
U ziet dat in de regio VS-West Canonical publiceert de *UbuntuServer* bieden op Azure. Maar welke SKU's? Uitvoeren als u deze waarden, `azure vm image list-skus` en stel de locatie, de uitgever en de aanbieding die u gedetecteerd:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Uitvoer:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Gebruik tot slot de `az vm image list` opdracht om te bepalen van een specifieke versie van de SKU die u wilt bijvoorbeeld *16.04 LTS*:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Uitvoer:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```

U kunt nu nauwkeurig de installatiekopie die u gebruiken wilt door de URN-waarde. Deze waarde met de `--image` parameter bij het maken van een virtuele machine met de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. Houd er rekening mee dat u eventueel het versienummer in de URN door 'nieuwste vervangen kunt'. Deze versie is altijd de nieuwste versie van de installatiekopie. 

Als u een virtuele machine met Resource Manager-sjabloon implementeert, u instellen van de parameters van de afbeelding afzonderlijk in de `imageReference` eigenschappen. Zie de [sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Eigenschappen van abonnement weergeven
Uitvoeren van een installatiekopie purchase plan om informatie te bekijken, de [az vm afbeelding worden weergegeven](/cli/azure/image#az_image_show) opdracht. Als de `plan` eigenschap in de uitvoer is niet `null`, de installatiekopie heeft de voorwaarden accepteren voordat programmatische implementatie moet u.

Bijvoorbeeld: de canonieke Ubuntu Server 16.04 TNS afbeelding geen aanvullende voorwaarden omdat de `plan` informatie is `null`:

```azurecli
az vm image show --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --version 16.04.201801260
```

Uitvoer:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/16.04.201801260",
  "location": "westus",
  "name": "16.04.201801260",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

Een vergelijkbaar opdracht uitgevoerd voor de RabbitMQ certificeert Bitnami afbeelding ziet u de volgende `plan` eigenschappen: `name`, `product`, en `publisher`. (Enkele afbeeldingen hebben ook een `promotion code` eigenschap.) Zie de volgende secties de voorwaarden accepteren en programmatische implementatie inschakelen voor het implementeren van deze installatiekopie.

```azurecli
az vm image show --location westus --publisher bitnami --offer rabbitmq --sku rabbitmq --version 3.7.1801130730
```
Uitvoer:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/bitnami/ArtifactTypes/VMImage/Offers/rabbitmq/Skus/rabbitmq/Versions/3.7.1801130730",
  "location": "westus",
  "name": "3.7.1801130730",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": {
    "name": "rabbitmq",
    "product": "rabbitmq",
    "publisher": "bitnami"
  },
  "tags": null
}
```

### <a name="accept-the-terms"></a>De voorwaarden accepteren
Als u wilt weergeven en accepteer de licentievoorwaarden, gebruiken de [az vm image accepteren-voorwaarden](/cli/azure/vm/image?#az_vm_image_accept_terms) opdracht. Wanneer u de voorwaarden accepteert, kunt u programmatische implementatie inschakelen in uw abonnement. U hoeft alleen te accepteren eenmaal per abonnement voor de installatiekopie. Bijvoorbeeld:

```azurecli
az vm image accept-terms --urn bitnami:rabbitmq:rabbitmq:latest
``` 

De uitvoer bevat een `licenseTextLink` aan de licentie termen en geeft aan dat de waarde van `accepted` is `true`:

```
{
  "accepted": true,
  "additionalProperties": {},
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.MarketplaceOrdering/offertypes/bitnami/offers/rabbitmq/plans/rabbitmq",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_BITNAMI%253a24RABBITMQ%253a24RABBITMQ%253a24IGRT7HHPIFOBV3IQYJHEN2O2FGUVXXZ3WUYIMEIVF3KCUNJ7GTVXNNM23I567GBMNDWRFOY4WXJPN5PUYXNKB2QLAKCHP4IE5GO3B2I.txt",
  "name": "rabbitmq",
  "plan": "rabbitmq",
  "privacyPolicyLink": "https://bitnami.com/privacy",
  "product": "rabbitmq",
  "publisher": "bitnami",
  "retrieveDatetime": "2018-02-22T04:06:28.7641907Z",
  "signature": "WVIEA3LAZIK7ZL2YRV5JYQXONPV76NQJW3FKMKDZYCRGXZYVDGX6BVY45JO3BXVMNA2COBOEYG2NO76ONORU7ITTRHGZDYNJNKLNLWI",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```

### <a name="deploy-using-purchase-plan-parameters"></a>Implementeren met behulp van inkoop plan parameters
Na het accepteren van de voorwaarden voor de installatiekopie, kunt u een virtuele machine in het abonnement te implementeren. De installatiekopie te implementeren met behulp van de `az vm create` opdracht, parameters voor het inkoopabonnement Daarnaast bieden aan een URN voor de installatiekopie. Als u bijvoorbeeld voor het implementeren van een virtuele machine met de RabbitMQ gecertificeerd door Bitnami afbeelding:

```azurecli
az group create --name myResourceGroupVM --location westus

az vm create --resource-group myResourceGroupVM --name myVM --image bitnami:rabbitmq:rabbitmq:latest --plan-name rabbitmq --plan-product rabbitmq --plan-publisher bitnami

```



## <a name="next-steps"></a>Volgende stappen
Om snel een virtuele machine maken met behulp van de installatiekopie-informatie, Zie [maken en beheren van virtuele Linux-machines met de Azure CLI](tutorial-manage-vm.md).