---
title: Beschikbaarheid van SAP HANA op Azure-regio's | Microsoft Docs
description: Een overzicht van de van beschikbaarheidsoverwegingen bij het uitvoeren van SAP HANA op Azure Virtual machines in meerdere Azure-regio's.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/11/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c12a8d342e2fec41cb2318ac7abfe1d3fce31cef
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391681"
---
# <a name="sap-hana-availability-across-azure-regions"></a>Beschikbaarheid van SAP HANA op Azure-regio 's

Dit artikel wordt beschreven scenario's met betrekking tot beschikbaarheid van SAP HANA in verschillende Azure-regio's. Vanwege de afstand tussen Azure-regio's omvat het instellen van de beschikbaarheid van SAP HANA in meerdere Azure-regio's speciale overwegingen.

## <a name="why-deploy-across-multiple-azure-regions"></a>Waarom meerdere Azure-regio's implementeren

Azure-regio's worden vaak gescheiden door grote afstand. Afhankelijk van de geopolitieke regio, de afstand tussen de Azure-regio's mogelijk grote afstand, of zelfs enkele duizenden mijl, zoals in de Verenigde Staten. De afstand netwerkverkeer tussen de assets die zijn geïmplementeerd in twee verschillende Azure-regio's aanzienlijke netwerklatentie voor retour ondervindt. De latentie is van belang genoeg is om uit te sluiten van synchrone gegevensuitwisseling tussen twee SAP HANA-instanties onder normale SAP-workloads. 

