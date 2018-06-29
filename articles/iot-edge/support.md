---
title: Azure IoT-rand platformondersteuning | Microsoft Docs
description: Platforms die worden ondersteund door Azure IoT rand
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a5cbabf8080efd1ae25ba151a1be339e8f5cad2
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081751"
---
# <a name="azure-iot-edge-support"></a>Azure IoT-Edge-ondersteuning
Er zijn verschillende manieren te zoeken naar ondersteuning voor het product Azure IoT rand.

**Rapportage van fouten** – het merendeel van de ontwikkeling die u in het product Azure IoT rand gebeurt er in het IoT rand open source-project. Fouten worden gerapporteerd in de [pagina kwesties](https://github.com/azure/iot-edge/issues) van het project. Oplossingen maken snel hun manier van het project in op productupdates.

**Microsoft Customer Support team** – gebruikers die beschikken over een [ondersteuningsplan](https://azure.microsoft.com/support/plans/) de klantondersteuning van Microsoft-team kan benaderen door het maken van een ondersteuningsticket rechtstreeks vanuit de [Azure-portal]( https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Functie aanvragen** – rand van de Azure-IoT-product houdt functie-aanvragen via het product [User Voice pagina](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>Besturingssystemen
Azure IoT-rand wordt uitgevoerd op de meeste besturingssystemen met containers; echter worden al deze waarden niet gelijkmatig ondersteund. Besturingssystemen zijn gegroepeerd in lagen die het niveau van ondersteuning gebruikers kunnen verwachten vertegenwoordigen.

### <a name="tier-1"></a>Categorie 1
Laag 1-systemen worden beschouwd als officieel ondersteund. Dit betekent dat Microsoft:
* Deze besturingssysteem heeft in automatische tests
* installatiepakketten biedt voor deze

Algemeen beschikbaar
* Ubuntu Server 18.04
* Ubuntu Server 16.04
* Raspbian stretch

Openbare Preview
* Windows 10-Server 1803
* Windows 10 IoT Enterprise (met April 2018 update)
* Windows 10 IoT Core (met April 2018 update)

### <a name="tier-2"></a>Categorie 2
Laag 2-systemen worden beschouwd als compatibel met Azure IoT rand en relatief eenvoudig kunnen worden gebruikt. Dit betekent dat:
* Microsoft heeft gedaan ad-hoc testen op de platforms of kent van een partner Azure IoT rand correct functioneert op het platform
* Installatiepakketten voor andere platforms werkt op deze platforms

Ubuntu 18.04

Ubuntu 16.04

De o 8

Yocto

Debian

Mac

## <a name="container-engines"></a>Container-engines
Azure IoT-rand moet een container engine voor het starten van modules, ongeacht het besturingssysteem waarop deze wordt uitgevoerd. Microsoft biedt een container-engine moby-engine, om te voldoen aan deze vereiste. Deze is gebaseerd op het Moby open source-project. Docker CE en Docker EE zijn andere motoren populaire container. Ze ook zijn gebaseerd op het Moby open-source-project en compatibel zijn met Azure IoT rand. Microsoft biedt best effort ondersteuning voor systemen met behulp van de container motoren; Microsoft heeft echter niet de mogelijkheid voor het verzenden van oplossingen voor problemen bevat. Microsoft raadt daarom moby-engine op productiesystemen.


<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 