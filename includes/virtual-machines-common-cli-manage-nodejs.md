U hebt een account van Azure nodig als u Azure CLI wilt gebruiken met opdrachten en sjablonen van Resource Manager voor het implementeren van Azure-resources en -werkbelastingen met behulp van resourcegroepen. Als u geen account hebt, kunt u [hier een gratis proefversie starten](https://azure.microsoft.com/pricing/free-trial/).

Als u Azure CLI nog niet hebt geïnstalleerd en gekoppeld aan uw abonnement, raadpleegt u [Azure CLI installeren](../articles/cli-install-nodejs.md), stelt u de modus in op `arm` met `azure config mode arm` en gebruikt u de opdracht `azure login` om verbinding te maken met Azure.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- Azure CLI 1.0: onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel (dit artikel)
- [Azure CLI 2.0](../articles/virtual-machines/linux/cli-manage.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Basisopdrachten van Azure Resource Manager in Azure CLI
In dit artikel worden de basisopdrachten behandeld die u kunt gebruiken met Azure CLI wilt voor het beheren en manipuleren van uw resources (voornamelijk VM's) in uw Azure-abonnement.  Voor meer informatie over specifieke opdrachtregelparameters en -opties kunt u de online Help van de opdrachtregelinterface raadplegen door `azure <command> <subcommand> --help` of `azure help <command> <subcommand>` te typen.

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
