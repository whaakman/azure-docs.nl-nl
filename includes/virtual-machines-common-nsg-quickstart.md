---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d9c8a0e6a3bd6d79a11ee0d0dab0500a209e5571
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38941367"
---
U een poort openen of maken van een eindpunt dat aan een virtuele machine (VM) in Azure met het maken van een netwerk-filter op een subnet of VM-netwerkinterface. U plaatst deze filters, die binnenkomend en uitgaand verkeer worden beheerd, op een Netwerkbeveiligingsgroep die is gekoppeld aan de resource die het verkeer ontvangt.

We gebruiken een algemeen voorbeeld van webverkeer op poort 80. Beschikt u over een virtuele machine die is geconfigureerd om te fungeren webaanvragen op de standaard TCP-poort 80 (Vergeet niet om de juiste services starten en firewallregels besturingssysteem op de virtuele machine ook openen), u:

1. Maak een Netwerkbeveiligingsgroep.
2. Maak een inkomende regel het verkeer met:
   * het poortbereik van doel van "80"
   * het bron-poortbereik van ' * ' (zodat elke bron-poort)
   * een prioriteitswaarde van minder 65,500 (om te worden hogere prioriteit dan de standaard catch-alle inkomende regel voor weigeren)
3. De Netwerkbeveiligingsgroep koppelen aan het VM-netwerkinterface of subnet.

U kunt complexe netwerkconfiguraties voor het beveiligen van uw omgeving met behulp van Netwerkbeveiligingsgroepen en regels kunt maken. In ons voorbeeld maakt gebruik van slechts één of twee regels waarmee HTTP-verkeer of extern beheer. Zie voor meer informatie de volgende ['Meer informatie'](#more-information-on-network-security-groups) sectie of [wat is er een Netwerkbeveiligingsgroep?](../articles/virtual-network/security-overview.md)

