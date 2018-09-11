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
ms.openlocfilehash: e5a5d462be5555090d1dfced5fa07c9b748eb312
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345655"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Het bewaken van SAP HANA (grote instanties) op Azure

SAP HANA op Azure (grote instanties) is niet anders als elke andere IaaS-implementatie, moet u controleren wat het besturingssysteem en de toepassing doet en hoe de toepassingen gebruiken voor de volgende bronnen:

- CPU
- Geheugen
- Netwerkbandbreedte
- Schijfruimte

Met Azure Virtual Machines u wilt achterhalen of de hierboven genoemde resourceklassen voldoende zijn, of ze ophalen uitgeput. Hier vindt u meer informatie over elk van de verschillende klassen:

**Resource-CPU-verbruik:** de hoogte-breedteverhouding die SAP gedefinieerd voor een bepaalde workload op basis van HANA wordt afgedwongen om ervoor te zorgen dat er onvoldoende CPU-bronnen beschikbaar om te werken via de gegevens die zijn opgeslagen in het geheugen moet zijn. Niettemin mogelijk zijn er gevallen waarbij HANA veel CPU's voor het uitvoeren van query's vanwege ontbrekende indexen of vergelijkbare problemen verbruikt. Dit betekent dat, moet u CPU-resourceverbruik van de HANA grote instantie eenheid, evenals de CPU-resources die worden gebruikt door de specifieke HANA-services controleren.

**Geheugengebruik:** Is het belangrijk om te controleren van binnen HANA, evenals buiten HANA op de eenheid. In HANA, moet u controleren hoe de gegevens al gebruikmaakt van de toegewezen geheugen om te kunnen blijven binnen de richtlijnen van de vereiste grootte van SAP HANA. U wilt ook geheugengebruik op het niveau van de grote instantie om ervoor te zorgen dat aanvullende geïnstalleerd niet-HANA-software niet te veel geheugen in beslag nemen, en daarom met HANA om geheugen concurreren te controleren.

**De netwerkbandbreedte:** de Azure-VNet-gateway is in de bandbreedte van de gegevens verplaatst naar het Azure-VNet beperkt, dus is het handig om te controleren door de gegevens worden ontvangen door alle Azure VM's binnen een VNet om te achterhalen hoe sluit u de limieten van de Azure gateway-SKU gaat die door sel te schrijven. Op de eenheid HANA grote instantie het verstandig ook binnenkomende en uitgaande netwerkverkeer te bewaken, en om de volumes die worden uitgevoerd na verloop van tijd bij te houden.

**Schijfruimte:** schijfruimteverbruik doorgaans na verloop van tijd toeneemt. Meest voorkomende oorzaken zijn: gegevensvolume toeneemt, uitvoering van transactielogboekback-ups, voor het opslaan van logboekbestanden voor tracering en het uitvoeren van de storage-momentopnamen. Het is daarom belangrijk voor het gebruik van schijfruimte bewaken en beheren van de schijfruimte die is gekoppeld aan de eenheid HANA grote instantie.

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