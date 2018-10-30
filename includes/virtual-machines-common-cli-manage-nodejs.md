---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 682288338d66f1006e9f1d7f3d6d30f822626425
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226739"
---
U hebt een account van Azure nodig als u Azure CLI wilt gebruiken met opdrachten en sjablonen van Resource Manager voor het implementeren van Azure-resources en -werkbelastingen met behulp van resourcegroepen. Als u geen account hebt, kunt u [hier een gratis proefversie starten](https://azure.microsoft.com/pricing/free-trial/).

Als u Azure CLI nog niet hebt geïnstalleerd en gekoppeld aan uw abonnement, raadpleegt u [Azure CLI installeren](../articles/cli-install-nodejs.md), stelt u de modus in op `arm` met `azure config mode arm` en gebruikt u de opdracht `azure login` om verbinding te maken met Azure.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- Klassieke Azure CLI: onze CLI voor het klassieke en resource Manager-implementatiemodel (dit artikel)
- [Azure CLI](../articles/virtual-machines/linux/cli-manage.md) : onze CLI de volgende generatie voor het resource Manager-implementatiemodel

## <a name="basic-azure-resource-manager-commands-in-azure-classic-cli"></a>Azure Resource Manager-basisopdrachten in Azure klassieke CLI

In dit artikel bevat informatie over eenvoudige opdrachten die u wilt gebruiken met Azure CLI voor klassieke om te beheren en manipuleren van uw resources (voornamelijk VM's) in uw Azure-abonnement.  Voor meer informatie over specifieke opdrachtregelparameters en -opties kunt u de online Help van de opdrachtregelinterface raadplegen door `azure <command> <subcommand> --help` of `azure help <command> <subcommand>` te typen.

> [!NOTE]
> Deze voorbeelden bevatten geen sjabloonbewerkingen die over het algemeen worden aanbevolen voor VM-implementaties in Resource Manager. Meer informatie vindt u in [Use the Azure CLI to manage Azure resources and resource groups](../articles/xplat-cli-azure-resource-manager.md) (Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen) en [Een virtuele Linux-machine maken met behulp van een Azure Resource Manager-sjabloon](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

| Taak | Resource Manager |
| --- | --- | --- |
| Een basis-VM maken |`azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Bepaal de `image-urn` met behulp van de opdracht `azure vm image list`. Zie [dit artikel](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor voorbeelden.) |
| Een Linux-VM maken |`azure  vm create [options] <resource-group> <name> <location> -y "Linux"` |
| Een Windows-VM maken |`azure  vm create [options] <resource-group> <name> <location> -y "Windows"` |
| Lijst met VM's opvragen |`azure  vm list [options]` |
| Informatie over een VM ophalen |`azure  vm show [options] <resource_group> <name>` |
| Een VM starten |`azure vm start [options] <resource_group> <name>` |
| Een VM stoppen |`azure vm stop [options] <resource_group> <name>` |
| De toewijzing van een VM ongedaan maken |`azure vm deallocate [options] <resource-group> <name>` |
| Een VM opnieuw opstarten |`azure vm restart [options] <resource_group> <name>` |
| Een VM verwijderen |`azure vm delete [options] <resource_group> <name>` |
| Een virtuele machine vastleggen |`azure vm capture [options] <resource_group> <name>` |
| Een VM maken van een gebruikersinstallatiekopie |`azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>` |
| Een VM maken van een gespecialiseerde schijf |`azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>` |
| Een gegevensschijf toevoegen aan een VM |`azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]` |
| Een gegevensschijf verwijderen van een VM |`azure  vm disk detach [options] <resource-group> <vm-name> <lun>` |
| Een algemene extensie toevoegen aan een VM |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>` |
| VM Access-extensie toevoegen aan een VM |`azure vm reset-access [options] <resource-group> <name>` |
| Docker-extensie toevoegen aan een VM |`azure  vm docker create [options] <resource-group> <name> <location> <os-type>` |
| Een VM-extensie verwijderen |`azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Verbruik van VM-resources opvragen |`azure vm list-usage [options] <location>` |
| Alle beschikbare VM-grootten opvragen |`azure vm sizes [options]` |

## <a name="next-steps"></a>Volgende stappen
* Zie [Using the Azure CLI with Azure Resource Manager](../articles/virtual-machines/azure-cli-arm-commands.md) (Azure CLI gebruiken met Azure Resource Manager) voor meer voorbeelden van geavanceerde CLI-opdrachten voor VM-beheer.
