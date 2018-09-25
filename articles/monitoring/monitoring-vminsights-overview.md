---
title: Wat is Azure Monitor voor virtuele machines? | Microsoft Docs
description: Azure Monitor voor virtuele machines is een functie van Azure Monitor die de status en prestaties bewaken van het besturingssysteem van de virtuele machine van Azure, evenals automatisch detecteren van onderdelen van de toepassing en afhankelijkheden met andere resources worden gecombineerd en wijst de communicatie tussen beide. In dit artikel bevat een overzicht.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 26fcc3eb78af53360cca57382b4c06b017f36c0e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063265"
---
# <a name="what-is-azure-monitor-for-vms"></a>Wat is Azure Monitor voor virtuele machines?

Azure Monitor voor virtuele machines bewaakt uw Azure virtual machines (VM) op schaal door het analyseren van de prestaties en status van uw Windows en Linux-VM's, met inbegrip van hun verschillende processen en de gekoppelde afhankelijkheden van andere bronnen en de externe processen. De oplossing biedt ondersteuning voor het bewaken van de prestaties en afhankelijkheden voor toepassingen voor virtuele machines die worden gehost on-premises of een andere cloudprovider.  Het bevat drie belangrijke functies voor het leveren van deze diepgaande informatie:

* Logische onderdelen van virtuele Azure-machines met Windows en Linux-besturingssysteem worden gemeten op basis van een set criteria die zijn vooraf geconfigureerde status en waarschuwingen wanneer de geëvalueerde voorwaarde wordt voldaan.  
* Core maatstaven voor prestaties van de processor, geheugen, schijf en de netwerkadapter van het gastbesturingssysteem van de virtuele machine worden verzameld en weergegeven in de vooraf gedefinieerde trending worden prestatiegrafieken weergegeven.
* Kaart van afhankelijkheden van de gedetecteerde onderling verbonden onderdelen met die virtuele machine vanaf meerdere resourcegroepen en abonnementen.  

Deze functies zijn ingedeeld in drie perspectieven:

* Status
* Prestaties
* Kaart

>[!NOTE]
>De Health-functie wordt momenteel alleen aangeboden voor Azure virtual machines.
>

Integratie met Log Analytics biedt krachtige aggregatie, te filteren en de mogelijkheid om uit te voeren trendanalyse van de gegevens na verloop van tijd. De uitgebreide bewaking van uw workloads worden niet alleen bereikt met Azure Monitor-, Service Map- of Log Analytics.  

U kunt deze gegevens weergeven in de context van één virtuele machine van de virtuele machine rechtstreeks of met Azure Monitor biedt een samengevoegde weergave van uw virtuele machines op basis van de volgende perspectief voor elke functie:

* Health - VM's met betrekking tot een resourcegroep
* Kaart en de prestaties - VM's geconfigureerd om te rapporteren aan een specifieke Log Analytics-werkruimte

![Perspectief van de virtuele machine inzichten van portal](./media/monitoring-vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps kunt effectief bieden voorspelbare prestaties en beschikbaarheid van vitale toepassingen door het identificeren van kritieke besturingssysteemgebeurtenissen en knelpunten in de prestaties, problemen met het netwerk, en te begrijpen als er een probleem is gerelateerd aan andere afhankelijkheden.  

## <a name="data-usage"></a>Gegevensgebruik 

Ingebouwde Azure-Monitor voor virtuele machines, gegevens die zijn verzameld door uw virtuele machines worden zo snel als u die zijn opgenomen en opgeslagen in Azure Monitor.  Azure Monitor voor virtuele machines wordt in rekening gebracht voor de gegevens die zijn opgenomen en behouden, aantal criteria voor beveiligingsstatus metrische time series-regels voor bewaakte, waarschuwingen gemaakt, meldingen verzonden, prijzen gepubliceerd op de Azure Monitor [pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/)

Grootte van het logboekbestand varieert op basis van de lengte van de tekenreeks van prestatiemeteritems en kunt verhogen met het aantal logische schijven en netwerkadapters.  Als u al een werkruimte hebt en deze prestatiemeteritems zijn verzameld, zullen niet er dubbele kosten die worden toegepast.  Als u al van Service Map gebruikmaakt, is de enige wijziging u ziet de extra verbinding-gegevens die worden verzonden naar Azure Monitor.

## <a name="next-steps"></a>Volgende stappen
Beoordeling [ingebouwde Azure-Monitor voor virtuele machines](monitoring-vminsights-onboard.md) voor informatie over vereisten en methoden voor bewaking van uw virtuele machines wilt inschakelen.
