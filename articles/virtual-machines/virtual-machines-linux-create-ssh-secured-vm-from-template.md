<properties
    pageTitle="Een virtuele Linux-machine maken met een Azure-sjabloon | Microsoft Azure"
    description="Een virtuele Linux-machine in Azure maken met behulp van een Azure Resource Manager-sjabloon."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="v-livech"/>


# Een virtuele Linux-machine maken met een Azure-sjabloon

Dit artikel laat zien hoe u in Azure snel een virtuele Linux-machine kunt implementeren met behulp van een Azure-sjabloon.  Als u dit artikel wilt lezen, moet u een Azure-account hebben ([registreer u voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)] en moet de [Azure CLI](../xplat-cli-install.md) zijn aangemeld (`azure login`) en zijn ingesteld op de Resource Manager-modus (`azure config mode arm`).  U kunt een virtuele Linux-machine ook snel implementeren met behulp van [Azure Portal](virtual-machines-linux-quick-create-portal.md) of de [Azure CLI](virtual-machines-linux-quick-create-cli.md).

## Beknopt overzicht van opdrachten

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Gedetailleerd overzicht

Met sjablonen kunt u in Azure virtuele machines maken met instellingen die u tijdens het starten wilt aanpassen, zoals gebruikersnamen en hostnamen. Voor dit artikel starten we een Azure-sjabloon waarbij we gebruikmaken van een Ubuntu VM en een netwerkbeveiligingsgroep (NSG) waarbij poort 22 is geopend voor SSH .

Azure Resource Manager-sjablonen zijn JSON-bestanden die kunnen worden gebruikt voor eenvoudige eenmalige taken, zoals het starten van een Ubuntu VM zoals in dit artikel wordt gedaan.  Azure-sjablonen kunnen ook worden gebruikt om complexe Azure-configuraties te maken van volledige omgevingen, zoals een test-, ontwikkelings- of productie-implementatiestack.

## De virtuele Linux-machine maken

De volgende voorbeeldcode laat zien hoe u `azure group create` aanroept om een resourcegroep te maken en op hetzelfde moment een met SSH beveiligde virtuele Linux-machine te implementeren met behulp van [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Vergeet niet om voor de implementatie namen te gebruiken die uniek zijn voor uw omgeving. In dit voorbeeld wordt `quicksecuretemplate` als de naam van de resourcegroep gebruikt, `securelinux` als de naam van de virtuele machine en `quicksecurelinux` als de subdomeinnaam.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Uitvoer

```bash
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== vlivech@azure
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

In dat voorbeeld werd een virtuele machine geïmplementeerd met de parameter `--template-uri`.  U kunt ook een sjabloon downloaden of lokaal een sjabloon maken en deze sjabloon vervolgens doorgeven met behulp van de parameter `--template-file`. Hierbij neemt u het pad naar het sjabloonbestand op als argument. De Azure CLI vraagt u om de parameters die vereist zijn voor de sjabloon.

## Volgende stappen

Doorzoek de [Sjablonengalerie](https://azure.microsoft.com/documentation/templates/) om te bekijken welke app-frameworks u hierna wilt gaan implementeren.



<!--HONumber=Sep16_HO3-->


