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
ms.date: 01/13/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 42ee74ac250e6594616652157fe85a9088f4021a
ms.openlocfilehash: 0fd7aa8f941adaeb9961fd0e4724161b9fe2eeee


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview-azpy"></a>Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0 Preview (az.py)
In dit artikel ziet u hoe u in Azure snel een virtuele Linux-machine (VM) kunt implementeren met behulp van de opdracht [az vm create](/cli/azure/vm#create) met de Azure-CLI 2.0 (Preview). Er wordt uitleg gegeven over het gebruik van Managed Disks en het gebruik van schijven in systeemeigen opslagaccounts.

> [!NOTE] 
> De Azure-CLI 2.0 (Preview) is onze multi-platform-CLI van de volgende generatie. [Probeer het nu.](https://docs.microsoft.com/cli/azure/install-az-cli2)
>
> Raadpleeg [Een virtuele machine maken met de Azure-CLI](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) als u een virtuele machine wilt maken met de bestaande Azure-CLI 1.0 en niet met de Azure-CLI 2.0 Preview.

Als u een virtuele machine wilt maken, hebt u het volgende nodig: 

* een Azure-account ([krijg een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI v. 2.0 (Preview)](/cli/azure/install-az-cli2) geïnstalleerd
* u moet zijn aangemeld bij uw Azure-account (typ [az login](/cli/azure/#login))

(U kunt een virtuele Linux-machine ook implementeren met behulp van [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

In het volgende voorbeeld ziet u hoe u een virtuele Debian-machine implementeert en hoe u er verbinding mee maakt middels de SSH-sleutel (Secure Shell). De argumenten kunnen afwijken. Als u een andere afbeelding wilt gebruiken, kunt u er [een zoeken](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="using-managed-disks"></a>Managed Disks gebruiken

Als u Azure Managed Disks wilt gebruiken, moet u een regio gebruiken waarin deze worden ondersteund. Typ eerst [az group create](/cli/azure/group#create) om een resourcegroep te maken die alle geïmplementeerde resources bevat:

```azurecli
 az group create -n myResourceGroup -l westus
```

De uitvoer ziet er als volgt uit (u kunt een andere `--output`-optie kiezen als u een andere indeling wilt gebruiken):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
### <a name="create-your-vm"></a>Een virtuele machine maken 
U kunt nu de virtuele machine en de omgeving hiervan maken. Denk eraan de waarde `--public-ip-address-dns-name` te vervangen door een unieke waarde. De onderstaande waarde wordt mogelijk al gebruikt.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


De uitvoer ziet er als volgt uit. Noteer ofwel de waarde `publicIpAddress` ofwel `fqdn` voor **ssh** in uw VM.


```json
{
  "fqdn": "manageddisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-E9-41",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.42.127.53",
  "resourceGroup": "myResourceGroup"
}
```

Meld u bij uw virtuele machine aan met behulp van het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) die in de uitvoer wordt vermeld.

```bash
ssh ops@manageddisks.westus.cloudapp.azure.com
```

Afhankelijk van de distributie die u hebt gekozen, ziet de uitvoer er ongeveer als volgt uit:

```bash
The authenticity of host 'manageddisks.westus.cloudapp.azure.com (134.42.127.53)' can't be established.
RSA key fingerprint is c9:93:f5:21:9e:33:78:d0:15:5c:b2:1a:23:fa:85:ba.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'manageddisks.westus.cloudapp.azure.com' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan 13 14:44:21 2017 from net-37-117-240-123.cust.vodafonedsl.it
ops@myVM:~$ 
```

Zie [Volgende stappen](#next-steps) voor informatie over andere dingen die u via Managed Disks kunt doen met uw nieuwe virtuele machine.

## <a name="using-unmanaged-disks"></a>Niet-beheerde schijven gebruiken 

Virtuele machines die gebruikmaken van niet-beheerde opslagschijven hebben niet-beheerde opslagaccounts. Typ [az group create](/cli/azure/group#create) om ervoor te zorgen dat uw resourcegroep alle geïmplementeerde resources bevat:

```azurecli
az group create --name nativedisks --location westus
```

De uitvoer ziet er als volgt uit (u kunt een andere `--output`-optie kiezen als u dat wenst):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks",
  "location": "westus",
  "managedBy": null,
  "name": "nativedisks",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### <a name="create-your-vm"></a>Een virtuele machine maken 

U kunt nu de virtuele machine en de omgeving hiervan maken. Denk eraan de waarde `--public-ip-address-dns-name` te vervangen door een unieke waarde. De onderstaande waarde wordt mogelijk al gebruikt.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name nativedisks \
--resource-group nativedisks \
--location westus \
--name myVM \
--use-native-disk
```

De uitvoer ziet er als volgt uit. Noteer ofwel de waarde `publicIpAddress` ofwel `fqdn` voor **ssh** in uw VM.

```json
{
  "fqdn": "nativedisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-33-24-3C",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.91.91.195",
  "resourceGroup": "nativedisks"
}
```

Meld u bij uw virtuele machine aan met behulp van het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN). Beide opties worden in de bovenstaande uitvoer vermeld.

```bash
ssh ops@nativedisks.westus.cloudapp.azure.com
```

Afhankelijk van de distributie die u hebt gekozen, ziet de uitvoer er ongeveer als volgt uit:

```
The authenticity of host 'nativedisks.westus.cloudapp.azure.com (13.91.93.195)' can't be established.
RSA key fingerprint is 3f:65:22:b9:07:c9:ef:7f:8c:1b:be:65:1e:86:94:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'nativedisks.westus.cloudapp.azure.com,13.91.93.195' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@myVM:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Volgende stappen
De opdracht `az vm create` is een goede manier om snel een virtuele machine te implementeren, zodat u zich kunt aanmelden bij een bash-shell en aan de slag kunt gaan. Wanneer u `az vm create` gebruikt, hebt u echter geen uitgebreide controle en kunt u geen complexere omgeving maken.  Als u een virtuele Linux-machine wilt implementeren die is afgestemd op uw infrastructuur, volg dan de instructies in een van deze artikelen:

* [Een Azure Resource Manager-sjabloon gebruiken om een specifieke implementatie te maken](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een met SSH beveiligde virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

U kunt ook het [Azure-stuurprogramma `docker-machine` gebruiken dat verschillende opdrachten heeft om snel een Linux-VM als een docker host](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) te maken. Als u Java gebruikt, kunt u daarnaast de methode [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine) proberen.




<!--HONumber=Feb17_HO2-->


