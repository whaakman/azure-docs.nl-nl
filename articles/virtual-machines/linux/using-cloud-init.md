---
title: Overzicht van cloud-init-ondersteuning voor virtuele Linux-machines in Azure | Microsoft Docs
description: Overzicht van cloud-init-mogelijkheden in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: c0a5e8695b712ca95952ea839fa829dab2c48824
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700091"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Cloud-init-ondersteuning voor virtuele machines in Azure
Dit artikel wordt uitgelegd voor de ondersteuning die er bestaat voor [cloud-init](https://cloudinit.readthedocs.io) het configureren van een virtuele machine (VM) of de virtuele machine-schaalsets (VMSS) inrichtingstijd in Azure. Deze cloud-init-scripts uitvoeren op de eerste keer opstarten nadat de resources zijn ingericht met Azure.  

## <a name="cloud-init-overview"></a>Overzicht van cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud-init gebruiken voor het installeren van pakketten en schrijven van bestanden, of om gebruikers en beveiliging te configureren. Omdat cloud-init wordt aangeroepen tijdens het opstartproces, zijn er geen extra stappen of agents vereist om toe te passen van uw configuratie.  Voor meer informatie over het juiste indeling uw `#cloud-config` bestanden, Zie de [cloud-init-documentatiesite](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` bestanden zijn tekstbestanden die zijn gecodeerd in base64.

Cloud-init werkt ook in distributies. U gebruikt bijvoorbeeld niet **apt-get install** of **yum install** om een pakket te installeren. In plaats daarvan kunt u een lijst definiëren met te installeren pakketten. Cloud-init maakt automatisch gebruik van het hulpprogramma voor systeemeigen pakketbeheer voor de distro die u selecteert.

 We zijn actief werkt met onze onderschreven Linux-distributie partners om cloud-init ingeschakeld installatiekopieën die beschikbaar zijn in de Azure marketplace. Deze installatiekopieën maken voor uw cloud-init-implementaties en configuraties werken naadloos met VM's en VM Scale Sets (VMSS). De volgende tabel geeft een overzicht van de huidige beschikbaarheid van de installatiekopieën van cloud-init ingeschakeld op het Azure-platform:

| Uitgever | Aanbieding | SKU | Versie | cloud-init gereed |
|:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |18.04-LTS |meest recente |ja | 
|Canonical |UbuntuServer |17.10 |meest recente |ja | 
|Canonical |UbuntuServer |16.04-LTS |meest recente |ja | 
|Canonical |UbuntuServer |14.04.5-LTS |meest recente |ja |
|CoreOS |CoreOS |Stabiel |meest recente |ja |
|OpenLogic |CentOS |7-CI |meest recente |preview |
|RedHat |RHEL |7-RAW-CI |meest recente |preview |

Azure Stack biedt momenteel geen ondersteuning voor het inrichten van RHEL 7.4 en CentOS 7.4 cloud-init gebruiken.

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Wat is het verschil tussen cloud-init- en Linux-Agent (WALA)?
WALA is een Azure-platform-specifieke-agent gebruikt voor het inrichten, configureren van virtuele machines en Azure-extensies verwerkt. We verbeteren van de taak van het configureren van virtuele machines voor het gebruik van cloud-init in plaats van de Linux-Agent kan het bestaande cloud-init-klanten om hun huidige cloud-init-scripts te gebruiken.  Als u bestaande investeringen in de cloud-init-scripts voor het configureren van Linux-systemen hebt, zijn er **geen extra instellingen vereist** in te schakelen. 

Als u niet de Azure CLI `--custom-data` switch op de tijd, WALA inrichting wordt de minimale Inrichtingsstatus van VM's vereiste parameters voor het inrichten van de virtuele machine en de implementatie met de standaardinstellingen te voltooien.  Als u verwijst naar de cloud-init `--custom-data` -switch, wat is opgenomen in uw aangepaste gegevens (afzonderlijke instellingen of volledige script) onderdrukt de standaardwaarden WALA. 

WALA configuraties van virtuele machines zijn tijd beperkt tot het werk binnen de maximale Inrichtingsstatus van VM's tijd.  Cloud-init-configuraties die zijn toegepast op VM's zijn geen beperkingen en leiden niet tot een implementatie mislukt door een time-out optreedt. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Virtuele Machine implementeren van een cloud-init ingeschakeld
Implementeren van een cloud-init ingeschakeld virtuele machine is net zo eenvoudig als die verwijst naar een cloud-init-ingeschakeld distributiepunt tijdens de implementatie.  Linux-distributie instaan hebben om te kiezen om te schakelen en cloud-init integreren in hun gepubliceerde basisinstallatiekopieën in Azure. Nadat u hebt vastgesteld dat de installatiekopie die u wilt implementeren cloud-init ingeschakeld is, kunt u de Azure CLI de installatiekopie te implementeren. 

De eerste stap bij het implementeren van deze afbeelding is het maken van een resourcegroep met de [az-groep maken](/cli/azure/group) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
De volgende stap is het maken van een bestand in uw huidige shell met de naam *cloud-init.txt* en plak de volgende configuratie. In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor cloud-init.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Druk op `ctrl-X` om af te sluiten van het bestand, typ `y` om op te slaan van het bestand en druk op `enter` ter bevestiging de naam van het bestand bij het afsluiten.

De laatste stap is het maken van een virtuele machine met de [az vm maken](/cli/azure/vm) opdracht. 

Het volgende voorbeeld wordt een virtuele machine met de naam *centos74* en SSH-sleutels gemaakt als deze niet al bestaan op een standaardsleutellocatie. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  Gebruik de `--custom-data`-parameter om door te geven in uw cloud-init-configuratiebestand. Geef het volledige pad naar *cloud-init.txt* op als u het bestand buiten uw huidige werkmap hebt opgeslagen. Het volgende voorbeeld wordt een virtuele machine met de naam *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Wanneer de virtuele machine is gemaakt, toont de Azure CLI informatie specifiek is voor uw implementatie. Noteer het `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.  Het duurt even voordat de virtuele machine wordt gemaakt, de pakketten te installeren en de app te starten. Er zijn achtergrondtaken die nog worden uitgevoerd nadat u door de Azure CLI bent teruggeleid naar de prompt. U kunt SSH in de virtuele machine en de stappen die worden beschreven in de sectie probleemoplossing gebruiken om de cloud-init-logboeken weer te geven. 

## <a name="troubleshooting-cloud-init"></a>Oplossen van problemen met cloud-init
Zodra de virtuele machine is ingericht, cloud-init wordt uitgevoerd via alle modules en scripts die zijn gedefinieerd in `--custom-data` om te configureren van de virtuele machine.  Als u nodig hebt om op te lossen fouten en weglatingen in de configuratie, moet u zoeken naar de modulenaam (`disk_setup` of `runcmd` bijvoorbeeld) in het logboek van cloud-init - bevindt zich in **/var/log/cloud-init.log**.

> [!NOTE]
> Niet elke-modulefout resulteert in een onherstelbare cloud-init algemene configuratie mislukt. Bijvoorbeeld, met behulp van de `runcmd` -module, als het script is mislukt, cloud-init nog steeds rapporteert inrichting geslaagd omdat de module runcmd uitgevoerd.

Raadpleeg voor meer informatie over cloud-init-logboekregistratie, het [cloud-init-documentatie](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor cloud-init voorbeelden van wijzigingen in de configuratie:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
