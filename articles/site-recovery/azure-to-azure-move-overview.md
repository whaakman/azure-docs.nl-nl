---
title: Azure IaaS VM's verplaatsen naar een andere Azure-regio met behulp van de service Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS-VM’s te verplaatsen van de ene Azure-regio naar een andere.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: dc49b33fd3e6d582b31af5fe0507884e60205757
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58078003"
---
# <a name="move-azure-vms-to-another-region"></a>Virtuele Azure-machines verplaatsen naar een andere regio

Azure groeit, samen met de klant basis en voegt ondersteuning toe voor de nieuwe regio's te laten groeien met toenemende behoeften. Nieuwe mogelijkheden ook maandelijks toegevoegd tussen services. Het is raadzaam om te verplaatsen van uw virtuele machines (VM's) naar een andere regio of in Beschikbaarheidszones om beschikbaarheid te vergroten.

Deze zelfstudie beschrijft verschillende scenario's waarin u wilt verplaatsen van uw virtuele machines. Ook wordt beschreven hoe u de architectuur in de doelregio voor hogere beschikbaarheid configureert. 

In deze zelfstudie leert u over:

> [!div class="checklist"]
> 
> * Redenen om te verplaatsen van virtuele machines
> * Typische architecturen
> * Het verplaatsen van virtuele machines als een doelregio
> * Het verplaatsen van virtuele machines om beschikbaarheid te vergroten

## <a name="reasons-to-move-azure-vms"></a>Redenen om te verplaatsen van virtuele Azure-machines

U kunt virtuele machines verplaatsen om de volgende redenen:

- U al hebt geïmplementeerd in één regio en een nieuwe ondersteuning voor de regio die zich dichter bij de eindgebruikers van uw toepassing of service is toegevoegd. In dit scenario zou u uw virtuele machines verplaatsen naar de nieuwe regio is om te verminderen latentie. Dezelfde benadering gebruiken als u wilt consolideren abonnementen of als er governance of organisatie regels waarvoor u wilt verplaatsen.
- Uw virtuele machine is geïmplementeerd als een single instance-machine of als onderdeel van een beschikbaarheidsset. Als u de beschikbaarheids-Sla's verhogen wilt, kunt u uw virtuele machines verplaatsen naar een Beschikbaarheidszone.

## <a name="steps-to-move-azure-vms"></a>Stappen voor het verplaatsen van virtuele Azure-machines

Het verplaatsen van virtuele machines omvat de volgende stappen:

1. Controleer de vereisten.
2. Bereid de bron-VM's.
3. Bereid de doelregio.
4. Gegevens kopiëren naar de doelregio. Gebruik Azure Site Recovery-Replicatietechnologie voor gegevens kopiëren van de bron-VM naar de doelregio.
5. Test de configuratie. Nadat de replicatie voltooid is, test u de configuratie door het uitvoeren van een testfailover met een niet-productie-netwerk.
6. Voer de verplaatsing.
7. Verwijdert de resources in de bronregio.

> [!NOTE]
> Meer informatie over deze stappen vindt u in de volgende secties.
> [!IMPORTANT]
> Op dit moment Azure Site Recovery ondersteunt de verplaatsing van virtuele machines van de ene regio naar een andere, maar biedt geen ondersteuning voor het verplaatsen binnen een regio.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Gangbare architecturen voor een implementatie met meerdere lagen

Deze sectie beschrijft de meest voorkomende implementatie-architecturen voor een toepassing met meerdere lagen in Azure. Het voorbeeld is een toepassing met drie lagen met een openbaar IP-adres. Elk van de lagen (web-, toepassings- en database) heeft elke twee virtuele machines, en ze zijn verbonden met een Azure load balancer naar de andere lagen. De databaselaag heeft SQL Server Always On-replicatie tussen de VM's voor hoge beschikbaarheid.

* **Single instance virtuele machines die zijn geïmplementeerd voor de verschillende lagen**: Elke virtuele machine in een laag is geconfigureerd als een single instance-machine en is verbonden door load balancers aan de andere lagen. Deze configuratie is de eenvoudigste vast te stellen.

     ![VM-implementatie voor één exemplaar voor lagen](media/move-vm-overview/regular-deployment.png)

* **Virtuele machines in elke laag in beschikbaarheidssets**: Elke virtuele machine in een laag is geconfigureerd in een beschikbaarheidsset. [Beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) zorgen ervoor dat de VM's die u in Azure implementeert over meerdere geïsoleerde hardwareknooppunten in een cluster worden verdeeld. Dit zorgt ervoor dat als een hardware- of softwarefout optreedt in Azure gebeurt, alleen een subset van uw virtuele machines worden beïnvloed en uw totale oplossing beschikbaar en operationeel blijft.

     ![VM-implementatie in beschikbaarheidssets](media/move-vm-overview/avset.png)

* **Virtuele machines in elke laag die is geïmplementeerd in meerdere Beschikbaarheidszones**: Elke virtuele machine in een laag is geconfigureerd voor [Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Een beschikbaarheidszone in een Azure-regio is een combinatie van een foutdomein en een updatedomein. Als u bijvoorbeeld drie of meer virtuele machines in drie zones in een Azure-regio maakt, worden uw virtuele machines effectief over drie foutdomeinen en drie updatedomeinen verdeeld. Het Azure-platform herkent deze verdeling over updatedomeinen om ervoor te zorgen dat virtuele machines in verschillende zones niet op hetzelfde moment worden bijgewerkt.

     ![Binnen een Beschikbaarheidszone implementatie](media/move-vm-overview/zone.png)

## <a name="move-vms-as-is-to-a-target-region"></a>Virtuele machines ongewijzigd naar een doelregio verplaatsen

Op basis van de [architecturen](#typical-architectures-for-a-multi-tier-deployment) eerder vermeld, dit is wat de implementaties eruitziet nadat u de overstap naar de doelregio is uitgevoerd.

* **Single instance virtuele machines die zijn geïmplementeerd voor de verschillende lagen**

     ![VM-implementatie voor één exemplaar voor lagen](media/move-vm-overview/single-zone.png)

* **Virtuele machines in elke laag in beschikbaarheidssets**

     ![Cross-beschikbaarheidssets voor de regio](media/move-vm-overview/crossregionaset.png)

* **Virtuele machines in elke laag die is geïmplementeerd in meerdere Beschikbaarheidszones**

     ![VM-implementatie in meerdere Beschikbaarheidszones](media/move-vm-overview/azonecross.png)

## <a name="move-vms-to-increase-availability"></a>Virtuele machines verplaatsen om beschikbaarheid te vergroten

* **Single instance virtuele machines die zijn geïmplementeerd voor de verschillende lagen**

     ![VM-implementatie voor één exemplaar voor lagen](media/move-vm-overview/single-zone.png)

* **Virtuele machines in elke laag in beschikbaarheidssets**: U kunt uw virtuele machines configureren in een beschikbaarheidsset in afzonderlijke Beschikbaarheidszones wanneer u replicatie voor de virtuele machine inschakelen met behulp van Azure Site Recovery. De SLA voor beschikbaarheid van zal 99,9% zijn, na het voltooien van de bewerking voor verplaatsen.

     ![VM-implementatie in beschikbaarheidssets en Beschikbaarheidszones](media/move-vm-overview/aset-azone.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> 
> * [Virtuele Azure-machines verplaatsen naar een andere regio](azure-to-azure-tutorial-migrate.md)
> 
> * [Virtuele Azure-machines verplaatsen naar beschikbaarheidszones](move-azure-vms-avset-azone.md)

