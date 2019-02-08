---
title: Ondersteunde besturingssystemen, container-engines - Azure IoT Edge | Microsoft Docs
description: Informatie over welke besturingssystemen voor de Azure IoT Edge-daemon en de runtime en de ondersteunde container-engines voor uw productieapparaten kunt uitvoeren
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: d8059ac4965ce5582b899ebc0d765e00ae9deb35
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892793"
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
Azure IoT Edge wordt uitgevoerd op de meeste besturingssystemen die containers; kunnen worden uitgevoerd. al deze waarden worden echter niet gelijkmatig ondersteund. Besturingssystemen zijn gegroepeerd in lagen, waarbij het niveau van ondersteuning voor gebruikers kunnen verwachten.
* Laag 1-systemen kunnen worden beschouwd als officieel ondersteund. Dit betekent dat Microsoft:
    * Dit besturingssysteem is in geautomatiseerde tests uit
    * biedt de installatiepakketten van deze
* Laag 2-systemen kunnen worden beschouwd als compatibel met Azure IoT Edge en relatief eenvoudig kunnen worden gebruikt. Dit betekent dat:
    * Microsoft heeft gedaan ad-hoc testen op een platform of kent van een partner die Azure IoT Edge is uitgevoerd op het platform
    * Installatiepakketten voor andere platforms werken op deze platforms
    
De familie van het hostbesturingssysteem moet altijd overeenkomen met de familie van het gastbesturingssysteem te installeren die wordt gebruikt in de container van een module. Met andere woorden, kunt u alleen Linux-containers in Linux en Windows-containers gebruiken op Windows. Wanneer u Windows gebruikt, zijn alleen proces geïsoleerde containers ondersteund, niet hyper-v geïsoleerde containers.  

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
| Windows 10 IoT Core build 17763 | Ja | Nee |
| Windows 10 IoT Enterprise build 17763 | Ja | Nee |
| Windows Server 2019 | Ja | Nee |

De bovenstaande Windows-besturingssystemen zijn de vereisten voor welke Windows-containers worden uitgevoerd op Windows-apparaten. Dit is de enige ondersteunde configuratie voor productie. Het gebruik van Linux-containers in Windows; van de Azure IoT Edge-installatiepakketten voor Windows toestaan Dit is echter alleen voor ontwikkeling en testen. Gebruik van Linux-containers in Windows is geen ondersteunde configuratie voor productie. Elke versie van Windows 10-build 14393 of hoger en WindowsServer 2016 of hoger kan worden gebruikt voor dit scenario voor het ontwikkelen.

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
Azure IoT Edge kunnen worden uitgevoerd in virtuele machines. Dit is gebruikelijk wanneer klanten willen bestaande infrastructuur uitbreiden met randintelligentie. De familie van de virtuele machine hostbesturingssysteem moet overeenkomen met de familie van het gastbesturingssysteem te installeren die wordt gebruikt in de container van een module. Dit is dezelfde vereisten als Azure IoT Edge rechtstreeks op een apparaat wordt uitgevoerd. Azure IoT Edge is los van de onderliggende virtualisatietechnologie en werkt in VM's met platformen, zoals Hyper-V- en vSphere.

<br>
<center>
![Azure IoT Edge op een virtuele machine](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Minimale systeemvereisten
Azure IoT Edge uitstekend wordt uitgevoerd op apparaten die zo klein is als een Raspberry Pi3 server geavanceerde hardware. Het kiezen van de juiste hardware voor uw scenario is erg afhankelijk zijn van de werkbelastingen die u wilt uitvoeren. Het laatste apparaat beslissing kan ingewikkeld; zijn u kunt echter eenvoudig ontwikkelen van prototypen een oplossing voor traditionele laptops of desktops starten.

Ervaringen bij het ontwikkelen van prototypen helpt uw apparaatselectie van het laatste. Vragen kunt u overwegen opnemen: hoeveel modules deel uitmaken van uw werkbelasting, het aantal lagen uw modules containers delen, in welke taal zijn de modules die zijn geschreven, hoeveel gegevens wordt uw modules worden verwerkt, een gespecialiseerde modules hoeft te doen hardware voor het versnellen van hun workloads, wat zijn de gewenste prestatiekenmerken van uw oplossing, wat is uw budget hardware?
