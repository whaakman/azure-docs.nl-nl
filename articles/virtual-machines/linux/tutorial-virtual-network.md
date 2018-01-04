---
title: Azure virtuele netwerken en virtuele Linux-Machines | Microsoft Docs
description: Zelfstudie - virtuele Azure-netwerken en virtuele Linux-Machines met de Azure CLI beheren
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0e7f4308290a14e592cf1739fa5b0b3360d7c68b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Beheren van virtuele Azure-netwerken en virtuele Linux-Machines met de Azure CLI

Azure netwerken virtuele Azure-machines gebruiken voor interne en externe communicatie. Deze zelfstudie wordt begeleid twee virtuele machines implementeren en configureren van Azure netwerken voor deze virtuele machines. De voorbeelden in deze zelfstudie wordt ervan uitgegaan dat de virtuele machines als host voor een webtoepassing met een database back-end optreden, maar een toepassing niet is geïmplementeerd in de zelfstudie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een virtueel netwerk en subnet maken
> * Een openbaar IP-adres maken
> * Een front-virtuele machine maken
> * Netwerkverkeer beveiligen
> * Maak een back-end-VM

Tijdens het voltooien van deze zelfstudie ziet u deze resources gemaakt:

![Virtueel netwerk met twee subnetten](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -het virtuele netwerk dat de virtuele machines gebruiken om te communiceren met elkaar en het internet.
- *myFrontendSubnet* -het subnet in *myVNet* die door de front-bronnen.
- *myPublicIPAddress* -het openbare IP-adres gebruikt voor toegang tot *myFrontendVM* vanaf internet.
- *myFrontentNic* -de netwerkinterface die wordt gebruikt door *myFrontendVM* om te communiceren met *myBackendVM*.
- *myFrontendVM* -de VM wordt gebruikt voor communicatie tussen het internet en *myBackendVM*.
- *myBackendNSG* -de netwerkbeveiligingsgroep waarmee de communicatie tussen de *myFrontendVM* en *myBackendVM*.
- *myBackendSubnet* -het subnet gekoppeld *myBackendNSG* en gebruikt door de back-end-resources.
- *myBackendNic* -de netwerkinterface die wordt gebruikt door *myBackendVM* om te communiceren met *myFrontendVM*.
- *myBackendVM* -de virtuele machine die gebruikmaakt van poort 22 en 3306 om te communiceren met *myFrontendVM*.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>Overzicht van VM-netwerken

Virtuele netwerken van Azure inschakelen beveiligde netwerkverbindingen tussen virtuele machines, internet en andere Azure-services zoals Azure SQL database. Virtuele netwerken zijn onderverdeeld in logische segmenten subnetten genoemd. Subnetten worden gebruikt voor Controlestroom netwerk, en als een beveiligingsgrens. Bij het implementeren van een virtuele machine, omvat het doorgaans de virtuele netwerkinterface, is gekoppeld aan een subnet.

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en subnet maken

Voor deze zelfstudie wordt één virtueel netwerk met twee subnetten gemaakt. Een front-end-subnet voor het hosten van een webtoepassing en een back-end-subnet voor het hosten van een database-server.

Voordat u een virtueel netwerk maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myRGNetwork* op de locatie eastus.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Virtueel netwerk maken

Gebruik de [az network vnet maken](/cli/azure/network/vnet#create) opdracht voor het maken van een virtueel netwerk. In dit voorbeeld wordt het netwerk is met de naam *mvVNet* en krijgt een adresvoorvoegsel van *10.0.0.0/16*. Een subnet wordt ook gemaakt met de naam *myFrontendSubnet* en het voorvoegsel *10.0.1.0/24*. Verderop in deze zelfstudie is een front-virtuele machine verbonden met dit subnet. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myFrontendSubnet \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Subnet maken

Een nieuw subnet is toegevoegd aan het virtuele netwerk met de [az network vnet subnet maken](/cli/azure/network/vnet/subnet#create) opdracht. In dit voorbeeld wordt het subnet heet *myBackendSubnet* en krijgt een adresvoorvoegsel van *10.0.2.0/24*. Dit subnet wordt gebruikt met alle back-end-services.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --address-prefix 10.0.2.0/24
```

Op dit moment is een netwerk gemaakt en gesegmenteerde in twee subnetten, één voor de front-end-services en een andere voor back-end-services. In de volgende sectie worden de virtuele machines gemaakt en verbonden met deze subnetten.

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Een openbaar IP-adres kan Azure-resources toegankelijk zijn op het internet. De toewijzingsmethode van het openbare IP-adres kan worden geconfigureerd als dynamische of statische. Een openbaar IP-adres wordt standaard dynamisch toegewezen. Dynamische IP-adressen worden vrijgegeven wanneer een virtuele machine toewijzing ongedaan wordt gemaakt. Dit gedrag zorgt ervoor dat het IP-adres moet wijzigen tijdens een bewerking met een VM toewijzing is opgeheven.

De toewijzingsmethode kan worden ingesteld op statische elektriciteit zorgt ervoor dat het IP-adres toegewezen aan een virtuele machine, zelfs tijdens de status van een toewijzing ongedaan is gemaakt blijft. Wanneer u een statisch toegewezen IP-adres, kan niet het IP-adres zelf worden opgegeven. Het wordt toegewezen uit een groep met beschikbare adressen.

```azurecli-interactive
az network public-ip create --resource-group myRGNetwork --name myPublicIPAddress
```

Bij het maken van een virtuele machine met de [az vm maken](/cli/azure/vm#create) opdracht, de standaard openbare IP-adres-toewijzingsmethode is dynamisch. Bij het maken van een virtuele machine met de [az vm maken](/cli/azure/vm#create) opdracht, bevatten de `--public-ip-address-allocation static` argument voor een statisch openbaar IP-adres toewijzen. Deze bewerking wordt niet gedemonstreerd in deze zelfstudie, maar in de volgende sectie een dynamisch toegewezen IP-adres wordt gewijzigd naar een statisch toegewezen adres. 

### <a name="change-allocation-method"></a>Toewijzingsmethode wijzigen

De toewijzingsmethode voor IP-adres kan worden gewijzigd met de [az netwerk openbare ip-update](/cli/azure/network/public-ip#update) opdracht. In dit voorbeeld wordt de toewijzingsmethode voor IP-adres van de front-virtuele machine in statisch gewijzigd.

Ten eerste toewijzing van de virtuele machine.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontendVM
```

Gebruik de [az netwerk openbare ip-update](/cli/azure/network/public-ip#update) opdracht om te werken de toewijzingsmethode. In dit geval de `--allocation-method` wordt ingesteld op *statische*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myPublicIPAddress --allocation-method static
```

Start de virtuele machine.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontendVM --no-wait
```

### <a name="no-public-ip-address"></a>Er is geen openbaar IP-adres

Een virtuele machine hoeft vaak niet toegankelijk zijn via internet. Voor het maken van een virtuele machine zonder een openbaar IP-adres gebruikt de `--public-ip-address ""` argument met een lege set dubbele aanhalingstekens. Deze configuratie wordt later in deze zelfstudie gedemonstreerd.

## <a name="create-a-front-end-vm"></a>Een front-virtuele machine maken

Gebruik de [az vm maken](/cli/azure/vm#create) opdracht voor het maken van de virtuele machine met de naam *myFrontendVM* met *myPublicIPAddress*.

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --vnet-name myVNet \
  --subnet myFrontendSubnet \
  --nsg myFrontendNSG \
  --public-ip-address myPublicIPAddress \
  --image UbuntuLTS \
  --generate-ssh-keys
```

## <a name="secure-network-traffic"></a>Netwerkverkeer beveiligen

Een netwerkbeveiligingsgroep (Network Security Group, NSG) bevat een lijst met beveiligingsregels waarmee netwerkverkeer naar resources die zijn verbonden met virtuele Azure-netwerken (VNet) wordt toegestaan of geweigerd. Nsg's kunnen worden gekoppeld aan subnetten of afzonderlijke netwerkinterfaces. Wanneer u een NSG is gekoppeld aan een netwerkinterface, geldt deze de bijbehorende virtuele machine. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. 

### <a name="network-security-group-rules"></a>Regels voor netwerkbeveiligingsgroepen

NSG-regels definiëren netwerken poorten waarover verkeer wordt toegestaan of geweigerd. De regels kunnen bron en doel-IP-adresbereiken bevatten, zodat verkeer wordt geregeld tussen specifieke systemen of subnetten. NSG-regels bevatten ook een prioriteit (tussen 1- en 4096). Regels worden geëvalueerd in de volgorde van prioriteit. Een regel met een prioriteit van 100 wordt geëvalueerd voor een regel met prioriteit 200.

Alle NSG's bevatten een set met standaardregels. De standaardregels kunnen niet worden verwijderd, maar omdat ze de laagste prioriteit hebben, kunnen ze worden overschreven door de regels die u maakt.

Standaardregels voor nsg's zijn:

- **Virtueel netwerk** - die afkomstig zijn van verkeer en eindigt in een virtueel netwerk is toegestaan beide kanten van binnenkomend en uitgaand.
- **Internet** - uitgaand verkeer is toegestaan, maar dat binnenkomend verkeer wordt geblokkeerd.
- **De load balancer** -toestaan dat de load balancer van Azure naar de status van uw VM's en rolinstanties. Als u een set met gelijke taakverdeling niet gebruikt, kunt u deze regel onderdrukken.

### <a name="create-network-security-groups"></a>Netwerk-beveiligingsgroepen maken

Een netwerkbeveiligingsgroep kan worden gemaakt op hetzelfde moment als een virtuele machine met de [az vm maken](/cli/azure/vm#create) opdracht. Wanneer doet, het NSG is gekoppeld aan de netwerkinterface van de virtuele machines en een regel voor het NSG wordt automatisch gemaakt voor verkeer op poort *22* van elke bron. Eerder in deze zelfstudie is het front-NSG automatisch wordt gemaakt met de front-virtuele machine. Een regel voor het NSG is ook automatisch gemaakt voor poort 22. 

In sommige gevallen kan het handig zijn vooraf maken een NSG, zoals wanneer SSH standaardregels mogen niet worden gemaakt of wanneer het NSG moet worden gekoppeld aan een subnet. 

Gebruik de [az netwerk nsg maken](/cli/azure/network/nsg#create) opdracht voor het maken van een netwerkbeveiligingsgroep.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myBackendNSG
```

In plaats van het NSG aan een netwerkinterface koppelen, is het gekoppeld aan een subnet. In deze configuratie is een virtuele machine die is gekoppeld aan het subnet neemt over het NSG-regels.

Bijwerken van de bestaande subnet met de naam *myBackendSubnet* met de nieuwe NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --network-security-group myBackendNSG
```

### <a name="secure-incoming-traffic"></a>Binnenkomend verkeer beveiligen

Wanneer de front-virtuele machine is gemaakt, is een regel voor het NSG gemaakt voor inkomend verkeer op poort 22. Deze regel kunnen de SSH-verbindingen met de virtuele machine. In dit voorbeeld moet ook verkeer op poort worden toegestaan *80*. Deze configuratie kunt een webtoepassing om te worden geopend op de virtuele machine.

Gebruik de [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) opdracht maakt u een regel voor poort *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myFrontendNSG \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

De front-virtuele machine is alleen toegankelijk is op poort *22* en poort *80*. Alle binnenkomend verkeer wordt geblokkeerd op de netwerkbeveiligingsgroep. Kan het handig zijn voor het visualiseren van de regel NSG configuraties. Retourneren van de configuratie van de regel NSG met de [az netwerk Regellijst](/cli/azure/network/nsg/rule#list) opdracht. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myFrontendNSG --output table
```

### <a name="secure-vm-to-vm-traffic"></a>Beveiligde virtuele machine naar VM-verkeer

Netwerkbeveiligingsgroepen kunnen ook tussen VM's toepassen. In dit voorbeeld de front-virtuele machine moet communiceren met de back-end-VM op poort *22* en *3306*. Deze configuratie zorgt ervoor dat SSH-verbindingen van de front-virtuele machine en ook toestaan dat een toepassing op de front-virtuele machine om te communiceren met een back-end MySQL-database. Al het andere verkeer moet tussen de front-end en back-end virtuele machines worden geblokkeerd.

Gebruik de [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) opdracht maakt u een regel voor poort 22. U ziet dat de `--source-address-prefix` geeft een waarde van argument *10.0.1.0/24*. Deze configuratie zorgt ervoor dat alleen verkeer vanuit het front-end-subnet dat door het NSG wordt toegestaan.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Nu een regel voor MySQL-verkeer op poort 3306 toevoegen.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Ten slotte omdat nsg's een standaardregel al het verkeer tussen virtuele machines in hetzelfde VNet hebben, kan een regel kan worden gemaakt voor de back-end nsg's om alle verkeer te blokkeren. Zoals u ziet hier de `--priority` krijgt een waarde van *300*, die is dat het NSG en de MySQL-regels te verlagen. Deze configuratie zorgt ervoor dat SSH en MySQL verkeer nog steeds door het NSG wordt toegestaan.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

## <a name="create-back-end-vm"></a>Back-end virtuele machine maken

Maak nu een virtuele machine, die is gekoppeld aan de *myBackendSubnet*. U ziet dat de `--nsg` argument heeft de waarde leeg dubbele aanhalingstekens. Een NSG hoeft niet te worden gemaakt met de virtuele machine. De virtuele machine is gekoppeld aan het subnet van de back-end dat is beveiligd met de vooraf gemaakte back-end-NSG. Deze NSG geldt voor de virtuele machine. U ziet ook hier die de `--public-ip-address` argument heeft de waarde leeg dubbele aanhalingstekens. Deze configuratie maakt u een virtuele machine zonder een openbaar IP-adres. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

De back-end-VM is alleen toegankelijk is op poort *22* en poort *3306* van de front-end-subnet. Alle binnenkomend verkeer wordt geblokkeerd op de netwerkbeveiligingsgroep. Kan het handig zijn voor het visualiseren van de regel NSG configuraties. Retourneren van de configuratie van de regel NSG met de [az netwerk Regellijst](/cli/azure/network/nsg/rule#list) opdracht. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myBackendNSG --output table
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt en beveiligd Azure-netwerken met betrekking tot virtuele machines. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtueel netwerk en subnet maken
> * Een openbaar IP-adres maken
> * Een front-virtuele machine maken
> * Netwerkverkeer beveiligen
> * Back-end virtuele machine maken

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van gegevens op virtuele machines met behulp van Azure backup. 

> [!div class="nextstepaction"]
> [Back-up van Linux virtuele machines in Azure](./tutorial-backup-vms.md)
