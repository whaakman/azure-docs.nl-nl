---
title: Wat is Azure Monitor voor virtuele machines (preview)? | Microsoft Docs
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
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: c7d2004da52d83ceda62dc31583797d9a218ef48
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085448"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>Wat is Azure Monitor voor virtuele machines (preview)?

Azure Monitor voor virtuele machines bewaakt uw Azure virtual machines (VM) en virtuele-machineschaalsets op schaal. De service analyseert de prestaties en status van uw Windows en Linux-VM's, hun processen en hun afhankelijkheden van andere bronnen en de externe processen bewaken. 

Azure Monitor voor virtuele machines bevat als een oplossing, ondersteuning voor het bewaken van prestatie- en afhankelijkheden voor virtuele machines die zich on-premises gehost of in een andere cloudprovider. Drie belangrijke functies bieden een diepgaande informatie:

* **Logische onderdelen van Azure-VM's met Windows en Linux**: ten opzichte van de van vooraf geconfigureerde gezondheidscriteria worden gemeten en ze een melding wanneer de geëvalueerde voorwaarde wordt voldaan.  

* **Vooraf gedefinieerde prestatiegrafieken populair**: core maatstaven voor prestaties van het gastbesturingssysteem van de virtuele machine weergeven.

* **Kaart van afhankelijkheden**: bevat de onderdelen met elkaar verbonden met de virtuele machine vanuit de verschillende resourcegroepen en abonnementen.  

De functies zijn ingedeeld in drie perspectieven:

* Status
* Prestaties
* Kaart

>[!NOTE]
>Op dit moment wordt de Health-functie aangeboden alleen voor virtuele Azure-machines en virtuele-machineschaalset sets. De functies voor prestaties en kaart ondersteuning voor zowel virtuele Azure-machines en virtuele machines die worden gehost in uw omgeving of andere cloudprovider.

Integratie met Log Analytics biedt krachtige aggregatie en filteren, en deze gegevenstrends na verloop van tijd kunt analyseren. Deze uitgebreide werkbelasting bewaking kan niet worden bereikt met Azure Monitor, serviceoverzicht of Log Analytics alleen.  

U kunt deze gegevens weergeven in een enkele virtuele machine van de virtuele machine rechtstreeks of u kunt Azure Monitor gebruiken voor het leveren van een samengevoegde weergave van uw virtuele machines. In deze weergave is gebaseerd op van elke functie perspectief:

* **Status**: de VM's zijn gekoppeld aan een resourcegroep.
* **Kaart** en **prestaties**: de virtuele machines zijn geconfigureerd voor rapportage aan een specifieke Log Analytics-werkruimte.

![Perspectief van de virtuele machine inzicht in de Azure-portal](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

Azure DevOps kan bieden voorspelbare prestaties en beschikbaarheid van essentiële toepassingen. Het identificeert kritieke besturingssysteemgebeurtenissen, knelpunten in de prestaties en netwerkproblemen. Azure DevOps kunt u inzicht in of een probleem is gerelateerd aan andere afhankelijkheden.  

## <a name="data-usage"></a>Gegevensgebruik 

Wanneer u Azure Monitor voor virtuele machines implementeert, wordt de gegevens die worden verzameld door uw virtuele machines die zijn opgenomen en opgeslagen in Azure Monitor. Op de prijzen die gepubliceerd op basis van de [Azure Monitor-pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/), Azure-Monitor voor virtuele machines wordt in rekening gebracht voor:
* De gegevens die is opgenomen en opgeslagen.
* Het nummer van de gezondheid van criteria metriek time series die worden bewaakt.
* De regels voor waarschuwingen die zijn gemaakt.
* De meldingen die worden verzonden. 

Grootte van het logboekbestand is afhankelijk van de lengte van de tekenreeks van prestatiemeteritems en deze kunt verhogen met het aantal logische schijven en netwerkadapters. Als u al een werkruimte hebt en deze prestatiemeteritems zijn verzameld, worden geen kosten verbonden aan dubbele toegepast. Als u al Serviceoverzicht, is de enige wijziging u ziet de extra verbinding-gegevens die worden verzonden naar Azure Monitor.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over de vereisten en de methoden die u helpen bij uw virtuele machines controleren, [implementeert Azure Monitor voor virtuele machines](vminsights-onboard.md).
