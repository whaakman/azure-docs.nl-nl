


Dit onderwerp wordt beschreven hoe:

* Gegevens invoeren in Azure een virtuele machine (VM), wanneer deze wordt ingericht.
* Deze ophalen voor Windows- en Linux.
* Speciale hulpprogramma's beschikbaar op sommige systemen gebruiken om te detecteren en aangepaste gegevens automatisch te verwerken.

> [!NOTE]
> Dit artikel wordt beschreven hoe aangepaste gegevens met behulp van een virtuele machine gemaakt met de Azure Service Management API kan worden ingevoegd. Zie voor het gebruik van de Azure Resource Management API [de voorbeeldsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Injecteren van aangepaste gegevens in uw virtuele machine van Azure
Deze functie is momenteel alleen ondersteund in de [Azure-opdrachtregelinterface](https://github.com/Azure/azure-xplat-cli). Hier maken we een `custom-data.txt` -bestand met onze gegevens vervolgens invoeren die in naar de virtuele machine tijdens het inrichten. Hoewel u een van de opties voor de `azure vm create` opdracht, de volgende demonstreert een zeer eenvoudige benadering:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Met behulp van aangepaste gegevens in de virtuele machine
* Als uw Azure VM een VM op basis van Windows is, wordt het bestand voor aangepaste gegevens wordt opgeslagen in `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Hoewel het base64-gecodeerd overzetten van de lokale computer naar de nieuwe virtuele machine, kan deze worden automatisch gedecodeerd en worden geopend of direct gebruikt.
  
  > [!NOTE]
  > Als het bestand bestaat, wordt deze overschreven. De beveiliging van de map is ingesteld op **System: volledig beheer** en **Administrators: volledig beheer**.
  > 
  > 
* Als uw Azure VM een VM op basis van Linux is, klikt u vervolgens het bestand voor aangepaste gegevens zich in een van de volgende locaties, afhankelijk van uw distro. De gegevens zijn mogelijk base64-gecodeerd, dus misschien moet u eerst de gegevens decoderen:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Cloud-init op Azure
Als uw Azure VM van een installatiekopie van een Ubuntu of virtuele CoreOS is, kunt u CustomData gebruiken om te verzenden van een cloud-config naar cloud init. Of als uw aangepaste gegevensbestand een script is, vervolgens cloud init kunt gewoon uitvoeren.

### <a name="ubuntu-cloud-images"></a>Ubuntu Cloud installatiekopieën
In de meeste Azure Linux-installatiekopieën, bewerkt u zou ' / etc/waagent.conf ' voor het configureren van de tijdelijke schijf en het wisselbestand. Zie [gebruikershandleiding voor Azure Linux Agent](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

Echter, op de Ubuntu Cloud-installatiekopieën, moet u cloud-init voor het configureren van de resource-schijf (dat wil zeggen, de 'kortstondige' schijf) en het wisselen van de partitie. Zie de volgende pagina op de wiki Ubuntu voor meer informatie: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Volgende stappen: met behulp van cloud-init
Zie voor meer informatie de [cloud init-documentatie voor Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Rol van Service Management REST API-verwijzing toevoegen](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure-opdrachtregelinterface](https://github.com/Azure/azure-xplat-cli)