Aan de andere kant hebben organisaties vaak een vereiste afstand tussen de locatie van het primaire datacenter en een secundair datacenter. Een vereiste afstand biedt beschikbaarheid als een natuurlijke ramp in een bredere geografische locatie optreedt. Voorbeelden zijn onder meer de orkanen die de Caribisch gebied en Florida in September en oktober 2017 bereikt. Uw organisatie mogelijk heeft ten minste een vereiste minimale afstand. Voor de meeste Azure-klanten, de definitie van een minimale afstand moet u om te ontwerpen voor beschikbaarheid van de hele [Azure-regio's](https://azure.microsoft.com/regions/). Omdat de afstand tussen twee Azure-regio's te groot is voor gebruik van de modus voor HANA synchrone replicatie, RTO en RPO-vereisten u beschikbaarheid-configuraties in één regio implementeren en vervolgens aanvullen met extra implementaties in een tweede mogelijk verplichten de regio.

Een ander aspect rekening houden met in dit scenario is failover en het omleiden van de client. De veronderstelling is dat een failover tussen SAP HANA-instanties in twee verschillende Azure-regio's altijd een handmatige failover. Omdat de replicatiemodus van SAP HANA-systeemreplicatie is ingesteld op asynchroon, is het mogelijk dat gegevens die zijn doorgevoerd in de primaire HANA-instantie is niet nog het gemaakt met het secundaire HANA-exemplaar. Daarom geen automatische failover een optie voor configuraties waarin de replicatie is asynchroon. Zelfs met handmatig gecontroleerde failover, zoals in een oefening failover moet u maatregelen om ervoor te zorgen dat de vastgelegde gegevens aan de primaire het gemaakt met het secundaire exemplaar voordat u handmatig naar de andere Azure-regio overzetten.
 
Azure-netwerk maakt gebruik van een ander IP-adresbereik. De IP-adressen worden geïmplementeerd in de tweede Azure-regio. Dus u beide wilt wijzigen van de configuratie van de SAP HANA-client of bij voorkeur, moet u stappen voor het wijzigen van de naamomzetting te maken. Op deze manier worden de clients omgeleid naar de nieuwe secundaire site server IP-adres. Zie voor meer informatie het artikel SAP [herstel van de Client verbinding na overname](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html).   

## <a name="simple-availability-between-two-azure-regions"></a>Eenvoudige beschikbaar is tussen de twee Azure-regio 's

U kunt niet alle beschikbaarheidsconfiguratie opstellen binnen één regio, maar nog steeds de vraag om de werkbelasting die wordt geleverd als zich een noodgeval voordoet. Typische gevallen voor systemen, zoals deze zijn niet-productieve systemen. Hoewel het systeem dat u voor een halve of zelfs een dag duurzame is, kunt u het systeem niet beschikbaar is voor 48 uur of langer kan niet toestaan. Als u de installatie minder dure, een ander systeem dat zelfs minder belangrijk is in de virtuele machine worden uitgevoerd. Het andere systeem fungeert als een doel. U kunt ook het formaat van de virtuele machine in de secundaire regio kleiner en ervoor kiest geen te vooraf laden van de gegevens. Omdat de failover handmatig is en veel meer stappen nodig om failover van de volledige toepassingsstack inhoudt, is de extra tijd voor de virtuele machine af en start de virtuele machine opnieuw op de grootte van acceptabel.

Als u van het scenario gebruikmaakt voor het delen van het doel voor herstel na Noodgevallen met een QA-systeem in één virtuele machine, moet u rekening houden met deze overwegingen account:

- Er zijn twee [bewerkingsmodi](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html) met delta_datashipping en logreplay, die beschikbaar zijn voor dit scenario zijn
- Beide bewerkingsmodi hebben verschillende geheugenvereisten zonder vooraf laden van gegevens
- Delta_datashipping mogelijk hebt u aanzienlijk minder geheugen zonder de optie vooraf dan logreplay nodig. Zie hoofdstuk 4.3 van de SAP-document [hoe aan uitvoeren System Replication voor SAP HANA](https://archive.sap.com/kmuuid2/9049e009-b717-3110-ccbd-e14c277d84a3/How%20to%20Perform%20System%20Replication%20for%20SAP%20HANA.pdf)
- De geheugenvereiste van de bewerkingsmodus logreplay zonder vooraf laden is niet deterministisch en is afhankelijk van de columnstore-structuren geladen


![Diagram van twee virtuele machines via twee regio 's](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> In deze configuratie moet u een RPO niet biedt = 0, omdat de replicatiemodus van uw HANA-systeem asynchroon is. Als u nodig hebt voor een RPO = 0, deze configuratie wordt de configuratie van keuze niet.

Een kleine wijziging die u in de configuratie maken kunt is mogelijk om gegevens te configureren als vooraf laden. Echter, gezien de aard handmatige failover-en het feit dat de toepassingslagen ook wilt verplaatsen naar de tweede regio, het wellicht niet verstandig om te vooraf laden van gegevens. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Combineer beschikbaarheid binnen één regio en tussen regio 's 

Een combinatie van beschikbaarheid binnen en tussen regio's kan worden aangestuurd door deze factoren:

- Een vereiste voor RPO = 0 binnen een Azure-regio.
- De organisatie niet bereid is of kan het algemene bewerkingen die worden beïnvloed door een grote natuurlijke ramp die van invloed op een grotere regio hebben. Was dit het geval is bij sommige orkanen die het Caribisch gebied in de afgelopen paar jaar bereikt.
- Regels die vraag die zijn afstanden tussen primaire en secundaire sites die duidelijk zijn dan wat Azure availability zones kunnen bieden.

In dergelijke gevallen kunt u de SAP-aanroepen instellen een [replicatieconfiguratie van SAP HANA-systeem met meerdere lagen](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) via HANA system replication. De architectuur zou er als volgt:

![Diagram van drie VM's via twee regio 's](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Voor meer informatie over het gebruik van logreplay als bewerkingsmodus deze configuratie biedt een RPO = 0, met een lage RTO, binnen de primaire regio. De configuratie biedt ook goede RPO als er een verplaatsing naar de tweede regio is betrokken. De RTO tijden in de tweede regio zijn afhankelijk van of gegevens vooraf geladen. Veel klanten maken gebruik van de virtuele machine in de secundaire regio om uit te voeren een testsysteem. Gebruik in dat geval, de gegevens kan niet worden geladen.

> [!IMPORTANT]
> De bewerkingsmodi tussen de verschillende lagen moeten homogene. U **kan geen** logreply gebruiken zoals Werkingsmodus tussen laag 1- en laag 2 en delta_datashipping om op te geven voor laag 3. U kunt alleen de een of andere Werkingsmodus die moet worden consistent zijn voor alle lagen. Omdat delta_datashipping is niet geschikt om u te bieden een RPO = 0, de bewerkingsmodus alleen redelijke voor dergelijke een multi-laag-configuratie logreplay blijft. Voor meer informatie over de bewerkingsmodi en er zijn enkele beperkingen, Zie het artikel SAP [bewerkingsmodi voor SAP HANA-systeemreplicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Volgende stappen

Zie voor stapsgewijze instructies over het instellen van deze configuraties in Azure:

- [SAP HANA-systeemreplicatie in Azure VM's instellen](sap-hana-high-availability.md)
- [Hoge beschikbaarheid voor SAP HANA met behulp van systeemreplicatie](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
