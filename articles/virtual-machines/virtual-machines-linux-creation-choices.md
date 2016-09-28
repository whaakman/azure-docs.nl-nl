<properties
    pageTitle="Verschillende manieren om een virtuele Linux-machine te maken | Microsoft Azure"
    description="Vermeldt de verschillende manieren om een virtuele Linux-machine te maken op Azure en maakt een koppeling naar hulpprogramma's en zelfstudies voor elke methode."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="iainfou"/>


# Verschillende manieren om een virtuele Linux-machine te maken met Resource Manager

Voor verschillende gebruikers en doelstellingen biedt Azure verschillende manieren om een virtuele machine te maken met behulp van het Resource Manager-implementatiemodel. Dit artikel bevat een overzicht van deze manieren en van de keuzes die u hebt wanneer u een virtuele Linux-machine wilt maken.

## Azure CLI 

De Azure CLI is op diverse platforms beschikbaar via een npm-pakket, door distributeurs geleverde pakketten of Docker-container. Meer informatie over het [installeren en configureren van de Azure CLI](../xplat-cli-install.md). In de volgende zelfstudies vindt u voorbeelden van het gebruik van de Azure CLI. Lees elk artikel voor meer informatie over de vermelde Quick Start-opdrachten voor CLI:

* [Een virtuele Linux-machine maken via Azure CLI voor ontwikkeling en tests](virtual-machines-linux-quick-create-cli.md)

    ```bash
    azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
    ```

* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

* [Een compleet nieuwe virtuele Linux-machine maken met behulp van de Azure CLI](virtual-machines-linux-create-cli-complete.md)

* [Een schijf toevoegen aan een virtuele Linux-machine](virtual-machines-linux-add-disk.md)

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM <size-in-GB>
    ```

## Azure-portal

De grafische gebruikersinterface van de [Azure-portal](https://portal.azure.com) biedt een eenvoudige manier om een virtuele machine uit te proberen, met name als u net begint met Azure, omdat u niets hoeft te installeren op uw systeem. De Azure-portal gebruiken om een virtuele machine te maken:

* [Een virtuele Linux-machine maken met behulp van de Azure-portal](virtual-machines-linux-quick-create-portal.md) 
* [Koppelen van een schijf met de Azure-portal](virtual-machines-linux-attach-disk-portal.md)

## Opties voor besturingssysteem en installatiekopieën
Bij het maken van een virtuele machine kiest u een installatiekopie op basis van het besturingssysteem dat u wilt uitvoeren. Azure en de diverse partners bieden een groot aantal installatiekopieën. Sommige hiervan bevatten toepassingen en hulpprogramma’s die vooraf zijn geïnstalleerd. U kunt ook een van uw eigen installatiekopieën uploaden (zie hieronder).

### Installatiekopieën van Azure
U kunt de CLI-opdrachten van `azure vm image` gebruiken om te zien wat er beschikbaar is per uitgever, distributierelease en build.

Beschikbare uitgevers vermelden:

```bash
azure vm image list-publishers --location WestUS
```

Beschikbare producten (aanbiedingen) voor een bepaalde uitgever vermelden:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Beschikbare SKU's (distributiereleases) van een bepaalde aanbieding vermelden:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Alle beschikbare installatiekopieën voor een bepaalde release vermelden:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Zie [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md) (Navigeren door en selecteren van installatiekopieën voor virtuele machines in Azure met de Azure CLI) voor meer voorbeelden van hoe u door de beschikbare installatiekopieën bladert en deze gebruikt.

De opdrachten `azure vm quick-create` en `azure vm create` hebben ook enkele aliassen waarmee u snel toegang kunt krijgen tot de meest voorkomende distributies en hun meest recente versies. Dit is gemakkelijker dan het telkens opgeven van de uitgever, aanbieding, SKU en versie wanneer u een virtuele machine maakt:

| Alias     | Uitgever | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiel      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | RedHat    | RHEL         | 7.2         | meest recente  |
| SLES      | SLES      | SLES         | 12-SP1      | meest recente  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | meest recente  |

### Uw eigen installatiekopie gebruiken

Als u specifieke aanpassingen nodig hebt, kunt u een installatiekopie gebruiken op basis van een bestaande virtuele machine in Azure, door de betreffende virtuele machine *vast te leggen*. U kunt ook uw eigen installatiekopie uploaden die is opgeslagen op een virtuele vaste schijf (VHD). Raadpleeg de volgende artikelen voor meer informatie over ondersteunde distributies en over hoe u uw eigen installatiekopieën kunt gebruiken:

* [Door Azure goedgekeurde distributies](virtual-machines-linux-endorsed-distros.md)

* [Informatie over niet-goedgekeurde distributies](virtual-machines-linux-create-upload-generic.md)

* [Een virtuele Linux-machine vastleggen als Resource Manager-sjabloon](virtual-machines-linux-capture-image.md). Quick Start-opdrachten:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## Volgende stappen

* Probeer een van de zelfstudies voor het maken van een virtuele Linux-machine via de [portal](virtual-machines-linux-quick-create-portal.md), met de [CLI](virtual-machines-linux-quick-create-cli.md) of met behulp van een Azure Resource Manager-[sjabloon](virtual-machines-linux-cli-deploy-templates.md).

* Nadat u een virtuele Linux-machine hebt gemaakt, kunt u gemakkelijk [een gegevensschijf toevoegen](virtual-machines-linux-add-disk.md).

* Snelle stappen om [een wachtwoord of SSH-sleutels opnieuw in te stellen +en gebruikers te beheren](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Sep16_HO3-->


