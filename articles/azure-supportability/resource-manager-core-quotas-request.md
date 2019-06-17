---
title: Azure Resource Manager-vCPU-quotum aanvragen verhogen | Microsoft Docs
description: Azure Resource Manager-vCPU-aanvragen voor het verhogen van quota
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076811"
---
# <a name="quota-increase-requests"></a>Aanvragen voor quotumverhoging

Resource Manager-vCPU-quota voor virtuele machines en virtuele-machineschaalsets worden afgedwongen op twee lagen voor elk abonnement, in elke regio. 

De eerste laag is de limiet van het totaal aantal regionale vcpu's (voor alle VM-serie) en de tweede laag is de limiet van de VM-reeks vcpu's (zoals de D-serie vcpu's) per. Telkens wanneer die een nieuwe virtuele machine kan worden geïmplementeerd, is moet de som van de nieuwe en bestaande vcpu's gebruik voor die VM-reeks het vCPU-quotum goedgekeurd voor die specifieke VM-reeks niet overschrijden. Het totaal aantal nieuwe en bestaande vCPU-aantal geïmplementeerd op alle VM-reeksen mag bovendien niet meer dan het totaal aantal regionale Vcpu-quotum goedgekeurd voor het abonnement. Als een van deze quota worden overschreden, wordt de VM-implementatie niet toegestaan.
U kunt een verhoging van de quotumlimiet voor uw vcpu's voor de VM-reeks aanvragen vanuit Azure portal. De limiet van het totaal aantal regionale vcpu's wordt een toename van het quotum-serie VM's automatisch evenveel. 

Wanneer een nieuw abonnement wordt gemaakt, de standaard-totaal aantal regionale vcpu's mogelijk niet gelijk zijn aan de som van de standaard vCPU-quota's voor alle afzonderlijke VM-reeksen. Dit kan resulteren in een abonnement voldoende quotum voor elke afzonderlijke VM-reeks die u wilt implementeren, maar er is onvoldoende quotum voor het totaal aantal regionale vcpu's voor alle implementaties. In dit geval moet u een verzoek indienen om de limiet van totaal aantal regionale vcpu's expliciet verhogen. Totaal aantal regionale vcpu's beperken niet langer zijn dan de som van de goedgekeurde quotum voor alle VM-reeksen voor de regio.

Meer informatie over quota's in de [virtuele machine vCPU-quota pagina](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) en [Azure-abonnement en Servicelimieten](https://aka.ms/quotalimits) pagina. 

