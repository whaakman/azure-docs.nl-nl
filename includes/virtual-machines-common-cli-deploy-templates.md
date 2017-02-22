
* [Snel een virtuele machine maken in Azure](#quick-create-a-vm-in-azure)
* [Een virtuele machine vanuit een sjabloon implementeren in Azure](#deploy-a-vm-in-azure-from-a-template)
* [Een virtuele machine vanuit een aangepaste installatiekopie maken](#create-a-custom-vm-image)
* [Een virtuele machine implementeren die gebruikmaakt van een virtueel netwerk en een load balancer](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Een resourcegroep verwijderen](#remove-a-resource-group)
* [Het logboek voor de implementatie van een resourcegroep weergeven](#show-the-log-for-a-resource-group-deployment)
* [Informatie weergeven over een virtuele machine](#display-information-about-a-virtual-machine)
* [Verbinding maken met een virtuele machine op basis van Linux](#log-on-to-a-linux-based-virtual-machine)
* [Een virtuele machine stoppen](#stop-a-virtual-machine)
* [Een virtuele machine starten](#start-a-virtual-machine)
* [Een gegevensschijf koppelen](#attach-a-data-disk)

## <a name="getting-ready"></a>Voorbereiding
Voordat u de Azure CLI met Azure-resourcegroepen kunt gebruiken, moet u de juiste Azure CLI-versie en een Azure-account hebben. Als u niet beschikt over de Azure CLI, moet u deze [installeren](../articles/xplat-cli-install.md).

### <a name="update-your-azure-cli-version-to-090-or-later"></a>Werk uw versie van Azure CLI bij naar 0.9.0 of hoger
Type `azure --version` om te zien of u versie 0.9.0 of hoger al hebt geïnstalleerd.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Als uw versie niet 0.9.0 of hoger is, moet u de CLI bijwerken via een van de native installatieprogramma's of via **npm** door `npm update -g azure-cli` te typen.

U kunt Azure CLI ook als een Docker-container uitvoeren met behulp van de volgende [Docker-installatiekopie](https://registry.hub.docker.com/u/microsoft/azure-cli/). Voer de volgende opdracht uit vanaf een Docker-host:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Uw Azure-account en -abonnement instellen
Als u nog geen Azure-abonnement hebt, maar wel een MSDM-abonnement, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). U kunt zich ook aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

[Meld u nu interactief aan bij uw Azure-account](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) door `azure login` te typen en de aanwijzingen voor een interactieve aanmeldingservaring bij uw Azure-account te volgen. 

> [!NOTE]
> Als u een werk- of school-ID hebt en weet dat u geen tweeledige verificatie hebt ingeschakeld, kunt u **ook** `azure login -u` gebruiken samen met de werk- of school-ID om u aan te melden *zonder* een interactieve sessie. Als u geen werk- of school-ID hebt, kunt u [een werk- of school-ID maken via uw persoonlijke Microsoft-account](../articles/virtual-machines/virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om u op dezelfde manier aan te melden.
>
>

Uw account kan meer dan één abonnement hebben. U kunt uw abonnementen weergeven door `azure account list` te typen. Dit ziet er mogelijk als volgt uit:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

U kunt het huidige Azure-abonnement instellen door het volgende te typen. Gebruik de naam van het abonnement of de ID waarin zich de resources bevinden die u wilt beheren.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-to-the-azure-cli-resource-group-mode"></a>Overschakelen naar de modus voor Azure CLI-resourcegroep
De Azure CLI wordt standaard gestart in de modus servicebeheer (**asm**-modus). Typ het volgende om over te schakelen naar de resourcegroepmodus.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Informatie over resourcesjablonen en resourcegroepen in Azure
De meeste toepassingen worden gebouwd met een combinatie van verschillende resourcetypen (zoals een of meer virtuele machines en opslagaccounts, een SQL-database, een virtueel netwerk of een netwerk voor contentlevering). De standaard Azure Service Management-API en de klassieke Azure-portal gaven deze items weer met een service-by-service-benadering. Met deze aanpak moet u services afzonderlijk implementeren en beheren (of andere hulpprogramma's installeren die dit doen), en niet als één logische implementatie-eenheid.

*Azure Resource Manager-sjablonen* maken het echter mogelijk om deze verschillende resources op een declaratieve manier te implementeren en te beheren als één logische implementatie-eenheid. U hoeft Azure niet meer met afzonderlijke opdrachten te vertellen wat er moet worden geïmplementeerd, maar beschrijft uw volledige implementatie in een JSON-bestand met alle resources en bijbehorende configuratie- en implementatieparameters. U geeft Azure daarna de instructie om al deze resources als een groep te implementeren.

Vervolgens kunt u de volledige levenscyclus van de resources in de groep beheren met behulp van opdrachten voor Azure CLI-resourcemanagement. U kunt dan:

* Alle resources in de groep tegelijkertijd stoppen, starten of verwijderen.
* Regels voor op rollen gebaseerd toegangsbeheer (RBAC) toepassen om er beveiligingsmachtigingen op te vergrendelen.
* Controlebewerkingen uitvoeren.
* Aanvullende metagegevens toevoegen aan resources om ze beter bij te houden.

Uitgebreide informatie over Azure-resourcegroepen en over wat ze voor u kunnen betekenen, vindt u in het [Overzicht van Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md). Als u geïnteresseerd bent in het ontwerpen van sjablonen, raadpleegt u [Azure Resource Manager-sjablonen maken](../articles/resource-group-authoring-templates.md).

## <a name="a-idquick-create-a-vm-in-azureatask-quick-create-a-vm-in-azure"></a><a id="quick-create-a-vm-in-azure"></a>Taak: Snel een virtuele machine maken in Azure
Soms weet u welke installatiekopie u nodig hebt en hebt u direct een virtuele machine van die installatiekopie nodig. De infrastructuur is niet zo belangrijk; wellicht moet u gewoon snel iets testen op een nieuwe virtuele machine. In dit geval kunt u de opdracht `azure vm quick-create` gebruiken en de benodigde argumenten doorgeven om een virtuele machine en de bijbehorende infrastructuur te maken.

Eerst maakt u een resourcegroep.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Daarna hebt u een installatiekopie nodig. Zie [Naar installatiekopieën van virtuele machine navigeren en deze selecteren met PowerShell en de Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) om een installatiekopie te vinden met de Azure CLI. Voor dit artikel vindt u hier een korte lijst met populaire installatiekopieën. Voor deze quick-create gebruiken we de installatiekopie Stable van CoreOS.

> [!NOTE]
> Voor ComputeImageVersion kunt u ook gewoon 'latest' opgeven als de parameter in de sjabloontaal en in de Azure CLI. Zo kunt u altijd de meest recente versie van de installatiekopie met de nieuwste patches gebruiken zonder dat u de scripts of sjablonen hoeft te wijzigen. Dit wordt hieronder weergegeven.
>
>

| PublisherName | Aanbieding | Sku | Versie |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Bèta |647.0.0 |
| CoreOS |CoreOS |Stabiel |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standard |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

U maakt uw virtuele machine door de opdracht `azure vm quick-create` in te voeren en te wachten op de vragen. Het ziet er ongeveer als volgt uit:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up the VM "coreos"
info:    Using the VM Size "Standard_A1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in the region "westus", trying to create new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up the storage account cli9fd3fce49e9a9b3d14302
+ Looking up the NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
+ Looking up the subnet "coreo-westu-1430261891570-snet" under the virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up the VM "coreos"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

U kunt nu verdergaan met de nieuwe virtuele machine.

## <a name="a-iddeploy-a-vm-in-azure-from-a-templateatask-deploy-a-vm-in-azure-from-a-template"></a><a id="deploy-a-vm-in-azure-from-a-template"></a>Taak: Een virtuele machine in Azure implementeren vanuit een sjabloon
Volg de instructies in dit gedeelte om met de Azure CLI een nieuwe virtuele Azure-machine te implementeren op basis van een sjabloon. Deze sjabloon maakt een enkele virtuele machine in een nieuw virtueel netwerk met één subnet en stelt u, in tegenstelling tot `azure vm quick-create`, in staat om precies te beschrijven wat u wilt en het proces te herhalen zonder fouten. Dit is wat deze sjabloon maakt:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-the-json-file-for-the-template-parameters"></a>Stap 1: controleer het JSON-bestand voor de sjabloonparameters
Hier vindt u de inhoud van het JSON-bestand voor de sjabloon. (De sjabloon bevindt zich ook in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)

Sjablonen zijn flexibel, dus de ontwerper kan u veel parameters geven of slechts een paar om een vastere sjabloon te maken. U moet de sjabloon doorgeven als parameters om de informatie te verzamelen die u nodig hebt. Open het sjabloonbestand (in dit onderwerp gebruikt u het inlinesjabloon hieronder) en bekijk de **parameterwaarden**.

In dit geval vraagt de onderstaande sjabloon om:

* Een unieke naam voor het opslagaccount.
* Ee beheerdersnaam voor de virtuele machine.
* Een wachtwoord.
* Een domeinnaam die externe gebruikers kunnen gebruiken.
* Een versienummer van Ubuntu Server. Er wordt slechts één nummer uit een lijst geaccepteerd.

Zie meer informatie over [vereisten voor gebruikersnaam en wachtwoord](../articles/virtual-machines/virtual-machines-linux-faq.md#what-are-the-username-requirements-when-creating-a-vm).

Wanneer u de te gebruiken waarden hebt bepaald, bent u klaar om een groep te maken en deze sjabloon te implementeren in uw Azure-abonnement.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the storage account where the virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for the virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for the virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the public IP used to access the virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-the-virtual-machine-by-using-the-template"></a>Stap 2: maak de virtuele machine met de sjabloon
Als uw parameterwaarden definitief zijn, moet u een resourcegroep maken voor de sjabloonimplementatie en vervolgens de sjabloon implementeren.

Typ voor het maken van de resourcegroep `azure group create <group name> <location>` met de naam van de gewenste groep en de locatie van het datacenter waarin u de implementatie wilt uitvoeren. Dit gebeurt snel:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Maak vervolgens de implementatie door `azure group deployment create` aan te roepen en door te geven:

* Het sjabloonbestand (als u de bovenstaande JSON-sjabloon hebt opgeslagen in een lokaal bestand).
* Een URI-sjabloon (als u wilt verwijzen naar het bestand in GitHub of een ander webadres).
* De resourcegroep waarnaar u wilt implementeren.
* Een optionele implementatienaam.

U wordt gevraagd de waarden van de parameters in het gedeelte 'parameters' van het JSON-bestand op te geven. Wanneer u de parameterwaarden hebt opgegeven, begint uw implementatie.

Hier volgt een voorbeeld:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

U ontvangt het volgende type informatie:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a name="a-idcreate-a-custom-vm-imageatask-create-a-custom-vm-image"></a><a id="create-a-custom-vm-image"></a>Taak: Een aangepaste VM-installatiekopie maken
U hebt het basisgebruik van sjablonen hierboven al gezien, zodat we soortgelijke instructies kunnen gebruiken om via de Azure CLI met behulp van een sjabloon een aangepaste virtuele machine te maken van een specifiek .VHD-bestand in Azure. Het verschil is dat deze sjabloon één virtuele machine maakt vanaf een opgegeven virtuele harde schijf (VHD).

### <a name="step-1-examine-the-json-file-for-the-template"></a>Stap 1: controleer het JSON-bestand voor de sjabloon
Hier vindt u de inhoud van het JSON-bestand voor de sjabloon die in dit gedeelte als voorbeeld wordt gebruikt. (De sjabloon bevindt zich ook in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)

Ook hier moet u de waarden bepalen die u wilt opgeven voor de parameters waarvoor geen standaardwaarden zijn. Bij het uitvoeren van de opdracht `azure group deployment create` vraagt de Azure CLI u om de waarden in te voeren.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-the-vhd"></a>Stap 2: de VHD verkrijgen
Uiteraard hebt u hier een .VHD-bestand voor nodig. U kunt een bestand gebruiken dat zich al in Azure bevindt, maar u kunt er ook een uploaden.

Zie [Een Windows Server-VHD voor Azure maken en uploaden](../articles/virtual-machines/virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) voor een op Windows gebaseerde virtuele machine.

Zie [Een virtuele harde schijf met het Linux-besturingssysteem maken en uploaden](../articles/virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) voor een op Linux gebaseerde virtuele machine.

### <a name="step-3-create-the-virtual-machine-by-using-the-template"></a>Stap 3: maak de virtuele machine met de sjabloon
U bent nu klaar om de nieuwe virtuele machine te maken op basis van het .VHD-bestand. Maak de groep waarin u de implementatie wilt uitvoeren. Dit doet u met `azure group create <location>`:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Maak vervolgens de implementatie met behulp van de optie `--template-uri` om de sjabloon rechtstreeks aan te roepen (of gebruik de optie `--template-file` om een bestand te gebruiken dat u lokaal hebt opgeslagen). U wordt slechts om enkele items gevraagd, omdat de sjabloon standaardinstellingen bevat. Als u de sjabloon op verschillende plaatsen implementeert, kan het gebeuren dat er naamconflicten optreden bij de standaardwaarden (met name de DNS-naam die u maakt).

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

De uitvoer ziet er ongeveer als volgt uit:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a name="a-iddeploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balanceratask-deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Taak: Een multi-VM-toepassing implementeren die gebruikmaakt van een virtueel netwerk en een externe load balancer
Met deze sjabloon kunt u twee virtuele machines onder een load balancer maken en een taakverdelingsregel configureren op poort 80. Deze sjabloon implementeert ook een opslagaccount, een virtueel netwerk, een openbaar IP-adres, een beschikbaarheidsset en netwerkinterfaces.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

Volg deze stappen om een multi-VM-toepassing die gebruikmaakt van een virtueel netwerk en een load balancer, te implementeren met een Resource Manager-sjabloon in de GitHub-opslagplaats voor sjablonen via Azure PowerShell-opdrachten.

### <a name="step-1-examine-the-json-file-for-the-template"></a>Stap 1: controleer het JSON-bestand voor de sjabloon
Hier vindt u de inhoud van het JSON-bestand voor de sjabloon. Als u de meest recente versie wilt gebruiken, kunt u deze vinden in de [Github-opslagplaats voor sjablonen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). In dit onderwerp wordt de switch `--template-uri` gebruikt om de sjabloon aan te roepen, maar u kunt ook de switch `--template-file` gebruiken om over te schakelen op een lokale versie.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix to use for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of the VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-the-deployment-by-using-the-template"></a>Stap 2: voer de implementatie uit met de sjabloon
Maak een resourcegroep voor de sjabloon met `azure group create <location>`. Maak vervolgens met `azure group deployment create` een implementatie in die resourcegroep, en geef de resourcegroep en de implementatienaam door. Beantwoord vervolgens de vragen voor parameters in de sjabloon die geen standaardwaarden hebben.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Gebruik nu de opdracht `azure group deployment create` en de optie `--template-uri` om de sjabloon te implementeren. Geef uw parameterwaarden op wanneer hierom wordt gevraagd, zoals hieronder weergegeven.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment to complete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Met deze sjabloon implementeert u een installatiekopie van Windows Server. Deze kan echter eenvoudig worden vervangen door een installatiekopie van Linux. Wilt u een Docker-cluster maken met meerdere swarm-managers? [Ook dat is mogelijk](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a name="a-idremove-a-resource-groupatask-remove-a-resource-group"></a><a id="remove-a-resource-group"></a>Taak: Een resourcegroep verwijderen
U kunt een implementatie opnieuw uitvoeren in een resourcegroep. Als u hier klaar mee bent, kunt u deze verwijderen met `azure group delete <group name>`.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a name="a-idshow-the-log-for-a-resource-group-deploymentatask-show-the-log-for-a-resource-group-deployment"></a><a id="show-the-log-for-a-resource-group-deployment"></a>Taak: Het logboek voor de implementatie van een resourcegroep weergeven
Dit is een veelvoorkomende bewerking wanneer u sjablonen maakt of gebruikt. De aanroep om de logboeken van de implementatie voor een groep weer te geven, is `azure group log show <groupname>`. Hiermee wordt veel informatie weergegeven die u helpt te begrijpen waarom iets wel of niet is gebeurd. (Zie [Veelvoorkomende fouten bij de Azure-implementatie oplossen met Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van problemen met uw implementaties, evenals andere informatie over problemen.)

Voor het oplossen van specifieke problemen kunt u bijvoorbeeld **jq** gebruiken om zaken nauwkeuriger te bekijken, zoals welke afzonderlijke fouten u moet oplossen. In het volgende voorbeeld wordt **jq** gebruikt om een implementatielogboek voor **lbgroup** te parseren en te zoeken naar fouten.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
U kunt zeer snel ontdekken wat er mis ging, het probleem herstellen en het opnieuw proberen. In het volgende geval heeft de sjabloon twee virtuele machines tegelijk gemaakt, waardoor er een vergrendeling op de .VHD is gemaakt. (Na wijziging van de sjabloon wordt de implementatie snel voltooid.)

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed to acquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a name="a-iddisplay-information-about-a-virtual-machineatask-display-information-about-a-virtual-machine"></a><a id="display-information-about-a-virtual-machine"></a>Taak: Informatie weergeven over een virtuele machine
U kunt informatie over specifieke virtuele machines in de resourcegroep bekijken met de opdracht `azure vm show <groupname> <vmname>`. Als uw groep meer dan één virtuele machine bevat, moet u de virtuele machines in uw groep mogelijk eerst weergeven met `azure vm list <groupname>`.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

Vervolgens zoekt u myVM1 op:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up the VM "myVM1"
+ Looking up the NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Als u de uitvoer van de consoleopdrachten programmatisch wilt opslaan en manipuleren, moet u mogelijk een hulpprogramma voor het parseren van JSON gebruiken, zoals ** [jq](https://github.com/stedolan/jq) ** of ** [jsawk](https://github.com/micha/jsawk)**. U kunt ook taalbibliotheken gebruiken die geschikt zijn voor de taak.
>
>

## <a name="a-idlog-on-to-a-linux-based-virtual-machineatask-log-on-to-a-linux-based-virtual-machine"></a><a id="log-on-to-a-linux-based-virtual-machine"></a>Taak: Verbinding maken met een virtuele machine op basis van Linux
Doorgaans zijn Linux-machines verbonden via SSH. Zie voor meer informatie [SSH gebruiken met Linux op Azure](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="a-idstop-a-virtual-machineatask-stop-a-vm"></a><a id="stop-a-virtual-machine"></a>Taak: Een virtuele machine stoppen
Voer deze opdracht uit:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Gebruik deze parameter om het virtuele IP-adres (VIP) van het VNet te behouden als het om de laatste virtuele machine in dat VNet gaat. <br><br> Als u de parameter `StayProvisioned` gebruikt, wordt de virtuele machine alsnog in rekening gebracht.
>
>

## <a name="a-idstart-a-virtual-machineatask-start-a-vm"></a><a id="start-a-virtual-machine"></a>Taak: Een virtuele machine starten
Voer deze opdracht uit:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a name="a-idattach-a-data-diskatask-attach-a-data-disk"></a><a id="attach-a-data-disk"></a>Taak: Een gegevensschijf koppelen
U moet ook beslissen of u een nieuwe schijf wilt koppelen of een schijf die al gegevens bevat. Voor een nieuwe schijf maakt de opdracht het .VHD-bestand en wordt dit in dezelfde opdracht gekoppeld.

Als u een nieuwe schijf wilt koppelen, voert u deze opdracht uit:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

Als u een bestaande gegevensschijf wilt koppelen, voert u deze opdracht uit:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Vervolgens koppelt u de schijf zoals u in Linux gewend bent.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van het gebruik van de Azure CLI met de modus **arm** [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md). Zie voor meer informatie over Azure-resources en de achterliggende concepten [Overzicht van Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).

Zie voor meer sjablonen die u kunt gebruiken, [Azure Quickstart-sjablonen](https://azure.microsoft.com/documentation/templates/) en [Toepassingsframeworks met sjablonen](../articles/virtual-machines/virtual-machines-linux-app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


<!--HONumber=Jan17_HO4-->


