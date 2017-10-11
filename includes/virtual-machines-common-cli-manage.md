De Azure CLI 2.0 kunt u maken en beheren van uw Azure-resources op Mac OS-, Linux- en Windows. In dit artikel vindt u details van de meest voorkomende opdrachten voor het maken en beheren van virtuele machines (VM's).

Dit artikel is vereist voor de Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). U kunt ook [Cloud Shell](/azure/cloud-shell/quickstart) vanuit de browser.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Basisopdrachten van Azure Resource Manager in Azure CLI
Voor meer hulp bij specifieke opdrachtregelopties en opties, kunt u de opdracht online help-opties en door te typen `az <command> <subcommand> --help`.

### <a name="create-vms"></a>Virtuele machines maken
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Een resourcegroep maken | `az group create --name myResourceGroup --location eastus` |
| Een Linux-VM maken | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Een Windows-VM maken | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>Status virtuele machine beheren
| Taak | Azure CLI-opdrachten |
| --- | --- |
| Een VM starten | `az vm start --resource-group myResourceGroup --name myVM` |
| Een VM stoppen | `az vm stop --resource-group myResourceGroup --name myVM` |
| De toewijzing van een VM ongedaan maken | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Een VM opnieuw opstarten | `az vm restart --resource-group myResourceGroup --name myVM` |
| Een virtuele machine opnieuw implementeren | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| Een VM verwijderen | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>VM-info ophalen
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
| Afbeelding van een virtuele machine maken | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Virtuele machine van de installatiekopie maken | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van de CLI-opdrachten de [maken en beheren van virtuele Linux-machines met de Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md) zelfstudie.

