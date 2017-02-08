---
title: Een virtuele Linux-machine maken met behulp van de Azure-CLI 2.0 (Preview) | Microsoft Azure
description: Een virtuele Linux-machine maken met behulp van de Azure-CLI 2.0 (Preview).
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
ms.assetid: 82005a05-053d-4f52-b0c2-9ae2e51f7a7e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 95b924257c64a115728c66956d5ea38eb8764a35
ms.openlocfilehash: 70592ac773aced0bfcec5c7418a6dc53555fab33


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview-azpy"></a>Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0 Preview (az.py)
In dit artikel ziet u hoe u in Azure snel een virtuele Linux-machine (VM) kunt implementeren met behulp van de opdracht [az vm create](/cli/azure/vm#create) met de Azure-CLI 2.0 (Preview). 

> [!NOTE] 
> De Azure-CLI 2.0 (Preview) is onze multi-platform-CLI van de volgende generatie. [Probeer het nu.](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2)
>
> In de overige documenten wordt gebruikgemaakt van de bestaande Azure-CLI. Raadpleeg [Een virtuele machine maken met Azure-CLI](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) als u een virtuele machine wilt maken met de bestaande Azure CLI 1.0 en niet met de CLI 2.0 Preview.

Als u een virtuele machine wilt maken, hebt u het volgende nodig: 

* een Azure-account ([krijg een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI v. 2.0 (Preview)](/cli/azure/install-az-cli2) geïnstalleerd
* u moet zijn aangemeld bij uw Azure-account (typ [az login](/cli/azure/#login))

(U kunt ook snel een virtuele Linux-machine implementeren met behulp van [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

In het volgende voorbeeld ziet u hoe u een virtuele Debian-machine implementeert en uw SSH-sleutel (Secure Shell) koppelt (uw argumenten kunnen afwijken; als u een andere installatiekopie wilt, [kunt u er een zoeken](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Typ eerst [az group create](/cli/azure/group#create) om een resourcegroep te maken die alle geïmplementeerde resources bevat:

```azurecli
az group create -n myResourceGroup -l westus
```

De uitvoer ziet er als volgt uit (u kunt een andere `--output`-optie kiezen als u dat wenst):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-your-vm-using-the-latest-debian-image"></a>Een virtuele machine maken met de nieuwste Debian-installatiekopie

U kunt nu de virtuele machine en de omgeving hiervan maken. Denk eraan de waarde `----public-ip-address-dns-name` te vervangen door een unieke waarde. De onderstaande waarde wordt mogelijk al gebruikt.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username ops \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name mydns \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


De uitvoer ziet er als volgt uit. Noteer ofwel de waarde `publicIpAddress` ofwel `fqdn` voor **ssh** in uw VM.


```json
{
  "fqdn": "mydns.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-05-07",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.112.217.29",
  "resourceGroup": "myResourceGroup"
}
```

Meld u met het openbare IP-adres dat in de uitvoer staat vermeld aan bij uw virtuele machine. U kunt ook de volledig gekwalificeerde domeinnaam (FQDN) gebruiken die wordt vermeld.

```bash
ssh ops@mydns.westus.cloudapp.azure.com
```

Afhankelijk van de distributie die u hebt gekozen, ziet de uitvoer er ongeveer als volgt uit:

```
The authenticity of host 'mydns.westus.cloudapp.azure.com (40.112.217.29)' can't be established.
RSA key fingerprint is SHA256:xbVC//lciRvKild64lvup2qIRimr/GB8C43j0tSHWnY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mydns.westus.cloudapp.azure.com,40.112.217.29' (RSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@mynewvm:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Volgende stappen
De opdracht `az vm create` is een goede manier om snel een virtuele machine te implementeren, zodat u zich kunt aanmelden bij een bash-shell en aan de slag kunt gaan. Wanneer u `az vm create` gebruikt, hebt u echter geen uitgebreide controle en kunt u geen complexere omgeving maken.  Als u een virtuele Linux-machine wilt implementeren die is afgestemd op uw infrastructuur, volg dan de instructies in een van deze artikelen:

* [Een Azure Resource Manager-sjabloon gebruiken om een specifieke implementatie te maken](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een met SSH beveiligde virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

U kunt ook het [Azure-stuurprogramma `docker-machine` gebruiken dat verschillende opdrachten heeft om snel een Linux-VM als een docker host](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) te maken. Als u Java gebruikt, kunt u daarnaast de methode [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine) proberen.




<!--HONumber=Jan17_HO1-->


