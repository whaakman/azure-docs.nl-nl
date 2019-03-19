---
title: Ondersteunde besturingssystemen, container-engines - Azure IoT Edge | Microsoft Docs
description: Informatie over welke besturingssystemen voor de Azure IoT Edge-daemon en de runtime en de ondersteunde container-engines voor uw productieapparaten kunt uitvoeren
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/18/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5bc133e81f9917aafb406a6bfb27922cdba48ef5
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190002"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IoT Edge ondersteunde systemen

Er zijn tal van manieren om te zoeken naar ondersteuning voor de Azure IoT Edge-product.

**Rapportage van fouten** – het merendeel van ontwikkeling die in de Azure IoT Edge-product gaat er gebeurt in de IoT Edge open source-project. Fouten worden gerapporteerd in de [kwesties](https://github.com/azure/iotedge/issues) van het project. Oplossingen maken snel mogelijk naar productupdates van het project in.

**Microsoft Customer Support team** : gebruikers die beschikken over een [ondersteuningsplan](https://azure.microsoft.com/support/plans/) het team Microsoft Customer Support kunnen verrichten door het maken van een ondersteuningsticket rechtstreeks vanuit de [Azure-portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Functie aanvragen** : de Azure IoT Edge-product functieverzoeken bijgehouden via van het product [Uservoice-pagina](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Container-engines
Azure IoT Edge moet een container-engine modules starten omdat ze worden geïmplementeerd als containers. Microsoft biedt een container-engine, moby-engine, om te voldoen aan deze vereiste. Deze is gebaseerd op de Moby open source-project. Docker CE- en Docker EE zijn andere populaire container-engines. Ze zijn ook afhankelijk van de Moby open source-project en compatibel zijn met Azure IoT Edge. Microsoft biedt best inspanning ondersteuning voor systemen met behulp van de container motoren; Microsoft heeft echter niet de mogelijkheid om oplossingen voor problemen in deze af te leveren. Microsoft adviseert om deze reden moby-engine op productiesystemen.

<br>
<center>

![Moby als container-runtime](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Besturingssystemen
Azure IoT Edge wordt uitgevoerd op de meeste besturingssystemen die containers; kunnen worden uitgevoerd. al deze systemen worden echter niet gelijkmatig ondersteund. Besturingssystemen zijn gegroepeerd in lagen, waarbij het niveau van ondersteuning voor gebruikers kunnen verwachten.
* Laag 1-systemen kunnen worden beschouwd als officieel ondersteund. Voor laag 1-systemen, Microsoft:
    * Dit besturingssysteem is in geautomatiseerde tests uit
    * biedt de installatiepakketten van deze
* Laag 2-systemen kunnen worden beschouwd als compatibel met Azure IoT Edge en relatief eenvoudig kunnen worden gebruikt. Voor laag 2-systemen:
    * Microsoft heeft gedaan ad-hoc testen op een platform of kent van een partner die Azure IoT Edge is uitgevoerd op het platform
    * Installatiepakketten voor andere platforms werken op deze platforms
    
De familie van het hostbesturingssysteem moet altijd overeenkomen met de familie van het gastbesturingssysteem te installeren die wordt gebruikt in de container van een module. Met andere woorden, kunt u alleen Linux-containers in Linux en Windows-containers gebruiken op Windows. Bij het gebruik van Windows, alleen proces geïsoleerde containers worden ondersteund, niet Hyper-V (geïsoleerd) containers.  

<br>
<center>

![Hostbesturingssysteem komt overeen met de Gast-OS](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Categorie 1
Algemeen beschikbaar

| Besturingssysteem | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Raspbian stretch | Nee | Ja|
| Ubuntu Server 16.04 | Ja | Nee |
| Ubuntu-Server 18.04 | Ja | Nee |

Openbare preview

| Besturingssysteem | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core, build 17763 | Ja | Nee |
| Windows 10 IoT Enterprise, build 17763 | Ja | Nee |
| Windows Server 2019 | Ja | Nee |

De Windows-besturingssystemen die hierboven worden vermeld, zijn de vereisten voor apparaten met Windows-containers in Windows. Deze configuratie is de enige ondersteunde configuraties voor productie. Het gebruik van Linux-containers in Windows; van de Azure IoT Edge-installatiepakketten voor Windows toestaan Deze configuratie is echter alleen voor ontwikkeling en testen. Gebruik van Linux-containers in Windows is geen ondersteunde configuratie voor productie. Elke versie van Windows 10-build 14393 of hoger en WindowsServer 2016 of hoger kan worden gebruikt voor dit scenario voor het ontwikkelen.

### <a name="tier-2"></a>Categorie 2

| Besturingssysteem | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Ja | Ja |
| Debian 8 | Ja | Ja |
| Debian 9 | Ja | Ja |
| RHEL 7.5 | Ja | Ja |
| Ubuntu 18.04 | Ja | Ja |
| Ubuntu 16.04 | Ja | Ja |
| Wind rivier 8 | Ja | Nee |
| Yocto | Ja | Nee |


## <a name="virtual-machines"></a>Virtuele machines
Azure IoT Edge kunnen worden uitgevoerd in virtuele machines. Met behulp van een virtuele machine als een IoT-Edge is apparaat gebruikelijk wanneer klanten willen bestaande infrastructuur uitbreiden met randintelligentie. De familie van de virtuele machine hostbesturingssysteem moet overeenkomen met de familie van het gastbesturingssysteem te installeren die wordt gebruikt in de container van een module. Deze vereiste is hetzelfde als bij Azure IoT Edge rechtstreeks op een apparaat wordt uitgevoerd. Azure IoT Edge is los van de onderliggende virtualisatietechnologie en werkt in VM's met platformen, zoals Hyper-V- en vSphere.

<br>
<center>

![Azure IoT Edge op een virtuele machine](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Minimale systeemvereisten
Azure IoT Edge uitstekend wordt uitgevoerd op apparaten die zo klein is als een Raspberry Pi3 server geavanceerde hardware. Kies de juiste hardware voor uw scenario, is afhankelijk van de werkbelastingen die u wilt uitvoeren. Het laatste apparaat beslissing kan ingewikkeld; zijn u kunt echter eenvoudig ontwikkelen van prototypen een oplossing voor traditionele laptops of desktops starten.

Ervaringen bij het ontwikkelen van prototypen helpt uw apparaatselectie van het laatste. U moet rekening houden met vragen zijn onder andere: 

* Hoeveel modules zijn in uw werkbelasting?
* Het aantal lagen maken gebruik van uw modules containers?
* In welke taal zijn de modules geschreven? 
* Hoeveel gegevens wordt uw modules verwerkt?
* Moeten uw modules geen speciale hardware voor het versnellen van hun workloads?
* Wat zijn de gewenste prestatiekenmerken van uw oplossing?
* Wat is uw budget hardware?
