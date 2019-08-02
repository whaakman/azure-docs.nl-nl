---
title: Ondersteunde besturingssystemen, container-engines - Azure IoT Edge | Microsoft Docs
description: Informatie over welke besturingssystemen voor de Azure IoT Edge-daemon en de runtime en de ondersteunde container-engines voor uw productieapparaten kunt uitvoeren
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9b9acac829fd128a66e3ceea603bb804adf2a88b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598599"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IoT Edge ondersteunde systemen

In dit artikel vindt u informatie over welke systemen en onderdelen worden ondersteund door IoT Edge, of dat nu officieel of in de preview-versie is. 

Als u problemen ondervindt bij het gebruik van de Azure IoT Edge-service, zijn er verschillende manieren om ondersteuning te zoeken. Probeer een van de volgende kanalen voor ondersteuning:

**Rapportage van fouten** – het merendeel van ontwikkeling die in de Azure IoT Edge-product gaat er gebeurt in de IoT Edge open source-project. Fouten worden gerapporteerd in de [kwesties](https://github.com/azure/iotedge/issues) van het project. Oplossingen maken snel mogelijk naar productupdates van het project in.

**Microsoft Customer Support team** : gebruikers die beschikken over een [ondersteuningsplan](https://azure.microsoft.com/support/plans/) het team Microsoft Customer Support kunnen verrichten door het maken van een ondersteuningsticket rechtstreeks vanuit de [Azure-portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Functie aanvragen** : de Azure IoT Edge-product functieverzoeken bijgehouden via van het product [Uservoice-pagina](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Container-engines

Azure IoT Edge-modules worden geïmplementeerd als containers, zodat IoT Edge een container Engine nodig hebt om deze te starten. Microsoft biedt een container-engine, moby-engine, om te voldoen aan deze vereiste. Deze container engine is gebaseerd op het open-source project Moby. Docker CE- en Docker EE zijn andere populaire container-engines. Ze zijn ook gebaseerd op het open-source project Moby en zijn compatibel met Azure IoT Edge. Micro soft biedt best mogelijke ondersteuning voor systemen die gebruikmaken van deze container motoren; Micro soft kan echter geen oplossingen verzenden voor problemen. Microsoft adviseert om deze reden moby-engine op productiesystemen.

<br>
<center>

![Moby als container runtime](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Besturingssystemen
Azure IoT Edge wordt uitgevoerd op de meeste besturings systemen waarop containers kunnen worden uitgevoerd. al deze systemen worden echter niet gelijk ondersteund. Besturingssystemen zijn gegroepeerd in lagen, waarbij het niveau van ondersteuning voor gebruikers kunnen verwachten.
* Systemen met laag 1 worden ondersteund. Voor systemen met laag 1, micro soft:
    * heeft dit besturings systeem in geautomatiseerde tests
    * biedt de installatiepakketten van deze
* Systemen uit de laag 2 zijn compatibel met Azure IoT Edge en kunnen relatief eenvoudig worden gebruikt. Voor laag 2-systemen:
    * Micro soft heeft ad hoc tests uitgevoerd op de platforms of weet of een partner Azure IoT Edge op het platform heeft uitgevoerd
    * Installatiepakketten voor andere platforms werken op deze platforms
    
De familie van het hostbesturingssysteem moet altijd overeenkomen met de familie van het gast besturingssysteem dat in de container van een module wordt gebruikt. Met andere woorden, u kunt alleen Linux-containers gebruiken op Linux-en Windows-containers in Windows. Wanneer u Windows gebruikt, worden alleen geïsoleerde containers verwerken ondersteund, niet geïsoleerde Hyper-V-containers.  

<br>
<center>

![Host OS komt overeen met gast besturingssysteem](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Categorie 1

De systemen die worden vermeld in de volgende tabel worden ondersteund door micro soft, algemeen beschikbaar of in open bare preview, en worden getest met elke nieuwe release. 

| Besturingssysteem | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspbian stretch |  | ![Raspbian stretch + ARM32v7](./media/tutorial-c-module/green-check.png) |  |
| Ubuntu Server 16.04 | ![Ubuntu-Server 16,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Openbare preview  |
| Ubuntu-Server 18.04 | ![Ubuntu-Server 18,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Openbare preview |
| Windows 10 IoT Enter prise, build 17763 | ![Windows 10 IoT Enter prise + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server 2019, build 17763 | ![Windows Server 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server IoT 2019, build 17763 | ![Windows Server IoT 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT core, build 17763 | Openbare preview |  |  |


De Windows-besturings systemen die hierboven worden vermeld, zijn de vereisten voor apparaten waarop Windows-containers in Windows worden uitgevoerd. Deze configuratie is de enige ondersteunde configuratie voor productie. Met de Azure IoT Edge-installatie pakketten voor Windows is het gebruik van Linux-containers in Windows toegestaan. deze configuratie is echter alleen voor ontwikkelings-en test doeleinden. Het gebruik van Linux-containers in Windows is geen ondersteunde configuratie voor productie. Een versie van Windows 10 build 14393 of hoger en Windows Server 2016 of nieuwer kunnen worden gebruikt voor dit ontwikkel scenario.

### <a name="tier-2"></a>Categorie 2

De systemen die in de volgende tabel worden vermeld, worden beschouwd als compatibel met Azure IoT Edge, maar worden niet actief getest of onderhouden. 

| Besturingssysteem | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| CentOS 7.5 | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 8 | ![Debian 8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 9 | ![Debian 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 10<sup>1</sup> | ![Debian 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| RHEL 7.5 | ![RHEL 7,5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 16.04 | ![Ubuntu 16,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 18.04 | ![Ubuntu 18,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Wind rivier 8 | ![Wind stroom 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Yocto | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspbian Buster<sup>1</sup> |  | ![Raspbian Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspbian Buster + ARM64](./media/tutorial-c-module/green-check.png) |

<sup>1</sup> Debian 10-systemen, met inbegrip van Raspian Buster, gebruiken een versie van OpenSSL die IOT Edge niet ondersteunt. Gebruik de volgende opdracht om een eerdere versie te installeren voordat u IoT Edge installeert: 

```bash
sudo apt-get install libssl1.0.2
```

## <a name="virtual-machines"></a>Virtuele machines
Azure IoT Edge kunnen worden uitgevoerd op virtuele machines. Het gebruik van een virtuele machine als een IoT Edge apparaat is gebruikelijk wanneer klanten bestaande infra structuur willen uitbreiden met Edge Intelligence. De serie van het VM-besturings systeem van de host moet overeenkomen met de familie van het gast besturingssysteem dat in de container van een module wordt gebruikt. Deze vereiste is hetzelfde als wanneer Azure IoT Edge rechtstreeks op een apparaat wordt uitgevoerd. Azure IoT Edge is neutraal van de onderliggende technologie en werkt in Vm's op basis van platforms, zoals Hyper-V en vSphere.

<br>
<center>

![Azure IoT Edge in een VM](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Minimale systeem vereisten
Azure IoT Edge is uitstekend geschikt voor apparaten met een Raspberry-Pi3 op server kwaliteit. Het kiezen van de juiste hardware voor uw scenario is afhankelijk van de werk belastingen die u wilt uitvoeren. Het maken van de definitieve beslissing van het apparaat kan gecompliceerd zijn. u kunt echter eenvoudig beginnen met het prototypen van een oplossing op traditionele laptops of Bureau bladen.

Ervaring tijdens het prototypen helpt bij het selecteren van de uiteindelijke selectie van apparaten. Vragen die u moet overwegen: 

* Hoeveel modules bevinden zich in uw werk belasting?
* Hoeveel lagen delen de containers van de modules?
* In welke taal zijn uw modules geschreven? 
* Hoeveel gegevens worden er door uw modules verwerkt?
* Hebben uw modules gespecialiseerde hardware nodig om de werk belastingen te versnellen?
* Wat zijn de gewenste prestatie kenmerken van uw oplossing?
* Wat is uw hardware-budget?
