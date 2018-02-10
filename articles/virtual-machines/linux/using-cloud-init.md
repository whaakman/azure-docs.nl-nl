---
title: Overzicht van cloud-init-ondersteuning voor virtuele Linux-machines in Azure | Microsoft Docs
description: Overzicht van cloud-init-mogelijkheden in Microsoft Azure
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 2d110705a86fa8bc05859bd8bfde34b0b5b11575
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Cloud-init-ondersteuning voor virtuele machines in Azure
Dit artikel wordt uitgelegd voor de ondersteuning die bestaat voor [cloud init](https://cloudinit.readthedocs.io) scale ingesteld voor het configureren van een virtuele machine (VM) of de virtuele machine (VMSS) bij het inrichten van de tijd in Azure. Deze cloud init-scripts worden uitgevoerd op de eerste keer wordt opgestart nadat de resources zijn ingericht met Azure.  

## <a name="cloud-init-overview"></a>Overzicht van cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud-init gebruiken voor het installeren van pakketten en schrijven van bestanden, of om gebruikers en beveiliging te configureren. Omdat cloud init wordt aangeroepen tijdens het opstartproces, zijn er geen extra stappen of de vereiste agents naar uw configuratie toe te passen.  Voor meer informatie over de juiste indeling uw `#cloud-config` bestanden, Zie de [documentatiesite voor cloud-init](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`bestanden zijn tekstbestanden gecodeerd in base64.

Cloud-init werkt ook via distributies. Bijvoorbeeld, u niet gebruikt **apt get-installatie** of **yum installeren** om een pakket te installeren. In plaats daarvan kunt u een lijst met pakketten te installeren. Het hulpprogramma voor systeemeigen pakket cloud init automatisch gebruikt voor de distro die u selecteert.

 Er wordt om beschikbare installatiekopieën van het cloud-init zijn ingeschakeld in de Azure marketplace gewerkt met onze aangebracht Linux distro partners. Deze installatiekopieën brengt uw cloud-init-implementaties en configuraties naadloos werken met virtuele machines en VM Scale Sets (VMSS). De volgende tabel geeft een overzicht van de huidige beschikbaarheid van de installatiekopieën van cloud-init zijn ingeschakeld op de Azure-platform:

| Uitgever | Aanbieding | SKU | Versie | cloud-init gereed
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04-LTS |meest recente |ja | 
|Canonical |UbuntuServer |14.04.5-LTS |meest recente |ja |
|CoreOS |CoreOS |Stabiel |meest recente |ja |
|OpenLogic |CentOS |7-CI |meest recente |preview |
|RedHat |RHEL |7-RAW-CI |meest recente |preview |

Tijdens de preview wordt Azure Stack geen ondersteuning voor het inrichten van de RHEL 7.4 en CentOS 7.4 met behulp van cloud-init.

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Wat is het verschil tussen cloud init- en Linux-Agent (WALA)?
WALA is een Azure-platform-specifieke agent gebruikt voor het inrichten en verwerken van de Azure-extensies VM's configureren. We zijn verbeteren van de taak van de configuratie van virtuele machines voor het gebruik van cloud-init in plaats van de Linux-Agent om bestaande cloud init klanten om hun huidige cloud init-scripts te gebruiken.  Als u investeringen in de cloud init-scripts hebt voor het configureren van Linux-systemen, er zijn **geen aanvullende instellingen vereist** te maken. 

Als u geen Azure CLI `--custom-data` switch op de tijd, WALA inrichting wordt de minimale VM-inrichting van de vereiste parameters voor het inrichten van de virtuele machine en de implementatie met de standaardinstellingen te voltooien.  Als u verwijst naar de cloud-init `--custom-data` switch, wat is opgenomen in uw aangepaste gegevens (afzonderlijke instellingen of volledige script) onderdrukt de standaardwaarden WALA. 

WALA configuraties van virtuele machines zijn binnen het maximum aantal VM-inrichting tijd werken beperkte tijd.  Cloud-init-configuraties die zijn toegepast op virtuele machines hebben geen tijdsbeperkingen en leidt niet tot een implementatie mislukt door een time-out optreedt. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Virtuele Machine voor het implementeren van een cloud-init ingeschakeld
Een cloud-init ingeschakeld virtuele machine implementeert is net zo eenvoudig als het verwijst naar een distributiepunt cloud-init ingeschakeld tijdens de implementatie.  Linux-distributie instaan hebben te kiezen voor het inschakelen en cloud-init integreren in hun gepubliceerde basisinstallatiekopieën in Azure. Zodra u hebt vastgesteld dat de installatiekopie die u wilt implementeren cloud-init ingeschakeld is, kunt u de Azure CLI kunt gebruiken om de installatiekopie. 

De eerste stap bij het implementeren van deze installatiekopie is voor het maken van een resourcegroep met de [az groep maken](/cli/azure/group#az_group_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
De volgende stap is het maken van een bestand in uw huidige shell, met de naam *cloud init.txt* en plak de volgende configuratie. Bijvoorbeeld, het bestand te maken in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloud-init.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  -httpd
```
Druk op `ctrl-X` om af te sluiten van het bestand, typ `y` om op te slaan van het bestand en druk op `enter` om te bevestigen dat de bestandsnaam bij afsluiten.

De laatste stap is het maken van een virtuele machine met de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. 

Het volgende voorbeeld wordt een virtuele machine met de naam *centos74* en SSH-sleutels gemaakt als deze niet al bestaan op de standaardlocatie van de sleutel. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  Gebruik de `--custom-data`-parameter om door te geven in uw cloud-init-configuratiebestand. Geef het volledige pad naar de *cloud init.txt* config als u het bestand buiten uw werkmap aanwezig opgeslagen. Het volgende voorbeeld wordt een virtuele machine met de naam *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Als de virtuele machine is gemaakt, de Azure CLI worden gegevens weergegeven specifiek zijn voor uw implementatie. Noteer het `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.  Het duurt enige tijd voor de virtuele machine moet worden gemaakt, de pakketten te installeren en de app te starten. Er zijn achtergrondtaken die uitvoeren blijven nadat de Azure CLI keert u terug naar de prompt. U kunt SSH in de virtuele machine en de stappen in de sectie probleemoplossing gebruiken om de cloud init-logboeken weer te geven. 

## <a name="troubleshooting-cloud-init"></a>Cloud-init probleemoplossing
Zodra de virtuele machine is ingericht, cloud init wordt uitgevoerd via alle modules en scripts die zijn gedefinieerd in `--custom-data` om te configureren van de virtuele machine.  Als u oplossen wilt, fouten of nalatig gedrag van de configuratie, moet u zoeken naar de naam van de module (`disk_setup` of `runcmd` bijvoorbeeld) in het logboek cloud init - bevindt zich in **/var/log/cloud-init.log**.

> [!NOTE]
> Niet elke-modulefout resulteert in een cloud-init fatale fout bij het algemene configuratie. Bijvoorbeeld, met behulp van de `runcmd` -module, als het script is mislukt, cloud init nog steeds rapporteert inrichten is voltooid omdat de module runcmd uitgevoerd.

Raadpleeg voor meer informatie over logboekregistratie van cloud-init de [cloud init-documentatie](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor cloud-init voorbeelden van wijzigingen in de configuratie:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
