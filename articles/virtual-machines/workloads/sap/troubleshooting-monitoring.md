---
title: Bewaking van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Monitor voor SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f046304121e0aed8efa1bbc2535d34186eba3496
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60713704"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Het bewaken van SAP HANA (grote instanties) op Azure

SAP HANA op Azure (grote instanties) is niet anders als elke andere IaaS-implementatie, moet u controleren wat het besturingssysteem en de toepassing doet en hoe de toepassingen gebruiken voor de volgende bronnen:

- CPU
- Geheugen
- Netwerkbandbreedte
- Schijfruimte

Met Azure Virtual Machines u wilt achterhalen of de hierboven genoemde resourceklassen voldoende zijn, of ze ophalen uitgeput. Hier vindt u meer informatie over elk van de verschillende klassen:

**CPU-verbruik van resources:** De verhouding tussen SAP gedefinieerd voor bepaalde werkbelasting op basis van HANA wordt afgedwongen om ervoor te zorgen dat er onvoldoende CPU-bronnen beschikbaar om te werken via de gegevens die zijn opgeslagen in het geheugen moet zijn. Niettemin mogelijk zijn er gevallen waarbij HANA veel CPU's voor het uitvoeren van query's vanwege ontbrekende indexen of vergelijkbare problemen verbruikt. Dit betekent dat, moet u CPU-resourceverbruik van de HANA grote instantie eenheid, evenals de CPU-resources die worden gebruikt door de specifieke HANA-services controleren.

**Geheugengebruik:** Is het belangrijk om te controleren van binnen HANA, evenals buiten HANA op de eenheid. In HANA, moet u controleren hoe de gegevens al gebruikmaakt van de toegewezen geheugen om te kunnen blijven binnen de richtlijnen van de vereiste grootte van SAP HANA. U wilt ook geheugengebruik op het niveau van de grote instantie om ervoor te zorgen dat aanvullende geïnstalleerd niet-HANA-software niet te veel geheugen in beslag nemen, en daarom met HANA om geheugen concurreren te controleren.

**Bandbreedte van het netwerk:** De Azure-VNet-gateway is beperkt in de bandbreedte van de gegevens verplaatst naar het Azure-VNet, dus is het handig is voor het bewaken van de gegevens die zijn ontvangen door alle Azure VM's binnen een VNet om te achterhalen hoe dicht u zijn de limieten van de Azure gateway-SKU die u hebt geselecteerd. Op de eenheid HANA grote instantie het verstandig ook binnenkomende en uitgaande netwerkverkeer te bewaken, en om de volumes die worden uitgevoerd na verloop van tijd bij te houden.

**Schijfruimte:** Schijfruimteverbruik wordt doorgaans na verloop van tijd toeneemt. Meest voorkomende oorzaken zijn: gegevensvolume toeneemt, uitvoering van transactielogboekback-ups, voor het opslaan van logboekbestanden voor tracering en het uitvoeren van de storage-momentopnamen. Het is daarom belangrijk voor het gebruik van schijfruimte bewaken en beheren van de schijfruimte die is gekoppeld aan de eenheid HANA grote instantie.

Voor de **SKU's Type II** van HANA grote instanties, de server wordt geleverd met de vooraf geladen system diagnostische hulpprogramma's. U kunt gebruikmaken van deze diagnostische hulpprogramma's om uit te voeren van de statuscontrole van het systeem. Voer de volgende opdracht uit om de status Controleer het logboekbestand op /var/log/health_check genereert.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Wanneer u met het team van Microsoft Support een probleem op te lossen werkt, kan u ook voor de logboekbestanden met behulp van deze diagnostische hulpprogramma's worden gevraagd. U kunt het bestand met de volgende opdracht zip.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Volgende stappen**

- Raadpleeg [SAP HANA (grote instanties) op Azure controleren](troubleshooting-monitoring.md).