<properties
   pageTitle="Een virtuele Linux-machine in Azure maken met de CLI | Microsoft Azure"
   description="Maak een virtuele Linux-machine in Azure met de CLI."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/08/2016"
   ms.author="v-livech"/>



# Een virtuele Linux-machine in Azure maken met de CLI

In dit artikel ziet u hoe u in Azure snel een virtuele Linux-machine (VM) kunt implementeren met behulp van de opdracht `azure vm quick-create` in de opdrachtregelinterface (CLI) van Azure. Met de opdracht `quick-create` wordt een virtuele machine binnen een beveiligde basisinfrastructuur geïmplementeerd. Deze virtuele machine kunt u gebruiken als prototype of om snel een concept te testen. Het artikel schrijft het volgende als vereiste voor:

- een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))

- de [Azure-CLI](../xplat-cli-install.md) die is aangemeld bij `azure login`

- de Azure-CLI _moet_ in de Azure Resource Manager-modus staan. `azure config mode arm`

U kunt een virtuele Linux-machine ook snel implementeren met behulp van de [Azure-portal](virtual-machines-linux-quick-create-portal.md).

## Snelle opdrachten

In het volgende voorbeeld ziet u hoe u een virtuele CoreOS-machine implementeert en uw SSH-sleutel (Secure Shell) koppelt (uw argumenten kunnen afwijken):

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

De uitleg in de volgende secties heeft betrekking op de opdracht en de vereisten ervan voor Ubuntu Server 14.04 TNS als de Linux-distributie.  

## Quick-create aliassen voor VM

Als u snel een distributie wilt kiezen, kunt u de Azure CLI-aliassen gebruiken die aan de meest voorkomende OS-distributies zijn toegewezen. De volgende tabel bevat de aliassen (vanaf Azure-CLI versie 0.10). Alle implementaties die gebruikmaken van `quick-create` zijn standaard virtuele machines die worden ondersteund door SSD-opslag (Solid-State Drive). Dit staat garant voor snelle inrichting en snelle toegang tot de schijf. (Deze aliassen vertegenwoordigen een klein gedeelte van de beschikbare distributies in Azure. Meer installatiekopieën vindt u in de Azure Marketplace door te [zoeken naar een installatiekopie](virtual-machines-linux-cli-ps-findimage.md), maar u kunt ook [uw eigen aangepaste installatiekopie uploaden](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Uitgever | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiel      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | Red Hat    | RHEL         | 7.2         | meest recente  |
| UbuntuLTS | Canonical | Ubuntu Server | 14.04.4-LTS | meest recente  |

In de volgende secties wordt de `UbuntuLTS`-alias voor de optie **ImageURN** gebruikt (`-Q`) om een Ubuntu 14.04.4 LTS Server te implementeren.

## Gedetailleerd overzicht

In het vorige voorbeeld van `quick-create` wordt alleen de vlag `-M` gebruikt om de openbare SSH-sleutel te identificeren die moet worden geüpload. Ondertussen worden SSH-wachtwoorden uitgeschakeld. U wordt daarom gevraagd naar de volgende argumenten:

- de naam van de resourcegroep (voor een eerste Azure-resourcegroep is doorgaans elke willekeurige tekenreeks geschikt )
- VM-naam
- locatie (`westus` of `westeurope` zijn goede standaardwaarden)
- Linux (om in Azure aan te geven welk besturingssysteem u wilt)
- gebruikersnaam

In het volgende voorbeeld worden alle waarden opgegeven, zodat er geen verdere vragen worden gesteld. Als u een `~/.ssh/id_rsa.pub` als openbaar-sleutelbestand met ssh-rsa-indeling hebt, werkt alles:

```bash
azure vm quick-create \
-g exampleResourceGroup \
-n exampleVMName \
-l westus \
-y Linux \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub \
-Q UbuntuLTS
```

Uw uitvoer moet eruitzien als in het volgende uitvoerblok:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "exampleVMName"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "exampleVMName"
+ Looking up the VM "exampleVMName"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :exampleVMName
data:      User Name                     :exampleAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Meld u met het openbare IP-adres dat in de uitvoer staat vermeld aan bij uw virtuele machine. U kunt ook de volledig gekwalificeerde domeinnaam (FQDN) gebruiken die wordt vermeld:

```bash
ssh -i ~/.ssh/id_rsa.pub exampleAdminUser@138.91.247.29
```

De aanmeldingsprocedure moet er ongeveer als het volgende uitvoerblok uitzien:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

exampleAdminUser@exampleVMName:~$
```

## Volgende stappen

De opdracht `azure vm quick-create` is een goede manier om snel een virtuele machine te implementeren, zodat u zich kunt aanmelden bij een bash-shell en aan de slag kunt gaan. Wanneer u `vm quick-create` gebruikt, hebt u echter geen uitgebreide controle en kunt u geen complexere omgeving maken.  Als u een virtuele Linux-machine wilt implementeren die is afgestemd op uw infrastructuur, volg dan de instructies in een van deze artikelen:

- [Een Azure Resource Manager-sjabloon gebruiken om een specifieke implementatie te maken](virtual-machines-linux-cli-deploy-templates.md)
- [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](virtual-machines-linux-create-cli-complete.md)
- [Een met SSH beveiligde virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

U kunt ook [het Azure-stuurprogramma `docker-machine` gebruiken dat verschillende opdrachten heeft om snel een Linux-VM als een docker host](virtual-machines-linux-docker-machine.md) te maken.



<!--HONumber=Sep16_HO5-->


