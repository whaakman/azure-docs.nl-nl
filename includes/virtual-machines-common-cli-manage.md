---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: e08e6ca4f6413b475bd8a0e5ff30b74921c2f0e6
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226427"
---
De Azure CLI kunt u maken en beheren van uw Azure-resources op macOS, Linux en Windows. Dit artikel worden enkele van de meest voorkomende opdrachten voor het maken en beheren van virtuele machines (VM's).

In dit artikel gebruikmaken van Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u upgraden wilt, raadpleegt u [Azure CLI installeren](/cli/azure/install-azure-cli). U kunt ook [Cloud Shell](/azure/cloud-shell/quickstart) vanuit uw browser.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Basisopdrachten van Azure Resource Manager in Azure CLI
Voor meer informatie over specifieke opdrachtregelparameters en opties, kunt u de opdracht online help-opties en door te typen `az <command> <subcommand> --help`.

### <a name="create-vms"></a>Virtuele machines maken
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Een resourcegroep maken | `az group create --name myResourceGroup --location eastus` |
| Een Linux-VM maken | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Een Windows-VM maken | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>Status van de virtuele machine beheren
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Een VM starten | `az vm start --resource-group myResourceGroup --name myVM` |
| Een VM stoppen | `az vm stop --resource-group myResourceGroup --name myVM` |
| De toewijzing van een VM ongedaan maken | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Een VM opnieuw opstarten | `az vm restart --resource-group myResourceGroup --name myVM` |
| Een virtuele machine opnieuw implementeren | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| Een VM verwijderen | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>VM ophalen
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Lijst met VM's opvragen | `az vm list` |
| Informatie over een VM ophalen | `az vm show --resource-group myResourceGroup --name myVM` |
| Verbruik van VM-resources opvragen | `az vm list-usage --location eastus` |
| Alle beschikbare VM-grootten opvragen | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Schijven en installatiekopieën
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Een gegevensschijf toevoegen aan een VM | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| Een gegevensschijf verwijderen van een VM | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| De grootte van een schijf wijzigen | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Een momentopname maken van een schijf | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Installatiekopie van een virtuele machine maken | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Virtuele machine maken van installatiekopie | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van de CLI-opdrachten de [maken en beheren van virtuele Linux-machines met de Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md) zelfstudie.

