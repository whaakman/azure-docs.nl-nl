---
title: Maken en een OpenBSD VM-installatiekopie uploaden naar Azure | Microsoft Docs
description: Meer informatie over het maken en uploaden van een virtuele harde schijf (VHD) die met het besturingssysteem OpenBSD voor het maken van een virtuele machine van Azure via Azure CLI
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: huishao
ms.openlocfilehash: 9b4163471f3dc8483993b9ac762694af4e926aa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a>Maken en een schijfkopie OpenBSD uploaden naar Azure
In dit artikel leest u hoe maken en uploaden van een virtuele harde schijf (VHD) die het besturingssysteem OpenBSD bevat. Nadat u deze uploaden, kunt u deze als uw eigen installatiekopie maken van een virtuele machine (VM) in Azure via Azure CLI.


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende items hebt:

* **Een Azure-abonnement** -als u geen account hebt, kunt u een in een paar minuten. Als u een MSDN-abonnement hebt, raadpleegt u [maandelijkse Azure-tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Anders wordt informatie over hoe [maken van een gratis proefaccount](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure CLI 2.0** -Zorg ervoor dat de meest recente [Azure CLI 2.0](/cli/azure/install-azure-cli) geïnstalleerd en aangemeld bij uw Azure-account met [az aanmelding](/cli/azure/#login).
* **OpenBSD-besturingssysteem is geïnstalleerd in een .vhd-bestand** -een ondersteund besturingssysteem van OpenBSD (versie 6.1) moet worden geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het VHD-bestanden maken. Bijvoorbeeld, kunt u een oplossing voor netwerkvirtualisatie zoals Hyper-V te maken van het VHD-bestand en het besturingssysteem te installeren. Zie voor instructies over het installeren en gebruiken van Hyper-V [Hyper-V installeren en een virtuele machine maken](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>De installatiekopie van het OpenBSD voorbereiden voor Azure
Ondersteuning op de virtuele machine waar u het OpenBSD besturingssysteem 6,1, dat toegevoegd Hyper-V hebt geïnstalleerd, voert u de volgende procedures:

1. Als DHCP niet is ingeschakeld tijdens de installatie, schakelt u de service als volgt uit:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Stel een seriële console als volgt in:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. De installatie van pakket als volgt configureren:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Standaard de `root` gebruiker is uitgeschakeld op virtuele machines in Azure. Gebruikers kunnen opdrachten uitvoeren met verhoogde bevoegdheden met behulp van de `doas` opdracht op OpenBSD VM. Doas is standaard ingeschakeld. Zie voor meer informatie [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Installeren en configureren van vereisten voor de Azure-Agent als volgt:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. De nieuwste versie van de Azure-agent altijd vindt u op [Github](https://github.com/Azure/WALinuxAgent/releases). Installeer de agent als volgt:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Nadat u Azure-Agent hebt geïnstalleerd, is het een goed idee om te verifiëren dat dit als volgt uitgevoerd:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Het systeem om deze op te schonen, waardoor het geschikt is voor reprovisioning inrichting ervan ongedaan. De volgende opdracht worden ook de laatste ingerichte gebruikersaccount en de bijbehorende gegevens verwijderd:

    ```sh
    waagent -deprovision+user -force
    ```

U kunt nu uw virtuele machine afsluiten.


## <a name="prepare-the-vhd"></a>Voorbereiden van de VHD
De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**. U kunt de schijf converteren naar vaste VHD-indeling met Hyper-V-beheer of de Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet. Een voorbeeld is als volgende.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Storage-resources maken en uploaden
Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroup --location eastus
```

Als u wilt uw VHD uploaden, maken van een opslagaccount met [az storage-account maken](/cli/azure/storage/account#create). Namen van opslagaccounts moeten uniek zijn, zodat uw eigen naam opgeven. Het volgende voorbeeld wordt een opslagaccount met de naam *mystorageaccount*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

Voor het beheren van toegang tot het opslagaccount ophalen van de opslagsleutel met [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list) als volgt:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

Logisch afzonderlijke de VHD's die u uploadt, maakt u een container in het opslagaccount met [az storage-container maken](/cli/azure/storage/container#create):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Ten slotte uw VHD met uploaden [uploaden van blob storage az](/cli/azure/storage/blob#upload) als volgt:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>Virtuele machine van de VHD maken
U kunt maken met een virtuele machine een [voorbeeldscript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) of rechtstreeks met [az vm maken](/cli/azure/vm#create). De OpenBSD VHD die u hebt geüpload, gebruikt u de `--image` parameter als volgt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Het IP-adres verkrijgen voor uw VM OpenBSD met [az vm lijst-ip-adressen](/cli/azure/vm#list-ip-addresses) als volgt:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

U kunt nu SSH voor uw OpenBSD VM als normale:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Volgende stappen
Als u meer weten over de Hyper-V-ondersteuning op OpenBSD6.1 wilt, leest u [OpenBSD 6.1](https://www.openbsd.org/61.html) en [hyperv.4](http://man.openbsd.org/hyperv.4).

Als u maken van een virtuele machine van beheerde schijf wilt, Lees [az schijf](/cli/azure/disk). 
