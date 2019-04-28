---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 09/12/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ec6cbcbc93fe87634c87caeb0041b75ec916a22f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405377"
---
U een poort openen of maken van een eindpunt dat aan een virtuele machine (VM) in Azure met het maken van een netwerk-filter op een subnet of een VM-netwerkinterface. U plaatst deze filters, die binnenkomend en uitgaand verkeer worden beheerd, op een netwerkbeveiligingsgroep die is gekoppeld aan de resource die het verkeer ontvangt.

Het voorbeeld in dit artikel ziet u hoe u een netwerk-filter die gebruikmaakt van de standaard TCP-poort 80 (er wordt aangenomen dat u al hebt gestart, de juiste services en firewallregels besturingssysteem op de virtuele machine geopend) te maken.

Nadat u een virtuele machine die geconfigureerd voor de webaanvragen worden verwerkt op de standaard TCP-poort 80 hebt gemaakt, kunt u het volgende doen:

1. Maak een netwerkbeveiligingsgroep.

2. Maak een beveiligingsregel voor binnenkomend verkeer wordt toegestaan en waarden toewijzen aan de volgende instellingen:

   - **Poortbereiken van doel**: 80

   - **Poortbereiken van bron**: * (elke bron-poort worden toegestaan)

   - **Prioriteitswaarde**: Voer een waarde die kleiner is dan 65.500 en hogere prioriteit dan de standaardwaarde catch-alle inkomende regel voor weigeren.

3. De netwerkbeveiligingsgroep koppelen aan het VM-netwerkinterface of subnet.

    Hoewel in dit voorbeeld een eenvoudige regel wordt HTTP-verkeer toe te staan, kunt u ook netwerkbeveiligingsgroepen en regels gebruiken om te maken van complexere configuraties van het netwerk.