---
title: Azure IaaS-VM's verplaatsen naar een andere Azure-regio met behulp van de Azure Site Recovery-service | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS-VM’s te verplaatsen van de ene Azure-regio naar een andere.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5d844692b6199d93fa835da1021c9753311e17de
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824513"
---
# <a name="move-azure-vms-to-another-region"></a>Virtuele Azure-machines verplaatsen naar een andere regio

Azure groeit sterk, evenals haar klantenbestand, en voegt ondersteuning toe voor nieuwe regio’s met stijgende vraag. Ook zijn er nieuwe mogelijkheden die maandelijks in verschillende services worden toegevoegd. Daarom zijn er momenten waarop u uw virtuele machines naar een andere regio of naar andere Azure-beschikbaarheidszones wilt verplaatsen om de beschikbaarheid te vergroten.

Dit document behandelt de verschillende scenario's waarin u uw virtuele machines zou willen verplaatsen en bevat een handleiding over de manier waarop de architectuur moet worden geconfigureerd om hogere beschikbaarheid te verkrijgen. 
> [!div class="checklist"]
> * [Waarom zou u virtuele Azure-machines verplaatsen](#why-would-you-move-azure-vms)
> * [Hoe kunt u virtuele Azure-machines verplaatsen](#how-to-move-azure-vms)
> * [Gangbare architecturen](#typical-architectures-for-a-multi-tier-deployment)
> * [Virtuele machines ongewijzigd naar een doelregio verplaatsen](#move-azure-vms-to-another-region)
> * [Virtuele machines verplaatsen om beschikbaarheid te vergroten](#move-vms-to-increase-availability)


## <a name="why-would-you-move-azure-vms"></a>Waarom zou u virtuele Azure-machines verplaatsen

Klanten verplaatsen virtuele machines om de volgende redenen:-

- Als u al in één regio hebt geïmplementeerd en een nieuwe ondersteunde regio wordt toegevoegd die zich dichter bij de eindgebruikers van uw toepassing of service bevindt, wilt u waarschijnlijk **uw virtuele machines ongewijzigd naar de nieuwe regio verplaatsen** om latentie te verminderen. Diezelfde aanpak wordt gebruikt als u abonnementen wilt samenvoegen of er bestuurlijke-/organisatieregels zijn op basis waarvan u verplicht bent te verplaatsen. 
- Als uw virtuele machine is geïmplementeerd als een enkele virtuele machine of als onderdeel van een beschikbaarheidsset en u de SLA voor beschikbaarheid wilt vergroten, kunt u uw **virtuele machines als een beschikbaarheidsset verplaatsen**. 

## <a name="how-to-move-azure-vms"></a>Hoe kunt u virtuele Azure-machines verplaatsen
Het verplaatsen van virtuele machines omvat de volgende stappen:

1. Vereisten verifiëren 
2. De bron-VM’s voorbereiden 
3. De doelregio voorbereiden 
4. Gegevens naar de doelregio kopiëren. Maak gebruik van de replicatietechnologie van Azure Site Recovery om gegevens van de bron-VM naar de doelregio te kopiëren
5. De configuratie testen: Zodra de replicatie is voltooid, test u de configuratie door een testfailover via een niet-productie-netwerk uit te voeren.
6. De verplaatsing uitvoeren 
7. De resources in de bronregio verwijderen 


> [!IMPORTANT]
> Momenteel ondersteunt Azure Site Recovery de verplaatsing van virtuele machines van de ene regio naar een andere regio en biedt het geen ondersteuning voor het verplaatsen binnen een regio. 

> [!NOTE]
> Gedetailleerde informatie over deze stappen wordt beschreven in de documentatie voor elk van de scenario’s die hier worden vermeld

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Gangbare architecturen voor een implementatie met meerdere lagen
In de onderstaande sectie worden de meest voorkomende implementatie-architecturen besproken waarmee klanten gaan werken, voor een toepassing met meerdere lagen in Azure. Het voorbeeld dat hier wordt genomen, is een toepassing met drie lagen met een openbaar IP-adres. Elk van de lagen - web, toepassing en database - bevat elk 2 VM's en is via een load balancer met de andere lagen verbonden. De databaselaag heeft SQL Always ON-replicatie tussen de VM's voor hoge beschikbaarheid.

1.  **Enkelvoudige virtuele machines die zijn geïmplementeerd in verschillende lagen**: elke virtuele machine in een laag is geconfigureerd als enkelvoudige VM, die via load balancers met de andere lagen is verbonden. Dit is de eenvoudigste configuratie die klanten kunnen gaan gebruiken.

       ![enkelvoudige VM’s](media/move-vm-overview/regular-deployment.PNG)

2. **Virtuele machines die in elke laag in de beschikbaarheidsset zijn geïmplementeerd**: elke virtuele machine in een laag is geconfigureerd in een beschikbaarheidsset. [Beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) zorgen ervoor dat de VM's die u in Azure implementeert over meerdere geïsoleerde hardwareknooppunten in een cluster worden verdeeld. Dit zorgt ervoor dat als er zich binnen Azure een hardware- of softwarestoring voordoet, er slechts een subset van uw VM's wordt beïnvloed en dat uw totale oplossing beschikbaar en operationeel blijft. 
   
      ![beschikbaarheidsset](media/move-vm-overview/AVset.PNG)

3. **Virtuele machines die in elke laag in de beschikbaarheidsset zijn geïmplementeerd**: elke virtuele machine in een laag is geconfigureerd in [Azure-beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Een beschikbaarheidszone in een Azure-regio is een combinatie van een foutdomein en een updatedomein. Als u bijvoorbeeld drie of meer virtuele machines in drie zones in een Azure-regio maakt, worden uw virtuele machines effectief over drie foutdomeinen en drie updatedomeinen verdeeld. Het Azure-platform herkent deze verdeling over updatedomeinen om ervoor te zorgen dat virtuele machines in verschillende zones niet op hetzelfde moment worden bijgewerkt.

      ![zone-implementatie](media/move-vm-overview/zone.PNG)



 ## <a name="move-vms-as-is-to-a-target-region"></a>Virtuele machines ongewijzigd naar een doelregio verplaatsen

Op basis van de hierboven genoemde [architecturen](#typical-architectures-for-a-multi-tier-deployment) ziet u hier hoe de implementaties eruit zien nadat u de virtuele machines ongewijzigd naar de doelregio hebt verplaatst.


1. **Enkelvoudige virtuele machines die zijn geïmplementeerd in verschillende lagen** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **Virtuele machines die in een beschikbaarheidsset zijn geïmplementeerd**

     ![crossregionAset.PNG](media/move-vm-overview/crossregionAset.PNG)


3. **Virtuele machines die in een beschikbaarheidszone zijn geïmplementeerd**
      

     ![AzoneCross.PNG](media/move-vm-overview/AzoneCross.PNG)

## <a name="move-vms-to-increase-availability"></a>Virtuele machines verplaatsen om beschikbaarheid te vergroten

1. **Enkelvoudige virtuele machines die zijn geïmplementeerd in verschillende lagen** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **Virtuele machines die in een beschikbaarheidsset zijn geïmplementeerd**: wanneer u replicatie voor uw virtuele machine met Azure Site Recovery inschakelt, kunt u kiezen voor een configuratie waarin uw virtuele machines in een beschikbaarheidsset in afzonderlijke beschikbaarheidszones worden geplaatst. Nadat u de verplaatsing hebt voltooid, is de SLA voor beschikbaarheid 99,9%.

      ![aset-Azone.PNG](media/move-vm-overview/aset-Azone.PNG)


## <a name="next-steps"></a>Volgende stappen

In dit document staan de algemene richtlijnen voor het verplaatsen van virtuele machines vermeld. Voor meer informatie over de stapsgewijze uitvoering hiervan leest u:


> [!div class="nextstepaction"]
> * [Virtuele Azure-machines verplaatsen naar een andere regio](azure-to-azure-tutorial-migrate.md)

> * [Virtuele Azure-machines verplaatsen naar beschikbaarheidszones](move-azure-VMs-AVset-Azone.md)

