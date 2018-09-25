


Dit artikel wordt beschreven hoe u:

* Wanneer deze wordt ingericht, kunt u gegevens invoeren in een Azure virtuele machine (VM).
* Deze ophalen voor zowel Windows als Linux.
* Speciale hulpprogramma's die beschikbaar op sommige systemen gebruiken om te detecteren en afhandelen van aangepaste gegevens automatisch.

> [!NOTE]
> Dit artikel wordt beschreven hoe aangepaste gegevens kunnen worden toegevoegd met behulp van een virtuele machine gemaakt met het klassieke implementatiemodel. Zie voor meer informatie over het gebruik van Azure Resource Management API, [de voorbeeldsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Aangepaste gegevens injecteren in uw Azure virtual machine

[!INCLUDE [classic-cli](contains-classic-cli-content.md)]

Deze functie is momenteel alleen ondersteund in de [Azure CLI](https://github.com/Azure/azure-xplat-cli). Hier maken we een `custom-data.txt` -bestand met gegevens van onze, klikt u vervolgens invoeren die in met de virtuele machine tijdens het inrichten. Hoewel u een van de opties voor de `azure vm create` opdracht, het volgende voorbeeld ziet u een eenvoudige benadering:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Met behulp van aangepaste gegevens in de virtuele machine
* Als uw Azure-VM een Windows-gebaseerde VM is, wordt het bestand met aangepaste wordt opgeslagen in `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Hoewel het base64-gecodeerd om over te dragen van de lokale computer naar de nieuwe virtuele machine, kan deze worden automatisch gedecodeerd en worden geopend of direct gebruikt.
  
  > [!NOTE]
  > Als het bestand bestaat, wordt deze overschreven. De beveiliging van de map is ingesteld op **System: volledig beheer** en **Administrators: volledig beheer**.
  > 
  > 
* Als uw Azure-VM een Linux-VM is, bevindt klikt u vervolgens het bestand met aangepaste zich in een van de volgende locaties, afhankelijk van uw distributie. De gegevens mogelijk base64-gecodeerd, dus misschien moet u eerst de gegevens decoderen:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Cloud-init op Azure
Als uw Azure-VM van een installatiekopie van een Ubuntu of CoreOS is, kunt u CustomData gebruiken voor het verzenden van een cloud-configuratie voor cloud-init. Of als uw aangepaste gegevensbestand een script is, cloud-init kunt uitvoeren.

### <a name="ubuntu-cloud-images"></a>Cloud Ubuntu-installatiekopieën
In de meeste Azure Linux-installatiekopieën die u wilt bewerken ' / etc/waagent.conf ' voor het configureren van de tijdelijke schijf en het wisselbestand. Zie [gebruikershandleiding voor Azure Linux Agent](../articles/virtual-machines/extensions/agent-linux.md) voor meer informatie.

Echter, in de Cloud Ubuntu-installatiekopieën moet u cloud-init om de resource-schijf (dat wil zeggen, de "kortstondige" schijf) te configureren en swap-partitie. Zie de volgende pagina op de Ubuntu-wiki voor meer informatie: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Volgende stappen: cloud-init gebruiken
Zie voor meer informatie de [cloud-init-documentatie voor Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Rol van Service Management REST API-verwijzing toevoegen](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure-opdrachtregelinterface](https://github.com/Azure/azure-xplat-cli)

